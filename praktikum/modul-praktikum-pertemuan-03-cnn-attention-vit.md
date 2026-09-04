# Modul Praktikum Pertemuan 03
## Representasi Visual Modern: CNN, Attention, dan Vision Transformer

**Mata kuliah:** EF256129 — Topik Dalam Pengolahan Citra Digital  
**Program:** Doktor/S3 Teknik Informatika  
**Dosen:** Dr. Darlis Herumurti  
**Departemen:** Teknik Informatika — ITS

---

Notebook ini menguji secara empiris perbedaan representasi visual pada **CNN** dan **Vision Transformer (ViT)**. Eksperimen menggunakan **CIFAR-10**, dataset publik populer berisi 60.000 citra berwarna dalam 10 kelas. Citra diubah ke resolusi 224×224 agar kompatibel dengan bobot pretrained ImageNet.

Model utama:

- **ResNet-18** — CNN modern dengan residual connection dan inductive bias lokal.
- **DeiT-Tiny/16** — Vision Transformer yang memproses citra sebagai patch token dan menggunakan self-attention global.

> **Estimasi mode cepat:** sekitar 10–25 menit pada GPU Colab, bergantung pada perangkat dan koneksi. Mode lengkap menjalankan tiga seed dan memerlukan waktu jauh lebih lama.

---

## 1. Capaian Pembelajaran

Setelah menyelesaikan praktikum, mahasiswa mampu:

1. menjelaskan perbedaan inductive bias CNN dan Vision Transformer;
2. melakukan transfer learning/fine-tuning dengan `timm` dan PyTorch;
3. merancang perbandingan arsitektur dengan split, transformasi, dan protokol yang terkontrol;
4. membandingkan accuracy, macro-F1, jumlah parameter, FLOPs, latensi, dan throughput;
5. membaca kurva pembelajaran, confusion matrix, feature map CNN, dan attention map ViT;
6. melakukan error analysis dan menyusun klaim yang proporsional terhadap bukti;
7. menghubungkan temuan praktikum dengan research gap dan domain penelitian masing-masing.

### Pertanyaan penelitian praktikum

- **RQ1:** Dengan data target terbatas dan pretraining yang sama-sama berbasis ImageNet, model mana yang memberikan kinerja klasifikasi lebih baik?
- **RQ2:** Bagaimana trade-off akurasi, macro-F1, ukuran model, FLOPs, latensi, dan throughput?
- **RQ3:** Apakah CNN dan ViT memusatkan representasi pada wilayah citra yang sama?
- **RQ4:** Kelas dan contoh seperti apa yang masih sulit bagi masing-masing arsitektur?

### Hipotesis awal

Tuliskan prediksi sebelum menjalankan eksperimen.

| Hipotesis | Prediksi dan alasan |
|---|---|
| H1 — Kinerja | _Isi sebelum eksperimen_ |
| H2 — Efisiensi | _Isi sebelum eksperimen_ |
| H3 — Representasi | _Isi sebelum eksperimen_ |

---

## 2. Desain Eksperimen dan Prinsip Fair Comparison

Variabel yang dikontrol:

| Komponen | Ketentuan |
|---|---|
| Dataset dan split | Indeks train/validation/test sama untuk kedua model |
| Resolusi input | 224×224 |
| Augmentasi | Identik untuk ResNet-18 dan DeiT-Tiny |
| Pretraining | Keduanya memakai supervised ImageNet-1k |
| Loss dan optimizer | Cross-entropy dan AdamW |
| Batch size dan epoch | Sama dalam satu mode eksperimen |
| Model selection | Bobot dengan validation accuracy terbaik |
| Evaluasi akhir | Test set hanya digunakan setelah pemilihan model |

**Catatan metodologis:** hyperparameter yang identik membuat perbandingan mudah dikontrol, tetapi belum tentu optimal untuk setiap arsitektur. Untuk publikasi, tambahkan tuning dengan budget pencarian yang setara bagi setiap model dan jalankan minimal tiga seed.

---

## 3. Setup Environment

Notebook dapat dijalankan di:

- Google Colab: pilih **Runtime → Change runtime type → GPU**;
- VS Code/Jupyter: gunakan Python 3.10+ dan environment dengan PyTorch;
- CPU: dapat berjalan, tetapi training dan benchmark akan lebih lambat.

Jalankan sel instalasi satu kali. Setelah instalasi di Colab, restart runtime hanya jika diminta.

---

```python
%pip install -q "timm>=1.0.20,<1.1" "torchinfo>=1.8" "fvcore>=0.1.5" "scikit-learn>=1.3" "seaborn>=0.13" "pandas>=2.0" "matplotlib>=3.7"
```

---

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
from sklearn.metrics import (
    accuracy_score,
    classification_report,
    confusion_matrix,
    f1_score,
)
from sklearn.model_selection import train_test_split
from torch.utils.data import DataLoader, Subset
from torchvision import datasets, transforms
from tqdm.auto import tqdm

warnings.filterwarnings("ignore", category=UserWarning)
sns.set_theme(style="whitegrid", context="notebook")

print("Python     :", platform.python_version())
print("PyTorch    :", torch.__version__)
print("Torchvision:", torchvision.__version__)
print("timm       :", timm.__version__)
print("scikit-learn:", sklearn.__version__)
print("CUDA aktif :", torch.cuda.is_available())
if torch.cuda.is_available():
    print("GPU        :", torch.cuda.get_device_name(0))
