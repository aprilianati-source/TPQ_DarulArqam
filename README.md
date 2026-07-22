<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rumah Qur'an Darul Arqam - Laporan Santri</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <!-- NAVBAR -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-success sticky-top shadow-sm" id="mainNav" style="display:none;">
    <div class="container">
      <a class="navbar-brand fw-bold" href="#">
        <i class="fa-solid fa-quran me-2"></i>RQ Darul Arqam
      </a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto align-items-center">
          <li class="nav-item">
            <a class="nav-link active" href="#" id="navHome" onclick="switchTab('home')"><i class="fa-solid fa-house me-1"></i> Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#" id="navLaporan" onclick="switchTab('laporan')"><i class="fa-solid fa-book-open me-1"></i> Laporan Santri</a>
          </li>
          <li class="nav-item ms-lg-3">
            <span class="badge bg-light text-success me-2" id="userRoleBadge">User</span>
            <button class="btn btn-outline-light btn-sm" onclick="logout()"><i class="fa-solid fa-right-from-bracket me-1"></i> Keluar</button>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- 1. HALAMAN LOGIN -->
  <div id="loginPage" class="container d-flex align-items-center justify-content-center min-vh-100">
    <div class="card shadow-lg border-0 login-card p-4 text-center">
      <div class="mb-3 text-success">
        <i class="fa-solid fa-quran fa-4x"></i>
      </div>
      <h3 class="fw-bold text-success mb-1">Rumah Qur'an</h3>
      <h5 class="text-secondary mb-4">Darul Arqam</h5>
      
      <div class="mb-3 text-start">
        <label class="form-label font-weight-bold">Tipe Login</label>
        <select class="form-select" id="loginType" onchange="toggleLoginInputs()">
          <option value="santri">Santri / Wali Santri</option>
          <option value="admin">Ustaz / Admin</option>
        </select>
      </div>

      <div class="mb-3 text-start" id="nikInputGroup">
        <label class="form-label">NIK Santri / Password</label>
        <input type="password" class="form-control" id="loginPassword" placeholder="Masukkan NIK Santri">
      </div>

      <div class="mb-3 text-start d-none" id="adminInputGroup">
        <label class="form-label">Password Admin</label>
        <input type="password" class="form-control" id="adminPassword" placeholder="Masukkan Password Admin">
      </div>

      <button class="btn btn-success w-100 fw-bold py-2 shadow-sm" onclick="handleLogin()">Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i></button>
      <p class="text-muted fs-7 mt-3 mb-0">*Default Pass Admin: <code>123456</code> | Pass Santri: NIK Santri</p>
    </div>
  </div>

  <!-- MAIN CONTENT CONTAINER -->
  <div class="container my-4">
    
    <!-- 2. HALAMAN HOME -->
    <div id="homePage" style="display:none;">
      <div class="p-4 p-md-5 mb-4 rounded-3 bg-success text-white shadow-sm hero-banner">
        <div class="container-fluid py-2">
          <h1 class="display-6 fw-bold">Selamat Datang di Portal TPQ</h1>
          <p class="fs-5">Rumah Qur'an Darul Arqam — Membentuk Generasi Rabbani yang Cinta Al-Qur'an.</p>
        </div>
      </div>

      <div class="row g-4">
        <div class="col-md-4">
          <div class="card border-0 shadow-sm h-100 text-center p-3">
            <div class="card-body">
              <i class="fa-solid fa-book-quran fa-3x text-success mb-3"></i>
              <h5 class="card-title fw-bold">Tilawah & Iqra</h5>
              <p class="card-text text-muted">Pemantauan bacaan jilid, surah, dan halaman secara berkala.</p>
            </div>
          </div>
        </div>
        <div class="col-md-4">
          <div class="card border-0 shadow-sm h-100 text-center p-3">
            <div class="card-body">
              <i class="fa-solid fa-brain fa-3x text-success mb-3"></i>
              <h5 class="card-title fw-bold">Hafalan Surah</h5>
              <p class="card-text text-muted">Mencatat kualitas hafalan surah-surah pendek dan Juz Amma.</p>
            </div>
          </div>
        </div>
        <div class="col-md-4">
          <div class="card border-0 shadow-sm h-100 text-center p-3">
            <div class="card-body">
              <i class="fa-solid fa-chart-line fa-3x text-success mb-3"></i>
              <h5 class="card-title fw-bold">Progres Realtime</h5>
              <p class="card-text text-muted">Wali santri dapat melihat perkembangan setoran harian santri.</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 3. HALAMAN LAPORAN SANTRI -->
    <div id="laporanPage" style="display:none;">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h4 class="fw-bold text-success mb-0"><i class="fa-solid fa-clipboard-list me-2"></i>Laporan Perkembangan Santri</h4>
        <button class="btn btn-success btn-sm" id="btnTambahLaporan" onclick="showModalTambah()" style="display:none;">
          <i class="fa-solid fa-plus me-1"></i> Tambah Setoran (Admin)
        </button>
      </div>

      <!-- Info Profil Santri -->
      <div class="card border-0 shadow-sm mb-4">
        <div class="card-body">
          <div class="row" id="profilSantriBox">
            <!-- Profil di-render via JS -->
          </div>
        </div>
      </div>

      <!-- Tabel Laporan -->
      <div class="card border-0 shadow-sm">
        <div class="card-body p-0">
          <div class="table-responsive">
            <table class="table table-hover align-middle mb-0">
              <thead class="table-success">
                <tr>
                  <th>Tanggal</th>
                  <th>Jilid/Al-Qur'an</th>
                  <th>Halaman / Ayat</th>
                  <th>Hafalan Surah</th>
                  <th>Predikat Tajwid</th>
                  <th>Kehadiran</th>
                  <th>Catatan Ustaz</th>
                </tr>
              </thead>
              <tbody id="tabelLaporanBody">
                <!-- Row data akan di-insert via JavaScript -->
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>

  </div>

  <!-- MODAL TAMBAH DATA (ADMIN ONLY) -->
  <div class="modal fade" id="modalLaporan" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header bg-success text-white">
          <h5 class="modal-title"><i class="fa-solid fa-pen-to-square me-2"></i>Tambah Setoran Santri</h5>
          <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
        </div>
        <div class="modal-body">
          <form id="formLaporan">
            <div class="mb-3">
              <label class="form-label">Tanggal</label>
              <input type="date" class="form-control" id="inputTanggal" required>
            </div>
            <div class="mb-3">
              <label class="form-label">Jilid / Kitab</label>
              <input type="text" class="form-control" id="inputJilid" placeholder="Contoh: Jilid 3 / Al-Qur'an" required>
            </div>
            <div class="mb-3">
              <label class="form-label">Halaman / Ayat</label>
              <input type="text" class="form-control" id="inputHalaman" placeholder="Contoh: Hal 12 / Ayat 1-15" required>
            </div>
            <div class="mb-3">
              <label class="form-label">Hafalan Surah</label>
              <input type="text" class="form-control" id="inputHafalan" placeholder="Contoh: An-Naba 1-10">
            </div>
            <div class="mb-3">
              <label class="form-label">Kualitas / Tajwid</label>
              <select class="form-select" id="inputTajwid">
                <option value="Sangat Baik (A)">Sangat Baik (A)</option>
                <option value="Baik (B)">Baik (B)</option>
                <option value="Cukup (C)">Cukup (C)</option>
                <option value="Perlu Ulang (D)">Perlu Ulang (D)</option>
              </select>
            </div>
            <div class="mb-3">
              <label class="form-label">Kehadiran</label>
              <select class="form-select" id="inputKehadiran">
                <option value="Hadir">Hadir</option>
                <option value="Izin">Izin</option>
                <option value="Sakit">Sakit</option>
                <option value="Alpha">Alpha</option>
              </select>
            </div>
            <div class="mb-3">
              <label class="form-label">Catatan Ustaz / Ustazah</label>
              <textarea class="form-control" id="inputCatatan" rows="2" placeholder="Catatan perbaikan tajwid/kelancaran..."></textarea>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
          <button type="button" class="btn btn-success" onclick="simpanLaporan()">Simpan Setoran</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Bootstrap JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
  <script src="app.js"></script>
</body>
</html>
