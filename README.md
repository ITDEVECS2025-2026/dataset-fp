# Studi Kasus: Rekayasa Edge AI untuk Pemantauan Lahan

Repository ini berisi dataset dan penugasan studi kasus **Edge Machine Learning** untuk deteksi anomali kondisi tanah. Peserta berperan sebagai Machine Learning Engineer yang merancang sistem pemantauan tanah berbasis sensor lingkungan, mulai dari analisis data, pelatihan model, hingga kompresi model agar dapat berjalan secara lokal (offline) pada mikrokontroler **ESP32**. Peserta merancang dan mengerjakan seluruh alur kerja EDA, pembersihan data, pemodelan, kuantisasi hingga deployment ESP32 sendiri berdasarkan dataset dan ketentuan di bawah.

## Dataset

**Nama file:** `dataset.csv`

Dataset berisi data mentah hasil pembacaan sensor tanah, dikumpulkan dari perangkat IoT yang tertera (diidentifikasi melalui `DeviceId`). Dataset tidak memiliki label apapun.

**Struktur kolom:**

| Kolom | Deskripsi |
|---|---|
| `Datetime` | Waktu pembacaan sensor |
| `DeviceId` | ID perangkat/sensor node |
| `N` | Kadar Nitrogen tanah |
| `P` | Kadar Fosfor tanah |
| `K` | Kadar Kalium tanah |
| `EC` | Electrical Conductivity (konduktivitas listrik tanah) |
| `pH` | Tingkat keasaman tanah |
| `Temperature_C` | Suhu (°C) |
| `Pressure_hPa` | Tekanan udara (hPa) |
| `Humidity_pct` | Kelembapan (%) |
| `Light_lux` | Intensitas cahaya (lux) |

Dataset terdiri dari 3000 baris data dan 9 parameter sensor lingkungan (di luar `Datetime` dan `DeviceId`), sesuai dengan ketentuan studi kasus (2000–4000 baris). Beberapa kolom memiliki nilai kosong (*missing values*), sehingga dataset perlu dianalisis dan dibersihkan terlebih dahulu sebelum digunakan untuk pemodelan.

## Studi Kasus

Bayangkan Saudara adalah seorang Machine Learning Engineer yang mendapatkan proyek strategis untuk merancang machine learning pada sistem pemantauan kondisi tanah. Model yang dibuat harus berfungsi dengan baik, efisien, dan mampu berjalan secara mandiri (offline) pada perangkat keras terbatas.

### 1. Deteksi Anomali Tanpa Panduan (Unsupervised Learning)

Sistem menerima data dari sensor yang membaca 9 parameter lingkungan (N, P, K, EC, pH, suhu, tekanan, kelembapan, cahaya).

- **Tujuan:** Model ML harus mampu mendeteksi kejanggalan (anomali) yang menandakan tanah sedang dalam kondisi tidak sehat.
- **Tantangan:** Data yang diberikan tidak memiliki label. Tidak ada petunjuk eksplisit mana data tanah yang "rusak". Model harus mempelajari pola normalnya sendiri dari data, lalu mendeteksi data yang menyimpang dari pola tersebut.

### 2. Fleksibilitas Pemrosesan dan Pemilihan Metode

Peserta bebas menentukan alur EDA, training, hingga proses quantization.

- **Pilihan algoritma:** Analisis dapat diselesaikan dengan pendekatan jaringan saraf tiruan skala kecil (Deep Learning) berupa **Autoencoder**. Jenis arsitektur Autoencoder dapat dipilih bebas sesuai pertimbangan masing-masing.
- **Pilihan metode kuantisasi:** Peserta dapat menentukan sendiri metode kuantisasi yang dianggap paling sesuai dengan karakteristik data dan tujuan yang direncanakan.

### 3. Batasan Ekstrem Perangkat Keras (Edge AI)

Ini adalah inti dari tantangan rekayasa. Model yang dibuat tidak akan dijalankan di server atau komputer, melainkan ditanamkan langsung ke dalam chip mikrokontroler **ESP32**, dan harus bekerja sepenuhnya secara lokal (offline).

### 4. Teknik Kompresi Tingkat Lanjut (Quantization)

Agar model dapat muat pada perangkat keras yang sangat terbatas, peserta wajib menggunakan kerangka kerja **TensorFlow Lite Micro**.

- **Optimasi wajib:** Menerapkan Quantization yakni teknik mengompresi beban komputasi model dari representasi angka desimal (floating point) menjadi angka bulat (integer) sederhana, sehingga program berjalan jauh lebih ringan tanpa mengorbankan akurasi secara drastis.

## Apa yang Harus Dilakukan Peserta

Menggunakan `dataset.csv`, peserta diharapkan mengerjakan alur berikut secara mandiri di **Google Colab**:

1. **Exploratory Data Analysis (EDA)** — memahami distribusi, korelasi antar parameter, dan pola data per `DeviceId`.
2. **Pembersihan/perbaikan dataset** — menangani *missing values* dan potensi outlier/noise pada data.
3. **Pemodelan** — merancang dan melatih model **Autoencoder** secara *unsupervised* untuk mempelajari pola normal data sensor tanah.
4. **Evaluasi model** — menentukan pendekatan untuk mengukur *reconstruction error* (misalnya MSE) dan menetapkan threshold anomali.
5. **Kuantisasi model** — mengonversi model terlatih ke **TensorFlow Lite** dan menerapkan **Full Integer INT8 Post-Training Quantization**.
6. **Persiapan deployment** — menyiapkan model hasil kuantisasi agar siap ditanamkan pada ESP32 untuk inferensi offline.

## Pengumpulan Tugas

Tugas dikumpulkan dalam **2 file utama**:

1. **Link Google Colab** — pastikan sudah dibagikan dengan akses **share for all**.
2. **Dokumen laporan (format PDF)** — menjelaskan secara mendetail dan berurutan:
   - Proses EDA dan temuan yang didapat dari data
   - Langkah yang dilakukan untuk memperbaiki/membersihkan dataset
   - Model yang digunakan beserta alasan pemilihan model tersebut
   - Proses training yang dilakukan
   - Hasil akhir model Edge ML yang telah dibuat (termasuk hasil kuantisasi dan kesiapan deployment)

Dokumen laporan disusun rapi dan jelas, dengan setiap langkah pengerjaan dijelaskan secara runtut.

## Catatan

- Dataset `dataset.csv` tidak memiliki label — seluruh pendekatan deteksi anomali bersifat *unsupervised*.
- Dataset mengandung *missing values* pada beberapa kolom sensor sehingga pembersihan data merupakan tahapan wajib sebelum training.
- Tidak ada notebook, kode, atau file model contoh yang disediakan pada repository ini — seluruh proses EDA, training, dan konversi model dirancang dan dikerjakan sendiri oleh peserta.

## Dataset Information

- **Nama dataset:** `dataset.csv`
- **Studi kasus:** Rekayasa Edge AI untuk Pemantauan Lahan
- **Tipe data:** Data sensor lingkungan tanah (mentah/belum bersih), tanpa label
- **Jumlah baris:** ±3000 baris
- **Jumlah parameter sensor:** 9 (N, P, K, EC, pH, Temperature, Pressure, Humidity, Light)
- **Metode ML yang disyaratkan:** Autoencoder (unsupervised learning)
- **Target deployment:** ESP32 (offline/lokal)
