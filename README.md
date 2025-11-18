<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <style>
      body{font-family: Arial; padding:10px;}
      input, select, button {padding:6px; margin:4px 0; width:100%;}
      .row{display:flex; gap:8px;}
      .col{flex:1;}
      table{width:100%; border-collapse:collapse; margin-top:12px;}
      th,td{border:1px solid #ddd; padding:6px; font-size:12px;}
      th{background:#f4f4f4;}
      .summary{margin-top:8px; padding:8px; background:#f9f9f9;}
    </style>
  </head>
  <body>
    <h3>Aplikasi Kas (Simpan ke Google Sheets)</h3>

    <div>
      <label>Tanggal</label>
      <input type="date" id="tanggal">
      <label>Deskripsi</label>
      <input type="text" id="deskripsi" placeholder="Contoh: Setoran kas, Gaji">
      <label>Kategori</label>
      <input type="text" id="kategori" placeholder="Opsional: Operasional, Gaji, dll.">
      <div class="row">
        <div class="col">
          <label>Tipe</label>
          <select id="tipe">
            <option value="Pemasukan">Pemasukan</option>
            <option value="Pengeluaran">Pengeluaran</option>
          </select>
        </div>
        <div class="col">
          <label>Jumlah (angka saja)</label>
          <input type="number" id="jumlah" step="0.01" placeholder="0">
        </div>
      </div>
      <button id="simpanBtn">Simpan Transaksi</button>
    </div>

    <div class="summary">
      <strong>Ringkasan:</strong>
      <div>Total Pemasukan: <span id="totalIn">0</span></div>
      <div>Total Pengeluaran: <span id="totalOut">0</span></div>
      <div>Saldo Akhir: <span id="saldoAkhir">0</span></div>
    </div>

    <h4>Riwayat Transaksi</h4>
    <table id="tbl">
      <thead>
        <tr><th>#</th><th>Tanggal</th><th>Deskripsi</th><th>Kategori</th><th>Tipe</th><th>Jumlah</th><th>Saldo</th></tr>
      </thead>
      <tbody id="bodyTbl"></tbody>
    </table>

    <script>
      // Inisialisasi: ambil data dan ringkasan
      function refreshAll(){
        google.script.run.withSuccessHandler(renderTransactions).getTransactions();
        google.script.run.withSuccessHandler(renderSummary).getSummary();
      }

      function renderTransactions(rows){
        var tbody = document.getElementById('bodyTbl');
        tbody.innerHTML = '';
        if (!rows || rows.length===0){ tbody.innerHTML = '<tr><td colspan="7" style="text-align:center">Belum ada transaksi</td></tr>'; return; }
        for (var i=0;i<rows.length;i++){
          var r = rows[i];
          var tr = document.createElement('tr');
          tr.innerHTML = '<td>'+(i+1)+'</td>' +
                         '<td>'+(r.tanggal||'')+'</td>' +
                         '<td>'+(r.deskripsi||'')+'</td>' +
                         '<td>'+(r.kategori||'')+'</td>' +
                         '<td>'+(r.tipe||'')+'</td>' +
                         '<td style="text-align:right">'+(r.jumlah||0)+'</td>' +
                         '<td style="text-align:right">'+(r.saldo||0)+'</td>';
          tbody.appendChild(tr);
        }
      }

      function renderSummary(s){
        document.getElementById('totalIn').innerText = (s.totalPemasukan || 0);
        document.getElementById('totalOut').innerText = (s.totalPengeluaran || 0);
        document.getElementById('saldoAkhir').innerText = (s.saldoAkhir || 0);
      }

      document.getElementById('simpanBtn').addEventListener('click', function(){
        var obj = {
          tanggal: document.getElementById('tanggal').value,
          deskripsi: document.getElementById('deskripsi').value,
          kategori: document.getElementById('kategori').value,
          tipe: document.getElementById('tipe').value,
          jumlah: document.getElementById('jumlah').value
        };
        if (!obj.jumlah || Number(obj.jumlah) <= 0){
          alert('Masukkan jumlah transaksi yang valid (>0).'); return;
        }
        google.script.run.withSuccessHandler(function(res){
          if (res && res.success){
            alert('Transaksi tersimpan. Saldo sekarang: ' + res.saldo);
            // kosongkan input
            document.getElementById('deskripsi').value = '';
            document.getElementById('kategori').value = '';
            document.getElementById('jumlah').value = '';
            refreshAll();
          } else {
            alert('Gagal menyimpan transaksi.');
          }
        }).addTransaction(obj);
      });

      // Jalankan saat load
      refreshAll();
    </script>
  </body>
  <!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <style>
      body{font-family: Arial; padding:10px;}
      input, select, button {padding:6px; margin:4px 0; width:100%;}
      .row{display:flex; gap:8px;}
      .col{flex:1;}
      table{width:100%; border-collapse:collapse; margin-top:12px;}
      th,td{border:1px solid #ddd; padding:6px; font-size:12px;}
      th{background:#f4f4f4;}
      .summary{margin-top:8px; padding:8px; background:#f9f9f9;}
    </style>
  </head>
  <body>
    <h3>Aplikasi Kas (Simpan ke Google Sheets)</h3>

    <div>
      <label>Tanggal</label>
      <input type="date" id="tanggal">
      <label>Deskripsi</label>
      <input type="text" id="deskripsi" placeholder="Contoh: Setoran kas, Gaji">
      <label>Kategori</label>
      <input type="text" id="kategori" placeholder="Opsional: Operasional, Gaji, dll.">
      <div class="row">
        <div class="col">
          <label>Tipe</label>
          <select id="tipe">
            <option value="Pemasukan">Pemasukan</option>
            <option value="Pengeluaran">Pengeluaran</option>
          </select>
        </div>
        <div class="col">
          <label>Jumlah (angka saja)</label>
          <input type="number" id="jumlah" step="0.01" placeholder="0">
        </div>
      </div>
      <button id="simpanBtn">Simpan Transaksi</button>
    </div>

    <div class="summary">
      <strong>Ringkasan:</strong>
      <div>Total Pemasukan: <span id="totalIn">0</span></div>
      <div>Total Pengeluaran: <span id="totalOut">0</span></div>
      <div>Saldo Akhir: <span id="saldoAkhir">0</span></div>
    </div>

    <h4>Riwayat Transaksi</h4>
    <table id="tbl">
      <thead>
        <tr><th>#</th><th>Tanggal</th><th>Deskripsi</th><th>Kategori</th><th>Tipe</th><th>Jumlah</th><th>Saldo</th></tr>
      </thead>
      <tbody id="bodyTbl"></tbody>
    </table>

    <script>
      // Inisialisasi: ambil data dan ringkasan
      function refreshAll(){
        google.script.run.withSuccessHandler(renderTransactions).getTransactions();
        google.script.run.withSuccessHandler(renderSummary).getSummary();
      }

      function renderTransactions(rows){
        var tbody = document.getElementById('bodyTbl');
        tbody.innerHTML = '';
        if (!rows || rows.length===0){ tbody.innerHTML = '<tr><td colspan="7" style="text-align:center">Belum ada transaksi</td></tr>'; return; }
        for (var i=0;i<rows.length;i++){
          var r = rows[i];
          var tr = document.createElement('tr');
          tr.innerHTML = '<td>'+(i+1)+'</td>' +
                         '<td>'+(r.tanggal||'')+'</td>' +
                         '<td>'+(r.deskripsi||'')+'</td>' +
                         '<td>'+(r.kategori||'')+'</td>' +
                         '<td>'+(r.tipe||'')+'</td>' +
                         '<td style="text-align:right">'+(r.jumlah||0)+'</td>' +
                         '<td style="text-align:right">'+(r.saldo||0)+'</td>';
          tbody.appendChild(tr);
        }
      }

      function renderSummary(s){
        document.getElementById('totalIn').innerText = (s.totalPemasukan || 0);
        document.getElementById('totalOut').innerText = (s.totalPengeluaran || 0);
        document.getElementById('saldoAkhir').innerText = (s.saldoAkhir || 0);
      }

      document.getElementById('simpanBtn').addEventListener('click', function(){
        var obj = {
          tanggal: document.getElementById('tanggal').value,
          deskripsi: document.getElementById('deskripsi').value,
          kategori: document.getElementById('kategori').value,
          tipe: document.getElementById('tipe').value,
          jumlah: document.getElementById('jumlah').value
        };
        if (!obj.jumlah || Number(obj.jumlah) <= 0){
          alert('Masukkan jumlah transaksi yang valid (>0).'); return;
        }
        google.script.run.withSuccessHandler(function(res){
          if (res && res.success){
            alert('Transaksi tersimpan. Saldo sekarang: ' + res.saldo);
            // kosongkan input
            document.getElementById('deskripsi').value = '';
            document.getElementById('kategori').value = '';
            document.getElementById('jumlah').value = '';
            refreshAll();
          } else {
            alert('Gagal menyimpan transaksi.');
          }
        }).addTransaction(obj);
      });

      // Jalankan saat load
      refreshAll();
    </script>
  </body>
</html>
</html>
