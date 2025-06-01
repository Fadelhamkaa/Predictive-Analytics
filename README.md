# Laporan Proyek Machine Learning - Muhammad Fadel Hamka

## 📌 Domain Proyek: Prediksi Suhu Rata-Rata Harian di Delhi

Suhu harian memengaruhi kesehatan masyarakat, perencanaan energi, dan aktivitas pertanian. Fluktuasi mendadak berpotensi meningkatkan risiko *heat stress* serta memengaruhi konsumsi listrik. Oleh karena itu, akurasi prediksi suhu harian sangat penting untuk mitigasi dan perencanaan kebijakan publik.

### 📚 Referensi:
- A. Sharma et al., "Climatic Factors and Health Impacts in Delhi Region," *International Journal of Environmental Research*, vol. 12, no. 4, pp. 345–356, 2018.

---

## 🎯 Business Understanding

### 🔍 Problem Statements
- Variasi suhu harian sulit diantisipasi tanpa model prediksi yang akurat.
- Kurangnya informasi prediktif mempersulit perencanaan energi dan mitigasi risiko kesehatan.

### 🎯 Goals
- Membangun model prediksi suhu rata-rata harian dengan akurasi yang dapat diterima.
- Membandingkan performa beberapa model regresi sederhana.
- Menyediakan estimasi suhu keesokan hari untuk membantu pengambilan keputusan publik.

### 💡 Solution Statement
- Menggunakan Linear Regression sebagai model baseline untuk estimasi suhu harian.
- Menggunakan Decision Tree Regressor sebagai model pembanding untuk melihat potensi peningkatan performa.

---

## 📊 Data Understanding

Dataset: **Daily Delhi Climate Train**
Sumber Data: Kaggle, "Daily Delhi Climate Train Dataset," 2019. [Online]. Available: https://www.kaggle.com/sudalairajkumar/daily-climate-time-series-data
Jumlah data: **1.462 baris** dan **5 kolom**.
Periode: **1 Januari 2013 – 1 Januari 2017** (Berdasarkan output `df.head()` dan tren plot suhu).

| Fitur         | Tipe     | Deskripsi                            |
|---------------|----------|--------------------------------------|
| `date`        | object   | Tanggal pencatatan (awalnya object, kemudian dikonversi ke datetime) |
| `meantemp`    | float    | Suhu rata-rata harian (°C) - **Target Variabel** |
| `humidity`    | float    | Persentase kelembapan (%)            |
| `wind_speed`  | float    | Kecepatan angin rata-rata (km/h)     |
| `meanpressure`| float    | Tekanan udara rata-rata (hPa)        |

- **EDA Insights Utama**:
  - `meantemp` vs `humidity`: Korelasi negatif sedang (-0.57). Kenaikan suhu cenderung diikuti penurunan kelembapan.
  - `humidity` vs `wind_speed`: Korelasi negatif sedang (-0.37). Kenaikan kelembapan cenderung diikuti penurunan kecepatan angin.
  - `meantemp` vs `wind_speed`: Korelasi positif lemah (0.31).
  - `meanpressure`: Terlihat adanya nilai ekstrem (maksimum 7679.33 hPa) yang mengindikasikan potensi outlier.
  - Tidak ditemukan nilai yang hilang (missing values) pada dataset.
  - Kolom `date` awalnya bertipe object dan perlu dikonversi.
  - Visualisasi tren suhu harian (`meantemp`) menunjukkan pola musiman tahunan yang jelas.

---

## 🧹 Data Preparation

- **Konversi Tipe Data**: Mengubah kolom `date` dari tipe `object` menjadi `datetime` menggunakan `pd.to_datetime(df['date'])`. Ini penting untuk analisis berbasis waktu, meskipun pada pemodelan ini fitur `date` tidak secara langsung digunakan sebagai prediktor.
- **Pengecekan Missing Values**: Dilakukan pengecekan menggunakan `df.isnull().sum()`, tidak ditemukan missing values.
- **Identifikasi Outlier**: Pada tahap EDA (Data Understanding), teridentifikasi potensi outlier pada `meanpressure`. Namun, tidak dilakukan penghapusan atau penanganan outlier secara spesifik pada tahap persiapan data sebelum pemodelan di notebook ini.
- **Pemilihan Fitur**: Fitur yang digunakan untuk prediksi (`X`) adalah `humidity`, `wind_speed`, dan `meanpressure`. Target variabel (`y`) adalah `meantemp`.
- **Pembagian Data**: Dataset dengan fitur dan target yang sudah dipilih kemudian dibagi menjadi data latih (train) dan data uji (test) dengan rasio **80:20** menggunakan `train_test_split` dari `sklearn.model_selection`. Parameter `random_state=42` digunakan untuk memastikan hasil pembagian data yang konsisten dan dapat direproduksi.
- **Scaling Fitur**: Tidak dilakukan penerapan teknik scaling fitur seperti `StandardScaler` pada notebook ini sebelum melatih model.

