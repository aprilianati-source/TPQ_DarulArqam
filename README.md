
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
  <title>Rumah Qur'an Darul Arqam</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <!-- Firebase SDK (Version 9 CDN Compatibility) -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

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
      transition: max-width 0.3s ease;
    }

    @media screen and (min-width: 768px), (orientation: landscape) {
      .mobile-container { max-width: 95% !important; }
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

    .bottom-nav {
      position: fixed;
      bottom: 0; left: 0; right: 0;
      height: 65px;
      background: white;
      border-top: 1px solid #e0e0e0;
      display: flex;
      justify-content: space-around;
      align-items: center;
      z-index: 1000;
    }

    .bottom-nav-item {
      text-align: center;
      color: #6c757d;
      font-size: 11px;
      text-decoration: none;
      background: none; border: none; flex: 1;
    }

    .bottom-nav-item i { font-size: 18px; display: block; margin-bottom: 2px; }
    .bottom-nav-item.active { color: var(--main-color); font-weight: bold; }

    .preview-img { max-width: 100%; height: auto; border-radius: 10px; margin-top: 8px; }

    .table-responsive {
      border-radius: 10px;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
    }

    th, td {
      white-space: nowrap;
      vertical-align: middle;
      font-size: 12px;
      padding: 6px 8px;
    }

    @media screen and (min-width: 768px), (orientation: landscape) {
      .table-responsive { overflow-x: visible !important; }
      .table-responsive table { width: 100% !important; table-layout: auto !important; }
      th, td { white-space: normal !important; word-wrap: break-word; }
    }
  </style>
</head>
<body>

  <!-- ================= HALAMAN LOGIN ================= -->
  <div id="loginPage" class="mobile-container pt-4">
    <div class="card card-mobile text-center p-4">
      <div class="mb-3">
        <div class="app-logo-wrapper">
          <i class="fa-solid fa-book-quran text-theme display-4" id="defaultLogo"></i>
          <img id="customLogo" src="" class="app-logo d-none" alt="Logo Aplikasi">
        </div>
      </div>

      <h3 class="fw-bold text-theme mb-0">Rumah Qur'an Darul Arqam</h3>
      <p class="text-muted small mb-4">Aplikasi Administrasi & Perkembangan Santri</p>

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
        <label class="form-label fw-bold small" id="labelPassword">Password Santri</label>
        <input type="password" class="form-control form-control-sm" id="loginPassword" placeholder="Masukkan Password">
      </div>

      <button class="btn btn-theme w-100 py-2 shadow-sm" onclick="handleLogin()">
        Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>
    </div>
  </div>

  <!-- ================= HALAMAN DASHBOARD ================= -->
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
          <h5 class="mb-0 fw-bold" id="userRoleTitle">Pengajar</h5>
          <span class="text-muted small" id="userRoleSubtitle">Mustawa</span>
        </div>
      </div>
    </div>

    <!-- VIEW 1: AKTIVITAS & INFORMASI -->
    <div id="viewAktivitas" class="dashboard-view">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-bullhorn me-1"></i> Informasi & Aktivitas</h6>
        <div id="containerAktivitas"></div>
      </div>
    </div>

    <!-- VIEW 2: LAPORAN PENILAIAN -->
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

    <!-- VIEW 3: INPUT NILAI PENGAJAR -->
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

    <!-- VIEW 4: MENU PENGATURAN -->
    <div id="viewPengaturan" class="dashboard-view d-none">
      <!-- FITUR KELOLA SANTRI -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-users-gear me-1"></i> Kelola Data Santri</h6>
        <div class="mb-3">
          <label class="form-label extra-small text-muted fw-bold mb-1" style="font-size:11px;">Tambah Santri Baru</label>
          <div class="input-group input-group-sm">
            <input type="text" id="newSantriName" class="form-control" placeholder="Nama Santri Baru">
            <button class="btn btn-theme" onclick="tambahSantriBaru()"><i class="fa-solid fa-user-plus me-1"></i>Tambah</button>
          </div>
        </div>
        <hr class="my-2">
        <div>
          <label class="form-label extra-small text-muted fw-bold mb-1" style="font-size:11px;">Hapus Santri (Selesai/Lulus/Keluar)</label>
          <div class="input-group input-group-sm">
            <select class="form-select" id="deleteSantriTarget"></select>
            <button class="btn btn-danger fw-bold" onclick="hapusSantriLulus()"><i class="fa-solid fa-user-minus me-1"></i>Hapus Santri</button>
          </div>
        </div>
      </div>

      <!-- UPLOAD INFORMASI -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-upload me-1"></i> Upload Informasi Aktivitas</h6>
        <div class="mb-2"><input type="text" id="infoJudul" class="form-control form-control-sm" placeholder="Judul Informasi"></div>
        <div class="mb-2"><input type="file" id="infoFotoFile" accept="image/*" class="form-control form-control-sm"></div>
        <div class="mb-2"><textarea id="infoDeskripsi" class="form-control form-control-sm" rows="2" placeholder="Tulis keterangan..."></textarea></div>
        <button class="btn btn-theme btn-sm w-100" onclick="simpanAktivitasInfo()">Publikasikan Info</button>
      </div>

      <!-- GANTI LOGO -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-image me-1"></i> Ganti Logo Halaman Login</h6>
        <input type="file" id="inputLogoFile" accept="image/*" class="form-control form-control-sm mb-2">
        <button class="btn btn-theme btn-sm w-100 mb-1" onclick="simpanLogoApp()">Simpan Logo Baru</button>
        <button class="btn btn-outline-secondary btn-sm w-100" onclick="resetLogoApp()">Reset Logo Default</button>
      </div>
    </div>

    <!-- VIEW 5: AKUN -->
    <div id="viewAkun" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3 text-center">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun</h6>
        <button class="btn btn-outline-danger btn-sm w-100 fw-bold py-2" onclick="logout()">
          <i class="fa-solid fa-right-from-bracket me-1"></i> Keluar Aplikasi
        </button>
      </div>
    </div>
  </div>

  <!-- BOTTOM NAVIGATION BAR -->
  <nav class="bottom-nav d-none" id="bottomNav">
    <button class="bottom-nav-item active" onclick="switchView('viewAktivitas', this)"><i class="fa-solid fa-newspaper"></i> Info</button>
    <button class="bottom-nav-item" onclick="switchView('viewLaporan', this)"><i class="fa-solid fa-list-check"></i> Laporan</button>
    <button class="bottom-nav-item d-none" id="navInputNilai" onclick="switchView('viewInputNilai', this)"><i class="fa-solid fa-pen-to-square"></i> Nilai</button>
    <button class="bottom-nav-item d-none" id="navPengaturan" onclick="switchView('viewPengaturan', this)"><i class="fa-solid fa-gear"></i> Pengaturan</button>
    <button class="bottom-nav-item" onclick="switchView('viewAkun', this)"><i class="fa-solid fa-circle-user"></i> Akun</button>
  </nav>

  <!-- JAVASCRIPT SYSTEM WITH REALTIME FIREBASE SYNC -->
  <script>
    // 1. ISI CONFIGURASI FIREBASE ANDA DI SINI:
    const firebaseConfig = {
      apiKey: "PASTE_API_KEY_HERE",
      authDomain: "PROJECT_ID.firebaseapp.com",
      databaseURL: "https://PROJECT_ID-default-rtdb.firebaseio.com",
      projectId: "PROJECT_ID",
      storageBucket: "PROJECT_ID.appspot.com",
      messagingSenderId: "123456789",
      appId: "1:123456789:web:xxxx"
    };

    // Inisialisasi Firebase
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let currentRoleCategory = "";
    let currentClassKey = "";
    let currentUserName = "";
    let appData = { santri: {}, reports: [], aktivitas: [], logo: "" };

    // DAFTAR SANTRI AWAL BERDASARKAN KELAS
    const defaultDataSantri = {
      "Santri Awwal": [
        "Ahmad Arkhan Wiratama", "Aishwa Nasha Razeeta", "Al Afkar Syabani", "Ananda Aisyah Syahidal Syail",
        "Aqila Rafania Adifa", "Arisha Fatimah", "Asyila Rahma Khadijah", "Athaya Humaira Althafia",
        "Bilal Zayyan Prinoza", "Desya Salsaila", "Fatimah", "Kenzie Attaya Depa", "Khuzaimah Summayyah",
        "Muhammad Adzriel Rafif Fakhri", "Muhammad Alfatih Rinaldi", "Muhammad Alzaahiy Rinaldi",
        "Muhammad Dzaky", "Muhammad Fathian Shariq", "Muhammad Gibran Saguftha",
        "Muhammad Ibrahim Al Fatih Isnanto", "Muhammad Razka Destha Athallah", "Muhammad Ustman",
        "Qahirah Arsylia Aftarinda", "Raid Asadel", "Shaqueena Salma Aryanta", "Shofiya Azzahra Hidayatulloh",
        "Sultan Ibrahim Akbar", "Syafia Az Zahra", "Syahfira Destriani", "Syahrika Destriani",
        "Syakira Beatric Setiawan", "Tsanwa Chayra Variin", "Vesha Sakilla", "Yusuf Al Fawwaz",
        "Zea Mikhayla Almeera Yendra"
      ],
      "Santri Tsani": [
        "Abdurrahman", "Akhtar Muhammad Rafasya", "Al Ghany Pratama", "Al Hando Pranstio",
        "Alfarizqi Khairan Yazid", "Anina Yumna Sakhi", "Binar Al Biru Chandra", "Chaerunnisa Fathiyaturahma",
        "Habiburahman El Shirazy", "Hana Shabiya Vina Pakpahan", "Keenan Ghayda Sakhi", "Keisha Chessy Tri Adiva",
        "Khadijah Athiyyah Samreno", "Khaif Shakiel Badillah", "Maryam Intan Dzakiyah", "Molin Sanjaya",
        "Muhamad Ibrahim Hidayatulloh", "Muhammad Al-Ghazello Arief", "Muhammad Hamiz Tabrani",
        "Muhammad Raihan Wildra", "Prisha Humairah", "Qallesha Louis Nawalla", "Risya Naifah Andami",
        "Rosa Adeliya", "Salsabila Putri Ayoenie Alfarizi", "Shaffiyah Mecca Al Fatih Isnanto",
        "Syifa Nursabrina Robka", "Syifa Oktaviani", "Zaim Faqih Alrasyid", "Ziyadah Khaira Pakpahan"
      ],
      "Santri Tsalits": ["Santri Tsalits 1", "Santri Tsalits 2"],
      "Santri Robi": ["Santri Robi 1", "Santri Robi 2"]
    };

    const defaultColumns = [
      "Iqro - Capaian", "Iqro - Catatan", 
      "Hafalan Surat - Murajaah", "Hafalan Surat - Ziyadah", "Hafalan Surat - Catatan",
      "Hafalan Lainnya - Hadits", "Hafalan Lainnya - Matan", "Hafalan Lainnya - Doa",
      "Catatan Akhlak", "Kehadiran (%)"
    ];

    // SINKRONISASI REALTIME DENGAN DATABASE
    window.onload = function() {
      db.ref('appData').on('value', (snapshot) => {
        const val = snapshot.val();
        if (val) {
          appData = val;
          if (!appData.santri) appData.santri = {};
          if (!appData.reports) appData.reports = [];
          if (!appData.aktivitas) appData.aktivitas = [];
        } else {
          // Jika DB Firebase masih kosong, upload daftar santri default
          let initSantri = {};
          Object.keys(defaultDataSantri).forEach(k => {
            initSantri[k] = defaultDataSantri[k].map(n => ({ nama: n, pass: n }));
          });
          appData.santri = initSantri;
          db.ref('appData/santri').set(initSantri);
        }

        loadAppLogo();
        toggleLoginInputs();

        // Update otomatis seluruh elemen jika dashboard sedang terbuka
        if(!document.getElementById('dashboardPage').classList.contains('d-none')) {
          renderAktivitasInfo();
          renderTabelPenilaian();
          if(currentRoleCategory === 'Pengajar') {
            loadDropdownSantriPengajar();
          }
        }
      });
    };

    function toggleLoginInputs() {
      const loginType = document.getElementById('loginType').value;
      const santriSelectGroup = document.getElementById('santriSelectGroup');
      const labelPassword = document.getElementById('labelPassword');
      const loginSantriName = document.getElementById('loginSantriName');

      loginSantriName.innerHTML = "";

      if (loginType.startsWith('Santri')) {
        santriSelectGroup.classList.remove('d-none');
        labelPassword.innerText = "Password Santri";

        let list = appData.santri[loginType] || [];
        list.forEach(s => {
          let opt = document.createElement('option');
          opt.value = s.nama;
          opt.innerText = s.nama;
          loginSantriName.appendChild(opt);
        });
      } else {
        santriSelectGroup.classList.add('d-none');
        labelPassword.innerText = "Password Pengajar";
      }
    }

    function handleLogin() {
      const loginType = document.getElementById('loginType').value;
      const passwordInput = document.getElementById('loginPassword').value;

      if (loginType.startsWith('Pengajar')) {
        if (passwordInput === "darul123") { // Password Utama Pengajar
          currentRoleCategory = "Pengajar";
          currentClassKey = loginType.replace("Pengajar ", "");
          bukaDashboard();
        } else {
          alert("Password Pengajar Salah!");
        }
      } else {
        const selectedSantri = document.getElementById('loginSantriName').value;
        let santriObj = (appData.santri[loginType] || []).find(s => s.nama === selectedSantri);

        if (santriObj && (santriObj.pass === passwordInput || passwordInput === selectedSantri)) {
          currentRoleCategory = "Santri";
          currentClassKey = loginType.replace("Santri ", "");
          currentUserName = selectedSantri;
          bukaDashboard();
        } else {
          alert("Password Santri Salah!");
        }
      }
    }

    function bukaDashboard() {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');
      document.getElementById('bottomNav').classList.remove('d-none');

      document.getElementById('userRoleTitle').innerText = currentRoleCategory === 'Pengajar' ? `Pengajar ${currentClassKey}` : currentUserName;
      document.getElementById('userRoleSubtitle').innerText = `Mustawa ${currentClassKey}`;

      if (currentRoleCategory === 'Pengajar') {
        document.getElementById('navInputNilai').classList.remove('d-none');
        document.getElementById('navPengaturan').classList.remove('d-none');
        loadDropdownSantriPengajar();
        renderFormInputsPenilaian();
      } else {
        document.getElementById('navInputNilai').classList.add('d-none');
        document.getElementById('navPengaturan').classList.add('d-none');
      }

      renderAktivitasInfo();
      renderTabelPenilaian();
      switchView('viewAktivitas', document.querySelector('.bottom-nav-item'));
    }

    function switchView(viewId, btnEl) {
      document.querySelectorAll('.dashboard-view').forEach(el => el.classList.add('d-none'));
      document.getElementById(viewId).classList.remove('d-none');
      document.querySelectorAll('.bottom-nav-item').forEach(el => el.classList.remove('active'));
      if(btnEl) btnEl.classList.add('active');
    }

    // UPLOAD LOGO KE SERVER
    function simpanLogoApp() {
      const fileInput = document.getElementById('inputLogoFile');
      if (fileInput.files && fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = e => {
          appData.logo = e.target.result;
          db.ref('appData/logo').set(e.target.result);
          alert('Logo berhasil diperbarui untuk semua HP!');
        };
        reader.readAsDataURL(fileInput.files[0]);
      }
    }

    function resetLogoApp() {
      db.ref('appData/logo').remove();
      alert('Logo dikembalikan ke default.');
    }

    function loadAppLogo() {
      const logoImg = document.getElementById('customLogo');
      const defaultIcon = document.getElementById('defaultLogo');
      if (appData.logo) {
        logoImg.src = appData.logo;
        logoImg.classList.remove('d-none');
        defaultIcon.classList.add('d-none');
      } else {
        logoImg.classList.add('d-none');
        defaultIcon.classList.remove('d-none');
      }
    }

    // TAMBAH SANTRI (LANGSUNG GABUNG KE INPUT PENILAIAN & DATABASE)
    function tambahSantriBaru() {
      const newName = document.getElementById('newSantriName').value.trim();
      if (!newName) return alert('Isi nama santri!');
      const classKey = "Santri " + currentClassKey;
      if (!appData.santri[classKey]) appData.santri[classKey] = [];

      // Periksa apakah nama sudah terdaftar
      if (appData.santri[classKey].some(s => s.nama === newName)) {
        return alert('Nama santri sudah terdaftar!');
      }

      // Tambahkan ke array database
      appData.santri[classKey].push({ nama: newName, pass: newName });
      
      // Simpan ke Firebase Realtime DB
      db.ref('appData/santri').set(appData.santri, (error) => {
        if (error) {
          alert('Gagal menyimpan data.');
        } else {
          alert(`Santri "${newName}" berhasil ditambahkan dan siap dinilai!`);
          document.getElementById('newSantriName').value = '';
          
          // Pembaruan langsung ke Form Penilaian & Hapus
          loadDropdownSantriPengajar();
        }
      });
    }

    function hapusSantriLulus() {
      const target = document.getElementById('deleteSantriTarget').value;
      if (!target) return alert('Pilih nama santri!');
      const classKey = "Santri " + currentClassKey;

      if (confirm(`Apakah Anda yakin menghapus santri "${target}"?`)) {
        appData.santri[classKey] = appData.santri[classKey].filter(s => s.nama !== target);
        
        // Hapus juga laporan nilai terkait santri tersebut
        if (appData.reports) {
          appData.reports = appData.reports.filter(r => !(r.kelasKey === currentClassKey && r.nama === target));
          db.ref('appData/reports').set(appData.reports);
        }

        db.ref('appData/santri').set(appData.santri, (err) => {
          if(!err) {
            alert(`Santri "${target}" telah berhasil dihapus.`);
            loadDropdownSantriPengajar();
            renderTabelPenilaian();
          }
        });
      }
    }

    // MEMUAT DROPDOWN SANTRI PADA MENU INPUT NILAI & PENGATURAN
    function loadDropdownSantriPengajar() {
      const classKey = "Santri " + currentClassKey;
      const list = appData.santri[classKey] || [];
      const targetInput = document.getElementById('inputSantriTarget');
      const targetDelete = document.getElementById('deleteSantriTarget');

      if(targetInput) targetInput.innerHTML = '<option value="">-- Pilih Santri --</option>';
      if(targetDelete) targetDelete.innerHTML = '<option value="">-- Pilih Santri --</option>';

      list.forEach(s => {
        if(targetInput) targetInput.innerHTML += `<option value="${s.nama}">${s.nama}</option>`;
        if(targetDelete) targetDelete.innerHTML += `<option value="${s.nama}">${s.nama}</option>`;
      });
    }

    // INPUT PENILAIAN
    function renderFormInputsPenilaian() {
      const container = document.getElementById('dynamicFormInputs');
      container.innerHTML = '';
      defaultColumns.forEach((col, idx) => {
        container.innerHTML += `
          <div class="col-6 mb-2">
            <label class="form-label fw-bold small text-muted mb-1" style="font-size:11px;">${col}</label>
            <input type="text" id="col_input_${idx}" class="form-control form-control-sm" placeholder="Isi...">
          </div>
        `;
      });
    }

    function simpanDataPenilaian(e) {
      e.preventDefault();
      const namaTarget = document.getElementById('inputSantriTarget').value;
      if(!namaTarget) return alert('Pilih nama santri!');

      let recordValues = {};
      defaultColumns.forEach((col, idx) => {
        recordValues[col] = document.getElementById(`col_input_${idx}`).value || '-';
      });

      let reports = appData.reports || [];
      let idx = reports.findIndex(r => r.kelasKey === currentClassKey && r.nama === namaTarget);
      if (idx !== -1) {
        reports[idx].values = recordValues;
      } else {
        reports.push({ kelasKey: currentClassKey, nama: namaTarget, values: recordValues });
      }

      db.ref('appData/reports').set(reports, (err) => {
        if(!err) {
          alert('Penilaian berhasil tersimpan!');
          document.getElementById('inputSantriTarget').value = '';
          defaultColumns.forEach((_, i) => document.getElementById(`col_input_${i}`).value = '');
          renderTabelPenilaian();
          switchView('viewLaporan', document.querySelectorAll('.bottom-nav-item')[1]);
        }
      });
    }

    function renderTabelPenilaian() {
      const thead = document.getElementById('tabelHeader');
      const tbody = document.getElementById('tabelDataSantri');

      let headerHtml = `<tr><th>No</th><th>Nama Santri</th>`;
      defaultColumns.forEach(col => headerHtml += `<th>${col}</th>`);
      headerHtml += `</tr>`;
      thead.innerHTML = headerHtml;

      tbody.innerHTML = '';
      let reports = appData.reports || [];
      let filtered = reports.filter(r => r.kelasKey === currentClassKey);
      if (currentRoleCategory === 'Santri') filtered = filtered.filter(r => r.nama === currentUserName);

      if (filtered.length === 0) {
        tbody.innerHTML = `<tr><td colspan="${defaultColumns.length + 2}" class="text-muted py-3">Belum ada data nilai.</td></tr>`;
        return;
      }

      filtered.forEach((item, index) => {
        let row = `<tr><td>${index + 1}</td><td class="fw-bold text-start">${item.nama}</td>`;
        defaultColumns.forEach(col => row += `<td>${item.values[col] || '-'}</td>`);
        row += `</tr>`;
        tbody.innerHTML += row;
      });
    }

    // INFORMASI AKTIVITAS
    function simpanAktivitasInfo() {
      const judul = document.getElementById('infoJudul').value;
      const deskripsi = document.getElementById('infoDeskripsi').value;
      const fileInput = document.getElementById('infoFotoFile');

      if(!judul || !deskripsi) return alert('Judul dan deskripsi wajib diisi!');

      const save = (foto) => {
        let list = appData.aktivitas || [];
        list.unshift({ judul, deskripsi, foto: foto || '', waktu: new Date().toLocaleDateString('id-ID') });
        db.ref('appData/aktivitas').set(list, (err) => {
          if(!err) {
            alert('Informasi dipublikasikan!');
            document.getElementById('infoJudul').value = '';
            document.getElementById('infoDeskripsi').value = '';
            document.getElementById('infoFotoFile').value = '';
            switchView('viewAktivitas', document.querySelector('.bottom-nav-item'));
          }
        });
      };

      if (fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = e => save(e.target.result);
        reader.readAsDataURL(fileInput.files[0]);
      } else {
        save('');
      }
    }

    function renderAktivitasInfo() {
      const container = document.getElementById('containerAktivitas');
      const list = appData.aktivitas || [];
      container.innerHTML = '';

      if (list.length === 0) {
        container.innerHTML = `<div class="text-center text-muted small py-3">Belum ada informasi/aktivitas.</div>`;
        return;
      }

      list.forEach((item) => {
        let fotoHtml = item.foto ? `<img src="${item.foto}" class="preview-img mb-2">` : '';
        container.innerHTML += `
          <div class="border-bottom pb-3 mb-3">
            <h6 class="fw-bold text-theme mb-1">${item.judul}</h6>
            <small class="text-muted d-block mb-2" style="font-size:10px;">${item.waktu}</small>
            ${fotoHtml}
            <p class="small text-secondary mb-0">${item.deskripsi}</p>
          </div>
        `;
      });
    }

    function logout() {
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('bottomNav').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
    }
  </script>
</body>
</html>
