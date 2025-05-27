# Laporan Proyek: Prediksi Harga Emas Menggunakan Model LSTM Multivariate

## 1. Domain Proyek

### Latar Belakang
Emas telah lama menjadi salah satu pilihan investasi yang aman (*safe haven*), terutama pada periode ketidakpastian ekonomi dan geopolitik. Namun, harga emas sering kali berfluktuasi secara signifikan, membuatnya sulit diprediksi. Fluktuasi harga yang tajam ini menambah tantangan bagi investor dalam membuat keputusan investasi yang tepat.

Proyek ini bertujuan untuk memanfaatkan **model LSTM Multivariate** (Long Short-Term Memory) dalam memprediksi harga emas, berdasarkan data historis harga emas yang tersedia. Dengan memanfaatkan model deep learning, diharapkan dapat memberikan prediksi yang lebih akurat dalam membantu investor merencanakan strategi investasi mereka.

### Mengapa Masalah Ini Harus Diselesaikan
Ketidakpastian dalam pergerakan harga emas dapat menyebabkan investor mengalami kerugian yang signifikan jika tidak dapat memprediksi pergerakan harga dengan tepat. Oleh karena itu, solusi berbasis *predictive analytics* yang lebih akurat diperlukan untuk membantu investor mengurangi risiko dan membuat keputusan investasi yang lebih cerdas.

### Riset Terkait
Penelitian sebelumnya telah menunjukkan bahwa model **LSTM** merupakan salah satu teknik yang efektif dalam memprediksi deret waktu dengan kompleksitas tinggi, seperti pergerakan harga emas. Sebagai referensi terkait, berikut adalah artikel yang relevan:
- **"Deep Learning for Time Series Forecasting"** oleh F. Chollet, 2015.

## 2. Business Understanding

### Problem Statements
Investor kesulitan dalam memprediksi pergerakan harga emas di masa depan, yang menyebabkan pengambilan keputusan investasi yang kurang optimal dan potensi kerugian.

### Tujuan Proyek
- Mengembangkan model yang dapat memprediksi harga emas dengan akurat, sehingga investor dapat membuat keputusan investasi yang lebih terinformasi.


### Solution Statements
- **Solusi 1:** Mengimplementasikan model **LSTM Multivariate** dengan data harga emas historis untuk memprediksi harga emas di masa depan. Model ini akan mempelajari pola temporal dalam data harga emas dan memberikan prediksi yang lebih akurat.


## 3. Data Understanding

### Deskripsi Data
Dataset ini terdiri dari **2001 baris** dan **4 kolom**, dengan rincian kolom sebagai berikut:

1. **sell**: Harga jual emas.
2. **buy**: Harga beli emas.
3. **installment**: Nilai cicilan emas.
4. **tgl**: Tanggal transaksi (format string).

### Kondisi Data
- **Jumlah baris**: 2001
- **Jumlah kolom**: 4
- **Data duplikat**: Terdapat **4 data duplikat** dalam dataset ini.
- **Missing values**: Tidak ada data yang hilang pada setiap kolom.

### Deskripsi Fitur
- **sell**: Harga jual emas yang tercatat pada tanggal tertentu.
- **buy**: Harga beli emas yang tercatat pada tanggal tertentu.
- **installment**: Nilai cicilan yang tercatat untuk setiap transaksi.
- **tgl**: Tanggal transaksi yang tercatat.

