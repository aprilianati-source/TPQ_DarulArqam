<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Absensi TPQ</title>

  <!-- Bootstrap CSS (CDN) -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

  <style>
    body { background:#f7f7f8; padding:18px; }
    .card { border-radius:12px; }
    .btn-scan { background:#ff7a00; color:#fff; }
    .btn-validate { background:#7b2aff; color:#fff; }
    .status-btn { min-width:90px; border-radius:10px; }
    #qr-reader { width:100%; }
    .small-muted { color: #6c757d; font-size:0.9rem; }
  </style>
</head>
<body>
<div class="container">
  <div class="card p-3 mb-4">
    <div class="d-flex justify-content-between align-items-center mb-2">
      <div>
        <h3 class="mb-0">Absensi TPQ - Kelas A</h3>
        <div class="small-muted">Kelola daftar santri & absensi harian</div>
      </div>
      <div>
        <button id="homeBtn" class="btn btn-outline-success">Home</button>
      </div>
    </div>

    <div class="row g-3">
      <div class="col-md-4">
        <label class="form-label">Pilih Tanggal</label>
        <input id="dateInput" type="date" class="form-control"/>
      </div>

      <div class="col-md-8">
        <label class="form-label">Mode Absensi</label>
        <div class="btn-group w-100" role="group">
          <input type="radio" class="btn-check" name="mode" id="modeQr" autocomplete="off" checked>
          <label class="btn btn-outline-secondary" for="modeQr">QR Code</label>

          <input type="radio" class="btn-check" name="mode" id="modeManual" autocomplete="off">
          <label class="btn btn-outline-secondary" for="modeManual">Manual</label>
        </div>
      </div>

      <div class="col-12">
        <div id="qrSection" class="mb-3">
          <button id="startScan" class="btn btn-scan me-2"><i class="bi-qr-scan"></i> Scan QR</button>
          <button id="stopScan" class="btn btn-secondary me-2" style="display:none">Stop</button>
        </div>

        <div id="qr-reader" style="display:none"></div>

        <div class="mt-3">
          <button id="resetBtn" class="btn btn-dark me-2">Reset</button>
          <button id="validateBtn" class="btn btn-validate">Validasi & Simpan</button>
          <button id="downloadCsv" class="btn btn-outline-primary ms-2">Unduh CSV</button>
        </div>
      </div>
    </div>
  </div>

  <div class="card p-3">
    <table class="table table-borderless">
      <thead class="border-bottom">
        <tr>
          <th style="width:70px">No.</th>
          <th>Nama</th>
          <th style="width:140px">Status</th>
          <th style="width:180px">Waktu Kehadiran</th>
        </tr>
      </thead>
      <tbody id="studentsBody">
        <!-- diisi oleh JS -->
      </tbody>
    </table>
  </div>
</div>

<!-- Modal ringkasan -->
<div class="modal" tabindex="-1" id="summaryModal">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Ringkasan Absensi</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body" id="summaryBody"></div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
      </div>
    </div>
  </div>
</div>

<!-- Bootstrap JS & dependencies -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<!-- html5-qrcode library untuk scan QR (CDN) -->
<script src="https://unpkg.com/html5-qrcode@2.3.8/minified/html5-qrcode.min.js"></script>

<script>
/* ===== DATA AWAL - sesuaikan daftar santri di sini ===== */
const initialStudents = [
  { id: '001', name: 'Aariz Zayan' },
  { id: '002', name: 'Aisha Zahira' },
  { id: '003', name: 'Hana Zafira' },
  { id: '004', name: 'Ahmad Ilham' },
  { id: '005', name: 'Siti Nur' }
];

/* ===== STATE ===== */
let students = [];
let qrScanner = null;
let scanning = false;

/* ===== UTIL ===== */
function nowTimeString() {
  const d = new Date();
  // format HH:MM:SS
  return d.toLocaleTimeString();
}

function formatDateInputValue(v) {
  // v already in YYYY-MM-DD
  return v || new Date().toISOString().slice(0,10);
}

/* ===== PERSISTENCE ===== */
function saveToLocal(dateStr) {
  const key = 'absensi_tpq_' + dateStr;
  const payload = { date: dateStr, students };
  localStorage.setItem(key, JSON.stringify(payload));
}

function loadFromLocal(dateStr) {
  const key = 'absensi_tpq_' + dateStr;
  const raw = localStorage.getItem(key);
  if (!raw) return null;
  try { return JSON.parse(raw); } catch(e) { return null; }
}

/* ===== RENDER ===== */
function renderTable() {
  const tbody = document.getElementById('studentsBody');
  tbody.innerHTML = '';
  students.forEach((s, idx) => {
    const tr = document.createElement('tr');

    const noTd = document.createElement('td');
    noTd.textContent = String(idx+1).padStart(3,'0');
    tr.appendChild(noTd);

    const nameTd = document.createElement('td');
    nameTd.textContent = s.name;
    tr.appendChild(nameTd);

    const statusTd = document.createElement('td');
    const btn = document.createElement('button');
    btn.className = 'btn status-btn';
    btn.innerText = s.status || 'Belum';
    btn.setAttribute('data-id', s.id);
    btn.disabled = (document.getElementById('modeQr').checked); // disable in QR mode
    // style
    if ((s.status||'Belum') === 'Hadir') {
      btn.classList.remove('btn-outline-secondary');
      btn.classList.add('btn-success');
    } else {
      btn.classList.remove('btn-success');
      btn.classList.add('btn-outline-secondary');
    }
    btn.addEventListener('click', onManualToggle);
    statusTd.appendChild(btn);
    tr.appendChild(statusTd);

    const timeTd = document.createElement('td');
    timeTd.textContent = s.time || '-';
    tr.appendChild(timeTd);

    tbody.appendChild(tr);
  });
}

/* ===== MANUAL TOGGLE ===== */
function onManualToggle(e) {
  const id = e.currentTarget.getAttribute('data-id');
  const s = students.find(x => x.id === id);
  if (!s) return;
  if ((s.status||'Belum') === 'Hadir') {
    s.status = 'Belum';
    s.time = '';
  } else {
    s.status = 'Hadir';
    s.time = nowTimeString();
  }
  renderTable();
}

/* ===== QR HANDLING ===== */
function markPresentById(id) {
  const s = students.find(x => x.id === id);
  if (!s) {
    // jika id tidak ditemukan, tampilkan notifikasi singkat
    flashMessage('QR tidak cocok: ' + id);
    return;
  }
  if ((s.status||'Belum') !== 'Hadir') {
    s.status = 'Hadir';
    s.time = nowTimeString();
    renderTable();
    flashMessage(s.name + ' terabsen');
  } else {
    flashMessage(s.name + ' sudah terabsen');
  }
}

function flashMessage(text) {
  // kecil notifikasi top
  const el = document.createElement('div');
  el.className = 'toast align-items-center text-bg-dark border-0';
  el.style.position = 'fixed';
  el.style.right = '18px';
  el.style.top = '18px';
  el.style.zIndex = '9999';
  el.role = 'alert';
  el.setAttribute('aria-live', 'assertive');
  el.setAttribute('aria-atomic', 'true');
  el.innerHTML = '<div class="d-flex"><div class="toast-body">'+text+'</div><button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast"></button></div>';
  document.body.appendChild(el);
  const bs = new bootstrap.Toast(el, { delay: 1800 });
  bs.show();
  el.addEventListener('hidden.bs.toast', ()=> el.remove());
}

/* ===== EVENTS UI ===== */
document.getElementById('dateInput').addEventListener('change', onDateChange);
document.getElementById('modeQr').addEventListener('change', onModeChange);
document.getElementById('modeManual').addEventListener('change', onModeChange);
document.getElementById('resetBtn').addEventListener('click', onReset);
document.getElementById('validateBtn').addEventListener('click', onValidate);
document.getElementById('startScan').addEventListener('click', startQrScanner);
document.getElementById('stopScan').addEventListener('click', stopQrScanner);
document.getElementById('downloadCsv').addEventListener('click', downloadCsv);
document.getElementById('homeBtn').addEventListener('click', ()=> location.reload());

function initialize() {
  // set default date = today
  const dateInput = document.getElementById('dateInput');
  dateInput.value = new Date().toISOString().slice(0,10);

  // init students from localStorage for the date if available
  loadForCurrentDate();

  renderTable();
  onModeChange();
}

function loadForCurrentDate() {
  const dateStr = document.getElementById('dateInput').value;
  const saved = loadFromLocal(dateStr);
  if (saved && saved.students) {
    students = saved.students;
  } else {
    // clone initialStudents with default status
    students = initialStudents.map(x => ({ ...x, status: 'Belum', time: '' }));
  }
}

function onDateChange() {
  stopQrScanner();
  loadForCurrentDate();
  renderTable();
}

function onModeChange() {
  const isQr = document.getElementById('modeQr').checked;
  document.getElementById('qrSection').style.display = isQr ? 'block' : 'none';
  // enable/disable manual buttons
  renderTable();
}

/* ===== Reset ===== */
function onReset() {
  if (!confirm('Reset semua status absensi untuk tanggal ini?')) return;
  students.forEach(s => { s.status = 'Belum'; s.time = ''; });
  renderTable();
  flashMessage('Semua status direset');
}

/* ===== Validate (simpan) ===== */
function onValidate() {
  const dateStr = document.getElementById('dateInput').value;
  saveToLocal(dateStr);
  // tampilkan ringkasan
  const hadir = students.filter(s => (s.status||'Belum') === 'Hadir').length;
  const belum = students.length - hadir;
  const body = document.getElementById('summaryBody');
  body.innerHTML = `<p>Tanggal: <strong>${dateStr}</strong></p>
                    <p>Jumlah santri: <strong>${students.length}</strong></p>
                    <p>Hadir: <strong>${hadir}</strong></p>
                    <p>Belum hadir: <strong>${belum}</strong></p>
                    <hr>
                    <p>Data telah disimpan ke <code>localStorage</code> browser.</p>`;
  const modal = new bootstrap.Modal(document.getElementById('summaryModal'));
  modal.show();
}

/* ===== CSV download ===== */
function downloadCsv() {
  const dateStr = document.getElementById('dateInput').value;
  let csv = 'No,ID,Nama,Status,Waktu\\n';
  students.forEach((s,idx)=> {
    csv += `${idx+1},"${s.id}","${s.name}","${s.status || 'Belum'}","${s.time || ''}"\\n`;
  });
  const blob = new Blob([csv], { type: 'text/csv' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `absensi_tpq_${dateStr}.csv`;
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
}

/* ===== QR SCANNER ===== */
function startQrScanner() {
  if (scanning) return;
  const qrReaderEl = document.getElementById('qr-reader');
  qrReaderEl.style.display = 'block';
  document.getElementById('startScan').style.display = 'none';
  document.getElementById('stopScan').style.display = 'inline-block';
  scanning = true;

  if (!qrScanner) {
    // create scanner
    qrScanner = new Html5Qrcode("qr-reader");
  }

  const config = { fps: 10, qrbox: { width: 250, height: 250 } };
  Html5Qrcode.getCameras().then(cameras => {
    const cameraId = cameras && cameras.length ? cameras[0].id : null;
    if (!cameraId) {
      flashMessage('Tidak ada kamera terdeteksi.');
      return;
    }
    qrScanner.start(
      cameraId,
      config,
      qrCodeMessage => {
        // hasil scan (string). Anggap kode QR berisi id santri, mis: "001"
        markPresentById(qrCodeMessage.trim());
      },
      errorMessage => {
        // console.log('scan error', errorMessage);
      }
    ).catch(err => {
      flashMessage('Gagal mengakses kamera: ' + err);
      scanning = false;
      document.getElementById('qr-reader').style.display = 'none';
      document.getElementById('startScan').style.display = 'inline-block';
      document.getElementById('stopScan').style.display = 'none';
    });
  }).catch(err => {
    flashMessage('Gagal mendapatkan kamera: ' + err);
  });
}

function stopQrScanner() {
  if (!scanning || !qrScanner) {
    document.getElementById('qr-reader').style.display = 'none';
    document.getElementById('startScan').style.display = 'inline-block';
    document.getElementById('stopScan').style.display = 'none';
    scanning = false;
    return;
  }
  qrScanner.stop().then(() => {
    scanning = false;
    document.getElementById('qr-reader').style.display = 'none';
    document.getElementById('startScan').style.display = 'inline-block';
    document.getElementById('stopScan').style.display = 'none';
  }).catch(err=>{
    scanning = false;
    document.getElementById('qr-reader').style.display = 'none';
    document.getElementById('startScan').style.display = 'inline-block';
    document.getElementById('stopScan').style.display = 'none';
  });
}

/* ===== Inisialisasi ===== */
initialize();

</script>
</body>
</html>
