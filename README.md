<!doctype html>

<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Aplikasi Presensi Siswa & Jurnal Mengajar</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {background:#f5f7fb}
    .sidebar {min-height:100vh}
    .hide {display:none}
    .pointer {cursor:pointer}
  </style>
</head>
<body><!-- ---------- LOGIN ---------- --><div class="container py-5" id="page-login">
  <div class="row justify-content-center">
    <div class="col-sm-10 col-md-6 col-lg-4">
      <div class="card shadow-sm">
        <div class="card-body">
          <h4 class="card-title text-center mb-3">Login - Presensi & Jurnal</h4>
          <div id="login-alert" class="alert alert-danger d-none" role="alert"></div>
          <form id="login-form">
            <div class="mb-3">
              <label class="form-label">Username</label>
              <input id="login-username" class="form-control" required>
            </div>
            <div class="mb-3">
              <label class="form-label">Password</label>
              <input id="login-password" type="password" class="form-control" required>
            </div>
            <div class="d-grid">
              <button class="btn btn-primary" type="submit">Login</button>
            </div>
            <div class="mt-3 text-muted small">Default akun: <strong>admin</strong> / <strong>admin123</strong></div>
          </form>
        </div>
      </div>
    </div>
  </div>
</div><!-- ---------- DASHBOARD ---------- --><div id="page-app" class="hide">
  <div class="container-fluid">
    <div class="row">
      <nav class="col-md-3 col-lg-2 bg-white sidebar p-3 border-end">
        <h5 class="mb-3">Dashboard</h5>
        <div class="list-group">
          <a class="list-group-item list-group-item-action pointer" data-menu="students">Manajemen Siswa</a>
          <a class="list-group-item list-group-item-action pointer" data-menu="attendance">Presensi Siswa</a>
          <a class="list-group-item list-group-item-action pointer" data-menu="journal">Jurnal Mengajar</a>
          <a class="list-group-item list-group-item-action pointer text-danger" id="btn-logout">Logout</a>
        </div>
      </nav><main class="col-md-9 col-lg-10 p-4">
    <div id="view-students" class="view">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h4>Manajemen Siswa</h4>
        <div>
          <button class="btn btn-sm btn-success" id="btn-show-add">+ Tambah Siswa</button>
        </div>
      </div>

      <div class="card mb-3">
        <div class="card-body">
          <table class="table table-striped" id="students-table">
            <thead>
              <tr><th>#</th><th>NIS</th><th>Nama</th><th>Kelas</th><th>Aksi</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>

      <!-- form tambah/edit -->
      <div class="card" id="card-add-student" style="display:none">
        <div class="card-body">
          <h5 id="form-title">Tambah Siswa</h5>
          <form id="form-student">
            <input type="hidden" id="student-index">
            <div class="row">
              <div class="col-md-4 mb-2"><input id="s-nis" class="form-control" placeholder="NIS" required></div>
              <div class="col-md-4 mb-2"><input id="s-nama" class="form-control" placeholder="Nama" required></div>
              <div class="col-md-4 mb-2"><input id="s-kelas" class="form-control" placeholder="Kelas" required></div>
            </div>
            <div class="mt-2">
              <button class="btn btn-primary btn-sm">Simpan</button>
              <button type="button" id="btn-cancel-add" class="btn btn-secondary btn-sm">Batal</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <div id="view-attendance" class="view hide">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h4>Presensi Siswa</h4>
        <div>
          <input type="date" id="att-date" class="form-control form-control-sm">
        </div>
      </div>

      <div class="card">
        <div class="card-body">
          <form id="form-attendance">
            <table class="table" id="attendance-table">
              <thead><tr><th>#</th><th>NIS</th><th>Nama</th><th>Kelas</th><th>Status</th></tr></thead>
              <tbody></tbody>
            </table>
            <div>
              <button class="btn btn-primary btn-sm">Simpan Presensi</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <div id="view-journal" class="view hide">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h4>Jurnal Mengajar</h4>
        <div>
          <button class="btn btn-sm btn-success" id="btn-add-journal">+ Tambah Jurnal</button>
        </div>
      </div>

      <div class="card mb-3" id="card-add-journal" style="display:none">
        <div class="card-body">
          <form id="form-journal">
            <input type="hidden" id="journal-index">
            <div class="row">
              <div class="col-md-3 mb-2"><input id="j-date" type="date" class="form-control" required></div>
              <div class="col-md-3 mb-2"><input id="j-class" class="form-control" placeholder="Kelas" required></div>
              <div class="col-md-6 mb-2"><input id="j-topic" class="form-control" placeholder="Topik / Materi" required></div>
            </div>
            <div class="mb-2"><textarea id="j-notes" class="form-control" rows="3" placeholder="Catatan / Refleksi"></textarea></div>
            <div>
              <button class="btn btn-primary btn-sm">Simpan Jurnal</button>
              <button type="button" id="btn-cancel-journal" class="btn btn-secondary btn-sm">Batal</button>
            </div>
          </form>
        </div>
      </div>

      <div id="journal-list" class="card">
        <div class="card-body">
          <table class="table table-sm" id="table-journal">
            <thead><tr><th>#</th><th>Tanggal</th><th>Kelas</th><th>Topik</th><th>Catatan</th><th>Aksi</th></tr></thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>

  </main>
</div>

  </div>
</div><script>
// ---------- DATA / STORAGE HELPERS ----------
const DB = {
  usersKey: 'app_users',
  studentsKey: 'app_students',
  attendanceKey: 'app_attendance',
  journalKey: 'app_journal'
}

function load(key, fallback){
  const raw = localStorage.getItem(key);
  return raw ? JSON.parse(raw) : fallback;
}
function save(key, value){ localStorage.setItem(key, JSON.stringify(value)); }

// seed default user
if(!load(DB.usersKey)){
  save(DB.usersKey, [{username:'admin', password:'admin123'}]);
}
if(!load(DB.studentsKey)) save(DB.studentsKey, []);
if(!load(DB.attendanceKey)) save(DB.attendanceKey, {}); // date -> [{nis,status}]
if(!load(DB.journalKey)) save(DB.journalKey, []);

// ---------- AUTH ----------
const loginForm = document.getElementById('login-form');
loginForm.addEventListener('submit', function(e){
  e.preventDefault();
  const u = document.getElementById('login-username').value.trim();
  const p = document.getElementById('login-password').value;
  const users = load(DB.usersKey, []);
  const ok = users.find(x=>x.username===u && x.password===p);
  const alert = document.getElementById('login-alert');
  if(ok){
    alert.classList.add('d-none');
    localStorage.setItem('app_current_user', u);
    showApp();
  } else {
    alert.textContent = 'Login failed: username atau password salah.';
    alert.classList.remove('d-none');
  }
});

function showApp(){
  document.getElementById('page-login').classList.add('hide');
  document.getElementById('page-app').classList.remove('hide');
  navigateTo('students');
  renderStudents();
}

// logout
document.getElementById('btn-logout').addEventListener('click', ()=>{
  localStorage.removeItem('app_current_user');
  document.getElementById('page-app').classList.add('hide');
  document.getElementById('page-login').classList.remove('hide');
});

// ---------- NAV ----------
document.querySelectorAll('[data-menu]').forEach(a=>{
  a.addEventListener('click', ()=> navigateTo(a.dataset.menu));
});
function navigateTo(menu){
  document.querySelectorAll('.view').forEach(v=>v.classList.add('hide'));
  document.getElementById('view-'+menu).classList.remove('hide');
}

// ---------- STUDENTS ----------
const studentsTableBody = document.querySelector('#students-table tbody');
function renderStudents(){
  const students = load(DB.studentsKey, []);
  studentsTableBody.innerHTML = '';
  students.forEach((s,i)=>{
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${i+1}</td><td>${s.nis}</td><td>${s.nama}</td><td>${s.kelas}</td>
      <td>
        <button class="btn btn-sm btn-warning me-1" onclick="editStudent(${i})">Edit</button>
        <button class="btn btn-sm btn-danger" onclick="deleteStudent(${i})">Hapus</button>
      </td>`;
    studentsTableBody.appendChild(tr);
  });
  renderAttendanceRows();
}

window.editStudent = function(idx){
  const s = load(DB.studentsKey, [])[idx];
  if(!s) return;
  document.getElementById('student-index').value = idx;
  document.getElementById('s-nis').value = s.nis;
  document.getElementById('s-nama').value = s.nama;
  document.getElementById('s-kelas').value = s.kelas;
  document.getElementById('form-title').textContent = 'Edit Siswa';
  document.getElementById('card-add-student').style.display = 'block';
}
window.deleteStudent = function(idx){
  if(!confirm('Hapus siswa ini?')) return;
  const arr = load(DB.studentsKey, []);
  arr.splice(idx,1);
  save(DB.studentsKey, arr);
  renderStudents();
}

// show add form
document.getElementById('btn-show-add').addEventListener('click', ()=>{
  document.getElementById('student-index').value = '';
  document.getElementById('s-nis').value = '';
  document.getElementById('s-nama').value = '';
  document.getElementById('s-kelas').value = '';
  document.getElementById('form-title').textContent = 'Tambah Siswa';
  document.getElementById('card-add-student').style.display = 'block';
});

document.getElementById('btn-cancel-add').addEventListener('click', ()=>{
  document.getElementById('card-add-student').style.display = 'none';
});

// submit add/edit
document.getElementById('form-student').addEventListener('submit', function(e){
  e.preventDefault();
  const idx = document.getElementById('student-index').value;
  const nis = document.getElementById('s-nis').value.trim();
  const nama = document.getElementById('s-nama').value.trim();
  const kelas = document.getElementById('s-kelas').value.trim();
  const arr = load(DB.studentsKey, []);
  if(idx===''){
    arr.push({nis,nama,kelas});
  } else {
    arr[parseInt(idx)] = {nis,nama,kelas};
  }
  save(DB.studentsKey, arr);
  document.getElementById('card-add-student').style.display = 'none';
  renderStudents();
});

// ---------- ATTENDANCE ----------
const attendanceTableBody = document.querySelector('#attendance-table tbody');
const attDateInput = document.getElementById('att-date');
if(!attDateInput.value) attDateInput.valueAsDate = new Date();
attDateInput.addEventListener('change', renderAttendanceRows);

function renderAttendanceRows(){
  const students = load(DB.studentsKey, []);
  const attendance = load(DB.attendanceKey, {});
  const date = attDateInput.value;
  const todays = attendance[date] || {};
  attendanceTableBody.innerHTML = '';
  students.forEach((s,i)=>{
    const tr = document.createElement('tr');
    const status = todays[s.nis] || 'Hadir';
    tr.innerHTML = `<td>${i+1}</td><td>${s.nis}</td><td>${s.nama}</td><td>${s.kelas}</td>
      <td>
        <select class="form-select form-select-sm" data-nis="${s.nis}">
          <option ${status==='Hadir'?'selected':''}>Hadir</option>
          <option ${status==='Izin'?'selected':''}>Izin</option>
          <option ${status==='Sakit'?'selected':''}>Sakit</option>
          <option ${status==='Alpha'?'selected':''}>Alpha</option>
        </select>
      </td>`;
    attendanceTableBody.appendChild(tr);
  });
}

// save attendance
document.getElementById('form-attendance').addEventListener('submit', function(e){
  e.preventDefault();
  const date = attDateInput.value;
  if(!date){ alert('Pilih tanggal terlebih dahulu'); return; }
  const selects = document.querySelectorAll('#attendance-table select');
  const record = {};
  selects.forEach(s=>{
    record[s.dataset.nis] = s.value;
  });
  const attendance = load(DB.attendanceKey, {});
  attendance[date] = record;
  save(DB.attendanceKey, attendance);
  alert('Presensi tersimpan untuk tanggal ' + date);
});

// ---------- JOURNAL ----------
const journalListBody = document.querySelector('#table-journal tbody');
function renderJournal(){
  const journals = load(DB.journalKey, []);
  journalListBody.innerHTML = '';
  journals.forEach((j,i)=>{
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${i+1}</td><td>${j.date}</td><td>${j.kelas}</td><td>${j.topic}</td><td>${j.notes}</td>
      <td>
        <button class="btn btn-sm btn-warning me-1" onclick="editJournal(${i})">Edit</button>
        <button class="btn btn-sm btn-danger" onclick="deleteJournal(${i})">Hapus</button>
      </td>`;
    journalListBody.appendChild(tr);
  });
}

document.getElementById('btn-add-journal').addEventListener('click', ()=>{
  document.getElementById('journal-index').value='';
  document.getElementById('j-date').valueAsDate = new Date();
  document.getElementById('j-class').value='';
  document.getElementById('j-topic').value='';
  document.getElementById('j-notes').value='';
  document.getElementById('card-add-journal').style.display='block';
});

document.getElementById('btn-cancel-journal').addEventListener('click', ()=>{
  document.getElementById('card-add-journal').style.display='none';
});

document.getElementById('form-journal').addEventListener('submit', function(e){
  e.preventDefault();
  const idx = document.getElementById('journal-index').value;
  const date = document.getElementById('j-date').value;
  const kelas = document.getElementById('j-class').value.trim();
  const topic = document.getElementById('j-topic').value.trim();
  const notes = document.getElementById('j-notes').value.trim();
  const arr = load(DB.journalKey, []);
  if(idx==='') arr.push({date, kelas, topic, notes}); else arr[parseInt(idx)] = {date,kelas,topic,notes};
  save(DB.journalKey, arr);
  document.getElementById('card-add-journal').style.display='none';
  renderJournal();
});

window.editJournal = function(i){
  const arr = load(DB.journalKey, []);
  const j = arr[i];
  document.getElementById('journal-index').value = i;
  document.getElementById('j-date').value = j.date;
  document.getElementById('j-class').value = j.kelas;
  document.getElementById('j-topic').value = j.topic;
  document.getElementById('j-notes').value = j.notes;
  document.getElementById('card-add-journal').style.display='block';
}
window.deleteJournal = function(i){ if(!confirm('Hapus jurnal?')) return; const arr = load(DB.journalKey, []); arr.splice(i,1); save(DB.journalKey, arr); renderJournal(); }

// init UI if already logged in
if(localStorage.getItem('app_current_user')) showApp();

// initial render
renderStudents(); renderJournal(); renderAttendanceRows();
</script><script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script></body>
</html>
