# MODUL PRAKTIKUM 01
## EF256129 — TD Pengolahan Citra Digital
### Evolusi Pengolahan Citra Digital dan Agenda Riset Computer Vision
### Praktikum: Eksplorasi Dataset dan Reproduksi Baseline Sederhana

**Program:** Doktor / S3 Ilmu Komputer  
**Pertemuan:** 01  
**Bahasa:** Python  
**Tools:** JupyterLab/Notebook, NumPy, Matplotlib, Pandas, scikit-image, scikit-learn  
**Tema:** Dataset exploration → representasi histogram intensitas → kNN → evaluasi → failure cases → refleksi riset

---

# 1. Tujuan Praktikum

Praktikum ini menjadi baseline awal sebelum mahasiswa mempelajari rekayasa representasi citra yang lebih kuat, CNN, Transformer, self-supervised learning, vision-language models, dan visual foundation models.

Alur utama:

```text
Dataset
  ↓
Inspeksi data
  ↓
Visualisasi sampel
  ↓
Distribusi kelas
  ↓
Train-test split
  ↓
Majority baseline
  ↓
Histogram intensitas
  ↓
kNN
  ↓
Evaluasi
  ↓
Confusion matrix
  ↓
Failure-case analysis
  ↓
Ablation sederhana
  ↓
Refleksi riset
```

Fokus utama bukan memperoleh akurasi tertinggi, melainkan memahami hubungan:

```text
representasi citra → classifier → evaluasi → keterbatasan → research question
```

---

# 2. Capaian Praktikum

Setelah menyelesaikan praktikum, mahasiswa mampu:

1. Menyiapkan environment eksperimen yang reproducible.
2. Memuat dan menginspeksi dataset citra.
3. Memahami citra sebagai matriks piksel.
4. Menganalisis distribusi kelas.
5. Membuat train-test split secara benar.
6. Membuat majority baseline.
7. Mengekstraksi histogram intensitas sebagai representasi citra.
8. Melatih classifier k-Nearest Neighbors.
9. Mengukur accuracy dan balanced accuracy.
10. Membaca classification report dan confusion matrix.
11. Mengidentifikasi failure cases.
12. Menguji pengaruh jumlah bin histogram dan nilai `k`.
13. Membandingkan representasi histogram dengan piksel mentah sebagai control experiment.
14. Menjelaskan keterbatasan representasi klasik.
15. Menyusun refleksi riset dan pertanyaan ilmiah awal.

---

# 3. Dataset

Gunakan dataset:

```python
sklearn.datasets.load_digits
```

Karakteristiknya:

- 1.797 citra;
- 10 kelas digit `0–9`;
- setiap citra berukuran `8 × 8`;
- grayscale;
- nilai piksel berada pada rentang `0–16`.

> **Penting:** dataset ini tidak menggunakan rentang piksel 0–255. Karena itu histogram harus menggunakan rentang yang sesuai. Pada modul ini digunakan `range=(0, 17)` agar nilai 0 sampai 16 terakomodasi dengan benar.

Dataset ini dipilih karena tersedia langsung di scikit-learn, kecil, cepat, dan cocok untuk memahami baseline klasik.

---

# 4. Struktur Folder Praktikum

Buat struktur:

```text
TD-PCD/
└── praktikum01/
    ├── notebook/
    │   └── Praktikum_01.ipynb
    ├── outputs/
    │   ├── figures/
    │   └── tables/
    ├── requirements.txt
    ├── requirements-lock.txt
    └── README.md
```

---

# 5. Setup Environment

## 5.1 Windows PowerShell

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

## 5.2 Windows Command Prompt

```cmd
python -m venv .venv
.venv\Scripts\activate.bat
```

## 5.3 Linux/macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Upgrade pip:

```bash
python -m pip install --upgrade pip
```

Instal library:

```bash
python -m pip install numpy pandas matplotlib scikit-image scikit-learn jupyterlab
```

Verifikasi:

```bash
python -c "import numpy, pandas, matplotlib, skimage, sklearn; print('Environment OK')"
```

Simpan versi:

```bash
python -m pip freeze > requirements-lock.txt
```

Jalankan Jupyter:

```bash
jupyter lab
```

---

# 6. Import Library dan Konfigurasi Reproducibility

