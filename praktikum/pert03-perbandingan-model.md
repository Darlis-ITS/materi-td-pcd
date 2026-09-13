# Modul Praktikum Pertemuan 03
## CNN Modern, Attention, dan Vision Transformer untuk Klasifikasi CIFAR-10

**Mata kuliah:** EF256129 - Topik Dalam Pengolahan Citra Digital  
**Program:** Doktor/S3 Teknik Informatika  
**Dosen:** Dr. Darlis Herumurti  
**Departemen:** Teknik Informatika - ITS  

---

## 1. Deskripsi Praktikum

Praktikum pertemuan 3 berfokus pada **perbandingan arsitektur model visual modern** untuk klasifikasi citra. Fokusnya bukan hanya membandingkan CNN dan Vision Transformer secara sederhana, tetapi juga membaca perkembangan arsitektur dari CNN residual, CNN modern, model efisien, Vision Transformer berbasis patch, hierarchical Transformer, sampai arsitektur hybrid.

Dataset yang digunakan adalah **CIFAR-10**, yaitu dataset publik berisi 60.000 citra berwarna dalam 10 kelas. Citra asli berukuran 32 x 32 piksel, kemudian diubah menjadi 256 x 256 agar sesuai dengan model pretrained ImageNet dari `timm`.

Model dibandingkan berdasarkan:

- akurasi;
- macro-F1;
- jumlah parameter;
- FLOPs;
- latensi inference;
- throughput;
- waktu training;
- stabilitas antar seed;
- trade-off antara performa dan biaya komputasi.

Praktikum ini juga melatih mahasiswa untuk tidak hanya mencari model dengan akurasi tertinggi, tetapi membaca **apakah performa tersebut sepadan dengan ukuran model, waktu training, kebutuhan GPU, dan tujuan penelitian**.

---

## 2. Capaian Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa mampu:

1. menjelaskan perbedaan CNN klasik, CNN modern, Vision Transformer, hierarchical Transformer, dan arsitektur hybrid;
2. menggunakan `timm`, PyTorch, dan torchvision untuk transfer learning;
3. merancang eksperimen perbandingan model secara terkontrol;
4. menjalankan head-only training dan full fine-tuning;
5. mengevaluasi model menggunakan accuracy, macro-F1, confusion matrix, dan classification report;
6. membandingkan model berdasarkan parameter, FLOPs, latency, throughput, dan waktu training;
7. menganalisis pengaruh random seed terhadap stabilitas hasil;
8. menyusun kesimpulan ilmiah yang proporsional terhadap bukti eksperimen.

---

## 3. Pertanyaan Penelitian Praktikum

Gunakan pertanyaan berikut sebagai dasar analisis.

| Kode | Pertanyaan |
|---|---|
| RQ1 | Model mana yang memberikan performa klasifikasi terbaik pada CIFAR-10 dengan data target terbatas? |
| RQ2 | Bagaimana trade-off antara accuracy, macro-F1, jumlah parameter, FLOPs, latency, dan throughput? |
| RQ3 | Apakah model yang lebih besar selalu lebih baik daripada model kecil? |
| RQ4 | Bagaimana perbedaan CNN, Transformer, dan hybrid memengaruhi performa dan efisiensi? |
| RQ5 | Apakah hasil konsisten pada beberapa random seed? |
| RQ6 | Kelas atau contoh seperti apa yang masih sulit diklasifikasikan? |

Sebelum menjalankan eksperimen, mahasiswa menuliskan hipotesis awal.

| Hipotesis | Prediksi dan alasan |
|---|---|
| H1 - Kinerja | Model dengan kapasitas lebih besar diperkirakan memiliki akurasi lebih tinggi. |
| H2 - Efisiensi | Model ringan seperti MobileNetV3 dan EfficientNet-B0 diperkirakan lebih cepat, tetapi akurasinya lebih rendah. |
| H3 - Fine-tuning | Full fine-tuning diperkirakan lebih baik daripada head-only, tetapi membutuhkan waktu dan memori lebih besar. |
| H4 - Stabilitas | Model yang akurasinya tinggi tetapi standar deviasinya besar belum tentu paling dapat diandalkan. |

---

## 4. Desain Eksperimen

Eksperimen harus dibuat adil agar perbedaan hasil terutama berasal dari arsitektur model, bukan dari konfigurasi yang berbeda.

| Komponen | Ketentuan |
|---|---|
| Dataset | CIFAR-10 |
| Split | Train, validation, dan test dipisahkan secara konsisten |
| Resolusi input | 256 x 256 |
| Pretraining | Menggunakan bobot pretrained dari `timm` |
| Optimizer | AdamW |
| Loss function | Cross-entropy |
| Batch size | Sama untuk semua model dalam satu mode eksperimen |
| Epoch | Sama untuk semua model dalam satu mode eksperimen |
| Seed | Beberapa seed untuk mengukur stabilitas |
| Model selection | Model terbaik dipilih berdasarkan validation accuracy |
| Evaluasi akhir | Test set digunakan setelah model dipilih |

**Catatan metodologis:** konfigurasi yang sama membuat eksperimen mudah dikontrol, tetapi belum tentu optimal untuk semua arsitektur. Untuk riset publikasi, setiap model sebaiknya diberi kesempatan tuning hyperparameter dengan budget yang setara.

---

## 5. Setup Environment

Notebook dapat dijalankan di:

