# Movie-Recommendation-System

## Project Overview

Industri perfilman adalah salah satu sektor hiburan yang berkembang pesat secara global. Dengan hadirnya berbagai platform digital yang sudah banya digunakan, jumlah film yang tersedia bagi pengguna semakin meningkat. Namun, dengan bertambahnya jumlah film yang ada, pengguna, bahkan saya sendiri kadang mengalami kebingungan dalam memilih film yang ingin ditonton. Berdasarkan riset oleh Menurut Gomez-Uribe dan Hunt (2015), sekitar 80% dari total waktu yang ditonton pengguna netflix berasal dari konten yang muncul dari sistem rekomendasi, yang menunjukkan bahwa sistem rekomendasi memainkan peran penting dalam meningkatkan pengalaman pengguna dan keterlibatan konsumen. Salah satu solusi untuk permasalahan ini adalah dengan membangun sistem rekomendasi film berbasis data. 

Sumber
Gomez-Uribe, C. A., & Hunt, N. (2015). The Netflix Recommender system. ACM Transactions on Management Information Systems, 6(4), 1–19. https://doi.org/10.1145/2843948

## Business Understanding

### Problem Statements

Menjelaskan pernyataan masalah:
- Bagaimana cara memberikan rekomendasi film yang relevan bagi pengguna berdasarkan fitur film?
- Bagaimana membangun sistem rekomendasi yang mampu menyarankan beberapa film yang paling mirip dengan film tertentu yang disukai pengguna?

### Goals

Menjelaskan tujuan proyek yang menjawab pernyataan masalah:
- Mengembangkan sistem rekomendasi berbasis content-based filtering dan model RecommenderNet yang dapat mengolah fitur seperti 'genres'.
- Menyediakan daftar film yang paling mirip sebagai rekomendasi berdasarkan film yang dipilih pengguna.

## Data Understanding

Menggunakan tiga dataset yaitu tmdb_5000_credits, tmdb_5000_movies_, dan ratings_small. Untuk movies dataset berisi informasi fitur mengenai film seperti budget, judul, sinopsis dan lain-lain. Dataset credit berisi pemeran dan kru dari film. Sementara ratings berisi rating yang diberikan oleh user kepada film tertentu yang terhubung dataset movies dengan id. Dataset rating memiliki jumlah data yang lebih besar daripada movies, sehingga perlu dilakukan join mengikuti dataset movies.

[Kaggle](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata).
[Kaggle](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset/data).

Variabel-variabel pada movie dataset adalah sebagai berikut:

Credits dataset 4,803 rows x 4 cols
- movie_id: Kode identifikasi tiap film.
- title: Judul film.
- cast: Pemeran film.
- crew: Kru film.

Movies dataset 4,803 rows x 20 cols
- budget: Anggaran pembuatan film dalam USD.
- genres: Genre film seperti action, action dll.
- homepage: URL halaman resmi film
- id: ID unik untuk setiap film
- keywords: Kata kunci yang menggambarkan film
- original_language: Bahasa asli film, dalam format kode ISO.
- original_title: Judul asli dari film.
- overview: Ringkasan atau sinopsis tentang alur cerita film.
- popularity: Skor popularitas.
- production_companies: Daftar perusahaan produksi yang membuat film.
- production_countries: Negara tempat produksi film.
- release_date: Tanggal rilis film.
- revenue: Total pendapatan atau dari film (dalam USD).
- runtime: Durasi film.
- spoken_languages: Bahasa yang digunakan dalam film.
- status: Status rilis film seperti “Released”, “Post Production”, “Rumored”.
- tagline: Kalimat promosi atau slogan film.
- title: Judul film yang ditampilkan secara umum.
- vote_average: Rata-rata skor rating dari pengguna TMDB.
- vote_count: Jumlah pengguna yang memberikan rating pada film tersebut.

Ratings dataset 100,004 rows x 4 cols
- userId: Id yang dimiliki oleh pengguna
- movieId: Id untuk film
- rating: Rating yang diberikan oleh tiap user
- timestamp: Kurang informasi mengenai kolom ini dan tidak terlalu digunakan

