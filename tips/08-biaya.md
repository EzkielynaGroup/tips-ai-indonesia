# 8. Menghitung biaya

[← Daftar isi](../README.md)

## Rumus dasar

```text
biaya per request = (token_input × harga_input + token_output × harga_output) / 1.000.000
```

Harga biasanya ditulis **per 1 juta token**, terpisah untuk input dan output. Token input yang ter-cache sering dihargai jauh lebih murah.

Patokan kasar untuk Bahasa Indonesia: **1.000 kata ≈ 1.500 token**, satu halaman dokumen ≈ 750 token. Angka pastinya tergantung tokenizer tiap model, jadi selalu cek field `usage` di respons:

```json
"usage": { "prompt_tokens": 1320, "completion_tokens": 180, "total_tokens": 1500 }
```

## Contoh: bot CS 300 pesan per hari

Misalkan sebuah model dihargai **$0,10 per 1 juta token input** dan **$0,40 per 1 juta token output** (angka contoh, cek harga penyedia yang kamu pakai). Satu pesan pelanggan rata-rata mengirim 1.300 token input (instruksi + data toko + riwayat) dan menerima 200 token output.

```text
per pesan  = (1.300 × 0,10 + 200 × 0,40) / 1.000.000 = $0,00021
per hari   = 300 pesan × $0,00021                   = $0,063
per bulan  = 30 hari × $0,063                       = $1,89  (±Rp30.000 pada kurs Rp16.000)
```

Hal yang paling mengubah hasil:
- **Panjang instruksi sistem dan riwayat** (dikirim ulang di setiap pesan).
- **Panjang jawaban** (output lebih mahal per token).
- **Model reasoning**: token "thinking" ikut ditagih sebagai output.
- **Cache**: prefix yang ter-cache bisa memangkas biaya input secara besar.

## Tips mengendalikan biaya

1. **Ukur 1 hari dulu.** Jalankan dengan pengguna nyata selama sehari, jumlahkan `usage`, lalu kalikan 30. Jauh lebih akurat daripada tebakan.
2. **Pasang batas di tiga tempat**: `max_tokens` per jawaban, batas pesan per pengguna per hari, dan batas saldo/kuota per key.
3. **Bandingkan per tugas, bukan per token.** Model yang dua kali lebih mahal per token tapi menyelesaikan tugas dalam satu kali coba bisa lebih hemat daripada model murah yang perlu tiga kali ulang.
4. **Ingat biaya platform.** WhatsApp Cloud API, hosting n8n, dan server ditagih terpisah dari token.

## Perkiraan per kasus

Perhitungan per bulan dengan harga live dalam Rupiah untuk beberapa kasus umum (dibuat oleh tim Ezkielyna, memakai harga di platform kami):

- [Coding agent](https://ezkielyna.store/biaya/coding-agent?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Bot WhatsApp / Telegram](https://ezkielyna.store/biaya/bot-whatsapp-telegram?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Otomasi n8n](https://ezkielyna.store/biaya/otomasi-n8n?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Ringkas dokumen dan PDF](https://ezkielyna.store/biaya/ringkas-dokumen-pdf?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Chatbot pribadi](https://ezkielyna.store/biaya/chatbot-pribadi?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Terjemahan](https://ezkielyna.store/biaya/terjemahan?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Kalkulator hemat](https://ezkielyna.store/hitung-hemat?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
