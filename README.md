# Tips AI Indonesia

Kumpulan tips dan trik memakai AI (LLM) lewat API, untuk **developer dan UMKM Indonesia**. Isinya hal praktis yang biasanya baru ketahuan setelah tagihan membengkak atau bot tiba-tiba error.

Semua tips di sini **tidak terikat ke satu penyedia**. Contoh kode memakai format API OpenAI (`/v1/chat/completions`) karena format itu dipakai hampir semua penyedia dan gateway, jadi cukup ganti `base_url`, API key, dan nama model.

> Punya tips sendiri? Kirim pull request. Panduannya ada di [CONTRIBUTING.md](CONTRIBUTING.md).

## Daftar isi

1. [Memilih model](tips/01-memilih-model.md): cepat dan murah atau pintar dan mahal, kapan pakai yang mana
2. [Menghemat token](tips/02-hemat-token.md): prompt caching, ringkas konteks, `max_tokens`, streaming
3. [Prompt yang bekerja](tips/03-prompt.md): template peran, contoh, output JSON
4. [Coding agent](tips/04-coding-agent.md): Claude Code, OpenCode, Cline, file `AGENTS.md` / `CLAUDE.md`, cara kerja konteks
5. [Otomasi dan bot](tips/05-otomasi-dan-bot.md): n8n, Google Sheets, bot WhatsApp dan Telegram
6. [Membaca error API](tips/06-error-api.md): 401, 404, 429, 5xx, konteks terlalu panjang
7. [Keamanan API key](tips/07-keamanan-api-key.md): `.env`, jangan di-commit, rotasi, batas kuota
8. [Menghitung biaya](tips/08-biaya.md): rumus token, contoh hitungan per bulan

## Lima tips paling berguna

1. **Mulai dari model kecil.** Untuk CS toko, ringkasan, dan klasifikasi, model "flash/mini" biasanya sudah cukup dan bisa 10 sampai 50 kali lebih murah. Naik ke model besar hanya untuk tugas yang benar-benar gagal di model kecil.
2. **Selalu isi `max_tokens`.** Jawaban yang tidak dibatasi adalah sumber tagihan kejutan paling umum.
3. **Taruh bagian prompt yang tetap di depan.** Instruksi sistem dan data toko yang sama di setiap request bisa ter-cache, sehingga lebih murah dan lebih cepat.
4. **Simpan key di `.env`, bukan di kode.** Tambahkan `.env` ke `.gitignore` sebelum commit pertama.
5. **Baca angka token di respons.** Field `usage` di setiap jawaban API memberi tahu berapa token yang benar-benar ditagih. Dari situ biaya bisa dihitung, bukan ditebak.

## Dibuat oleh

Repo ini dirawat oleh tim [Ezkielyna](https://ezkielyna.store/?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia), gateway API AI independen yang kompatibel dengan format OpenAI dan Anthropic, dengan saldo yang bisa dibayar QRIS. Tips di sini berlaku untuk penyedia mana pun.

Bacaan lanjutan dari kami:
- [Panduan setup per aplikasi](https://ezkielyna.store/panduan?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia) (Claude Code, Cursor, Cline, n8n, dan lainnya)
- [Arti pesan error API AI](https://ezkielyna.store/error?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- [Perkiraan biaya AI per kasus](https://ezkielyna.store/biaya?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia)
- Contoh kode: [EzkielynaGroup/ezkielyna-api-examples](https://github.com/EzkielynaGroup/ezkielyna-api-examples), starter bot: [EzkielynaGroup/bot-wa-ai-starter](https://github.com/EzkielynaGroup/bot-wa-ai-starter)

## Lisensi

Teks di repo ini memakai [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE): boleh disalin, diterjemahkan, dan dipakai ulang (termasuk untuk komersial) asalkan menyebut sumbernya, misalnya dengan link ke repo ini. Nama produk dan merek yang disebut adalah milik pemiliknya masing-masing.
