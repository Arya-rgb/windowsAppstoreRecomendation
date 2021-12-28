# Laporan Proyek Machine Learning - Dicky Arya Pratama

## Project Overview

<p align="center">
  <img width="500" height="250" src="https://sm.pcmag.com/pcmag_ap/news/h/how-to-fin/how-to-find-download-microsoft-windows-store-apps_m1qz.jpg" alt="Sumber : https://www.kaggle.com/vishnuvarthanrao/windows-store">
</p>

Pada proyek ini, akan di buat sistem rekomendasi aplikasi windows store apps, windows store apps adalah sebuah platform resmi dari microsoft sebagai tempat download aplikasi komputer yang menggunakan operasi sistem windows, di windows store apps terdapat aplikasi yang berlisensi geratis dan ada yang berbayar, ada banyak aplikasi yang telah di upload di windows app store ini, bahkan sekarang pada windows 11 windows app store sudah terintegrasi dengan amazon app store dan user windows 11 dapat mendownload dan membuka aplikasi android melalui amazon app store.

Di sisi lain, dengan banyak nya aplikasi di windows app store, pastinya pengguna kebingungan untuk mendownload aplikasi mana yang akan di gunakan sesuai minat dan kebutuhan nya, jika kita mencari satu-satu tentunya hal tersebut dapat menyulitkan dan memakan banyak waktu, oleh karna itu semoga dengan ada nya sistem rekomendasi ini dapat membantu user untuk mendownload aplikasi yang sesuai dengan keinginan nya sesuai rekomendasi yang di berikan.

Bagi pengembang, developer yang aplikasi nya tidak di temui oleh user yang membutuhkan karna tidak tahu, ini adalah sebuah hal yang perlu di hindari, dengan sistem rekomendasi user bisa mendapatkan rekomendasi sesuai aplikasi yang dia sukai, oleh karna itu di harapkan dengan sistem ini kedua belah pihak dari sisi developer dan pengguna dapat mendapatkan kemudahan.

