<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Crypto Agent</title>
    <link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        :root { --bg:#0a0a0f; --bg2:#10101a; --border:#1e1e3a; --accent:#7c3aed; --text:#e2e8f0; --muted:#64748b; }
        body { background: var(--bg); color: var(--text); font-family: 'Noto Sans KR', sans-serif; margin: 0; padding: 20px; }
        .container { max-width: 1200px; margin: 0 auto; }
        header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--border); padding-bottom: 20px; margin-bottom: 20px; }
        .card { background: var(--bg2); border: 1px solid var(--border); padding: 20px; border-radius: 12px; margin-bottom: 15px; }
        .btn { background: var(--accent); color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; margin-right: 10px; }
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th, td { text-align: left; padding: 12px; border-bottom: 1px solid var(--border); }
        th { color: var(--muted); font-size: 0.85rem; }
        .qty { font-family: 'Space Mono', monospace; font-weight: bold; color: var(--accent); }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1 style="margin:0; font-family:'Space Mono';">YOUR CRYPTO <span style="color:var(--accent)">AGENT</span></h1>
            <div id="lastUpdate" style="font-size:0.8rem; color:var(--muted)">Data as of: 2026-04-15</div>
        </header>

        <div class="card">
            <h3>Wallet & CEX Assets</h3>
            <table id="assetTable">
                <thead><tr><th>Asset</th><th>Quantity</th><th>Source</th><th>Type</th></tr></thead>
                <tbody id="assetBody"></tbody>
            </table>
        </div>

        <div class="card">
            <h3>Staking & Yields</h3>
            <table id="yieldTable">
                <thead><tr><th>Protocol</th><th>Pool</th><th>Deposit</th><th>APY/APR</th></tr></thead>
                <tbody id="yieldBody"></tbody>
            </table>
        </div>
    </div>

    <script>
        const data = {
            assets: [
                {s:"FIL", q:7934, src:"Binance", t:"CEX"},
                {s:"PENGU", q:567214, src:"Binance", t:"CEX"},
                {s:"WLD", q:5033, src:"Binance", t:"CEX"},
                {s:"OPEN", q:3249, src:"Binance", t:"CEX"},
                {s:"LINEA", q:1500625, src:"MetaMask", t:"Wallet"},
                {s:"ETH", q:0.15, src:"MetaMask", t:"Wallet"},
                {s:"BIRB", q:6588, src:"Binance", t:"CEX"}
            ],
            yields: [
                {n:"Ether.fi", p:"Liquid USD Vault", d:20043, a:4.56},
                {n:"Doppler Finance", p:"APR Weekly", d:35208, a:7},
                {n:"Pendle", p:"RE-USDC", d:1005, a:12.7},
                {n:"DP Vault", p:"XRP-Vault", d:66082, a:3},
                {n:"NEXO", p:"XRP Flexible", d:10100, a:3.5},
                {n:"Ether.fi Staking", p:"ETHFI Staking", d:18702, a:10}
            ]
        };

        document.getElementById('assetBody').innerHTML = data.assets.map(a => 
            `<tr><td><b>${a.s}</b></td><td class="qty">${a.q.toLocaleString()}</td><td>${a.src}</td><td>${a.t}</td></tr>`
        ).join('');

        document.getElementById('yieldBody').innerHTML = data.yields.map(y => 
            `<tr><td>${y.n}</td><td>${y.p}</td><td class="qty">${y.d.toLocaleString()}</td><td style="color:#10b981">${y.a}%</td></tr>`
        ).join('');
    </script>
</body>
</html>
