<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>開刀房財產盤點</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.x/dist/tabler-icons.min.css">
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<script type="module">
import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js';
import { getFirestore, collection, addDoc, getDocs, query, orderBy } from 'https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js';
const firebaseConfig = {
  apiKey:"AIzaSyBf7Of7ZQW3o_J3P0CXwiOCeqRsqqe-I4c",
  authDomain:"or-checklist-9208e.firebaseapp.com",
  projectId:"or-checklist-9208e",
  storageBucket:"or-checklist-9208e.firebasestorage.app",
  messagingSenderId:"698322497360",
  appId:"1:698322497360:web:1c157351862511a5e40655"
};
const app = initializeApp(firebaseConfig);
const db  = getFirestore(app);
window._db=db; window._fsAdd=addDoc; window._fsColl=collection;
window._fsQuery=query; window._fsGetDoc=getDocs; window._fsOrder=orderBy;
window._firebaseReady=true;
window.dispatchEvent(new Event('firebase-ready'));
</script>
<style>
:root{
  --blue:#1e4d8c;--blue-dark:#132f56;--blue-light:#2563eb;
  --green:#16a34a;--amber:#d97706;--red:#dc2626;
  --bg:#f1f5f9;--card:#fff;--border:#e2e8f0;
  --text:#0f172a;--text2:#475569;--text3:#94a3b8;
  --radius:14px;--shadow:0 1px 3px rgba(0,0,0,.08),0 4px 16px rgba(0,0,0,.06);
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:var(--bg);min-height:100vh;color:var(--text)}
.app{max-width:430px;margin:0 auto;min-height:100vh;display:flex;flex-direction:column}
 