```

---

## 4. Konfigurasi Eksperimen

Pilih salah satu mode:

- `QUICK`: subset kecil, 1 seed, dan 3 epoch—untuk demonstrasi kelas.
- `FULL`: data lebih besar, 3 seed, dan 10 epoch—untuk laporan praktikum.
- `CUSTOM`: ubah nilai konfigurasi sesuai sumber daya.

`HEAD_ONLY=True` membekukan backbone dan melatih classifier. Ini membuat eksperimen cepat serta menguji kualitas representasi pretrained melalui linear probing sederhana. Ubah menjadi `False` untuk fine-tuning seluruh jaringan; gunakan learning rate lebih kecil.

---

```python
MODE = "QUICK"                 # "QUICK", "FULL", atau "CUSTOM"
HEAD_ONLY = True               # False = fine-tuning seluruh parameter
DATA_DIR = Path("./data")
OUTPUT_DIR = Path("./outputs_pertemuan_03")
OUTPUT_DIR.mkdir(parents=True, exist_ok=True)

if MODE == "QUICK":
    CONFIG = dict(
        train_size=5_000,
        val_size=1_000,
        test_size=1_000,
        epochs=3,
        batch_size=32,
        seeds=[42],
    )
elif MODE == "FULL":
    CONFIG = dict(
        train_size=40_000,
        val_size=10_000,
        test_size=10_000,
        epochs=10,
        batch_size=64,
        seeds=[42, 52, 62],
    )
else:
    CONFIG = dict(
        train_size=10_000,
        val_size=2_000,
        test_size=2_000,
        epochs=5,
        batch_size=32,
        seeds=[42],
    )

CONFIG.update(
    image_size=224,
    num_workers=2,
    learning_rate=3e-4 if HEAD_ONLY else 3e-5,
    weight_decay=1e-4,
    head_only=HEAD_ONLY,
    dataset="CIFAR-10",
    models=["resnet18", "deit_tiny_patch16_224"],
)

DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")
if os.name == "nt":
    CONFIG["num_workers"] = 0  # lebih stabil pada Windows/Jupyter

print(json.dumps(CONFIG, indent=2))
print("Device:", DEVICE)
```

---

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

## 5. Dataset: CIFAR-10

CIFAR-10 memiliki 50.000 citra train dan 10.000 citra test berukuran 32×32 piksel dengan kelas:

`airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck`.

Alasan pemilihan:

- publik, populer, dan mudah direproduksi;
- cukup ringan untuk Colab;
- memiliki pasangan kelas yang secara visual mirip, sehingga cocok untuk error analysis;
- ukuran kecil membuat pengaruh transfer learning dan inductive bias dapat didiskusikan.

Keterbatasan utama: upsampling 32×32 ke 224×224 **tidak menambah detail visual baru**. Karena itu, hasil tidak boleh digeneralisasi langsung ke citra resolusi tinggi atau domain medis/satelit.

**Alternatif pengembangan:** ganti CIFAR-10 dengan Oxford-IIIT Pet, Food-101, EuroSAT, Beans dari Hugging Face, atau dataset publik yang dekat dengan domain disertasi. Jika dataset diganti, dokumentasikan lisensi, proses sampling, distribusi kelas, potensi kebocoran data, dan alasan pemilihannya.

---

```python
# Dataset tanpa transformasi dipakai untuk membaca label dan EDA.
raw_train = datasets.CIFAR10(DATA_DIR, train=True, download=True)
raw_test = datasets.CIFAR10(DATA_DIR, train=False, download=True)

CLASS_NAMES = raw_train.classes
NUM_CLASSES = len(CLASS_NAMES)
print("Kelas:", CLASS_NAMES)
print("Train asli:", len(raw_train), "| Test asli:", len(raw_test))
```

---

```python
fig, axes = plt.subplots(2, 5, figsize=(14, 6))
seen = set()
for image, label in raw_train:
    if label not in seen:
        ax = axes.flat[label]
        ax.imshow(image)
        ax.set_title(CLASS_NAMES[label])
        ax.axis("off")
        seen.add(label)
    if len(seen) == NUM_CLASSES:
        break
plt.suptitle("Satu contoh dari setiap kelas CIFAR-10", y=1.02, fontsize=15)
plt.tight_layout()
plt.show()

label_counts = pd.Series(raw_train.targets).value_counts().sort_index()
display(pd.DataFrame({"class": CLASS_NAMES, "count": label_counts.values}))
```

---

### Transformasi

Normalisasi memakai statistik ImageNet karena kedua model menggunakan bobot pretrained ImageNet-1k. Augmentasi hanya diterapkan pada train set. Validation dan test set bersifat deterministik.

---

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

---

```python
def stratified_take(indices, labels, n, seed):
    # Ambil subset berstrata tanpa mengubah proporsi kelas.
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
test_idx = stratified_take(
    np.arange(len(raw_test)), raw_test.targets, CONFIG["test_size"], 42
)

train_base = datasets.CIFAR10(DATA_DIR, train=True, transform=train_transform)
val_base = datasets.CIFAR10(DATA_DIR, train=True, transform=eval_transform)
test_base = datasets.CIFAR10(DATA_DIR, train=False, transform=eval_transform)

train_ds = Subset(train_base, train_idx)
val_ds = Subset(val_base, val_idx)
test_ds = Subset(test_base, test_idx)

pin_memory = DEVICE.type == "cuda"
train_loader = DataLoader(
    train_ds, batch_size=CONFIG["batch_size"], shuffle=True,
    num_workers=CONFIG["num_workers"], pin_memory=pin_memory,
)
val_loader = DataLoader(
    val_ds, batch_size=CONFIG["batch_size"], shuffle=False,
    num_workers=CONFIG["num_workers"], pin_memory=pin_memory,
)
test_loader = DataLoader(
    test_ds, batch_size=CONFIG["batch_size"], shuffle=False,
    num_workers=CONFIG["num_workers"], pin_memory=pin_memory,
)