```python
from pathlib import Path
import sys
import time

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import skimage
import sklearn

from sklearn.datasets import load_digits
from sklearn.model_selection import (
    train_test_split,
    StratifiedKFold,
    cross_val_score,
)
from sklearn.dummy import DummyClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import (
    accuracy_score,
    balanced_accuracy_score,
    classification_report,
    confusion_matrix,
    ConfusionMatrixDisplay,
)

RANDOM_STATE = 42
np.random.seed(RANDOM_STATE)

OUTPUT_DIR = Path("outputs")
FIGURE_DIR = OUTPUT_DIR / "figures"
TABLE_DIR = OUTPUT_DIR / "tables"

FIGURE_DIR.mkdir(parents=True, exist_ok=True)
TABLE_DIR.mkdir(parents=True, exist_ok=True)

print("Python       :", sys.version.split()[0])
print("NumPy        :", np.__version__)
print("scikit-image :", skimage.__version__)
print("scikit-learn :", sklearn.__version__)
```

**Mengapa seed perlu dicatat?**  
Karena eksperimen ilmiah harus dapat diulang dengan kondisi yang sama.

---

# 7. Memuat Dataset

```python
digits = load_digits()

images = digits.images
targets = digits.target
class_names = digits.target_names

print("Shape images :", images.shape)
print("Shape targets:", targets.shape)
print("Classes      :", class_names)
print("Data type    :", images.dtype)
print("Pixel min    :", images.min())
print("Pixel max    :", images.max())
```

Interpretasikan:

```text
(1797, 8, 8)
```

sebagai:

```text
1797 citra × 8 piksel tinggi × 8 piksel lebar
```

---

# 8. Memahami Citra sebagai Matriks

```python
sample_index = 0
sample_image = images[sample_index]
sample_label = targets[sample_index]

print("Label:", sample_label)
print(sample_image)
```

Visualisasi:

```python
plt.figure(figsize=(4, 4))
plt.imshow(sample_image, cmap="gray")
plt.title(f"Label: {sample_label}")
plt.axis("off")
plt.tight_layout()
plt.show()
```

Tampilkan nilai piksel:

```python
plt.figure(figsize=(6, 6))
plt.imshow(sample_image, cmap="gray")

for row in range(sample_image.shape[0]):
    for col in range(sample_image.shape[1]):
        plt.text(
            col,
            row,
            str(int(sample_image[row, col])),
            ha="center",
            va="center",
            fontsize=8
        )

plt.title("Citra sebagai Matriks Intensitas")
plt.xticks(range(8))
plt.yticks(range(8))
plt.tight_layout()
plt.show()
```

### Pertanyaan

1. Apa arti piksel bernilai 0?
2. Apa arti piksel bernilai 16?
3. Mengapa digit dengan label sama tetap mempunyai matriks berbeda?
4. Apa yang terjadi jika objek bergeser satu atau dua piksel?

---

# 9. Visualisasi Satu Sampel per Kelas

```python
fig, axes = plt.subplots(2, 5, figsize=(12, 5))

for label, ax in zip(range(10), axes.ravel()):
    idx = np.where(targets == label)[0][0]
    ax.imshow(images[idx], cmap="gray")
    ax.set_title(f"Kelas {label}")
    ax.axis("off")

plt.suptitle("Satu Sampel dari Setiap Kelas")
plt.tight_layout()

plt.savefig(
    FIGURE_DIR / "sample_per_class.png",
    dpi=150,
    bbox_inches="tight"
)

plt.show()
```

Tuliskan minimal tiga observasi mengenai:

- ketebalan goresan;
- posisi digit;
- kemiringan;
- bentuk yang mirip;
- kemungkinan kelas yang tertukar.

---

# 10. Distribusi Kelas

```python
labels, counts = np.unique(
    targets,
    return_counts=True
)

distribution_df = pd.DataFrame({
    "class": labels,
    "count": counts
})

distribution_df
```

Visualisasi:

```python
plt.figure(figsize=(10, 4))
plt.bar(labels, counts)

plt.xlabel("Kelas")
plt.ylabel("Jumlah Sampel")
plt.title("Distribusi Kelas")
plt.xticks(labels)
plt.tight_layout()

plt.savefig(
    FIGURE_DIR / "class_distribution.png",
    dpi=150,
    bbox_inches="tight"
)

plt.show()
```

