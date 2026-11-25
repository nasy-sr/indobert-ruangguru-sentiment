# IndoBERT Sentiment Analysis for Digital Education Quality Evaluation

Official repository for the published paper in **Prosiding SENAM 2026: Seminar Nasional Sistem Informasi dan Teknik Informatika Universitas Ma Chung**, Vol. 6 No. 1 (2026), pp. 43–57.

* **Title**: *Analisis Sentimen Ulasan Pengguna Aplikasi Ruangguru Berbasis IndoBERT untuk Evaluasi Kualitas Layanan Pendidikan Digital*
* **Authors**: Nasywa Salsabiila Romadhona, Dwi Wahyu Prabowo, Nurahman
* **Publication**: [Prosiding SENAM 2026 (pp. 43–57)](https://ocs.machung.ac.id/index.php/seminarnasionalmachung/issue/view/40) · [Full Paper PDF](https://ocs.machung.ac.id/index.php/seminarnasionalmachung/article/view/1388)

## Experimental Pipeline
The research is structured into three sequential notebooks reflecting the iterative methodology:

| Stage | Notebook | Description |
| :--- | :--- | :--- |
| **01** | [`01_eda_preprocessing_bigram.ipynb`](https://github.com/nasy-sr/indobert-ruangguru-sentiment/blob/main/01_eda_preprocessing_bigram.ipynb) | Dataset merging (211,838 raw reviews -> 209,343 clean reviews), regex cleaning, rating-based sentiment labeling, EDA visualizations, WordClouds, and Top 10 Bigram extraction. |
| **02** | [`02_preliminary_experiment_imbalanced.ipynb`](https://github.com/nasy-sr/indobert-ruangguru-sentiment/blob/main/02_preliminary_experiment_imbalanced.ipynb) | Preliminary 10-Fold CV experiment on a natural imbalanced subsample (15,000 reviews, `max_len=64`, 2 epochs) identifying the accuracy paradox between overall accuracy and minority-class recall. |
| **03** | [`03_main_experiment_4scenarios_wilcoxon.ipynb`](https://github.com/nasy-sr/indobert-ruangguru-sentiment/blob/main/03_main_experiment_4scenarios_wilcoxon.ipynb) | Main benchmark on stratified balanced samples (9,000 reviews; 3,000 per class, `max_len=128`, 3 epochs) comparing four imbalance-handling strategies via 10-Fold Stratified CV and Wilcoxon Signed-Rank testing. |

## Key Findings
* **Model & Hyperparameters**: Fine-tuned `indobenchmark/indobert-base-p1` using AdamW (`lr=2e-5`), linear warmup scheduler (10%), batch size 16, and 3 epochs.
* **Four Imbalance-Handling Scenarios (10-Fold Stratified CV)**:
  * **Baseline (Standard Cross-Entropy)**: **0.6951 ± 0.0138** weighted F1-score (Accuracy: **0.6970**)
  * **Random Oversampling**: 0.6927 ± 0.0134 weighted F1-score (Accuracy: 0.6951)
  * **Focal Loss (gamma = 2.0)**: 0.6922 ± 0.0183 weighted F1-score (Accuracy: 0.6942)
  * **Cost-Sensitive Learning (Weighted CE)**: 0.6920 ± 0.0152 weighted F1-score (Accuracy: 0.6943)
* **Statistical Validation**: Pairwise Wilcoxon Signed-Rank tests ($\alpha = 0.05$) confirm no statistically significant difference across scenarios once classes are stratified at the design level ($p > 0.05$), while bigram diagnostics pinpoint actionable service issues such as streaming bugs, quota usage, and TV casting.
