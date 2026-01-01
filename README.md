
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>$BURN ECOSYSTEM | Web3 Terminal</title>
    
    <style>
        /* --- BRANDING & COLOR VARIABLES --- */
        :root { 
            --gold: #ff9d00; 
            --bg-color: #050505; 
            --panel-bg: #111114; 
            --text-main: #eeeeee; 
            --text-dim: #888888; 
            --success: #00ff88;
        }

        /* --- GLOBAL STYLES --- */
        body { 
            background-color: var(--bg-color); 
            color: var(--text-main); 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            min-height: 100vh;
        }

        /* --- LIVE BURN TICKER --- */
        .burn-ticker {
            width: 100%; 
            background: #000; 
            border-bottom: 1px solid #222;
            padding: 10px 0; 
            overflow: hidden; 
            white-space: nowrap; 
            font-family: 'Courier New', Courier, monospace;
            font-size: 13px;
        }
        .ticker-text { 
            display: inline-block; 
            animation: scroll-left 35s linear infinite; 
            color: var(--gold); 
        }
        @keyframes scroll-left { 
            from { transform: translateX(100%); } 
            to { transform: translateX(-100%); } 
        }

        /* --- HEADER & CONNECT WALLET --- */
        header { 
            width: 100%; 
            padding: 20px 0; 
            display: flex;
            justify-content: center;
            align-items: center;
            border-bottom: 1px solid #222; 
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(10px);
            position: relative;
        }
        .logo { 
            font-weight: bold; 
            font-size: 28px; 
            color: var(--gold); 
            letter-spacing: 5px; 
            text-transform: uppercase;
        }
        .wallet-container {
            position: absolute;
            right: 5%;
        }
        .btn-connect {
            background: transparent;
            border: 1px solid var(--gold);
            color: var(--gold);
            padding: 10px 20px;
            border-radius: 30px;
            font-weight: bold;
            font-size: 12px;
            cursor: pointer;
            transition: 0.3s;
            text-transform: uppercase;
        }
        .btn-connect:hover {
            background: var(--gold);
            color: #000;
            box-shadow: 0 0 15px rgba(255, 157, 0, 0.4);
        }

        /* --- MAIN LAYOUT GRID --- */
        .layout-wrapper { 
            display: flex; 
            gap: 25px; 
            width: 95%; 
            max-width: 1200px; 
            margin-top: 40px; 
        }

        /* --- ADVERTISEMENT SIDEBARS --- */
        .ad-sidebar { 
            flex: 0 0 160px; 
            display: flex; 
            flex-direction: column; 
            gap: 15px; 
        }
        .ad-placeholder {
            width: 100%; height: 250px; background: var(--panel-bg); border: 1px solid #222;
            display: flex; align-items: center; justify-content: center; text-align: center;
            font-size: 11px; color: #444; font-weight: bold; cursor: pointer; border-radius: 8px;
            transition: 0.3s;
        }
        .ad-placeholder:hover { border-color: var(--gold); color: var(--gold); }

        /* --- CENTER TERMINAL CORE --- */
        .terminal-container { flex: 1; min-width: 350px; }
        .navigation-tabs { display: flex; gap: 10px; margin-bottom: 20px; }
        .tab-button { 
            flex: 1; padding: 14px; background: #1a1a1a; border: 1px solid #333; 
            color: #666; cursor: pointer; font-weight: bold; border-radius: 6px;
            text-transform: uppercase; transition: 0.3s;
        }
        .tab-button.active { background: var(--panel-bg); border-color: var(--gold); color: var(--gold); }

        .panel { 
            background: var(--panel-bg); border: 1px solid #222; padding: 40px; 
            border-radius: 12px; box-shadow: 0 15px 40px rgba(0,0,0,0.6);
        }

        /* --- GAME UI ELEMENTS --- */
        .balance-val { font-size: 48px; font-weight: bold; display: block; margin: 10px 0; color: #fff; }
        .yield-badge { color: var(--success); font-size: 15px; font-weight: bold; }

        .action-stack { display: flex; flex-direction: column; gap: 15px; margin-top: 30px; }
        .main-btn { padding: 20px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; text-transform: uppercase; font-size: 14px; transition: 0.2s; }
        .btn-auth { background: #ffffff; color: #000; }
        .btn-start { background: var(--gold); color: #000; }
        .btn-upgrade { background: linear-gradient(45deg, #a349a4, #6c2cf5); color: #fff; }
        .main-btn:disabled { opacity: 0.2; cursor: not-allowed; }

        /* --- WHITEPAPER --- */
        .whitepaper-body { line-height: 1.8; font-size: 16px; color: #cccccc; }
        .whitepaper-body h2 { color: var(--gold); border-bottom: 1px solid #333; padding-bottom: 10px; }
        .strategy-box { background: #000; padding: 20px; border-left: 5px solid var(--gold); margin: 25px 0; border-radius: 4px; }

        footer { margin-top: auto; padding: 40px; color: #333; font-size: 12px; }

        @media (max-width: 900px) { 
            .ad-sidebar { display: none; } 
            .wallet-container { position: static; margin-top: 10px; }
            header { flex-direction: column; }
        }
    </style>
</head>

<body onload="init()">

    <audio id="bgMusic" loop>
        <source src="https://www.bensound.com/bensound-music/bensound-thejazzpiano.mp3" type="audio/mpeg">
    </audio>

    <div class="burn-ticker">
        <div class="ticker-text">
            NETWORK_STATUS: ONLINE // TOTAL_BURNED: 154,209,101 $BURN // REVENUE_BUYBACK_PENDING: 4.2 ETH // NODE_ACTIVE_USERS: 1,429 // GLOBAL_BURN_INITIATED >> 
        </div>
    </div>

    <header>
        <div class="logo">$BURN ECOSYSTEM</div>
        <div class="wallet-container">
            <button class="btn-connect" id="walletBtn" onclick="connectWallet()">Connect Wallet</button>
        </div>
    </header>

    <div class="layout-wrapper">
        
        <aside class="ad-sidebar">
            <div class="ad-placeholder" onclick="adInquiry()">THIS COULD BE<br>YOUR AD</div>
            <div class="ad-placeholder" onclick="adInquiry()">ADVERTISE<br>HERE</div>
        </aside>

        <main class="terminal-container">
            <div class="navigation-tabs">
                <button id="nav-term" class="tab-button active" onclick="switchTab('terminal-view', 'nav-term')">Terminal</button>
                <button id="nav-wp" class="tab-button" onclick="switchTab('wp-view', 'nav-wp')">Whitepaper</button>
            </div>

            <div id="terminal-view" class="panel">
                <span style="font-size: 11px; color: var(--text-dim); letter-spacing: 2px;">SECURE VAULT BALANCE</span>
                <span class="balance-val" id="display-bal">1,000.0000</span>
                <span class="yield-badge">Mining Rate: +<span id="display-yield">5.00</span> $BURN / 24h</span>

                <div class="action-stack">
                    <button class="main-btn btn-auth" onclick="runAuthSequence()">1. Authorize Shift (3 Ads)</button>
                    <button class="main-btn btn-start" id="start-node-btn" disabled onclick="activateMining()">2. Activate Node</button>
                    <button class="main-btn btn-upgrade" onclick="buyTurbo()">
                        Turbo Multiplier (2x)
                        <div style="font-size: 10px; opacity: 0.8; margin-top: 4px;" id="display-cost">Cost: 1,000 $BURN</div>
                    </button>
                </div>
                
                <p id="system-status" style="text-align: center; color: #444; font-size: 12px; margin-top: 25px; font-weight: bold;">SYSTEM STATUS: STANDBY</p>
            </div>

            <div id="wp-view" class="panel whitepaper-body" style="display: none;">
                <h2>The $BURN Strategy</h2>
                <div class="strategy-box">
                    <b>THE REVENUE CYCLE:</b><br>
                    1. <b>Capture:</b> Real-world revenue from terminal advertisements.<br>
                    2. <b>Market Action:</b> 100% of revenue buys $BURN from liquidity.<br>
                    3. <b>Destruction:</b> Tokens are sent to the 0xDEAD address.
                </div>
                <button onclick="resetSystemData()" style="color: #400; background:none; border:none; cursor:pointer; font-size:10px;">[ WIPE DATA ]</button>
            </div>
        </main>

        <aside class="ad-sidebar">
            <div class="ad-placeholder" onclick="adInquiry()">SPACE FOR RENT</div>
            <div class="ad-placeholder" onclick="adInquiry()">YOUR BRAND<br>HERE</div>
        </aside>

    </div>

    <footer>FLORIDA DEVELOPMENT UNIT // FOUNDER: SHANE</footer>

    <script>
        let gameState = { balance: 1000.0, yieldRate: 5.0, multiplierCost: 1000, isNodeActive: false, lastSaved: Date.now() };

        function init() {
            const saved = localStorage.getItem('burner_dev_v4_4');
            if (saved) {
                const p = JSON.parse(saved);
                const diff = (Date.now() - p.lastSaved) / 1000;
                let earnings = p.isNodeActive ? (p.yieldRate / 86400) * diff : 0;
                gameState = { ...p, balance: p.balance + earnings, lastSaved: Date.now() };
                if (earnings > 0.01) alert("OFFLINE REVENUE: " + earnings.toFixed(4));
            }
            updateInterface();
            setInterval(processYield, 500);
            setInterval(autoSave, 1000);
        }

        // --- WEB3 SIMULATION ---
        function connectWallet() {
            const btn = document.getElementById('walletBtn');
            btn.innerText = "Connecting...";
            setTimeout(() => {
                const fakeAddress = "0x71" + Math.random().toString(36).substring(2, 6) + "..." + Math.random().toString(36).substring(2, 6);
                btn.innerText = fakeAddress;
                btn.style.borderColor = "var(--success)";
                btn.style.color = "var(--success)";
                alert("Wallet Connected Successfully.");
            }, 1000);
        }

        function switchTab(viewId, buttonId) {
            document.getElementById('terminal-view').style.display = (viewId === 'terminal-view') ? 'block' : 'none';
            document.getElementById('wp-view').style.display = (viewId === 'wp-view') ? 'block' : 'none';
            document.querySelectorAll('.tab-button').forEach(btn => btn.classList.remove('active'));
            document.getElementById(buttonId).classList.add('active');
        }

        function runAuthSequence() {
            const audio = document.getElementById('bgMusic');
            if (audio.paused) audio.play();
            alert("Ad 1/3..."); alert("Ad 2/3..."); alert("Ad 3/3...");
            document.getElementById('start-node-btn').disabled = false;
        }

        function activateMining() {
            gameState.isNodeActive = true;
            document.getElementById('system-status').innerText = "SYSTEM STATUS: NODE ONLINE";
            document.getElementById('system-status').style.color = "var(--success)";
        }

        function buyTurbo() {
            if (gameState.balance >= gameState.multiplierCost) {
                gameState.balance -= gameState.multiplierCost;
                gameState.yieldRate *= 2;
                gameState.multiplierCost *= 10;
                updateInterface();
            } else { alert("Insufficient Balance."); }
        }

        function processYield() {
            if (gameState.isNodeActive) {
                gameState.balance += (gameState.yieldRate / 172800);
                document.getElementById('display-bal').innerText = gameState.balance.toLocaleString(undefined, {minimumFractionDigits: 4});
            }
        }

        function updateInterface() {
            document.getElementById('display-bal').innerText = gameState.balance.toLocaleString(undefined, {minimumFractionDigits: 4});
            document.getElementById('display-yield').innerText = gameState.yieldRate.toFixed(2);
            document.getElementById('display-cost').innerText = "Cost: " + gameState.multiplierCost.toLocaleString() + " $BURN";
        }

        function autoSave() {
            gameState.lastSaved = Date.now();
            localStorage.setItem('burner_dev_v4_4', JSON.stringify(gameState));
        }

        function adInquiry() { alert("Contact Dev Team for ad slot inquiry."); }
        function resetSystemData() { if (confirm("Wipe all data?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
