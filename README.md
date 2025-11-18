<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <style>
      body{font-family: Arial; padding:10px; width:360px;}
      label{font-size:12px;}
      input, textarea, select, button {padding:6px; margin:6px 0; width:100%; box-sizing:border-box;}
      table{width:100%; border-collapse:collapse; margin-top:8px; font-size:12px;}
      th,td{border:1px solid #ddd; padding:6px; text-align:left;}
      th{background:#f4f4f4;}
      .row{display:flex; gap:6px;}
      .col{flex:1;}
    </style>
  </head>
  <body>
    <h4>Aplikasi Kas - Input</h4>

    <label>Alokasi Dana</label>
    <input id="alokasi" placeholder="Contoh: Operasional / Kas Umum">

    <div class="row">
      <div class="col">
        <label>Pemasukan</label>
        <input id="pemasukan" type="number" step="0.01" placeholder="0">
      </div>
      <div class="col">
        <label>Pengeluaran</label>
        <input id="pengeluaran" type="number" step="0.01" placeholder="0">
      </div>
    </div>

    <label>Pembagian Sisa Saldo (opsional)</label>
    <input id="pembagian" placeholder="Format: Nama:50% , Nama2:50%  OR Nama:100000, ...">

    <label>Catatan</label>
    <input id="catatan" placeholder="Keterangan / bukti">

    <button id="btnSave">Simpan Transaksi</button>

    <div style="margin-top:8px;">
      <strong>Ringkasan:</strong>
      <div>Total Pemasukan: <span id="totalIn">0</span></div>
      <div>Total Pengeluaran: <span id="totalOut">0</span></div>
      <div>Saldo Akhir: <span id="saldoAkhir">0</span></div>
    </div>

    <h5>Riwayat (terbaru paling bawah)</h5>
    <div style="max-height:260px; overflow:auto;">
      <table id="tbl">
        <thead><tr><th>No</th><th>Alokasi</th><th>Pemasukan</th><th>Pengeluaran</th><th>Sisa</th></tr></thead>
        <tbody id="bodyTbl"></tbody>
      </table>
    </div>

    <script>
      // Load awal: ambil ringkasan dan riwayat
      function initialLoad(){
        google.script.run.withSuccessHandler(renderSummary).getSummary();
        google.script.run.withSuccessHandler(renderTransactions).getTransactions();
      }

      function renderSummary(s){
        document.getElementById('totalIn').innerText = (s.totalPemasukan || 0);
        document.getElementById('totalOut').innerText = (s.totalPengeluaran || 0);
        document.getElementById('saldoAkhir').innerText = (s.saldoAkhir || 0);
      }

      function renderTransactions(rows){
        var tbody = document.getElementById('bodyTbl');
        tbody.innerHTML = '';
        if (!rows || rows.length === 0){
          tbody.innerHTML = '<tr><td colspan="5" style="text-align:center;">Belum ada transaksi</td></tr>';
          return;
        }
        // tampilkan semua atau sebagian (misal semua)
        for (var i=0;i<rows.length;i++){
          appendRowToTable(rows[i], false); // false => jangan scroll
        }
        // scroll ke bawah supaya terlihat terbaru
        var container = tbody.parentElement;
        container.scrollTop = container.scrollHeight;
      }

      // Fungsi untuk menambahkan baris ke tabel riwayat di UI
      // jika toBottom true -> scroll ke bawah
      function appendRowToTable(rowObj, toBottom){
        var tbody = document.getElementById('bodyTbl');
        // jika sebelumnya hanya ada pesan "Belum ada transaksi", hapus
        if (tbody.children.length === 1 && tbody.children[0].children.length === 1){
          tbody.innerHTML = '';
        }
        var tr = document.createElement('tr');
        tr.innerHTML = '<td>' + (rowObj.no || '') + '</td>' +
                       '<td>' + (rowObj.alokasi || '') + '</td>' +
                       '<td style="text-align:right">' + (rowObj.pemasukan || 0) + '</td>' +
                       '<td style="text-align:right">' + (rowObj.pengeluaran || 0) + '</td>' +
                       '<td style="text-align:right">' + (rowObj.sisaSaldo || 0) + '</td>';
        tbody.appendChild(tr);
        if (toBottom){
          var container = tbody.parentElement;
          container.scrollTop = container.scrollHeight;
        }
      }

      document.getElementById('btnSave').addEventListener('click', function(){
        var obj = {
          alokasi: document.getElementById('alokasi').value,
          pemasukan: document.getElementById('pemasukan').value,
          pengeluaran: document.getElementById('pengeluaran').value,
          pembagian: document.getElementById('pembagian').value,
          catatan: document.getElementById('catatan').value
        };
        if ((obj.pemasukan === '' || Number(obj.pemasukan) === 0) && (obj.pengeluaran === '' || Number(obj.pengeluaran) === 0)){
          alert('Masukkan nilai pemasukan atau pengeluaran (minimal salah satu).'); return;
        }

        // Panggil server untuk simpan. Saat sukses, server mengembalikan objek transaksi baru.
        google.script.run.withSuccessHandler(function(res){
          if (res && res.success){
            var newTx = res.transaksi;
            // langsung tambahkan ke riwayat UI tanpa reload penuh
            appendRowToTable(newTx, true);
            // perbarui ringkasan (ambil dari server)
            google.script.run.withSuccessHandler(renderSummary).getSummary();

            alert('Tersimpan. Sisa saldo: ' + newTx.sisaSaldo + (newTx.pembagian ? '\nPembagian: ' + newTx.pembagian : ''));
            // kosongkan input
            document.getElementById('alokasi').value = '';
            document.getElementById('pemasukan').value = '';
            document.getElementById('pengeluaran').value = '';
            document.getElementById('pembagian').value = '';
            document.getElementById('catatan').value = '';
          } else {
            alert('Gagal menyimpan transaksi.');
          }
        }).addTransaction(obj);
      });

      // Jalankan load awal saat dibuka
      initialLoad();
    </script>
  </body>
</html>