### Data yang kosong

df_merged
- homepage	3091
- overview	3
- release_date	1
- runtime	2
- tagline	844

df_ratings
- Tidak ada

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan tahapan eksplorasi data dengan melihat list film dengan popularitas tertinggi. Yaitu Minions, Interstellar, Deadpool, Guardians of The Galaxy, dan Mad Max: Fury Road sebagai top 5.

## Data Preparation
 
 - Menggabungkan dua dataframe menjadi df_merged
 - Mengubah df_ratings menjadi hanya data yang ada pada df_merged
 - Drop kolom homepage karena memiliki data kosong yang sangat banyak
 - Drop baris yang kosong
 - Cek data duplikat dan tidak ada sehingga tidak perlu melakukan handling duplikat
 - Cek informasi tipe data df_merged
 - Ubah kolom 'movieId' menjadi 'id' agar lebih mudah pada ratings
 - drop kolom movieId
 - Cek informasi tipe data df_ratings
 - Membersihkan kolom genres sebelum melakukan vectorization dengan TF-IDF
 - Menghitung kemiripan kosinus antar film berdasarkan TF-IDF genres dan membuat indeks untuk mencari posisi film berdasarkan judul
 - Dengan  menggunakan label encoder mengubah id user dan film menjadi angka berurutan
 - Mendefinisikan fitur dan label, melakukan normalisasi rating, membagi data menjadi training dan testing

## Modeling

- Content-based
Model sistem rekomendasi yang dibuat menggunakan content-based filtering dengan menggunakan fitur genre pada data film. Model akan menghitung kemiripan antar film menggunakan cosine similarity untuk mengetahui seberapa mirip genre satu film dengan yang lain. Dengan memetakan judul film ke indeks data, sistem dapat merekomendasikan 10 film yang paling mirip secara konten dengan film yang dipilih, berdasarkan skor kemiripan tertinggi.

Output
 id    title                            overview                                                                
 7	Avengers: Age of Ultron	            When Tony Stark tries to jumpstart a dormant p...
16	The Avengers	                    When an unexpected enemy emerges and threatens...
26	Captain America: Civil War	        Following the events of Age of Ultron, the col...
31	Iron Man 3	                        When Tony Stark's world is torn apart by a for...
35	Transformers: Revenge of the Fallen	Sam Witwicky leaves the Autobots behind for a ...
36	Transformers: Age of Extinction	    As humanity picks up the pieces, following the...
39	TRON: Legacy	                    Sam Flynn, the tech-savvy and daring son of Ke...
47	Star Trek Into Darkness	            When the crew of the Enterprise is called back...
51	Pacific Rim	                        When legions of monstrous creatures, known as ...
52	Transformers: Dark of the Moon	    Sam Witwicky takes his first tenuous steps int... 

- RecommenderNet
Selain content based filtering saya juga menggunakan RecommenderNet sebagai model sistem rekomendasi. RecommenderNet adalah model sistem rekomendasi berbasis neural network yang dirancang untuk memprediksi rating pengguna terhadap item, seperti film atau produk.

Output
Rekomendasi film untuk User ID 30:
----------------------------
Judul: Terminator 3: Rise of the Machines
ID Film: 296
Prediksi Rating: 0.94
Sinopsis: It's been 10 years since John Connor saved Earth from Judgment Day, and he's now living under the radar, steering clear of using anything Skynet can trace. That is, until he encounters T-X, a robotic assassin ordered to finish what T-1000 started. Go...

----------------------------
Judul: Cinderella Man
ID Film: 921
Prediksi Rating: 0.93
Sinopsis: The true story of boxer, Jim Braddock who, in the 1920’s after his retirement, has a surprise comeback in order to get him and his family out of a socially poor state....

----------------------------
Judul: Flags of Our Fathers
ID Film: 3683
Prediksi Rating: 0.92
Sinopsis: There were five Marines and one Navy Corpsman photographed raising the U.S. flag on Mt. Suribachi by Joe Rosenthal on February 23, 1945. This is the story of three of the six surviving servicemen – John 'Doc' Bradley, Pvt. Rene Gagnon and Pvt. Ira Ha...

