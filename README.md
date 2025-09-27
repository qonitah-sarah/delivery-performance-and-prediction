# Finally, it’s always six: When physical isn’t enough - Performance Evaluation & Time Prediction in Zomato’s Last-Mile Delivery

## Latar Belakang Proyek
Dataset yang digunakan berasal dari salah satu perusahaan layanan pesan antar makanan yaitu **Zomato**. Zomato merupakan platform berbasis di India yang menyediakan layanan pemesanan dan pengantaran makanan dari berbagai kategori. Layanan ini beroperasi di tiga jenis wilayah yaitu **metropolitan**, **urban**, dan **semi-urban**.

Dataset ini mencatat data historis selama **3 bulan di tahun 2022**, yaitu bulan **Februari, Maret, dan April**. Meskipun hanya mencakup 3 bulan, jumlah pesanan yang tercatat mencapai **45.584 pesanan**, yang menunjukkan tingginya volume penggunaan layanan Zomato dalam periode tersebut.

Dalam proyek ini, dilakukan analisis untuk mengevaluasi **performa pengantaran dari perspektif kurir**. Fokusnya adalah melihat faktor-faktor apa saja yang mempengaruhi performa mereka dalam mengantarkan pesanan. Selain itu, juga dibangun **model prediksi** untuk memperkirakan durasi pengantaran berdasarkan variabel-variabel tertentu. 

Tujuan dari proyek ini adalah memberikan masukan yang dapat digunakan untuk meningkatkan efisiensi pengantaran, serta mendukung proses pengambilan keputusan dalam pengelolaan operasional kurir.

