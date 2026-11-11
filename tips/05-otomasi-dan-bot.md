# 5. Otomasi dan bot

[← Daftar isi](../README.md)

## n8n

- **Simpan key di Credentials, bukan di node.** Untuk penyedia yang kompatibel dengan OpenAI, buat credential **OpenAI** lalu isi **Base URL** penyedia dan API key-nya. Node **OpenAI Chat Model** dan **AI Agent** akan memakainya.
- **Isi Maximum Number of Tokens** di opsi node chat model. Default-nya tidak dibatasi.
- **Pin data saat menguji.** Setelah satu node AI berhasil, pin output-nya supaya node berikutnya bisa diuji tanpa memanggil AI lagi (dan tanpa bayar lagi).
- **AI Agent jauh lebih boros daripada Basic LLM Chain.** Agent bisa memanggil tools berkali-kali dalam satu eksekusi. Kalau tugasnya hanya "ringkas" atau "klasifikasi", pakai Basic LLM Chain.
- **Pasang Retry On Fail** (di Settings node) untuk error sementara seperti 429 atau 502, dan buat **Error Workflow** yang mengirim notifikasi ke Telegram/email saat workflow gagal.
- **Potong teks panjang** sebelum masuk ke AI, misalnya dengan expression `{{ $json.text.slice(0, 20000) }}`.

Contoh workflow siap impor: [EzkielynaGroup/n8n-workflows-indonesia](https://github.com/EzkielynaGroup/n8n-workflows-indonesia).

## Google Sheets (Apps Script)

Fungsi kustom seperti `=AI(A2)` sangat praktis, tapi ada jebakan:

- **Fungsi kustom dihitung ulang** saat sheet dibuka ulang atau isinya berubah. Seribu baris berarti seribu request, dan bisa terulang. Setelah hasil keluar, salin lalu **Paste special → Values only**.
- **Pakai `CacheService`** untuk menyimpan hasil per isi sel selama beberapa jam, jadi teks yang sama tidak dikirim dua kali.
- **Fungsi kustom harus selesai dalam ±30 detik.** Untuk ratusan baris, pakai menu kustom atau trigger berwaktu yang memproses per batch, bukan rumus di setiap sel.
- **Simpan key di Script Properties** (Project Settings → Script Properties), bukan ditulis di kode, supaya tidak ikut terbagi saat sheet di-share.

```javascript
const KEY = PropertiesService.getScriptProperties().getProperty('AI_API_KEY');
```

## Bot WhatsApp dan Telegram

- **Telegram** paling mudah untuk mulai: buat bot di @BotFather, jalankan dengan long polling, tidak perlu server publik.
- **WhatsApp**: pakai **WhatsApp Cloud API** dari Meta (atau penyedia BSP). Library yang mengendalikan WhatsApp Web melanggar ketentuan WhatsApp dan nomornya bisa diblokir.
- **Ingatan pendek per chat.** Simpan beberapa tanya-jawab terakhir per nomor/ID chat; jangan satu riwayat untuk semua pengguna.
- **Sediakan jalan keluar ke manusia.** Kata kunci seperti `admin` atau tombol "Hubungi CS" yang meneruskan ke orang sungguhan.
- **Batas pesan per pengguna per hari** mencegah satu orang menghabiskan saldo.
- **Jangan biarkan bot mengarang harga atau stok.** Ambil data dari spreadsheet atau database, masukkan ke prompt, dan suruh model hanya menjawab dari data itu (lihat [Prompt yang bekerja](03-prompt.md)).

Starter open-source bot Telegram + WhatsApp Cloud API dalam Python: [EzkielynaGroup/bot-wa-ai-starter](https://github.com/EzkielynaGroup/bot-wa-ai-starter).
