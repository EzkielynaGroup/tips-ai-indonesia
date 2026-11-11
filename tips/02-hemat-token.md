# 2. Menghemat token

[← Daftar isi](../README.md)

Biaya API AI dihitung per token: token **input** (semua yang kamu kirim, termasuk riwayat chat dan instruksi sistem) dan token **output** (jawaban model). Output biasanya beberapa kali lebih mahal per token daripada input. Hampir semua penghematan datang dari dua hal: mengirim lebih sedikit dan meminta jawaban lebih pendek.

## Batasi jawaban dengan `max_tokens`

```python
resp = client.chat.completions.create(
    model="nama-model",
    messages=messages,
    max_tokens=400,   # batas atas panjang jawaban (dan biaya output)
)
```

- Bot CS: 200 sampai 400 token sudah cukup untuk jawaban yang enak dibaca di HP.
- Ringkasan: tentukan jumlah poin di prompt ("maksimal 5 poin") dan beri `max_tokens` sebagai pengaman.
- Kalau jawaban terpotong, `finish_reason` bernilai `length`. Naikkan batasnya sedikit, jangan dihapus.
- Pada model reasoning, token "thinking" ikut dihitung. Batas yang terlalu kecil bisa membuat jawaban kosong karena habis untuk berpikir.

## Manfaatkan prompt caching

Banyak penyedia memberi diskon besar untuk bagian awal prompt yang sama persis dengan request sebelumnya (prefix yang ter-cache). Sebagian melakukannya otomatis, sebagian (misalnya API Anthropic) meminta penanda `cache_control`. Supaya cache sering kena:

1. Urutkan prompt dari yang paling tetap ke yang paling berubah: instruksi sistem → data toko/dokumen → riwayat chat → pesan terbaru.
2. Jangan menaruh tanggal, jam, atau angka acak di awal instruksi sistem. Satu karakter berbeda di depan membuat seluruh cache meleset.
3. Untuk tanya-jawab atas dokumen yang sama, kirim dokumennya di posisi yang sama setiap kali.

Cek field `usage` di respons: banyak penyedia menampilkan jumlah token yang ter-cache (misalnya `prompt_tokens_details.cached_tokens`).

## Ringkas konteks, jangan kirim semuanya

- **Riwayat chat**: simpan 4 sampai 8 tanya-jawab terakhir saja. Untuk percakapan panjang, buat ringkasan singkat dari bagian lama lalu buang aslinya.
- **Dokumen panjang**: potong per bab, ringkas tiap bab, lalu ringkas gabungan ringkasannya. Atau cari dulu bagian yang relevan (pencarian kata kunci atau embedding) dan kirim bagian itu saja.
- **Data tabel dan HTML**: kirim kolom yang dibutuhkan saja (CSV lebih hemat daripada JSON), buang tag, menu, dan footer.

## Streaming

`stream=True` tidak membuat request lebih murah, tapi pengguna melihat jawaban mulai muncul dalam satu dua detik, jadi bot terasa jauh lebih cepat.

```python
for chunk in client.chat.completions.create(model="nama-model", messages=messages, stream=True, max_tokens=400):
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

## Hemat lainnya

- **Jangan panggil AI untuk hal yang bisa dijawab kode.** Jam buka, ongkir dari tabel, status pesanan dari database: jawab langsung tanpa model.
- **Batasi pemakaian per pengguna.** Misalnya 50 pesan per hari per nomor, supaya satu pengguna iseng tidak menghabiskan saldo.
- **Instruksi sistem yang ringkas.** Prompt sistem 3.000 token yang dikirim 1.000 kali sehari adalah 3 juta token input per hari. Hapus kalimat yang tidak mengubah hasil.