Hitung rasio sederhana:

```python
imbalance_ratio = counts.max() / counts.min()

print("Largest class :", counts.max())
print("Smallest class:", counts.min())
print("Ratio         :", imbalance_ratio)
```

Analisis harus berbasis angka, bukan hanya pernyataan "dataset seimbang".

---

# 11. Statistik Piksel

```python
pixel_stats = {
    "min": images.min(),
    "max": images.max(),
    "mean": images.mean(),
    "std": images.std(),
    "median": np.median(images),
}

pixel_stats
```

Distribusi intensitas:

```python
plt.figure(figsize=(9, 4))

plt.hist(
    images.ravel(),
    bins=17,
    range=(0, 17)
)

plt.xlabel("Intensitas")
plt.ylabel("Frekuensi")
plt.title("Distribusi Intensitas Seluruh Piksel")
plt.tight_layout()
plt.show()
```

### Pertanyaan

1. Mengapa piksel bernilai 0 banyak muncul?
2. Apa yang terjadi jika seluruh citra diringkas menjadi satu histogram?
3. Informasi spasial apa yang hilang?

---

# 12. Train-Test Split

```python
X_train_img, X_test_img, y_train, y_test = train_test_split(
    images,
    targets,
    test_size=0.20,
    random_state=RANDOM_STATE,
    stratify=targets
)

print("Train:", X_train_img.shape)
print("Test :", X_test_img.shape)
```

Periksa distribusi:

```python
split_df = pd.DataFrame({
    "class": range(10),
    "train": np.bincount(y_train, minlength=10),
    "test": np.bincount(y_test, minlength=10),
})

split_df
```

Simpan:

```python
split_df.to_csv(
    TABLE_DIR / "train_test_distribution.csv",
    index=False
)
```

**Mengapa `stratify=targets`?**  
Agar proporsi kelas tetap terjaga pada training dan test set.

---

# 13. Baseline 0 — Majority Class

```python
dummy = DummyClassifier(
    strategy="most_frequent"
)

dummy.fit(
    np.zeros((len(y_train), 1)),
    y_train
)

dummy_pred = dummy.predict(
    np.zeros((len(y_test), 1))
)

dummy_accuracy = accuracy_score(
    y_test,
    dummy_pred
)

print("Majority baseline accuracy:", dummy_accuracy)
```

Pertanyaan:

> Model baru dikatakan baik dibandingkan apa?

Baseline sederhana harus selalu menjadi konteks evaluasi.

---

# 14. Ekstraksi Histogram Intensitas

```python
def extract_histogram(image, bins=17):
    hist, _ = np.histogram(
        image.ravel(),
        bins=bins,
        range=(0, 17)
    )

    hist = hist.astype(np.float64)

    if hist.sum() > 0:
        hist /= hist.sum()

    return hist
```

Uji:

```python
hist = extract_histogram(
    X_train_img[0],
    bins=17
)

print("Shape:", hist.shape)
print("Sum  :", hist.sum())
print(hist)
```

Histogram dinormalisasi sehingga totalnya mendekati `1.0`.

---

# 15. Visualisasi Citra dan Histogram

```python
image = X_train_img[0]
label = y_train[0]
hist = extract_histogram(image)

fig, axes = plt.subplots(
    1,
    2,
    figsize=(10, 4)
)

axes[0].imshow(image, cmap="gray")
axes[0].set_title(f"Citra — Label {label}")
axes[0].axis("off")

axes[1].bar(
    np.arange(len(hist)),
    hist
)

axes[1].set_title("Histogram Intensitas")
axes[1].set_xlabel("Bin")
axes[1].set_ylabel("Proporsi")

plt.tight_layout()
plt.show()
```

Histogram menyimpan:

```text
berapa banyak piksel pada intensitas tertentu
```

tetapi tidak menyimpan:

```text
di mana piksel tersebut berada
```

---

# 16. Membangun Feature Matrix

```python
def build_histogram_features(
    image_array,
    bins=17
):
    return np.vstack([
        extract_histogram(
            image,
            bins=bins
        )
        for image in image_array
    ])
```

Gunakan:

```python
BINS = 17

X_train_hist = build_histogram_features(
    X_train_img,
    bins=BINS
)

X_test_hist = build_histogram_features(
    X_test_img,
    bins=BINS
)

print(X_train_hist.shape)
print(X_test_hist.shape)
```