## Struktur Data dan Pemeriksaan Awal
Dataset yang digunakan diperoleh dari [Kaggle: Zomato Delivery Operations Analytics Dataset](https://www.kaggle.com/datasets/saurabhbadole/zomato-delivery-operations-analytics-dataset), terdiri dari **20 kolom** dan **45.584 baris** yang merepresentasikan aktivitas pengantaran makanan dalam periode 3 bulan. Secara garis besar, data memuat informasi terkait:
- **Kurir** sebagai pelaksana pengantaran
- **Kendaraan** yang digunakan
- **Kondisi lingkungan saat pengantaran**, seperti cuaca dan kepadatan lalu lintas
- **Detail pesanan**, termasuk waktu dan jenis pengiriman

Pada tahap awal pemeriksaan, ditemukan bahwa kondisi data belum sepenuhnya bersih. Beberapa permasalahan yang muncul antara lain:
- Penamaan kolom yang mengandung **typo** atau tidak konsisten
- **Format data yang tidak sesuai**, seperti kolom waktu yang belum dikonversi ke tipe datetime
- Ditemukannya **nilai kosong (NaN)** di sejumlah kolom

## Metode
Langkah-langkah yang dilakukan dalam proyek ini meliputi:

### 1. Data Preparation
Langkah pertama adalah mempersiapkan data untuk dianalisis. Beberapa hal yang dilakukan:
- Mengecek kondisi awal data (struktur, tipe data, dll.)
- Memperbaiki **nama kolom yang typo** agar lebih konsisten dan sesuai
- Mengubah **format data** (misalnya tipe waktu dari string ke datetime)
- Memeriksa informasi dasar kondisi data seperti jumlah nilai kosong dan distribusinya

### 2. Data Cleaning (Data Analyst Perspective)
Data cleaning dilakukan sebelum masuk ke tahap modeling. Beberapa hal yang dilakukan antara lain:
- **Tidak ditemukan duplikasi data**
- Ditemukan **missing value**, baik berupa `NaN` maupun nilai yang tidak sesuai
- Nilai yang tidak sesuai diubah menjadi `NaN` untuk menjaga integritas data
- **Missing value** tidak dihapus karena persentasenya masih di bawah 20%
- Ditemukan **outlier** di beberapa kolom, namun nilainya masih wajar sehingga **tidak dihapus**
- Juga ditemukan **pengetikan yang salah** pada beberapa kategori

> Catatan: Proses data cleaning ini masih dalam konteks *data analyst*, belum tahap *machine learning*.

### 3. Feature Engineering (Pra-Modeling)
Feature engineering dilakukan untuk membantu interpretasi dan memudahkan analisis:
- Membuat label interpretatif dari angka-angka pada kolom sebelumnya
- Melakukan segmentasi pada kolom numerik kontinu seperti usia (misalnya mengelompokkan range umur)
- Menambahkan fitur baru yang bisa memperkaya insight dari data

### 4. Exploratory Data Analysis (EDA)
EDA dilakukan untuk memahami data secara mendalam:
- **Statistical descriptive** untuk melihat ringkasan data secara umum
- **Korelasi antar variabel numerik** menggunakan metode **Spearman** (karena terdapat outlier)
- Untuk variabel kategorikal, korelasi dianalisis melalui **visualisasi**
- Analisis khusus dilakukan berdasarkan **kategori usia kurir** terhadap beberapa variabel lain
- Visualisasi lanjutan dan eksplorasi dilakukan menggunakan **Power BI**, dan disusun dalam bentuk dashboard

### 5. Data Preparation untuk Machine Learning
Sebelum masuk ke model, dilakukan persiapan data lanjutan:
- Dataset dibersihkan kembali dan dipilih hanya kolom yang relevan
- Dari total kolom, **17 kolom di-drop**, tersisa **14 kolom**
- Ditemukan 1 kolom dengan missing value di atas 20%, sehingga kolom tersebut dihapus
- Missing value lainnya diimputasi:
  - **Median** untuk data numerik
  - **Modus** untuk data kategorikal

### 6. Feature Engineering (Encoding)
Encoding dilakukan dengan **Label Encoding**, baik untuk fitur ordinal maupun nominal.
> Alasan penggunaan satu jenis encoding:
> Karena model yang digunakan adalah model **tree-based**, yang tidak mengasumsikan urutan atau skala numerik dari hasil encoding.

### 7. Pemodelan & Evaluasi
Model yang digunakan adalah tiga algoritma **tree-based regression**:
- `Random Forest Regressor`
- `XGBoost Regressor`
- `LightGBM Regressor`

#### Evaluasi dilakukan menggunakan metrik:
- `MAE` (Mean Absolute Error)
- `MSE` (Mean Squared Error)
- `RMSE` (Root Mean Squared Error)
- `R²` (R-squared)

> Dari ketiga model, **LightGBM** menghasilkan performa terbaik.

### 8. Hyperparameter Tuning
- **Hyperparameter tuning** dilakukan pada model LightGBM menggunakan `RandomizedSearchCV` dengan **cross-validation**
- Setelah tuning, terjadi penurunan **RMSE sebesar 0,04%**
- Model masih mengalami **overfitting**, namun nilai error menjadi lebih kecil dibanding sebelumnya

### 9. Feature Importance
Setelah model terbaik dipilih dan dituning, dilakukan analisis **feature importance** untuk mengetahui variabel mana saja yang paling berpengaruh terhadap durasi pengantaran.

## Ikhtisar Eksekutif

### Ikhtisar Temuan
<img width="2042" height="1154" alt="Screenshot 2025-07-19 073941" src="https://github.com/user-attachments/assets/80f4728c-ada5-4d75-bc4c-1ffbe2a0e639" />

Berdasarkan hasil analisis data, ditemukan beberapa pola dan insight penting terkait performa pengantaran berdasarkan kelompok usia kurir dan kondisi pengantaran:

- **Total pengantar** sebanyak **1.320 orang**, dan seluruhnya pernah melakukan pengantaran di wilayah **metropolitan**, namun hanya **11,28%** yang pernah ke wilayah **semi-urban**.
- Kurir diklasifikasikan berdasarkan usia ke dalam 4 kategori menurut WHO:
  - **Remaja (15 tahun)**
  - **Dewasa muda (20–29 tahun)**
  - **Dewasa (30–39 tahun)**
  - **Mature (50 tahun)**
- **Remaja** hanya mengantar di wilayah metropolitan dan urban, dan seluruhnya menerima rating **buruk**. Mereka juga memiliki kondisi kendaraan yang **buruk**, dan beberapa informasi penting seperti waktu order, cuaca, dan kepadatan jalan tidak tersedia, sehingga analisis terbatas.
- **Dewasa muda** sebagian kecil menerima rating buruk, yang dikaitkan dengan:
  - **Durasi pick-up** yang lama
  - **Jarak pengantaran yang jauh**
  - **Waktu order di malam hari**
  - **Rating tetap rendah meskipun kendaraan dalam kondisi baik**
- **Dewasa** menerima rating baik secara umum, namun tetap ada kasus rating cukup meskipun kondisi kendaraan dan pengiriman dalam keadaan optimal.
- **Mature** mendapatkan rating **sempurna (6,0)** meskipun mereka mengalami kondisi pengantaran yang beragam — baik dari sisi cuaca, jarak, durasi, maupun festival. Beberapa informasi juga tidak tersedia.

Temuan ini menunjukkan bahwa:
> **Faktor teknis seperti kendaraan, kecepatan pengiriman, dan jarak tidak sepenuhnya menentukan penilaian pelanggan.**  
> Ada kemungkinan bahwa faktor seperti **interaksi kurir, sikap, dan pengalaman pelanggan** turut memengaruhi rating — didukung juga oleh literatur dan domain knowledge.

<img width="1046" height="528" alt="image" src="https://github.com/user-attachments/assets/908752d4-5acd-4911-ba10-b52c8f66a1fa" />

Model **LightGBM** menjadi model terbaik dalam memprediksi `time_taken (min)` dengan RMSE yang menurun sebesar **0,04%** setelah hyperparameter tuning.

Fitur paling berpengaruh dalam model adalah:
1. `Weather_conditions` (kondisi cuaca)
2. `distance_km` (jarak pengantaran)
3. `Road_traffic_density` (kepadatan lalu lintas)
4. `vehicle_condition` (kondisi kendaraan)

Keempat fitur ini merepresentasikan aspek **teknis** dari proses pengantaran. Oleh karena itu, model menyarankan bahwa peningkatan terhadap fitur-fitur ini dapat membantu mempercepat waktu pengantaran dan meningkatkan efisiensi.

### 2. Rekomendasi

Berdasarkan temuan dari analisis data dan hasil pemodelan, berikut adalah beberapa rekomendasi yang dapat dilakukan untuk meningkatkan kualitas layanan pengantaran:

#### Evaluasi Data dan Penilaian

Untuk memperoleh hasil evaluasi performa kurir yang lebih komprehensif, perlu adanya perbaikan dan pelengkapan dalam sistem pencatatan data:

- **Menambahkan dimensi kualitatif** yang dapat mengukur faktor interpersonal, seperti sikap dan interaksi kurir dengan pelanggan
- **Memperbaiki dan mewajibkan pengisian data tertentu** yang masih banyak kosong, terutama pada kategori remaja dan mature
- **Melengkapi sistem penilaian** dengan memberikan opsi alasan dari rating yang diberikan, agar lebih mudah diinterpretasi dan ditindaklanjuti

#### Aspek Fisik Layanan

Untuk meningkatkan efisiensi dan ketepatan waktu pengantaran, berikut rekomendasi dari sisi operasional:

- **Melakukan perawatan kendaraan secara berkala** untuk menjaga performa kurir dan mengurangi risiko keterlambatan
- **Melatih efisiensi proses kerja kurir**, khususnya dalam tahap pick-up hingga delivery
- **Mengoptimalkan rute dan navigasi**, yang dapat dilakukan dengan dukungan teknologi prediksi durasi pengantaran

> Optimalisasi ini dapat memanfaatkan model **LightGBM** yang telah dibangun dalam proyek ini.  
> Berdasarkan hasil analisis **feature importance**, faktor yang paling berpengaruh terhadap durasi pengantaran adalah:
>
> 1. `Weather_conditions` (kondisi cuaca)  
> 2. `distance_km` (jarak pengantaran)  
> 3. `Road_traffic_density` (kepadatan lalu lintas)  
> 4. `vehicle_condition` (kondisi kendaraan)  
>
> Dengan mengetahui faktor-faktor tersebut, perusahaan dapat lebih fokus dalam melakukan perbaikan operasional yang berdampak langsung terhadap waktu pengiriman.

#### Pengembangan Soft Skill Kurir

Selain faktor teknis, hasil analisis menunjukkan bahwa **kurir usia mature** cenderung mendapatkan **rating yang sempurna** dalam berbagai kondisi.

Rekomendasi dari sisi soft skill:

- **Memberikan pelatihan soft skill** kepada kurir lain, terutama dalam hal komunikasi, sikap, dan cara berinteraksi dengan pelanggan
- **Menjadikan kurir mature sebagai role model**, karena performa mereka secara konsisten dinilai baik oleh pelanggan meskipun kondisi teknis mereka tidak selalu optimal

### 3. Dampak Potensial

Berdasarkan hasil analisis dan rekomendasi yang telah diberikan, implementasi perbaikan berpotensi memberikan dampak signifikan dalam berbagai aspek. Dari sisi operasional, perusahaan dapat meningkatkan efisiensi pengantaran dengan memanfaatkan informasi dari model prediktif serta penguatan pada faktor-faktor teknis yang paling berpengaruh, seperti kondisi cuaca, jarak tempuh, kepadatan lalu lintas, dan kondisi kendaraan. Penerapan strategi ini diharapkan mampu menurunkan waktu pengantaran secara keseluruhan dan meningkatkan ketepatan waktu layanan.

Di sisi lain, perbaikan sistem evaluasi dan pencatatan data juga memungkinkan perusahaan untuk menilai kinerja kurir secara lebih adil dan menyeluruh, tidak hanya dari aspek kuantitatif, tetapi juga dari sisi interpersonal dan pengalaman pelanggan. Hal ini dapat meningkatkan akurasi dalam pengambilan keputusan manajerial, seperti pemberian pelatihan, insentif, atau pengelompokan penugasan.

Selain itu, pengembangan soft skill kurir juga berpotensi meningkatkan kepuasan pelanggan secara signifikan. Temuan bahwa kurir berusia mature mendapatkan penilaian yang sangat baik terlepas dari kondisi teknis menunjukkan bahwa aspek sikap dan interaksi juga sangat memengaruhi persepsi layanan. Oleh karena itu, mengadopsi pendekatan yang lebih holistik—menggabungkan optimalisasi teknis dan penguatan soft skill—dapat membentuk standar layanan yang lebih unggul dan berkesinambungan.








