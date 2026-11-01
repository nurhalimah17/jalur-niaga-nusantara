# Panduan Hosting — Jalur Niaga Nusantara (GitHub + Vercel)

Folder ini berisi **hanya** dua tampilan untuk siswa:

| Alamat | Isi | Kapan dipakai |
|---|---|---|
| `/` | **Atlas Siswa**: profil dan potensi (+) tiap pulau, 38 provinsi. Tanpa kunci, tanpa rute, tanpa kolom defisit. Bisa dipasang ke layar utama HP dan tetap terbuka tanpa sinyal setelah pernah dimuat. | Saat pelajaran (1 HP per kelompok) |
| `/ulasan` | **Ulasan**: ditambah kebutuhan (defisit) dan jalur kapal. Tetap tanpa kunci. | Setelah pelajaran |

> **Jangan pernah mengunggah** `Jalur-Niaga-Nusantara-GURU.html`. Berkas itu memuat kunci jawaban. Folder `hosting/` ini sengaja tidak memuatnya.

## Yang dibutuhkan dari client
1. Akun **GitHub** (nama pengguna saja).
2. Akun **Vercel**, mendaftar lewat tombol *Continue with GitHub*.
3. Nama proyek yang diinginkan, misalnya `jalur-niaga-ypk`. Alamat jadinya `https://jalur-niaga-ypk.vercel.app`.

**Jangan mengirim kata sandi atau token.** Cukup nama pengguna GitHub dan alamat akhir. Bila pengembang perlu mengunggah, cukup diundang sebagai *Collaborator*.

## Langkah A — client mengunggah sendiri (paling aman)
1. Masuk GitHub → **New repository**. Nama: `jalur-niaga-nusantara`. Pilih **Private** (cukup, Vercel tetap bisa membacanya). Jangan centang "Add a README".
2. Pada halaman repositori kosong, klik **uploading an existing file**. Seret **isi** folder `hosting` ke halaman itu (berkas `vercel.json`, `.gitignore`, dan folder `public`). Tunggu semua terunggah, lalu **Commit changes**.
3. Masuk **vercel.com** → **Add New… → Project** → pilih repositori tadi → **Import**.
   - Framework Preset: **Other**
   - Root Directory: `./`
   - Build Command dan Install Command: **dibiarkan kosong**
   - Output Directory: `public` (sudah ditetapkan oleh `vercel.json`)
   - Klik **Deploy**. Selesai dalam sekitar satu menit.
4. Catat alamat yang diberikan Vercel (`https://….vercel.app`). Alamat dapat diganti di *Settings → Domains*.
5. Kirim alamat itu ke pengembang. Pengembang menjalankan:
   ```bash
   cd build
   node make-qr.mjs https://alamat-anda.vercel.app
   ```
   Hasilnya `dist/cetak/07-Kartu-QR.pdf` (8 kartu QR: 7 kelompok + guru, dan 1 poster QR Ulasan) serta berkas QR di `hosting/qr/`. Perintah ini membaca ulang setiap QR yang dibuat dan berhenti bila tidak cocok dengan alamatnya.

## Langkah B — pengembang yang mengunggah
Client menambahkan pengembang sebagai Collaborator pada repositori, lalu dari folder `hosting`:
```bash
git init && git add . && git commit -m "Atlas Jalur Niaga Nusantara"
git branch -M main
git remote add origin https://github.com/NAMA-PENGGUNA/jalur-niaga-nusantara.git
git push -u origin main
```

## Memperbarui isi
Bangun ulang paket (`node build-app.mjs`, lalu `node build-hosting.mjs`), ganti isi `hosting/public` di GitHub, dan *commit*. Vercel menerbitkan versi baru otomatis. HP siswa yang sedang online memuat versi terbaru; yang offline memakai salinan terakhir.
Bila ada kekeliruan: Vercel → **Deployments** → pilih versi sebelumnya → **Promote to Production**.

## Privasi dan keamanan (sudah diatur di `vercel.json`)
- Tidak ada akun siswa, tidak ada pelacakan, tidak ada analitik, dan tidak ada data yang dikirim ke mana pun. Pengaturan suara hanya tersimpan di HP masing-masing.
- Halaman tidak boleh diindeks mesin pencari (`noindex`, `robots.txt`).
- Kebijakan keamanan konten (CSP) membatasi halaman hanya memuat dari dirinya sendiri; kamera, mikrofon, dan lokasi dinonaktifkan.
- Lisensi data peta: batas wilayah BIG (via repositori GeoJSON berlisensi MIT), garis pantai dan relief Natural Earth (domain publik). Musik dan efek suara dibangkitkan program, tanpa berkas pihak ketiga.

## Uji setelah live (5 menit)
- [ ] `https://alamat/` terbuka di HP; peta tampil, tombol pulau bisa digeser.
- [ ] Tidak ada tombol "Misi" atau kolom "Defisit" pada Atlas Siswa.
- [ ] `https://alamat/ulasan` menampilkan defisit dan tombol "Semua jalur dagang".
- [ ] Matikan data seluler/Wi-Fi, buka ulang `https://alamat/`: tetap terbuka (setelah pernah dimuat sekali).
- [ ] Chrome HP: menu ⋮ → **Tambahkan ke layar utama** menampilkan ikon kompas.
- [ ] Pindai kartu QR dengan kamera HP siswa: terbuka ke atlas.
