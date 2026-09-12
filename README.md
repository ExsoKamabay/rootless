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
dan tiga dari empat image di sini melewati batas itu.

Pola URL unduhannya:

```
https://github.com/ExsoKamabay/rootless/releases/download/rootfs-20260913/<nama-file>
```

## Isi rilis rootfs-20260913

| File | Ukuran | ABI | Asal |
|---|---|---|---|
| `debian-trixie-aarch64-pd-v4.37.0.tar.xz` | 35 MB | arm64-v8a | build proot-distro Debian 13 (trixie) |
| `kali-nethunter-rootfs-nano-arm64.tar.xz` | 198 MB | arm64-v8a | Kali NetHunter 2026.2, varian nano |
| `kali-nethunter-rootfs-minimal-arm64.tar.xz` | 137 MB | arm64-v8a | Kali NetHunter 2026.2, varian minimal |
| `kali-nethunter-rootfs-full-arm64.tar.xz` | 1,8 GB | arm64-v8a | Kali NetHunter 2026.2, varian full |

Semuanya arm64. Itu satu-satunya ABI yang didukung dracxterm, karena biner
proot dan busybox bawaannya hanya dibangun untuk arm64-v8a.

## Memeriksa unduhan

Digest keempat file ada di [`SHA256SUMS`](SHA256SUMS). Setelah mengunduh,
jalankan di direktori yang sama:

```bash
sha256sum -c SHA256SUMS
```

Tiga digest Kali bisa dicocokkan ulang dengan berkas yang diterbitkan Kali
sendiri di
`https://kali.download/nethunter-images/kali-2026.2/rootfs/SHA256SUMS`.

Digest Debian tidak bisa dicocokkan ke sumber lain lagi. File itu dipin
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

Mirror ini menghapus kedua masalah tersebut. Byte yang disajikan tetap sama,
jadi digest yang dipin di dalam APK tidak berubah.

## Lisensi

`LICENSE` di repositori ini berlaku untuk isi repositorinya sendiri, yaitu teks
dan daftar digest. Arsip rootfs di Releases dibangun dari Debian dan Kali
Linux, dan paket di dalamnya tunduk pada lisensinya masing-masing. Lihat
`/usr/share/doc` di dalam rootfs yang terpasang untuk salinan lisensi tiap
paket.