- Google Colab dengan runtime GPU;
- Jupyter Notebook atau JupyterLab;
- VS Code dengan Python environment;
- server lokal dengan GPU CUDA.

Untuk RTX 5090 32 GB, konfigurasi awal yang aman adalah `batch_size=32`, terutama untuk full fine-tuning model besar.

### 5.1 Membuat Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

Pada Windows PowerShell:

```powershell
.venv\Scripts\activate
python -m pip install --upgrade pip
```

### 5.2 Instalasi Library

```python
%pip install -q "timm>=1.0.20,<1.1" "torchinfo>=1.8" "fvcore>=0.1.5" \
    "scikit-learn>=1.3" "seaborn>=0.13" "pandas>=2.0" "matplotlib>=3.7"
```

Library utama:

| Library | Fungsi |
|---|---|
| `torch` | Training dan inference deep learning |
| `torchvision` | Dataset CIFAR-10 dan transformasi citra |
| `timm` | Memuat model pretrained modern |
| `scikit-learn` | Accuracy, F1-score, confusion matrix |
| `pandas` | Mengelola tabel hasil eksperimen |
| `matplotlib`, `seaborn` | Visualisasi hasil |
| `fvcore` | Estimasi FLOPs |

### 5.3 Import Library dan Verifikasi GPU

```python
import copy
import json
import math
import os
import platform
import random
import time
import warnings
from pathlib import Path

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
import sklearn
import timm
import torch
import torch.nn as nn
import torchvision
from IPython.display import display
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix, f1_score
from sklearn.model_selection import train_test_split
from torch.utils.data import DataLoader, Subset
from torchvision import datasets, transforms
from tqdm.auto import tqdm

warnings.filterwarnings("ignore", category=UserWarning)
sns.set_theme(style="whitegrid", context="notebook")

print("Python      :", platform.python_version())
print("PyTorch     :", torch.__version__)
print("Torchvision :", torchvision.__version__)
print("timm        :", timm.__version__)
print("CUDA aktif  :", torch.cuda.is_available())
if torch.cuda.is_available():
    print("GPU         :", torch.cuda.get_device_name(0))
```

---

## 6. Konfigurasi Eksperimen

Notebook menyediakan tiga mode:

| Mode | Fungsi |
|---|---|
| `QUICK` | Demonstrasi cepat di kelas, subset kecil, 1 seed, epoch sedikit |
| `FULL` | Eksperimen lebih lengkap dengan data dan epoch lebih besar |
| `CUSTOM` | Konfigurasi dapat disesuaikan dengan sumber daya |

Contoh konfigurasi yang digunakan pada praktikum:

```python
MODE = "CUSTOM"
HEAD_ONLY = False
DATA_DIR = Path("./data")
OUTPUT_DIR = Path("./outputs_pertemuan_03_architecture_comparison")
OUTPUT_DIR.mkdir(parents=True, exist_ok=True)

CONFIG = dict(
    train_size=10_000,
    val_size=5_000,
    test_size=10_000,
    epochs=5,
    batch_size=32,
    seeds=[42, 52, 62],
    image_size=256,
    num_workers=4,
    pin_memory=True,
    persistent_workers=True,
    prefetch_factor=4,
    learning_rate=6e-5,
    weight_decay=1e-4,
    head_only=HEAD_ONLY,
    dataset="CIFAR-10",
)

DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(json.dumps(CONFIG, indent=2))
print("Device:", DEVICE)
```

### 6.1 Head-Only dan Full Fine-Tuning

Ada dua mode training utama.

| Mode | Penjelasan | Kelebihan | Keterbatasan |
|---|---|---|---|
| Head-only | Backbone dibekukan, hanya classifier dilatih | Cepat, hemat VRAM, cocok untuk linear probing | Adaptasi fitur terbatas |
| Full fine-tuning | Seluruh parameter model dilatih | Adaptasi domain lebih kuat | Lebih lambat dan lebih boros VRAM |

Pada mode `head_only`, jumlah parameter total model tetap sama, tetapi parameter yang diperbarui optimizer jauh lebih sedikit. Pada mode `full_finetuning`, seluruh backbone dan classifier dapat berubah selama training.

---

## 7. Random Seed dan Reproducibility

Random seed digunakan agar eksperimen lebih dapat direproduksi.

```python
def set_seed(seed: int = 42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False
```

Jika seed, konfigurasi, versi library, dan hardware sama, maka proses sampling data, inisialisasi tertentu, dan augmentasi acak akan lebih konsisten. Namun, pada GPU modern, masih mungkin ada variasi kecil karena operasi paralel dan kernel CUDA tertentu.

Oleh karena itu, eksperimen menggunakan beberapa seed, misalnya `42`, `52`, dan `62`, agar hasil tidak hanya bergantung pada satu run.

---

## 8. Dataset CIFAR-10

CIFAR-10 memiliki 10 kelas:

```text
airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck
```

Alasan pemilihan CIFAR-10:

- dataset publik dan mudah direproduksi;
- ukuran relatif kecil sehingga cocok untuk praktikum;
- memiliki kelas yang mirip secara visual, seperti `cat` dan `dog`, atau `automobile` dan `truck`;
- cocok untuk membahas transfer learning dan inductive bias.

Keterbatasan penting: citra CIFAR-10 berukuran 32 x 32. Ketika di-resize ke 256 x 256, detail visual baru tidak tercipta. Resize hanya menyesuaikan input agar cocok dengan model pretrained ImageNet.

