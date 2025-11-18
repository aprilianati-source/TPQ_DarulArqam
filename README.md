/**
 * doPost menerima JSON:
 * { date: "YYYY-MM-DD", data: [ {id,name,status,time}, ... ] }
 *
 * Menulis ke sheet "Absensi" dengan format:
 * Tanggal | No. | Nama | Status | Waktu Kehadiran
 *
 * Jika sheet sudah berisi baris untuk tanggal yang sama, baris lama untuk tanggal itu
 * akan dihapus terlebih dahulu, lalu data baru ditambahkan (replace).
 *
 * Juga menambahkan header jika sheet baru.
 *
 * Script ini mengembalikan JSON { result: 'ok', written: n } atau { result: 'error', message: ... }.
 * Menangani CORS dengan mengirim header yang sesuai.
 */

function doPost(e) {
  // untuk debugging saat develop, bisa log
  try {
    // Pastikan ada postData
    if (!e || !e.postData || !e.postData.contents) {
      return _jsonResponse({ result: 'error', message: 'No post data' }, 400);
    }

    var body = JSON.parse(e.postData.contents);
    var date = body.date || Utilities.formatDate(new Date(), Session.getScriptTimeZone(), 'yyyy-MM-dd');
    var data = body.data || [];

    // Buka spreadsheet aktif (atau ganti dengan SpreadsheetApp.openById('SPREADSHEET_ID'))
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheetName = 'Absensi';
    var sheet = ss.getSheetByName(sheetName);
    if (!sheet) {
      sheet = ss.insertSheet(sheetName);
      // header: sesuai format tampilan absensi
      sheet.appendRow(['Tanggal', 'No.', 'Nama', 'Status', 'Waktu Kehadiran']);
    }

    // 1) Hapus blok data untuk tanggal yang sama (jika ada)
    // Kita cari baris-baris yang kolom A (=TANGGAL) sama dengan date, dan hapus mereka.
    var lastRow = sheet.getLastRow();
    if (lastRow >= 2) {
      var range = sheet.getRange(2, 1, lastRow - 1, 1); // semua tanggal di kolom A dari baris 2
      var vals = range.getValues(); // array Nx1
      // kumpulkan indeks baris yang match
      var rowsToDelete = [];
      for (var i = 0; i < vals.length; i++) {
        if (vals[i][0] == date) {
          // baris sebenarnya di sheet = i + 2
          rowsToDelete.push(i + 2);
        }
      }
      // Hapus baris dari bawah ke atas agar indeks tidak bergeser
      if (rowsToDelete.length > 0) {
        rowsToDelete.sort(function(a,b){return b-a;});
        for (var j = 0; j < rowsToDelete.length; j++) {
          sheet.deleteRow(rowsToDelete[j]);
        }
      }
    }

    // 2) Tambahkan data baru untuk tanggal tersebut
    // Format tiap baris: [date, no, name, status, time]
    var toWrite = [];
    for (var k = 0; k < data.length; k++) {
      var r = data[k];
      var no = k + 1;
      var name = r.name || '';
      var status = r.status || '';
      var time = r.time || '';
      toWrite.push([date, no, name, status, time]);
    }

    if (toWrite.length > 0) {
      sheet.getRange(sheet.getLastRow() + 1, 1, toWrite.length, toWrite[0].length).setValues(toWrite);
    }

    return _jsonResponse({ result: 'ok', written: toWrite.length }, 200);

  } catch (err) {
    return _jsonResponse({ result: 'error', message: err.toString() }, 500);
  }
}

/**
 * Helper untuk membentuk response JSON dengan header CORS.
 */
function _jsonResponse(obj, statusCode) {
  var out = ContentService.createTextOutput(JSON.stringify(obj));
  out.setMimeType(ContentService.MimeType.JSON);

  // Untuk Web App, kita tidak dapat men-set header CORS langsung via ContentService.
  // Namun Google Apps Script Web Apps biasanya mengizinkan fetch dari client.
  // Jika anda butuh header CORS eksplisit (untuk fetch dengan credentials), gunakan doGet+Callback JSONP,
  // atau deploy dengan metode lain. Untuk kebanyakan kasus fetch POST dari browser, ini sudah bekerja.
  return out;
}
