
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Burner Ecosystem | Secure Terminal</title>
    <style>
        :root { --gold: #ff9d00; --bg: #050505; --card: #111; --text: #eee; --green: #00ff88; --blue: #1DA1F2; --tg: #0088cc; --purple: #a349a4; }
        body { background: var(--bg); color: var(--text); font-family: 'Segoe UI', Roboto, sans-serif; margin: 0; display: flex; flex-direction: column; align-items: center; min-height: 100vh; }
        header { width: 100%; padding: 20px; text-align: center; border-bottom: 1px solid #222; background: #000; }
        .logo { font-weight: bold; font-size: 24px; color: var(--gold); letter-spacing: 3px; }
        .social-row { display: flex; gap: 10px; margin-top: 20px; justify-content: center; flex-wrap: wrap; }
        .small-btn { padding: 8px 15px; font-size: 11px; border-radius: 20px; text-decoration: none; font-weight: bold; text-transform: uppercase; border: 1px solid #333; cursor:pointer; }
        .s-tw { color: var(--blue); border-color: var(--blue); }
        .s-tg { color: var(--tg); border-color: var(--tg); }
        .terminal { background: var(--card); border: 2px solid #222; border-radius: 15px; padding: 30px; margin: 20px 10px; max-width: 400px; width: 90%; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        .balance-amount { font-size: 38px; font-weight: bold; color: #fff; display: block; margin: 5px 0; }
        .daily-rate { color: var(--green); font-size: 14px; }
        .btn-grid { display: flex; flex-direction: column; gap: 12px; margin-top: 20px; }
        .btn { padding: 18px; border-radius: 8px; border: none; font-weight: bold; cursor: pointer; text-transform: uppercase; font-size: 14px; transition: 0.2s; }
        .btn-auth { background: #fff; color: #000; }
        .btn-active { background: var(--gold); color: #000; }
        .btn-multiplier { background: linear-gradient(45deg, var(--purple), #6c2cf5); color: #fff; }
        .btn:disabled { opacity: 0.2; cursor: not-allowed; }
        #save-status { font-size: 9px; color: #444; margin-top: 10px; text-transform: uppercase; letter-spacing: 1px; }
        footer { margin-top: auto; padding: 20px; color: #444; font-size: 11px; text-align:center; }
    </style>
</head>
<body onload="init()">

    <header>
        <div class="logo">$BURN ECOSYSTEM</div>
        <div class="social-row">
            <a href="https://x.com/BurnToken268358" target="_blank" class="small-btn s-tw">Twitter</a>
            <a href="https://t.me/BurnTokenEcosystem" target="_blank" class="small-btn s-tg">Telegram</a>
        </div>
    </header>

    <div class="terminal">
        <div class="balance-display">
            <span style="font-size: 10px; color: #666; letter-spacing: 1px;">VAULTED $BURN</span>
            <span class="balance-amount" id="bal">1000.0000</span>
            <span class="daily-rate">Yield: +<span id="yield">5.00</span> / 24h</span>
        </div>

        <div class="btn-grid">
            <button class="btn btn-auth" onclick="triggerAds()">1. Authorize Shift</button>
            <button class="btn btn-active" id="btn-start" disabled onclick="startShift()">2. Activate Node</button>
            <button class="btn btn-multiplier" id="btn-mult" onclick="buyMultiplier()">
                Turbo Multiplier (2x)
                <div style="font-size:10px;" id="mult-cost">Cost: 1000 $BURN</div>
            </button>
            <button onclick="resetGame()" style="background:none; border:none; color:#333; font-size:10px; margin-top:10px; cursor:pointer;">[ Reset All Data ]</button>
        </div>
        <div id="save-status" style="text-align: center;">Syncing with Browser...</div>
        <p id="status-msg" style="font-size: 11px; color: #555; margin-top: 15px; text-align: center;">Ready.</p>
    </div>

    <footer>&copy; 2025 Burner Ecosystem. Shane, Founder.</footer>

    <script>
        window.stats = {
            balance: 1000.0000,
            dailyYield: 5.00,
            multiplierCost: 1000,
            isRunning: false,
            lastSaved: Date.now()
        };

        function init() {
            const saved = localStorage.getItem('burnerSaveV3');
            if (saved) {
                const loadedData = JSON.parse(saved);
                const now = Date.now();
                const secondsPassed = (now - loadedData.lastSaved) / 1000;
                
                let offlineEarnings = 0;
                if (loadedData.isRunning) {
                    offlineEarnings = (loadedData.dailyYield / 86400) * secondsPassed;
                }

                window.stats = {
                    ...loadedData,
                    balance: loadedData.balance + offlineEarnings,
                    lastSaved: now
                };
                
                if (offlineEarnings > 0.0001) {
                    alert("OFFLINE REVENUE: " + offlineEarnings.toFixed(4) + " $BURN collected.");
                }
                updateUI();
            }
        }

        function saveGame() {
            window.stats.lastSaved = Date.now();
            localStorage.setItem('burnerSaveV3', JSON.stringify(window.stats));
            
            // Visual feedback for the high-speed save
            const status = document.getElementById('save-status');
            status.style.color = "var(--green)";
            setTimeout(() => { status.style.color = "#222"; }, 200);
        }

        function triggerAds() {
            alert("Ads Verified.");
            document.getElementById('btn-start').disabled = false;
        }

        function startShift() {
            window.stats.isRunning = true;
            document.getElementById('status-msg').innerText = "Node Active...";
            document.getElementById('status-msg').style.color = "var(--green)";
            saveGame();
        }

        function buyMultiplier() {
            if (window.stats.balance >= window.stats.multiplierCost) {
                window.stats.balance -= window.stats.multiplierCost;
                window.stats.dailyYield *= 2;
                window.stats.multiplierCost *= 10;
                updateUI();
                saveGame();
            } else {
                alert("Insufficient $BURN");
            }
        }

        function updateUI() {
            document.getElementById('bal').innerText = window.stats.balance.toFixed(4);
            document.getElementById('yield').innerText = window.stats.dailyYield.toFixed(2);
            document.getElementById('mult-cost').innerText = "Cost: " + window.stats.multiplierCost.toLocaleString() + " $BURN";
        }

        function resetGame() {
            if(confirm("Erase all progress?")) {
                localStorage.removeItem('burnerSaveV3');
                location.reload();
            }
        }

        // Live Ticking (Visual)
        setInterval(() => {
            if(window.stats.isRunning) {
                window.stats.balance += (window.stats.dailyYield / 172800); // Ticking every 500ms
                document.getElementById('bal').innerText = window.stats.balance.toFixed(4);
            }
        }, 500);

        // High-Frequency Save (Every 0.5 Seconds)
        setInterval(() => {
            saveGame();
        }, 500);
    </script>
</body>
</html>