Jika `bins=17`, setiap citra menjadi vektor 17 dimensi.

---

# 17. Baseline 1 — k-Nearest Neighbors

```python
K = 5

knn = KNeighborsClassifier(
    n_neighbors=K
)

start = time.perf_counter()

knn.fit(
    X_train_hist,
    y_train
)

training_time = (
    time.perf_counter() - start
)

start = time.perf_counter()

y_pred = knn.predict(
    X_test_hist
)

prediction_time = (
    time.perf_counter() - start
)

print("Training time  :", training_time)
print("Prediction time:", prediction_time)
```

---

# 18. Evaluasi

```python
knn_accuracy = accuracy_score(
    y_test,
    y_pred
)

knn_balanced_accuracy = balanced_accuracy_score(
    y_test,
    y_pred
)

print("Majority baseline :", dummy_accuracy)
print("Histogram + kNN  :", knn_accuracy)
print("Balanced accuracy:", knn_balanced_accuracy)
```

Hitung peningkatan absolut:

```python
print(
    "Improvement:",
    knn_accuracy - dummy_accuracy
)
```

Jangan berhenti pada angka accuracy.

---

# 19. Classification Report

```python
report = classification_report(
    y_test,
    y_pred,
    output_dict=True
)

report_df = pd.DataFrame(
    report
).transpose()

report_df
```

Simpan:

```python
report_df.to_csv(
    TABLE_DIR / "classification_report_hist_knn.csv"
)
```

Versi teks:

```python
print(
    classification_report(
        y_test,
        y_pred
    )
)
```

Analisis:

- kelas dengan precision tertinggi;
- kelas dengan recall terendah;
- kelas yang paling sulit;
- apakah accuracy saja sudah cukup.

---

# 20. Confusion Matrix

```python
cm = confusion_matrix(
    y_test,
    y_pred
)

fig, ax = plt.subplots(
    figsize=(8, 8)
)

ConfusionMatrixDisplay(
    confusion_matrix=cm,
    display_labels=class_names
).plot(
    ax=ax,
    cmap="Blues",
    colorbar=False
)

ax.set_title(
    "Confusion Matrix — Histogram + kNN"
)

plt.tight_layout()

plt.savefig(
    FIGURE_DIR / "confusion_matrix_hist_knn.png",
    dpi=150,
    bbox_inches="tight"
)

plt.show()
```

Tuliskan minimal tiga pola kesalahan:

```text
Kelas A → sering diprediksi B.
Jumlah kasus = ....
Hipotesis penyebab = ....
```

Hipotesis harus diverifikasi dengan melihat citra salah.

---

# 21. Failure-Case Analysis

```python
wrong_indices = np.where(
    y_pred != y_test
)[0]

print(
    "Jumlah salah:",
    len(wrong_indices)
)
```

Visualisasikan maksimal 15:

```python
n_show = min(
    15,
    len(wrong_indices)
)

fig, axes = plt.subplots(
    3,
    5,
    figsize=(12, 8)
)

for ax in axes.ravel():
    ax.axis("off")

for ax, idx in zip(
    axes.ravel(),
    wrong_indices[:n_show]
):
    ax.imshow(
        X_test_img[idx],
        cmap="gray"
    )

    ax.set_title(
        f"True={y_test[idx]}, "
        f"Pred={y_pred[idx]}"
    )

    ax.axis("off")

plt.suptitle(
    "Failure Cases — Histogram + kNN"
)

plt.tight_layout()

plt.savefig(
    FIGURE_DIR / "failure_cases.png",
    dpi=150,
    bbox_inches="tight"
)

plt.show()
```

Analisis minimal 10 kesalahan berdasarkan:

- bentuk ambigu;
- posisi;
- ketebalan goresan;
- kemiripan histogram;
- kehilangan informasi spasial;
- kemungkinan kelemahan classifier.

---

# 22. Melihat Tetangga dari Satu Failure Case

```python
case_id = wrong_indices[0]

query_feature = X_test_hist[
    case_id
].reshape(1, -1)

distances, neighbor_indices = (
    knn.kneighbors(
        query_feature,
        n_neighbors=K
    )
)

print("True :", y_test[case_id])
print("Pred :", y_pred[case_id])
print("Dist :", distances[0])
```

