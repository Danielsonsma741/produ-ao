<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ambipar - Sistema de Produção</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: sans-serif;
        }
        
        body {
            background-color: #39ff14;
            color: #333;
            line-height: 1.6;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        /* Tela de Login */
        .login-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #CAF880 0%, #000000 100%);
        }
        
        .login-box {
            background-color: white;
            border-radius: 15px;
            padding: 40px;
            width: 100%;
            max-width: 400px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            text-align: center;
        }
        
        .login-logo {
            margin-bottom: 30px;
        }
        
        .login-logo h1 {
            color: #CAF880;
            font-size: 2rem;
            margin-bottom: 10px;
        }
        
        .login-logo p {
            color: #666;
            font-size: 0.9rem;
        }
        
        .login-form .form-group {
            margin-bottom: 20px;
            text-align: left;
        }
        
        .login-form label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }
        
        .login-form input {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
            transition: all 0.3s;
        }
        
        .login-form input:focus {
            outline: none;
            border-color: #CAF880;
            box-shadow: 0 0 0 2px rgba(202, 248, 128, 0.2);
        }
        
        .login-btn {
            background-color: #CAF880;
            color: black;
            border: none;
            padding: 14px;
            width: 100%;
            border-radius: 6px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 10px;
        }
        
        .login-btn:hover {
            background-color: #b8e66a;
        }
        
        .login-error {
            background-color: #ffebee;
            color: #c62828;
            padding: 12px;
            border-radius: 6px;
            margin-bottom: 20px;
            display: none;
        }
        
        /* Header após login */
        header {
            background: linear-gradient(135deg, #CAF880 0%, #008542 100%);
            color: white;
            padding: 15px 0;
            margin-bottom: 30px;
            box-shadow: 0 4px 12px rgba(0, 168, 89, 0.2);
            position: relative;
        }
        
        .header-content {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 20px;
        }
        
        .logo-container {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #39ff14;
            border-radius: 8px;
            padding: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .logo img {
            height: 50px;
            width: auto;
        }
        
        .logo-text {
            text-align: left;
        }
        
        h1 {
            font-size: 1.8rem;
            margin-bottom: 5px;
            font-weight: 700;
        }
        
        .subtitle {
            font-size: 1rem;
            opacity: 0.9;
            font-weight: 400;
        }
        
        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .user-avatar {
            width: 40px;
            height: 40px;
            background-color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #CAF880;
            font-weight: bold;
        }
        
        .user-name {
            font-weight: 600;
        }
        
        .logout-btn {
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.3);
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 0.9rem;
        }
        
        .logout-btn:hover {
            background-color: rgba(255, 255, 255, 0.3);
        }
        
        /* Sistema de abas */
        .tabs {
            display: flex;
            background-color: white;
            border-radius: 10px 10px 0 0;
            overflow: hidden;
            margin-bottom: 0;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
            border-bottom: 3px solid #CAF880;
            flex-wrap: wrap;
        }
        
        .tab {
            padding: 18px 15px;
            text-align: center;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            color: #666;
            background-color: #f8f9fa;
            border: none;
            flex: 1;
            min-width: 150px;
        }
        
        .tab:hover {
            background-color: #e9f9f0;
            color: #00a859;
        }
        
        .tab.active {
            background-color: white;
            color: #CAF880;
            border-top: 3px solid #CAF880;
            margin-top: -3px;
        }
        
        .admin-tab {
            background-color: #fff8e1;
            color: #ff8f00;
        }
        
        .admin-tab:hover {
            background-color: #ffecb3;
        }
        
        .tab-content {
            display: none;
            background-color: white;
            padding: 30px;
            border-radius: 0 0 10px 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            margin-bottom: 30px;
            border-top: none;
        }
        
        .tab-content.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .section-title {
            font-size: 1.5rem;
            color: #CAF880;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #e9f9f0;
            font-weight: 600;
        }
        
        /* Cards e formulários */
        .card {
            background-color: white;
            border-radius: 10px;
            padding: 25px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
            margin-bottom: 25px;
            border-left: 4px solid #CAF880;
        }
        
        .form-row {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-bottom: 25px;
        }
        
        .form-group {
            flex: 1;
            min-width: 200px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }
        
        select, input, textarea {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
            background-color: white;
            transition: all 0.3s;
        }
        
        select:focus, input:focus, textarea:focus {
            outline: none;
            border-color: #CAF880;
            box-shadow: 0 0 0 2px rgba(202, 248, 128, 0.2);
        }
        
        .btn {
            background-color: #CAF880;
            color: black;
            border: none;
            padding: 14px 20px;
            font-weight: 600;
            cursor: pointer;
            border-radius: 6px;
            transition: all 0.3s;
            font-size: 1rem;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .btn:hover {
            background-color: #b8e66a;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(202, 248, 128, 0.3);
        }
        
        .btn-secondary {
            background-color: #6c757d;
        }
        
        .btn-secondary:hover {
            background-color: #545b62;
        }
        
        .btn-danger {
            background-color: #dc3545;
        }
        
        .btn-danger:hover {
            background-color: #c82333;
        }
        
        /* Estatísticas */
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .stat-card {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
            text-align: center;
            border-top: 4px solid #CAF880;
            transition: transform 0.3s;
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 16px rgba(0, 168, 89, 0.1);
        }
        
        .stat-value {
            font-size: 2.5rem;
            font-weight: 700;
            color: #00a859;
            margin: 10px 0;
        }
        
        .stat-label {
            color: #666;
            font-size: 0.9rem;
        }
        
        /* Gráficos */
        .chart-container {
            height: 400px;
            margin-top: 30px;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            border: 1px solid #e9ecef;
        }
        
        /* Histórico de produção */
        .production-list {
            list-style-type: none;
        }
        
        .production-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            border-bottom: 1px solid #e9ecef;
            transition: background-color 0.2s;
            position: relative;
        }
        
        .production-item:hover {
            background-color: #f8f9fa;
        }
        
        .production-item:last-child {
            border-bottom: none;
        }
        
        .model-badge {
            display: inline-block;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            background-color: #f8f9fa;
        }
        
        .bistro { background-color: #ffeaa7; color: #8a6300; }
        .alegra { background-color: #a29bfe; color: #2d1b8a; }
        .folha { background-color: #55efc4; color: #005a40; }
        .coral { background-color: #fd79a8; color: #880e4f; }
        
        /* Tabelas de administração */
        .admin-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        .admin-table th {
            background-color: #f8f9fa;
            padding: 12px 15px;
            text-align: left;
            font-weight: 600;
            color: #555;
            border-bottom: 2px solid #e9ecef;
        }
        
        .admin-table td {
            padding: 12px 15px;
            border-bottom: 1px solid #e9ecef;
        }
        
        .admin-table tr:hover {
            background-color: #f8f9fa;
        }
        
        .action-buttons {
            display: flex;
            gap: 8px;
        }
        
        .action-btn {
            padding: 6px 12px;
            border-radius: 4px;
            font-size: 0.85rem;
            cursor: pointer;
            border: none;
            display: inline-flex;
            align-items: center;
            gap: 5px;
        }
        
        .edit-btn {
            background-color: #e3f2fd;
            color: #1976d2;
        }
        
        .delete-btn {
            background-color: #ffebee;
            color: #d32f2f;
        }
        
        /* Relatórios */
        .report-filters {
            background-color: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 20px;
            border: 1px solid #e9ecef;
        }
        
        .report-actions {
            display: flex;
            gap: 15px;
            margin-top: 20px;
        }
        
        /* Footer */
        .ambipar-footer {
            background-color: #004d2a;
            color: white;
            padding: 20px 0;
            margin-top: 40px;
            border-radius: 0 0 10px 10px;
        }
        
        .footer-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .footer-logo {
            font-weight: 700;
            font-size: 1.2rem;
        }
        
        .footer-text {
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        /* Responsividade */
        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                text-align: center;
                gap: 15px;
            }
            
            .user-info {
                flex-direction: column;
                gap: 10px;
            }
            
            .tabs {
                flex-direction: column;
            }
            
            .tab {
                min-width: 100%;
            }
            
            .form-row {
                flex-direction: column;
            }
            
            .stats-container {
                grid-template-columns: 1fr;
            }
            
            .footer-content {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }
            
            .action-buttons {
                flex-direction: column;
            }
        }
        
        /* Utilitários */
        .hidden {
            display: none !important;
        }
        
        .loading {
            text-align: center;
            padding: 40px;
            color: #666;
        }
        
        .permission-warning {
            background-color: #fff3cd;
            color: #856404;
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 20px;
            border: 1px solid #ffeaa7;
        }
    </style>
</head>
<body>
    <!-- Tela de Login -->
    <div id="loginScreen" class="login-container">
        <div class="login-box">
            <div class="login-logo">
                <h1><i class="fas fa-leaf"></i> Ambipar</h1>
                <p>Sistema de Produção de Cadeiras</p>
                <p style="font-size: 0.8rem; margin-top: 10px; color: #00a859;">Controle de produção sustentável</p>
            </div>
            
            <div id="loginError" class="login-error">
                <i class="fas fa-exclamation-circle"></i> <span id="errorMessage">Credenciais inválidas</span>
            </div>
            
            <form id="loginForm" class="login-form">
                <div class="form-group">
                    <label for="loginEmail"><i class="fas fa-envelope"></i> E-mail</label>
                    <input type="email" id="loginEmail" placeholder="seu@email.com" required>
                </div>
                
                <div class="form-group">
                    <label for="loginPassword"><i class="fas fa-lock"></i> Senha</label>
                    <input type="password" id="loginPassword" placeholder="Sua senha" required>
                </div>
                
                <button type="submit" class="login-btn">
                    <i class="fas fa-sign-in-alt"></i> Entrar no Sistema
                </button>
                
                <div style="margin-top: 20px; font-size: 0.85rem; color: #666;">
                    <p><strong>Credenciais de teste:</strong></p>
                    <p>Admin: admin@ambipar.com / senha: admin123</p>
                    <p>Operador: operador@ambipar.com / senha: operador123</p>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Sistema Principal (após login) -->
    <div id="mainApp" class="hidden">
        <header>
            <div class="container">
                <div class="header-content">
                    <div class="logo-container">
                        <div class="logo">
                            <h1 style="color: black; font-size: 1.2rem; font-weight: bold; margin: 0; text-align: center;">Ambipar<br>Environment</h1>
                        </div>
                        <div class="logo-text">
                            <h1>Sistema de Produção</h1>
                            <p class="subtitle">Controle de produção sustentável</p>
                        </div>
                    </div>
                    
                    <div class="user-info">
                        <div class="user-avatar" id="userAvatar">A</div>
                        <div>
                            <div class="user-name" id="userName">Administrador</div>
                            <div class="user-role" id="userRole">Administrador</div>
                        </div>
                        <button id="logoutBtn" class="logout-btn">
                            <i class="fas fa-sign-out-alt"></i> Sair
                        </button>
                    </div>
                </div>
            </div>
        </header>
        
        <div class="container">
            <div class="tabs" id="mainTabs">
                <!-- Abas comuns (para todos os usuários) -->
                <button class="tab" data-tab="register">
                    <i class="fas fa-plus-circle"></i> Registrar Produção
                </button>
                <button class="tab" data-tab="dashboard">
                    <i class="fas fa-chart-line"></i> Dashboard
                </button>
                <button class="tab" data-tab="history">
                    <i class="fas fa-history"></i> Histórico
                </button>
                <button class="tab" data-tab="warnings">
                    <i class="fas fa-exclamation-triangle"></i> Avisos da Liderança
                </button>
                
                <!-- Abas de administração (somente para admin) -->
                <button class="tab admin-tab" data-tab="operators">
                    <i class="fas fa-users"></i> Operadores
                </button>
                <button class="tab admin-tab" data-tab="reports">
                    <i class="fas fa-file-alt"></i> Relatórios
                </button>
                <button class="tab admin-tab" data-tab="admin">
                    <i class="fas fa-cog"></i> Administração
                </button>
            </div>
            
            <!-- Tab 1: Registrar Produção -->
            <div id="register" class="tab-content active">
                <h2 class="section-title"><i class="fas fa-plus-circle"></i> Registrar Nova Produção</h2>
                
                <div class="card">
                    <div class="form-row">
                        <div class="form-group">
                            <label for="operator"><i class="fas fa-user"></i> Operador</label>
                            <select id="operator">
                                <option value="">Selecione um operador</option>
                                <!-- Operadores serão carregados do Firebase -->
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="model"><i class="fas fa-chair"></i> Modelo da Cadeira</label>
                            <select id="model">
                                <option value="">Selecione um modelo</option>
                                <option value="bistro">Cadeira Bistro</option>
                                <option value="alegra">Cadeira Alegra</option>
                                <option value="folha">Cadeira Folha</option>
                                <option value="coral">Cadeira Coral</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="form-row">
                        <div class="form-group">
                            <label for="period"><i class="fas fa-clock"></i> Período de Produção</label>
                            <select id="period">
                                <option value="daily">Diário</option>
                                <option value="hourly">Hora a Hora</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="quantity"><i class="fas fa-boxes"></i> Quantidade Produzida</label>
                            <input type="number" id="quantity" min="1" max="1000" placeholder="Ex: 50">
                        </div>
                        
                        <div class="form-group">
                            <label for="losses"><i class="fas fa-minus-circle"></i> Perdas</label>
                            <input type="number" id="losses" min="0" max="1000" placeholder="Ex: 5">
                        </div>
                        
                        <div class="form-group">
                            <label for="date"><i class="fas fa-calendar-alt"></i> Data</label>
                            <input type="date" id="date">
                        </div>
                    </div>
                    
                    <div class="form-row">
                        <div class="form-group">
                            <label for="notes"><i class="fas fa-sticky-note"></i> Observações (opcional)</label>
                            <input type="text" id="notes" placeholder="Observações sobre a produção">
                        </div>
                    </div>
                    
                    <button id="registerProduction" class="btn">
                        <i class="fas fa-save"></i> Registrar Produção
                    </button>
                </div>
            </div>
            
            <!-- Tab 2: Dashboard -->
            <div id="dashboard" class="tab-content">
                <h2 class="section-title"><i class="fas fa-chart-line"></i> Dashboard de Produção</h2>
                
                <div class="stats-container">
                    <div class="stat-card">
                        <div class="stat-label">Produção Total Hoje</div>
                        <div class="stat-value" id="totalToday">0</div>
                        <div class="stat-label">unidades</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-label">Operadores Ativos</div>
                        <div class="stat-value" id="activeOperators">0</div>
                        <div class="stat-label">pessoas</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-label">Eficiência de Produção</div>
                        <div class="stat-value" id="efficiency">0%</div>
                        <div class="stat-label">meta: 85%</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-label">Produção do Mês</div>
                        <div class="stat-value" id="monthProduction">0</div>
                        <div class="stat-label">unidades</div>
                    </div>
                </div>
                
                <div class="card">
                    <h3 class="section-title">Produção por Modelo</h3>
                    <div class="chart-container">
                        <canvas id="modelChart"></canvas>
                    </div>
                </div>
                
                <div class="card">
                    <h3 class="section-title">Produção por Operador</h3>
                    <div class="chart-container">
                        <canvas id="operatorChart"></canvas>
                    </div>
                </div>
            </div>
            
            <!-- Tab 3: Histórico -->
            <div id="history" class="tab-content">
                <h2 class="section-title"><i class="fas fa-history"></i> Histórico de Produção</h2>
                
                <div class="report-filters">
                    <div class="form-row">
                        <div class="form-group">
                            <label for="filterOperator">Filtrar por Operador</label>
                            <select id="filterOperator">
                                <option value="all">Todos os operadores</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="filterModel">Filtrar por Modelo</label>
                            <select id="filterModel">
                                <option value="all">Todos os modelos</option>
                                <option value="bistro">Cadeira Bistro</option>
                                <option value="alegra">Cadeira Alegra</option>
                                <option value="folha">Cadeira Folha</option>
                                <option value="coral">Cadeira Coral</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="filterDate">Filtrar por Data</label>
                            <input type="date" id="filterDate">
                        </div>
                    </div>
                    
                    <button id="applyFilters" class="btn">
                        <i class="fas fa-filter"></i> Aplicar Filtros
                    </button>
                    <button id="clearFilters" class="btn btn-secondary">
                        <i class="fas fa-eraser"></i> Limpar Filtros
                    </button>
                </div>
                
                <div class="card">
                    <h3 class="section-title">Registros de Produção</h3>
                    <div id="historyLoading" class="loading">
                        <i class="fas fa-spinner fa-spin"></i> Carregando histórico...
                    </div>
                    <ul class="production-list" id="productionHistory">
                        <!-- Histórico será carregado aqui via Firebase -->
                    </ul>
                </div>
            </div>
            
            <!-- Tab 4: Avisos da Liderança -->
            <div id="warnings" class="tab-content">
                <h2 class="section-title"><i class="fas fa-exclamation-triangle"></i> Avisos da Liderança</h2>
                
                <div class="card">
                    <p>Aqui você encontrará comunicados importantes da liderança sobre metas, procedimentos e atualizações.</p>
                    <ul style="margin-top: 20px;">
                        <li><strong>Meta de Produção:</strong> Atingir 100 unidades por dia até o final do mês.</li>
                        <li><strong>Segurança:</strong> Sempre usar equipamentos de proteção.</li>
                        <li><strong>Qualidade:</strong> Verificar todos os produtos antes do envio.</li>
                    </ul>
                </div>
            </div>
            
            <!-- Tab 5: Operadores (Admin apenas) -->
            <div id="operators" class="tab-content">
                <div id="operatorPermissionWarning" class="permission-warning hidden">
                    <i class="fas fa-exclamation-triangle"></i> Você não tem permissão para acessar esta área. Apenas administradores podem gerenciar operadores.
                </div>
                
                <div id="operatorAdminContent">
                    <h2 class="section-title"><i class="fas fa-users"></i> Gerenciamento de Operadores</h2>
                    
                    <div class="card">
                        <h3 class="section-title">Adicionar Novo Operador</h3>
                        <div class="form-row">
                            <div class="form-group">
                                <label for="newOperatorName">Nome Completo</label>
                                <input type="text" id="newOperatorName" placeholder="Digite o nome do operador">
                            </div>
                            
                            <div class="form-group">
                                <label for="newOperatorEmail">E-mail</label>
                                <input type="email" id="newOperatorEmail" placeholder="operador@ambipar.com">
                            </div>
                            
                            <div class="form-group">
                                <label for="newOperatorPassword">Senha</label>
                                <input type="password" id="newOperatorPassword" placeholder="Digite a senha">
                            </div>
                            
                            <div class="form-group">
                                <label for="newOperatorId">ID do Operador</label>
                                <input type="text" id="newOperatorId" placeholder="Ex: OP001">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="newOperatorTeam">Equipe</label>
                                <select id="newOperatorTeam">
                                    <option value="A">Equipe A</option>
                                    <option value="B">Equipe B</option>
                                    <option value="C">Equipe C</option>
                                </select>
                            </div>
                            
                            <div class="form-group">
                                <label for="newOperatorStatus">Status</label>
                                <select id="newOperatorStatus">
                                    <option value="ativo">Ativo</option>
                                    <option value="inativo">Inativo</option>
                                    <option value="ferias">Férias</option>
                                </select>
                            </div>
                            
                            <div class="form-group">
                                <label for="newOperatorRole">Tipo de Usuário</label>
                                <select id="newOperatorRole">
                                    <option value="operador">Operador</option>
                                    <option value="admin">Administrador</option>
                                </select>
                            </div>
                        </div>
                        
                        <button id="addOperator" class="btn">
                            <i class="fas fa-user-plus"></i> Adicionar Operador
                        </button>
                    </div>
                    
                    <div class="card">
                        <h3 class="section-title">Operadores Cadastrados</h3>
                        <div id="operatorsLoading" class="loading">
                            <i class="fas fa-spinner fa-spin"></i> Carregando operadores...
                        </div>
                        <table class="admin-table" id="operatorsTable">
                            <thead>
                                <tr>
                                    <th>Nome</th>
                                    <th>ID</th>
                                    <th>E-mail</th>
                                    <th>Equipe</th>
                                    <th>Status</th>
                                    <th>Tipo</th>
                                    <th>Ações</th>
                                </tr>
                            </thead>
                            <tbody id="operatorsList">
                                <!-- Operadores serão carregados aqui -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            
            <!-- Tab 5: Relatórios (Admin apenas) -->
            <div id="reports" class="tab-content">
                <div id="reportPermissionWarning" class="permission-warning hidden">
                    <i class="fas fa-exclamation-triangle"></i> Você não tem permissão para acessar esta área. Apenas administradores podem gerar relatórios.
                </div>
                
                <div id="reportAdminContent">
                    <h2 class="section-title"><i class="fas fa-file-alt"></i> Relatórios de Produção</h2>
                    
                    <div class="card">
                        <h3 class="section-title">Gerar Relatório</h3>
                        <div class="form-row">
                            <div class="form-group">
                                <label for="reportType">Tipo de Relatório</label>
                                <select id="reportType">
                                    <option value="daily">Diário</option>
                                    <option value="weekly">Semanal</option>
                                    <option value="monthly">Mensal</option>
                                    <option value="custom">Período Customizado</option>
                                </select>
                            </div>
                            
                            <div class="form-group">
                                <label for="reportStartDate">Data Inicial</label>
                                <input type="date" id="reportStartDate">
                            </div>
                            
                            <div class="form-group">
                                <label for="reportEndDate">Data Final</label>
                                <input type="date" id="reportEndDate">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="reportOperator">Filtrar por Operador</label>
                                <select id="reportOperator">
                                    <option value="all">Todos os operadores</option>
                                </select>
                            </div>
                            
                            <div class="form-group">
                                <label for="reportModel">Filtrar por Modelo</label>
                                <select id="reportModel">
                                    <option value="all">Todos os modelos</option>
                                    <option value="bistro">Cadeira Bistro</option>
                                    <option value="alegra">Cadeira Alegra</option>
                                    <option value="folha">Cadeira Folha</option>
                                    <option value="coral">Cadeira Coral</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="report-actions">
                            <button id="generateReport" class="btn">
                                <i class="fas fa-chart-bar"></i> Gerar Relatório
                            </button>
                            <button id="exportReport" class="btn btn-secondary">
                                <i class="fas fa-file-export"></i> Exportar para Excel
                            </button>
                            <button id="printReport" class="btn btn-secondary">
                                <i class="fas fa-print"></i> Imprimir Relatório
                            </button>
                        </div>
                    </div>
                    
                    <div class="card">
                        <h3 class="section-title">Resultado do Relatório</h3>
                        <div id="reportResult">
                            <p style="text-align: center; color: #666; padding: 40px;">
                                Selecione os filtros e clique em "Gerar Relatório" para visualizar os dados.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Tab 6: Administração (Admin apenas) -->
            <div id="admin" class="tab-content">
                <div id="adminPermissionWarning" class="permission-warning hidden">
                    <i class="fas fa-exclamation-triangle"></i> Você não tem permissão para acessar esta área. Apenas administradores podem acessar esta seção.
                </div>
                
                <div id="adminContent">
                    <h2 class="section-title"><i class="fas fa-cog"></i> Configurações do Sistema</h2>
                    
                    <div class="card">
                        <h3 class="section-title">Configurações de Produção</h3>
                        <p>Defina as metas diárias de produção por modelo de cadeira.</p>
                        <div class="form-row">
                            <div class="form-group">
                                <label for="bistroTarget">Meta Diária - Cadeira Bistro</label>
                                <input type="number" id="bistroTarget" value="50">
                            </div>
                            
                            <div class="form-group">
                                <label for="alegraTarget">Meta Diária - Cadeira Alegra</label>
                                <input type="number" id="alegraTarget" value="40">
                            </div>
                            
                            <div class="form-group">
                                <label for="folhaTarget">Meta Diária - Cadeira Folha</label>
                                <input type="number" id="folhaTarget" value="30">
                            </div>
                            
                            <div class="form-group">
                                <label for="coralTarget">Meta Diária - Cadeira Coral</label>
                                <input type="number" id="coralTarget" value="20">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="efficiencyGoal">Meta de Eficiência (%)</label>
                                <input type="number" id="efficiencyGoal" value="85" min="0" max="100">
                            </div>
                        </div>
                        
                        <button id="saveSettings" class="btn">
                            <i class="fas fa-save"></i> Salvar Configurações
                        </button>
                    </div>
                    
                    <div class="card">
                        <h3 class="section-title">Gerenciar Avisos para Operadores</h3>
                        <p>Edite os avisos que aparecerão na aba "Avisos da Liderança" para todos os operadores.</p>
                        
                        <div class="form-group">
                            <label for="warningsText">Texto dos Avisos</label>
                            <textarea id="warningsText" rows="6" placeholder="Digite os avisos aqui, separados por linha..."></textarea>
                        </div>
                        
                        <button id="saveWarnings" class="btn">
                            <i class="fas fa-save"></i> Salvar Avisos
                        </button>
                    </div>
                    
                    <div class="card">
                        <h3 class="section-title">Editar Produção</h3>
                        <p>Use esta seção para corrigir registros de produção inseridos incorretamente.</p>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="editProductionId">ID do Registro</label>
                                <input type="text" id="editProductionId" placeholder="Digite o ID do registro">
                            </div>
                            
                            <div class="form-group">
                                <label for="searchProduction">Buscar Registro</label>
                                <button id="searchProduction" class="btn">
                                    <i class="fas fa-search"></i> Buscar
                                </button>
                            </div>
                        </div>
                        
                        <div id="editProductionForm" class="hidden">
                            <div class="form-row">
                                <div class="form-group">
                                    <label for="editOperator">Operador</label>
                                    <select id="editOperator">
                                        <!-- Operadores serão carregados aqui -->
                                    </select>
                                </div>
                                
                                <div class="form-group">
                                    <label for="editModel">Modelo da Cadeira</label>
                                    <select id="editModel">
                                        <option value="bistro">Cadeira Bistro</option>
                                        <option value="alegra">Cadeira Alegra</option>
                                        <option value="folha">Cadeira Folha</option>
                                        <option value="coral">Cadeira Coral</option>
                                    </select>
                                </div>
                            </div>
                            
                            <div class="form-row">
                                <div class="form-group">
                                    <label for="editQuantity">Quantidade Produzida</label>
                                    <input type="number" id="editQuantity" min="1" max="1000">
                                </div>
                                
                                <div class="form-group">
                                    <label for="editLosses">Perdas</label>
                                    <input type="number" id="editLosses" min="0" max="1000">
                                </div>
                                
                                <div class="form-group">
                                    <label for="editDate">Data</label>
                                    <input type="date" id="editDate">
                                </div>
                            </div>
                            
                            <div class="form-row">
                                <div class="form-group">
                                    <label for="editNotes">Observações</label>
                                    <textarea id="editNotes" rows="3"></textarea>
                                </div>
                            </div>
                            
                            <div class="form-row">
                                <button id="updateProduction" class="btn">
                                    <i class="fas fa-sync-alt"></i> Atualizar Registro
                                </button>
                                <button id="deleteProduction" class="btn btn-danger">
                                    <i class="fas fa-trash-alt"></i> Excluir Registro
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <footer class="ambipar-footer">
                <div class="footer-content">
                    <div class="footer-logo">
                        <i class="fas fa-leaf"></i> Ambipar - Sistema de Produção
                    </div>
                    <div class="footer-text">
                        Solução integrada de gestão de produção com foco em sustentabilidade
                    </div>
                    <div class="footer-text">
                        &copy; 2023 Ambipar - Todos os direitos reservados
                    </div>
                </div>
            </footer>
        </div>
    </div>

    <!-- Configuração do Firebase -->
    <script>
        // Configuração do Firebase - Substitua com suas credenciais
        const firebaseConfig = {
            apiKey: "AIzaSyA3a8qIMMLLF4WR3SNmz_G4XDmMPwfUsSg",
            authDomain: "aplicativodeprodutos.firebaseapp.com",
            projectId: "aplicativodeprodutos",
            storageBucket: "aplicativodeprodutos.firebasestorage.app",
            messagingSenderId: "861467266713",
            appId: "1:861467266713:web:4426a1e837e8a5060e7208",
            measurementId: "G-HZWJGSMBMF"
        };
        
        // Inicializar Firebase
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        const auth = firebase.auth();
        
        // Configuração de segurança (as regras fornecidas)
        // Estas regras devem ser configuradas no Console do Firebase
        // Elas bloqueiam todas as operações por padrão
        // Vamos configurar a autenticação para liberar acesso apenas a usuários autorizados
        
        // Dados da aplicação
        let currentUser = null;
        let userRole = 'operador'; // Padrão: operador
        let operators = [];
        let productionHistory = [];
        let systemSettings = {
            dailyTarget: 100,
            efficiencyGoal: 85
        };
        
        // Inicializar a data atual
        document.getElementById('date').valueAsDate = new Date();
        document.getElementById('reportStartDate').valueAsDate = new Date();
        document.getElementById('reportEndDate').valueAsDate = new Date();
        
        // Sistema de login (alterado para e-mail/senha)
        document.getElementById('loginForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            
            if (!email || !password) {
                document.getElementById('errorMessage').textContent = 'Por favor, preencha e-mail e senha.';
                document.getElementById('loginError').style.display = 'block';
                return;
            }
            
            try {
                const userCredential = await auth.signInWithEmailAndPassword(email, password);
                currentUser = userCredential.user;
                
                // Salvar log de acesso no Firestore
                await db.collection('access_logs').add({
                    uid: currentUser.uid,
                    email: email,
                    action: 'login',
                    timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                    ip: 'unknown',
                    userAgent: navigator.userAgent
                });
                
                // Definir role baseado no e-mail
                userRole = email === 'souza73e@gmail.com' ? 'admin' : 'operador';
                
                // Buscar ou criar documento do usuário no Firestore
                const userDocRef = db.collection('users').doc(currentUser.uid);
                const userDoc = await userDocRef.get();
                
                if (!userDoc.exists) {
                    await userDocRef.set({
                        email: email,
                        name: email.split('@')[0], // Nome baseado no e-mail
                        role: userRole,
                        createdAt: firebase.firestore.FieldValue.serverTimestamp()
                    });
                } else {
                    const userData = userDoc.data();
                    userRole = userData.role || userRole; // Usar role salvo se existir
                }
                
                // Atualizar interface com informações do usuário
                document.getElementById('userName').textContent = email.split('@')[0];
                document.getElementById('userRole').textContent = userRole === 'admin' ? 'Administrador' : 'Operador';
                document.getElementById('userAvatar').textContent = email.charAt(0).toUpperCase();
                
                // Mostrar apenas as abas apropriadas para o tipo de usuário
                setupUserPermissions();
                
                // Carregar dados do sistema
                loadSystemData();
                
                // Mostrar aplicação principal
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('mainApp').classList.remove('hidden');
                
                // Mostrar mensagem de boas-vindas
                showNotification(`Bem-vindo, ${email.split('@')[0]}!`, 'success');
                
            } catch (error) {
                console.error('Erro no login:', error);
                document.getElementById('errorMessage').textContent = getErrorMessage(error.code);
                document.getElementById('loginError').style.display = 'block';
            }
        });
        
        // Logout
        document.getElementById('logoutBtn').addEventListener('click', async () => {
            if (currentUser) {
                // Salvar log de logout no Firestore
                await db.collection('access_logs').add({
                    uid: currentUser.uid,
                    email: currentUser.email,
                    action: 'logout',
                    timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                    ip: 'unknown',
                    userAgent: navigator.userAgent
                });
            }
            
            auth.signOut().then(() => {
                currentUser = null;
                userRole = 'operador';
                document.getElementById('mainApp').classList.add('hidden');
                document.getElementById('loginScreen').classList.remove('hidden');
                document.getElementById('loginForm').reset();
                document.getElementById('loginError').style.display = 'none';
            });
        });
        
        // Função para configurar permissões do usuário
        function setupUserPermissions() {
            const adminTabs = document.querySelectorAll('.admin-tab');
            const dashboardTab = document.querySelector('.tab[data-tab="dashboard"]');
            const historyTab = document.querySelector('.tab[data-tab="history"]');
            const warningsTab = document.querySelector('.tab[data-tab="warnings"]');
            
            if (userRole === 'admin') {
                adminTabs.forEach(tab => {
                    tab.classList.remove('hidden');
                });
                dashboardTab.classList.remove('hidden');
                historyTab.classList.remove('hidden');
                warningsTab.classList.add('hidden');
                
                // Remover avisos de permissão
                document.querySelectorAll('.permission-warning').forEach(warning => {
                    warning.classList.add('hidden');
                });
                document.querySelectorAll('#operatorAdminContent, #reportAdminContent, #adminContent').forEach(content => {
                    content.classList.remove('hidden');
                });
            } else {
                adminTabs.forEach(tab => {
                    tab.classList.add('hidden');
                });
                dashboardTab.classList.add('hidden');
                historyTab.classList.add('hidden');
                warningsTab.classList.remove('hidden');
                
                // Se estiver em uma aba escondida, redirecionar para register
                const activeTab = document.querySelector('.tab.active');
                if (activeTab && (activeTab.classList.contains('admin-tab') || activeTab.getAttribute('data-tab') === 'dashboard' || activeTab.getAttribute('data-tab') === 'history')) {
                    switchTab('register');
                }
            }
        }
        
        // Carregar dados do sistema
        async function loadSystemData() {
            // Carregar operadores
            await loadOperators();
            
            // Carregar histórico de produção
            await loadProductionHistory();
            
            // Carregar configurações
            await loadSettings();
            
            // Carregar avisos
            await loadWarnings();
            
            // Atualizar dashboard
            updateDashboard();
        }
        
        // Carregar operadores do Firestore
        async function loadOperators() {
            try {
                const operatorsSnapshot = await db.collection('operators').get();
                operators = [];
                
                operatorsSnapshot.forEach(doc => {
                    operators.push({
                        id: doc.id,
                        ...doc.data()
                    });
                });
                
                // Atualizar selects de operadores
                updateOperatorSelects();
                
                // Atualizar tabela de operadores (se for admin)
                if (userRole === 'admin') {
                    updateOperatorsTable();
                }
                
            } catch (error) {
                console.error('Erro ao carregar operadores:', error);
                showNotification('Erro ao carregar operadores', 'error');
            }
        }
        
        // Atualizar selects de operadores
        function updateOperatorSelects() {
            const operatorSelect = document.getElementById('operator');
            const filterOperatorSelect = document.getElementById('filterOperator');
            const reportOperatorSelect = document.getElementById('reportOperator');
            const editOperatorSelect = document.getElementById('editOperator');
            
            // Limpar opções atuais (exceto a primeira)
            [operatorSelect, filterOperatorSelect, reportOperatorSelect, editOperatorSelect].forEach(select => {
                while (select.options.length > 1) {
                    select.remove(1);
                }
            });
            
            // Adicionar operadores ativos
            operators.filter(op => op.status === 'ativo').forEach(operator => {
                const option = document.createElement('option');
                option.value = operator.id;
                option.textContent = operator.name;
                operatorSelect.appendChild(option);
                
                const filterOption = document.createElement('option');
                filterOption.value = operator.id;
                filterOption.textContent = operator.name;
                filterOperatorSelect.appendChild(filterOption);
                
                const reportOption = document.createElement('option');
                reportOption.value = operator.id;
                reportOption.textContent = operator.name;
                reportOperatorSelect.appendChild(reportOption);
                
                const editOption = document.createElement('option');
                editOption.value = operator.id;
                editOption.textContent = operator.name;
                editOperatorSelect.appendChild(editOption);
            });
        }
        
        // Carregar histórico de produção
        async function loadProductionHistory() {
            try {
                document.getElementById('historyLoading').classList.remove('hidden');
                
                const historySnapshot = await db.collection('production')
                    .orderBy('createdAt', 'desc')
                    .limit(100)
                    .get();
                
                productionHistory = [];
                
                historySnapshot.forEach(doc => {
                    productionHistory.push({
                        id: doc.id,
                        ...doc.data()
                    });
                });
                
                // Sort client-side by date desc, then createdAt desc
                productionHistory.sort((a, b) => {
                    if (a.date > b.date) return -1;
                    if (a.date < b.date) return 1;
                    return b.createdAt.seconds - a.createdAt.seconds; // assuming timestamp
                });
                
                updateHistoryList();
                document.getElementById('historyLoading').classList.add('hidden');
                
            } catch (error) {
                console.error('Erro ao carregar histórico:', error);
                showNotification('Erro ao carregar histórico', 'error');
                document.getElementById('historyLoading').classList.add('hidden');
            }
        }
        
        // Carregar configurações do sistema
        async function loadSettings() {
            try {
                const settingsDoc = await db.collection('settings').doc('production').get();
                
                if (settingsDoc.exists) {
                    systemSettings = settingsDoc.data();
                    
                    // Atualizar campos de configuração
                    document.getElementById('bistroTarget').value = systemSettings.bistroTarget || 50;
                    document.getElementById('alegraTarget').value = systemSettings.alegraTarget || 40;
                    document.getElementById('folhaTarget').value = systemSettings.folhaTarget || 30;
                    document.getElementById('coralTarget').value = systemSettings.coralTarget || 20;
                    document.getElementById('efficiencyGoal').value = systemSettings.efficiencyGoal || 85;
                }
                
            } catch (error) {
                console.error('Erro ao carregar configurações:', error);
            }
        }
        
        // Carregar avisos
        async function loadWarnings() {
            try {
                const warningsDoc = await db.collection('settings').doc('warnings').get();
                
                if (warningsDoc.exists) {
                    const warningsData = warningsDoc.data();
                    const warningsText = warningsData.text || '';
                    
                    // Para operadores, atualizar a aba de avisos
                    if (userRole === 'operador') {
                        updateWarningsDisplay(warningsText);
                    }
                    
                    // Para admin, preencher o textarea
                    if (userRole === 'admin') {
                        document.getElementById('warningsText').value = warningsText;
                    }
                }
                
            } catch (error) {
                console.error('Erro ao carregar avisos:', error);
            }
        }
        
        // Sistema de abas
        document.querySelectorAll('.tab').forEach(tab => {
            tab.addEventListener('click', () => {
                const tabId = tab.getAttribute('data-tab');
                
                // Verificar permissão para abas de admin
                if (tab.classList.contains('admin-tab') && userRole !== 'admin') {
                    showNotification('Apenas administradores podem acessar esta área.', 'error');
                    return;
                }
                
                switchTab(tabId);
            });
        });
        
        function switchTab(tabId) {
            // Remover classe active de todas as abas e conteúdos
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            
            // Adicionar classe active à aba clicada e ao conteúdo correspondente
            document.querySelector(`.tab[data-tab="${tabId}"]`).classList.add('active');
            document.getElementById(tabId).classList.add('active');
            
            // Ações específicas por aba
            if (tabId === 'dashboard') {
                updateDashboard();
            } else if (tabId === 'history') {
                updateHistoryList();
            } else if (tabId === 'operators' && userRole === 'admin') {
                updateOperatorsTable();
            }
        }
        
        // Registrar produção
        document.getElementById('registerProduction').addEventListener('click', async () => {
            const operatorSelect = document.getElementById('operator');
            const modelSelect = document.getElementById('model');
            const quantityInput = document.getElementById('quantity');
            const lossesInput = document.getElementById('losses');
            const dateInput = document.getElementById('date');
            const periodSelect = document.getElementById('period');
            const notesInput = document.getElementById('notes');
            
            if (!operatorSelect.value || !modelSelect.value || !quantityInput.value || !dateInput.value) {
                showNotification('Por favor, preencha todos os campos obrigatórios.', 'error');
                return;
            }
            
            // Encontrar o operador selecionado
            const operatorId = operatorSelect.value;
            const operator = operators.find(op => op.id === operatorId);
            
            if (!operator) {
                showNotification('Operador não encontrado.', 'error');
                return;
            }
            
            try {
                // Adicionar registro ao Firestore
                const newRecord = {
                    operatorId: operatorId,
                    operatorName: operator.name,
                    model: modelSelect.value,
                    quantity: parseInt(quantityInput.value),
                    losses: parseInt(lossesInput.value) || 0,
                    period: periodSelect.value,
                    date: dateInput.value,
                    notes: notesInput.value,
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    createdBy: currentUser.uid
                };
                
                await db.collection('production').add(newRecord);
                
                // Adicionar ao array local
                newRecord.id = 'temp-' + Date.now();
                productionHistory.unshift(newRecord);
                
                // Limpar o formulário
                quantityInput.value = '';
                lossesInput.value = '';
                notesInput.value = '';
                modelSelect.value = '';
                
                // Atualizar dashboard e histórico
                updateDashboard();
                if (document.getElementById('history').classList.contains('active')) {
                    updateHistoryList();
                }
                
                showNotification('Produção registrada com sucesso!', 'success');
                
            } catch (error) {
                console.error('Erro ao registrar produção:', error);
                showNotification('Erro ao registrar produção', 'error');
            }
        });
        
        // Adicionar operador (admin only)
        document.getElementById('addOperator').addEventListener('click', async () => {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem adicionar operadores.', 'error');
                return;
            }
            
            const nameInput = document.getElementById('newOperatorName');
            const emailInput = document.getElementById('newOperatorEmail');
            const passwordInput = document.getElementById('newOperatorPassword');
            const idInput = document.getElementById('newOperatorId');
            const teamSelect = document.getElementById('newOperatorTeam');
            const statusSelect = document.getElementById('newOperatorStatus');
            const roleSelect = document.getElementById('newOperatorRole');
            
            if (!nameInput.value.trim() || !idInput.value.trim() || !emailInput.value.trim() || !passwordInput.value.trim()) {
                showNotification('Por favor, preencha todos os campos obrigatórios.', 'error');
                return;
            }
            
            if (passwordInput.value.length < 6) {
                showNotification('A senha deve ter pelo menos 6 caracteres.', 'error');
                return;
            }
            
            try {
                const operatorData = {
                    name: nameInput.value.trim(),
                    email: emailInput.value.trim(),
                    operatorId: idInput.value.trim(),
                    team: teamSelect.value,
                    status: statusSelect.value,
                    role: roleSelect.value,
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    createdBy: currentUser.uid
                };
                
                await db.collection('operators').add(operatorData);
                
                // Criar conta de autenticação
                const email = emailInput.value.trim();
                const password = passwordInput.value.trim();
                try {
                    await auth.createUserWithEmailAndPassword(email, password);
                    await auth.signOut();
                    showNotification('Operador adicionado e conta criada. Você foi desconectado; faça login novamente.', 'warning');
                } catch (authError) {
                    console.error('Erro ao criar conta:', authError);
                    showNotification('Operador adicionado, mas erro ao criar conta de login: ' + authError.message, 'warning');
                }
                
                // Atualizar lista local
                operatorData.id = 'temp-' + Date.now();
                operators.push(operatorData);
                
                // Limpar campos
                nameInput.value = '';
                emailInput.value = '';
                passwordInput.value = '';
                idInput.value = '';
                
                // Atualizar selects e tabela
                updateOperatorSelects();
                updateOperatorsTable();
                
                showNotification('Operador adicionado com sucesso!', 'success');
                
            } catch (error) {
                console.error('Erro ao adicionar operador:', error);
                showNotification('Erro ao adicionar operador', 'error');
            }
        });
        
        // Atualizar tabela de operadores
        function updateOperatorsTable() {
            const tbody = document.getElementById('operatorsList');
            tbody.innerHTML = '';
            
            operators.forEach(operator => {
                const row = document.createElement('tr');
                
                row.innerHTML = `
                    <td>${operator.name}</td>
                    <td>${operator.operatorId || operator.id}</td>
                    <td>${operator.email || '-'}</td>
                    <td>${operator.team || '-'}</td>
                    <td>
                        <span class="status-badge ${operator.status}">
                            ${operator.status || 'ativo'}
                        </span>
                    </td>
                    <td>${operator.role === 'admin' ? 'Administrador' : 'Operador'}</td>
                    <td>
                        <div class="action-buttons">
                            <button class="action-btn edit-btn" onclick="editOperator('${operator.id}')">
                                <i class="fas fa-edit"></i> Editar
                            </button>
                            <button class="action-btn delete-btn" onclick="deleteOperator('${operator.id}', '${operator.name}')">
                                <i class="fas fa-trash-alt"></i> Excluir
                            </button>
                        </div>
                    </td>
                `;
                
                tbody.appendChild(row);
            });
            
            document.getElementById('operatorsLoading').classList.add('hidden');
        }
        
        // Editar operador
        window.editOperator = async function(operatorId) {
            const operator = operators.find(op => op.id === operatorId);
            
            if (!operator) {
                showNotification('Operador não encontrado.', 'error');
                return;
            }
            
            // Preencher modal de edição (simplificado)
            const newName = prompt('Novo nome do operador:', operator.name);
            if (newName && newName.trim() !== '') {
                try {
                    await db.collection('operators').doc(operatorId).update({
                        name: newName.trim()
                    });
                    
                    operator.name = newName.trim();
                    updateOperatorSelects();
                    updateOperatorsTable();
                    
                    showNotification('Operador atualizado com sucesso!', 'success');
                } catch (error) {
                    console.error('Erro ao atualizar operador:', error);
                    showNotification('Erro ao atualizar operador', 'error');
                }
            }
        };
        
        // Excluir operador
        window.deleteOperator = async function(operatorId, operatorName) {
            if (!confirm(`Tem certeza que deseja excluir o operador "${operatorName}"?`)) {
                return;
            }
            
            try {
                await db.collection('operators').doc(operatorId).delete();
                
                // Remover da lista local
                operators = operators.filter(op => op.id !== operatorId);
                
                updateOperatorSelects();
                updateOperatorsTable();
                
                showNotification('Operador excluído com sucesso!', 'success');
            } catch (error) {
                console.error('Erro ao excluir operador:', error);
                showNotification('Erro ao excluir operador', 'error');
            }
        };
        
        // Atualizar lista de histórico
        function updateHistoryList() {
            const historyList = document.getElementById('productionHistory');
            historyList.innerHTML = '';
            
            const filterOperator = document.getElementById('filterOperator').value;
            const filterModel = document.getElementById('filterModel').value;
            const filterDate = document.getElementById('filterDate').value;
            
            // Filtrar registros
            let filteredRecords = productionHistory;
            
            if (filterOperator !== 'all') {
                filteredRecords = filteredRecords.filter(record => record.operatorId === filterOperator);
            }
            
            if (filterModel !== 'all') {
                filteredRecords = filteredRecords.filter(record => record.model === filterModel);
            }
            
            if (filterDate) {
                filteredRecords = filteredRecords.filter(record => record.date === filterDate);
            }
            
            if (filteredRecords.length === 0) {
                historyList.innerHTML = '<li class="production-item" style="text-align: center; color: #666; padding: 30px;">Nenhum registro encontrado para os filtros aplicados.</li>';
                return;
            }
            
            // Adicionar registros à lista
            filteredRecords.forEach(record => {
                const listItem = document.createElement('li');
                listItem.className = 'production-item';
                listItem.dataset.id = record.id;
                
                const modelClass = record.model;
                const modelName = getModelName(record.model);
                
                // Formatar data
                const dateObj = record.date ? new Date(record.date) : new Date();
                const formattedDate = dateObj.toLocaleDateString('pt-BR');
                
                // Criar elementos de forma segura
                const leftDiv = document.createElement('div');
                const operatorNameDiv = document.createElement('div');
                operatorNameDiv.style.fontWeight = '600';
                operatorNameDiv.style.marginBottom = '5px';
                operatorNameDiv.style.color = '#00a859';
                operatorNameDiv.textContent = record.operatorName;
                
                const badgesDiv = document.createElement('div');
                badgesDiv.style.display = 'flex';
                badgesDiv.style.gap = '10px';
                badgesDiv.style.marginBottom = '8px';
                
                const dateBadge = document.createElement('span');
                dateBadge.className = 'date-badge';
                dateBadge.textContent = formattedDate;
                
                const periodBadge = document.createElement('span');
                periodBadge.className = 'period-badge';
                periodBadge.textContent = record.period === 'daily' ? 'Diário' : 'Hora a Hora';
                
                badgesDiv.appendChild(dateBadge);
                badgesDiv.appendChild(periodBadge);
                
                leftDiv.appendChild(operatorNameDiv);
                leftDiv.appendChild(badgesDiv);
                
                if (record.notes) {
                    const notesDiv = document.createElement('div');
                    notesDiv.style.fontSize = '0.9rem';
                    notesDiv.style.color = '#666';
                    notesDiv.style.fontStyle = 'italic';
                    notesDiv.textContent = '"' + record.notes + '"';
                    leftDiv.appendChild(notesDiv);
                }
                
                // Botões de ação (somente para admin)
                if (userRole === 'admin') {
                    const actionButtons = document.createElement('div');
                    actionButtons.className = 'action-buttons';
                    actionButtons.style.marginTop = '10px';
                    
                    const editBtn = document.createElement('button');
                    editBtn.className = 'action-btn edit-btn';
                    editBtn.style.fontSize = '0.8rem';
                    editBtn.onclick = () => editProductionRecord(record.id);
                    editBtn.innerHTML = '<i class="fas fa-edit"></i> Editar';
                    
                    const deleteBtn = document.createElement('button');
                    deleteBtn.className = 'action-btn delete-btn';
                    deleteBtn.style.fontSize = '0.8rem';
                    deleteBtn.onclick = () => deleteProductionRecord(record.id);
                    deleteBtn.innerHTML = '<i class="fas fa-trash-alt"></i> Excluir';
                    
                    actionButtons.appendChild(editBtn);
                    actionButtons.appendChild(deleteBtn);
                    leftDiv.appendChild(actionButtons);
                }
                
                const rightDiv = document.createElement('div');
                rightDiv.style.textAlign = 'right';
                
                const modelBadge = document.createElement('div');
                modelBadge.className = 'model-badge ' + modelClass;
                modelBadge.textContent = modelName;
                
                const quantityDiv = document.createElement('div');
                quantityDiv.style.fontSize = '1.8rem';
                quantityDiv.style.fontWeight = '700';
                quantityDiv.style.marginTop = '5px';
                quantityDiv.style.color = '#00a859';
                quantityDiv.textContent = record.quantity - (record.losses || 0);
                
                const unitsDiv = document.createElement('div');
                unitsDiv.style.fontSize = '0.85rem';
                unitsDiv.style.color = '#7f8c8d';
                unitsDiv.textContent = 'unidades';
                
                rightDiv.appendChild(modelBadge);
                rightDiv.appendChild(quantityDiv);
                rightDiv.appendChild(unitsDiv);
                
                if (record.losses && record.losses > 0) {
                    const lossesDiv = document.createElement('div');
                    lossesDiv.style.fontSize = '0.8rem';
                    lossesDiv.style.color = '#e74c3c';
                    lossesDiv.textContent = `-${record.losses} perdas`;
                    rightDiv.appendChild(lossesDiv);
                }
                
                listItem.appendChild(leftDiv);
                listItem.appendChild(rightDiv);
                
                historyList.appendChild(listItem);
            });
        }
        
        // Aplicar filtros no histórico
        document.getElementById('applyFilters').addEventListener('click', updateHistoryList);
        
        // Limpar filtros
        document.getElementById('clearFilters').addEventListener('click', () => {
            document.getElementById('filterOperator').value = 'all';
            document.getElementById('filterModel').value = 'all';
            document.getElementById('filterDate').value = '';
            updateHistoryList();
        });
        
        // Atualizar dashboard
        function updateDashboard() {
            // Calcular produção total de hoje
            const d = new Date();
            const today = d.getFullYear() + '-' + String(d.getMonth() + 1).padStart(2, '0') + '-' + String(d.getDate()).padStart(2, '0');
            const todayProduction = productionHistory
                .filter(record => record.date === today)
                .reduce((sum, record) => sum + (record.quantity - (record.losses || 0)), 0);
            
            document.getElementById('totalToday').textContent = todayProduction;
            
            // Calcular operadores ativos (com produção hoje)
            const activeOps = new Set(productionHistory
                .filter(record => record.date === today)
                .map(record => record.operatorId));
            
            document.getElementById('activeOperators').textContent = activeOps.size;
            
            // Calcular eficiência (produção atual vs meta)
            const bistroTarget = systemSettings.bistroTarget || 50;
            const alegraTarget = systemSettings.alegraTarget || 40;
            const folhaTarget = systemSettings.folhaTarget || 30;
            const coralTarget = systemSettings.coralTarget || 20;
            const totalTarget = bistroTarget + alegraTarget + folhaTarget + coralTarget;
            const efficiency = todayProduction > 0 ? Math.min(Math.round((todayProduction / totalTarget) * 100), 100) : 0;
            document.getElementById('efficiency').textContent = efficiency + '%';
            
            // Atualizar meta no label
            const efficiencyGoal = systemSettings.efficiencyGoal || 85;
            document.querySelector('.stat-card:nth-child(3) .stat-label:last-child').textContent = `meta: ${efficiencyGoal}%`;
            
            // Atualizar estatísticas de produção do mês
            const currentMonth = new Date().getMonth();
            const currentYear = new Date().getFullYear();
            const monthProduction = productionHistory
                .filter(record => {
                    const recordDate = new Date(record.date);
                    return recordDate.getMonth() === currentMonth && recordDate.getFullYear() === currentYear;
                })
                .reduce((sum, record) => sum + (record.quantity - (record.losses || 0)), 0);
            
            document.getElementById('monthProduction').textContent = monthProduction;
            
            // Atualizar gráficos
            updateCharts();
        }
        
        // Atualizar gráficos
        function updateCharts() {
            // Dados para o gráfico de modelos
            const modelData = {};
            productionHistory.forEach(record => {
                modelData[record.model] = (modelData[record.model] || 0) + (record.quantity - (record.losses || 0));
            });
            
            const modelLabels = ['Bistro', 'Alegra', 'Folha', 'Coral'];
            const modelValues = ['bistro', 'alegra', 'folha', 'coral'].map(key => modelData[key] || 0);
            
            // Gráfico de modelos
            const modelCtx = document.getElementById('modelChart').getContext('2d');
            
            if (window.modelChart && typeof window.modelChart.destroy === 'function') {
                window.modelChart.destroy();
            }
            
            try {
                window.modelChart = new Chart(modelCtx, {
                    type: 'bar',
                    data: {
                        labels: modelLabels,
                        datasets: [{
                            label: 'Produção por Modelo',
                            data: modelValues,
                            backgroundColor: ['#ff6b6b', '#4ecdc4', '#ffe66d', '#ff9a76'],
                            borderColor: ['#ff5252', '#26a69a', '#ffd600', '#ff8a65'],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                ticks: {
                                    callback: function(value) {
                                        return value + ' unidades';
                                    }
                                }
                            }
                        }
                    }
                });
            } catch (error) {
                console.error('Erro ao criar gráfico de modelos:', error);
                window.modelChart = null;
            }
            
            // Dados para o gráfico de operadores
            const operatorData = {};
            productionHistory.forEach(record => {
                operatorData[record.operatorId] = (operatorData[record.operatorId] || 0) + (record.quantity - (record.losses || 0));
            });
            
            const operatorLabels = operators.map(op => op.name);
            const operatorValues = operators.map(op => operatorData[op.id] || 0);
            
            // Gráfico de operadores
            const operatorCtx = document.getElementById('operatorChart').getContext('2d');
            
            if (window.operatorChart && typeof window.operatorChart.destroy === 'function') {
                window.operatorChart.destroy();
            }
            
            try {
                window.operatorChart = new Chart(operatorCtx, {
                    type: 'bar',
                    data: {
                        labels: operatorLabels,
                        datasets: [{
                            label: 'Produção por Operador',
                            data: operatorValues,
                            backgroundColor: [
                                '#00a859',
                                '#6bc9a5',
                                '#ff6b6b',
                                '#4ecdc4',
                                '#ffe66d',
                                '#ff9a76',
                                '#a29bfe'
                            ],
                            borderColor: [
                                '#007a4a',
                                '#5aa085',
                                '#ff5252',
                                '#26a69a',
                                '#ffd600',
                                '#ff8a65',
                                '#6c5ce7'
                            ],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                ticks: {
                                    callback: function(value) {
                                        return value + ' unidades';
                                    }
                                }
                            }
                        }
                    }
                });
            } catch (error) {
                console.error('Erro ao criar gráfico de operadores:', error);
                window.operatorChart = null;
            }
        }
        
        // Atualizar exibição de avisos para operadores
        function updateWarningsDisplay(warningsText) {
            const warningsCard = document.querySelector('#warnings .card');
            if (warningsText) {
                const lines = warningsText.split('\n').filter(line => line.trim());
                let html = '<ul style="margin-top: 20px;">';
                lines.forEach(line => {
                    html += `<li>${line.trim()}</li>`;
                });
                html += '</ul>';
                warningsCard.innerHTML = '<p>Aqui você encontrará comunicados importantes da liderança sobre metas, procedimentos e atualizações.</p>' + html;
            } else {
                warningsCard.innerHTML = '<p>Aqui você encontrará comunicados importantes da liderança sobre metas, procedimentos e atualizações.</p><p style="color: #666;">Nenhum aviso disponível no momento.</p>';
            }
        }
        
        function getErrorMessage(errorCode) {
            const errors = {
                'auth/invalid-email': 'E-mail inválido.',
                'auth/user-disabled': 'Esta conta foi desativada.',
                'auth/user-not-found': 'Usuário não encontrado.',
                'auth/wrong-password': 'Senha incorreta.',
                'auth/too-many-requests': 'Muitas tentativas de login. Tente novamente mais tarde.'
            };
            return errors[errorCode] || 'Erro ao fazer login. Tente novamente.';
        }
        
        function showNotification(message, type) {
            // Criar notificação temporária
            const notification = document.createElement('div');
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                padding: 15px 20px;
                background-color: ${type === 'success' ? '#00a859' : '#dc3545'};
                color: white;
                border-radius: 6px;
                box-shadow: 0 4px 12px rgba(0,0,0,0.15);
                z-index: 1000;
                font-weight: 600;
                animation: slideIn 0.3s ease;
            `;
            
            notification.innerHTML = `
                <i class="fas fa-${type === 'success' ? 'check-circle' : 'exclamation-circle'}"></i>
                ${message}
            `;
            
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.style.animation = 'slideOut 0.3s ease';
                setTimeout(() => {
                    document.body.removeChild(notification);
                }, 300);
            }, 3000);
            
            // Adicionar estilos de animação se não existirem
            if (!document.getElementById('notification-styles')) {
                const style = document.createElement('style');
                style.id = 'notification-styles';
                style.textContent = `
                    @keyframes slideIn {
                        from { transform: translateX(100%); opacity: 0; }
                        to { transform: translateX(0); opacity: 1; }
                    }
                    @keyframes slideOut {
                        from { transform: translateX(0); opacity: 1; }
                        to { transform: translateX(100%); opacity: 0; }
                    }
                `;
                document.head.appendChild(style);
            }
        }
        
        // Funções de administração
        window.editProductionRecord = async function(recordId) {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem editar registros.', 'error');
                return;
            }
            
            const record = productionHistory.find(r => r.id === recordId);
            if (!record) {
                showNotification('Registro não encontrado.', 'error');
                return;
            }
            
            // Preencher formulário de edição
            document.getElementById('editProductionId').value = recordId;
            document.getElementById('editOperator').value = record.operatorId;
            document.getElementById('editModel').value = record.model;
            document.getElementById('editQuantity').value = record.quantity;
            document.getElementById('editLosses').value = record.losses || 0;
            document.getElementById('editDate').value = record.date;
            document.getElementById('editNotes').value = record.notes || '';
            
            // Mostrar formulário
            document.getElementById('editProductionForm').classList.remove('hidden');
            
            // Ir para aba de administração
            switchTab('admin');
        };
        
        window.deleteProductionRecord = async function(recordId) {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem excluir registros.', 'error');
                return;
            }
            
            if (!confirm('Tem certeza que deseja excluir este registro de produção?')) {
                return;
            }
            
            try {
                await db.collection('production').doc(recordId).delete();
                
                // Remover da lista local
                productionHistory = productionHistory.filter(r => r.id !== recordId);
                
                // Atualizar interface
                updateHistoryList();
                updateDashboard();
                
                showNotification('Registro excluído com sucesso!', 'success');
            } catch (error) {
                console.error('Erro ao excluir registro:', error);
                showNotification('Erro ao excluir registro', 'error');
            }
        };
        
        // Atualizar registro de produção
        document.getElementById('updateProduction').addEventListener('click', async () => {
            const recordId = document.getElementById('editProductionId').value;
            
            if (!recordId) {
                showNotification('Nenhum registro selecionado para edição.', 'error');
                return;
            }
            
            const operatorId = document.getElementById('editOperator').value;
            const model = document.getElementById('editModel').value;
            const quantity = parseInt(document.getElementById('editQuantity').value);
            const losses = parseInt(document.getElementById('editLosses').value) || 0;
            const date = document.getElementById('editDate').value;
            const notes = document.getElementById('editNotes').value;
            
            if (!operatorId || !model || !quantity || !date) {
                showNotification('Por favor, preencha todos os campos obrigatórios.', 'error');
                return;
            }
            
            try {
                // Encontrar o operador
                const operator = operators.find(op => op.id === operatorId);
                
                const updateData = {
                    operatorId: operatorId,
                    operatorName: operator ? operator.name : 'Operador Desconhecido',
                    model: model,
                    quantity: quantity,
                    losses: losses,
                    date: date,
                    notes: notes,
                    updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
                    updatedBy: currentUser.uid
                };
                
                await db.collection('production').doc(recordId).update(updateData);
                
                // Atualizar no array local
                const recordIndex = productionHistory.findIndex(r => r.id === recordId);
                if (recordIndex !== -1) {
                    productionHistory[recordIndex] = {
                        ...productionHistory[recordIndex],
                        ...updateData
                    };
                }
                
                // Atualizar interface
                updateHistoryList();
                updateDashboard();
                
                // Limpar formulário
                document.getElementById('editProductionForm').classList.add('hidden');
                document.getElementById('editProductionId').value = '';
                
                showNotification('Registro atualizado com sucesso!', 'success');
                
            } catch (error) {
                console.error('Erro ao atualizar registro:', error);
                showNotification('Erro ao atualizar registro', 'error');
            }
        });
        
        // Salvar configurações
        document.getElementById('saveSettings').addEventListener('click', async () => {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem alterar configurações.', 'error');
                return;
            }
            
            try {
                               const settings = {
                    bistroTarget: parseInt(document.getElementById('bistroTarget').value) || 50,
                    alegraTarget: parseInt(document.getElementById('alegraTarget').value) || 40,
                    folhaTarget: parseInt(document.getElementById('folhaTarget').value) || 30,
                    coralTarget: parseInt(document.getElementById('coralTarget').value) || 20,
                    efficiencyGoal: parseInt(document.getElementById('efficiencyGoal').value) || 85,
                    updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
                    updatedBy: currentUser.uid
                };
                
                await db.collection('settings').doc('production').set(settings, { merge: true });
                
                systemSettings = settings;
                
                showNotification('Configurações salvas com sucesso!', 'success');
                updateDashboard();
                
            } catch (error) {
                console.error('Erro ao salvar configurações:', error);
                showNotification('Erro ao salvar configurações', 'error');
            }
        });
        
        // Salvar avisos
        document.getElementById('saveWarnings').addEventListener('click', async () => {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem alterar avisos.', 'error');
                return;
            }
            
            try {
                const warningsText = document.getElementById('warningsText').value.trim();
                
                await db.collection('settings').doc('warnings').set({
                    text: warningsText,
                    updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
                    updatedBy: currentUser.uid
                }, { merge: true });
                
                showNotification('Avisos salvos com sucesso!', 'success');
                
            } catch (error) {
                console.error('Erro ao salvar avisos:', error);
                showNotification('Erro ao salvar avisos', 'error');
            }
        });
        
        // Gerar relatório
        document.getElementById('generateReport').addEventListener('click', async () => {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem gerar relatórios.', 'error');
                return;
            }
            
            const reportType = document.getElementById('reportType').value;
            const startDate = document.getElementById('reportStartDate').value;
            const endDate = document.getElementById('reportEndDate').value;
            const operatorId = document.getElementById('reportOperator').value;
            const model = document.getElementById('reportModel').value;
            
            // Filtrar dados
            let filteredData = productionHistory;
            
            if (operatorId !== 'all') {
                filteredData = filteredData.filter(record => record.operatorId === operatorId);
            }
            
            if (model !== 'all') {
                filteredData = filteredData.filter(record => record.model === model);
            }
            
            // Filtrar por data baseado no tipo de relatório
            const end = new Date(endDate);
            const start = new Date(startDate);
            
            filteredData = filteredData.filter(record => {
                const recordDate = new Date(record.date);
                return recordDate >= start && recordDate <= end;
            });
            
            // Calcular estatísticas
            const totalProduction = filteredData.reduce((sum, record) => sum + (record.quantity - (record.losses || 0)), 0);
            const avgDailyProduction = filteredData.length > 0 ? 
                totalProduction / filteredData.length : 0;
            
            const operatorStats = {};
            filteredData.forEach(record => {
                if (!operatorStats[record.operatorId]) {
                    operatorStats[record.operatorId] = {
                        name: record.operatorName,
                        total: 0,
                        count: 0
                    };
                }
                operatorStats[record.operatorId].total += (record.quantity - (record.losses || 0));
                operatorStats[record.operatorId].count++;
            });
            
            const modelStats = {};
            filteredData.forEach(record => {
                modelStats[record.model] = (modelStats[record.model] || 0) + (record.quantity - (record.losses || 0));
            });
            
            // Gerar HTML do relatório
            let reportHTML = `
                <h3>Relatório de Produção</h3>
                <p><strong>Período:</strong> ${startDate} a ${endDate}</p>
                <p><strong>Total de registros:</strong> ${filteredData.length}</p>
                <p><strong>Produção total:</strong> ${totalProduction} unidades</p>
                <p><strong>Média diária:</strong> ${avgDailyProduction.toFixed(2)} unidades</p>
                
                <h4 style="margin-top: 20px;">Produção por Operador</h4>
                <table class="admin-table">
                    <thead>
                        <tr>
                            <th>Operador</th>
                            <th>Produção Total</th>
                            <th>Registros</th>
                            <th>Média por Registro</th>
                        </tr>
                    </thead>
                    <tbody>
            `;
            
            Object.values(operatorStats).forEach(stat => {
                reportHTML += `
                    <tr>
                        <td>${stat.name}</td>
                        <td>${stat.total} unidades</td>
                        <td>${stat.count}</td>
                        <td>${(stat.total / stat.count).toFixed(2)} unidades</td>
                    </tr>
                `;
            });
            
            reportHTML += `

                    </tbody>
                </table>
                
                <h4 style="margin-top: 20px;">Produção por Modelo</h4>
                <table class="admin-table">
                    <thead>
                        <tr>
                            <th>Modelo</th>
                            <th>Produção Total</th>
                            <th>Percentual</th>
                        </tr>
                    </thead>
                    <tbody>
            `;
            
            Object.entries(modelStats).forEach(([model, total]) => {
                const percentage = total > 0 ? ((total / totalProduction) * 100).toFixed(1) : 0;
                reportHTML += `
                    <tr>
                        <td>${getModelName(model)}</td>
                        <td>${total} unidades</td>
                        <td>${percentage}%</td>
                    </tr>
                `;
            });
            
            reportHTML += `
                    </tbody>
                </table>
            `;
            
            document.getElementById('reportResult').innerHTML = reportHTML;
            showNotification('Relatório gerado com sucesso!', 'success');
        });
        
        // Exportar relatório para Excel (simulação)
        document.getElementById('exportReport').addEventListener('click', () => {
            if (userRole !== 'admin') {
                showNotification('Apenas administradores podem exportar relatórios.', 'error');
                return;
            }
            
            showNotification('Funcionalidade de exportação em desenvolvimento.', 'info');
        });
        
        // Buscar registro para edição
        document.getElementById('searchProduction').addEventListener('click', () => {
            const recordId = document.getElementById('editProductionId').value.trim();
            
            if (!recordId) {
                showNotification('Digite o ID do registro que deseja editar.', 'error');
                return;
            }
            
            const record = productionHistory.find(r => r.id === recordId);
            
            if (!record) {
                showNotification('Registro não encontrado. Verifique o ID.', 'error');
                return;
            }
            
            // Preencher formulário
            document.getElementById('editOperator').value = record.operatorId;
            document.getElementById('editModel').value = record.model;
            document.getElementById('editQuantity').value = record.quantity;
            document.getElementById('editLosses').value = record.losses || 0;
            document.getElementById('editDate').value = record.date;
            document.getElementById('editNotes').value = record.notes || '';
            
            // Mostrar formulário
            document.getElementById('editProductionForm').classList.remove('hidden');
            
            showNotification('Registro encontrado. Você pode editá-lo agora.', 'success');
        });
        
        // Inicialização do Firebase Auth
        auth.onAuthStateChanged((user) => {
            if (user) {
                // Usuário já está logado
                currentUser = user;
                
                // Carregar dados do usuário
                db.collection('users').doc(user.uid).get().then((doc) => {
                    if (doc.exists) {
                        const userData = doc.data();
                        userRole = userData.role || (user.email === 'souza73e@gmail.com' ? 'admin' : 'operador');
                        
                        document.getElementById('userName').textContent = userData.name || user.email.split('@')[0];
                        document.getElementById('userRole').textContent = userRole === 'admin' ? 'Administrador' : 'Operador';
                        document.getElementById('userAvatar').textContent = user.email.charAt(0).toUpperCase();
                        
                        setupUserPermissions();
                        loadSystemData();
                        
                        document.getElementById('loginScreen').classList.add('hidden');
                        document.getElementById('mainApp').classList.remove('hidden');
                    }
                });
            }
        });
        
        // Adicionar estilo para badges de status
        const style = document.createElement('style');
        style.textContent = `
            .status-badge {
                padding: 4px 8px;
                border-radius: 12px;
                font-size: 0.8rem;
                font-weight: 600;
            }
            
            .status-badge.ativo {
                background-color: #d4edda;
                color: #155724;
            }
            
            .status-badge.inativo {
                background-color: #f8d7da;
                color: #721c24;
            }
            
            .status-badge.ferias {
                background-color: #fff3cd;
                color: #856404;
            }
        `;
        document.head.appendChild(style);
        
        // Nota: Para configurar o Firebase corretamente, você precisa:
        // 1. Criar um projeto no Firebase Console
        // 2. Ativar Authentication (com provedor Email/Senha)
        // 3. Ativar Firestore Database
        // 4. Configurar as regras de segurança fornecidas
        // 5. Adicionar usuários manualmente no Firestore:
        //    - Coleção "users" com documentos contendo: email, name, role ("admin" ou "operador")
        //    - Coleção "operators" para cadastrar os operadores
        //    - Coleção "production" para os registros de produção
        //    - Coleção "settings" para configurações do sistema
    </script>
</body>
</html>
