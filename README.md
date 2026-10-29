# Batik Lestari: Analisis Penjualan Q3 2026

Analisis menggunakan Python di Google Colab untuk membersihkan catatan transaksi Q3, menjawab pertanyaan bisnis, dan menyusun rekomendasi untuk Q4.

![Pratinjau dashboard](Batik%20Lestari%20Q3%20Sales%20Dashboard.png)

[Lihat laporan dashboard](Batik%20Lestari.pdf) | [Buka notebook analisis](Batik_Lestari.ipynb)


## Problem

Menjelang kampanye Q4 dan musim belanja akhir tahun, Batik Lestari perlu mengetahui channel penjualan yang paling berkontribusi, produk yang unggul berdasarkan jumlah item maupun nilai penjualan, serta masalah pembatalan pesanan yang perlu ditangani. Data transaksi awal mengandung baris duplikat yang identik, penulisan teks yang tidak konsisten, dan nilai kosong. Karena itu, kualitas data perlu diperiksa sebelum analisis dilakukan.

## Data

Dataset ini mencakup periode 1 Juli sampai 30 September 2026 dan berisi 1.206 baris serta 12 kolom. Satu baris mewakili satu catatan pesanan. Kolomnya meliputi tanggal pesanan, channel, kota pembeli, kategori produk, produk, harga satuan, jumlah, diskon, metode pembayaran, status pesanan, dan rating.

Keputusan pembersihan data didokumentasikan secara eksplisit:

| Masalah | Bukti | Keputusan |
|---|---|---|
| Duplikat identik | 26 baris memiliki nilai yang sama pada seluruh kolom | Menghapus 26 baris duplikat sehingga tersisa 1.180 baris dan 1.180 ID pesanan unik |
| Penulisan tidak konsisten | Tujuh variasi nilai mentah mewakili empat channel penjualan; penulisan status pesanan juga berbeda kapitalisasinya | Menghapus spasi berlebih dan menyeragamkan penulisan channel serta status pesanan |
| Kota kosong | 31 baris tidak memiliki informasi kota pembeli | Mengganti nilai kosong dengan `Tidak Diketahui`; tetap dihitung dalam total keseluruhan, tetapi tidak dimasukkan ke pemeringkatan kota |
| Rating kosong | 268 rating kosong: 84 pesanan dibatalkan, 174 pesanan selesai tanpa rating, dan 10 pesanan dikembalikan | Membiarkan rating tetap kosong agar tidak membuat nilai yang tidak didukung data |
| Tipe tanggal | Tanggal pesanan tersimpan sebagai teks | Mengonversinya menjadi tipe datetime untuk analisis harian dan bulanan |

**Definisi pendapatan:** analisis ini hanya menghitung pesanan dengan status `Selesai` sebagai penjualan setelah diskon. Terdapat 1.062 pesanan selesai dengan nilai penjualan setelah diskon sebesar Rp276,4 juta. Pesanan yang dibatalkan dan dikembalikan tidak dimasukkan ke total penjualan tersebut, tetapi tetap digunakan dalam analisis status pesanan dan pembatalan.

```text
pendapatan_bersih = harga_satuan x jumlah x (1 - diskon_persen / 100)
```

## Approach

1. **Inspeksi:** memeriksa bentuk data, tipe data, statistik deskriptif, variasi nilai teks, duplikasi, dan nilai kosong.
2. **Pembersihan:** menghapus duplikat identik, menyeragamkan label, menangani kota dan rating kosong secara eksplisit, serta mengonversi tanggal dan kolom numerik.
3. **Pembentukan variabel:** menghitung nilai pesanan setelah diskon dan mengambil informasi bulan dari tanggal pesanan.
4. **Analisis:** membandingkan penjualan per channel, jumlah dan nilai penjualan produk, tren bulanan dan tanggal kembar, tingkat pembatalan menurut metode pembayaran, jumlah pesanan per kota, serta rating per kategori.
5. **Visualisasi:** menggunakan diagram batang, diagram garis, dan visualisasi dengan Seaborn untuk menyampaikan temuan.
6. **Rekomendasi:** menerjemahkan hasil analisis menjadi prioritas praktis untuk Q4.

## Result

- **Channel:** Shopee menghasilkan Rp95,5 juta dan TikTok Shop Rp82,7 juta. Keduanya menyumbang 64,5% dari total Rp276,4 juta penjualan pesanan selesai setelah diskon.
- **Produk:** Aksesoris memiliki jumlah item terjual terbanyak, yaitu 871 item. Sementara itu, Kemeja Pria menghasilkan nilai penjualan tertinggi sebesar Rp85,3 juta. Produk Kemeja Batik Parang Lengan Pendek menyumbang Rp45,3 juta dari penjualan pesanan selesai setelah diskon.
- **Tren bulanan:** penjualan setelah diskon meningkat dari Rp91,6 juta pada Juli menjadi Rp99,7 juta pada Agustus, lalu turun menjadi Rp85,2 juta pada September. Penurunan terbesar terjadi pada TikTok Shop (Rp6,8 juta), Shopee (Rp4,2 juta), dan toko offline (Rp3,4 juta).
- **Tanggal kembar:** penjualan pesanan selesai pada 7.7, 8.8, dan 9.9 mencapai sekitar Rp6,0 juta sampai Rp7,0 juta per hari, dibandingkan rata-rata sekitar Rp2,9 juta pada hari biasa. Pesanan pada tanggal kembar juga mencatat rata-rata diskon lebih tinggi (18,9% dibandingkan 5,5%) dan rata-rata nilai pesanan terhitung lebih rendah (sekitar Rp217 ribu dibandingkan Rp264 ribu). Ini merupakan pola yang teramati, bukan bukti bahwa diskon menyebabkan peningkatan volume.
- **Pembatalan:** 14,6% pesanan COD dibatalkan, dibandingkan 5,2% untuk pesanan non-COD di marketplace. Nilai terhitung seluruh pesanan yang dibatalkan mencapai Rp19,5 juta, dengan Rp11,2 juta terkait COD. Angka ini menunjukkan nilai pesanan yang dibatalkan, bukan estimasi pendapatan yang dapat dipulihkan atau kerugian laba yang telah terverifikasi.
- **Kota:** Jakarta mencatat pesanan terbanyak, yaitu 267 pesanan (22,6%), diikuti Pekalongan (165) dan Surabaya (149). Karena dataset tidak memiliki ID pembeli, angka ini menunjukkan jumlah pesanan, bukan jumlah pelanggan unik.
- **Rating:** rata-rata per kategori berkisar dari 4,03 hingga 4,21, dengan Kain Batik memiliki rata-rata tertinggi sebesar 4,21. Karena 268 rating kosong, rata-rata ini hanya menggambarkan catatan yang memiliki rating.

## Impact

Hasil analisis mendukung tiga tindakan praktis untuk Q4:

1. **Prioritaskan dua marketplace utama.** Shopee dan TikTok Shop menyumbang 64,5% penjualan pesanan selesai setelah diskon. Rencanakan kesiapan stok dan kampanye setiap channel berdasarkan kontribusi tersebut.
2. **Rencanakan stok berdasarkan nilai penjualan dan jumlah item.** Kemeja Pria unggul dari sisi nilai penjualan, sedangkan Aksesoris unggul dari sisi jumlah item. Tetapkan prioritas stok yang berbeda untuk masing-masing kategori dan evaluasi kampanye berdasarkan pesanan selesai serta nilai penjualan, bukan jumlah pesanan saja.
3. **Uji proses konfirmasi COD.** Tingkat pembatalan COD mencapai 14,6%. Uji konfirmasi sebelum pengiriman, lalu pantau tingkat pembatalan, jumlah pesanan selesai, dan biaya operasional sebelum memutuskan apakah proses tersebut perlu diperluas.

Rekomendasi ini berasal dari analisis data Q3, bukan hasil yang sudah diukur dari kampanye Q4 yang telah dijalankan. Bundling Aksesoris dengan produk bernilai lebih tinggi juga masih berupa hipotesis yang perlu diuji: dataset mencatat jumlah item, tetapi tidak menunjukkan apakah satu pesanan berisi beberapa produk yang berbeda.

## Tools

Python, pandas, NumPy, Matplotlib, Seaborn, dan Google Colab.

## Isi Repository

```text
Batik Lestari/
  README.md
  Batik Lestari Q3 Sales Dashboard.png
  Batik_Lestari.ipynb
  Batik Lestari.pdf
```

**Kebutuhan dataset:** file CSV sumber belum disertakan dalam folder ini. Notebook membutuhkan file bernama `Batik Lestari Q3 Sales.csv` di direktori kerja. Untuk menjalankan ulang notebook, tambahkan dataset yang diizinkan untuk dibagikan ke folder atau unggah ke runtime Google Colab. Repository belum sepenuhnya dapat direproduksi sampai file input tersebut tersedia bagi orang yang menjalankannya.