print(f"Train={len(train_ds):,} | Validation={len(val_ds):,} | Test={len(test_ds):,}")
```

---

```python
def denormalize(x):
    mean = torch.tensor(IMAGENET_MEAN).view(3, 1, 1)
    std = torch.tensor(IMAGENET_STD).view(3, 1, 1)
    return (x.cpu() * std + mean).clamp(0, 1)


images, labels = next(iter(train_loader))
fig, axes = plt.subplots(2, 5, figsize=(14, 6))
for ax, image, label in zip(axes.flat, images[:10], labels[:10]):
    ax.imshow(denormalize(image).permute(1, 2, 0))
    ax.set_title(CLASS_NAMES[label.item()])
    ax.axis("off")
plt.suptitle("Contoh setelah resize dan augmentasi", y=1.02, fontsize=15)
plt.tight_layout()
plt.show()
```

---

## 6. Membangun ResNet-18 dan DeiT-Tiny

Kedua model diambil melalui API `timm` agar pemanggilan dan penggantian classifier konsisten.

### Mengapa model ini?

| Model | Representasi | Inductive bias utama |
|---|---|---|
| ResNet-18 | Feature map bertingkat | Lokalitas, weight sharing, translasi ekuivariansi |
| DeiT-Tiny/16 | Sekuens patch token | Hubungan global melalui self-attention; bias lokal lebih lemah |

DeiT-Tiny dipilih agar ukuran eksperimen masih realistis untuk kelas. Ini bukan klaim bahwa DeiT-Tiny adalah pasangan yang sepenuhnya ekuivalen dengan ResNet-18 dalam seluruh aspek.

---

```python
MODEL_LABELS = {
    "resnet18": "ResNet-18",
    "deit_tiny_patch16_224": "DeiT-Tiny/16",
}


def build_model(model_name, num_classes=10, head_only=True):
    model = timm.create_model(model_name, pretrained=True, num_classes=num_classes)
    if head_only:
        for parameter in model.parameters():
            parameter.requires_grad = False
        classifier = model.get_classifier()
        for parameter in classifier.parameters():
            parameter.requires_grad = True
    return model


preview_models = {
    name: build_model(name, NUM_CLASSES, CONFIG["head_only"])
    for name in CONFIG["models"]
}

rows = []
for name, model in preview_models.items():
    rows.append({
        "model": MODEL_LABELS[name],
        "total_parameters": sum(p.numel() for p in model.parameters()),
        "trainable_parameters": sum(p.numel() for p in model.parameters() if p.requires_grad),
    })
parameter_table = pd.DataFrame(rows)
parameter_table["total_M"] = parameter_table["total_parameters"] / 1e6
parameter_table["trainable_M"] = parameter_table["trainable_parameters"] / 1e6
display(parameter_table[["model", "total_M", "trainable_M"]].round(3))

del preview_models
if torch.cuda.is_available():
    torch.cuda.empty_cache()
```

---

## 7. Training dan Validation

Pipeline berikut:

1. membangun ulang model pada setiap seed;
2. melatih dengan loss dan optimizer yang sama;
3. menyimpan bobot dengan validation accuracy terbaik;
4. mengevaluasi bobot terbaik pada test set;
5. mencatat waktu training dan seluruh prediksi untuk analisis berikutnya.

> Test set tidak dipakai untuk memilih epoch atau hyperparameter.

---

```python
def run_epoch(model, loader, criterion, optimizer=None):
    is_training = optimizer is not None
    model.train(is_training)
    if is_training and CONFIG["head_only"]:
        # Linear probing: backbone (termasuk statistik BatchNorm) tetap beku.
        model.eval()
        model.get_classifier().train()
    running_loss, correct, total = 0.0, 0, 0

    for inputs, targets in tqdm(loader, leave=False):
        inputs = inputs.to(DEVICE, non_blocking=True)
        targets = targets.to(DEVICE, non_blocking=True)

        if is_training:
            optimizer.zero_grad(set_to_none=True)

        with torch.set_grad_enabled(is_training):
            outputs = model(inputs)
            loss = criterion(outputs, targets)
            if is_training:
                loss.backward()
                optimizer.step()

        running_loss += loss.item() * inputs.size(0)
        correct += (outputs.argmax(1) == targets).sum().item()
        total += inputs.size(0)

    return running_loss / total, correct / total


@torch.inference_mode()
def predict(model, loader):
    model.eval()
    y_true, y_pred, y_prob = [], [], []
    for inputs, targets in tqdm(loader, leave=False):
        logits = model(inputs.to(DEVICE, non_blocking=True))
        probabilities = logits.softmax(dim=1).cpu()
        y_true.extend(targets.numpy().tolist())
        y_pred.extend(probabilities.argmax(1).numpy().tolist())
        y_prob.extend(probabilities.numpy().tolist())
    return np.asarray(y_true), np.asarray(y_pred), np.asarray(y_prob)