Visualisasikan:

```python
fig, axes = plt.subplots(
    1,
    K + 1,
    figsize=(14, 3)
)

axes[0].imshow(
    X_test_img[case_id],
    cmap="gray"
)

axes[0].set_title(
    f"QUERY\n"
    f"True={y_test[case_id]}\n"
    f"Pred={y_pred[case_id]}"
)

axes[0].axis("off")

for pos, train_idx in enumerate(
    neighbor_indices[0],
    start=1
):
    axes[pos].imshow(
        X_train_img[train_idx],
        cmap="gray"
    )

    axes[pos].set_title(
        f"Neighbor\n"
        f"Label={y_train[train_idx]}"
    )

    axes[pos].axis("off")

plt.tight_layout()
plt.show()
```

Pertanyaan:

> Apakah tetangga tersebut benar-benar mirip bentuk, atau hanya mirip distribusi intensitas?

---

# 23. Eksperimen 1 — Jumlah Bin Histogram

Uji:

```text
4, 8, 17, 32
```

Gunakan CV pada **training set saja**.

```python
cv = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=RANDOM_STATE
)

bin_options = [4, 8, 17, 32]
bin_results = []

for bins in bin_options:
    X_hist = build_histogram_features(
        X_train_img,
        bins=bins
    )

    model = KNeighborsClassifier(
        n_neighbors=5
    )

    scores = cross_val_score(
        model,
        X_hist,
        y_train,
        cv=cv,
        scoring="accuracy"
    )

    bin_results.append({
        "bins": bins,
        "mean_cv_accuracy": scores.mean(),
        "std_cv_accuracy": scores.std()
    })

bin_results_df = pd.DataFrame(
    bin_results
)

bin_results_df
```

Simpan:

```python
bin_results_df.to_csv(
    TABLE_DIR / "experiment_bins.csv",
    index=False
)
```

Plot:

```python
plt.figure(figsize=(7, 4))

plt.errorbar(
    bin_results_df["bins"],
    bin_results_df["mean_cv_accuracy"],
    yerr=bin_results_df["std_cv_accuracy"],
    marker="o",
    capsize=4
)

plt.xlabel("Jumlah Bin")
plt.ylabel("Mean CV Accuracy")
plt.xticks(bin_options)
plt.title("Pengaruh Jumlah Bin")
plt.tight_layout()
plt.show()
```

Analisis apakah bin lebih banyak selalu lebih baik.

---

# 24. Eksperimen 2 — Nilai k

```python
K_OPTIONS = [
    1, 3, 5, 7, 9, 11
]

X_train_hist_exp = (
    build_histogram_features(
        X_train_img,
        bins=17
    )
)

k_results = []

for k in K_OPTIONS:
    model = KNeighborsClassifier(
        n_neighbors=k
    )

    scores = cross_val_score(
        model,
        X_train_hist_exp,
        y_train,
        cv=cv,
        scoring="accuracy"
    )

    k_results.append({
        "k": k,
        "mean_cv_accuracy": scores.mean(),
        "std_cv_accuracy": scores.std()
    })

k_results_df = pd.DataFrame(
    k_results
)

k_results_df
```

Pilih parameter dari hasil CV training:

```python
best_row = k_results_df.loc[
    k_results_df[
        "mean_cv_accuracy"
    ].idxmax()
]

BEST_K = int(best_row["k"])

print("Best k:", BEST_K)
```

Jangan memilih parameter dengan melihat test set berulang kali.

---

# 25. Final Evaluation Setelah Pemilihan k

```python
final_model = KNeighborsClassifier(
    n_neighbors=BEST_K
)

final_model.fit(
    X_train_hist_exp,
    y_train
)

final_pred = final_model.predict(
    build_histogram_features(
        X_test_img,
        bins=17
    )
)

final_accuracy = accuracy_score(
    y_test,
    final_pred
)

print(
    "Final test accuracy:",
    final_accuracy
)
```

**Prinsip penting:** test set adalah evaluasi akhir, bukan tempat memilih hyperparameter.

---

# 26. Control Experiment — Raw Pixel + kNN

Ini bukan pengganti baseline histogram, tetapi kontrol untuk melihat efek representasi.

Flatten:

```python
X_train_flat = X_train_img.reshape(
    len(X_train_img),
    -1
)

X_test_flat = X_test_img.reshape(
    len(X_test_img),
    -1
)

print(X_train_flat.shape)
```

Latih classifier yang sama:

```python
pixel_knn = KNeighborsClassifier(
    n_neighbors=5
)

pixel_knn.fit(
    X_train_flat,
    y_train
)

pixel_pred = pixel_knn.predict(
    X_test_flat
)

pixel_accuracy = accuracy_score(
    y_test,
    pixel_pred
)

print("Histogram + kNN:", knn_accuracy)
print("Raw pixel + kNN :", pixel_accuracy)
```

Diskusikan:

> Jika classifier sama tetapi hasil berubah signifikan, apa peran representasi?

---

# 27. Ringkasan Eksperimen

```python
summary_df = pd.DataFrame([
    {
        "experiment":
            "Majority baseline",
        "representation":
            "None",
        "classifier":
            "Dummy most frequent",
        "accuracy":
            dummy_accuracy
    },
    {
        "experiment":
            "Histogram baseline",
        "representation":
            "Intensity histogram",
        "classifier":
            f"kNN k={K}",
        "accuracy":
            knn_accuracy
    },
    {
        "experiment":
            "Control",
        "representation":
            "Raw pixels 64D",
        "classifier":
            "kNN k=5",
        "accuracy":
            pixel_accuracy
    }
])

summary_df
```

Simpan:

```python
summary_df.to_csv(
    TABLE_DIR / "experiment_summary.csv",
    index=False
)
```

Visualisasi:

```python
plt.figure(figsize=(9, 4))

plt.bar(
    summary_df["experiment"],
    summary_df["accuracy"]
)

plt.ylim(0, 1)
plt.ylabel("Accuracy")
plt.title("Perbandingan Baseline")
plt.tight_layout()

plt.savefig(
    FIGURE_DIR / "baseline_comparison.png",
    dpi=150,
    bbox_inches="tight"
)

plt.show()
```

---

# 28. Reproducibility Log

```python
experiment_log = {
    "random_state":
        RANDOM_STATE,
    "dataset":
        "sklearn.datasets.load_digits",
    "n_samples":
        len(images),
    "image_shape":
        str(images.shape[1:]),
    "pixel_min":
        float(images.min()),
    "pixel_max":
        float(images.max()),
    "test_size":
        0.20,
    "hist_bins":
        BINS,
    "knn_k":
        K,
    "majority_accuracy":
        float(dummy_accuracy),
    "hist_knn_accuracy":
        float(knn_accuracy),
    "pixel_knn_accuracy":
        float(pixel_accuracy),
    "numpy_version":
        np.__version__,
    "skimage_version":
        skimage.__version__,
    "sklearn_version":
        sklearn.__version__,
}

pd.Series(
    experiment_log
).to_csv(
    TABLE_DIR / "experiment_log.csv",
    header=["value"]
)
```

---

# 29. Output Minimum

Setelah praktikum:

```text
outputs/
├── figures/
│   ├── sample_per_class.png
│   ├── class_distribution.png
│   ├── confusion_matrix_hist_knn.png
│   ├── failure_cases.png
│   └── baseline_comparison.png
└── tables/
    ├── train_test_distribution.csv
    ├── classification_report_hist_knn.csv
    ├── experiment_bins.csv
    ├── experiment_summary.csv
    └── experiment_log.csv
```

---

# 30. Pertanyaan Analisis Wajib

## Dataset

1. Bagaimana distribusi kelas?
2. Apakah terdapat class imbalance?
3. Variasi visual apa yang paling jelas?

## Representasi

4. Apa yang direpresentasikan histogram intensitas?
5. Informasi apa yang hilang?
6. Mengapa citra berbeda dapat mempunyai histogram serupa?
7. Mengapa fitur histogram interpretabel tetapi terbatas?

## Model

8. Mengapa majority baseline perlu?
9. Bagaimana nilai `k` memengaruhi kNN?
10. Apa trade-off `k` kecil dan besar?

## Evaluasi

11. Kelas mana paling mudah?
12. Kelas mana paling sulit?
13. Kelas mana paling sering tertukar?
14. Mengapa accuracy tidak cukup?
15. Apa tambahan informasi dari confusion matrix?

## Failure Cases

