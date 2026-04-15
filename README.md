<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Your Crypto Agent</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css">
<style>
  :root {
    --bg:#0a0a0f; --bg2:#10101a; --bg3:#16162a; --border:#1e1e3a;
    --accent:#7c3aed; --accent2:#06b6d4; --accent3:#10b981; --red:#ef4444;
    --text:#e2e8f0; --muted:#64748b;
    --mono:'Space Mono',monospace; --sans:'Pretendard','Inter',-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  body{background:var(--bg);color:var(--text);font-family:var(--sans);min-height:100vh;overflow-x:hidden;-webkit-font-smoothing:antialiased;}
  body::before{content:'';position:fixed;inset:0;background-image:linear-gradient(rgba(124,58,237,.04) 1px,transparent 1px),linear-gradient(90deg,rgba(124,58,237,.04) 1px,transparent 1px);background-size:40px 40px;pointer-events:none;z-index:0;}
  .app{position:relative;z-index:1;max-width:1200px;margin:0 auto;padding:24px 20px;}

  /* HEADER */
  header{display:flex;align-items:center;justify-content:space-between;margin-bottom:32px;padding-bottom:20px;border-bottom:1px solid var(--border);}
  .logo{display:flex;align-items:center;gap:12px;}
  .logo-mark{width:38px;height:38px;background:linear-gradient(135deg,var(--accent),var(--accent2));border-radius:10px;display:flex;align-items:center;justify-content:center;font-family:var(--mono);font-weight:700;font-size:14px;color:#fff;}
  .logo h1{font-size:18px;font-weight:700;letter-spacing:-.5px;}
  .logo span{font-size:11px;color:var(--muted);display:block;font-weight:300;}
  .header-right{display:flex;align-items:center;gap:10px;}
  .refresh-btn{background:var(--bg3);border:1px solid var(--border);color:var(--text);padding:8px 14px;border-radius:8px;font-family:var(--sans);font-size:13px;cursor:pointer;transition:all .2s;display:flex;align-items:center;gap:6px;}
  .refresh-btn:hover{border-color:var(--accent);color:var(--accent);}
  .refresh-btn svg{transition:transform .6s;}
  .refresh-btn.loading svg{animation:spin 1s linear infinite;}
  @keyframes spin{to{transform:rotate(360deg);}}
  .status-dot{width:8px;height:8px;border-radius:50%;background:var(--accent3);box-shadow:0 0 6px var(--accent3);animation:pulse 2s infinite;}
  @keyframes pulse{0%,100%{opacity:1;}50%{opacity:.4;}}
  .update-time{font-size:11px;color:var(--muted);font-family:var(--mono);}

  /* TOTAL CARD */
  .total-card{background:linear-gradient(135deg,#13132a 0%,#1a0a2e 50%,#0a1a2a 100%);border:1px solid var(--border);border-radius:20px;padding:32px 36px;margin-bottom:24px;position:relative;overflow:hidden;}
  .total-card::before{content:'';position:absolute;top:-80px;right:-80px;width:260px;height:260px;background:radial-gradient(circle,rgba(124,58,237,.2) 0%,transparent 70%);pointer-events:none;}
  .total-label{font-size:11px;color:var(--muted);letter-spacing:2px;text-transform:uppercase;margin-bottom:8px;}
  .total-value{font-family:var(--mono);font-size:48px;font-weight:700;letter-spacing:-2px;line-height:1;background:linear-gradient(135deg,#fff 0%,var(--accent2) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:6px;}
  .total-meta{display:flex;gap:28px;margin-top:20px;flex-wrap:wrap;}
  .meta-item .ml{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;}
  .meta-item .mv{font-family:var(--mono);font-size:16px;font-weight:700;margin-top:2px;}
  .mv.up{color:var(--accent3);} .mv.down{color:var(--red);}

  /* CHARTS */
  .charts-row{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:20px;}
  @media(max-width:700px){.charts-row{grid-template-columns:1fr;}}
  .chart-card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:22px;}
  .card-title{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:18px;}
  .donut-wrap{display:flex;align-items:center;gap:20px;}
  .donut-legend{flex:1;display:flex;flex-direction:column;gap:7px;}
  .legend-item{display:flex;align-items:center;gap:8px;font-size:12px;}
  .legend-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0;}
  .legend-name{flex:1;} .legend-pct{font-family:var(--mono);color:var(--muted);font-size:11px;}
  .bar-chart{display:flex;flex-direction:column;gap:10px;}
  .bar-row{display:flex;align-items:center;gap:10px;}
  .bar-label{font-size:12px;width:70px;color:var(--muted);text-align:right;flex-shrink:0;}
  .bar-track{flex:1;height:8px;background:var(--bg3);border-radius:4px;overflow:hidden;}
  .bar-fill{height:100%;border-radius:4px;transition:width 1s cubic-bezier(.16,1,.3,1);}
  .bar-val{font-family:var(--mono);font-size:11px;color:var(--muted);width:70px;flex-shrink:0;}

  /* TABS */
  .tabs{display:flex;gap:4px;margin-bottom:16px;flex-wrap:wrap;}
  .tab-btn{background:var(--bg2);border:1px solid var(--border);color:var(--muted);padding:8px 18px;border-radius:8px;font-family:var(--sans);font-size:13px;cursor:pointer;transition:all .15s;}
  .tab-btn.active{background:var(--accent);border-color:var(--accent);color:#fff;}
  .tab-btn:hover:not(.active){border-color:var(--accent);color:var(--accent);}

  /* TABLE */
  .table-card{background:var(--bg2);border:1px solid var(--border);border-radius:16px;overflow:hidden;margin-bottom:20px;}
  .table-header{display:flex;align-items:center;justify-content:space-between;padding:18px 22px;border-bottom:1px solid var(--border);flex-wrap:wrap;gap:10px;}
  .table-filters{display:flex;gap:5px;flex-wrap:wrap;}
  .filter-btn{background:transparent;border:1px solid var(--border);color:var(--muted);padding:5px 11px;border-radius:6px;font-family:var(--sans);font-size:12px;cursor:pointer;transition:all .15s;}
  .filter-btn.active{background:var(--accent);border-color:var(--accent);color:#fff;}
  .filter-btn:hover:not(.active){border-color:var(--accent);color:var(--accent);}
  table{width:100%;border-collapse:collapse;}
  thead tr{border-bottom:1px solid var(--border);}
  thead th{padding:11px 20px;text-align:left;font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;font-weight:400;}
  thead th:not(:first-child){text-align:right;}
  tbody tr{border-bottom:1px solid rgba(30,30,58,.5);transition:background .15s;}
  tbody tr:hover{background:rgba(124,58,237,.05);}
  tbody tr:last-child{border-bottom:none;}
  td{padding:13px 20px;font-size:13px;}
  td:not(:first-child){text-align:right;}
  .asset-cell{display:flex;align-items:center;gap:10px;}
  .asset-icon{width:34px;height:34px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-family:var(--mono);font-size:10px;font-weight:700;flex-shrink:0;}
  .asset-name{font-weight:500;font-size:13px;} .asset-sym{font-size:11px;color:var(--muted);font-family:var(--mono);}
  .source-badge{display:inline-flex;align-items:center;gap:4px;font-size:10px;padding:2px 7px;border-radius:4px;font-family:var(--mono);background:var(--bg3);border:1px solid var(--border);}
  .source-dot{width:5px;height:5px;border-radius:50%;}
  .price-cell,.change-cell,.value-cell{font-family:var(--mono);font-size:12px;}
  .value-cell{font-weight:700;}
  .change-cell.up{color:var(--accent3);} .change-cell.down{color:var(--red);}
  .pnl-cell{font-family:var(--mono);font-size:12px;}
  .pnl-cell.up{color:var(--accent3);} .pnl-cell.down{color:var(--red);} .pnl-cell.none{color:var(--muted);}
  .qty-cell{font-family:var(--mono);font-size:12px;color:var(--muted);cursor:pointer;}
  .qty-cell:hover{color:var(--accent2);text-decoration:underline;}
  .del-btn{background:none;border:none;color:var(--muted);cursor:pointer;font-size:14px;padding:2px 6px;border-radius:4px;transition:all .15s;}
  .del-btn:hover{color:var(--red);background:rgba(239,68,68,.1);}
  .defi-tag{font-size:10px;padding:1px 6px;border-radius:3px;font-family:var(--mono);}
  .defi-tag.lp{background:rgba(16,185,129,.15);color:#10b981;}
  .defi-tag.staking{background:rgba(124,58,237,.15);color:#a78bfa;}
  .defi-tag.wallet{background:rgba(6,182,212,.15);color:#06b6d4;}
  .defi-tag.farming{background:rgba(245,158,11,.15);color:#f59e0b;}
  .price-loading{color:var(--muted);font-size:11px;font-family:var(--mono);}

  /* 탭별 요약 카드 */
  .tab-summary{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;margin-bottom:20px;}
  .sum-card{background:var(--bg2);border:1px solid var(--border);border-radius:12px;padding:16px 18px;}
  .sum-label{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;}
  .sum-value{font-family:var(--mono);font-size:20px;font-weight:700;}
  .sum-value.green{color:var(--accent3);} .sum-value.cyan{color:var(--accent2);}
  .sum-divider{height:1px;background:var(--border);margin:10px 0;}
  .sum-sub{font-size:11px;color:var(--muted);}

  /* YIELD TRACKER */
  .yield-section{background:var(--bg2);border:1px solid var(--border);border-radius:16px;overflow:hidden;margin-bottom:20px;}
  .yield-header{display:flex;align-items:center;justify-content:space-between;padding:18px 22px;border-bottom:1px solid var(--border);flex-wrap:wrap;gap:10px;}
  .yield-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:0;}
  .yield-card{padding:20px 22px;border-right:1px solid var(--border);border-bottom:1px solid var(--border);position:relative;min-height:180px;}
  .yield-card:hover{background:rgba(124,58,237,.04);}
  .yield-protocol{display:flex;align-items:center;gap:10px;margin-bottom:12px;}
  .yield-logo{width:32px;height:32px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:12px;flex-shrink:0;font-weight:700;font-family:var(--mono);}
  .yield-name{font-size:13px;font-weight:500;} .yield-pool{font-size:11px;color:var(--muted);font-family:var(--mono);}
  .yield-apy{font-family:var(--mono);font-size:28px;font-weight:700;color:var(--accent3);line-height:1;margin-bottom:4px;}
  .yield-apy.manual{color:var(--accent2);}
  .yield-apy-label{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:12px;}
  .yield-meta{display:flex;gap:16px;flex-wrap:wrap;}
  .yml{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;}
  .ymv{font-family:var(--mono);font-size:13px;font-weight:500;margin-top:1px;}
  .yield-badge{position:absolute;top:14px;right:14px;font-size:9px;padding:2px 6px;border-radius:4px;font-family:var(--mono);}
  .yield-badge.live{background:rgba(16,185,129,.15);color:#10b981;}
  .yield-badge.manual{background:rgba(6,182,212,.15);color:#06b6d4;}
  .yield-btns{position:absolute;bottom:14px;right:14px;display:flex;gap:5px;}
  .yield-edit-btn{background:none;border:1px solid var(--border);color:var(--muted);padding:3px 8px;border-radius:5px;font-size:11px;cursor:pointer;transition:all .15s;font-family:var(--sans);}
  .yield-edit-btn:hover{border-color:var(--accent2);color:var(--accent2);}
  .yield-del-btn{background:none;border:1px solid rgba(239,68,68,.3);color:var(--red);padding:3px 8px;border-radius:5px;font-size:11px;cursor:pointer;transition:all .15s;font-family:var(--sans);}
  .yield-del-btn:hover{background:rgba(239,68,68,.1);}
  .add-yield-card{padding:20px 22px;display:flex;align-items:center;justify-content:center;cursor:pointer;color:var(--muted);font-size:13px;gap:8px;border-bottom:1px solid var(--border);transition:all .15s;min-height:80px;}
  .add-yield-card:hover{color:var(--accent);background:rgba(124,58,237,.04);}

  /* ADD SECTION */
  .add-section{background:var(--bg2);border:1px dashed var(--border);border-radius:16px;padding:22px;margin-bottom:20px;}
  .add-section h3{font-size:13px;font-weight:500;color:var(--muted);margin-bottom:14px;}
  .add-tabs{display:flex;gap:6px;margin-bottom:16px;}
  .add-tab{background:var(--bg3);border:1px solid var(--border);color:var(--muted);padding:5px 12px;border-radius:6px;font-size:12px;cursor:pointer;transition:all .15s;font-family:var(--sans);}
  .add-tab.active{background:var(--accent);border-color:var(--accent);color:#fff;}
  .add-form{display:flex;gap:8px;flex-wrap:wrap;align-items:flex-end;}
  .form-group{display:flex;flex-direction:column;gap:4px;}
  .form-group label{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;}
  .add-form input,.add-form select{background:var(--bg3);border:1px solid var(--border);color:var(--text);padding:9px 12px;border-radius:8px;font-family:var(--sans);font-size:13px;outline:none;transition:border-color .15s;}
  .add-form input:focus,.add-form select:focus{border-color:var(--accent);}
  .add-form select option{background:var(--bg3);}
  .add-btn{background:linear-gradient(135deg,var(--accent),#5b21b6);border:none;color:#fff;padding:10px 20px;border-radius:8px;font-family:var(--sans);font-size:13px;font-weight:600;cursor:pointer;transition:opacity .15s;white-space:nowrap;}
  .add-btn:hover{opacity:.85;}
  .apy-type-toggle{display:flex;gap:0;border:1px solid var(--border);border-radius:6px;overflow:hidden;}
  .apy-type-btn{background:var(--bg3);border:none;color:var(--muted);padding:6px 12px;font-size:12px;cursor:pointer;font-family:var(--sans);transition:all .15s;}
  .apy-type-btn.active{background:var(--accent);color:#fff;}

  /* MODAL */
  .modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:100;align-items:center;justify-content:center;}
  .modal-overlay.open{display:flex;}
  .modal{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:28px;min-width:320px;max-width:90vw;max-height:90vh;overflow-y:auto;}
  .modal h3{font-size:15px;margin-bottom:16px;}
  .modal-label{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;margin-bottom:4px;margin-top:12px;}
  .modal input,.modal select{background:var(--bg3);border:1px solid var(--border);color:var(--text);padding:10px 14px;border-radius:8px;font-family:var(--mono);font-size:13px;width:100%;outline:none;margin-bottom:2px;}
  .modal input:focus,.modal select:focus{border-color:var(--accent);}
  .modal-btns{display:flex;gap:8px;justify-content:flex-end;margin-top:16px;}
  .modal-btn{padding:8px 18px;border-radius:8px;font-family:var(--sans);font-size:13px;cursor:pointer;border:1px solid var(--border);background:var(--bg3);color:var(--text);transition:all .15s;}
  .modal-btn.primary{background:var(--accent);border-color:var(--accent);color:#fff;}
  .modal-btn:hover{opacity:.85;}

  /* SAVE FLASH */
  .save-flash{position:fixed;bottom:20px;right:20px;background:var(--accent3);color:#fff;padding:10px 18px;border-radius:10px;font-size:13px;z-index:200;opacity:0;transition:opacity .3s;pointer-events:none;}
  .save-flash.show{opacity:1;}

  @keyframes fadeUp{from{opacity:0;transform:translateY(12px);}to{opacity:1;transform:translateY(0);}}
  @media(max-width:800px){.total-value{font-size:34px;}}
</style>
</head>
<body>
<div class="app">

  <!-- HEADER -->
  <header>
    <div class="logo">
      <div class="logo-mark">CP</div>
      <div><h1>Your Crypto Agent</h1><span>Your Personal Crypto Dashboard</span></div>
    </div>
    <div class="header-right">
      <div class="status-dot"></div>
      <span class="update-time" id="updateTime">--:--:--</span>
      <button class="refresh-btn" onclick="exportData()" title="내 데이터 백업">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M7 10l5 5 5-5M12 15V3"/></svg>
        내보내기
      </button>
      <label class="refresh-btn" style="cursor:pointer" title="백업 파일로 복원">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M17 8l-5-5-5 5M12 3v12"/></svg>
        가져오기
        <input type="file" accept=".json" onchange="importData(event)" style="display:none">
      </label>
      <button class="refresh-btn" id="refreshBtn" onclick="fetchPrices()">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c-4.97 0-9-4.03-9-9m9-9a9 9 0 019 9"/></svg>
        가격 갱신
      </button>
    </div>
  </header>

  <!-- TOTAL -->
  <div class="total-card">
    <div class="total-label">총 평가 자산</div>
    <div class="total-value" id="totalValue">$0.00</div>
    <div class="total-meta">
      <div class="meta-item"><div class="ml">거래소 자산</div><div class="mv" id="cexTotal">$0.00</div></div>
      <div class="meta-item"><div class="ml">DeFi 자산</div><div class="mv" id="defiTotal">$0.00</div></div>
      <div class="meta-item"><div class="ml">보유 종목</div><div class="mv" id="assetCount">0</div></div>
      <div class="meta-item"><div class="ml">24h 변동</div><div class="mv" id="dayChange">--</div></div>
      <div class="meta-item"><div class="ml">총 미실현 손익</div><div class="mv" id="totalPnl">--</div></div>
    </div>
  </div>

  <!-- CHARTS -->
  <div class="charts-row">
    <div class="chart-card">
      <div class="card-title">자산 비중</div>
      <div class="donut-wrap">
        <svg width="120" height="120" viewBox="0 0 120 120" id="donutSvg">
          <circle cx="60" cy="60" r="48" fill="none" stroke="#16162a" stroke-width="20"/>
          <text x="60" y="55" text-anchor="middle" fill="#64748b" font-size="10" font-family="Space Mono">총계</text>
          <text x="60" y="70" text-anchor="middle" fill="#e2e8f0" font-size="13" font-family="Space Mono" font-weight="700" id="donutCenter">0</text>
        </svg>
        <div class="donut-legend" id="donutLegend"></div>
      </div>
    </div>
    <div class="chart-card">
      <div class="card-title">출처별 분포</div>
      <div class="bar-chart" id="sourceBarChart"></div>
    </div>
  </div>

  <!-- TABS -->
  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('cex',this)">거래소 자산</button>
    <button class="tab-btn" onclick="switchTab('defi',this)">DeFi 포지션</button>
    <button class="tab-btn" onclick="switchTab('all',this)">통합 자산</button>
  </div>

  <!-- ── CEX 패널 ── -->
  <div id="cexPanel">
    <div class="tab-summary" id="cexSummary"></div>
    <div class="table-card">
      <div class="table-header">
        <div class="card-title" style="margin:0">거래소 보유 자산</div>
        <div class="table-filters">
          <button class="filter-btn active" onclick="setFilter('all',this)">전체</button>
          <span id="filterBtns"></span>
        </div>
      </div>
      <table><thead><tr>
        <th>자산</th><th>거래소</th><th>수량</th><th>현재가</th><th>24h</th><th>평가금액</th><th>손익</th><th></th>
      </tr></thead><tbody id="cexTable"></tbody></table>
    </div>
  </div>

  <!-- ── DeFi 패널 ── -->
  <div id="defiPanel" style="display:none">
    <div class="tab-summary" id="defiSummary"></div>

    <!-- 수익률 트래커 -->
    <div class="yield-section">
      <div class="yield-header">
        <div>
          <div class="card-title" style="margin:0">DeFi 수익률 트래커</div>
          <div style="font-size:11px;color:var(--muted);margin-top:3px">🟢 자동연동 · 🔵 수동입력</div>
        </div>
        <button class="refresh-btn" id="yieldRefreshBtn" onclick="fetchYields()">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c-4.97 0-9-4.03-9-9m9-9a9 9 0 019 9"/></svg>
          수익률 갱신
        </button>
      </div>
      <div class="yield-grid" id="yieldGrid"></div>
    </div>

    <!-- DeFi 보유 자산 테이블 -->
    <div class="table-card">
      <div class="table-header"><div class="card-title" style="margin:0">DeFi 보유 자산</div></div>
      <table><thead><tr>
        <th>자산</th><th>프로토콜/유형</th><th>수량</th><th>현재가</th><th>진입가</th><th>평가금액</th><th>손익</th><th></th>
      </tr></thead><tbody id="defiTable"></tbody></table>
    </div>
  </div>

  <!-- ── 통합 패널 ── -->
  <div id="allPanel" style="display:none">
    <div class="tab-summary" id="allSummary"></div>
    <div class="table-card">
      <div class="table-header"><div class="card-title" style="margin:0">전체 자산 통합</div></div>
      <table><thead><tr>
        <th>자산</th><th>출처</th><th>수량</th><th>현재가</th><th>평가금액</th><th>손익</th><th></th>
      </tr></thead><tbody id="allTable"></tbody></table>
    </div>
  </div>

  <!-- ADD SECTION -->
  <div class="add-section">
    <h3>자산 추가</h3>
    <div class="add-tabs">
      <button class="add-tab active" onclick="switchAddTab('cex',this)">거래소 자산</button>
      <button class="add-tab" onclick="switchAddTab('wallet',this)">지갑 자산</button>
      <button class="add-tab" onclick="switchAddTab('defi',this)">DeFi 포지션</button>
    </div>
    <div class="add-form" id="addForm">
      <div class="form-group">
        <label>심볼</label>
        <input type="text" id="addSymbol" placeholder="BTC" maxlength="10" style="width:80px;text-transform:uppercase">
      </div>
      <div class="form-group">
        <label>수량</label>
        <input type="number" id="addQty" placeholder="0.5" step="any" style="width:110px">
      </div>
      <!-- CEX 전용 -->
      <div class="form-group" id="sourceGroup">
        <label>거래소</label>
        <div style="display:flex;gap:4px">
          <select id="addSource" onchange="handleSourceChange(this)" style="flex:1;min-width:110px">
            <option value="Binance">Binance</option>
            <option value="Upbit">Upbit</option>
            <option value="OKX">OKX</option>
            <option value="Bybit">Bybit</option>
            <option value="Bithumb">Bithumb</option>
            <option value="Kraken">Kraken</option>
            <option value="Gate.io">Gate.io</option>
            <option value="MEXC">MEXC</option>
            <option value="HTX">HTX</option>
            <option value="__custom__">✏️ 직접입력</option>
          </select>
          <input type="text" id="addSourceCustom" placeholder="거래소명" style="display:none;width:100px">
        </div>
      </div>
      <!-- 지갑 전용 -->
      <div class="form-group" id="walletGroup" style="display:none">
        <label>지갑</label>
        <div style="display:flex;gap:4px">
          <select id="addWallet" onchange="handleWalletChange(this)" style="flex:1;min-width:110px">
            <option value="MetaMask">🦊 MetaMask</option>
            <option value="Phantom">👻 Phantom</option>
            <option value="Keplr">🔑 Keplr</option>
            <option value="Trust Wallet">🛡️ Trust Wallet</option>
            <option value="Rabby">🐰 Rabby</option>
            <option value="__custom__">✏️ 직접입력</option>
          </select>
          <input type="text" id="addWalletCustom" placeholder="지갑명" style="display:none;width:100px">
        </div>
      </div>
      <!-- DeFi 전용 -->
      <div id="defiAddFields" style="display:none;contents">
        <div class="form-group">
          <label>프로토콜</label>
          <input type="text" id="addProtocol" placeholder="Uniswap..." style="width:120px">
        </div>
        <div class="form-group">
          <label>유형</label>
          <select id="addDefiType" style="width:110px">
            <option value="wallet">지갑보유</option>
            <option value="lp">LP 풀</option>
            <option value="staking">스테이킹</option>
            <option value="farming">파밍</option>
          </select>
        </div>
      </div>
      <!-- 공통: 진입가 -->
      <div class="form-group">
        <label>진입 평균가 (USD)</label>
        <input type="number" id="addEntryPrice" placeholder="선택사항" step="any" style="width:140px">
      </div>
      <div class="form-group" style="justify-content:flex-end">
        <label>&nbsp;</label>
        <button class="add-btn" onclick="addAsset()">+ 추가</button>
      </div>
    </div>
  </div>

</div><!-- /app -->

<!-- ASSET EDIT MODAL -->
<div class="modal-overlay" id="editModal">
  <div class="modal">
    <h3 id="modalTitle">수량 수정</h3>
    <div class="modal-label">수량</div>
    <input type="number" id="modalQty" placeholder="수량" step="any">
    <div class="modal-label">진입 평균가 (USD, 선택)</div>
    <input type="number" id="modalEntry" placeholder="달러">
    <div class="modal-btns">
      <button class="modal-btn" onclick="closeModal()">취소</button>
      <button class="modal-btn primary" onclick="saveModal()">저장</button>
    </div>
  </div>
</div>

<!-- YIELD MODAL -->
<div class="modal-overlay" id="yieldModal">
  <div class="modal" style="min-width:340px">
    <h3 id="yieldModalTitle">프로토콜 수정</h3>
    <div class="modal-label">프로토콜명</div>
    <input type="text" id="yieldModalName" placeholder="예: Doppler Finance">
    <div class="modal-label">풀 / 전략명</div>
    <input type="text" id="yieldModalPool" placeholder="예: USDC 볼트">
    <div class="modal-label" id="apyLabelEl">APY / APR (%)</div>
    <div style="display:flex;gap:8px;align-items:center;margin-bottom:2px">
      <div class="apy-type-toggle" id="apyTypeToggle">
        <button class="apy-type-btn active" onclick="setApyType('APY',this)">APY</button>
        <button class="apy-type-btn" onclick="setApyType('APR',this)">APR</button>
      </div>
      <input type="number" id="yieldModalApy" placeholder="예: 12.5" step="0.01" style="flex:1;margin:0">
    </div>
    <div class="modal-label">예치 자산 (심볼)</div>
    <input type="text" id="yieldModalAssetSym" placeholder="예: USDC, ETH" style="text-transform:uppercase">
    <div class="modal-label">예치 수량</div>
    <input type="number" id="yieldModalDeposit" placeholder="예: 1000" step="any">
    <div class="modal-label">이자 지급 주기</div>
    <select id="yieldModalCompoundFreq" style="width:100%;background:var(--bg3);border:1px solid var(--border);color:var(--text);padding:10px 14px;border-radius:8px;font-family:var(--sans);font-size:13px;outline:none;margin-bottom:0">
      <option value="">없음 (단리)</option>
      <option value="daily">매일</option>
      <option value="weekly">매주</option>
      <option value="biweekly">격주</option>
      <option value="monthly">매월</option>
      <option value="quarterly">분기 (3개월)</option>
    </select>
    <div class="modal-label">이자 지급일 (선택)</div>
    <input type="text" id="yieldModalCompoundDate" placeholder="예: 매주 금요일, 매월 1일" style="margin-bottom:0">
    <div class="modal-label">메모 (선택)</div>
    <input type="text" id="yieldModalNote" placeholder="예: 복리 적용 중">
    <div class="modal-btns">
      <button class="modal-btn" onclick="closeYieldModal()">취소</button>
      <button class="modal-btn primary" onclick="saveYieldModal()">저장</button>
    </div>
  </div>
</div>

<div class="save-flash" id="saveFlash">저장됨 ✓</div>

<script>
// ── 상수 ──────────────────────────────────────────────
const SOURCE_COLORS = {
  Binance:'#f0b90b',Upbit:'#1763b0',OKX:'#c8c8c8',Bybit:'#f7a600',
  Bithumb:'#f84960',Kraken:'#5741d9','Gate.io':'#2ecc71',MEXC:'#1eabe1',HTX:'#1e80ff',
  MetaMask:'#e2761b',Phantom:'#ab9ff2',Keplr:'#576eff','Trust Wallet':'#3375bb',Rabby:'#8697ff',
  DeFi:'#10b981',
};
const ASSET_COLORS = ['#7c3aed','#06b6d4','#10b981','#f59e0b','#ef4444','#8b5cf6','#0ea5e9','#34d399','#fbbf24','#f87171'];
const STABLES = new Set(['USDT','USDC','DAI','BUSD','TUSD','PYUSD','FRAX','LUSD','SUSD','USDP','GUSD','CUSD','EURC','USDE','USDS']);
const ASSET_KEY = 'cp_assets_v3';
const YIELD_KEY = 'cp_yields_v2';

function getSourceColor(src){ return SOURCE_COLORS[src]||'#888'; }
function isStable(sym){ return STABLES.has(sym.toUpperCase()); }

// ── 자산 데이터 ──────────────────────────────────────
const DEFAULT_ASSETS = [
  {id:1,symbol:'BTC', qty:0.35, source:'Binance',type:'cex',entryPrice:72000},
  {id:2,symbol:'ETH', qty:4.2,  source:'Binance',type:'cex',entryPrice:2800},
  {id:3,symbol:'SOL', qty:22,   source:'OKX',    type:'cex',entryPrice:120},
  {id:4,symbol:'ETH', qty:1.8,  source:'MetaMask',type:'wallet',entryPrice:2200},
  {id:5,symbol:'USDT',qty:3500, source:'Upbit',  type:'cex'},
  {id:6,symbol:'SUI', qty:500,  source:'Phantom', type:'wallet'},
  {id:7,symbol:'ETH', qty:1.0,  source:'MetaMask',type:'defi',defiType:'lp',protocol:'Uniswap',entryPrice:2000},
];

function loadAssets(){
  try{ const r=localStorage.getItem(ASSET_KEY); if(r) return JSON.parse(r); }catch(e){}
  return DEFAULT_ASSETS;
}
function saveAssets(){
  localStorage.setItem(ASSET_KEY,JSON.stringify(assets));
  showFlash();
}

// ── 수익률 데이터 ─────────────────────────────────────
const BUILTIN_YIELDS = [
  {id:'etherfi-usdc',name:'Ether.fi',pool:'Liquid USD Vault (USDC)',logo:'EF',logoColor:'#10b981',
   mode:'live',llamaProject:'ether.fi-liquid',llamaSymbol:'USDC',
   llamaPoolId:'7c12f175-37bc-41db-967a-1d7f1f4a23c4',
   assetSym:'USDC',apyType:'APY'},
  {id:'doppler',name:'Doppler Finance',pool:'APR (수동 - 매주 금요일)',logo:'DP',
   logoColor:'#7c3aed',mode:'manual',assetSym:'USDC',apyType:'APR'},
];

function loadYields(){
  try{ const r=localStorage.getItem(YIELD_KEY); if(r) return JSON.parse(r); }catch(e){}
  return BUILTIN_YIELDS.map(b=>({...b,apy:null,depositQty:null,tvlUsd:null,note:'',updatedAt:null}));
}
function saveYields(){
  localStorage.setItem(YIELD_KEY,JSON.stringify(yieldData));
  showFlash();
}

let assets = loadAssets();
let yieldData = loadYields();
let prices = {};
let currentFilter = 'all';
let currentMainTab = 'cex';
let currentAddTab = 'cex';
let editingId = null;
let editingYieldId = null;
let editingApyType = 'APY';
let nextId = Math.max(...assets.map(a=>a.id),0)+1;

function showFlash(){
  const f=document.getElementById('saveFlash');
  f.classList.add('show');
  setTimeout(()=>f.classList.remove('show'),1500);
}

// ── 가격 API ─────────────────────────────────────────
async function fetchPrices(){
  const btn=document.getElementById('refreshBtn');
  btn.classList.add('loading');
  const syms=[...new Set(assets.map(a=>a.symbol.toUpperCase()))];
  try{
    const res=await fetch('https://api.binance.com/api/v3/ticker/24hr');
    const data=await res.json();
    const map={};
    data.forEach(t=>{ if(t.symbol.endsWith('USDT')){ const s=t.symbol.replace('USDT',''); map[s]={usd:parseFloat(t.lastPrice),change24h:parseFloat(t.priceChangePercent)}; } });
    syms.forEach(s=>{
      if(STABLES.has(s)){ prices[s]={usd:1.0,change24h:0}; return; }
      if(map[s]) prices[s]=map[s];
    });
    document.getElementById('updateTime').textContent=new Date().toLocaleTimeString('ko-KR');
  }catch(e){ document.getElementById('updateTime').textContent='연결 실패'; }
  btn.classList.remove('loading');
  renderAll();
}

// ── 수익률 API ────────────────────────────────────────
async function fetchYields(){
  const btn=document.getElementById('yieldRefreshBtn');
  btn.classList.add('loading');
  const liveItems=yieldData.filter(y=>y.mode==='live');
  for(const y of liveItems){
    try{
      if(y.llamaPoolId){
        const r=await fetch(`https://yields.llama.fi/chart/${y.llamaPoolId}`);
        if(r.ok){
          const j=await r.json();
          const last=j.data?.slice(-1)[0];
          if(last){ y.apy=parseFloat(last.apy.toFixed(2)); y.tvlUsd=last.tvlUsd; y.updatedAt=new Date().toLocaleTimeString('ko-KR'); }
        }
      }
    }catch(e){ console.warn('yield fetch 실패:',y.name,e.message); }
  }
  saveYields();
  btn.classList.remove('loading');
  renderYieldGrid();
  renderDefiSummary();
}

// ── 포맷 ──────────────────────────────────────────────
function fmtUSD(v){
  if(v==null||isNaN(v)) return '--';
  if(v>=1e6) return '$'+(v/1e6).toFixed(2)+'M';
  if(v>=1e3) return '$'+v.toLocaleString('en-US',{minimumFractionDigits:2,maximumFractionDigits:2});
  return '$'+v.toFixed(v>=1?2:4);
}
function fmtQty(v,sym){
  if(STABLES.has(sym)) return v.toLocaleString('en-US',{maximumFractionDigits:0});
  if(v>=100) return v.toFixed(1);
  return v.toPrecision(4);
}
function fmtPnl(entry,current,qty){
  if(!entry||!current) return{str:'--',cls:'none'};
  const pnl=(current-entry)*qty, pct=pnl/(entry*qty)*100;
  return{str:(pnl>=0?'+':'')+fmtUSD(Math.abs(pnl))+' ('+(pct>=0?'+':'')+pct.toFixed(1)+'%)',cls:pnl>=0?'up':'down'};
}
function assetVal(a){ const p=prices[a.symbol.toUpperCase()]; return p?p.usd*a.qty:0; }

// ── renderAll ─────────────────────────────────────────
function renderAll(){
  renderTotals();
  renderFilterBtns();
  renderCexTable();
  renderCexSummary();
  renderDefiTable();
  renderDefiSummary();
  renderAllTable();
  renderAllSummary();
  renderYieldGrid();
  renderDonut();
  renderSourceBars();
}

// ── 상단 총합 ─────────────────────────────────────────
function renderTotals(){
  let total=0,cex=0,defi=0,changeSum=0,changeW=0,pnlTotal=0,hasPnl=false;
  assets.forEach(a=>{
    const p=prices[a.symbol.toUpperCase()]; if(!p) return;
    const v=p.usd*a.qty; total+=v;
    if(a.type==='defi') defi+=v; else cex+=v;
    changeSum+=p.change24h*v; changeW+=v;
    if(a.entryPrice){ pnlTotal+=(p.usd-a.entryPrice)*a.qty; hasPnl=true; }
  });
  document.getElementById('totalValue').textContent=fmtUSD(total);
  document.getElementById('cexTotal').textContent=fmtUSD(cex);
  document.getElementById('defiTotal').textContent=fmtUSD(defi);
  document.getElementById('assetCount').textContent=[...new Set(assets.map(a=>a.symbol))].length;
  if(changeW>0){
    const avg=changeSum/changeW, el=document.getElementById('dayChange');
    el.textContent=(avg>=0?'+':'')+avg.toFixed(2)+'%';
    el.className='mv '+(avg>=0?'up':'down');
  }
  if(hasPnl){
    const el=document.getElementById('totalPnl');
    el.textContent=(pnlTotal>=0?'+':'')+fmtUSD(Math.abs(pnlTotal));
    el.className='mv '+(pnlTotal>=0?'up':'down');
  }
}

// ── CEX 요약 ──────────────────────────────────────────
function renderCexSummary(){
  const list=assets.filter(a=>a.type==='cex'||a.type==='wallet');
  let total=0,pnl=0,hasPnl=false;
  list.forEach(a=>{ const p=prices[a.symbol.toUpperCase()]; if(!p) return; total+=p.usd*a.qty; if(a.entryPrice){pnl+=(p.usd-a.entryPrice)*a.qty;hasPnl=true;} });
  document.getElementById('cexSummary').innerHTML=`
    <div class="sum-card"><div class="sum-label">총 거래소+지갑 자산</div><div class="sum-value">${fmtUSD(total)}</div></div>
    <div class="sum-card"><div class="sum-label">미실현 손익</div><div class="sum-value ${hasPnl?(pnl>=0?'green':''):''}"> ${hasPnl?(pnl>=0?'+':'')+fmtUSD(Math.abs(pnl)):'--'}</div></div>
    <div class="sum-card"><div class="sum-label">보유 종목 수</div><div class="sum-value">${[...new Set(list.map(a=>a.symbol))].length}종</div></div>`;
}

// ── DeFi 요약 ─────────────────────────────────────────
function renderDefiSummary(){
  const list=assets.filter(a=>a.type==='defi');
  let stableVal=0,volVal=0,totalMonthly=0;
  list.forEach(a=>{
    const p=prices[a.symbol.toUpperCase()]; if(!p) return;
    const v=p.usd*a.qty;
    if(isStable(a.symbol)) stableVal+=v; else volVal+=v;
  });
  // 수익률 카드에서 월수익 합산
  yieldData.forEach(y=>{
    if(y.apy==null||!y.depositQty) return;
    const sym=(y.assetSym||'').toUpperCase();
    const p=prices[sym];
    const depositUsd=p?p.usd*y.depositQty:(isStable(sym)?y.depositQty:0);
    if(depositUsd) totalMonthly+=depositUsd*y.apy/100/12;
  });
  document.getElementById('defiSummary').innerHTML=`
    <div class="sum-card"><div class="sum-label">스테이블 자산</div><div class="sum-value green">${fmtUSD(stableVal)}</div><div class="sum-divider"></div><div class="sum-sub">USDC·USDT 등</div></div>
    <div class="sum-card"><div class="sum-label">변동성 자산</div><div class="sum-value cyan">${fmtUSD(volVal)}</div><div class="sum-divider"></div><div class="sum-sub">ETH·BTC 등</div></div>
    <div class="sum-card"><div class="sum-label">DeFi 총자산</div><div class="sum-value">${fmtUSD(stableVal+volVal)}</div></div>
    <div class="sum-card"><div class="sum-label">예상 월 수익</div><div class="sum-value green">${totalMonthly>0?fmtUSD(totalMonthly):'--'}</div><div class="sum-divider"></div><div class="sum-sub">수익률 트래커 기준</div></div>`;
}

// ── 통합 요약 ─────────────────────────────────────────
function renderAllSummary(){
  let total=0,stableTotal=0,volTotal=0,pnl=0,hasPnl=false;
  assets.forEach(a=>{
    const p=prices[a.symbol.toUpperCase()]; if(!p) return;
    const v=p.usd*a.qty; total+=v;
    if(isStable(a.symbol)) stableTotal+=v; else volTotal+=v;
    if(a.entryPrice){pnl+=(p.usd-a.entryPrice)*a.qty;hasPnl=true;}
  });
  document.getElementById('allSummary').innerHTML=`
    <div class="sum-card"><div class="sum-label">전체 총자산</div><div class="sum-value">${fmtUSD(total)}</div></div>
    <div class="sum-card"><div class="sum-label">스테이블 자산</div><div class="sum-value green">${fmtUSD(stableTotal)}</div></div>
    <div class="sum-card"><div class="sum-label">변동성 자산</div><div class="sum-value cyan">${fmtUSD(volTotal)}</div></div>
    <div class="sum-card"><div class="sum-label">총 미실현 손익</div><div class="sum-value ${hasPnl?(pnl>=0?'green':''):''}"> ${hasPnl?(pnl>=0?'+':'')+fmtUSD(Math.abs(pnl)):'--'}</div></div>`;
}

// ── 필터 버튼 ─────────────────────────────────────────
function renderFilterBtns(){
  const srcs=[...new Set(assets.filter(a=>a.type==='cex'||a.type==='wallet').map(a=>a.source))];
  document.getElementById('filterBtns').innerHTML=srcs.map(s=>`<button class="filter-btn" onclick="setFilter('${s}',this)" style="margin-left:4px">${s}</button>`).join('');
}

// ── CEX 테이블 ────────────────────────────────────────
function renderCexTable(){
  const tbody=document.getElementById('cexTable');
  const list=assets.filter(a=>a.type==='cex'||a.type==='wallet');
  const filtered=currentFilter==='all'?list:list.filter(a=>a.source===currentFilter);
  tbody.innerHTML=filtered.map((a,i)=>{
    const sym=a.symbol.toUpperCase(), p=prices[sym];
    const priceStr=p?fmtUSD(p.usd):'<span class="price-loading">조회중...</span>';
    const val=p?p.usd*a.qty:0, valStr=p?fmtUSD(val):'--';
    const change=p?p.change24h:null;
    const changeStr=change!==null?`<span class="change-cell ${change>=0?'up':'down'}">${change>=0?'+':''}${change.toFixed(2)}%</span>`:'--';
    const pnl=fmtPnl(a.entryPrice,p?.usd,a.qty);
    const color=ASSET_COLORS[i%ASSET_COLORS.length], sColor=getSourceColor(a.source);
    return `<tr>
      <td><div class="asset-cell"><div class="asset-icon" style="background:${color}22;color:${color}">${sym.slice(0,3)}</div>
        <div><div class="asset-name">${sym}</div><div class="asset-sym">${a.type==='wallet'?'지갑':'CEX'}</div></div></div></td>
      <td><span class="source-badge"><span class="source-dot" style="background:${sColor}"></span>${a.source}</span></td>
      <td class="qty-cell" onclick="openModal(${a.id})">${fmtQty(a.qty,sym)}</td>
      <td class="price-cell">${priceStr}</td>
      <td>${changeStr}</td>
      <td class="value-cell">${valStr}</td>
      <td class="pnl-cell ${pnl.cls}">${pnl.str}</td>
      <td><button class="del-btn" onclick="deleteAsset(${a.id})">✕</button></td>
    </tr>`;
  }).join('');
}

// ── DeFi 테이블 ───────────────────────────────────────
function renderDefiTable(){
  const tbody=document.getElementById('defiTable');
  const list=assets.filter(a=>a.type==='defi');
  if(!list.length){ tbody.innerHTML='<tr><td colspan="8" style="text-align:center;color:var(--muted);padding:32px;font-size:13px">DeFi 포지션이 없습니다.</td></tr>'; return; }
  tbody.innerHTML=list.map((a,i)=>{
    const sym=a.symbol.toUpperCase(), p=prices[sym];
    const val=p?p.usd*a.qty:0;
    const pnl=fmtPnl(a.entryPrice,p?.usd,a.qty);
    const color=ASSET_COLORS[(i+4)%ASSET_COLORS.length];
    const dtype=a.defiType||'wallet';
    const dtypeLabel={lp:'LP 풀',staking:'스테이킹',farming:'파밍',wallet:'지갑보유'}[dtype]||dtype;
    return `<tr>
      <td><div class="asset-cell"><div class="asset-icon" style="background:${color}22;color:${color}">${sym.slice(0,3)}</div>
        <div><div class="asset-name">${sym}</div><div class="asset-sym">${a.protocol||'DeFi'}</div></div></div></td>
      <td><span class="defi-tag ${dtype}">${dtypeLabel}</span><div style="font-size:11px;color:var(--muted);margin-top:2px">${a.protocol||''}</div></td>
      <td class="qty-cell" onclick="openModal(${a.id})">${fmtQty(a.qty,sym)}</td>
      <td class="price-cell">${p?fmtUSD(p.usd):'<span class="price-loading">조회중</span>'}</td>
      <td class="price-cell" style="color:var(--muted)">${a.entryPrice?fmtUSD(a.entryPrice):'--'}</td>
      <td class="value-cell">${p?fmtUSD(val):'--'}</td>
      <td class="pnl-cell ${pnl.cls}">${pnl.str}</td>
      <td><button class="del-btn" onclick="deleteAsset(${a.id})">✕</button></td>
    </tr>`;
  }).join('');
}

// ── 통합 테이블 ───────────────────────────────────────
function renderAllTable(){
  const tbody=document.getElementById('allTable');
  tbody.innerHTML=assets.map((a,i)=>{
    const sym=a.symbol.toUpperCase(), p=prices[sym];
    const val=p?p.usd*a.qty:0;
    const pnl=fmtPnl(a.entryPrice,p?.usd,a.qty);
    const color=ASSET_COLORS[i%ASSET_COLORS.length];
    const typeLabel={cex:'거래소',wallet:'지갑',defi:'DeFi'}[a.type]||a.type;
    return `<tr>
      <td><div class="asset-cell"><div class="asset-icon" style="background:${color}22;color:${color}">${sym.slice(0,3)}</div>
        <div><div class="asset-name">${sym}</div><div class="asset-sym">${typeLabel}</div></div></div></td>
      <td><span class="source-badge">${a.source}</span></td>
      <td class="qty-cell" onclick="openModal(${a.id})">${fmtQty(a.qty,sym)}</td>
      <td class="price-cell">${p?fmtUSD(p.usd):'--'}</td>
      <td class="value-cell">${p?fmtUSD(val):'--'}</td>
      <td class="pnl-cell ${pnl.cls}">${pnl.str}</td>
      <td><button class="del-btn" onclick="deleteAsset(${a.id})">✕</button></td>
    </tr>`;
  }).join('');
}

// ── 수익률 그리드 ─────────────────────────────────────
function renderYieldGrid(){
  const grid=document.getElementById('yieldGrid');
  if(!yieldData.length){
    grid.innerHTML='<div style="padding:32px;color:var(--muted);font-size:13px;text-align:center;grid-column:1/-1">등록된 프로토콜이 없습니다.</div>';
    return;
  }
  grid.innerHTML=yieldData.map(y=>{
    const sym=(y.assetSym||'').toUpperCase();
    const p=prices[sym];
    // 예치 USD 계산: 변동성이면 시세 반영
    let depositUsd=null;
    if(y.depositQty){
      if(isStable(sym)||!sym) depositUsd=y.depositQty;
      else if(p) depositUsd=p.usd*y.depositQty;
    }
    const apyDisplay=y.apy!=null?y.apy.toFixed(2)+'%':(y.mode==='live'?'연동중...':'--');
    const apyClass=y.mode==='live'?'':'manual';
    const badgeClass=y.mode==='live'?'live':'manual';
    const badgeText=y.mode==='live'?'🟢 자동':'🔵 수동';
    const tvl=y.tvlUsd?fmtUSD(y.tvlUsd):'--';
    const depositStr=depositUsd?fmtUSD(depositUsd)+(y.depositQty&&sym?` (${fmtQty(y.depositQty,sym)} ${sym})`:''):'--';
    const monthly=y.apy!=null&&depositUsd?fmtUSD(depositUsd*y.apy/100/12)+'/월':'--';
    const apyTypeLabel=y.apyType||'APY';
    const updated=y.updatedAt||y.note||'미입력';
    const freqLabel={'daily':'매일','weekly':'매주','biweekly':'격주','monthly':'매월','quarterly':'분기'}[y.compoundFreq]||null;
    const compoundStr=freqLabel?(y.compoundDate?`${freqLabel} (${y.compoundDate})`:`${freqLabel} 복리`):'단리';
    return `<div class="yield-card">
      <span class="yield-badge ${badgeClass}">${badgeText}</span>
      <div class="yield-protocol">
        <div class="yield-logo" style="background:${y.logoColor}22;color:${y.logoColor}">${y.logo||'??'}</div>
        <div><div class="yield-name">${y.name}</div><div class="yield-pool">${y.pool}</div></div>
      </div>
      <div class="yield-apy ${apyClass}">${apyDisplay}</div>
      <div class="yield-apy-label">${apyTypeLabel} · ${sym||'-'}</div>
      <div class="yield-meta">
        <div><div class="yml">TVL</div><div class="ymv">${tvl}</div></div>
        <div><div class="yml">예치금</div><div class="ymv" style="font-size:11px">${depositStr}</div></div>
        <div><div class="yml">월 수익</div><div class="ymv" style="color:var(--accent3)">${monthly}</div></div>
        <div><div class="yml">이자 방식</div><div class="ymv" style="font-size:11px">${compoundStr}</div></div>
      </div>
      <div style="margin-top:10px;font-size:10px;color:var(--muted);font-family:var(--mono)">갱신: ${updated}</div>
      <div class="yield-btns">
        <button class="yield-edit-btn" onclick="openYieldModal('${y.id}')">수정</button>
        <button class="yield-del-btn" onclick="deleteYield('${y.id}')">삭제</button>
      </div>
    </div>`;
  }).join('')+`<div class="add-yield-card" onclick="openYieldModalNew()"><span style="font-size:18px">+</span> 프로토콜 추가</div>`;
}

// ── 도넛/바 차트 ──────────────────────────────────────
function renderDonut(){
  const svg=document.getElementById('donutSvg'), legend=document.getElementById('donutLegend');
  const bySymbol={};
  assets.forEach(a=>{ const p=prices[a.symbol.toUpperCase()]; if(!p) return; bySymbol[a.symbol]=(bySymbol[a.symbol]||0)+p.usd*a.qty; });
  const total=Object.values(bySymbol).reduce((s,v)=>s+v,0);
  if(!total){legend.innerHTML='';return;}
  document.getElementById('donutCenter').textContent=Object.keys(bySymbol).length+'종';
  const entries=Object.entries(bySymbol).sort((a,b)=>b[1]-a[1]).slice(0,6);
  const r=48,cx=60,cy=60,stroke=20,circ=2*Math.PI*r;
  svg.querySelectorAll('.arc').forEach(e=>e.remove());
  let offset=0;
  entries.forEach(([sym,val],i)=>{
    const pct=val/total,dashLen=pct*circ,color=ASSET_COLORS[i%ASSET_COLORS.length];
    const c=document.createElementNS('http://www.w3.org/2000/svg','circle');
    c.setAttribute('class','arc'); c.setAttribute('cx',cx); c.setAttribute('cy',cy); c.setAttribute('r',r);
    c.setAttribute('fill','none'); c.setAttribute('stroke',color); c.setAttribute('stroke-width',stroke);
    c.setAttribute('stroke-dasharray',`${dashLen} ${circ-dashLen}`);
    c.setAttribute('stroke-dashoffset',circ/4-offset);
    svg.insertBefore(c,svg.querySelector('text')); offset+=dashLen;
  });
  legend.innerHTML=entries.map(([sym,val],i)=>`<div class="legend-item"><span class="legend-dot" style="background:${ASSET_COLORS[i%ASSET_COLORS.length]}"></span><span class="legend-name">${sym}</span><span class="legend-pct">${(val/total*100).toFixed(1)}%</span></div>`).join('');
}

function renderSourceBars(){
  const c=document.getElementById('sourceBarChart'), bySource={};
  assets.forEach(a=>{ const p=prices[a.symbol.toUpperCase()]; if(!p) return; bySource[a.source]=(bySource[a.source]||0)+p.usd*a.qty; });
  const total=Object.values(bySource).reduce((s,v)=>s+v,0);
  if(!total){c.innerHTML='';return;}
  c.innerHTML=Object.entries(bySource).sort((a,b)=>b[1]-a[1]).map(([src,val])=>{
    const pct=(val/total*100).toFixed(1), color=getSourceColor(src);
    return `<div class="bar-row"><div class="bar-label">${src}</div><div class="bar-track"><div class="bar-fill" style="width:0%;background:${color}" data-w="${pct}"></div></div><div class="bar-val">${fmtUSD(val)}</div></div>`;
  }).join('');
  requestAnimationFrame(()=>{ c.querySelectorAll('.bar-fill').forEach(el=>requestAnimationFrame(()=>el.style.width=el.dataset.w+'%')); });
}

// ── 탭 전환 ───────────────────────────────────────────
function switchTab(tab,btn){
  currentMainTab=tab;
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('cexPanel').style.display=tab==='cex'?'':'none';
  document.getElementById('defiPanel').style.display=tab==='defi'?'':'none';
  document.getElementById('allPanel').style.display=tab==='all'?'':'none';
}

function setFilter(f,btn){
  currentFilter=f;
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  renderCexTable();
}

// ── 자산 추가 탭 ──────────────────────────────────────
function switchAddTab(tab,btn){
  currentAddTab=tab;
  document.querySelectorAll('.add-tab').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('sourceGroup').style.display=tab==='cex'?'':'none';
  document.getElementById('walletGroup').style.display=tab==='wallet'?'':'none';
  document.getElementById('defiAddFields').style.display=tab==='defi'?'':'none';
}

function handleSourceChange(sel){
  const c=document.getElementById('addSourceCustom');
  c.style.display=sel.value==='__custom__'?'':'none';
  if(sel.value==='__custom__') c.focus();
}
function handleWalletChange(sel){
  const c=document.getElementById('addWalletCustom');
  c.style.display=sel.value==='__custom__'?'':'none';
  if(sel.value==='__custom__') c.focus();
}

// ── 자산 추가 ─────────────────────────────────────────
function addAsset(){
  const sym=document.getElementById('addSymbol').value.trim().toUpperCase();
  const qty=parseFloat(document.getElementById('addQty').value);
  const entryPrice=parseFloat(document.getElementById('addEntryPrice').value)||null;
  if(!sym||isNaN(qty)||qty<=0){alert('심볼과 수량을 입력해주세요.');return;}

  if(currentAddTab==='cex'){
    const srcSel=document.getElementById('addSource').value;
    const source=srcSel==='__custom__'?(document.getElementById('addSourceCustom').value.trim()||'Custom'):srcSel;
    assets.push({id:nextId++,symbol:sym,qty,source,type:'cex',entryPrice});
  } else if(currentAddTab==='wallet'){
    const walSel=document.getElementById('addWallet').value;
    const source=walSel==='__custom__'?(document.getElementById('addWalletCustom').value.trim()||'Wallet'):walSel;
    assets.push({id:nextId++,symbol:sym,qty,source,type:'wallet',entryPrice});
  } else {
    const protocol=document.getElementById('addProtocol').value.trim()||'DeFi';
    const defiType=document.getElementById('addDefiType').value;
    assets.push({id:nextId++,symbol:sym,qty,source:'DeFi',type:'defi',defiType,protocol,entryPrice});
  }

  ['addSymbol','addQty','addEntryPrice','addProtocol'].forEach(id=>{const el=document.getElementById(id);if(el)el.value='';});
  saveAssets();
  renderAll();
  fetchPrices();
}

// ── 자산 삭제 ─────────────────────────────────────────
function deleteAsset(id){
  if(!confirm('이 자산을 삭제할까요?')) return;
  assets=assets.filter(a=>a.id!==id);
  saveAssets();
  renderAll(); // 즉시 반영
}

// ── 자산 수정 모달 ────────────────────────────────────
function openModal(id){
  editingId=id;
  const a=assets.find(x=>x.id===id); if(!a) return;
  document.getElementById('modalTitle').textContent=`${a.symbol} 수정`;
  document.getElementById('modalQty').value=a.qty;
  document.getElementById('modalEntry').value=a.entryPrice||'';
  document.getElementById('editModal').classList.add('open');
}
function closeModal(){ document.getElementById('editModal').classList.remove('open'); editingId=null; }
function saveModal(){
  const qty=parseFloat(document.getElementById('modalQty').value);
  const entry=parseFloat(document.getElementById('modalEntry').value)||null;
  if(isNaN(qty)||qty<=0){alert('수량을 입력해주세요.');return;}
  const a=assets.find(x=>x.id===editingId);
  if(a){a.qty=qty;a.entryPrice=entry;}
  saveAssets(); closeModal(); renderAll();
}

// ── 수익률 모달 ───────────────────────────────────────
function setApyType(type,btn){
  editingApyType=type;
  document.querySelectorAll('.apy-type-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}

function openYieldModal(id){
  editingYieldId=id;
  const y=yieldData.find(x=>x.id===id); if(!y) return;
  document.getElementById('yieldModalTitle').textContent=y.name+' 수정';
  document.getElementById('yieldModalName').value=y.name;
  document.getElementById('yieldModalPool').value=y.pool;
  document.getElementById('yieldModalApy').value=y.apy??'';
  document.getElementById('yieldModalApy').disabled=y.mode==='live';
  document.getElementById('yieldModalAssetSym').value=y.assetSym||'';
  document.getElementById('yieldModalDeposit').value=y.depositQty??'';
  document.getElementById('yieldModalNote').value=y.note||'';
  document.getElementById('yieldModalCompoundFreq').value=y.compoundFreq||'';
  document.getElementById('yieldModalCompoundDate').value=y.compoundDate||'';
  editingApyType=y.apyType||'APY';
  document.querySelectorAll('.apy-type-btn').forEach(b=>b.classList.toggle('active',b.textContent===editingApyType));
  document.getElementById('yieldModal').classList.add('open');
}

function openYieldModalNew(){
  editingYieldId='__new__';
  document.getElementById('yieldModalTitle').textContent='프로토콜 추가';
  ['yieldModalName','yieldModalPool','yieldModalAssetSym','yieldModalNote','yieldModalCompoundDate'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('yieldModalCompoundFreq').value='';
  document.getElementById('yieldModalApy').value='';
  document.getElementById('yieldModalApy').disabled=false;
  document.getElementById('yieldModalDeposit').value='';
  editingApyType='APY';
  document.querySelectorAll('.apy-type-btn').forEach((b,i)=>b.classList.toggle('active',i===0));
  document.getElementById('yieldModal').classList.add('open');
}

function closeYieldModal(){ document.getElementById('yieldModal').classList.remove('open'); editingYieldId=null; }

function saveYieldModal(){
  const name=document.getElementById('yieldModalName').value.trim();
  const pool=document.getElementById('yieldModalPool').value.trim();
  const apy=parseFloat(document.getElementById('yieldModalApy').value)||null;
  const assetSym=document.getElementById('yieldModalAssetSym').value.trim().toUpperCase();
  const depositQty=parseFloat(document.getElementById('yieldModalDeposit').value)||null;
  const note=document.getElementById('yieldModalNote').value.trim();
  const compoundFreq=document.getElementById('yieldModalCompoundFreq').value;
  const compoundDate=document.getElementById('yieldModalCompoundDate').value.trim();

  if(editingYieldId==='__new__'){
    if(!name){alert('프로토콜명을 입력해주세요.');return;}
    const id='custom_'+Date.now();
    const initials=name.replace(/[^A-Za-z0-9]/g,'').slice(0,2).toUpperCase()||'??';
    yieldData.push({id,name,pool:pool||'직접입력',logo:initials,logoColor:'#64748b',mode:'manual',
      apy,apyType:editingApyType,assetSym,depositQty,tvlUsd:null,note,compoundFreq,compoundDate,updatedAt:new Date().toLocaleTimeString('ko-KR')});
  } else {
    const y=yieldData.find(x=>x.id===editingYieldId); if(!y) return;
    y.name=name||y.name; y.pool=pool||y.pool;
    if(y.mode==='manual') y.apy=apy;
    y.apyType=editingApyType; y.assetSym=assetSym; y.depositQty=depositQty;
    y.note=note; y.compoundFreq=compoundFreq; y.compoundDate=compoundDate; y.updatedAt=new Date().toLocaleTimeString('ko-KR');
  }
  saveYields(); closeYieldModal(); renderYieldGrid(); renderDefiSummary();
}

function deleteYield(id){
  const y=yieldData.find(x=>x.id===id); if(!y) return;
  if(!confirm(`'${y.name}' 프로토콜을 삭제할까요?`)) return;
  yieldData=yieldData.filter(x=>x.id!==id);
  saveYields(); renderYieldGrid(); renderDefiSummary(); // 즉시 반영
}

// ── 모달 외부 클릭 닫기 ───────────────────────────────
document.getElementById('editModal').addEventListener('click',e=>{ if(e.target===e.currentTarget) closeModal(); });
document.getElementById('yieldModal').addEventListener('click',e=>{ if(e.target===e.currentTarget) closeYieldModal(); });

// ── 복리 계산: 원금 × (1 + r/n)^(n×t) ─────────────────
function getCompoundedQty(y){
  if(!y.depositQty||!y.apy||!y.compoundFreq) return y.depositQty||0;
  const r=y.apy/100;
  const freqMap={daily:365,weekly:52,biweekly:26,monthly:12,quarterly:4};
  const n=freqMap[y.compoundFreq]||1;
  // 마지막 갱신 시점부터 현재까지 경과 연수 계산
  const since=y.compoundSince?new Date(y.compoundSince):new Date();
  const now=new Date();
  const years=(now-since)/(1000*60*60*24*365);
  if(years<=0) return y.depositQty;
  return y.depositQty*Math.pow(1+r/n,n*years);
}

// ── 데이터 내보내기 ───────────────────────────────────────
function exportData(){
  const data={
    exportedAt:new Date().toISOString(),
    assets:JSON.parse(localStorage.getItem('cp_assets_v3')||'[]'),
    yields:JSON.parse(localStorage.getItem('cp_yields_v2')||'[]'),
  };
  const blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download='crypto_portfolio_backup_'+new Date().toISOString().slice(0,10)+'.json';
  a.click();
  URL.revokeObjectURL(a.href);
}

// ── 데이터 가져오기 ───────────────────────────────────────
function importData(e){
  const file=e.target.files[0]; if(!file) return;
  const reader=new FileReader();
  reader.onload=ev=>{
    try{
      const data=JSON.parse(ev.target.result);
      if(!data.assets&&!data.yields){alert('올바른 백업 파일이 아닙니다.');return;}
      if(!confirm('현재 데이터를 백업 파일로 덮어씁니다. 계속할까요?')) return;
      if(data.assets) localStorage.setItem('cp_assets_v3',JSON.stringify(data.assets));
      if(data.yields) localStorage.setItem('cp_yields_v2',JSON.stringify(data.yields));
      location.reload();
    }catch(err){alert('파일을 읽는 중 오류가 발생했습니다.');}
  };
  reader.readAsText(file);
  e.target.value='';
}

// ── 초기화 ────────────────────────────────────────────
fetchPrices();
fetchYields();
setInterval(fetchPrices,60000);
</script>
</body>
</html>