berikut adalah [referensi](https://link.springer.com/chapter/10.1007/978-1-4842-0805-2_14) yang dapat anda lihat.

## Business Understanding

### Problem Statements

Setelah mengetahui beberapa masalah diatas, berikut ini merupakan rincian masalah yang perlu diselesaikan di proyek ini:

-   Sistem rekomendasi apa yang baik untuk diterapkan pada kasus ini?
-   Bagaimana cara membuat sistem rekomendasi aplikasi untuk pengguna di windows app store?

### Goals

Berikut adalah tujuan dari dibuatnya proyek ini:

-   Membuat sistem rekomendasi aplikasi untuk pengguna di windows app store.
-   Memberikan rekomendasi untuk aplikasi yang kemungkinan disukai pengguna.

### Solution approach

Gambar dibawah ini adalah diagram alir langkah-langkah yang dilakukan untuk melaksanakan proyek ini :

![Diagram Alir](https://i.ibb.co/txGNnrr/Blank-diagram.png)

Solusi yang di jalankan untuk mencapai tujuan pada proyek ini adalah :

-   Untuk bagian pra-pemrosesan data dilakukan beberapa cara yaitu :

    -   Memperbaiki tipe data pada setiap kolom.
    -   Membersihkan data kosong pada kolom.

-   Untuk bagian persiapan data (sebelum dimasukkan ke model) dilakukan beberapa teknik yaitu :

    -   Konversi label kategori menjadi _one-hot encoding_.
    -   Standarisasi label numerik.

-   Kemudian untuk sistem rekomendasi yang dibuat, dipilih sistem rekomendasi _content based filtering_ karena sesuai dengan karateristik datasetnya. Sehingga sistem rekomendasi dibuat untuk memberikan rekomendasi pada pengguna terhadap aplikasi yang sebelumnya disukai. Beberapa algoritma yang digunakan untuk membuat sistem rekomendasi di proyek ini diantaranya :

    -   model algoritma K-Nearest Neighbor. Algoritma tersebut dipilih karena mudah digunakan dan juga cocok untuk kasus clustering pada sistem rekomendasi. Algoritma ini mengasumsikan bahwa sesuatu yang serupa pasti selalu berdekatan.

        lalu berikut ini merupakan kelebihan dan kekurangan algoritma K-Nearest Neighbor.

        -   Kelebihan :
            -   Algoritmanya mudah digunakan dan sederhana
            -   Algoritmanya sangat fleksibel, dapat diimplementasikan pada kasus klasifikasi, regresi dan pencarian
        -   Kekurangan :
            -   Algoritme menjadi lebih lambat secara signifikan karena jumlah contoh dan variabel yang meningkat.

    -   Model algoritma _cosine similarity_. Algoritma ini dipilih karena mudah digunakan dan juga sebagai pembanding dengan sistem rekomendasi dengan model. _Cosine similarity_ singkatnya digunakan untuk mengukur kemiripan antara dua buah vektor dan kesamaan arahnya dengan cara menghitung sudut kosinus dari kedua vektornya.

## Data Understanding

![Sampul Dataset](https://i.ibb.co/rMYjQQX/download.png)

Tabel dibawah ini merupakan informasi dari dataset yang digunakan :

| Jenis                   | Keterangan                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| Sumber                  | [Kaggle Dataset : Windows App Store](https://www.kaggle.com/vishnuvarthanrao/windows-store) |
| Lisensi                 | CC0: Public Domain                                                                 |
| Kategori                | Business, arts and entertaiment, computer science, software                                                                 |
| Rating Penggunaan       | 10.0                                                                                       |
| Jenis dan Ukuran Berkas | CSV (300KB)                                                                                       |

Berkas windows_store.csv berisi data mengenai aplikasi yang tersedia di windows app store, terdapat lebih dari 5000 data pada dataset ini, berikut adalah uraian variabel dari setiap kolom pada dataset :

1. Kolom Rating merupakan kolom dengan data rating oleh user.

2. Kolom No of people Rated merupakan kolom dengan data jumlah user yang memberikan rating.

3. Kolom Category merupakan kolom dengan data kategori aplikasi.

4. Kolom Date merupakan kolom dengan data tanggal aplikasi di upload ke windows app store.

5. Kolom Price merupakan kolom dengan data harga dari aplikasi.

6. Kolom Name merupakan kolom dengan data nama dari aplikasi.

## Data Preparation

berikut adalah tahapan-tahapan dalam melakukan pra-pemrosesan data :

-   Memperbaiki tipe data pada setiap kolom. Hal ini dilakukan karena tipe data pada setiap kolom belum sesuai dengan data di kolomnya, selain itu juga memperbaiki data yang ada di kolomnya. Proses yang dilakukan pada setiap kolom berbeda-beda, berikut ini merupakan rinciannya :
    -   Kolom Rating : Menghapus data yang value nya kosong pada baris 5321
    -   Kolom No of people Rated : sudah benar berupa angka dan bertipe integer, baris 5321 pada kolom ini juga akan di hapus karna data yang lain nya kosong.
    -   Kolom Category : Menghapus data yang value nya kosong pada baris 5321
    -   Kolom Date : Menghapus simbol "-" lalu di jadikan int, Menghapus data yang value nya kosong pada baris 5321
    -   Kolom Price : Menghapus simbol "₹", ".", "space kosong", "," dan mengganti "free" menjadi "0" lalu di jadikan int, Menghapus data yang value nya kosong pada baris 5321
    -   Kolom Name : menghapus/drop value kosong pada baris 5321
    Dengan memperbaiki data pada kolomnya, tipe data kolom pun secara otomatis berubah.
-   Membersihkan data kosong pada kolom Rating, Category, Date dan Price. Hal ini dilakukan karena data yang tidak ada value nya sangat sedikit, jadi lebih baik di drop untuk kasus ini.
-   Konversi label kategori menjadi _one-hot encoding_. Hal ini dilakukan untuk memudahkan pencarian nilai terdekat dari setiap aplikasi. Sebelumnya data ini merupakan data kategori dan dirubah menjadi data numerik yang ada pada setiap kolom yang berbeda kategori. Proses ini dilakukan menggunakan _method_ `get_dummies` pada kolom _dataframe_ dari dataset yang selanjutnya datanya disatukan pada _dataframe_.
-   Standarisasi label numerik. Hal ini dilakukan agar rentang nilai pada label numerik hanya antara 0-1 sehingga dapat mempercepat komputasinya. Selain itu standarisasi juga membuat semua label numerik memiliki rentang nilai yang sama. Proses ini dilakukan menggunakan MinMaxScaler dari sklearn.

## Modeling

Setelah dilakukan pra-pemrosesan data, selanjutnya kita bisa membuat sistem rekomendasi _content based filtering_.

1. menggunakan model Nearest Neighbor (NN)

    Untuk membangun model ini, digunakan fungsi [NearestNeighbor](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html) dari sklearn dengan parameter metriksnya yakni euclidian. Kemudian fungsi tersebut di inisiasikan sebagai model yang selanjutnya dilakukan _fitting_ terhadap data yang ada pada _dataframe_. Setelah itu dibuat fungsi `getRecommendedApps` untuk memberikan rekomendasi terhadap suatu nama aplikasi yang dimana skenarionya adalah apabila pengguna menyukai atau mengunduh aplikasi tersebut, maka berikan rekomendasi ini sebagai aplikasi yang mungkin disukai. Hasil rekomendasinya adalah seperti berikut :
    
    ![hasil_rekomendasi](https://i.ibb.co/rvXbC1H/revisi.png)
    
    parameter yang saya pilih untuk model Nearest Neighbor(NN) ini adalah Euclidean distance karna dalam kasus ini terdapat lebih dari 2 independent variables, untuk menghitung jaraknya kita bisa gunakan rumus Euclidean Distance. Mirip dengan Pythagoras, hanya saja Euclidean Distance memiliki dimensi lebih dari 2, berikut adalah formula nya:
    
    ![euclidean](https://miro.medium.com/max/814/0*_a-87NPzfpmUwOd-.png)

2. Dengan _cosine similarity_

    Selanjutnya, rekomendasi pun dapat diberikan dengan menghitung _cosine similarity_ dari setiap data di dataset menggunakan fungsi [cosine_similarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html) dari sklearn. Prosesnya adalah dengan memanggil fungsi `cosine_similarity` dengan argumen _dataframe_ sebagai objeknya. Kemudian hasil dari perhitungannya disimpan pada dataframe baru. Untuk tahapan pemberian rekomendasinya, dibuat fungsi `getRecommendedApps_cosine` dimana fungsi tersebut akan memberikan rekomendasi terhadap suatu nama aplikasi dengan skenario yang sama dengan nomor 1, agar model tidak melakukan prediksi yang salah dan menghasilkan nilai False Positive, harus melakukan vektorisasi terlebih dahulu terhadap feature pembangun didalam dataset menggunakan TF-IDF Vectorizer, Hasil rekomendasinya adalah seperti berikut :
    
    ![cosine_similarity_hasil](https://i.ibb.co/DpWCx85/image-2021-11-02-140234.png)
    
    Dari kedua model yang telah di coba model Nearest Neighbor (NN) memiliki performa yang lebih baik, bisa di lihat dari akurasi kesamaan data yang di rekomendasikan oleh user, jadi di sini saya menempatkan model Nearest Neighbor (NN) sebagai model terbaik sebagai solusi.

## Evaluation

Untuk mengukur kinerja model NN untuk sistem rekomendasi, mertrik yang di gunakan adalah sebagai berikut :

1. Skor Calinski Harabasz

    Indeks Calinski-Harabasz juga dikenal sebagai Variance Ratio Criterion, adalah rasio jumlah dispersi antar-cluster dan dispersi antar-cluster untuk semua cluster, semakin tinggi skornya, semakin baik kinerjanya. . 

    Kelebihan dari metriks ini adalah :

    - Skornya tinggi apabila kluster padat dan terpisah dengan baik, yang mana bergantung pada konsep standar dari sebuah kluster.
    - Skornya cepat untuk dihitung.

    Sedangkan kekurangannya :

    - Metriks ini hanya baik digunakan pada kasus _convex cluster_.

    Penerapannya pada kode adalah dengan menggunakan fungsi calinski_harabasz_score dari sklearn. Fungsi tersebut menerima argumen dari sebuah data yang digunakan untuk membuat model dan labelnya. Berikut ini adalah hasil penerapannya pada model NN.

    Pada model ini, terlihat bahwa kluster belum padat dan terpisahkan dengan baik karena nilai skornya masih cukup rendah. Kesalahan pada rekomendasi dimungkinkan, data rekomendasi masih ada yang tidak sesuai dengan aplikasi yang di sukai oleh user, berikut adalah cara metric bekerja untuk mengukur akurasi dari model:
    
    ![Calinski Harabasz](https://i.ibb.co/ZmKF3TW/1-7-FFw-FXlz7t-MPq-Zq-T72v-Pi-A.png)
    
    dan berikut adalah hasil hasil evaluasi model menggunakan Calinski Harabasz:
    
    ![hasil_evaluasi](https://i.ibb.co/hcL2fSY/Screenshot-from-2021-10-31-20-57-40.png)
    
--bagian akhir dari laporan--