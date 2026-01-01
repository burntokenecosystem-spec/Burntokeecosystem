
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>$BURN ECOSYSTEM | Live Web3</title>
    
    <script src="https://unpkg.com/@thirdweb-dev/sdk@latest/dist/thirdweb-sdk.umd.js"></script>
    
    <style>
        :root { 
            --gold: #ffc800; --bg: #1e1f22; --panel: #2b2d31; 
            --text: #ffffff; --subtext: #949ba4; --green: #43b581; --purple: #9b59b6;
        }

        body { 
            background-color: var(--bg); color: var(--text); 
            font-family: 'Inter', sans-serif; margin: 0; 
            display: flex; flex-direction: column; align-items: center; min-height: 100vh;
        }

        /* TICKER */
        .burn-ticker {
            width: 100%; background: #000; border-bottom: 1px solid #333;
            padding: 12px 0; overflow: hidden; white-space: nowrap; font-family: monospace;
        }
        .ticker-text { display: inline-block; animation: scroll 40s linear infinite; color: var(--gold); font-size: 13px; }
        @keyframes scroll { from { transform: translateX(100%); } to { transform: translateX(-100%); } }

        /* HEADER */
        header { width: 90%; max-width: 1200px; padding: 25px 0; display: flex; justify-content: space-between; align-items: center; }
        .logo { font-size: 32px; font-weight: 800; color: var(--gold); letter-spacing: 2px; }
        .nav-links { display: flex; gap: 25px; align-items: center; }
        .nav-links a { color: var(--subtext); text-decoration: none; font-size: 14px; font-weight: 600; }

        /* REAL WALLET BUTTON STYLE */
        #connect-btn {
            background-color: var(--gold); color: #000; border: none;
            padding: 12px 24px; border-radius: 20px; font-weight: 700;
            cursor: pointer; font-size: 14px; transition: 0.3s;
        }
        #connect-btn:hover { background-color: #ffd65c; transform: scale(1.05); }

        /* MAIN UI */
        .container { display: flex; gap: 40px; width: 95%; max-width: 1200px; margin-top: 40px; justify-content: center; align-items: flex-start; }
        .ad-sidebar { flex: 0 0 220px; display: flex; flex-direction: column; gap: 20px; }
        .ad-box {
            width: 100%; height: 250px; background: var(--panel); border: 1px solid #444; border-radius: 12px;
            display: flex; align-items: center; justify-content: center; text-align: center;
            color: var(--gold); font-size: 13px; font-weight: 700; padding: 20px; box-sizing: border-box;
        }

        .terminal {
            flex: 1; max-width: 500px; background: var(--panel); border-radius: 20px;
            padding: 60px 40px; box-shadow: 0 20px 50px rgba(0,0,0,0.4); text-align: center;
        }
        .balance { font-size: 60px; font-weight: 800; margin: 20px 0; letter-spacing: -2px; }
        
        .main-btn {
            width: 100%; padding: 20px; border-radius: 12px; border: none; font-weight: 800;
            font-size: 16px; cursor: pointer; transition: 0.2s; text-transform: uppercase; margin-bottom: 15px;
        }
        .btn-yellow { background-color: #ffcc33; color: #000; }
        .btn-purple { background: linear-gradient(135deg, #a463f2 0%, #7d3cf2 100%); color: #fff; }
        .main-btn:disabled { opacity: 0.2; cursor: not-allowed; }

        footer { margin-top: auto; padding: 50px; color: #333; font-size: 12px; letter-spacing: 2px; }
        @media (max-width: 1000px) { .ad-sidebar { display: none; } }
    </style>
</head>
<body onload="init()">

    <audio id="bgMusic" loop><source src="https://www.bensound.com/bensound-music/bensound-thejazzpiano.mp3" type="audio/mpeg"></audio>

    <div class="burn-ticker">
        <div class="ticker-text">NETWORK_STATUS: ONLINE // TOTAL_BURNED: 154,209,101 $BURN // REVENUE_BUYBACK_PENDING: 4.2 ETH // NODE_ACTIVE_USERS: 1,429 // GLOBAL_BURN_INITIATED >></div>
    </div>

    <header>
        <div class="logo">$BURN</div>
        <div class="nav-links">
            <a href="https://x.com/BurnToken268358" target="_blank">Twitter</a>
            <a href="https://t.me/BurnTokenEcosystem" target="_blank">Telegram</a>
            <button id="connect-btn" onclick="connectWallet()">Connect Wallet</button>
        </div>
    </header>

    <div class="container">
        <div class="ad-sidebar">
            <div class="ad-box">THIS COULD BE<br>YOUR AD</div>
            <div class="ad-box">YOUR BRAND<br>HERE</div>
        </div>

        <div class="terminal">
            <span style="font-size: 13px; color: var(--subtext); text-transform: uppercase;">Vault Balance</span>
            <div class="balance" id="bal">1,000.0000</div>
            <div style="color: var(--green); font-weight: bold; margin-bottom: 30px;">Mining Rate: +<span id="yield">5.00</span> $BURN / 24h</div>

            <button class="main-btn btn-yellow" onclick="runAuth()">1. Authorize Shift (3 Ads)</button>
            <button class="main-btn btn-yellow" id="startBtn" disabled onclick="bootNode()">2. Activate Node</button>
            <button class="main-btn btn-purple" onclick="upgrade()">
                Turbo Multiplier (2x)
                <div style="font-size: 11px; opacity: 0.8;" id="cost">Cost: 1,000 $BURN</div>
            </button>
        </div>

        <div class="ad-sidebar">
            <div class="ad-box">THIS COULD BE<br>YOUR AD</div>
            <div class="ad-box">YOUR BRAND<br>HERE</div>
        </div>
    </div>

    <footer>&copy; 2025 $BURN ECOSYSTEM</footer>

    <script>
        let s = { bal: 1000.0, yld: 5.0, cst: 1000, active: false, last: Date.now() };

        // --- REAL WALLET LOGIC ---
        async function connectWallet() {
            const btn = document.getElementById('connect-btn');
            
            // Check if MetaMask or other wallet is installed
            if (window.ethereum) {
                try {
                    btn.innerText = "Connecting...";
                    // Request account access
                    const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
                    const account = accounts[0];
                    
                    // Show short version of address
                    btn.innerText = account.substring(0, 6) + "..." + account.substring(account.length - 4);
                    btn.style.backgroundColor = "var(--green)";
                    btn.style.color = "#fff";
                    
                    console.log("Connected to:", account);
                } catch (error) {
                    console.error("User denied account access");
                    btn.innerText = "Connect Wallet";
                }
            } else {
                alert("No Web3 Wallet detected. Please install MetaMask or Phantom.");
                window.open('https://metamask.io/download/', '_blank');
            }
        }

        function init() {
            const data = localStorage.getItem('burn_live_v5_2');
            if (data) {
                const p = JSON.parse(data);
                const diff = (Date.now() - p.last) / 1000;
                let earnings = p.active ? (p.yld / 86400) * diff : 0;
                s = { ...p, bal: p.bal + earnings, last: Date.now() };
            }
            updateUI();
            setInterval(() => {
                if (s.active) {
                    s.bal += (s.yld / 172800);
                    document.getElementById('bal').innerText = s.bal.toLocaleString(undefined, {minimumFractionDigits: 4});
                }
            }, 500);
            setInterval(() => { s.last = Date.now(); localStorage.setItem('burn_live_v5_2', JSON.stringify(s)); }, 1000);
        }

        function runAuth() {
            document.getElementById('bgMusic').play();
            alert("Streaming Ad 1/3..."); alert("Streaming Ad 2/3..."); alert("Streaming Ad 3/3...");
            document.getElementById('startBtn').disabled = false;
        }

        function bootNode() { s.active = true; alert("Neural Node Active."); }

        function upgrade() {
            if (s.bal >= s.cst) {
                s.bal -= s.cst; s.yld *= 2; s.cst *= 10; updateUI();
            } else { alert("Insufficient Balance"); }
        }

        function updateUI() {
            document.getElementById('bal').innerText = s.bal.toLocaleString(undefined, {minimumFractionDigits: 4});
            document.getElementById('yield').innerText = s.yld.toFixed(2);
            document.getElementById('cost').innerText = "Cost: " + s.cst.toLocaleString() + " $BURN";
        }
    </script>
</body>
</html>
