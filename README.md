#WolfSanCh
import pandas as pd
import numpy as np
import yfinance as yf
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import schedule
import time
import datetime

class TradingBot:
    def __init__(self, symbol='BTC-USD', period='1y', interval='1d'):
        self.symbol = symbol
        self.period = period
        self.interval = interval
        self.model = RandomForestClassifier(n_estimators=100, random_state=42)
        self.last_action = None
        self.balance = 1000  # Balance inicial
        self.holdings = 0
        self.entry_price = 0
        
    def get_data(self):
        """Descarga datos históricos"""
        print(f"Descargando datos para {self.symbol}...")
        data = yf.download(self.symbol, period=self.period, interval=self.interval)
        return data
    
    def prepare_features(self, data):
        """Prepara características para el modelo"""
        # Crear indicadores técnicos básicos
        df = data.copy()
        
        # Media móvil simple
        df['SMA20'] = df['Close'].rolling(window=20).mean()
        df['SMA50'] = df['Close'].rolling(window=50).mean()
        
        # RSI (Índice de Fuerza Relativa)
        delta = df['Close'].diff()
        gain = delta.where(delta > 0, 0).rolling(window=14).mean()
        loss = -delta.where(delta < 0, 0).rolling(window=14).mean()
        rs = gain / loss
        df['RSI'] = 100 - (100 / (1 + rs))
        
        # MACD
        df['EMA12'] = df['Close'].ewm(span=12).mean()
        df['EMA26'] = df['Close'].ewm(span=26).mean()
        df['MACD'] = df['EMA12'] - df['EMA26']
        df['Signal_Line'] = df['MACD'].ewm(span=9).mean()
        
        # Crear target (si el precio subió o bajó al día siguiente)
        df['Target'] = np.where(df['Close'].shift(-1) > df['Close'], 1, 0)
        
        # Eliminar filas con NaN
        df = df.dropna()
        
        # Seleccionar características
        features = ['SMA20', 'SMA50', 'RSI', 'MACD', 'Signal_Line', 'Volume']
        X = df[features]
        y = df['Target']
        
        return X, y, df
    
    def train_model(self):
        """Entrena el modelo de aprendizaje automático"""
        data = self.get_data()
        X, y, _ = self.prepare_features(data)
        
        # Dividir en conjuntos de entrenamiento y prueba
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
        
        # Entrenar el modelo
        print("Entrenando modelo...")
        self.model.fit(X_train, y_train)
        
        # Evaluar el modelo
        predictions = self.model.predict(X_test)
        accuracy = accuracy_score(y_test, predictions)
        print(f"Precisión del modelo: {accuracy:.2f}")
        
        return accuracy
    
    def predict_next_move(self):
        """Predice si comprar, vender o mantener basado en datos actuales"""
        data = self.get_data()
        X, _, df = self.prepare_features(data)
        
        # Obtener la última fila de datos para predicción
        latest_data = X.iloc[-1].values.reshape(1, -1)
        
        # Hacer predicción
        prediction = self.model.predict(latest_data)[0]
        probability = self.model.predict_proba(latest_data)[0]
        
        current_price = df['Close'].iloc[-1]
        
        # Lógica de trading
        action = None
        if prediction == 1 and probability[1] > 0.6:  # Alta probabilidad de subida
            action = "BUY"
        elif prediction == 0 and probability[0] > 0.6:  # Alta probabilidad de bajada
            action = "SELL"
        else:
            action = "HOLD"
            
        return action, current_price, probability
    
    def execute_trade(self, action, price):
        """Ejecuta una operación de compra o venta"""
        if action == "BUY" and self.last_action != "BUY" and self.balance > 0:
            # Simular compra
            amount_to_buy = self.balance * 0.95  # Usar 95% del balance
            self.holdings = amount_to_buy / price
            self.balance -= amount_to_buy
            self.entry_price = price
            self.last_action = "BUY"
            print(f"COMPRA: {self.holdings:.6f} {self.symbol} a ${price:.2f}")
            
        elif action == "SELL" and self.holdings > 0:
            # Simular venta
            amount_sold = self.holdings * price
            self.balance += amount_sold
            
            profit = amount_sold - (self.holdings * self.entry_price)
            profit_percentage = (profit / (self.holdings * self.entry_price)) * 100
            
            print(f"VENTA: {self.holdings:.6f} {self.symbol} a ${price:.2f}")
            print(f"Beneficio: ${profit:.2f} ({profit_percentage:.2f}%)")
            
            self.holdings = 0
            self.last_action = "SELL"
            
        self.print_status(price)
    
    def print_status(self, current_price):
        """Muestra el estado actual de la cuenta"""
        holdings_value = self.holdings * current_price
        total_value = self.balance + holdings_value
        
        print(f"\n--- Estado de la cuenta {datetime.datetime.now()} ---")
        print(f"Efectivo disponible: ${self.balance:.2f}")
        print(f"Holdings: {self.holdings:.6f} {self.symbol} (${holdings_value:.2f})")
        print(f"Valor total: ${total_value:.2f}")
        print("-----------------------------\n")
    
    def run(self):
        """Ejecuta el ciclo principal del bot"""
        print(f"Iniciando bot de trading para {self.symbol}...")
        
        # Entrenar el modelo inicialmente
        self.train_model()
        
        # Ejecutar la primera predicción
        action, price, probs = self.predict_next_move()
        print(f"Acción recomendada: {action} (Confianza: {max(probs):.2f})")
        self.execute_trade(action, price)
        
        # Programar ejecuciones periódicas
        def job():
            print("\nReentrenando modelo...")
            self.train_model()
            action, price, probs = self.predict_next_move()
            print(f"Acción recomendada: {action} (Confianza: {max(probs):.2f})")
            self.execute_trade(action, price)
        
        # Programar trabajo diario (ajustar según necesidades)
        schedule.every().day.at("09:00").do(job)
        
        print("Bot en ejecución. Presiona Ctrl+C para detener.")
        try:
            while True:
                schedule.run_pending()
                time.sleep(60)
        except KeyboardInterrupt:
            print("Bot detenido por el usuario.")

# Función para enviar notificaciones (implementación básica)
def send_notification(message):
    # Aquí podrías integrar con servicios como Telegram, Email, etc.
    print(f"NOTIFICACIÓN: {message}")

# Función para conectar con tu cuenta bancaria (solo como ejemplo)
def connect_to_bank(bank_account):
    # Esta es solo una función de ejemplo. En una implementación real,
    # necesitarías usar APIs específicas del banco.
    print(f"Conectado a cuenta bancaria: {bank_account}")
    return True

# Ejemplo de uso
if __name__ == "__main__":
    # Puedes cambiar el símbolo según el activo que quieras operar
    bot = TradingBot(symbol="BTC-USD", period="1y", interval="1d")
    
    # Simulación de conexión a cuenta bancaria
    bank_connected = connect_to_bank("Nu-67121041")
    
    if bank_connected:
        bot.run()
    else:
        print("Error al conectar con la cuenta bancaria. Verifica tus credenciales.")
