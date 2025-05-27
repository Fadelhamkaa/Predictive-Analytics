# Laporan Proyek Machine Learning - [Nama Anda]

## 📌 Domain Proyek: Prediksi Suhu Rata-Rata Harian di Delhi

Suhu harian memengaruhi kesehatan masyarakat, perencanaan energi, dan aktivitas pertanian. Fluktuasi mendadak berpotensi meningkatkan risiko *heat stress* serta memengaruhi konsumsi listrik. Oleh karena itu, akurasi prediksi suhu harian sangat penting untuk mitigasi dan perencanaan kebijakan publik.

### 📚 Referensi:
- A. Sharma et al., "Climatic Factors and Health Impacts in Delhi Region," *International Journal of Environmental Research*, vol. 12, no. 4, pp. 345–356, 2018.
- Kaggle, "Daily Delhi Climate Train Dataset," 2019. [Online]. Available: https://www.kaggle.com/sudalairajkumar/daily-climate-time-series-data

---

## 🎯 Business Understanding

### 🔍 Problem Statements
- Variasi suhu harian sulit diantisipasi tanpa model prediksi yang akurat.
- Kurangnya informasi prediktif mempersulit perencanaan energi dan mitigasi risiko kesehatan.

### 🎯 Goals
- Membangun model prediksi suhu rata-rata harian dengan akurasi tinggi.
- Menyediakan estimasi suhu keesokan hari untuk membantu pengambilan keputusan publik.

### 💡 Solution Statement
- **Baseline**: Linear Regression untuk estimasi suhu harian.
- **Improvement (opsional)**: Hyperparameter tuning dengan algoritma tree-based seperti Random Forest untuk membandingkan performa.

---

## 📊 Data Understanding

Dataset: **Daily Delhi Climate Train**  
Jumlah data: **1.462 baris**  
Periode: **2013–2017**

| Fitur         | Tipe     | Deskripsi                            |
|---------------|----------|--------------------------------------|
| `date`        | datetime | Tanggal pencatatan                   |
| `meantemp`    | float    | Suhu rata-rata harian (°C)           |
| `humidity`    | float    | Persentase kelembapan (%)            |
| `wind_speed`  | float    | Kecepatan angin rata-rata (km/h)     |
| `meanpressure`| float    | Tekanan udara rata-rata (hPa)        |

- EDA menunjukkan korelasi negatif tinggi antara kelembapan dan suhu (**–0.57**)
- Data tekanan udara mengandung beberapa **outlier ekstrem**

---

## 🧹 Data Preparation

- **Cleaning**: Menghapus baris dengan `meanpressure` < 900 hPa atau > 1100 hPa.
- **Splitting**: Membagi data menjadi **train:test = 80:20**, menggunakan `random_state=42`.
- **Scaling**: Menerapkan `StandardScaler` pada fitur `humidity`, `wind_speed`, dan `meanpressure`.

---

## 🔧 Modeling

### ✅ Linear Regression (Baseline)
- Model: `LinearRegression()`
- Dilatih menggunakan data yang telah dinormalisasi.

### 🌲 (Opsional) Random Forest
- Disiapkan untuk eksplorasi lanjutan dengan hyperparameter tuning dan cross-validation.

---

## 📈 Evaluation

| Metrik | Nilai     |
|--------|-----------|
| RMSE   | 3.46 °C   |
| R²     | 0.77      |

- **RMSE** menunjukkan rata-rata galat prediksi sebesar **3.46 °C**
- **R² Score** 0.77 artinya **77% variabilitas suhu harian** dapat dijelaskan oleh model

---

## 🧾 Kesimpulan & Saran

- Model Linear Regression cukup baik sebagai **baseline** dengan **R² = 0.77**
- **Saran**:
  - Implementasikan Random Forest dengan hyperparameter tuning dan cross-validation untuk meningkatkan akurasi.
  - Tambahkan fitur baru seperti *urban heat index* atau variabel cuaca tambahan untuk memperkaya model.

---

## 📁 Struktur Berkas Submission

submission/
│
├── notebook.ipynb # Notebook utama dengan seluruh tahapan
├── script.py # Versi script Python
├── report.md # Laporan Markdown
└── README.md # File ini
