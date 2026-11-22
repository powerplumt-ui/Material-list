
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Power Plum Admin Hub v4</title>
  <style>
    :root {
      --bg: #f1f5f9;
      --surface: #ffffff;
      --surface-soft: #f8fafc;
      --ink: #0f172a;
      --muted: #64748b;
      --border: #e2e8f0;
      --primary: #4f46e5;
      --purple: #7c3aed;
      --teal: #0d9488;
      --rose: #e11d48;
      --amber: #d97706;
      --emerald: #059669;
      --blue: #2563eb;
      --radius: 18px;
      --shadow: 0 10px 28px rgba(15, 23, 42, 0.07);
    }
    * { box-sizing: border-box; font-family: "Inter", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
    body { margin: 0; background: var(--bg); color: var(--ink); min-height: 100vh; }

    .topbar {
      position: sticky; top: 0; z-index: 40;
      background: rgba(255, 255, 255, 0.96);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid var(--border);
      padding: 12px 18px;
      display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 12px;
    }
    .brand-group { display: flex; align-items: center; gap: 12px; }
    .brand-logo {
      width: 40px; height: 40px; border-radius: 12px;
      background: linear-gradient(135deg, #4f46e5, #7c3aed);
      color: #fff; font-weight: 900; font-size: 16px;
      display: flex; align-items: center; justify-content: center;
    }
    .brand-title { margin: 0; font-size: 16px; font-weight: 800; }
    .brand-sub { margin: 2px 0 0; font-size: 11px; color: var(--emerald); font-weight: 700; }

    .nav-pills { display: none; flex-wrap: wrap; gap: 6px; background: var(--surface-soft); padding: 4px; border-radius: 14px; border: 1px solid var(--border); }
    .nav-btn {
      border: none; background: transparent; color: var(--muted);
      padding: 8px 13px; border-radius: 11px; font-size: 12px; font-weight: 700; cursor: pointer;
    }
    .nav-btn.active { background: var(--primary); color: #fff; }

    .actions-row { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
    .btn {
      border: none; border-radius: 12px; padding: 9px 14px;
      font-size: 12px; font-weight: 700; cursor: pointer; color: #fff; background: var(--primary);
    }
    .btn:hover { filter: brightness(1.06); }
    .btn-sm { padding: 6px 11px; font-size: 11px; border-radius: 9px; }
    .btn-full { width: 100%; }
    .btn-green { background: var(--emerald); }
    .btn-red { background: var(--rose); }
    .btn-amber { background: var(--amber); }
    .btn-blue { background: var(--blue); }
    .btn-slate { background: #334155; }
    .btn-teal { background: var(--teal); }
    .btn-violet { background: var(--purple); }
    .btn-outline { background: #fff; color: var(--ink); border: 1px solid var(--border); }

    .auth-wrap { min-height: calc(100vh - 70px); display: flex; align-items: center; justify-content: center; padding: 20px; }
    .auth-card {
      width: 100%; max-width: 420px; background: var(--surface);
      border-radius: 22px; padding: 26px; border: 1px solid var(--border); box-shadow: var(--shadow);
    }
    .auth-badge {
      display: inline-block; background: #eef2ff; color: var(--primary);
      font-size: 11px; font-weight: 800; padding: 4px 10px; border-radius: 999px; margin-bottom: 10px;
    }
    .auth-title { margin: 0 0 6px; font-size: 20px; font-weight: 800; }
    .auth-desc { margin: 0 0 16px; font-size: 12px; color: var(--muted); line-height: 1.5; }

    input, select {
      width: 100%; padding: 10px 12px; border: 1px solid #cbd5e1;
      border-radius: 12px; font-size: 13px; background: #fff; color: var(--ink);
    }
    input:focus, select:focus { outline: 2px solid var(--primary); border-color: var(--primary); }

    .feedback { margin-top: 10px; font-size: 12px; font-weight: 700; min-height: 18px; }
    .err { color: var(--rose); }
    .ok { color: var(--emerald); }

    .page-wrap { display: none; max-width: 1120px; margin: 0 auto; padding: 18px 16px 72px; }
    .admin-tab { display: none; }
    .admin-tab.active-tab { display: block; }

    .section-header { display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; gap: 10px; margin-bottom: 14px; }
    .section-title { margin: 0; font-size: 18px; font-weight: 800; }
    .section-sub { margin: 2px 0 0; font-size: 12px; color: var(--muted); }

    .kpi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(210px, 1fr)); gap: 14px; margin-bottom: 16px; }
    .kpi-card { border-radius: 18px; padding: 18px; color: #fff; box-shadow: 0 12px 24px rgba(15, 23, 42, 0.12); }
    .kpi-teal { background: linear-gradient(135deg, #06b6d4, #2563eb); }
    .kpi-purple { background: linear-gradient(135deg, #a855f7, #4f46e5); }
    .kpi-rose { background: linear-gradient(135deg, #f43f5e, #e11d48); }
    .kpi-amber { background: linear-gradient(135deg, #f59e0b, #ea580c); }
    .kpi-label { font-size: 11px; font-weight: 700; text-transform: uppercase; opacity: 0.92; }
    .kpi-value { font-size: 28px; font-weight: 900; margin: 6px 0 4px; }
    .kpi-meta { font-size: 11px; opacity: 0.92; font-weight: 600; }

    .card {
      background: var(--surface); border-radius: var(--radius); padding: 18px;
      border: 1px solid var(--border); box-shadow: var(--shadow); margin-bottom: 16px;
    }
    .card-title { margin: 0 0 6px; font-size: 14px; font-weight: 800; }
    .card-desc { margin: 0 0 12px; font-size: 12px; color: var(--muted); line-height: 1.45; }
    .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 16px; }
    .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 1fr)); gap: 10px; }
    .mt-2 { margin-top: 8px; }
    .mt-3 { margin-top: 12px; }
    .row { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }

    .charts-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(340px, 1fr)); gap: 16px; margin-bottom: 16px; }
    .chart-box { width: 100%; overflow-x: auto; }
    .legend-list { display: flex; flex-wrap: wrap; gap: 12px; margin-top: 10px; font-size: 11px; font-weight: 700; color: var(--muted); }
    .dot { width: 9px; height: 9px; border-radius: 50%; display: inline-block; margin-right: 5px; }

    .perf-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(290px, 1fr)); gap: 14px; margin-bottom: 16px; }
    .tech-card {
      background: var(--surface); border: 1px solid var(--border); border-radius: 16px;
      padding: 15px; box-shadow: 0 6px 18px rgba(15, 23, 42, 0.05);
      display: flex; flex-direction: column; justify-content: space-between; gap: 10px;
    }
    .tech-top { display: flex; justify-content: space-between; align-items: flex-start; gap: 8px; }
    .tech-name { margin: 0; font-size: 15px; font-weight: 800; }
    .tech-sub { font-size: 11px; color: var(--muted); margin-top: 2px; }
    .pill-score { background: #eef2ff; color: var(--primary); font-size: 11px; font-weight: 800; padding: 4px 9px; border-radius: 999px; white-space: nowrap; }
    .metrics-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; text-align: center; }
    .metric-box { background: var(--surface-soft); border: 1px solid var(--border); border-radius: 12px; padding: 8px 6px; }
    .metric-num { font-size: 16px; font-weight: 900; }
    .metric-lbl { font-size: 10px; color: var(--muted); font-weight: 700; margin-top: 2px; }
    .prog-wrap { height: 7px; background: #e2e8f0; border-radius: 999px; overflow: hidden; }
    .prog-bar { height: 100%; background: linear-gradient(90deg, #4f46e5, #06b6d4); border-radius: 999px; }

    .table-wrap { overflow-x: auto; max-height: 290px; border: 1px solid var(--border); border-radius: 14px; }
    table { width: 100%; border-collapse: collapse; font-size: 12px; text-align: left; }
    th { background: #0f172a; color: #fff; padding: 10px; position: sticky; top: 0; font-size: 11px; }
    td { padding: 9px 10px; border-bottom: 1px solid #f1f5f9; }

    .list-box {
      border: 1px solid var(--border); border-radius: 12px; padding: 8px;
      max-height: 140px; overflow-y: auto; background: var(--surface-soft);
      margin-top: 8px; font-size: 11px;
    }
    .list-item { display: flex; justify-content: space-between; align-items: center; gap: 8px; padding: 6px 4px; border-bottom: 1px solid #e2e8f0; }
    .list-item:last-child { border-bottom: none; }
    .truncate { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

    .toggle-row { display: flex; justify-content: space-between; align-items: center; gap: 8px; margin-bottom: 10px; flex-wrap: wrap; }
    .toggle-label { display: inline-flex; align-items: center; gap: 6px; font-size: 12px; font-weight: 700; cursor: pointer; }
    .toggle-label input { width: auto; cursor: pointer; }

    .modal-bg {
      display: none; position: fixed; inset: 0; background: rgba(15, 23, 42, 0.78);
      align-items: center; justify-content: center; padding: 16px; z-index: 100;
    }
    .modal-bg.open { display: flex; }
    .modal-card { background: #fff; width: 100%; max-width: 460px; border-radius: 18px; overflow: hidden; }
    .modal-head { background: #0f172a; color: #fff; padding: 10px 14px; display: flex; justify-content: space-between; align-items: center; font-size: 12px; font-weight: 700; }
    .modal-img { width: 100%; aspect-ratio: 16 / 9; object-fit: cover; background: #0f172a; display: block; }
    .modal-foot { background: #0f172a; color: #fff; padding: 12px 14px; font-size: 12px; }

    .link-status-banner {
      background: #eff6ff; border: 1px solid #bfdbfe; color: #1e3a8a;
      border-radius: 14px; padding: 12px 14px; font-size: 12px; line-height: 1.5; margin-bottom: 16px;
    }
  </style>
</head>
<body>

  <div id="previewPopupModal" class="modal-bg">
    <div class="modal-card">
      <div class="modal-head">
        <span>Popup Ad Preview</span>
        <button id="closePreviewPopupBtn" type="button" class="btn btn-red btn-sm">Close</button>
      </div>
      <img id="previewPopupImg" src="" alt="Preview" class="modal-img" />
      <div class="modal-foot">
        <div id="previewPopupCaption" style="font-weight:700;"></div>
        <div id="previewPopupLink" class="truncate" style="color:#fbbf24;font-size:11px;margin-top:4px;"></div>
      </div>
    </div>
  </div>

  <div id="techDetailModal" class="modal-bg">
    <div class="modal-card">
      <div class="modal-head">
        <span id="techModalTitle">Technician Performance Detail</span>
        <button id="closeTechModalBtn" type="button" class="btn btn-red btn-sm">Close</button>
      </div>
      <div id="techModalBody" style="padding:16px;font-size:12px;line-height:1.6;max-height:70vh;overflow-y:auto;"></div>
    </div>
  </div>

  <header class="topbar">
    <div class="brand-group">
      <div class="brand-logo">PP</div>
      <div>
        <h1 class="brand-title">Power Plum Admin Hub</h1>
        <p id="liveBridgeStatus" class="brand-sub">● App Link Ready (LocalStorage + BroadcastChannel + Cloud API)</p>
      </div>
    </div>

    <nav id="adminNav" class="nav-pills">
      <button type="button" class="nav-btn active" data-tab="tab-home">🏠 Home</button>
      <button type="button" class="nav-btn" data-tab="tab-techs">👷 Technicians</button>
      <button type="button" class="nav-btn" data-tab="tab-ads">📢 Ads & Media</button>
      <button type="button" class="nav-btn" data-tab="tab-sync">⚙️ Sync & DB</button>
    </nav>

    <div class="actions-row">
      <button id="refreshAdminDataBtn" type="button" class="btn btn-blue btn-sm">↻ Refresh</button>
      <button id="lockAdminBtn" type="button" class="btn btn-red btn-sm" style="display:none;">🔒 Lock</button>
    </div>
  </header>

  <section id="adminAuthCard" class="auth-wrap">
    <div class="auth-card">
      <span class="auth-badge">POWER PLUM ADMIN LOGIN</span>
      <h2 class="auth-title">Admin Panel Login</h2>
      <p class="auth-desc">
        PIN (<strong>8659</strong>) daal kar login karein. Login ke baad sabse pehle <strong>Home Screen</strong> khulegi jisme live data, graphs, aur technician performance dikhegi.
      </p>
      <form id="adminPinForm" onsubmit="return false;">
        <input type="password" id="adminPinInput" autocomplete="off" placeholder="Enter PIN (8659)" />
        <button id="adminUnlockBtn" type="submit" class="btn btn-full mt-3">Login to Home ➔</button>
      </form>
      <div id="adminPinFeedback" class="feedback"></div>
    </div>
  </section>

  <main id="adminDashboard" class="page-wrap">

    <div id="tab-home" class="admin-tab active-tab">
      <div id="appLinkSummaryBox" class="link-status-banner"></div>

      <div class="section-header">
        <div>
          <h2 class="section-title">Home Overview & Live Performance</h2>
          <p class="section-sub">Sabse pehle home summary, visual graphics, aur har technician ka kaam.</p>
        </div>
        <div class="row">
          <button id="loadSampleTechDataBtn" type="button" class="btn btn-outline btn-sm">+ Demo Tech Stats</button>
          <button id="goTechsTabBtn" type="button" class="btn btn-blue btn-sm">All Technicians ➔</button>
          <button id="goAdsTabBtn" type="button" class="btn btn-violet btn-sm">Manage Ads</button>
        </div>
      </div>

      <div class="kpi-grid">
        <div class="kpi-card kpi-teal">
          <div class="kpi-label">Registered Technicians</div>
          <div id="kpiTechCount" class="kpi-value">0</div>
          <div class="kpi-meta">Active field & shop profiles</div>
        </div>
        <div class="kpi-card kpi-purple">
          <div class="kpi-label">Total App Opens</div>
          <div id="kpiOpenCount" class="kpi-value">0</div>
          <div class="kpi-meta">Across all technician sessions</div>
        </div>
        <div class="kpi-card kpi-rose">
          <div class="kpi-label">Total PDFs Generated</div>
          <div id="kpiPdfCount" class="kpi-value">0</div>
          <div class="kpi-meta">Estimates, BOMs & challans</div>
        </div>
        <div class="kpi-card kpi-amber">
          <div class="kpi-label">Total Ad Clicks</div>
          <div id="kpiAdClickCount" class="kpi-value">0</div>
          <div id="kpiAdBreakdown" class="kpi-meta">Square, Popup & 16:9 Banners</div>
        </div>
      </div>

      <div class="charts-grid">
        <section class="card">
          <h3 class="card-title">Technician Work Comparison (Bar Graph)</h3>
          <p class="card-desc">Har technician ke App Opens, PDFs aur Ad Clicks ka comparison.</p>
          <div id="techBarChartContainer" class="chart-box"></div>
          <div class="legend-list">
            <span><i class="dot" style="background:#4f46e5;"></i>App Opens</span>
            <span><i class="dot" style="background:#059669;"></i>PDFs</span>
            <span><i class="dot" style="background:#ea580c;"></i>Ad Clicks</span>
          </div>
        </section>

        <section class="card">
          <h3 class="card-title">Engagement & Ad Distribution (Donut Graph)</h3>
          <p class="card-desc">Total app opens, generated PDFs, aur ad clicks ka ratio.</p>
          <div id="engagementDonutContainer" class="chart-box"></div>
        </section>
      </div>

      <section class="card">
        <div class="toggle-row">
          <div>
            <h3 class="card-title" style="margin:0;">Technician Performance Cards</h3>
            <p class="card-desc" style="margin:2px 0 0;">Har technician ne kitni baar app khola, kitne PDF banaye, aur kitne ads click kiye.</p>
          </div>
          <input type="text" id="homeTechFilterInput" placeholder="Filter by name, mobile, area..." style="max-width:240px;" />
        </div>
        <div id="homeTechPerfCards" class="perf-grid mt-2"></div>
      </section>
    </div>

    <div id="tab-techs" class="admin-tab">
      <div class="section-header">
        <div>
          <h2 class="section-title">Technicians Directory & Detailed Performance</h2>
          <p class="section-sub">Har technician ki chhoti-se-chhoti detail, activity log, aur CSV export.</p>
        </div>
        <button id="exportExcelCsvBtn" type="button" class="btn btn-green btn-sm">⬇ Download CSV</button>
      </div>

      <div class="grid-2">
        <section class="card">
          <h3 class="card-title">Search Technician Activity & Stats</h3>
          <div class="row">
            <input type="text" id="adminSearchMobile" placeholder="Enter Mobile No or Name..." />
            <button id="searchTechBtn" type="button" class="btn btn-blue" style="width:auto;">Search</button>
          </div>
          <div id="adminSearchResult" class="list-box" style="display:none;"></div>
        </section>

        <section class="card">
          <div class="toggle-row">
            <h3 class="card-title" style="margin:0;">Ad-Wise Click Counter</h3>
            <button id="resetAdClicksBtn" type="button" class="btn btn-red btn-sm">Reset Clicks</button>
          </div>
          <div id="allAdsClickStatsContainer" class="list-box"></div>
        </section>
      </div>

      <section class="card">
        <h3 class="card-title">Complete Technician Performance Table (<span id="totalTechCount">0</span>)</h3>
        <div class="table-wrap mt-2">
          <table>
            <thead>
              <tr>
                <th>#</th>
                <th>Technician</th>
                <th>Mobile</th>
                <th>Trade & Shop</th>
                <th>Area</th>
                <th>App Opens</th>
                <th>PDFs</th>
                <th>Ad Clicks</th>
                <th>Last Active</th>
                <th>Detail</th>
              </tr>
            </thead>
            <tbody id="techRegistryTableBody"></tbody>
          </table>
        </div>
        <p id="emptyTechRegistryMsg" class="card-desc mt-2" style="text-align:center;">Abhi koi technician registered nahi hai.</p>
      </section>
    </div>

    <div id="tab-ads" class="admin-tab">
      <div class="section-header">
        <div>
          <h2 class="section-title">Ads, Banners, YouTube & Notifications</h2>
          <p class="section-sub">Main app ke ads, banners, channel link, aur notification bell control karein.</p>
        </div>
      </div>

      <div class="grid-2">
        <section class="card">
          <div class="toggle-row">
            <h3 class="card-title" style="margin:0;">Right-Side 1:1 Square Ad</h3>
            <label class="toggle-label"><input type="checkbox" id="adminSquareAdToggle" /><span>Show</span></label>
          </div>
          <div class="grid-2">
            <input type="text" id="adminSquareAdTag" placeholder="Badge Tag (e.g. DEAL)" />
            <input type="text" id="adminSquareAdTitle" placeholder="Short Caption" />
          </div>
          <input type="text" id="adminSquareAdImgUrl" class="mt-2" placeholder="1:1 Square Image URL (https://...)" />
          <input type="text" id="adminSquareAdTargetUrl" class="mt-2" placeholder="Target Link (https://...)" />
          <button id="addSquareAdBtn" type="button" class="btn btn-teal btn-full mt-2">+ Add 1:1 Square Ad</button>
          <div id="squareAdListContainer" class="list-box"></div>
        </section>

        <section class="card">
          <div class="toggle-row">
            <h3 class="card-title" style="margin:0;">Full-Screen Popup Ads</h3>
            <label class="toggle-label"><input type="checkbox" id="adminPopupAdToggle" /><span>Enable Popup</span></label>
          </div>
          <input type="text" id="adminPopupAdImgUrl" placeholder="Popup Image URL (https://...)" />
          <input type="text" id="adminPopupAdCaption" class="mt-2" placeholder="Popup Offer Caption" />
          <input type="text" id="adminPopupAdTargetUrl" class="mt-2" placeholder="Target Link (https://...)" />
          <div class="grid-3 mt-2">
            <button id="addPopupAdItemBtn" type="button" class="btn btn-red btn-sm">+ Add</button>
            <button id="savePopupAdToggleOnlyBtn" type="button" class="btn btn-slate btn-sm">Save Toggle</button>
            <button id="previewPopupAdBtn" type="button" class="btn btn-amber btn-sm">👁 Preview</button>
          </div>
          <div id="popupAdListContainer" class="list-box"></div>
        </section>
      </div>

      <section class="card">
        <h3 class="card-title">Main App 16:9 Banner Slider Manager</h3>
        <div class="grid-3">
          <input type="text" id="adminBannerUrlInput" placeholder="Image URL (https://...)" />
          <input type="text" id="adminBannerTextInput" placeholder="Banner Caption" />
          <input type="text" id="adminBannerTargetLink" placeholder="Target Link (https://...)" />
        </div>
        <button id="addBannerBtn" type="button" class="btn btn-amber btn-full mt-2">+ Add 16:9 Banner</button>
        <div id="bannerListContainer" class="list-box"></div>
      </section>

      <div class="grid-2">
        <section class="card">
          <h3 class="card-title">Official YouTube Channel / Video URL</h3>
          <input type="text" id="adminYoutubeUrlInput" placeholder="https://www.youtube.com/@sulemanplumbing" />
          <button id="saveYoutubeUrlBtn" type="button" class="btn btn-red btn-full mt-2">Save YouTube Link</button>
        </section>

        <section class="card">
          <h3 class="card-title">Broadcast Notification Bell</h3>
          <input type="text" id="adminNotifMsgInput" placeholder="Type update message for all technicians..." />
          <button id="sendAdminNotifBtn" type="button" class="btn btn-amber btn-full mt-2">Send Notification</button>
        </section>
      </div>
    </div>

    <div id="tab-sync" class="admin-tab">
      <div class="section-header">
        <div>
          <h2 class="section-title">Cloud Live Sync & Master Trade Database</h2>
          <p class="section-sub">Multi-device sync URL aur master items/brands manage karein.</p>
        </div>
      </div>

      <section class="card">
        <h3 class="card-title">Multi-Device Live Sync (Google Apps Script / Cloud API)</h3>
        <p class="card-desc">Same browser mein localStorage + BroadcastChannel se live sync hota hai. Alag phones/devices ke liye Google Apps Script Web App URL save karein:</p>
        <input type="text" id="adminSyncApiInput" placeholder="https://script.google.com/macros/s/.../exec" />
        <div class="grid-3 mt-2">
          <button id="saveSyncApiBtn" type="button" class="btn btn-violet">Save Sync URL</button>
          <button id="pushFullStateToCloudBtn" type="button" class="btn btn-green">Push All to Cloud</button>
          <button id="pullStateFromCloudBtn" type="button" class="btn btn-blue">Pull From Cloud</button>
        </div>
      </section>

      <section class="card">
        <h3 class="card-title">Add Material / Brand to Master Trade Database</h3>
        <div class="grid-3">
          <select id="adminTradeSelect"></select>
          <input type="text" id="adminCrudItem" placeholder="New Item Name" />
          <input type="text" id="adminCrudBrand" placeholder="New Brand Name" />
        </div>
        <button id="addNewCrudBtn" type="button" class="btn btn-full mt-2">+ Add to Master Database & Notify</button>
      </section>
    </div>

  </main>

  <script>
  (function() {
    var ADMIN_PIN = '8659';
    var syncChannel = null;
    try {
      if ('BroadcastChannel' in window) {
        syncChannel = new BroadcastChannel('power_plum_live_sync_v1');
      }
    } catch (e) {}

    function safeGet(key, fallback) {
      try {
        var raw = localStorage.getItem(key);
        return raw ? JSON.parse(raw) : fallback;
      } catch (e) {
        return fallback;
      }
    }

    function safeSet(key, val) {
      try {
        localStorage.setItem(key, JSON.stringify(val));
      } catch (e) {}
    }

    function escapeHtml(str) {
      return String(str || '').replace(/[&<>"']/g, function(m) {
        return ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' })[m];
      });
    }

    function notifyMainApp(triggerPopup) {
      if (syncChannel) {
        try {
          syncChannel.postMessage({
            type: 'ADMIN_STATE_UPDATED',
            payload: { triggerPopup: !!triggerPopup },
            at: Date.now()
          });
        } catch (e) {}
      }
    }

    function getSyncApiUrl() {
      try {
        return localStorage.getItem('powerPlumSheetApiUrl') || '';
      } catch (e) {
        return '';
      }
    }

    function getAdClicks(adKey) {
      var counts = safeGet('powerPlumAdClickCounts', {});
      return Number(counts[adKey] || 0);
    }

    function setPinMsg(msg, isErr) {
      var el = document.getElementById('adminPinFeedback');
      if (!el) return;
      el.textContent = msg;
      el.className = 'feedback ' + (isErr ? 'err' : 'ok');
    }

    function switchTab(tabId) {
      var tabs = document.querySelectorAll('.admin-tab');
      for (var i = 0; i < tabs.length; i++) {
        if (tabs[i].id === tabId) tabs[i].classList.add('active-tab');
        else tabs[i].classList.remove('active-tab');
      }
      var btns = document.querySelectorAll('.nav-btn');
      for (var j = 0; j < btns.length; j++) {
        if (btns[j].getAttribute('data-tab') === tabId) btns[j].classList.add('active');
        else btns[j].classList.remove('active');
      }
    }

    function computeTechnicianPerformanceList() {
      var registry = safeGet('powerPlumTechRegistry', []);
      var logs = safeGet('powerPlumLogs', []);
      var map = {};

      function ensureEntry(key, seed) {
        if (!map[key]) {
          map[key] = {
            key: key,
            name: (seed && seed.name) ? seed.name : 'Unknown Technician',
            mobile: (seed && seed.mobile) ? String(seed.mobile) : '-',
            trade: (seed && seed.trade) ? seed.trade : 'Plumbing',
            company: (seed && seed.company) ? seed.company : 'Powerplum',
            address: (seed && seed.address) ? seed.address : 'Delhi NCR',
            registeredAt: (seed && seed.timestamp) ? seed.timestamp : '-',
            appOpens: 0,
            pdfsGenerated: 0,
            adClicks: 0,
            lastActive: (seed && seed.timestamp) ? seed.timestamp : '-',
            activities: []
          };
        }
        return map[key];
      }

      for (var i = 0; i < registry.length; i++) {
        var r = registry[i];
        var k = (r.mobile && String(r.mobile).trim()) ? String(r.mobile).trim() : ('reg_' + i);
        ensureEntry(k, r);
      }

      for (var j = 0; j < logs.length; j++) {
        var l = logs[j];
        var mob = (l.mobile && String(l.mobile).trim()) ? String(l.mobile).trim() : '';
        var nm = (l.name && String(l.name).trim()) ? String(l.name).trim() : '';
        var matchedKey = mob;
        if (!matchedKey && nm) {
          for (var prop in map) {
            if ((map[prop].name || '').toLowerCase() === nm.toLowerCase()) {
              matchedKey = prop;
              break;
            }
          }
        }
        if (!matchedKey) matchedKey = nm ? ('name_' + nm.toLowerCase()) : 'guest_user';

        var entry = ensureEntry(matchedKey, { name: nm || mob || 'Technician', mobile: mob || '-', timestamp: l.timestamp || '-' });
        if (nm && entry.name === 'Unknown Technician') entry.name = nm;
        if (mob && entry.mobile === '-') entry.mobile = mob;
        if (l.timestamp) entry.lastActive = l.timestamp;

        if (l.type === 'APP_OPEN') {
          entry.appOpens = Math.max(entry.appOpens + 1, Number(l.openCount || 0));
        } else if (l.type === 'PDF_GENERATE') {
          entry.pdfsGenerated += 1;
        } else if (l.type === 'AD_CLICK' || l.type === 'BANNER_CLICK') {
          entry.adClicks += 1;
        }

        entry.activities.unshift({
          type: l.type || 'ACTIVITY',
          detail: l.detail || l.title || l.adId || l.type || 'Action recorded',
          time: l.timestamp || '-'
        });
      }

      var arr = [];
      for (var keyName in map) {
        var item = map[keyName];
        item.score = (item.appOpens * 1) + (item.pdfsGenerated * 5) + (item.adClicks * 2);
        arr.push(item);
      }
      arr.sort(function(a, b) { return b.score - a.score; });
      return arr;
    }

    function renderLinkStatusBanner(perfList) {
      var box = document.getElementById('appLinkSummaryBox');
      if (!box) return;
      var syncUrl = getSyncApiUrl();
      var hasMainKeys = !!(localStorage.getItem('powerPlumTechRegistry') || localStorage.getItem('powerPlumLogs') || localStorage.getItem('powerPlumMasterDb'));
      var statusText = hasMainKeys
        ? '<strong>Main App Connection Status: LINKED (Local Browser Storage Active)</strong> — Is browser mein Main App ke saath data (Technicians, Logs, Ads, Banners, Master DB) share ho raha hai.'
        : '<strong>Main App Connection Status: READY TO LINK</strong> — Same browser mein Main App open karne par ya "+ Demo Tech Stats" dabane par live technician data yahan turant dikhega.';
      var cloudText = syncUrl
        ? (' | <strong>Cloud Sync URL:</strong> Configured (' + escapeHtml(syncUrl.slice(0, 42)) + '...)')
        : ' | <strong>Cross-Device Cloud Sync:</strong> Optional (Sync & DB tab se Apps Script URL joda ja sakta hai).';
      box.innerHTML = statusText + cloudText + ' | <strong>Technicians Found:</strong> ' + perfList.length;
    }

    function renderKpisAndCharts(perfList) {
      var popups = safeGet('powerPlumPopupAdsList', []);
      var banners = safeGet('powerPlumBanners', []);
      var squares = safeGet('powerPlumSquareAdsList', []);

      var totalOpens = 0, totalPdfs = 0, totalTechAdClicks = 0;
      for (var i = 0; i < perfList.length; i++) {
        totalOpens += perfList[i].appOpens;
        totalPdfs += perfList[i].pdfsGenerated;
        totalTechAdClicks += perfList[i].adClicks;
      }

      var adCounterTotal = 0;
      for (var p = 0; p < popups.length; p++) adCounterTotal += getAdClicks(popups[p].id);
      for (var b = 0; b < banners.length; b++) adCounterTotal += getAdClicks(banners[b].id);
      for (var s = 0; s < squares.length; s++) adCounterTotal += getAdClicks(squares[s].id);

      var displayAdClicks = Math.max(totalTechAdClicks, adCounterTotal);

      document.getElementById('kpiTechCount').textContent = String(perfList.length);
      document.getElementById('kpiOpenCount').textContent = String(totalOpens);
      document.getElementById('kpiPdfCount').textContent = String(totalPdfs);
      document.getElementById('kpiAdClickCount').textContent = String(displayAdClicks);
      document.getElementById('kpiAdBreakdown').textContent = squares.length + ' Square • ' + popups.length + ' Popup • ' + banners.length + ' Banner';

      var barBox = document.getElementById('techBarChartContainer');
      var topTechs = perfList.slice(0, 6);
      if (!topTechs.length) {
        barBox.innerHTML = '<div style="padding:26px;text-align:center;color:#94a3b8;font-size:12px;">Technician stats aane par bar graph yahan dikhega.</div>';
      } else {
        var maxVal = 5;
        for (var m = 0; m < topTechs.length; m++) {
          maxVal = Math.max(maxVal, topTechs[m].appOpens, topTechs[m].pdfsGenerated, topTechs[m].adClicks);
        }
        var barW = 14, gap = 4, groupW = 64, chartH = 145, padTop = 16;
        var svgW = Math.max(340, topTechs.length * groupW + 24);
        var svgH = chartH + 46;
        var barsSvg = '';
        for (var idx = 0; idx < topTechs.length; idx++) {
          var t = topTechs[idx];
          var x0 = 18 + idx * groupW;
          var hOpen = Math.round((t.appOpens / maxVal) * (chartH - 20));
          var hPdf = Math.round((t.pdfsGenerated / maxVal) * (chartH - 20));
          var hAd = Math.round((t.adClicks / maxVal) * (chartH - 20));
          var yOpen = padTop + (chartH - 20 - hOpen);
          var yPdf = padTop + (chartH - 20 - hPdf);
          var yAd = padTop + (chartH - 20 - hAd);

          barsSvg += '<rect x="' + x0 + '" y="' + yOpen + '" width="' + barW + '" height="' + Math.max(2, hOpen) + '" rx="4" fill="#4f46e5"></rect>';
          barsSvg += '<rect x="' + (x0 + barW + gap) + '" y="' + yPdf + '" width="' + barW + '" height="' + Math.max(2, hPdf) + '" rx="4" fill="#059669"></rect>';
          barsSvg += '<rect x="' + (x0 + (barW + gap) * 2) + '" y="' + yAd + '" width="' + barW + '" height="' + Math.max(2, hAd) + '" rx="4" fill="#ea580c"></rect>';
          var shortName = (t.name || 'Tech').split(' ')[0].slice(0, 8);
          barsSvg += '<text x="' + (x0 + 22) + '" y="' + (chartH + 15) + '" text-anchor="middle" font-size="10" font-weight="700" fill="#475569">' + escapeHtml(shortName) + '</text>';
          barsSvg += '<text x="' + (x0 + 22) + '" y="' + (chartH + 28) + '" text-anchor="middle" font-size="9" fill="#94a3b8">O:' + t.appOpens + ' P:' + t.pdfsGenerated + ' A:' + t.adClicks + '</text>';
        }
        barBox.innerHTML = '<svg width="' + svgW + '" height="' + svgH + '" viewBox="0 0 ' + svgW + ' ' + svgH + '" style="display:block;margin:0 auto;"><line x1="10" y1="' + (chartH - 2) + '" x2="' + (svgW - 10) + '" y2="' + (chartH - 2) + '" stroke="#e2e8f0" stroke-width="1.5" />' + barsSvg + '</svg>';
      }

      var donutBox = document.getElementById('engagementDonutContainer');
      var sum = totalOpens + totalPdfs + displayAdClicks;
      if (sum === 0) {
        donutBox.innerHTML = '<div style="padding:26px;text-align:center;color:#94a3b8;font-size:12px;">Abhi koi activity count nahi hai.</div>';
      } else {
        var r = 44, cx = 72, cy = 72, circ = 2 * Math.PI * r;
        var lenOpen = (totalOpens / sum) * circ;
        var lenPdf = (totalPdfs / sum) * circ;
        var lenAd = (displayAdClicks / sum) * circ;
        donutBox.innerHTML =
          '<div style="display:flex;align-items:center;justify-content:center;gap:18px;flex-wrap:wrap;padding:6px 0;">' +
            '<svg width="144" height="144" viewBox="0 0 144 144">' +
              '<circle cx="' + cx + '" cy="' + cy + '" r="' + r + '" fill="none" stroke="#f1f5f9" stroke-width="15" />' +
              '<circle cx="' + cx + '" cy="' + cy + '" r="' + r + '" fill="none" stroke="#4f46e5" stroke-width="15" stroke-dasharray="' + lenOpen + ' ' + circ + '" stroke-dashoffset="0" transform="rotate(-90 ' + cx + ' ' + cy + ')" />' +
              '<circle cx="' + cx + '" cy="' + cy + '" r="' + r + '" fill="none" stroke="#059669" stroke-width="15" stroke-dasharray="' + lenPdf + ' ' + circ + '" stroke-dashoffset="' + (-lenOpen) + '" transform="rotate(-90 ' + cx + ' ' + cy + ')" />' +
              '<circle cx="' + cx + '" cy="' + cy + '" r="' + r + '" fill="none" stroke="#ea580c" stroke-width="15" stroke-dasharray="' + lenAd + ' ' + circ + '" stroke-dashoffset="' + (-(lenOpen + lenPdf)) + '" transform="rotate(-90 ' + cx + ' ' + cy + ')" />' +
              '<text x="' + cx + '" y="' + (cy - 2) + '" text-anchor="middle" font-size="16" font-weight="900" fill="#0f172a">' + sum + '</text>' +
              '<text x="' + cx + '" y="' + (cy + 14) + '" text-anchor="middle" font-size="9" font-weight="700" fill="#64748b">TOTAL</text>' +
            '</svg>' +
            '<div style="font-size:12px;line-height:1.8;">' +
              '<div><i class="dot" style="background:#4f46e5;"></i><strong>App Opens:</strong> ' + totalOpens + ' (' + Math.round((totalOpens / sum) * 100) + '%)</div>' +
              '<div><i class="dot" style="background:#059669;"></i><strong>PDFs Generated:</strong> ' + totalPdfs + ' (' + Math.round((totalPdfs / sum) * 100) + '%)</div>' +
              '<div><i class="dot" style="background:#ea580c;"></i><strong>Ad Clicks:</strong> ' + displayAdClicks + ' (' + Math.round((displayAdClicks / sum) * 100) + '%)</div>' +
            '</div>' +
          '</div>';
      }
    }

    function openTechDetail(techKey) {
      var list = computeTechnicianPerformanceList();
      var t = null;
      for (var i = 0; i < list.length; i++) {
        if (list[i].key === techKey) { t = list[i]; break; }
      }
      if (!t) return;
      document.getElementById('techModalTitle').textContent = t.name + ' — Performance Detail';
      var actHtml = '';
      if (t.activities.length) {
        for (var j = 0; j < Math.min(15, t.activities.length); j++) {
          var a = t.activities[j];
          actHtml += '<div class="list-item"><span><strong>[' + escapeHtml(a.type) + ']</strong> ' + escapeHtml(a.detail) + '</span><span style="color:#64748b;font-size:10px;">' + escapeHtml(a.time) + '</span></div>';
        }
      } else {
        actHtml = '<div style="color:#94a3b8;text-align:center;padding:10px;">Koi activity log nahi hai.</div>';
      }
      document.getElementById('techModalBody').innerHTML =
        '<div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;background:#f8fafc;padding:12px;border-radius:12px;border:1px solid #e2e8f0;">' +
          '<div><strong>Name:</strong> ' + escapeHtml(t.name) + '</div>' +
          '<div><strong>Mobile:</strong> ' + escapeHtml(t.mobile) + '</div>' +
          '<div><strong>Trade:</strong> ' + escapeHtml(t.trade) + '</div>' +
          '<div><strong>Shop / Co:</strong> ' + escapeHtml(t.company) + '</div>' +
          '<div><strong>Area:</strong> ' + escapeHtml(t.address) + '</div>' +
          '<div><strong>Last Active:</strong> ' + escapeHtml(t.lastActive) + '</div>' +
        '</div>' +
        '<div class="metrics-3 mt-3">' +
          '<div class="metric-box"><div class="metric-num" style="color:#4f46e5;">' + t.appOpens + '</div><div class="metric-lbl">App Opens</div></div>' +
          '<div class="metric-box"><div class="metric-num" style="color:#059669;">' + t.pdfsGenerated + '</div><div class="metric-lbl">PDFs Generated</div></div>' +
          '<div class="metric-box"><div class="metric-num" style="color:#ea580c;">' + t.adClicks + '</div><div class="metric-lbl">Ad Clicks</div></div>' +
        '</div>' +
        '<div style="margin-top:12px;font-weight:800;">Activity Breakdown:</div>' +
        '<div class="list-box">' + actHtml + '</div>';
      document.getElementById('techDetailModal').classList.add('open');
    }

    function renderHomeTechCards(perfList) {
      var q = (document.getElementById('homeTechFilterInput').value || '').trim().toLowerCase();
      var box = document.getElementById('homeTechPerfCards');
      var filtered = [];
      for (var i = 0; i < perfList.length; i++) {
        var t = perfList[i];
        if (!q || (t.name || '').toLowerCase().indexOf(q) >= 0 || (t.mobile || '').toLowerCase().indexOf(q) >= 0 || (t.address || '').toLowerCase().indexOf(q) >= 0) {
          filtered.push(t);
        }
      }
      if (!filtered.length) {
        box.innerHTML = '<div style="grid-column:1/-1;text-align:center;color:#94a3b8;padding:18px;font-size:12px;">Koi technician card nahi mila.</div>';
        return;
      }
      var topScore = 10;
      for (var k = 0; k < perfList.length; k++) if (perfList[k].score > topScore) topScore = perfList[k].score;

      var html = '';
      for (var j = 0; j < Math.min(8, filtered.length); j++) {
        var item = filtered[j];
        var pct = Math.min(100, Math.round((item.score / topScore) * 100));
        html +=
          '<div class="tech-card">' +
            '<div>' +
              '<div class="tech-top">' +
                '<div>' +
                  '<h4 class="tech-name">' + escapeHtml(item.name) + '</h4>' +
                  '<div class="tech-sub">📞 ' + escapeHtml(item.mobile) + ' • ' + escapeHtml(item.trade) + '</div>' +
                  '<div class="tech-sub">📍 ' + escapeHtml(item.address) + ' • ' + escapeHtml(item.company) + '</div>' +
                '</div>' +
                '<span class="pill-score">Score ' + item.score + '</span>' +
              '</div>' +
              '<div class="metrics-3 mt-2">' +
                '<div class="metric-box"><div class="metric-num" style="color:#4f46e5;">' + item.appOpens + '</div><div class="metric-lbl">App Opens</div></div>' +
                '<div class="metric-box"><div class="metric-num" style="color:#059669;">' + item.pdfsGenerated + '</div><div class="metric-lbl">PDFs</div></div>' +
                '<div class="metric-box"><div class="metric-num" style="color:#ea580c;">' + item.adClicks + '</div><div class="metric-lbl">Ad Clicks</div></div>' +
              '</div>' +
            '</div>' +
            '<div>' +
              '<div class="prog-wrap"><div class="prog-bar" style="width:' + pct + '%;"></div></div>' +
              '<div style="display:flex;justify-content:space-between;align-items:center;margin-top:8px;font-size:10px;color:#64748b;">' +
                '<span>Last: ' + escapeHtml(item.lastActive) + '</span>' +
                '<button type="button" class="btn btn-outline btn-sm" data-tech-key="' + encodeURIComponent(item.key) + '">Full Detail</button>' +
              '</div>' +
            '</div>' +
          '</div>';
      }
      box.innerHTML = html;

      var detailBtns = box.querySelectorAll('[data-tech-key]');
      for (var d = 0; d < detailBtns.length; d++) {
        detailBtns[d].addEventListener('click', function() {
          openTechDetail(decodeURIComponent(this.getAttribute('data-tech-key')));
        });
      }
    }

    function renderTechTable(perfList) {
      var tbody = document.getElementById('techRegistryTableBody');
      var emptyMsg = document.getElementById('emptyTechRegistryMsg');
      document.getElementById('totalTechCount').textContent = String(perfList.length);
      tbody.innerHTML = '';
      if (!perfList.length) {
        emptyMsg.style.display = 'block';
        return;
      }
      emptyMsg.style.display = 'none';
      for (var i = 0; i < perfList.length; i++) {
        var r = perfList[i];
        var tr = document.createElement('tr');
        tr.innerHTML =
          '<td>' + (i + 1) + '</td>' +
          '<td style="font-weight:800;">' + escapeHtml(r.name) + '</td>' +
          '<td style="font-weight:700;color:#2563eb;">' + escapeHtml(r.mobile) + '</td>' +
          '<td>' + escapeHtml(r.trade) + ' (' + escapeHtml(r.company) + ')</td>' +
          '<td>' + escapeHtml(r.address) + '</td>' +
          '<td style="font-weight:800;color:#4f46e5;">' + r.appOpens + '</td>' +
          '<td style="font-weight:800;color:#059669;">' + r.pdfsGenerated + '</td>' +
          '<td style="font-weight:800;color:#ea580c;">' + r.adClicks + '</td>' +
          '<td style="font-size:10px;color:#64748b;">' + escapeHtml(r.lastActive) + '</td>' +
          '<td><button type="button" class="btn btn-blue btn-sm" data-row-key="' + encodeURIComponent(r.key) + '">View</button></td>';
        tbody.appendChild(tr);
      }
      var rowBtns = tbody.querySelectorAll('[data-row-key]');
      for (var j = 0; j < rowBtns.length; j++) {
        rowBtns[j].addEventListener('click', function() {
          openTechDetail(decodeURIComponent(this.getAttribute('data-row-key')));
        });
      }
    }

    function populateTradeSelect() {
      var db = safeGet('powerPlumMasterDb', {});
      var sel = document.getElementById('adminTradeSelect');
      if (!sel) return;
      var trades = Object.keys(db);
      if (!trades.length) trades = ['Plumbing', 'RO Service', 'Fire Safety'];
      sel.innerHTML = '';
      for (var i = 0; i < trades.length; i++) {
        var opt = document.createElement('option');
        opt.value = trades[i];
        opt.textContent = trades[i];
        sel.appendChild(opt);
      }
    }

    function renderAdsLists() {
      var popups = safeGet('powerPlumPopupAdsList', []);
      var banners = safeGet('powerPlumBanners', []);
      var squares = safeGet('powerPlumSquareAdsList', []);

      var clickBox = document.getElementById('allAdsClickStatsContainer');
      var rows = [];
      for (var p = 0; p < popups.length; p++) rows.push({ label: '[Popup #' + (p + 1) + '] ' + (popups[p].caption || 'Popup Ad'), clicks: getAdClicks(popups[p].id) });
      for (var b = 0; b < banners.length; b++) rows.push({ label: '[Banner #' + (b + 1) + '] ' + (banners[b].text || 'Banner'), clicks: getAdClicks(banners[b].id) });
      for (var s = 0; s < squares.length; s++) rows.push({ label: '[Square #' + (s + 1) + '] ' + (squares[s].title || 'Square Ad'), clicks: getAdClicks(squares[s].id) });

      clickBox.innerHTML = rows.length ? rows.map(function(x) {
        return '<div class="list-item"><span class="truncate">' + escapeHtml(x.label) + '</span><strong>' + x.clicks + ' Clicks</strong></div>';
      }).join('') : '<div style="color:#94a3b8;text-align:center;">No ads configured yet.</div>';

      var sqBox = document.getElementById('squareAdListContainer');
      sqBox.innerHTML = squares.length ? squares.map(function(ad, idx) {
        return '<div class="list-item"><div class="truncate"><strong>[' + escapeHtml(ad.tag) + ']</strong> ' + escapeHtml(ad.title) + ' (' + getAdClicks(ad.id) + ' clicks)</div><button type="button" data-del-sq="' + idx + '" class="btn btn-red btn-sm">Del</button></div>';
      }).join('') : '<div style="color:#94a3b8;text-align:center;">Koi 1:1 square ad nahi hai.</div>';

      var popBox = document.getElementById('popupAdListContainer');
      popBox.innerHTML = popups.length ? popups.map(function(ad, idx) {
        return '<div class="list-item"><div class="truncate"><strong>' + (idx + 1) + '. ' + escapeHtml(ad.caption) + '</strong> (' + getAdClicks(ad.id) + ' clicks)</div><button type="button" data-del-pop="' + idx + '" class="btn btn-red btn-sm">Del</button></div>';
      }).join('') : '<div style="color:#94a3b8;text-align:center;">Koi popup ad nahi hai.</div>';

      var banBox = document.getElementById('bannerListContainer');
      banBox.innerHTML = banners.length ? banners.map(function(b, idx) {
        return '<div class="list-item"><div class="truncate"><strong>' + escapeHtml(b.text) + '</strong> (' + getAdClicks(b.id) + ' clicks)</div><button type="button" data-del-ban="' + idx + '" class="btn btn-red btn-sm">Del</button></div>';
      }).join('') : '<div style="color:#94a3b8;text-align:center;">Koi banner nahi hai.</div>';

      sqBox.querySelectorAll('[data-del-sq]').forEach(function(btn) {
        btn.addEventListener('click', function() {
          var arr = safeGet('powerPlumSquareAdsList', []);
          arr.splice(Number(this.getAttribute('data-del-sq')), 1);
          safeSet('powerPlumSquareAdsList', arr);
          renderAll();
          notifyMainApp(false);
        });
      });
      popBox.querySelectorAll('[data-del-pop]').forEach(function(btn) {
        btn.addEventListener('click', function() {
          var arr = safeGet('powerPlumPopupAdsList', []);
          arr.splice(Number(this.getAttribute('data-del-pop')), 1);
          safeSet('powerPlumPopupAdsList', arr);
          renderAll();
          notifyMainApp(false);
        });
      });
      banBox.querySelectorAll('[data-del-ban]').forEach(function(btn) {
        btn.addEventListener('click', function() {
          var arr = safeGet('powerPlumBanners', []);
          arr.splice(Number(this.getAttribute('data-del-ban')), 1);
          safeSet('powerPlumBanners', arr);
          renderAll();
          notifyMainApp(false);
        });
      });
    }

    function renderAll() {
      var perfList = computeTechnicianPerformanceList();
      renderLinkStatusBanner(perfList);
      renderKpisAndCharts(perfList);
      renderHomeTechCards(perfList);
      renderTechTable(perfList);
      populateTradeSelect();
      renderAdsLists();
      document.getElementById('adminSyncApiInput').value = getSyncApiUrl();
      document.getElementById('adminYoutubeUrlInput').value = localStorage.getItem('powerPlumYoutubeUrl') || 'https://www.youtube.com/@sulemanplumbing';
      var popSet = safeGet('powerPlumPopupAdSettings', { enabled: false });
      var sqSet = safeGet('powerPlumSquareAdSettings', { enabled: true });
      document.getElementById('adminPopupAdToggle').checked = !!(popSet && popSet.enabled);
      document.getElementById('adminSquareAdToggle').checked = !!(sqSet && sqSet.enabled);
    }

    function performLogin() {
      var inputEl = document.getElementById('adminPinInput');
      var val = (inputEl && inputEl.value ? inputEl.value : '').trim();
      if (val === ADMIN_PIN) {
        setPinMsg('Login successful! Opening Home...', false);
        document.getElementById('adminAuthCard').style.display = 'none';
        document.getElementById('adminDashboard').style.display = 'block';
        document.getElementById('adminNav').style.display = 'flex';
        document.getElementById('lockAdminBtn').style.display = 'inline-block';
        switchTab('tab-home');
        renderAll();
      } else {
        setPinMsg('Galat PIN! Sahi PIN 8659 daalein.', true);
      }
    }

    document.getElementById('adminPinForm').addEventListener('submit', function(e) {
      e.preventDefault();
      performLogin();
    });
    document.getElementById('adminUnlockBtn').addEventListener('click', function(e) {
      e.preventDefault();
      performLogin();
    });

    document.getElementById('lockAdminBtn').addEventListener('click', function() {
      document.getElementById('adminPinInput').value = '';
      setPinMsg('', false);
      document.getElementById('adminDashboard').style.display = 'none';
      document.getElementById('adminNav').style.display = 'none';
      document.getElementById('lockAdminBtn').style.display = 'none';
      document.getElementById('adminAuthCard').style.display = 'flex';
    });

    var navButtons = document.querySelectorAll('.nav-btn');
    for (var n = 0; n < navButtons.length; n++) {
      navButtons[n].addEventListener('click', function() {
        switchTab(this.getAttribute('data-tab'));
      });
    }

    document.getElementById('goTechsTabBtn').addEventListener('click', function() { switchTab('tab-techs'); });
    document.getElementById('goAdsTabBtn').addEventListener('click', function() { switchTab('tab-ads'); });
    document.getElementById('refreshAdminDataBtn').addEventListener('click', renderAll);
    document.getElementById('homeTechFilterInput').addEventListener('input', function() { renderHomeTechCards(computeTechnicianPerformanceList()); });

    document.getElementById('closeTechModalBtn').addEventListener('click', function() {
      document.getElementById('techDetailModal').classList.remove('open');
    });
    document.getElementById('closePreviewPopupBtn').addEventListener('click', function() {
      document.getElementById('previewPopupModal').classList.remove('open');
    });

    document.getElementById('loadSampleTechDataBtn').addEventListener('click', function() {
      var sampleReg = [
        { name: 'Ramesh Verma', mobile: '9811100011', trade: 'Plumbing', company: 'Powerplum', address: 'Narela, Delhi', timestamp: '22/11/2026, 10:15 AM' },
        { name: 'Suraj Kumar', mobile: '9811100022', trade: 'Plumbing & Pump', company: 'Suleman Plumbing', address: 'Bawana, Delhi', timestamp: '22/11/2026, 11:30 AM' },
        { name: 'Deepak Sharma', mobile: '9811100033', trade: 'RO & Fittings', company: 'Powerplum', address: 'Kundli', timestamp: '22/11/2026, 01:05 PM' }
      ];
      var sampleLogs = [
        { type: 'APP_OPEN', name: 'Ramesh Verma', mobile: '9811100011', detail: 'Opened App Home', timestamp: '22/11/2026, 10:15 AM' },
        { type: 'APP_OPEN', name: 'Ramesh Verma', mobile: '9811100011', detail: 'Opened App Home', timestamp: '22/11/2026, 12:10 PM' },
        { type: 'PDF_GENERATE', name: 'Ramesh Verma', mobile: '9811100011', detail: 'Generated BOM Estimate PDF', timestamp: '22/11/2026, 12:25 PM' },
        { type: 'PDF_GENERATE', name: 'Ramesh Verma', mobile: '9811100011', detail: 'Generated Challan PDF', timestamp: '22/11/2026, 02:10 PM' },
        { type: 'AD_CLICK', name: 'Ramesh Verma', mobile: '9811100011', detail: 'Clicked 1:1 Square Deal Ad', timestamp: '22/11/2026, 02:15 PM' },
        { type: 'APP_OPEN', name: 'Suraj Kumar', mobile: '9811100022', detail: 'Opened App Home', timestamp: '22/11/2026, 11:30 AM' },
        { type: 'PDF_GENERATE', name: 'Suraj Kumar', mobile: '9811100022', detail: 'Generated Pump Estimate PDF', timestamp: '22/11/2026, 01:40 PM' },
        { type: 'AD_CLICK', name: 'Suraj Kumar', mobile: '9811100022', detail: 'Clicked 16:9 Banner Ad', timestamp: '22/11/2026, 01:45 PM' },
        { type: 'AD_CLICK', name: 'Suraj Kumar', mobile: '9811100022', detail: 'Clicked Popup Offer Ad', timestamp: '22/11/2026, 01:48 PM' },
        { type: 'APP_OPEN', name: 'Deepak Sharma', mobile: '9811100033', detail: 'Opened App Home', timestamp: '22/11/2026, 01:05 PM' }
      ];
      safeSet('powerPlumTechRegistry', sampleReg);
      safeSet('powerPlumLogs', sampleLogs);
      renderAll();
      notifyMainApp(false);
    });

    document.getElementById('exportExcelCsvBtn').addEventListener('click', function() {
      var perf = computeTechnicianPerformanceList();
      var csv = 'Technician Name,Mobile Number,Trade,Shop / Company,Area,App Opens,PDFs Generated,Ad Clicks,Performance Score,Last Active\n';
      for (var i = 0; i < perf.length; i++) {
        var r = perf[i];
        csv += '"' + r.name + '","' + r.mobile + '","' + r.trade + '","' + r.company + '","' + r.address + '",' + r.appOpens + ',' + r.pdfsGenerated + ',' + r.adClicks + ',' + r.score + ',"' + r.lastActive + '"\n';
      }
      var blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      var a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'PowerPlum_Technician_Performance.csv';
      a.click();
    });

    document.getElementById('searchTechBtn').addEventListener('click', function() {
      var q = (document.getElementById('adminSearchMobile').value || '').trim().toLowerCase();
      var box = document.getElementById('adminSearchResult');
      if (!q) { alert('Mobile ya Name daalein'); return; }
      box.style.display = 'block';
      var perf = computeTechnicianPerformanceList();
      var found = null;
      for (var i = 0; i < perf.length; i++) {
        if ((perf[i].mobile || '').toLowerCase().indexOf(q) >= 0 || (perf[i].name || '').toLowerCase().indexOf(q) >= 0) {
          found = perf[i];
          break;
        }
      }
      if (!found) {
        box.innerHTML = '<div style="color:#dc2626;font-weight:700;">Record nahi mila.</div>';
        return;
      }
      box.innerHTML =
        '<div><strong>Name:</strong> ' + escapeHtml(found.name) + '</div>' +
        '<div><strong>Mobile:</strong> ' + escapeHtml(found.mobile) + '</div>' +
        '<div><strong>Trade & Shop:</strong> ' + escapeHtml(found.trade) + ' (' + escapeHtml(found.company) + ')</div>' +
        '<div><strong>App Opens:</strong> ' + found.appOpens + ' | <strong>PDFs:</strong> ' + found.pdfsGenerated + ' | <strong>Ad Clicks:</strong> ' + found.adClicks + '</div>';
    });

    document.getElementById('resetAdClicksBtn').addEventListener('click', function() {
      safeSet('powerPlumAdClickCounts', {});
      renderAll();
      notifyMainApp(false);
    });

    document.getElementById('adminSquareAdToggle').addEventListener('change', function() {
      safeSet('powerPlumSquareAdSettings', { enabled: this.checked });
      notifyMainApp(false);
    });

    document.getElementById('addSquareAdBtn').addEventListener('click', function() {
      var tag = (document.getElementById('adminSquareAdTag').value || '').trim() || 'DEAL';
      var title = (document.getElementById('adminSquareAdTitle').value || '').trim() || 'Special Offer';
      var img = (document.getElementById('adminSquareAdImgUrl').value || '').trim() || 'https://images.unsplash.com/photo-1581092160607-ee22621dd758?w=400&auto=format&fit=crop&q=80';
      var link = (document.getElementById('adminSquareAdTargetUrl').value || '').trim();
      if (!link) { alert('Target Link daalein!'); return; }
      var list = safeGet('powerPlumSquareAdsList', []);
      list.push({ id: 'SQ_AD_' + Date.now(), tag: tag, title: title, img: img, link: link });
      safeSet('powerPlumSquareAdsList', list);
      document.getElementById('adminSquareAdTag').value = '';
      document.getElementById('adminSquareAdTitle').value = '';
      document.getElementById('adminSquareAdImgUrl').value = '';
      document.getElementById('adminSquareAdTargetUrl').value = '';
      renderAll();
      notifyMainApp(false);
      alert('1:1 Square Ad add ho gaya!');
    });

    document.getElementById('savePopupAdToggleOnlyBtn').addEventListener('click', function() {
      var enabled = document.getElementById('adminPopupAdToggle').checked;
      safeSet('powerPlumPopupAdSettings', { enabled: enabled });
      notifyMainApp(enabled);
      alert(enabled ? 'Popup Ad ON!' : 'Popup Ad OFF!');
    });

    document.getElementById('addPopupAdItemBtn').addEventListener('click', function() {
      var img = (document.getElementById('adminPopupAdImgUrl').value || '').trim();
      var caption = (document.getElementById('adminPopupAdCaption').value || '').trim() || 'Power Plum Offer';
      var link = (document.getElementById('adminPopupAdTargetUrl').value || '').trim() || '#';
      if (!img) { alert('Popup Image URL daalein!'); return; }
      var enabled = document.getElementById('adminPopupAdToggle').checked;
      safeSet('powerPlumPopupAdSettings', { enabled: enabled });
      var list = safeGet('powerPlumPopupAdsList', []);
      list.push({ id: 'POPUP_' + Date.now(), img: img, caption: caption, link: link });
      safeSet('powerPlumPopupAdsList', list);
      document.getElementById('adminPopupAdImgUrl').value = '';
      document.getElementById('adminPopupAdCaption').value = '';
      document.getElementById('adminPopupAdTargetUrl').value = '';
      renderAll();
      notifyMainApp(enabled);
      alert('Popup Ad add ho gaya!');
    });

    document.getElementById('previewPopupAdBtn').addEventListener('click', function() {
      var list = safeGet('powerPlumPopupAdsList', []);
      if (!list.length) { alert('Pehle koi Popup Ad add karein'); return; }
      document.getElementById('previewPopupImg').src = list[0].img;
      document.getElementById('previewPopupCaption').textContent = list[0].caption;
      document.getElementById('previewPopupLink').textContent = list[0].link;
      document.getElementById('previewPopupModal').classList.add('open');
    });

    document.getElementById('addBannerBtn').addEventListener('click', function() {
      var img = (document.getElementById('adminBannerUrlInput').value || '').trim();
      var text = (document.getElementById('adminBannerTextInput').value || '').trim() || 'Power Plum Service';
      var link = (document.getElementById('adminBannerTargetLink').value || '').trim() || '#';
      if (!img) { alert('Banner Image URL daalein'); return; }
      var list = safeGet('powerPlumBanners', []);
      list.push({ id: 'BANNER_' + Date.now(), img: img, text: text, link: link });
      safeSet('powerPlumBanners', list);
      document.getElementById('adminBannerUrlInput').value = '';
      document.getElementById('adminBannerTextInput').value = '';
      document.getElementById('adminBannerTargetLink').value = '';
      renderAll();
      notifyMainApp(false);
      alert('16:9 Banner add ho gaya!');
    });

    document.getElementById('saveYoutubeUrlBtn').addEventListener('click', function() {
      var url = (document.getElementById('adminYoutubeUrlInput').value || '').trim();
      if (!url) { alert('YouTube URL daalein'); return; }
      localStorage.setItem('powerPlumYoutubeUrl', url);
      notifyMainApp(false);
      alert('YouTube Link update ho gaya!');
    });

    document.getElementById('sendAdminNotifBtn').addEventListener('click', function() {
      var msg = (document.getElementById('adminNotifMsgInput').value || '').trim();
      if (!msg) { alert('Notification message likhein'); return; }
      localStorage.setItem('powerPlumActiveNotif', msg);
      document.getElementById('adminNotifMsgInput').value = '';
      notifyMainApp(false);
      alert('Notification bhej diya gaya!');
    });

    document.getElementById('saveSyncApiBtn').addEventListener('click', function() {
      var url = (document.getElementById('adminSyncApiInput').value || '').trim();
      localStorage.setItem('powerPlumSheetApiUrl', url);
      renderAll();
      alert('Sync URL save ho gaya!');
    });

    document.getElementById('pushFullStateToCloudBtn').addEventListener('click', async function() {
      var url = getSyncApiUrl();
      if (!url) { alert('Pehle Sync URL daalein.'); return; }
      try {
        await fetch(url, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'text/plain;charset=utf-8' },
          body: JSON.stringify({ action: 'SYNC_ADMIN_STATE', timestamp: new Date().toLocaleString('en-IN') })
        });
        alert('Cloud par push request bhej di gayi hai!');
      } catch (e) {
        alert('Push fail hua.');
      }
    });

    document.getElementById('pullStateFromCloudBtn').addEventListener('click', async function() {
      var url = getSyncApiUrl();
      if (!url) { alert('Pehle Sync URL daalein.'); return; }
      try {
        var res = await fetch(url + '?action=get_admin_state&_t=' + Date.now());
        var remote = await res.json();
        if (remote.techRegistry) safeSet('powerPlumTechRegistry', remote.techRegistry);
        if (remote.logs) safeSet('powerPlumLogs', remote.logs);
        if (remote.adClickCounts) safeSet('powerPlumAdClickCounts', remote.adClickCounts);
        renderAll();
        alert('Cloud se data sync ho gaya!');
      } catch (e) {
        alert('Cloud pull fail hua.');
      }
    });

    document.getElementById('addNewCrudBtn').addEventListener('click', function() {
      var trade = document.getElementById('adminTradeSelect').value;
      var item = (document.getElementById('adminCrudItem').value || '').trim();
      var brand = (document.getElementById('adminCrudBrand').value || '').trim();
      if (!item && !brand) { alert('Item ya Brand enter karein'); return; }
      var db = safeGet('powerPlumMasterDb', {});
      if (!db[trade]) db[trade] = { items: [], brands: [], sizes1: [], sizes2: [] };
      if (item && db[trade].items.indexOf(item) < 0) db[trade].items.unshift(item);
      if (brand && db[trade].brands.indexOf(brand) < 0) db[trade].brands.unshift(brand);
      safeSet('powerPlumMasterDb', db);
      localStorage.setItem('powerPlumActiveNotif', 'New item added in ' + trade + ': ' + (item || brand));
      document.getElementById('adminCrudItem').value = '';
      document.getElementById('adminCrudBrand').value = '';
      renderAll();
      notifyMainApp(false);
      alert('Master DB update ho gaya!');
    });

    window.addEventListener('storage', function() {
      if (document.getElementById('adminDashboard').style.display === 'block') renderAll();
    });

    if (syncChannel) {
      syncChannel.onmessage = function() {
        if (document.getElementById('adminDashboard').style.display === 'block') renderAll();
      };
    }
  })();
  </script>
</body>
</html>