/* TopBar */
.topbar{background:linear-gradient(135deg,var(--blue-dark),var(--blue));color:#fff;padding:14px 18px 12px;display:flex;align-items:center;justify-content:space-between;flex-shrink:0}
.brand{display:flex;align-items:center;gap:10px}
.brand-icon{width:36px;height:36px;background:rgba(255,255,255,.15);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:18px}
.brand-t1{font-size:15px;font-weight:700}
.brand-t2{font-size:11px;opacity:.65;margin-top:1px}
.db-dot{width:8px;height:8px;border-radius:50%;background:#94a3b8;display:inline-block;transition:background .3s}
.db-dot.ok{background:#4ade80}
 
/* Tabs */
.tab-bar{display:flex;background:var(--card);border-bottom:1px solid var(--border);flex-shrink:0}
.tab{flex:1;padding:11px 4px 9px;text-align:center;font-size:11px;color:var(--text3);cursor:pointer;border-bottom:2.5px solid transparent;font-weight:600;display:flex;flex-direction:column;align-items:center;gap:3px}
.tab i{font-size:18px}
.tab.on{color:var(--blue);border-bottom-color:var(--blue)}
.page{display:none;padding:14px;overflow-y:auto;flex:1}
.page.on{display:block}
 
/* Upload */
.upload-hero{text-align:center;padding:24px 12px 16px}
.upload-hero-icon{width:68px;height:68px;background:linear-gradient(135deg,#dbeafe,#bfdbfe);border-radius:20px;display:flex;align-items:center;justify-content:center;font-size:34px;margin:0 auto 12px;color:var(--blue)}
.upload-hero h2{font-size:18px;font-weight:700;margin-bottom:6px}
.upload-hero p{font-size:13px;color:var(--text2);line-height:1.6}
.upload-zone{border:2px dashed #cbd5e1;border-radius:var(--radius);padding:24px 16px;text-align:center;cursor:pointer;background:var(--card);margin-bottom:10px;transition:all .2s}
.upload-zone:active{border-color:var(--blue);background:#eff6ff}
.upload-zone input{display:none}
.upload-result{background:linear-gradient(135deg,#f0fdf4,#dcfce7);border:1px solid #86efac;border-radius:var(--radius);padding:13px 16px;margin-bottom:12px;display:none;align-items:center;gap:10px}
.upload-result.show{display:flex}
.ur-icon{width:34px;height:34px;background:#16a34a;border-radius:9px;display:flex;align-items:center;justify-content:center;color:#fff;font-size:17px;flex-shrink:0}
.ur-t1{font-size:13px;font-weight:700;color:#14532d;margin-bottom:2px}
.ur-t2{font-size:11px;color:#166534;opacity:.85}
.start-btn{width:100%;padding:14px;background:linear-gradient(135deg,var(--blue),var(--blue-dark));color:#fff;border:none;border-radius:var(--radius);font-size:16px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px;box-shadow:0 4px 14px rgba(30,77,140,.3)}
.start-btn:disabled{opacity:.35;cursor:not-allowed;box-shadow:none}
 
/* Nurse picker */
.nurse-section{display:none;margin-bottom:12px}
.nurse-label{font-size:13px;font-weight:700;color:var(--text2);margin-bottom:8px;display:flex;align-items:center;gap:6px}
.nurse-search{width:100%;padding:10px 12px 10px 34px;border:1.5px solid var(--border);border-radius:10px;font-size:13px;background:var(--card);margin-bottom:8px}
.nurse-search-wrap{position:relative;margin-bottom:8px}
.nurse-search-wrap i{position:absolute;left:11px;top:50%;transform:translateY(-50%);color:var(--text3);font-size:15px;pointer-events:none}
.nurse-grid{display:grid;grid-template-columns:1fr 1fr;gap:7px;max-height:280px;overflow-y:auto}
.nurse-card{padding:11px 10px;background:var(--card);border:1.5px solid var(--border);border-radius:10px;cursor:pointer;text-align:center;font-size:13px;font-weight:600;color:var(--text);box-shadow:var(--shadow);transition:all .15s}
.nurse-card.on{border-color:var(--blue);background:#eff6ff;color:var(--blue)}
.nurse-card-sub{font-size:10px;color:var(--text3);margin-top:2px;font-weight:400}
.nurse-card.on .nurse-card-sub{color:var(--blue);opacity:.7}
 
/* Checklist */
.toolbar{display:flex;gap:8px;margin-bottom:10px;align-items:center}
.nurse-info{flex:1;background:var(--card);border-radius:10px;padding:9px 13px;box-shadow:var(--shadow);display:flex;align-items:center;gap:8px;overflow:hidden}
.nurse-info i{font-size:17px;color:var(--blue);flex-shrink:0}
.nurse-info-name{font-size:13px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.nurse-info-cnt{font-size:11px;color:var(--text3);flex-shrink:0}
.change-btn{padding:9px 11px;border:1.5px solid var(--border);border-radius:10px;background:var(--card);cursor:pointer;font-size:12px;color:var(--text2);box-shadow:var(--shadow);white-space:nowrap;display:flex;align-items:center;gap:4px}
 
/* Search */
.search-wrap{position:relative;margin-bottom:10px}
.search-wrap i{position:absolute;left:12px;top:50%;transform:translateY(-50%);color:var(--text3);font-size:16px;pointer-events:none}
.search-box{width:100%;padding:10px 12px 10px 36px;border:1.5px solid var(--border);border-radius:10px;font-size:14px;background:var(--card);color:var(--text);box-shadow:var(--shadow)}
.search-box:focus{outline:none;border-color:var(--blue)}
 
/* Progress */
.prog-card{background:var(--card);border-radius:var(--radius);padding:13px 16px;margin-bottom:10px;box-shadow:var(--shadow)}
.prog-top{display:flex;justify-content:space-between;align-items:flex-end;margin-bottom:8px}
.prog-num{font-size:26px;font-weight:800;color:var(--blue);line-height:1}
.prog-sub{font-size:12px;color:var(--text3);margin-bottom:2px}
.prog-pct{font-size:13px;font-weight:700;color:var(--blue)}
.prog-bar{height:7px;background:#e2e8f0;border-radius:99px;overflow:hidden}
.prog-fill{height:7px;background:linear-gradient(90deg,var(--blue-light),var(--blue));border-radius:99px;transition:width .4s}
.anom-pill{display:inline-flex;align-items:center;gap:4px;background:#fef3c7;color:#92400e;font-size:11px;padding:2px 8px;border-radius:99px;font-weight:600;margin-top:6px}
 
/* Items */
.item-card{background:var(--card);border-radius:12px;margin-bottom:7px;overflow:hidden;box-shadow:var(--shadow);border:1.5px solid transparent;transition:border-color .2s}
.item-card.checked{border-color:#bbf7d0}
.item-inner{display:flex;align-items:flex-start;padding:12px 13px;gap:11px}
.chk{width:24px;height:24px;border-radius:50%;border:2px solid #cbd5e1;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:all .18s;color:transparent;font-size:13px;cursor:pointer;margin-top:1px}
.chk.on{background:var(--green);border-color:var(--green);color:#fff;box-shadow:0 2px 8px rgba(22,163,74,.28)}
.item-body{flex:1;min-width:0;cursor:pointer}
.item-name{font-size:14px;font-weight:600;color:var(--text);line-height:1.3}
.item-name.done{color:var(--text3);text-decoration:line-through}
.item-code{font-size:11px;color:var(--text3);margin-top:3px;font-family:monospace;letter-spacing:.3px}
.item-loc{font-size:11px;color:#0ea5e9;background:#f0f9ff;border:1px solid #bae6fd;border-radius:6px;padding:2px 7px;margin-top:4px;display:inline-block;max-width:100%;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.item-anom{font-size:11px;color:var(--amber);background:#fffbeb;border:1px solid #fde68a;border-radius:6px;padding:2px 7px;margin-top:3px;display:inline-block}
.item-btns{display:flex;flex-direction:column;gap:5px;flex-shrink:0}
.ibtn{width:28px;height:28px;border-radius:7px;border:1.5px solid var(--border);display:flex;align-items:center;justify-content:center;cursor:pointer;font-size:14px;color:var(--text3);background:transparent}
.ibtn.loc{color:#0ea5e9;border-color:#bae6fd;background:#f0f9ff}
.ibtn.anom{color:var(--amber);border-color:#fbbf24;background:#fffbeb}
.ibtn.keeper{color:#7c3aed;border-color:#c4b5fd;background:#f5f3ff}
.item-keeper{font-size:11px;color:#7c3aed;background:#f5f3ff;border:1px solid #c4b5fd;border-radius:6px;padding:2px 7px;margin-top:3px;display:inline-block}
 
/* Footer */
.footer-pad{padding:10px 0 24px}
.sig-card{background:var(--card);border-radius:var(--radius);padding:13px 16px;margin-bottom:10px;box-shadow:var(--shadow);border:1.5px dashed var(--border);cursor:pointer;display:flex;align-items:center;gap:11px}
.sig-card.done{border-style:solid;border-color:#86efac;background:#f0fdf4}
.sig-av{width:34px;height:34px;border-radius:9px;background:#f1f5f9;display:flex;align-items:center;justify-content:center;font-size:17px;color:var(--text3);flex-shrink:0}
.sig-card.done .sig-av{background:#dcfce7;color:var(--green)}
.sig-t1{font-size:13px;font-weight:600}
.sig-t2{font-size:11px;color:var(--text3);margin-top:1px}
.sub-btn{width:100%;padding:14px;background:linear-gradient(135deg,var(--blue),var(--blue-dark));color:#fff;border:none;border-radius:var(--radius);font-size:15px;font-weight:700;cursor:pointer;box-shadow:0 4px 14px rgba(30,77,140,.3)}
.sub-btn:disabled{opacity:.35;cursor:not-allowed;box-shadow:none}
.saving-bar{background:#eff6ff;border:1px solid #bfdbfe;border-radius:10px;padding:9px 13px;font-size:12px;color:var(--blue);display:none;align-items:center;gap:8px;margin-bottom:9px}
.saving-bar.show{display:flex}
@keyframes spin{to{transform:rotate(360deg)}}
.spinner{width:13px;height:13px;border:2px solid #bfdbfe;border-top-color:var(--blue);border-radius:50%;animation:spin .7s linear infinite;flex-shrink:0}
 
/* Modal */
.modal-bg{position:fixed;inset:0;background:rgba(15,23,42,.55);display:none;align-items:flex-end;justify-content:center;z-index:200;backdrop-filter:blur(3px)}
.modal-bg.show{display:flex}
.modal{background:var(--card);border-radius:20px 20px 0 0;padding:18px 18px 36px;width:100%;max-width:430px;animation:slideUp .22s ease}
@keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
.modal-handle{width:38px;height:4px;background:#e2e8f0;border-radius:2px;margin:0 auto 14px}
.modal-title{font-size:15px;font-weight:700;margin-bottom:4px}
.modal-sub{font-size:12px;color:var(--text3);margin-bottom:10px}
.modal textarea{width:100%;border:1.5px solid var(--border);border-radius:11px;padding:11px;font-size:14px;resize:none;height:82px;font-family:inherit;background:#f8fafc;color:var(--text)}
.modal textarea:focus{outline:none;border-color:var(--blue)}
.modal-btns{display:flex;gap:8px;margin-top:10px}
.modal-btns button{flex:1;padding:11px;border-radius:11px;border:1.5px solid var(--border);font-size:14px;cursor:pointer;background:var(--card);font-weight:600;color:var(--text)}
.modal-btns .pri{background:var(--blue);color:#fff;border-color:var(--blue)}
 
/* Done */
.done-wrap{display:none;flex-direction:column;align-items:center;padding:40px 20px 24px;text-align:center;gap:13px}
.done-wrap.show{display:flex}
.done-icon{width:72px;height:72px;border-radius:50%;background:linear-gradient(135deg,#dcfce7,#bbf7d0);display:flex;align-items:center;justify-content:center;font-size:38px;animation:popIn .4s cubic-bezier(.34,1.56,.64,1)}
@keyframes popIn{from{transform:scale(0);opacity:0}to{transform:scale(1);opacity:1}}
.done-title{font-size:20px;font-weight:800}
.done-sub{font-size:13px;color:var(--text2);line-height:1.8;white-space:pre-line;background:var(--card);border-radius:var(--radius);padding:13px 17px;width:100%;text-align:left;box-shadow:var(--shadow)}
.dl-btn{width:100%;padding:14px;background:linear-gradient(135deg,#16a34a,#15803d);color:#fff;border:none;border-radius:var(--radius);font-size:15px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px;box-shadow:0 4px 14px rgba(22,163,74,.3)}
.reset-btn{width:100%;padding:12px;background:var(--card);color:var(--text2);border:1.5px solid var(--border);border-radius:var(--radius);font-size:14px;font-weight:600;cursor:pointer}
 
/* History */
.hist-empty{padding:48px 20px;text-align:center;color:var(--text3)}
.hist-empty i{font-size:40px;display:block;margin-bottom:10px;opacity:.35}
.hist-card{background:var(--card);border-radius:var(--radius);padding:15px 16px;margin-bottom:9px;box-shadow:var(--shadow);border-left:4px solid var(--border)}
.hist-card.ok{border-left-color:var(--green)}
.hist-card.warn{border-left-color:var(--amber)}
.hist-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px}
.hist-date{font-size:13px;font-weight:700}
.hist-area{font-size:11px;color:var(--text3);margin-top:2px}
.pill{font-size:11px;padding:3px 9px;border-radius:99px;font-weight:700}
.pill.ok{background:#dcfce7;color:#166534}
.pill.warn{background:#fef3c7;color:#92400e}
.hist-meta{font-size:12px;color:var(--text2);margin-bottom:5px;display:flex;align-items:center;gap:7px}
.hist-anoms{font-size:12px;color:var(--amber);margin-bottom:7px;line-height:1.5}
.hist-dl{padding:7px 12px;border-radius:8px;border:1.5px solid var(--border);background:#f8fafc;font-size:12px;cursor:pointer;color:var(--text2);font-weight:600;display:inline-flex;align-items:center;gap:5px}
 
/* Empty search */
.empty-search{padding:32px;text-align:center;color:var(--text3);font-size:13px}
.empty-search i{font-size:32px;display:block;margin-bottom:8px;opacity:.35}
</style>
</head>
<body>
<div class="app">
 
<!-- TopBar -->
<div class="topbar">
  <div class="brand">
    <div class="brand-icon">🏥</div>
    <div>
      <div class="brand-t1">開刀房財產盤點</div>
      <div class="brand-t2" id="topdate"></div>
    </div>
  </div>
  <span class="db-dot" id="db-dot" title="資料庫狀態"></span>
</div>
 
<!-- Tabs -->
<div class="tab-bar">
  <div class="tab on" onclick="showTab('check')"><i class="ti ti-clipboard-check"></i>盤點清單</div>
  <div class="tab" onclick="showTab('hist')"><i class="ti ti-history"></i>歷史記錄</div>
</div>
 
<!-- 盤點頁 -->
<div id="pg-check" class="page on">
 
 
 
  <!-- 位置 Modal -->
  <div id="modal-loc" class="modal-bg">
    <div class="modal">
      <div class="modal-handle"></div>
      <div class="modal-title" id="loc-title">📍 位置備註</div>
      <div class="modal-sub">記錄此財產放置位置，永久保存不會消失</div>
      <textarea id="loc-ta" placeholder="例如：手術室A 第3層架、護理站抽屜2…"></textarea>
      <div class="modal-btns">
        <button onclick="closeModal('loc')">取消</button>
        <button class="pri" onclick="saveLoc()">儲存位置</button>
      </div>
    </div>
  </div>
 
  <!-- 保管人 Modal -->
  <div id="modal-keeper" class="modal-bg">
    <div class="modal">
      <div class="modal-handle"></div>
      <div class="modal-title" id="keeper-title">👤 保管人</div>
      <div class="modal-sub">記錄此財產的負責保管人，永久保存</div>
      <textarea id="keeper-ta" placeholder="例如：王小明護理師、李護理長…" style="height:64px"></textarea>
      <div class="modal-btns">
        <button onclick="closeModal('keeper')">取消</button>
        <button class="pri" onclick="saveKeeper()">儲存</button>
      </div>
    </div>
  </div>
 
  <!-- 上傳區 -->
  <div id="upload-section">
    <div class="upload-hero">
      <div class="upload-hero-icon"><i class="ti ti-building-hospital"></i></div>
      <h2>財產盤點系統</h2>
      <p>上傳財產報表後選擇護理師<br>即可開始逐項勾選盤點</p>
    </div>
    <div class="upload-zone" onclick="document.getElementById('file-inp').click()">
      <input type="file" id="file-inp" accept=".xls,.xlsx" onchange="handleFile(this.files[0])">
      <i class="ti ti-file-spreadsheet" style="font-size:30px;color:#94a3b8;display:block;margin-bottom:7px"></i>
      <div style="font-size:14px;font-weight:600;color:var(--text2);margin-bottom:3px">點此上傳財產報表</div>
      <div style="font-size:12px;color:var(--text3)">支援 .xls / .xlsx</div>
    </div>
    <div class="upload-result" id="upload-result">
      <div class="ur-icon"><i class="ti ti-check"></i></div>
      <div><div class="ur-t1" id="ur-t1"></div><div class="ur-t2" id="ur-t2"></div></div>
    </div>
    <button class="start-btn" id="start-btn" disabled onclick="startChecklist()">
      <i class="ti ti-clipboard-check" style="font-size:18px"></i>開始盤點
    </button>
  </div>
 
  <!-- 盤點主畫面 -->
  <div id="main-area" style="display:none">
    <div class="toolbar">
      <div class="nurse-info">
        <i class="ti ti-clipboard-list"></i>
        <span class="nurse-info-name">財產盤點清單</span>
        <span class="nurse-info-cnt" id="nurse-cnt-display"></span>
      </div>
      <button class="change-btn" id="save-btn" onclick="saveProgress()" style="background:#16a34a;color:#fff;border-color:#16a34a">
        <i class="ti ti-device-floppy" style="font-size:13px"></i>儲存
      </button>
      <button class="change-btn" onclick="showUpload()"><i class="ti ti-refresh" style="font-size:13px"></i>換報表</button>
    </div>
 
    <!-- 搜尋框 -->
    <div class="search-wrap">
      <i class="ti ti-search"></i>
      <input class="search-box" id="search-box" placeholder="搜尋品項名稱或財產編號…" oninput="filterItems()">
    </div>
 
    <!-- 進度 -->
    <div class="prog-card">
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;margin-bottom:10px">
        <div style="text-align:center;padding:8px 4px;background:#f0fdf4;border-radius:10px;border:1px solid #bbf7d0">
          <div style="font-size:22px;font-weight:800;color:var(--green)" id="prog-done">0</div>
          <div style="font-size:11px;color:#166534;font-weight:600;margin-top:2px">已盤點</div>
        </div>
        <div style="text-align:center;padding:8px 4px;background:#fef2f2;border-radius:10px;border:1px solid #fecaca">
          <div style="font-size:22px;font-weight:800;color:var(--red)" id="prog-undone">0</div>
          <div style="font-size:11px;color:#991b1b;font-weight:600;margin-top:2px">未盤點</div>
        </div>
        <div style="text-align:center;padding:8px 4px;background:#eff6ff;border-radius:10px;border:1px solid #bfdbfe">
          <div style="font-size:22px;font-weight:800;color:var(--blue)" id="prog-pct">0%</div>
          <div style="font-size:11px;color:#1e40af;font-weight:600;margin-top:2px">完成率</div>
        </div>
      </div>
      <div class="prog-bar"><div class="prog-fill" id="prog-fill" style="width:0%"></div></div>
      <div id="anom-ct"></div>
    </div>
 
    <!-- 清單 -->
    <div id="item-list"></div>
 
    <!-- 送出 -->
    <div class="footer-pad">
 
      <div class="saving-bar" id="saving-bar"><div class="spinner"></div>正在儲存至雲端…</div>
      <button class="sub-btn" id="sub-btn" disabled onclick="doSubmit()">
        <i class="ti ti-send" style="font-size:16px"></i>送出並下載 Excel
      </button>
    </div>
  </div>
 
  <!-- 完成畫面 -->
  <div id="done-wrap" class="done-wrap">
    <div class="done-icon">✓</div>
    <div class="done-title" id="done-title">盤點完成</div>
    <div class="done-sub" id="done-sub"></div>
    <button class="dl-btn" onclick="downloadExcel()">
      <i class="ti ti-file-spreadsheet" style="font-size:17px"></i>下載 Excel 盤點記錄
    </button>
    <button class="reset-btn" onclick="resetApp()">重新盤點 / 換報表</button>
  </div>
</div>
 
<!-- 歷史頁 -->
<div id="pg-hist" class="page">
  <div id="hist-list"></div>
</div>
 
</div>
 
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<script>
// ── 狀態 ─────────────────────────────────────────
let NURSES={}, RAW={};
let allNurseNames=[], currentNurse='';
let state={}, searchQ='';
let currentLocId=null;
let sigName='', sigDone=false;
let LOCATION_NOTES={};
let KEEPER_NOTES={};
let history=[], lastRecord=null;
 
// ── Firebase ──────────────────────────────────────
function checkDb(){
  const dot=document.getElementById('db-dot');
  if(window._firebaseReady){dot.className='db-dot ok';dot.title='雲端已連線';}
}
setTimeout(checkDb,2500);
 
async function saveToFirebase(rec){
  if(!window._firebaseReady||!window._db)return false;
  try{
    await window._fsAdd(window._fsColl(window._db,'checklist_records'),{
      time:rec.time, area:rec.area, by:rec.by,
      total:rec.items.length,
      checked:rec.items.filter(x=>x.checked).length,
      anoms:rec.anoms, anomDetails:rec.anomDetails,
      deviceId:getDeviceId(), createdAt:new Date()
    });
    return true;
  }catch(e){console.error(e);return false;}
}
function getDeviceId(){
  let id=localStorage.getItem('or_device_id');
  if(!id){id='dev_'+Math.random().toString(36).slice(2,10);localStorage.setItem('or_device_id',id);}
  return id;
}
 
// ── 上傳報表 ──────────────────────────────────────
function handleFile(file){
  if(!file)return;
  const reader=new FileReader();
  reader.onload=e=>{
    try{
      const wb=XLSX.read(e.target.result,{type:'array'});
      const ws=wb.Sheets[wb.SheetNames[0]];
      const rows=XLSX.utils.sheet_to_json(ws,{defval:''});
      const sample=rows[0]||{}, keys=Object.keys(sample);
      const col=n=>keys.find(k=>k.includes(n))||'';
      const cCode=col('財產編號'), cName=col('名稱'), cSeq=col('財產序號')||col('序號')||'';
      const cUnit=col('財產單位')||col('單位'), cLoc=col('置放地點')||col('地點'), cKeeper=col('保管人');
      if(!cCode||!cName){alert('找不到「財產編號」或「名稱」欄位');return;}
 
      const nurses={};
      rows.forEach(r=>{
        const name=(r[cName]||'').toString().trim();
        const code=(r[cCode]||'').toString().trim();
        const unit=(r[cUnit]||'').toString().trim();
        const loc=(r[cLoc]||'未分類').toString().trim();
        const keeper=(r[cKeeper]||'').toString().trim()||loc;
        const seq=cSeq?(r[cSeq]||'').toString().trim().padStart(6,'0'):'';
        if(!name||!code)return;
        if(!nurses[keeper])nurses[keeper]=[];
        nurses[keeper].push({name,code,seq,unit,loc,fullCode:seq?code+'-'+seq:code});
      });
 
      NURSES=nurses; RAW=nurses;
      allNurseNames=Object.keys(nurses).sort();
      window._lastFileName=file.name;
      saveToLocal();
 
      const ti=Object.values(nurses).reduce((s,a)=>s+a.length,0);
      document.getElementById('ur-t1').textContent='✓ 成功載入「'+file.name+'」';
      document.getElementById('ur-t2').textContent='共 '+allNurseNames.length+' 位保管人，'+ti+' 件財產';
      document.getElementById('upload-result').classList.add('show');
      document.getElementById('start-btn').disabled=false;
    }catch(err){alert('讀取失敗：'+err.message);}
  };
  reader.readAsArrayBuffer(file);
}
 
function renderNurseGrid(names){
  const grid=document.getElementById('nurse-grid');
  grid.innerHTML=names.map(n=>`
    <div class="nurse-card${n===currentNurse?' on':''}" onclick="selectNurse('${n.replace(/'/g,"\\'")}',this)">
      <i class="ti ti-user" style="font-size:15px;display:block;margin-bottom:3px;color:var(--text3)"></i>
      ${n}
      <div class="nurse-card-sub">${(NURSES[n]||[]).length} 件</div>
    </div>`).join('');
}
function filterNurses(){
  const q=document.getElementById('nurse-search').value.toLowerCase();
  renderNurseGrid(allNurseNames.filter(n=>n.toLowerCase().includes(q)));
}
function selectNurse(name,el){
  currentNurse=name;
  document.querySelectorAll('.nurse-card').forEach(d=>d.classList.remove('on'));
  if(el)el.classList.add('on');
  const btn=document.getElementById('start-btn');
  btn.disabled=false;
  btn.innerHTML='<i class="ti ti-clipboard-check" style="font-size:18px"></i>開始盤點 — '+name;
}
 
function startChecklist(){
  // 把所有品項合併為一個清單
  state={}; searchQ=''; currentNurse='__ALL__'; sigName='盤點'; sigDone=true;
  // 合併全部護理師品項
  const allItems=[];
  allNurseNames.forEach(n=>(NURSES[n]||[]).forEach(it=>allItems.push(it)));
  NURSES['__ALL__']=allItems;
  const total=allItems.length;
  document.getElementById('nurse-cnt-display').textContent=total+' 件';
  document.getElementById('upload-section').style.display='none';
  document.getElementById('main-area').style.display='block';
  document.getElementById('done-wrap').classList.remove('show');
  initState();
  // 嘗試讀取上次儲存的進度
  const savedTime = loadProgress();
  if(savedTime){
    if(confirm('發現上次未完成的盤點記錄（'+savedTime+'），是否繼續上次進度？')){
      // 進度已在 loadProgress 裡載入到 state
    } else {
      state[currentNurse]=null;
      clearProgress();
      initState();
    }
  }
  renderList(); updateProgress();
}
 
function showUpload(){
  document.getElementById('main-area').style.display='none';
  document.getElementById('upload-section').style.display='block';
  currentNurse='';
  const btn=document.getElementById('start-btn');
  btn.disabled=allNurseNames.length===0;
  btn.innerHTML='<i class="ti ti-clipboard-check" style="font-size:18px"></i>開始盤點';
}
 
// ── 狀態管理 ──────────────────────────────────────
function initState(){
  if(!state[currentNurse]){
    state[currentNurse]=(NURSES[currentNurse]||[]).map((it,i)=>({
      id:currentNurse+'_'+i,
      name:it.name, code:it.code, seq:it.seq||'',
      fullCode:it.fullCode||it.code, unit:it.unit,
      checked:false, scanTime:''
    }));
  }
}
function allItems(){return state[currentNurse]||[];}
function filteredItems(){
  const q=searchQ.toLowerCase();
  if(!q)return allItems();
  return allItems().filter(it=>
    it.name.toLowerCase().includes(q)||
    it.code.toLowerCase().includes(q)||
    it.seq.includes(q)||
    it.fullCode.toLowerCase().includes(q)||
    (LOCATION_NOTES[it.fullCode]||'').toLowerCase().includes(q)
  );
}
function nowStr(){const n=new Date();return n.getFullYear()+'/'+(n.getMonth()+1)+'/'+n.getDate()+' '+n.getHours()+':'+String(n.getMinutes()).padStart(2,'0');}
 
// ── 進度 ──────────────────────────────────────────
function updateProgress(){
  const all=allItems();
  const done=all.filter(x=>x.checked).length, total=all.length;
  const pct=total?Math.round(done/total*100):0;
  document.getElementById('prog-done').textContent=done;
  document.getElementById('prog-undone').textContent=total-done;
  document.getElementById('prog-pct').textContent=pct+'%';
  document.getElementById('prog-fill').style.width=pct+'%';
  document.getElementById('anom-ct').innerHTML='';
  document.getElementById('sub-btn').disabled=!(done===total&&sigDone);
}
 
// ── 清單渲染 ──────────────────────────────────────
function renderList(){
  const list=document.getElementById('item-list');
  const items=filteredItems();
  list.innerHTML='';
  if(!items.length){
    list.innerHTML='<div class="empty-search"><i class="ti ti-search"></i>找不到符合的品項</div>';
    return;
  }
  items.forEach(it=>{
    const loc=LOCATION_NOTES[it.fullCode]||'';
    const keeper=KEEPER_NOTES[it.fullCode]||'';
    const card=document.createElement('div');
    card.className='item-card'+(it.checked?' checked':'');
    card.id='card-'+it.id;
    card.innerHTML=`
      <div class="item-inner" style="flex-direction:column;gap:8px">
        <div style="display:flex;align-items:flex-start;gap:11px">
          <div class="chk${it.checked?' on':''}" id="chk-${it.id}" onclick="toggle('${it.id}')">✓</div>
          <div class="item-body" onclick="toggle('${it.id}')">
            <div class="item-name${it.checked?' done':''}" id="nm-${it.id}">${it.name}</div>
            <div class="item-code">${it.code}${it.seq?' — '+parseInt(it.seq)+'號':''}${it.unit?' · '+it.unit:''}</div>
            ${loc?`<div class="item-loc" id="loc-tag-${it.id}">📍 ${loc}</div>`:`<div id="loc-tag-${it.id}"></div>`}
            ${keeper?`<div class="item-keeper" id="keeper-tag-${it.id}">👤 ${keeper}</div>`:`<div id="keeper-tag-${it.id}"></div>`}
 
          </div>
        </div>
        <div class="item-btns" style="flex-direction:row;gap:4px;flex-wrap:wrap;justify-content:flex-end;margin-top:6px">
          <div class="ibtn ${loc?'loc':'loc-empty'}" id="lb-${it.id}" onclick="openLocModal('${it.id}')">📍 位置</div>
          <div class="ibtn ${keeper?'keeper':'keeper-empty'}" id="kb-${it.id}" onclick="openKeeperModal('${it.id}')">👤 保管</div>
 
        </div>
      </div>`;
    list.appendChild(card);
  });
}
 
function toggle(id){
  const it=allItems().find(x=>x.id===id); if(!it)return;
  it.checked=!it.checked;
  if(it.checked&&!it.scanTime)it.scanTime=nowStr();
  document.getElementById('chk-'+id).className='chk'+(it.checked?' on':'');
  document.getElementById('nm-'+id).className='item-name'+(it.checked?' done':'');
  const card=document.getElementById('card-'+id);
  if(card)card.className='item-card'+(it.checked?' checked':'');
  updateProgress();
  autoSave();
}
 
function filterItems(){
  searchQ=document.getElementById('search-box').value;
  renderList();
}
 
 
 
// ── 位置備註 Modal ────────────────────────────────
function openLocModal(id){
  currentLocId=id;
  const it=allItems().find(x=>x.id===id);
  document.getElementById('loc-title').textContent='📍 位置備註：'+it.name;
  document.getElementById('loc-ta').value=LOCATION_NOTES[it.fullCode]||'';
  document.getElementById('modal-loc').classList.add('show');
}
function saveLoc(){
  const val=document.getElementById('loc-ta').value.trim();
  const it=allItems().find(x=>x.id===currentLocId); if(!it)return;
  LOCATION_NOTES[it.fullCode]=val;
  saveLocNotes();
  closeModal('loc');
  autoSave();
  const lb=document.getElementById('lb-'+currentLocId);
  if(lb)lb.className='ibtn '+(val?'loc':'loc-empty');
  const tag=document.getElementById('loc-tag-'+currentLocId);
  if(tag)tag.innerHTML=val?`<div class="item-loc">📍 ${val}</div>`:'';
}
// ── 保管人 Modal ──────────────────────────────────
function openKeeperModal(id){
  currentLocId=id;
  const it=allItems().find(x=>x.id===id);
  document.getElementById('keeper-title').textContent='👤 保管人：'+it.name;
  document.getElementById('keeper-ta').value=KEEPER_NOTES[it.fullCode]||'';
  document.getElementById('modal-keeper').classList.add('show');
}
function saveKeeper(){
  const val=document.getElementById('keeper-ta').value.trim();
  const it=allItems().find(x=>x.id===currentLocId); if(!it)return;
  KEEPER_NOTES[it.fullCode]=val;
  saveKeeperNotes();
  closeModal('keeper');
  autoSave();
  const kb=document.getElementById('kb-'+currentLocId);
  if(kb)kb.className='ibtn '+(val?'keeper':'keeper-empty');
  const tag=document.getElementById('keeper-tag-'+currentLocId);
  if(tag)tag.innerHTML=val?`<div class="item-keeper">👤 ${val}</div>`:'';
}
// ── 沒看到 ────────────────────────────────────────
function toggleMissing(id){
  const it=allItems().find(x=>x.id===id); if(!it)return;
  it.missing=!it.missing;
  // 沒看到時取消勾選
  if(it.missing){ it.checked=false;
    document.getElementById('chk-'+id).className='chk';
    document.getElementById('nm-'+id).className='item-name';
    const card=document.getElementById('card-'+id);
    if(card)card.className='item-card';
  }
  const mb=document.getElementById('mb-'+id);
  if(mb)mb.className='ibtn '+(it.missing?'missing':'missing-empty');
  autoSave();
  const tag=document.getElementById('missing-tag-'+id);
  if(tag)tag.innerHTML=it.missing?'<div class="item-missing">❌ 沒看到</div>':'';
  updateProgress();
}
 
function closeModal(type){document.getElementById('modal-'+type).classList.remove('show');}
 
// ── 送出 ──────────────────────────────────────────
async function doSubmit(){
  const all=allItems(); const anoms=all.filter(x=>x.note); const t=nowStr();
  lastRecord={time:t,area:currentNurse,items:JSON.parse(JSON.stringify(all)),anoms:anoms.length,by:sigName,anomDetails:[]};
  document.getElementById('sub-btn').disabled=true;
  document.getElementById('saving-bar').classList.add('show');
  const ok=await saveToFirebase(lastRecord);
  document.getElementById('saving-bar').classList.remove('show');
  history.unshift(lastRecord); saveToLocal(); clearProgress(); renderHistory();
  document.getElementById('main-area').style.display='none';
  document.getElementById('done-wrap').classList.add('show');
  const checked=all.filter(x=>x.checked).length;
  document.getElementById('done-title').textContent=anoms.length?'盤點完成，有 '+anoms.length+' 項異常':'盤點完成，全數正常 ✓';
  document.getElementById('done-sub').textContent='時間：'+t+'\n完成：'+checked+'/'+all.length+' 件'+(ok?'\n✓ 已同步雲端':'\n⚠ 雲端同步失敗（已存本機）')+(anoms.length?'\n\n⚠ 異常：'+anoms.map(x=>x.name).join('、'):'');
}
 
function downloadExcel(rec){
  if(!rec)rec=lastRecord; if(!rec){alert('找不到記錄');return;}
  const wb=XLSX.utils.book_new();
  const ws=XLSX.utils.aoa_to_sheet([
    ['財產編號','序號','名稱','實際盤點狀況','保管人','位置備註'],
    ...(rec.items||[]).map(it=>[
      it.code, it.seq?parseInt(it.seq):'', it.name,
      it.checked?'已盤點':'未盤點',
      KEEPER_NOTES[it.fullCode]||'',
      LOCATION_NOTES[it.fullCode]||''
    ])
  ]);
  ws['!cols']=[{wch:12},{wch:6},{wch:30},{wch:10},{wch:14},{wch:22}];
  XLSX.utils.book_append_sheet(wb,ws,'盤點記錄');
  const checked=(rec.items||[]).filter(x=>x.checked).length;
  const ws2=XLSX.utils.aoa_to_sheet([
    ['盤點日期',rec.time],['盤點人員',rec.by],
    ['總件數',(rec.items||[]).length||rec.total||0],
    ['已盤點',checked],['未盤點',((rec.items||[]).length||rec.total||0)-checked],
    ['異常件數',rec.anoms],[''],
    ...((rec.anomDetails||[]).length?[['異常明細',''],...(rec.anomDetails||[]).map(d=>['',d])]:[])
  ]);
  ws2['!cols']=[{wch:12},{wch:40}];
  XLSX.utils.book_append_sheet(wb,ws2,'摘要');
  XLSX.writeFile(wb,'盤點記錄_'+rec.by+'_'+rec.time.replace(/[/:]/g,'-').replace(' ','_')+'.xlsx');
}
 
function resetApp(){
  state={}; currentNurse='';
  document.getElementById('done-wrap').classList.remove('show');
  document.getElementById('main-area').style.display='none';
  document.getElementById('upload-section').style.display='block';
  const btn=document.getElementById('start-btn');
  btn.disabled=allNurseNames.length===0;
  btn.innerHTML='<i class="ti ti-clipboard-check" style="font-size:18px"></i>開始盤點';
  if(allNurseNames.length>0){
    document.getElementById('upload-result').classList.add('show');
  } else {
    document.getElementById('upload-result').classList.remove('show');
  }
}
 
// ── 歷史 ──────────────────────────────────────────
function renderHistory(){
  const el=document.getElementById('hist-list');
  if(!history.length){el.innerHTML='<div class="hist-empty"><i class="ti ti-history"></i>尚無盤點記錄</div>';return;}
  el.innerHTML=history.map((h,i)=>{
    const checked=h.checked!==undefined?h.checked:(h.items?h.items.filter(x=>x.checked).length:0);
    const total=h.total!==undefined?h.total:(h.items?h.items.length:0);
    return`<div class="hist-card ${h.anoms?'warn':'ok'}">
      <div class="hist-top">
        <div><div class="hist-date">${h.by}</div><div class="hist-area">${h.time}</div></div>
        <span class="pill ${h.anoms?'warn':'ok'}">${h.anoms?h.anoms+' 項異常':'全數正常'}</span>
      </div>
      <div class="hist-meta"><i class="ti ti-checkbox" style="font-size:13px"></i>${checked}/${total} 完成</div>
      ${(h.anomDetails||[]).length?'<div class="hist-anoms">⚠ '+(h.anomDetails||[]).join('；')+'</div>':''}
      ${h.items?`<button class="hist-dl" onclick="downloadExcel(history[${i}])"><i class="ti ti-download" style="font-size:13px"></i>下載 Excel</button>`:'<div style="font-size:11px;color:var(--text3);margin-top:4px">雲端記錄</div>'}
    </div>`;
  }).join('');
}
 
function showTab(t){
  ['check','hist'].forEach(p=>{document.getElementById('pg-'+p).classList.toggle('on',p===t);});
  document.querySelectorAll('.tab').forEach((el,i)=>{el.classList.toggle('on',['check','hist'][i]===t);});
  if(t==='hist')renderHistory();
}
 
// ── 本地儲存 ──────────────────────────────────────
function saveToLocal(){
  try{
    localStorage.setItem('or_nurses',JSON.stringify(NURSES));
    localStorage.setItem('or_history',JSON.stringify(history.slice(0,50)));
    localStorage.setItem('or_rawmeta',JSON.stringify({name:window._lastFileName||'',time:nowStr()}));
  }catch(e){}
}
 
// ── 儲存 / 讀取盤點進度 ──────────────────────────
function autoSave(){
  try{
    if(!state[currentNurse])return;
    localStorage.setItem('or_progress',JSON.stringify({
      nurse:currentNurse, state:state[currentNurse], time:nowStr()
    }));
  }catch(e){}
}
 
function saveProgress(){
  try{
    if(!state[currentNurse])return;
    localStorage.setItem('or_progress', JSON.stringify({
      nurse: currentNurse,
      state: state[currentNurse],
      time: nowStr()
    }));
    // 按鈕動畫回饋
    const btn=document.getElementById('save-btn');
    btn.innerHTML='<i class="ti ti-check" style="font-size:13px"></i>已儲存';
    btn.style.background='#15803d';
    setTimeout(()=>{
      btn.innerHTML='<i class="ti ti-device-floppy" style="font-size:13px"></i>儲存';
      btn.style.background='#16a34a';
    },2000);
  }catch(e){alert('儲存失敗：'+e.message);}
}
 
function loadProgress(){
  try{
    const p=localStorage.getItem('or_progress');
    if(!p)return false;
    const data=JSON.parse(p);
    // 確認是同一個護理師/清單
    if(data.nurse!==currentNurse)return false;
    state[currentNurse]=data.state;
    return data.time;
  }catch(e){return false;}
}
 
function clearProgress(){
  try{localStorage.removeItem('or_progress');}catch(e){}
}
function saveLocNotes(){
  try{localStorage.setItem('or_loc_notes',JSON.stringify(LOCATION_NOTES));}catch(e){}
}
function saveKeeperNotes(){
  try{localStorage.setItem('or_keeper_notes',JSON.stringify(KEEPER_NOTES));}catch(e){}
}
function loadFromLocal(){
  try{
    const nurses=localStorage.getItem('or_nurses');
    const hist=localStorage.getItem('or_history');
    const meta=localStorage.getItem('or_rawmeta');
    const loc=localStorage.getItem('or_loc_notes');
    if(nurses){
      NURSES=JSON.parse(nurses); RAW=NURSES;
      allNurseNames=Object.keys(NURSES).sort();
    }
    if(loc)LOCATION_NOTES=JSON.parse(loc);
    const kn=localStorage.getItem('or_keeper_notes');
    if(kn)KEEPER_NOTES=JSON.parse(kn);
    if(hist)history=JSON.parse(hist);
    if(meta&&allNurseNames.length>0){
      const m=JSON.parse(meta);
      const ti=Object.values(NURSES).filter((_,i)=>Object.keys(NURSES)[i]!=='__ALL__').reduce((s,a)=>s+a.length,0);
      document.getElementById('ur-t1').textContent='✓ 上次載入：'+(m.name||'財產報表');
      document.getElementById('ur-t2').textContent='共 '+ti+' 件財產 · '+m.time;
      document.getElementById('upload-result').classList.add('show');
      document.getElementById('start-btn').disabled=false;
    }
  }catch(e){}
}
 
// ── 初始化 ────────────────────────────────────────
const _n=new Date();
document.getElementById('topdate').textContent=(_n.getFullYear()-1911)+'年'+(_n.getMonth()+1)+'月'+_n.getDate()+'日';
loadFromLocal();
renderHistory();
</script>
</body>
</html>
 

