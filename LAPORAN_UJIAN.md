# Laporan UTS Web Software Tools

**Nama:*Ilham Restu Aji*
**Nomor Induk Mahasiswa:*12523009*
**Kelas:**
**Tanggal:*14/11/2026*

## A. Bukti pemeriksaan masalah
| Masalah | Alat yang digunakan | Bukti pemeriksaan | Perkiraan penyebab | Perbaikan | Hasil setelah perbaikan |
|---|---|---|---|---|---|
| Produk | Browser DevTools (Network & Console) | Network: Request GET ke `/api/barang` mengembalikan status 404 Not Found. Console: Error `Gagal mengambil daftar produk: Permintaan gagal dengan status 404`. | Di `public/app.js` fungsi `loadProducts()` memanggil endpoint `/api/barang`, sedangkan `server.mjs` menyediakan endpoint `/api/products`. | Mengubah URL pemanggilan API pada `fetch` di `public/app.js` baris 48 dari `/api/barang` menjadi `/api/products`. | Request GET ke `/api/products` mengembalikan status 200 OK, teks status menampilkan "4 produk tersedia", dan keempat daftar produk tampil dengan benar. |
| Tombol keranjang | Browser DevTools (Elements & Console) | Elements: Tombol dirender dengan `class="add-button"`. Console: Seleksi `document.querySelectorAll(".add-to-cart")` bernilai kosong (`NodeList(0)`), serta panel Event Listeners menunjukkan tidak ada event click terpasang pada tombol. | Terjadi perbedaan nama class (*selector mismatch*): kode HTML tombol menggunakan class `add-button`, namun pemanggilan event listener di `public/app.js` mencari `.add-to-cart`. | Mengubah selector di `public/app.js` baris 27 dari `document.querySelectorAll(".add-to-cart")` menjadi `document.querySelectorAll(".add-button")`. | Tombol "+ Tambah ke Keranjang" aktif dan bereaksi saat diklik; produk berhasil masuk ke keranjang belanja dan jumlah item bertambah. |
| Total belanja | Pengujian otomatis (`node --test`) & Browser | Dari 7 skenario uji otomatis awal, ada 2 tes yang gagal (tes beli 2 mouse hanya menghasilkan 75.000 padahal seharusnya 150.000). Di halaman web, saat menambah jumlah produk yang sama, total harga belanja juga tidak bertambah. | Pada fungsi `calculateTotal()` di `src/cart.js`, rumus perhitungannya hanya menjumlahkan harga satuan (`item.price`), tanpa mengalikan dengan jumlah barang (`item.quantity`). | Memperbaiki rumus penjumlahan pada `src/cart.js` menjadi `cart.reduce((total, item) => total + item.price * item.quantity, 0)`. | Seluruh 7 skenario pengujian otomatis lolos semua (7 passed, 0 fail), dan perhitungan total belanja di halaman web sudah akurat sesuai jumlah barang yang dibeli. |

## B. Hasil pengujian
- Jumlah pengujian sebelum perbaikan: 5 berhasil dan 2 gagal (dari total 7 pengujian).
- Jumlah pengujian berhasil setelah perbaikan: 7 pengujian berhasil lulus semua (100% pass, 0 fail).
- Catatan perubahan pada fungsi yang diperiksa: Pada fungsi `calculateTotal` di dalam file `src/cart.js`, logika penjumlahan disesuaikan agar mengalikan harga satuan barang (`item.price`) dengan kuantitasnya (`item.quantity`).

## C. Kualitas kode
- Temuan sebelum perbaikan:
  1. `npm run lint`: Menemukan peringatan `noUnusedVariables` pada file `src/cart.js` baris 3 karena variabel `var discount = 0;` dideklarasikan namun tidak pernah dipakai.
  2. `npm run format:check`: Menemukan ketidaksesuaian kerapian format dan spasi pada file `public/app.js` serta baris kosong di `src/cart.js`.
- Perbaikan yang dilakukan:
  1. Menghapus baris variabel `var discount = 0;` pada file `src/cart.js`.
  2. Menjalankan perintah format otomatis Biome (`npx @biomejs/biome format --write public/app.js src/cart.js`) agar struktur penulisan kode seragam dan rapi.
- Hasil pemeriksaan ulang:
  Saat dijalankan kembali perintah `npm run quality:check`, kedua berkas berhasil lolos pemeriksaan dengan status bersih (Checked 2 files, 0 error, 0 warning).

## D. Git
- Branch (jika membuat): main
- Ringkasan commit:
  - `95ebd2f chore: initial exam` : Menyimpan kondisi awal project saat pertama kali dibuka sebelum ada modifikasi kode.
  - `89e02f6 fix: repair product list endpoint` : Memperbaiki URL pemanggilan API pada fetch dari `/api/barang` menjadi `/api/products` agar produk tampil.
  - `2127f27 fix: attcah click even listener to add button` : Menyesuaikan selector tombol dari `.add-to-cart` ke `.add-button` agar tombol tambah keranjang dapat diklik.
  - `5da8b27 fix: di oerhitungan di cart.js disana gada item wuantity atau jumlah barangnya maka disitulah letak nya makanysa saya tambahkan item quantity` : Memperbaiki logika perhitungan fungsi `calculateTotal` agar harga barang dikalikan dengan kuantitasnya.
  - Commit terakhir untuk perapian kualitas kode Biome dan finalisasi laporan.
- Hasil pemeriksaan `git status`:
  Status menunjukkan `On branch main`, `nothing to commit, working tree clean` (seluruh tahapan perubahan kode telah terekam dan tersimpan dengan baik).

## E. Pemakaian kecerdasan buatan (jika digunakan)
- Nama alat: Antigravity AI Assistant
- Pertanyaan/instruksi yang diberikan: Meminta bantuan untuk mengamati error melalui bukti DevTools peramban dan hasil pengujian otomatis, memandu langkah perbaikan kode secara bertahap, serta membantu menyusun dokumentasi laporan pemeriksaan.
- Bagian jawaban yang diterapkan:
  1. Perbaikan URL fetch API ke endpoint yang sesuai pada `public/app.js`.
  2. Penyesuaian class selector tombol di `public/app.js` agar event listener terpasang.
  3. Penambahan pengali kuantitas barang (`* item.quantity`) pada fungsi `calculateTotal` di `src/cart.js`.
  4. Penghapusan variabel nganggur (`var discount`) serta perapian format kode dengan Biome.
- Cara memeriksa apakah saran tersebut benar:
  1. Memeriksa tab Network dan Console DevTools di browser: respons status 200 OK dan 4 produk berhasil tampil.
  2. Menguji klik tombol di browser: barang berhasil masuk ke keranjang dan jumlah ter-update.
  3. Menjalankan pengujian otomatis (`node --test tests/cart.test.mjs`): seluruh 7 pengujian lulus 100% (7 passed, 0 fail).
  4. Menjalankan `npm run quality:check`: kode bersih dari warning maupun error linter/formatter.

## F. Refleksi singkat
- Satu pelajaran paling penting dari ujian ini: Belajar membiasakan alur kerja penyelesaian masalah perangkat lunak berbasis bukti nyata (mengamati DevTools Console/Network/Elements dan pengujian otomatis) daripada hanya sekadar menebak-nebak, serta memahami pentingnya mencatat riwayat perubahan secara disiplin menggunakan Git dan menjaga kualitas kode dengan linter.