----------------------------
Judul: Galaxy Quest
ID Film: 926
Prediksi Rating: 0.92
Sinopsis: The stars of a 1970s sci-fi show - now scraping a living through re-runs and sci-fi conventions - are beamed aboard an alien spacecraft. Believing the cast's heroic on-screen dramas are historical documents of real-life adventures, the band of aliens...
...
ID Film: 348
Prediksi Rating: 0.92
Sinopsis: During its return to the earth, commercial spaceship Nostromo intercepts a distress signal from a distant planet. When a three-member team of the crew discovers a chamber containing thousands of eggs on the planet, a creature inside one of the eggs a...

## Evaluation
- RecommenderNet
Pada proyek ini digunakan Mean Squared Error (MSE) dan Mean Absolute Error (MAE) sebagai metrik evaluasi model RecommenderNet yang dibangun. Pemilihan metrik ini sesuai karena model melakukan prediksi skor rating film yang berupa data numerik.

Epoch 1/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 5s 5ms/step - loss: 0.0799 - mae: 0.2366 - val_loss: 0.0697 - val_mae: 0.2172
Epoch 2/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 5ms/step - loss: 0.0634 - mae: 0.2050 - val_loss: 0.0563 - val_mae: 0.1938
Epoch 3/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0474 - mae: 0.1759 - val_loss: 0.0463 - val_mae: 0.1732
Epoch 4/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0348 - mae: 0.1477 - val_loss: 0.0424 - val_mae: 0.1630
Epoch 5/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0262 - mae: 0.1250 - val_loss: 0.0410 - val_mae: 0.1593
Epoch 6/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0204 - mae: 0.1095 - val_loss: 0.0407 - val_mae: 0.1584
Epoch 7/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0163 - mae: 0.0972 - val_loss: 0.0410 - val_mae: 0.1589
Epoch 8/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0126 - mae: 0.0849 - val_loss: 0.0416 - val_mae: 0.1601
Epoch 9/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 4ms/step - loss: 0.0097 - mae: 0.0747 - val_loss: 0.0423 - val_mae: 0.1615
Epoch 10/10
465/465 ━━━━━━━━━━━━━━━━━━━━ 2s 5ms/step - loss: 0.0076 - mae: 0.0651 - val_loss: 0.0430 - val_mae: 0.1626

Selama proses pelatihan selama 10 epoch, model mampu melakukan performa yang baik. Nilai MSE pada data validasi berhasil menurun dari 0.0697 pada epoch pertama menjadi 0.0430 di epoch ke 10. Sementara itu, MAE juga menurun dari 0.2172 menjadi 0.1626. Terdapat dua model yang dibuat pada projek ini, yaitu content based dan juga RecommenderNet. Content based mengandalkan kemiripan konten satu dengan yang lainnya, sementara RecommenderNet menghitung kecocokan pengguna dengan sebuah film berdasarkan rating yang diberikan. Dengan membangun model sistem rekomendasi ini, pihak penyedia film pada platform digital bisa memberikan rekomendasi film yang semakin relevan bagi pengguna.

- Content-based Filtering
Untuk model rekomendasi content based filtering, akan digunakan metrik evaluasi precision@k, recall@k, dan NDCG@k.

Average Precision@10: 0.0773
Average Recall@10   : 0.1081
Average NDCG@10     : 0.1034

Berdasarkan hasil evaluasi model memiliki precision@10 sebesar 0.0773, recall@10 sebesar 0.1081, dan NDCG@10 sebesar 0.1034, dapat dilihat model memiliki kemampuan yang cukup dalam merekomendasikan film yang relevan kepada pengguna. Ketiga metrik evaluasi dipilih karena mampu memberikan gambaran langsung tentang performa sistem merekomendasikan film yang relevan kepada pengguna. Dengan melakukan evaluasi antara kedua model penyedia film pada platform digital bisa memilih model yang lebih cocok dan lebih baik dalam menampilkan rekomendasi untuk pengguna mereka. 
**---Ini adalah bagian akhir laporan---**
