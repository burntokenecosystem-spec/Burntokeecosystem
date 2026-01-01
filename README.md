
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>$BURN | Ecosystem Terminal</title>
    <style>
        :root { 
            --gold: #ff9d00; --gold-glow: rgba(255, 157, 0, 0.3);
            --bg: #0a0a0c; --card: rgba(25, 25, 30, 0.8); 
            --text: #ffffff; --subtext: #a0a0a8;
            --green: #00ff88; --purple: #8a2be2;
        }

        body { 
            background: radial-gradient(circle at top, #1a1a2e 0%, var(--bg) 70%);
            color: var(--text); font-family: 'Inter', system-ui, sans-serif;
            margin: 0; display: flex; flex-direction: column; align-items: center; min-height: 100vh;
            overflow-x: hidden;
        }

        /* Glassmorphism Header */
        header { 
            width: 100%; padding: 25px 0; text-align: center;
            backdrop-filter: blur(10px); background: rgba(0,0,0,0.5);
            border-bottom: 1px solid rgba(255,255,255,0.05);
        }

        .logo { 
            font-weight: 900; font-size: 28px; color: var(--gold);
            text-transform: uppercase; letter-spacing: 5px;
            text-shadow: 0 0 20px var(--gold-glow);
        }

        /* Control Bar */
        .controls { display: flex; gap: 15px; margin-top: 15px; justify-content: center; }
        .icon-btn { 
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
            color: var(--subtext); padding: 8px 15px; border-radius: 30px; 
            cursor: pointer; font-size: 11px; font-weight: bold; transition: 0.3s;
        }
        .icon-btn:hover { background: var(--gold); color: #000; border-color: var(--gold); }

        /* Tabs */
        .tabs { display: flex; gap: 10px; margin: 30px 0; width: 90%; max-width: 420px; }
        .tab-btn { 
            flex: 1; padding: 14px; background: var(--card); border: 1px solid rgba(255,255,255,0.05);
            color: var(--subtext); border-radius: 12px; cursor: pointer; font-weight: bold;
            transition: 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .tab-btn.active { 
            background: var(--gold); color: #000; box-shadow: 0 0 25px var(--gold-glow);
            transform: translateY(-2px);
        }

        /* Content Sections */
        .content-section { display: none; width: 90%; max-width: 420px; }
        .content-section.active { display: block; animation: slideUp 0.5s ease-out; }

        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* Terminal Card */
        .glass-panel { 
            background: var(--card); backdrop-filter: blur(15px);
            border: 1px solid rgba(255,255,255,0.1); border-radius: 24px;
            padding: 40px; box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        .balance-amount { font-size: 42px; font-weight: 800; color: #fff; margin: 10px 0; }
        .daily-rate { color: var(--green); font-size: 14px; opacity: 0.9; }

        /* Buttons */
        .btn-grid { display: flex; flex-direction: column; gap: 15px; margin-top: 30px; }
        .main-btn { 
            padding: 20px; border-radius: 14px; border: none; font-weight: 800;
            text-transform: uppercase; cursor: pointer; transition: 0.3s;
        }
        .btn-gold { background: var(--gold); color: #000; box-shadow: 0 4px 15px var(--gold-glow); }
        .btn-gold:hover { transform: scale(1.02); filter: brightness(1.1); }
        .btn-purple { background: linear-gradient(135deg, #6e45e2 0%, #88d3ce 100%); color: #fff; }

        /* Whitepaper Styling */
        .wp-text { color: var(--subtext); line-height: 1.8; font-size: 15px; }
        .wp-highlight { color: var(--gold); font-weight: bold; }

        footer { margin-top: auto; padding: 30px; color: #333; font-size: 12px; }
    </style>
</head>
<body onload="init()">

    <audio id="bgMusic" loop>
        <source src="https://www.bensound.com/bensound-music/bensound-thejazzpiano.mp3" type="audio/mpeg">
    </audio>

    <header>
        <div class="logo">$BURN ECOSYSTEM</div>
        <div class="controls">
            <button class="icon-btn" onclick="toggleMusic()" id="musicBtn">Music: OFF</button>
            <a href="https://t.me/BurnTokenEcosystem" target="_blank" class="icon-btn">Telegram</a>
        </div>
    </header>

    <div class="tabs">
        <button id="t1" class="tab-btn active" onclick="openTab('terminal-tab', 't1')">Terminal</button>
        <button id="t2" class="tab-btn" onclick="openTab('whitepaper-tab', 't2')">Whitepaper</button>
    </div>

    <div id="terminal-tab" class="content-section active">
        <div class="glass-panel">
            <span style="font-size: 11px; color: var(--subtext); letter-spacing: 2px;">VAULTED ASSETS</span>
            <div class="balance-amount" id="bal">1,000.0000</div>
            <div class="daily-rate">Mining Rate: +<span id="yield">5.00</span> $BURN / 24h</div>

            <div class="btn-grid">
                <button class="main-btn btn-gold" id="btn-start" onclick="startNode()">Activate Mining Node</button>
                <button class="main-btn btn-purple" onclick="buyMultiplier()">
                    Turbo Multiplier (2x)
                    <div style="font-size: 10px; opacity: 0.8;" id="mult-cost">Cost: 1,000 $BURN</div>
                </button>
            </div>
            <p id="status" style="text-align:center; font-size:12px; color:#555; margin-top:20px;">System Hibernating.</p>
        </div>
    </div>

    <div id="whitepaper-tab" class="content-section">
        <div class="glass-panel wp-text">
            <h2 style="color:#fff; margin-top:0;">Ecosystem Vision</h2>
            <p>The <span class="wp-highlight">$BURN</span> protocol transforms digital attention into tangible scarcity. By leveraging ad-revenue cycles, we create a constant buy-pressure on the token.</p>
            <div style="background:rgba(255,255,255,0.03); padding:15px; border-radius:10px; border-left:4px solid var(--gold);">
                <b>Revenue Split:</b> 100% of Node revenue is used for automated $BURN buy-backs and permanent destruction.
            </div>
            <h3 style="color:#fff;">Phase 1</h3>
            <p>Development of the Terminal UI and Proof-of-Concept mining logic. Establishing the foundation for a global ad-network integration.</p>
            <button onclick="resetGame()" style="background:none; border:none; color:#444; font-size:10px; cursor:pointer; margin-top:20px;">[ RESET TERMINAL ]</button>
        </div>
    </div>

    <footer>EST. 2025 | SHANE, FOUNDER</footer>

    <script>
        let stats = { balance: 1000.0, yield: 5.0, cost: 1000, running: false, last: Date.now() };

        function init() {
            const saved = localStorage.getItem('burn_v2_2');
            if (saved) {
                const data = JSON.parse(saved);
                const diff = (Date.now() - data.last) / 1000;
                const earnings = data.running ? (data.yield / 86400) * diff : 0;
                stats = { ...data, balance: data.balance + earnings, last: Date.now() };
                if (earnings > 0.01) alert("Node Revenue Collected: " + earnings.toFixed(4) + " $BURN");
            }
            updateUI();
        }

        function save() {
            stats.last = Date.now();
            localStorage.setItem('burn_v2_2', JSON.stringify(stats));
        }

        function openTab(id, btn) {
            document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById(btn).classList.add('active');
        }

        function startNode() {
            stats.running = true;
            document.getElementById('status').innerText = "NODE ONLINE // MINING ACTIVE";
            document.getElementById('status').style.color = "var(--green)";
            save();
        }

        function buyMultiplier() {
            if (stats.balance >= stats.cost) {
                stats.balance -= stats.cost;
                stats.yield *= 2;
                stats.cost *= 10;
                updateUI();
                save();
            } else { alert("Insufficient Balance"); }
        }

        function updateUI() {
            document.getElementById('bal').innerText = stats.balance.toLocaleString(undefined, {minimumFractionDigits: 4});
            document.getElementById('yield').innerText = stats.yield.toFixed(2);
            document.getElementById('mult-cost').innerText = "Cost: " + stats.cost.toLocaleString() + " $BURN";
        }

        function toggleMusic() {
            const m = document.getElementById('bgMusic');
            const btn = document.getElementById('musicBtn');
            if (m.paused) { m.play(); btn.innerText = "Music: ON"; }
            else { m.pause(); btn.innerText = "Music: OFF"; }
        }

        function resetGame() { if(confirm("Erase all progress?")) { localStorage.clear(); location.reload(); } }

        setInterval(() => {
            if(stats.running) {
                stats.balance += (stats.yield / 172800);
                document.getElementById('bal').innerText = stats.balance.toLocaleString(undefined, {minimumFractionDigits: 4});
            }
        }, 500);

        setInterval(save, 500);
    </script>
</body>
</html>