16. Analisis minimal 10 kesalahan.
17. Pisahkan dugaan kegagalan karena representasi dan classifier.
18. Apakah tetangga terdekat benar-benar mirip secara bentuk?

## Refleksi Riset

19. Jika CNN belum boleh digunakan, bagaimana baseline dapat diperbaiki?
20. Apakah HOG/LBP/SIFT berpotensi lebih baik? Mengapa?
21. Keterbatasan apa yang diharapkan dapat diatasi CNN?
22. Pertanyaan riset apa yang muncul dari eksperimen ini?

---

# 31. Tugas Praktikum

## Tugas 1 — Dataset Exploration

Wajib:

- shape data;
- rentang intensitas;
- satu sampel per kelas;
- distribusi kelas;
- minimal tiga observasi.

## Tugas 2 — Majority Baseline

Laporkan:

- accuracy;
- kelas mayoritas;
- fungsi baseline dalam penelitian.

## Tugas 3 — Histogram + kNN

Laporkan:

- jumlah bin;
- nilai k;
- accuracy;
- balanced accuracy;
- classification report;
- confusion matrix.

## Tugas 4 — Failure Cases

Analisis minimal 10 citra salah.

## Tugas 5 — Ablation

Uji:

```text
bins ∈ {4, 8, 17, 32}
k    ∈ {1, 3, 5, 7, 9}
```

Gunakan cross-validation pada training set.

---

# 32. Challenge Doktoral

Pilih minimal dua.

## Challenge A — Histogram per Region

Bagi citra menjadi 4 kuadran dan gabungkan histogram masing-masing.

Tujuan:

> Menambahkan informasi spasial kasar.

## Challenge B — HOG

Gunakan HOG dari `skimage.feature`.

Bandingkan:

```text
Intensity Histogram + kNN
vs
HOG + kNN
```

Analisis:

- accuracy;
- dimensionalitas;
- waktu ekstraksi;
- interpretabilitas.

## Challenge C — Classifier Berbeda

Dengan fitur histogram yang sama, bandingkan:

```text
kNN
SVM
Random Forest
```

Pertanyaan:

> Seberapa jauh classifier yang lebih kuat dapat memperbaiki representasi yang lemah?

## Challenge D — Dataset Sesuai Minat Riset

Ulangi pipeline pada dataset lain:

- citra medis;
- satelit;
- tekstur;
- defect detection;
- wajah;
- objek alam;
- domain penelitian mahasiswa.

---

# 33. Hubungan dengan Evolusi Computer Vision

Pipeline praktikum:

```text
Citra
→ Feature Engineering
→ Classifier
→ Output
```

Pipeline CNN:

```text
Citra
→ Learned Representation
→ Classifier Head
→ Output
```

Diskusikan:

1. Bagian mana yang digantikan CNN?
2. Mengapa feature engineering manual dapat menjadi bottleneck?
3. Mengapa baseline klasik tetap penting di paper modern?
4. Kapan metode klasik justru lebih rasional?
5. Apa trade-off interpretabilitas, data, dan komputasi?

---

# 34. Refleksi Research Interest

Tuliskan 2–3 paragraf:

```text
Domain:
Masalah:
Jenis data:
Baseline sederhana:
Keterbatasan baseline:
Metode modern yang relevan:
Eksperimen pertama yang logis:
```

---

# 35. Peta Literatur Awal

Pilih 5–10 paper.

| No | Paper | Tahun | Masalah | Metode | Dataset | Metrik | Hasil | Keterbatasan | Peluang |
|---:|---|---:|---|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | ... | ... | ... | ... | ... |

Tidak cukup hanya mengumpulkan judul. Untuk tiap paper pahami:

- masalah;
- kontribusi;
- baseline;
- dataset;
- metrik;
- keterbatasan;
- peluang penelitian.

---

# 36. Pertanyaan Ilmiah Awal

Tuliskan 3–5 pertanyaan yang dapat diuji.

Pola yang baik:

```text
Seberapa besar A dibandingkan B pada kondisi C?
```

```text
Bagaimana pengaruh X terhadap Y?
```

```text
Dalam kondisi apa metode A lebih baik daripada metode B?
```

Hindari pertanyaan terlalu umum.

---

# 37. Format Laporan

