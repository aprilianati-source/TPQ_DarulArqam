
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
    :root {
      --gold-main: #D4AF37;
      --gold-dark: #B8860B;
      --gold-light: #FFF8DC;
      --gold-hover: #AA7C11;
    }
    body { 
      background-color: var(--gold-light); 
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
    }
    .bg-gold { background-color: var(--gold-main) !important; color: white; }
    .bg-gold-dark { background-color: var(--gold-dark) !important; color: white; }
    .text-gold { color: var(--gold-dark) !important; }
    .btn-gold { 
      background-color: var(--gold-main); 
      color: white; 
      font-weight: bold; 
      border: none;
    }
    .btn-gold:hover { 
      background-color: var(--gold-hover); 
      color: white; 
    }
    .card-gold {
      border: 2px solid var(--gold-main);
      border-radius: 12px;
    }
    .login-card { max-width: 450px; margin: 40px auto; border-radius: 15px; }
    .table-responsive { background: white; border-radius: 8px; padding: 15px; }
    th { vertical-align: middle; text-align: center; }
    .app-logo { max-width: 100px; max-height: 100px; object-fit: contain; }
  </style>
</head>
<body>

  <!-- ================= HALAMAN LOGIN ================= -->
  <div id="loginPage" class="container">
    <div class="card shadow login-card text-center p-4 card-gold bg-white">
      <div class="mb-3 text-center" id="logoContainer">
        <i class="fa-solid fa-book-quran text-gold display-3" id="defaultLogo"></i>
        <img id="customLogo" src="" class="app-logo d-none" alt="Logo Aplikasi">
      </div>
      <h3 class="fw-bold text-gold mb-1">Rumah Qur'an</h3>
      <p class="text-muted small mb-4">Darul Arqam</p>

      <!-- Dropdown Tipe Login -->
      <div class="mb-3 text-start">
        <label class="form-label fw-bold">Tipe Akses Login</label>
        <select class="form-select" id="loginType" onchange="toggleLoginInputs()">
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

      <!-- Dropdown Pilihan Nama Santri (Jika Login Santri) -->
      <div class="mb-3 text-start" id="santriSelectGroup">
        <label class="form-label fw-bold">Pilih Nama Santri</label>
        <select class="form-select" id="loginSantriName">
          <!-- Diisi via JS -->
        </select>
      </div>

      <!-- Input Password -->
      <div class="mb-3 text-start">
        <label class="form-label fw-bold" id="labelPassword">Password Santri</label>
        <input type="password" class="form-control" id="loginPassword" placeholder="Masukkan Password">
        <small class="text-muted d-block mt-1 d-none" id="infoLupaPassSantri">
          *Lupa password? Silakan hubungi Pengajar Kelas Anda.
        </small>
      </div>

      <button class="btn btn-gold w-100 py-2 shadow-sm mt-2" onclick="handleLogin()">
        Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>
    </div>
  </div>

  <!-- ================= HALAMAN DASHBOARD ================= -->
  <div id="dashboardPage" class="container-fluid px-4 mt-4 d-none">
    <div class="d-flex justify-content-between align-items-center mb-3">
      <div>
        <h3 class="fw-bold text-gold mb-0">Laporan & Info Santri</h3>
        <span class="text-muted">Rumah Qur'an Darul Arqam</span>
      </div>
      <div>
        <button class="btn btn-warning btn-sm me-2" onclick="openModalGantiPassword()">
          <i class="fa-solid fa-key"></i> Ganti Password
        </button>
        <button class="btn btn-outline-danger btn-sm" onclick="logout()">
          <i class="fa-solid fa-right-from-bracket"></i> Keluar
        </button>
      </div>
    </div>

    <!-- Status Badge -->
    <div class="alert bg-gold text-white py-2" id="userRoleBadge"></div>

    <!-- PENGUMUMAN & AKTIVITAS (Tampil untuk Santri & Pengajar) -->
    <div class="card p-3 mb-4 card-gold bg-white shadow-sm">
      <h5 class="fw-bold text-gold"><i class="fa-solid fa-bullhorn"></i> Informasi & Aktivitas Rumah Qur'an</h5>
      <hr class="my-2">
      <div id="containerAktivitas" class="row g-3">
        <!-- Konten Pengumuman/Foto Muncul di Sini -->
      </div>
    </div>

    <!-- PANEL KHUSUS PENGAJAR -->
    <div id="adminPanel" class="d-none">
      
      <!-- NAV TABS PENGELOLAAN PENGAJAR -->
      <ul class="nav nav-tabs mb-3" id="pengajarTab" role="tablist">
        <li class="nav-item">
          <button class="nav-link active fw-bold text-gold" id="tab-nilai" data-bs-toggle="tab" data-bs-target="#panel-nilai">
            <i class="fa-solid fa-pen-to-square"></i> Input Nilai Santri
          </button>
        </li>
        <li class="nav-item">
          <button class="nav-link fw-bold text-gold" id="tab-santri" data-bs-toggle="tab" data-bs-target="#panel-santri">
            <i class="fa-solid fa-user-plus"></i> Tambah / Reset Pass Santri
          </button>
        </li>
        <li class="nav-item">
          <button class="nav-link fw-bold text-gold" id="tab-info" data-bs-toggle="tab" data-bs-target="#panel-info">
            <i class="fa-solid fa-upload"></i> Upload Informasi / Foto Aktivitas
          </button>
        </li>
        <li class="nav-item">
          <button class="nav-link fw-bold text-gold" id="tab-pengaturan" data-bs-toggle="tab" data-bs-target="#panel-pengaturan">
            <i class="fa-solid fa-sliders"></i> Pengaturan Tabel & Logo
          </button>
        </li>
      </ul>

      <div class="tab-content mb-4" id="pengajarTabContent">
        
        <!-- 1. TAB INPUT NILAI -->
        <div class="tab-pane fade show active card p-3 card-gold bg-white" id="panel-nilai">
          <h5 class="fw-bold text-gold mb-3">Form Input Perkembangan Santri</h5>
          <form id="formPenilaian" onsubmit="simpanDataPenilaian(event)">
            <div class="row g-3">
              <div class="col-md-6">
                <label class="form-label fw-bold">Pilih Santri</label>
                <select class="form-select" id="inputSantriTarget" required>
                  <!-- Diisi Dinamis -->
                </select>
              </div>

              <!-- Container Input Dinamis Kolom Penilaian -->
              <div class="col-12"><hr></div>
              <div class="row g-3" id="dynamicFormInputs">
                <!-- Diisi via JS sesuai struktur kolom -->
              </div>

              <div class="col-12 text-end">
                <button type="submit" class="btn btn-gold px-4">
                  <i class="fa-solid fa-floppy-disk"></i> Simpan Penilaian
                </button>
              </div>
            </div>
          </form>
        </div>

        <!-- 2. TAB MANAJEMEN SANTRI -->
        <div class="tab-pane fade card p-3 card-gold bg-white" id="panel-santri">
          <div class="row g-4">
            <!-- Tambah Santri Baru -->
            <div class="col-md-6 border-end">
              <h5 class="fw-bold text-gold"><i class="fa-solid fa-user-plus"></i> Tambah Santri Baru</h5>
              <p class="small text-muted">Nama yang ditambah akan otomatis menjadi password default santri untuk login.</p>
              <div class="mb-3">
                <label class="form-label">Nama Lengkap Santri</label>
                <input type="text" id="newSantriName" class="form-control" placeholder="Contoh: Muhammad Budi">
              </div>
              <button class="btn btn-gold btn-sm" onclick="tambahSantriBaru()">
                <i class="fa-solid fa-plus"></i> Tambahkan Santri
              </button>
            </div>
            
            <!-- Reset Password Santri Lupa Pass -->
            <div class="col-md-6">
              <h5 class="fw-bold text-gold"><i class="fa-solid fa-key"></i> Reset Password Santri</h5>
              <p class="small text-muted">Ubah password santri yang lupa password.</p>
              <div class="mb-2">
                <label class="form-label">Pilih Santri</label>
                <select class="form-select" id="resetSantriTarget">
                  <!-- Diisi Dinamis -->
                </select>
              </div>
              <div class="mb-3">
                <label class="form-label">Password Baru</label>
                <input type="text" id="resetSantriNewPass" class="form-control" placeholder="Masukkan Password Baru">
              </div>
              <button class="btn btn-warning btn-sm fw-bold" onclick="resetPasswordSantri()">
                <i class="fa-solid fa-rotate-right"></i> Reset Password
              </button>
            </div>
          </div>
        </div>

        <!-- 3. TAB UPLOAD INFORMASI / AKTIVITAS -->
        <div class="tab-pane fade card p-3 card-gold bg-white" id="panel-info">
          <h5 class="fw-bold text-gold"><i class="fa-solid fa-file-circle-plus"></i> Upload Informasi / Foto Aktivitas</h5>
          <div class="row g-3">
            <div class="col-md-6">
              <label class="form-label">Judul Aktivitas / Informasi</label>
              <input type="text" id="infoJudul" class="form-control" placeholder="Kunjungan Santri / Lomba">
            </div>
            <div class="col-md-6">
              <label class="form-label">URL Gambar / Foto (Opsional)</label>
              <input type="url" id="infoFotoUrl" class="form-control" placeholder="https://domain.com/foto.jpg">
            </div>
            <div class="col-12">
              <label class="form-label">Deskripsi / Keterangan</label>
              <textarea id="infoDeskripsi" class="form-control" rows="2" placeholder="Tulis rincian aktivitas..."></textarea>
            </div>
            <div class="col-12 text-end">
              <button class="btn btn-gold btn-sm" onclick="simpanAktivitasInfo()">
                <i class="fa-solid fa-paper-plane"></i> Publikasikan Info
              </button>
            </div>
          </div>
        </div>

        <!-- 4. TAB PENGATURAN TABEL & LOGO -->
        <div class="tab-pane fade card p-3 card-gold bg-white" id="panel-pengaturan">
          <div class="row g-4">
            <!-- Edit Kolom Tabel Penilaian -->
            <div class="col-md-7 border-end">
              <h5 class="fw-bold text-gold"><i class="fa-solid fa-table-columns"></i> Kelola Kolom Tabel Penilaian</h5>
              <div class="input-group mb-3">
                <input type="text" id="newColumnName" class="form-control" placeholder="Nama Kolom Baru (misal: Hafalan Doa)">
                <button class="btn btn-gold" onclick="tambahKolomTabel()">
                  <i class="fa-solid fa-plus"></i> Tambah Kolom
                </button>
              </div>

              <h6>Daftar Kolom Penilaian Saat Ini:</h6>
              <ul class="list-group" id="listKolomTabel">
                <!-- Diisi via JS -->
              </ul>
            </div>

            <!-- Edit Logo Aplikasi -->
            <div class="col-md-5">
              <h5 class="fw-bold text-gold"><i class="fa-solid fa-image"></i> Ubah Logo Aplikasi</h5>
              <p class="small text-muted">Masukkan Link URL Gambar Logo baru untuk Tampilan Login.</p>
              <div class="mb-3">
                <input type="url" id="inputLogoUrl" class="form-control" placeholder="https://example.com/logo.png">
              </div>
              <button class="btn btn-gold btn-sm mb-2" onclick="simpanLogoApp()">
                <i class="fa-solid fa-floppy-disk"></i> Simpan Logo Baru
              </button>
              <button class="btn btn-outline-secondary btn-sm mb-2" onclick="resetLogoApp()">
                Reset Logo Bawaan
              </button>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- TABEL LAPORAN PENILAIAN -->
    <div class="card p-3 shadow-sm mb-5 card-gold bg-white">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h5 class="fw-bold text-gold mb-0" id="tabelJudul"><i class="fa-solid fa-table"></i> Laporan Perkembangan Santri</h5>
      </div>

      <div class="table-responsive">
        <table class="table table-bordered align-middle">
          <thead class="bg-gold-dark text-white" id="tabelHeader">
            <!-- Diisi Dinamis via JS -->
          </thead>
          <tbody id="tabelDataSantri" class="small">
            <!-- Data dimasukkan secara dinamis melalui JS -->
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- MODAL GANTI PASSWORD -->
  <div class="modal fade" id="modalGantiPassword" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header bg-gold text-white">
          <h5 class="modal-title"><i class="fa-solid fa-key"></i> Ganti Password Akses Anda</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
        </div>
        <div class="modal-body">
          <div class="mb-3">
            <label class="form-label">Password Baru</label>
            <input type="password" id="userNewPassInput" class="form-control" placeholder="Masukkan Password Baru">
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
          <button type="button" class="btn btn-gold" onclick="simpanGantiPasswordUser()">Simpan Password</button>
        </div>
      </div>
    </div>
  </div>

  <!-- JAVASCRIPT LOGIC -->
  <script>
    // DATA DEFAULT SANTRI BERDASARKAN KELAS
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
      "Santri Tsalits": ["Santri Contoh Tsalits 1", "Santri Contoh Tsalits 2"],
      "Santri Robi": ["Santri Contoh Robi 1", "Santri Contoh Robi 2"]
    };

    // KOLOM DEFAULT PENILAIAN
    const defaultColumns = [
      "Iqro - Capaian", "Iqro - Catatan", 
      "Hafalan Surat - Murajaah", "Hafalan Surat - Ziyadah", "Hafalan Surat - Catatan",
      "Hafalan Lainnya - Hadits", "Hafalan Lainnya - Matan", "Hafalan Lainnya - Doa",
      "Catatan Akhlak", "Kehadiran (%)"
    ];

    let currentRoleCategory = ""; // Pengajar atau Santri
    let currentClassKey = "";     // Awwal, Tsani, Tsalits, Robi
    let currentUserName = "";     // Nama santri jika login santri

    // INISIALISASI DATABASE LOKAL
    function initDatabase() {
      if (!localStorage.getItem('rq_santri_db')) {
        let dbSantri = {};
        Object.keys(defaultDataSantri).forEach(key => {
          dbSantri[key] = defaultDataSantri[key].map(nama => ({
            nama: nama,
            pass: nama // Default password = nama
          }));
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

    // FUNGSI TOGGLE TAMPILAN LOGIN
    function toggleLoginInputs() {
      const loginType = document.getElementById('loginType').value;
      const santriSelectGroup = document.getElementById('santriSelectGroup');
      const labelPassword = document.getElementById('labelPassword');
      const infoLupaPassSantri = document.getElementById('infoLupaPassSantri');
      const loginSantriName = document.getElementById('loginSantriName');

      loginSantriName.innerHTML = "";

      if (loginType.startsWith('Santri')) {
        santriSelectGroup.classList.remove('d-none');
        infoLupaPassSantri.classList.remove('d-none');
        labelPassword.innerText = "Password Santri";

        // Load Nama Santri Sesuai Kelas
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
        infoLupaPassSantri.classList.add('d-none');
        labelPassword.innerText = "Password Pengajar";
      }
      document.getElementById('loginPassword').value = '';
    }

    // HANDLE LOGIN
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
          alert('Password Pengajar Salah!');
        }
      } else {
        const selectedSantri = document.getElementById('loginSantriName').value;
        if (!selectedSantri) {
          alert('Pilih nama santri!');
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
          alert('Password Santri Salah!');
        }
      }
    }

    // BUKA DASHBOARD
    function bukaDashboard() {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');

      const roleBadge = document.getElementById('userRoleBadge');
      const adminPanel = document.getElementById('adminPanel');

      if (currentRoleCategory === 'Pengajar') {
        roleBadge.innerHTML = `<strong>Mode Akses: Pengajar Kelas ${currentClassKey}.</strong> Anda memiliki akses penuh pengelolaan kelas ini.`;
        adminPanel.classList.remove('d-none');
        loadDropdownSantriPengajar();
        renderFormInputsPenilaian();
        renderListKolomSettings();
      } else {
        roleBadge.innerHTML = `<strong>Mode Akses: Santri / Wali Santri (${currentUserName} - Kelas ${currentClassKey}).</strong>`;
        adminPanel.classList.add('d-none');
      }

      renderAktivitasInfo();
      renderTabelPenilaian();
    }

    // RENDER AKTIVITAS & INFORMASI
    function renderAktivitasInfo() {
      const container = document.getElementById('containerAktivitas');
      const listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
      container.innerHTML = '';

      if (listInfo.length === 0) {
        container.innerHTML = `<div class="col-12 text-muted small">Belum ada informasi / foto aktivitas yang dipublikasikan.</div>`;
        return;
      }

      listInfo.forEach((item, idx) => {
        let fotoHtml = item.foto ? `<img src="${item.foto}" class="img-fluid rounded mb-2" style="max-height:200px; object-fit:cover;">` : '';
        let btnHapus = (currentRoleCategory === 'Pengajar') ? 
          `<button class="btn btn-danger btn-sm mt-2" onclick="hapusAktivitasInfo(${idx})"><i class="fa-solid fa-trash"></i> Hapus Info</button>` : '';

        container.innerHTML += `
          <div class="col-md-6">
            <div class="border rounded p-3 bg-light">
              <h6 class="fw-bold text-gold">${item.judul}</h6>
              ${fotoHtml}
              <p class="small mb-1">${item.deskripsi}</p>
              <small class="text-muted d-block" style="font-size:10px;">Dipublikasikan: ${item.waktu}</small>
              ${btnHapus}
            </div>
          </div>
        `;
      });
    }

    function simpanAktivitasInfo() {
      const judul = document.getElementById('infoJudul').value;
      const foto = document.getElementById('infoFotoUrl').value;
      const deskripsi = document.getElementById('infoDeskripsi').value;

      if (!judul || !deskripsi) {
        alert('Judul dan Deskripsi wajib diisi!');
        return;
      }

      let listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
      listInfo.unshift({
        judul: judul,
        foto: foto,
        deskripsi: deskripsi,
        waktu: new Date().toLocaleDateString('id-ID')
      });

      localStorage.setItem('rq_aktivitas', JSON.stringify(listInfo));
      alert('Informasi berhasil dipublikasikan!');
      document.getElementById('infoJudul').value = '';
      document.getElementById('infoFotoUrl').value = '';
      document.getElementById('infoDeskripsi').value = '';
      renderAktivitasInfo();
    }

    function hapusAktivitasInfo(idx) {
      if (confirm('Hapus informasi ini?')) {
        let listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
        listInfo.splice(idx, 1);
        localStorage.setItem('rq_aktivitas', JSON.stringify(listInfo));
        renderAktivitasInfo();
      }
    }

    // MANAJEMEN DROPDOWN PENGAJAR
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

    // FORM PENILAIAN DINAMIS
    function renderFormInputsPenilaian() {
      const container = document.getElementById('dynamicFormInputs');
      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      container.innerHTML = '';

      columns.forEach((col, idx) => {
        container.innerHTML += `
          <div class="col-md-4">
            <label class="form-label small fw-bold">${col}</label>
            <input type="text" id="col_input_${idx}" class="form-control form-control-sm" placeholder="Isi ${col}">
          </div>
        `;
      });
    }

    function simpanDataPenilaian(e) {
      e.preventDefault();
      const nama = document.getElementById('inputSantriTarget').value;
      if (!nama) {
        alert('Pilih santri terlebih dahulu!');
        return;
      }

      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      let recordValues = {};

      columns.forEach((col, idx) => {
        let val = document.getElementById(`col_input_${idx}`).value;
        recordValues[col] = val || '-';
      });

      let reportData = {
        kelasKey: currentClassKey,
        nama: nama,
        values: recordValues,
        timestamp: new Date().toISOString()
      };

      let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
      reports.push(reportData);
      localStorage.setItem('rq_reports', JSON.stringify(reports));

      alert('Data penilaian berhasil disimpan!');
      document.getElementById('formPenilaian').reset();
      renderTabelPenilaian();
    }

    // RENDER TABEL PENILAIAN
    function renderTabelPenilaian() {
      const thead = document.getElementById('tabelHeader');
      const tbody = document.getElementById('tabelDataSantri');
      const columns = JSON.parse(localStorage.getItem('rq_columns'));

      // Header
      let headerHtml = `<tr><th style="width:40px;">No</th><th>Nama Santri</th>`;
      columns.forEach(col => {
        headerHtml += `<th>${col}</th>`;
      });
      if (currentRoleCategory === 'Pengajar') {
        headerHtml += `<th style="width:60px;">Aksi</th>`;
      }
      headerHtml += `</tr>`;
      thead.innerHTML = headerHtml;

      // Body
      tbody.innerHTML = '';
      let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');

      // Filter
      let filtered = reports.filter(r => r.kelasKey === currentClassKey);
      if (currentRoleCategory === 'Santri') {
        filtered = filtered.filter(r => r.nama === currentUserName);
      }

      if (filtered.length === 0) {
        let colSpanTotal = columns.length + (currentRoleCategory === 'Pengajar' ? 3 : 2);
        tbody.innerHTML = `<tr><td colspan="${colSpanTotal}" class="text-center text-muted">Belum ada data laporan penilaian.</td></tr>`;
        return;
      }

      filtered.forEach((item, index) => {
        let row = `<tr><td class="text-center">${index + 1}</td><td class="fw-bold">${item.nama}</td>`;
        columns.forEach(col => {
          row += `<td>${item.values[col] || '-'}</td>`;
        });

        if (currentRoleCategory === 'Pengajar') {
          let origIndex = reports.indexOf(item);
          row += `<td class="text-center">
            <button class="btn btn-danger btn-sm py-0 px-2" onclick="hapusPenilaian(${origIndex})">
              <i class="fa-solid fa-trash"></i>
            </button>
          </td>`;
        }
        row += `</tr>`;
        tbody.innerHTML += row;
      });
    }

    function hapusPenilaian(index) {
      if (confirm('Yakin ingin menghapus baris penilaian ini?')) {
        let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
        reports.splice(index, 1);
        localStorage.setItem('rq_reports', JSON.stringify(reports));
        renderTabelPenilaian();
      }
    }

    // TAMBAH SANTRI BARU
    function tambahSantriBaru() {
      const newName = document.getElementById('newSantriName').value.trim();
      if (!newName) {
        alert('Masukkan nama santri baru!');
        return;
      }

      const classSantriKey = "Santri " + currentClassKey;
      let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));

      // Cek Duplikat
      if (dbSantri[classSantriKey].some(s => s.nama.toLowerCase() === newName.toLowerCase())) {
        alert('Nama santri sudah ada di kelas ini!');
        return;
      }

      dbSantri[classSantriKey].push({
        nama: newName,
        pass: newName // Password default = Nama Santri
      });

      localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
      alert(`Santri ${newName} berhasil ditambahkan! Password login default = ${newName}`);
      document.getElementById('newSantriName').value = '';
      loadDropdownSantriPengajar();
    }

    // RESET PASSWORD SANTRI (Oleh Pengajar)
    function resetPasswordSantri() {
      const targetName = document.getElementById('resetSantriTarget').value;
      const newPass = document.getElementById('resetSantriNewPass').value.trim();

      if (!targetName || !newPass) {
        alert('Pilih santri dan isi password baru!');
        return;
      }

      const classSantriKey = "Santri " + currentClassKey;
      let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
      let santri = dbSantri[classSantriKey].find(s => s.nama === targetName);

      if (santri) {
        santri.pass = newPass;
        localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
        alert(`Password santri ${targetName} berhasil diubah!`);
        document.getElementById('resetSantriNewPass').value = '';
      }
    }

    // EDIT KOLOM TABEL
    function renderListKolomSettings() {
      const list = document.getElementById('listKolomTabel');
      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      list.innerHTML = '';

      columns.forEach((col, idx) => {
        list.innerHTML += `
          <li class="list-group-item d-flex justify-content-between align-items-center py-2">
            <span>${col}</span>
            <button class="btn btn-outline-danger btn-sm py-0 px-2" onclick="hapusKolomTabel(${idx})">
              <i class="fa-solid fa-xmark"></i> Hapus
            </button>
          </li>
        `;
      });
    }

    function tambahKolomTabel() {
      const colName = document.getElementById('newColumnName').value.trim();
      if (!colName) {
        alert('Masukkan nama kolom!');
        return;
      }

      let columns = JSON.parse(localStorage.getItem('rq_columns'));
      columns.push(colName);
      localStorage.setItem('rq_columns', JSON.stringify(columns));

      document.getElementById('newColumnName').value = '';
      renderListKolomSettings();
      renderFormInputsPenilaian();
      renderTabelPenilaian();
    }

    function hapusKolomTabel(idx) {
      let columns = JSON.parse(localStorage.getItem('rq_columns'));
      if (columns.length <= 1) {
        alert('Minimal harus ada 1 kolom tabel!');
        return;
      }
      if (confirm(`Hapus kolom "${columns[idx]}"?`)) {
        columns.splice(idx, 1);
        localStorage.setItem('rq_columns', JSON.stringify(columns));
        renderListKolomSettings();
        renderFormInputsPenilaian();
        renderTabelPenilaian();
      }
    }

    // MANAJEMEN LOGO
    function simpanLogoApp() {
      const url = document.getElementById('inputLogoUrl').value.trim();
      if (!url) {
        alert('Masukkan URL logo!');
        return;
      }
      localStorage.setItem('rq_custom_logo', url);
      loadAppLogo();
      alert('Logo aplikasi berhasil diubah!');
    }

    function resetLogoApp() {
      localStorage.removeItem('rq_custom_logo');
      loadAppLogo();
      alert('Logo dikembalikan ke tampilan awal!');
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

    // MODAL GANTI PASSWORD USER
    function openModalGantiPassword() {
      var myModal = new bootstrap.Modal(document.getElementById('modalGantiPassword'));
      myModal.show();
    }

    function simpanGantiPasswordUser() {
      const newPass = document.getElementById('userNewPassInput').value.trim();
      if (!newPass) {
        alert('Masukkan password baru!');
        return;
      }

      if (currentRoleCategory === 'Pengajar') {
        const keyPengajar = "Pengajar " + currentClassKey;
        let passPengajarDb = JSON.parse(localStorage.getItem('rq_pass_pengajar'));
        passPengajarDb[keyPengajar] = newPass;
        localStorage.setItem('rq_pass_pengajar', JSON.stringify(passPengajarDb));
        alert('Password Pengajar berhasil diubah!');
      } else {
        const classSantriKey = "Santri " + currentClassKey;
        let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));
        let santri = dbSantri[classSantriKey].find(s => s.nama === currentUserName);
        if (santri) {
          santri.pass = newPass;
          localStorage.setItem('rq_santri_db', JSON.stringify(dbSantri));
          alert('Password Santri berhasil diubah!');
        }
      }

      document.getElementById('userNewPassInput').value = '';
      bootstrap.Modal.getInstance(document.getElementById('modalGantiPassword')).hide();
    }

    // LOGOUT
    function logout() {
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
      document.getElementById('loginPassword').value = '';
      currentRoleCategory = "";
      currentClassKey = "";
      currentUserName = "";
    }
  </script>

  <!-- Bootstrap 5 JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
