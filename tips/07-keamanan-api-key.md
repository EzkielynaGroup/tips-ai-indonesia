# 7. Keamanan API key

[← Daftar isi](../README.md)

API key adalah uang. Siapa pun yang memegangnya bisa menghabiskan saldomu. Key yang ter-push ke repo publik bisa ditemukan bot dalam hitungan menit.

## Simpan di `.env`, jangan di kode

```bash
# .env  (JANGAN di-commit)
AI_API_KEY=sk-...
AI_BASE_URL=https://alamat-penyedia/v1
```

```bash
# .gitignore  (buat SEBELUM commit pertama)
.env
.env.*
!.env.example
```

Sertakan `.env.example` berisi nama variabel dengan nilai contoh (`sk-...`) supaya orang lain tahu apa yang harus diisi.

## Kalau key terlanjur ter-commit

1. **Hapus atau ganti key di dashboard penyedia sekarang juga.** Ini langkah terpenting.
2. Menghapus file di commit berikutnya **tidak cukup**: key masih ada di riwayat git dan di salinan orang lain.
3. Buat key baru, simpan di `.env`, dan cek pemakaian/log di dashboard untuk request yang tidak kamu kenali.

Pencegahan: aktifkan secret scanning / push protection di GitHub, atau pasang pemindai seperti `gitleaks` sebagai pre-commit hook.

## Jangan taruh key di sisi klien

- Kode di browser (JavaScript frontend), aplikasi Android/iOS, dan file APK **bisa dibaca siapa saja**.
- Panggil AI dari **server/backend** milikmu, lalu aplikasi memanggil server itu. Server juga tempat yang tepat untuk membatasi pemakaian per pengguna.
- Begitu juga Google Sheets yang di-share: simpan key di Script Properties, bukan di sel atau kode.

## Batasi kerusakan

- **Satu key per aplikasi atau per lingkungan** (dev, produksi, bot A, bot B). Kalau satu bocor, cukup satu yang dicabut, dan log pemakaian lebih mudah dibaca.
- **Pasang batas kuota atau saldo per key** kalau penyedia mendukungnya. Key untuk uji coba cukup diberi saldo kecil.
- **Rotasi berkala**, misalnya tiap 3 bulan, dan segera saat ada anggota tim yang keluar.
- **Pantau log pemakaian.** Lonjakan request tengah malam atau model yang tidak pernah kamu pakai adalah tanda key bocor.
- **Jangan menempelkan key ke chat** (grup WhatsApp, Discord, chat AI, issue GitHub, screenshot). Kalau perlu minta bantuan, sensor jadi `sk-...`.
- **Jangan menyimpan key di log aplikasi.** Saat mencatat request untuk debugging, buang header `Authorization`.
