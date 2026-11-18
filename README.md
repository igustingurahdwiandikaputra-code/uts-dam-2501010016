# UTS Data Acquisition & Management - Daya Tarik Wisata Bali

I Gusti Ngurah Dwi Andika Putra (2501010016) - Kelas E, Informatika, INSTIKI

Akuisisi tiga sumber data lalu digabung per kabupaten/kota:
1. **Berkas**: daftar 108 objek wisata Bali (`data/raw/dataset_wisata_cuaca_bali_2026.xlsx`)
2. **API**: cuaca harian 2026 dari Open-Meteo (`data/raw/openmeteo_*.json`)
3. **Scraping**: profil kabupaten/kota dari Wikipedia (`data/raw/wikipedia_*.html`)

## Struktur
- `*.ipynb` : notebook (semua sel sudah dijalankan)
- `data/raw/` : data mentah, tidak diubah
- `data/processed/` : hasil olahan dan penggabungan
- `metadata/` : `provenance.csv` (URL, tanggal akses, lisensi, MD5) dan metadata hasil gabungan
- `docs/` : Data Ethics Checklist
- `evidence/` : bukti robots.txt dan lisensi

## Atribusi dan lisensi
- Cuaca: Weather data by Open-Meteo.com (CC BY 4.0)
- Profil wilayah: Wikipedia "List of districts in Bali" (CC BY-SA 4.0)
- Daftar objek wisata: merujuk data.go.id (Pemprov Bali); lisensi tidak dicantumkan pada halaman dataset
- Dipakai untuk tujuan akademik non-komersial.
