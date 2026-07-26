<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Aplikasi Realtime Multi-User</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <style>
    body { background-color: #f8f9fa; font-family: sans-serif; }
    .card-custom { border-radius: 12px; border: none; box-shadow: 0 4px 12px rgba(0,0,0,0.05); }
    .status-badge { font-size: 0.8rem; padding: 6px 12px; border-radius: 20px; }
    .status-online { background-color: #d1e7dd; color: #0f5132; }
    .status-offline { background-color: #f8d7da; color: #842029; }
    .item-card { transition: all 0.2s ease-in-out; }
    .item-card:hover { transform: translateY(-2px); }
  </style>
</head>
<body>

  <!-- Status Bar / Header Navbar -->
  <nav class="navbar navbar-light bg-white border-bottom shadow-sm px-3">
    <span class="navbar-brand mb-0 h1 fw-bold text-primary"><i class="fa-solid fa-bolt me-2"></i>Realtime App</span>
    
    <div class="d-flex align-items-center gap-2">
      <!-- Status Indikator Jaringan & Reconnect -->
      <span id="networkStatus" class="status-badge status-online">
        <i class="fa-solid fa-wifi me-1"></i> Terhubung
      </span>
      <button id="btnLogout" class="btn btn-outline-danger btn-sm d-none" onclick="handleLogout()">
        <i class="fa-solid fa-right-from-bracket"></i> Keluar
      </button>
    </div>
  </nav>

  <div class="container py-4" style="max-width: 600px;">

    <!-- Alert Notifikasi Error/Info Global -->
    <div id="alertBox" class="alert alert-danger alert-dismissible d-none" role="alert">
      <span id="alertMessage"></span>
      <button type="button" class="btn-close" onclick="hideAlert()"></button>
    </div>

    <!-- Loading Indicator -->
    <div id="loadingSpinner" class="text-center py-5">
      <div class="spinner-border text-primary" role="status" style="width: 3rem; height: 3rem;">
        <span class="visually-hidden">Memuat...</span>
      </div>
      <p class="text-muted mt-2 small">Menghubungkan ke Cloud Database...</p>
    </div>

    <!-- ================= 1. FORM AUTHENTICATION (LOGIN / REGISTER) ================= -->
    <div id="authSection" class="card card-custom p-4 d-none">
      <h4 class="fw-bold text-center mb-1" id="authTitle">Masuk ke Akun</h4>
      <p class="text-center text-muted small mb-4">Akses data kolaboratif secara realtime</p>

      <form id="authForm" onsubmit="handleAuth(event)">
        <div class="mb-3">
          <label class="form-label small fw-bold">Alamat Email</label>
          <input type="email" id="authEmail" class="form-control" placeholder="nama@email.com" required>
        </div>
        <div class="mb-3">
          <label class="form-label small fw-bold">Kata Sandi</label>
          <input type="password" id="authPassword" class="form-control" placeholder="••••••••" required minlength="6">
        </div>

        <button type="submit" id="btnAuthSubmit" class="btn btn-primary w-100 py-2 fw-bold mb-3">
          Masuk
        </button>
      </form>

      <div class="text-center">
        <button class="btn btn-link btn-sm text-decoration-none" id="toggleAuthBtn" onclick="toggleAuthMode()">
          Belum punya akun? Daftar sekarang
        </button>
      </div>
    </div>

    <!-- ================= 2. DASHBOARD REALTIME DATA ================= -->
    <div id="dashboardSection" class="d-none">
      
      <!-- Welcome Info -->
      <div class="card card-custom p-3 mb-3 bg-primary text-white">
        <div class="d-flex align-items-center justify-content-between">
          <div>
            <small class="d-block text-white-50">Pengguna Aktif:</small>
            <strong id="userDisplayEmail">-</strong>
          </div>
          <i class="fa-solid fa-users-viewfinder fs-2 opacity-50"></i>
        </div>
      </div>

      <!-- Input Data Baru -->
      <div class="card card-custom p-3 mb-4">
        <h6 class="fw-bold mb-2"><i class="fa-solid fa-plus me-1 text-primary"></i> Tambah Catatan / Data Baru</h6>
        <form onsubmit="handleSendData(event)" class="d-flex gap-2">
          <input type="text" id="inputDataText" class="form-control" placeholder="Tuliskan sesuatu..." required>
          <button type="submit" id="btnSend" class="btn btn-primary px-3">
            <i class="fa-solid fa-paper-plane"></i>
          </button>
        </form>
      </div>

      <!-- List Data Realtime -->
      <div class="d-flex align-items-center justify-content-between mb-2">
        <h6 class="fw-bold text-secondary mb-0"><i class="fa-solid fa-arrows-rotate me-1"></i> Data Stream Live</h6>
        <span class="badge bg-secondary" id="itemCount">0 Item</span>
      </div>

      <div id="dataList" class="d-flex flex-column gap-2">
        <!-- Item data akan dirender secara otomatis lewat Firestore snapshot listener -->
      </div>

    </div>

  </div>

  <!-- Firebase Web SDK (v10.12.0) Modular CDN -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
    import { 
      getAuth, 
      signInWithEmailAndPassword, 
      createUserWithEmailAndPassword, 
      signOut, 
      onAuthStateChanged 
    } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
    import { 
      getFirestore, 
      collection, 
      addDoc, 
      deleteDoc, 
      doc, 
      onSnapshot, 
      query, 
      orderBy, 
      serverTimestamp,
      enableIndexedDbPersistence 
    } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

    // ------------------------------------------------------------------
    // 1. KONFIGURASI FIREBASE
    // Ganti nilai properti di bawah ini dengan konfigurasi Firebase Project Anda!
    // ------------------------------------------------------------------
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    // Inisialisasi SDK
    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getFirestore(app);

    // OPTIMASI BANDWIDTH & SPEED: Aktifkan Offline Persistence (Cache lokal)
    enableIndexedDbPersistence(db).catch((err) => {
      if (err.code === 'failed-precondition') {
        console.warn('Persistence gagal: Banyak tab terbuka sekaligus.');
      } else if (err.code === 'unimplemented') {
        console.warn('Browser tidak mendukung offline persistence.');
      }
    });

    // Variable State App
    let isLoginMode = true;
    let unsubscribeStream = null;

    // ------------------------------------------------------------------
    // 2. SISTEM RECONNECT OTOMATIS & CEK JARINGAN INTERNET
    // ------------------------------------------------------------------
    const networkStatusEl = document.getElementById('networkStatus');

    function updateNetworkStatus() {
      if (navigator.onLine) {
        networkStatusEl.className = "status-badge status-online";
        networkStatusEl.innerHTML = '<i class="fa-solid fa-wifi me-1"></i> Terhubung';
      } else {
        networkStatusEl.className = "status-badge status-offline";
        networkStatusEl.innerHTML = '<i class="fa-solid fa-wifi-slash me-1"></i> Terputus (Mode Offline)';
        showAlert("Koneksi internet terputus. Perubahan akan disimpan lokal dan disinkronkan saat terhubung kembali.", "warning");
      }
    }

    window.addEventListener('online', () => {
      updateNetworkStatus();
      hideAlert();
    });
    window.addEventListener('offline', updateNetworkStatus);

    // ------------------------------------------------------------------
    // 3. LISTEN OTENTIKASI USER (AUTH LISTENER)
    // ------------------------------------------------------------------
    onAuthStateChanged(auth, (user) => {
      hideLoading();
      if (user) {
        // User Terautentikasi
        document.getElementById('authSection').classList.add('d-none');
        document.getElementById('dashboardSection').classList.remove('d-none');
        document.getElementById('btnLogout').classList.remove('d-none');
        document.getElementById('userDisplayEmail').innerText = user.email;
        
        // Jalankan Listener Realtime Cloud Database
        listenToRealtimeData();
      } else {
        // User Keluar / Belum Login
        document.getElementById('authSection').classList.remove('d-none');
        document.getElementById('dashboardSection').classList.add('d-none');
        document.getElementById('btnLogout').classList.add('d-none');
        
        // Stop stream listener hemat bandwidth
        if (unsubscribeStream) unsubscribeStream();
      }
    });

    // ------------------------------------------------------------------
    // 4. ESEKUSI AUTHENTICATION (LOGIN & REGISTER)
    // ------------------------------------------------------------------
    window.handleAuth = async (e) => {
      e.preventDefault();
      showLoading();
      hideAlert();

      const email = document.getElementById('authEmail').value;
      const password = document.getElementById('authPassword').value;

      try {
        if (isLoginMode) {
          await signInWithEmailAndPassword(auth, email, password);
        } else {
          await createUserWithEmailAndPassword(auth, email, password);
        }
      } catch (error) {
        hideLoading();
        handleFirebaseError(error);
      }
    };

    window.toggleAuthMode = () => {
      isLoginMode = !isLoginMode;
      document.getElementById('authTitle').innerText = isLoginMode ? "Masuk ke Akun" : "Daftar Akun Baru";
      document.getElementById('btnAuthSubmit').innerText = isLoginMode ? "Masuk" : "Daftar";
      document.getElementById('toggleAuthBtn').innerText = isLoginMode 
        ? "Belum punya akun? Daftar sekarang" 
        : "Sudah punya akun? Masuk di sini";
    };

    window.handleLogout = () => {
      if (confirm("Yakin ingin keluar dari akun ini?")) {
        signOut(auth);
      }
    };

    // ------------------------------------------------------------------
    // 5. STREAMING DATA REALTIME CLOUD DATABASE (FIRESTORE)
    // ------------------------------------------------------------------
    function listenToRealtimeData() {
      // Query dengan urutan waktu (Mengoptimalkan pembacaan & urutan)
      const q = query(collection(db, "shared_notes"), orderBy("createdAt", "desc"));

      // ON SNAPSHOT: Mendengarkan perubahan data secara langsung di semua perangkat
      unsubscribeStream = onSnapshot(q, (snapshot) => {
        const dataListEl = document.getElementById('dataList');
        document.getElementById('itemCount').innerText = `${snapshot.docs.length} Item`;
        
        if (snapshot.empty) {
          dataListEl.innerHTML = `
            <div class="text-center py-4 text-muted card card-custom">
              <i class="fa-solid fa-inbox fs-2 mb-2"></i>
              <p class="mb-0 small">Belum ada data. Tambahkan data pertama Anda!</p>
            </div>`;
          return;
        }

        let html = '';
        snapshot.forEach((doc) => {
          const item = doc.data();
          const id = doc.id;
          const isMyItem = auth.currentUser && auth.currentUser.email === item.createdBy;

          html += `
            <div class="card card-custom item-card p-3 d-flex flex-row align-items-center justify-content-between">
              <div>
                <p class="mb-0 fw-bold text-dark">${escapeHtml(item.text)}</p>
                <small class="text-muted" style="font-size:0.75rem;">
                  <i class="fa-regular fa-user me-1"></i>${item.createdBy || 'Anonim'}
                </small>
              </div>
              <div>
                <button class="btn btn-outline-danger btn-sm border-0" onclick="handleDeleteData('${id}')" title="Hapus Data">
                  <i class="fa-solid fa-trash"></i>
                </button>
              </div>
            </div>
          `;
        });

        dataListEl.innerHTML = html;
      }, (error) => {
        handleFirebaseError(error);
      });
    }

    // ------------------------------------------------------------------
    // 6. MANAJEMEN DATA (KIRIM & HAPUS)
    // ------------------------------------------------------------------
    window.handleSendData = async (e) => {
      e.preventDefault();
      const inputEl = document.getElementById('inputDataText');
      const text = inputEl.value.trim();

      if (!text) return;

      try {
        inputEl.value = ''; // Instant feedback UI
        // Tambah ke Firestore Cloud Database
        await addDoc(collection(db, "shared_notes"), {
          text: text,
          createdBy: auth.currentUser.email,
          createdAt: serverTimestamp()
        });
      } catch (error) {
        handleFirebaseError(error);
      }
    };

    window.handleDeleteData = async (id) => {
      try {
        await deleteDoc(doc(db, "shared_notes", id));
      } catch (error) {
        handleFirebaseError(error);
      }
    };

    // ------------------------------------------------------------------
    // 7. HELPER, ERROR HANDLING & UI STATE
    // ------------------------------------------------------------------
    function showLoading() { document.getElementById('loadingSpinner').classList.remove('d-none'); }
    function hideLoading() { document.getElementById('loadingSpinner').classList.add('d-none'); }

    function showAlert(msg, type = "danger") {
      const alertBox = document.getElementById('alertBox');
      const alertMessage = document.getElementById('alertMessage');
      alertBox.className = `alert alert-${type} alert-dismissible`;
      alertMessage.innerText = msg;
      alertBox.classList.remove('d-none');
    }
    window.hideAlert = () => { document.getElementById('alertBox').classList.add('d-none'); };

    function handleFirebaseError(err) {
      console.error(err);
      let msg = err.message;
      if (err.code === 'auth/invalid-credential') msg = "Email atau kata sandi salah.";
      if (err.code === 'auth/email-already-in-use') msg = "Email ini sudah terdaftar.";
      if (err.code === 'auth/weak-password') msg = "Kata sandi terlalu lemah (minimal 6 karakter).";
      if (err.code === 'permission-denied') msg = "Anda tidak memiliki izin untuk melakukan aksi ini.";
      
      showAlert(msg, "danger");
    }

    function escapeHtml(text) {
      return text.replace(/[&<>"']/g, (m) => ({
        '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#039;'
      })[m]);
    }
  </script>
</body>
</html>
