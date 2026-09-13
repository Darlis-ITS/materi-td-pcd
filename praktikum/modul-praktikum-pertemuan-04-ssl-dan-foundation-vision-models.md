# Modul Praktikum Pertemuan 04
## Self-Supervised Learning dan Foundation Vision Models

**Mata kuliah:** EF256129 — Topik Dalam Pengolahan Citra Digital  
**Program:** Doktor/S3 Teknik Informatika  
**Dosen:** Dr. Darlis Herumurti  
**Departemen:** Teknik Informatika — ITS  
**Bahasa pemrograman:** Python  
**Platform:** Jupyter Notebook/JupyterLab atau Google Colab  
**Dataset utama:** Oxford-IIIT Pet melalui `torchvision`  
**Model utama:** DINOv2-Small dan ResNet-50 pretrained ImageNet  

---

## Ringkasan Praktikum

Praktikum ini mempelajari cara mengevaluasi kualitas **representasi visual yang dipelajari tanpa label**. Mahasiswa tidak melakukan pretraining DINOv2 dari awal. Sebaliknya, mahasiswa menggunakan foundation vision model yang telah melalui self-supervised pretraining sebagai **frozen feature extractor**, kemudian menguji apakah embedding-nya:

1. membentuk struktur semantik;
2. mendukung pencarian citra serupa;
3. dapat dipisahkan oleh classifier linear;
4. efisien terhadap jumlah label;
5. tetap berguna ketika data mengalami distribution shift sederhana; dan
6. lebih atau kurang transferable dibanding fitur supervised.

Alur utama:

```text
Oxford-IIIT Pet
      ↓
Audit data dan fixed split
      ↓
┌─────────────────────┬──────────────────────┐
│ DINOv2-Small        │ ResNet-50 ImageNet   │
│ self-supervised     │ supervised baseline  │
└──────────┬──────────┴──────────┬───────────┘
           ↓                     ↓
        Frozen embedding extraction
                     ↓
      PCA/t-SNE + nearest-neighbor retrieval
                     ↓
       Linear probe: 1%, 10%, 50%, 100% label
                     ↓
 Accuracy + balanced accuracy + macro-F1
                     ↓
 Confusion matrix + failure analysis + robustness
                     ↓
 Research gap + rekomendasi untuk domain penelitian
```

> Fokus praktikum bukan mencari model yang selalu menang, tetapi menguji **transferability representasi** melalui protokol yang terkontrol dan dapat direproduksi.

---

## 1. Posisi dan Hubungan dengan Praktikum 01–03

Empat praktikum pertama membentuk alur berjenjang dari representasi sederhana menuju foundation vision model.

| Praktikum | Pertanyaan utama | Representasi/model | Posisi dalam alur |
|---|---|---|---|
| 01 | Apakah representasi piksel sederhana cukup untuk klasifikasi? | Histogram intensitas, raw pixel, kNN | Baseline klasik, evaluasi, failure case, dan research log |
| 02 | Bagaimana CNN dan Transformer membangun representasi? | Konvolusi manual, SimpleCNN, residual connection, Q/K/V, attention, patch token, Tiny Image Transformer | Membuka komponen internal model dari nol |
| 03 | Bagaimana CNN dan ViT pretrained dibandingkan pada tugas yang sama? | ResNet-18 dan DeiT-Tiny | Fair comparison, linear probing/fine-tuning, efisiensi, attention/feature map, robustness |
| **04** | **Seberapa berguna representasi yang dipelajari tanpa label ketika label downstream dibatasi atau distribusi data berubah?** | **DINOv2 frozen embedding vs ResNet-50 supervised embedding** | **Evaluasi transferability, label efficiency, struktur embedding, dan foundation model** |

Hubungan konseptualnya adalah:

```text
Praktikum 01: representasi dirancang manusia
        ↓
Praktikum 02: representasi dipelajari dari label melalui model yang dibangun dari nol
        ↓
Praktikum 03: representasi pretrained dibandingkan antararsitektur
        ↓
Praktikum 04: sumber supervisi saat pretraining dijadikan variabel ilmiah
```

### Apa yang digunakan kembali?

- Dari Praktikum 01: baseline, stratified split, metrik, confusion matrix, failure-case analysis, dan research log.
- Dari Praktikum 02: pemahaman tensor, patch, token, embedding, residual connection, dan self-attention.
- Dari Praktikum 03: frozen backbone, pretrained model, fair comparison, efisiensi, multi-seed, robustness, dan larangan menggunakan test set untuk memilih konfigurasi.

### Apa yang baru pada Praktikum 04?

- self-supervised representation dan foundation vision model;
- perbandingan sumber pretraining: tanpa label vs berlabel;
- ekstraksi dan penyimpanan embedding;
- PCA/t-SNE sebagai alat eksplorasi, bukan bukti tunggal;
- nearest-neighbor retrieval;
- label-efficiency curve;
- analisis transferability dan domain shift;
- perumusan rekomendasi penggunaan foundation model untuk riset.

### Jembatan menuju Praktikum 05

Pada Praktikum 05, representasi visual akan dihubungkan dengan representasi bahasa melalui vision-language model seperti CLIP. Dengan demikian, Praktikum 04 berada di antara:

```text
arsitektur visual modern → representasi visual tanpa label → representasi multimodal citra-teks
```

---

## 2. Capaian Pembelajaran Praktikum

Setelah menyelesaikan praktikum, mahasiswa mampu:

1. menjelaskan perbedaan supervised, unsupervised, dan self-supervised learning;
2. menjelaskan hubungan contrastive learning, teacher-student learning, momentum teacher, dan masked image modeling dengan DINO/DINOv2;
3. memuat DINOv2 dan model supervised melalui API publik;
4. mengekstrak embedding citra secara batch dan menyimpannya untuk reproduksibilitas;
5. membandingkan dimensi, norma, waktu ekstraksi, dan throughput embedding;
6. mengeksplorasi struktur embedding menggunakan PCA dan t-SNE;
7. melakukan nearest-neighbor retrieval berbasis cosine similarity;
8. melatih linear probe tanpa mengubah backbone;
9. mengukur label efficiency pada 1%, 10%, 50%, dan 100% label;
10. menjalankan eksperimen dengan minimal tiga seed;
11. melaporkan accuracy, balanced accuracy, macro-F1, mean, dan standard deviation;
12. menganalisis confusion matrix dan failure cases;
13. menguji robustness terhadap satu perturbasi terkontrol;
14. membedakan bukti eksploratif dari bukti kuantitatif;
15. merumuskan research gap dan rekomendasi foundation model untuk domain penelitian.

---

## 3. Pertanyaan Penelitian dan Hipotesis

### 3.1 Pertanyaan penelitian utama

- **RQ1 — Linear separability:** Apakah embedding DINOv2 dapat memisahkan 37 kelas Oxford-IIIT Pet menggunakan classifier linear?
- **RQ2 — Label efficiency:** Model mana yang lebih baik ketika hanya 1%, 10%, atau 50% label training tersedia?
- **RQ3 — Struktur semantik:** Apakah tetangga terdekat dan cluster embedding mencerminkan kemiripan kelas atau karakter visual?
- **RQ4 — Robustness:** Model mana yang mengalami penurunan kinerja lebih kecil pada distribution shift yang sama?
- **RQ5 — Efisiensi:** Bagaimana trade-off dimensi embedding, jumlah parameter, waktu ekstraksi, throughput, dan kinerja?
- **RQ6 — Transferability:** Apakah temuan pada citra hewan natural mendukung penggunaan model tersebut pada domain penelitian mahasiswa?

### 3.2 Hipotesis awal

Sebelum menjalankan eksperimen, isi hipotesis berikut.

| ID | Hipotesis sebelum eksperimen | Dasar teori | Hasil mendukung/menolak |
|---|---|---|---|
| H1 | ... | ... | Diisi setelah eksperimen |
| H2 | ... | ... | Diisi setelah eksperimen |
| H3 | ... | ... | Diisi setelah eksperimen |

> Hipotesis tidak boleh diubah setelah hasil diketahui. Jika hasil berbeda dari dugaan, jelaskan penyebab yang mungkin dan eksperimen lanjutan untuk mengujinya.

---

## 4. Konsep Inti Sebelum Praktikum

### 4.1 Self-supervised learning

Pada supervised learning, label manusia menjadi sinyal pelatihan. Pada self-supervised learning (SSL), sinyal pelatihan dibangun dari struktur data itu sendiri. Dua augmentasi dari citra yang sama dapat diperlakukan sebagai pasangan yang harus konsisten, atau sebagian patch dapat disembunyikan untuk diprediksi.

