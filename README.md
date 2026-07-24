<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Rumah Qur'an - Darul Arqam</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --bg-color: #FFF8DC;
      --main-color: #D4AF37;
      --dark-color: #B8860B;
      --text-color: #212529;
    }
    
    body { 
      background-color: var(--bg-color); 
      color: var(--text-color);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      padding-bottom: 75px; /* Ruang untuk Bottom Navbar */
    }

    /* Mobile First Styling */
    .mobile-container {
      max-width: 500px;
      margin: 0 auto;
      padding: 12px;
    }

    .card-mobile {
      border: 1.5px solid var(--main-color);
      border-radius: 16px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
      background: white;
    }

    .btn-theme {
      background-color: var(--main-color);
      color: white;
      font-weight: 600;
      border-radius: 10px;
      border: none;
    }
    .btn-theme:hover, .btn-theme:active {
      background-color: var(--dark-color);
      color: white;
    }

    .text-theme { color: var(--dark-color) !important; }

    /* Bottom Navigation Bar */
    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: 65px;
      background: white;
      border-top: 1px solid #ddd;
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
      color: var(--dark-color);
      font-weight: bold;
    }

    .app-logo { 
      max-width: 90px; 
      max-height: 90px; 
      object-fit: contain; 
      border-radius: 12px;
    }

    .preview-img {
      max-width: 100%;
      height: auto;
      border-radius: 10px;
      margin-top: 8px;
    }

    .table-responsive {
      border-radius: 10px;
      overflow: hidden;
    }
  </style>