### 8.1 Memuat Dataset

```python
raw_train = datasets.CIFAR10(DATA_DIR, train=True, download=True)
raw_test = datasets.CIFAR10(DATA_DIR, train=False, download=True)

CLASS_NAMES = raw_train.classes
NUM_CLASSES = len(CLASS_NAMES)

print("Kelas:", CLASS_NAMES)
print("Train asli:", len(raw_train), "| Test asli:", len(raw_test))
```

### 8.2 Transformasi Data

Normalisasi menggunakan statistik ImageNet karena model menggunakan bobot pretrained ImageNet.

```python
IMAGENET_MEAN = (0.485, 0.456, 0.406)
IMAGENET_STD = (0.229, 0.224, 0.225)

train_transform = transforms.Compose([
    transforms.Resize((CONFIG["image_size"], CONFIG["image_size"])),
    transforms.RandomHorizontalFlip(p=0.5),
    transforms.RandomRotation(10),
    transforms.ColorJitter(brightness=0.15, contrast=0.15, saturation=0.10),
    transforms.ToTensor(),
    transforms.Normalize(IMAGENET_MEAN, IMAGENET_STD),
])

eval_transform = transforms.Compose([
    transforms.Resize((CONFIG["image_size"], CONFIG["image_size"])),
    transforms.ToTensor(),
    transforms.Normalize(IMAGENET_MEAN, IMAGENET_STD),
])
```

Perbedaan transformasi:

| Split | Transformasi |
|---|---|
| Train | Resize, augmentasi, ToTensor, normalisasi |
| Validation | Resize, ToTensor, normalisasi |
| Test | Resize, ToTensor, normalisasi |

Augmentasi hanya diterapkan pada training set. Validation dan test harus deterministik agar evaluasi stabil.

### 8.3 Split Data Berstrata

Split berstrata menjaga distribusi kelas tetap seimbang.

```python
def stratified_take(indices, labels, n, seed):
    indices = np.asarray(indices)
    if n is None or n >= len(indices):
        return indices.tolist()
    selected, _ = train_test_split(
        indices,
        train_size=n,
        stratify=np.asarray(labels)[indices],
        random_state=seed,
    )
    return selected.tolist()

all_train_idx = np.arange(len(raw_train))
train_idx, val_idx = train_test_split(
    all_train_idx,
    test_size=10_000,
    stratify=np.asarray(raw_train.targets),
    random_state=42,
)

train_idx = stratified_take(train_idx, raw_train.targets, CONFIG["train_size"], 42)
val_idx = stratified_take(val_idx, raw_train.targets, CONFIG["val_size"], 42)
test_idx = stratified_take(np.arange(len(raw_test)), raw_test.targets, CONFIG["test_size"], 42)
```

---

## 9. Model yang Dibandingkan

Semua model dimuat menggunakan `timm`.

| Model | Keluarga | Karakteristik |
|---|---|---|
| ResNet-18 | CNN residual | Baseline CNN ringan |
| ResNet-50 | CNN residual | CNN lebih dalam dan umum digunakan |
| ResNeXt-101 32x8d | CNN residual variant | Grouped convolution dan kapasitas besar |
| ConvNeXt-Tiny | CNN modern | CNN dengan desain terinspirasi Transformer |
| ConvNeXtV2-Base | CNN modern | CNN modern berkapasitas besar |
| EfficientNet-B0 | CNN efisien | Compound scaling dan parameter kecil |
| MobileNetV3-Large | CNN mobile | Efisien untuk edge dan mobile |
| ViT-Base/16 | Vision Transformer | Patch token dan global self-attention |
| ViT-Large/14 CLIP | Vision Transformer | Model besar dengan pretraining CLIP |
| Swin-Tiny | Hierarchical Transformer | Windowed dan shifted-window attention |
| SwinV2-Base | Hierarchical Transformer | Swin versi lebih besar dan stabil |
| DeiT-Tiny/16 | Data-efficient ViT | ViT kecil untuk eksperimen cepat |
| DeiT-Small/16 | Data-efficient ViT | ViT kecil-menengah yang efisien |
| BEiT-v2-Base | Masked image modeling | Pretraining berbasis token visual |
| MaxViT-Base | Hybrid attention | Menggabungkan attention lokal dan global |
| EVA02-Base/14 | Foundation vision model | Representasi visual kuat dari pretraining besar |

### 9.1 Konfigurasi Model

```python
MODEL_SPECS = {
    "resnet18": "resnet18",
    "resnet50": "resnet50",
    "resnext101_32x8d": "resnext101_32x8d",
    "convnext_tiny": "convnext_tiny",
    "convnextv2_base": "convnextv2_base",
    "efficientnet_b0": "efficientnet_b0",
    "mobilenetv3_large_100": "mobilenetv3_large_100",
    "vit_base_patch16_224": "vit_base_patch16_224",
    "vit_large_patch14_clip_224": "vit_large_patch14_clip_224",
    "swin_tiny_patch4_window7_224": "swin_tiny_patch4_window7_224",
    "swinv2_base_window8_256": "swinv2_base_window8_256",
    "deit_tiny_patch16_224": "deit_tiny_patch16_224",
    "deit_small_patch16_224": "deit_small_patch16_224",
    "beitv2_base_patch16_224": "beitv2_base_patch16_224",
    "maxvit_rmlp_base_rw_224": "maxvit_rmlp_base_rw_224",
    "eva02_base_patch14_448": "eva02_base_patch14_448",
}
```

