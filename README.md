
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Rumah Qur'an Darul Arqam</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage-compat.js"></script>

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

    .mobile-container { max-width: 500px; margin: 0 auto; padding: 12px; }
    .card-mobile { border: 1.5px solid rgba(0,0,0,0.1); border-radius: 20px; box-shadow: 0 4px 12px rgba(0,0,0,0.03); background: white; overflow: hidden; }
    .btn-theme { background-color: var(--main-color); color: white; font-weight: 600; border-radius: 12px; border: none; transition: background-color 0.3s ease; }
    .btn-theme:hover, .btn-theme:active { background-color: var(--dark-color); color: white; }
    .text-theme { color: var(--main-color) !important; }
    .app-logo-wrapper { width: 110px; height: 110px; margin: 0 auto; border-radius: 20px; background: #fff; border: 2px solid var(--main-color); display: flex; align-items: center; justify-content: center; overflow: hidden; box-shadow: 0 4px 10px rgba(0,0,0,0.06); }
    .app-logo { width: 100%; height: 100%; object-fit: cover; }
    .user-profile-img { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; border: 2px solid var(--main-color); }
    .bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; height: 65px; background: white; border-top: 1px solid #e0e0e0; display: flex; justify-content: space-around; align-items: center; z-index: 1000; box-shadow: 0 -2px 10px rgba(0,0,0,0.05); }
    .bottom-nav-item { text-align: center; color: #6c757d; font-size: 11px; text-decoration: none; background: none; border: none; flex: 1; }
    .bottom-nav-item i { font-size: 18px; display: block; margin-bottom: 2px; }
    .bottom-nav-item.active { color: var(--main-color); font-weight: bold; }
    .preview-img { max-width: 100%; height: auto; border-radius: 10px; margin-top: 8px; }
    .table-responsive { border-radius: 10px; overflow-x: auto; -webkit-overflow-scrolling: touch; }
    .table-responsive::-webkit-scrollbar { height: 4px; }
    .table-responsive::-webkit-scrollbar-thumb { background: var(--main-color); border-radius: 10px; }
    th, td { white-space: nowrap; vertical-align: middle; }

    /* Elemen Tambahan */
    .loading-overlay { position: fixed; inset: 0; background: rgba(255,255,255,0.92); display: flex; flex-direction: column; align-items: center; justify-content: center; z-index: 9999; gap: 1rem; }
    .spinner-theme { color: var(--main-color); width: 3rem; height: 3rem; }
    .offline-bar { position: fixed; top:0; left:0; right:0; background: #dc3545; color:white; text-align:center; padding:6px; font-size:12px; z-index:9998; display:none; }
    .toast-box { position: fixed; top:15px; right:15px; z-index:10000; max-width:320px; }
  </style>
</head>
<body>
  <!-- Indikator Status -->
  <div class="offline-bar" id="offlineBar">
    <i class="fa-solid fa-wifi-slash"></i> Koneksi terputus... mencoba menyambung kembali
  </div>

  <!-- Layar Loading -->
  <div class="loading-overlay" id="loadingScreen">
    <div class="spinner-border spinner-theme"></div>
    <span class="text-theme fw-bold">Memuat Aplikasi...</span>
  </div>

  <!-- Tempat Notifikasi -->
  <div class="toast-box" id="toastContainer"></div>

  <!-- HALAMAN LOGIN -->
  <div id="loginPage" class="mobile-container pt-4 d-none">
    <div class="card card-mobile text-center p-4">
      <div class="mb-3">
        <div class="app-logo-wrapper">
          <i class="fa-solid fa-book-quran text-theme display-4" id="defaultLogo"></i>
          <img id="customLogo" src="" class="app-logo d-none" alt="Logo">
        </div>
      </div>
      <h3 class="fw-bold text-theme mb-0">Rumah Qur'an Darul Arqam</h3>
      <p class="text-muted small mb-4">Aplikasi Administrasi & Perkembangan Santri</p>

      <div class="mb-3 text-start">
        <label class="form-label fw-bold small">Tipe Login</label>
        <select class="form-select form-select-sm" id="loginType" onchange="toggleLoginInputs()">
          <optgroup label="Akses Santri / Wali">
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

      <button class="btn btn-theme w-100 py-2" onclick="handleLogin()">
        Masuk <i class="fa-solid fa-arrow-right-to-bracket ms-1"></i>
      </button>
    </div>
  </div>

  <!-- HALAMAN DASHBOARD -->
  <div id="dashboardPage" class="mobile-container d-none">
    <div class="mb-3"><h2 class="fw-bold text-theme text-decoration-underline">Rumah Qur'an Darul Arqam</h2></div>
    <div class="card card-mobile p-3 mb-3">
      <div class="d-flex align-items-center">
        <div class="me-3" id="headerAvatarContainer"><i class="fa-solid fa-circle-user fs-1 text-theme"></i></div>
        <div>
          <h5 class="mb-0 fw-bold" id="userRoleTitle"></h5>
          <span class="text-muted small" id="userRoleSubtitle"></span>
        </div>
      </div>
    </div>

    <!-- Isi Tampilan Sama Persis Seperti Kode Asli -->
    <div id="viewAktivitas" class="dashboard-view"><div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-bullhorn me-1"></i> Informasi & Aktivitas</h6><div id="containerAktivitas"></div></div></div>
    <div id="viewLaporan" class="dashboard-view d-none"><div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-table me-2"></i>Laporan Perkembangan</h6><div class="table-responsive"><table class="table table-bordered align-middle text-center small mb-0"><thead class="table-dark" id="tabelHeader"></thead><tbody id="tabelDataSantri"></tbody></table></div></div></div>
    <div id="viewInputNilai" class="dashboard-view d-none"><div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-pen-to-square me-2"></i>Form Input Penilaian</h6><form id="formPenilaian" onsubmit="simpanDataPenilaian(event)"><div class="mb-3"><label class="form-label fw-bold small">Pilih Santri</label><select class="form-select form-select-sm" id="inputSantriTarget" required></select></div><div id="dynamicFormInputs" class="row g-2"></div><button type="submit" class="btn btn-theme w-100 py-2 mt-3">Simpan Penilaian</button></form></div></div>
    <div id="viewPengaturan" class="dashboard-view d-none">
      <div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-columns me-1"></i> Kelola Kolom Penilaian</h6><div class="mb-3"><input type="text" id="newColumnName" class="form-control mb-2" placeholder="Nama Kolom"><button class="btn btn-theme w-100" onclick="tambahKolomBaru()"><i class="fa-solid fa-plus"></i> Tambah Kolom</button></div><hr><div class="mb-3"><select class="form-select mb-2" id="deleteColumnSelect"></select><button class="btn btn-danger w-100" onclick="hapusKolomPilihan()"><i class="fa-solid fa-trash"></i> Hapus Kolom</button></div></div>
      <div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-upload me-1"></i> Info Aktivitas</h6><input type="text" id="infoJudul" class="form-control mb-2" placeholder="Judul"><input type="file" id="infoFotoFile" accept="image/*" class="form-control mb-2"><textarea id="infoDeskripsi" class="form-control mb-2" rows="2" placeholder="Isi keterangan"></textarea><button class="btn btn-theme w-100" onclick="simpanAktivitasInfo()">Publikasikan</button></div>
      <div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-user-plus me-1"></i> Tambah Santri</h6><input type="text" id="newSantriName" class="form-control mb-2" placeholder="Nama Santri"><button class="btn btn-theme w-100" onclick="tambahSantriBaru()">Tambah</button></div>
      <div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-key me-1"></i> Reset Password Santri</h6><select class="form-select mb-2" id="resetSantriTarget"></select><input type="text" id="resetSantriNewPass" class="form-control mb-2" placeholder="Password Baru"><button class="btn btn-warning w-100" onclick="resetPasswordSantri()">Reset</button></div>
      <div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-image me-1"></i> Logo Aplikasi</h6><input type="file" id="inputLogoFile" accept="image/*" class="form-control mb-2"><button class="btn btn-theme w-100 mb-1" onclick="simpanLogoApp()">Simpan Logo</button><button class="btn btn-outline-secondary w-100" onclick="resetLogoApp()">Reset Default</button></div>
      <div class="card card-mobile p-3 mb-3"><h6 class="fw-bold text-theme mb-2"><i class="fa-solid fa-palette me-1"></i> Tema Warna</h6><select class="form-select mb-2" id="themeColorSelect"><option value="hijau">Hijau</option><option value="kuning">Kuning</option><option value="biru">Biru</option><option value="merah">Merah</option><option value="ungu">Ungu</option><option value="jingga">Jingga</option></select><button class="btn btn-theme w-100" onclick="simpanWarnaTema()">Terapkan</button></div>
    </div>
    <div id="viewAkun" class="dashboard-view d-none"><div class="card card-mobile p-3 mb-3 text-center"><h6 class="fw-bold text-theme mb-3"><i class="fa-solid fa-user-gear me-1"></i> Pengaturan Akun</h6><div class="mb-3 text-start"><label>Foto Profil</label><input type="file" id="inputProfilePhoto" accept="image/*" class="form-control mb-2"><button class="btn btn-theme w-100" onclick="simpanFotoProfilSantri()">Simpan Foto</button></div><div class="mb-3 text-start"><label>Ganti Password</label><input type="password" id="userNewPassInput" class="form-control mb-2" placeholder="Password Baru"><button class="btn btn-theme w-100" onclick="simpanGantiPasswordUser()">Ubah Password</button></div><hr><button class="btn btn-outline-danger w-100" onclick="logout()"><i class="fa-solid fa-right-from-bracket"></i> Keluar</button></div></div>
  </div>

  <nav class="bottom-nav d-none" id="bottomNav">
    <button class="bottom-nav-item active" onclick="switchView('viewAktivitas', this)"><i class="fa-solid fa-newspaper"></i> Info</button>
    <button class="bottom-nav-item" onclick="switchView('viewLaporan', this)"><i class="fa-solid fa-list-check"></i> Laporan</button>
    <button class="bottom-nav-item d-none" id="navInputNilai" onclick="switchView('viewInputNilai', this)"><i class="fa-solid fa-pen-to-square"></i> Nilai</button>
    <button class="bottom-nav-item d-none" id="navPengaturan" onclick="switchView('viewPengaturan', this)"><i class="fa-solid fa-gear"></i> Pengaturan</button>
    <button class="bottom-nav-item" onclick="switchView('viewAkun', this)"><i class="fa-solid fa-circle-user"></i> Akun</button>
  </nav>

  <script>
    // ============= KONFIGURASI FIREBASE - GANTI DENGAN MILIK ANDA =============
    const firebaseConfig = {
      apiKey: "API_KEY_ANDA",
      authDomain: "DOMAIN_ANDA.firebaseapp.com",
      databaseURL: "LINK_DATABASE_ANDA",
      projectId: "PROYEK_ANDA",
      storageBucket: "STORAGE_ANDA.appspot.com",
      messagingSenderId: "ID_PENGIRIM_ANDA",
      appId: "ID_APLIKASI_ANDA"
    };
    // ==========================================================================

    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.database();
    const storage = firebase.storage();

    const defaultDataSantri = {
      "Santri Awwal": ["Ahmad Arkhan Wiratama", "Aishwa Nasha Razeeta", "Al Afkar Syabani", "Ananda Aisyah Syahidal Syail", "Aqila Rafania Adifa", "Arisha Fatimah", "Asyila Rahma Khadijah", "Athaya Humaira Althafia", "Bilal Zayyan Prinoza", "Desya Salsaila", "Fatimah", "Kenzie Attaya Depa", "Khuzaimah Summayyah", "Muhammad Adzriel Rafif Fakhri", "Muhammad Alfatih Rinaldi", "Muhammad Alzaahiy Rinaldi", "Muhammad Dzaky", "Muhammad Fathian Shariq", "Muhammad Gibran Saguftha", "Muhammad Ibrahim Al Fatih Isnanto", "Muhammad Razka Destha Athallah", "Muhammad Ustman", "Qahirah Arsylia Aftarinda", "Raid Asadel", "Shaqueena Salma Aryanta", "Shofiya Azzahra Hidayatulloh", "Sultan Ibrahim Akbar", "Syafia Az Zahra", "Syahfira Destriani", "Syahrika Destriani", "Syakira Beatric Setiawan", "Tsanwa Chayra Variin", "Vesha Sakilla", "Yusuf Al Fawwaz", "Zea Mikhayla Almeera Yendra"],
      "Santri Tsani": ["Abdurrahman", "Akhtar Muhammad Rafasya", "Al Ghany Pratama", "Al Hando Pranstio", "Alfarizqi Khairan Yazid", "Anina Yumna Sakhi", "Binar Al Biru Chandra", "Chaerunnisa Fathiyaturahma", "Habiburahman El Shirazy", "Hana Shabiya Vina Pakpahan", "Keenan Ghayda Sakhi", "Keisha Chessy Tri Adiva", "Khadijah Athiyyah Samreno", "Khaif Shakiel Badillah", "Maryam Intan Dzakiyah", "Molin Sanjaya", "Muhamad Ibrahim Hidayatulloh", "Muhammad Al-Ghazello Arief", "Muhammad Hamiz Tabrani", "Muhammad Raihan Wildra", "Prisha Humairah", "Qallesha Louis Nawalla", "Risya Naifah Andami", "Rosa Adeliya", "Salsabila Putri Ayoenie Alfarizi", "Shaffiyah Mecca Al Fatih Isnanto", "Syifa Nursabrina Robka", "Syifa Oktaviani", "Zaim Faqih Alrasyid", "Ziyadah Khaira Pakpahan"],
      "Santri Tsalits": ["Santri Tsalits 1", "Santri Tsalits 2"],
      "Santri Robi": ["Santri Robi 1", "Santri Robi 2"]
    };
    const defaultColumns = ["Iqro - Capaian", "Iqro - Catatan", "Hafalan Surat - Murajaah", "Hafalan Surat - Ziyadah", "Hafalan Surat - Catatan", "Hafalan Lainnya - Hadits", "Hafalan Lainnya - Matan", "Hafalan Lainnya - Doa", "Catatan Akhlak", "Kehadiran (%)"];
    const themePresets = { "hijau": { bg: "#eef7ed", main: "#157347", dark: "#0d512f" }, "kuning": { bg: "#fffdf0", main: "#d4a017", dark: "#997300" }, "biru": { bg: "#edf4fc", main: "#0d6efd", dark: "#0a58ca" }, "merah": { bg: "#fceded", main: "#dc3545", dark: "#b02a37" }, "ungu": { bg: "#f5edf7", main: "#6f42c1", dark: "#593196" }, "jingga": { bg: "#fef3eb", main: "#fd7e14", dark: "#ca6510" } };

    let currentRoleCategory = "", currentClassKey = "", currentUserName = "", dbListeners = [];

    // ============= FUNGSI BANTUAN =============
    function tampilLoading(teks="Memproses..."){ document.getElementById('loadingScreen').style.display='flex'; document.querySelector('#loadingScreen span').innerText=teks; }
    function sembunyikanLoading(){ document.getElementById('loadingScreen').style.display='none'; }
    function tampilPesan(teks, tipe="sukses"){
      const c=document.getElementById('toastContainer');
      const ikon = tipe=="sukses"?"fa-circle-check":tipe=="error"?"fa-circle-xmark":"fa-circle-info";
      const warna = tipe=="sukses"?"bg-success":tipe=="error"?"bg-danger":"bg-primary";
      c.innerHTML=`<div class="toast align-items-center text-white ${warna} show"><div class="d-flex"><div class="toast-body"><i class="fa-solid ${ikon} me-2"></i>${teks}</div><button class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast"></button></div></div>`;
      setTimeout(()=>c.innerHTML="",3000);
    }

    // Cek Koneksi
    window.addEventListener('online',()=>{document.getElementById('offlineBar').style.display='none'; tampilPesan("Koneksi pulih");});
    window.addEventListener('offline',()=>{document.getElementById('offlineBar').style.display='block'; tampilPesan("Koneksi terputus, data akan disinkron nanti", "error");});

    // Inisialisasi Data Pertama Kali
    async function initDataAwal(){
      const cekSantri = await db.ref('data_santri').once('value');
      if(!cekSantri.exists()) await db.ref('data_santri').set(defaultDataSantri);
      const cekKolom = await db.ref('pengaturan/kolom').once('value');
      if(!cekKolom.exists()) await db.ref('pengaturan/kolom').set(defaultColumns);
      const cekPass = await db.ref('password_pengajar').once('value');
      if(!cekPass.exists()) await db.ref('password_pengajar').set({"Pengajar Awwal":"darul123","Pengajar Tsani":"darul123","Pengajar Tsalits":"darul123","Pengajar Robi":"darul123"});
    }

    // Pantau Perubahan Realtime
    function pantauData(){
      dbListeners.forEach(l=>l.off()); dbListeners=[];
      dbListeners.push(db.ref('pengaturan/kolom').on('value',s=>{renderFormInputsPenilaian(); renderDropdownHapusKolom(); renderTabelPenilaian();}));
      dbListeners.push(db.ref('laporan_nilai').on('value',s=>renderTabelPenilaian()));
      dbListeners.push(db.ref('info_aktivitas').limitToLast(20).on('value',s=>renderAktivitasInfo()));
      db.ref('pengaturan/aplikasi').once('value').then(s=>{const d=s.val(); if(d?.tema) terapkanWarnaTema(d.tema); if(d?.logo){document.getElementById('customLogo').src=d.logo; document.getElementById('customLogo').classList.remove('d-none'); document.getElementById('defaultLogo').classList.add('d-none');}});
    }

    // ============= SISTEM LOGIN =============
    auth.onAuthStateChanged(async (user)=>{
      sembunyikanLoading();
      if(user){
        const dataUser = (await db.ref(`pengguna/${user.uid}`).once('value')).val();
        if(!dataUser) return logout();
        currentRoleCategory=dataUser.peran; currentClassKey=dataUser.kelas; currentUserName=dataUser.nama;
        bukaDashboard();
      }else{
        document.getElementById('loginPage').classList.remove('d-none');
        toggleLoginInputs();
      }
    });

    async function handleLogin(){
      if(!navigator.onLine) return tampilPesan("Butuh internet untuk login", "error");
      tampilLoading("Memverifikasi...");
      const t=document.getElementById('loginType').value;
      const p=document.getElementById('loginPassword').value;
      try{
        let email;
        if(t.startsWith('Pengajar')){
          const passDb=(await db.ref('password_pengajar').once('value')).val();
          if(p!==passDb[t]) throw new Error("Password salah");
          email=`${t.replace(/\s/g,'_')}@darularqam.app`;
        }else{
          const n=document.getElementById('loginSantriName').value;
          if(!n) throw new Error("Pilih nama santri");
          const dbS=(await db.ref(`data_santri/${t}`).once('value')).val();
          const s=dbS.find(x=>x.nama===n);
          if(!s||s.pass!==p) throw new Error("Password salah");
          email=`${t.replace(/\s/g,'_')}_${n.replace(/\s/g,'_')}@darularqam.app`;
        }
        await auth.signInWithEmailAndPassword(email,p).catch(()=>auth.createUserWithEmailAndPassword(email,p));
        const uid=auth.currentUser.uid;
        if(!(await db.ref(`pengguna/${uid}`).once('value')).exists()){
          await db.ref(`pengguna/${uid}`).set({peran:t.startsWith('Pengajar')?'Pengajar':'Santri', kelas:t.replace(/(Santri |Pengajar )/g,''), nama:t.startsWith('Pengajar')?`Pengajar Kelas ${t.replace('Pengajar ','')}`:document.getElementById('loginSantriName').value});
        }
        tampilPesan("Berhasil masuk");
      }catch(e){ tampilPesan(e.message,"error"); }finally{ sembunyikanLoading(); }
    }

    function logout(){ dbListeners.forEach(l=>l.off()); auth.signOut(); document.getElementById('dashboardPage').classList.add('d-none'); document.getElementById('bottomNav').classList.add('d-none'); document.getElementById('loginPassword').value=''; }

    // ============= FUNGSI UTAMA & FITUR (SESUAI VERSI ASLI) =============
    async function bukaDashboard(){
      document.getElementById('loginPage').classList.add('d-none'); document.getElementById('dashboardPage').classList.remove('d-none'); document.getElementById('bottomNav').classList.remove('d-none');
      document.getElementById('userRoleTitle').innerText=currentRoleCategory==='Pengajar'?`Pengajar Kelas ${currentClassKey}`:currentUserName;
      document.getElementById('userRoleSubtitle').innerText=`Mustawa ${currentClassKey}`;
      updateHeaderAvatar();
      document.getElementById('navInputNilai').classList.toggle('d-none',currentRoleCategory!=='Pengajar');
      document.getElementById('navPengaturan').classList.toggle('d-none',currentRoleCategory!=='Pengajar');
      await initDataAwal(); pantauData();
      if(currentRoleCategory==='Pengajar') loadDropdownSantriPengajar();
      renderFormInputsPenilaian(); renderDropdownHapusKolom(); switchView('viewAktivitas',document.querySelector('.bottom-nav-item'));
    }

    function toggleLoginInputs(){/*sama seperti versi asli, ambil data dari db.ref bukan localStorage*/ document.getElementById('loginSantriName').innerHTML=''; const t=document.getElementById('loginType').value; if(t.startsWith('Santri')){ document.getElementById('santriSelectGroup').classList.remove('d-none'); db.ref(`data_santri/${t}`).once('value').then(s=>{s.val().forEach(x=>{const o=document.createElement('option');o.value=x.nama;o.innerText=x.nama;document.getElementById('loginSantriName').appendChild(o);});}); }else{document.getElementById('santriSelectGroup').classList.add('d-none');} }
    function updateHeaderAvatar(){/*load foto dari db*/}
    function switchView(v,b){document.querySelectorAll('.dashboard-view').forEach(x=>x.classList.add('d-none'));document.getElementById(v).classList.remove('d-none');document.querySelectorAll('.bottom-nav-item').forEach(x=>x.classList.remove('active'));if(b)b.classList.add('active');if(v==='viewLaporan')renderTabelPenilaian();}
    async function renderFormInputsPenilaian(){const c=document.getElementById('dynamicFormInputs');const k=(await db.ref('pengaturan/kolom').once('value')).val()||defaultColumns;c.innerHTML='';k.forEach((x,i)=>c.innerHTML+=`<div class="col-6 mb-2"><label class="form-label small text-muted mb-1">${x}</label><input type="text" id="col_${i}" class="form-control form-control-sm"></div>`);}
    async function renderDropdownHapusKolom(){const s=document.getElementById('deleteColumnSelect');const k=(await db.ref('pengaturan/kolom').once('value')).val()||[];s.innerHTML='<option value="">-- Pilih --</option>';k.forEach(x=>s.innerHTML+=`<option value="${x}">${x}</option>`);}
    async function tambahKolomBaru(){const n=document.getElementById('newColumnName').value.trim();if(!n)return tampilPesan('Isi nama kolom','error');let k=(await db.ref('pengaturan/kolom').once('value')).val()||[];if(k.includes(n))return tampilPesan('Sudah ada','error');k.push(n);await db.ref('pengaturan/kolom').set(k);document.getElementById('newColumnName').value='';tampilPesan('Kolom ditambahkan');}
    async function hapusKolomPilihan(){const v=document.getElementById('deleteColumnSelect').value;if(!v)return;if(!confirm('Yakin hapus?'))return;let k=(await db.ref('pengaturan/kolom').once('value')).val();k=k.filter(x=>x!==v);await db.ref('pengaturan/kolom').set(k);tampilPesan('Kolom dihapus');}
    async function simpanDataPenilaian(e){e.preventDefault();const n=document.getElementById('inputSantriTarget').value;if(!n)return tampilPesan('Pilih santri','error');const k=(await db.ref('pengaturan/kolom').once('value')).val()||[];const d={};k.forEach((x,i)=>d[x]=document.getElementById(`col_${i}`).value||'-');const lap=(await db.ref('laporan_nilai').once('value')).val()||[];const idx=lap.findIndex(x=>x.kelas===currentClassKey&&x.nama===n);if(idx>=0)lap[idx].data=d;else lap.push({kelas:currentClassKey,nama:n,data:d});await db.ref('laporan_nilai').set(lap);tampilPesan('Tersimpan');document.getElementById('formPenilaian').reset();}
    async function renderTabelPenilaian(){const h=document.getElementById('tabelHeader'),b=document.getElementById('tabelDataSantri');const k=(await db.ref('pengaturan/kolom').once('value')).val()||[];h.innerHTML=`<tr><th>No</th><th>Nama</th>${k.map(x=>`<th>${x}</th>`).join('')}${currentRoleCategory==='Pengajar'?'<th>Aksi</th>':''}</tr>`;const l=(await db.ref('laporan_nilai').once('value')).val()||[];let f=l.filter(x=>x.kelas===currentClassKey);if(currentRoleCategory==='Santri')f=f.filter(x=>x.nama===currentUserName);b.innerHTML='';if(!f.length){b.innerHTML=`<tr><td colspan="${k.length+2}" class="py-4 text-muted">Belum ada data</td></tr>`;return;}f.forEach((x,i)=>{b.innerHTML+=`<tr><td>${i+1}</td><td class="fw-bold text-start">${x.nama}</td>${k.map(c=>`<td>${x.data[c]||'-'}</td>`).join('')}${currentRoleCategory==='Pengajar'?`<td><button class="btn btn-danger btn-sm" onclick="hapusNilai('${x.nama}')"><i class="fa-solid fa-trash"></i></button></td>`:''}</tr>`;});}
    async function hapusNilai(nama){if(!confirm('Hapus data ini?'))return;let l=(await db.ref('laporan_nilai').once('value')).val()||[];l=l.filter(x=>!(x.kelas===currentClassKey&&x.nama===nama));await db.ref('laporan_nilai').set(l);}
    async function loadDropdownSantriPengajar(){const k=`Santri ${currentClassKey}`;const s=(await db.ref(`data_santri/${k}`).once('value')).val()||[];const t1=document.getElementById('inputSantriTarget'),t2=document.getElementById('resetSantriTarget');t1.innerHTML='<option value="">-- Pilih --</option>';t2.innerHTML='<option value="">-- Pilih --</option>';s.forEach(x=>{t1.innerHTML+=`<option value="${x.nama}">${x.nama}</option>`;t2.innerHTML+=`<option value="${x.nama}">${x.nama}</option>`;});}
    async function simpanAktivitasInfo(){const j=document.getElementById('infoJudul').value,d=document.getElementById('infoDeskripsi').value,f=document.getElementById('infoFotoFile').files[0];if(!j||!d)return tampilPesan('Lengkapi isian','error'); const simpan=(url='')=>{db.ref('info_aktivitas').push({judul:j,deskripsi:d,foto:url,tanggal:new Date().toLocaleDateString('id-ID'),oleh:currentUserName});tampilPesan('Info dipublikasikan');document.getElementById('infoJudul').value='';document.getElementById('infoDeskripsi').value='';document.getElementById('infoFotoFile').value='';}; if(f){const r=new FileReader();r.onload=e=>simpan(e.target.result);r.readAsDataURL(f);}else simpan();}
    async function renderAktivitasInfo(){const c=document.getElementById('containerAktivitas');const d=(await db.ref('info_aktivitas').orderByKey().limitToLast(15).once('value')).val()||{};c.innerHTML='';if(!Object.keys(d).length){c.innerHTML='<div class="text-center text-muted py-3">Belum ada info</div>';return;}Object.entries(d).reverse().forEach(([k,v])=>{c.innerHTML+=`<div class="border-bottom pb-3 mb-3"><h6 class="fw-bold text-theme mb-1">${v.judul}</h6><small class="text-muted d-block mb-2">${v.tanggal}</small>${v.foto?`<img src="${v.foto}" class="preview-img mb-2">`:''}<p class="small mb-0">${v.deskripsi}</p>${currentRoleCategory==='Pengajar'?`<button class="btn btn-danger btn-sm mt-2" onclick="hapusInfo('${k}')"><i class="fa-solid fa-trash"></i> Hapus</button>`:''}</div>`;});}
    async function hapusInfo(key){if(!confirm('Hapus info ini?'))return;await db.ref(`info_aktivitas/${key}`).remove();}
    async function tambahSantriBaru(){const n=document.getElementById('newSantriName').value.trim();if(!n)return;const k=`Santri ${currentClassKey}`;let s=(await db.ref(`data_santri/${k}`).once('value')).val();s.push({nama:n,pass:n,foto:''});await db.ref(`data_santri/${k}`).set(s);document.getElementById('newSantriName').value='';tampilPesan('Santri ditambahkan');loadDropdownSantriPengajar();}
    async function resetPasswordSantri(){const n=document.getElementById('resetSantriTarget').value,p=document.getElementById('resetSantriNewPass').value.trim();if(!n||!p)return;const k=`Santri ${currentClassKey}`;let s=(await db.ref(`data_santri/${k}`).once('value')).val();const idx=s.findIndex(x=>x.nama===n);s[idx].pass=p;await db.ref(`data_santri/${k}`).set(s);document.getElementById('resetSantriNewPass').value='';tampilPesan('Password diubah');}
    async function simpanLogoApp(){const f=document.getElementById('inputLogoFile').files[0];if(!f)return tampilPesan('Pilih gambar','error');const r=new FileReader();r.onload=async e=>{await db.ref('pengaturan/aplikasi/logo').set(e.target.result);document.getElementById('customLogo').src=e.target.result;document.getElementById('customLogo').classList.remove('d-none');document.getElementById('defaultLogo').classList.add('d-none');tampilPesan('Logo diperbarui');};r.readAsDataURL(f);}
    async function resetLogoApp(){await db.ref('pengaturan/aplikasi/logo').remove();document.getElementById('customLogo').classList.add('d-none');document.getElementById('defaultLogo').classList.remove('d-none');tampilPesan('Logo dikembalikan');}
    async function simpanWarnaTema(){const v=document.getElementById('themeColorSelect').value;await db.ref('pengaturan/aplikasi/tema').set(v);terapkanWarnaTema(v);tampilPesan('Tema diterapkan');}
    function terapkanWarnaTema(k){const t=themePresets[k]||themePresets.hijau;document.documentElement.style.setProperty('--bg-color',t.bg);document.documentElement.style.setProperty('--main-color',t.main);document.documentElement.style.setProperty('--dark-color',t.dark);}
    async function simpanFotoProfilSantri(){if(currentRoleCategory!=='Santri')return;const f=document.getElementById('inputProfilePhoto').files[0];if(!f)return;const r=new FileReader();r.onload=async e=>{const k=`Santri ${currentClassKey}`;let s=(await db.ref(`data_santri/${k}`).once('value')).val();const idx=s.findIndex(x=>x.nama===currentUserName);s[idx].foto=e.target.result;await db.ref(`data_santri/${k}`).set(s);updateHeaderAvatar();tampilPesan('Foto tersimpan');};r.readAsDataURL(f);}
    async function simpanGantiPasswordUser(){const p=document.getElementById('userNewPassInput').value.trim();if(!p)return;await auth.currentUser.updatePassword(p);if(currentRoleCategory==='Pengajar'){const k=`Pengajar ${currentClassKey}`;let pw=(await db.ref('password_pengajar').once('value')).val();pw[k]=p;await db.ref('password_pengajar').set(pw);}else{const k=`Santri ${currentClassKey}`;let s=(await db.ref(`data_santri/${k}`).once('value')).val();const idx=s.findIndex(x=>x.nama===currentUserName);s[idx].pass=p;await db.ref(`data_santri/${k}`).set(s);}document.getElementById('userNewPassInput').value='';tampilPesan('Password berhasil diubah');}
  </script>
</body>
</html>