def train_one_run(model_name, seed):
    set_seed(seed)
    model = build_model(model_name, NUM_CLASSES, CONFIG["head_only"]).to(DEVICE)
    criterion = nn.CrossEntropyLoss()
    optimizer = torch.optim.AdamW(
        [p for p in model.parameters() if p.requires_grad],
        lr=CONFIG["learning_rate"],
        weight_decay=CONFIG["weight_decay"],
    )

    history = {"train_loss": [], "train_acc": [], "val_loss": [], "val_acc": []}
    best_val_acc = -math.inf
    best_state = None
    start = time.perf_counter()

    print(f"\n{MODEL_LABELS[model_name]} | seed={seed}")
    for epoch in range(CONFIG["epochs"]):
        train_loss, train_acc = run_epoch(model, train_loader, criterion, optimizer)
        val_loss, val_acc = run_epoch(model, val_loader, criterion)
        history["train_loss"].append(train_loss)
        history["train_acc"].append(train_acc)
        history["val_loss"].append(val_loss)
        history["val_acc"].append(val_acc)

        if val_acc > best_val_acc:
            best_val_acc = val_acc
            best_state = copy.deepcopy({k: v.detach().cpu() for k, v in model.state_dict().items()})

        print(
            f"Epoch {epoch+1:02d}/{CONFIG['epochs']} | "
            f"loss={train_loss:.4f} | train_acc={train_acc:.4f} | "
            f"val_acc={val_acc:.4f}"
        )

    training_seconds = time.perf_counter() - start
    model.load_state_dict(best_state)
    model.to(DEVICE)
    y_true, y_pred, y_prob = predict(model, test_loader)

    metrics = {
        "model_key": model_name,
        "model": MODEL_LABELS[model_name],
        "seed": seed,
        "best_val_accuracy": best_val_acc,
        "test_accuracy": accuracy_score(y_true, y_pred),
        "test_macro_f1": f1_score(y_true, y_pred, average="macro"),
        "training_seconds": training_seconds,
    }
    return model, history, metrics, (y_true, y_pred, y_prob)
```

---

```python
run_records = []
histories = {}
predictions = {}
best_models = {}
best_scores = {name: -math.inf for name in CONFIG["models"]}

for model_name in CONFIG["models"]:
    for seed in CONFIG["seeds"]:
        model, history, metrics, pred = train_one_run(model_name, seed)
        key = f"{model_name}_seed{seed}"
        histories[key] = history
        predictions[key] = pred
        run_records.append(metrics)

        if metrics["best_val_accuracy"] > best_scores[model_name]:
            best_scores[model_name] = metrics["best_val_accuracy"]
            best_models[model_name] = copy.deepcopy(model).cpu()

        del model
        if torch.cuda.is_available():
            torch.cuda.empty_cache()

per_run_df = pd.DataFrame(run_records)
display(per_run_df.round(4))
```

---

## 8. Kurva Pembelajaran

Interpretasikan pola berikut:

- **train naik, validation stagnan/turun:** indikasi overfitting;
- **train dan validation sama-sama rendah:** indikasi underfitting atau training belum cukup;
- **gap kecil dan stabil:** generalisasi relatif baik pada split tersebut;
- **kurva satu run saja:** belum menunjukkan variabilitas antar-seed.

---

```python
fig, axes = plt.subplots(1, 2, figsize=(14, 5))
for key, history in histories.items():
    label = key.replace("_patch16_224", "").replace("_", " ")
    epochs = np.arange(1, len(history["train_loss"]) + 1)
    axes[0].plot(epochs, history["train_loss"], "--", alpha=0.75, label=f"{label} train")
    axes[0].plot(epochs, history["val_loss"], alpha=0.9, label=f"{label} val")
    axes[1].plot(epochs, history["train_acc"], "--", alpha=0.75, label=f"{label} train")
    axes[1].plot(epochs, history["val_acc"], alpha=0.9, label=f"{label} val")

axes[0].set(title="Loss", xlabel="Epoch", ylabel="Cross-entropy")
axes[1].set(title="Accuracy", xlabel="Epoch", ylabel="Accuracy", ylim=(0, 1))
for ax in axes:
    ax.legend(fontsize=8)
plt.tight_layout()
plt.show()
```

---

## 9. Evaluasi Klasifikasi dan Ketidakpastian Antar-Seed

Accuracy mudah dibaca, sedangkan macro-F1 memberi bobot setara pada setiap kelas. CIFAR-10 seimbang, tetapi macro-F1 tetap berguna untuk melihat apakah performa konsisten lintas kelas.

Jika hanya satu seed dijalankan, standar deviasi tidak dapat dihitung dan akan ditampilkan sebagai `NaN`. Jangan menafsirkan satu run sebagai bukti statistik yang kuat.

---

```python
summary_df = (
    per_run_df.groupby("model", as_index=False)
    .agg(
        accuracy_mean=("test_accuracy", "mean"),
        accuracy_std=("test_accuracy", "std"),
        macro_f1_mean=("test_macro_f1", "mean"),
        macro_f1_std=("test_macro_f1", "std"),
        training_seconds_mean=("training_seconds", "mean"),
        runs=("seed", "count"),
    )
)
display(summary_df.round(4))

ax = summary_df.plot(
    x="model", y=["accuracy_mean", "macro_f1_mean"],
    kind="bar", figsize=(9, 5), ylim=(0, 1), rot=0,
    color=["#1976D2", "#FF9800"]
)
ax.set_ylabel("Skor")
ax.set_title("Kinerja test set")
ax.legend(["Accuracy", "Macro-F1"])
plt.tight_layout()
plt.show()
```

---

```python
# Classification report dari run dengan validation accuracy terbaik per arsitektur.
for model_name in CONFIG["models"]:
    candidates = per_run_df[per_run_df["model_key"] == model_name]
    best_row = candidates.loc[candidates["best_val_accuracy"].idxmax()]
    key = f"{model_name}_seed{int(best_row['seed'])}"
    y_true, y_pred, _ = predictions[key]
    print("\n", "=" * 70)
    print(MODEL_LABELS[model_name])
    report = classification_report(
        y_true, y_pred, target_names=CLASS_NAMES, digits=4, output_dict=True
    )
    display(pd.DataFrame(report).T.round(4))