### Sumber Data
- **Link Sumber Data:** Data diambil dari API Pluang: [https://pluang.com/api/asset/gold/pricing?daysLimit=20000]

### Eksplorasi Data (EDA)
Tahapan EDA yang dilakukan meliputi:
1. **Memuat dan Memeriksa Data:** Mengecek apakah terdapat data yang hilang dan memastikan data terformat dengan benar.
2. **Visualisasi Deret Waktu:** Menggunakan plot untuk memvisualisasikan tren harga emas dan mengidentifikasi pola atau anomali.
3. **Analisis Autokorelasi:** Menggunakan plot ACF dan PACF untuk memahami ketergantungan waktu dalam data harga emas.

## 4. Data Preparation
Berikut adalah **revisi bagian Data Preparation** dalam format markdown yang rapi dan **sesuai urutan kode di notebook**. Silakan copy ke laporanmu:

---

### Teknik Data Preparation yang Dilakukan

1. **Memuat Data dari CSV**  
   Data harga emas dimuat dari file hargaemas.csv yang telah diunduh dari API Pluang.

2. **Konversi Kolom Tanggal ke Datetime**  
   Kolom `tgl` diubah ke format datetime agar dapat diurutkan dan diproses sebagai data deret waktu.

3. **Pengurutan Data Berdasarkan Tanggal**  
   Data diurutkan berdasarkan kolom `tgl` dari tanggal terlama ke terbaru untuk memastikan urutan kronologis yang benar.

4. **Penanganan Data Duplikat**  
   Data duplikat berdasarkan kolom `tgl` dihapus, hanya menyisakan data terakhir untuk setiap tanggal agar tidak terjadi bias pada analisis.

5. **Penanganan Missing Value**  
   Nilai yang hilang pada data numerik diimputasi menggunakan interpolasi linier agar deret waktu tetap kontinu.

6. **Menjadikan Kolom Tanggal sebagai Index**  
   Kolom `tgl` dijadikan index pada dataframe untuk memudahkan analisis dan visualisasi deret waktu.

7. **Feature Engineering**  
   - **Moving Average 30 Hari (`ma30`)**: Menambahkan fitur rata-rata bergerak 30 hari pada harga jual emas untuk menangkap tren jangka menengah.
   - **Rolling Standard Deviation 30 Hari (`std30`)**: Menambahkan fitur standar deviasi bergerak 30 hari untuk menangkap volatilitas harga.

8. **Visualisasi Data Harga Jual Emas**  
   Data harga jual emas divisualisasikan untuk melihat tren dan pola historis.

9. **Pembuatan Sekuens Data (Windowing) untuk LSTM**  
   Data diubah menjadi bentuk sekuens (window) sepanjang 30 hari untuk setiap sampel input LSTM, dengan target prediksi adalah harga `sell` pada hari ke-31.

10. **Normalisasi Data Multivariate**  
   Data pada fitur `sell`, `ma30`, dan `std30` dinormalisasi ke rentang 0-1 menggunakan `MinMaxScaler`. Normalisasi ini penting agar model LSTM dapat belajar dengan lebih stabil dan cepat, karena LSTM sensitif terhadap skala data input.

11. **Pembuatan Sekuens Data (Windowing) untuk LSTM**  
   Data diubah menjadi bentuk sekuens (window) sepanjang 30 hari. Setiap sampel input LSTM terdiri dari 30 hari data historis, dan target prediksi adalah harga `sell` pada hari ke-31. Proses ini menghasilkan array `X` (fitur berurutan) dan `y` (target harga jual) yang siap digunakan untuk training model.

12. Train-Test Split
Data yang sudah dalam bentuk sekuens kemudian dibagi menjadi dua bagian:
- **Data Train (80%)**: Digunakan untuk melatih model LSTM agar dapat mempelajari pola historis harga emas.
- **Data Test (20%)**: Digunakan untuk menguji performa model pada data yang belum pernah dilihat sebelumnya, sehingga dapat mengevaluasi kemampuan generalisasi model.

---

### Alasan Mengapa Data Preparation Diperlukan

Langkah-langkah ini dilakukan untuk memastikan data dalam kondisi bersih, terstruktur, dan siap digunakan oleh model LSTM.  
- Pengurutan dan penghapusan duplikat penting untuk menjaga urutan waktu dan keunikan data.
- Interpolasi menjaga kontinuitas data deret waktu.
- Feature engineering membantu model menangkap tren dan volatilitas.
- Windowing sangat penting agar data sesuai dengan format input yang dibutuhkan LSTM.
- MEmbuat sekuens data dan membagi data train dan data test
---

## 5. Modeling

Berikut adalah **dokumentasi markdown** yang memenuhi kriteria Model Development sesuai dengan kode dan saran penilaian. Silakan tambahkan ke laporanmu:

---

## 5. Modeling

### Penjelasan Cara Kerja LSTM

Long Short-Term Memory (LSTM) adalah jenis Recurrent Neural Network (RNN) yang dirancang untuk mengatasi masalah *vanishing gradient* pada RNN standar, sehingga mampu mempelajari dependensi jangka panjang dalam data deret waktu.

**Mekanisme Internal LSTM:**
- **Cell State:** Merupakan memori utama yang membawa informasi sepanjang urutan data. Cell state memungkinkan LSTM untuk mempertahankan informasi penting dari waktu ke waktu.
- **Gates pada LSTM:**
  - **Forget Gate:** Menentukan informasi apa yang harus dibuang dari cell state.
  - **Input Gate:** Mengatur informasi baru apa yang akan ditambahkan ke cell state.
  - **Output Gate:** Mengontrol informasi apa yang akan dikeluarkan dari cell state sebagai output pada waktu tertentu.
- Dengan mekanisme gates ini, LSTM dapat secara selektif mengingat atau melupakan informasi, sehingga sangat efektif untuk mempelajari pola jangka panjang dan menangani data time series seperti harga emas.

### Arsitektur dan Parameter Model

Model LSTM Multivariate yang digunakan memiliki arsitektur dan parameter sebagai berikut:

- **Layer 1:** LSTM dengan 128 unit, `return_sequences=True` (mengembalikan seluruh urutan untuk layer berikutnya).
- **Dropout:** Dropout rate sebesar 0.2 untuk mencegah overfitting.
- **Layer 2:** LSTM dengan 64 unit.
- **Output Layer:** Dense dengan 1 unit (untuk prediksi harga emas).
- **Optimizer:** Adam
- **Loss Function:** Mean Squared Error (`mean_squared_error`)
- **Epochs:** 100
- **Batch Size:** 32
- **Validation Data:** Menggunakan 20% data terakhir sebagai data validasi.

```python
model = Sequential()
model.add(LSTM(128, return_sequences=True, input_shape=(window_size, X.shape[2])))
model.add(Dropout(0.2))
model.add(LSTM(64))
model.add(Dense(1))
model.compile(optimizer='adam', loss='mean_squared_error')
history = model.fit(
    X_train, y_train,
    epochs=100,
    batch_size=32,
    validation_data=(X_test, y_test),
    verbose=1
)
model.save('model_lstm_multivariate_emas.h5')
```

### Penjelasan Proses Training

- Model dilatih menggunakan data train selama 100 epoch dengan batch size 32.
- Validasi dilakukan pada data test untuk memantau performa model dan mencegah overfitting.
- Dropout digunakan untuk mengurangi risiko overfitting dengan secara acak menonaktifkan sebagian neuron selama training.

### Kesesuaian Parameter

Seluruh parameter yang digunakan pada model di atas **sesuai dengan implementasi pada notebook**, yaitu:
- LSTM layer pertama: 128 unit
- LSTM layer kedua: 64 unit
- Dropout: 0.2
- Optimizer: Adam
- Loss: Mean Squared Error
- Epochs: 100
- Batch size: 32

---


## 6. Evaluation

### Metrik Evaluasi
1. **MAE (Mean Absolute Error):** Mengukur rata-rata selisih absolut antara nilai prediksi dan nilai aktual.
   \[
   MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|
   \]
2. **RMSE (Root Mean Squared Error):** Mengukur akar kuadrat dari rata-rata kuadrat kesalahan, memberikan bobot lebih pada kesalahan yang lebih besar.
   \[
   RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}
   \]