```text
1. Identitas
2. Tujuan
3. Environment
4. Dataset
5. Eksplorasi Dataset
6. Majority Baseline
7. Representasi Histogram
8. Histogram + kNN
9. Evaluasi
10. Confusion Matrix
11. Failure Cases
12. Ablation Bins
13. Ablation k
14. Control Experiment
15. Diskusi
16. Research Reflection
17. Kesimpulan
18. Reproducibility Log
```

---

# 38. Rubrik Penilaian

| Komponen | Bobot |
|---|---:|
| Kebenaran pipeline | 20% |
| Reproducibility | 10% |
| Eksplorasi dataset | 10% |
| Baseline dan evaluasi | 15% |
| Failure-case analysis | 15% |
| Eksperimen/ablation | 15% |
| Refleksi ilmiah | 15% |
| **Total** | **100%** |

---

# 39. Checklist Sebelum Dikumpulkan

- [ ] Notebook dapat dijalankan dari awal sampai akhir.
- [ ] Tidak ada path absolut komputer pribadi.
- [ ] Random seed ditetapkan.
- [ ] Train dan test tidak tercampur.
- [ ] Hyperparameter tidak dipilih menggunakan test set.
- [ ] Majority baseline dilaporkan.
- [ ] Histogram dinormalisasi.
- [ ] Range intensitas sesuai dataset.
- [ ] Classification report tersedia.
- [ ] Confusion matrix tersedia.
- [ ] Minimal 10 failure cases dianalisis.
- [ ] Eksperimen bins dilakukan.
- [ ] Eksperimen k dilakukan.
- [ ] Versi library dicatat.
- [ ] Kesimpulan membahas keterbatasan, bukan hanya accuracy.
- [ ] Research reflection tersedia.
- [ ] 3–5 pertanyaan ilmiah awal tersedia.

---

# 40. Troubleshooting

## `ModuleNotFoundError: No module named 'skimage'`

```bash
python -m pip install scikit-image
```

## Kernel Jupyter salah

Periksa:

```python
import sys
print(sys.executable)
```

Jika perlu:

```bash
python -m pip install ipykernel
python -m ipykernel install --user --name tdpcd-p01 --display-name "TD PCD Praktikum 01"
```

## Accuracy rendah

Periksa:

1. range histogram;
2. normalisasi histogram;
3. shape fitur;
4. label;
5. split;
6. apakah performa rendah memang berasal dari representasi yang lemah.

Jangan langsung menganggap performa rendah berarti kode salah.

---

# 41. Kesimpulan Konseptual

Histogram intensitas bersifat:

```text
+ sederhana
+ cepat
+ interpretabel
- kehilangan posisi
- kehilangan bentuk
- lemah untuk semantik kompleks
```

Karena itu praktikum ini menjadi titik awal untuk memahami mengapa bidang bergerak dari:

```text
pixel
→ handcrafted feature
→ learned feature
→ embedding
→ multimodal representation
```

---

# 42. Persiapan Pertemuan Berikutnya

Simpan baseline ini sebagai pembanding untuk:

- filtering;
- enhancement;
- histogram;
- morphology;
- texture;
- edge detection;
- feature descriptor.

Pertanyaan untuk Pertemuan 2:

> Apakah rekayasa representasi citra yang lebih baik memberikan peningkatan yang terukur dibanding baseline paling sederhana?

---

# 43. Deliverable

```text
NIM_Nama_Praktikum01/
├── Praktikum_01.ipynb
├── requirements-lock.txt
├── outputs/
│   ├── figures/
│   └── tables/
├── literature_map.md
└── README.md
```

Isi minimum `README.md`:

```text
Nama:
NIM:
Environment:
Cara menjalankan:
Dataset:
Baseline:
Hasil utama:
Failure cases:
Kesimpulan:
Research interest:
```

---

# Penutup

Praktikum pertama bukan kompetisi mendapatkan akurasi tertinggi. Mahasiswa harus mampu menjelaskan:

```text
apa yang direpresentasikan,
apa yang dipelajari model,
bagaimana model dievaluasi,
mengapa model gagal,
dan eksperimen apa yang logis dilakukan berikutnya.
```

Kerangka ini menjadi fondasi untuk membandingkan pendekatan klasik, CNN, Transformer, self-supervised learning, vision-language models, dan visual foundation models secara ilmiah.