### 9.2 Membuat Model dari timm

```python
def create_model(model_name, pretrained=True, head_only=False):
    model = timm.create_model(
        model_name,
        pretrained=pretrained,
        num_classes=NUM_CLASSES,
    )

    if head_only:
        for param in model.parameters():
            param.requires_grad = False

        classifier = model.get_classifier()
        for param in classifier.parameters():
            param.requires_grad = True

    return model
```

Jika API classifier berbeda antar model, gunakan fungsi utilitas dari notebook untuk mengambil dan mengganti classifier secara aman.

---

## 10. Training Model

Training menggunakan cross-entropy dan AdamW.

```python
criterion = nn.CrossEntropyLoss()

optimizer = torch.optim.AdamW(
    filter(lambda p: p.requires_grad, model.parameters()),
    lr=CONFIG["learning_rate"],
    weight_decay=CONFIG["weight_decay"],
)
```

### 10.1 Satu Epoch Training

```python
def train_one_epoch(model, loader, optimizer, criterion):
    model.train()
    running_loss = 0.0
    all_true, all_pred = [], []

    for images, labels in tqdm(loader, leave=False):
        images = images.to(DEVICE, non_blocking=True)
        labels = labels.to(DEVICE, non_blocking=True)

        optimizer.zero_grad(set_to_none=True)
        outputs = model(images)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        running_loss += loss.item() * images.size(0)
        preds = outputs.argmax(dim=1)
        all_true.extend(labels.cpu().numpy())
        all_pred.extend(preds.cpu().numpy())

    epoch_loss = running_loss / len(loader.dataset)
    epoch_acc = accuracy_score(all_true, all_pred)
    return epoch_loss, epoch_acc
```

### 10.2 Evaluasi

```python
@torch.inference_mode()
def evaluate(model, loader, criterion):
    model.eval()
    running_loss = 0.0
    all_true, all_pred = [], []

    for images, labels in tqdm(loader, leave=False):
        images = images.to(DEVICE, non_blocking=True)
        labels = labels.to(DEVICE, non_blocking=True)

        outputs = model(images)
        loss = criterion(outputs, labels)
        preds = outputs.argmax(dim=1)

        running_loss += loss.item() * images.size(0)
        all_true.extend(labels.cpu().numpy())
        all_pred.extend(preds.cpu().numpy())

    epoch_loss = running_loss / len(loader.dataset)
    epoch_acc = accuracy_score(all_true, all_pred)
    return epoch_loss, epoch_acc
```

### 10.3 Pemilihan Model Terbaik

Bobot terbaik dipilih berdasarkan validation accuracy.

```python
best_val_acc = -1
best_state = None

for epoch in range(CONFIG["epochs"]):
    train_loss, train_acc = train_one_epoch(model, train_loader, optimizer, criterion)
    val_loss, val_acc = evaluate(model, val_loader, criterion)

    if val_acc > best_val_acc:
        best_val_acc = val_acc
        best_state = copy.deepcopy(model.state_dict())

model.load_state_dict(best_state)
```

Test set tidak digunakan untuk memilih model. Test set hanya dipakai setelah proses training dan pemilihan model selesai.

---

## 11. Evaluasi Kinerja

Metrik utama:

| Metrik | Fungsi |
|---|---|
| Accuracy | Proporsi prediksi benar |
| Macro-F1 | Rata-rata F1 semua kelas dengan bobot setara |
| Confusion matrix | Melihat pola kesalahan antar kelas |
| Classification report | Precision, recall, dan F1 per kelas |

Accuracy cocok untuk CIFAR-10 karena datanya seimbang. Namun, macro-F1 tetap penting agar mahasiswa tidak hanya melihat skor agregat.

```python
y_true, y_pred, y_prob = predict(model, test_loader)

test_accuracy = accuracy_score(y_true, y_pred)
test_macro_f1 = f1_score(y_true, y_pred, average="macro")

print("Test Accuracy:", test_accuracy)
print("Test Macro-F1:", test_macro_f1)
```

### 11.1 Ringkasan Mean dan Standard Deviation

```python
summary_df = (
    per_run_df.groupby(["ablation", "model"], as_index=False)
    .agg(
        accuracy_mean=("test_accuracy", "mean"),
        accuracy_std=("test_accuracy", "std"),
        macro_f1_mean=("test_macro_f1", "mean"),
        macro_f1_std=("test_macro_f1", "std"),
        training_seconds_mean=("training_seconds", "mean"),
        runs=("seed", "count"),
    )
)
```

Standar deviasi membantu melihat stabilitas. Model dengan rata-rata tinggi tetapi standar deviasi besar perlu dianalisis hati-hati.

---

## 12. Confusion Matrix dan Error Analysis

Confusion matrix digunakan untuk melihat kelas yang sering tertukar.

```python
cm = confusion_matrix(y_true, y_pred, normalize="true")

sns.heatmap(
    cm,
    annot=True,
    fmt=".2f",
    cmap="Blues",
    xticklabels=CLASS_NAMES,
    yticklabels=CLASS_NAMES,
)
plt.xlabel("Prediksi")
plt.ylabel("Label sebenarnya")
plt.show()
```

