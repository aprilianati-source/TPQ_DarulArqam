<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Rumah Qur'an Darul Arqam</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
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

    /* Logo Wrapper */
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

    /* Bottom Nav Bar */
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

    /* Perbaikan CSS Tabel & Scrollbar */
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

    <!-- User Header Status -->
    <div class="card card-mobile p-3 mb-3">
      <div class="d-flex align-items-center">
        <div class="me-3" id="headerAvatarContainer">
          <i class="fa-solid fa-circle-user fs-1 text-theme"></i>
        </div>
        <div>
          <h5 class="mb-0 fw-bold" id="userRoleTitle">Pengajar Kelas Awwal</h5>
          <span class="text-muted small" id="userRoleSubtitle">Mustawa Awwal</span>
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

    <!-- VIEW 3: INPUT NILAI (KHUSUS PENGAJAR) -->
    <div id="viewInputNilai" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-pen-to-square me-2"></i>Form Input Penilaian</h6>
        <form id="formPenilaian" onsubmit="simpanDataPenilaian(event)">
          <div class="mb-3">
            <label class="form-label fw-bold small">Pilih Santri</label>
            <select class="form-select form-select-sm" id="inputSantriTarget" required></select>
          </div>
          
          <div id="dynamicFormInputs" class="row g-2"></div>
          
          <button type="submit" class="btn btn-theme w-100 py-2 mt-3 shadow-sm">
            Simpan Penilaian
          </button>
        </form>
      </div>
    </div>

    <!-- VIEW 4: MENU PENGATURAN (KHUSUS PENGAJAR) -->
    <div id="viewPengaturan" class="dashboard-view d-none">
      
      <!-- FITUR TAMBAH & HAPUS KOLOM PENILAIAN -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-columns me-1"></i> Kelola Kolom Penilaian</h6>
        
        <!-- Tambah Kolom -->
        <div class="mb-3">
          <label class="form-label extra-small text-muted fw-bold mb-1" style="font-size:11px;">Tambah Kolom Penilaian Baru</label>
          <div class="input-group input-group-sm">
            <input type="text" id="newColumnName" class="form-control" placeholder="Contoh: Hafalan Juz 30">
            <button class="btn btn-theme" onclick="tambahKolomBaru()"><i class="fa-solid fa-plus me-1"></i>Tambah</button>
          </div>
        </div>

        <hr class="my-2">

        <!-- Hapus Kolom -->
        <div>
          <label class="form-label extra-small text-muted fw-bold mb-1" style="font-size:11px;">Hapus Kolom yang Tidak Dipakai</label>
          <div class="input-group input-group-sm">
            <select class="form-select" id="deleteColumnSelect"></select>
            <button class="btn btn-danger fw-bold" onclick="hapusKolomPilihan()"><i class="fa-solid fa-trash me-1"></i>Hapus</button>
          </div>
        </div>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-upload me-1"></i> Upload Informasi Aktivitas</h6>
        <div class="mb-2">
          <input type="text" id="infoJudul" class="form-control form-control-sm" placeholder="Judul Informasi">
        </div>
        <div class="mb-2">
          <input type="file" id="infoFotoFile" accept="image/*" class="form-control form-control-sm">
        </div>
        <div class="mb-2">
          <textarea id="infoDeskripsi" class="form-control form-control-sm" rows="2" placeholder="Tulis keterangan..."></textarea>
        </div>
        <button class="btn btn-theme btn-sm w-100" onclick="simpanAktivitasInfo()">Publikasikan Info</button>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-user-plus me-1"></i> Tambah Santri Baru</h6>
        <div class="input-group input-group-sm mb-2">
          <input type="text" id="newSantriName" class="form-control" placeholder="Nama Santri Baru">
          <button class="btn btn-theme" onclick="tambahSantriBaru()">Tambah</button>
        </div>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-key me-1"></i> Reset Password Santri</h6>
        <div class="mb-2">
          <select class="form-select form-select-sm" id="resetSantriTarget"></select>
        </div>
        <div class="input-group input-group-sm mb-2">
          <input type="text" id="resetSantriNewPass" class="form-control" placeholder="Password Baru">
          <button class="btn btn-warning btn-sm fw-bold" onclick="resetPasswordSantri()">Reset</button>
        </div>
      </div>

      <!-- MENU GANTI LOGO LOGIN -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-image me-1"></i> Ganti Logo Halaman Login</h6>
        <p class="text-muted extra-small mb-2" style="font-size: 11px;">Unggah gambar dari HP Anda untuk dijadikan logo halaman depan/login.</p>
        <input type="file" id="inputLogoFile" accept="image/*" class="form-control form-control-sm mb-2">
        <button class="btn btn-theme btn-sm w-100 mb-1" onclick="simpanLogoApp()">Simpan Logo Baru</button>
        <button class="btn btn-outline-secondary btn-sm w-100" onclick="resetLogoApp()">Reset Logo Default</button>
      </div>

      <!-- MENU PENGATURAN WARNA BACKGROUND / TEMA -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-palette me-1"></i> Pengaturan Warna Tema</h6>
        <div class="mb-2">
          <select class="form-select form-select-sm" id="themeColorSelect">
            <option value="hijau">Hijau (Default)</option>
            <option value="kuning">Kuning Emas</option>
            <option value="biru">Biru</option>
            <option value="merah">Merah</option>
            <option value="ungu">Ungu</option>
            <option value="jingga">Jingga</option>
          </select>
        </div>
        <button class="btn btn-theme btn-sm w-100" onclick="simpanWarnaTema()">Terapkan Warna Tema</button>
      </div>
    </div>

    <!-- VIEW 5: MENU AKUN -->
    <div id="viewAkun" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3 text-center">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun</h6>
        
        <div class="mb-4 text-start border p-3 rounded-3 bg-light" id="profileUploadSection">
          <label class="form-label small fw-bold mb-1"><i class="fa-solid fa-image me-1"></i> Unggah Foto Profil Santri</label>
          <input type="file" id="inputProfilePhoto" accept="image/*" class="form-control form-control-sm mb-2">
          <button class="btn btn-theme btn-sm w-100" onclick="simpanFotoProfilSantri()">Simpan Foto Profil</button>
        </div>

        <div class="mb-4 text-start">
          <label class="form-label small fw-bold"><i class="fa-solid fa-lock me-1"></i> Ganti Password Akses</label>
          <input type="password" id="userNewPassInput" class="form-control form-control-sm mb-2" placeholder="Masukkan Password Baru">
          <button class="btn btn-theme btn-sm w-100" onclick="simpanGantiPasswordUser()">Simpan Password</button>
        </div>

        <hr>

        <button class="btn btn-outline-danger btn-sm w-100 fw-bold py-2" onclick="logout()">
          <i class="fa-solid fa-right-from-bracket me-1"></i> Keluar Aplikasi
        </button>
      </div>
    </div>

  </div>

  <!-- ================= BOTTOM NAVIGATION BAR ================= -->
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

  <!-- JAVASCRIPT SYSTEM -->
  <script>
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

    const themePresets = {
      "hijau": { bg: "#eef7ed", main: "#157347", dark: "#0d512f" },
      "kuning": { bg: "#fffdf0", main: "#d4a017", dark: "#997300" },
      "biru": { bg: "#edf4fc", main: "#0d6efd", dark: "#0a58ca" },
      "merah": { bg: "#fceded", main: "#dc3545", dark: "#b02a37" },
      "ungu": { bg: "#f5edf7", main: "#6f42c1", dark: "#593196" },
      "jingga": { bg: "#fef3eb", main: "#fd7e14", dark: "#ca6510" }
    };

    let currentRoleCategory = "";
    let currentClassKey = "";
    let currentUserName = "";

    function initDatabase() {
      if (!localStorage.getItem('rq_santri_db')) {
        let dbSantri = {};
        Object.keys(defaultDataSantri).forEach(key => {
          dbSantri[key] = defaultDataSantri[key].map(nama => ({ nama: nama, pass: nama, foto: '' }));
        });
        localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
      }

      if (!localStorage.getItem('rq_pass_pengajar')) {
        let passPengajar = {
          "Pengajar Awwal": "darul123",
          "Pengajar Tsani": "darul123",
          "Pengajar Tsalits": "darul123",
          "Pengajar Robi": "darul123"
        };
        localStorage.setItem('rq_pass_pengajar', JSON.stringify(passPengajar));
      }

      if (!localStorage.getItem('rq_columns')) {
        localStorage.setItem('rq_columns', JSON.stringify(defaultColumns));
      }

      loadAppLogo();
      loadWarnaTema();
      toggleLoginInputs();
    }

    window.onload = initDatabase;

    /* SKEMA DENGAN MANAJEMEN KOLOM */
    function renderDropdownHapusKolom() {
      const selectEl = document.getElementById('deleteColumnSelect');
      const columns = JSON.parse(localStorage.getItem('rq_columns') || '[]');
      selectEl.innerHTML = '<option value="">-- Pilih Kolom --</option>';

      columns.forEach(col => {
        selectEl.innerHTML += `<option value="${col}">${col}</option>`;
      });
    }

    function tambahKolomBaru() {
      const inputEl = document.getElementById('newColumnName');
      const newCol = inputEl.value.trim();

      if (!newCol) {
        alert('Silakan isi nama kolom terlebih dahulu!');
        return;
      }

      let columns = JSON.parse(localStorage.getItem('rq_columns') || '[]');

      if (columns.includes(newCol)) {
        alert('Nama kolom sudah ada!');
        return;
      }

      columns.push(newCol);
      localStorage.setItem('rq_columns', JSON.stringify(columns));

      inputEl.value = '';
      alert(`Kolom "${newCol}" berhasil ditambahkan!`);

      renderFormInputsPenilaian();
      renderDropdownHapusKolom();
      renderTabelPenilaian();
    }

    function hapusKolomPilihan() {
      const selectEl = document.getElementById('deleteColumnSelect');
      const colToDelete = selectEl.value;

      if (!colToDelete) {
        alert('Silakan pilih kolom yang mau dihapus!');
        return;
      }

      if (confirm(`Apakah Anda yakin ingin menghapus kolom "${colToDelete}"?`)) {
        let columns = JSON.parse(localStorage.getItem('rq_columns') || '[]');
        columns = columns.filter(c => c !== colToDelete);
        localStorage.setItem('rq_columns', JSON.stringify(columns));

        alert(`Kolom "${colToDelete}" berhasil dihapus.`);

        renderFormInputsPenilaian();
        renderDropdownHapusKolom();
        renderTabelPenilaian();
      }
    }

    /* SKEMA WARNA TEMA */
    function simpanWarnaTema() {
      const selectedTheme = document.getElementById('themeColorSelect').value;
      localStorage.setItem('rq_app_theme', selectedTheme);
      terapkanWarnaTema(selectedTheme);
      alert('Warna tema berhasil diperbarui!');
    }

    function loadWarnaTema() {
      const savedTheme = localStorage.getItem('rq_app_theme') || 'hijau';
      const selectEl = document.getElementById('themeColorSelect');
      if (selectEl) selectEl.value = savedTheme;
      terapkanWarnaTema(savedTheme);
    }

    function terapkanWarnaTema(themeKey) {
      const theme = themePresets[themeKey] || themePresets["hijau"];
      document.documentElement.style.setProperty('--bg-color', theme.bg);
      document.documentElement.style.setProperty('--main-color', theme.main);
      document.documentElement.style.setProperty('--dark-color', theme.dark);
    }

    function toggleLoginInputs() {
      const loginType = document.getElementById('loginType').value;
      const santriSelectGroup = document.getElementById('santriSelectGroup');
      const labelPassword = document.getElementById('labelPassword');
      const loginSantriName = document.getElementById('loginSantriName');

      loginSantriName.innerHTML = "";

      if (loginType.startsWith('Santri')) {
        santriSelectGroup.classList.remove('d-none');
        labelPassword.innerText = "Password Santri";

        let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
        let list = dbSantri[loginType] || [];
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
      document.getElementById('loginPassword').value = '';
    }

    function handleLogin() {
      const loginType = document.getElementById('loginType').value;
      const passwordInput = document.getElementById('loginPassword').value;

      if (loginType.startsWith('Pengajar')) {
        let passPengajarDb = JSON.parse(localStorage.getItem('rq_pass_pengajar'));
        if (passwordInput === passPengajarDb[loginType]) {
          currentRoleCategory = "Pengajar";
          currentClassKey = loginType.replace("Pengajar ", "");
          bukaDashboard();
        } else {
          alert("Password Anda salah, silakan ulangi lagi.");
        }
      } else {
        const selectedSantri = document.getElementById('loginSantriName').value;
        if (!selectedSantri) {
          alert("Password Anda salah, silakan ulangi lagi.");
          return;
        }

        let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
        let santriObj = dbSantri[loginType].find(s => s.nama === selectedSantri);

        if (santriObj && santriObj.pass === passwordInput) {
          currentRoleCategory = "Santri";
          currentClassKey = loginType.replace("Santri ", "");
          currentUserName = selectedSantri;
          bukaDashboard();
        } else {
          alert("Password Anda salah, silakan ulangi lagi.");
        }
      }
    }

    function bukaDashboard() {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');
      document.getElementById('bottomNav').classList.remove('d-none');

      document.getElementById('userRoleTitle').innerText = currentRoleCategory === 'Pengajar' ? `Pengajar Kelas ${currentClassKey}` : currentUserName;
      document.getElementById('userRoleSubtitle').innerText = `Mustawa ${currentClassKey}`;

      updateHeaderAvatar();

      const navInputNilai = document.getElementById('navInputNilai');
      const navPengaturan = document.getElementById('navPengaturan');

      if (currentRoleCategory === 'Pengajar') {
        navInputNilai.classList.remove('d-none');
        navPengaturan.classList.remove('d-none');
        loadDropdownSantriPengajar();
        renderFormInputsPenilaian();
        renderDropdownHapusKolom();
      } else {
        navInputNilai.classList.add('d-none');
        navPengaturan.classList.add('d-none');
      }

      renderAktivitasInfo();
      renderTabelPenilaian();
      switchView('viewAktivitas', document.querySelector('.bottom-nav-item'));
    }

    function updateHeaderAvatar() {
      const container = document.getElementById('headerAvatarContainer');
      if (currentRoleCategory === 'Santri') {
        const classSantriKey = "Santri " + currentClassKey;
        let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
        let santri = dbSantri[classSantriKey]?.find(s => s.nama === currentUserName);

        if (santri && santri.foto) {
          container.innerHTML = `<img src="${santri.foto}" class="user-profile-img">`;
          return;
        }
      }
      container.innerHTML = `<i class="fa-solid fa-circle-user fs-1 text-theme"></i>`;
    }

    function switchView(viewId, btnEl) {
      document.querySelectorAll('.dashboard-view').forEach(el => el.classList.add('d-none'));
      document.getElementById(viewId).classList.remove('d-none');

      document.querySelectorAll('.bottom-nav-item').forEach(el => el.classList.remove('active'));
      if(btnEl) btnEl.classList.add('active');

      if (viewId === 'viewLaporan') {
        renderTabelPenilaian();
      }
    }

    function renderFormInputsPenilaian() {
      const container = document.getElementById('dynamicFormInputs');
      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      container.innerHTML = '';

      columns.forEach((col, idx) => {
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
      const namaSantriSelected = document.getElementById('inputSantriTarget').value;
      if (!namaSantriSelected) {
        alert('Silakan pilih nama santri terlebih dahulu!');
        return;
      }

      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      let recordValues = {};

      columns.forEach((col, idx) => {
        let inputVal = document.getElementById(`col_input_${idx}`).value;
        recordValues[col] = inputVal ? inputVal : '-';
      });

      let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
      let existingIndex = reports.findIndex(r => r.kelasKey === currentClassKey && r.nama === namaSantriSelected);
      
      if (existingIndex !== -1) {
        reports[existingIndex].values = recordValues;
      } else {
        reports.push({
          kelasKey: currentClassKey,
          nama: namaSantriSelected,
          values: recordValues
        });
      }

      localStorage.setItem('rq_reports', JSON.stringify(reports));

      alert(`Penilaian untuk ${namaSantriSelected} berhasil disimpan!`);

      document.getElementById('inputSantriTarget').value = '';
      columns.forEach((col, idx) => {
        document.getElementById(`col_input_${idx}`).value = '';
      });

      renderTabelPenilaian();
      switchView('viewLaporan', document.querySelectorAll('.bottom-nav-item')[1]);
    }

    function renderTabelPenilaian() {
      const thead = document.getElementById('tabelHeader');
      const tbody = document.getElementById('tabelDataSantri');
      const columns = JSON.parse(localStorage.getItem('rq_columns'));

      let headerHtml = `<tr><th>No</th><th>Nama</th>`;
      columns.forEach(col => headerHtml += `<th>${col}</th>`);
      if (currentRoleCategory === 'Pengajar') headerHtml += `<th>Aksi</th>`;
      headerHtml += `</tr>`;
      thead.innerHTML = headerHtml;

      tbody.innerHTML = '';
      let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
      
      let filtered = reports.filter(r => r.kelasKey === currentClassKey);
      
      if (currentRoleCategory === 'Santri') {
        filtered = filtered.filter(r => r.nama === currentUserName);
      }

      if (filtered.length === 0) {
        tbody.innerHTML = `<tr><td colspan="${columns.length + (currentRoleCategory === 'Pengajar' ? 3 : 2)}" class="text-muted py-3">Belum ada data nilai.</td></tr>`;
        return;
      }

      filtered.forEach((item, index) => {
        let row = `<tr><td>${index + 1}</td><td class="fw-bold text-start">${item.nama}</td>`;
        columns.forEach(col => row += `<td>${item.values[col] || '-'}</td>`);
        
        if (currentRoleCategory === 'Pengajar') {
          let origIndex = reports.indexOf(item);
          row += `<td><button class="btn btn-danger btn-sm py-0 px-2" onclick="hapusPenilaian(${origIndex})"><i class="fa-solid fa-trash"></i></button></td>`;
        }
        row += `</tr>`;
        tbody.innerHTML += row;
      });
    }

    function hapusPenilaian(index) {
      if (confirm('Apakah Anda yakin ingin menghapus data penilaian ini?')) {
        let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
        reports.splice(index, 1);
        localStorage.setItem('rq_reports', JSON.stringify(reports));
        renderTabelPenilaian();
      }
    }

    function loadDropdownSantriPengajar() {
      const keyClass = "Santri " + currentClassKey;
      let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
      let list = dbSantri[keyClass] || [];

      const targetInput = document.getElementById('inputSantriTarget');
      const targetReset = document.getElementById('resetSantriTarget');

      targetInput.innerHTML = '<option value="">-- Pilih Santri --</option>';
      targetReset.innerHTML = '<option value="">-- Pilih Santri --</option>';

      list.forEach(s => {
        targetInput.innerHTML += `<option value="${s.nama}">${s.nama}</option>`;
        targetReset.innerHTML += `<option value="${s.nama}">${s.nama}</option>`;
      });
    }

    function simpanAktivitasInfo() {
      const judul = document.getElementById('infoJudul').value;
      const fileInput = document.getElementById('infoFotoFile');
      const deskripsi = document.getElementById('infoDeskripsi').value;

      if (!judul || !deskripsi) return alert('Judul dan deskripsi wajib diisi!');

      const processSave = (fotoData) => {
        let listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
        listInfo.unshift({
          judul: judul, foto: fotoData || '', deskripsi: deskripsi,
          waktu: new Date().toLocaleDateString('id-ID')
        });

        localStorage.setItem('rq_aktivitas', JSON.stringify(listInfo));
        alert('Informasi berhasil dipublikasikan!');
        document.getElementById('infoJudul').value = '';
        document.getElementById('infoFotoFile').value = '';
        document.getElementById('infoDeskripsi').value = '';
        renderAktivitasInfo();
        switchView('viewAktivitas', document.querySelector('.bottom-nav-item'));
      };

      if (fileInput.files && fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = e => processSave(e.target.result);
        reader.readAsDataURL(fileInput.files[0]);
      } else {
        processSave('');
      }
    }

    function renderAktivitasInfo() {
      const container = document.getElementById('containerAktivitas');
      const listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
      container.innerHTML = '';

      if (listInfo.length === 0) {
        container.innerHTML = `<div class="text-center text-muted small py-3">Belum ada informasi/aktivitas.</div>`;
        return;
      }

      listInfo.forEach((item, idx) => {
        let fotoHtml = item.foto ? `<img src="${item.foto}" class="preview-img mb-2">` : '';
        let btnHapus = (currentRoleCategory === 'Pengajar') ? 
          `<button class="btn btn-danger btn-sm py-0 px-2 mt-2" onclick="hapusAktivitasInfo(${idx})"><i class="fa-solid fa-trash"></i> Hapus</button>` : '';

        container.innerHTML += `
          <div class="border-bottom pb-3 mb-3">
            <h6 class="fw-bold text-theme mb-1">${item.judul}</h6>
            <small class="text-muted d-block mb-2" style="font-size:10px;">${item.waktu}</small>
            ${fotoHtml}
            <p class="small text-secondary mb-0">${item.deskripsi}</p>
            ${btnHapus}
          </div>
        `;
      });
    }

    function hapusAktivitasInfo(idx) {
      if (confirm('Hapus informasi ini?')) {
        let listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
        listInfo.splice(idx, 1);
        localStorage.setItem('rq_aktivitas', JSON.stringify(listInfo));
        renderAktivitasInfo();
      }
    }

    function tambahSantriBaru() {
      const newName = document.getElementById('newSantriName').value.trim();
      if (!newName) return alert('Isi nama santri!');

      const classSantriKey = "Santri " + currentClassKey;
      let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));

      dbSantri[classSantriKey].push({ nama: newName, pass: newName, foto: '' });
      localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
      alert(`Santri ${newName} berhasil ditambahkan!`);
      document.getElementById('newSantriName').value = '';
      loadDropdownSantriPengajar();
    }

    function resetPasswordSantri() {
      const targetName = document.getElementById('resetSantriTarget').value;
      const newPass = document.getElementById('resetSantriNewPass').value.trim();
      if (!targetName || !newPass) return alert('Pilih santri dan password baru!');

      const classSantriKey = "Santri " + currentClassKey;
      let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
      let santri = dbSantri[classSantriKey].find(s => s.nama === targetName);

      if (santri) {
        santri.pass = newPass;
        localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
        alert(`Password ${targetName} berhasil diubah!`);
        document.getElementById('resetSantriNewPass').value = '';
      }
    }

    function simpanLogoApp() {
      const fileInput = document.getElementById('inputLogoFile');
      if (fileInput.files && fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = e => {
          localStorage.setItem('rq_custom_logo', e.target.result);
          loadAppLogo();
          alert('Logo halaman login berhasil diubah!');
          fileInput.value = '';
        };
        reader.readAsDataURL(fileInput.files[0]);
      } else {
        alert('Silakan pilih berkas foto dari HP terlebih dahulu!');
      }
    }

    function resetLogoApp() {
      localStorage.removeItem('rq_custom_logo');
      loadAppLogo();
      alert('Logo dikembalikan ke tampilan default!');
    }

    function loadAppLogo() {
      const customLogo = localStorage.getItem('rq_custom_logo');
      const logoImg = document.getElementById('customLogo');
      const defaultIcon = document.getElementById('defaultLogo');

      if (customLogo) {
        logoImg.src = customLogo;
        logoImg.classList.remove('d-none');
        defaultIcon.classList.add('d-none');
      } else {
        logoImg.classList.add('d-none');
        defaultIcon.classList.remove('d-none');
      }
    }

    function simpanFotoProfilSantri() {
      if (currentRoleCategory !== 'Santri') return alert('Fitur khusus akun Santri.');

      const fileInput = document.getElementById('inputProfilePhoto');
      if (fileInput.files && fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = e => {
          const classSantriKey = "Santri " + currentClassKey;
          let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
          let santri = dbSantri[classSantriKey].find(s => s.nama === currentUserName);

          if (santri) {
            santri.foto = e.target.result;
            localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
            updateHeaderAvatar();
            alert('Foto profil berhasil diubah!');
            fileInput.value = '';
          }
        };
        reader.readAsDataURL(fileInput.files[0]);
      } else {
        alert('Pilih foto terlebih dahulu!');
      }
    }

    function simpanGantiPasswordUser() {
      const newPass = document.getElementById('userNewPassInput').value.trim();
      if (!newPass) return alert('Isi password baru!');

      if (currentRoleCategory === 'Pengajar') {
        const keyPengajar = "Pengajar " + currentClassKey;
        let passPengajarDb = JSON.parse(localStorage.getItem('rq_pass_pengajar'));
        passPengajarDb[keyPengajar] = newPass;
        localStorage.setItem('rq_pass_pengajar', JSON.stringify(passPengajarDb));
      } else {
        const classSantriKey = "Santri " + currentClassKey;
        let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
        let santri = dbSantri[classSantriKey].find(s => s.nama === currentUserName);
        if (santri) {
          santri.pass = newPass;
          localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
        }
      }

      alert('Password berhasil diubah!');
      document.getElementById('userNewPassInput').value = '';
    }

    function logout() {
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('bottomNav').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
      document.getElementById('loginPassword').value = '';
    }
  </script>
</body>
</html>
