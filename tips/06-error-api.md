# 6. Membaca error API

[← Daftar isi](../README.md)

Sebelum mengganti kode, baca tiga hal: **kode status HTTP**, **pesan error** di body respons, dan **URL** yang dipanggil. Hampir semua error bisa ditebak dari kombinasi ketiganya.

| Kode / pesan | Penyebab paling umum | Yang dicek |
|---|---|---|
| `401 Unauthorized`, `invalid_api_key` | key salah ketik, terpotong saat disalin, sudah dihapus, atau ada spasi/baris baru di ujungnya | salin ulang key; pastikan header `Authorization: Bearer sk-...` |
| `402` / `403`, `insufficient_quota` | saldo atau kuota habis, atau key tidak punya akses ke model itu | saldo di dashboard; grup/izin key |
| `404 Not Found` | base URL salah: kurang atau kelebihan `/v1`, atau endpoint salah ketik | base URL format OpenAI biasanya diakhiri `/v1`; klien Anthropic biasanya tanpa `/v1` |
| `model_not_found` | nama model salah ketik, atau model sudah tidak tersedia | ambil daftar model dari endpoint `GET /v1/models` |
| `429 Too Many Requests` | terlalu banyak request per menit, atau batas token per menit | tunggu lalu ulangi dengan jeda yang makin panjang (exponential backoff); kurangi paralel |
| `context_length_exceeded` | input + `max_tokens` melebihi batas konteks model | potong riwayat chat atau dokumen; kecilkan `max_tokens`; pilih model berkonteks lebih besar |
| `500`, `502`, `503`, `504` | server penyedia atau upstream bermasalah, atau request terlalu lama | ulangi 2–3 kali dengan jeda; pakai streaming untuk jawaban panjang; naikkan timeout klien |

## Pola retry yang aman

```python
import time, random

def panggil_dengan_retry(fn, percobaan=4):
    for i in range(percobaan):
        try:
            return fn()
        except Exception as e:
            status = getattr(e, "status_code", None)
            if status not in (429, 500, 502, 503, 504) or i == percobaan - 1:
                raise
            time.sleep((2 ** i) + random.random())   # 1s, 2s, 4s, ... plus sedikit acak
```

- **Jangan retry error 4xx selain 429.** Key salah atau model salah tidak akan sembuh dengan diulang, malah membuat log penuh.
- **Batasi jumlah percobaan.** Retry tanpa batas pada request berbayar bisa menggandakan biaya.

## Jawaban kosong atau terpotong

- `finish_reason: "length"`: jawaban terpotong oleh `max_tokens`.
- Jawaban kosong di model reasoning: token habis untuk "thinking". Naikkan `max_tokens` atau turunkan tingkat reasoning.
- `finish_reason: "content_filter"`: jawaban ditahan filter konten penyedia.

Penjelasan lengkap per error dalam Bahasa Indonesia: [ezkielyna.store/error](https://ezkielyna.store/error?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), misalnya [401 Unauthorized](https://ezkielyna.store/error/401-unauthorized?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), [insufficient quota](https://ezkielyna.store/error/insufficient-quota?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), [model not found](https://ezkielyna.store/error/model-not-found?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), [429 rate limit](https://ezkielyna.store/error/429-rate-limit?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), [context length exceeded](https://ezkielyna.store/error/context-length-exceeded?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), dan [502/504 timeout](https://ezkielyna.store/error/502-504-timeout?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia).