---

## 🔧 Modeling

### ✅ Linear Regression (Baseline)
- **Model**: `LinearRegression()` dari `sklearn.linear_model`.
- **Penjelasan Cara Kerja**: Model Linear Regression bekerja dengan mencari hubungan linear terbaik antara fitur-fitur independen (X) dan variabel target (y). Model ini mencoba menemukan garis (atau hyperplane dalam kasus multi-fitur) yang paling sesuai dengan data dengan metode *Ordinary Least Squares* (OLS), yaitu meminimalkan jumlah kuadrat selisih antara nilai aktual dan nilai prediksi.
- **Pelatihan**: Model dilatih menggunakan data fitur pelatihan (`X_train`) dan target pelatihan (`y_train`).
- **Parameter**: Menggunakan parameter default dari `LinearRegression()`.

### 🌲 Decision Tree Regressor
- **Model**: `DecisionTreeRegressor(random_state=42)` dari `sklearn.tree`.
- **Penjelasan Cara Kerja**: Decision Tree Regressor bekerja dengan membuat model prediktif dalam bentuk struktur pohon. Model ini mempartisi data secara rekursif berdasarkan nilai fitur untuk membuat serangkaian aturan keputusan (if-then-else) yang mengarah pada prediksi nilai target. Pada setiap node, model memilih fitur dan titik pemisah yang paling baik dalam mengurangi varians (atau impurity) pada data target. Prediksi untuk data baru dilakukan dengan mengikuti jalur dari root hingga leaf node berdasarkan nilai fitur data tersebut.
- **Pelatihan**: Model dilatih menggunakan data fitur pelatihan (`X_train`) dan target pelatihan (`y_train`).
- **Parameter**: Parameter `random_state=42` digunakan untuk memastikan hasil yang konsisten dan dapat direproduksi saat model dibangun. Parameter lain menggunakan nilai default.

### 🚀 (Opsional) Random Forest
- Disiapkan untuk eksplorasi lanjutan dengan hyperparameter tuning dan cross-validation untuk potensi peningkatan akurasi.

---

## 📈 Evaluation

Metrik evaluasi yang digunakan adalah Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), dan R-squared (R²).

| Metrik        | Linear Regression | Decision Tree Regressor |
|---------------|-------------------|-------------------------|
| MAE           | 5.20 °C           | 2.73 °C                 |
| RMSE          | 6.10 °C           | 3.70 °C                 |
| R²            | 0.31              | 0.75                    |


- **Interpretasi**:
  - **Linear Regression**:
    - MAE sebesar 5.20 °C dan RMSE sebesar 6.10 °C menunjukkan rata-rata kesalahan prediksi dari model ini.
    - R² sebesar 0.31 mengindikasikan bahwa model Linear Regression hanya mampu menjelaskan sekitar 31% variabilitas dalam data suhu rata-rata.
  - **Decision Tree Regressor**:
    - MAE (2.73 °C) dan RMSE (3.70 °C) jauh lebih rendah dibandingkan Linear Regression, yang menunjukkan kesalahan prediksi yang lebih kecil.
    - R² sebesar 0.75 jauh lebih tinggi, menandakan bahwa model Decision Tree Regressor mampu menjelaskan sekitar 75% variabilitas dalam data suhu rata-rata.

Berdasarkan metrik evaluasi, **Decision Tree Regressor** menunjukkan performa yang jauh lebih baik daripada Linear Regression dalam memprediksi suhu rata-rata pada dataset ini.

---

## 🧾 Kesimpulan & Saran

- Model Linear Regression memiliki R² sebesar 0.31 dan berfungsi sebagai baseline.
- Model Decision Tree Regressor menunjukkan performa yang lebih baik dengan R² sebesar 0.75, MAE 2.73 °C, dan RMSE 3.70 °C. Ini menunjukkan bahwa Decision Tree lebih mampu menangkap pola dalam data untuk prediksi suhu.

- **Saran**:
  - Implementasikan algoritma tree-based lain seperti Random Forest dengan hyperparameter tuning dan cross-validation untuk potensi peningkatan akurasi lebih lanjut.
  - Lakukan penanganan outlier pada fitur `meanpressure` yang teridentifikasi pada tahap EDA, karena ini mungkin memengaruhi performa model.
  - Eksplorasi penggunaan teknik scaling fitur, meskipun tidak diimplementasikan pada iterasi ini, mungkin dapat memberikan peningkatan performa pada beberapa model.
  - Tambahkan fitur baru seperti *urban heat index* atau variabel cuaca tambahan untuk memperkaya model dan meningkatkan kemampuan prediktif.

---

## 📁 Struktur Berkas Submission
submission/
│
├── notebook.ipynb # Notebook utama dengan seluruh tahapan (misal: Predictive_Analytics_Muhammad_Fadel_Hamka.ipynb)
├── script.py # Versi script Python (jika ada)
├── report.md # Laporan Markdown (misal: nama_anda_report.md)
└── README.md # File ini