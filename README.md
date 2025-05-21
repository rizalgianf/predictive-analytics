# Prediksi Harga Emas dengan Model Time Series (ARIMA & SARIMA)

## Pendahuluan

Proyek ini bertujuan untuk mengembangkan dan mengevaluasi model *time series* untuk memprediksi harga emas harian. Harga emas merupakan aset penting dalam investasi dan pasar komoditas, namun fluktuasinya yang signifikan seringkali menyulitkan investor dalam pengambilan keputusan. Dengan memanfaatkan data historis, proyek ini berupaya membangun model prediktif yang dapat memberikan informasi berharga untuk perencanaan strategi investasi.

## Domain Proyek

Proyek ini berfokus pada **prediksi harga emas** dalam domain **pasar komoditas dan investasi**. Emas secara historis dianggap sebagai aset *safe haven* dan pilihan investasi yang populer. Namun, harganya sangat dinamis, dipengaruhi oleh berbagai faktor ekonomi makro, geopolitik, dan sentimen pasar. Ketidakpastian ini menyebabkan tantangan bagi investor. Proyek ini hadir untuk mengurangi ketidakpastian tersebut dengan menyediakan perkiraan harga yang lebih akurat, membantu investor membuat keputusan yang lebih cerdas, mengoptimalkan portofolio, dan mengurangi risiko finansial. Studi terkait menunjukkan bahwa model *time series* seperti ARIMA dan SARIMA efektif dalam memprediksi harga komoditas finansial dengan menangkap pola dan tren historis.

## Business Understanding

### Pernyataan Masalah

* Investor kesulitan memprediksi pergerakan harga emas di masa depan, yang menyebabkan pengambilan keputusan investasi yang suboptimal dan potensi kerugian.
* Kurangnya model yang akurat dan dapat diandalkan untuk memprediksi harga emas membuat strategi investasi jangka pendek dan panjang sulit untuk dirumuskan secara efektif.

### Tujuan Proyek

* Menyediakan prediksi harga emas yang akurat untuk membantu investor membuat keputusan investasi yang lebih terinformasi dan meminimalkan risiko.
* Mengembangkan model *time series* yang *robust* yang mampu menangkap tren dan pola musiman dalam data harga emas historis untuk mendukung perencanaan strategi investasi.

### Solusi yang Diusulkan

* Mengembangkan model regresi *time series* **ARIMA (Autoregressive Integrated Moving Average)** untuk memprediksi harga emas. Model ini akan dilatih pada data harga emas historis dan dievaluasi menggunakan metrik **MAE (Mean Absolute Error)** dan **RMSE (Root Mean Squared Error)**.
* Melakukan *improvement* pada model *time series* dengan menggunakan **SARIMA (Seasonal Autoregressive Integrated Moving Average)**, yang merupakan ekstensi dari ARIMA yang mampu menangani pola musiman dalam data. Kinerja model SARIMA akan dibandingkan dengan ARIMA menggunakan metrik MAE dan RMSE untuk menentukan model terbaik.

## Data Understanding

Data yang digunakan dalam proyek ini adalah **harga emas harian dalam IDR (Rupiah Indonesia)**, yang diperoleh melalui *web scraping* dari situs web Logam Mulia Indonesia. Dataset ini mencakup informasi harga emas historis dari 14 Maret 2019 hingga 21 Mei 2025.

### Variabel-variabel pada dataset:

* **Tanggal:** Merepresentasikan tanggal pencatatan harga emas. Ini akan diubah menjadi format datetime dan diatur sebagai indeks deret waktu.
* **Harga Emas (IDR):** Merepresentasikan harga emas per gram dalam mata uang Rupiah Indonesia pada tanggal tertentu. Ini adalah variabel target untuk diprediksi.

### Eksplorasi Data (EDA)

Tahapan EDA meliputi:
* **Pemuatan dan Inspeksi Data:** Memeriksa struktur awal data, tipe data, dan keberadaan nilai yang hilang.
* **Konversi Tipe Data dan Pengaturan Indeks:** Mengubah kolom 'Tanggal' menjadi format datetime dan menjadikannya indeks untuk analisis deret waktu.
* **Visualisasi Deret Waktu:** Plot garis untuk memvisualisasikan tren historis harga emas, membantu mengidentifikasi pola atau anomali.
* **Analisis Autokorelasi (ACF dan PACF):** Plot ACF dan PACF digunakan untuk mengidentifikasi orde parameter yang sesuai untuk model ARIMA dan SARIMA, baik untuk komponen non-musiman maupun musiman.

## Data Preparation

Langkah-langkah persiapan data yang dilakukan:

1.  **Pengambilan Data Harga Emas:** Fokus pada kolom harga emas sebagai deret waktu utama untuk pemodelan.
2.  **Penanganan *Missing Values* dan Duplikat:** Memastikan kontinuitas deret waktu dengan menangani nilai-nilai yang hilang atau duplikat pada indeks tanggal (jika ada, dilakukan interpolasi atau *resampling*).
3.  **Memastikan Indeks adalah `DatetimeIndex`:** Mengonversi kolom tanggal menjadi indeks bertipe datetime agar kompatibel dengan fungsi-fungsi *time series*.


## Modeling

Proyek ini menggunakan model **ARIMA** dan **SARIMA** untuk peramalan harga emas.

### ARIMA (Autoregressive Integrated Moving Average)

Model ARIMA memiliki tiga komponen: **AR** (Autoregressive), **I** (Integrated), dan **MA** (Moving Average), masing-masing diwakili oleh orde `p`, `d`, dan `q`. ARIMA cocok untuk deret waktu dengan tren atau pola autokorelasi, namun tidak secara langsung menangani musiman. Pemilihan parameternya didasarkan pada analisis ACF/PACF.

