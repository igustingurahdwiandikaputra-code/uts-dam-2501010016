# UTS Data Acquisition & Management — Daya Tarik Wisata Bali dan Kondisi Cuaca

## Deskripsi
Proyek ini merupakan tugas UTS mata kuliah **Data Acquisition & Management**. Proyek mengakuisisi, memeriksa, mengolah, dan menggabungkan data daya tarik wisata di Provinsi Bali dengan data kondisi cuaca tahun 2026 serta profil wilayah kabupaten/kota.

Tujuan proyek adalah membangun dataset gabungan yang dapat digunakan sebagai dasar eksplorasi dan analisis daya tarik wisata Bali dalam konteks wilayah dan kondisi cuaca.

## Tujuan
- Mengakuisisi dataset daya tarik wisata dari sumber resmi.
- Mengumpulkan data cuaca historis tahun 2026 menggunakan API.
- Mengambil data profil kabupaten/kota dari tabel web.
- Memeriksa sumber, lisensi, ketentuan akses, dan provenance.
- Mengidentifikasi potensi data pribadi (PII) dan menerapkan minimalisasi data.
- Menggabungkan sumber data serta memeriksa konsistensi dan duplikasi.
- Menyimpan dataset hasil, metadata, dan bukti proses akuisisi.

## Sumber Data
| Sumber | Metode | Isi | Lisensi/ketentuan yang dicatat dalam notebook |
|---|---|---|---|
| Dinas Pariwisata Provinsi Bali / Bali Satu Data — Buku Data DTW 2024 | Unduhan berkas XLSX | Daftar daya tarik wisata per kabupaten/kota | Data publik pemerintah daerah; digunakan untuk tujuan akademik dengan atribusi sumber |
| Open-Meteo Historical Weather API | API | Data cuaca harian tahun 2026 | CC BY 4.0; atribusi: Weather data by Open-Meteo.com |
| Wikipedia — List of districts in Bali | Web scraping tabel HTML | Profil wilayah kabupaten/kota | CC BY-SA 4.0; pemeriksaan robots.txt dicatat dalam notebook |

Sumber dan URL rinci, tanggal akses, lisensi, checksum, serta lokasi berkas dicatat pada `metadata/provenance.csv` (dibuat saat notebook dijalankan).

## Cakupan
- **Wilayah:** 9 kabupaten/kota di Provinsi Bali.
- **Data daya tarik wisata:** data tahun 2024, berdasarkan data/penetapan yang tercantum pada sumber.
- **Data cuaca:** 1 Januari 2026 hingga tanggal akhir pengambilan yang ditentukan notebook (berdasarkan tanggal eksekusi dan jeda pemrosesan API).
- **Data cuaca:** suhu rata-rata, suhu maksimum, suhu minimum, curah hujan, kecepatan angin maksimum, serta kelembapan jika tersedia.
- **Data profil wilayah:** tabel kabupaten/kota yang diambil dari halaman Wikipedia.

## Alur Akuisisi dan Pengolahan
1. Menyiapkan library, konfigurasi request, dan struktur direktori.
2. Membaca workbook daya tarik wisata dan mengekstrak data dari sheet kabupaten/kota.
3. Mengidentifikasi karakteristik dataset, cakupan, kolom, nilai kosong, dan ID.
4. Mencatat provenance dan estimasi kebutuhan penyimpanan.
5. Memeriksa sumber, lisensi, terms, dan bukti akses.
6. Mengidentifikasi PII/quasi-identifier, melakukan minimalisasi kolom, dan memeriksa data yang tersisa.
7. Mengambil data cuaca historis per kabupaten/kota melalui Open-Meteo dan menyimpan respons mentah.
8. Mengambil tabel profil wilayah dari Wikipedia setelah pemeriksaan `robots.txt`.
9. Merangkum data cuaca per kabupaten/kota.
10. Menggabungkan data wisata, cuaca, dan profil wilayah menggunakan kunci `kabupaten_kota`.
11. Memvalidasi hasil penggabungan, ID, baris, dan duplikasi.
12. Menyimpan dataset gabungan, metadata, checklist etika, serta paket hasil.

## Struktur Folder
Struktur berikut merupakan struktur yang disiapkan/dihasilkan notebook. Sebagian berkas dan folder bukti dapat muncul setelah notebook dijalankan.

