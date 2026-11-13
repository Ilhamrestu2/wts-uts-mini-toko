# Laporan UTS Web Software Tools

**Nama:**
**Nomor Induk Mahasiswa:**
**Kelas:**
**Tanggal:**

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
- Perbaikan yang dilakukan:
- Hasil pemeriksaan ulang:

## D. Git
- Branch (jika membuat):
- Ringkasan commit:
- Hasil pemeriksaan `git status`:

## E. Pemakaian kecerdasan buatan (jika digunakan)
- Nama alat:
- Pertanyaan/instruksi yang diberikan:
- Bagian jawaban yang diterapkan:
- Cara memeriksa apakah saran tersebut benar:

## F. Refleksi singkat
- Satu pelajaran paling penting dari ujian ini:
