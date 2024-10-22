# Laporan Proyek Machine Learning - VERY

## Project Overview

Dalam era digital saat ini, permintaan akan anime terus meningkat, membuat pengguna kesulitan menemukan judul yang sesuai dengan preferensi mereka. Rekomendasi yang tidak tepat dapat mengurangi pengalaman pengguna dan keterlibatan dengan platform streaming.

Sistem rekomendasi berbasis pengguna sering kali tidak efektif untuk pengguna baru yang belum memiliki riwayat penonton. Oleh karena itu, diperlukan pendekatan content-based filtering yang memanfaatkan fitur-fitur spesifik anime, seperti tipe (TV, movie) dan genre.

Proyek ini bertujuan mengembangkan sistem rekomendasi anime menggunakan dataset dari MyAnimeList. Dengan menganalisis karakteristik setiap judul, sistem ini akan memberikan rekomendasi yang relevan dan personal, meningkatkan kepuasan dan retensi pengguna di platform streaming.

## Business Understanding

### Problem Statements

Menjelaskan pernyataan masalah:

1. Bagaimana cara mengembangkan sistem rekomendasi anime yang efektif menggunakan metode content-based filtering berdasarkan fitur-fitur spesifik seperti tipe anime (TV, movie) dan genre?
2. Apa saja karakteristik anime yang paling memengaruhi preferensi pengguna, dan bagaimana hubungan antar karakteristik tersebut dalam menentukan rekomendasi yang tepat?
3. Bagaimana penerapan sistem rekomendasi ini dapat meningkatkan pengalaman pengguna dan retensi di platform streaming anime?

### Goals

Menjelaskan tujuan proyek yang menjawab pernyataan masalah:

1. Mengembangkan sistem rekomendasi anime yang mampu memberikan saran yang relevan dan personal berdasarkan analisis fitur-fitur anime, seperti tipe dan genre.
2. Mengidentifikasi karakteristik utama yang memengaruhi preferensi pengguna dan memahami bagaimana karakteristik tersebut berkontribusi pada rekomendasi yang diberikan.
3. Menerapkan sistem rekomendasi untuk meningkatkan kepuasan pengguna dan keterlibatan di platform streaming anime, sehingga menciptakan pengalaman menonton yang lebih baik dan meningkatkan loyalitas pengguna.

## Data Understanding

Dataset yang digunakan untuk proyek ini di upload oleh [NIKHIL ](https://www.kaggle.com/nikhil1e9) pada :

Dataset: [Anime and Manga dataset (2023)](https://www.kaggle.com/datasets/nikhil1e9/myanimelist-anime-and-manga)

### Kondisi Dataset

Dataset ini terdapat dalam format zip dengan dua file: anime.csv dan manga.csv. Saya menggunakan anime.csv yang memiliki:

- Jumlah Data: 12,774 baris data
- Jumlah Kolom: 10 kolom

Namun, untuk kasus ini, saya hanya menggunakan 6 variabel berikut:

- Title: Judul anime
- Rank: Peringkat anime berdasarkan popularitas atau rating
- Type: Tipe anime (misalnya, TV, Movie, OVA)
- Episodes: Jumlah episode yang tersedia
- Aired: Tanggal tayang anime
- Members: Jumlah anggota yang mengikuti atau memberikan rating pada anime
- Score: Nilai rata-rata dari penilaian yang diberikan oleh pengguna

menggunakan mentode isnull.().sum() terdapat 0 nilai null sehingga tidak perlu dilakukan data cleaning untuk missing value

## Data Preparation

### Unknown Value Processing

Beberapa kolom, seperti kolom Episodes, mungkin memiliki nilai yang tidak diketahui (misalnya, '?'). saya mengganti nilai tersebut dan mengolah informasi yang ada. saya membuat kolom baru yang menunjukkan status anime (Selesai/Ongoing)

### TF-ID vectorizer

Pada proyek ini, model rekomendasi menggunakan metode content-based filtering yang bergantung pada kesamaan antar anime berdasarkan fitur Type (seperti TV, Movie, dll.). Karena model machine learning tidak dapat memproses data teks secara langsung, diperlukan teknik untuk mengubah data teks tersebut menjadi representasi numerik. Salah satu teknik yang digunakan adalah TF-IDF Vectorizer (Term Frequency-Inverse Document Frequency).

TF-IDF adalah metode yang sering digunakan dalam pemrosesan teks untuk merepresentasikan dokumen (atau dalam konteks ini, anime) sebagai vektor. Teknik ini menggabungkan dua metrik:

Pada proyek ini, kami menggunakan TfidfVectorizer dari library sklearn untuk menghitung nilai TF-IDF dari kolom Type. Setelah menghitung nilai TF-IDF, data teks Type anime diubah menjadi matriks sparse yang berisi representasi numerik. Setiap anime dalam dataset direpresentasikan sebagai vektor yang menunjukkan seberapa signifikan Type tersebut untuk masing-masing anime.

## Modeling

Setelah melakukan data preparation dan mengubah fitur Type menjadi representasi numerik menggunakan TF-IDF Vectorizer, langkah selanjutnya adalah membangun model untuk memberikan rekomendasi anime. Pada proyek ini, model menggunakan metode cosine similarity untuk mengukur kesamaan antar anime berdasarkan tipe (TV, Movie, dll.).

### proses modeling

1. Saya menghitung cosine similarity pada matriks TF-IDF yang telah dibuat sebelumnya untuk fitur Type dari anime. Ini menghasilkan matriks yang menunjukkan seberapa mirip setiap anime dengan yang lainnya.
2. Selanjutnya, saya membuat fungsi recommend_anime yang menerima judul anime sebagai input dan mengembalikan beberapa rekomendasi berdasarkan nilai cosine similarity yang telah dihitung. Fungsi ini memungkinkan pengguna untuk mendapatkan saran anime yang relevan berdasarkan preferensi mereka.
3. =Saya kemudian menguji fungsi rekomendasi dengan judul anime yang berbeda untuk melihat hasilnya. Ini memberikan gambaran tentang seberapa baik model dapat merekomendasikan anime yang mirip.

### Cosine Similarity

Cosine similarity adalah metrik yang digunakan untuk mengukur kesamaan antara dua vektor berdasarkan sudut di antara keduanya. Nilai cosine similarity berkisar antara 0 hingga 1, di mana:

- 1 berarti kedua vektor memiliki arah yang sama, yang menunjukkan bahwa kedua anime sangat mirip.
- 0 berarti kedua vektor tidak memiliki kesamaan.

Cosine Similarity adalah metode yang digunakan untuk mengukur seberapa mirip dua vektor dalam ruang multidimensi. Dalam konteks sistem rekomendasi, khususnya dalam content-based filtering, cosine similarity sering digunakan untuk menentukan seberapa mirip dua item (seperti film, anime, atau produk) berdasarkan fitur yang diwakili oleh vektor

Pada proyek ini, cosine similarity digunakan untuk mengukur kesamaan antar anime berdasarkan vektor TF-IDF yang dihasilkan dari fitur Type. Dengan mengukur sudut antara vektor, model dapat menemukan anime yang paling mirip berdasarkan tipe.

## Evaluation and Results

### Result

Setelah menerapkan model rekomendasi berbasis content-based filtering menggunakan cosine similarity, model berhasil memberikan rekomendasi anime yang relevan berdasarkan fitur Type.

jika mencoba fitur rekomendasi dengan type TV seperti 'Re:Zero kara Hajimeru Isekai Seikatsu maka akan didapatkan hasil berikut:
| Title | Type |
| :--------------------------------------- | :--: |
| Net-juu no Susume | TV |
| Narutaru: Mukuro Naru Hoshi Tama Taru Ko | TV |
| Tenchi Souzou Design-bu | TV |
| Fantastic Children | TV |
| Duel Masters!! | TV |

jike mencoba input anime dengan Type movie seperti 'Koe no Katachi' maka akan didapatkan hasil seperti berikut:

| Title                                             | Type  |
| :------------------------------------------------ | :---: |
| Dr. Slump Movie 03: Arale-chan Hoyoyo! Sekai I    | Movie |
| Hi no Tori: Houou-hen                             | Movie |
| Armageddon                                        | Movie |
| Hinomaru Hatanosuke: Inazuma-gumi Tobatsu no Maki | Movie |
| Ninja Hattori-kun: Nin Nin Ninpo Enikki no Maki   | Movie |

Untuk mengevaluasi model rekomendasi, metrik Precision digunakan. Akurasi dalam konteks ini mengukur seberapa tepat rekomendasi yang diberikan model berdasarkan tipe anime yang sesuai. Akurasi dihitung sebagai rasio jumlah anime yang memiliki tipe yang sama dengan anime input terhadap total anime yang direkomendasikan (k).

rumus untuk perhitungan Precision adalah

$$
\text{Precision} = \left( \frac{\text{Number of Relevant Recommendations}}{\text{Total Recommendations}} \right) \times 100
$$



Pengujian berdasarkan result menunjukkan bahwa model mencapai 100% Precision dalam memberikan rekomendasi. Setiap rekomendasi yang diberikan sesuai dengan preferensi pengguna, menunjukkan bahwa model dapat dengan efektif mengidentifikasi anime yang mirip berdasarkan karakteristik yang ada.

berdasarkan problem statement :

1. Model rekomendasi belum sepenuhnya dapat memberikan rekomendasi yang relevan. Meskipun mencapai akurasi 100% pada pengujian awal, diperlukan evaluasi lebih lanjut untuk memastikan konsistensi rekomendasi di berbagai konteks anime yang berbeda.

2. Hubungan antara fitur Type dan preferensi pengguna masih perlu dieksplorasi lebih dalam. Meskipun model memberikan rekomendasi berdasarkan kemiripan, tidak ada analisis mendalam tentang faktor lain yang mungkin mempengaruhi preferensi pengguna.

3. Dengan model yang hanya mengandalkan fitur Type, kemampuan sistem untuk memahami preferensi pengguna menjadi terbatas. Pengguna mungkin merasa kurang terlayani jika tidak ada rekomendasi yang lebih variatif dan disesuaikan.

sehingga goals :

1. Meskipun model menunjukkan hasil yang baik dalam pengujian awal, diperlukan evaluasi lebih lanjut untuk memastikan konsistensi rekomendasi di berbagai konteks anime. Hal ini penting karena preferensi pengguna mungkin bervariasi, dan model perlu diuji dengan dataset yang lebih luas untuk memahami apakah rekomendasi tetap relevan dalam berbagai situasi. Saat ini, model masih terbatas pada fitur Type, sehingga rekomendasi yang lebih variatif diperlukan.

2. Tujuan untuk memperkaya model dengan analisis lebih dalam terhadap fitur lain belum sepenuhnya tercapai. Dengan hanya bergantung pada fitur Type, model saat ini mungkin kehilangan nuansa dalam memahami preferensi pengguna. Oleh karena itu, diperlukan pengembangan lebih lanjut untuk mengintegrasikan fitur-fitur tambahan yang dapat memberikan wawasan yang lebih komprehensif tentang preferensi pengguna.

3. Meskipun model telah mencapai akurasi tinggi, pendekatan saat ini cenderung menghasilkan rekomendasi yang homogen dan mungkin tidak memenuhi ekspektasi pengguna yang menginginkan variasi dalam pilihan anime. Dengan meningkatkan model untuk mempertimbangkan lebih banyak fitur dan karakteristik, seperti genre, rating, dan bahkan sinopsis, sistem rekomendasi dapat menjadi lebih disesuaikan dan memberikan pengalaman menonton yang lebih memuaskan.

**---Ini adalah bagian akhir laporan---**
