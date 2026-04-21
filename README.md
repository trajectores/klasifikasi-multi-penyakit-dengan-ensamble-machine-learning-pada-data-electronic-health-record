# Multi-Disease Classification EHR App
Aplikasi berbasis Machine Learning untuk memprediksi kategori penyakit utama pasien berdasarkan data **Electronic Health Record (EHR)**. Sistem ini mengklasifikasikan pasien ke dalam lima kelompok penyakit, yaitu **Cardiovascular, Infection_Sepsis, Neurological, Other_Metabolic_GI,** dan **Respiratory**. Model akhir yang digunakan adalah **LightGBM hasil hyperparameter tuning**, dengan mode fitur terbaik **score mode** dan rasio pembagian data **85:15**. :contentReference[oaicite:0]{index=0}

## 📌 Deskripsi
Proyek ini bertujuan untuk membangun sistem klasifikasi multi-kelas pada data rekam medis elektronik (*Electronic Health Record / EHR*) menggunakan pendekatan Machine Learning. Dataset yang digunakan berisi informasi pasien saat masuk rumah sakit atau unit perawatan, sehingga model dapat memprediksi kelompok penyakit utama berdasarkan kondisi awal pasien.

Berbeda dari klasifikasi biner sederhana, proyek ini menyelesaikan permasalahan **multiclass classification** dengan lima kelas target penyakit. Dalam prosesnya, proyek ini tidak hanya melakukan pelatihan model, tetapi juga mencakup tahapan lengkap mulai dari **data understanding**, **data cleaning**, **feature engineering**, **preprocessing**, **baseline modeling**, **hyperparameter tuning**, **evaluasi**, **visualisasi**, hingga **deployment berbasis web menggunakan Gradio**.

Beberapa faktor yang digunakan dalam prediksi antara lain:

- Usia pasien
- Jenis kelamin
- Etnis
- Diagnosis saat masuk
- Tinggi badan
- Berat badan saat masuk
- Berat badan saat keluar
- Jam masuk rumah sakit
- Sumber masuk rumah sakit
- Tipe unit perawatan
- Jam masuk unit
- Sumber masuk unit
- Nomor kunjungan unit
- Tipe rawat unit
- BMI
- Kelompok usia
- Status lansia
- Rasio berat terhadap tinggi badan
- Kategori BMI
- dan fitur klinis/administratif lainnya yang telah direkayasa dari dataset

Aplikasi ini dirancang menggunakan **Gradio** sehingga dapat dijalankan secara interaktif sebagai antarmuka prediksi berbasis web dari model Machine Learning yang telah dilatih.

## 🎯 Tujuan
- Membangun model klasifikasi multi-penyakit berbasis data EHR
- Membandingkan performa beberapa algoritma Machine Learning
- Melakukan tuning untuk mendapatkan model terbaik
- Mengimplementasikan model ke dalam aplikasi web interaktif
- Membantu proses prediksi kategori penyakit secara otomatis berdasarkan data pasien

## 🧠 Metode yang Digunakan

### 📊 Dataset
- **Nama dataset**: Electronic Health Record (EHR) Dataset
- **Jumlah data**: **1.447 data**
- **Jumlah fitur awal**: **29 kolom**
- **Target klasifikasi**: 5 kelas penyakit
  - Cardiovascular
  - Infection_Sepsis
  - Neurological
  - Other_Metabolic_GI
  - Respiratory

Setelah proses cleaning dan feature engineering, dataset siap modeling menjadi **25 kolom** termasuk target hasil pemetaan diagnosis.

### ⚙️ Tahapan Machine Learning

#### 1. Data Understanding
- Analisis bentuk dataset
- Pemeriksaan tipe data
- Identifikasi missing values
- Analisis jumlah diagnosis unik pada fitur `apacheadmissiondx`
- Pemeriksaan distribusi diagnosis awal pasien