```text
Data tanpa label → pretext/objective → encoder → representasi → downstream task
```

### 4.2 Contrastive dan non-contrastive learning

- **SimCLR:** mendekatkan embedding dua view dari citra yang sama dan menjauhkan sampel berbeda.
- **MoCo:** memakai momentum encoder dan queue agar tersedia banyak negative samples tanpa batch sangat besar.
- **BYOL/DINO:** memakai student dan teacher tanpa negative samples eksplisit; stabilitas dijaga dengan stop-gradient, momentum, centering, sharpening, atau mekanisme lain.
- **MAE:** menutup banyak patch dan merekonstruksi bagian yang hilang.

### 4.3 DINO dan DINOv2

DINO menggunakan self-distillation tanpa label. Student belajar mencocokkan output teacher pada beberapa view citra; parameter teacher diperbarui sebagai exponential moving average dari student. DINOv2 memperluas ide ini pada skala data dan model yang lebih besar serta menggabungkan objective tingkat citra dan patch untuk menghasilkan fitur visual serbaguna.

### 4.4 Frozen feature extraction dan linear probing

Pada praktikum ini, backbone tidak diubah:

$$
\mathbf{z}_i = f_{\theta}(x_i), \qquad \theta \text{ tetap}
$$

Classifier linear mempelajari:

$$
\hat{y}_i = \operatorname{softmax}(W\mathbf{z}_i+b)
$$

Karena hanya $W$ dan $b$ yang dipelajari, hasil lebih langsung menunjukkan apakah informasi kelas sudah tersusun secara linear pada embedding.

### 4.5 Mengapa tidak melakukan pretraining dari nol?

Pretraining SSL modern membutuhkan data, waktu, dan komputasi besar. Menjalankan versi sangat kecil dari nol pada sesi praktikum berisiko membuat mahasiswa hanya mengamati kegagalan optimisasi. Fokus modul ini adalah **menguji representasi yang telah dipelajari**, bukan mereproduksi seluruh biaya pretraining. Implementasi mini-SimCLR disediakan sebagai challenge opsional.

---

## 5. Dataset

### 5.1 Dataset utama: Oxford-IIIT Pet

Dataset dimuat melalui:

```python
torchvision.datasets.OxfordIIITPet
```

Karakteristik penting:

- 37 kategori anjing dan kucing;
- citra RGB dengan ukuran dan latar beragam;
- menyediakan split resmi `trainval` dan `test`;
- cocok untuk klasifikasi fine-grained karena beberapa ras memiliki kemiripan visual tinggi;
- tersedia anotasi segmentasi, tetapi praktikum inti hanya memakai label kategori.

Alasan pemilihan:

1. lebih semantik dan lebih realistis daripada citra 32×32;
2. cukup kecil untuk ekstraksi embedding di Colab;
3. cocok untuk nearest-neighbor retrieval dan failure analysis;
4. memungkinkan perluasan ke dense feature atau segmentasi.

### 5.2 Keterbatasan dataset

- hanya berisi domain hewan peliharaan;
- distribusi latar dapat menjadi shortcut;
- kemiripan antarbreed dapat membuat label sulit dibedakan bahkan oleh manusia;
- hasil tidak otomatis berlaku pada citra medis, satelit, dokumen, bawah air, atau industri.

### 5.3 Alternatif dataset publik

| Dataset | Sumber | Fokus pengembangan |
|---|---|---|
| CIFAR-10/CIFAR-100 | `torchvision` | eksperimen cepat dan kesinambungan dengan Praktikum 02–03 |
| Food-101 | `torchvision` | fine-grained classification dan variasi tampilan |
| EuroSAT | Hugging Face/`torchgeo` | transfer ke citra satelit |
| Beans | Hugging Face Datasets | penyakit daun dan domain shift |
| PatchCamelyon | `torchvision`/TensorFlow Datasets | transfer ke histopatologi |
| Dataset penelitian mahasiswa | repositori resmi masing-masing | validasi eksternal dan peluang kontribusi |

Jika dataset diganti, dokumentasikan sumber, versi, lisensi, unit analisis, split, distribusi kelas, duplikasi, risiko leakage, dan alasan ilmiah pemilihannya.

---

## 6. Desain Eksperimen dan Fair Comparison

### 6.1 Model yang dibandingkan

| Encoder | Sumber pretraining | Arsitektur | Output yang dipakai |
|---|---|---|---|
| DINOv2-Small | Self-supervised, data tanpa label manual | ViT-S/14 | token global `[CLS]` |
| ResNet-50 | Supervised ImageNet-1K | CNN residual | feature sebelum classifier |

Perbandingan ini **bukan isolasi sempurna terhadap sumber supervisi**, karena arsitektur dan skala pretraining juga berbeda. Oleh sebab itu, klaim yang aman adalah:

> “Pada dua checkpoint publik yang diuji dengan protokol downstream yang sama, representasi A menghasilkan ...”

Bukan:

> “Self-supervised learning selalu lebih baik daripada supervised learning.”

### 6.2 Variabel eksperimen

- **Variabel bebas:** encoder, persentase label, seed, dan kondisi clean/shifted.
- **Variabel terikat:** accuracy, balanced accuracy, macro-F1, waktu ekstraksi, throughput, dan penurunan kinerja.
- **Variabel kontrol:** split resmi, sampel per kondisi, preprocessing bawaan checkpoint, classifier, nilai `C`, iterasi maksimum, dan perangkat.

### 6.3 Pencegahan data leakage

1. Gunakan split resmi `trainval` untuk melatih probe.
2. Gunakan split resmi `test` hanya untuk evaluasi akhir.
3. Fitting scaler dan classifier hanya pada embedding train.
4. PCA untuk visualisasi test boleh di-fit pada subset visualisasi karena tidak digunakan untuk prediksi; jelaskan bahwa ini eksploratif.
5. Jangan memilih hyperparameter berdasarkan skor test.
6. Jika melakukan tuning, pisahkan validation set dari `trainval`.

---

## 7. Struktur Folder

```text
praktikum-04-ssl-foundation-vision/
├── Praktikum_04_SSL_Foundation_Vision.ipynb
├── data/
├── cache_embeddings/
├── outputs_pertemuan_04/
│   ├── config.json
│   ├── environment.json
│   ├── embedding_summary.csv
│   ├── linear_probe_runs.csv
│   ├── linear_probe_summary.csv
│   ├── confusion_matrix_*.png
│   ├── tsne_*.png
│   └── research_log.csv
└── laporan_praktikum_04.pdf
```

---

## 8. Setup Environment

### 8.1 Google Colab

Pilih **Runtime → Change runtime type → T4 GPU** jika tersedia, kemudian jalankan:

```python
!pip -q install -U transformers torchvision scikit-learn pandas matplotlib seaborn tqdm joblib
```

Setelah instalasi yang mengubah versi package utama, restart runtime jika Colab memintanya.

### 8.2 Local environment

```bash
python -m venv .venv
```

Aktivasi Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Aktivasi Linux/macOS:

```bash
source .venv/bin/activate
```

Instal dependensi:

```bash
python -m pip install --upgrade pip
python -m pip install torch torchvision transformers scikit-learn pandas matplotlib seaborn tqdm joblib jupyterlab
```

Jalankan notebook:

```bash
jupyter lab
```

> Unduhan pertama dataset dan checkpoint memerlukan koneksi internet. Setelah file tersimpan di cache, run berikutnya dapat menggunakannya kembali.

---

## 9. Import Library dan Konfigurasi Reproducibility

```python
from pathlib import Path
from collections import Counter
import json
import os
import platform
import random
import time

import joblib
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
import sklearn
import torch
import torchvision
import transformers

from PIL import ImageFilter
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression
from sklearn.manifold import TSNE
from sklearn.metrics import (
    accuracy_score,
    balanced_accuracy_score,
    classification_report,
    confusion_matrix,
    f1_score,
)
from sklearn.neighbors import NearestNeighbors
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from torch import nn
from torch.utils.data import DataLoader, Dataset, Subset
from torchvision.datasets import OxfordIIITPet
from torchvision.models import ResNet50_Weights, resnet50
from tqdm.auto import tqdm
from transformers import AutoImageProcessor, AutoModel

print("Python       :", platform.python_version())
print("PyTorch      :", torch.__version__)
print("torchvision  :", torchvision.__version__)
print("transformers :", transformers.__version__)
print("scikit-learn:", sklearn.__version__)
print("CUDA aktif   :", torch.cuda.is_available())
if torch.cuda.is_available():
    print("GPU          :", torch.cuda.get_device_name(0))
```

