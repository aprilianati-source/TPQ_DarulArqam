<!DOCTYPE html>
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
    body { background-color: #f4f6f9; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    .login-card { max-width: 450px; margin: 40px auto; border-radius: 12px; }
    .table-responsive { background: white; border-radius: 8px; padding: 15px; }
    th { vertical-align: middle; text-align: center; }
  </style>
</head>
<body>

  <!-- ================= HALAMAN LOGIN ================= -->
  <div id="loginPage" class="container">
    <div class="card shadow-sm p-4 login-card text-center">
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
        <label class="form-label fw-bold">Nama Santri (Sebagai Password)</label>
        <input type="text" class="form-control" id="santriPassword" placeholder="Masukkan Nama Lengkap Santri">
        <small class="text-muted d-block mt-1">*Contoh: <code>Ahmad Arkhan Wiratama</code></small>
      </div>

      <!-- Input Password Admin -->
      <div class="mb-3 text-start d-none" id="adminInputGroup">
        <label class="form-label fw-bold">Password Ustaz / Ustadzah</label>
        <input type="password" class="form-control" id="adminPassword" placeholder="Masukkan Password Ustaz/Ustadzah">
      </div>

      <!-- Tombol Masuk -->
      <button class="btn btn-success w-100 fw-bold py-2 shadow-sm mt-2" onclick="handleLogin()">
        Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>

      <p class="text-muted fs-7 mt-3 mb-0 small">
        *Pass Ustadzah: <code class="text-danger">darul123</code>
      </p>
    </div>
  </div>

  <!-- ================= HALAMAN DASHBOARD ================= -->
  <div id="dashboardPage" class="container-fluid px-4 mt-4 d-none">
    <div class="d-flex justify-content-between align-items-center mb-3">
      <div>
        <h3 class="fw-bold text-success mb-0">Laporan Perkembangan Santri</h3>
        <span class="text-muted">Rumah Qur'an Darul Arqam</span>
      </div>
      <button class="btn btn-outline-danger btn-sm" onclick="logout()">
        <i class="fa-solid fa-right-from-bracket"></i> Keluar
      </button>
    </div>

    <!-- Status Badge -->
    <div class="alert alert-info py-2" id="userRoleBadge"></div>

    <!-- FORM INPUT KHUSUS USTADZ / USTADZAH -->
    <div id="adminPanel" class="card p-3 mb-4 border-success d-none shadow-sm">
      <h5 class="text-success fw-bold"><i class="fa-solid fa-pen-to-square"></i> Form Input Perkembangan Santri</h5>
      <hr class="my-2">
      <form id="formPenilaian" onsubmit="simpanData(event)">
        <div class="row g-3">
          <!-- Pilih Santri -->
          <div class="col-md-6">
            <label class="form-label fw-bold">Pilih Santri</label>
            <select class="form-select" id="inputSantri" required>
              <option value="">-- Pilih Santri --</option>
            </select>
          </div>

          <div class="col-md-6">
            <label class="form-label fw-bold">Presentase Kehadiran (%)</label>
            <input type="text" id="inputKehadiran" class="form-control" placeholder="Contoh: 95%">
          </div>

          <!-- Iqro -->
          <div class="col-md-6">
            <label class="form-label fw-bold text-primary">Iqro - Capaian</label>
            <input type="text" id="inputIqroCapaian" class="form-control" placeholder="Jilid 3 Hal 12">
          </div>
          <div class="col-md-6">
            <label class="form-label fw-bold text-primary">Iqro - Catatan</label>
            <input type="text" id="inputIqroCatatan" class="form-control" placeholder="Perhatikan Mad Asli">
          </div>

          <!-- Hafalan Surat -->
          <div class="col-md-4">
            <label class="form-label fw-bold text-success">Hafalan Surat - Muraja'ah</label>
            <input type="text" id="inputSuratMurajaah" class="form-control" placeholder="An-Naba - An-Nazi'at">
          </div>
          <div class="col-md-4">
            <label class="form-label fw-bold text-success">Hafalan Surat - Ziyadah</label>
            <input type="text" id="inputSuratZiyadah" class="form-control" placeholder="Abasa 1-15">
          </div>
          <div class="col-md-4">
            <label class="form-label fw-bold text-success">Hafalan Surat - Catatan</label>
            <input type="text" id="inputSuratCatatan" class="form-control" placeholder="Kelancaran cukup baik">
          </div>

          <!-- Hafalan Lainnya -->
          <div class="col-md-4">
            <label class="form-label fw-bold text-warning">Hafalan - Hadits</label>
            <input type="text" id="inputHadits" class="form-control" placeholder="Hadits Niat">
          </div>
          <div class="col-md-4">
            <label class="form-label fw-bold text-warning">Hafalan - Matan</label>
            <input type="text" id="inputMatan" class="form-control" placeholder="-">
          </div>
          <div class="col-md-4">
            <label class="form-label fw-bold text-warning">Hafalan - Do'a</label>
            <input type="text" id="inputDoa" class="form-control" placeholder="Doa Sebelum Makan">
          </div>

          <!-- Catatan Akhlak -->
          <div class="col-12">
            <label class="form-label fw-bold text-secondary">Catatan Akhlak di Rumah Qur'an</label>
            <textarea id="inputAkhlak" class="form-control" rows="2" placeholder="Sopan, tekun, dan tenang saat menyimak."></textarea>
          </div>

          <div class="col-12 text-end">
            <button type="submit" class="btn btn-success fw-bold px-4">
              <i class="fa-solid fa-floppy-disk"></i> Simpan / Tambah Perkembangan
            </button>
          </div>
        </div>
      </form>
    </div>

    <!-- TABEL LAPORAN PENILAIAN -->
    <div class="card p-3 shadow-sm mb-5">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h5 class="fw-bold mb-0" id="tabelJudul"><i class="fa-solid fa-table"></i> Laporan Perkembangan</h5>
      </div>

      <div class="table-responsive">
        <table class="table table-bordered align-middle">
          <thead class="table-dark small">
            <tr>
              <th rowspan="2" style="width: 50px;">No</th>
              <th rowspan="2">Nama Santri</th>
              <th colspan="2" class="table-primary text-dark">Iqro</th>
              <th colspan="3" class="table-success text-dark">Hafalan Surat</th>
              <th colspan="3" class="table-warning text-dark">Hafalan Lainnya</th>
              <th rowspan="2" class="table-secondary text-dark">Catatan Akhlak di Rumah Qur'an</th>
              <th rowspan="2">Presentase Kehadiran</th>
            </tr>
            <tr>
              <!-- Iqro -->
              <th class="table-primary text-dark">Capaian</th>
              <th class="table-primary text-dark">Catatan</th>
              <!-- Hafalan Surat -->
              <th class="table-success text-dark">Muraja'ah</th>
              <th class="table-success text-dark">Ziyadah</th>
              <th class="table-success text-dark">Catatan</th>
              <!-- Hafalan Lainnya -->
              <th class="table-warning text-dark">Hadits</th>
              <th class="table-warning text-dark">Matan</th>
              <th class="table-warning text-dark">Do'a</th>
            </tr>
          </thead>
          <tbody id="tabelDataSantri" class="small">
            <!-- Data dimasukkan secara dinamis melalui JavaScript -->
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- JAVASCRIPT LOGIC -->
  <script>
    // DATA MASTER SANTRI SAMA KELASNYA
    const daftarSantri = [
      // KELAS MUSTAWA AWWAL
      { nama: "Ahmad Arkhan Wiratama", kelas: "Mustawa Awwal" },
      { nama: "Aishwa Nasha Razeeta", kelas: "Mustawa Awwal" },
      { nama: "Al Afkar Syabani", kelas: "Mustawa Awwal" },
      { nama: "Ananda Aisyah Syahidal Syail", kelas: "Mustawa Awwal" },
      { nama: "Aqila Rafania Adifa", kelas: "Mustawa Awwal" },
      { nama: "Arisha Fatimah", kelas: "Mustawa Awwal" },
      { nama: "Asyila Rahma Khadijah", kelas: "Mustawa Awwal" },
      { nama: "Athaya Humaira Althafia", kelas: "Mustawa Awwal" },
      { nama: "Bilal Zayyan Prinoza", kelas: "Mustawa Awwal" },
      { nama: "Desya Salsaila", kelas: "Mustawa Awwal" },
      { nama: "Fatimah", kelas: "Mustawa Awwal" },
      { nama: "Kenzie Attaya Depa", kelas: "Mustawa Awwal" },
      { nama: "Khuzaimah Summayyah", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Adzriel Rafif Fakhri", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Alfatih Rinaldi", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Alzaahiy Rinaldi", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Dzaky", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Fathian Shariq", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Gibran Saguftha", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Ibrahim Al Fatih Isnanto", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Razka Destha Athallah", kelas: "Mustawa Awwal" },
      { nama: "Muhammad Ustman", kelas: "Mustawa Awwal" },
      { nama: "Qahirah Arsylia Aftarinda", kelas: "Mustawa Awwal" },
      { nama: "Raid Asadel", kelas: "Mustawa Awwal" },
      { nama: "Shaqueena Salma Aryanta", kelas: "Mustawa Awwal" },
      { nama: "Shofiya Azzahra Hidayatulloh", kelas: "Mustawa Awwal" },
      { nama: "Sultan Ibrahim Akbar", kelas: "Mustawa Awwal" },
      { nama: "Syafia Az Zahra", kelas: "Mustawa Awwal" },
      { nama: "Syahfira Destriani", kelas: "Mustawa Awwal" },
      { nama: "Syahrika Destriani", kelas: "Mustawa Awwal" },
      { nama: "Syakira Beatric Setiawan", kelas: "Mustawa Awwal" },
      { nama: "Tsanwa Chayra Variin", kelas: "Mustawa Awwal" },
      { nama: "Vesha Sakilla", kelas: "Mustawa Awwal" },
      { nama: "Yusuf Al Fawwaz", kelas: "Mustawa Awwal" },
      { nama: "Zea Mikhayla Almeera Yendra", kelas: "Mustawa Awwal" },

      // KELAS MUSTAWA TSANI
      { nama: "Abdurrahman", kelas: "Mustawa Tsani" },
      { nama: "Akhtar Muhammad Rafasya", kelas: "Mustawa Tsani" },
      { nama: "Al Ghany Pratama", kelas: "Mustawa Tsani" },
      { nama: "Al Hando Pranstio", kelas: "Mustawa Tsani" },
      { nama: "Alfarizqi Khairan Yazid", kelas: "Mustawa Tsani" },
      { nama: "Anina Yumna Sakhi", kelas: "Mustawa Tsani" },
      { nama: "Binar Al Biru Chandra", kelas: "Mustawa Tsani" },
      { nama: "Chaerunnisa Fathiyaturahma", kelas: "Mustawa Tsani" },
      { nama: "Habiburahman El Shirazy", kelas: "Mustawa Tsani" },
      { nama: "Hana Shabiya Vina Pakpahan", kelas: "Mustawa Tsani" },
      { nama: "Keenan Ghayda Sakhi", kelas: "Mustawa Tsani" },
      { nama: "Keisha Chessy Tri Adiva", kelas: "Mustawa Tsani" },
      { nama: "Khadijah Athiyyah Samreno", kelas: "Mustawa Tsani" },
      { nama: "Khaif Shakiel Badillah", kelas: "Mustawa Tsani" },
      { nama: "Maryam Intan Dzakiyah", kelas: "Mustawa Tsani" },
      { nama: "Molin Sanjaya", kelas: "Mustawa Tsani" },
      { nama: "Muhamad Ibrahim Hidayatulloh", kelas: "Mustawa Tsani" },
      { nama: "Muhammad Al-Ghazello Arief", kelas: "Mustawa Tsani" },
      { nama: "Muhammad Hamiz Tabrani", kelas: "Mustawa Tsani" },
      { nama: "Muhammad Raihan Wildra", kelas: "Mustawa Tsani" },
      { nama: "Prisha Humairah", kelas: "Mustawa Tsani" },
      { nama: "Qallesha Louis Nawalla", kelas: "Mustawa Tsani" },
      { nama: "Risya Naifah Andami", kelas: "Mustawa Tsani" },
      { nama: "Rosa Adeliya", kelas: "Mustawa Tsani" },
      { nama: "Salsabila Putri Ayoenie Alfarizi", kelas: "Mustawa Tsani" },
      { nama: "Shaffiyah Mecca Al Fatih Isnanto", kelas: "Mustawa Tsani" },
      { nama: "Syifa Nursabrina Robka", kelas: "Mustawa Tsani" },
      { nama: "Syifa Oktaviani", kelas: "Mustawa Tsani" },
      { nama: "Zaim Faqih Alrasyid", kelas: "Mustawa Tsani" },
      { nama: "Ziyadah Khaira Pakpahan", kelas: "Mustawa Tsani" }
    ];

    let currentRole = "";
    let currentSantriName = "";

    // Inisialisasi Dropdown Santri untuk Admin
    window.onload = function() {
      const select = document.getElementById('inputSantri');
      daftarSantri.forEach(s => {
        let opt = document.createElement('option');
        opt.value = s.nama;
        opt.innerHTML = `${s.nama} (${s.kelas})`;
        select.appendChild(opt);
      });
    }

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

    function handleLogin() {
      const loginType = document.getElementById('loginType').value;

      if (loginType === 'admin') {
        const passAdmin = document.getElementById('adminPassword').value;
        if (passAdmin === 'darul123') {
          currentRole = 'admin';
          bukaDashboard();
        } else {
          alert('Password Ustaz/Ustadzah Salah! Gunakan: darul123');
        }
      } else {
        const inputName = document.getElementById('santriPassword').value.trim();
        const found = daftarSantri.find(s => s.nama.toLowerCase() === inputName.toLowerCase());

        if (found) {
          currentRole = 'santri';
          currentSantriName = found.nama;
          bukaDashboard();
        } else {
          alert('Nama tidak ditemukan dalam daftar santri! Pastikan ejaan nama sesuai.');
        }
      }
    }

    function bukaDashboard() {
      document.getElementById('loginPage').classList.add('d-none');
      document.getElementById('dashboardPage').classList.remove('d-none');

      const roleBadge = document.getElementById('userRoleBadge');
      const adminPanel = document.getElementById('adminPanel');

      if (currentRole === 'admin') {
        roleBadge.className = "alert alert-success";
        roleBadge.innerHTML = "<strong>Mode Akses: Ustaz / Ustadzah.</strong> Anda dapat menambah & memperbarui laporan perkembangan santri.";
        adminPanel.classList.remove('d-none');
      } else {
        roleBadge.className = "alert alert-warning";
        roleBadge.innerHTML = `<strong>Mode Akses: Wali Santri (${currentSantriName}).</strong> Menampilkan Laporan Perkembangan Santri.`;
        adminPanel.classList.add('d-none');
      }

      renderTabel();
    }

    function simpanData(e) {
      e.preventDefault();
      const nama = document.getElementById('inputSantri').value;
      if (!nama) {
        alert('Silakan pilih nama santri terlebih dahulu!');
        return;
      }

      const reportData = {
        nama: nama,
        iqroCapaian: document.getElementById('inputIqroCapaian').value || '-',
        iqroCatatan: document.getElementById('inputIqroCatatan').value || '-',
        suratMurajaah: document.getElementById('inputSuratMurajaah').value || '-',
        suratZiyadah: document.getElementById('inputSuratZiyadah').value || '-',
        suratCatatan: document.getElementById('inputSuratCatatan').value || '-',
        hadits: document.getElementById('inputHadits').value || '-',
        matan: document.getElementById('inputMatan').value || '-',
        doa: document.getElementById('inputDoa').value || '-',
        akhlak: document.getElementById('inputAkhlak').value || '-',
        kehadiran: document.getElementById('inputKehadiran').value || '-'
      };

      // Simpan ke LocalStorage
      let db = JSON.parse(localStorage.getItem('rq_reports') || '{}');
      if (!db[nama]) db[nama] = [];
      db[nama].push(reportData);
      localStorage.setItem('rq_reports', JSON.stringify(db));

      alert('Data penilaian berhasil disimpan!');
      document.getElementById('formPenilaian').reset();
      renderTabel();
    }

    function renderTabel() {
      const tbody = document.getElementById('tabelDataSantri');
      tbody.innerHTML = '';
      const db = JSON.parse(localStorage.getItem('rq_reports') || '{}');

      let recordsToDisplay = [];

      if (currentRole === 'admin') {
        // Tampilkan semua data yang pernah tersimpan
        Object.keys(db).forEach(nama => {
          db[nama].forEach(item => recordsToDisplay.push(item));
        });
      } else {
        // Santri hanya lihat miliknya sendiri
        if (db[currentSantriName]) {
          recordsToDisplay = db[currentSantriName];
        }
      }

      if (recordsToDisplay.length === 0) {
        tbody.innerHTML = `<tr><td colspan="11" class="text-center text-muted">Belum ada data laporan perkembangan.</td></tr>`;
        return;
      }

      recordsToDisplay.forEach((item, index) => {
        const row = `
          <tr>
            <td class="text-center">${index + 1}</td>
            <td class="fw-bold">${item.nama}</td>
            <td>${item.iqroCapaian}</td>
            <td>${item.iqroCatatan}</td>
            <td>${item.suratMurajaah}</td>
            <td>${item.suratZiyadah}</td>
            <td>${item.suratCatatan}</td>
            <td>${item.hadits}</td>
            <td>${item.matan}</td>
            <td>${item.doa}</td>
            <td>${item.akhlak}</td>
            <td class="text-center fw-bold">${item.kehadiran}</td>
          </tr>
        `;
        tbody.innerHTML += row;
      });
    }

    function logout() {
      document.getElementById('dashboardPage').classList.add('d-none');
      document.getElementById('loginPage').classList.remove('d-none');
      document.getElementById('adminPassword').value = '';
      document.getElementById('santriPassword').value = '';
      currentRole = "";
      currentSantriName = "";
    }
  </script>

  <!-- Bootstrap 5 JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
