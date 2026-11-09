# Movie Recommendation System

Sistem rekomendasi film dengan dua pendekatan: content-based filtering dan collaborative filtering berbasis neural network.

## Ringkasan

- **Content-based filtering**: kemiripan antarfilm dihitung dengan TF-IDF dan cosine similarity.
- **Collaborative filtering (RecommenderNet)**: model embedding di TensorFlow yang dilatih dari data rating pengguna.

## Hasil

RecommenderNet, 10 epoch:

| Metrik | Epoch 1 | Epoch 10 |
|---|---|---|
| Validation MSE | 0.0697 | 0.0430 |
| Validation MAE | 0.2172 | 0.1626 |

Content-based filtering, k = 10:

| Precision@10 | Recall@10 | NDCG@10 |
|---|---|---|
| 0.0773 | 0.1081 | 0.1034 |

## Dataset

- [TMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata): 4.803 film (metadata dan credits)
- [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset) (`ratings_small`): 100.004 rating

## Struktur

```
.
├── recommendation_notebook.ipynb
├── tmdb_5000.zip
├── ratings_small.csv.zip
└── docs/
    └── laporan.md
```

## Menjalankan

```bash
pip install pandas numpy scikit-learn tensorflow matplotlib seaborn plotly jupyter
jupyter notebook recommendation_notebook.ipynb
```

Ekstrak kedua file `.zip`, lalu ubah path pada `read_csv()` di notebook agar sesuai dengan lokasi file.

## Dokumentasi

Laporan lengkap ada di [docs/laporan.md](docs/laporan.md).
