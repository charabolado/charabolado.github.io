<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚡ Calculadora de Mana - Blindones [TODOS OS SONS]</title>
    <link rel="manifest" href="manifest.json">
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-primary: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            --bg-card: rgba(255, 255, 255, 0.98);
            --text-primary: #1a1a2e;
            --text-secondary: #64748b;
            --primary: #667eea;
            --secondary: #764ba2;
            --success: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --border: #e2e8f0;
            --shadow: 0 20px 60px rgba(0,0,0,0.3);
            --section-bg: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
        }
        [data-theme="dark"] {
            --bg-primary: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            --bg-card: rgba(30, 30, 46, 0.98);
            --text-primary: #e2e8f0;
            --text-secondary: #94a3b8;
            --border: #374151;
            --section-bg: linear-gradient(135deg, #1f2937 0%, #111827 100%);
        }
        * { margin:0; padding:0; box-sizing:border-box; }
        body { font-family:'Inter',sans-serif; background:var(--bg-primary); min-height:100vh; padding:20px; color:var(--text-primary); transition:background 0.3s,color 0.3s; }

        /* VERSION BADGE */
        .version-badge {
            position: fixed; top: 80px; right: 20px; z-index: 1001;
            background: linear-gradient(135deg, #7c3aed, #6d28d9);
            color: white; padding: 8px 16px; border-radius: 100px;
            font-size: 0.78em; font-weight: 800; letter-spacing: 0.05em;
            box-shadow: 0 4px 20px rgba(124,58,237,0.5);
            font-family: 'Orbitron', sans-serif;
            animation: badge-pulse 2s ease-in-out infinite;
        }
        @keyframes badge-pulse {
            0%,100%{ box-shadow: 0 4px 20px rgba(239,68,68,0.5); }
            50%{ box-shadow: 0 4px 30px rgba(124,58,237,0.9); }
        }

        #bg-status-bar { position:fixed; bottom:0; left:0; right:0; padding:10px 20px; background:rgba(0,0,0,0.85); color:#fff; font-size:0.82em; font-family:'Orbitron',monospace; display:flex; align-items:center; gap:12px; z-index:9999; backdrop-filter:blur(10px); border-top:1px solid rgba(255,255,255,0.1); }
        #bg-status-bar .dot { width:8px; height:8px; border-radius:50%; flex-shrink:0; animation:pulse-dot 2s ease-in-out infinite; }
        .dot-green { background:#10b981; box-shadow:0 0 8px #10b981; }
        .dot-yellow { background:#f59e0b; box-shadow:0 0 8px #f59e0b; }
        .dot-red { background:#ef4444; box-shadow:0 0 8px #ef4444; }
        @keyframes pulse-dot { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:0.5;transform:scale(0.7)} }
        #bg-status-text { flex:1; }
        #bg-enable-btn { padding:6px 14px; border-radius:8px; border:none; font-size:0.85em; font-weight:700; cursor:pointer; background:#667eea; color:white; transition:all 0.2s; font-family:'Orbitron',monospace; }
        #bg-enable-btn:hover { background:#5568d3; transform:scale(1.05); }
        #bg-enable-btn.hidden { display:none; }

        #toast-container { position:fixed; top:20px; left:50%; transform:translateX(-50%); z-index:10000; display:flex; flex-direction:column; gap:10px; pointer-events:none; width:min(500px,90vw); }
        .toast { background:#1a1a2e; color:white; padding:16px 22px; border-radius:14px; border-left:4px solid #10b981; box-shadow:0 8px 32px rgba(0,0,0,0.5); font-weight:600; font-size:1em; pointer-events:all; animation:toast-in 0.4s cubic-bezier(0.34,1.56,0.64,1) forwards; display:flex; align-items:center; gap:12px; }
        .toast.warning { border-left-color:#f59e0b; }
        .toast.error { border-left-color:#ef4444; }
        @keyframes toast-in { from{opacity:0;transform:translateY(-20px) scale(0.9)} to{opacity:1;transform:translateY(0) scale(1)} }
        @keyframes toast-out { from{opacity:1;transform:translateY(0) scale(1)} to{opacity:0;transform:translateY(-20px) scale(0.9)} }

        .settings-toggle { position:fixed; top:20px; right:20px; z-index:1000; display:flex; gap:10px; }
        .settings-btn { width:50px; height:50px; border-radius:50%; background:var(--bg-card); border:2px solid var(--border); cursor:pointer; display:flex; align-items:center; justify-content:center; font-size:1.5em; transition:all 0.3s; box-shadow:0 4px 12px rgba(0,0,0,0.15); }
        .settings-btn:hover { transform:translateY(-2px); box-shadow:0 8px 20px rgba(0,0,0,0.25); }
        .settings-panel { position:fixed; top:0; right:-400px; width:400px; height:100vh; background:var(--bg-card); box-shadow:-5px 0 30px rgba(0,0,0,0.3); padding:30px; z-index:999; transition:right 0.4s cubic-bezier(0.4,0,0.2,1); overflow-y:auto; }
        .settings-panel.active { right:0; }
        .settings-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:30px; padding-bottom:20px; border-bottom:2px solid var(--border); }
        .settings-header h2 { font-family:'Orbitron',sans-serif; font-size:1.8em; color:var(--primary); }
        .close-settings { background:none; border:none; font-size:2em; cursor:pointer; color:var(--text-secondary); transition:color 0.2s; }
        .close-settings:hover { color:var(--danger); }
        .setting-group { margin-bottom:30px; }
        .setting-group h3 { font-size:1.1em; margin-bottom:15px; color:var(--text-primary); font-weight:600; }
        .setting-item { display:flex; justify-content:space-between; align-items:center; padding:15px; background:var(--section-bg); border-radius:12px; margin-bottom:10px; transition:all 0.2s; }
        .setting-item:hover { transform:translateX(5px); }
        .toggle-switch { position:relative; width:60px; height:30px; }
        .toggle-switch input { opacity:0; width:0; height:0; }
        .toggle-slider { position:absolute; cursor:pointer; top:0; left:0; right:0; bottom:0; background-color:#cbd5e1; transition:0.4s; border-radius:30px; }
        .toggle-slider:before { position:absolute; content:""; height:22px; width:22px; left:4px; bottom:4px; background:white; transition:0.4s; border-radius:50%; }
        input:checked + .toggle-slider { background:linear-gradient(135deg,var(--primary),var(--secondary)); }
        input:checked + .toggle-slider:before { transform:translateX(30px); }
        .color-picker-group { display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-top:10px; }
        .color-option { width:100%; aspect-ratio:1; border-radius:12px; border:3px solid transparent; cursor:pointer; transition:all 0.2s; position:relative; }
        .color-option:hover { transform:scale(1.1); border-color:var(--text-primary); }
        .color-option.active { border-color:var(--text-primary); box-shadow:0 0 0 4px rgba(102,126,234,0.2); }
        .color-option::after { content:'✓'; position:absolute; top:50%; left:50%; transform:translate(-50%,-50%); color:white; font-size:1.5em; font-weight:bold; opacity:0; transition:opacity 0.2s; }
        .color-option.active::after { opacity:1; }
        .icon-picker-group { display:grid; grid-template-columns:repeat(5,1fr); gap:10px; margin-top:10px; }
        .icon-option { aspect-ratio:1; border-radius:12px; border:2px solid var(--border); cursor:pointer; display:flex; align-items:center; justify-content:center; font-size:1.5em; transition:all 0.2s; background:var(--section-bg); }
        .icon-option:hover { transform:scale(1.1); border-color:var(--primary); }
        .icon-option.active { background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border-color:transparent; }

        .main-container { max-width:1400px; margin:0 auto; }
        .header { text-align:center; margin-bottom:40px; }
        h1 { font-family:'Orbitron',sans-serif; color:var(--text-primary); font-size:3.5em; font-weight:900; text-shadow:0 4px 12px rgba(0,0,0,0.2); letter-spacing:2px; margin-bottom:15px; }
        .subtitle { color:var(--text-secondary); font-size:1.2em; font-weight:300; }
        .tabs-controls { display:flex; flex-direction:column; gap:15px; margin-bottom:30px; }
        .tab-row { display:flex; gap:15px; align-items:center; flex-wrap:wrap; background:var(--bg-card); padding:15px; border-radius:15px; backdrop-filter:blur(20px); border:2px solid var(--border); transition:all 0.3s; }
        .tab-row:hover { transform:translateY(-2px); box-shadow:var(--shadow); }
        .tab-icon { width:45px; height:45px; border-radius:12px; display:flex; align-items:center; justify-content:center; font-size:1.5em; background:var(--section-bg); flex-shrink:0; }
        .tab-button { padding:12px 28px; background:var(--section-bg); color:var(--text-primary); border:2px solid var(--border); border-radius:12px; cursor:pointer; transition:all 0.3s; font-weight:600; font-size:1em; }
        .tab-button:hover { transform:translateY(-2px); box-shadow:0 8px 20px rgba(0,0,0,0.15); }
        .tab-button.active { background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border-color:transparent; }
        .add-tab-btn { padding:15px 30px; background:linear-gradient(135deg,var(--success),#059669); color:white; border:none; border-radius:12px; cursor:pointer; font-weight:700; font-size:1.1em; transition:all 0.3s; box-shadow:0 8px 20px rgba(16,185,129,0.3); }
        .add-tab-btn:hover { transform:translateY(-3px); box-shadow:0 12px 30px rgba(16,185,129,0.5); }
        .remove-tab-btn { padding:12px 24px; background:linear-gradient(135deg,var(--danger),#dc2626); color:white; border:none; border-radius:12px; cursor:pointer; font-weight:600; transition:all 0.3s; }
        .remove-tab-btn:hover { transform:translateY(-2px); box-shadow:0 8px 20px rgba(239,68,68,0.4); }
        .container { background:var(--bg-card); border-radius:25px; padding:40px; box-shadow:var(--shadow); display:none; }
        .container.active { display:block; }
        .section { margin-bottom:35px; padding:30px; background:var(--section-bg); border-radius:20px; border:2px solid var(--border); transition:all 0.3s; }
        .section:hover { transform:translateY(-2px); box-shadow:0 12px 30px rgba(0,0,0,0.08); }
        .section-title { font-family:'Orbitron',sans-serif; font-size:1.6em; color:var(--primary); margin-bottom:20px; font-weight:700; display:flex; align-items:center; gap:10px; }
        .tab-name-input { flex:1; padding:14px 20px; border:2px solid var(--border); background:var(--bg-card); color:var(--text-primary); border-radius:12px; font-size:1.1em; font-weight:600; transition:all 0.3s; }
        .tab-name-input:focus { outline:none; border-color:var(--primary); box-shadow:0 0 0 4px rgba(102,126,234,0.1); }
        .promotion-options { display:grid; grid-template-columns:repeat(auto-fit,minmax(200px,1fr)); gap:20px; }
        .promotion-btn { padding:20px; border:3px solid var(--border); background:var(--bg-card); border-radius:16px; cursor:pointer; transition:all 0.3s; text-align:center; }
        .promotion-btn:hover { transform:translateY(-4px); box-shadow:0 12px 30px rgba(102,126,234,0.2); border-color:var(--primary); }
        .promotion-btn.active { background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border-color:transparent; box-shadow:0 8px 25px rgba(102,126,234,0.4); }
        .promotion-btn div { font-size:1.3em; font-weight:700; margin-bottom:8px; }
        .promotion-btn small { font-size:0.95em; opacity:0.9; }
        .runes-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:20px; }
        .rune-option { display:flex; align-items:center; gap:15px; padding:20px; background:var(--bg-card); border:3px solid var(--border); border-radius:16px; cursor:pointer; transition:all 0.3s; }
        .rune-option:hover { transform:translateY(-4px); box-shadow:0 12px 30px rgba(102,126,234,0.2); border-color:var(--primary); }
        .rune-option.selected { background:linear-gradient(135deg,#dbeafe,#bfdbfe); border-color:var(--primary); box-shadow:0 0 20px rgba(102,126,234,0.3); }
        [data-theme="dark"] .rune-option.selected { background:linear-gradient(135deg,#1e3a8a,#1e40af); }
        .rune-image { width:60px; height:60px; border-radius:12px; display:flex; align-items:center; justify-content:center; flex-shrink:0; overflow:hidden; border:2px solid var(--border); background:var(--section-bg); }
        .rune-image img { width:100%; height:100%; object-fit:contain; }
        .rune-info { flex:1; }
        .rune-name { font-weight:700; color:var(--primary); font-size:1.2em; margin-bottom:4px; }
        .rune-mana { color:var(--text-secondary); font-size:0.95em; }
        .rune-time { color:var(--secondary); font-size:0.9em; font-weight:600; margin-top:6px; }
        .sound-selector { display:grid; grid-template-columns:repeat(auto-fit,minmax(180px,1fr)); gap:15px; }
        .sound-option { position:relative; padding:20px; background:var(--bg-card); border:3px solid var(--border); border-radius:16px; cursor:pointer; transition:all 0.3s; text-align:center; }
        .sound-option:hover { transform:translateY(-3px); box-shadow:0 8px 20px rgba(0,0,0,0.1); border-color:var(--primary); }
        .sound-option.active { background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border-color:transparent; box-shadow:0 8px 25px rgba(102,126,234,0.4); }
        .sound-name { font-weight:600; font-size:1.05em; margin-bottom:10px; }
        .volume-control { display:flex; align-items:center; gap:8px; margin-top:10px; }
        .volume-slider { flex:1; height:6px; border-radius:3px; background:rgba(0,0,0,0.1); outline:none; -webkit-appearance:none; }
        .sound-option.active .volume-slider { background:rgba(255,255,255,0.3); }
        .volume-slider::-webkit-slider-thumb { -webkit-appearance:none; width:16px; height:16px; border-radius:50%; background:var(--primary); cursor:pointer; }
        .sound-option.active .volume-slider::-webkit-slider-thumb { background:white; }
        .volume-label { font-size:0.85em; font-weight:600; min-width:35px; }
        .timer-section { text-align:center; padding:40px; background:linear-gradient(135deg,var(--primary) 0%,var(--secondary) 100%); border-radius:20px; color:white; box-shadow:0 8px 30px rgba(102,126,234,0.4); }
        .timer-display { font-family:'Orbitron',monospace; font-size:4em; font-weight:900; margin:30px 0; text-shadow:0 0 30px rgba(255,255,255,0.5); letter-spacing:8px; }
        .timer-display.finishing { animation:flash-timer 0.5s ease-in-out infinite; }
        @keyframes flash-timer { 0%,100%{opacity:1} 50%{opacity:0.3} }
        .timer-progress { width:100%; height:8px; background:rgba(255,255,255,0.2); border-radius:4px; margin-bottom:25px; overflow:hidden; }
        .timer-progress-bar { height:100%; background:rgba(255,255,255,0.8); border-radius:4px; transition:width 0.5s linear; box-shadow:0 0 10px rgba(255,255,255,0.5); }
        .timer-status { font-size:1em; font-weight:700; margin-bottom:15px; padding:8px 20px; border-radius:20px; display:inline-block; }
        .timer-status.running { background:rgba(16,185,129,0.3); border:1px solid rgba(16,185,129,0.5); }
        .timer-status.paused { background:rgba(245,158,11,0.3); border:1px solid rgba(245,158,11,0.5); }
        .timer-status.stopped { background:rgba(255,255,255,0.1); border:1px solid rgba(255,255,255,0.2); }
        .timer-controls { display:flex; gap:20px; justify-content:center; flex-wrap:wrap; }
        .btn { padding:16px 40px; border:none; border-radius:12px; font-size:1.2em; font-weight:700; cursor:pointer; transition:all 0.3s; text-transform:uppercase; letter-spacing:1px; }
        .btn-start { background:linear-gradient(135deg,var(--success),#059669); color:white; box-shadow:0 8px 20px rgba(16,185,129,0.4); }
        .btn-start:hover { transform:translateY(-3px); box-shadow:0 12px 30px rgba(16,185,129,0.6); }
        .btn-stop { background:linear-gradient(135deg,var(--danger),#dc2626); color:white; box-shadow:0 8px 20px rgba(239,68,68,0.4); }
        .btn-stop:hover { transform:translateY(-3px); box-shadow:0 12px 30px rgba(239,68,68,0.6); }
        .btn-reset { background:linear-gradient(135deg,var(--warning),#d97706); color:white; box-shadow:0 8px 20px rgba(245,158,11,0.4); }
        .btn-reset:hover { transform:translateY(-3px); box-shadow:0 12px 30px rgba(245,158,11,0.6); }
        .custom-mana { display:flex; gap:15px; align-items:center; margin-top:20px; }
        .custom-mana input { flex:1; padding:16px 20px; border:3px solid var(--border); border-radius:12px; font-size:1.1em; background:var(--bg-card); color:var(--text-primary); transition:all 0.3s; }
        .custom-mana input:focus { outline:none; border-color:var(--primary); box-shadow:0 0 0 4px rgba(102,126,234,0.1); }
        .custom-mana button { padding:16px 32px; background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border:none; border-radius:12px; cursor:pointer; font-weight:700; font-size:1.1em; transition:all 0.3s; }
        .custom-mana button:hover { transform:translateY(-2px); box-shadow:0 8px 25px rgba(102,126,234,0.4); }
        .settings-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(280px,1fr)); gap:20px; }
        .setting-option { display:flex; align-items:center; gap:12px; padding:18px 20px; background:var(--bg-card); border:3px solid var(--border); border-radius:12px; cursor:pointer; transition:all 0.3s; }
        .setting-option:hover { border-color:var(--primary); transform:translateY(-2px); }
        .setting-option input[type="checkbox"] { width:24px; height:24px; cursor:pointer; accent-color:var(--primary); }
        .setting-option label { cursor:pointer; font-size:1.05em; font-weight:600; }
        .alert { position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); background:var(--bg-card); padding:50px; border-radius:25px; box-shadow:0 30px 80px rgba(0,0,0,0.5); z-index:10001; text-align:center; display:none; min-width:400px; }
        .alert h2 { font-family:'Orbitron',sans-serif; color:var(--success); font-size:2.5em; margin-bottom:15px; }
        .alert p { color:var(--text-primary); font-size:1.4em; font-weight:600; }
        .overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.8); backdrop-filter:blur(5px); z-index:10000; display:none; }
        @media(max-width:768px) { h1{font-size:2.2em} .timer-display{font-size:2.5em} .container{padding:25px} .section{padding:20px} .settings-panel{width:100%;right:-100%} }
        .color-blue{background:linear-gradient(135deg,#667eea,#764ba2) !important}
        .color-green{background:linear-gradient(135deg,#10b981,#059669) !important}
        .color-red{background:linear-gradient(135deg,#ef4444,#dc2626) !important}
        .color-orange{background:linear-gradient(135deg,#f59e0b,#d97706) !important}
        .color-purple{background:linear-gradient(135deg,#8b5cf6,#7c3aed) !important}
        .color-pink{background:linear-gradient(135deg,#ec4899,#db2777) !important}
        .color-cyan{background:linear-gradient(135deg,#06b6d4,#0891b2) !important}
        .color-emerald{background:linear-gradient(135deg,#34d399,#10b981) !important}
        #body-spacer{height:50px}
    </style>
</head>
<body>
    <div id="toast-container"></div>
    <div class="version-badge">🎵 20 SONS</div>

    <div class="settings-toggle">
        <button class="settings-btn" onclick="toggleSettings()" title="Configurações">⚙️</button>
        <button class="settings-btn" onclick="toggleDarkMode()" id="themeBtn" title="Alternar Tema">🌙</button>
    </div>

    <div class="settings-panel" id="settingsPanel">
        <div class="settings-header">
            <h2>⚙️ Configurações</h2>
            <button class="close-settings" onclick="toggleSettings()">×</button>
        </div>
        <div class="setting-group">
            <h3>🎨 Personalizar Aba Ativa</h3>
            <div class="setting-item"><span>Cor da Aba</span></div>
            <div class="color-picker-group">
                <div class="color-option color-blue active" onclick="setTabColor('blue')" data-color="blue"></div>
                <div class="color-option color-green" onclick="setTabColor('green')" data-color="green"></div>
                <div class="color-option color-red" onclick="setTabColor('red')" data-color="red"></div>
                <div class="color-option color-orange" onclick="setTabColor('orange')" data-color="orange"></div>
                <div class="color-option color-purple" onclick="setTabColor('purple')" data-color="purple"></div>
                <div class="color-option color-pink" onclick="setTabColor('pink')" data-color="pink"></div>
                <div class="color-option color-cyan" onclick="setTabColor('cyan')" data-color="cyan"></div>
                <div class="color-option color-emerald" onclick="setTabColor('emerald')" data-color="emerald"></div>
            </div>
            <div class="setting-item" style="margin-top:20px;"><span>Ícone da Aba</span></div>
            <div class="icon-picker-group">
                <div class="icon-option active" onclick="setTabIcon('⚡')" data-icon="⚡">⚡</div>
                <div class="icon-option" onclick="setTabIcon('🔥')" data-icon="🔥">🔥</div>
                <div class="icon-option" onclick="setTabIcon('💎')" data-icon="💎">💎</div>
                <div class="icon-option" onclick="setTabIcon('⭐')" data-icon="⭐">⭐</div>
                <div class="icon-option" onclick="setTabIcon('🎯')" data-icon="🎯">🎯</div>
                <div class="icon-option" onclick="setTabIcon('🏆')" data-icon="🏆">🏆</div>
                <div class="icon-option" onclick="setTabIcon('🎮')" data-icon="🎮">🎮</div>
                <div class="icon-option" onclick="setTabIcon('🚀')" data-icon="🚀">🚀</div>
                <div class="icon-option" onclick="setTabIcon('✨')" data-icon="✨">✨</div>
                <div class="icon-option" onclick="setTabIcon('🌟')" data-icon="🌟">🌟</div>
            </div>
        </div>
        <div class="setting-group">
            <h3>🔔 Notificações em Segundo Plano</h3>
            <div class="setting-item">
                <span>Ativar Notificações</span>
                <label class="toggle-switch">
                    <input type="checkbox" id="notificationsToggle" onchange="toggleNotifications()">
                    <span class="toggle-slider"></span>
                </label>
            </div>
        </div>
        <div class="setting-group">
            <h3>🌙 Aparência</h3>
            <div class="setting-item">
                <span>Modo Escuro</span>
                <label class="toggle-switch">
                    <input type="checkbox" id="darkModeToggle" onchange="toggleDarkMode()">
                    <span class="toggle-slider"></span>
                </label>
            </div>
        </div>
    </div>

    <div class="main-container">
        <div class="header">
            <h1>⚡ CALCULADORA DE MANA</h1>
            <p class="subtitle">Sistema Profissional de Regeneração - Blindones</p>
        </div>
        <div class="tabs-controls" id="tabsControls">
            <button class="add-tab-btn" onclick="addNewTab()">➕ ADICIONAR NOVA ABA</button>
        </div>
        <div id="tabsContent"></div>
    </div>

    <div id="body-spacer"></div>
    <div id="bg-status-bar">
        <div class="dot dot-yellow" id="status-dot"></div>
        <span id="bg-status-text">Inicializando...</span>
        <button id="bg-enable-btn" class="hidden" onclick="enableBackground()">ATIVAR SEGUNDO PLANO</button>
    </div>
    <div class="overlay" id="overlay" onclick="closeAlert()"></div>
    <div class="alert" id="alert">
        <h2>✅ RUNA PRONTA!</h2>
        <p id="alertMessage">Sua runa está pronta!</p>
        <button class="btn btn-start" onclick="closeAlert()" style="margin-top:25px;">FECHAR</button>
    </div>

    <script id="worker-code" type="text/worker">
        let timers = {};
        let checkInterval = null;
        self.onmessage = function(e) {
            const { type, payload } = e.data;
            switch(type) {
                case 'SET_TIMER':
                    timers[payload.tabId] = payload;
                    if (!checkInterval) startChecking();
                    break;
                case 'CLEAR_TIMER':
                    delete timers[payload.tabId];
                    if (Object.keys(timers).length === 0) stopChecking();
                    break;
                case 'CLEAR_ALL':
                    timers = {};
                    stopChecking();
                    break;
            }
        };
        function startChecking() {
            if (checkInterval) return;
            checkInterval = setInterval(checkAll, 500);
        }
        function stopChecking() {
            if (checkInterval) { clearInterval(checkInterval); checkInterval = null; }
        }
        function checkAll() {
            const now = Date.now();
            for (const tabId in timers) {
                const timer = timers[tabId];
                const elapsed = now - timer.startTimestamp;
                const remaining = timer.targetMs - elapsed;
                self.postMessage({ type:'TICK', tabId:parseInt(tabId), elapsed:Math.floor(elapsed/1000), remaining:Math.max(0,Math.floor(remaining/1000)), progress:Math.min(1,elapsed/timer.targetMs) });
                if (elapsed >= timer.targetMs && !timer.fired) {
                    timer.fired = true;
                    self.postMessage({ type:'TIMER_DONE', tabId:parseInt(tabId), name:timer.name, runeName:timer.runeName, sound:timer.sound, volumes:timer.volumes });
                    if (timer.autoReset) { timer.startTimestamp = Date.now(); timer.fired = false; }
                    else { delete timers[tabId]; if (Object.keys(timers).length === 0) stopChecking(); }
                }
            }
        }
    </script>

    <script>
    'use strict';

    let tabs = [], activeTabId = null, tabCounter = 0;
    let notificationsEnabled = false, darkMode = false;
    let bgWorker = null, serviceWorkerReady = false;
    let audioContext = null, pendingAlerts = [];

    // ── TODOS OS 20 SONS (15 originais + 5 novos ultra altos) ───────────────
    const soundConfigs = {
        // ── ORIGINAIS ──────────────────────────────────────────────────────
        'emergency_siren':  { name: '🚨 Sirene Emergência',  volume: 0.85, pattern: [{freq:800,d:0.15},{freq:1200,d:0.15},{freq:800,d:0.15},{freq:1200,d:0.15},{freq:800,d:0.15}] },
        'alarm_clock':      { name: '⏰ Despertador',         volume: 0.9,  pattern: [{freq:1400,d:0.1},{freq:1400,d:0.1},{freq:0,d:0.05},{freq:1400,d:0.1},{freq:1400,d:0.1}] },
        'air_horn':         { name: '📯 Buzina Potente',      volume: 0.8,  pattern: [{freq:300,d:0.4},{freq:0,d:0.1},{freq:300,d:0.4}] },
        'police_whistle':   { name: '🎺 Apito Polícia',      volume: 0.85, pattern: [{freq:2500,d:0.2},{freq:2800,d:0.2},{freq:2500,d:0.2}] },
        'ship_horn':        { name: '🚢 Buzina Navio',        volume: 0.8,  pattern: [{freq:180,d:0.6},{freq:0,d:0.2},{freq:180,d:0.6}] },
        'referee_whistle':  { name: '⚽ Apito Árbitro',      volume: 0.88, pattern: [{freq:3000,d:0.3},{freq:0,d:0.1},{freq:3000,d:0.3},{freq:0,d:0.1},{freq:3000,d:0.3}] },
        'train_horn':       { name: '🚂 Buzina Trem',         volume: 0.82, pattern: [{freq:250,d:0.5},{freq:200,d:0.5},{freq:250,d:0.5}] },
        'emergency_alarm':  { name: '🚑 Alarme Emergência',  volume: 0.9,  pattern: [{freq:1800,d:0.08},{freq:1600,d:0.08},{freq:1800,d:0.08},{freq:1600,d:0.08},{freq:1800,d:0.08},{freq:1600,d:0.08}] },
        'stadium_horn':     { name: '🏟️ Buzina Estádio',     volume: 0.85, pattern: [{freq:400,d:0.25},{freq:450,d:0.25},{freq:400,d:0.25},{freq:450,d:0.25}] },
        'truck_horn':       { name: '🚛 Buzina Caminhão',     volume: 0.83, pattern: [{freq:220,d:0.6},{freq:280,d:0.6}] },
        'gentle_bell':      { name: '🔔 Sino Suave',          volume: 0.35, pattern: [{freq:880,d:0.3},{freq:660,d:0.3},{freq:440,d:0.3}] },
        'notification':     { name: '📱 Notificação',         volume: 0.4,  pattern: [{freq:800,d:0.08},{freq:1000,d:0.08}] },
        'doorbell':         { name: '🚪 Campainha',           volume: 0.38, pattern: [{freq:700,d:0.15},{freq:600,d:0.15}] },
        'success_chime':    { name: '✨ Chime Sucesso',       volume: 0.4,  pattern: [{freq:523,d:0.15},{freq:659,d:0.15},{freq:784,d:0.25}] },
        'soft_beep':        { name: '💫 Beep Suave',          volume: 0.35, pattern: [{freq:1000,d:0.1},{freq:0,d:0.05},{freq:1000,d:0.1}] },
        // ── NOVOS ULTRA ALTOS ──────────────────────────────────────────────
        'nuclear_alarm': {
            name: '☢️ Alarme Nuclear',
            volume: 1.0,
            pattern: [
                {freq:400,d:0.08},{freq:500,d:0.08},{freq:650,d:0.08},{freq:800,d:0.08},
                {freq:1000,d:0.08},{freq:1200,d:0.08},{freq:1000,d:0.08},{freq:800,d:0.08},
                {freq:650,d:0.08},{freq:500,d:0.08},{freq:400,d:0.08}
            ]
        },
        'foghorn': {
            name: '🌊 Buzina de Nevoeiro',
            volume: 1.0,
            pattern: [
                {freq:110,d:0.9},{freq:0,d:0.1},{freq:130,d:0.7},{freq:0,d:0.1},{freq:110,d:0.5}
            ]
        },
        'tornado_siren': {
            name: '🌪️ Sirene de Tornado',
            volume: 1.0,
            pattern: [
                {freq:250,d:0.15},{freq:350,d:0.15},{freq:450,d:0.15},{freq:600,d:0.15},
                {freq:750,d:0.15},{freq:950,d:0.15},{freq:1200,d:0.15},{freq:1500,d:0.15},
                {freq:1200,d:0.12},{freq:950,d:0.12},{freq:750,d:0.12}
            ]
        },
        'air_raid': {
            name: '✈️ Alerta Aéreo',
            volume: 1.0,
            pattern: [
                {freq:440,d:0.12},{freq:600,d:0.12},{freq:800,d:0.12},{freq:600,d:0.12},
                {freq:440,d:0.12},{freq:0,d:0.06},{freq:600,d:0.12},{freq:800,d:0.12},
                {freq:1000,d:0.12},{freq:800,d:0.12},{freq:600,d:0.12}
            ]
        },
        'klaxon': {
            name: '🔔 Klaxon Industrial',
            volume: 1.0,
            pattern: [
                {freq:700,d:0.1},{freq:0,d:0.03},{freq:700,d:0.1},{freq:0,d:0.03},
                {freq:900,d:0.1},{freq:0,d:0.03},{freq:900,d:0.1},{freq:0,d:0.03},
                {freq:700,d:0.1},{freq:0,d:0.03},{freq:900,d:0.1}
            ]
        }
    };

    class ManaTab {
        constructor(id, name) {
            this.id = id; this.name = name || `Timer ${id}`;
            this.tibiaVersion = '7.4'; this.promotion = false;
            this.selectedMana = 0; this.selectedRuneName = '';
            this.targetTime = 0; this.currentDisplay = 0; this.progressDisplay = 0;
            this.isRunning = false; this.startTimestamp = null; this.pausedRemaining = null;
            this.selectedSound = 'emergency_siren'; this.soundVolumes = {};
            this.autoReset = false; this.tabColor = 'blue'; this.tabIcon = '⚡';
            Object.keys(soundConfigs).forEach(k => { this.soundVolumes[k] = soundConfigs[k].volume; });
        }
        calculateTime(mana) {
            const s = this.tibiaVersion === '7.4' ? (this.promotion ? 4 : 6) : (this.promotion ? 2 : 3);
            const t = this.tibiaVersion === '7.4' ? 1 : 2;
            return Math.ceil(mana / t) * s;
        }
        getRuneMana(r) {
            return this.tibiaVersion === '7.4' ? {IH:60,UH:100,HMM:70,GFB:120}[r] : {IH:240,UH:400,HMM:280,GFB:480}[r];
        }
        formatTime(s) {
            const sec = Math.max(0, Math.floor(s));
            return `${Math.floor(sec/60)}:${(sec%60).toString().padStart(2,'0')}`;
        }
        toStorage() {
            return { id:this.id, name:this.name, tibiaVersion:this.tibiaVersion, promotion:this.promotion,
                selectedMana:this.selectedMana, selectedRuneName:this.selectedRuneName, targetTime:this.targetTime,
                isRunning:this.isRunning, startTimestamp:this.startTimestamp, pausedRemaining:this.pausedRemaining,
                selectedSound:this.selectedSound, soundVolumes:this.soundVolumes,
                autoReset:this.autoReset, tabColor:this.tabColor, tabIcon:this.tabIcon };
        }
        static fromStorage(d) { const t = new ManaTab(d.id, d.name); Object.assign(t, d); return t; }
    }

    // ── AUDIO ──────────────────────────────────────────────────────────────
    function getAudioContext() {
        if (!audioContext) audioContext = new (window.AudioContext || window.webkitAudioContext)();
        if (audioContext.state === 'suspended') audioContext.resume();
        return audioContext;
    }

    function playSound(soundType, customVolume) {
        try {
            const cfg = soundConfigs[soundType]; if (!cfg) return;
            const ctx = getAudioContext();
            const vol = customVolume !== undefined ? customVolume : cfg.volume;
            let t = ctx.currentTime;

            // ULTRA LOUD: use multiple oscillators stacked for more power
            cfg.pattern.forEach(note => {
                if (note.freq > 0) {
                    // Primary oscillator (sine)
                    const osc1 = ctx.createOscillator();
                    const g1 = ctx.createGain();
                    osc1.connect(g1); g1.connect(ctx.destination);
                    osc1.frequency.value = note.freq;
                    osc1.type = 'sawtooth'; // sawtooth = brighter, louder perceived
                    g1.gain.setValueAtTime(vol, t);
                    g1.gain.exponentialRampToValueAtTime(0.001, t + note.d);
                    osc1.start(t); osc1.stop(t + note.d + 0.01);

                    // Second layer slightly detuned for thickness
                    const osc2 = ctx.createOscillator();
                    const g2 = ctx.createGain();
                    osc2.connect(g2); g2.connect(ctx.destination);
                    osc2.frequency.value = note.freq * 1.005;
                    osc2.type = 'square'; // square = harsh, cuts through
                    g2.gain.setValueAtTime(vol * 0.4, t);
                    g2.gain.exponentialRampToValueAtTime(0.001, t + note.d);
                    osc2.start(t); osc2.stop(t + note.d + 0.01);
                }
                t += (note.d || 0.1);
            });
        } catch(e) { console.warn('[Audio]', e); }
    }

    function playSoundRepeat(soundType, volumes, times) {
        for (let i = 0; i < (times || 5); i++) {
            setTimeout(() => playSound(soundType, volumes ? volumes[soundType] : undefined), i * 900);
        }
    }

    // ── WORKER ─────────────────────────────────────────────────────────────
    function initWorker() {
        try {
            const code = document.getElementById('worker-code').textContent;
            const blob = new Blob([code], { type:'application/javascript' });
            bgWorker = new Worker(URL.createObjectURL(blob));
            bgWorker.onmessage = handleWorkerMessage;
            bgWorker.onerror = () => { updateStatus('yellow','⚠️ Worker falhou — usando fallback'); initFallbackTimer(); };
            updateStatus('green','🟢 Worker ativo — timers rodam em segundo plano');
        } catch(e) {
            updateStatus('yellow','⚠️ Web Worker indisponível — usando fallback');
            initFallbackTimer();
        }
    }

    function handleWorkerMessage(e) {
        const { type, tabId } = e.data;
        if (type === 'TICK') {
            const tab = getTab(tabId); if (!tab) return;
            tab.currentDisplay = e.data.remaining;
            tab.progressDisplay = e.data.progress;
            updateTimerDisplayLive(tab);
            saveState();
        } else if (type === 'TIMER_DONE') {
            const tab = getTab(tabId);
            if (tab) { tab.isRunning = false; tab.startTimestamp = null; tab.currentDisplay = 0; }
            onTimerDone(e.data); saveState();
        }
    }

    function initFallbackTimer() {
        setInterval(() => {
            let needsRender = false;
            tabs.forEach(tab => {
                if (!tab.isRunning || !tab.startTimestamp) return;
                const elapsed = (Date.now() - tab.startTimestamp) / 1000;
                tab.currentDisplay = Math.max(0, tab.targetTime - elapsed);
                tab.progressDisplay = Math.min(1, elapsed / tab.targetTime);
                updateTimerDisplayLive(tab);
                if (elapsed >= tab.targetTime) {
                    tab.isRunning = false; tab.startTimestamp = null;
                    onTimerDone({ tabId:tab.id, name:tab.name, runeName:tab.selectedRuneName, sound:tab.selectedSound, volumes:tab.soundVolumes, autoReset:tab.autoReset });
                    if (tab.autoReset && tab.selectedMana > 0) { tab.startTimestamp = Date.now(); tab.isRunning = true; }
                    needsRender = true;
                }
            });
            if (needsRender) renderTabs();
            saveState();
        }, 500);
    }

    async function initServiceWorker() {
        if (!('serviceWorker' in navigator)) return;
        try {
            const reg = await navigator.serviceWorker.register('sw.js', { scope:'./' });
            serviceWorkerReady = true;
            navigator.serviceWorker.onmessage = e => { if (e.data?.type === 'SW_CHECK_TIMERS') checkAndNotifyFromSW(); };
            if (reg.active) reg.active.postMessage({ type:'START_BACKGROUND_CHECK' });
        } catch(e) { console.warn('[SW]', e); }
    }

    function checkAndNotifyFromSW() {
        tabs.forEach(tab => {
            if (!tab.isRunning || !tab.startTimestamp) return;
            if ((Date.now() - tab.startTimestamp) / 1000 >= tab.targetTime) sendDesktopNotification(tab.name, tab.selectedRuneName);
        });
    }

    async function toggleNotifications() {
        const cb = document.getElementById('notificationsToggle');
        if (!cb.checked) { notificationsEnabled = false; showToast('🔕 Notificações desativadas','warning'); return; }
        if (!('Notification' in window)) { cb.checked = false; showToast('❌ Navegador não suporta notificações','error'); return; }
        const perm = await Notification.requestPermission();
        if (perm === 'granted') {
            notificationsEnabled = true;
            new Notification('✅ Notificações Ativadas!', { body:'Avisos quando suas runas ficarem prontas.', tag:'mana-test' });
            showToast('✅ Notificações ativadas!');
            document.getElementById('bg-enable-btn').classList.add('hidden');
            updateStatusFromCapabilities();
        } else { cb.checked = false; notificationsEnabled = false; showToast('⚠️ Permissão negada.','warning'); }
    }

    function sendDesktopNotification(tabName, runeName) {
        if (!notificationsEnabled || Notification.permission !== 'granted') return;
        new Notification('⚡ RUNA PRONTA!', {
            body:`[${tabName}] Sua runa ${runeName} está pronta!`,
            icon:'data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><text y="80" font-size="80">⚡</text></svg>',
            requireInteraction:true, tag:`mana-${tabName}`, renotify:true
        });
    }

    async function enableBackground() {
        document.getElementById('notificationsToggle').checked = true;
        await toggleNotifications();
    }

    function onTimerDone({ tabId, name, runeName, sound, volumes, autoReset }) {
        const tab = getTab(tabId);
        sendDesktopNotification(name, runeName);
        if (!document.hidden) { showAlert(`[${name}] Sua runa ${runeName} está pronta!`); playSoundRepeat(sound, volumes); }
        else { pendingAlerts.push({ tabId, name, runeName, sound, volumes }); try { playSoundRepeat(sound, volumes); } catch(e){} }
        if (tab && autoReset) {
            setTimeout(() => { if (tab.selectedMana > 0) { tab.startTimestamp = Date.now(); tab.isRunning = true; syncWorkerTimer(tab); renderTabs(); } }, 1000);
        } else if (tab) renderTabs();
    }

    // ── TIMER CONTROLS — PAUSE BUG FIXED ──────────────────────────────────
    function startTimer(tabId) {
        const tab = getTab(tabId);
        if (!tab || tab.selectedMana === 0) { showToast('⚠️ Selecione uma runa primeiro!','warning'); return; }
        if (tab.isRunning) return;

        if (tab.pausedRemaining != null && tab.pausedRemaining > 0) {
            // ✅ RESUME from paused position — adjust startTimestamp so remaining = pausedRemaining
            tab.startTimestamp = Date.now() - ((tab.targetTime - tab.pausedRemaining) * 1000);
            tab.pausedRemaining = null;
            showToast(`▶️ Timer retomado: ${tab.name}`);
        } else {
            // ✅ Fresh start
            tab.startTimestamp = Date.now();
            tab.pausedRemaining = null;
            showToast(`▶️ Timer iniciado: ${tab.name}`);
        }
        tab.isRunning = true;
        tab.currentDisplay = tab.pausedRemaining || tab.targetTime;
        tab.progressDisplay = tab.startTimestamp ? Math.min(1, (Date.now() - tab.startTimestamp) / (tab.targetTime * 1000)) : 0;

        syncWorkerTimer(tab); saveState(); renderTabs();
    }

    function stopTimer(tabId) {
        const tab = getTab(tabId);
        if (!tab || !tab.isRunning) return;
        // ✅ Save exactly how much time is remaining
        const elapsed = (Date.now() - tab.startTimestamp) / 1000;
        tab.pausedRemaining = Math.max(0, tab.targetTime - elapsed);
        tab.isRunning = false; tab.startTimestamp = null;
        if (bgWorker) bgWorker.postMessage({ type:'CLEAR_TIMER', payload:{ tabId } });
        saveState(); renderTabs();
        showToast(`⏸️ Timer pausado: ${tab.name} — ${tab.formatTime(tab.pausedRemaining)} restante`, 'warning');
    }

    function resetTimer(tabId) {
        const tab = getTab(tabId); if (!tab) return;
        tab.isRunning = false; tab.startTimestamp = null;
        tab.currentDisplay = 0; tab.progressDisplay = 0; tab.pausedRemaining = null;
        if (bgWorker) bgWorker.postMessage({ type:'CLEAR_TIMER', payload:{ tabId } });
        saveState(); renderTabs();
    }

    function syncWorkerTimer(tab) {
        if (!bgWorker) return;
        bgWorker.postMessage({ type:'SET_TIMER', payload:{ tabId:tab.id, startTimestamp:tab.startTimestamp, targetMs:tab.targetTime*1000, name:tab.name, runeName:tab.selectedRuneName, sound:tab.selectedSound, volumes:tab.soundVolumes, autoReset:tab.autoReset } });
    }

    function updateTimerDisplayLive(tab) {
        const d = document.getElementById(`timer-${tab.id}`);
        const b = document.getElementById(`progress-${tab.id}`);
        const s = document.getElementById(`status-${tab.id}`);
        if (d) {
            d.textContent = tab.formatTime(tab.currentDisplay);
            d.classList.toggle('finishing', tab.currentDisplay <= 10 && tab.isRunning);
        }
        if (b) b.style.width = `${Math.round(tab.progressDisplay * 100)}%`;
        if (s) {
            s.className = 'timer-status ' + (tab.isRunning ? 'running' : (tab.pausedRemaining > 0 ? 'paused' : 'stopped'));
            s.textContent = tab.isRunning ? '▶ RODANDO' : (tab.pausedRemaining > 0 ? `⏸ PAUSADO — ${tab.formatTime(tab.pausedRemaining)} restante` : '⏹ PARADO');
        }
    }

    function saveState() {
        try { localStorage.setItem('mana_todos_v1', JSON.stringify({ tabs:tabs.map(t=>t.toStorage()), activeTabId, tabCounter, darkMode })); } catch(e){}
    }
    function loadState() {
        try {
            const raw = localStorage.getItem('mana_todos_v1'); if (!raw) return false;
            const s = JSON.parse(raw);
            tabCounter = s.tabCounter||0; activeTabId = s.activeTabId||null; darkMode = s.darkMode||false;
            if (s.tabs && s.tabs.length > 0) {
                tabs = s.tabs.map(d => ManaTab.fromStorage(d));
                tabs.forEach(tab => {
                    if (tab.isRunning && tab.startTimestamp) {
                        const elapsed = (Date.now() - tab.startTimestamp) / 1000;
                        if (elapsed >= tab.targetTime) {
                            tab.isRunning = false;
                            showToast(`⚠️ Runa ${tab.selectedRuneName} (${tab.name}) terminou ${formatMissedTime(elapsed - tab.targetTime)} atrás!`, 'warning');
                            if (tab.autoReset) {
                                const adj = elapsed - (Math.floor(elapsed/tab.targetTime) * tab.targetTime);
                                tab.startTimestamp = Date.now() - (adj * 1000); tab.isRunning = true; syncWorkerTimer(tab);
                            }
                        } else { tab.currentDisplay = tab.targetTime - elapsed; syncWorkerTimer(tab); }
                    }
                });
                return true;
            }
        } catch(e){}
        return false;
    }
    function formatMissedTime(s) { if(s<60) return `${Math.floor(s)}s`; if(s<3600) return `${Math.floor(s/60)}m`; return `${Math.floor(s/3600)}h`; }

    function updateStatus(color, text, showBtn) {
        document.getElementById('status-dot').className = `dot dot-${color}`;
        document.getElementById('bg-status-text').textContent = text;
        document.getElementById('bg-enable-btn').classList.toggle('hidden', !showBtn);
    }
    function updateStatusFromCapabilities() {
        const hw = bgWorker !== null, hn = notificationsEnabled;
        if (hw && hn) updateStatus('green','🟢 SEGUNDO PLANO ATIVO — Timer + notificações funcionam com aba minimizada');
        else if (hw) updateStatus('yellow','🟡 Timer ativo em segundo plano | Ative notificações para alertas',true);
        else if (hn) updateStatus('yellow','🟡 Notificações ativas | Worker indisponível');
        else updateStatus('red','🔴 Segundo plano limitado — mantenha a aba visível',true);
    }

    function showToast(msg, type) {
        const c = document.getElementById('toast-container');
        const t = document.createElement('div');
        t.className = `toast ${type||''}`; t.innerHTML = `<span>${msg}</span>`; c.appendChild(t);
        setTimeout(() => { t.style.animation='toast-out 0.3s ease forwards'; setTimeout(()=>t.remove(),300); }, 4000);
    }

    document.addEventListener('visibilitychange', () => {
        if (!document.hidden) {
            if (pendingAlerts.length > 0) { pendingAlerts.forEach(a => { showAlert(`[${a.name}] Sua runa ${a.runeName} está pronta!`); playSoundRepeat(a.sound, a.volumes, 3); }); pendingAlerts = []; }
            tabs.forEach(tab => { if (tab.isRunning && tab.startTimestamp) { const e = (Date.now()-tab.startTimestamp)/1000; tab.currentDisplay = Math.max(0,tab.targetTime-e); tab.progressDisplay = Math.min(1,e/tab.targetTime); updateTimerDisplayLive(tab); } });
        }
    });

    function showAlert(msg) { document.getElementById('alertMessage').textContent = msg; document.getElementById('overlay').style.display='block'; document.getElementById('alert').style.display='block'; }
    function closeAlert() { document.getElementById('overlay').style.display='none'; document.getElementById('alert').style.display='none'; }

    function addNewTab() { tabCounter++; const t = new ManaTab(tabCounter); tabs.push(t); activeTabId = t.id; saveState(); renderTabs(); showToast(`➕ Nova aba criada: ${t.name}`); }
    function removeTab(id) {
        if (tabs.length===1) { showToast('⚠️ Precisa ter pelo menos uma aba!','warning'); return; }
        const tab = getTab(id); if (!tab) return;
        if (bgWorker) bgWorker.postMessage({ type:'CLEAR_TIMER', payload:{tabId:id} });
        tabs = tabs.filter(t=>t.id!==id); if (activeTabId===id) activeTabId = tabs[0].id;
        saveState(); renderTabs();
    }
    function switchTab(id) {
        activeTabId = id; const tab = getTab(id);
        if (tab) {
            document.querySelectorAll('.color-option').forEach(o=>o.classList.remove('active'));
            document.querySelector(`[data-color="${tab.tabColor}"]`)?.classList.add('active');
            document.querySelectorAll('.icon-option').forEach(o=>o.classList.remove('active'));
            document.querySelector(`[data-icon="${tab.tabIcon}"]`)?.classList.add('active');
        }
        renderTabs();
    }
    function getTab(id) { return tabs.find(t=>t.id===id); }
    function updateTabName(id, name) { const t = getTab(id); if(t){ t.name = name||`Timer ${id}`; saveState(); } }
    function setTabColor(c) { const t=getTab(activeTabId); if(t){ t.tabColor=c; document.querySelectorAll('.color-option').forEach(o=>o.classList.remove('active')); document.querySelector(`[data-color="${c}"]`)?.classList.add('active'); saveState(); renderTabs(); } }
    function setTabIcon(i) { const t=getTab(activeTabId); if(t){ t.tabIcon=i; document.querySelectorAll('.icon-option').forEach(o=>o.classList.remove('active')); document.querySelector(`[data-icon="${i}"]`)?.classList.add('active'); saveState(); renderTabs(); } }
    function setTibiaVersion(id, v) { const t=getTab(id); if(!t) return; t.tibiaVersion=v; if(t.selectedMana>0){ const m={'Intense Healing (IH)':'IH','Ultimate Healing (UH)':'UH','Heavy Magic Missile (HMM)':'HMM','Great Fireball (GFB)':'GFB'}; for(const[n,k] of Object.entries(m)){ if(t.selectedRuneName===n){ t.selectedMana=t.getRuneMana(k); break; } } t.targetTime=t.calculateTime(t.selectedMana); } saveState(); renderTabs(); }
    function setPromotion(id, p) { const t=getTab(id); if(!t) return; t.promotion=p; if(t.selectedMana>0) t.targetTime=t.calculateTime(t.selectedMana); saveState(); renderTabs(); }
    function selectRune(id, type, name) { const t=getTab(id); if(!t) return; t.selectedMana=t.getRuneMana(type); t.selectedRuneName=name; t.targetTime=t.calculateTime(t.selectedMana); resetTimer(id); }
    function setCustomMana(id) { const t=getTab(id); const inp=document.getElementById(`customMana-${id}`); const m=parseInt(inp.value); if(t&&m>0){ t.selectedMana=m; t.selectedRuneName=`Mana Customizada (${m})`; t.targetTime=t.calculateTime(m); resetTimer(id); } }
    function selectSound(id, sound) { const t=getTab(id); if(!t) return; t.selectedSound=sound; saveState(); renderTabs(); getAudioContext(); playSound(sound, t.soundVolumes[sound]); }
    function updateVolume(id, sound, val) { const t=getTab(id); if(t) t.soundVolumes[sound]=val/100; const l=event.target.parentElement.querySelector('.volume-label'); if(l) l.textContent=val+'%'; saveState(); }
    function toggleAutoReset(id) { const t=getTab(id); const cb=document.getElementById(`autoReset-${id}`); if(t&&cb){ t.autoReset=cb.checked; saveState(); } }
    function toggleSettings() { document.getElementById('settingsPanel').classList.toggle('active'); }
    function toggleDarkMode() { darkMode=!darkMode; document.body.dataset.theme=darkMode?'dark':''; document.getElementById('darkModeToggle').checked=darkMode; document.getElementById('themeBtn').textContent=darkMode?'☀️':'🌙'; saveState(); }

    const ihImage='data:image/png;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/4gHYSUNDX1BST0ZJTEUAAQEAAAHIAAAAAAQwAABtbnRyUkdCIFhZWiAH4AABAAEAAAAAAABhY3NwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAA9tYAAQAAAADTLQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAlkZXNjAAAA8AAAACRyWFlaAAABFAAAABRnWFlaAAABKAAAABRiWFlaAAABPAAAABR3dHB0AAABUAAAABRyVFJDAAABZAAAAChnVFJDAAABZAAAAChiVFJDAAABZAAAAChjcHJ0AAABjAAAADxtbHVjAAAAAAAAAAEAAAAMZW5VUwAAAAgAAAAcAHMAUgBHAEJYWVogAAAAAAAAb6IAADj1AAADkFhZWiAAAAAAAABimQAAt4UAABjaWFlaIAAAAAAAACSgAAAPhAAAts9YWVogAAAAAAAA9tYAAQAAAADTLXBhcmEAAAAAAAQAAAACZmYAAPKnAAANWQAAE9AAAApbAAAAAAAAAABtbHVjAAAAAAAAAAEAAAAMZW5VUwAAACAAAAAcAEcAbwBvAGcAbABlACAASQBuAGMALgAgADIAMAAxADb/2wBDAAUDBAQEAwUEBAQFBQUGBwwIBwcHBw8LCwkMEQ8SEhEPERETFhwXExQaFRERGCEYGh0dHx8fExciJCIeJBweHx7/2wBDAQUFBQcGBw4ICA4eFBEUHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh7/wAARCAAmACYDASIAAhEBAxEB/8QAGgAAAgMBAQAAAAAAAAAAAAAAAAcFBggEA//EAC8QAAICAgEDAwIEBgMAAAAAAAECAwQFEQAGEiEHE0EiMRQVJFEWIzIzU2Fxo9H/xAAYAQADAQEAAAAAAAAAAAAAAAADBAUCAv/EACIRAAICAQQCAwEAAAAAAAAAAAECAAMEERIhMRNRM0Fx8P/aAAwDAQACEQMRAD8ArmG/JsV0lhoaOFqfhXhiTa0arP7hrwSSM0ksEjsxeVj5bQAAGh4FsweBhyVY2noCGupUNIKVFtFjpVAFQlmJICqAWYkAAkgGD6HupF05hsfcrJarWBGXUj6hupU0QfjmifTnCVkyV1ooyK2GeOpWhY7CyPAk0lj7bLlZ1iG9lQjaIEjDlVhXXSp28mQ6UNz8mKKx0RjWYIkksM0sipClvERURIzHSorz0o1ZyfsiksfJAOjqu5DGDGq4uY/seM9oVqVIkkfvutzUEl/E5bK5TpS3FFYZKccssMumWSORnUjtP312rv4+teLDqKksRy9DKmS3ZxPsvBPJ9TvWlDCMSOTtnVopV7iNlBGSWYseDx7ELbXXuEvxQq7k+vyZV9Za0F3FvPZx1WCelcghhkhrRQM8UsUjEOIlRW08fg9u/JBJAGjnr613BZx+TYEMPzGmPGtDUNj/AN4cWyVC2kARnELFO42PS7E0rmFw89m9FXMYh+lh5INOqd8fHS+agw9+atd/SQ5uaCSnK0gZPxAgSFq5ZdjuCQLICSO73CAD2MeZdx9u3DjcFUps4aWCE7Hx+jq+TxsdM9UYaHD2unctZleK2oSaxGw3C29iRNg6dDplb4ZQf21RehrKFI50iGPaKrOejpGb0Z0G+C68yXUv8QzWoZYJ0gpSQdvsLLJ7jbk7j3L3bIHaD9j8cqfUeVxtjqTqyVp4zFPDQUtJs7QSWv6QD9wNf65ww9X5utiYsXaq2osiD7E2Qhrs9JkI/uq+1Up29zhO5WJBQBSdcrPqVNCenIkwLmSpApabuIMkpOgZXIA2fAHgAKAoAVQAFsSgvYPUcyrgtZA7MQPrLJG0WTjikLRLeplfpA1uOz4+/wDrhyD61mSxictIjbH5jTH/AFWOHA5fFzfsLifH/ehJin6h4ibp2hFLaytC1HAkMywY+KZSY4o4gVdplOisSMR2jyT5PjnCescWgKx5rOsCd7bGwAn/AJ/m8OHOkvsVQAxiLgbyunAkhZ9VLFigtF81lvYXWlFKP4Gv8v7cMd6mRU4khGWyror9w78bE2h8rr3gNH5HDhzee3rcZiB3pKj1h1Bh58dYqYmK07Xrkdud5oVhEZjWRQqKHfYPunyT47QOHDhxZ2JbUynjgeMGf//Z';
    const uhImage='data:image/gif;base64,R0lGODlhIAAgAPeaAO3t7evr69ra2tLS0ubm5vDw8NnZ2e/v7+Hh4ejo6Onp6eDg4PT09PLy8tzc3NHR0fHx8erq6vX19eTk5OXl5eLi4s/Pz9vb2/Pz8+Pj4x8fH9PT0/b29s7Ozt/f3w8PD/v7+83NzVxcXMfHx93d3SQkJK+vrzU1NRUVHMjIyNjY2MrKyv39/dDQ0N7e3rm5uczMzMbGxtfX19XV1fz8/Ofn55eXl5ubm9TU1JKSkhwcHPf398HBwcXFxRsbIdbW1js7O52dnRsbH0BAQMDAwO7u7re3tyMjIywsMRAQFIqKirOzsxYWJg0NEwoKCkdHRyAgIBYWJxMTHqKiovn5+RERER4eHouLixwcOTQ0NBkZIDMzMxYWG0JCQqGhoZ6enhgYIAoKDxYVJb+/v/r6+iEhIS4uMioqKnNzcyYmKi8vLwwMFbe9yCYmJmVlZcnJyR4eK1VVVcvLyxAQEEZGRhQUFOzs7A4OEgwMEwUFBRUVHoiIiLu7uwkJCTIyMpmZmQ0NGpWVlRcXFykpLhISEiMjM1paWhoaLHt7e7y8vAkJFRMTF1BQUDExNxISJaqqqhUVGaysrB0dIBERJi4uNBISHRERFSwsLE5OTv///////wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH5BAEAAJoALAAAAAAgACAAAAj/ADUJHEiwoMGDCBMqXMiwocOHEDWUKKHjzBGICQXFACDgAQ4LfjRYwUgQxYBMKFHSkMAAgQgdJDXBYZESBA2amahAcNCGJIoFmTIEYAChwQEADMg0MHAJox4ZmRAUgFAgQYYFNQrsYJCoDEQfUBNwwCAhgIAFBOwECOCBj5qHPoBGOCAhQYALWSokqIBAwQA0UBwOApApQQQFBAQ8qWMowYYBChbIOeEQSQEMFRRMMMCIxCMQIh48+LEiBJEkDc0UKLCAwoQ4PwQAMYDjRIgBFmC0aNKw0YEILlx+6DInzwdMfaoQSBFjyZ2GlMp6EDABEQEeTiJxAJKixYMeXyw1tkyTCcAFByQq0FFxw4GGER02WBiTA1JDSQwyOXAxY4aBLR8MEcIbHRgAgxdBcNEQIS9kEoACCDzwmBsADNCBBw6MEEgQizh0CBsgAIBBYhdMoMIFDShggAk3aAERIDXs0AAGABAwVwMcQMDDH3uEgREWCNhYQBEQHHWABTbkAEZMkwgQAAVQZkDCCDYoUUhMAjkywAov9GCECVNcIQSWA+FRiRRiMBHFGoqQ6eabcMYp55x0IhQQADs=';
    const hmmImage='data:image/gif;base64,R0lGODlhIAAgAPfSABAEGgAAAYA9oggCEAAAA/+X/aBPzZBMplctcYM/qaNV1LNf56lY3GEmdahb3LRe5XY3lmMmdeqE+Hw/oX88oaNS0qBT0DYKO4FBp10vceqA6ns2mHU+kJtMyJNOonc9lQYCCQUCDGs2igEBBiERKPOL+WwuhAcEChoGKII/pp9SzcZp951QyoRAqlgiaKZV1zYNQapb4A8IFJFIutB18lIfaJhLvykJOZBKu3MyjH8/ozwVUVYsbT4VU5FJvJRKwM5u74hDsG02jgEBBKNT0MFs7tBy8ctr3XQyjjINQOiD94M+o2M1c5JKvJZJvnY7lrtk7RgMHx0HLK5d5V8odzgdSCAPKQUDB4xGs3c8m8Jn7qBU0QsDFCoVNotFtI5GtahW2a9bvMFm7oE/pns1kxYFIgwDEKBNyYVDrZdNwyQIMWstg1gjbVojapBJuqlX2Q4EFmgtf4tFsyAHL4xItpVLwBwOIZRLwIpFsYpHtdF181UraYxFtFQeY0INQ49KunU0lIlCrzIPRmAndwQCCgMBAg8DEgQBCikVNgYDCCwJNSIIMm4xiG01ihoOGz4STH48oZtQy3MyjatX3rhi7AcCDJ9SzoZFrwIBCJxPxkcZWVkhbYRBq5BGuV0weKxX3J1PzFMkcSsJM7df7MRp7qlZ2nQ8iHs3mS4XPEYWViYSMR8QJOF89Z9T0EcXWjQaQ8hq61QgZo1CqwEBA/iN+oI/p3g7mphLwIlBrII9oKpXykIYW5hLvIc/qV8peYZCqpZLwGI1Z3tAlSMSK1wueHY3mMNn7oE/pJdOxE8dZEUZW5dMxLRj65VMxXU0krdh7EglUgAAAP///wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH5BAEAANIALAAAAAAgACAAQAj/AKUJHEiwoMGDCAuiMFLAyy8fuJYESEhxYIA8zKa0wIPow50mEFwoqkhxgIJissgAqtHD1RCSMGPCVKbnFqdGV07Y6eBsEwCZBhe9aKVqUhZYHBjwGhQNqNOnUIFGa7bFRq9CUQfekKDkjwA6boIgaZqVkJgKGHR0enUkjJxTbcg+3VWKCAIhjg4Es0ShAYioAGg8SzBmDw8NDkRpCpFVYLRoBMpwqdS4suXLmAUGkAJAbmU4rCj9+JTJ16HGA2LQeoNGADJIOTw/3VFCS60UnhIJE8BGNlA1pL7Y6vIEmodVZyJc8A0z2oIHfAxgQaUrTSRGfgxBxRRKRZFLUWQMRjMV6JgJM1EFWQBVZRkJIEzqbOgzK+ocY2CIWRExIcMKSakwBxMBo0AxgxM4AMMCFTAkIWBM0eSyRhyxPJLMCJllqOFlAQEAOw==';
    const gfbImage='data:image/gif;base64,R0lGODlhIAAgAPf/ALZBMbI/MLU/L64/Mq8+LyUOA38rGUobFRAGATsWEWcjE2EhEw4FAS4QA28mGBcIBAIBAFogEWAgESgOAzoVBgkDAUMYCDMTDxkJArVAMLI/L8ZHN9NLPSsQA8FFNcNGNNhNPNpOQLI+L4swH7VBL9RLPMdHOblCMvpiVedRRPpkVudTRsZGOv1wXrZALbM/MKU5KXEpHHAnGV0jGlskG6U6KetSQZAyI+dSRN1hSX8sHMxHNHMoFg4GBG0lFlUeDvxuXgwFAcBFM3QnFe5WRNxPQY4yIrhAKa0+LsNGN64+MMdRPflhVuRwWPpjVv9/e8RGNxkLCaI6MONQQWUjE9dLPIUvH/pZSvBWRikPDMhIOv97b689MPdZS+ZSQftqXe5YReNRPhQHBbhBM/+AdiQODP9/boErGdZMPs1JOjUTDrtCMb9ENL9FNXgrIdBJN2slFsNGOHYoFzUSBM9KPMtIOcdIOblBMb9DM/xvYiwQDMRDL8dHNzsWCdZMOz4WCfxqXPxrXf52Zb1DMtJKPPlfUvhbTbE/MchHOMpHOcBFN0saDfJYRj0VB7pDM/tmVvpjVfhaS/lfT8RHOR8LAsJYR6M6KrNALtpOP7xBLc9HMvlhUO1URrJJN6w8LK4+Lc5IObtPPXsqGmsvJvxuYIRAM+5UR/ZkUP96bDkTB9BKOOFQQ7M/LvxqXaQ6KmQiEJUzI9ZLNb1DNflfUNhPQuVSQqY6K/+Ae0MXCMtJOUcZCqA4JsNGNXYoFrtBMoQwInYrIPRXSLlCNCEMCcxJO8FFNINJA75DM71DNPhbSq87J7hCMmskFHYpF85KPMdIOpI6LPhdS+BQP9ZMQcpINycSD8FDNP95a6E3IdZJM6c7LNhNPsVDMdpOPZdFNbtAL79EM/RWRogvH0AdF+1YSOxURScOC7lCMelSQ1ogGWwlE04eF8FDLsZGNjoVD1AcF/JXSddOPf9+bu9TRUgZDEsbD+BRQvtnV5I0H8VGODIUDxMHAeJRP/+Ad9VLPP///yH5BAEAAP8ALAAAAAAgACAAQAj/AP8JHEiwoMGDCAV2GIBCnpkW91oF2pKHyRdBq/pUSEhwAiFJITR40APtzokNuRAdKEXHw7R8IwwsgMCxps2bOG/uQwNo3osMbmhkWAOAlbR13mSpYneEWyYJCGx28CQCRg0hXMpU0mKrFw9mDv5EzUm2rNmzaAUiSHHrSTQQATT8SnAgnZJTs5KhM0TEWgwGHCtgQjFFAIEHWDIUA8DLnqMo1D74igPi26uNCRm4KoJjgzMo4xKYyzKqiYlnbRTd8KEuSM4H9Cz8WCADToRFGPYBTsu7t+/fwIMnrFDAgi4KFBoYazAhFa59mHEiCIGKzKMS3crVusKpi5NCpK5BtjpUIDrCCir6RdpF4py7GUvAhLpwwY+XPdlshNtRzzXCfZNsEswlnwyTAwEkHMMBHgNU084g/ryhjCbYjHXQHPAA4cILSIjRBDhhnCBCGlLow8EKtJggACwR0IQQBEaUYEoVoAijBjB1jMHCOz3wY0cSicRjhQQN2ISBNtvw8QExJpDTCSNsyLKMJeIoIIoMHeSEARVnGDDECLHgI0czOijQCAK7mVUBA5QgUAEE5gkn55x0ohUQADs=';

    function renderTabs() {
        const ctrl = document.getElementById('tabsControls');
        const content = document.getElementById('tabsContent');
        ctrl.innerHTML = '<button class="add-tab-btn" onclick="addNewTab()">➕ ADICIONAR NOVA ABA</button>';
        tabs.forEach(tab => {
            const row = document.createElement('div'); row.className='tab-row';
            const icon = document.createElement('div'); icon.className=`tab-icon color-${tab.tabColor}`; icon.textContent=tab.tabIcon;
            const nameInp = document.createElement('input'); nameInp.type='text'; nameInp.className='tab-name-input'; nameInp.value=tab.name; nameInp.placeholder='Nome da aba'; nameInp.onchange=e=>updateTabName(tab.id,e.target.value);
            const selBtn = document.createElement('button'); selBtn.className=`tab-button ${activeTabId===tab.id?'active':''}`; selBtn.textContent='👁️ Ver Aba'; selBtn.onclick=()=>switchTab(tab.id);
            const rmBtn = document.createElement('button'); rmBtn.className='remove-tab-btn'; rmBtn.textContent='❌ Remover'; rmBtn.onclick=()=>removeTab(tab.id);
            row.append(icon,nameInp,selBtn,rmBtn); ctrl.appendChild(row);
        });
        content.innerHTML='';
        tabs.forEach(tab=>content.appendChild(createTabContent(tab)));
    }

    function createTabContent(tab) {
        const c = document.createElement('div');
        c.className=`container ${activeTabId===tab.id?'active':''}`; c.id=`tab-content-${tab.id}`;
        const iM=tab.getRuneMana('IH'), uM=tab.getRuneMana('UH'), hM=tab.getRuneMana('HMM'), gM=tab.getRuneMana('GFB');
        const rd = tab.tibiaVersion==='7.4' ? {noPromo:'+1 mana a cada 6s',promo:'+1 mana a cada 4s'} : {noPromo:'+2 mana a cada 3s',promo:'+2 mana a cada 2s'};
        let sHtml='';
        Object.keys(soundConfigs).forEach(k=>{
            const cfg=soundConfigs[k]; const v=Math.round(tab.soundVolumes[k]*100);
            sHtml+=`<div class="sound-option ${tab.selectedSound===k?'active':''}" onclick="selectSound(${tab.id},'${k}')"><div class="sound-name">${cfg.name}</div><div style="font-size:0.78em;opacity:0.75;margin-bottom:8px;">Volume máximo — ${v}%</div><div class="volume-control"><span class="volume-label">${v}%</span><input type="range" class="volume-slider" min="0" max="100" value="${v}" onclick="event.stopPropagation()" oninput="updateVolume(${tab.id},'${k}',this.value)"></div></div>`;
        });
        const elapsed = tab.isRunning&&tab.startTimestamp ? (Date.now()-tab.startTimestamp)/1000 : 0;
        const remaining = tab.isRunning ? Math.max(0,tab.targetTime-elapsed) : (tab.pausedRemaining!=null ? tab.pausedRemaining : (tab.currentDisplay||0));
        const progress = tab.isRunning&&tab.targetTime>0 ? Math.min(1,elapsed/tab.targetTime) : (tab.pausedRemaining!=null&&tab.targetTime>0 ? (tab.targetTime-tab.pausedRemaining)/tab.targetTime : 0);
        const statusClass = tab.isRunning ? 'running' : (tab.pausedRemaining>0 ? 'paused' : 'stopped');
        const statusText = tab.isRunning ? '▶ RODANDO' : (tab.pausedRemaining>0 ? `⏸ PAUSADO — ${tab.formatTime(tab.pausedRemaining)} restante` : '⏹ PARADO');

        c.innerHTML=`
        <div class="section"><div class="section-title">🎮 Versão do Tibia</div><div class="promotion-options">
            <div class="promotion-btn ${tab.tibiaVersion==='7.4'?'active':''}" onclick="setTibiaVersion(${tab.id},'7.4')"><div>Tibia 7.4</div><small>Sistema clássico</small></div>
            <div class="promotion-btn ${tab.tibiaVersion==='8.0'?'active':''}" onclick="setTibiaVersion(${tab.id},'8.0')"><div>Tibia 8.0</div><small>Sistema novo</small></div>
        </div></div>
        <div class="section"><div class="section-title">🎖️ Selecione sua Vocação</div><div class="promotion-options">
            <div class="promotion-btn ${!tab.promotion?'active':''}" onclick="setPromotion(${tab.id},false)"><div>No Promotion</div><small>${rd.noPromo}</small></div>
            <div class="promotion-btn ${tab.promotion?'active':''}" onclick="setPromotion(${tab.id},true)"><div>Promotion</div><small>${rd.promo}</small></div>
        </div></div>
        <div class="section"><div class="section-title">📜 Selecione uma Runa</div><div class="runes-grid">
            <div class="rune-option ${tab.selectedMana===iM?'selected':''}" onclick="selectRune(${tab.id},'IH','Intense Healing (IH)')"><div class="rune-image"><img src="${ihImage}" alt="IH"></div><div class="rune-info"><div class="rune-name">IH</div><div class="rune-mana">${iM} mana</div><div class="rune-time">${tab.formatTime(tab.calculateTime(iM))}</div></div></div>
            <div class="rune-option ${tab.selectedMana===uM?'selected':''}" onclick="selectRune(${tab.id},'UH','Ultimate Healing (UH)')"><div class="rune-image"><img src="${uhImage}" alt="UH"></div><div class="rune-info"><div class="rune-name">UH</div><div class="rune-mana">${uM} mana</div><div class="rune-time">${tab.formatTime(tab.calculateTime(uM))}</div></div></div>
            <div class="rune-option ${tab.selectedMana===hM?'selected':''}" onclick="selectRune(${tab.id},'HMM','Heavy Magic Missile (HMM)')"><div class="rune-image"><img src="${hmmImage}" alt="HMM"></div><div class="rune-info"><div class="rune-name">HMM</div><div class="rune-mana">${hM} mana</div><div class="rune-time">${tab.formatTime(tab.calculateTime(hM))}</div></div></div>
            <div class="rune-option ${tab.selectedMana===gM?'selected':''}" onclick="selectRune(${tab.id},'GFB','Great Fireball (GFB)')"><div class="rune-image"><img src="${gfbImage}" alt="GFB"></div><div class="rune-info"><div class="rune-name">GFB</div><div class="rune-mana">${gM} mana</div><div class="rune-time">${tab.formatTime(tab.calculateTime(gM))}</div></div></div>
        </div><div class="custom-mana"><input type="number" id="customMana-${tab.id}" placeholder="Digite quantidade de mana customizada..." min="1"><button onclick="setCustomMana(${tab.id})">CALCULAR</button></div></div>
        <div class="section"><div class="section-title">🔊 Todos os Sons (20)</div><div class="sound-selector">${sHtml}</div></div>
        <div class="section"><div class="section-title">⚙️ Configurações</div><div class="settings-grid"><div class="setting-option"><input type="checkbox" id="autoReset-${tab.id}" ${tab.autoReset?'checked':''} onchange="toggleAutoReset(${tab.id})"><label for="autoReset-${tab.id}">🔄 Resetar automaticamente</label></div></div></div>
        <div class="section timer-section">
            <div class="section-title" style="color:rgba(255,255,255,0.9);">⏱️ TIMER DE REGENERAÇÃO</div>
            <div class="timer-progress"><div class="timer-progress-bar" id="progress-${tab.id}" style="width:${Math.round(progress*100)}%"></div></div>
            <div class="timer-display ${tab.currentDisplay<=10&&tab.isRunning?'finishing':''}" id="timer-${tab.id}">${tab.formatTime(remaining)}</div>
            <div class="timer-status ${statusClass}" id="status-${tab.id}">${statusText}</div>
            <div style="margin-bottom:25px;font-size:1.2em;font-weight:600;">${tab.selectedMana>0?`<strong>${tab.selectedRuneName}</strong> — ${tab.selectedMana} mana — ${tab.formatTime(tab.targetTime)}`:'Selecione uma runa para começar'}</div>
            <div class="timer-controls">
                <button class="btn btn-start" onclick="startTimer(${tab.id})">▶️ INICIAR</button>
                <button class="btn btn-stop" onclick="stopTimer(${tab.id})">⏸️ PAUSAR</button>
                <button class="btn btn-reset" onclick="resetTimer(${tab.id})">🔄 RESETAR</button>
            </div>
        </div>`;
        return c;
    }

    function init() {
        const loaded = loadState();
        if (darkMode) { document.body.dataset.theme='dark'; document.getElementById('darkModeToggle').checked=true; document.getElementById('themeBtn').textContent='☀️'; }
        if ('Notification' in window && Notification.permission==='granted') { notificationsEnabled=true; document.getElementById('notificationsToggle').checked=true; }
        initWorker(); initServiceWorker();
        if (!loaded || tabs.length===0) addNewTab(); else renderTabs();
        setTimeout(updateStatusFromCapabilities, 500);
        setInterval(updateStatusFromCapabilities, 5000);
        document.addEventListener('click', ()=>{ try{ getAudioContext(); }catch(e){} }, { once:true });
    }

    init();
    </script>
</body>
</html>
