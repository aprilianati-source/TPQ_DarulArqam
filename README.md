<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Absensi TPQ - Simpan ke Spreadsheet (Urut Nama)</title>

  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

  <style>
    body { background:#f7f7f8; padding:18px; }
    .card { border-radius:12px; }
    .btn-scan { background:#ff7a00; color:#fff; }
    .btn-validate { background:#7b2aff; color:#fff; }
    .status-btn { min-width:90px; border-radius:10px; }
    #qr-reader { width:100%; margin-top:10px; }
    .small-muted { color: #6c757d; font-size:0.9rem; }
    .table thead th { font-weight:600; }
  </style>
</head>
<body>
<div class="container">
  <div class="card p-3 mb-3">
    <div class="d-flex justify-content-between align-items-center mb-2">
      <div>
        <h3 class="mb-0">Absensi TPQ - Kelas A</h3>
        <div class="small-muted">Kelola daftar santri & absensi harian (urut nama saat simpan)</div>
      </div>
      <div>
        <button id="homeBtn" class="btn btn-outline-success">Home</button>
      </div>
    </div>

    <div class="row g-3">
      <div class="col-md-3">
        <label class="form-label">Pilih Tanggal</label>
        <input id="dateInput" type="date" class="form-control"/>
      </div>

      <div class="col-md-3">
        <label class="form-label">Aksi Santri</label>
        <div class="d-flex gap-2">
          <button id="addStudentBtn" class="btn btn-primary w-100">Tambah Santri</button>
          <button id="delAllBtn" class="btn btn-outline-danger" title="Hapus semua santri">Hapus Semua</button>
        </div>
      </div>

      <div class="col-md-6">
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
          <button id="startScan" class="btn btn-scan me-2">Scan QR</button>
          <button id="stopScan" class="btn btn-secondary me-2" style="display:none">Stop</button>
        </div>

        <div id="qr-reader" style="display:none"></div>

        <div class="mt-3 d-flex gap-2">
          <button id="resetBtn" class="btn btn-dark">Reset</button>
          <button id="validateBtn" class="btn btn-validate">Validasi & Simpan (local)</button>
          <button id="downloadCsv" class="btn btn-outline-primary">Unduh CSV</button>
          <button id="saveToSheet" class="btn btn-success">Simpan ke Spreadsheet (urut nama)</button>
          <div id="sheetStatus" class="align-self-center small-muted ms-2"></div>
        </div>
      </div>
    </div>
  </div>

  <div class="card p-3">
    <table class="table table-borderless">
      <thead class="border-bottom">
        <tr>
          <th style="width:70px">No.</th>
          <th style="width:90px">ID</th>
          <th>Nama</th>
          <th style="width:140px">Status</th>
          <th style="width:180px">Waktu Kehadiran</th>
          <th style="width:120px">Aksi</th>
        </tr>
      </thead>
      <tbody id="studentsBody"></tbody>
    </table>
  </div>
</div>

<!-- Modal Tambah/Edit -->
<div class="modal" tabindex="-1" id="studentModal">
  <div class="modal-dialog">
    <div class="modal-content">
      <form id="studentForm">
        <div class="modal-header">
          <h5 class="modal-title" id="studentModalTitle">Tambah Santri</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
        </div>
        <div class="modal-body">
          <input type="hidden" id="editIndex" />
          <div class="mb-3">
            <label class="form-label">ID Santri</label>
            <input id="studentId" class="form-control" required placeholder="mis: 001" />
          </div>
          <div class="mb-3">
            <label class="form-label">Nama Santri</label>
            <input id="studentName" class="form-control" required placeholder="Nama lengkap" />
          </div>
        </div>
        <div class="modal-footer">
          <button id="saveStudentBtn" type="submit" class="btn btn-primary">Simpan</button>
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
        </div>
      </form>
    </div>
  </div>
</div>

<!-- Bootstrap & html5-qrcode -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="https://unpkg.com/html5-qrcode@2.3.8/minified/html5-qrcode.min.js"></script>

<script>
/* ===== MASUKKAN INI: Web App URL dari Apps Script yang akan di-deploy =====
   Contoh: https://script.google.com/macros/s/AKfycbx.../exec
*/
const GOOGLE_SHEETS_WEBAPP_URL = ''; // <-- masukkan di sini

/* ===== DATA AWAL (ubah sesuai kebutuhan) ===== */
const defaultStudents = [
  { id: '001', name: 'Aariz Zayan' },
  { id: '002', name: 'Aisha Zahira' },
  { id: '003', name: 'Hana Zafira' }
];

let students = []; // {id,name,status,time}
let qrScanner = null;
let scanning = false;

/* ===== UTIL ===== */
function nowTimeString() { return new Date().toLocaleTimeString(); }
function storageKeyStudents() { return 'absensi_tpq_students'; }
function storageKeyForDate(d) { return 'absensi_tpq_' + d; }

/* ===== MASTER STUDENT LIST PERSISTENCE ===== */
function loadStudentList() {
  const raw = localStorage.getItem(storageKeyStudents());
  if (raw) {
    try { return JSON.parse(raw); } catch(e){ /* ignore */ }
  }
  localStorage.setItem(storageKeyStudents(), JSON.stringify(defaultStudents));
  return defaultStudents.slice();
}
function saveStudentList(list) { localStorage.setItem(storageKeyStudents(), JSON.stringify(list)); }

/* ===== ATTENDANCE PER DATE ===== */
function loadAttendanceForDate(dateStr) {
  const raw = localStorage.getItem(storageKeyForDate(dateStr));
  if (raw) {
    try { return JSON.parse(raw).students; } catch(e) {}
  }
  const list = loadStudentList();
  return list.map(s => ({ id: s.id, name: s.name, status: 'Belum', time: '' }));
}
function saveAttendanceForDate(dateStr, arr) {
  const payload = { date: dateStr, students: arr };
  localStorage.setItem(storageKeyForDate(dateStr), JSON.stringify(payload));
}

/* ===== RENDER TABLE ===== */
function renderTable() {
  const tbody = document.getElementById('studentsBody');
  tbody.innerHTML = '';
  students.forEach((s, idx) => {
    const tr = document.createElement('tr');

    const noTd = document.createElement('td'); noTd.textContent = String(idx+1).padStart(3,'0'); tr.appendChild(noTd);
    const idTd = document.createElement('td'); idTd.textContent = s.id; tr.appendChild(idTd);
    const nameTd = document.createElement('td'); nameTd.textContent = s.name; tr.appendChild(nameTd);

    const statusTd = document.createElement('td');
    const btn = document.createElement('button');
    btn.className = 'btn status-btn';
    btn.innerText = s.status || 'Belum';
    btn.disabled = (document.getElementById('modeQr').checked);
    btn.addEventListener('click', ()=> {
      if ((s.status||'Belum') === 'Hadir') { s.status = 'Belum'; s.time = ''; }
      else { s.status = 'Hadir'; s.time = nowTimeString(); }
      renderTable();
    });
    if ((s.status||'Belum') === 'Hadir') btn.classList.add('btn-success'); else btn.classList.add('btn-outline-secondary');
    statusTd.appendChild(btn); tr.appendChild(statusTd);

    const timeTd = document.createElement('td'); timeTd.textContent = s.time || '-'; tr.appendChild(timeTd);

    const actionTd = document.createElement('td');
    const editBtn = document.createElement('button'); editBtn.className='btn btn-sm btn-outline-primary me-1'; editBtn.textContent='Edit';
    editBtn.addEventListener('click', ()=> openStudentModal('edit', idx));
    const delBtn = document.createElement('button'); delBtn.className='btn btn-sm btn-outline-danger'; delBtn.textContent='Hapus';
    delBtn.addEventListener('click', ()=> { if(confirm('Hapus santri ini?')) removeStudent(s.id); });
    actionTd.appendChild(editBtn); actionTd.appendChild(delBtn);
    tr.appendChild(actionTd);

    tbody.appendChild(tr);
  });
}

/* ===== MODAL: TAMBAH / EDIT ===== */
function openStudentModal(mode='add', index=null) {
  const modalEl = document.getElementById('studentModal');
  const bs = new bootstrap.Modal(modalEl);
  document.getElementById('studentForm').reset();
  document.getElementById('editIndex').value = '';
  document.getElementById('studentModalTitle').innerText = mode === 'add' ? 'Tambah Santri' : 'Edit Santri';
  if (mode === 'edit' && index !== null) {
    const s = students[index];
    document.getElementById('studentId').value = s.id;
    document.getElementById('studentName').value = s.name;
    document.getElementById('editIndex').value = index;
  }
  bs.show();
}

function addOrUpdateStudentFromForm(e) {
  e.preventDefault();
  const id = document.getElementById('studentId').value.trim();
  const name = document.getElementById('studentName').value.trim();
  if (!id || !name) return alert('ID dan Nama wajib diisi');
  const editIndex = document.getElementById('editIndex').value;
  let master = loadStudentList();

  if (editIndex !== '') {
    // edit existing in both master and current attendance row
    const oldId = students[editIndex].id;
    master = master.map(m => m.id === oldId ? { id, name } : m);
    saveStudentList(master);
    students[editIndex].id = id; students[editIndex].name = name;
  } else {
    // add new, ensure unique id
    if (master.find(x => x.id === id)) return alert('ID sudah ada, gunakan ID unik.');
    master.push({ id, name });
    saveStudentList(master);
    students.push({ id, name, status: 'Belum', time: '' });
  }

  const dateStr = document.getElementById('dateInput').value;
  saveAttendanceForDate(dateStr, students);
  renderTable();
  bootstrap.Modal.getInstance(document.getElementById('studentModal')).hide();
}

function removeStudent(id) {
  let master = loadStudentList().filter(x => x.id !== id);
  saveStudentList(master);
  students = students.filter(s => s.id !== id);
  const dateStr = document.getElementById('dateInput').value;
  saveAttendanceForDate(dateStr, students);
  renderTable();
}

/* ===== RESET / VALIDATE / CSV ===== */
function onReset() {
  if (!confirm('Reset semua status absensi untuk tanggal ini?')) return;
  students.forEach(s => { s.status = 'Belum'; s.time = ''; });
  renderTable();
}
function onValidate() {
  const dateStr = document.getElementById('dateInput').value;
  saveAttendanceForDate(dateStr, students);
  flashMessage('Data disimpan ke localStorage untuk tanggal ' + dateStr);
}
function downloadCsv() {
  const dateStr = document.getElementById('dateInput').value;
  let csv = 'No,ID,Nama,Status,Waktu\n';
  students.forEach((s,idx)=> {
    csv += `${idx+1},"${s.id}","${s.name}","${s.status || 'Belum'}","${s.time || ''}"\n`;
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

/* ===== QR SCAN ===== */
function markPresentById(id) {
  const s = students.find(x => x.id === id);
  if (!s) { flashMessage('QR tidak cocok: ' + id); return; }
  if ((s.status||'Belum') !== 'Hadir') {
    s.status = 'Hadir'; s.time = nowTimeString(); renderTable(); flashMessage(s.name + ' terabsen');
  } else flashMessage(s.name + ' sudah terabsen');
}

function startQrScanner() {
  if (scanning) return;
  let qrReaderEl = document.getElementById('qr-reader');
  qrReaderEl.style.display = 'block';
  document.getElementById('startScan').style.display = 'none';
  document.getElementById('stopScan').style.display = 'inline-block';
  scanning = true;
  if (!qrScanner) qrScanner = new Html5Qrcode("qr-reader");
  const config = { fps: 10, qrbox: { width: 250, height: 250 } };
  Html5Qrcode.getCameras().then(cameras => {
    const cameraId = cameras && cameras.length ? cameras[0].id : null;
    if (!cameraId) { flashMessage('Tidak ada kamera'); return; }
    qrScanner.start(cameraId, config, qrCodeMessage => { markPresentById(qrCodeMessage.trim()); })
      .catch(err=> { flashMessage('Gagal akses kamera: '+err); scanning=false; });
  }).catch(err => { flashMessage('Gagal dapat kamera: '+err); });
}

function stopQrScanner() {
  if (!scanning || !qrScanner) { document.getElementById('qr-reader').style.display='none'; document.getElementById('startScan').style.display='inline-block'; document.getElementById('stopScan').style.display='none'; scanning=false; return; }
  qrScanner.stop().then(()=> {
    scanning=false;
    document.getElementById('qr-reader').style.display='none';
    document.getElementById('startScan').style.display='inline-block';
    document.getElementById('stopScan').style.display='none';
  }).catch(()=> { scanning=false; document.getElementById('qr-reader').style.display='none'; });
}

/* ===== FLASH MESSAGE ===== */
function flashMessage(text) {
  const el = document.createElement('div');
  el.className = 'toast align-items-center text-bg-dark border-0';
  el.style.position='fixed'; el.style.right='18px'; el.style.top='18px'; el.style.zIndex='9999';
  el.role='alert'; el.setAttribute('aria-live','assertive'); el.setAttribute('aria-atomic','true');
  el.innerHTML = '<div class="d-flex"><div class="toast-body">'+text+'</div><button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast"></button></div>';
  document.body.appendChild(el);
  const bs = new bootstrap.Toast(el,{delay:1500}); bs.show();
  el.addEventListener('hidden.bs.toast', ()=> el.remove());
}

/* ===== SAVE TO GOOGLE SHEETS (POST) =====
   Payload:
   { date: "YYYY-MM-DD", data: [ {id,name,status,time}, ... ] }
   NOTE: sebelum dikirim, kita URUTKAN data berdasarkan nama (A->Z) di sisi client,
   agar Apps Script tidak perlu mengurutkan lagi (Apps Script juga akan mengurutkan jika diperlukan).
*/
async function saveToGoogleSheets() {
  if (!GOOGLE_SHEETS_WEBAPP_URL) {
    alert('URL Google Sheets WebApp belum dikonfigurasi. Masukkan URL di variable GOOGLE_SHEETS_WEBAPP_URL di file HTML.');
    return;
  }

  // Clone data and sort by name ascending
  const payloadData = students.map(s => ({ id: s.id, name: s.name, status: s.status || 'Belum', time: s.time || '' }))
                             .sort((a,b) => a.name.localeCompare(b.name));

  const payload = { date: document.getElementById('dateInput').value, data: payloadData };
  document.getElementById('sheetStatus').innerText = 'Mengirim...';
  try {
    const resp = await fetch(GOOGLE_SHEETS_WEBAPP_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });
    const text = await resp.text();
    if (resp.ok) {
      document.getElementById('sheetStatus').innerText = 'Tersimpan ke Spreadsheet';
      flashMessage('Sinkron ke spreadsheet berhasil (data terurut A→Z)');
    } else {
      document.getElementById('sheetStatus').innerText = 'Gagal: ' + resp.status;
      alert('Gagal menulis ke spreadsheet: ' + text);
    }
  } catch(err) {
    document.getElementById('sheetStatus').innerText = 'Gagal koneksi';
    alert('Gagal mengirim ke server: ' + err);
  } finally {
    setTimeout(()=> { document.getElementById('sheetStatus').innerText = ''; }, 3000);
  }
}

/* ===== UI EVENTS ===== */
document.getElementById('dateInput').addEventListener('change', ()=> {
  stopQrScanner();
  const dateStr = document.getElementById('dateInput').value;
  students = loadAttendanceForDate(dateStr);
  renderTable();
});
document.getElementById('modeQr').addEventListener('change', ()=> renderTable());
document.getElementById('modeManual').addEventListener('change', ()=> renderTable());
document.getElementById('resetBtn').addEventListener('click', onReset);
document.getElementById('validateBtn').addEventListener('click', ()=> { saveAttendanceForDate(document.getElementById('dateInput').value, students); flashMessage('Tersimpan lokal'); });
document.getElementById('downloadCsv').addEventListener('click', downloadCsv);
document.getElementById('startScan').addEventListener('click', startQrScanner);
document.getElementById('stopScan').addEventListener('click', stopQrScanner);
document.getElementById('addStudentBtn').addEventListener('click', ()=> openStudentModal('add'));
document.getElementById('studentForm').addEventListener('submit', addOrUpdateStudentFromForm);
document.getElementById('delAllBtn').addEventListener('click', ()=> {
  if (!confirm('Hapus semua santri (master list)? Ini juga akan menghapus dari localStorage daftar). Lanjutkan?')) return;
  localStorage.removeItem(storageKeyStudents());
  saveStudentList(defaultStudents.slice());
  const dateStr = document.getElementById('dateInput').value;
  students = loadAttendanceForDate(dateStr);
  saveAttendanceForDate(dateStr, students);
  renderTable();
});
document.getElementById('saveToSheet').addEventListener('click', saveToGoogleSheets);
document.getElementById('homeBtn').addEventListener('click', ()=> location.reload());

/* ===== INIT ===== */
function initialize() {
  document.getElementById('dateInput').value = new Date().toISOString().slice(0,10);
  const dateStr = document.getElementById('dateInput').value;
  students = loadAttendanceForDate(dateStr);
  renderTable();
}
initialize();
</script>
</body>
</html>
