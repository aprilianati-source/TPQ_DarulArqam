<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>TPQ Darul Arqam - Sistem Penilaian</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <style>
    :root {
      /* Tema Warna Kuning Emas / Gold */
      --gold-main: #d4af37;
      --gold-dark: #aa820a;
      --gold-light: #fff8e7;
      --gold-card: #f9e29c;
      --bg-color: #fdfbf7;
      --text-color: #2b2b2b;
    }
    
    body { 
      background-color: var(--bg-color); 
      color: var(--text-color);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      padding-bottom: 90px;
    }

    .mobile-container {
      max-width: 480px;
      margin: 0 auto;
      padding: 16px;
    }

    /* Kustomisasi Kartu UI Elegan */
    .card-mobile {
      border: none;
      border-radius: 20px;
      box-shadow: 0 4px 15px rgba(212, 175, 55, 0.15);
      background: white;
    }

    /* Header Profile Card khas WheelCare Layout */
    .profile-card-header {
      background: linear-gradient(135deg, var(--gold-main), var(--gold-dark));
      color: white;
      border-radius: 24px;
      padding: 20px;
      box-shadow: 0 6px 18px rgba(170, 130, 10, 0.25);
      position: relative;
    }

    .btn-gold {
      background-color: var(--gold-main);
      color: white;
      font-weight: 600;
      border-radius: 14px;
      border: none;
      transition: all 0.2s;
    }
    .btn-gold:hover, .btn-gold:active {
      background-color: var(--gold-dark);
      color: white;
    }

    .text-gold { color: var(--gold-dark) !important; }

    /* Logo Wrapper */
    .app-logo-wrapper {
      width: 100px;
      height: 100px;
      margin: 0 auto;
      border-radius: 24px;
      background: #ffffff;
      border: 3px solid var(--gold-main);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    .app-logo { 
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    /* Floating Action / Stats Cards */
    .stat-card-1 {
      background: #198754;
      color: white;
      border-radius: 18px;
    }
    .stat-card-2 {
      background: #0dcaf0;
      color: white;
      border-radius: 18px;
    }
    .stat-card-3 {
      background: #fd7e14;
      color: white;
      border-radius: 18px;
    }

    /* Bottom Navigation Bar khas Aplikasi Mobile */
    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: 70px;
      background: white;
      border-top: 1px solid #f0e6d2;
      display: flex;
      justify-content: space-around;
      align-items: center;
      z-index: 1000;
      box-shadow: 0 -4px 15px rgba(0,0,0,0.04);
      max-width: 500px;
      margin: 0 auto;
    }

    .bottom-nav-item {
      text-align: center;
      color: #a0a0a0;
      font-size: 11px;
      text-decoration: none;
      background: none;
      border: none;
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 3px;
    }

    .bottom-nav-item i {
      font-size: 19px;
    }

    .bottom-nav-item.active {
      color: var(--gold-dark);
      font-weight: bold;
    }

    /* Central Floating Action Button (Tombol Input Penilaian) */
    .nav-btn-center {
      background: linear-gradient(135deg, var(--gold-main), var(--gold-dark));
      color: white !important;
      width: 52px;
      height: 52px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-top: -25px;
      box-shadow: 0 4px 12px rgba(170, 130, 10, 0.4);
    }
    .nav-btn-center i { font-size: 20px !important; margin: 0; }

    .preview-img {
      max-width: 100%;
      border-radius: 14px;
      margin-top: 10px;
    }

    .user-profile-img {
      width: 55px;
      height: 55px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid white;
    }
  </style>
</head>
<body>

  <!-- ================= HALAMAN LOGIN ================= -->
  <div id="loginPage" class="mobile-container pt-4">
    <div class="card card-mobile text-center p-4">
      
      <div class="mb-3 pt-2">
        <div class="app-logo-wrapper">
          <i class="fa-solid fa-book-quran text-gold display-4" id="defaultLogo"></i>
          <img id="customLogo" src="" class="app-logo d-none" alt="Logo Aplikasi">
        </div>
      </div>

      <h3 class="fw-bold text-gold mb-0">TPQ Darul Arqam</h3>
      <p class="text-muted small mb-4">Sistem Informasi Penilaian Santri</p>

      <div class="mb-3 text-start">
        <label class="form-label fw-bold small">Tipe Akses User</label>
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

      <div class="mb-4 text-start">
        <label class="form-label fw-bold small" id="labelPassword">Password Santri</label>
        <input type="password" class="form-control form-control-sm" id="loginPassword" placeholder="Masukkan Password">
      </div>

      <button class="btn btn-gold w-100 py-2 shadow-sm" onclick="handleLogin()">
        Masuk Aplikasi <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>
    </div>
  </div>

  <!-- ================= HALAMAN DASHBOARD MAIN ================= -->
  <div id="dashboardPage" class="mobile-container d-none">
    
    <div class="d-flex align-items-center justify-content-between mb-3">
      <h3 class="fw-bold text-gold mb-0">TPQ Darul Arqam</h3>
      <span class="badge bg-warning text-dark px-2 py-1" id="currentRoleBadge">Awwal</span>
    </div>

    <!-- Header UI Card Khas WheelCare -->
    <div class="profile-card-header mb-3">
      <div class="d-flex align-items-center justify-content-between mb-2">
        <div class="d-flex align-items-center">
          <div class="me-3" id="headerAvatarContainer">
            <i class="fa-solid fa-circle-user fs-1 text-white opacity-75"></i>
          </div>
          <div>
            <h5 class="mb-0 fw-bold" id="userRoleTitle">Pengajar Kelas Awwal</h5>
            <small class="text-white-50" id="userRoleSubtitle">Mustawa Awwal</small>
          </div>
        </div>
      </div>
      <hr class="my-2 opacity-25">
      <div class="d-flex justify-content-between align-items-center small">
        <span>Status Akses: <strong class="text-white">Aktif</strong></span>
        <span>Tahun Ajaran: <strong>2026</strong></span>
      </div>
    </div>

    <!-- VIEW 1: BERANDA / AKTIVITAS INFORMASI -->
    <div id="viewAktivitas" class="dashboard-view">
      
      <!-- Stats Ringkasan Penilaian Khas WheelCare Cards -->
      <div class="card stat-card-1 p-3 mb-3">
        <div class="d-flex justify-content-between align-items-center">
          <div>
            <small class="d-block opacity-75">Progres Hafalan / Iqro</small>
            <h4 class="fw-bold mb-0" id="statCapaian">Lancar / Terus Tingkatkan</h4>
          </div>
          <i class="fa-solid fa-book-open fs-1 opacity-50"></i>
        </div>
      </div>

      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-3"><i class="fa-solid fa-bullhorn me-1"></i> Informasi & Aktivitas TPQ</h6>
        <div id="containerAktivitas"></div>
      </div>
    </div>

    <!-- VIEW 2: LAPORAN PENILAIAN SANTRI -->
    <div id="viewLaporan" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-3"><i class="fa-solid fa-file-lines me-2"></i>Laporan Perkembangan Santri</h6>
        <div class="table-responsive">
          <table class="table table-bordered align-middle text-center small mb-0">
            <thead class="table-warning text-dark" id="tabelHeader"></thead>
            <tbody id="tabelDataSantri"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- VIEW 3: INPUT NILAI (KHUSUS PENGAJAR) -->
    <div id="viewInputNilai" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-3"><i class="fa-solid fa-pen-to-square me-2"></i>Form Penilaian Santri</h6>
        <form id="formPenilaian" onsubmit="simpanDataPenilaian(event)">
          <div class="mb-3">
            <label class="form-label fw-bold small">Pilih Santri Target</label>
            <select class="form-select form-select-sm" id="inputSantriTarget" required></select>
          </div>
          
          <div id="dynamicFormInputs" class="row g-2"></div>
          
          <button type="submit" class="btn btn-gold w-100 py-2 mt-3 shadow-sm">
            <i class="fa-solid fa-floppy-disk me-1"></i> Simpan Penilaian
          </button>
        </form>
      </div>
    </div>

    <!-- VIEW 4: PENGATURAN / KELOLA (KHUSUS PENGAJAR) -->
    <div id="viewPengaturan" class="dashboard-view d-none">
      <!-- Upload Info Aktivitas -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-2"><i class="fa-solid fa-upload me-1"></i> Upload Aktivitas TPQ</h6>
        <div class="mb-2">
          <input type="text" id="infoJudul" class="form-control form-control-sm" placeholder="Judul Pengumuman/Aktivitas">
        </div>
        <div class="mb-2">
          <label class="form-label style-small text-muted mb-0" style="font-size: 11px;">Pilih Foto dari Galeri HP</label>
          <input type="file" id="infoFotoFile" accept="image/*" class="form-control form-control-sm">
        </div>
        <div class="mb-2">
          <textarea id="infoDeskripsi" class="form-control form-control-sm" rows="2" placeholder="Tuliskan detail aktivitas..."></textarea>
        </div>
        <button class="btn btn-gold btn-sm w-100" onclick="simpanAktivitasInfo()">Publikasikan Aktivitas</button>
      </div>

      <!-- Tambah Santri Baru -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-2"><i class="fa-solid fa-user-plus me-1"></i> Tambah Santri Baru</h6>
        <p class="text-muted small mb-2" style="font-size: 11px;">Nama santri baru otomatis menjadi password awal login santri.</p>
        <div class="input-group input-group-sm mb-2">
          <input type="text" id="newSantriName" class="form-control" placeholder="Nama Lengkap Santri">
          <button class="btn btn-gold" onclick="tambahSantriBaru()">Tambah</button>
        </div>
      </div>

      <!-- Ubah Logo Login -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-2"><i class="fa-solid fa-image me-1"></i> Ubah Logo Login Aplikasi</h6>
        <input type="file" id="inputLogoFile" accept="image/*" class="form-control form-control-sm mb-2">
        <button class="btn btn-gold btn-sm w-100 mb-1" onclick="simpanLogoApp()">Simpan Logo Baru</button>
        <button class="btn btn-outline-secondary btn-sm w-100" onclick="resetLogoApp()">Reset Logo Default</button>
      </div>
    </div>

    <!-- VIEW 5: AKUN & PENGATURAN LOGOUT SANITIZED -->
    <div id="viewAkun" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3 text-center">
        <h6 class="fw-bold text-gold mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun & Keamanan</h6>
        
        <!-- khusus Santri -->
        <div class="mb-4 text-start border p-3 rounded-3 bg-light d-none" id="profileUploadSection">
          <label class="form-label small fw-bold mb-1"><i class="fa-solid fa-camera me-1"></i> Unggah Foto Profil Santri</label>
          <input type="file" id="inputProfilePhoto" accept="image/*" class="form-control form-control-sm mb-2">
          <button class="btn btn-gold btn-sm w-100" onclick="simpanFotoProfilSantri()">Simpan Foto Profil</button>
        </div>

        <!-- Ganti Password Saya -->
        <div class="mb-4 text-start border p-3 rounded-3 bg-light">
          <label class="form-label small fw-bold mb-1"><i class="fa-solid fa-key me-1"></i> Ganti Password Saya</label>
          <input type="password" id="userNewPassInput" class="form-control form-control-sm mb-2" placeholder="Password Baru">
          <button class="btn btn-gold btn-sm w-100" onclick="simpanGantiPasswordUser()">Simpan Password Baru</button>
        </div>

        <!-- Khusus Pengajar: Reset Password Santri -->
        <div class="mb-4 text-start border p-3 rounded-3 bg-light d-none" id="resetSantriSection">
          <label class="form-label small fw-bold mb-1"><i class="fa-solid fa-user-lock me-1"></i> Reset Password Santri Lupa</label>
          <select class="form-select form-select-sm mb-2" id="resetSantriTarget"></select>
          <input type="text" id="resetSantriNewPass" class="form-control form-control-sm mb-2" placeholder="Password Baru Santri">
          <button class="btn btn-warning btn-sm w-100 fw-bold" onclick="resetPasswordSantri()">Reset Password Santri</button>
        </div>

        <hr>

        <!-- Tombol Keluar / Logout -->
        <button class="btn btn-outline-danger w-100 fw-bold py-2" onclick="logout()">
          <i class="fa-solid fa-right-from-bracket me-1"></i> Keluar Aplikasi
        </button>
      </div>
    </div>

  </div>

  <!-- ================= BOTTOM NAVIGATION BAR (WHEELCARE DESIGN) ================= -->
  <nav class="bottom-nav d-none" id="bottomNav">
    <button class="bottom-nav-item active" onclick="switchView('viewAktivitas', this)">
      <i class="fa-solid fa-house"></i>
      <span>Beranda</span>
    </button>
    <button class="bottom-nav-item" onclick="switchView('viewLaporan', this)">
      <i class="fa-solid fa-list-check"></i>
      <span>Laporan</span>
    </button>

    <!-- Center Action Button (Input Nilai Pengajar) -->
    <button class="bottom-nav-item d-none" id="navInputNilai" onclick="switchView('viewInputNilai', this)">
      <div class="nav-btn-center">
        <i class="fa-solid fa-plus"></i>
      </div>
      <span style="margin-top: 2px;">Penilaian</span>
    </button>

    <button class="bottom-nav-item d-none" id="navPengaturan" onclick="switchView('viewPengaturan', this)">
      <i class="fa-solid fa-sliders"></i>
      <span>Kelola</span>
    </button>
    <button class="bottom-nav-item" onclick="switchView('viewAkun', this)">
      <i class="fa-solid fa-user"></i>
      <span>Akun</span>
    </button>
  </nav>

  <!-- JAVASCRIPT SYSTEM -->
  <script>
    // Data Default Santri Sesuai Daftar
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
      "Iqro / Capaian", "Catatan Iqro", 
      "Hafalan Surat", "Catatan Hafalan", 
      "Hafalan Doa / Hadits", "Catatan Akhlak", "Kehadiran (%)"
    ];

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
      toggleLoginInputs();
    }

    window.onload = initDatabase;

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
      
