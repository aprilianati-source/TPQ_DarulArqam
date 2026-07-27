
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Rumah Qur'an Darul Arqam - Realtime</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database-compat.js"></script>

  <style>
    :root {
      --bg-color: #eef7ed;
      --main-color: #157347;
      --dark-color: #0d512f;
      --text-color: #212529;
    }
    
    body { 
      background-color: var(--bg-color); 
      color: var(--text-color);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      padding-bottom: 90px;
      transition: background-color 0.3s ease;
    }

    .mobile-container {
      max-width: 500px;
      margin: 0 auto;
      padding: 12px;
    }

    .card-mobile {
      border: 1.5px solid rgba(0, 0, 0, 0.1);
      border-radius: 20px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.03);
      background: white;
      overflow: hidden;
    }

    .btn-theme {
      background-color: var(--main-color);
      color: white;
      font-weight: 600;
      border-radius: 12px;
      border: none;
      transition: background-color 0.3s ease;
    }
    .btn-theme:hover, .btn-theme:active {
      background-color: var(--dark-color);
      color: white;
    }

    .text-theme { color: var(--main-color) !important; }

    .app-logo-wrapper {
      width: 110px;
      height: 110px;
      margin: 0 auto;
      border-radius: 20px;
      background: #ffffff;
      border: 2px solid var(--main-color);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      box-shadow: 0 4px 10px rgba(0,0,0,0.06);
    }

    .app-logo { 
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .user-profile-img {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid var(--main-color);
    }

    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: 65px;
      background: white;
      border-top: 1px solid #e0e0e0;
      display: flex;
      justify-content: space-around;
      align-items: center;
      z-index: 1000;
      box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
    }

    .bottom-nav-item {
      text-align: center;
      color: #6c757d;
      font-size: 11px;
      text-decoration: none;
      background: none;
      border: none;
      flex: 1;
    }

    .bottom-nav-item i {
      font-size: 18px;
      display: block;
      margin-bottom: 2px;
    }

    .bottom-nav-item.active {
      color: var(--main-color);
      font-weight: bold;
    }

    .preview-img {
      max-width: 100%;
      height: auto;
      border-radius: 10px;
      margin-top: 8px;
    }

    .table-responsive {
      border-radius: 10px;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      margin-bottom: 0;
    }

    .table-responsive::-webkit-scrollbar {
      height: 4px;
    }
    .table-responsive::-webkit-scrollbar-thumb {
      background: var(--main-color);
      border-radius: 10px;
    }

    th, td {
      white-space: nowrap;
      vertical-align: middle;
    }

    /* Loading & Status */
    .loading-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(255,255,255,0.9);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 9999;
    }
    .status-bar {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      padding: 6px;
      text-align: center;
      font-size: 12px;
      z-index: 999;
    }
    .status-offline { background: #fff3cd; color: #856404; }
    .status-online { background: #d1e7dd; color: #0f5132; }
  </style>
</head>
<body>

  <!-- Status Koneksi -->
  <div id="statusBar" class="status-bar d-none"></div>
  <!-- Loading Layar -->
  <div id="loadingOverlay" class="loading-overlay">
    <div class="text-center">
      <div class="spinner-border text-theme mb-3" role="status"></div>
      <p class="fw-bold">Menghubungkan ke Server...</p>
    </div>
  </div>

  <!-- HALAMAN LOGIN -->
  <div id="loginPage" class="mobile-container pt-4 d-none">
    <div class="card card-mobile text-center p-4">
      <div class="mb-3">
        <div class="app-logo-wrapper">
          <i class="fa-solid fa-book-quran text-theme display-4" id="defaultLogo"></i>
          <img id="customLogo" src="" class="app-logo d-none" alt="Logo Aplikasi">
        </div>
      </div>
      <h3 class="fw-bold text-theme mb-0">Rumah Qur'an Darul Arqam</h3>
      <p class="text-muted small mb-4">Sistem Administrasi & Perkembangan Santri Realtime</p>

      <div class="mb-3 text-start">
        <label class="form-label fw-bold small">Tipe Login</label>
        <select class="form-select form-select-sm" id="loginType" onchange="toggleLoginInputs()">
          <optgroup label="Akses Santri / Wali Santri">
            <option value="Santri Awwal">Santri Mustawa Awwal</option>
            <option value="Santri Tsani">Santri Mustawa Tsani</option>
            <option value="Santri Tsalits">Santri Mustawa Tsalits</option>
            <option value="Santri Robi">Santri Mustawa Robi'</option>
          </optgroup>
          <optgroup label="Akses Pengajar">
            <option value="Pengajar Awwal">Pengajar Kelas Awwal</option>
            <option value="Pengajar Tsani">Pengajar Kelas Tsani</option>
            <option value="Pengajar Tsalits">Pengajar Kelas Tsalits</option>
            <option value="Pengajar Robi">Pengajar Kelas Robi'</option>
          </optgroup>
        </select>
      </div>

      <div class="mb-3 text-start" id="santriSelectGroup">
        <label class="form-label fw-bold small">Pilih Nama Santri</label>
        <select class="form-select form-select-sm" id="loginSantriName"></select>
      </div>

      <div class="mb-3 text-start">
        <label class="form-label fw-bold small" id="labelPassword">Password</label>
        <input type="password" class="form-control form-control-sm" id="loginPassword" placeholder="Masukkan Password">
      </div>

      <button class="btn btn-theme w-100 py-2 shadow-sm" onclick="handleLogin()">
        Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>
    </div>
  </div>

  <!-- HALAMAN DASHBOARD -->
  <div id="dashboardPage" class="mobile-container d-none">
    <div class="mb-3">
      <h2 class="fw-bold text-theme text-decoration-underline">Rumah Qur'an Darul Arqam</h2>
    </div>
    <div class="card card-mobile p-3 mb-3">
      <div class="d-flex align-items-center">
        <div class="me-3" id="headerAvatarContainer">
          <i class="fa-solid fa-circle-user fs-1 text-theme"></i>
        </div>
        <div>
          <h5 class="mb-0 fw-bold" id="userRoleTitle"></h5>
          <span class="text-muted small" id="userRoleSubtitle"></span>
        </div>
      </div>
    </div>

    <!-- VIEW INFO -->
    <div id="viewAktivitas" class="dashboard-view">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-bullhorn me-1"></i> Informasi & Aktivitas</h6>
        <div id="containerAktivitas"></div>
      </div>
    </div>

    <!-- VIEW LAPORAN -->
    <div id="viewLaporan" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-table me-2"></i>Laporan Perkembangan</h6>
        <div class="table-responsive">
          <table class="table table-bordered align-middle text-center small mb-0">
            <thead class="table-dark" id="tabelHeader"></thead>
            <tbody id="tabelDataSantri"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- VIEW INPUT NILAI (PENGAJAR) -->
    <div id="viewInputNilai" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-pen-to-square me-2"></i>Form Input Penilaian</h6>
        <form id="formPenilaian" onsubmit="simpanDataPenilaian(event)">
          <div class="mb-3">
            <label class="form-label fw-bold small">Pilih Santri</label>
            <select class="form-select form-select-sm" id="inputSantriTarget" required></select>
          </div>
          <div id="dynamicFormInputs" class="row g-2"></div>
          <button type="submit" class="btn btn-theme w-100 py-2 mt-3 shadow-sm">Simpan Penilaian</button>
        </form>
      </div>
    </div>

    <!-- VIEW PENGATURAN (PENGAJAR) -->
    <div id="viewPengaturan" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-columns me-1"></i> Kelola Kolom Penilaian</h6>
        <div class="mb-3">
          <input type="text" id="newColumnName" class="form-control" placeholder="Nama Kolom Baru">
          <button class="btn btn-theme btn-sm mt-2" onclick="tambahKolomBaru()"><i class="fa-solid fa-plus"></i> Tambah</button>
        </div>
        <hr class="my-2">
        <div class="mb-2">
          <select class="form-select" id="deleteColumnSelect"></select>
          <button class="btn btn-danger btn-sm mt-2" onclick="hapusKolomPilihan()"><i class="fa-solid fa-trash"></i> Hapus</button>
        </div>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-upload me-1"></i> Upload Informasi</h6>
        <input type="text" id="infoJudul" class="form-control form-control-sm mb-2" placeholder="Judul">
        <textarea id="infoDeskripsi" class="form-control form-control-sm mb-2" rows="2" placeholder="Keterangan"></textarea>
        <button class="btn btn-theme btn-sm w-100" onclick="simpanAktivitasInfo()">Publikasikan</button>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-user-plus me-1"></i> Tambah Santri</h6>
        <input type="text" id="newSantriName" class="form-control mb-2" placeholder="Nama Santri">
        <button class="btn btn-theme btn-sm w-100" onclick="tambahSantriBaru()">Tambah</button>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-palette me-1"></i> Pengaturan Tema</h6>
        <select class="form-select form-select-sm mb-2" id="themeColorSelect">
          <option value="hijau">Hijau</option>
          <option value="kuning">Kuning</option>
          <option value="biru">Biru</option>
          <option value="merah">Merah</option>
        </select>
        <button class="btn btn-theme btn-sm w-100" onclick="simpanWarnaTema()">Terapkan</button>
      </div>
    </div>

    <!-- VIEW AKUN -->
    <div id="viewAkun" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun</h6>
        <div class="mb-3">
          <input type="password" id="userNewPassInput" class="form-control form-control-sm mb-2" placeholder="Password Baru">
          <button class="btn btn-theme btn-sm w-100" onclick="simpanGantiPassword()">Ubah Password</button>
        </div>
        <hr>
        <button class="btn btn-outline-danger btn-sm w-100 fw-bold py-2" onclick="logout()">
          <i class="fa-solid fa-right-from-bracket me-1"></i> Keluar
        </button>
      </div>
    </div>
  </div>

  <!-- NAVIGASI BAWAH -->
  <nav class="bottom-nav d-none" id="bottomNav">
    <button class="bottom-nav-item active" onclick="switchView('viewAktivitas', this)">
      <i class="fa-solid fa-newspaper"></i> Info
    </button>
    <button class="bottom-nav-item" onclick="switchView('viewLaporan', this)">
      <i class="fa-solid fa-list-check"></i> Laporan
    </button>
    <button class="bottom-nav-item d-none" id="navInputNilai" onclick="switchView('viewInputNilai', this)">
      <i class="fa-solid fa-pen-to-square"></i> Nilai
    </button>
    <button class="bottom-nav-item d-none" id="navPengaturan" onclick="switchView('viewPengaturan', this)">
      <i class="fa-solid fa-gear"></i> Pengaturan
    </button>
    <button class="bottom-nav-item" onclick="switchView('viewAkun', this)">
      <i class="fa-solid fa-circle-user"></i> Akun
    </button>
  </nav>

  <script>
    // ==============================================
    // 1. KONFIGURASI FIREBASE (GANTI DENGAN MILIK ANDA)
    // ==============================================
    const firebaseConfig = {
      apiKey: "API_KEY_ANDA",
      authDomain: "PROYEK_ANDA.firebaseapp.com",
      databaseURL: "https://PROYEK_ANDA-default-rtdb.asia-southeast1.firebasedatabase.app",
      projectId: "PROYEK_ANDA",
      storageBucket: "PROYEK_ANDA.appspot.com",
      messagingSenderId: "ID_PESAN_ANDA",
      appId: "ID_APLIKASI_ANDA"
    };

    // Inisialisasi Firebase
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.database();

    // Variabel Global
    let currentUser = null;
    let currentRoleCategory = "";
    let currentClassKey = "";
    let currentUserName = "";
    let dbListeners = [];

    // ==============================================
    // 2. SISTEM KONEKSI & STATUS
    // ==============================================
    function pantauKoneksi() {
      const statusBar = document.getElementById('statusBar');
      const connectedRef = db.ref(".info/connected");
      
      connectedRef.on("value", (snap) => {
        if (snap.val() === true) {
          statusBar.className = "status-bar status-online";
          statusBar.textContent = "✅ Terhubung ke Server";
          setTimeout(() => statusBar.classList.add('d-none'), 2000);
        } else {
          statusBar.className = "status-bar status-offline";
          statusBar.textContent = "⚠️ Terputus! Menyambung ulang...";
          statusBar.classList.remove('d-none');
        }
      });
    }

    // ==============================================
    // 3. INISIALISASI AWAL & CEK SESI
    // ==============================================
    window.onload = () => {
      pantauKoneksi();
      
      auth.onAuthStateChanged(user => {
        document.getElementById('loadingOverlay').classList.add('d-none');
        if (user) {
          currentUser = user;
          muatDataPengguna();
        } else {
          document.getElementById('loginPage').classList.remove('d-none');
          toggleLoginInputs();
        }
      });
    };

    // ==============================================
    // 4. FUNGSI UTILITAS & BANTUAN
    // ==============================================
    const themePresets = {
      "hijau": { bg: "#eef7ed", main: "#157347", dark: "#0d512f" },
      "kuning": { bg: "#fffdf0", main: "#d4a017", dark: "#997300" },
      "biru": { bg: "#edf4fc", main: "#0d6efd", dark: "#0a58ca" },
      "merah": { bg: "#fceded", main: "#dc3545", dark: "#b02a37" }
    };

    function terapkanWarnaTema(themeKey) {
      const t = themePresets[themeKey] || themePresets["hijau"];
      document.documentElement.style.setProperty('--bg-color', t.bg);
      document.documentElement.style.setProperty('--main-color', t.main);
      document.documentElement.style.setProperty('--dark-color', t.dark);
    }

    function showLoading(show) {
      document.getElementById('loadingOverlay').classList.toggle('d-none', !show);
    }

    function tampilPesan(teks, tipe='info') {
      alert(teks);
    }

    // ==============================================
    // 5. SISTEM LOGIN & OTENTIKASI
    // ==============================================
    function toggleLoginInputs() {
      const t = document.getElementById('loginType').value;
      const grp = document.getElementById('santriSelectGroup');
      const lbl = document.getElementById('labelPassword');
      document.getElementById('loginSantriName').innerHTML = "";

      if (t.startsWith('Santri')) {
        grp.classList.remove('d-none');
        lbl.innerText = "Password Santri";
        db.ref(`santri/${t}`).once('value', snap => {
          snap.forEach(data => {
            const opt = `<option value="${data.val().nama}">${data.val().nama}</option>`;
            document.getElementById('loginSantriName').innerHTML += opt;
          });
        });
      } else {
        grp.classList.add('d-none');
        lbl.innerText = "Password Pengajar";
      }
    }

    async function handleLogin() {
      showLoading(true);
      const tipe = document.getElementById('loginType').value;
      const pass = document.getElementById('loginPassword').value;

      try {
        if (tipe.startsWith('Pengajar')) {
          const snap = await db.ref(`pengajar/${tipe}`).once('value');
          if (snap.exists() && snap.val().password === pass) {
            currentRoleCategory = "Pengajar";
            currentClassKey = tipe.replace("Pengajar ", "");
            await auth.signInAnonymously();
            bukaDashboard();
          } else throw "Password salah";
        } else {
          const nama = document.getElementById('loginSantriName').value;
          const snap = await db.ref(`santri/${tipe}`).once('value');
          let ditemukan = false;
          snap.forEach(d => {
            if (d.val().nama === nama && d.val().password === pass) {
              currentRoleCategory = "Santri";
              currentClassKey = tipe.replace("Santri ", "");
              currentUserName = nama;
              ditemukan = true;
            }
          });
          if (!ditemukan) throw "Nama atau Password salah";
          await auth.signInAnonymously();
          bukaDashboard();
        }
      } catch (err) {
        tampilPesan("Gagal Masuk: " + err);
      } finally {
        showLoading(false);
      }
    }

    function muatDataPengguna() {
      db.ref(`pengaturan/tema`).once('value', s => terapkanWarnaTema(s.val() || 'hijau'));
      db.ref(`pengaturan/logo`).once('value', s => {
        if(s.val()) {
          document.getElementById('customLogo').src = s.val();
          document.getElementById('customLogo').classList.remove('d-none');
          document.getElementById('defaultLogo').classList.add('d-none');
        }
      });
    }

    function bukaDashboard() {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');
      document.getElementById('bottomNav').classList.remove('d-none');

      document.getElementById('userRoleTitle').innerText = 
        currentRoleCategory === 'Pengajar' ? `Pengajar Kelas ${currentClassKey}` : currentUserName;
      document.getElementById('userRoleSubtitle').innerText = `Mustawa ${currentClassKey}`;

      document.getElementById('navInputNilai').classList.toggle('d-none', currentRoleCategory !== 'Pengajar');
      document.getElementById('navPengaturan').classList.toggle('d-none', currentRoleCategory !== 'Pengajar');

      pasangPendengarRealtime();
      switchView('viewAktivitas', document.querySelector('.bottom-nav-item'));
    }

    // ==============================================
    // 6. PEMANTAUAN DATA REALTIME
    // ==============================================
    function pasangPendengarRealtime() {
      // Hapus pendengar lama jika ada
      dbListeners.forEach(l => db.ref().off(l));
      dbListeners = [];

      // Pantau Info Aktivitas
      const infoRef = db.ref('aktivitas');
      infoRef.on('value', s => {
        renderAktivitasInfo(s.val() || []);
      });
      dbListeners.push(infoRef);

      // Pantau Kolom Penilaian
      const kolomRef = db.ref('kolom_nilai');
      kolomRef.on('value', s => {
        renderFormInputsPenilaian(s.val() || []);
        renderDropdownHapusKolom(s.val() || []);
      });
      dbListeners.push(kolomRef);

      // Pantau Data Nilai
      const nilaiRef = db.ref('nilai/' + currentClassKey);
      nilaiRef.on('value', s => {
        renderTabelPenilaian(s.val() || {});
      });
      dbListeners.push(nilaiRef);
    }

    // ==============================================
    // 7. RENDER & FUNGSI DATA
    // ==============================================
    function switchView(viewId, btn) {
      document.querySelectorAll('.dashboard-view').forEach(e => e.classList.add('d-none'));
      document.getElementById(viewId).classList.remove('d-none');
      document.querySelectorAll('.bottom-nav-item').forEach(e => e.classList.remove('active'));
      if(btn) btn.classList.add('active');
    }

    function renderAktivitasInfo(data) {
      const el = document.getElementById('containerAktivitas');
      if(!data || data.length === 0) {
        el.innerHTML = `<div class="text-center text-muted py-3">Belum ada informasi.</div>`;
        return;
      }
      el.innerHTML = '';
      Object.values(data).reverse().forEach(item => {
        el.innerHTML += `
          <div class="border-bottom pb-3 mb-3">
            <h6 class="fw-bold text-theme mb-1">${item.judul}</h6>
            <small class="text-muted d-block mb-2">${item.waktu}</small>
            <p class="small mb-0">${item.deskripsi}</p>
          </div>`;
      });
    }

    function renderFormInputsPenilaian(kolom) {
      const el = document.getElementById('dynamicFormInputs');
      const pilih = document.getElementById('inputSantriTarget');
      el.innerHTML = ''; pilih.innerHTML = '<option value="">-- Pilih Santri --</option>';
      
      db.ref(`santri/Santri ${currentClassKey}`).once('value', s => {
        s.forEach(d => pilih.innerHTML += `<option value="${d.val().nama}">${d.val().nama}</option>`);
      });

      kolom.forEach((k, i) => {
        el.innerHTML += `<div class="col-6 mb-2"><label class="form-label small">${k}</label><input type="text" id="col_${i}" class="form-control form-control-sm"></div>`;
      });
    }

    function renderTabelPenilaian(dataNilai) {
      const hd = document.getElementById('tabelHeader');
      const bd = document.getElementById('tabelDataSantri');
      db.ref('kolom_nilai').once('value', async kolomSnap => {
        const kolom = kolomSnap.val() || [];
        hd.innerHTML = `<tr><th>No</th><th>Nama</th>`;
        kolom.forEach(k => hd.innerHTML += `<th>${k}</th>`);
        hd.innerHTML += `</tr>`; bd.innerHTML = '';

        let list = Object.values(dataNilai).filter(d => 
          currentRoleCategory === 'Santri' ? d.nama === currentUserName : true
        );

        if(list.length === 0) {
          bd.innerHTML = `<tr><td colspan="${kolom.length + 2}" class="text-muted">Belum ada data</td></tr>`;
          return;
        }

        list.forEach((d, i) => {
          let baris = `<tr><td>${i+1}</td><td class="fw-bold text-start">${d.nama}</td>`;
          kolom.forEach(k => baris += `<td>${d.values[k] || '-'}</td>`);
          bd.innerHTML += baris + `</tr>`;
        });
      });
    }

    function renderDropdownHapusKolom(kolom) {
      const el = document.getElementById('deleteColumnSelect');
      el.innerHTML = '<option value="">-- Pilih Kolom --</option>';
      kolom.forEach(k => el.innerHTML += `<option value="${k}">${k}</option>`);
    }

    // ==============================================
    // 8. FUNGSI SIMPAN DATA KE CLOUD
    // ==============================================
    async function simpanDataPenilaian(e) {
      e.preventDefault(); showLoading(true);
      const nama = document.getElementById('inputSantriTarget').value;
      if(!nama) return tampilPesan("Pilih Santri dulu");

      const kolomSnap = await db.ref('kolom_nilai').once('value');
      const kolom = kolomSnap.val() || [];
      const data = {};
      kolom.forEach((k, i) => data[k] = document.getElementById(`col_${i}`).value || '-');

      await db.ref(`nilai/${currentClassKey}/${nama.replace(/\W/g,'_')}`).set({
        nama: nama, values: data
      });
      tampilPesan("Data tersimpan!"); showLoading(false);
    }

    async function tambahKolomBaru() {
      const nama = document.getElementById('newColumnName').value.trim();
      if(!nama) return;
      const kolom = await db.ref('kolom_nilai').once('value');
      let arr = kolom.val() || [];
      if(arr.includes(nama)) return tampilPesan("Sudah ada");
      arr.push(nama);
      await db.ref('kolom_nilai').set(arr);
      document.getElementById('newColumnName').value = '';
    }

    async function hapusKolomPilihan() {
      const pilih = document.getElementById('deleteColumnSelect').value;
      if(!pilih || !confirm("Yakin hapus?")) return;
      let arr = await db.ref('kolom_nilai').once('value').then(s => s.val() || []);
      arr = arr.filter(k => k !== pilih);
      await db.ref('kolom_nilai').set(arr);
    }

    async function simpanAktivitasInfo() {
      const judul = document.getElementById('infoJudul').value;
      const isi = document.getElementById('infoDeskripsi').value;
      if(!judul || !isi) return tampilPesan("Lengkapi data");
      await db.ref('aktivitas').push({
        judul: judul, deskripsi: isi, waktu: new Date().toLocaleDateString('id-ID')
      });
      document.getElementById('infoJudul').value = '';
      document.getElementById('infoDeskripsi').value = '';
      tampilPesan("Info dipublikasikan");
    }

    async function tambahSantriBaru() {
      const nama = document.getElementById('newSantriName').value.trim();
      if(!nama) return;
      await db.ref(`santri/Santri ${currentClassKey}/${nama.replace(/\W/g,'_')}`).set({
        nama: nama, password: nama
      });
      document.getElementById('newSantriName').value = '';
      tampilPesan("Santri ditambahkan");
    }

    async function simpanWarnaTema() {
      const pilih = document.getElementById('themeColorSelect').value;
      await db.ref('pengaturan/tema').set(pilih);
      terapkanWarnaTema(pilih);
      tampilPesan("Tema diperbarui");
    }

    async function simpanGantiPassword() {
      const pass = document.getElementById('userNewPassInput').value.trim();
      if(!pass) return;
      if(currentRoleCategory === 'Pengajar') {
        await db.ref(`pengajar/Pengajar ${currentClassKey}/password`).set(pass);
      } else {
        await db.ref(`santri/Santri ${currentClassKey}/${currentUserName.replace(/\W/g,'_')}/password`).set(pass);
      }
      document.getElementById('userNewPassInput').value = '';
      tampilPesan("Password diubah");
    }

    function logout() {
      auth.signOut();
      dbListeners.forEach(l => db.ref().off(l));
      dbListeners = [];
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('bottomNav').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
      document.getElementById('loginPassword').value = '';
    }
  </script>
</body>
</html>
