# Panduan Hosting — Jalur Niaga Nusantara (GitHub + Vercel)

© 2026 · Nur Halimah, S.Pd. · SMP YPK BONTANG

## Isi yang diterbitkan

| Alamat | Isi | Siapa |
|---|---|---|
| `/` | **Media utama** dengan jendela sambutan: **Siswa** (langsung masuk) atau **Guru** (kata sandi). | Siswa dan guru |
| `/ulasan` | **Ulasan**: ditambah kebutuhan wilayah dan jalur kapal. Tanpa kunci jawaban. | Siswa, setelah pelajaran |

## Bagaimana kunci jawaban dilindungi

Halaman utama hanya memuat data untuk siswa secara terbuka. Seluruh data khusus guru (kunci jawaban, kolom kebutuhan, dan semua rute kapal) disimpan **terenkripsi** (AES-256-GCM; kunci diturunkan dari kata sandi lewat PBKDF2-SHA256, 600 ribu putaran). Tanpa kata sandi, isinya tidak dapat dibaca, termasuk oleh siswa yang membuka "lihat sumber halaman".

- Masuk ke mode Guru **selalu meminta kata sandi**, termasuk saat beralih dari Siswa ke Guru. Memuat ulang halaman (F5) saat di mode Guru menutup mode itu.
- Tiga kali salah berturut-turut memberi jeda yang makin lama sebelum boleh mencoba lagi.
- Kekuatan pelindung bergantung pada kata sandi. Kata sandi bawaan terdiri dari empat kata dan empat angka. Jika kata sandi bocor ke siswa, **ganti** (lihat di bawah) dan terbitkan ulang.
- Kata sandi **tidak** ada di repositori. Ia hanya ada di berkas `rahasia/sandi-guru.txt` di komputer pengembang. Berkas itu tidak boleh diunggah ke GitHub atau Vercel dan tidak boleh dikirim ke siswa. Berikan kata sandi kepada Bu Halimah melalui pesan pribadi.

### Mengganti kata sandi
1. Ubah baris pertama `rahasia/sandi-guru.txt` (minimal 8 karakter; disarankan 12 atau lebih; huruf besar/kecil tidak dibedakan).
2. Dari folder `build`: `node build-app.mjs`, lalu `node build-hosting.mjs`.
3. Unggah isi folder `hosting/public` yang baru ke GitHub (atau `git push`). Vercel menerbitkan ulang otomatis.
Salinan lama yang sudah tersimpan di HP/laptop tetap memakai kata sandi lama sampai dimuat ulang saat online.

## Yang dibutuhkan dari client
1. Akun **GitHub** (nama pengguna saja) dan akun **Vercel** (mendaftar lewat *Continue with GitHub*).
2. Nama proyek yang diinginkan, misalnya `jalur-niaga-ypk` → alamat `https://jalur-niaga-ypk.vercel.app`.

**Jangan mengirim kata sandi atau token GitHub/Vercel.** Pengembang cukup diundang sebagai *Collaborator*, atau client mengunggah sendiri.

## Langkah A — client mengunggah sendiri
1. GitHub → **New repository** → `jalur-niaga-nusantara` (Private cukup). Jangan centang "Add a README".
2. **uploading an existing file** → seret **isi** folder `hosting` (`vercel.json`, `.gitignore`, `.gitattributes`, `README.md`, folder `public`) → **Commit changes**.
3. vercel.com → **Add New… → Project** → pilih repositori → **Import**.
   - Framework Preset: **Other** · Root Directory: `./` · Build Command & Install Command: **kosong** · Output Directory: `public` (sudah di `vercel.json`) → **Deploy**.
   - Bila repositori tidak muncul: **Adjust GitHub App Permissions** dan izinkan repositori itu.
4. Catat alamat `https://….vercel.app`, lalu kirim ke pengembang untuk membuat kartu QR:
   ```bash
   cd build
   node make-qr.mjs https://alamat-anda.vercel.app
   ```
   Hasilnya `dist/cetak/07-Kartu-QR.pdf` (8 kartu + 1 poster Ulasan) dan berkas QR di `hosting/qr/`. Setiap QR dibaca ulang dan dicocokkan dengan alamatnya.

## Langkah B — pengembang yang mengunggah
Setelah diundang sebagai Collaborator, dari folder `hosting`: `git add -A`, `git commit -m "Pembaruan media"`, lalu `git push`. Gunakan pesan commit biasa, **tanpa atribusi AI**.

## Memperbarui isi
`node build-app.mjs` → `node build-hosting.mjs` → `git commit` → `git push`. Bila ada kekeliruan: Vercel → **Deployments** → pilih versi sebelumnya → **Promote to Production**.

## Privasi dan keamanan
- Tidak ada akun siswa, pelacakan, analitik, atau data yang dikirim ke mana pun. Pengaturan suara dan pilihan mode hanya tersimpan di peramban masing-masing.
- Tidak diindeks mesin pencari (`noindex`, `robots.txt`). Header keamanan dan kebijakan konten (CSP) diatur di `vercel.json`; kamera, mikrofon, dan lokasi dinonaktifkan.
- Data: batas wilayah BIG (via GeoJSON berlisensi MIT), garis pantai dan relief Natural Earth (domain publik). Musik dan efek suara dibangkitkan program.
- Paket gratis Vercel (*Hobby*) setahu pengembang diperuntukkan bagi penggunaan non-komersial; client sebaiknya membaca ketentuannya.

## Uji setelah live (5 menit)
- [ ] Membuka `https://alamat/` di tab baru menampilkan jendela sambutan; peta di belakangnya terburamkan.
- [ ] **Siswa** langsung masuk: tidak ada tab Misi, kolom defisit, rute, atau timer.
- [ ] **Guru** meminta kata sandi; sandi salah ditolak; sandi benar membuka Misi, Uji Jawaban, rute, dan timer.
- [ ] Tombol **Mode** di header: Guru → Siswa langsung; Siswa → Guru meminta kata sandi lagi.
- [ ] `https://alamat/ulasan` menampilkan jalur kapal dan kebutuhan, tanpa kunci.
- [ ] Setelah dibuka sekali, matikan sinyal lalu buka ulang: media tetap terbuka.
- [ ] Menu ⋮ Chrome HP → **Tambahkan ke layar utama** menampilkan ikon kompas.
