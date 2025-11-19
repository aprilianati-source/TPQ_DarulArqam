<!doctype html>

<html lang="id">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Aplikasi Maintenance Unit</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body { background:#f8f9fa; }
    .app-card { max-width:980px; margin:30px auto; }
  </style>
</head>
<body>
<div class="container app-card">
  <div class="card shadow-sm">
    <div class="card-body">
      <div id="page-login">
        <h3 class="card-title">Login Aplikasi Maintenance Unit</h3>
        <p class="text-muted">Silakan login terlebih dahulu.</p>
        <div id="login-alert" class="alert alert-danger d-none" role="alert"></div>
        <form id="login-form" class="row g-3" onsubmit="return false">
          <div class="col-md-6">
            <label class="form-label">Username</label>
            <input id="username" class="form-control" required>
          </div>
          <div class="col-md-6">
            <label class="form-label">Password</label>
            <input id="password" type="password" class="form-control" required>
          </div>
          <div class="col-12">
            <button id="btn-login" class="btn btn-primary">Login</button>
            <small class="text-muted ms-3">(contoh: <code>admin</code> / <code>admin123</code>)</small>
          </div>
        </form>
      </div><div id="page-dashboard" class="d-none">
    <div class="d-flex justify-content-between align-items-center mb-3">
      <h3>Dashboard - Maintenance Unit</h3>
      <div>
        <span id="logged-user" class="me-3"></span>
        <button id="btn-logout" class="btn btn-outline-danger btn-sm">Log Out</button>
      </div>
    </div>

    <ul class="nav nav-tabs" id="mainTabs" role="tablist">
      <li class="nav-item" role="presentation"><button class="nav-link active" id="tab-monitoring" data-bs-toggle="tab" data-bs-target="#monitoring" type="button" role="tab">Monitoring Redo Service</button></li>
      <li class="nav-item" role="presentation"><button class="nav-link" id="tab-midlife" data-bs-toggle="tab" data-bs-target="#midlife" type="button" role="tab">Midlife Component</button></li>
    </ul>

    <div class="tab-content mt-3">
      <!-- Monitoring Redo Service -->
      <div class="tab-pane fade show active" id="monitoring" role="tabpanel">
        <h5>Input Monitoring Redo Service</h5>
        <form id="form-redo" class="row g-3" onsubmit="return false">
          <div class="col-md-3"><label class="form-label">Nomor</label><input id="redo-nomor" class="form-control" required></div>
          <div class="col-md-3"><label class="form-label">Tanggal</label><input id="redo-tanggal" type="date" class="form-control" required></div>
          <div class="col-md-3"><label class="form-label">DT (Nama Unit)</label><input id="redo-dt" class="form-control" required></div>
          <div class="col-md-3"><label class="form-label">Trouble / Penyebab</label><input id="redo-trouble" class="form-control" required></div>
          <div class="col-12"><button id="btn-add-redo" class="btn btn-success">Simpan</button></div>
        </form>

        <hr>
        <h6>Daftar Monitoring</h6>
        <div id="redo-list" class="table-responsive"></div>
      </div>

      <!-- Midlife Component -->
      <div class="tab-pane fade" id="midlife" role="tabpanel">
        <h5>Input Midlife Component</h5>
        <form id="form-midlife" class="row g-3" onsubmit="return false">
          <div class="col-md-4"><label class="form-label">Nomor</label><input id="mid-nomor" class="form-control" required></div>
          <div class="col-md-4"><label class="form-label">Tanggal Penggantian</label><input id="mid-tanggal" type="date" class="form-control" required></div>
          <div class="col-md-4"><label class="form-label">Nama Komponen</label><input id="mid-komponen" class="form-control" required></div>
          <div class="col-12"><button id="btn-add-mid" class="btn btn-success">Simpan</button></div>
        </form>

        <hr>
        <h6>Daftar Midlife Components</h6>
        <div id="midlife-list" class="table-responsive"></div>
      </div>
    </div>

  </div>

</div>

  </div>  <footer class="text-center mt-3 text-muted">Aplikasi sederhana - data disimpan di browser (localStorage).</footer>
</div><script>
// --- Simple auth (hardcoded) ---
const CREDENTIALS = { username: 'admin', password: 'admin123' };

// Elements
const pageLogin = document.getElementById('page-login');
const pageDashboard = document.getElementById('page-dashboard');
const loginAlert = document.getElementById('login-alert');
const loggedUser = document.getElementById('logged-user');

// Storage keys
const KEY_REDO = 'redoServices_v1';
const KEY_MID = 'midlifeComponents_v1';
const KEY_AUTH = 'maintenance_auth_v1';

// Helpers
function show(el){ el.classList.remove('d-none'); }
function hide(el){ el.classList.add('d-none'); }

function saveArray(key, arr){ localStorage.setItem(key, JSON.stringify(arr)); }
function loadArray(key){ try { return JSON.parse(localStorage.getItem(key) || '[]'); } catch(e){ return []; } }

// Auth
function setAuth(username){ localStorage.setItem(KEY_AUTH, username); }
function clearAuth(){ localStorage.removeItem(KEY_AUTH); }
function getAuth(){ return localStorage.getItem(KEY_AUTH); }

// Init
function init(){
  const user = getAuth();
  if(user){ showDashboard(user); } else { showLogin(); }
  renderRedoTable(); renderMidTable();
}

function showLogin(){ show(pageLogin); hide(pageDashboard); }
function showDashboard(user){ hide(pageLogin); show(pageDashboard); loggedUser.textContent = user; }

// Login handler
document.getElementById('btn-login').addEventListener('click', ()=>{
  const u = document.getElementById('username').value.trim();
  const p = document.getElementById('password').value;
  if(u === CREDENTIALS.username && p === CREDENTIALS.password){
    setAuth(u); showDashboard(u); loginAlert.classList.add('d-none');
  } else {
    loginAlert.textContent = 'Login gagal — username atau password salah.';
    loginAlert.classList.remove('d-none');
  }
});

// Logout
document.getElementById('btn-logout').addEventListener('click', ()=>{
  clearAuth(); showLogin();
});

// Add redo
document.getElementById('btn-add-redo').addEventListener('click', ()=>{
  const nomor = document.getElementById('redo-nomor').value.trim();
  const tanggal = document.getElementById('redo-tanggal').value;
  const dt = document.getElementById('redo-dt').value.trim();
  const trouble = document.getElementById('redo-trouble').value.trim();
  if(!nomor||!tanggal||!dt||!trouble){ alert('Lengkapi semua field.'); return; }
  const arr = loadArray(KEY_REDO);
  arr.push({ id: Date.now(), nomor, tanggal, dt, trouble });
  saveArray(KEY_REDO, arr);
  document.getElementById('form-redo').reset(); renderRedoTable();
});

// Add midlife
document.getElementById('btn-add-mid').addEventListener('click', ()=>{
  const nomor = document.getElementById('mid-nomor').value.trim();
  const tanggal = document.getElementById('mid-tanggal').value;
  const komponen = document.getElementById('mid-komponen').value.trim();
  if(!nomor||!tanggal||!komponen){ alert('Lengkapi semua field.'); return; }
  const arr = loadArray(KEY_MID);
  arr.push({ id: Date.now(), nomor, tanggal, komponen });
  saveArray(KEY_MID, arr);
  document.getElementById('form-midlife').reset(); renderMidTable();
});

// Render redo table
function renderRedoTable(){
  const arr = loadArray(KEY_REDO);
  if(arr.length === 0){ document.getElementById('redo-list').innerHTML = '<div class="alert alert-secondary">Belum ada data.</div>'; return; }
  let html = '<table class="table table-sm table-striped"><thead><tr><th>Nomor</th><th>Tanggal</th><th>DT (unit)</th><th>Trouble</th><th>Aksi</th></tr></thead><tbody>';
  for(const r of arr){ html += `<tr><td>${escapeHtml(r.nomor)}</td><td>${escapeHtml(r.tanggal)}</td><td>${escapeHtml(r.dt)}</td><td>${escapeHtml(r.trouble)}</td><td><button class="btn btn-sm btn-danger" onclick="deleteRedo(${r.id})">Hapus</button></td></tr>`; }
  html += '</tbody></table>';
  document.getElementById('redo-list').innerHTML = html;
}

// Render mid table
function renderMidTable(){
  const arr = loadArray(KEY_MID);
  if(arr.length === 0){ document.getElementById('midlife-list').innerHTML = '<div class="alert alert-secondary">Belum ada data.</div>'; return; }
  let html = '<table class="table table-sm table-striped"><thead><tr><th>Nomor</th><th>Tanggal Penggantian</th><th>Nama Komponen</th><th>Aksi</th></tr></thead><tbody>';
  for(const r of arr){ html += `<tr><td>${escapeHtml(r.nomor)}</td><td>${escapeHtml(r.tanggal)}</td><td>${escapeHtml(r.komponen)}</td><td><button class="btn btn-sm btn-danger" onclick="deleteMid(${r.id})">Hapus</button></td></tr>`; }
  html += '</tbody></table>';
  document.getElementById('midlife-list').innerHTML = html;
}

// Delete functions (exposed to global)
function deleteRedo(id){ if(!confirm('Hapus data ini?')) return; let arr = loadArray(KEY_REDO); arr = arr.filter(x=>x.id!==id); saveArray(KEY_REDO, arr); renderRedoTable(); }
function deleteMid(id){ if(!confirm('Hapus data ini?')) return; let arr = loadArray(KEY_MID); arr = arr.filter(x=>x.id!==id); saveArray(KEY_MID, arr); renderMidTable(); }

// small HTML escape
function escapeHtml(s){ return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }

// expose delete funcs for inline onclick
window.deleteRedo = deleteRedo; window.deleteMid = deleteMid;

init();
</script><script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script></body>
</html>