```

---

## 10. Confusion Matrix

Confusion matrix membantu menjawab pertanyaan yang tidak terlihat dari accuracy agregat: pasangan kelas mana yang tertukar dan apakah pola kesalahan kedua arsitektur berbeda?

---

```python
fig, axes = plt.subplots(1, 2, figsize=(18, 7))
for ax, model_name in zip(axes, CONFIG["models"]):
    candidates = per_run_df[per_run_df["model_key"] == model_name]
    best_row = candidates.loc[candidates["best_val_accuracy"].idxmax()]
    key = f"{model_name}_seed{int(best_row['seed'])}"
    y_true, y_pred, _ = predictions[key]
    cm = confusion_matrix(y_true, y_pred, normalize="true")
    sns.heatmap(
        cm, annot=True, fmt=".2f", cmap="Blues", cbar=False,
        xticklabels=CLASS_NAMES, yticklabels=CLASS_NAMES, ax=ax,
    )
    ax.set_title(MODEL_LABELS[model_name])
    ax.set_xlabel("Prediksi")
    ax.set_ylabel("Label sebenarnya")
plt.tight_layout()
plt.show()
```

---

## 11. Efisiensi: Parameter, FLOPs, Latensi, dan Throughput

- **Parameter** menunjukkan kapasitas penyimpanan model, bukan langsung biaya inferensi.
- **FLOPs** adalah estimasi jumlah operasi untuk satu forward pass.
- **Latensi** adalah waktu rata-rata per citra pada batch 1.
- **Throughput** adalah jumlah citra per detik pada batch yang lebih besar.

FLOPs, latensi, dan throughput dipengaruhi resolusi, batch size, perangkat, backend, warm-up, dan versi library. Karena itu, laporkan lingkungan eksekusi bersama hasil.

---

```python
from fvcore.nn import FlopCountAnalysis


def estimate_flops(model):
    # Salinan mencegah perubahan mode attention memengaruhi benchmark latensi.
    model = copy.deepcopy(model).cpu().eval()
    for module in model.modules():
        if hasattr(module, "fused_attn"):
            module.fused_attn = False
    dummy = torch.randn(1, 3, CONFIG["image_size"], CONFIG["image_size"])
    try:
        return FlopCountAnalysis(model, dummy).total()
    except Exception as exc:
        print("FLOPs tidak dapat dihitung:", exc)
        return np.nan


@torch.inference_mode()
def benchmark_model(model, batch_size=1, warmup=10, repeats=30):
    model = model.to(DEVICE).eval()
    if DEVICE.type == "cpu":
        warmup, repeats = min(warmup, 3), min(repeats, 10)
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


efficiency_rows = []
for model_name, model in best_models.items():
    total_params = sum(p.numel() for p in model.parameters())
    flops = estimate_flops(model)
    latency = benchmark_model(model, batch_size=1)
    throughput_batch = min(32, CONFIG["batch_size"])
    throughput_time = benchmark_model(model, batch_size=throughput_batch)
    efficiency_rows.append({
        "model": MODEL_LABELS[model_name],
        "parameters_M": total_params / 1e6,
        "GFLOPs": flops / 1e9,
        "latency_ms_per_image": latency.mean() * 1000,
        "latency_std_ms": latency.std() * 1000,
        "throughput_images_per_second": throughput_batch / throughput_time.mean(),
        "device": str(DEVICE),
    })
    best_models[model_name] = model.cpu()
    if torch.cuda.is_available():
        torch.cuda.empty_cache()

efficiency_df = pd.DataFrame(efficiency_rows)
display(efficiency_df.round(3))
```

---

```python
final_table = summary_df.merge(efficiency_df, on="model", how="left")
display(final_table.round(4))

fig, axes = plt.subplots(1, 3, figsize=(16, 4.5))
sns.barplot(data=final_table, x="model", y="accuracy_mean", ax=axes[0], color="#1976D2")
sns.barplot(data=final_table, x="model", y="parameters_M", ax=axes[1], color="#FF9800")
sns.barplot(data=final_table, x="model", y="latency_ms_per_image", ax=axes[2], color="#009688")
axes[0].set(title="Accuracy", xlabel="", ylabel="Skor", ylim=(0, 1))
axes[1].set(title="Jumlah parameter", xlabel="", ylabel="Juta parameter")
axes[2].set(title=f"Latensi ({DEVICE})", xlabel="", ylabel="ms/citra")
for ax in axes:
    ax.tick_params(axis="x", rotation=10)
plt.tight_layout()
plt.show()
```

---

## 12. Visualisasi Representasi CNN: Feature Map

Sel ini mengambil aktivasi dari blok awal ResNet-18, merata-ratakan channel, lalu menampilkannya sebagai heatmap. Visualisasi ini menunjukkan **respons fitur**, bukan penjelasan kausal final atas prediksi.

---

```python
def show_cnn_feature_map(model, image_tensor, class_name):
    model = model.to(DEVICE).eval()
    activation = {}

    def hook_fn(_module, _input, output):
        activation["value"] = output.detach()

    handle = model.layer1[-1].register_forward_hook(hook_fn)
    with torch.inference_mode():
        _ = model(image_tensor.unsqueeze(0).to(DEVICE))
    handle.remove()

    fmap = activation["value"][0].mean(dim=0, keepdim=True).unsqueeze(0)
    fmap = torch.nn.functional.interpolate(
        fmap, size=(CONFIG["image_size"], CONFIG["image_size"]),
        mode="bilinear", align_corners=False,
    )[0, 0].cpu().numpy()
    fmap = (fmap - fmap.min()) / (fmap.max() - fmap.min() + 1e-8)

    image_np = denormalize(image_tensor).permute(1, 2, 0).numpy()
    fig, axes = plt.subplots(1, 3, figsize=(13, 4))
    axes[0].imshow(image_np)
    axes[0].set_title(f"Input: {class_name}")
    axes[1].imshow(fmap, cmap="magma")
    axes[1].set_title("Rata-rata feature map")
    axes[2].imshow(image_np)
    axes[2].imshow(fmap, cmap="jet", alpha=0.45)
    axes[2].set_title("Overlay")
    for ax in axes:
        ax.axis("off")
    plt.tight_layout()
    plt.show()
    return model.cpu()


