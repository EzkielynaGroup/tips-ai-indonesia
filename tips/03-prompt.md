# 3. Prompt yang bekerja

[← Daftar isi](../README.md)

Prompt yang baik bukan yang paling panjang, tapi yang membuat hasil **konsisten** dari request ke request.

## Template instruksi sistem

```text
Kamu adalah admin CS Toko Kopi Senja di Bandung.
Tugas: menjawab pertanyaan pembeli tentang produk, harga, ongkir, dan jam buka.

Aturan:
- Jawab dalam Bahasa Indonesia santai dan sopan, maksimal 3 kalimat.
- Hanya pakai informasi di bagian DATA TOKO. Kalau tidak ada, jawab
  "Sebentar ya kak, saya tanyakan ke admin dulu."
- Jangan menjanjikan diskon, stok, atau waktu kirim yang tidak tertulis.

DATA TOKO:
- Jam buka: Senin–Sabtu 08.00–20.00 WIB
- Kopi susu 1 liter: Rp55.000
- Ongkir dalam kota: Rp10.000, gratis di atas Rp150.000
```

Kenapa ini bekerja:
- **Peran dan tugas** di kalimat pertama memberi arah.
- **Aturan berupa daftar** lebih mudah diikuti daripada paragraf panjang.
- **Jawaban cadangan** yang ditulis persis mencegah model mengarang.
- **Data dipisah** dengan judul yang jelas, jadi mudah diganti tanpa menyentuh aturan.

## Beri contoh (few-shot)

Satu dua contoh tanya-jawab sering lebih efektif daripada sepuluh kalimat aturan, terutama untuk nada bahasa dan format.

```text
Contoh:
Pembeli: kak bisa cod?
Admin: Bisa kak, COD tersedia untuk wilayah Bandung kota ya 😊
```

Pakai contoh yang mirip pertanyaan nyata, termasuk salah ketik dan singkatan yang biasa dipakai pelangganmu.

## Minta output JSON untuk diproses kode

Kalau hasil AI akan dibaca program (dimasukkan ke Google Sheets, database, atau dipakai untuk IF di n8n), minta JSON dan tuliskan bentuknya:

```text
Baca email berikut. Balas HANYA dengan JSON tanpa teks lain:
{"kategori": "pesanan|komplain|pertanyaan|spam",
 "urgent": true/false,
 "ringkasan": "maksimal 20 kata"}
```

Tips JSON:
- Kalau penyedia mendukung `response_format={"type": "json_object"}` (atau JSON schema), pakai itu.
- Tetap validasi di kode: `json.loads` dalam `try`, dan siapkan nilai default kalau gagal.
- Pakai pilihan tetap (`pesanan|komplain|...`) supaya mudah dicabangkan.
- Suhu (`temperature`) rendah, misalnya 0 sampai 0,3, membuat output lebih stabil.

## Kesalahan yang sering terjadi

- **Instruksi bertentangan**: "jawab singkat" di atas, "jelaskan selengkapnya" di bawah. Model akan memilih salah satu secara acak.
- **Data pelanggan bercampur instruksi**: pesan pembeli bisa berisi "abaikan aturan di atas". Taruh input pengguna di bagian terpisah dan jangan beri model akses ke tindakan berbahaya (refund, hapus data) tanpa konfirmasi manusia.
- **Tidak pernah diuji ulang**: simpan 20 sampai 30 contoh pertanyaan, jalankan ulang setiap kali prompt diubah.
