<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>TPQ Darul Arqam - Penilaian Santri</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <style>
    :root {
      --gold-main: #d4af37;
      --gold-dark: #aa820a;
      --gold-light: #fff8e7;
      --bg-color: #fdfbf7;
      --text-color: #2b2b2b;
    }
    
    body { 
      background-color: var(--bg-color); 
      color: var(--text-color);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      padding-bottom: 90px;
    }

    .mobile-container {
      max-width: 480px;
      margin: 0 auto;
      padding: 16px;
    }

    .card-mobile {
      border: none;
      border-radius: 20px;
      box-shadow: 0 4px 15px rgba(212, 175, 55, 0.15);
      background: white;
    }

    /* Top Banner Card khas WheelCare Layout */
    .profile-card-header {
      background: linear-gradient(135deg, var(--gold-main), var(--gold-dark));
      color: white;
      border-radius: 24px;
      padding: 20px;
      box-shadow: 0 6px 18px rgba(170, 130, 10, 0.25);
    }

    .btn-gold {
      background-color: var(--gold-main);
      color: white;
      font-weight: 600;
      border-radius: 14px;
      border: none;
    }
    .btn-gold:hover, .btn-gold:active {
      background-color: var(--gold-dark);
      color: white;
    }

    .text-gold { color: var(--gold-dark) !important; }

    /* Custom App Logo */
    .app-logo-wrapper {
      width: 90px;
      height: 90px;
      margin: 0 auto;
      border-radius: 22px;
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

    .stat-card-1 {
      background: #198754;
      color: white;
      border-radius: 18px;
    }

    /* Bottom Navigation Bar */
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

    .bottom-nav-item i { font-size: 19px; }

    .bottom-nav-item.active {
      color: var(--gold-dark);
      font-weight: bold;
    }

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
          <i class="fa-solid fa-book-quran text-gold display-5" id="defaultLogo"></i>
          <img id="customLogo" src="" class="app-logo d-none" alt="Logo">
        </div>
      </div>

      <h3 class="fw-bold text-gold mb-0">TPQ Darul Arqam</h3>
      <p class="text-muted small mb-4">Sistem Penilaian Santri</p>

      <!-- 1. Dropdown Pengguna (Langsung Terisi Pengajar & Nama-Nama Santri) -->
      <div class="mb-3 text-start">
        <label class="form-label fw-bold small">Pilih Akun / Nama Santri</label>
        <select class="form-select form-select-sm" id="loginType" onchange="updateLoginLabel()">
          <optgroup label="Akses Pengajar">
            <option value="Pengajar Awwal" data-type="Pengajar" data-kelas="Awwal">Pengajar - Mustawa Awwal</option>
            <option value="Pengajar Tsani" data-type="Pengajar" data-kelas="Tsani">Pengajar - Mustawa Tsani</option>
            <option value="Pengajar Tsalits" data-type="Pengajar" data-kelas="Tsalits">Pengajar - Mustawa Tsalits</option>
            <option value="Pengajar Robi" data-type="Pengajar" data-kelas="Robi'">Pengajar - Mustawa Robi'</option>
          </optgroup>

          <!-- MUSTAWA AWWAL -->
          <optgroup label="Santri Mustawa Awwal">
            <option value="Ahmad Arkhan Wiratama" data-type="Santri" data-kelas="Awwal">Ahmad Arkhan Wiratama</option>
            <option value="Aishwa Nasha Razeeta" data-type="Santri" data-kelas="Awwal">Aishwa Nasha Razeeta</option>
            <option value="Al Afkar Syabani" data-type="Santri" data-kelas="Awwal">Al Afkar Syabani</option>
            <option value="Ananda Aisyah Syahidal Syail" data-type="Santri" data-kelas="Awwal">Ananda Aisyah Syahidal Syail</option>
            <option value="Aqila Rafania Adifa" data-type="Santri" data-kelas="Awwal">Aqila Rafania Adifa</option>
            <option value="Arisha Fatimah" data-type="Santri" data-kelas="Awwal">Arisha Fatimah</option>
            <option value="Asyila Rahma Khadijah" data-type="Santri" data-kelas="Awwal">Asyila Rahma Khadijah</option>
            <option value="Athaya Humaira Althafia" data-type="Santri" data-kelas="Awwal">Athaya Humaira Althafia</option>
            <option value="Bilal Zayyan Prinoza" data-type="Santri" data-kelas="Awwal">Bilal Zayyan Prinoza</option>
            <option value="Desya Salsaila" data-type="Santri" data-kelas="Awwal">Desya Salsaila</option>
            <option value="Fatimah" data-type="Santri" data-kelas="Awwal">Fatimah</option>
            <option value="Kenzie Attaya Depa" data-type="Santri" data-kelas="Awwal">Kenzie Attaya Depa</option>
            <option value="Khuzaimah Summayyah" data-type="Santri" data-kelas="Awwal">Khuzaimah Summayyah</option>
            <option value="Muhammad Adzriel Rafif Fakhri" data-type="Santri" data-kelas="Awwal">Muhammad Adzriel Rafif Fakhri</option>
            <option value="Muhammad Alfatih Rinaldi" data-type="Santri" data-kelas="Awwal">Muhammad Alfatih Rinaldi</option>
            <option value="Muhammad Alzaahiy Rinaldi" data-type="Santri" data-kelas="Awwal">Muhammad Alzaahiy Rinaldi</option>
            <option value="Muhammad Dzaky" data-type="Santri" data-kelas="Awwal">Muhammad Dzaky</option>
            <option value="Muhammad Fathian Shariq" data-type="Santri" data-kelas="Awwal">Muhammad Fathian Shariq</option>
            <option value="Muhammad Gibran Saguftha" data-type="Santri" data-kelas="Awwal">Muhammad Gibran Saguftha</option>
            <option value="Muhammad Ibrahim Al Fatih Isnanto" data-type="Santri" data-kelas="Awwal">Muhammad Ibrahim Al Fatih Isnanto</option>
            <option value="Muhammad Razka Destha Athallah" data-type="Santri" data-kelas="Awwal">Muhammad Razka Destha Athallah</option>
            <option value="Muhammad Ustman" data-type="Santri" data-kelas="Awwal">Muhammad Ustman</option>
            <option value="Qahirah Arsylia Aftarinda" data-type="Santri" data-kelas="Awwal">Qahirah Arsylia Aftarinda</option>
            <option value="Raid Asadel" data-type="Santri" data-kelas="Awwal">Raid Asadel</option>
            <option value="Shaqueena Salma Aryanta" data-type="Santri" data-kelas="Awwal">Shaqueena Salma Aryanta</option>
            <option value="Shofiya Azzahra Hidayatulloh" data-type="Santri" data-kelas="Awwal">Shofiya Azzahra Hidayatulloh</option>
            <option value="Sultan Ibrahim Akbar" data-type="Santri" data-kelas="Awwal">Sultan Ibrahim Akbar</option>
            <option value="Syafia Az Zahra" data-type="Santri" data-kelas="Awwal">Syafia Az Zahra</option>
            <option value="Syahfira Destriani" data-type="Santri" data-kelas="Awwal">Syahfira Destriani</option>
            <option value="Syahrika Destriani" data-type="Santri" data-kelas="Awwal">Syahrika Destriani</option>
            <option value="Syakira Beatric Setiawan" data-type="Santri" data-kelas="Awwal">Syakira Beatric Setiawan</option>
            <option value="Tsanwa Chayra Variin" data-type="Santri" data-kelas="Awwal">Tsanwa Chayra Variin</option>
            <option value="Vesha Sakilla" data-type="Santri" data-kelas="Awwal">Vesha Sakilla</option>
            <option value="Yusuf Al Fawwaz" data-type="Santri" data-kelas="Awwal">Yusuf Al Fawwaz</option>
            <option value="Zea Mikhayla Almeera Yendra" data-type="Santri" data-kelas="Awwal">Zea Mikhayla Almeera Yendra</option>
          </optgroup>

          <!-- MUSTAWA TSANI -->
          <optgroup label="Santri Mustawa Tsani">
            <option value="Abdurrahman" data-type="Santri" data-kelas="Tsani">Abdurrahman</option>
            <option value="Akhtar Muhammad Rafasya" data-type="Santri" data-kelas="Tsani">Akhtar Muhammad Rafasya</option>
            <option value="Al Ghany Pratama" data-type="Santri" data-kelas="Tsani">Al Ghany Pratama</option>
            <option value="Al Hando Pranstio" data-type="Santri" data-kelas="Tsani">Al Hando Pranstio</option>
            <option value="Alfarizqi Khairan Yazid" data-type="Santri" data-kelas="Tsani">Alfarizqi Khairan Yazid</option>
            <option value="Anina Yumna Sakhi" data-type="Santri" data-kelas="Tsani">Anina Yumna Sakhi</option>
            <option value="Binar Al Biru Chandra" data-type="Santri" data-kelas="Tsani">Binar Al Biru Chandra</option>
            <option value="Chaerunnisa Fathiyaturahma" data-type="Santri" data-kelas="Tsani">Chaerunnisa Fathiyaturahma</option>
            <option value="Habiburahman El Shirazy" data-type="Santri" data-kelas="Tsani">Habiburahman El Shirazy</option>
            <option value="Hana Shabiya Vina Pakpahan" data-type="Santri" data-kelas="Tsani">Hana Shabiya Vina Pakpahan</option>
            <option value="Keenan Ghayda Sakhi" data-type="Santri" data-kelas="Tsani">Keenan Ghayda Sakhi</option>
            <option value="Keisha Chessy Tri Adiva" data-type="Santri" data-kelas="Tsani">Keisha Chessy Tri Adiva</option>
            <option value="Khadijah Athiyyah Samreno" data-type="Santri" data-kelas="Tsani">Khadijah Athiyyah Samreno</option>
            <option value="Khaif Shakiel Badillah" data-type="Santri" data-kelas="Tsani">Khaif Shakiel Badillah</option>
            <option value="Maryam Intan Dzakiyah" data-type="Santri" data-kelas="Tsani">Maryam Intan Dzakiyah</option>
            <option value="Molin Sanjaya" data-type="Santri" data-kelas="Tsani">Molin Sanjaya</option>
            <option value="Muhamad Ibrahim Hidayatulloh" data-type="Santri" data-kelas="Tsani">Muhamad Ibrahim Hidayatulloh</option>
            <option value="Muhammad Al-Ghazello Arief" data-type="Santri" data-kelas="Tsani">Muhammad Al-Ghazello Arief</option>
            <option value="Muhammad Hamiz Tabrani" data-type="Santri" data-kelas="Tsani">Muhammad Hamiz Tabrani</option>
            <option value="Muhammad Raihan Wildra" data-type="Santri" data-kelas="Tsani">Muhammad Raihan Wildra</option>
            <option value="Prisha Humairah" data-type="Santri" data-kelas="Tsani">Prisha Humairah</option>
            <option value="Qallesha Louis Nawalla" data-type="Santri" data-kelas="Tsani">Qallesha Louis Nawalla</option>
            <option value="Risya Naifah Andami" data-type="Santri" data-kelas="Tsani">Risya Naifah Andami</option>
            <option value="Rosa Adeliya" data-type="Santri" data-kelas="Tsani">Rosa Adeliya</option>
            <option value="Salsabila Putri Ayoenie Alfarizi" data-type="Santri" data-kelas="Tsani">Salsabila Putri Ayoenie Alfarizi</option>
            <option value="Shaffiyah Mecca Al Fatih Isnanto" data-type="Santri" data-kelas="Tsani">Shaffiyah Mecca Al Fatih Isnanto</option>
            <option value="Syifa Nursabrina Robka" data-type="Santri" data-kelas="Tsani">Syifa Nursabrina Robka</option>
            <option value="Syifa Oktaviani" data-type="Santri" data-kelas="Tsani">Syifa Oktaviani</option>
            <option value="Zaim Faqih Alrasyid" data-type="Santri" data-kelas="Tsani">Zaim Faqih Alrasyid</option>
            <option value="Ziyadah Khaira Pakpahan" data-type="Santri" data-kelas="Tsani">Ziyadah Khaira Pakpahan</option>
          </optgroup>
        </select>
      </div>

      <!-- Password Input -->
      <div class="mb-4 text-start">
        <label class="form-label fw-bold small" id="labelPassword">Password Pengajar</label>
        <input type="password" class="form-control form-control-sm" id="loginPassword" placeholder="Masukkan Password">
      </div>

      <button class="btn btn-gold w-100 py-2 shadow-sm" onclick="handleLogin()">
        Masuk Aplikasi <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>
    </div>
  </div>

  <!-- ================= DASHBOARD UTAMA ================= -->
  <div id="dashboardPage" class="mobile-container d-none">
    
    <div class="d-flex align-items-center justify-content-between mb-3">
      <h4 class="fw-bold text-gold mb-0">TPQ Darul Arqam</h4>
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
            <h6 class="mb-0 fw-bold fs-6" id="userRoleTitle">Pengajar</h6>
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

    <!-- VIEW 1: BERANDA -->
    <div id="viewAktivitas" class="dashboard-view">
      <div class="card stat-card-1 p-3 mb-3">
        <div class="d-flex justify-content-between align-items-center">
          <div>
            <small class="d-block opacity-75">Perkembangan Belajar</small>
            <h5 class="fw-bold mb-0">Lancar & Rajin</h5>
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
        <h6 class="fw-bold text-gold mb-3"><i class="fa-solid fa-file-lines me-2"></i>Laporan Penilaian Santri</h6>
        <div class="table-responsive">
          <table class="table table-bordered align-middle text-center small mb-0">
            <thead class="table-warning text-dark" id="tabelHeader"></thead>
            <tbody id="tabelDataSantri"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- VIEW 3: INPUT PENILAIAN (KHUSUS PENGAJAR) -->
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

    <!-- VIEW 4: KELOLA (KHUSUS PENGAJAR) -->
    <div id="viewPengaturan" class="dashboard-view d-none">
      <!-- Upload Info Aktivitas -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-2"><i class="fa-solid fa-upload me-1"></i> Upload Informasi / Foto Aktivitas</h6>
        <div class="mb-2">
          <input type="text" id="infoJudul" class="form-control form-control-sm" placeholder="Judul Aktivitas">
        </div>
        <div class="mb-2">
          <input type="file" id="infoFotoFile" accept="image/*" class="form-control form-control-sm">
        </div>
        <div class="mb-2">
          <textarea id="infoDeskripsi" class="form-control form-control-sm" rows="2" placeholder="Detail aktivitas..."></textarea>
        </div>
        <button class="btn btn-gold btn-sm w-100" onclick="simpanAktivitasInfo()">Publikasikan</button>
      </div>

      <!-- Tambah Santri Baru -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-2"><i class="fa-solid fa-user-plus me-1"></i> Tambah Santri Baru</h6>
        <div class="input-group input-group-sm mb-2">
          <input type="text" id="newSantriName" class="form-control" placeholder="Nama Lengkap Santri">
          <button class="btn btn-gold" onclick="tambahSantriBaru()">Tambah</button>
        </div>
      </div>

      <!-- Edit Logo App -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-gold mb-2"><i class="fa-solid fa-image me-1"></i> Ubah Logo Login</h6>
        <input type="file" id="inputLogoFile" accept="image/*" class="form-control form-control-sm mb-2">
        <button class="btn btn-gold btn-sm w-100 mb-1" onclick="simpanLogoApp()">Simpan Logo</button>
        <button class="btn btn-outline-secondary btn-sm w-100" onclick="resetLogoApp()">Reset Default</button>
      </div>
    </div>

    <!-- VIEW 5: AKUN & RESET PASSWORD -->
    <div id="viewAkun" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3 text-center">
        <h6 class="fw-bold text-gold mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun & Keamanan</h6>
        
        <div class="mb-3 text-start border p-3 rounded-3 bg-light d-none" id="profileUploadSection">
          <label class="form-label small fw-bold mb-1"><i class="fa-solid fa-camera me-1"></i> Foto Profil Santri</label>
          <input type="file" id="inputProfilePhoto" accept="image/*" class="form-control form-control-sm mb-2">
          <button class="btn btn-gold btn-sm w-100" onclick="simpanFotoProfilSantri()">Simpan Foto</button>
        </div>

        <div class="mb-3 text-start border p-3 rounded-3 bg-light">
          <label class="form-label small fw-bold mb-1"><i class="fa-solid fa-key me-1"></i> Ganti Password Saya</label>
          <input type="password" id="userNewPassInput" class="form-control form-control-sm mb-2" placeholder="Password Baru">
          <button class="btn btn-gold btn-sm w-100" onclick="simpanGantiPasswordUs
