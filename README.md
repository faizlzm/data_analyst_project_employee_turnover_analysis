# Employee Turnover Analysis Dashboard

Analisis turnover karyawan dari 3.000 data kepegawaian menggunakan Microsoft Excel (data cleaning, feature engineering, pivot table, dan dashboard) untuk menemukan pola keluar-kerja dan menyusun rekomendasi strategi retensi berbasis data.

---

## Daftar Isi

- [Latar Belakang](#latar-belakang)
- [Tujuan Analisis](#tujuan-analisis)
- [Struktur Repositori](#struktur-repositori)
- [Dataset](#dataset)
- [Metodologi](#metodologi)
- [Temuan Utama](#temuan-utama)
- [Rekomendasi](#rekomendasi)
- [Keterbatasan](#keterbatasan)
- [Cara Menggunakan Repositori Ini](#cara-menggunakan-repositori-ini)
- [Penulis](#penulis)

---

## Latar Belakang

Catatan perusahaan berisi **3.000 karyawan**, dan **51,1% (1.533 karyawan)** di antaranya telah keluar dari perusahaan. Angka ini berdampak pada produktivitas, biaya rekrutmen, dan stabilitas operasional. Diperlukan analisis untuk memahami pola dan faktor utama di balik fenomena tersebut.

## Tujuan Analisis

1. Mengidentifikasi segmen atau kelompok karyawan dengan tingkat turnover tertinggi.
2. Menemukan faktor yang paling berkorelasi dengan keputusan keluar-kerja.
3. Menghasilkan rekomendasi berbasis data untuk strategi retensi yang lebih tepat sasaran.

## Struktur Repositori

```text
data_analyst_project_employee_turnover_analysis/
├── analysis_process/
│   └── Assignment Case Study Excel.xlsx   # Workbook proses analisis
├── dataset/
│   └── employee_data.csv                  # Dataset mentah (3.000 baris, 26 kolom)
├── report/
│   └── Analisis EmployeeHR Dataset.pdf    # Laporan presentasi hasil analisis
└── README.md
```

Isi workbook `Assignment Case Study Excel.xlsx`:

| Sheet | Fungsi |
| --- | --- |
| `Data_Master` | Salinan data mentah sebelum diolah |
| `Cleaning` | Data hasil pembersihan beserta kolom turunan |
| `Pivot` | Pivot table turnover per kategori, departemen, gender, lama kerja, performance score, tipe karyawan, dan kelompok usia |
| `Dashboard` | Visualisasi ringkasan hasil analisis |
| `Outlier` | Pengecekan outlier dengan metode IQR |

## Dataset

Dataset `employee_data.csv` bersumber dari Kaggle dan berisi data kepegawaian sebuah perusahaan. Tanggal mulai kerja dalam data berada pada rentang Agustus 2018 sampai Agustus 2023.

- **Jumlah baris:** 3.000 karyawan
- **Kolom awal:** 26
- **Kolom setelah cleaning:** 32

Kelompok fitur penting:

| Kelompok | Kolom |
| --- | --- |
| Identitas dan organisasi | `EmpID`, `FirstName`, `LastName`, `DepartmentType`, `Division`, `Supervisor`, `BusinessUnit`, `State`, `LocationCode` |
| Status ketenagakerjaan | `EmployeeStatus`, `EmployeeType`, `StartDate`, `ExitDate`, `TerminationType`, `PayZone`, `EmployeeClassificationType` |
| Demografi dan kinerja | `DOB`, `GenderCode`, `RaceDesc`, `MaritalDesc`, `Performance Score`, `Current Employee Rating` |

> Dataset memuat nama dan email karyawan. Gunakan hanya untuk keperluan pembelajaran dan jangan disebarkan sebagai data personal.

## Metodologi

### 1. Data Preparation

| Masalah | Penanganan |
| --- | --- |
| 1.467 dari 3.000 baris (48,9%) kosong pada `ExitDate` dan `TerminationDescription` | Secara logika berarti karyawan masih aktif. Baris tidak dihapus dan tidak diisi nilai buatan. |
| 991 baris berstatus `Active` tetapi memiliki `ExitDate` dan `TerminationType` | Dibuat kolom baru `IsActive` sebagai indikator status yang lebih andal dibanding `EmployeeStatus`. |
| Duplikasi dan outlier | Tidak ditemukan duplikat maupun outlier pada `Age` sehingga tidak perlu proses lanjutan. |
| Format data | Tipe data distandarkan, spasi berlebih dihapus dengan `TRIM`, dan format tanggal diseragamkan. |

### 2. Feature Engineering

Kolom turunan yang ditambahkan:

| Kolom | Keterangan |
| --- | --- |
| `IsActive` | `TRUE` jika karyawan masih aktif, `FALSE` jika sudah keluar |
| `LeaveCategory` | Kategori status: `Aktif`, `Resign`, `Dipecat`, `Pensiun` |
| `Age` dan `KategoriUsia` | Usia karyawan dan pengelompokannya per dekade (20-29, 30-39, dst.) |
| `LamaKerja` dan `KategoriLamaKerja` | Masa kerja dalam tahun dan pengelompokannya per tahun (0-1, 1-2, dst.) |

Usia dan masa kerja dihitung dengan tanggal acuan **30 Desember 2023** (asumsi tahun terbaru di dataset).

Distribusi `LeaveCategory`:

| Kategori | Jumlah |
| --- | ---: |
| Aktif | 1.467 |
| Resign | 768 |
| Dipecat | 388 |
| Pensiun | 377 |
| **Total** | **3.000** |

### 3. Analisis

Turnover rate dihitung sebagai **jumlah karyawan yang keluar dibagi total karyawan** pada tiap segmen, kemudian dibandingkan antar segmen menggunakan pivot table dan chart di Excel. Segmen yang dianalisis: lama kerja, departemen, usia, gender, tipe karyawan, dan performance score.

## Temuan Utama

**Gambaran umum:** dari 3.000 karyawan, turnover rate keseluruhan adalah **51,1%**.

### Lama masa kerja adalah faktor yang paling berpengaruh

| Masa kerja | Turnover rate |
| --- | ---: |
| 0–1 tahun | 70,36% |
| 1–2 tahun | 57,89% |
| 2–3 tahun | 46,11% |
| 3–4 tahun | 28,61% |
| 4–5 tahun | 12,16% |
| 5–6 tahun | 0,00% (hanya 3 karyawan, belum ada yang keluar) |

Turnover menurun tajam seiring bertambahnya masa kerja. Karyawan dengan masa kerja 0–1 tahun mencatat 724 dari 1.533 total karyawan yang keluar, sehingga tahun pertama adalah periode paling kritis.

### Departemen Production menyumbang volume turnover terbesar

| Departemen | Karyawan | Keluar | Turnover rate | Kontribusi ke total keluar |
| --- | ---: | ---: | ---: | ---: |
| Production | 2.020 | 1.014 | 50,20% | 66,1% |
| IT/IS | 430 | 224 | 52,09% | 14,6% |
| Sales | 331 | 164 | 49,55% | 10,7% |
| Software Engineering | 115 | 64 | 55,65% | 4,2% |
| Admin Offices | 80 | 48 | 60,00% | 3,1% |
| Executive Office | 24 | 19 | 79,17% | 1,2% |

- Executive Office (79,2%) dan Admin Offices (60,0%) memiliki rate tertinggi, tetapi jumlah karyawannya kecil (24 dan 80 orang) sehingga rentan bias sampel kecil.
- Tiga departemen (Production, IT/IS, Sales) menyumbang **91,5%** dari seluruh karyawan yang keluar.

### Faktor demografi dan kinerja tidak menunjukkan perbedaan berarti

| Faktor | Rentang turnover rate |
| --- | --- |
| Gender | Female 51,43%, Male 50,68% |
| Kelompok usia | 48,09% – 53,28% |
| Tipe karyawan | Contract 50,99%, Full-Time 51,45%, Part-Time 50,84% |
| Performance score | 50,91% (Fully Meets) – 52,69% (PIP) |

Rate pada semua segmen ini berada di sekitar rata-rata keseluruhan (51,1%), sehingga faktor-faktor tersebut tidak bisa dijadikan dasar utama pengambilan keputusan.

## Rekomendasi

1. **Fokuskan retensi di tahun pertama.** Alokasikan anggaran dan program onboarding intensif serta mentoring pada 12 bulan pertama masa kerja.
2. **Prioritaskan departemen Production untuk kebijakan taktis.** Production menyumbang 66,1% dari total karyawan yang keluar, sehingga perbaikan kecil di sini berdampak jauh lebih besar pada angka total dibanding unit kecil bertingkat tinggi.
3. **Hindari program retensi berbasis segmentasi demografi atau kinerja.** Usia, gender, tipe karyawan, dan performance score tidak menunjukkan perbedaan turnover yang berarti, sehingga program berbasis segmen tersebut berpotensi tidak efektif.

## Keterbatasan

- Analisis bersifat **deskriptif** (perbandingan rate antar segmen). Tidak dilakukan uji statistik formal maupun pemodelan prediktif, sehingga temuan menunjukkan pola, bukan hubungan sebab-akibat.
- Segmen kecil (Executive Office, Admin Offices, masa kerja 5–6 tahun) memiliki sampel terlalu sedikit untuk disimpulkan secara kuat.
- Faktor yang belum dianalisis mendalam, misalnya `Supervisor`, `BusinessUnit`, `PayZone`, dan `Current Employee Rating`, dapat menjadi pengembangan selanjutnya.

## Penulis

**Ahmad Faiz Ali Azmi**
Lulusan Teknik Informatika, Universitas Brawijaya. Peserta Offline Bootcamp Data Analyst Batch 4 di dibimbing.id.

GitHub: [@faizlzm](https://github.com/faizlzm)
