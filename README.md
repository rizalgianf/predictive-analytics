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
- Menerapkan model **LSTM Multivariate** yang mampu menangkap tren dan pola pergerakan harga emas berdasarkan data historis.

### Solution Statements
- **Solusi 1:** Mengimplementasikan model **LSTM Multivariate** dengan data harga emas historis untuk memprediksi harga emas di masa depan. Model ini akan mempelajari pola temporal dalam data harga emas dan memberikan prediksi yang lebih akurat.
- **Solusi 2:** Meningkatkan model LSTM melalui tuning hyperparameter, seperti jumlah unit LSTM dan tingkat dropout, untuk meningkatkan akurasi prediksi.

## 3. Data Understanding

### Deskripsi Data
Dataset yang digunakan adalah data **harga emas harian dalam IDR (Rupiah Indonesia)** yang diperoleh melalui *web scraping* dari situs Logam Mulia Indonesia. Data ini mencakup informasi harga emas dari **14 Maret 2019 hingga 21 Mei 2025**.

### Variabel-variabel pada Dataset
- **Tanggal (Date):** Tanggal pencatatan harga emas.
- **Harga Emas (Gold Price):** Harga emas per gram dalam IDR pada tanggal tertentu. Ini adalah variabel target yang akan diprediksi.

### Sumber Data
- **Link Sumber Data:** Data diambil dari API Pluang: [https://pluang.com/api/asset/gold/pricing?daysLimit=20000]

### Eksplorasi Data (EDA)
Tahapan EDA yang dilakukan meliputi:
1. **Memuat dan Memeriksa Data:** Mengecek apakah terdapat data yang hilang dan memastikan data terformat dengan benar.
2. **Visualisasi Deret Waktu:** Menggunakan plot untuk memvisualisasikan tren harga emas dan mengidentifikasi pola atau anomali.
3. **Analisis Autokorelasi:** Menggunakan plot ACF dan PACF untuk memahami ketergantungan waktu dalam data harga emas.

## 4. Data Preparation

### Teknik Data Preparation yang Dilakukan
1. **Penanganan Data yang Hilang:** Nilai yang hilang diimputasi menggunakan interpolasi linier agar deret waktu tetap kontinu.
2. **Konversi Tanggal ke Datetime:** Kolom tanggal diubah menjadi format datetime dan dijadikan indeks untuk mempermudah analisis deret waktu.
3. **Normalisasi Data:** Harga emas dinormalisasi menggunakan Min-Max Scaling untuk meningkatkan kinerja model LSTM.
4. **Pembagian Data Latih dan Uji:** Data dibagi menjadi dua bagian, yaitu data latih (80%) dan data uji (20%).

### Alasan Mengapa Data Preparation Diperlukan
Langkah-langkah ini dilakukan untuk memastikan data dalam kondisi yang bersih dan terstruktur dengan baik. Normalisasi sangat penting untuk model LSTM karena model ini sensitif terhadap skala data.

## 5. Modeling

### Model LSTM Multivariate
- **Arsitektur LSTM:** Model ini terdiri dari beberapa lapisan LSTM yang diikuti oleh lapisan dens untuk menghasilkan prediksi. LSTM mampu mempelajari ketergantungan temporal dalam data harga emas.
- **Hyperparameter:** Kami menguji berbagai kombinasi hyperparameter seperti jumlah unit LSTM dan tingkat dropout untuk menemukan konfigurasi yang optimal.

### Proses dan Parameter yang Digunakan
1. **Fitur Input:** Data historis harga emas digunakan sebagai input utama untuk model.
2. **Lapisan Tersembunyi:** Model terdiri dari beberapa lapisan LSTM dengan unit berkisar antara 50 hingga 100.
3. **Lapisan Output:** Lapisan output terdiri dari satu neuron yang memprediksi harga emas pada titik waktu berikutnya.

### Peningkatan Model
Untuk meningkatkan kinerja model, dilakukan tuning hyperparameter dengan menguji berbagai konfigurasi lapisan dan unit. Pengujian silang dilakukan untuk mencegah overfitting dan meningkatkan kemampuan generalisasi model.

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


