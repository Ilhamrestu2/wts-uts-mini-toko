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
| Total belanja | | | | | |

## B. Hasil pengujian
- Jumlah pengujian sebelum perbaikan:
- Jumlah pengujian berhasil setelah perbaikan:
- Catatan perubahan pada fungsi yang diperiksa:

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