**Parameter ARIMA yang Digunakan:**
Model ARIMA yang digunakan memiliki orde (5, 1, 0). Ini berarti model mempertimbangkan 5 *lag* autokorelatif, 1 tingkat diferensiasi untuk mencapai stasionaritas, dan 0 *lag* *moving average*.

### SARIMA (Seasonal Autoregressive Integrated Moving Average)

SARIMA adalah pengembangan dari ARIMA yang menambahkan komponen musiman. Selain orde non-musiman (p, d, q), SARIMA memiliki orde musiman (P, D, Q, S), di mana S adalah panjang periode musiman. SARIMA efektif untuk deret waktu dengan pola musiman yang jelas, meskipun lebih kompleks dalam implementasi dan pemilihan parameternya.

**Parameter SARIMA yang Digunakan:**
Model SARIMA yang digunakan memiliki orde non-musiman (5, 1, 0) dan orde musiman (1, 1, [1], 12). Ini mengindikasikan 5 *lag* autokorelatif non-musiman, 1 tingkat diferensiasi non-musiman, dan 0 *lag* *moving average* non-musiman. Untuk komponen musiman, digunakan 1 *lag* autokorelatif musiman, 1 tingkat diferensiasi musiman, dan 1 *lag* *moving average* musiman dengan periode musiman 12 (misalnya, bulanan).

### Pemilihan Model Terbaik dan Proses Improvement

**SARIMA** dipilih sebagai model terbaik dibandingkan ARIMA karena kemampuannya dalam memodelkan pola musiman, yang mungkin ada dalam data harga emas meskipun tidak terlalu dominan.

**Proses Improvement:**
Proses optimasi model SARIMA melibatkan:
1.  **Pemilihan Model SARIMA:** Memutuskan penggunaan SARIMA untuk mengatasi potensi pola musiman.
2.  **Penentuan Orde Parameter:** Menentukan orde (p, d, q) dan musiman (P, D, Q, S) melalui analisis plot ACF dan PACF dari deret waktu (baik asli maupun setelah diferensiasi). Pengujian stasionaritas (misalnya dengan uji Augmented Dickey-Fuller) juga dilakukan untuk menentukan jumlah diferensiasi yang diperlukan. Iterasi dilakukan dengan mencoba beberapa kombinasi parameter dan memilih yang menghasilkan nilai Akaike Information Criterion (AIC) atau Bayesian Information Criterion (BIC) terendah, yang menunjukkan keseimbangan terbaik antara *goodness of fit* dan kompleksitas model.

## Evaluation

Metrik evaluasi yang digunakan adalah **MAE (Mean Absolute Error)** dan **RMSE (Root Mean Squared Error)**, yang relevan untuk kasus prediksi nilai numerik.

### Penjelasan Metrik:

1.  **Mean Absolute Error (MAE):** Mengukur rata-rata selisih absolut antara nilai prediksi dan nilai aktual. MAE memberikan bobot yang sama pada semua kesalahan. Formula: $MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$.

2.  **Root Mean Squared Error (RMSE):** Mengukur akar kuadrat dari rata-rata kuadrat kesalahan. RMSE memberikan bobot lebih besar pada kesalahan yang besar. Formula: $RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}$.

### Hasil Proyek Berdasarkan Metrik Evaluasi

Berdasarkan hasil evaluasi model:

**Hasil Model ARIMA:**
* **Log Likelihood:** -20711.485
* **AIC (Akaike Information Criterion):** 41434.970
* **BIC (Bayesian Information Criterion):** 41469.308
* **MAE:** 5676.94
* **RMSE:** 16831.91

**Hasil Model SARIMA:**
* **Log Likelihood:** -20998.935
* **AIC (Akaike Information Criterion):** 42013.870
* **BIC (Bayesian Information Criterion):** 42059.613
* **MAE:** 7387.20
* **RMSE:** 20611.11

**Interpretasi Hasil:**

Berdasarkan perbandingan metrik, model **ARIMA** (MAE 5676.94, RMSE 16831.91) menunjukkan kinerja yang lebih baik dalam hal kesalahan prediksi dibandingkan model **SARIMA** (MAE 7387.20, RMSE 20611.11) pada data yang dievaluasi. Model ARIMA juga memiliki nilai AIC dan BIC yang lebih rendah, yang mengindikasikan *goodness of fit* yang lebih baik dengan kompleksitas model yang mungkin lebih sederhana untuk data ini.

Nilai RMSE yang lebih tinggi dari MAE untuk kedua model menunjukkan adanya beberapa kesalahan prediksi yang cukup besar yang memiliki dampak signifikan pada total kesalahan. Meskipun SARIMA dirancang untuk menangani pola musiman, pada data harga emas ini, komponen musiman dengan periode 12 mungkin tidak terlalu dominan atau belum sepenuhnya dioptimalkan, sehingga model ARIMA dasar memberikan akurasi yang lebih baik.

Dibandingkan dengan skala harga emas (yang bisa ratusan ribu hingga jutaan IDR per gram), nilai MAE dan RMSE ini memberikan indikasi tingkat akurasi model. Semakin rendah nilai MAE dan RMSE, semakin baik kinerja model dalam memprediksi harga emas. Hasil ini akan menjadi *baseline* kinerja model untuk prediksi harga emas. Untuk interpretasi yang lebih mendalam, perlu perbandingan dengan *benchmark* sederhana dan ekspektasi toleransi kesalahan dari perspektif bisnis.

Secara keseluruhan, kedua model telah berhasil dilatih dan dievaluasi. Model ARIMA menunjukkan kinerja yang lebih superior pada dataset ini, yang menjadi dasar kuat untuk prediksi harga emas.