sample_image, sample_label = test_ds[0]
best_models["resnet18"] = show_cnn_feature_map(
    best_models["resnet18"], sample_image, CLASS_NAMES[sample_label]
)
```

---

## 13. Visualisasi Attention ViT

DeiT membagi citra 224×224 menjadi patch 16×16, sehingga terbentuk grid 14×14 atau 196 patch. Sel berikut menghitung attention dari token `[CLS]` menuju patch pada blok Transformer pertama, lalu merata-ratakan seluruh head.

**Peringatan interpretasi:** attention map menunjukkan distribusi bobot attention pada satu lapisan. `Attention ≠ explanation`; bobot tinggi tidak otomatis membuktikan bahwa patch tersebut menyebabkan keputusan model.

---

```python
@torch.inference_mode()
def extract_cls_attention(model, image_tensor, block_index=0):
    model = model.to(DEVICE).eval()
    x = image_tensor.unsqueeze(0).to(DEVICE)
    x = model.patch_embed(x)
    x = model._pos_embed(x)
    x = model.patch_drop(x)
    x = model.norm_pre(x)

    block = model.blocks[block_index]
    x_norm = block.norm1(x)
    attn_module = block.attn
    batch, tokens, channels = x_norm.shape
    qkv = (
        attn_module.qkv(x_norm)
        .reshape(batch, tokens, 3, attn_module.num_heads, channels // attn_module.num_heads)
        .permute(2, 0, 3, 1, 4)
    )
    q, k, _v = qkv.unbind(0)
    if hasattr(attn_module, "q_norm"):
        q = attn_module.q_norm(q)
        k = attn_module.k_norm(k)
    attention = (q * attn_module.scale) @ k.transpose(-2, -1)
    attention = attention.softmax(dim=-1)

    num_prefix = getattr(model, "num_prefix_tokens", 1)
    cls_to_patches = attention[0, :, 0, num_prefix:].mean(dim=0)
    grid_h, grid_w = model.patch_embed.grid_size
    heatmap = cls_to_patches.reshape(1, 1, grid_h, grid_w)
    heatmap = torch.nn.functional.interpolate(
        heatmap, size=(CONFIG["image_size"], CONFIG["image_size"]),
        mode="bilinear", align_corners=False,
    )[0, 0].cpu().numpy()
    heatmap = (heatmap - heatmap.min()) / (heatmap.max() - heatmap.min() + 1e-8)
    return heatmap, model.cpu()


attention_map, vit_model = extract_cls_attention(
    best_models["deit_tiny_patch16_224"], sample_image, block_index=0
)
best_models["deit_tiny_patch16_224"] = vit_model
image_np = denormalize(sample_image).permute(1, 2, 0).numpy()

fig, axes = plt.subplots(1, 3, figsize=(13, 4))
axes[0].imshow(image_np)
axes[0].set_title(f"Input: {CLASS_NAMES[sample_label]}")
axes[1].imshow(attention_map, cmap="magma")
axes[1].set_title("[CLS] → patch attention")
axes[2].imshow(image_np)
axes[2].imshow(attention_map, cmap="jet", alpha=0.45)
axes[2].set_title("Overlay")
for ax in axes:
    ax.axis("off")
plt.tight_layout()
plt.show()
```

---

### Eksperimen attention lanjutan

Ulangi visualisasi dengan `block_index` berbeda, misalnya blok awal, tengah, dan akhir. Bandingkan:

- apakah attention menjadi lebih terfokus pada lapisan yang lebih dalam;
- apakah background masih mendapat bobot besar;
- apakah pola berubah pada prediksi benar dan salah;
- apakah satu attention head menunjukkan pola berbeda dari head lain.

---

## 14. Error Analysis: Contoh Salah Klasifikasi

Contoh salah klasifikasi membantu memeriksa apakah kegagalan terkait kemiripan bentuk, background, resolusi rendah, pose, atau kemungkinan ambiguitas label.

---

```python
def show_misclassified(model_name, max_images=10):
    candidates = per_run_df[per_run_df["model_key"] == model_name]
    best_row = candidates.loc[candidates["best_val_accuracy"].idxmax()]
    key = f"{model_name}_seed{int(best_row['seed'])}"
    y_true, y_pred, y_prob = predictions[key]
    wrong = np.where(y_true != y_pred)[0][:max_images]

    if len(wrong) == 0:
        print("Tidak ada salah klasifikasi pada subset ini.")
        return

    cols = 5
    rows = math.ceil(len(wrong) / cols)
    fig, axes = plt.subplots(rows, cols, figsize=(15, 3.2 * rows))
    axes = np.atleast_1d(axes).flat
    for ax, idx in zip(axes, wrong):
        image, _ = test_ds[idx]
        confidence = y_prob[idx, y_pred[idx]]
        ax.imshow(denormalize(image).permute(1, 2, 0))
        ax.set_title(
            f"True: {CLASS_NAMES[y_true[idx]]}\n"
            f"Pred: {CLASS_NAMES[y_pred[idx]]} ({confidence:.2f})",
            fontsize=9,
        )
        ax.axis("off")
    for ax in axes:
        ax.axis("off")
    plt.suptitle(f"Salah klasifikasi — {MODEL_LABELS[model_name]}", y=1.01)
    plt.tight_layout()
    plt.show()


for name in CONFIG["models"]:
    show_misclassified(name)
```

---

## 15. Analisis Hasil

Isi setelah seluruh eksperimen selesai.

### A. Menjawab RQ1 — Kinerja

- Model dengan accuracy tertinggi: **...**
- Model dengan macro-F1 tertinggi: **...**
- Selisih absolut: **...**
- Apakah selisih konsisten pada seluruh seed? **...**
- Apakah bukti cukup untuk mengatakan satu arsitektur unggul? Jelaskan: **...**

### B. Menjawab RQ2 — Efisiensi

- Model dengan parameter lebih sedikit: **...**
- Model dengan FLOPs lebih rendah: **...**
- Model dengan latensi lebih rendah: **...**
- Apakah urutan FLOPs konsisten dengan latensi aktual? Mengapa mungkin berbeda? **...**

### C. Menjawab RQ3 — Representasi

- Pola feature map CNN: **...**
- Pola attention ViT: **...**
- Persamaan dan perbedaannya: **...**
- Keterbatasan metode visualisasi: **...**

### D. Menjawab RQ4 — Pola Kesalahan

- Tiga pasangan kelas yang paling sering tertukar: **...**
- Contoh kegagalan yang menarik: **...**
- Dugaan penyebab: **...**
- Eksperimen tambahan untuk menguji dugaan tersebut: **...**

---

## 16. Uji Robustness Sederhana — Tugas Pengembangan

Pilih **satu** distribution shift yang relevan dan evaluasi kedua model tanpa retraining:

1. Gaussian blur;
2. random occlusion/cutout;
3. pengurangan brightness;
4. Gaussian noise;
5. rotasi kecil.

Laporkan penurunan kinerja absolut:

$$\Delta Acc = Acc_{clean} - Acc_{shifted}$$

Pertanyaan: apakah model yang paling akurat pada data bersih juga paling robust? Jangan mengasumsikan hasil sebelum pengujian.

---

```python
# TODO mahasiswa: aktifkan satu transformasi shift dan evaluasi ulang.
# Contoh kerangka Gaussian blur:
shift_transform = transforms.Compose([
    transforms.Resize((CONFIG["image_size"], CONFIG["image_size"])),
    transforms.GaussianBlur(kernel_size=9, sigma=2.0),
    transforms.ToTensor(),
    transforms.Normalize(IMAGENET_MEAN, IMAGENET_STD),
])

# shifted_base = datasets.CIFAR10(DATA_DIR, train=False, transform=shift_transform)
# shifted_ds = Subset(shifted_base, test_idx)
# shifted_loader = DataLoader(shifted_ds, batch_size=CONFIG["batch_size"], shuffle=False)
# for model_name, model in best_models.items():
#     y_true_s, y_pred_s, _ = predict(model.to(DEVICE), shifted_loader)
#     shifted_acc = accuracy_score(y_true_s, y_pred_s)
#     print(model_name, shifted_acc)
```

---

## 17. Ablation Study — Pilih Minimal Satu

| Ablation | Variabel yang diubah | Pertanyaan |
|---|---|---|
| Pretraining | `pretrained=True` vs `False` | Seberapa besar kontribusi pretraining? |
| Fine-tuning | head-only vs seluruh layer | Apakah adaptasi backbone diperlukan? |
| Ukuran data | 10%, 25%, 50%, 100% | Bagaimana sample efficiency kedua arsitektur? |
| Augmentasi | sederhana vs lebih kuat | Apakah ViT lebih terbantu oleh regularisasi? |
| Resolusi | 128 vs 224 | Bagaimana trade-off informasi dan biaya? |

**Prinsip:** ubah satu faktor utama pada satu waktu. Jangan mengubah pretraining, augmentasi, resolusi, dan optimizer sekaligus karena penyebab perubahan hasil menjadi tidak dapat diisolasi.

---

## 18. Research Log

Isi log setiap menjalankan eksperimen. Jangan hanya mencatat run terbaik.

| Tanggal/Waktu | Run ID | Model | Seed | Data | Pretraining | Trainable layer | LR | Epoch | Val Acc | Test Acc/F1 | Latensi | Catatan/Anomali |
|---|---|---|---:|---:|---|---|---:|---:|---:|---|---:|---|
| | | | | | | | | | | | | |
| | | | | | | | | | | | | |
| | | | | | | | | | | | | |

### Informasi reproduksibilitas

- Runtime dan OS: **...**
- CPU/GPU: **...**
- Versi Python/PyTorch/timm: **...**
- Mode eksperimen: **...**
- Perubahan dari konfigurasi awal: **...**
- Masalah teknis dan penyelesaiannya: **...**

---

## 19. Ekspor Hasil

Sel ini menyimpan tabel hasil, konfigurasi, dan bobot terbaik. File bobot dapat cukup besar; simpan hanya jika diperlukan untuk reproduksi atau analisis lanjutan.

---

```python
per_run_df.to_csv(OUTPUT_DIR / "per_run_results.csv", index=False)
final_table.to_csv(OUTPUT_DIR / "experiment_summary.csv", index=False)

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

# Aktifkan jika bobot terbaik perlu disimpan.
SAVE_WEIGHTS = False
if SAVE_WEIGHTS:
    for model_name, model in best_models.items():
        torch.save(model.state_dict(), OUTPUT_DIR / f"{model_name}_best.pt")

print("Hasil tersimpan di:", OUTPUT_DIR.resolve())
for path in sorted(OUTPUT_DIR.iterdir()):
    print("-", path.name)
```

---

## 20. Pertanyaan Diskusi Kritis

1. Apakah ResNet-18 dan DeiT-Tiny dapat disebut *fair comparison* hanya karena dataset dan epoch sama?
2. Bagaimana ukuran pretraining dataset memengaruhi interpretasi keunggulan arsitektur?
3. Mengapa ViT dari awal pada dataset kecil berisiko kalah, tetapi ViT pretrained dapat kompetitif?
4. Apakah parameter yang lebih sedikit selalu berarti inferensi lebih cepat?
5. Mengapa FLOPs tidak selalu berkorelasi sempurna dengan latensi nyata?
6. Apakah attention map dapat dianggap sebagai penjelasan prediksi? Apa keterbatasannya?
7. Apakah temuan pada CIFAR-10 berlaku untuk citra medis, satelit, dokumen, atau domain disertasi Anda?
8. Eksperimen kontrol apa yang perlu ditambahkan sebelum hasil dapat menjadi klaim publikasi?
9. Jika selisih accuracy hanya 0,5%, bukti statistik apa yang diperlukan?
10. Research gap apa yang dapat diturunkan dari pola kegagalan, bukan hanya dari selisih skor?

---

## 21. Tugas Laporan Praktikum

Susun laporan ringkas 5–8 halaman dengan struktur:

1. **Judul dan identitas**
2. **Latar belakang dan research question**
3. **Hipotesis** yang ditulis sebelum eksperimen
4. **Dataset dan protokol** termasuk split, transformasi, seed, hardware, serta versi library
5. **Hasil utama** berupa tabel mean±std, kurva, confusion matrix, dan efisiensi
6. **Analisis representasi** feature map CNN dan attention ViT
7. **Error analysis** minimal lima contoh atau pola kegagalan
8. **Satu robustness test atau ablation study**
9. **Diskusi kritis** mengenai arsitektur vs pretraining, validitas, dan generalisasi
10. **Kesimpulan dan peluang research gap**

### Rubrik Penilaian

| Aspek | Bobot |
|---|---:|
| Ketepatan dan reproduksibilitas protokol | 20% |
| Kelengkapan metrik dan visualisasi | 20% |
| Analisis kritis trade-off | 25% |
| Robustness test/ablation dan error analysis | 20% |
| Kualitas kesimpulan dan keterkaitan research gap | 15% |

---

## 22. Checklist Penyelesaian

### Setup dan data

- [ ] Environment berhasil dibuat dan GPU/CPU terdeteksi.
- [ ] Seed, versi library, serta hardware dicatat.
- [ ] CIFAR-10 berhasil diunduh dan distribusi kelas diperiksa.
- [ ] Train, validation, dan test tidak tumpang tindih.
- [ ] Transform train dan evaluasi dibedakan dengan benar.

### Model dan eksperimen

- [ ] ResNet-18 dan DeiT-Tiny berhasil dimuat dengan bobot pretrained.
- [ ] Classifier diganti menjadi 10 kelas.
- [ ] Protokol perbandingan dikontrol dan perubahan konfigurasi dicatat.
- [ ] Minimal tiga seed dijalankan untuk laporan final.
- [ ] Checkpoint dipilih berdasarkan validation set, bukan test set.

### Evaluasi

- [ ] Accuracy dan macro-F1 dilaporkan sebagai mean±std.
- [ ] Confusion matrix dan kurva pembelajaran dianalisis.
- [ ] Parameter, FLOPs, latensi, throughput, dan perangkat dilaporkan.
- [ ] Feature map CNN dan attention map ViT diperiksa secara kritis.
- [ ] Error analysis dilakukan pada contoh nyata.
- [ ] Minimal satu robustness test atau ablation dijalankan.

### Pelaporan ilmiah

- [ ] Klaim tidak melebihi bukti eksperimen.
- [ ] Pengaruh arsitektur dibedakan dari pengaruh pretraining.
- [ ] Keterbatasan CIFAR-10 dan upsampling 32→224 dibahas.
- [ ] Hasil dihubungkan dengan domain penelitian dan research gap.
- [ ] Research log serta file konfigurasi dilampirkan.

---

## 23. Kesimpulan Praktikum

Praktikum ini tidak dirancang untuk mencari “pemenang mutlak” antara CNN dan ViT. Tujuan utamanya adalah memperlihatkan bahwa pemilihan representasi visual merupakan keputusan empiris yang dipengaruhi oleh:

- inductive bias;
- ukuran dan karakteristik data;
- skala serta jenis pretraining;
- strategi fine-tuning;
- kebutuhan akurasi, robustness, interpretabilitas, dan efisiensi;
- perangkat tempat model akan digunakan.

Temuan dari notebook ini menjadi fondasi untuk Pertemuan 4: **Self-Supervised Learning dan Foundation Vision Models**, ketika kualitas representasi akan dianalisis melampaui supervised pretraining.

### Referensi kunci

1. He et al. (2016), *Deep Residual Learning for Image Recognition*.
2. Vaswani et al. (2017), *Attention Is All You Need*.
3. Dosovitskiy et al. (2021), *An Image Is Worth 16×16 Words*.
4. Touvron et al. (2021), *Training Data-Efficient Image Transformers & Distillation through Attention*.
5. Krizhevsky (2009), *Learning Multiple Layers of Features from Tiny Images*.
6. Dokumentasi resmi PyTorch, torchvision, dan timm.