### Konfigurasi mode

- `QUICK`: demonstrasi kelas; 20 train dan 10 test per kelas; satu seed untuk probe awal.
- `FULL`: seluruh split resmi; tiga seed; dipakai untuk laporan.
- `CUSTOM`: dapat disesuaikan dengan sumber daya.

```python
MODE = "QUICK"  # "QUICK", "FULL", atau "CUSTOM"

DATA_DIR = Path("./data")
CACHE_DIR = Path("./cache_embeddings")
OUTPUT_DIR = Path("./outputs_pertemuan_04")
for directory in [DATA_DIR, CACHE_DIR, OUTPUT_DIR]:
    directory.mkdir(parents=True, exist_ok=True)

if MODE == "QUICK":
    CONFIG = dict(
        train_per_class=20,
        test_per_class=10,
        batch_size=32,
        seeds=[42],
        label_fractions=[0.01, 0.10, 0.50, 1.00],
        tsne_samples=600,
    )
elif MODE == "FULL":
    CONFIG = dict(
        train_per_class=None,
        test_per_class=None,
        batch_size=64,
        seeds=[42, 52, 62],
        label_fractions=[0.01, 0.10, 0.50, 1.00],
        tsne_samples=1500,
    )
else:
    CONFIG = dict(
        train_per_class=50,
        test_per_class=25,
        batch_size=32,
        seeds=[42, 52, 62],
        label_fractions=[0.10, 0.50, 1.00],
        tsne_samples=1000,
    )

CONFIG.update(
    mode=MODE,
    dataset="Oxford-IIIT Pet",
    dinov2_checkpoint="facebook/dinov2-small",
    supervised_checkpoint="ResNet50_Weights.DEFAULT",
    num_workers=0 if os.name == "nt" else 2,
    logistic_C=1.0,
    logistic_max_iter=3000,
)

DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(json.dumps(CONFIG, indent=2))
print("Device:", DEVICE)
```

```python
def set_seed(seed: int = 42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False


set_seed(CONFIG["seeds"][0])
```

---

## 10. Memuat dan Mengaudit Dataset

```python
raw_train = OxfordIIITPet(
    root=DATA_DIR,
    split="trainval",
    target_types="category",
    download=True,
)
raw_test = OxfordIIITPet(
    root=DATA_DIR,
    split="test",
    target_types="category",
    download=True,
)

CLASS_NAMES = raw_train.classes
NUM_CLASSES = len(CLASS_NAMES)

print("Jumlah kelas :", NUM_CLASSES)
print("Trainval     :", len(raw_train))
print("Test         :", len(raw_test))
print("Contoh kelas :", CLASS_NAMES[:10])
```

### 10.1 Mengambil label tanpa memproses seluruh citra

```python
def collect_labels(dataset):
    return np.asarray([dataset[i][1] for i in range(len(dataset))], dtype=np.int64)


train_labels_all = collect_labels(raw_train)
test_labels_all = collect_labels(raw_test)

train_counts = Counter(train_labels_all.tolist())
test_counts = Counter(test_labels_all.tolist())

audit_df = pd.DataFrame({
    "class_id": range(NUM_CLASSES),
    "class_name": CLASS_NAMES,
    "trainval": [train_counts[i] for i in range(NUM_CLASSES)],
    "test": [test_counts[i] for i in range(NUM_CLASSES)],
})
display(audit_df)
```

### 10.2 Fixed subset untuk mode cepat

Sampling dilakukan per kelas agar setiap kelas tetap terwakili.

```python
def fixed_per_class_indices(labels, n_per_class=None, seed=42):
    labels = np.asarray(labels)
    rng = np.random.default_rng(seed)
    selected = []
    for class_id in np.unique(labels):
        class_indices = np.flatnonzero(labels == class_id)
        rng.shuffle(class_indices)
        if n_per_class is not None:
            class_indices = class_indices[:n_per_class]
        selected.extend(class_indices.tolist())
    return sorted(selected)


train_indices = fixed_per_class_indices(
    train_labels_all, CONFIG["train_per_class"], seed=42
)
test_indices = fixed_per_class_indices(
    test_labels_all, CONFIG["test_per_class"], seed=42
)

train_ds = Subset(raw_train, train_indices)
test_ds = Subset(raw_test, test_indices)

print(f"Train digunakan: {len(train_ds):,}")
print(f"Test digunakan : {len(test_ds):,}")
```

### 10.3 Visualisasi satu sampel per beberapa kelas

```python
fig, axes = plt.subplots(3, 4, figsize=(13, 10))
seen = set()
for image, label in train_ds:
    if label not in seen:
        ax = axes.flat[len(seen)]
        ax.imshow(image)
        ax.set_title(CLASS_NAMES[label])
        ax.axis("off")
        seen.add(label)
    if len(seen) == len(axes.flat):
        break
plt.suptitle("Contoh citra Oxford-IIIT Pet", y=1.01)
plt.tight_layout()
plt.show()
```

### Pertanyaan audit data

1. Apakah setiap kelas memiliki jumlah train dan test yang sama?
2. Variasi apa yang tampak pada pose, skala, crop, pencahayaan, dan latar?
3. Kelas mana yang diperkirakan sulit dibedakan?
4. Shortcut apa yang mungkin dipelajari model dari latar atau komposisi?
5. Apakah split resmi cukup untuk menjawab pertanyaan penelitian Anda?

---

## 11. DataLoader untuk Citra PIL

Preprocessing DINOv2 dan ResNet berbeda dan mengikuti checkpoint masing-masing. Dataset mengembalikan citra PIL, lalu preprocessing dilakukan di dalam extractor.

```python
def pil_collate(batch):
    images, labels = zip(*batch)
    return list(images), torch.tensor(labels, dtype=torch.long)


train_loader = DataLoader(
    train_ds,
    batch_size=CONFIG["batch_size"],
    shuffle=False,
    num_workers=CONFIG["num_workers"],
    pin_memory=DEVICE.type == "cuda",
    collate_fn=pil_collate,
)
test_loader = DataLoader(
    test_ds,
    batch_size=CONFIG["batch_size"],
    shuffle=False,
    num_workers=CONFIG["num_workers"],
    pin_memory=DEVICE.type == "cuda",
    collate_fn=pil_collate,
)
```

> `shuffle=False` penting agar urutan embedding tetap sejajar dengan label dan indeks dataset.

---

## 12. Memuat DINOv2 Self-Supervised

```python
DINO_ID = CONFIG["dinov2_checkpoint"]
dinov2_processor = AutoImageProcessor.from_pretrained(DINO_ID)
dinov2_model = AutoModel.from_pretrained(DINO_ID).to(DEVICE).eval()

for parameter in dinov2_model.parameters():
    parameter.requires_grad = False

dinov2_params = sum(p.numel() for p in dinov2_model.parameters())
print(f"DINOv2 parameters: {dinov2_params / 1e6:.2f} M")
```

### Memeriksa bentuk output

```python
sample_images, sample_labels = next(iter(train_loader))
sample_inputs = dinov2_processor(images=sample_images[:2], return_tensors="pt")
sample_inputs = {k: v.to(DEVICE) for k, v in sample_inputs.items()}

with torch.inference_mode():
    sample_outputs = dinov2_model(**sample_inputs)

print("last_hidden_state:", sample_outputs.last_hidden_state.shape)
print("CLS embedding     :", sample_outputs.last_hidden_state[:, 0].shape)
```

Interpretasi umum:

- dimensi pertama: batch;
- dimensi kedua: token `[CLS]` dan patch tokens;
- dimensi ketiga: embedding dimension;
- `[:, 0]`: token global yang digunakan untuk klasifikasi dan retrieval pada modul ini.

---

## 13. Memuat ResNet-50 Supervised Baseline

```python
resnet_weights = ResNet50_Weights.DEFAULT
resnet_transform = resnet_weights.transforms()
resnet_model = resnet50(weights=resnet_weights)
resnet_model.fc = nn.Identity()
resnet_model = resnet_model.to(DEVICE).eval()

for parameter in resnet_model.parameters():
    parameter.requires_grad = False

resnet_params = sum(p.numel() for p in resnet_model.parameters())
print(f"ResNet-50 parameters: {resnet_params / 1e6:.2f} M")
```

### Mengapa preprocessing tidak disamakan secara manual?

Checkpoint dapat menggunakan resize, crop, resampling, mean, dan standard deviation tertentu. Mengikuti preprocessing resmi checkpoint menghindari penurunan kinerja akibat input yang tidak sesuai. Konsekuensinya, biaya preprocessing menjadi bagian dari pipeline aktual masing-masing model dan harus dilaporkan.

