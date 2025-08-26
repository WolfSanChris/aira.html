<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FinTech AI - Gestión Financiera Inteligente</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
            color: #fff;
            min-height: 100vh;
            overflow-x: hidden;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 0;
            border-bottom: 1px solid #333;
            margin-bottom: 30px;
        }

        .logo {
            font-size: 28px;
            font-weight: bold;
            background: linear-gradient(45deg, #00d4ff, #ffd700);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .user-profile {
            display: flex;
            align-items: center;
            gap: 15px;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px 20px;
            border-radius: 50px;
            backdrop-filter: blur(10px);
        }

        /* Dashboard Grid */
        .dashboard {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 30px;
        }

        .card {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 30px;
            backdrop-filter: blur(20px);
            position: relative;
            overflow: hidden;
            transition: all 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
            border-color: #00d4ff;
            box-shadow: 0 20px 40px rgba(0, 212, 255, 0.2);
        }

        .card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: conic-gradient(transparent, rgba(0, 212, 255, 0.1), transparent);
            animation: rotate 4s linear infinite;
            z-index: -1;
        }

        @keyframes rotate {
            100% { transform: rotate(360deg); }
        }

        .card h3 {
            color: #00d4ff;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* Balance Display */
        .balance-card {
            text-align: center;
            grid-column: 1 / -1;
        }

        .balance-amount {
            font-size: 48px;
            font-weight: bold;
            background: linear-gradient(45deg, #00d4ff, #ffd700);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin: 20px 0;
        }

        .balance-change {
            color: #4ade80;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }

        /* Transaction Form */
        .transaction-form {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .form-group label {
            color: #00d4ff;
            font-weight: 500;
        }

        .form-input {
            padding: 15px;
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.05);
            color: #fff;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .form-input:focus {
            outline: none;
            border-color: #00d4ff;
            box-shadow: 0 0 20px rgba(0, 212, 255, 0.3);
        }

        .form-select {
            padding: 15px;
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.05);
            color: #fff;
            font-size: 16px;
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .btn-primary {
            background: linear-gradient(45deg, #00d4ff, #0ea5e9);
            color: #fff;
        }

        .btn-success {
            background: linear-gradient(45deg, #4ade80, #22c55e);
            color: #fff;
        }

        .btn-danger {
            background: linear-gradient(45deg, #f87171, #ef4444);
            color: #fff;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        }

        .btn:active {
            transform: translateY(0);
        }

        /* AI Assistant */
        .ai-assistant {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 350px;
            max-height: 500px;
            background: rgba(0, 0, 0, 0.9);
            border: 2px solid #00d4ff;
            border-radius: 20px;
            backdrop-filter: blur(20px);
            display: none;
            flex-direction: column;
            z-index: 1000;
        }

        .ai-header {
            padding: 20px;
            border-bottom: 1px solid #333;
            display: flex;
            justify-content: between;
            align-items: center;
        }

        .ai-chat {
            flex-grow: 1;
            padding: 20px;
            overflow-y: auto;
            max-height: 300px;
        }

        .ai-input-container {
            padding: 20px;
            border-top: 1px solid #333;
            display: flex;
            gap: 10px;
        }

        .ai-toggle {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: linear-gradient(45deg, #00d4ff, #ffd700);
            border: none;
            color: #000;
            font-size: 24px;
            cursor: pointer;
            transition: all 0.3s ease;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(0, 212, 255, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(0, 212, 255, 0); }
            100% { box-shadow: 0 0 0 0 rgba(0, 212, 255, 0); }
        }

        /* Transaction History */
        .transaction-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s ease;
        }

        .transaction-item:hover {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
        }

        .transaction-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .transaction-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
        }

        .transaction-income {
            background: rgba(74, 222, 128, 0.2);
            color: #4ade80;
        }

        .transaction-expense {
            background: rgba(248, 113, 113, 0.2);
            color: #f87171;
        }

        .amount-positive {
            color: #4ade80;
            font-weight: bold;
        }

        .amount-negative {
            color: #f87171;
            font-weight: bold;
        }

        /* Security Indicators */
        .security-indicator {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(74, 222, 128, 0.2);
            color: #4ade80;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: 600;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .dashboard {
                grid-template-columns: 1fr;
            }
            
            .ai-assistant {
                width: calc(100vw - 40px);
                right: 20px;
            }
            
            .balance-amount {
                font-size: 36px;
            }
        }

        /* Charts placeholder */
        .chart-placeholder {
            height: 200px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
            margin-top: 15px;
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 10px;
            color: #fff;
            font-weight: 500;
            opacity: 0;
            transform: translateX(100%);
            transition: all 0.3s ease;
            z-index: 1001;
        }

        .notification.show {
            opacity: 1;
            transform: translateX(0);
        }

        .notification.success {
            background: linear-gradient(45deg, #4ade80, #22c55e);
        }

        .notification.error {
            background: linear-gradient(45deg, #f87171, #ef4444);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <div class="logo">
                <i class="fas fa-brain"></i>
                FinTech AI
            </div>
            <div class="user-profile">
                <i class="fas fa-shield-alt" style="color: #4ade80;"></i>
                <span>Seguridad Máxima</span>
                <i class="fas fa-user-circle" style="font-size: 24px;"></i>
            </div>
        </div>

        <!-- Balance Principal -->
        <div class="card balance-card">
            <div class="security-indicator">
                <i class="fas fa-lock"></i> Encriptado AES-256
            </div>
            <h3><i class="fas fa-wallet"></i> Balance Total</h3>
            <div class="balance-amount" id="totalBalance">$0.00</div>
            <div class="balance-change">
                <i class="fas fa-arrow-up"></i>
                <span>+2.5% desde ayer</span>
            </div>
        </div>

        <!-- Dashboard Principal -->
        <div class="dashboard">
            <!-- Formulario de Transacciones -->
            <div class="card">
                <h3><i class="fas fa-exchange-alt"></i> Nueva Transacción</h3>
                <form class="transaction-form" id="transactionForm">
                    <div class="form-group">
                        <label>Tipo de Transacción</label>
                        <select class="form-select" id="transactionType" required>
                            <option value="">Seleccionar tipo</option>
                            <option value="income">💰 Ingreso</option>
                            <option value="expense">💸 Gasto</option>
                            <option value="investment">📈 Inversión</option>
                            <option value="withdrawal">🏧 Retiro</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Monto</label>
                        <input type="number" class="form-input" id="amount" placeholder="$0.00" step="0.01" required>
                    </div>
                    <div class="form-group">
                        <label>Descripción</label>
                        <input type="text" class="form-input" id="description" placeholder="Descripción de la transacción" required>
                    </div>
                    <div class="form-group">
                        <label>Categoría</label>
                        <select class="form-select" id="category" required>
                            <option value="">Seleccionar categoría</option>
                            <option value="food">🍽️ Comida</option>
                            <option value="transport">🚗 Transporte</option>
                            <option value="business">💼 Negocios</option>
                            <option value="crypto">₿ Criptomonedas</option>
                            <option value="other">📦 Otros</option>
                        </select>
                    </div>
                    <button type="submit" class="btn btn-primary">
                        <i class="fas fa-plus"></i> Agregar Transacción
                    </button>
                </form>
            </div>

            <!-- Historial de Transacciones -->
            <div class="card">
                <h3><i class="fas fa-history"></i> Transacciones Recientes</h3>
                <div id="transactionHistory">
                    <div class="transaction-item">
                        <div class="transaction-info">
                            <div class="transaction-icon transaction-income">
                                <i class="fas fa-arrow-down"></i>
                            </div>
                            <div>
                                <div>Depósito inicial</div>
                                <div style="font-size: 12px; color: #666;">Hace 2 horas</div>
                            </div>
                        </div>
                        <div class="amount-positive">+$1,000.00</div>
                    </div>
                </div>
            </div>

            <!-- Análisis IA -->
            <div class="card">
                <h3><i class="fas fa-chart-line"></i> Análisis Inteligente</h3>
                <div id="aiAnalysis">
                    <p><i class="fas fa-lightbulb" style="color: #ffd700;"></i> <strong>Recomendación IA:</strong></p>
                    <p>Basado en tus patrones de gasto, podrías ahorrar $150 este mes optimizando gastos de transporte.</p>
                    <div class="chart-placeholder">
                        <i class="fas fa-chart-area" style="font-size: 48px; opacity: 0.3;"></i>
                    </div>
                </div>
            </div>

            <!-- Generador de Ingresos IA -->
            <div class="card">
                <h3><i class="fas fa-money-bill-wave"></i> Generador de Ingresos IA</h3>
                <div id="incomeGenerator">
                    <div class="income-stats">
                        <div style="display: flex; justify-content: space-between; margin-bottom: 15px;">
                            <div>
                                <div style="font-size: 24px; color: #4ade80; font-weight: bold;" id="dailyEarnings">$0.00</div>
                                <div style="font-size: 12px; color: #666;">Ganado hoy</div>
                            </div>
                            <div>
                                <div style="font-size: 24px; color: #00d4ff; font-weight: bold;" id="weeklyEarnings">$0.00</div>
                                <div style="font-size: 12px; color: #666;">Esta semana</div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="income-opportunities">
                        <div class="transaction-item" onclick="activateOpportunity('survey')">
                            <div class="transaction-info">
                                <div class="transaction-icon" style="background: rgba(74, 222, 128, 0.2); color: #4ade80;">
                                    <i class="fas fa-poll"></i>
                                </div>
                                <div>
                                    <div>Encuestas IA</div>
                                    <div style="font-size: 12px; color: #666;">5-15 min • Disponible</div>
                                </div>
                            </div>
                            <div style="color: #4ade80; font-weight: bold;">+$3-12</div>
                        </div>
                        
                        <div class="transaction-item" onclick="activateOpportunity('microtask')">
                            <div class="transaction-info">
                                <div class="transaction-icon" style="background: rgba(0, 212, 255, 0.2); color: #00d4ff;">
                                    <i class="fas fa-tasks"></i>
                                </div>
                                <div>
                                    <div>Micro-tareas</div>
                                    <div style="font-size: 12px; color: #666;">Validar datos • Activo</div>
                                </div>
                            </div>
                            <div style="color: #00d4ff; font-weight: bold;">+$5-20</div>
                        </div>
                        
                        <div class="transaction-item" onclick="activateOpportunity('cashback')">
                            <div class="transaction-info">
                                <div class="transaction-icon" style="background: rgba(255, 215, 0, 0.2); color: #ffd700;">
                                    <i class="fas fa-percentage"></i>
                                </div>
                                <div>
                                    <div>Cashback Inteligente</div>
                                    <div style="font-size: 12px; color: #666;">Compras optimizadas</div>
                                </div>
                            </div>
                            <div style="color: #ffd700; font-weight: bold;">+$8-25</div>
                        </div>
                        
                        <div class="transaction-item" onclick="activateOpportunity('arbitrage')">
                            <div class="transaction-info">
                                <div class="transaction-icon" style="background: rgba(168, 85, 247, 0.2); color: #a855f7;">
                                    <i class="fas fa-exchange-alt"></i>
                                </div>
                                <div>
                                    <div>Arbitraje de Precios</div>
                                    <div style="font-size: 12px; color: #666;">IA detectó oportunidad</div>
                                </div>
                            </div>
                            <div style="color: #a855f7; font-weight: bold;">+$15-45</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 20px;">
                        <button class="btn btn-success" onclick="startAutoEarning()" id="autoEarnBtn">
                            <i class="fas fa-play"></i> Iniciar Generación Automática
                        </button>
                    </div>
                </div>
            </div>

            <!-- Oportunidades de Inversión -->
            <div class="card">
                <h3><i class="fas fa-rocket"></i> Inversiones Inteligentes</h3>
                <div id="opportunities">
                    <div class="transaction-item">
                        <div class="transaction-info">
                            <div class="transaction-icon" style="background: rgba(255, 215, 0, 0.2); color: #ffd700;">
                                <i class="fab fa-bitcoin"></i>
                            </div>
                            <div>
                                <div>DCA Bitcoin (Micro)</div>
                                <div style="font-size: 12px; color: #666;">Desde $10 • Riesgo Medio</div>
                            </div>
                        </div>
                        <div style="color: #4ade80; font-weight: bold;">+8.5% anual</div>
                    </div>
                    <div class="transaction-item">
                        <div class="transaction-info">
                            <div class="transaction-icon" style="background: rgba(0, 212, 255, 0.2); color: #00d4ff;">
                                <i class="fas fa-store"></i>
                            </div>
                            <div>
                                <div>Productos Tendencia</div>
                                <div style="font-size: 12px; color: #666;">Dropshipping • $20 inicial</div>
                            </div>
                        </div>
                        <div style="color: #4ade80; font-weight: bold;">+45% ROI</div>
                    </div>
                    <div class="transaction-item">
                        <div class="transaction-info">
                            <div class="transaction-icon" style="background: rgba(34, 197, 94, 0.2); color: #22c55e;">
                                <i class="fas fa-seedling"></i>
                            </div>
                            <div>
                                <div>Fondo de Emergencia</div>
                                <div style="font-size: 12px; color: #666;">Alto rendimiento • Seguro</div>
                            </div>
                        </div>
                        <div style="color: #22c55e; font-weight: bold;">+5.2% APY</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Asistente IA -->
    <div class="ai-assistant" id="aiAssistant">
        <div class="ai-header">
            <div>
                <i class="fas fa-robot" style="color: #00d4ff;"></i>
                <strong style="margin-left: 10px;">Asistente IA</strong>
            </div>
            <button onclick="toggleAI()" style="background: none; border: none; color: #fff; font-size: 18px; cursor: pointer;">
                <i class="fas fa-times"></i>
            </button>
        </div>
        <div class="ai-chat" id="aiChat">
            <div style="background: rgba(0, 212, 255, 0.1); padding: 10px; border-radius: 10px; margin-bottom: 10px;">
                <strong>🤖 IA:</strong> ¡Hola! Soy tu asistente financiero inteligente. ¿En qué puedo ayudarte hoy?
            </div>
        </div>
        <div class="ai-input-container">
            <input type="text" id="aiInput" placeholder="Pregúntame sobre tus finanzas..." style="flex-grow: 1; padding: 10px; border: 1px solid #333; border-radius: 5px; background: rgba(255, 255, 255, 0.1); color: #fff;">
            <button onclick="sendAIMessage()" class="btn btn-primary" style="padding: 10px 15px;">
                <i class="fas fa-paper-plane"></i>
            </button>
        </div>
    </div>

    <!-- Botón Toggle IA -->
    <button class="ai-toggle" onclick="toggleAI()" id="aiToggle">
        <i class="fas fa-robot"></i>
    </button>

    <!-- Notification Container -->
    <div id="notification" class="notification"></div>

    <script>
        // Estado de la aplicación
        let appState = {
            balance: 1000.00,
            transactions: [],
            aiActive: false,
            dailyEarnings: 0,
            weeklyEarnings: 0,
            autoEarningActive: false,
            earningOpportunities: {
                survey: { name: 'Encuestas IA', min: 3, max: 12, time: 10000, active: false },
                microtask: { name: 'Micro-tareas', min: 5, max: 20, time: 15000, active: false },
                cashback: { name: 'Cashback Inteligente', min: 8, max: 25, time: 20000, active: false },
                arbitrage: { name: 'Arbitraje de Precios', min: 15, max: 45, time: 25000, active: false }
            }
        };

        // Inicializar la aplicación
        document.addEventListener('DOMContentLoaded', function() {
            updateBalance();
            loadTransactions();
            updateEarningsDisplay();
            
            // Simulación de seguridad
            setTimeout(() => {
                showNotification('Conexión segura establecida con cifrado AES-256', 'success');
            }, 1000);
            
            // Iniciar oportunidades de ingresos en background
            setTimeout(() => {
                showNotification('IA detectó 4 oportunidades de ingresos disponibles', 'success');
            }, 3000);
        });

        // Manejar formulario de transacciones
        document.getElementById('transactionForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const type = document.getElementById('transactionType').value;
            const amount = parseFloat(document.getElementById('amount').value);
            const description = document.getElementById('description').value;
            const category = document.getElementById('category').value;
            
            if (!type || !amount || !description || !category) {
                showNotification('Por favor completa todos los campos', 'error');
                return;
            }
            
            // Simulación de autenticación biométrica
            showNotification('Verificando identidad biométrica...', 'success');
            
            setTimeout(() => {
                addTransaction(type, amount, description, category);
                this.reset();
                showNotification('Transacción procesada con éxito', 'success');
            }, 2000);
        });

        function addTransaction(type, amount, description, category) {
            const transaction = {
                id: Date.now(),
                type: type,
                amount: amount,
                description: description,
                category: category,
                date: new Date().toLocaleDateString(),
                time: new Date().toLocaleTimeString()
            };
            
            appState.transactions.unshift(transaction);
            
            // Actualizar balance
            if (type === 'income') {
                appState.balance += amount;
            } else {
                appState.balance -= amount;
            }
            
            updateBalance();
            loadTransactions();
            updateAIAnalysis(transaction);
        }

        function updateBalance() {
            document.getElementById('totalBalance').textContent = 
                '$' + appState.balance.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
        }

        function loadTransactions() {
            const historyContainer = document.getElementById('transactionHistory');
            
            if (appState.transactions.length === 0) {
                historyContainer.innerHTML = `
                    <div class="transaction-item">
                        <div class="transaction-info">
                            <div class="transaction-icon transaction-income">
                                <i class="fas fa-arrow-down"></i>
                            </div>
                            <div>
                                <div>Depósito inicial</div>
                                <div style="font-size: 12px; color: #666;">Hace 2 horas</div>
                            </div>
                        </div>
                        <div class="amount-positive">+$1,000.00</div>
                    </div>
                `;
                return;
            }
            
            historyContainer.innerHTML = appState.transactions
                .slice(0, 5)
                .map(transaction => {
                    const isIncome = transaction.type === 'income';
                    const iconClass = isIncome ? 'transaction-income' : 'transaction-expense';
                    const amountClass = isIncome ? 'amount-positive' : 'amount-negative';
                    const sign = isIncome ? '+' : '-';
                    const icon = isIncome ? 'fa-arrow-down' : 'fa-arrow-up';
                    
                    return `
                        <div class="transaction-item">
                            <div class="transaction-info">
                                <div class="transaction-icon ${iconClass}">
                                    <i class="fas ${icon}"></i>
                                </div>
                                <div>
                                    <div>${transaction.description}</div>
                                    <div style="font-size: 12px; color: #666;">${transaction.date} ${transaction.time}</div>
                                </div>
                            </div>
                            <div class="${amountClass}">${sign}$${transaction.amount.toFixed(2)}</div>
                        </div>
                    `;
                }).join('');
        }

        function updateAIAnalysis(newTransaction) {
            const analysisContainer = document.getElementById('aiAnalysis');
            
            // Simulación de análisis IA
            const insights = [
                `Detecté un patrón de gasto en ${newTransaction.category}. Te sugiero establecer un límite mensual.`,
                `Tu flujo de efectivo es positivo. Considera invertir $${(newTransaction.amount * 0.1).toFixed(2)} en ETFs.`,
                `Has gastado ${newTransaction.amount > 100 ? 'más' : 'menos'} de lo usual. La IA sugiere ${newTransaction.amount > 100 ? 'reducir' : 'mantener'} este patrón.`,
                `Oportunidad detectada: Puedes ahorrar 15% en ${newTransaction.category} usando cupones inteligentes.`
            ];
            
            const randomInsight = insights[Math.floor(Math.random() * insights.length)];
            
            analysisContainer.innerHTML = `
                <p><i class="fas fa-lightbulb" style="color: #ffd700;"></i> <strong>Recomendación IA:</strong></p>
                <p>${randomInsight}</p>
                <div class="chart-placeholder">
                    <i class="fas fa-chart-area" style="font-size: 48px; opacity: 0.3;"></i>
                </div>
            `;
        }

        // Sistema de IA Chat
        function toggleAI() {
            const aiAssistant = document.getElementById('aiAssistant');
            const aiToggle = document.getElementById('aiToggle');
            
            appState.aiActive = !appState.aiActive;
            
            if (appState.aiActive) {
                aiAssistant.style.display = 'flex';
                aiToggle.style.display = 'none';
            } else {
                aiAssistant.style.display = 'none';
                aiToggle.style.display = 'block';
            }
        }

        function sendAIMessage() {
            const input = document.getElementById('aiInput');
            const message = input.value.trim();
            
            if (!message) return;
            
            const chatContainer = document.getElementById('aiChat');
            
            // Agregar mensaje del usuario
            chatContainer.innerHTML += `
                <div style="background: rgba(255, 255, 255, 0.1); padding: 10px; border-radius: 10px; margin-bottom: 10px; text-align: right;">
                    <strong>👤 Tú:</strong> ${message}
                </div>
            `;
            
            input.value = '';
            
            // Simulación de respuesta IA
            setTimeout(() => {
                const aiResponses = [
                    `Tu balance actual es ${appState.balance.toFixed(2)}. Has generado ${appState.dailyEarnings.toFixed(2)} hoy con la IA.`,
                    `He detectado que puedes generar ${(Math.random() * 50 + 20).toFixed(2)} adicionales hoy. ¿Activo las oportunidades automáticamente?`,
                    `Tu IA ha identificado ${Math.floor(Math.random() * 3) + 2} oportunidades de micro-ingresos. Potencial: ${(Math.random() * 30 + 15).toFixed(2)}.`,
                    `Análisis completado: Puedes generar ${(Math.random() * 25 + 12).toFixed(2)} en las próximas 2 horas con tareas automatizadas.`,
                    `Oportunidad de arbitraje detectada: Diferencia de precios de ${(Math.random() * 40 + 10).toFixed(2)}. ¿Ejecuto la operación?`,
                    `Tu perfil de micro-inversiones está generando ${(Math.random() * 8 + 3).toFixed(2)} por hora. ¿Incremento la exposición?`,
                    `He optimizado tus gastos y encontré ${(Math.random() * 60 + 25).toFixed(2)} en ahorros potenciales este mes.`
                ];
                
                const response = aiResponses[Math.floor(Math.random() * aiResponses.length)];
                
                chatContainer.innerHTML += `
                    <div style="background: rgba(0, 212, 255, 0.1); padding: 10px; border-radius: 10px; margin-bottom: 10px;">
                        <strong>🤖 IA:</strong> ${response}
                    </div>
                `;
                
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }, 1000);
            
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        // Sistema de generación de ingresos
        function activateOpportunity(type) {
            const opportunity = appState.earningOpportunities[type];
            
            if (opportunity.active) {
                showNotification(`${opportunity.name} ya está activo`, 'error');
                return;
            }
            
            opportunity.active = true;
            showNotification(`Activando ${opportunity.name}... Generando ingresos automáticamente`, 'success');
            
            // Simular proceso de generación de ingresos
            setTimeout(() => {
                const earning = Math.random() * (opportunity.max - opportunity.min) + opportunity.min;
                const roundedEarning = Math.round(earning * 100) / 100;
                
                // Agregar ingreso automático
                addTransaction('income', roundedEarning, `${opportunity.name} - Generado por IA`, 'business');
                
                appState.dailyEarnings += roundedEarning;
                appState.weeklyEarnings += roundedEarning;
                updateEarningsDisplay();
                
                showNotification(`¡Éxito! Generaste ${roundedEarning} con ${opportunity.name}`, 'success');
                
                // Reactivar después de un tiempo
                setTimeout(() => {
                    opportunity.active = false;
                }, opportunity.time);
                
            }, 2000 + Math.random() * 3000); // Entre 2-5 segundos
        }
        
        function startAutoEarning() {
            const btn = document.getElementById('autoEarnBtn');
            
            if (appState.autoEarningActive) {
                // Detener generación automática
                appState.autoEarningActive = false;
                btn.innerHTML = '<i class="fas fa-play"></i> Iniciar Generación Automática';
                btn.className = 'btn btn-success';
                showNotification('Generación automática de ingresos detenida', 'error');
                return;
            }
            
            // Iniciar generación automática
            appState.autoEarningActive = true;
            btn.innerHTML = '<i class="fas fa-pause"></i> Detener Generación';
            btn.className = 'btn btn-danger';
            showNotification('IA iniciada: Buscando oportunidades de ingresos automáticamente', 'success');
            
            // Ciclo de generación automática
            autoEarningCycle();
        }
        
        function autoEarningCycle() {
            if (!appState.autoEarningActive) return;
            
            const availableOpportunities = Object.keys(appState.earningOpportunities)
                .filter(key => !appState.earningOpportunities[key].active);
            
            if (availableOpportunities.length > 0) {
                const randomOpportunity = availableOpportunities[Math.floor(Math.random() * availableOpportunities.length)];
                
                // 30% probabilidad de activar una oportunidad cada ciclo
                if (Math.random() < 0.3) {
                    activateOpportunity(randomOpportunity);
                }
            }
            
            // Micro-ingresos aleatorios (pequeñas cantidades)
            if (Math.random() < 0.15) { // 15% probabilidad
                const microEarning = Math.random() * 8 + 2; // Entre $2-10
                const roundedEarning = Math.round(microEarning * 100) / 100;
                
                addTransaction('income', roundedEarning, 'Micro-ingreso IA - Optimización automática', 'business');
                appState.dailyEarnings += roundedEarning;
                appState.weeklyEarnings += roundedEarning;
                updateEarningsDisplay();
                
                showNotification(`Micro-ingreso: +${roundedEarning} generado automáticamente`, 'success');
            }
            
            // Repetir el ciclo cada 8-15 segundos
            setTimeout(autoEarningCycle, Math.random() * 7000 + 8000);
        }
        
        function updateEarningsDisplay() {
            document.getElementById('dailyEarnings').textContent = '

        function showNotification(message, type) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            
            setTimeout(() => {
                notification.classList.add('show');
            }, 100);
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Simulación de actualizaciones en tiempo real
        setInterval(() => {
            // Simulación de fluctuaciones de mercado
            const opportunities = document.getElementById('opportunities');
            const currentOpportunities = opportunities.querySelectorAll('.transaction-item');
            
            currentOpportunities.forEach(opportunity => {
                const potentialElement = opportunity.querySelector('div:last-child');
                if (potentialElement && potentialElement.textContent.includes('%')) {
                    const currentValue = parseFloat(potentialElement.textContent.match(/\d+\.\d+/)[0]);
                    const newValue = currentValue + (Math.random() - 0.5) * 2;
                    potentialElement.textContent = `+${newValue.toFixed(1)}% anual`;
                }
            });
            
            // Simulación de ingresos pasivos pequeños cada minuto
            if (Math.random() < 0.1 && appState.autoEarningActive) { // 10% probabilidad
                const passiveEarning = Math.random() * 3 + 0.5; // Entre $0.50-3.50
                const roundedEarning = Math.round(passiveEarning * 100) / 100;
                
                addTransaction('income', roundedEarning, 'Ingreso Pasivo IA - Background', 'business');
                appState.dailyEarnings += roundedEarning;
                appState.weeklyEarnings += roundedEarning;
                updateEarningsDisplay();
            }
        }, 5000);

        // Sistema de reset diario (simulado cada 2 minutos para demo)
        setInterval(() => {
            if (appState.dailyEarnings > 0) {
                showNotification(`Resumen del día: +${appState.dailyEarnings.toFixed(2)} generados automáticamente`, 'success');
                appState.dailyEarnings = 0;
                updateEarningsDisplay();
            }
        }, 120000); // 2 minutos para demo

        // Seguridad adicional - Simulación de monitoreo
        setInterval(() => {
            const securityEvents = [
                'Escaneo de seguridad completado ✅',
                'Verificación de integridad de datos ✅',
                'Monitoreo de transacciones activo 🛡️',
                'Backup automático realizado 💾'
            ];
            
            if (Math.random() < 0.3) { // 30% probabilidad
                const event = securityEvents[Math.floor(Math.random() * securityEvents.length)];
                console.log(`🔒 Seguridad: ${event}`);
            }
        }, 10000);
     + appState.dailyEarnings.toFixed(2);
            document.getElementById('weeklyEarnings').textContent = '

        function showNotification(message, type) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            
            setTimeout(() => {
                notification.classList.add('show');
            }, 100);
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Simulación de actualizaciones en tiempo real
        setInterval(() => {
            // Simulación de fluctuaciones de mercado
            const opportunities = document.getElementById('opportunities');
            const currentOpportunities = opportunities.querySelectorAll('.transaction-item');
            
            currentOpportunities.forEach(opportunity => {
                const potentialElement = opportunity.querySelector('div:last-child');
                const currentValue = parseFloat(potentialElement.textContent.match(/\d+\.\d+/)[0]);
                const newValue = currentValue + (Math.random() - 0.5) * 2;
                potentialElement.textContent = `+${newValue.toFixed(1)}% potencial`;
            });
        }, 5000);

        // Seguridad adicional - Simulación de monitoreo
        setInterval(() => {
            const securityEvents = [
                'Escaneo de seguridad completado ✅',
                'Verificación de integridad de datos ✅',
                'Monitoreo de transacciones activo 🛡️',
                'Backup automático realizado 💾'
            ];
            
            if (Math.random() < 0.3) { // 30% probabilidad
                const event = securityEvents[Math.floor(Math.random() * securityEvents.length)];
                console.log(`🔒 Seguridad: ${event}`);
            }
        }, 10000);
     + appState.weeklyEarnings.toFixed(2);
        }

        function showNotification(message, type) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            
            setTimeout(() => {
                notification.classList.add('show');
            }, 100);
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Simulación de actualizaciones en tiempo real
        setInterval(() => {
            // Simulación de fluctuaciones de mercado
            const opportunities = document.getElementById('opportunities');
            const currentOpportunities = opportunities.querySelectorAll('.transaction-item');
            
            currentOpportunities.forEach(opportunity => {
                const potentialElement = opportunity.querySelector('div:last-child');
                const currentValue = parseFloat(potentialElement.textContent.match(/\d+\.\d+/)[0]);
                const newValue = currentValue + (Math.random() - 0.5) * 2;
                potentialElement.textContent = `+${newValue.toFixed(1)}% potencial`;
            });
        }, 5000);

        // Seguridad adicional - Simulación de monitoreo
        setInterval(() => {
            const securityEvents = [
                'Escaneo de seguridad completado ✅',
                'Verificación de integridad de datos ✅',
                'Monitoreo de transacciones activo 🛡️',
                'Backup automático realizado 💾'
            ];
            
            if (Math.random() < 0.3) { // 30% probabilidad
                const event = securityEvents[Math.floor(Math.random() * securityEvents.length)];
                console.log(`🔒 Seguridad: ${event}`);
            }
        }, 10000);
