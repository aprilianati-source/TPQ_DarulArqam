<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <style>
      body{font-family: Arial; padding:10px; width:350px;}
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
    <h4>Aplikasi Kas - Input Transaksi</h4>

    <label>Alokasi Dana</label>
    <input id="alokasi" placeholder="Contoh: Operasional / Kas Umum">

    <div class="row">
      <div class="col">
        <label>Pemasukan (angka)</label>
        <input id="pemasukan" type="number" step="0.01" placeholder="0">
      </div>
      <div class="col">
        <label>Pengeluaran (angka)</label>
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
    <div style="max-height:220px; overflow:auto;">
      <table id="tbl">
        <thead><tr><th>No</th><th>Alokasi</th><th>Pemasukan</th><th>Pengeluaran</th><th>Sisa</th></tr></thead>
        <tbody id="bodyTbl"></tbody>
      </table>
    </div>

    <script>
      function refreshAll(){
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
        // tampilkan beberapa baris terakhir (misal 50)
        var start = Math.max(0, rows.length - 50);
        for (var i=start;i<rows.length;i++){
          var r = rows[i];
          var tr = document.createElement('tr');
          tr.innerHTML = '<td>'+(r.no||'')+'</td>' +
                         '<td>'+(r.alokasi||'')+'</td>' +
                         '<td style="text-align:right">'+(r.pemasukan||0)+'</td>' +
                         '<td style="text-align:right">'+(r.pengeluaran||0)+'</td>' +
                         '<td style="text-align:right">'+(r.sisaSaldo||0)+'</td>';
          tbody.appendChild(tr);
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
        google.script.run.withSuccessHandler(function(res){
          if (res && res.success){
            alert('Tersimpan. Sisa saldo: ' + res.sisaSaldo + (res.pembagian ? '\\nPembagian: ' + res.pembagian : ''));
            // kosongkan input
            document.getElementById('alokasi').value = '';
            document.getElementById('pemasukan').value = '';
            document.getElementById('pengeluaran').value = '';
            document.getElementById('pembagian').value = '';
            document.getElementById('catatan').value = '';
            refreshAll();
          } else {
            alert('Gagal menyimpan transaksi.');
          }
        }).addTransaction(obj);
      });

      // initial load
      refreshAll();
    </script>
  </body>
</html>