Contoh pola kesalahan yang mungkin muncul pada CIFAR-10:

| Pasangan kelas | Kemungkinan penyebab |
|---|---|
| cat - dog | Bentuk tubuh mirip, resolusi rendah |
| deer - horse | Struktur tubuh dan background mirip |
| automobile - truck | Objek kendaraan darat dengan bentuk mirip |
| bird - airplane | Siluet atau background langit |
| ship - airplane | Background dan bentuk global tertentu |

Error analysis tidak hanya mencatat model salah, tetapi menjawab **mengapa model salah** dan **apakah kesalahan tersebut berhubungan dengan karakter arsitektur**.

---

## 13. Analisis Efisiensi

Selain akurasi, model perlu dibandingkan dari sisi efisiensi.

| Ukuran | Makna |
|---|---|
| Parameter | Ukuran kapasitas model dan kebutuhan penyimpanan bobot |
| FLOPs | Estimasi jumlah operasi untuk forward pass |
| Latency | Waktu rata-rata untuk memproses satu citra |
| Throughput | Jumlah citra yang dapat diproses per detik |
| Training time | Waktu pelatihan per run |

### 13.1 Menghitung Parameter dan FLOPs

```python
from fvcore.nn import FlopCountAnalysis

def estimate_flops(model):
    model = copy.deepcopy(model).cpu().eval()
    dummy = torch.randn(1, 3, CONFIG["image_size"], CONFIG["image_size"])
    try:
        return FlopCountAnalysis(model, dummy).total()
    except Exception as exc:
        print("FLOPs tidak dapat dihitung:", exc)
        return np.nan
```

### 13.2 Benchmark Latency dan Throughput

```python
@torch.inference_mode()
def benchmark_model(model, batch_size=1, warmup=10, repeats=30):
    model = model.to(DEVICE).eval()
    dummy = torch.randn(
        batch_size, 3, CONFIG["image_size"], CONFIG["image_size"], device=DEVICE
    )

    for _ in range(warmup):
        _ = model(dummy)

    if DEVICE.type == "cuda":
        torch.cuda.synchronize()

    times = []
    for _ in range(repeats):
        start = time.perf_counter()
        _ = model(dummy)
        if DEVICE.type == "cuda":
            torch.cuda.synchronize()
        times.append(time.perf_counter() - start)

    return np.asarray(times)
```

FLOPs tidak selalu berbanding lurus dengan latency nyata karena latency dipengaruhi oleh implementasi kernel, memory access, parallelism GPU, ukuran batch, dan optimasi backend.

---

## 14. Interpretasi Hasil Praktikum

Berdasarkan hasil eksperimen pada CIFAR-10 dengan 3 seed, `train_size=10000`, `val_size=5000`, `test_size=10000`, `epochs=5`, `batch_size=32`, dan GPU RTX 5090, ringkasan full fine-tuning adalah sebagai berikut.

| Peringkat | Model | Accuracy Mean | Macro-F1 Mean | Waktu Training Mean |
|---:|---|---:|---:|---:|
| 1 | MaxViT-Base | 97.70% | 97.69% | 267.79 s |
| 2 | ConvNeXtV2-Base | 97.64% | 97.65% | 204.03 s |
| 3 | SwinV2-Base | 96.30% | 96.30% | 157.20 s |
| 4 | EVA02-Base/14 | 95.97% | 95.96% | 128.80 s |
| 5 | BEiT-v2-Base | 95.88% | 95.87% | 86.53 s |
| 6 | DeiT-Small/16 | 95.29% | 95.28% | 35.96 s |
| 7 | ConvNeXt-Tiny | 95.26% | 95.29% | 47.73 s |
| 8 | ViT-Base/16 | 94.83% | 94.83% | 74.84 s |

### 14.1 Model Terbaik Secara Akurasi

Model dengan accuracy tertinggi adalah **MaxViT-Base** dengan accuracy sekitar **97.70%**. Namun, selisihnya dengan **ConvNeXtV2-Base** sangat kecil, yaitu sekitar **0.06 percentage point**.

Dengan mempertimbangkan waktu training dan latency, **ConvNeXtV2-Base** dapat dianggap sebagai model yang paling seimbang. Model ini hampir menyamai MaxViT-Base, tetapi lebih efisien.

### 14.2 Model Efisien untuk Praktikum

Model yang menarik untuk praktikum:

| Model | Alasan |
|---|---|
| DeiT-Small/16 | Akurasi tinggi, waktu training relatif cepat |
| ConvNeXt-Tiny | CNN modern yang kuat dan efisien |
| ResNet-50 | Baseline klasik yang stabil |
| MobileNetV3-Large | Cocok untuk diskusi deployment dan edge inference |
| EfficientNet-B0 | Parameter kecil dan konsep efisiensi arsitektur jelas |

### 14.3 Head-Only vs Full Fine-Tuning

Full fine-tuning umumnya meningkatkan performa secara signifikan.

| Model | Head-Only Accuracy | Full Fine-Tuning Accuracy | Kenaikan |
|---|---:|---:|---:|
| BEiT-v2-Base | 47.58% | 95.88% | +48.30 pp |
| MobileNetV3-Large | 53.96% | 90.22% | +36.26 pp |
| EfficientNet-B0 | 57.46% | 91.38% | +33.92 pp |
| ResNet-18 | 66.63% | 88.81% | +22.19 pp |
| ResNet-50 | 78.85% | 93.68% | +14.83 pp |
| DeiT-Tiny/16 | 82.11% | 92.59% | +10.48 pp |
| ConvNeXtV2-Base | 96.63% | 97.64% | +1.02 pp |
| EVA02-Base/14 | 96.89% | 95.97% | -0.92 pp |
| ViT-Large/14 CLIP | 95.32% | 94.12% | -1.19 pp |

