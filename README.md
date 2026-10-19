# Telegram Token Manager Pro

## Lokasi kode bot dan database
Kode bot (`index.js`, `package.json`, `config.js`, dan file lain) cukup disimpan di server tempat bot dijalankan, misalnya Pterodactyl. **Kode bot tidak perlu diunggah ke GitHub.** GitHub hanya dipakai sebagai database melalui GitHub Contents API.

## Konfigurasi lokal
1. Gunakan Node.js 18 atau lebih baru.
2. Isi `config.js` dengan token Telegram, GitHub token, ID Developer Utama, repository database, dan channel wajib. Jangan unggah atau bagikan `config.js`.
3. Token GitHub gunakan Fine-grained personal access token dengan akses hanya ke repository database dan izin Repository permissions: **Contents: Read and write**.
4. Jalankan `npm install`, lalu `npm start`.
5. Bot perlu menjadi admin channel untuk memeriksa keanggotaan. Untuk menghapus pesan anggota yang belum join dari grup, bot perlu izin Delete Messages.

## Struktur repository GitHub DATABASE SAJA
```text
NAMA_REPOSITORY_DATABASE/
├── tokens.json
└── roles.json
```
`tokens.json`:
```json
{ "tokens": [] }
```
`roles.json` — satu file untuk menyimpan semua ID role:
```json
{
  "fullup": [],
  "reseller": [],
  "partner": [],
  "owner": [],
  "developer": []
}
```
Bot akan membuat file jika belum ada dan memperbaruinya otomatis. Jangan unggah kode bot ke repository database.

## Perintah
- `/start`, `/help`, `/myrole`
- Developer Utama saja: `/setrole ID fullup 30d`, `/setrole ID fullup perma`, `/setrole ID reseller perma`, `/setrole ID partner perma`, `/setrole ID owner perma`, `/setrole ID developer perma`, `/delrole ID`, `/listrole`
- Pengelolaan token: `/addtoken TOKEN_BOT`, `/deltoken TOKEN_BOT`, `/listtoken` — hanya role dengan izin token yang dapat menggunakannya; Full Up dikecualikan.

## Aturan akses
- `fullup`: hanya izin `script:use`; tidak dapat menambah, menghapus, atau melihat token. Integrasi script harus disediakan oleh proyek yang dijalankan.
- `reseller`, `partner`, `owner`: sesuai izin yang dikonfigurasi aplikasi; tidak dapat mengatur role.
- `developer`: role aplikasi; pemberian role hanya dapat dilakukan Developer Utama yang ID-nya ada di `config.js`.
- Developer Utama dikunci melalui `DEVELOPER_ID` pada `config.js`.

Telegram tidak mendukung inline button berwarna/berkedip secara native. Role aplikasi tidak otomatis menjadikan pengguna admin grup.


## Daftar Harga (GitHub)
Tambahkan `prices.json` di root repository database dengan isi awal `{ "harga": [], "hargaUp": [] }`. Pastikan token GitHub memiliki izin Contents: Read and write.

- Ketik `harga` atau `list harga` untuk melihat daftar harga umum.
- Ketik `harga up` atau `list harga up` untuk melihat daftar harga UP.
- Developer Utama: `/setharga Nama|Harga` untuk menambah/mengubah harga.
- Developer Utama: `/sethargaup Nama|Harga` untuk menambah/mengubah harga UP.
- Developer Utama: `/delharga Nama` dan `/delhargaup Nama` untuk menghapus item.

Contoh: `/setharga Paket Basic|Rp 25.000`
Contoh: `/sethargaup UP 30 Hari|Rp 50.000`