```text
.
├── data/
│   ├── raw/
│   │   ├── dataset daya tarik wisata (XLSX)
│   │   ├── JSON cuaca Open-Meteo per kabupaten/kota
│   │   └── HTML mentah Wikipedia
│   └── processed/
│       ├── cuaca_harian_bali_2026.csv
│       └── gabungan_wisata_cuaca_profil_bali.csv
├── docs/
│   ├── data_ethics_checklist.csv
│   └── data_ethics_checklist.md
├── evidence/
│   └── bukti pemeriksaan lisensi dan robots.txt
├── metadata/
│   ├── provenance.csv
│   └── gabungan_metadata.json
└── UTS_DAYATARIKWISATA.ipynb
```

## Hasil
Hasil utama yang disimpan notebook:
- `data/processed/cuaca_harian_bali_2026.csv` — data cuaca harian yang telah dirapikan.
- `data/processed/gabungan_wisata_cuaca_profil_bali.csv` — dataset gabungan daya tarik wisata, ringkasan cuaca, dan profil wilayah.
- `metadata/provenance.csv` — catatan sumber, metode akuisisi, tanggal akses, lisensi, lokasi berkas, dan MD5.
- `metadata/gabungan_metadata.json` — metadata dataset gabungan, informasi penggabungan, dan catatan keterbatasan.
- `docs/data_ethics_checklist.csv` dan `docs/data_ethics_checklist.md` — checklist etika akuisisi data.

## Metode Penggabungan
Data daya tarik wisata digabungkan dengan ringkasan cuaca dan profil wilayah menggunakan kolom `kabupaten_kota`. Penggabungan dilakukan dengan metode **left join** dan validasi relasi many-to-one (`m:1`), untuk menjaga jumlah baris data wisata serta menghindari duplikasi akibat penggabungan.

Cuaca diringkas pada tingkat kabupaten/kota. Oleh karena itu, nilai cuaca yang terhubung ke objek wisata merupakan gambaran wilayah, bukan kondisi cuaca spesifik pada titik lokasi masing-masing objek.

## Etika, Lisensi, dan Keterbatasan
- URL sumber dan ketentuan lisensi dicatat.
- Pemeriksaan `robots.txt` dilakukan sebelum scraping Wikipedia.
- Request menggunakan timeout, User-Agent, jeda, dan mekanisme retry.
- PII diperiksa; data diminimalkan sesuai kebutuhan analisis.
- Data cuaca Open-Meteo merupakan reanalisis berbasis grid, bukan pengukuran langsung dari stasiun pada setiap objek wisata.
- Koordinat cuaca mewakili kabupaten/kota, bukan koordinat individual objek wisata.
- Dataset daya tarik wisata bersumber dari data tahun 2024, sedangkan data cuaca menggunakan periode tahun 2026; kedua data tidak menggambarkan periode pengamatan yang sama.
- Data hasil digunakan untuk kepentingan akademik. Atribusi sumber wajib dipertahankan sesuai ketentuan sumber masing-masing.

## Cara Menjalankan
Notebook dirancang untuk dijalankan melalui Google Colab.

1. Buka notebook di Google Colab.
2. Siapkan dataset XLSX daya tarik wisata sesuai nama berkas yang diharapkan notebook, lalu unggah saat diminta atau letakkan pada folder `data/raw/`.
3. Jalankan sel notebook secara berurutan, dari persiapan hingga pengemasan.
4. Pastikan koneksi internet tersedia untuk API dan proses pengambilan tabel web.
5. Periksa keluaran, berkas hasil, metadata, dan checklist.

### Library yang digunakan
- Python
- pandas
- NumPy
- requests
- `openpyxl` (untuk pembacaan Excel)
- `lxml` (dapat diperlukan oleh `pandas.read_html`)

## Pengiriman ke GitHub
Notebook menyediakan sel tambahan untuk push hasil ke GitHub melalui Google Colab. Sel tersebut memerlukan repository GitHub dan token yang disimpan pada Colab Secrets dengan nama `GITHUB_TOKEN`.

**Jangan menuliskan token GitHub langsung ke notebook, README, atau repository.** Sel push merupakan langkah terpisah setelah proses notebook utama selesai.

## Atribusi
- Data daya tarik wisata: Dinas Pariwisata Provinsi Bali / Bali Satu Data — Buku Data Daya Tarik Wisata 2024.
- Data cuaca: Weather data by Open-Meteo.com (CC BY 4.0).
- Profil wilayah: Wikipedia, *List of districts in Bali* (CC BY-SA 4.0).

---
**UTS Data Acquisition & Management — INSTIKI**
