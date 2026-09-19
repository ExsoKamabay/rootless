# rootless

Tempat penyimpanan image rootfs Linux untuk aplikasi
[dracxterm](https://github.com/ExsoKamabay/dxterm). Repositori ini tidak berisi
kode program. Isinya hanya arsip rootfs yang diunduh aplikasi ketika pengguna
memilih distro, ditambah daftar digest untuk memeriksanya.

dracxterm membaca katalog `app/src/main/assets/rootfsURLS.json`. Tiap entri di
katalog itu menunjuk ke salah satu file di sini dan membawa digest SHA-256 yang
dipin. Aplikasi menolak menyerahkan arsip ke extractor kalau digest file yang
selesai diunduh tidak sama persis.

## Letak file

Arsipnya ada di [Releases](https://github.com/ExsoKamabay/rootless/releases),
tidak di dalam pohon Git. GitHub menolak file di atas 100 MB lewat push biasa,
dan tiga dari lima image di sini melewati batas itu.

Pola URL unduhannya:

```
https://github.com/ExsoKamabay/rootless/releases/download/<tag>/<nama-file>
```

`<tag>` adalah `rootfs-20260913` untuk image arm64 dan `rootfs-20260920` untuk
image x86_64.

## Isi rilis rootfs-20260913

| File | Ukuran | ABI | Asal |
|---|---|---|---|
| `debian-trixie-aarch64-pd-v4.37.0.tar.xz` | 35 MB | arm64-v8a | build proot-distro Debian 13 (trixie) |
| `kali-nethunter-rootfs-nano-arm64.tar.xz` | 198 MB | arm64-v8a | Kali NetHunter 2026.2, varian nano |
| `kali-nethunter-rootfs-minimal-arm64.tar.xz` | 137 MB | arm64-v8a | Kali NetHunter 2026.2, varian minimal |
| `kali-nethunter-rootfs-full-arm64.tar.xz` | 1,8 GB | arm64-v8a | Kali NetHunter 2026.2, varian full |

Keempatnya untuk arm64-v8a, ABI yang dipakai ponsel.

## Isi rilis rootfs-20260920

| File | Ukuran | ABI | Asal |
|---|---|---|---|
| `debian-trixie-amd64-lxc-20260919.tar.xz` | 93 MB | x86_64 | Debian 13 (trixie) amd64, build `20260919_05:24` dari images.linuxcontainers.org |
| `debian-trixie-amd64-lxc-20260919.SHA256SUMS` | 743 B | - | `SHA256SUMS` asli build itu |
| `debian-trixie-amd64-lxc-20260919.SHA256SUMS.asc` | 833 B | - | tanda tangan GPG linuxcontainers untuk berkas di atas |

dracxterm berjalan di x86_64 sejak versi 1.0.5, untuk emulator Android,
WayDroid, dan ChromeOS. Di katalog aplikasi entri ini masih ditandai uji coba.

## Memeriksa unduhan

Digest kelima image ada di [`SHA256SUMS`](SHA256SUMS). Setelah mengunduh,
jalankan di direktori yang sama:

```bash
sha256sum -c SHA256SUMS
```

Tiga digest Kali bisa dicocokkan ulang dengan berkas yang diterbitkan Kali
sendiri di
`https://kali.download/nethunter-images/kali-2026.2/rootfs/SHA256SUMS`.

Digest image x86_64 tetap bisa ditelusuri ke linuxcontainers walaupun build
aslinya sudah dirotasi keluar dari server mereka. Dua berkas pendampingnya
disalin apa adanya dari direktori build itu, hanya namanya yang diganti, dan
tanda tangan GPG tidak bergantung pada nama berkas:

```bash
gpg --keyserver hkps://keyserver.ubuntu.com --recv-keys E7FB0CAEC8173D669066514CBAEFF88C22F6E216
gpg --verify debian-trixie-amd64-lxc-20260919.SHA256SUMS.asc debian-trixie-amd64-lxc-20260919.SHA256SUMS
grep ' rootfs.tar.xz$' debian-trixie-amd64-lxc-20260919.SHA256SUMS
```

Kunci itu milik "LXC pre-built images". Baris `rootfs.tar.xz` yang tercetak
memuat digest yang sama dengan `debian-trixie-amd64-lxc-20260919.tar.xz`.

Digest Debian arm64 tidak bisa dicocokkan ke sumber lain lagi. File itu dipin
dracxterm sejak versi 1.0.0, waktu itu diambil dari `easycli.sh`. Operator situs
tersebut menghentikan hosting, dan URL lamanya sekarang menjawab 302 ke `/`
dengan pesan 43 byte. Byte yang sama masih tersimpan di sini, dan digestnya
sama dengan yang dipin sejak awal.

## Kenapa repositori ini ada

Dua sumber upstream dracxterm bermasalah dengan cara yang berbeda.

`easycli.sh` mati untuk selamanya, dan itu memutus entri Debian. Penggantinya,
`images.linuxcontainers.org`, hanya menyimpan build bertanggal sekitar tiga
hari, jadi URL yang dipin berhenti resolve begitu build itu dirotasi keluar.
Build `20260904_05:24` yang dipakai dracxterm 1.0.1 sampai 1.0.3 sudah 404.
Entri x86_64 kena hal yang sama sebelum sempat dirilis: build amd64
`20260913_05:24` yang dipakai selama pengembangan 1.0.5 sudah 404 pada
2026-09-20.

Mirror ini menghapus kedua masalah tersebut. Untuk image arm64 byte yang
disajikan tetap sama, jadi digest yang dipin di dalam APK tidak berubah. Image
x86_64 memakai build yang lebih baru, dan digestnya dipin mulai dracxterm 1.0.5.

## Lisensi

`LICENSE` di repositori ini berlaku untuk isi repositorinya sendiri, yaitu teks
dan daftar digest. Arsip rootfs di Releases dibangun dari Debian dan Kali
Linux, dan paket di dalamnya tunduk pada lisensinya masing-masing. Lihat
`/usr/share/doc` di dalam rootfs yang terpasang untuk salinan lisensi tiap
paket.