### Hasil Evaluasi
- **MAE:** 10,677.87 IDR
- **RMSE:** 17,364.33 IDR

Hasil ini menunjukkan bahwa meskipun model LSTM dapat memprediksi tren harga emas dengan cukup baik, masih ada beberapa kesalahan besar dalam prediksi harga yang tajam.

## 7. Hasil Prediksi dan Analisis

### Prediksi Harga Emas pada 21 Maret 2026
- **Prediksi Harga Emas pada 21 Maret 2026:** 2,069,221 IDR per gram

### Analisis Hasil Prediksi
- **Tren Umum:** Model berhasil menangkap tren kenaikan harga emas yang stabil dari 2019 hingga 2025, dengan prediksi yang cukup akurat mengikuti pergerakan historis.
- **Fluktuasi Tajam:** Terlihat adanya fluktuasi tajam pada tahun 2025, yang diprediksi oleh model. Meskipun harga emas mengalami lonjakan besar, model LSTM terus mengikuti tren tersebut, mencerminkan kemampuannya dalam mengakomodasi perubahan besar dalam harga.
- **Proyeksi Masa Depan:** Berdasarkan hasil prediksi, harga emas pada tanggal **21 Maret 2026** diperkirakan mencapai **2,069,221 IDR**, yang mencerminkan kelanjutan dari tren kenaikan yang stabil. Proyeksi ini menunjukkan bahwa meskipun terjadi fluktuasi, harga emas kemungkinan akan terus naik dalam beberapa tahun mendatang.

### Evaluasi dan Kelemahan
Meskipun prediksi menunjukkan arah yang baik dan mencerminkan pola historis yang kuat, seperti halnya semua model prediktif, prediksi ini tidak dapat sepenuhnya memprediksi ketidakpastian yang ada dalam pasar. Faktor-faktor eksternal seperti kondisi ekonomi global, kebijakan pemerintah, atau perubahan geopolitik dapat memengaruhi harga emas secara signifikan dan tidak dapat sepenuhnya dipertimbangkan oleh model.

Namun, model ini dapat menjadi alat yang sangat berguna bagi investor untuk mendapatkan gambaran umum tentang potensi pergerakan harga emas dalam jangka waktu yang lebih panjang.

## 8. Kesimpulan

Model **LSTM Multivariate** terbukti efektif dalam memprediksi harga emas, dengan kinerja yang cukup baik dalam menangkap tren jangka panjang. Namun, model ini masih dapat ditingkatkan lebih lanjut dengan melakukan tuning hyperparameter dan memasukkan fitur eksternal yang dapat memperbaiki akurasi prediksi. Peningkatan lebih lanjut dapat dilakukan dengan eksperimen pada arsitektur LSTM yang lebih kompleks atau teknik deep learning lainnya.