---

## 14. Fungsi Ekstraksi Embedding

### 14.1 Normalisasi L2

```python
def l2_normalize(x, eps=1e-12):
    return x / np.clip(np.linalg.norm(x, axis=1, keepdims=True), eps, None)
```

Normalisasi L2 membuat cosine similarity setara dengan dot product dan mengurangi pengaruh skala norma embedding.

### 14.2 Extractor DINOv2

```python
@torch.inference_mode()
def extract_dinov2(loader):
    features, labels = [], []
    start = time.perf_counter()

    for images, targets in tqdm(loader, desc="DINOv2 embedding"):
        inputs = dinov2_processor(images=images, return_tensors="pt")
        inputs = {k: v.to(DEVICE, non_blocking=True) for k, v in inputs.items()}
        outputs = dinov2_model(**inputs)
        batch_features = outputs.last_hidden_state[:, 0]
        features.append(batch_features.cpu().numpy())
        labels.append(targets.numpy())

    elapsed = time.perf_counter() - start
    X = l2_normalize(np.concatenate(features))
    y = np.concatenate(labels)
    return X, y, elapsed
```

### 14.3 Extractor ResNet-50

```python
@torch.inference_mode()
def extract_resnet(loader):
    features, labels = [], []
    start = time.perf_counter()

    for images, targets in tqdm(loader, desc="ResNet-50 embedding"):
        pixel_values = torch.stack([resnet_transform(image) for image in images])
        pixel_values = pixel_values.to(DEVICE, non_blocking=True)
        batch_features = resnet_model(pixel_values)
        features.append(batch_features.cpu().numpy())
        labels.append(targets.numpy())

    elapsed = time.perf_counter() - start
    X = l2_normalize(np.concatenate(features))
    y = np.concatenate(labels)
    return X, y, elapsed
```

### 14.4 Ekstraksi train dan test

```python
embedding_sets = {}
timing_rows = []

for encoder_name, extractor in {
    "dinov2_small_ssl": extract_dinov2,
    "resnet50_supervised": extract_resnet,
}.items():
    X_train, y_train, train_seconds = extractor(train_loader)
    X_test, y_test, test_seconds = extractor(test_loader)

    embedding_sets[encoder_name] = {
        "X_train": X_train,
        "y_train": y_train,
        "X_test": X_test,
        "y_test": y_test,
    }

    timing_rows.append({
        "encoder": encoder_name,
        "embedding_dim": X_train.shape[1],
        "train_images": len(X_train),
        "train_seconds": train_seconds,
        "train_images_per_second": len(X_train) / train_seconds,
        "test_images": len(X_test),
        "test_seconds": test_seconds,
        "test_images_per_second": len(X_test) / test_seconds,
    })

embedding_summary = pd.DataFrame(timing_rows)
display(embedding_summary.round(3))
```

### 14.5 Sanity checks

```python
for name, data in embedding_sets.items():
    assert data["X_train"].ndim == 2
    assert data["X_test"].ndim == 2
    assert len(data["X_train"]) == len(data["y_train"])
    assert len(data["X_test"]) == len(data["y_test"])
    assert np.isfinite(data["X_train"]).all()
    assert np.isfinite(data["X_test"]).all()
    assert np.allclose(np.linalg.norm(data["X_train"], axis=1), 1.0, atol=1e-4)
    assert np.array_equal(data["y_train"], next(iter(embedding_sets.values()))["y_train"])
    assert np.array_equal(data["y_test"], next(iter(embedding_sets.values()))["y_test"])
    print(name, "OK", data["X_train"].shape, data["X_test"].shape)
```

### 14.6 Menyimpan cache embedding

```python
for name, data in embedding_sets.items():
    np.savez_compressed(
        CACHE_DIR / f"{MODE.lower()}_{name}.npz",
        **data,
        train_indices=np.asarray(train_indices),
        test_indices=np.asarray(test_indices),
    )

embedding_summary.to_csv(OUTPUT_DIR / "embedding_summary.csv", index=False)
```

> Cache mencegah ekstraksi backbone berulang. Jika model, preprocessing, dataset, atau indeks berubah, hapus cache lama atau gunakan nama cache baru.

---

## 15. Analisis Norma dan Similarity Embedding

Karena embedding telah dinormalisasi, normanya seharusnya mendekati satu.

```python
for name, data in embedding_sets.items():
    X = data["X_test"]
    norms = np.linalg.norm(X, axis=1)
    random_pairs = np.random.default_rng(42).integers(0, len(X), size=(1000, 2))
    similarities = np.sum(X[random_pairs[:, 0]] * X[random_pairs[:, 1]], axis=1)
    print(
        name,
        "norm mean/std =", round(norms.mean(), 4), round(norms.std(), 4),
        "cosine mean/std =", round(similarities.mean(), 4), round(similarities.std(), 4),
    )
```

Pertanyaan:

1. Apakah distribusi similarity terlalu rapat atau cukup menyebar?
2. Apakah dua encoder memiliki anisotropy yang berbeda?
3. Apakah normalisasi mengubah hasil classifier linear? Uji sebagai ablation opsional.

---

## 16. Visualisasi Embedding dengan PCA dan t-SNE

Visualisasi 2D membantu eksplorasi, tetapi tidak boleh menjadi satu-satunya bukti kualitas representasi. t-SNE dapat mengubah jarak global dan bentuk cluster bergantung pada seed serta hyperparameter.

```python
def stratified_visualization_indices(y, max_samples, seed=42):
    y = np.asarray(y)
    per_class = max(1, max_samples // len(np.unique(y)))
    return np.asarray(fixed_per_class_indices(y, per_class, seed))[:max_samples]


def plot_tsne(X, y, title, output_path, seed=42):
    idx = stratified_visualization_indices(y, CONFIG["tsne_samples"], seed)
    X_sub, y_sub = X[idx], y[idx]

    pca_dim = min(50, X_sub.shape[1], len(X_sub) - 1)
    X_pca = PCA(n_components=pca_dim, random_state=seed).fit_transform(X_sub)
    perplexity = min(30, max(5, (len(X_sub) - 1) // 3))
    X_2d = TSNE(
        n_components=2,
        perplexity=perplexity,
        init="pca",
        learning_rate="auto",
        random_state=seed,
    ).fit_transform(X_pca)

    plt.figure(figsize=(11, 8))
    scatter = plt.scatter(
        X_2d[:, 0], X_2d[:, 1], c=y_sub,
        cmap="gist_ncar", s=15, alpha=0.75,
    )
    plt.colorbar(scatter, label="class id")
    plt.title(title)
    plt.tight_layout()
    plt.savefig(output_path, dpi=180, bbox_inches="tight")
    plt.show()


for name, data in embedding_sets.items():
    plot_tsne(
        data["X_test"], data["y_test"],
        title=f"t-SNE test embedding — {name}",
        output_path=OUTPUT_DIR / f"tsne_{name}.png",
    )
```

### Analisis wajib

- Sebutkan kelas yang tampak terpisah dan kelas yang saling tumpang tindih.
- Bandingkan dengan confusion matrix; apakah cluster yang berdekatan juga sering tertukar?
- Ulangi satu visualisasi dengan seed t-SNE berbeda. Apakah interpretasi utama tetap sama?
- Hindari klaim “cluster terbukti baik” hanya berdasarkan plot.

---

## 17. Nearest-Neighbor Retrieval

Retrieval menguji apakah kedekatan embedding memiliki makna visual dan semantik.

```python
def show_nearest_neighbors(encoder_name, query_position, k=5):
    data = embedding_sets[encoder_name]
    X, y = data["X_test"], data["y_test"]

    nn_index = NearestNeighbors(metric="cosine", n_neighbors=k + 1)
    nn_index.fit(X)
    distances, positions = nn_index.kneighbors(X[query_position:query_position + 1])
    positions = positions[0][1:]
    distances = distances[0][1:]

    fig, axes = plt.subplots(1, k + 1, figsize=(3 * (k + 1), 3.4))
    query_image, query_label = test_ds[query_position]
    axes[0].imshow(query_image)
    axes[0].set_title(f"QUERY\n{CLASS_NAMES[query_label]}")
    axes[0].axis("off")

    for ax, pos, distance in zip(axes[1:], positions, distances):
        image, label = test_ds[int(pos)]
        ax.imshow(image)
        ax.set_title(f"{CLASS_NAMES[label]}\nd={distance:.3f}")
        ax.axis("off")

    plt.suptitle(encoder_name)
    plt.tight_layout()
    plt.show()


show_nearest_neighbors("dinov2_small_ssl", query_position=0, k=5)
show_nearest_neighbors("resnet50_supervised", query_position=0, k=5)
```

