# Analisis Graf dan Sistem Rekomendasi Produk RetailRocket

## Deskripsi Proyek

Proyek ini merupakan tugas Ujian Tengah Semester (UTS) mata kuliah Graf Sains Data. Proyek menggunakan dataset **RetailRocket E-commerce** untuk menganalisis interaksi pengguna dengan produk menggunakan pendekatan graf dan pemodelan vektor.

Data interaksi dimodelkan ke dalam graph database Neo4j untuk menggambarkan hubungan antara pengguna dan item. Selanjutnya, dilakukan analisis kemiripan antar-item serta penerapan metode *K-Nearest Neighbors* (KNN) untuk menghasilkan rekomendasi produk.

## Tujuan

- Melakukan preprocessing dan filtering data interaksi berdasarkan periode waktu.
- Membentuk graf pengguna dan item menggunakan Neo4j.
- Menganalisis struktur graf dan karakteristik matriks interaksi.
- Menghitung kemiripan antar-item menggunakan cosine similarity.
- Menghasilkan rekomendasi item berdasarkan kemiripan perilaku pengguna.
- Mengevaluasi hasil rekomendasi menggunakan data periode pengujian.

## Dataset

Dataset yang digunakan adalah [RetailRocket E-commerce Dataset](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset).

Dataset berisi catatan interaksi pengguna terhadap produk, meliputi:

| Kolom           | Keterangan                         |
| --------------- | ---------------------------------- |
| `timestamp`     | Waktu terjadinya interaksi         |
| `visitorid`     | Identitas pengguna                 |
| `event`         | Jenis interaksi pengguna           |
| `itemid`        | Identitas produk                   |
| `transactionid` | Identitas transaksi, jika tersedia |

## Pembagian Periode Data

Data dibagi menjadi dua periode untuk proses pelatihan dan pengujian.

| Periode | Rentang Waktu        | Fungsi        |
| ------- | -------------------- | ------------- |
| P1      | 3 Mei–12 Juni 2015   | Data training |
| P2      | 13 Juni–25 Juli 2015 | Data testing  |

Data difilter berdasarkan timestamp, kemudian disimpan dalam file CSV terpisah, yaitu `events_P1.csv` dan `events_P2.csv`.

## Teknologi yang Digunakan

- **Python** — preprocessing dan analisis data.
- **Pandas & NumPy** — manipulasi data.
- **SciPy** — pembentukan sparse matrix.
- **Scikit-learn** — normalisasi matriks dan perhitungan kemiripan.
- **Neo4j** — penyimpanan dan pengelolaan graf.
- **Cypher** — query untuk mengakses node dan relationship.
- **Jupyter Notebook** — dokumentasi dan pelaksanaan analisis.

## Pemodelan Graf

Graf dibangun menggunakan dua jenis node utama:

- `Visitor`: merepresentasikan pengguna.
- `Item`: merepresentasikan produk.

Relationship yang digunakan:

| Relationship    | Makna                                    |
| --------------- | ---------------------------------------- |
| `VIEWED`        | Pengguna melihat produk                  |
| `ADDED_TO_CART` | Pengguna menambahkan produk ke keranjang |
| `BOUGHT`        | Pengguna membeli produk                  |
| `SIMILAR_TO`    | Menunjukkan kemiripan antar-item         |
| `KNOWS`         | Relationship dari data tutorial Neo4j    |

## Metode Analisis

### 1. Data Wrangling dan Exploratory Data Analysis

Data interaksi difilter berdasarkan periode P1 dan P2. Selanjutnya, dilakukan pemeriksaan jumlah node dan relationship untuk mengetahui komposisi graf yang terbentuk.

### 2. Vector Space Modelling (VSM)

Interaksi pengguna dan item direpresentasikan dalam bentuk matriks sparse. Pada matriks berbobot, interaksi diberi bobot sebagai berikut:

- View: 1
- Add to cart: 2
- Transaction: 3

Matriks digunakan untuk merepresentasikan pola interaksi pengguna terhadap produk.

### 3. Item Similarity

Kemiripan antar-item dihitung menggunakan **cosine similarity** berdasarkan pola interaksi pengguna. Hasilnya digunakan untuk membentuk relationship `SIMILAR_TO` pada Neo4j.

### 4. Sistem Rekomendasi KNN

Sistem rekomendasi menggunakan pendekatan *user-based K-Nearest Neighbors*. Pengguna dengan pola interaksi yang mirip dicari berdasarkan cosine similarity. Item yang berinteraksi dengan pengguna tetangga digunakan untuk menghasilkan kandidat rekomendasi yang belum pernah diinteraksikan oleh pengguna target.

### 5. Evaluasi Rekomendasi

Hasil rekomendasi dievaluasi menggunakan data periode P2. Metrik yang digunakan adalah:

- **Hit\@5** — memeriksa apakah terdapat item relevan dalam daftar rekomendasi.
- **NDCG\@5** — mengukur kualitas urutan rekomendasi berdasarkan relevansinya.

## Struktur Proyek

Struktur direktori proyek secara umum:

```text
UTS-Graf-SADA/
├── myenv
├── data/
│   ├── events_filtered.csv
│   ├── events_P1.csv
│   └── events_P2.csv
├── notebooks/
│   └── UTS-Graf-SADA.ipynb
├── README.md
└── requirements.txt
```

*Sesuaikan nama dan lokasi notebook dengan struktur folder yang benar di repositori.*

## Instalasi dan Penggunaan

### 1. Clone repositori

```bash
git clone <URL_REPOSITORY>
cd UTS-Graf-SADA
```

### 2. Instal dependensi

```bash
pip install pandas numpy scipy scikit-learn neo4j jupyter
```

### 3. Siapkan Neo4j

Jalankan database Neo4j, lalu sesuaikan URI, username, dan password pada konfigurasi koneksi Python. Jangan menyimpan password asli di repositori publik.

### 4. Siapkan dataset

Unduh dataset RetailRocket dari Kaggle dan letakkan file yang diperlukan pada direktori `data/`.

### 5. Jalankan notebook

Buka notebook menggunakan Jupyter Notebook atau VS Code, lalu jalankan setiap cell secara berurutan sesuai tahapan analisis.

## Hasil yang Diharapkan

Proyek ini menghasilkan:

- Dataset interaksi yang telah difilter berdasarkan periode.
- Graf pengguna dan item pada Neo4j.
- Matriks interaksi pengguna-item dan item-pengguna.
- Relationship kemiripan antar-item.
- Daftar rekomendasi produk untuk pengguna target.
- Evaluasi rekomendasi menggunakan data periode pengujian.

## Catatan

- Hasil rekomendasi bergantung pada data interaksi dan parameter yang digunakan.
- Nilai evaluasi perlu diinterpretasikan berdasarkan jumlah item relevan dan jumlah rekomendasi yang berhasil dihasilkan.
- Dataset RetailRocket digunakan untuk keperluan pembelajaran dan analisis data.

## Author

**Daris Ikhwana Khoir Suhaya**
Program Studi S1 Sains Data
Universitas Negeri Surabaya

Proyek Ujian Tengah Semester — Graf Sains Data, 2026.