Interpretasi:

- Model seperti BEiT-v2, MobileNetV3, EfficientNet-B0, dan ResNet sangat terbantu oleh full fine-tuning.
- Model besar tertentu seperti EVA02 dan ViT-Large CLIP tidak otomatis membaik dengan full fine-tuning.
- Pada model pretrained besar, learning rate dan jumlah data sangat penting. Full fine-tuning dengan data terbatas dapat mengganggu representasi pretrained.

### 14.4 Stabilitas Antar Seed

Model stabil memiliki standar deviasi kecil.

| Model | Accuracy Mean | Accuracy Std |
|---|---:|---:|
| ResNet-50 | 93.68% | 0.080 pp |
| ViT-Base/16 | 94.83% | 0.099 pp |
| ConvNeXtV2-Base | 97.64% | 0.122 pp |
| EfficientNet-B0 | 91.38% | 0.149 pp |
| ResNet-18 | 88.81% | 0.174 pp |

ConvNeXtV2-Base menarik karena akurasinya sangat tinggi sekaligus stabil. Ini membuatnya kuat sebagai kandidat model utama dalam laporan.

### 14.5 Accuracy dan Macro-F1

Nilai accuracy dan macro-F1 hampir sama pada sebagian besar model. Ini menunjukkan bahwa performa relatif seimbang antar kelas. Karena CIFAR-10 memiliki distribusi kelas yang seimbang, pola ini wajar.

Namun, macro-F1 yang mirip dengan accuracy belum cukup untuk menyimpulkan tidak ada masalah per kelas. Mahasiswa tetap perlu memeriksa classification report dan confusion matrix.

---

## 15. Pembahasan Arsitektur

### 15.1 CNN Klasik dan Residual Network

ResNet-18 dan ResNet-50 menjadi baseline penting. ResNet-50 lebih baik daripada ResNet-18 karena memiliki kapasitas lebih besar. Namun, keduanya masih tertinggal dari CNN modern dan hybrid.

Kelebihan ResNet:

- mudah dipahami;
- stabil;
- banyak digunakan sebagai baseline;
- cocok untuk pembanding awal.

Keterbatasan:

- desainnya lebih lama;
- representasi kurang kompetitif dibanding ConvNeXtV2 atau MaxViT;
- tidak memanfaatkan mekanisme attention eksplisit.

### 15.2 CNN Modern

ConvNeXt dan ConvNeXtV2 menunjukkan bahwa CNN masih sangat kuat. ConvNeXtV2-Base bahkan hampir menyamai MaxViT-Base.

Hal penting yang dapat dibahas:

- CNN modern mengadopsi beberapa praktik desain dari Transformer;
- local inductive bias CNN tetap cocok untuk citra;
- tidak semua peningkatan performa harus berasal dari self-attention.

### 15.3 Vision Transformer

ViT dan DeiT menggunakan citra sebagai sekuens patch token. Self-attention memungkinkan model membaca hubungan global antar patch.

Namun, ViT murni biasanya membutuhkan pretraining besar agar kompetitif. Pada data kecil, inductive bias CNN sering lebih menguntungkan.

Pada praktikum ini, DeiT-Small menjadi contoh menarik karena relatif cepat dan akurasinya tinggi.

### 15.4 Hierarchical Transformer

Swin dan SwinV2 menggunakan struktur hierarkis dan window attention. Ini membuatnya lebih dekat dengan cara CNN membangun representasi bertingkat.

SwinV2-Base mencapai performa tinggi, tetapi membutuhkan waktu training dan biaya komputasi lebih besar dibanding model yang lebih kecil.

### 15.5 Hybrid Architecture

MaxViT menggabungkan perhatian lokal dan global. Hasilnya sangat kuat, tetapi biaya inference dan training juga tinggi.

Arsitektur hybrid penting untuk dibahas karena banyak model modern tidak lagi murni CNN atau murni Transformer. Banyak desain baru menggabungkan convolution, attention, hierarchical representation, dan pretraining besar.

---

## 16. Uji Robustness Sederhana

Mahasiswa memilih satu bentuk distribution shift dan menguji model tanpa retraining.

Pilihan shift:

1. Gaussian blur;
2. random occlusion atau cutout;
3. pengurangan brightness;
4. Gaussian noise;
5. rotasi kecil.

Contoh Gaussian blur:

```python
shift_transform = transforms.Compose([
    transforms.Resize((CONFIG["image_size"], CONFIG["image_size"])),
    transforms.GaussianBlur(kernel_size=9, sigma=2.0),
    transforms.ToTensor(),
    transforms.Normalize(IMAGENET_MEAN, IMAGENET_STD),
])
```

Hitung penurunan akurasi:

$$
\Delta Acc = Acc_{clean} - Acc_{shifted}
$$

Pertanyaan analisis:

- Apakah model dengan akurasi tertinggi pada data bersih juga paling robust?
- Apakah model kecil lebih tahan terhadap shift tertentu?
- Apakah Transformer lebih sensitif terhadap blur atau noise?
- Apakah kesalahan meningkat pada kelas tertentu?

