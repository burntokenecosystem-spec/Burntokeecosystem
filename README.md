
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>$BURN | Neural Ad-Network</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;900&family=Rajdhani:wght@300;600&display=swap');

        :root { 
            --gold: #ff9d00; --gold-glow: rgba(255, 157, 0, 0.4);
            --neon-blue: #00d2ff; --bg: #030305; --panel: rgba(15, 15, 20, 0.95);
        }

        body { 
            background: var(--bg); color: #fff; font-family: 'Rajdhani', sans-serif; margin: 0;
            display: flex; flex-direction: column; align-items: center; min-height: 100vh;
            background-image: radial-gradient(circle at 50% 0%, #1a1a2e 0%, transparent 70%);
        }

        /* LIVE BURN TICKER */
        .burn-ticker {
            width: 100%; background: rgba(255, 157, 0, 0.1); border-bottom: 1px solid var(--gold);
            padding: 10px 0; overflow: hidden; white-space: nowrap; font-family: 'Orbitron'; font-size: 10px;
        }
        .ticker-text { display: inline-block; animation: scroll 30s linear infinite; color: var(--gold); }
        @keyframes scroll { from { transform: translateX(100%); } to { transform: translateX(-100%); } }

        header { width: 100%; padding: 30px 0; text-align: center; border-bottom: 1px solid rgba(255,255,255,0.05); }
        .logo { font-family: 'Orbitron'; font-size: 32px; color: var(--gold); letter-spacing: 10px; text-shadow: 0 0 15px var(--gold-glow); }

        .main-container { display: flex; gap: 20px; width: 95%; max-width: 1000px; margin-top: 20px; flex-wrap: wrap; }

        /* AD SIDEBARS */
        .ad-sidebar { 
            flex: 1; min-width: 160px; background: rgba(255,255,255,0.02); border: 1px dashed #333;
            display: flex; flex-direction: column; gap: 10px; padding: 10px; border-radius: 8px;
        }
        .ad-unit {
            width: 100%; height: 200px; background: #000; border: 1px solid #222;
            display: flex; align-items: center; justify-content: center; text-align: center;
            font-size: 11px; color: #444; text-transform: uppercase; cursor: pointer; transition: 0.3s;
        }
        .ad-unit:hover { border-color: var(--gold); color: var(--gold); background: rgba(255,157,0,0.05); }

        /* CENTER TERMINAL */
        .terminal-core { flex: 2; min-width: 350px; }
        .tabs { display: flex; background: #111; padding: 5px; border-radius: 8px; margin-bottom: 15px; }
        .tab-btn { flex: 1; background: none; border: none; color: #555; font-family: 'Orbitron'; font-size: 10px; cursor: pointer; padding: 10px; transition: 0.3s; }
        .tab-btn.active { color: var(--gold); background: rgba(255,157,0,0.1); }

        .hud-panel { 
            background: var(--panel); border: 1px solid rgba(255,255,255,0.1); padding: 30px; 
            border-radius: 4px; box-shadow: 0 0 40px rgba(0,0,0,0.8);
        }

        .balance-val { font-family: 'Orbitron'; font-size: 42px; color: #fff; text-shadow: 0 0 20px var(--gold-glow); }

        .cyber-btn {
            width: 100%; padding: 18px; margin-top: 15px; background: transparent; border: 1px solid var(--gold);
            color: var(--gold); font-family: 'Orbitron'; font-size: 11px; cursor: pointer; transition: 0.3s;
        }
        .cyber-btn:hover:not(:disabled) { background: var(--gold); color: #000; box-shadow: 0 0 20px var(--gold-glow); }
        .cyber-btn:disabled { border-color: #222; color: #222; }

        .wp-content { font-size: 14px; line-height: 1.8; color: #aaa; display: none; }
        .wp-content.active { display: block; }
        .wp-content h2 { color: #fff; font-family: 'Orbitron'; font-size: 18px; margin-top: 0; }
        .wp-highlight { color: var(--gold); }

        footer { margin-top: auto; padding: 40px; color: #222; font-family: 'Orbitron'; font-size: 9px; }
    </style>
</head>
<body onload="init()">

    <audio id="bgMusic" loop>
        <source src="https://www.bensound.com/bensound-music/bensound-thejazzpiano.mp3" type="audio/mpeg">
    </audio>

    <div class="burn-ticker">
        <div class="ticker-text">
            GLOBAL_BURN_INITIATED >> USER_741 BURNED 4,200 $BURN // REVENUE_BUYBACK COMPLETED: 12.4 ETH >> TOTAL_BURNED: 154,209,101 $BURN // NODE_77_ACTIVE >> 
            GLOBAL_BURN_INITIATED >> USER_992 BURNED 1,500 $BURN // REVENUE_BUYBACK COMPLETED: 0.8 ETH >> TOTAL_BURNED: 154,210,601 $BURN //
        </div>
    </div>

    <header>
        <div class="logo">$BURN</div>
    </header>

    <div class="main-container">
        <div class="ad-sidebar">
            <div class="ad-unit" onclick="alert('Ad space available. Contact Dev.')">THIS COULD BE<br>YOUR AD</div>
            <div class="ad-unit" onclick="alert('Ad space available. Contact Dev.')">SPACE FOR RENT<br>0.5 ETH/WK</div>
        </div>

        <div class="terminal-core">
            <div class="tabs">
                <button id="b1" class="tab-btn active" onclick="nav('term', 'b1')">TERMINAL</button>
                <button id="b2" class="tab-btn" onclick="nav('wp', 'b2')">WHITEPAPER</button>
            </div>

            <div class="hud-panel" id="term-panel">
                <div id="term-view">
                    <span style="font-size: 10px; color: var(--gold); letter-spacing: 3px;">NEURAL_VAULT</span>
                    <div class="balance-val" id="bal">1,000.0000</div>
                    <div style="color:var(--neon-blue); font-size: 11px;">YIELD: +<span id="yield">5.00</span> / 24H</div>

                    <button class="cyber-btn" onclick="auth()">01_AUTHORIZE_SHIFT (3_ADS)</button>
                    <button class="cyber-btn" id="start" disabled onclick="boot()">02_BOOT_NODE</button>
                    <button class="cyber-btn" onclick="upgrade()" style="border-color:#9d50bb; color:#9d50bb;">
                        TURBO_MULTIPLIER_2X
                        <div style="font-size: 8px;" id="cost">COST: 1,000</div>
                    </button>
                </div>

                <div id="wp-view" class="wp-content">
                    <h2>THE $BURN STRATEGY</h2>
                    <p>The <span class="wp-highlight">$BURN Ecosystem</span> is a first-of-its-kind decentralized ad-revenue aggregator. </p>
                    <p><b>1. REVENUE CAPTURE:</b> All ads displayed on this terminal generate real-world capital.</p>
                    <p><b>2. REVENUE RECYCLING:</b> 100% of generated revenue is used to purchase $BURN from the liquidity pool.</p>
                    <p><b>3. THE VOID:</b> Every token purchased via revenue is sent to the <span class="wp-highlight">0x000...DEAD</span> address, ensuring your tokens grow rarer every second.</p>
                    <button onclick="wipe()" style="color: #400; background:none; border:none; font-size:9px; cursor:pointer;">[ ERASE_DATA ]</button>
                </div>
            </div>
        </div>

        <div class="ad-sidebar">
            <div class="ad-unit" onclick="alert('Ad space available. Contact Dev.')">ADVERTISE<br>HERE</div>
            <div class="ad-unit" onclick="alert('Ad space available. Contact Dev.')">PROMOTIONS<br>OPEN</div>
        </div>
    </div>

    <footer>FLORIDA_DEVELOPMENT_UNIT // 2025</footer>

    <script>
        let state = { bal: 1000.0, yld: 5.0, cst: 1000, active: false, ts: Date.now() };

        function init() {
            const save = localStorage.getItem('burn_cyber_v4');
            if (save) {
                const d = JSON.parse(save);
                const seconds = (Date.now() - d.ts) / 1000;
                let earned = d.active ? (d.yld / 86400) * seconds : 0;
                state = { ...d, bal: d.bal + earned, ts: Date.now() };
                if (earned > 0.01) alert("NEURAL REVENUE RESTORED: " + earned.toFixed(4));
            }
            ui();
        }

        function save() { state.ts = Date.now(); localStorage.setItem('burn_cyber_v4', JSON.stringify(state)); }

        function nav(type, btn) {
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(btn).classList.add('active');
            if(type === 'term') {
                document.getElementById('term-view').style.display = 'block';
                document.getElementById('wp-view').style.display = 'none';
            } else {
                document.getElementById('term-view').style.display = 'none';
                document.getElementById('wp-view').style.display = 'block';
            }
        }

        function auth() {
            document.getElementById('bgMusic').play();
            alert("UPLINKING AD 1/3...");
            alert("UPLINKING AD 2/3...");
            alert("UPLINKING AD 3/3...");
            document.getElementById('start').disabled = false;
        }

        function boot() { state.active = true; save(); alert("NODE ONLINE"); }

        function upgrade() {
            if (state.bal >= state.cst) {
                state.bal -= state.cst; state.yld *= 2; state.cst *= 10; ui(); save();
            } else { alert("INSUFFICIENT FUNDS"); }
        }

        function ui() {
            document.getElementById('bal').innerText = state.bal.toLocaleString(undefined, {minimumFractionDigits: 4});
            document.getElementById('yield').innerText = state.yld.toFixed(2);
            document.getElementById('cost').innerText = "COST: " + state.cst.toLocaleString();
        }

        function wipe() { if(confirm("DELETE SAVE?")) { localStorage.clear(); location.reload(); } }

        setInterval(() => {
            if(state.active) {
                state.bal += (state.yld / 172800);
                document.getElementById('bal').innerText = state.bal.toLocaleString(undefined, {minimumFractionDigits: 4});
            }
        }, 500);

        setInterval(save, 500);
    </script>
</body>
</html>
