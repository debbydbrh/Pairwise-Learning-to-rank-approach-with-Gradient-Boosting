# Pairwise Learning to Rank Approach with Gradient Tree Boosting Algorithm 
_Learning to Rank_ adalah bagian dari penerapan_ machine learning_ dalam pembangunan model perankingan pada sistem temu balik informasi. Salah satu pendekatan untuk membangun model _Learning to Rank_ yaitu _pairwise approach_ dimana pasangan dokumen dianggap sebagai input untuk sistem pembelajaran. Untuk meningkatkan kinerja model yang akan dibangun diterapkan algoritma _gradient tree boosting_. Salah satu implementasi _gradient tree boosting_ terbaik yang tersedia saat ini adalah XGBoost (_eXtreme Gradient Boosting_). Pada implementasinya, XGBoost menggunakan model LambdaMart dan pendekatan _pairwise_ untuk mempertimbangkan urutan relative dokumen dalam pasangan kueri dan dokumen. Evaluasi performa perankingan _Learning to Rank_ menggunakan daftar peringkat _Normalized Discounted Cumulative Gain_ (NDCG).

### Dataset
Million queries dataset from TREC 2008 :
[MQ2008](https://www.microsoft.com/en-us/research/project/letor-learning-rank-information-retrieval/#!letor-4-0)

| Dataset name |       | rows   | columns | num samples in queries (min, median, max) | 
|--------------|-------|--------|---------|-------------------------------------------| 
| mq2008       | train | 9630   | 46      | (5, 8, 121)                               | 
|              | test  | 2874   | 46      | (6, 14, 119)                              | 

### Parameter
Untuk membangun model perankingan digunakan library XGBoost yang memanfaatkan _packages_ XGBRanker dimana parameter yang digunakan adalah sebagai berikut:

```
...
objective : 'rank:pairwise'
eval_metric : 'ndcg'
learning_rate: 0.1
gamma : 1.0
min_child_weight : 0.1,
max_depth : 6
n_estimators : 4
...
```

### Skor Prediksi Perangkingan
 - Fold 1: 0.783, 0.168, 0.711, ..., 0.493, 0.673, 0.289
 - Fold 2: 0.383, 0.280, 0.495, ..., 0.520, 0.208, 0.669
 - Fold 3: 0.445, 0.218, 0.394, ..., 0.181, 0.390 , 0.465
 - Fold 4: 0.276, 0.527, 0.153, ..., 0.313, 0.153, 0.193
 - Fold 5: 0.608, 0.174, 0.649 , ..., 0.179, 0.383


### Hasil Evaluasi 

|Fold | Estimator 0 | Estimator 1 | Estimator 2 | Estimator 3 | Rata - Rata |
|-----|-------------|-------------|-------------|-------------|-------------|
|  1  | 0.793475    | 0.79652     | 0.808798    | 0.807923    | 0.801679    |
|  2  | 0.821042    | 0.826573    | 0.831727    | 0.832029    | 0.827843    |
|  3  | 0.808025    | 0.806311	  | 0.816868    | 0.819315    | 0.812629	  |
|  4  | 0.777406    | 0.791057    | 0.790832    | 0.790213    | 0.787377    |
|  5  | 0.790501	  | 0.798573    | 0.804969    | 0.809962    | 0.801001	  |

Rata - rata skor NDCG kelima Fold : 0.806106

Skor NDCG  kelima fold atau rata-rata skor NDCG setiap _fold_ yaitu  0.806106 dari 1 (_perfect ranking_). 
Skor NDCG ini membuktikan bahwa penerapan algoritma _gradient tree boosting_ dan pendekatan _pairwise_ sebagai pendekatan untuk membangun model _Learning to Rank_, menghasilkan skor NDCG yang baik untuk setiap 5 Fold partisi dataset LETOR 4.0 MQ2008 dan kualitas perankingan yang dihasilkan oleh model juga sudah baik.