### Quantitative retrieval: Precision@k

```python
def precision_at_k(X, y, k=5):
    neighbors = NearestNeighbors(metric="cosine", n_neighbors=k + 1).fit(X)
    indices = neighbors.kneighbors(X, return_distance=False)[:, 1:]
    matches = y[indices] == y[:, None]
    return matches.mean()


for name, data in embedding_sets.items():
    for k in [1, 5, 10]:
        score = precision_at_k(data["X_test"], data["y_test"], k=k)
        print(f"{name:24s} P@{k}: {score:.4f}")
```

> Karena query dan gallery sama-sama berasal dari test set, evaluasi ini bersifat analisis representasi, bukan protokol deployment final.

---

## 18. Linear Probing dan Label Efficiency

### 18.1 Mengambil persentase label secara stratified

Pada fraksi sangat kecil, sampling global dapat menghilangkan kelas tertentu. Fungsi berikut mengambil minimal satu sampel per kelas.

```python
def stratified_fraction_indices(y, fraction, seed):
    y = np.asarray(y)
    rng = np.random.default_rng(seed)
    selected = []

    for class_id in np.unique(y):
        class_indices = np.flatnonzero(y == class_id)
        rng.shuffle(class_indices)
        n_take = max(1, int(round(fraction * len(class_indices))))
        selected.extend(class_indices[:n_take].tolist())

    return np.asarray(sorted(selected))
```

### 18.2 Membangun classifier linear

```python
def build_linear_probe():
    return make_pipeline(
        StandardScaler(),
        LogisticRegression(
            C=CONFIG["logistic_C"],
            max_iter=CONFIG["logistic_max_iter"],
            class_weight="balanced",
            solver="lbfgs",
        ),
    )
```

`StandardScaler` dan logistic regression di-fit hanya pada subset train. Backbone tetap beku. `class_weight="balanced"` mengurangi pengaruh ketidakseimbangan, tetapi bukan pengganti audit distribusi kelas.

### 18.3 Menjalankan seluruh kondisi

```python
probe_rows = []
trained_probes = {}

for encoder_name, data in embedding_sets.items():
    X_train, y_train = data["X_train"], data["y_train"]
    X_test, y_test = data["X_test"], data["y_test"]

    for fraction in CONFIG["label_fractions"]:
        for seed in CONFIG["seeds"]:
            labeled_idx = stratified_fraction_indices(y_train, fraction, seed)
            probe = build_linear_probe()

            start = time.perf_counter()
            probe.fit(X_train[labeled_idx], y_train[labeled_idx])
            fit_seconds = time.perf_counter() - start

            y_pred = probe.predict(X_test)
            row = {
                "encoder": encoder_name,
                "label_fraction": fraction,
                "seed": seed,
                "n_labeled": len(labeled_idx),
                "accuracy": accuracy_score(y_test, y_pred),
                "balanced_accuracy": balanced_accuracy_score(y_test, y_pred),
                "macro_f1": f1_score(y_test, y_pred, average="macro"),
                "probe_fit_seconds": fit_seconds,
            }
            probe_rows.append(row)
            trained_probes[(encoder_name, fraction, seed)] = probe

probe_runs = pd.DataFrame(probe_rows)
display(probe_runs.round(4))
```

> Pada mode `QUICK`, hanya satu seed dijalankan untuk demonstrasi. Laporan final wajib memakai mode `FULL` atau konfigurasi yang menjalankan minimal tiga seed.

### 18.4 Ringkasan mean ± standard deviation

```python
probe_summary = (
    probe_runs
    .groupby(["encoder", "label_fraction"], as_index=False)
    .agg(
        n_labeled_mean=("n_labeled", "mean"),
        accuracy_mean=("accuracy", "mean"),
        accuracy_std=("accuracy", "std"),
        balanced_accuracy_mean=("balanced_accuracy", "mean"),
        balanced_accuracy_std=("balanced_accuracy", "std"),
        macro_f1_mean=("macro_f1", "mean"),
        macro_f1_std=("macro_f1", "std"),
        fit_seconds_mean=("probe_fit_seconds", "mean"),
    )
)

display(probe_summary.round(4))
```

Jika hanya satu seed, `std` akan bernilai `NaN`; jangan menyajikannya sebagai bukti kestabilan.

### 18.5 Kurva label efficiency

```python
plt.figure(figsize=(9, 6))
for name, group in probe_summary.groupby("encoder"):
    group = group.sort_values("label_fraction")
    x = group["label_fraction"] * 100
    y = group["macro_f1_mean"]
    yerr = group["macro_f1_std"].fillna(0)
    plt.errorbar(x, y, yerr=yerr, marker="o", capsize=4, label=name)

plt.xscale("log")
plt.xticks([1, 10, 50, 100], ["1%", "10%", "50%", "100%"])
plt.xlabel("Label training yang digunakan")
plt.ylabel("Macro-F1 pada test")
plt.title("Label efficiency frozen representations")
plt.grid(alpha=0.3)
plt.legend()
plt.tight_layout()
plt.savefig(OUTPUT_DIR / "label_efficiency.png", dpi=180)
plt.show()
```

### Cara membaca hasil

- Jika kinerja tinggi pada 1%, embedding sudah menyimpan informasi yang mudah dipisahkan dengan sedikit label.
- Jika selisih baru muncul pada 100%, representasi mungkin memerlukan lebih banyak contoh untuk membentuk boundary.
- Kurva tidak membuktikan penyebab perbedaan karena arsitektur dan data pretraining tidak dikontrol sempurna.
- Bandingkan mean, variasi antar-seed, dan pola per kelas; jangan hanya membandingkan satu angka maksimum.

---

## 19. Evaluasi Detail pada Kondisi 100% Label

```python
evaluation_objects = {}

for encoder_name, data in embedding_sets.items():
    seed = CONFIG["seeds"][0]
    fraction = 1.0
    probe = trained_probes[(encoder_name, fraction, seed)]
    y_true = data["y_test"]
    y_pred = probe.predict(data["X_test"])

    print("\n", "=" * 80)
    print(encoder_name)
    print(classification_report(
        y_true,
        y_pred,
        target_names=CLASS_NAMES,
        digits=3,
        zero_division=0,
    ))

    evaluation_objects[encoder_name] = {
        "probe": probe,
        "y_true": y_true,
        "y_pred": y_pred,
    }
```

---

## 20. Confusion Matrix

```python
for encoder_name, obj in evaluation_objects.items():
    cm = confusion_matrix(obj["y_true"], obj["y_pred"], normalize="true")
    plt.figure(figsize=(15, 13))
    sns.heatmap(cm, cmap="Blues", vmin=0, vmax=1, cbar_kws={"label": "proporsi"})
    plt.title(f"Normalized confusion matrix — {encoder_name}")
    plt.xlabel("Predicted class")
    plt.ylabel("True class")
    plt.tight_layout()
    plt.savefig(OUTPUT_DIR / f"confusion_matrix_{encoder_name}.png", dpi=180)
    plt.show()
```

### Lima pasangan kelas paling sering tertukar

```python
def top_confusions(y_true, y_pred, class_names, top_n=5):
    cm = confusion_matrix(y_true, y_pred)
    np.fill_diagonal(cm, 0)
    pairs = []
    for true_id, pred_id in np.dstack(
        np.unravel_index(np.argsort(cm.ravel())[::-1], cm.shape)
    )[0]:
        count = cm[true_id, pred_id]
        if count == 0:
            break
        pairs.append({
            "true": class_names[true_id],
            "predicted": class_names[pred_id],
            "count": int(count),
        })
        if len(pairs) == top_n:
            break
    return pd.DataFrame(pairs)


for encoder_name, obj in evaluation_objects.items():
    print(encoder_name)
    display(top_confusions(obj["y_true"], obj["y_pred"], CLASS_NAMES))
```

---

## 21. Failure-Case Analysis

```python
def show_failures(encoder_name, n=12):
    obj = evaluation_objects[encoder_name]
    wrong = np.flatnonzero(obj["y_true"] != obj["y_pred"])
    rng = np.random.default_rng(42)
    chosen = rng.choice(wrong, size=min(n, len(wrong)), replace=False)

    rows = int(np.ceil(len(chosen) / 4))
    fig, axes = plt.subplots(rows, 4, figsize=(14, 3.6 * rows))
    axes = np.atleast_1d(axes).ravel()

    for ax, pos in zip(axes, chosen):
        image, _ = test_ds[int(pos)]
        true_name = CLASS_NAMES[obj["y_true"][pos]]
        pred_name = CLASS_NAMES[obj["y_pred"][pos]]
        ax.imshow(image)
        ax.set_title(f"T: {true_name}\nP: {pred_name}", fontsize=9)
        ax.axis("off")

    for ax in axes[len(chosen):]:
        ax.axis("off")

    plt.suptitle(f"Failure cases — {encoder_name}", y=1.01)
    plt.tight_layout()
    plt.show()


show_failures("dinov2_small_ssl")
show_failures("resnet50_supervised")
```