#### 2. Data Cleaning & Feature Engineering
- Membersihkan kolom usia menjadi format numerik
- Mengambil jam dari variabel waktu masuk rumah sakit dan unit
- Membersihkan teks diagnosis
- Memetakan diagnosis menjadi 5 kelas target penyakit
- Menghitung **BMI**
- Membentuk **age_group**
- Membuat fitur **is_elderly**
- Membuat fitur **weight_height_ratio**
- Membuat fitur **bmi_category**
- Menghapus kolom yang berpotensi menjadi *data leakage* seperti data discharge tertentu dan ID unik

#### 3. Preprocessing
Proyek ini menggunakan dua mode fitur, yaitu:

- **Strict mode**  
  Menggunakan fitur numerik dan kategorikal yang lebih aman secara akademik

- **Score mode**  
  Menggunakan fitur numerik, kategorikal, serta teks diagnosis yang telah dibersihkan untuk mengejar performa model yang lebih tinggi

Langkah preprocessing meliputi:
- Imputasi missing value numerik dengan median
- Standardisasi fitur numerik menggunakan **StandardScaler**
- Imputasi kategorikal dengan modus
- Encoding kategorikal menggunakan **OneHotEncoder**
- Ekstraksi fitur teks diagnosis menggunakan **TF-IDF Vectorizer**
- Reduksi dimensi fitur teks dengan **TruncatedSVD** pada score mode

#### 4. Baseline Modeling
Model baseline diuji pada beberapa kombinasi mode dan rasio split data untuk mencari konfigurasi awal terbaik.

Rasio split yang dicoba:
- 70:30
- 75:25
- 80:20
- 85:15
- 90:10

#### 5. Modeling Menggunakan 3 Algoritma
- **XGBoost**
- **LightGBM**
- **CatBoost**

Ketiga algoritma ini dibandingkan untuk melihat model mana yang paling baik dalam mengklasifikasikan data pasien ke lima kelas penyakit.

#### 6. Hyperparameter Tuning
- Menggunakan **RandomizedSearchCV**
- Tuning dilakukan pada konfigurasi baseline terbaik
- Cross-validation menggunakan **StratifiedKFold**

#### 7. Evaluation
Model dievaluasi menggunakan:
- **Accuracy**
- **F1-Score (Weighted)**
- **Classification Report**
- **Confusion Matrix**

#### 8. Final Model Selection
Model akhir dipilih berdasarkan akurasi tertinggi setelah tuning. Berdasarkan hasil proyek, model final yang terpilih adalah:

- **Final Model**: LightGBM
- **Sumber Model**: Tuned Model
- **Mode Terbaik**: score
- **Split Terbaik**: 85:15 :contentReference[oaicite:1]{index=1}

#### 9. Inference & Deployment
Setelah model terbaik diperoleh:
- Model disimpan dalam format `.pkl`
- Label encoder disimpan terpisah
- Metadata model juga disimpan
- Sistem prediksi dibangun ke dalam antarmuka **Gradio**
- Pengguna dapat memasukkan data pasien dan mendapatkan hasil prediksi kelas penyakit secara langsung

## 📈 Visualisasi yang Digunakan
Proyek ini juga dilengkapi visualisasi untuk mendukung analisis data dan hasil model, antara lain:

- Distribusi target 5 kelas
- Pie chart proporsi kelas penyakit
- Heatmap korelasi fitur numerik
- Confusion matrix
- Visualisasi fitur numerik penting
- Analisis distribusi data pasien

Visualisasi ini membantu memahami karakteristik dataset, keseimbangan kelas, hubungan antar fitur, serta performa model akhir.

## 🌐 Live Demo
Jika aplikasi sudah di-deploy, kamu bisa menambahkan link di sini.

Contoh:
`👉 [Demo Aplikasi](https://your-app-link-here)`

## ⚙️ Cara Menjalankan (Local)

### 1. Clone Repository
```bash
git clone https://github.com/username/nama-repo.git
cd nama-repo
