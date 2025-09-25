# delivery-performance-and-prediction
## Latar Belakang Proyek
Dataset yang digunakan berasal dari salah satu perusahaan layanan pesan antar makanan yaitu **Zomato**. Zomato merupakan platform berbasis di India yang menyediakan layanan pemesanan dan pengantaran makanan dari berbagai kategori. Layanan ini beroperasi di tiga jenis wilayah yaitu **metropolitan**, **urban**, dan **semi-urban**.

Dataset ini mencatat data historis selama **3 bulan di tahun 2022**, yaitu bulan **Februari, Maret, dan April**. Meskipun hanya mencakup 3 bulan, jumlah pesanan yang tercatat mencapai **45.584 pesanan**, yang menunjukkan tingginya volume penggunaan layanan Zomato dalam periode tersebut.

Dalam proyek ini, dilakukan analisis untuk mengevaluasi **performa pengantaran dari perspektif kurir**. Fokusnya adalah melihat faktor-faktor apa saja yang mempengaruhi performa mereka dalam mengantarkan pesanan. Selain itu, juga dibangun **model prediksi** untuk memperkirakan durasi pengantaran berdasarkan variabel-variabel tertentu. 

Tujuan dari proyek ini adalah memberikan masukan yang dapat digunakan untuk meningkatkan efisiensi pengantaran, serta mendukung proses pengambilan keputusan dalam pengelolaan operasional kurir.

## Struktur Data dan Pemeriksaan Awal
Dataset ini terdiri dari **20 kolom** dan **45.584 baris** yang merepresentasikan aktivitas pengantaran makanan dalam periode 3 bulan. Secara garis besar, data memuat informasi terkait:
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