Kategorikan minimal sepuluh failure cases:

| Kategori dugaan | Contoh indikator |
|---|---|
| Fine-grained ambiguity | breed memiliki bentuk/warna sangat mirip |
| Occlusion | wajah atau tubuh tertutup |
| Scale/crop | objek terlalu kecil atau terpotong |
| Pose | sudut pandang tidak lazim |
| Background shortcut | latar lebih dominan daripada hewan |
| Image quality | blur, noise, pencahayaan buruk |
| Label ambiguity | anotasi sulit dibedakan secara visual |

> Failure analysis menghasilkan hipotesis penyebab. Penyebab baru menjadi lebih kuat setelah diuji melalui eksperimen kontrol.

---

## 22. Uji Robustness terhadap Distribution Shift

Praktikum inti memakai Gaussian blur. Mahasiswa boleh menggantinya dengan brightness reduction, grayscale, occlusion, noise, atau resolusi rendah, tetapi kedua encoder harus menerima shift yang setara.

```python
class PerturbedDataset(Dataset):
    def __init__(self, subset, perturbation):
        self.subset = subset
        self.perturbation = perturbation

    def __len__(self):
        return len(self.subset)

    def __getitem__(self, index):
        image, label = self.subset[index]
        return self.perturbation(image), label


def gaussian_blur(image):
    return image.filter(ImageFilter.GaussianBlur(radius=3.0))


shifted_test_ds = PerturbedDataset(test_ds, gaussian_blur)
shifted_loader = DataLoader(
    shifted_test_ds,
    batch_size=CONFIG["batch_size"],
    shuffle=False,
    num_workers=CONFIG["num_workers"],
    pin_memory=DEVICE.type == "cuda",
    collate_fn=pil_collate,
)
```

```python
shifted_sets = {}
for encoder_name, extractor in {
    "dinov2_small_ssl": extract_dinov2,
    "resnet50_supervised": extract_resnet,
}.items():
    X_shifted, y_shifted, seconds = extractor(shifted_loader)
    shifted_sets[encoder_name] = (X_shifted, y_shifted, seconds)
```

```python
robustness_rows = []
for encoder_name, (X_shifted, y_shifted, seconds) in shifted_sets.items():
    probe = trained_probes[(encoder_name, 1.0, CONFIG["seeds"][0])]

    clean_obj = evaluation_objects[encoder_name]
    clean_acc = accuracy_score(clean_obj["y_true"], clean_obj["y_pred"])
    clean_f1 = f1_score(clean_obj["y_true"], clean_obj["y_pred"], average="macro")

    shifted_pred = probe.predict(X_shifted)
    shifted_acc = accuracy_score(y_shifted, shifted_pred)
    shifted_f1 = f1_score(y_shifted, shifted_pred, average="macro")

    robustness_rows.append({
        "encoder": encoder_name,
        "shift": "gaussian_blur_radius_3",
        "clean_accuracy": clean_acc,
        "shifted_accuracy": shifted_acc,
        "delta_accuracy": clean_acc - shifted_acc,
        "clean_macro_f1": clean_f1,
        "shifted_macro_f1": shifted_f1,
        "delta_macro_f1": clean_f1 - shifted_f1,
    })

robustness_df = pd.DataFrame(robustness_rows)
display(robustness_df.round(4))
```

Interpretasi:

$$
\Delta Acc = Acc_{clean} - Acc_{shifted}
$$

- Nilai $\Delta$ lebih kecil menunjukkan penurunan lebih kecil untuk shift yang diuji.
- Model dengan clean accuracy tertinggi belum tentu memiliki $\Delta$ terkecil.
- Satu jenis shift tidak cukup untuk menyimpulkan robustness umum.

---

## 23. Analisis Efisiensi

```python
efficiency_df = embedding_summary.copy()
efficiency_df["parameters_M"] = efficiency_df["encoder"].map({
    "dinov2_small_ssl": dinov2_params / 1e6,
    "resnet50_supervised": resnet_params / 1e6,
})
efficiency_df["embedding_size_kb_float32"] = (
    efficiency_df["embedding_dim"] * 4 / 1024
)
display(efficiency_df.round(3))
```

Diskusikan:

1. Apakah parameter lebih sedikit berarti throughput lebih tinggi pada perangkat ini?
2. Berapa kebutuhan penyimpanan untuk satu juta embedding?
3. Apakah dimensi lebih besar selalu menghasilkan linear-probe accuracy lebih tinggi?
4. Apakah waktu preprocessing termasuk dalam pengukuran? Pada kode ini: ya.
5. Mengapa hasil throughput tidak boleh dibandingkan langsung jika perangkat atau batch size berbeda?

Estimasi ukuran penyimpanan:

$$
\text{storage bytes} = N \times d \times \text{bytes per value}
$$

---

## 24. Analisis Hasil yang Harus Diisi Mahasiswa

### A. RQ1 — Linear separability

- Encoder dengan accuracy/macro-F1 tertinggi pada 100% label: **...**
- Selisih mean: **...**
- Variasi antar-seed: **...**
- Bukti per kelas yang mendukung: **...**

### B. RQ2 — Label efficiency

- Encoder terbaik pada 1% label: **...**
- Perubahan dari 1% ke 10%, 50%, dan 100%: **...**
- Titik diminishing return: **...**
- Implikasi untuk domain dengan anotasi mahal: **...**

### C. RQ3 — Struktur semantik

- Temuan dari t-SNE: **...**
- Temuan dari nearest-neighbor retrieval: **...**
- Apakah retrieval lebih dipengaruhi breed, warna, pose, atau latar? **...**
- Apakah hasil retrieval konsisten dengan linear probing? **...**

### D. RQ4 — Robustness

- Penurunan accuracy dan macro-F1 setiap encoder: **...**
- Model paling robust untuk shift yang diuji: **...**
- Apakah hasil cukup untuk mengklaim robustness umum? **...**

### E. RQ5 — Efisiensi

- Parameter, dimensi embedding, waktu, dan throughput: **...**
- Trade-off yang paling relevan untuk deployment: **...**

### F. RQ6 — Transferability

- Kesamaan dataset dengan domain penelitian: **...**
- Perbedaan domain yang mengancam validitas eksternal: **...**
- Eksperimen tambahan sebelum model direkomendasikan: **...**

---

## 25. Ablation Study — Pilih Minimal Satu

| Ablation | Variabel diubah | Variabel tetap | Pertanyaan |
|---|---|---|---|
| Normalisasi | embedding asli vs L2 | data, probe, seed | Seberapa penting norma embedding? |
| Probe strength | logistic regression vs kNN | embedding dan split | Apakah fitur baik hanya untuk classifier tertentu? |
| Resolusi | preprocessing standar vs input resolusi lebih rendah | encoder dan data | Seberapa sensitif fitur terhadap detail? |
| Pooling | `[CLS]` vs mean patch tokens DINOv2 | checkpoint dan probe | Representasi global mana yang lebih separable? |
| Label fraction | 1/10/50/100% | test dan probe | Bagaimana sample efficiency? |
| Shift severity | blur radius 1/3/5 | probe dan data test | Bagaimana degradation curve? |
| Encoder scale | DINOv2-Small vs Base | protokol downstream | Apakah skala sepadan dengan biaya? |

Prinsip ablation:

1. ubah satu faktor utama pada satu waktu;
2. gunakan split dan seed yang sama;
3. catat seluruh konfigurasi;
4. laporkan hasil negatif;
5. hindari memilih kondisi berdasarkan test set.

---

## 26. Eksperimen Pengembangan Doktoral

Pilih satu jalur yang paling dekat dengan minat riset.

### A. Transfer lintas domain

Ganti Oxford-IIIT Pet dengan dataset medis, satelit, industri, dokumen, pertanian, atau bawah air. Bandingkan penurunan dari domain natural ke domain target.

### B. Few-shot transfer

Gunakan 1, 2, 4, 8, dan 16 label per kelas. Jalankan minimal lima episode sampling agar estimasi tidak bergantung pada satu subset.

### C. Dense representation

