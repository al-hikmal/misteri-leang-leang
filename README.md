# Misteri Leang-Leang

Game petualangan 3D berbasis browser yang mengajak pemain menjelajahi **Taman Prasejarah Leang-Leang, Kabupaten Maros, Sulawesi Selatan**, mulai dari gerbang, jalur karst, hingga gua-gua purba, sambil belajar tentang karst, lukisan gua, dan peninggalan manusia purba.

Proyek akhir mata kuliah **Grafika Komputer** (materi Three.js).

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Draft v1.0](https://img.shields.io/badge/Status-Draft%20v1.0-orange.svg)

> **Status:** Draft v1.0 untuk laporan sementara. Game masih dalam tahap pengembangan.

## Cerita Singkat

Kamu adalah seorang arkeolog muda yang menerima catatan tua tentang sebuah pesan leluhur. Pesan itu terkunci oleh **empat kepingan pengetahuan** yang tersebar di Leang-Leang. Untuk mendapatkannya, kamu harus membaca papan informasi, menjawab pertanyaan, dan menjelajahi kawasan sampai ke ruang terdalam gua.

## Fitur

- Dunia 3D orang pertama yang dibangun sepenuhnya dengan kode (tanpa file model atau gambar eksternal)
- 5 area berurutan: Gerbang, Jalur Karst, Leang Pettae, Leang Pettakere, dan Gua Terdalam
- Sistem misi bertahap dengan "segel cahaya" yang terbuka setelah pemain menjawab kuis
- Kuis pilihan ganda di setiap stasiun, lengkap dengan penjelasan setelah menjawab
- Teka-teki mengumpulkan 3 bola cahaya (fragmen lukisan) di Leang Pettakere
- Lukisan dinding prasejarah (cap tangan dan babi rusa) yang digambar secara prosedural
- Transisi suasana dari luar ruangan ke dalam gua (kabut, warna langit, dan pencahayaan berubah)
- Narator teks dan opsi narator suara berbahasa Indonesia
- Kontrol keyboard, mouse, dan layar sentuh

## Cara Menjalankan

Tidak perlu instalasi atau build.

1. Unduh file `Misteri Leang-Leang.html`.
2. Buka file tersebut di browser modern (Chrome, Edge, Firefox, atau Safari).
3. Pastikan perangkat **terhubung ke internet**, karena Three.js dan font dimuat dari CDN.

> Jika ingin dijalankan offline, unduh `three.min.js` (r128) dan ganti alamat `<script src="...">` di bagian bawah file dengan jalur lokal.

## Kontrol

| Aksi | Keyboard / Mouse | Layar Sentuh |
|---|---|---|
| Bergerak | `W` `A` `S` `D` atau tombol panah | Geser di sisi kiri layar |
| Melihat sekitar | Tahan klik dan seret mouse | Geser di sisi kanan layar |
| Interaksi | `E` | Tombol **Interaksi** yang muncul saat dekat titik emas |
| Narator suara | Tombol "Narator suara" di kiri atas | Sama |

## Alur Permainan

Setiap area punya satu stasiun (titik emas yang melayang). Mendekati stasiun, lalu menekan `E` akan menampilkan narasi dan satu pertanyaan. Jawaban benar membuka area berikutnya.

| No | Stasiun | Lokasi | Topik | Hadiah |
|---|---|---|---|---|
| 1 | Papan informasi di gerbang | Gerbang | Lokasi Leang-Leang di Maros | Segel pertama terbuka |
| 2 | Jalur karst | Jalur Karst | Terbentuknya karst dari batu gamping | Kepingan Karst |
| 3 | Dinding lukisan | Leang Pettae | Cara membuat cap tangan prasejarah | Kepingan Lukisan |
| 4 | Fragmen dan alat batu | Leang Pettakere | Peninggalan alat batu | Kepingan Alat Batu |
| 5 | Altar | Gua Terdalam (area fiksi) | Sikap menjaga situs prasejarah | Kepingan Pesan |

Di Leang Pettakere, pemain harus lebih dulu mengumpulkan **3 bola cahaya** sebelum bisa berinteraksi dengan stasiun. Setelah keempat kepingan terkumpul, layar akhir "Misteri terpecahkan" ditampilkan beserta ringkasan pelajaran.

> Gua Terdalam dengan altar dan kristal adalah **elemen fiksi** untuk kebutuhan cerita, bukan lokasi nyata.

## Teknologi

- **Three.js r128** (dimuat dari cdnjs)
- JavaScript murni, HTML, dan CSS dalam satu file
- Canvas 2D untuk membuat tekstur secara prosedural
- Web Speech API (`speechSynthesis`) untuk narator suara
- Pointer Events untuk kontrol mouse dan sentuh

## Konsep Grafika Komputer yang Dipakai

- **Scene graph, kamera perspektif, dan renderer WebGL**
- **Pencahayaan:** `HemisphereLight`, `DirectionalLight` (matahari), `SpotLight` (senter di kamera), dan `PointLight` (obor dan kristal)
- **Material dan tekstur:** `MeshStandardMaterial`, `MeshBasicMaterial`, dan `CanvasTexture` dengan pengulangan tekstur (repeat)
- **Geometri prosedural:** bukit karst (`ConeGeometry`), stalaktit, kristal, pohon, gerbang, dan altar
- **Fog dan interpolasi warna** untuk transisi dari luar ke dalam gua
- **Transformasi dan animasi:** objek melayang berputar, kedipan obor, dan efek goyang kamera saat berjalan
- **Kontrol orang pertama:** konversi input menjadi gerak berdasarkan arah pandang (yaw dan pitch)
- **Deteksi jarak** untuk interaksi dan pembatas area gerak

## Struktur Kode

Seluruh logika ada di satu tag `<script>`, dengan urutan:

| Bagian | Isi |
|---|---|
| Scene dan renderer | `scene`, `cam`, `ren`, cahaya dasar |
| Tekstur prosedural | `tex()`, `noise()`, `paint()` (lukisan dinding) |
| Dunia | Tanah, jalur, gua, bukit karst, pohon, gerbang, stalaktit, kristal, altar, obor |
| Data level | `ST` (stasiun, narasi, kuis), `LIM` (batas gerak tiap tahap), `OB` (teks misi) |
| Objek interaktif | Penanda emas, segel cahaya (`bars`), bola fragmen (`orbs`) |
| Status dan dialog | `stage`, `gems`, `interact()`, `quiz()`, `win()` |
| Input | Keyboard, pointer (mouse dan sentuh) |
| Game loop | Gerak, batas area, transisi cahaya, animasi, render |

## Kustomisasi

**Mengubah isi pertanyaan atau narasi:** edit array `ST`. Tiap entri memiliki:

```js
{
  p: [x, z],            // posisi stasiun
  t: 'Judul',
  n: 'Narasi',
  q: 'Pertanyaan',
  o: ['Opsi A', 'Opsi B', 'Opsi C'],
  a: 0,                 // indeks jawaban benar
  f: 'Penjelasan setelah menjawab'
}
```

**Menambah level baru:**

1. Tambahkan satu entri di `ST`.
2. Tambahkan satu angka batas jalan di `LIM`.
3. Tambahkan satu teks misi di `OB`.
4. Bangun geometri areanya, lalu sesuaikan `zoneOf()` agar nama area tampil.

## Catatan Konten Edukasi

Fakta pada narasi dan kuis (lokasi, proses terbentuknya karst, teknik cap tangan, dan perkiraan usia lukisan) disusun secara ringkas untuk keperluan edukasi. Sebelum dikumpulkan atau dipublikasikan, sebaiknya diverifikasi ulang dengan sumber resmi, misalnya Balai Pelestarian Kebudayaan dan Dinas Pariwisata Kabupaten Maros.

## Keterbatasan

- Model dan tekstur bersifat prosedural, sehingga tampilannya sederhana dibanding game dengan aset 3D profesional
- Belum ada tabrakan dengan objek (hanya pembatas area gerak)
- Belum ada penyimpanan progres, permainan dimulai dari awal setiap dimuat ulang
- Narator suara bergantung pada dukungan suara bahasa Indonesia di perangkat

## Pengembangan Lanjutan

- Mengganti geometri prosedural dengan model GLTF dan tekstur foto
- Menambahkan musik latar dan efek suara langkah
- Menambahkan level baru, misalnya kawasan karst Rammang-Rammang
- Menyimpan progres dan skor
- Menambahkan peta mini dan koleksi "Kartu Pengetahuan"

## Tim Pengembang

**Nama tim: Leang Studio**

| Nama | NIM | Peran |
| AL Hikmal | 47226023 | Ketua & Pemrogram |
| Nur Auliya Zalzabila | 47226010 | Pencetus Ide 1: Cerita & Konten Edukasi |
| Aisyah Mutiara Arsyad Rahman | 47226007 | Pencetus Ide 2: Desain Visual & Gameplayi |
| Yulia Rasty Junidar | 47226025 | Pembuat Laporan |

**Mata kuliah:** Grafika Komputer
**Dosen pengampu:** Alvian Bastian, S.ST., M.Sc.  M. Rudini Kurniawan Amiruddin, S.ST.T., M.T.

## Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE).

Three.js dimuat dari CDN dan memiliki lisensi MIT miliknya sendiri.

## Referensi

- Dokumentasi Three.js: https://threejs.org/docs/
- Taman Prasejarah Leang-Leang, Kabupaten Maros (sumber resmi dari instansi terkait)
