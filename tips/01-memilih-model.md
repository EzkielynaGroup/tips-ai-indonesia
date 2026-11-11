# 1. Memilih model

[← Daftar isi](../README.md)

## Dua kelompok besar

| | Model cepat/murah ("flash", "mini", "lite", "haiku") | Model pintar/mahal ("pro", "opus", model reasoning) |
|---|---|---|
| Cocok untuk | balas chat pelanggan, ringkasan, klasifikasi, terjemahan, ekstraksi data | coding sulit, analisis panjang, perencanaan bertahap, soal matematika |
| Kecepatan | jawaban mulai muncul dalam hitungan detik | bisa "berpikir" puluhan detik sebelum menjawab |
| Harga | sering 10 sampai 50 kali lebih murah per token | mahal, apalagi token "thinking" juga ditagih |

Aturan praktis: **mulai dari model murah, ukur hasilnya, naik kelas hanya kalau gagal.**

## Cara menguji dengan adil

1. Kumpulkan 20 sampai 30 contoh nyata (chat pelanggan, email, potongan kode) beserta jawaban yang kamu anggap benar.
2. Jalankan contoh yang sama ke dua atau tiga model dengan prompt yang sama.
3. Nilai hasilnya: benar/salah, nada bahasa, panjang jawaban. Catat juga token dan waktu dari field `usage`.
4. Pilih model dengan biaya terendah yang lolos. Simpan set contoh ini supaya bisa diuji ulang saat ada model baru.

## Tips lain

- **Bahasa Indonesia tidak sama rata.** Sebagian model lancar berbahasa Inggris tapi kaku di Bahasa Indonesia atau bahasa daerah. Uji dengan contoh berbahasa Indonesia, jangan hanya membaca benchmark.
- **Model reasoning tidak selalu lebih baik.** Untuk chat CS yang singkat, model reasoning malah lebih lambat dan lebih mahal karena token "thinking" ikut ditagih. Kalau penyedia memberi opsi seperti `reasoning_effort`, pakai nilai rendah untuk tugas ringan.
- **Pisahkan tugas.** Satu alur bisa memakai dua model: model murah untuk memilah atau meringkas, model pintar hanya untuk langkah yang sulit.
- **Cek panjang konteks.** Dokumen 200 halaman butuh model dengan konteks besar. Kalau konteks kurang, potong dokumen jadi beberapa bagian (lihat [Menghemat token](02-hemat-token.md)).
- **Nama model bisa berubah.** Penyedia sering menambah atau mematikan model. Simpan nama model di file konfigurasi atau variabel lingkungan, jangan ditulis langsung di banyak tempat di kode.