Gunakan patch tokens DINOv2 untuk nearest-neighbor correspondence atau segmentasi sederhana. Jelaskan perbedaan antara global `[CLS]` embedding dan dense patch embedding.

### D. Domain-specific self-supervision

Rancang pretext task yang memanfaatkan struktur domain, misalnya konsistensi antar-view, temporal consistency, multi-scale crop, inpainting area tertentu, atau transformasi fisik yang valid.

### E. Mini-SimCLR

Implementasikan SimCLR skala kecil pada subset dataset. Bandingkan random initialization, supervised ImageNet, dan DINOv2. Nyatakan dengan jelas bahwa eksperimen kecil tidak mereproduksi skala pretraining asli.

### F. Bias dan shortcut

Kelompokkan citra berdasarkan latar, warna dominan, atau kualitas. Uji apakah kinerja berbeda antar-subkelompok dan apakah nearest neighbor mengikuti objek atau latar.

---

## 27. Paper Discussion

Baca minimal dua paper utama dan satu paper yang dekat dengan domain penelitian.

| Paper | Pertanyaan pembacaan kritis |
|---|---|
| DINO | Bagaimana teacher diperbarui? Mengapa centering dan sharpening diperlukan? Apa emergent property yang dilaporkan? |
| DINOv2 | Bagaimana data dikurasi? Objective apa yang digabungkan? Bagaimana klaim general-purpose feature diuji? |
| SimCLR/MoCo/MAE | Apa perbedaan objective, kebutuhan komputasi, dan downstream protocol? |
| Paper domain | Apakah foundation model benar-benar diuji lintas domain, atau hanya fine-tuned pada satu benchmark? |

### Template critical reading

1. Masalah yang ingin diselesaikan: **...**
2. Klaim utama: **...**
3. Data pretraining: **...**
4. Objective/pretext task: **...**
5. Arsitektur dan skala: **...**
6. Downstream tasks: **...**
7. Baseline: **...**
8. Metrik dan protokol evaluasi: **...**
9. Ablation yang paling meyakinkan: **...**
10. Failure case/limitation: **...**
11. Ancaman terhadap validitas: **...**
12. Research gap yang dapat diuji: **...**

---

## 28. Research Log

Isi satu baris untuk setiap run, termasuk run gagal.

| Waktu | Run ID | Encoder | Dataset/split | Seed | Label | Normalisasi | Probe | Shift | Acc | Macro-F1 | Waktu | Catatan/anomali |
|---|---|---|---|---:|---:|---|---|---|---:|---:|---:|---|
| | | | | | | | | | | | | |
| | | | | | | | | | | | | |

### Informasi reproduksibilitas

- OS/runtime: **...**
- CPU/GPU: **...**
- Versi Python, PyTorch, torchvision, transformers, dan scikit-learn: **...**
- Model ID/checkpoint: **...**
- Dataset dan split: **...**
- Mode/configuration: **...**
- Seed: **...**
- Perubahan dari modul: **...**
- Masalah teknis dan penyelesaiannya: **...**

---

## 29. Ekspor Hasil

```python
probe_runs.to_csv(OUTPUT_DIR / "linear_probe_runs.csv", index=False)
probe_summary.to_csv(OUTPUT_DIR / "linear_probe_summary.csv", index=False)
robustness_df.to_csv(OUTPUT_DIR / "robustness_results.csv", index=False)
efficiency_df.to_csv(OUTPUT_DIR / "efficiency_results.csv", index=False)

with open(OUTPUT_DIR / "config.json", "w", encoding="utf-8") as file:
    json.dump(CONFIG, file, indent=2)

environment = {
    "python": platform.python_version(),
    "pytorch": torch.__version__,
    "torchvision": torchvision.__version__,
    "transformers": transformers.__version__,
    "scikit_learn": sklearn.__version__,
    "device": str(DEVICE),
    "gpu": torch.cuda.get_device_name(0) if torch.cuda.is_available() else None,
}
with open(OUTPUT_DIR / "environment.json", "w", encoding="utf-8") as file:
    json.dump(environment, file, indent=2)

for key, probe in trained_probes.items():
    encoder_name, fraction, seed = key
    if fraction == 1.0:
        joblib.dump(
            probe,
            OUTPUT_DIR / f"probe_{encoder_name}_labels100_seed{seed}.joblib",
        )

print("Output:", OUTPUT_DIR.resolve())
for path in sorted(OUTPUT_DIR.iterdir()):
    print("-", path.name)
```

---

## 30. Pertanyaan Diskusi Kritis

1. Apakah perbandingan DINOv2-Small dan ResNet-50 mengisolasi pengaruh self-supervision? Mengapa?
2. Mengapa linear probing lebih tepat daripada full fine-tuning untuk pertanyaan kualitas frozen representation?
3. Apakah accuracy tinggi berarti embedding bersifat semantik?
4. Mengapa t-SNE tidak boleh dijadikan bukti tunggal?
5. Apakah nearest-neighbor yang tampak benar bagi manusia selalu relevan untuk downstream task?
6. Bagaimana data curation pretraining memengaruhi bias dan transferability?
7. Kapan fine-tuning diperlukan meskipun linear probing sudah baik?
8. Apakah satu distribution shift cukup untuk menyimpulkan robustness?
9. Bagaimana menentukan bahwa penurunan kinerja berasal dari domain gap, bukan label noise atau preprocessing?
10. Bagaimana risiko overlap antara data pretraining foundation model dan dataset downstream memengaruhi klaim?
11. Metrik apa yang lebih relevan untuk dataset tidak seimbang daripada accuracy?
12. Jika hasil antar-seed tumpang tindih, klaim apa yang masih aman?
13. Apakah model besar yang lebih akurat selalu layak untuk deployment?
14. Bagaimana menguji apakah embedding lebih bergantung pada tekstur, bentuk, atau latar?
15. Research gap apa yang muncul dari failure case, bukan hanya dari selisih skor?

---

## 31. Tugas Praktikum

### Tugas inti

1. Jalankan audit Oxford-IIIT Pet.
2. Ekstrak embedding DINOv2-Small dan ResNet-50 pada split yang sama.
3. Laporkan dimensi, parameter, waktu, dan throughput.
4. Buat visualisasi PCA/t-SNE untuk kedua encoder.
5. Tampilkan minimal lima query nearest-neighbor dari kelas berbeda.
6. Hitung Precision@1, Precision@5, dan Precision@10.
7. Jalankan linear probing pada 1%, 10%, 50%, dan 100% label.
8. Jalankan minimal tiga seed untuk laporan final.
9. Laporkan accuracy, balanced accuracy, dan macro-F1 sebagai mean ± std.
10. Analisis confusion matrix dan minimal sepuluh failure cases.
11. Jalankan satu robustness test dan satu ablation.
12. Turunkan minimal satu research gap dan desain eksperimen lanjutannya.

### Deliverable

- notebook `.ipynb` yang dapat dijalankan ulang;
- laporan PDF 6–10 halaman;
- folder output berisi tabel `.csv`, konfigurasi `.json`, dan visualisasi;
- research log;
- satu slide ringkasan hasil dan rekomendasi;
- tautan/salinan paper yang dibahas sesuai ketentuan hak cipta institusi.

---

## 32. Struktur Laporan

1. **Judul dan identitas**
2. **Latar belakang dan posisi terhadap Praktikum 01–03**
3. **Research question dan hipotesis awal**
4. **Konsep singkat SSL, DINOv2, foundation model, dan linear probing**
5. **Dataset audit**, lisensi/sumber, split, dan potensi bias
6. **Metode**, preprocessing, checkpoint, sampling label, seed, serta hardware
7. **Hasil embedding**, t-SNE/PCA, retrieval, dan Precision@k
8. **Hasil linear probing**, mean ± std dan label-efficiency curve
9. **Confusion matrix dan failure analysis**
10. **Robustness dan ablation**
11. **Efisiensi dan trade-off deployment**
12. **Diskusi validitas**, termasuk confounding arsitektur dan skala pretraining
13. **Research gap dan eksperimen lanjutan**
14. **Kesimpulan dan rekomendasi untuk domain penelitian**
15. **Research log dan referensi**

---

## 33. Rubrik Penilaian

| Aspek | Bobot |
|---|---:|
| Ketepatan setup, data, preprocessing, dan reproducibility | 15% |
| Ketepatan ekstraksi embedding dan fair comparison | 15% |
| Linear probing, multi-seed, metrik, dan label efficiency | 20% |
| Visualisasi embedding, retrieval, dan confusion matrix | 15% |
| Failure analysis, robustness, dan ablation | 20% |
| Kedalaman diskusi, validitas klaim, research gap, dan rekomendasi | 15% |
| **Total** | **100%** |

