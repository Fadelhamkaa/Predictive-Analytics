# Laporan Proyek Machine Learning - [Muhammad Fadel Hamka]

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

Model dievaluasi menggunakan tiga metrik utama:

- **MAE (Mean Absolute Error)**: Mengukur rata-rata galat absolut antara nilai aktual dan prediksi.
- **RMSE (Root Mean Squared Error)**: Menghitung akar dari rata-rata kuadrat galat, memberikan penalti lebih besar untuk kesalahan besar.
- **R² Score (Coefficient of Determination)**: Mengukur seberapa baik model menjelaskan variabilitas data target.

### Hasil Evaluasi:

**Linear Regression**
- MAE  : 5.20 °C
- RMSE : 6.10 °C
- R²   : 0.31

**Decision Tree Regressor**
- MAE  : 2.73 °C
- RMSE : 3.70 °C
- R²   : 0.75

### Interpretasi:

Model **Decision Tree Regressor** menunjukkan performa yang jauh lebih baik dibandingkan **Linear Regression**, dengan nilai MAE dan RMSE yang lebih rendah serta R² yang jauh lebih tinggi. Artinya, model pohon keputusan lebih mampu menangkap pola non-linier dalam data dan lebih cocok untuk memodelkan suhu rata-rata harian berdasarkan fitur cuaca lainnya.

Sebaliknya, model **Linear Regression** memiliki akurasi rendah (R² hanya 0.31), mengindikasikan bahwa model linier tidak cukup untuk menjelaskan variasi suhu rata-rata harian dengan baik.

### Rekomendasi:

- Gunakan **Decision Tree Regressor** sebagai model utama untuk prediksi suhu.
- Pertimbangkan untuk menggunakan model lain seperti **Random Forest** atau **Gradient Boosting** untuk hasil yang lebih baik.

---

## ✅ **Kesimpulan & Saran**

### Kesimpulan:

* Model **Linear Regression** kurang efektif untuk memodelkan data suhu harian di Delhi, terbukti dari R² yang hanya 0.31.
* Model **Decision Tree Regressor** menunjukkan performa jauh lebih baik, dengan R² sebesar 0.75 dan MAE yang cukup rendah.
* Data cuaca memiliki hubungan non-linear yang lebih baik ditangani oleh model berbasis pohon keputusan.

### Saran:

* Gunakan **Decision Tree Regressor** atau model non-linear lainnya (misalnya **Random Forest**, **Gradient Boosting**) untuk implementasi sistem prediksi suhu harian.
* Lakukan **hyperparameter tuning** dan **cross-validation** untuk meningkatkan generalisasi model.
* Pertimbangkan penambahan fitur baru seperti indeks urban heat, kategori musim, atau variabel waktu (bulan/hari) untuk meningkatkan akurasi model.

---

## 📁 Struktur Berkas Submission

submission/
│
├── Predictive_Analytics_Muhammad_Fadel_Hamka.ipynb # Notebook utama dengan seluruh tahapan
├── predictive_analytics_muhammad_fadel_hamka.py # Versi script Python
└── README.md # File ini