---

## 17. Ablation Study

Mahasiswa memilih minimal satu ablation.

| Ablation | Variabel yang diubah | Pertanyaan |
|---|---|---|
| Pretraining | `pretrained=True` vs `False` | Seberapa besar kontribusi pretraining? |
| Fine-tuning | Head-only vs full fine-tuning | Apakah adaptasi backbone diperlukan? |
| Ukuran data | 10%, 25%, 50%, 100% | Bagaimana sample efficiency model? |
| Augmentasi | Sederhana vs lebih kuat | Apakah augmentasi membantu generalisasi? |
| Resolusi | 128 vs 224/256 | Bagaimana trade-off informasi dan biaya? |
| Learning rate | 3e-4, 1e-4, 6e-5 | Apakah setiap model membutuhkan LR berbeda? |

Prinsip penting: ubah satu faktor utama pada satu waktu. Jangan mengubah dataset, resolusi, optimizer, epoch, dan augmentasi sekaligus karena penyebab perubahan hasil menjadi sulit diisolasi.

---

## 18. Template Analisis Hasil

Gunakan template berikut setelah eksperimen selesai.

### A. Menjawab RQ1 - Kinerja

- Model dengan accuracy tertinggi:
- Model dengan macro-F1 tertinggi:
- Selisih absolut dengan model peringkat kedua:
- Apakah selisih konsisten pada semua seed?
- Apakah bukti cukup untuk menyatakan satu arsitektur unggul?

### B. Menjawab RQ2 - Efisiensi

- Model dengan parameter paling sedikit:
- Model dengan FLOPs paling rendah:
- Model dengan latency paling rendah:
- Model dengan throughput tertinggi:
- Apakah FLOPs konsisten dengan latency aktual?
- Model mana yang paling seimbang?

### C. Menjawab RQ3 - Skala Model

- Apakah model terbesar selalu terbaik?
- Apakah model kecil masih kompetitif?
- Apakah peningkatan akurasi sepadan dengan tambahan biaya komputasi?

### D. Menjawab RQ4 - Arsitektur

- Bagaimana performa CNN klasik?
- Bagaimana performa CNN modern?
- Bagaimana performa Vision Transformer?
- Bagaimana performa hierarchical Transformer?
- Bagaimana performa hybrid architecture?

### E. Menjawab RQ5 - Stabilitas

- Model mana yang standar deviasinya paling kecil?
- Model mana yang paling sensitif terhadap seed?
- Apakah kesimpulan berubah jika hanya memakai satu seed?

### F. Menjawab RQ6 - Error Analysis

- Tiga pasangan kelas yang paling sering tertukar:
- Contoh salah klasifikasi yang menarik:
- Dugaan penyebab:
- Eksperimen tambahan untuk menguji dugaan:

---

## 19. Research Log

Isi log setiap menjalankan eksperimen.

| Tanggal/Waktu | Run ID | Model | Seed | Data | Pretraining | Mode | LR | Epoch | Val Acc | Test Acc | Macro-F1 | Latency | Catatan |
|---|---|---|---:|---:|---|---|---:|---:|---:|---:|---:|---:|---|
| | | | | | | | | | | | | | |
| | | | | | | | | | | | | | |

Informasi reproduksibilitas:

- Runtime dan OS:
- CPU/GPU:
- Versi Python:
- Versi PyTorch:
- Versi torchvision:
- Versi timm:
- Mode eksperimen:
- Perubahan dari konfigurasi awal:
- Masalah teknis dan penyelesaiannya:

---

## 20. Ekspor Hasil

Notebook menyimpan hasil dalam beberapa file.

```python
per_run_path = OUTPUT_DIR / "per_run_results.csv"
summary_path = OUTPUT_DIR / "experiment_summary.csv"

per_run_df.to_csv(per_run_path, index=False)
final_table.to_csv(summary_path, index=False)

with open(OUTPUT_DIR / "config.json", "w", encoding="utf-8") as file:
    json.dump(CONFIG, file, indent=2)

environment = {
    "python": platform.python_version(),
    "pytorch": torch.__version__,
    "torchvision": torchvision.__version__,
    "timm": timm.__version__,
    "device": str(DEVICE),
    "gpu": torch.cuda.get_device_name(0) if torch.cuda.is_available() else None,
}

with open(OUTPUT_DIR / "environment.json", "w", encoding="utf-8") as file:
    json.dump(environment, file, indent=2)
```

File yang perlu dilampirkan pada laporan:

| File | Isi |
|---|---|
| `per_run_results.csv` | Hasil setiap model dan setiap seed |
| `experiment_summary.csv` | Ringkasan mean dan standard deviation |
| `config.json` | Konfigurasi eksperimen |
| `environment.json` | Versi software dan hardware |

---

## 21. Pertanyaan Diskusi Kritis

