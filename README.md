<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Form for Record Data Inventory — PWB in PCBA</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap');

  :root{
    --page:#0a1220;
    --surface:#121b2d;
    --surface-2:#1a2538;
    --ink:#e7ecf3;
    --muted:#8c9bb3;
    --primary:#6fa8ff;
    --primary-strong:#2f5fd6;
    --primary-soft:#1e2f4d;
    --accent-2:#8b6cff;
    --border:#283447;
    --danger:#ff6b6b;
    --danger-strong:#c0392b;
    --danger-soft:#3a2228;
  }

  *{ box-sizing:border-box; }
  html,body{ margin:0; }
  html{ overflow-x:hidden; }

  body{
    position:relative;
    min-height:100vh;
    overflow-x:hidden;
    background:var(--page);
    font-family:'Inter', sans-serif;
    color:var(--ink);
    line-height:1.5;
    padding:24px 16px 48px;
    display:flex;
    justify-content:center;
  }

  /* ---------- ambient animated background ---------- */
  body::before, body::after{
    content:'';
    position:fixed;
    border-radius:50%;
    filter:blur(90px);
    opacity:.32;
    pointer-events:none;
    z-index:0;
    will-change:transform;
  }
  body::before{
    width:48vmax; height:48vmax;
    top:-20vmax; left:-16vmax;
    background:var(--primary);
    animation:driftA 28s ease-in-out infinite alternate;
  }
  body::after{
    width:42vmax; height:42vmax;
    bottom:-18vmax; right:-14vmax;
    background:var(--accent-2);
    animation:driftB 32s ease-in-out infinite alternate;
  }
  @keyframes driftA{
    from{ transform:translate(0,0) scale(1); }
    to{ transform:translate(7vmax,5vmax) scale(1.12); }
  }
  @keyframes driftB{
    from{ transform:translate(0,0) scale(1); }
    to{ transform:translate(-6vmax,-5vmax) scale(1.08); }
  }
  @media (prefers-reduced-motion: reduce){
    body::before, body::after{ animation:none; }
  }

  .layout{
    position:relative;
    z-index:1;
    display:flex;
    flex-wrap:wrap;
    align-items:flex-start;
    justify-content:center;
    gap:20px;
    width:100%;
    max-width:1320px;
  }

  /* ---------- count sheet card ---------- */
  .sheet{
    width:100%;
    max-width:600px;
    flex:1 1 400px;
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:8px;
    box-shadow:0 1px 2px rgba(0,0,0,.2), 0 20px 40px -24px rgba(0,0,0,.6);
    overflow:hidden;
  }

  .sheet-header{
    background:var(--primary-strong);
    color:#fff;
    padding:14px 20px;
  }
  .sheet-header h1{
    margin:0;
    font-size:14px;
    font-weight:700;
    letter-spacing:.03em;
    line-height:1.3;
    text-transform:uppercase;
  }
  .sheet-header p{
    margin:3px 0 0;
    font-size:11px;
    color:rgba(255,255,255,.78);
  }

  .doc-info{
    display:grid;
    grid-template-columns:100px 1fr 1fr 1fr;
    border-bottom:1px solid var(--border);
  }
  .doc-cell{
    padding:9px 14px;
    border-right:1px solid var(--border);
  }
  .doc-cell:last-child{ border-right:none; }
  .doc-cell.tagno{ background:var(--primary-soft); }
  .doc-cell label{
    display:block;
    font-family:'IBM Plex Mono', monospace;
    font-size:9px;
    font-weight:600;
    letter-spacing:.16em;
    text-transform:uppercase;
    color:var(--muted);
    margin-bottom:4px;
  }
  .doc-cell input, .doc-cell select{
    width:100%;
    border:none;
    background:transparent;
    font-family:'IBM Plex Mono', monospace;
    font-size:14px;
    font-weight:600;
    color:var(--ink);
    padding:0;
  }
  .doc-cell.tagno input{ color:var(--primary); }
  .doc-cell input:focus, .doc-cell select:focus{ outline:none; }
  .doc-cell input:focus-visible, .doc-cell select:focus-visible{ outline:2px solid var(--primary); outline-offset:2px; border-radius:2px; }
  .doc-cell select{ color-scheme:dark; }

  .sheet-body{ padding:16px 20px; }

  .section{ margin-bottom:14px; }
  .section-title{
    display:flex;
    align-items:center;
    gap:8px;
    font-size:10px;
    font-weight:700;
    letter-spacing:.18em;
    text-transform:uppercase;
    color:var(--primary);
    margin:0 0 10px;
  }
  .section-title::before{
    content:'';
    width:14px;
    height:2px;
    background:var(--primary);
    display:block;
    flex-shrink:0;
  }

  .row{ display:grid; gap:10px; }
  .row + .row{ margin-top:10px; }
  .row-2{ grid-template-columns:1fr 1fr; }

  .field label{
    display:block;
    font-size:10px;
    font-weight:600;
    letter-spacing:.06em;
    text-transform:uppercase;
    color:var(--muted);
    margin-bottom:4px;
  }

  .field input, .field select{
    width:100%;
    font-family:'Inter', sans-serif;
    font-size:13px;
    color:var(--ink);
    background:var(--surface-2);
    border:1px solid var(--border);
    border-radius:6px;
    padding:7px 10px;
    color-scheme:dark;
  }

  .field select{
    appearance:none;
    background-image:
      linear-gradient(45deg, transparent 50%, var(--muted) 50%),
      linear-gradient(135deg, var(--muted) 50%, transparent 50%);
    background-position: calc(100% - 18px) calc(50% + 2px), calc(100% - 12px) calc(50% + 2px);
    background-size:6px 6px, 6px 6px;
    background-repeat:no-repeat;
    padding-right:34px;
  }

  .field input::placeholder{ color:var(--muted); opacity:.65; }

  .field input:focus, .field select:focus{
    outline:none;
    border-color:var(--primary);
    background:var(--surface);
    box-shadow:0 0 0 3px rgba(111,168,255,.22);
  }

  .field-hint{
    display:none;
    margin-top:5px;
    font-size:11px;
    font-weight:600;
    letter-spacing:.04em;
    color:var(--primary);
  }

  .edit-notice{
    margin:0 0 12px;
    padding:8px 12px;
    border-radius:6px;
    background:var(--primary-soft);
    font-size:12px;
    color:var(--primary);
  }

  .actions{
    display:flex;
    justify-content:flex-end;
    gap:8px;
    flex-wrap:wrap;
    padding-top:14px;
    border-top:1px solid var(--border);
  }

  .btn{
    display:inline-flex;
    align-items:center;
    gap:6px;
    font-family:'Inter', sans-serif;
    font-size:12.5px;
    font-weight:600;
    padding:8px 14px;
    border-radius:6px;
    border:1px solid transparent;
    cursor:pointer;
    transition:background-color .12s ease, border-color .12s ease, opacity .12s ease;
  }
  .btn:active{ transform:translateY(1px); }
  .btn:disabled{ opacity:.4; cursor:not-allowed; }
  .btn:focus-visible{ outline:2px solid var(--primary); outline-offset:2px; }

  .btn-primary{ background:var(--primary-strong); color:#fff; }
  .btn-primary:hover:not(:disabled){ background:#3d6ee8; }

  .btn-ghost{ background:var(--surface-2); border-color:var(--border); color:var(--ink); }
  .btn-ghost:hover:not(:disabled){ background:#22304a; }

  .btn-danger{ background:transparent; border-color:var(--danger); color:var(--danger); }
  .btn-danger:hover:not(:disabled){ background:var(--danger-soft); }

  .btn-danger-solid{ background:var(--danger-strong); color:#fff; }
  .btn-danger-solid:hover:not(:disabled){ background:#a8311f; }

  .btn-sm{ padding:6px 12px; font-size:11.5px; }

  /* ---------- session log ---------- */
  .log{
    width:100%;
    max-width:880px;
    flex:1 1 540px;
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:8px;
    box-shadow:0 1px 2px rgba(0,0,0,.2), 0 20px 40px -24px rgba(0,0,0,.6);
    overflow:hidden;
  }
  .log-header{
    display:flex;
    justify-content:space-between;
    align-items:center;
    flex-wrap:wrap;
    gap:8px;
    padding:12px 16px;
    border-bottom:1px solid var(--border);
    background:var(--surface-2);
  }
  .log-header h2{
    margin:0;
    font-size:10px;
    font-weight:700;
    letter-spacing:.2em;
    text-transform:uppercase;
    color:var(--muted);
  }
  .log-actions{ display:flex; gap:8px; flex-wrap:wrap; }

  .table-wrap{
    overflow:auto;
    max-height:680px;
  }
  .table-wrap::-webkit-scrollbar{ width:8px; height:8px; }
  .table-wrap::-webkit-scrollbar-track{ background:var(--surface-2); }
  .table-wrap::-webkit-scrollbar-thumb{ background:var(--border); border-radius:4px; }
  .table-wrap::-webkit-scrollbar-thumb:hover{ background:var(--muted); }

  table{
    width:100%;
    border-collapse:collapse;
    font-size:12px;
    min-width:780px;
  }
  th, td{
    text-align:left;
    padding:8px 12px;
    border-bottom:1px solid var(--border);
    white-space:nowrap;
    vertical-align:middle;
  }
  th{
    font-size:10px;
    font-weight:700;
    letter-spacing:.14em;
    text-transform:uppercase;
    color:var(--primary);
    background:var(--primary-soft);
    position:sticky;
    top:0;
  }
  tbody tr:nth-child(even) td{ background:var(--surface-2); }
  .editing-row td{ background:var(--primary-soft) !important; }

  .row-actions{ width:60px; white-space:nowrap; }
  .select-col{ width:36px; text-align:center; }
  .qr-col{ width:68px; text-align:center; }
  .qr-thumb{
    width:56px; height:56px;
    display:inline-flex;
    align-items:center;
    justify-content:center;
    background:#fff;
    border-radius:5px;
    padding:3px;
    cursor:pointer;
    line-height:0;
  }
  .qr-thumb svg{ display:block; width:100%; height:100%; }
  .qr-thumb:hover{ box-shadow:0 0 0 2px var(--primary); }
  .qr-thumb:focus-visible{ outline:2px solid var(--primary); outline-offset:2px; }
  input[type="checkbox"]{
    width:15px; height:15px;
    accent-color:var(--primary);
    cursor:pointer;
    vertical-align:middle;
  }
  .icon-btn{
    border:none; background:transparent; cursor:pointer; line-height:1;
    padding:4px; border-radius:4px; color:var(--muted);
    display:inline-flex; align-items:center; justify-content:center;
  }
  .icon-btn.edit:hover{ background:var(--primary-soft); color:var(--primary); }
  .icon-btn.del:hover{ background:var(--danger-soft); color:var(--danger); }
  .icon-btn:focus-visible{ outline:2px solid var(--primary); outline-offset:1px; }

  .empty{
    padding:28px 20px;
    text-align:center;
    font-size:12px;
    color:var(--muted);
    margin:0;
  }

  /* ---------- printable log report ---------- */
  #printArea{ display:none; }
  #printArea h1{
    font-family:'Inter', sans-serif;
    font-size:15px;
    font-weight:700;
    margin:0 0 4px;
    color:var(--ink);
  }
  #printArea .print-meta{
    font-size:11px;
    color:var(--muted);
    margin:0 0 16px;
  }
  #printTable{
    width:100%;
    border-collapse:collapse;
    font-size:11px;
  }
  #printTable th, #printTable td{
    border:1px solid #b9c2cc;
    padding:6px 8px;
    text-align:left;
    vertical-align:middle;
  }
  #printTable th{
    background:var(--primary-soft);
    color:var(--primary);
    text-transform:uppercase;
    letter-spacing:.08em;
    font-size:9px;
  }
  #printTable .qr-col{ width:96px; text-align:center; }
  .qr-print{
    width:80px; height:80px;
    background:#fff;
    margin:0 auto;
    line-height:0;
  }
  .qr-print svg{ display:block; width:100%; height:100%; }

  /* ---------- modal ---------- */
  .modal-overlay{
    position:fixed;
    inset:0;
    background:rgba(3,7,14,.65);
    display:none;
    align-items:center;
    justify-content:center;
    padding:16px;
    z-index:50;
  }
  .modal-overlay.open{ display:flex; }

  .modal{
    width:100%;
    max-width:380px;
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:8px;
    padding:22px;
    box-shadow:0 24px 48px -16px rgba(0,0,0,.6);
  }
  .modal-icon{
    width:42px; height:42px;
    border-radius:50%;
    background:var(--danger-soft);
    color:var(--danger);
    display:flex; align-items:center; justify-content:center;
    margin-bottom:12px;
  }
  .modal h3{ margin:0 0 6px; font-size:15px; font-weight:700; color:var(--ink); }
  .modal p{ margin:0; font-size:13px; color:var(--muted); }
  .modal .field{ margin:14px 0 0; }
  .modal-error{
    display:none;
    margin-top:8px;
    font-size:12px;
    font-weight:600;
    color:var(--danger);
  }
  .modal-actions{
    display:flex;
    justify-content:flex-end;
    gap:10px;
    margin-top:16px;
  }

  .qr-modal{ max-width:320px; text-align:center; }
  .qr-modal h3{ margin-bottom:12px; }
  .qr-modal-code{
    width:220px; height:220px;
    margin:0 auto 14px;
    background:#fff;
    border-radius:8px;
    padding:14px;
    line-height:0;
  }
  .qr-modal-code svg{ display:block; width:100%; height:100%; }
  .qr-modal-text{
    margin:0;
    text-align:left;
    font-family:'IBM Plex Mono', monospace;
    font-size:11px;
    line-height:1.6;
    color:var(--muted);
    background:var(--surface-2);
    border:1px solid var(--border);
    border-radius:6px;
    padding:10px 12px;
    white-space:pre-wrap;
    word-break:break-word;
  }

  /* ---------- responsive ---------- */
  @media (max-width:640px){
    .row-2{ grid-template-columns:1fr; }
    .doc-info{ grid-template-columns:1fr 1fr; }
    .doc-cell{ border-bottom:1px solid var(--border); }
    .doc-cell:nth-child(2n){ border-right:none; }
    .doc-cell.tagno{ grid-column:1 / -1; border-right:none; }
    .actions{ justify-content:stretch; }
    .btn{ flex:1; justify-content:center; }
  }

  /* ---------- print ---------- */
  @media print{
    :root{
      --page:#ffffff;
      --surface:#ffffff;
      --surface-2:#f5f7fa;
      --ink:#1f2933;
      --muted:#5b6b7a;
      --primary:#1d4e89;
      --primary-strong:#1d4e89;
      --primary-soft:#e7f0f9;
      --border:#d9e1e8;
      --danger:#b3261e;
      --danger-strong:#b3261e;
      --danger-soft:#fbeceb;
    }
    body::before, body::after{ display:none; }
    body{ background:#fff; padding:0; }
    .sheet{ box-shadow:none; border:none; max-width:100%; }
    .actions, .log, .modal-overlay{ display:none; }
    .sheet-header, .doc-cell.tagno, th{
      -webkit-print-color-adjust:exact;
      print-color-adjust:exact;
    }

    body.printing-log .layout{ display:none; }
    body.printing-log #printArea{ display:block; }
    body.printing-log #printTable th{
      -webkit-print-color-adjust:exact;
      print-color-adjust:exact;
    }
  }
