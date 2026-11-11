# 4. Coding agent

[← Daftar isi](../README.md)

Coding agent (Claude Code, OpenCode, Cline, Roo Code, Continue, Cursor, dan sejenisnya) membaca file, menjalankan perintah, dan mengedit kode atas namamu. Hasilnya sangat tergantung pada **konteks** yang ia lihat.

## Cara kerja konteks (dan kenapa tagihannya bisa besar)

Setiap langkah agent mengirim ulang seluruh percakapan: instruksi sistem tool, isi file yang sudah dibaca, output perintah, dan semua pesan sebelumnya. Satu sesi panjang bisa berisi puluhan ribu token per langkah, dan satu tugas bisa terdiri dari puluhan langkah. Karena itu:

- **Mulai sesi baru untuk tugas baru.** Di Claude Code ada `/clear`; di aplikasi lain buka chat atau task baru.
- **Ringkas sesi yang panjang.** Claude Code punya `/compact`, OpenCode dan Cline punya fitur serupa untuk memadatkan percakapan.
- **Prompt caching sangat membantu** karena bagian awal percakapan selalu sama. Pilih model/penyedia yang mendukung cache untuk agent.
- **Jangan biarkan agent membaca file raksasa**: log, `node_modules`, data dump, file build. Batasi lewat pengaturan izin atau file ignore yang didukung tool-nya.

## File instruksi proyek: `AGENTS.md` dan `CLAUDE.md`

Hampir semua agent membaca file instruksi di root repo setiap kali sesi dimulai:

| Tool | File |
|---|---|
| Claude Code | `CLAUDE.md` (bisa juga di subfolder) |
| OpenCode, Codex CLI, dan banyak tool lain | `AGENTS.md` |
| Cline | `.clinerules` |
| Cursor | `.cursor/rules/` |

Isi yang berguna, singkat saja (file ini ikut terkirim di setiap request):

```markdown
# Proyek: toko-online
- Stack: Next.js 15, Prisma, PostgreSQL. Bahasa UI: Indonesia.
- Jalankan tes: `npm test`. Lint: `npm run lint`. Wajib lulus sebelum selesai.
- Jangan mengubah file di `prisma/migrations/` yang sudah ada; buat migrasi baru.
- Harga disimpan dalam Rupiah (integer), jangan pakai float.
- Kode baru: TypeScript strict, fungsi maksimal ±50 baris.
```

Tulis hal yang **tidak bisa ditebak** dari kode: perintah tes, aturan bisnis, folder yang tidak boleh disentuh. Jangan menyalin dokumentasi framework yang sudah diketahui model.

## Tips memakai agent sehari-hari

1. **Minta rencana dulu.** Untuk perubahan besar, minta agent menjelaskan rencana sebelum mengedit (Claude Code dan Cline punya mode Plan). Koreksi rencana jauh lebih murah daripada koreksi kode.
2. **Satu tugas, satu permintaan yang jelas.** "Tambahkan validasi nomor HP di form checkout, format 08xx atau +62, dengan tes" lebih baik daripada "perbaiki form".
3. **Beri cara memeriksa hasil.** Sebutkan perintah tes atau langkah manual. Agent yang bisa menjalankan tes sendiri membuat jauh lebih sedikit kesalahan.
4. **Pakai git.** Commit sebelum meminta perubahan besar, supaya mudah kembali dengan `git diff` / `git restore`.
5. **Jangan beri izin tanpa batas.** Tinjau perintah yang berbahaya (`rm -rf`, migrasi database, `git push --force`) sebelum disetujui.
6. **Rahasiakan key.** Agent bisa membaca `.env`. Jangan menempelkan key produksi ke chat agent; pakai key terpisah dengan batas kuota kecil untuk pengembangan.

## Memakai agent dengan penyedia lain

Banyak agent menerima base URL kustom yang kompatibel dengan OpenAI atau Anthropic: cukup isi base URL, API key, dan nama model. Contoh pengaturan per tool ada di [panduan setup](https://ezkielyna.store/panduan?utm_source=github&utm_medium=readme&utm_campaign=tips-ai-indonesia) dan [ezkielyna-api-examples](https://github.com/EzkielynaGroup/ezkielyna-api-examples).
