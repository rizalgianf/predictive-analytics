# Laporan Proyek Machine Learning: Prediksi Harga Emas - Rizal Gian Febriantama

---

## 1. Domain Proyek

### Latar Belakang

Harga emas adalah komoditas bernilai tinggi dan sering digunakan sebagai instrumen investasi. Seiring fluktuasi harga yang terjadi, prediksi harga emas menjadi penting bagi investor untuk mengambil keputusan yang lebih baik dalam merencanakan investasi. Setyowibowo, S., As’ad, M., Sujito, & Farida, E. (2022).
Pada proyek ini, digunakan model **ARIMA** dan **SARIMA** untuk memprediksi harga emas di masa depan berdasarkan data historis.

---

## 2. Pernyataan Masalah

- **Pernyataan Masalah 1:**  
    Harga emas sangat fluktuatif dan dipengaruhi oleh berbagai faktor ekonomi global.

- **Pernyataan Masalah 2:**  
    Bagaimana cara memprediksi harga emas yang akurat berdasarkan data historis yang ada?

---

## 3. Tujuan

Membangun model prediksi harga emas yang dapat membantu investor dalam merencanakan investasi dengan lebih tepat berdasarkan proyeksi harga emas di masa depan.

---

## 4. Solution Statement

- **Solusi 1:**  
    Menggunakan **ARIMA** untuk memprediksi harga emas dengan mempertimbangkan komponen non-musiman.

- **Solusi 2:**  
    Menggunakan **SARIMA** untuk memperhitungkan komponen musiman dalam harga emas, yang cenderung berfluktuasi mengikuti siklus tahunan.

---

## 5. Business Understanding

### Pernyataan Masalah

- Prediksi harga emas penting untuk membantu investor mengambil keputusan dalam perencanaan finansial.
- Memprediksi harga emas yang akurat memerlukan model yang mampu menangani data time series dengan komponen musiman.

### Goals

- **Goal 1:** Membuat model yang dapat memprediksi harga emas untuk 3 tahun ke depan dengan akurasi yang baik.
- **Goal 2:** Membandingkan kinerja ARIMA dan SARIMA dalam hal prediksi harga emas.

---

## 6. Data Understanding

### Informasi Dataset

Dataset yang digunakan terdiri dari beberapa kolom:

| Kolom         | Deskripsi                                 |
|---------------|-------------------------------------------|
| `sell`        | Harga jual emas                           |
| `buy`         | Harga beli emas                           |
| `installment` | Harga emas untuk pembelian secara cicilan |
| `tgl`         | Tanggal pencatatan harga emas             |

**Variabel Utama:**  
Menggunakan harga jual emas (`sell`) untuk prediksi harga emas ke depan.

---

## 7. Data Preparation

**Proses Data Preparation:**

1. **Pembersihan Data:**  
     - Menghapus data duplikat  
     - Memeriksa dan mengonversi kolom `tgl` menjadi tipe data datetime

2. **Pemilihan Variabel:**  
     - Menggunakan harga jual (`sell`) sebagai variabel target

3. **Transformasi Data:**  
     - Menggunakan log-transformasi pada harga emas untuk mengatasi heteroskedastisitas

---

## 8. Modeling

### Model yang Digunakan

- **ARIMA (AutoRegressive Integrated Moving Average):**  
    Untuk menangani komponen non-musiman dalam data.
- **SARIMA (Seasonal ARIMA):**  
    Untuk memperhitungkan komponen musiman dalam harga emas (pola musiman tahunan).

### Parameter Model

- **ARIMA(5, 1, 0):**  
    Model ARIMA dengan 5 lags AR untuk menangani tren dalam data.
- **SARIMA(1, 1, 1, 12):**  
    Model SARIMA dengan 1 lag musiman AR, 1 differencing musiman, dan 1 lag musiman MA (periode musiman 12 bulan).

### Proses Pemodelan

- Membangun kedua model (ARIMA dan SARIMA) untuk memprediksi harga emas berdasarkan data historis.
- Hyperparameter tuning berdasarkan ACF dan PACF plots.

---

## 9. Evaluation

### Metrik Evaluasi

- **MAE (Mean Absolute Error):**  
    Mengukur rata-rata perbedaan absolut antara harga yang diprediksi dan harga aktual.
- **RMSE (Root Mean Squared Error):**  
    Mengukur kesalahan prediksi dengan penalti lebih besar pada kesalahan besar.

### Hasil Evaluasi

| Model  | MAE (IDR) | RMSE (IDR) |
|--------|-----------|------------|
| ARIMA  | 5,657.22  | 16,765.55  |
| SARIMA | 7,321.33  | 21,208.04  |

### Visualisasi Hasil

Grafik perbandingan antara harga emas historis dan prediksi harga emas untuk periode 3 tahun ke depan menunjukkan bahwa model dapat memperkirakan harga emas meskipun terdapat kesalahan prediksi.

### Residual Analysis

Analisis residual menunjukkan autokorelasi pada beberapa residual model. Namun, model SARIMA yang memanfaatkan komponen musiman menunjukkan hasil yang lebih baik dibandingkan ARIMA dalam menangkap pola musiman pada harga emas.

---

## 10. Kesimpulan

Proyek ini berhasil mengembangkan model **SARIMA** dan **ARIMA** untuk memprediksi harga emas.  
Berdasarkan evaluasi, model SARIMA dengan komponen musiman memberikan hasil yang lebih baik dibandingkan ARIMA dalam menangkap pola musiman pada harga emas.

---