</style>
</head>
<body>
  <div class="layout">

  <main class="sheet">
    <header class="sheet-header">
      <h1>Form for Record Data Inventory — PWB in PCBA</h1>
      <p>PWB stock record for PCBA — complete one entry per item counted</p>
    </header>

    <div class="doc-info">
      <div class="doc-cell tagno">
        <label for="tagNo">Tag No</label>
        <input type="text" id="tagNo" name="tagNo" value="0001" inputmode="numeric" />
      </div>
      <div class="doc-cell">
        <label for="year">Year</label>
        <input type="number" id="year" name="year" min="2000" max="2100" required />
      </div>
      <div class="doc-cell">
        <label for="month">Month</label>
        <select id="month" name="month" required>
          <option value="">--</option>
          <option value="1">01 — Jan</option>
          <option value="2">02 — Feb</option>
          <option value="3">03 — Mar</option>
          <option value="4">04 — Apr</option>
          <option value="5">05 — May</option>
          <option value="6">06 — Jun</option>
          <option value="7">07 — Jul</option>
          <option value="8">08 — Aug</option>
          <option value="9">09 — Sep</option>
          <option value="10">10 — Oct</option>
          <option value="11">11 — Nov</option>
          <option value="12">12 — Dec</option>
        </select>
      </div>
      <div class="doc-cell">
        <label for="day">Day</label>
        <input type="number" id="day" name="day" min="1" max="31" required />
      </div>
    </div>

    <div class="sheet-body">
      <form id="countForm">

        <div class="section">
          <p class="section-title">Item</p>
          <div class="row row-2">
            <div class="field">
              <label for="itemNo">Item No</label>
              <input type="text" id="itemNo" name="itemNo" required />
            </div>
            <div class="field">
              <label for="location">Location</label>
              <input type="text" id="location" name="location" required />
            </div>
          </div>
          <div class="row">
            <div class="field">
              <label for="detail">Detail</label>
              <input type="text" id="detail" name="detail" placeholder="Item description, or e.g. (4x5)+34+56" required />
              <span class="field-hint" id="detailHint"></span>
            </div>
          </div>
          <div class="row row-2">
            <div class="field">
              <label for="qty">Qty</label>
              <input type="text" id="qty" name="qty" inputmode="decimal" placeholder="e.g. 10+5+3" required />
              <span class="field-hint" id="qtyHint"></span>
            </div>
            <div class="field">
              <label for="unit">Unit</label>
              <input type="text" id="unit" name="unit" list="unitOptions" placeholder="PCS" value="PCS" />
              <datalist id="unitOptions">
                <option value="PCS"></option>
                <option value="BOX"></option>
                <option value="CARTON"></option>
                <option value="SET"></option>
                <option value="PACK"></option>
                <option value="ROLL"></option>
                <option value="KG"></option>
                <option value="G"></option>
                <option value="L"></option>
                <option value="ML"></option>
                <option value="M"></option>
                <option value="UNIT"></option>
              </datalist>
            </div>
          </div>
        </div>

        <div class="section">
          <p class="section-title">Verification</p>
          <div class="row row-2">
            <div class="field">
              <label for="countBy">Counted By</label>
              <input type="text" id="countBy" name="countBy" required />
            </div>
            <div class="field">
              <label for="checkBy">Checked By</label>
              <input type="text" id="checkBy" name="checkBy" />
            </div>
          </div>
        </div>

        <p class="edit-notice" id="editNotice" style="display:none;"></p>
        <div class="actions">
          <button type="button" class="btn btn-ghost" id="clearBtn">Clear</button>
          <button type="button" class="btn btn-ghost" id="printBtn">Print</button>
          <button type="button" class="btn btn-ghost" id="cancelEditBtn" style="display:none;">Cancel edit</button>
          <button type="submit" class="btn btn-primary" id="submitBtn">Save count</button>
        </div>

      </form>
    </div>
  </main>

  <section class="log">
    <div class="log-header">
      <h2>Session Log</h2>
      <div class="log-actions">
        <button class="btn btn-ghost btn-sm" id="csvBtn">Export CSV</button>
        <button class="btn btn-ghost btn-sm" id="printLogBtn">
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><polyline points="6 9 6 2 18 2 18 9"/><path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/><rect x="6" y="14" width="12" height="8"/></svg>
          Print selected
        </button>
        <button class="btn btn-danger btn-sm" id="clearLogBtn">
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="4" y="11" width="16" height="9" rx="2"/><path d="M8 11V7a4 4 0 0 1 8 0v4"/></svg>
          Clear log
        </button>
      </div>
    </div>
    <div class="table-wrap" id="tableWrap" style="display:none;">
      <table id="logTable">
        <thead>
          <tr>
            <th class="select-col"><input type="checkbox" id="selectAllRows" aria-label="Select all entries" /></th>
            <th class="qr-col">QR</th>
            <th>Tag No</th>
            <th>Date</th>
            <th>Item No</th>
            <th>Detail</th>
            <th>Location</th>
            <th>Qty</th>
            <th>Unit</th>
            <th>Counted By</th>
            <th>Checked By</th>
            <th class="row-actions">Edit</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
    <p class="empty" id="emptyMsg">No counts recorded yet — saved entries appear here.</p>
  </section>

  </div>

  <!-- Printable report for selected log entries -->
  <div id="printArea">
    <h1>Form for Record Data Inventory — PWB in PCBA</h1>
    <p class="print-meta"><span id="printDate"></span> &middot; <span id="printCount"></span></p>
    <table id="printTable">
      <thead>
        <tr>
          <th class="qr-col">QR</th>
          <th>Tag No</th>
          <th>Date</th>
          <th>Item No</th>
          <th>Detail</th>
          <th>Location</th>
          <th>Qty</th>
          <th>Unit</th>
          <th>Counted By</th>
          <th>Checked By</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>

  <!-- Clear log confirmation modal -->
  <div class="modal-overlay" id="clearModal">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="clearModalTitle">
      <div class="modal-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="4" y="11" width="16" height="9" rx="2"/><path d="M8 11V7a4 4 0 0 1 8 0v4"/></svg>
      </div>
      <h3 id="clearModalTitle">Clear session log</h3>
      <p id="clearModalText">This will permanently remove all recorded entries from this session. Enter the password to continue.</p>
      <div class="field">
        <label for="clearPassword">Password</label>
        <input type="password" id="clearPassword" autocomplete="off" />
        <span class="modal-error" id="clearError">Incorrect password. Try again.</span>
      </div>
      <div class="modal-actions">
        <button type="button" class="btn btn-ghost" id="clearModalCancel">Cancel</button>
        <button type="button" class="btn btn-danger-solid" id="clearModalConfirm">Clear log</button>
      </div>
    </div>
  </div>

  <!-- QR code detail modal -->
  <div class="modal-overlay" id="qrModal">
    <div class="modal qr-modal" role="dialog" aria-modal="true" aria-labelledby="qrModalTitle">
      <h3 id="qrModalTitle">Entry QR code</h3>
      <div class="qr-modal-code" id="qrModalCode"></div>
      <pre class="qr-modal-text" id="qrModalText"></pre>
      <div class="modal-actions">
        <button type="button" class="btn btn-ghost" id="qrModalClose">Close</button>
      </div>
    </div>
  </div>

  <!--
    Embedded QR code library: qrcodejs (https://github.com/davidshimjs/qrcodejs)
    MIT License - Copyright (c) 2012 davidshimjs.
    Bundled inline so this form generates QR codes fully offline (no CDN needed).
  -->
  <script>
/**
 * @fileoverview
 * - Using the 'QRCode for Javascript library'
 * - Fixed dataset of 'QRCode for Javascript library' for support full-spec.
 * - this library has no dependencies.
 * 
 * @author davidshimjs
 * @see <a href="http://www.d-project.com/" target="_blank">http://www.d-project.com/</a>
 * @see <a href="http://jeromeetienne.github.com/jquery-qrcode/" target="_blank">http://jeromeetienne.github.com/jquery-qrcode/</a>
 */
var QRCode;

(function () {
	//---------------------------------------------------------------------
	// QRCode for JavaScript
	//
	// Copyright (c) 2009 Kazuhiko Arase
	//
	// URL: http://www.d-project.com/
	//
	// Licensed under the MIT license:
	//   http://www.opensource.org/licenses/mit-license.php
	//
	// The word "QR Code" is registered trademark of 
	// DENSO WAVE INCORPORATED
	//   http://www.denso-wave.com/qrcode/faqpatent-e.html
	//
	//---------------------------------------------------------------------
	function QR8bitByte(data) {
		this.mode = QRMode.MODE_8BIT_BYTE;
		this.data = data;
		this.parsedData = [];

		// Added to support UTF-8 Characters
		for (var i = 0, l = this.data.length; i < l; i++) {
			var byteArray = [];
			var code = this.data.charCodeAt(i);

			if (code > 0x10000) {
				byteArray[0] = 0xF0 | ((code & 0x1C0000) >>> 18);
				byteArray[1] = 0x80 | ((code & 0x3F000) >>> 12);
				byteArray[2] = 0x80 | ((code & 0xFC0) >>> 6);
				byteArray[3] = 0x80 | (code & 0x3F);
			} else if (code > 0x800) {
				byteArray[0] = 0xE0 | ((code & 0xF000) >>> 12);
				byteArray[1] = 0x80 | ((code & 0xFC0) >>> 6);
				byteArray[2] = 0x80 | (code & 0x3F);
			} else if (code > 0x80) {
				byteArray[0] = 0xC0 | ((code & 0x7C0) >>> 6);
				byteArray[1] = 0x80 | (code & 0x3F);
			} else {
				byteArray[0] = code;
			}

			this.parsedData.push(byteArray);
		}

		this.parsedData = Array.prototype.concat.apply([], this.parsedData);

		if (this.parsedData.length != this.data.length) {
			this.parsedData.unshift(191);
			this.parsedData.unshift(187);
			this.parsedData.unshift(239);
		}
	}

	QR8bitByte.prototype = {
		getLength: function (buffer) {
			return this.parsedData.length;
		},
		write: function (buffer) {
			for (var i = 0, l = this.parsedData.length; i < l; i++) {
				buffer.put(this.parsedData[i], 8);
			}
		}
	};

	function QRCodeModel(typeNumber, errorCorrectLevel) {
		this.typeNumber = typeNumber;
		this.errorCorrectLevel = errorCorrectLevel;
		this.modules = null;
		this.moduleCount = 0;
		this.dataCache = null;
		this.dataList = [];
	}

	QRCodeModel.prototype={addData:function(data){var newData=new QR8bitByte(data);this.dataList.push(newData);this.dataCache=null;},isDark:function(row,col){if(row<0||this.moduleCount<=row||col<0||this.moduleCount<=col){throw new Error(row+","+col);}
	return this.modules[row][col];},getModuleCount:function(){return this.moduleCount;},make:function(){this.makeImpl(false,this.getBestMaskPattern());},makeImpl:function(test,maskPattern){this.moduleCount=this.typeNumber*4+17;this.modules=new Array(this.moduleCount);for(var row=0;row<this.moduleCount;row++){this.modules[row]=new Array(this.moduleCount);for(var col=0;col<this.moduleCount;col++){this.modules[row][col]=null;}}
	this.setupPositionProbePattern(0,0);this.setupPositionProbePattern(this.moduleCount-7,0);this.setupPositionProbePattern(0,this.moduleCount-7);this.setupPositionAdjustPattern();this.setupTimingPattern();this.setupTypeInfo(test,maskPattern);if(this.typeNumber>=7){this.setupTypeNumber(test);}
	if(this.dataCache==null){this.dataCache=QRCodeModel.createData(this.typeNumber,this.errorCorrectLevel,this.dataList);}
	this.mapData(this.dataCache,maskPattern);},setupPositionProbePattern:function(row,col){for(var r=-1;r<=7;r++){if(row+r<=-1||this.moduleCount<=row+r)continue;for(var c=-1;c<=7;c++){if(col+c<=-1||this.moduleCount<=col+c)continue;if((0<=r&&r<=6&&(c==0||c==6))||(0<=c&&c<=6&&(r==0||r==6))||(2<=r&&r<=4&&2<=c&&c<=4)){this.modules[row+r][col+c]=true;}else{this.modules[row+r][col+c]=false;}}}},getBestMaskPattern:function(){var minLostPoint=0;var pattern=0;for(var i=0;i<8;i++){this.makeImpl(true,i);var lostPoint=QRUtil.getLostPoint(this);if(i==0||minLostPoint>lostPoint){minLostPoint=lostPoint;pattern=i;}}
	return pattern;},createMovieClip:function(target_mc,instance_name,depth){var qr_mc=target_mc.createEmptyMovieClip(instance_name,depth);var cs=1;this.make();for(var row=0;row<this.modules.length;row++){var y=row*cs;for(var col=0;col<this.modules[row].length;col++){var x=col*cs;var dark=this.modules[row][col];if(dark){qr_mc.beginFill(0,100);qr_mc.moveTo(x,y);qr_mc.lineTo(x+cs,y);qr_mc.lineTo(x+cs,y+cs);qr_mc.lineTo(x,y+cs);qr_mc.endFill();}}}
	return qr_mc;},setupTimingPattern:function(){for(var r=8;r<this.moduleCount-8;r++){if(this.modules[r][6]!=null){continue;}
	this.modules[r][6]=(r%2==0);}
	for(var c=8;c<this.moduleCount-8;c++){if(this.modules[6][c]!=null){continue;}
	this.modules[6][c]=(c%2==0);}},setupPositionAdjustPattern:function(){var pos=QRUtil.getPatternPosition(this.typeNumber);for(var i=0;i<pos.length;i++){for(var j=0;j<pos.length;j++){var row=pos[i];var col=pos[j];if(this.modules[row][col]!=null){continue;}
	for(var r=-2;r<=2;r++){for(var c=-2;c<=2;c++){if(r==-2||r==2||c==-2||c==2||(r==0&&c==0)){this.modules[row+r][col+c]=true;}else{this.modules[row+r][col+c]=false;}}}}}},setupTypeNumber:function(test){var bits=QRUtil.getBCHTypeNumber(this.typeNumber);for(var i=0;i<18;i++){var mod=(!test&&((bits>>i)&1)==1);this.modules[Math.floor(i/3)][i%3+this.moduleCount-8-3]=mod;}
	for(var i=0;i<18;i++){var mod=(!test&&((bits>>i)&1)==1);this.modules[i%3+this.moduleCount-8-3][Math.floor(i/3)]=mod;}},setupTypeInfo:function(test,maskPattern){var data=(this.errorCorrectLevel<<3)|maskPattern;var bits=QRUtil.getBCHTypeInfo(data);for(var i=0;i<15;i++){var mod=(!test&&((bits>>i)&1)==1);if(i<6){this.modules[i][8]=mod;}else if(i<8){this.modules[i+1][8]=mod;}else{this.modules[this.moduleCount-15+i][8]=mod;}}
	for(var i=0;i<15;i++){var mod=(!test&&((bits>>i)&1)==1);if(i<8){this.modules[8][this.moduleCount-i-1]=mod;}else if(i<9){this.modules[8][15-i-1+1]=mod;}else{this.modules[8][15-i-1]=mod;}}
	this.modules[this.moduleCount-8][8]=(!test);},mapData:function(data,maskPattern){var inc=-1;var row=this.moduleCount-1;var bitIndex=7;var byteIndex=0;for(var col=this.moduleCount-1;col>0;col-=2){if(col==6)col--;while(true){for(var c=0;c<2;c++){if(this.modules[row][col-c]==null){var dark=false;if(byteIndex<data.length){dark=(((data[byteIndex]>>>bitIndex)&1)==1);}
	var mask=QRUtil.getMask(maskPattern,row,col-c);if(mask){dark=!dark;}
	this.modules[row][col-c]=dark;bitIndex--;if(bitIndex==-1){byteIndex++;bitIndex=7;}}}
	row+=inc;if(row<0||this.moduleCount<=row){row-=inc;inc=-inc;break;}}}}};QRCodeModel.PAD0=0xEC;QRCodeModel.PAD1=0x11;QRCodeModel.createData=function(typeNumber,errorCorrectLevel,dataList){var rsBlocks=QRRSBlock.getRSBlocks(typeNumber,errorCorrectLevel);var buffer=new QRBitBuffer();for(var i=0;i<dataList.length;i++){var data=dataList[i];buffer.put(data.mode,4);buffer.put(data.getLength(),QRUtil.getLengthInBits(data.mode,typeNumber));data.write(buffer);}
	var totalDataCount=0;for(var i=0;i<rsBlocks.length;i++){totalDataCount+=rsBlocks[i].dataCount;}
	if(buffer.getLengthInBits()>totalDataCount*8){throw new Error("code length overflow. ("
	+buffer.getLengthInBits()
	+">"
	+totalDataCount*8
	+")");}
	if(buffer.getLengthInBits()+4<=totalDataCount*8){buffer.put(0,4);}
	while(buffer.getLengthInBits()%8!=0){buffer.putBit(false);}
	while(true){if(buffer.getLengthInBits()>=totalDataCount*8){break;}
	buffer.put(QRCodeModel.PAD0,8);if(buffer.getLengthInBits()>=totalDataCount*8){break;}
	buffer.put(QRCodeModel.PAD1,8);}
	return QRCodeModel.createBytes(buffer,rsBlocks);};QRCodeModel.createBytes=function(buffer,rsBlocks){var offset=0;var maxDcCount=0;var maxEcCount=0;var dcdata=new Array(rsBlocks.length);var ecdata=new Array(rsBlocks.length);for(var r=0;r<rsBlocks.length;r++){var dcCount=rsBlocks[r].dataCount;var ecCount=rsBlocks[r].totalCount-dcCount;maxDcCount=Math.max(maxDcCount,dcCount);maxEcCount=Math.max(maxEcCount,ecCount);dcdata[r]=new Array(dcCount);for(var i=0;i<dcdata[r].length;i++){dcdata[r][i]=0xff&buffer.buffer[i+offset];}
	offset+=dcCount;var rsPoly=QRUtil.getErrorCorrectPolynomial(ecCount);var rawPoly=new QRPolynomial(dcdata[r],rsPoly.getLength()-1);var modPoly=rawPoly.mod(rsPoly);ecdata[r]=new Array(rsPoly.getLength()-1);for(var i=0;i<ecdata[r].length;i++){var modIndex=i+modPoly.getLength()-ecdata[r].length;ecdata[r][i]=(modIndex>=0)?modPoly.get(modIndex):0;}}
	var totalCodeCount=0;for(var i=0;i<rsBlocks.length;i++){totalCodeCount+=rsBlocks[i].totalCount;}
	var data=new Array(totalCodeCount);var index=0;for(var i=0;i<maxDcCount;i++){for(var r=0;r<rsBlocks.length;r++){if(i<dcdata[r].length){data[index++]=dcdata[r][i];}}}
	for(var i=0;i<maxEcCount;i++){for(var r=0;r<rsBlocks.length;r++){if(i<ecdata[r].length){data[index++]=ecdata[r][i];}}}
	return data;};var QRMode={MODE_NUMBER:1<<0,MODE_ALPHA_NUM:1<<1,MODE_8BIT_BYTE:1<<2,MODE_KANJI:1<<3};var QRErrorCorrectLevel={L:1,M:0,Q:3,H:2};var QRMaskPattern={PATTERN000:0,PATTERN001:1,PATTERN010:2,PATTERN011:3,PATTERN100:4,PATTERN101:5,PATTERN110:6,PATTERN111:7};var QRUtil={PATTERN_POSITION_TABLE:[[],[6,18],[6,22],[6,26],[6,30],[6,34],[6,22,38],[6,24,42],[6,26,46],[6,28,50],[6,30,54],[6,32,58],[6,34,62],[6,26,46,66],[6,26,48,70],[6,26,50,74],[6,30,54,78],[6,30,56,82],[6,30,58,86],[6,34,62,90],[6,28,50,72,94],[6,26,50,74,98],[6,30,54,78,102],[6,28,54,80,106],[6,32,58,84,110],[6,30,58,86,114],[6,34,62,90,118],[6,26,50,74,98,122],[6,30,54,78,102,126],[6,26,52,78,104,130],[6,30,56,82,108,134],[6,34,60,86,112,138],[6,30,58,86,114,142],[6,34,62,90,118,146],[6,30,54,78,102,126,150],[6,24,50,76,102,128,154],[6,28,54,80,106,132,158],[6,32,58,84,110,136,162],[6,26,54,82,110,138,166],[6,30,58,86,114,142,170]],G15:(1<<10)|(1<<8)|(1<<5)|(1<<4)|(1<<2)|(1<<1)|(1<<0),G18:(1<<12)|(1<<11)|(1<<10)|(1<<9)|(1<<8)|(1<<5)|(1<<2)|(1<<0),G15_MASK:(1<<14)|(1<<12)|(1<<10)|(1<<4)|(1<<1),getBCHTypeInfo:function(data){var d=data<<10;while(QRUtil.getBCHDigit(d)-QRUtil.getBCHDigit(QRUtil.G15)>=0){d^=(QRUtil.G15<<(QRUtil.getBCHDigit(d)-QRUtil.getBCHDigit(QRUtil.G15)));}
	return((data<<10)|d)^QRUtil.G15_MASK;},getBCHTypeNumber:function(data){var d=data<<12;while(QRUtil.getBCHDigit(d)-QRUtil.getBCHDigit(QRUtil.G18)>=0){d^=(QRUtil.G18<<(QRUtil.getBCHDigit(d)-QRUtil.getBCHDigit(QRUtil.G18)));}
	return(data<<12)|d;},getBCHDigit:function(data){var digit=0;while(data!=0){digit++;data>>>=1;}
	return digit;},getPatternPosition:function(typeNumber){return QRUtil.PATTERN_POSITION_TABLE[typeNumber-1];},getMask:function(maskPattern,i,j){switch(maskPattern){case QRMaskPattern.PATTERN000:return(i+j)%2==0;case QRMaskPattern.PATTERN001:return i%2==0;case QRMaskPattern.PATTERN010:return j%3==0;case QRMaskPattern.PATTERN011:return(i+j)%3==0;case QRMaskPattern.PATTERN100:return(Math.floor(i/2)+Math.floor(j/3))%2==0;case QRMaskPattern.PATTERN101:return(i*j)%2+(i*j)%3==0;case QRMaskPattern.PATTERN110:return((i*j)%2+(i*j)%3)%2==0;case QRMaskPattern.PATTERN111:return((i*j)%3+(i+j)%2)%2==0;default:throw new Error("bad maskPattern:"+maskPattern);}},getErrorCorrectPolynomial:function(errorCorrectLength){var a=new QRPolynomial([1],0);for(var i=0;i<errorCorrectLength;i++){a=a.multiply(new QRPolynomial([1,QRMath.gexp(i)],0));}
	return a;},getLengthInBits:function(mode,type){if(1<=type&&type<10){switch(mode){case QRMode.MODE_NUMBER:return 10;case QRMode.MODE_ALPHA_NUM:return 9;case QRMode.MODE_8BIT_BYTE:return 8;case QRMode.MODE_KANJI:return 8;default:throw new Error("mode:"+mode);}}else if(type<27){switch(mode){case QRMode.MODE_NUMBER:return 12;case QRMode.MODE_ALPHA_NUM:return 11;case QRMode.MODE_8BIT_BYTE:return 16;case QRMode.MODE_KANJI:return 10;default:throw new Error("mode:"+mode);}}else if(type<41){switch(mode){case QRMode.MODE_NUMBER:return 14;case QRMode.MODE_ALPHA_NUM:return 13;case QRMode.MODE_8BIT_BYTE:return 16;case QRMode.MODE_KANJI:return 12;default:throw new Error("mode:"+mode);}}else{throw new Error("type:"+type);}},getLostPoint:function(qrCode){var moduleCount=qrCode.getModuleCount();var lostPoint=0;for(var row=0;row<moduleCount;row++){for(var col=0;col<moduleCount;col++){var sameCount=0;var dark=qrCode.isDark(row,col);for(var r=-1;r<=1;r++){if(row+r<0||moduleCount<=row+r){continue;}
	for(var c=-1;c<=1;c++){if(col+c<0||moduleCount<=col+c){continue;}
	if(r==0&&c==0){continue;}
	if(dark==qrCode.isDark(row+r,col+c)){sameCount++;}}}
	if(sameCount>5){lostPoint+=(3+sameCount-5);}}}
	for(var row=0;row<moduleCount-1;row++){for(var col=0;col<moduleCount-1;col++){var count=0;if(qrCode.isDark(row,col))count++;if(qrCode.isDark(row+1,col))count++;if(qrCode.isDark(row,col+1))count++;if(qrCode.isDark(row+1,col+1))count++;if(count==0||count==4){lostPoint+=3;}}}
	for(var row=0;row<moduleCount;row++){for(var col=0;col<moduleCount-6;col++){if(qrCode.isDark(row,col)&&!qrCode.isDark(row,col+1)&&qrCode.isDark(row,col+2)&&qrCode.isDark(row,col+3)&&qrCode.isDark(row,col+4)&&!qrCode.isDark(row,col+5)&&qrCode.isDark(row,col+6)){lostPoint+=40;}}}
	for(var col=0;col<moduleCount;col++){for(var row=0;row<moduleCount-6;row++){if(qrCode.isDark(row,col)&&!qrCode.isDark(row+1,col)&&qrCode.isDark(row+2,col)&&qrCode.isDark(row+3,col)&&qrCode.isDark(row+4,col)&&!qrCode.isDark(row+5,col)&&qrCode.isDark(row+6,col)){lostPoint+=40;}}}
	var darkCount=0;for(var col=0;col<moduleCount;col++){for(var row=0;row<moduleCount;row++){if(qrCode.isDark(row,col)){darkCount++;}}}
	var ratio=Math.abs(100*darkCount/moduleCount/moduleCount-50)/5;lostPoint+=ratio*10;return lostPoint;}};var QRMath={glog:function(n){if(n<1){throw new Error("glog("+n+")");}
	return QRMath.LOG_TABLE[n];},gexp:function(n){while(n<0){n+=255;}
	while(n>=256){n-=255;}
	return QRMath.EXP_TABLE[n];},EXP_TABLE:new Array(256),LOG_TABLE:new Array(256)};for(var i=0;i<8;i++){QRMath.EXP_TABLE[i]=1<<i;}
	for(var i=8;i<256;i++){QRMath.EXP_TABLE[i]=QRMath.EXP_TABLE[i-4]^QRMath.EXP_TABLE[i-5]^QRMath.EXP_TABLE[i-6]^QRMath.EXP_TABLE[i-8];}
	for(var i=0;i<255;i++){QRMath.LOG_TABLE[QRMath.EXP_TABLE[i]]=i;}
	function QRPolynomial(num,shift){if(num.length==undefined){throw new Error(num.length+"/"+shift);}
	var offset=0;while(offset<num.length&&num[offset]==0){offset++;}
	this.num=new Array(num.length-offset+shift);for(var i=0;i<num.length-offset;i++){this.num[i]=num[i+offset];}}
	QRPolynomial.prototype={get:function(index){return this.num[index];},getLength:function(){return this.num.length;},multiply:function(e){var num=new Array(this.getLength()+e.getLength()-1);for(var i=0;i<this.getLength();i++){for(var j=0;j<e.getLength();j++){num[i+j]^=QRMath.gexp(QRMath.glog(this.get(i))+QRMath.glog(e.get(j)));}}
	return new QRPolynomial(num,0);},mod:function(e){if(this.getLength()-e.getLength()<0){return this;}
	var ratio=QRMath.glog(this.get(0))-QRMath.glog(e.get(0));var num=new Array(this.getLength());for(var i=0;i<this.getLength();i++){num[i]=this.get(i);}
	for(var i=0;i<e.getLength();i++){num[i]^=QRMath.gexp(QRMath.glog(e.get(i))+ratio);}
	return new QRPolynomial(num,0).mod(e);}};function QRRSBlock(totalCount,dataCount){this.totalCount=totalCount;this.dataCount=dataCount;}
	QRRSBlock.RS_BLOCK_TABLE=[[1,26,19],[1,26,16],[1,26,13],[1,26,9],[1,44,34],[1,44,28],[1,44,22],[1,44,16],[1,70,55],[1,70,44],[2,35,17],[2,35,13],[1,100,80],[2,50,32],[2,50,24],[4,25,9],[1,134,108],[2,67,43],[2,33,15,2,34,16],[2,33,11,2,34,12],[2,86,68],[4,43,27],[4,43,19],[4,43,15],[2,98,78],[4,49,31],[2,32,14,4,33,15],[4,39,13,1,40,14],[2,121,97],[2,60,38,2,61,39],[4,40,18,2,41,19],[4,40,14,2,41,15],[2,146,116],[3,58,36,2,59,37],[4,36,16,4,37,17],[4,36,12,4,37,13],[2,86,68,2,87,69],[4,69,43,1,70,44],[6,43,19,2,44,20],[6,43,15,2,44,16],[4,101,81],[1,80,50,4,81,51],[4,50,22,4,51,23],[3,36,12,8,37,13],[2,116,92,2,117,93],[6,58,36,2,59,37],[4,46,20,6,47,21],[7,42,14,4,43,15],[4,133,107],[8,59,37,1,60,38],[8,44,20,4,45,21],[12,33,11,4,34,12],[3,145,115,1,146,116],[4,64,40,5,65,41],[11,36,16,5,37,17],[11,36,12,5,37,13],[5,109,87,1,110,88],[5,65,41,5,66,42],[5,54,24,7,55,25],[11,36,12],[5,122,98,1,123,99],[7,73,45,3,74,46],[15,43,19,2,44,20],[3,45,15,13,46,16],[1,135,107,5,136,108],[10,74,46,1,75,47],[1,50,22,15,51,23],[2,42,14,17,43,15],[5,150,120,1,151,121],[9,69,43,4,70,44],[17,50,22,1,51,23],[2,42,14,19,43,15],[3,141,113,4,142,114],[3,70,44,11,71,45],[17,47,21,4,48,22],[9,39,13,16,40,14],[3,135,107,5,136,108],[3,67,41,13,68,42],[15,54,24,5,55,25],[15,43,15,10,44,16],[4,144,116,4,145,117],[17,68,42],[17,50,22,6,51,23],[19,46,16,6,47,17],[2,139,111,7,140,112],[17,74,46],[7,54,24,16,55,25],[34,37,13],[4,151,121,5,152,122],[4,75,47,14,76,48],[11,54,24,14,55,25],[16,45,15,14,46,16],[6,147,117,4,148,118],[6,73,45,14,74,46],[11,54,24,16,55,25],[30,46,16,2,47,17],[8,132,106,4,133,107],[8,75,47,13,76,48],[7,54,24,22,55,25],[22,45,15,13,46,16],[10,142,114,2,143,115],[19,74,46,4,75,47],[28,50,22,6,51,23],[33,46,16,4,47,17],[8,152,122,4,153,123],[22,73,45,3,74,46],[8,53,23,26,54,24],[12,45,15,28,46,16],[3,147,117,10,148,118],[3,73,45,23,74,46],[4,54,24,31,55,25],[11,45,15,31,46,16],[7,146,116,7,147,117],[21,73,45,7,74,46],[1,53,23,37,54,24],[19,45,15,26,46,16],[5,145,115,10,146,116],[19,75,47,10,76,48],[15,54,24,25,55,25],[23,45,15,25,46,16],[13,145,115,3,146,116],[2,74,46,29,75,47],[42,54,24,1,55,25],[23,45,15,28,46,16],[17,145,115],[10,74,46,23,75,47],[10,54,24,35,55,25],[19,45,15,35,46,16],[17,145,115,1,146,116],[14,74,46,21,75,47],[29,54,24,19,55,25],[11,45,15,46,46,16],[13,145,115,6,146,116],[14,74,46,23,75,47],[44,54,24,7,55,25],[59,46,16,1,47,17],[12,151,121,7,152,122],[12,75,47,26,76,48],[39,54,24,14,55,25],[22,45,15,41,46,16],[6,151,121,14,152,122],[6,75,47,34,76,48],[46,54,24,10,55,25],[2,45,15,64,46,16],[17,152,122,4,153,123],[29,74,46,14,75,47],[49,54,24,10,55,25],[24,45,15,46,46,16],[4,152,122,18,153,123],[13,74,46,32,75,47],[48,54,24,14,55,25],[42,45,15,32,46,16],[20,147,117,4,148,118],[40,75,47,7,76,48],[43,54,24,22,55,25],[10,45,15,67,46,16],[19,148,118,6,149,119],[18,75,47,31,76,48],[34,54,24,34,55,25],[20,45,15,61,46,16]];QRRSBlock.getRSBlocks=function(typeNumber,errorCorrectLevel){var rsBlock=QRRSBlock.getRsBlockTable(typeNumber,errorCorrectLevel);if(rsBlock==undefined){throw new Error("bad rs block @ typeNumber:"+typeNumber+"/errorCorrectLevel:"+errorCorrectLevel);}
	var length=rsBlock.length/3;var list=[];for(var i=0;i<length;i++){var count=rsBlock[i*3+0];var totalCount=rsBlock[i*3+1];var dataCount=rsBlock[i*3+2];for(var j=0;j<count;j++){list.push(new QRRSBlock(totalCount,dataCount));}}
	return list;};QRRSBlock.getRsBlockTable=function(typeNumber,errorCorrectLevel){switch(errorCorrectLevel){case QRErrorCorrectLevel.L:return QRRSBlock.RS_BLOCK_TABLE[(typeNumber-1)*4+0];case QRErrorCorrectLevel.M:return QRRSBlock.RS_BLOCK_TABLE[(typeNumber-1)*4+1];case QRErrorCorrectLevel.Q:return QRRSBlock.RS_BLOCK_TABLE[(typeNumber-1)*4+2];case QRErrorCorrectLevel.H:return QRRSBlock.RS_BLOCK_TABLE[(typeNumber-1)*4+3];default:return undefined;}};function QRBitBuffer(){this.buffer=[];this.length=0;}
	QRBitBuffer.prototype={get:function(index){var bufIndex=Math.floor(index/8);return((this.buffer[bufIndex]>>>(7-index%8))&1)==1;},put:function(num,length){for(var i=0;i<length;i++){this.putBit(((num>>>(length-i-1))&1)==1);}},getLengthInBits:function(){return this.length;},putBit:function(bit){var bufIndex=Math.floor(this.length/8);if(this.buffer.length<=bufIndex){this.buffer.push(0);}
	if(bit){this.buffer[bufIndex]|=(0x80>>>(this.length%8));}
	this.length++;}};var QRCodeLimitLength=[[17,14,11,7],[32,26,20,14],[53,42,32,24],[78,62,46,34],[106,84,60,44],[134,106,74,58],[154,122,86,64],[192,152,108,84],[230,180,130,98],[271,213,151,119],[321,251,177,137],[367,287,203,155],[425,331,241,177],[458,362,258,194],[520,412,292,220],[586,450,322,250],[644,504,364,280],[718,560,394,310],[792,624,442,338],[858,666,482,382],[929,711,509,403],[1003,779,565,439],[1091,857,611,461],[1171,911,661,511],[1273,997,715,535],[1367,1059,751,593],[1465,1125,805,625],[1528,1190,868,658],[1628,1264,908,698],[1732,1370,982,742],[1840,1452,1030,790],[1952,1538,1112,842],[2068,1628,1168,898],[2188,1722,1228,958],[2303,1809,1283,983],[2431,1911,1351,1051],[2563,1989,1423,1093],[2699,2099,1499,1139],[2809,2213,1579,1219],[2953,2331,1663,1273]];
	
	function _isSupportCanvas() {
		return typeof CanvasRenderingContext2D != "undefined";
	}
	
	// android 2.x doesn't support Data-URI spec
	function _getAndroid() {
		var android = false;
		var sAgent = navigator.userAgent;
		
		if (/android/i.test(sAgent)) { // android
			android = true;
			var aMat = sAgent.toString().match(/android ([0-9]\.[0-9])/i);
			
			if (aMat && aMat[1]) {
				android = parseFloat(aMat[1]);
			}
		}
		
		return android;
	}
	
	var svgDrawer = (function() {

		var Drawing = function (el, htOption) {
			this._el = el;
			this._htOption = htOption;
		};

		Drawing.prototype.draw = function (oQRCode) {
			var _htOption = this._htOption;
			var _el = this._el;
			var nCount = oQRCode.getModuleCount();
			var nWidth = Math.floor(_htOption.width / nCount);
			var nHeight = Math.floor(_htOption.height / nCount);

			this.clear();

			function makeSVG(tag, attrs) {
				var el = document.createElementNS('http://www.w3.org/2000/svg', tag);
				for (var k in attrs)
					if (attrs.hasOwnProperty(k)) el.setAttribute(k, attrs[k]);
				return el;
			}

			var svg = makeSVG("svg" , {'viewBox': '0 0 ' + String(nCount) + " " + String(nCount), 'width': '100%', 'height': '100%', 'fill': _htOption.colorLight});
			svg.setAttributeNS("http://www.w3.org/2000/xmlns/", "xmlns:xlink", "http://www.w3.org/1999/xlink");
			_el.appendChild(svg);

			svg.appendChild(makeSVG("rect", {"fill": _htOption.colorLight, "width": "100%", "height": "100%"}));
			svg.appendChild(makeSVG("rect", {"fill": _htOption.colorDark, "width": "1", "height": "1", "id": "template"}));

			for (var row = 0; row < nCount; row++) {
				for (var col = 0; col < nCount; col++) {
					if (oQRCode.isDark(row, col)) {
						var child = makeSVG("use", {"x": String(col), "y": String(row)});
						child.setAttributeNS("http://www.w3.org/1999/xlink", "href", "#template")
						svg.appendChild(child);
					}
				}
			}
		};
		Drawing.prototype.clear = function () {
			while (this._el.hasChildNodes())
				this._el.removeChild(this._el.lastChild);
		};
		return Drawing;
	})();

	var useSVG = document.documentElement.tagName.toLowerCase() === "svg";

	// Drawing in DOM by using Table tag
	var Drawing = useSVG ? svgDrawer : !_isSupportCanvas() ? (function () {
		var Drawing = function (el, htOption) {
			this._el = el;
			this._htOption = htOption;
		};
			
		/**
		 * Draw the QRCode
		 * 
		 * @param {QRCode} oQRCode
		 */
		Drawing.prototype.draw = function (oQRCode) {
            var _htOption = this._htOption;
            var _el = this._el;
			var nCount = oQRCode.getModuleCount();
			var nWidth = Math.floor(_htOption.width / nCount);
			var nHeight = Math.floor(_htOption.height / nCount);
			var aHTML = ['<table style="border:0;border-collapse:collapse;">'];
			
			for (var row = 0; row < nCount; row++) {
				aHTML.push('<tr>');
				
				for (var col = 0; col < nCount; col++) {
					aHTML.push('<td style="border:0;border-collapse:collapse;padding:0;margin:0;width:' + nWidth + 'px;height:' + nHeight + 'px;background-color:' + (oQRCode.isDark(row, col) ? _htOption.colorDark : _htOption.colorLight) + ';"></td>');
				}
				
				aHTML.push('</tr>');
			}
			
			aHTML.push('</table>');
			_el.innerHTML = aHTML.join('');
			
			// Fix the margin values as real size.
			var elTable = _el.childNodes[0];
			var nLeftMarginTable = (_htOption.width - elTable.offsetWidth) / 2;
			var nTopMarginTable = (_htOption.height - elTable.offsetHeight) / 2;
			
			if (nLeftMarginTable > 0 && nTopMarginTable > 0) {
				elTable.style.margin = nTopMarginTable + "px " + nLeftMarginTable + "px";	
			}
		};
		
		/**
		 * Clear the QRCode
		 */
		Drawing.prototype.clear = function () {
			this._el.innerHTML = '';
		};
		
		return Drawing;
	})() : (function () { // Drawing in Canvas
		function _onMakeImage() {
			this._elImage.src = this._elCanvas.toDataURL("image/png");
			this._elImage.style.display = "block";
			this._elCanvas.style.display = "none";			
		}
		
		// Android 2.1 bug workaround
		// http://code.google.com/p/android/issues/detail?id=5141
		if (this._android && this._android <= 2.1) {
	    	var factor = 1 / window.devicePixelRatio;
	        var drawImage = CanvasRenderingContext2D.prototype.drawImage; 
	    	CanvasRenderingContext2D.prototype.drawImage = function (image, sx, sy, sw, sh, dx, dy, dw, dh) {
	    		if (("nodeName" in image) && /img/i.test(image.nodeName)) {
		        	for (var i = arguments.length - 1; i >= 1; i--) {
		            	arguments[i] = arguments[i] * factor;
		        	}
	    		} else if (typeof dw == "undefined") {
	    			arguments[1] *= factor;
	    			arguments[2] *= factor;
	    			arguments[3] *= factor;
	    			arguments[4] *= factor;
	    		}
	    		
	        	drawImage.apply(this, arguments); 
	    	};
		}
		
		/**
		 * Check whether the user's browser supports Data URI or not
		 * 
		 * @private
		 * @param {Function} fSuccess Occurs if it supports Data URI
		 * @param {Function} fFail Occurs if it doesn't support Data URI
		 */
		function _safeSetDataURI(fSuccess, fFail) {
            var self = this;
            self._fFail = fFail;
            self._fSuccess = fSuccess;

            // Check it just once
            if (self._bSupportDataURI === null) {
                var el = document.createElement("img");
                var fOnError = function() {
                    self._bSupportDataURI = false;

                    if (self._fFail) {
                        self._fFail.call(self);
                    }
                };
                var fOnSuccess = function() {
                    self._bSupportDataURI = true;

                    if (self._fSuccess) {
                        self._fSuccess.call(self);
                    }
                };

                el.onabort = fOnError;
                el.onerror = fOnError;
                el.onload = fOnSuccess;
                el.src = "data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg=="; // the Image contains 1px data.
                return;
            } else if (self._bSupportDataURI === true && self._fSuccess) {
                self._fSuccess.call(self);
            } else if (self._bSupportDataURI === false && self._fFail) {
                self._fFail.call(self);
            }
		};
		
		/**
		 * Drawing QRCode by using canvas
		 * 
		 * @constructor
		 * @param {HTMLElement} el
		 * @param {Object} htOption QRCode Options 
		 */
		var Drawing = function (el, htOption) {
    		this._bIsPainted = false;
    		this._android = _getAndroid();
		
			this._htOption = htOption;
			this._elCanvas = document.createElement("canvas");
			this._elCanvas.width = htOption.width;
			this._elCanvas.height = htOption.height;
			el.appendChild(this._elCanvas);
			this._el = el;
			this._oContext = this._elCanvas.getContext("2d");
			this._bIsPainted = false;
			this._elImage = document.createElement("img");
			this._elImage.alt = "Scan me!";
			this._elImage.style.display = "none";
			this._el.appendChild(this._elImage);
			this._bSupportDataURI = null;
		};
			
		/**
		 * Draw the QRCode
		 * 
		 * @param {QRCode} oQRCode 
		 */
		Drawing.prototype.draw = function (oQRCode) {
            var _elImage = this._elImage;
            var _oContext = this._oContext;
            var _htOption = this._htOption;
            
			var nCount = oQRCode.getModuleCount();
			var nWidth = _htOption.width / nCount;
			var nHeight = _htOption.height / nCount;
			var nRoundedWidth = Math.round(nWidth);
			var nRoundedHeight = Math.round(nHeight);

			_elImage.style.display = "none";
			this.clear();
			
			for (var row = 0; row < nCount; row++) {
				for (var col = 0; col < nCount; col++) {
					var bIsDark = oQRCode.isDark(row, col);
					var nLeft = col * nWidth;
					var nTop = row * nHeight;
					_oContext.strokeStyle = bIsDark ? _htOption.colorDark : _htOption.colorLight;
					_oContext.lineWidth = 1;
					_oContext.fillStyle = bIsDark ? _htOption.colorDark : _htOption.colorLight;					
					_oContext.fillRect(nLeft, nTop, nWidth, nHeight);
					
					// 안티 앨리어싱 방지 처리
					_oContext.strokeRect(
						Math.floor(nLeft) + 0.5,
						Math.floor(nTop) + 0.5,
						nRoundedWidth,
						nRoundedHeight
					);
					
					_oContext.strokeRect(
						Math.ceil(nLeft) - 0.5,
						Math.ceil(nTop) - 0.5,
						nRoundedWidth,
						nRoundedHeight
					);
				}
			}
			
			this._bIsPainted = true;
		};
			
		/**
		 * Make the image from Canvas if the browser supports Data URI.
		 */
		Drawing.prototype.makeImage = function () {
			if (this._bIsPainted) {
				_safeSetDataURI.call(this, _onMakeImage);
			}
		};
			
		/**
		 * Return whether the QRCode is painted or not
		 * 
		 * @return {Boolean}
		 */
		Drawing.prototype.isPainted = function () {
			return this._bIsPainted;
		};
		
		/**
		 * Clear the QRCode
		 */
		Drawing.prototype.clear = function () {
			this._oContext.clearRect(0, 0, this._elCanvas.width, this._elCanvas.height);
			this._bIsPainted = false;
		};
		
		/**
		 * @private
		 * @param {Number} nNumber
		 */
		Drawing.prototype.round = function (nNumber) {
			if (!nNumber) {
				return nNumber;
			}
			
			return Math.floor(nNumber * 1000) / 1000;
		};
		
		return Drawing;
	})();
	
	/**
	 * Get the type by string length
	 * 
	 * @private
	 * @param {String} sText
	 * @param {Number} nCorrectLevel
	 * @return {Number} type
	 */
	function _getTypeNumber(sText, nCorrectLevel) {			
		var nType = 1;
		var length = _getUTF8Length(sText);
		
		for (var i = 0, len = QRCodeLimitLength.length; i <= len; i++) {
			var nLimit = 0;
			
			switch (nCorrectLevel) {
				case QRErrorCorrectLevel.L :
					nLimit = QRCodeLimitLength[i][0];
					break;
				case QRErrorCorrectLevel.M :
					nLimit = QRCodeLimitLength[i][1];
					break;
				case QRErrorCorrectLevel.Q :
					nLimit = QRCodeLimitLength[i][2];
					break;
				case QRErrorCorrectLevel.H :
					nLimit = QRCodeLimitLength[i][3];
					break;
			}
			
			if (length <= nLimit) {
				break;
			} else {
				nType++;
			}
		}
		
		if (nType > QRCodeLimitLength.length) {
			throw new Error("Too long data");
		}
		
		return nType;
	}

	function _getUTF8Length(sText) {
		var replacedText = encodeURI(sText).toString().replace(/\%[0-9a-fA-F]{2}/g, 'a');
		return replacedText.length + (replacedText.length != sText ? 3 : 0);
	}
	
	/**
	 * @class QRCode
	 * @constructor
	 * @example 
	 * new QRCode(document.getElementById("test"), "http://jindo.dev.naver.com/collie");
	 *
	 * @example
	 * var oQRCode = new QRCode("test", {
	 *    text : "http://naver.com",
	 *    width : 128,
	 *    height : 128
	 * });
	 * 
	 * oQRCode.clear(); // Clear the QRCode.
	 * oQRCode.makeCode("http://map.naver.com"); // Re-create the QRCode.
	 *
	 * @param {HTMLElement|String} el target element or 'id' attribute of element.
	 * @param {Object|String} vOption
	 * @param {String} vOption.text QRCode link data
	 * @param {Number} [vOption.width=256]
	 * @param {Number} [vOption.height=256]
	 * @param {String} [vOption.colorDark="#000000"]
	 * @param {String} [vOption.colorLight="#ffffff"]
	 * @param {QRCode.CorrectLevel} [vOption.correctLevel=QRCode.CorrectLevel.H] [L|M|Q|H] 
	 */
	QRCode = function (el, vOption) {
		this._htOption = {
			width : 256, 
			height : 256,
			typeNumber : 4,
			colorDark : "#000000",
			colorLight : "#ffffff",
			correctLevel : QRErrorCorrectLevel.H
		};
		
		if (typeof vOption === 'string') {
			vOption	= {
				text : vOption
			};
		}
		
		// Overwrites options
		if (vOption) {
			for (var i in vOption) {
				this._htOption[i] = vOption[i];
			}
		}
		
		if (typeof el == "string") {
			el = document.getElementById(el);
		}

		if (this._htOption.useSVG) {
			Drawing = svgDrawer;
		}
		
		this._android = _getAndroid();
		this._el = el;
		this._oQRCode = null;
		this._oDrawing = new Drawing(this._el, this._htOption);
		
		if (this._htOption.text) {
			this.makeCode(this._htOption.text);	
		}
	};
	
	/**
	 * Make the QRCode
	 * 
	 * @param {String} sText link data
	 */
	QRCode.prototype.makeCode = function (sText) {
		this._oQRCode = new QRCodeModel(_getTypeNumber(sText, this._htOption.correctLevel), this._htOption.correctLevel);
		this._oQRCode.addData(sText);
		this._oQRCode.make();
		this._el.title = sText;
		this._oDrawing.draw(this._oQRCode);			
		this.makeImage();
	};
	
	/**
	 * Make the Image from Canvas element
	 * - It occurs automatically
	 * - Android below 3 doesn't support Data-URI spec.
	 * 
	 * @private
	 */
	QRCode.prototype.makeImage = function () {
		if (typeof this._oDrawing.makeImage == "function" && (!this._android || this._android >= 3)) {
			this._oDrawing.makeImage();
		}
	};
	
	/**
	 * Clear the QRCode
	 */
	QRCode.prototype.clear = function () {
		this._oDrawing.clear();
	};
	
	/**
	 * @name QRCode.CorrectLevel
	 */
	QRCode.CorrectLevel = QRErrorCorrectLevel;
})();
  </script>

<script>
  // Change this to set your own password for clearing the session log.
  const CLEAR_LOG_PASSWORD = '1234';

  const form = document.getElementById('countForm');
  const tbody = document.querySelector('#logTable tbody');
  const tableWrap = document.getElementById('tableWrap');
  const emptyMsg = document.getElementById('emptyMsg');
  const submitBtn = document.getElementById('submitBtn');
  const cancelEditBtn = document.getElementById('cancelEditBtn');
  const editNotice = document.getElementById('editNotice');
  const csvBtn = document.getElementById('csvBtn');
  const clearLogBtn = document.getElementById('clearLogBtn');
  const printLogBtn = document.getElementById('printLogBtn');
  const selectAllRows = document.getElementById('selectAllRows');
  const printTableBody = document.querySelector('#printTable tbody');
  const printDate = document.getElementById('printDate');
  const printCount = document.getElementById('printCount');
  const qrModal = document.getElementById('qrModal');
  const qrModalCode = document.getElementById('qrModalCode');
  const qrModalText = document.getElementById('qrModalText');
  const qrModalClose = document.getElementById('qrModalClose');
  let entries = [];
  let editingIndex = null;
  let selected = new Set();

  const pad = n => String(n).padStart(2, '0');
  const esc = s => (s || '').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));

  // Builds the human-readable text encoded into each entry's QR code.
  function qrPayload(e){
    return 'TAG NO: ' + e.tagNo + '\n' +
      'DATE: ' + e.year + '-' + pad(e.month) + '-' + pad(e.day) + '\n' +
      'ITEM NO: ' + e.itemNo + '\n' +
      'DETAIL: ' + e.detail + '\n' +
      'LOCATION: ' + e.location + '\n' +
      'QTY: ' + e.qty + ' ' + e.unit + '\n' +
      'COUNTED BY: ' + e.countBy + '\n' +
      'CHECKED BY: ' + e.checkBy;
  }

  // Renders a QR code (SVG) for the given text into container, sized size x size.
  function renderQrInto(container, text, size){
    if (!container) return;
    container.innerHTML = '';
    if (typeof QRCode === 'undefined') {
      container.textContent = 'QR N/A';
      return;
    }
    try {
      new QRCode(container, {
        text: text,
        width: size,
        height: size,
        useSVG: true,
        correctLevel: QRCode.CorrectLevel.M
      });
    } catch (err) {
      container.textContent = 'QR error';
    }
  }

  // Evaluates expressions like "(4x5)+34+56", "10+5-2", "3*4/2".
  // Accepts x, X or × as multiply. Returns null if the string isn't a pure expression.
  function evalExpression(expr){
    if (expr == null) return null;
    let s = String(expr).replace(/[xX×]/g, '*').replace(/\s+/g, '');
    if (!s) return null;
    if (!/^[0-9.+\-*/()]+$/.test(s)) return null;

    let pos = 0;

    function parseExpr(){
      let value = parseTerm();
      while (pos < s.length && (s[pos] === '+' || s[pos] === '-')) {
        const op = s[pos++];
        const rhs = parseTerm();
        value = op === '+' ? value + rhs : value - rhs;
      }
      return value;
    }
    function parseTerm(){
      let value = parseFactor();
      while (pos < s.length && (s[pos] === '*' || s[pos] === '/')) {
        const op = s[pos++];
        const rhs = parseFactor();
        value = op === '*' ? value * rhs : value / rhs;
      }
      return value;
    }
    function parseFactor(){
      if (s[pos] === '(') {
        pos++;
        const value = parseExpr();
        if (s[pos] !== ')') throw new Error('mismatched parentheses');
        pos++;
        return value;
      }
      if (s[pos] === '-') { pos++; return -parseFactor(); }
      if (s[pos] === '+') { pos++; return parseFactor(); }
      const start = pos;
      while (pos < s.length && /[0-9.]/.test(s[pos])) pos++;
      if (start === pos) throw new Error('expected number');
      return parseFloat(s.slice(start, pos));
    }

    try {
      const result = parseExpr();
      if (pos !== s.length || !isFinite(result)) return null;
      return Math.round(result * 100) / 100;
    } catch (e) {
      return null;
    }
  }

  function formatQty(n){
    return String(n);
  }

  const qtyInput = document.getElementById('qty');
  const qtyHint = document.getElementById('qtyHint');
  const detailInput = document.getElementById('detail');
  const detailHint = document.getElementById('detailHint');

  qtyInput.addEventListener('input', () => {
    const val = qtyInput.value;
    const total = evalExpression(val);
    if (total !== null && /[+\-*/xX×]/.test(val)) {
      qtyHint.textContent = '= ' + formatQty(total);
      qtyHint.style.display = 'block';
    } else {
      qtyHint.style.display = 'none';
    }
  });

  qtyInput.addEventListener('blur', () => {
    const total = evalExpression(qtyInput.value);
    if (total !== null) qtyInput.value = formatQty(total);
    qtyHint.style.display = 'none';
  });

  function getQty(){
    const total = evalExpression(qtyInput.value);
    return total !== null ? formatQty(total) : qtyInput.value.trim();
  }

  // Typing a calculation in Detail (e.g. "(4x5)+34+56") fills Qty with the result.
  detailInput.addEventListener('input', () => {
    const val = detailInput.value;
    const result = evalExpression(val);
    if (result !== null && /[+\-*/xX×]/.test(val)) {
      qtyInput.value = formatQty(result);
      qtyHint.style.display = 'none';
      detailHint.textContent = '= ' + formatQty(result) + ' \u2192 Qty';
      detailHint.style.display = 'block';
    } else {
      detailHint.style.display = 'none';
    }
  });

  // prefill today's date
  const today = new Date();
  document.getElementById('year').value = today.getFullYear();
  document.getElementById('month').value = today.getMonth() + 1;
  document.getElementById('day').value = today.getDate();

  function renderLog(){
    tbody.innerHTML = '';
    entries.forEach((e, i) => {
      const tr = document.createElement('tr');
      if (i === editingIndex) tr.classList.add('editing-row');
      const checked = selected.has(i) ? ' checked' : '';
      tr.innerHTML =
        '<td class="select-col"><input type="checkbox" class="row-select" data-i="' + i + '"' + checked + ' aria-label="Select entry ' + (i + 1) + '"></td>' +
        '<td class="qr-col"><div class="qr-thumb" data-i="' + i + '" role="button" tabindex="0" aria-label="View QR code for entry ' + (i + 1) + '"></div></td>' +
        '<td>' + esc(e.tagNo) + '</td>' +
        '<td>' + e.year + '-' + pad(e.month) + '-' + pad(e.day) + '</td>' +
        '<td>' + esc(e.itemNo) + '</td>' +
        '<td>' + esc(e.detail) + '</td>' +
        '<td>' + esc(e.location) + '</td>' +
        '<td>' + esc(e.qty) + '</td>' +
        '<td>' + esc(e.unit) + '</td>' +
        '<td>' + esc(e.countBy) + '</td>' +
        '<td>' + esc(e.checkBy) + '</td>' +
        '<td class="row-actions">' +
          '<button type="button" class="icon-btn edit" data-i="' + i + '" aria-label="Edit entry"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4Z"/></svg></button>' +
          '<button type="button" class="icon-btn del" data-i="' + i + '" aria-label="Remove entry"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/></svg></button>' +
        '</td>';
      tbody.appendChild(tr);
    });
    entries.forEach((e, i) => {
      const thumb = tbody.querySelector('.qr-thumb[data-i="' + i + '"]');
      renderQrInto(thumb, qrPayload(e), 56);
    });
    const has = entries.length > 0;
    tableWrap.style.display = has ? 'block' : 'none';
    emptyMsg.style.display = has ? 'none' : 'block';
    csvBtn.disabled = !has;
    clearLogBtn.disabled = !has;
    selectAllRows.disabled = !has;
    selectAllRows.checked = has && selected.size === entries.length;
    selectAllRows.indeterminate = selected.size > 0 && selected.size < entries.length;
    printLogBtn.disabled = selected.size === 0;
  }

  function fillForm(data){
    document.getElementById('tagNo').value = data.tagNo;
    document.getElementById('year').value = data.year;
    document.getElementById('month').value = data.month;
    document.getElementById('day').value = data.day;
    document.getElementById('itemNo').value = data.itemNo;
    document.getElementById('qty').value = data.qty;
    qtyHint.style.display = 'none';
    document.getElementById('unit').value = data.unit;
    document.getElementById('detail').value = data.detail;
    detailHint.style.display = 'none';
    document.getElementById('location').value = data.location;
    document.getElementById('countBy').value = data.countBy;
    document.getElementById('checkBy').value = data.checkBy;
  }

  function startEdit(i){
    editingIndex = i;
    fillForm(entries[i]);
    submitBtn.textContent = 'Update count';
    cancelEditBtn.style.display = '';
    editNotice.style.display = '';
    editNotice.textContent = 'Editing entry ' + (i + 1) + ' — Tag No ' + entries[i].tagNo + '. Save to apply changes, or cancel to leave it unchanged.';
    renderLog();
    document.getElementById('itemNo').focus();
  }

  function exitEdit(){
    editingIndex = null;
    submitBtn.textContent = 'Save count';
    cancelEditBtn.style.display = 'none';
    editNotice.style.display = 'none';
    renderLog();
  }

  function resetItemFields(){
    ['itemNo', 'qty', 'detail', 'location'].forEach(id => {
      document.getElementById(id).value = '';
    });
    document.getElementById('unit').value = 'PCS';
    qtyHint.style.display = 'none';
    detailHint.style.display = 'none';
  }

  form.addEventListener('submit', e => {
    e.preventDefault();
    const data = {
      tagNo: document.getElementById('tagNo').value.trim(),
      year: document.getElementById('year').value,
      month: document.getElementById('month').value,
      day: document.getElementById('day').value,
      itemNo: document.getElementById('itemNo').value.trim(),
      qty: getQty(),
      unit: document.getElementById('unit').value.trim(),
      detail: document.getElementById('detail').value.trim(),
      location: document.getElementById('location').value.trim(),
      countBy: document.getElementById('countBy').value.trim(),
      checkBy: document.getElementById('checkBy').value.trim()
    };

    if (editingIndex !== null) {
      entries[editingIndex] = data;
      exitEdit();
      resetItemFields();
      document.getElementById('itemNo').focus();
      return;
    }

    entries.push(data);
    renderLog();

    // auto-increment tag no if purely numeric
    const tn = document.getElementById('tagNo');
    if (/^\d+$/.test(tn.value)) {
      const next = (parseInt(tn.value, 10) + 1).toString().padStart(tn.value.length, '0');
      tn.value = next;
    }

    // clear item-specific fields for the next tag, keep date / names
    resetItemFields();
    document.getElementById('itemNo').focus();
  });

  cancelEditBtn.addEventListener('click', () => {
    exitEdit();
    resetItemFields();
  });

  document.getElementById('clearBtn').addEventListener('click', () => {
    form.reset();
    document.getElementById('year').value = today.getFullYear();
    document.getElementById('month').value = today.getMonth() + 1;
    document.getElementById('day').value = today.getDate();
    document.getElementById('unit').value = 'PCS';
    qtyHint.style.display = 'none';
    detailHint.style.display = 'none';
    exitEdit();
  });

  document.getElementById('printBtn').addEventListener('click', () => window.print());

  document.getElementById('csvBtn').addEventListener('click', () => {
    if (!entries.length) return;
    const headers = ['Tag No','Date','Item No','Detail','Location','Qty','Unit','Counted By','Checked By'];
    const rows = entries.map(e => [
      e.tagNo, e.year + '-' + pad(e.month) + '-' + pad(e.day),
      e.itemNo, e.detail, e.location, e.qty, e.unit, e.countBy, e.checkBy
    ]);
    const csv = [headers, ...rows]
      .map(r => r.map(v => '"' + String(v).replace(/"/g, '""') + '"').join(','))
      .join('\n');
    const blob = new Blob([csv], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'inventory-count.csv';
    document.body.appendChild(a);
    a.click();
    a.remove();
    URL.revokeObjectURL(url);
  });

  tbody.addEventListener('click', e => {
    const btn = e.target.closest('button');
    if (!btn) return;
    const i = +btn.dataset.i;

    if (btn.classList.contains('edit')) {
      startEdit(i);
      return;
    }

    if (btn.classList.contains('del')) {
      entries.splice(i, 1);
      if (editingIndex !== null) {
        if (i === editingIndex) {
          exitEdit();
          resetItemFields();
        } else if (i < editingIndex) {
          editingIndex--;
        }
      }
      const shifted = new Set();
      selected.forEach(idx => {
        if (idx === i) return;
        shifted.add(idx > i ? idx - 1 : idx);
      });
      selected = shifted;
      renderLog();
    }
  });

  tbody.addEventListener('change', e => {
    if (!e.target.classList.contains('row-select')) return;
    const i = +e.target.dataset.i;
    if (e.target.checked) selected.add(i); else selected.delete(i);
    selectAllRows.checked = entries.length > 0 && selected.size === entries.length;
    selectAllRows.indeterminate = selected.size > 0 && selected.size < entries.length;
    printLogBtn.disabled = selected.size === 0;
  });

  selectAllRows.addEventListener('change', () => {
    if (selectAllRows.checked) {
      entries.forEach((_, i) => selected.add(i));
    } else {
      selected.clear();
    }
    renderLog();
  });

  // ---------- QR code detail modal ----------
  function openQrModal(i){
    const e = entries[i];
    if (!e) return;
    const text = qrPayload(e);
    renderQrInto(qrModalCode, text, 220);
    qrModalText.textContent = text;
    qrModal.classList.add('open');
    qrModalClose.focus();
  }

  function closeQrModal(){
    qrModal.classList.remove('open');
  }

  tbody.addEventListener('click', e => {
    const thumb = e.target.closest('.qr-thumb');
    if (thumb) openQrModal(+thumb.dataset.i);
  });

  tbody.addEventListener('keydown', e => {
    if (e.key !== 'Enter' && e.key !== ' ') return;
    const thumb = e.target.closest('.qr-thumb');
    if (!thumb) return;
    e.preventDefault();
    openQrModal(+thumb.dataset.i);
  });

  qrModalClose.addEventListener('click', closeQrModal);
  qrModal.addEventListener('click', e => {
    if (e.target === qrModal) closeQrModal();
  });

  // ---------- Clear log (password-protected) ----------
  const clearModal = document.getElementById('clearModal');
  const clearPassword = document.getElementById('clearPassword');
  const clearError = document.getElementById('clearError');
  const clearModalText = document.getElementById('clearModalText');
  const clearModalCancel = document.getElementById('clearModalCancel');
  const clearModalConfirm = document.getElementById('clearModalConfirm');

  function openClearModal(){
    if (!entries.length) return;
    const count = entries.length;
    clearModalText.textContent = 'This will permanently remove all ' + count + (count === 1 ? ' entry' : ' entries') + ' recorded in this session. Enter the password to continue.';
    clearError.style.display = 'none';
    clearPassword.value = '';
    clearModal.classList.add('open');
    clearPassword.focus();
  }

  function closeClearModal(){
    clearModal.classList.remove('open');
  }

  function attemptClear(){
    if (clearPassword.value === CLEAR_LOG_PASSWORD) {
      entries = [];
      selected = new Set();
      if (editingIndex !== null) {
        exitEdit();
        resetItemFields();
      }
      renderLog();
      closeClearModal();
    } else {
      clearError.style.display = 'block';
      clearPassword.value = '';
      clearPassword.focus();
    }
  }

  clearLogBtn.addEventListener('click', openClearModal);
  clearModalCancel.addEventListener('click', closeClearModal);
  clearModalConfirm.addEventListener('click', attemptClear);

  clearModal.addEventListener('click', e => {
    if (e.target === clearModal) closeClearModal();
  });

  clearPassword.addEventListener('keydown', e => {
    if (e.key === 'Enter') { e.preventDefault(); attemptClear(); }
  });
  clearPassword.addEventListener('input', () => {
    clearError.style.display = 'none';
  });

  document.addEventListener('keydown', e => {
    if (e.key !== 'Escape') return;
    if (clearModal.classList.contains('open')) closeClearModal();
    if (qrModal.classList.contains('open')) closeQrModal();
  });

  // ---------- Print selected log entries ----------
  printLogBtn.addEventListener('click', () => {
    if (!selected.size) return;
    const rows = Array.from(selected).sort((a, b) => a - b).map(i => entries[i]);

    printTableBody.innerHTML = rows.map((e, idx) =>
      '<tr>' +
        '<td class="qr-col"><div class="qr-print" data-pi="' + idx + '"></div></td>' +
        '<td>' + esc(e.tagNo) + '</td>' +
        '<td>' + e.year + '-' + pad(e.month) + '-' + pad(e.day) + '</td>' +
        '<td>' + esc(e.itemNo) + '</td>' +
        '<td>' + esc(e.detail) + '</td>' +
        '<td>' + esc(e.location) + '</td>' +
        '<td>' + esc(e.qty) + '</td>' +
        '<td>' + esc(e.unit) + '</td>' +
        '<td>' + esc(e.countBy) + '</td>' +
        '<td>' + esc(e.checkBy) + '</td>' +
      '</tr>'
    ).join('');

    rows.forEach((e, idx) => {
      const cell = printTableBody.querySelector('.qr-print[data-pi="' + idx + '"]');
      renderQrInto(cell, qrPayload(e), 80);
    });

    printDate.textContent = 'Printed ' + new Date().toLocaleString();
    printCount.textContent = rows.length + (rows.length === 1 ? ' entry selected' : ' entries selected');

    document.body.classList.add('printing-log');
    window.print();
  });

  window.addEventListener('afterprint', () => {
    document.body.classList.remove('printing-log');
  });

  renderLog();
</script>

</body>
</html>