---

## 34. Checklist Penyelesaian

### Setup dan data

- [ ] Environment dan device berhasil dideteksi.
- [ ] Versi package dan hardware dicatat.
- [ ] Oxford-IIIT Pet berhasil diunduh.
- [ ] Split resmi trainval/test digunakan.
- [ ] Distribusi kelas dan variasi visual diaudit.
- [ ] Fixed subset terdokumentasi jika tidak memakai seluruh data.

### Model dan embedding

- [ ] DINOv2-Small berhasil dimuat.
- [ ] ResNet-50 supervised berhasil dimuat.
- [ ] Kedua backbone dibekukan.
- [ ] Preprocessing checkpoint digunakan dengan benar.
- [ ] Embedding train/test sejajar dengan label.
- [ ] Sanity check finite value, bentuk, dan norma lolos.
- [ ] Cache embedding disimpan.

### Analisis representasi

- [ ] PCA/t-SNE dibuat untuk kedua encoder.
- [ ] Keterbatasan t-SNE dijelaskan.
- [ ] Minimal lima query retrieval diperiksa.
- [ ] Precision@1/5/10 dihitung.

### Linear probe dan evaluasi

- [ ] Label 1/10/50/100% diambil secara stratified.
- [ ] Minimal tiga seed digunakan pada laporan final.
- [ ] Scaler hanya di-fit pada train.
- [ ] Test tidak dipakai untuk memilih hyperparameter.
- [ ] Accuracy, balanced accuracy, dan macro-F1 dilaporkan.
- [ ] Mean ± std dihitung dengan benar.
- [ ] Confusion matrix dan per-class result dianalisis.

### Analisis ilmiah

- [ ] Minimal sepuluh failure cases dikategorikan.
- [ ] Satu robustness test diselesaikan.
- [ ] Satu ablation study diselesaikan.
- [ ] Efisiensi dan storage embedding dihitung.
- [ ] Confounding arsitektur/pretraining dibahas.
- [ ] Klaim tidak melebihi bukti.
- [ ] Research gap dan eksperimen lanjutan dirumuskan.
- [ ] Hubungan ke Praktikum 05 dijelaskan.

### Pengumpulan

- [ ] Notebook dijalankan dari awal sampai akhir tanpa error.
- [ ] Sel output penting tidak dihapus.
- [ ] File CSV/JSON/gambar tersedia.
- [ ] Research log lengkap, termasuk run gagal.
- [ ] Laporan dan slide ringkasan disertakan.

---

## 35. Troubleshooting

### `ModuleNotFoundError: No module named 'transformers'`

```bash
python -m pip install -U transformers
```

Pastikan kernel notebook menggunakan environment yang sama dengan lokasi instalasi.

### Dataset gagal diunduh

- periksa koneksi dan ruang penyimpanan;
- hapus hanya arsip yang gagal/korup, bukan seluruh folder kerja;
- jalankan ulang dengan `download=True`;
- pada jaringan institusi, periksa aturan proxy tanpa memasukkan kredensial ke notebook.

### Checkpoint DINOv2 gagal dimuat

- periksa koneksi ke Hugging Face;
- perbarui `transformers`;
- pastikan ID tepat: `facebook/dinov2-small`;
- simpan pesan error lengkap di research log.

### CUDA out of memory

- turunkan `batch_size` menjadi 16, 8, atau 4;
- ekstrak satu encoder pada satu waktu;
- hapus objek model yang tidak dipakai dan jalankan `torch.cuda.empty_cache()`;
- gunakan CPU jika GPU tidak tersedia, dengan subset `QUICK`.

### `DataLoader worker exited unexpectedly`

Ubah:

```python
CONFIG["num_workers"] = 0
```

### Logistic regression tidak konvergen

- naikkan `logistic_max_iter`;
- pastikan `StandardScaler` digunakan;
- catat warning, bukan menyembunyikannya;
- jangan mengubah banyak hyperparameter sekaligus.

### t-SNE terlalu lambat

- turunkan `tsne_samples`;
- gunakan PCA sebelum t-SNE;
- jangan menggunakan seluruh dataset jika visualisasi hanya bersifat eksploratif.

### Standard deviation bernilai `NaN`

Ini terjadi jika hanya satu seed. Jalankan minimal tiga seed pada mode final.

### Hasil berbeda antarrun

Sedikit variasi dapat tetap terjadi akibat implementasi GPU. Catat seed, perangkat, versi library, dan toleransi reproduksibilitas. Jangan menghapus variasi yang tidak sesuai dugaan.

---

## 36. Validitas dan Etika Penelitian

### Validitas internal

- urutan data dan label harus identik antarmodel;
- preprocessing harus sesuai checkpoint;
- probe dan split harus sama;
- test set tidak boleh mengarahkan pemilihan model.

### Validitas eksternal

- Oxford-IIIT Pet hanya satu domain natural;
- generalisasi ke domain lain memerlukan external validation;
- hasil satu jenis perturbasi bukan robustness universal.

### Validitas konstruk

- linear probing mengukur linear separability, bukan seluruh informasi dalam embedding;
- t-SNE mengukur tampilan proyeksi lokal, bukan kualitas representasi secara keseluruhan;
- accuracy tidak cukup untuk semua domain.

### Etika dan governance

- periksa lisensi dataset dan checkpoint;
- jangan mengunggah data sensitif ke layanan publik tanpa izin;
- audit bias representasi dan ketimpangan subkelompok;
- dokumentasikan sumber data, model card, dan keterbatasan;
- hindari klaim klinis atau keselamatan tanpa validasi domain yang sesuai.

---

## 37. Kesimpulan Konseptual

Praktikum 01–03 menyiapkan tiga fondasi: baseline, mekanisme arsitektur, dan perbandingan pretrained model. Praktikum 04 mengubah fokus dari arsitektur menuju **cara representasi diperoleh dan dipindahkan**.

Pesan utama:

1. label downstream tidak selalu harus banyak jika embedding pretrained sudah kuat;
2. linear probing menguji separabilitas frozen representation;
3. visualisasi dan retrieval membantu membaca struktur embedding, tetapi harus didukung metrik;
4. foundation model tidak otomatis bebas bias atau selalu transferable;
5. sumber pretraining, arsitektur, skala data, preprocessing, dan domain gap saling berkontribusi;
6. failure case, label-efficiency curve, dan robustness test dapat membuka research gap;
7. rekomendasi ilmiah harus dibatasi pada data dan kondisi yang benar-benar diuji.

Hasil praktikum ini menjadi fondasi Pertemuan 05, ketika mahasiswa mempelajari bagaimana representasi visual dan bahasa disejajarkan dalam vision-language model dan digunakan untuk zero-shot prediction.

---

## 38. Referensi Utama

1. Caron, M., et al. (2021). *Emerging Properties in Self-Supervised Vision Transformers*. ICCV. <https://arxiv.org/abs/2104.14294>
2. Oquab, M., et al. (2023/2024). *DINOv2: Learning Robust Visual Features without Supervision*. TMLR. <https://arxiv.org/abs/2304.07193>
3. Chen, T., et al. (2020). *A Simple Framework for Contrastive Learning of Visual Representations*. ICML. <https://arxiv.org/abs/2002.05709>
4. He, K., et al. (2020). *Momentum Contrast for Unsupervised Visual Representation Learning*. CVPR. <https://arxiv.org/abs/1911.05722>
5. He, K., et al. (2022). *Masked Autoencoders Are Scalable Vision Learners*. CVPR. <https://arxiv.org/abs/2111.06377>
6. Parkhi, O. M., et al. (2012). *Cats and Dogs*. CVPR. <https://www.robots.ox.ac.uk/~vgg/data/pets/>
7. Dokumentasi DINOv2 — Hugging Face Transformers: <https://huggingface.co/docs/transformers/model_doc/dinov2>
8. Dokumentasi Oxford-IIIT Pet — torchvision: <https://docs.pytorch.org/vision/stable/generated/torchvision.datasets.OxfordIIITPet.html>
9. Dokumentasi ResNet-50 dan pretrained weights — torchvision: <https://docs.pytorch.org/vision/stable/models/generated/torchvision.models.resnet50.html>

---

## Penutup

Gunakan foundation vision model sebagai objek penelitian, bukan sekadar alat siap pakai. Tanyakan selalu:

- data apa yang membentuk representasinya;
- objective apa yang dipakai;
- informasi apa yang tersimpan atau hilang;
- pada domain dan kondisi apa representasi tersebut gagal;
- bukti tambahan apa yang diperlukan sebelum membuat klaim.