1. Apakah ResNet-18 dan DeiT-Tiny dapat disebut fair comparison hanya karena dataset dan epoch sama?
2. Mengapa pretraining dapat membuat ViT kompetitif pada dataset kecil?
3. Apakah parameter lebih sedikit selalu berarti inference lebih cepat?
4. Mengapa FLOPs tidak selalu berkorelasi sempurna dengan latency?
5. Apakah model yang paling akurat selalu paling robust?
6. Apakah attention map dapat dianggap sebagai penjelasan prediksi?
7. Apakah hasil pada CIFAR-10 dapat digeneralisasi ke citra medis, satelit, atau dokumen?
8. Eksperimen kontrol apa yang perlu ditambahkan sebelum hasil dapat menjadi klaim publikasi?
9. Jika selisih accuracy hanya 0.5%, bukti statistik apa yang diperlukan?
10. Research gap apa yang dapat diturunkan dari pola kesalahan model?

---

## 22. Tugas Laporan Praktikum

Susun laporan ringkas 5-8 halaman dengan struktur:

1. Judul dan identitas;
2. latar belakang dan research question;
3. hipotesis awal sebelum eksperimen;
4. dataset dan protokol eksperimen;
5. hasil utama berupa tabel mean dan standard deviation;
6. kurva training dan validation;
7. confusion matrix dan classification report;
8. analisis perbandingan arsitektur;
9. analisis efisiensi;
10. error analysis;
11. satu robustness test atau ablation study;
12. diskusi keterbatasan;
13. kesimpulan dan peluang research gap.

### Rubrik Penilaian

| Aspek | Bobot |
|---|---:|
| Ketepatan dan reproduksibilitas protokol | 20% |
| Kelengkapan metrik dan visualisasi | 20% |
| Analisis kritis trade-off | 25% |
| Robustness test atau ablation study | 20% |
| Kualitas kesimpulan dan keterkaitan research gap | 15% |

---

## 23. Checklist Penyelesaian

### Setup dan Data

- [ ] Environment berhasil dibuat.
- [ ] GPU atau CPU terdeteksi.
- [ ] Versi Python, PyTorch, torchvision, dan timm dicatat.
- [ ] CIFAR-10 berhasil diunduh.
- [ ] Distribusi kelas diperiksa.
- [ ] Train, validation, dan test tidak tumpang tindih.
- [ ] Transformasi train dan evaluasi dibedakan dengan benar.

### Model dan Training

- [ ] Model berhasil dimuat dari `timm`.
- [ ] Classifier diganti menjadi 10 kelas.
- [ ] Mode head-only atau full fine-tuning dicatat.
- [ ] Minimal tiga seed dijalankan untuk laporan final.
- [ ] Bobot terbaik dipilih berdasarkan validation set.
- [ ] Test set tidak digunakan untuk memilih model.

### Evaluasi

- [ ] Accuracy dilaporkan sebagai mean dan standard deviation.
- [ ] Macro-F1 dilaporkan sebagai mean dan standard deviation.
- [ ] Classification report dianalisis.
- [ ] Confusion matrix dibuat.
- [ ] Parameter, FLOPs, latency, dan throughput dilaporkan.
- [ ] Error analysis dilakukan pada contoh salah klasifikasi.
- [ ] Minimal satu robustness test atau ablation study dilakukan.

### Pelaporan Ilmiah

- [ ] Klaim tidak melebihi bukti eksperimen.
- [ ] Pengaruh arsitektur dibedakan dari pengaruh pretraining.
- [ ] Keterbatasan CIFAR-10 dan resize 32 x 32 ke 256 x 256 dibahas.
- [ ] Hasil dihubungkan dengan domain penelitian masing-masing.
- [ ] Research log dan konfigurasi dilampirkan.

---

## 24. Kesimpulan Praktikum

Praktikum ini menunjukkan bahwa pemilihan arsitektur visual merupakan keputusan empiris. Model terbaik tidak hanya ditentukan oleh akurasi tertinggi, tetapi juga oleh stabilitas, efisiensi, biaya komputasi, kebutuhan deployment, dan kesesuaian dengan domain penelitian.

Temuan penting:

- CNN klasik seperti ResNet tetap penting sebagai baseline.
- CNN modern seperti ConvNeXt dan ConvNeXtV2 sangat kompetitif.
- Vision Transformer membutuhkan pretraining dan konfigurasi yang tepat.
- Hierarchical Transformer seperti SwinV2 menawarkan performa tinggi dengan biaya komputasi lebih besar.
- Hybrid architecture seperti MaxViT dapat mencapai akurasi sangat tinggi, tetapi perlu dianalisis bersama latency dan waktu training.
- Full fine-tuning umumnya meningkatkan performa, tetapi tidak selalu unggul untuk model pretrained besar.

Praktikum ini menjadi jembatan menuju pertemuan berikutnya tentang **Self-Supervised Learning dan Foundation Vision Models**, ketika kualitas representasi visual dianalisis lebih dalam, termasuk dalam skenario label terbatas dan domain yang lebih kompleks.

---

## Referensi Kunci

1. He et al. (2016). *Deep Residual Learning for Image Recognition*.
2. Vaswani et al. (2017). *Attention Is All You Need*.
3. Dosovitskiy et al. (2021). *An Image Is Worth 16 x 16 Words: Transformers for Image Recognition at Scale*.
4. Touvron et al. (2021). *Training Data-Efficient Image Transformers and Distillation through Attention*.
5. Liu et al. (2021). *Swin Transformer: Hierarchical Vision Transformer using Shifted Windows*.
6. Liu et al. (2022). *A ConvNet for the 2020s*.
7. Krizhevsky (2009). *Learning Multiple Layers of Features from Tiny Images*.
8. Dokumentasi resmi PyTorch, torchvision, dan timm.
