# Koreksi Bias Data CHIRPS: Metode Linear Scaling (Time-Variant)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini memuat implementasi kode Python untuk melakukan **koreksi bias** pada data estimasi curah hujan satelit **CHIRPS v3.0**. Metode yang digunakan adalah **Linear Scaling (LS)** dengan pendekatan *Time-Variant* (Dinamis).

Berbeda dengan koreksi statis yang menggunakan satu faktor untuk seluruh periode, metode ini menghitung faktor skala yang unik untuk setiap bulan (*monthly time step*). Pendekatan ini efektif untuk mengoreksi bias yang fluktuatif antar musim (misal: satelit *overestimate* di musim hujan, tapi *underestimate* di musim kemarau).

## Metodologi

Metode Linear Scaling mengoreksi data satelit berdasarkan rasio antara rata-rata curah hujan observasi ($P_{obs}$) dan rata-rata curah hujan satelit ($P_{sat}$) pada langkah waktu tertentu.

Rumus koreksi didefinisikan sebagai:

$$P_{corr} = P_{sat} \times Scaling Factor$$

Dimana *Scaling Factor* (SF) dihitung setiap bulan ($t$) dengan persamaan:

$$SF_t = \frac{\bar{P}_{obs,t}}{\bar{P}_{sat,t}}$$

Keterangan:
- $P_{corr}$: Curah hujan terkoreksi.
- $\bar{P}_{obs,t}$: Rata-rata hujan di seluruh stasiun kalibrasi pada bulan $t$.
- $\bar{P}_{sat,t}$: Rata-rata hujan satelit di lokasi stasiun pada bulan $t$.

Jika pada bulan tertentu $SF > 1$, berarti satelit mengalami *underestimation* (perlu dinaikkan). Sebaliknya, jika $SF < 1$, satelit mengalami *overestimation* (perlu diturunkan).

## Prasyarat Instalasi

Script ini dikembangkan untuk lingkungan **Google Colab**. Berikut adalah dependensi pustaka yang digunakan:

### Daftar Pustaka
* **Geospasial:** `rasterio`, `geopandas`, `shapely`, `folium`
* **Analisis Data:** `pandas`, `numpy`, `scikit-learn`, `scipy`
* **Visualisasi:** `matplotlib`, `seaborn`
* **Utilitas:** `tqdm`

### Instalasi
```bash
pip install rasterio geopandas shapely matplotlib scikit-learn pandas numpy seaborn tqdm folium

```

## Struktur Direktori Data

Script ini membutuhkan struktur direktori data sebagai berikut di Google Drive:

```text
/Direktori_Project/
├── CHIRPS v3/                  # File raster .tif (input)
│   └── bias_corrected_scaling/ # (Output) Hasil koreksi
├── Batas Adm Jambi/            # Shapefile batas wilayah
├── Data Stasiun BWS/           # File .csv data curah hujan observasi
└── script_scaling.ipynb        # Script utama

```

## Cara Penggunaan

1. **Siapkan Data**: Pastikan data raster (CHIRPS) dan data stasiun (Time Series CSV) sudah siap.
2. **Setup Script**: Sesuaikan variabel `BASE_DIR`, `bias_stations` (kalibrasi), dan `validation_stations` (validasi).
3. **Jalankan Script**: Script akan memproses data bulan demi bulan:
* Menghitung rata-rata hujan stasiun vs satelit pada bulan tersebut.
* Menghitung rasio (Scaling Factor).
* Mengalikan raster CHIRPS dengan rasio tersebut.
* Melakukan validasi independen.


4. **Analisis Output**: Periksa file CSV hasil validasi dan grafik distribusi *Scaling Factor* untuk melihat karakteristik bias satelit.

## Contoh Hasil Validasi

Metode ini umumnya memberikan keseimbangan massa air yang baik. Berikut contoh ringkasan statistik:

| Stasiun Validasi | NSE (Raw) | NSE (Corrected) | RSR (Raw) | RSR (Corrected) |
| --- | --- | --- | --- | --- |
| **Muara Tembesi** | -0.45 | **0.58** | 1.20 | **0.65** |
| **Rantau Pandan** | 0.12 | **0.52** | 0.93 | **0.69** |

*Script juga menghasilkan histogram distribusi Scaling Factor untuk menganalisis kecenderungan bias satelit setiap bulan.*

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## 📄 Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Koreksi Bias CHIRPS Menggunakan Linear Scaling*. GitHub Repository.
