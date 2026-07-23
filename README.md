
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rumah Qur'an - Darul Arqam</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    body { background-color: #f8f9fa; }
    .login-card { max-width: 400px; margin: 50px auto; border-radius: 12px; }
  </style>
</head>
<body>

  <!-- ================= HALAMAN LOGIN ================= -->
  <div id="loginPage" class="container">
    <div class="card shadow-sm p-4 login-card text-center">
      <!-- Logo & Judul -->
      <div class="mb-3">
        <i class="fa-solid fa-book-quran text-success display-3"></i>
      </div>
      <h3 class="fw-bold text-success mb-1">Rumah Qur'an</h3>
      <p class="text-muted small mb-4">Darul Arqam</p>

      <!-- Dropdown Tipe Login -->
      <div class="mb-3 text-start">
        <label class="form-label fw-bold">Tipe Login</label>
        <select class="form-select" id="loginType" onchange="toggleLoginInputs()">
          <option value="santri">Santri / Wali Santri</option>
          <option value="admin">Ustaz / Ustadzah</option>
        </select>
      </div>

      <!-- Input Password Santri -->
      <div class="mb-3 text-start" id="santriInputGroup">
        <label class="form-label">Password Wali Santri</label>
        <input type="password" class="form-control" id="santriPassword" placeholder="Masukkan Password Wali Santri">
      </div>

      <!-- Input Password Admin -->
      <div class="mb-3 text-start d-none" id="adminInputGroup">
        <label class="form-label">Password Ustaz / Ustadzah</label>
        <input type="password" class="form-control" id="adminPassword" placeholder="Masukkan Password Ustadzah">
      </div>

      <!-- Tombol Masuk -->
      <button class="btn btn-success w-100 fw-bold py-2 shadow-sm" onclick="handleLogin()">
        Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>

      <!-- Catatan Password -->
      <p class="text-muted fs-7 mt-3 mb-0 small">
        *Pass Ustadzah: <code class="text-danger">Darul123</code> | Pass Wali Santri: <code class="text-danger">Arqam123</code>
      </p>
    </div>
  </div>

  <!-- ================= HALAMAN DASHBOARD DATA ================= -->
  <div id="dashboardPage" class="container mt-4 d-none">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h2>Dashboard Info & Nilai Santri</h2>
      <button class="btn btn-outline-danger btn-sm" onclick="logout()">
        <i class="fa-solid fa-right-from-bracket"></i> Keluar
      </button>
    </div>

    <!-- Status Hak Akses -->
    <div class="alert alert-info" id="userRoleBadge"></div>

    <!-- FITUR KHUSUS ADMIN / USTADZAH (Input Nilai & Tambah Info) -->
    <div id="adminPanel" class="card p-3 mb-4 border-success d-none">
      <h5 class="text-success"><i class="fa-solid fa-pen-to-square"></i> Form Input Nilai & Informasi (Khusus Ustadzah)</h5>
      <hr>
      <div class="row g-3">
        <div class="col-md-4">
          <input type="text" id="inputNama" class="form-control" placeholder="Nama Santri">
        </div>
        <div class="col-md-3">
          <input type="number" id="inputNilai" class="form-control" placeholder="Nilai Hafalan">
        </div>
        <div class="col-md-5">
          <input type="text" id="inputCatatan" class="form-control" placeholder="Catatan / Info Santri">
        </div>
        <div class="col-12 text-end">
          <button class="btn btn-success" onclick="tambahDataSantri()">
            <i class="fa-solid fa-plus"></i> Simpan Data
          </button>
        </div>
      </div>
    </div>

    <!-- TABEL DATA SANTRI (Bisa Dilihat oleh Semua, tapi hanya Admin yang bisa edit) -->
    <div class="card p-3">
      <h5><i class="fa-solid fa-list-check"></i> Data & Nilai Santri</h5>
      <table class="table table-striped mt-3">
        <thead class="table-dark">
          <tr>
            <th>No</th>
            <th>Nama Santri</th>
            <th>Nilai Hafalan</th>
            <th>Informasi / Catatan</th>
          </tr>
        </thead>
        <tbody id="tabelDataSantri">
          <tr>
            <td>1</td>
            <td>Ahmad Abdullah</td>
            <td>85</td>
            <td>Juz 30 Lancar, Muraja'ah Juz 29</td>
          </tr>
          <tr>
            <td>2</td>
            <td>Siti Aisyah</td>
            <td>90</td>
            <td>Selesai Hafalan Surah Al-Baqarah</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- ================= JAVASCRIPT LOGIC ================= -->
  <script>
    // Toggle Tampilan Input Password sesuai Pilihan Dropdown
    function toggleLoginInputs() {
      const loginType = document.getElementById('loginType').value;
      const santriGroup = document.getElementById('santriInputGroup');
      const adminGroup = document.getElementById('adminInputGroup');

      if (loginType === 'admin') {
        santriGroup.classList.add('d-none');
        adminGroup.classList.remove('d-none');
      } else {
        santriGroup.classList.remove('d-none');
        adminGroup.classList.add('d-none');
      }
    }

    // Fungsi Validasi & Login
    function handleLogin() {
      const loginType = document.getElementById('loginType').value;

      if (loginType === 'admin') {
        const passAdmin = document.getElementById('adminPassword').value;
        if (passAdmin === 'Darul123') {
          bukaDashboard('Ustadzah (Admin)');
        } else {
          alert('Password Ustadzah Salah! Gunakan: Darul123');
        }
      } else {
        const passSantri = document.getElementById('santriPassword').value;
        if (passSantri === 'Arqam123') {
          bukaDashboard('Wali Santri');
        } else {
          alert('Password Wali Santri Salah! Gunakan: Arqam123');
        }
      }
    }

    // Fungsi Menampilkan Dashboard Sesuai Role
    function bukaDashboard(role) {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');
      
      const roleBadge = document.getElementById('userRoleBadge');
      const adminPanel = document.getElementById('adminPanel');

      if (role === 'Ustadzah (Admin)') {
        roleBadge.className = "alert alert-success";
        roleBadge.innerHTML = "<strong>Mode Access: Ustaz / Ustadzah.</strong> Anda memiliki hak akses penuh untuk melihat, menambah, dan mengedit data santri.";
        adminPanel.classList.remove('d-none'); // Tampilkan form input
      } else {
        roleBadge.className = "alert alert-warning";
        roleBadge.innerHTML = "<strong>Mode Access: Wali Santri.</strong> Anda hanya dapat melihat (read-only) nilai dan informasi santri.";
        adminPanel.classList.add('d-none'); // Sembunyikan form input
      }
    }

    // Fungsi Tambah Data Santri (Khusus Admin)
    function tambahDataSantri() {
      const nama = document.getElementById('inputNama').value;
      const nilai = document.getElementById('inputNilai').value;
      const catatan = document.getElementById('inputCatatan').value;

      if (!nama || !nilai) {
        alert('Mohon isi Nama dan Nilai Santri!');
        return;
      }

      const tabel = document.getElementById('tabelDataSantri');
      const rowCount = tabel.rows.length + 1;

      const newRow = `
        <tr>
          <td>${rowCount}</td>
          <td>${nama}</td>
          <td>${nilai}</td>
          <td>${catatan || '-'}</td>
        </tr>
      `;

      tabel.innerHTML += newRow;

      // Reset Form
      document.getElementById('inputNama').value = '';
      document.getElementById('inputNilai').value = '';
      document.getElementById('inputCatatan').value = '';
    }

    // Fungsi Logout
    function logout() {
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
      document.getElementById('adminPassword').value = '';
      document.getElementById('santriPassword').value = '';
    }
  </script>
</body>
</html>