</head>
<body>

  <!-- ================= HALAMAN LOGIN ================= -->
  <div id="loginPage" class="mobile-container pt-4">
    <div class="card card-mobile text-center p-4">
      <div class="mb-3 text-center">
        <i class="fa-solid fa-book-quran text-theme display-3" id="defaultLogo"></i>
        <img id="customLogo" src="" class="app-logo d-none" alt="Logo Aplikasi">
      </div>
      <h4 class="fw-bold text-theme mb-1">Rumah Qur'an</h4>
      <p class="text-muted small mb-4">Darul Arqam</p>

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
    
    <!-- Status Access Header -->
    <div class="alert alert-sm bg-white border-0 shadow-sm p-3 mb-3 rounded-4">
      <div class="d-flex align-items-center">
        <div class="me-3 fs-3 text-theme"><i class="fa-solid fa-circle-user"></i></div>
        <div>
          <h6 class="mb-0 fw-bold" id="userRoleTitle">Santri Access</h6>
          <small class="text-muted" id="userRoleSubtitle">Mustawa Awwal</small>
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
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-table me-1"></i> Laporan Perkembangan</h6>
        <div class="table-responsive">
          <table class="table table-bordered align-middle text-center small">
            <thead class="table-dark" id="tabelHeader"></thead>
            <tbody id="tabelDataSantri"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- VIEW 3: INPUT NILAI (KHUSUS PENGAJAR) -->
    <div id="viewInputNilai" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-pen-to-square me-1"></i> Form Input Penilaian</h6>
        <form onsubmit="simpanDataPenilaian(event)">
          <div class="mb-2">
            <label class="form-label small fw-bold">Pilih Santri</label>
            <select class="form-select form-select-sm" id="inputSantriTarget" required></select>
          </div>
          <div id="dynamicFormInputs" class="row g-2"></div>
          <button type="submit" class="btn btn-theme w-100 btn-sm mt-3">Simpan Penilaian</button>
        </form>
      </div>
    </div>

    <!-- VIEW 4: MENU PENGATURAN (KHUSUS PENGAJAR) -->
    <div id="viewPengaturan" class="dashboard-view d-none">
      
      <!-- Upload Aktivitas -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-upload me-1"></i> Upload Informasi / Foto Aktivitas</h6>
        <div class="mb-2">
          <input type="text" id="infoJudul" class="form-control form-control-sm" placeholder="Judul Informasi">
        </div>
        <div class="mb-2">
          <label class="form-label small mb-1">Foto dari HP (Opsional)</label>
          <input type="file" id="infoFotoFile" accept="image/*" class="form-control form-control-sm">
        </div>
        <div class="mb-2">
          <textarea id="infoDeskripsi" class="form-control form-control-sm" rows="2" placeholder="Tulis rincian/keterangan..."></textarea>
        </div>
        <button class="btn btn-theme btn-sm w-100" onclick="simpanAktivitasInfo()">Publikasikan Info</button>
      </div>

      <!-- Tambah Santri Baru -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-user-plus me-1"></i> Tambah Santri Baru</h6>
        <div class="input-group input-group-sm mb-2">
          <input type="text" id="newSantriName" class="form-control" placeholder="Nama Santri Baru">
          <button class="btn btn-theme" onclick="tambahSantriBaru()">Tambah</button>
        </div>
      </div>

      <!-- Ubah Sandi Santri -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-key me-1"></i> Ubah Sandi Santri</h6>
        <div class="mb-2">
          <select class="form-select form-select-sm" id="resetSantriTarget"></select>
        </div>
        <div class="input-group input-group-sm mb-2">
          <input type="text" id="resetSantriNewPass" class="form-control" placeholder="Password Baru">
          <button class="btn btn-warning fw-bold" onclick="resetPasswordSantri()">Reset</button>
        </div>
      </div>

      <!-- Pengaturan Logo & Background -->
      <div class="card card-mobile p-3 mb-3">
        <h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-sliders me-1"></i> Tampilan Aplikasi</h6>
        
        <!-- Ubah Logo Upload dari HP -->
        <div class="mb-3">
          <label class="form-label small fw-bold mb-1">Ubah Logo Login (Upload Foto dari HP)</label>
          <input type="file" id="inputLogoFile" accept="image/*" class="form-control form-control-sm mb-2">
          <button class="btn btn-theme btn-sm w-100 mb-1" onclick="simpanLogoApp()">Simpan Logo Baru</button>
          <button class="btn btn-outline-secondary btn-sm w-100" onclick="resetLogoApp()">Reset Logo Bawaan</button>
        </div>

        <hr class="my-2">

        <!-- Ubah Warna Background -->
        <div>
          <label class="form-label small fw-bold mb-1">Ubah Warna Background</label>
          <div class="d-flex gap-2">
            <button class="btn btn-sm flex-fill" style="background:#FFF8DC; border:1px solid #ccc;" onclick="setThemeBg('#FFF8DC','#D4AF37','#B8860B')">Kuning Emas</button>
            <button class="btn btn-sm flex-fill text-white" style="background:#198754;" onclick="setThemeBg('#E8F5E9','#198754','#0F5132')">Hijau</button>
            <button class="btn btn-sm flex-fill text-white" style="background:#0d6efd;" onclick="setThemeBg('#E3F2FD','#0d6efd','#0A58CA')">Biru</button>
          </div>
        </div>
      </div>

    </div>

    <!-- VIEW 5: MENU AKUN (RESET/GANTI PASS & KELUAR) -->
    <div id="viewAkun" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3 text-center">
        <h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun & Akses</h6>
        
        <div class="mb-4 text-start">
          <label class="form-label small fw-bold">Ganti Password Akses Anda</label>
          <input type="password" id="userNewPassInput" class="form-control form-control-sm mb-2" placeholder="Masukkan Password Baru">
          <button class="btn btn-theme btn-sm w-100" onclick="simpanGantiPasswordUser()">Simpan Password Baru</button>
        </div>

        <hr>

        <button class="btn btn-outline-danger btn-sm w-100 fw-bold py-2 mt-2" onclick="logout()">
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

  <!-- JAVASCRIPT LOGIC -->
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

    let currentRoleCategory = "";
    let currentClassKey = "";
    let currentUserName = "";

    function initDatabase() {
      if (!localStorage.getItem('rq_santri_db')) {
        let dbSantri = {};
        Object.keys(defaultDataSantri).forEach(key => {
          dbSantri[key] = defaultDataSantri[key].map(nama => ({ nama: nama, pass: nama }));
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

      // Load Saved Theme
      const savedTheme = JSON.parse(localStorage.getItem('rq_theme_bg') || 'null');
      if (savedTheme) {
        setThemeBg(savedTheme.bg, savedTheme.main, savedTheme.dark, false);
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
      const passwordInput = document.getElementById('loginPassword').value;

      if (loginType.startsWith('Pengajar')) {
        let passPengajarDb = JSON.parse(localStorage.getItem('rq_pass_pengajar'));
        if (passwordInput === passPengajarDb[loginType]) {
          currentRoleCategory = "Pengajar";
          currentClassKey = loginType.replace("Pengajar ", "");
          bukaDashboard();
        } else {
          alert("pasword sampean salah silahkan ulangi lagi");
        }
      } else {
        const selectedSantri = document.getElementById('loginSantriName').value;
        if (!selectedSantri) {
          alert("pasword sampean salah silahkan ulangi lagi");
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
          alert("pasword sampean salah silahkan ulangi lagi");
        }
      }
    }

    function bukaDashboard() {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');
      document.getElementById('bottomNav').classList.remove('d-none');

      document.getElementById('userRoleTitle').innerText = currentRoleCategory === 'Pengajar' ? `Pengajar Kelas ${currentClassKey}` : currentUserName;
      document.getElementById('userRoleSubtitle').innerText = `Mustawa ${currentClassKey}`;

      const navInputNilai = document.getElementById('navInputNilai');
      const navPengaturan = document.getElementById('navPengaturan');

      if (currentRoleCategory === 'Pengajar') {
        navInputNilai.classList.remove('d-none');
        navPengaturan.classList.remove('d-none');
        loadDropdownSantriPengajar();
        renderFormInputsPenilaian();
      } else {
        navInputNilai.classList.add('d-none');
        navPengaturan.classList.add('d-none');
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

    // UPLOAD FOTO AKTIVITAS LOKAL (HP)
    function simpanAktivitasInfo() {
      const judul = document.getElementById('infoJudul').value;
      const fileInput = document.getElementById('infoFotoFile');
      const deskripsi = document.getElementById('infoDeskripsi').value;

      if (!judul || !deskripsi) {
        alert('Judul dan deskripsi wajib diisi!');
        return;
      }

      const processSave = (fotoData) => {
        let listInfo = JSON.parse(localStorage.getItem('rq_aktivitas') || '[]');
        listInfo.unshift({
          judul: judul,
          foto: fotoData || '',
          deskripsi: deskripsi,
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
        reader.onload = function(e) {
          processSave(e.target.result);
        };
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

    // FORM PENILAIAN
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

    function renderFormInputsPenilaian() {
      const container = document.getElementById('dynamicFormInputs');
      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      container.innerHTML = '';

      columns.forEach((col, idx) => {
        container.innerHTML += `
          <div class="col-6">
            <label class="form-label style="font-size:10px;" class="fw-bold">${col}</label>
            <input type="text" id="col_input_${idx}" class="form-control form-control-sm" placeholder="Isi...">
          </div>
        `;
      });
    }

    function simpanDataPenilaian(e) {
      e.preventDefault();
      const nama = document.getElementById('inputSantriTarget').value;
      if (!nama) return alert('Pilih santri!');

      const columns = JSON.parse(localStorage.getItem('rq_columns'));
      let recordValues = {};

      columns.forEach((col, idx) => {
        recordValues[col] = document.getElementById(`col_input_${idx}`).value || '-';
      });

      let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
      reports.push({ kelasKey: currentClassKey, nama: nama, values: recordValues });
      localStorage.setItem('rq_reports', JSON.stringify(reports));

      alert('Penilaian tersimpan!');
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
      if (currentRoleCategory === 'Santri') filtered = filtered.filter(r => r.nama === currentUserName);

      if (filtered.length === 0) {
        tbody.innerHTML = `<tr><td colspan="${columns.length + 3}" class="text-muted">Belum ada data.</td></tr>`;
        return;
      }

      filtered.forEach((item, index) => {
        let row = `<tr><td>${index + 1}</td><td class="fw-bold">${item.nama}</td>`;
        columns.forEach(col => row += `<td>${item.values[col] || '-'}</td>`);
        if (currentRoleCategory === 'Pengajar') {
          let origIndex = reports.indexOf(item);
          row += `<td><button class="btn btn-danger btn-sm py-0 px-1" onclick="hapusPenilaian(${origIndex})"><i class="fa-solid fa-trash"></i></button></td>`;
        }
        row += `</tr>`;
        tbody.innerHTML += row;
      });
    }

    function hapusPenilaian(index) {
      if (confirm('Hapus baris penilaian ini?')) {
        let reports = JSON.parse(localStorage.getItem('rq_reports') || '[]');
        reports.splice(index, 1);
        localStorage.setItem('rq_reports', JSON.stringify(reports));
        renderTabelPenilaian();
      }
    }

    function tambahSantriBaru() {
      const newName = document.getElementById('newSantriName').value.trim();
      if (!newName) return alert('Isi nama santri!');

      const classSantriKey = "Santri " + currentClassKey;
      let dbSantri = JSON.parse(localStorage.getItem('rq_santri_db'));

      dbSantri[classSantriKey].push({ nama: newName, pass: newName });
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

    // LOGO UPLOAD
    function simpanLogoApp() {
      const fileInput = document.getElementById('inputLogoFile');
      if (fileInput.files && fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = function(e) {
          localStorage.setItem('rq_custom_logo', e.target.result);
          loadAppLogo();
          alert('Logo aplikasi diubah!');
        };
        reader.readAsDataURL(fileInput.files[0]);
      } else {
        alert('Pilih foto logo dari HP terlebih dahulu!');
      }
    }

    function resetLogoApp() {
      localStorage.removeItem('rq_custom_logo');
      loadAppLogo();
      alert('Logo dikembalikan!');
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

    // TEMA BACKGROUND
    function setThemeBg(bgColor, mainColor, darkColor, save = true) {
      document.documentElement.style.setProperty('--bg-color', bgColor);
      document.documentElement.style.setProperty('--main-color', mainColor);
      document.documentElement.style.setProperty('--dark-color', darkColor);

      if (save) {
        localStorage.setItem('rq_theme_bg', JSON.stringify({ bg: bgColor, main: mainColor, dark: darkColor }));
        alert('Warna background diubah!');
      }
    }

    // GANTI PASSWORD USER
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

      alert('Password Anda berhasil diperbarui!');
      document.getElementById('userNewPassInput').value = '';
    }

    function logout() {
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('bottomNav').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
      document.getElementById('loginPassword').value = '';
    }
  </script>

  <!-- Bootstrap 5 JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
