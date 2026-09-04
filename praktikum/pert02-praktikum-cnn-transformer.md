# Modul Praktikum 02
## Fondasi CNN dan Transformer untuk Representasi Citra

**Mata kuliah:** EF256129 — Topik Dalam Pengolahan Citra Digital  
**Program:** Doktor/S3 Teknik Informatika  
**Dosen:** Dr. Darlis Herumurti  
**Departemen:** Teknik Informatika — ITS

---

Modul ini menjadi jembatan sebelum Praktikum 03 **Representasi Visual Modern: CNN, Attention, dan Vision Transformer**. Seluruh model pada modul ini dibangun dari komponen dasar, tanpa pretrained model, agar alur data tidak tersembunyi di balik API tingkat tinggi.

**Dataset:** CIFAR-10 — dataset publik populer dengan 60.000 citra RGB berukuran 32×32 dalam 10 kelas.

**Model yang dibangun:**

1. `SimpleCNN` untuk memahami convolution, activation, pooling, feature map, dan receptive field.
2. `TinyImageTransformer` untuk memahami token, embedding, Q/K/V, self-attention, multi-head attention, positional encoding, residual connection, dan Transformer encoder.

> Fokus utama adalah pemahaman. Hasil komparasi model pada modul ini bersifat pedagogis dan bukan klaim bahwa salah satu keluarga arsitektur selalu lebih unggul.

---

## 1. Capaian Pembelajaran

Setelah menyelesaikan praktikum, mahasiswa mampu:

1. menjelaskan citra sebagai tensor `batch × channel × height × width`;
2. menerapkan convolution manual menggunakan beberapa kernel;
3. menjelaskan stride, padding, feature map, pooling, dan receptive field;
4. membangun serta melatih CNN sederhana dari nol;
5. menjelaskan fungsi residual connection;
6. mengubah sekumpulan token menjadi query, key, dan value;
7. menghitung scaled dot-product attention;
8. menjelaskan multi-head attention dan positional encoding;
9. membagi citra menjadi patch token;
10. membangun Transformer encoder kecil untuk klasifikasi citra;
11. membandingkan pola belajar CNN dan Transformer secara kritis;
12. menyiapkan fondasi konseptual untuk ResNet, DeiT, dan Vision Transformer pada Praktikum 03.

### Prasyarat minimum

- Python dasar;
- NumPy dasar;
- konsep matriks dan perkalian matriks;
- konsep training, validation, dan test set;
- pengertian umum supervised learning.

Tidak diperlukan pengalaman sebelumnya menggunakan CNN atau Transformer.

---

## 2. Peta Praktikum

```text
Piksel dan tensor
    ↓
Konvolusi manual dan feature map
    ↓
Simple CNN dan training
    ↓
Residual connection
    ↓
Token, embedding, Q/K/V
    ↓
Self-attention dan multi-head attention
    ↓
Positional encoding
    ↓
Patch tokenization
    ↓
Tiny Image Transformer
    ↓
Komparasi, visualisasi, dan refleksi
```

### Pertanyaan utama

- Bagaimana CNN mengekstraksi pola lokal dari citra?
- Mengapa Transformer memerlukan token dan positional encoding?
- Bagaimana self-attention menghubungkan token yang berjauhan?
- Apa perbedaan alur representasi CNN dan Transformer?
- Mengapa CNN sering lebih mudah dilatih dari nol pada dataset relatif kecil?

---

## 3. Setup Environment

Notebook dapat dijalankan di Google Colab, VS Code dengan ekstensi Jupyter, atau JupyterLab. GPU direkomendasikan untuk bagian training, tetapi mode cepat masih dapat dijalankan pada CPU.

Sel berikut hanya memasang package yang belum tersedia pada kernel aktif. Jika instalasi mengubah versi PyTorch pada environment lokal, restart kernel sebelum melanjutkan.

---

```python
import importlib.util
import subprocess
import sys

requirements = {
    "torch": "torch>=2.2",
    "torchvision": "torchvision>=0.17",
    "sklearn": "scikit-learn>=1.3",
    "pandas": "pandas>=2.0",
    "seaborn": "seaborn>=0.13",
    "matplotlib": "matplotlib>=3.7",
    "tqdm": "tqdm>=4.66",
}

missing = [package for module, package in requirements.items()
           if importlib.util.find_spec(module) is None]

if missing:
    print("Menginstal:", missing)
    subprocess.check_call([sys.executable, "-m", "pip", "install", "-q", *missing])
else:
    print("Semua dependensi utama sudah tersedia.")
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
from pathlib import Path

import matplotlib.pyplot as plt
import matplotlib.patches as patches
import numpy as np
import pandas as pd
import seaborn as sns
import sklearn
import torch
import torch.nn as nn
import torch.nn.functional as F
import torchvision
from IPython.display import display
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix, f1_score
from sklearn.model_selection import train_test_split
from torch.utils.data import DataLoader, Subset
from torchvision import datasets, transforms
from tqdm.auto import tqdm

sns.set_theme(style="whitegrid", context="notebook")

print("Python      :", platform.python_version())
print("PyTorch     :", torch.__version__)
print("Torchvision :", torchvision.__version__)
print("Scikit-learn:", sklearn.__version__)
print("CUDA aktif  :", torch.cuda.is_available())
if torch.cuda.is_available():
    print("GPU         :", torch.cuda.get_device_name(0))
```

---

## 4. Konfigurasi dan Reproduksibilitas

Tersedia tiga mode:

- `QUICK`: memvalidasi seluruh alur dengan subset kecil;
- `FULL`: eksperimen lebih lengkap dengan tiga seed;
- `CUSTOM`: konfigurasi dapat diubah sesuai sumber daya.

Gunakan `QUICK` terlebih dahulu. Setelah seluruh sel berhasil, ubah ke `FULL` untuk laporan final.

---

```python
MODE = "QUICK"  # "QUICK", "FULL", atau "CUSTOM"
DATA_DIR = Path("./data")
OUTPUT_DIR = Path("./outputs_praktikum_02")
OUTPUT_DIR.mkdir(parents=True, exist_ok=True)

if MODE == "QUICK":
    CONFIG = dict(train_size=5_000, val_size=1_000, test_size=1_000,
                  epochs=3, batch_size=64, seeds=[42])
elif MODE == "FULL":
    CONFIG = dict(train_size=45_000, val_size=5_000, test_size=10_000,
                  epochs=12, batch_size=128, seeds=[42, 52, 62])
else:
    CONFIG = dict(train_size=10_000, val_size=2_000, test_size=2_000,
                  epochs=5, batch_size=64, seeds=[42])

CONFIG.update(
    learning_rate=1e-3,
    weight_decay=1e-4,
    num_workers=0 if os.name == "nt" else 2,
    image_size=32,
    num_classes=10,
    dataset="CIFAR-10",
)

DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(json.dumps(CONFIG, indent=2))
print("Device:", DEVICE)
```

---

```python
def set_seed(seed=42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False


set_seed(CONFIG["seeds"][0])
```

---

## 5. Memahami Citra sebagai Tensor

Citra RGB dapat direpresentasikan sebagai tensor tiga dimensi `channel × height × width`. Ketika beberapa citra digabungkan dalam batch, bentuknya menjadi:

```text
batch × channel × height × width
```

Untuk CIFAR-10:

```text
B × 3 × 32 × 32
```

Nilai `3` menyatakan channel merah, hijau, dan biru. PyTorch menggunakan urutan channel lebih dahulu, sedangkan Matplotlib mengharapkan `height × width × channel`.

---

```python
raw_train = datasets.CIFAR10(DATA_DIR, train=True, download=True)
raw_test = datasets.CIFAR10(DATA_DIR, train=False, download=True)

CLASS_NAMES = raw_train.classes
sample_pil, sample_label = raw_train[0]
sample_tensor = transforms.ToTensor()(sample_pil)

print("Kelas             :", CLASS_NAMES)
print("Ukuran PIL        :", sample_pil.size)
print("Bentuk tensor CHW :", tuple(sample_tensor.shape))
print("Tipe data         :", sample_tensor.dtype)
print("Rentang nilai     :", float(sample_tensor.min()), "sampai", float(sample_tensor.max()))
print("Label             :", sample_label, "=", CLASS_NAMES[sample_label])
```

---

```python
fig, axes = plt.subplots(1, 4, figsize=(14, 3.5))
axes[0].imshow(sample_tensor.permute(1, 2, 0))
axes[0].set_title("RGB")
channel_names = ["Red", "Green", "Blue"]
cmaps = ["Reds", "Greens", "Blues"]
for index in range(3):
    axes[index + 1].imshow(sample_tensor[index], cmap=cmaps[index], vmin=0, vmax=1)
    axes[index + 1].set_title(channel_names[index])
for ax in axes:
    ax.axis("off")
plt.suptitle(f"Citra sebagai tiga channel — {CLASS_NAMES[sample_label]}", y=1.03)
plt.tight_layout()
plt.show()
```

---

### Refleksi 1

Sebelum menjalankan sel berikutnya, jawab:

1. Mengapa bentuk tensor PyTorch bukan `32 × 32 × 3`?
2. Apa yang terjadi jika urutan channel salah?
3. Apakah resize dapat menambah detail yang tidak ada pada citra asli?

---

## 6. Konvolusi Manual

Konvolusi menggeser kernel kecil di atas citra. Pada setiap posisi, nilai piksel dan kernel dikalikan lalu dijumlahkan.

Untuk input dua dimensi, operasi sederhananya dapat ditulis:

$$
Y[i,j] = \sum_m\sum_n X[i+m,j+n]W[m,n]
$$

Kernel yang berbeda memberikan respons berbeda. Praktikum ini menggunakan kernel blur, deteksi tepi horizontal, deteksi tepi vertikal, dan sharpening.

---

```python
gray = transforms.functional.rgb_to_grayscale(sample_tensor).unsqueeze(0)  # 1×1×32×32

kernels = torch.tensor([
    [[1, 1, 1], [1, 1, 1], [1, 1, 1]],
    [[-1, -2, -1], [0, 0, 0], [1, 2, 1]],
    [[-1, 0, 1], [-2, 0, 2], [-1, 0, 1]],
    [[0, -1, 0], [-1, 5, -1], [0, -1, 0]],
], dtype=torch.float32).unsqueeze(1)
kernels[0] /= kernels[0].sum()

with torch.no_grad():
    responses = F.conv2d(gray, kernels, padding=1)[0]

names = ["Blur", "Horizontal edge", "Vertical edge", "Sharpen"]
fig, axes = plt.subplots(1, 5, figsize=(17, 3.5))
axes[0].imshow(gray[0, 0], cmap="gray")
axes[0].set_title("Input grayscale")
for ax, response, name in zip(axes[1:], responses, names):
    ax.imshow(response, cmap="gray")
    ax.set_title(name)
for ax in axes:
    ax.axis("off")
plt.tight_layout()
plt.show()
```

---

### Cara membaca hasil

- Kernel blur merata-ratakan piksel sehingga detail berfrekuensi tinggi berkurang.
- Kernel Sobel horizontal merespons perubahan intensitas pada arah vertikal sehingga menonjolkan tepi horizontal.
- Kernel Sobel vertikal menonjolkan tepi vertikal.
- Kernel sharpen memperkuat perbedaan piksel pusat dan tetangganya.

CNN tidak menggunakan kernel tetap seperti contoh tersebut. Nilai kernel menjadi parameter yang dipelajari melalui backpropagation.

---

## 7. Stride, Padding, dan Ukuran Feature Map

Ukuran keluaran convolution satu dimensi adalah:

$$
O=\left\lfloor\frac{I+2P-K}{S}\right\rfloor+1
$$

dengan:

- $I$: ukuran input;
- $K$: ukuran kernel;
- $P$: padding;
- $S$: stride;
- $O$: ukuran output.

---

```python
def conv_output_size(input_size, kernel_size, padding=0, stride=1):
    return math.floor((input_size + 2 * padding - kernel_size) / stride) + 1


examples = []
for kernel, padding, stride in [(3, 0, 1), (3, 1, 1), (3, 1, 2), (5, 2, 1)]:
    examples.append({
        "Input": 32,
        "Kernel": kernel,
        "Padding": padding,
        "Stride": stride,
        "Output": conv_output_size(32, kernel, padding, stride),
    })
display(pd.DataFrame(examples))
```

---

### Latihan singkat

Tanpa menjalankan kode, hitung ukuran output untuk:

1. input 64, kernel 3, padding 1, stride 1;
2. input 64, kernel 3, padding 1, stride 2;
3. input 32, kernel 5, padding 0, stride 1.

Setelah itu, gunakan fungsi `conv_output_size()` untuk memeriksa jawaban.

---

## 8. Menyiapkan CIFAR-10

Training transform menggunakan random crop dan horizontal flip. Validation dan test set tidak memakai transformasi acak. Normalisasi menggunakan statistik CIFAR-10.

Split dilakukan secara berstrata agar proporsi kelas tetap terjaga. Test set tidak dipakai untuk memilih epoch atau hyperparameter.

---

```python
CIFAR_MEAN = (0.4914, 0.4822, 0.4465)
CIFAR_STD = (0.2470, 0.2435, 0.2616)

train_transform = transforms.Compose([
    transforms.RandomCrop(32, padding=4),
    transforms.RandomHorizontalFlip(),
    transforms.ToTensor(),
    transforms.Normalize(CIFAR_MEAN, CIFAR_STD),
])

eval_transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize(CIFAR_MEAN, CIFAR_STD),
])


def stratified_take(indices, targets, n, seed):
    indices = np.asarray(indices)
    if n >= len(indices):
        return indices.tolist()
    selected, _ = train_test_split(
        indices, train_size=n, stratify=np.asarray(targets)[indices], random_state=seed
    )
    return selected.tolist()


all_indices = np.arange(len(raw_train))
train_pool, val_pool = train_test_split(
    all_indices, test_size=5_000, stratify=raw_train.targets, random_state=42
)
train_indices = stratified_take(train_pool, raw_train.targets, CONFIG["train_size"], 42)
val_indices = stratified_take(val_pool, raw_train.targets, CONFIG["val_size"], 42)
test_indices = stratified_take(
    np.arange(len(raw_test)), raw_test.targets, CONFIG["test_size"], 42
)

train_dataset = Subset(
    datasets.CIFAR10(DATA_DIR, train=True, transform=train_transform), train_indices
)
val_dataset = Subset(
    datasets.CIFAR10(DATA_DIR, train=True, transform=eval_transform), val_indices
)
test_dataset = Subset(
    datasets.CIFAR10(DATA_DIR, train=False, transform=eval_transform), test_indices
)

print(f"Train={len(train_dataset):,} | Validation={len(val_dataset):,} | Test={len(test_dataset):,}")
```

---

```python
def make_loaders(seed):
    generator = torch.Generator().manual_seed(seed)
    common = dict(batch_size=CONFIG["batch_size"],
                  num_workers=CONFIG["num_workers"],
                  pin_memory=DEVICE.type == "cuda")
    return (
        DataLoader(train_dataset, shuffle=True, generator=generator, **common),
        DataLoader(val_dataset, shuffle=False, **common),
        DataLoader(test_dataset, shuffle=False, **common),
    )


train_loader, val_loader, test_loader = make_loaders(CONFIG["seeds"][0])
images, labels = next(iter(train_loader))
print("Bentuk satu batch:", tuple(images.shape), tuple(labels.shape))
```

---

```python
def denormalize(image):
    mean = torch.tensor(CIFAR_MEAN).view(3, 1, 1)
    std = torch.tensor(CIFAR_STD).view(3, 1, 1)
    return (image.cpu() * std + mean).clamp(0, 1)


fig, axes = plt.subplots(2, 5, figsize=(12, 5))
for ax, image, label in zip(axes.flat, images[:10], labels[:10]):
    ax.imshow(denormalize(image).permute(1, 2, 0))
    ax.set_title(CLASS_NAMES[label.item()])
    ax.axis("off")
plt.suptitle("Contoh batch setelah augmentasi", y=1.02)
plt.tight_layout()
plt.show()
```

---

## 9. Membangun Simple CNN

Arsitektur berikut memiliki tiga blok convolution. Jumlah channel bertambah dari 3 menjadi 32, 64, lalu 128. Resolusi spasial dikurangi menggunakan max pooling.

```text
3×32×32
→ Conv 32 channel
→ Pool: 32×16×16
→ Conv 64 channel
→ Pool: 64×8×8
→ Conv 128 channel
→ Pool: 128×4×4
→ Global average pooling
→ 128 fitur
→ Linear classifier 10 kelas
```

---

```python
class SimpleCNN(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=3, padding=1),
            nn.BatchNorm2d(32),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2),

            nn.Conv2d(32, 64, kernel_size=3, padding=1),
            nn.BatchNorm2d(64),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2),

            nn.Conv2d(64, 128, kernel_size=3, padding=1),
            nn.BatchNorm2d(128),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2),
        )
        self.pool = nn.AdaptiveAvgPool2d(1)
        self.classifier = nn.Linear(128, num_classes)

    def forward(self, x):
        x = self.features(x)
        x = self.pool(x)
        x = torch.flatten(x, 1)
        return self.classifier(x)


cnn = SimpleCNN(CONFIG["num_classes"])
dummy = torch.randn(2, 3, 32, 32)
print("Input :", tuple(dummy.shape))
print("Output:", tuple(cnn(dummy).shape))
print("Parameter:", f"{sum(p.numel() for p in cnn.parameters()):,}")
```

---

### Memeriksa perubahan bentuk tensor

Forward hook digunakan untuk melihat bentuk output setiap lapisan tanpa mengubah definisi model.

---

```python
hooks = []


def shape_hook(name):
    def hook(_module, _inputs, output):
        print(f"{name:<18} -> {tuple(output.shape)}")
    return hook


for index, layer in enumerate(cnn.features):
    if isinstance(layer, (nn.Conv2d, nn.MaxPool2d)):
        hooks.append(layer.register_forward_hook(shape_hook(f"{index}: {layer.__class__.__name__}")))

_ = cnn(dummy)
for hook in hooks:
    hook.remove()
```

---

## 10. Receptive Field

Receptive field adalah wilayah input yang dapat memengaruhi satu aktivasi. Kernel 3×3 hanya melihat lingkungan lokal pada lapisan pertama. Setelah beberapa convolution dan pooling, receptive field membesar.

Hal ini menjelaskan mengapa CNN membangun hubungan global secara bertahap, tidak langsung pada lapisan pertama.

---

```python
def receptive_field_table(layers):
    receptive_field, jump = 1, 1
    rows = []
    for name, kernel, stride in layers:
        receptive_field = receptive_field + (kernel - 1) * jump
        jump *= stride
        rows.append({"Layer": name, "Kernel": kernel, "Stride": stride,
                     "Receptive field": receptive_field, "Jump": jump})
    return pd.DataFrame(rows)


cnn_layers = [
    ("Conv1", 3, 1), ("Pool1", 2, 2),
    ("Conv2", 3, 1), ("Pool2", 2, 2),
    ("Conv3", 3, 1), ("Pool3", 2, 2),
]
display(receptive_field_table(cnn_layers))
```

---

## 11. Residual Connection sebagai Jembatan ke ResNet

Jaringan yang semakin dalam dapat sulit dioptimasi. Residual block mempelajari fungsi residual dan menambahkan input melalui identity path:

$$
y=F(x)+x
$$

Jika jumlah channel atau ukuran spasial berubah, identity path perlu diproyeksikan agar bentuk tensor cocok.

---

```python
class BasicResidualBlock(nn.Module):
    def __init__(self, in_channels, out_channels, stride=1):
        super().__init__()
        self.conv1 = nn.Conv2d(in_channels, out_channels, 3, stride=stride,
                               padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(out_channels)
        self.conv2 = nn.Conv2d(out_channels, out_channels, 3, padding=1, bias=False)
        self.bn2 = nn.BatchNorm2d(out_channels)
        self.shortcut = nn.Identity()
        if stride != 1 or in_channels != out_channels:
            self.shortcut = nn.Sequential(
                nn.Conv2d(in_channels, out_channels, 1, stride=stride, bias=False),
                nn.BatchNorm2d(out_channels),
            )

    def forward(self, x):
        residual = self.shortcut(x)
        out = F.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        return F.relu(out + residual)


block_same = BasicResidualBlock(32, 32)
block_down = BasicResidualBlock(32, 64, stride=2)
x = torch.randn(2, 32, 16, 16)
print("Identity compatible:", tuple(x.shape), "->", tuple(block_same(x).shape))
print("Dengan projection :", tuple(x.shape), "->", tuple(block_down(x).shape))
```

---

### Refleksi 2

1. Mengapa `out + residual` memerlukan bentuk tensor yang sama?
2. Mengapa digunakan convolution 1×1 pada shortcut ketika channel berubah?
3. Apakah residual connection hanya digunakan pada CNN?

Jawaban pertanyaan ketiga akan terlihat pada Transformer block.

---

## 12. Dari Feature Map ke Token

CNN mempertahankan struktur grid dua dimensi selama ekstraksi fitur. Transformer menerima sekuens token dengan bentuk:

```text
batch × jumlah token × dimensi embedding
```

Contoh berikut membuat empat token berdimensi delapan. Token masih bersifat abstrak; tujuannya memahami bentuk tensor sebelum menggunakan patch citra.

---

```python
set_seed(7)
X = torch.randn(4, 8)  # 4 token, embedding dimension 8
token_table = pd.DataFrame(X.numpy(), index=[f"Token {i+1}" for i in range(4)])
display(token_table.round(3))
print("Bentuk X:", tuple(X.shape))
```

---

## 13. Query, Key, dan Value

Setiap token diproyeksikan menjadi query, key, dan value:

$$Q=XW_Q,\qquad K=XW_K,\qquad V=XW_V$$

- Query menyatakan informasi yang dicari token.
- Key menyatakan karakteristik yang dapat dicocokkan.
- Value membawa informasi yang akan digabungkan.

Matriks bobot pada contoh ini diinisialisasi acak. Karena belum dilatih, pola attention belum memiliki makna semantik.

---

```python
d_model = X.shape[1]
W_Q = torch.randn(d_model, d_model) / math.sqrt(d_model)
W_K = torch.randn(d_model, d_model) / math.sqrt(d_model)
W_V = torch.randn(d_model, d_model) / math.sqrt(d_model)

Q = X @ W_Q
K = X @ W_K
V = X @ W_V

print("Q:", tuple(Q.shape), "K:", tuple(K.shape), "V:", tuple(V.shape))
```

---

## 14. Scaled Dot-Product Attention

Skor kesesuaian dihitung menggunakan perkalian query dan transpose key. Pembagian dengan $\sqrt{d_k}$ menjaga skala nilai agar softmax tidak terlalu mudah jenuh.

$$
A=\operatorname{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)
$$

Output attention adalah:

$$
Z=AV
$$

Setiap baris matriks $A$ berjumlah satu karena menunjukkan distribusi bobot satu query terhadap seluruh key.

---

```python
scores = (Q @ K.T) / math.sqrt(d_model)
attention_weights = torch.softmax(scores, dim=-1)
attention_output = attention_weights @ V

labels_token = [f"T{i+1}" for i in range(4)]
display(pd.DataFrame(attention_weights.numpy(),
                     index=[f"Query {x}" for x in labels_token],
                     columns=[f"Key {x}" for x in labels_token]).round(3))
print("Jumlah setiap baris:", attention_weights.sum(dim=-1))
print("Bentuk output:", tuple(attention_output.shape))
```

---

```python
plt.figure(figsize=(6, 5))
sns.heatmap(attention_weights.numpy(), annot=True, fmt=".2f", cmap="viridis",
            xticklabels=labels_token, yticklabels=labels_token)
plt.xlabel("Key yang diperhatikan")
plt.ylabel("Query")
plt.title("Attention matrix — bobot acak, belum dilatih")
plt.tight_layout()
plt.show()
```

---

### Cara membaca attention matrix

Nilai pada baris `T1` dan kolom `T3` menyatakan seberapa besar Token 1 menggunakan informasi Value dari Token 3. Attention bersifat terarah: bobot T1→T3 tidak harus sama dengan T3→T1.

Karena matriks proyeksi masih acak, visualisasi ini hanya menjelaskan mekanisme matematika, bukan pola yang telah dipelajari.

---

## 15. Multi-Head Attention

Satu attention head menghasilkan satu pola hubungan. Multi-head attention membagi embedding ke beberapa head sehingga model dapat mempelajari beberapa jenis hubungan secara paralel.

Dengan `d_model=8` dan `num_heads=2`, setiap head menggunakan dimensi empat. Nilai `d_model` harus dapat dibagi habis oleh jumlah head.

---

```python
multihead = nn.MultiheadAttention(embed_dim=8, num_heads=2, batch_first=True)
X_batch = X.unsqueeze(0)  # 1×4×8

mha_output, mha_weights = multihead(
    X_batch, X_batch, X_batch,
    need_weights=True,
    average_attn_weights=False,
)

print("Input             :", tuple(X_batch.shape))
print("Output            :", tuple(mha_output.shape))
print("Attention weights :", tuple(mha_weights.shape))
print("Urutan weights    : batch × head × query token × key token")
```

---

```python
fig, axes = plt.subplots(1, 2, figsize=(11, 4))
for head in range(2):
    sns.heatmap(mha_weights[0, head].detach().numpy(), annot=True, fmt=".2f",
                cmap="magma", xticklabels=labels_token, yticklabels=labels_token,
                ax=axes[head], cbar=False)
    axes[head].set_title(f"Head {head + 1}")
    axes[head].set_xlabel("Key")
    axes[head].set_ylabel("Query")
plt.tight_layout()
plt.show()
```

---

## 16. Positional Encoding

Self-attention tidak memiliki konsep urutan atau posisi secara bawaan. Jika token dipermutasi, operasi attention ikut terpermusi tetapi tidak mengetahui bahwa satu token berasal dari kiri atas atau kanan bawah.

Positional encoding menambahkan informasi posisi pada embedding. Contoh berikut menggunakan sinusoidal positional encoding, sedangkan model klasifikasi citra pada bagian berikut menggunakan positional embedding yang dipelajari.

---

```python
def sinusoidal_position_encoding(length, dimension):
    positions = torch.arange(length).unsqueeze(1)
    div_term = torch.exp(
        torch.arange(0, dimension, 2) * (-math.log(10_000.0) / dimension)
    )
    encoding = torch.zeros(length, dimension)
    encoding[:, 0::2] = torch.sin(positions * div_term)
    encoding[:, 1::2] = torch.cos(positions * div_term)
    return encoding


position_encoding = sinusoidal_position_encoding(length=16, dimension=32)
plt.figure(figsize=(10, 4))
sns.heatmap(position_encoding.numpy(), cmap="coolwarm", center=0)
plt.xlabel("Dimensi embedding")
plt.ylabel("Posisi token")
plt.title("Sinusoidal positional encoding")
plt.tight_layout()
plt.show()
```

---

### Refleksi 3

1. Mengapa posisi tidak cukup direpresentasikan oleh isi patch?
2. Apa yang terjadi jika patch kiri dan kanan ditukar tetapi positional embedding tidak digunakan?
3. Apa perbedaan positional encoding sinusoidal dan learnable positional embedding?

---

## 17. Membagi Citra Menjadi Patch

Untuk memasukkan citra ke Transformer, citra dibagi menjadi patch. Pada citra CIFAR-10 32×32 dengan patch 4×4:

$$
N=\frac{32}{4}\times\frac{32}{4}=8\times8=64\text{ patch}
$$

Setiap patch dapat di-flatten kemudian diproyeksikan menjadi embedding. Implementasi praktis dapat menggunakan convolution dengan `kernel_size=patch_size` dan `stride=patch_size`.

---

```python
def visualize_patches(image_tensor, patch_size=4):
    image = image_tensor.permute(1, 2, 0).numpy()
    fig, ax = plt.subplots(figsize=(6, 6))
    ax.imshow(image)
    height, width = image.shape[:2]
    for y in range(0, height, patch_size):
        for x in range(0, width, patch_size):
            ax.add_patch(patches.Rectangle(
                (x - 0.5, y - 0.5), patch_size, patch_size,
                fill=False, edgecolor="yellow", linewidth=0.7
            ))
    ax.set_title(f"Patch {patch_size}×{patch_size}: {(height//patch_size)*(width//patch_size)} token")
    ax.axis("off")
    plt.show()


visualize_patches(sample_tensor, patch_size=4)
```

---

```python
patch_size = 4
embedding_dim = 128
patch_embedding = nn.Conv2d(
    in_channels=3,
    out_channels=embedding_dim,
    kernel_size=patch_size,
    stride=patch_size,
)

patch_grid = patch_embedding(torch.randn(2, 3, 32, 32))
patch_tokens = patch_grid.flatten(2).transpose(1, 2)

print("Setelah Conv2d  :", tuple(patch_grid.shape), "= B × D × grid_h × grid_w")
print("Setelah flatten :", tuple(patch_tokens.shape), "= B × token × D")
```

---

## 18. Membangun Transformer Encoder Block

Satu block terdiri atas:

```text
Input
→ LayerNorm
→ Multi-Head Self-Attention
→ Tambah residual
→ LayerNorm
→ MLP
→ Tambah residual
→ Output
```

Residual connection ternyata tidak hanya digunakan pada CNN. Transformer juga menggunakannya agar optimasi jaringan dalam lebih stabil.

---

```python
class TransformerBlock(nn.Module):
    def __init__(self, dim=128, num_heads=4, mlp_ratio=2.0, dropout=0.1):
        super().__init__()
        self.norm1 = nn.LayerNorm(dim)
        self.attention = nn.MultiheadAttention(
            embed_dim=dim, num_heads=num_heads, dropout=dropout, batch_first=True
        )
        self.norm2 = nn.LayerNorm(dim)
        hidden_dim = int(dim * mlp_ratio)
        self.mlp = nn.Sequential(
            nn.Linear(dim, hidden_dim),
            nn.GELU(),
            nn.Dropout(dropout),
            nn.Linear(hidden_dim, dim),
            nn.Dropout(dropout),
        )

    def forward(self, x, return_attention=False):
        normalized = self.norm1(x)
        attention_output, attention_weights = self.attention(
            normalized, normalized, normalized,
            need_weights=return_attention,
            average_attn_weights=False,
        )
        x = x + attention_output
        x = x + self.mlp(self.norm2(x))
        if return_attention:
            return x, attention_weights
        return x


block = TransformerBlock(dim=128, num_heads=4)
toy_tokens = torch.randn(2, 65, 128)  # 64 patch + 1 CLS token
print("Input block :", tuple(toy_tokens.shape))
print("Output block:", tuple(block(toy_tokens).shape))
```

---

## 19. Membangun Tiny Image Transformer dari Nol

Model ini menggunakan:

- patch embedding 4×4;
- 64 patch token;
- satu token `[CLS]`;
- learnable positional embedding;
- dua Transformer block;
- empat attention head;
- embedding dimension 128;
- classifier linear untuk 10 kelas.

Model disebut `TinyImageTransformer` untuk menekankan bahwa ini adalah implementasi pendidikan. Model ini bukan reproduksi resmi ViT atau DeiT.

---

```python
class TinyImageTransformer(nn.Module):
    def __init__(self, image_size=32, patch_size=4, in_channels=3,
                 num_classes=10, dim=128, depth=2, num_heads=4,
                 mlp_ratio=2.0, dropout=0.1):
        super().__init__()
        assert image_size % patch_size == 0, "Image size harus habis dibagi patch size."
        self.image_size = image_size
        self.patch_size = patch_size
        self.grid_size = image_size // patch_size
        self.num_patches = self.grid_size ** 2

        self.patch_embedding = nn.Conv2d(
            in_channels, dim, kernel_size=patch_size, stride=patch_size
        )
        self.cls_token = nn.Parameter(torch.zeros(1, 1, dim))
        self.positional_embedding = nn.Parameter(
            torch.zeros(1, self.num_patches + 1, dim)
        )
        self.dropout = nn.Dropout(dropout)
        self.blocks = nn.ModuleList([
            TransformerBlock(dim, num_heads, mlp_ratio, dropout)
            for _ in range(depth)
        ])
        self.norm = nn.LayerNorm(dim)
        self.classifier = nn.Linear(dim, num_classes)
        nn.init.trunc_normal_(self.cls_token, std=0.02)
        nn.init.trunc_normal_(self.positional_embedding, std=0.02)

    def forward_features(self, x, return_attention=False):
        x = self.patch_embedding(x).flatten(2).transpose(1, 2)
        cls = self.cls_token.expand(x.size(0), -1, -1)
        x = torch.cat([cls, x], dim=1)
        x = self.dropout(x + self.positional_embedding)

        attention_maps = []
        for block in self.blocks:
            if return_attention:
                x, attention = block(x, return_attention=True)
                attention_maps.append(attention)
            else:
                x = block(x)
        x = self.norm(x)
        if return_attention:
            return x[:, 0], attention_maps
        return x[:, 0]

    def forward(self, x):
        return self.classifier(self.forward_features(x))


transformer = TinyImageTransformer(num_classes=CONFIG["num_classes"])
print("Input :", tuple(dummy.shape))
print("Output:", tuple(transformer(dummy).shape))
print("Patch :", transformer.num_patches)
print("Parameter:", f"{sum(p.numel() for p in transformer.parameters()):,}")
```

---

### Perbandingan alur representasi

| Aspek | SimpleCNN | Tiny Image Transformer |
|---|---|---|
| Unit awal | Piksel lokal | Patch token |
| Operasi utama | Convolution | Self-attention |
| Bias lokal | Kuat | Lebih lemah |
| Hubungan jauh | Bertahap melalui receptive field | Langsung antar-token |
| Posisi | Tersirat pada grid convolution | Ditambahkan melalui positional embedding |
| Normalisasi | BatchNorm | LayerNorm |
| Residual | Ditunjukkan pada residual block | Digunakan pada setiap Transformer block |

---

## 20. Training Loop Bersama

CNN dan Transformer akan dilatih menggunakan data, loss, optimizer, batch size, dan jumlah epoch yang sama. Ini adalah kontrol pedagogis, bukan jaminan bahwa hyperparameter tersebut optimal bagi keduanya.

Checkpoint terbaik dipilih berdasarkan validation accuracy. Test set hanya digunakan setelah training selesai.

---

```python
def run_epoch(model, loader, criterion, optimizer=None):
    training = optimizer is not None
    model.train(training)
    total_loss, total_correct, total_samples = 0.0, 0, 0

    for inputs, targets in tqdm(loader, leave=False):
        inputs = inputs.to(DEVICE, non_blocking=True)
        targets = targets.to(DEVICE, non_blocking=True)
        if training:
            optimizer.zero_grad(set_to_none=True)

        with torch.set_grad_enabled(training):
            logits = model(inputs)
            loss = criterion(logits, targets)
            if training:
                loss.backward()
                optimizer.step()

        total_loss += loss.item() * inputs.size(0)
        total_correct += (logits.argmax(1) == targets).sum().item()
        total_samples += inputs.size(0)

    return total_loss / total_samples, total_correct / total_samples


@torch.inference_mode()
def predict(model, loader):
    model.eval()
    y_true, y_pred, y_prob = [], [], []
    for inputs, targets in tqdm(loader, leave=False):
        probabilities = model(inputs.to(DEVICE)).softmax(dim=1).cpu()
        y_true.extend(targets.numpy())
        y_pred.extend(probabilities.argmax(1).numpy())
        y_prob.extend(probabilities.numpy())
    return np.asarray(y_true), np.asarray(y_pred), np.asarray(y_prob)
```

---

```python
MODEL_BUILDERS = {
    "SimpleCNN": lambda: SimpleCNN(CONFIG["num_classes"]),
    "TinyImageTransformer": lambda: TinyImageTransformer(
        image_size=32, patch_size=4, num_classes=CONFIG["num_classes"],
        dim=128, depth=2, num_heads=4
    ),
}


def train_one_run(model_name, seed):
    set_seed(seed)
    train_loader, val_loader, test_loader = make_loaders(seed)
    model = MODEL_BUILDERS[model_name]().to(DEVICE)
    criterion = nn.CrossEntropyLoss()
    optimizer = torch.optim.AdamW(
        model.parameters(), lr=CONFIG["learning_rate"],
        weight_decay=CONFIG["weight_decay"]
    )

    history = {"train_loss": [], "train_acc": [], "val_loss": [], "val_acc": []}
    best_state, best_val = None, -math.inf
    start = time.perf_counter()

    print(f"\n{model_name} | seed={seed}")
    for epoch in range(CONFIG["epochs"]):
        train_loss, train_acc = run_epoch(model, train_loader, criterion, optimizer)
        val_loss, val_acc = run_epoch(model, val_loader, criterion)
        for key, value in zip(history, [train_loss, train_acc, val_loss, val_acc]):
            history[key].append(value)
        if val_acc > best_val:
            best_val = val_acc
            best_state = copy.deepcopy({k: v.detach().cpu() for k, v in model.state_dict().items()})
        print(f"Epoch {epoch+1:02d}/{CONFIG['epochs']} | "
              f"loss={train_loss:.4f} | train_acc={train_acc:.4f} | val_acc={val_acc:.4f}")

    training_seconds = time.perf_counter() - start
    model.load_state_dict(best_state)
    model.to(DEVICE)
    y_true, y_pred, y_prob = predict(model, test_loader)
    result = {
        "model": model_name,
        "seed": seed,
        "best_val_accuracy": best_val,
        "test_accuracy": accuracy_score(y_true, y_pred),
        "test_macro_f1": f1_score(y_true, y_pred, average="macro"),
        "training_seconds": training_seconds,
        "parameters": sum(p.numel() for p in model.parameters()),
    }
    return model.cpu(), history, result, (y_true, y_pred, y_prob)
```

---

### Jalankan eksperimen

Sel berikut merupakan bagian paling lama. Pada mode `QUICK`, hasil digunakan untuk memahami pipeline. Untuk laporan, jalankan `FULL` dan simpan seluruh run, bukan hanya hasil terbaik.

---

```python
results = []
histories = {}
predictions = {}
best_models = {}
best_validation = {name: -math.inf for name in MODEL_BUILDERS}

for model_name in MODEL_BUILDERS:
    for seed in CONFIG["seeds"]:
        model, history, result, prediction = train_one_run(model_name, seed)
        key = f"{model_name}_seed{seed}"
        results.append(result)
        histories[key] = history
        predictions[key] = prediction
        if result["best_val_accuracy"] > best_validation[model_name]:
            best_validation[model_name] = result["best_val_accuracy"]
            best_models[model_name] = copy.deepcopy(model)
        del model
        if torch.cuda.is_available():
            torch.cuda.empty_cache()

per_run_df = pd.DataFrame(results)
display(per_run_df.round(4))
```

---

## 21. Kurva Pembelajaran

Periksa apakah:

- training loss turun;
- validation accuracy meningkat;
- terdapat gap besar antara training dan validation;
- Transformer memerlukan lebih banyak epoch untuk mencapai pola stabil;
- hasil satu seed berbeda dari seed lain.

---

```python
fig, axes = plt.subplots(1, 2, figsize=(14, 5))
for key, history in histories.items():
    epoch_axis = np.arange(1, len(history["train_loss"]) + 1)
    axes[0].plot(epoch_axis, history["train_loss"], "--", label=f"{key} train")
    axes[0].plot(epoch_axis, history["val_loss"], label=f"{key} val")
    axes[1].plot(epoch_axis, history["train_acc"], "--", label=f"{key} train")
    axes[1].plot(epoch_axis, history["val_acc"], label=f"{key} val")
axes[0].set(title="Loss", xlabel="Epoch", ylabel="Cross-entropy")
axes[1].set(title="Accuracy", xlabel="Epoch", ylabel="Accuracy", ylim=(0, 1))
for ax in axes:
    ax.legend(fontsize=8)
plt.tight_layout()
plt.show()
```

---

## 22. Metrik dan Confusion Matrix

Accuracy menunjukkan proporsi prediksi benar. Macro-F1 menghitung F1 setiap kelas dengan bobot yang sama. Confusion matrix memperlihatkan pasangan kelas yang sering tertukar.

Jika mode cepat hanya menggunakan satu seed, standard deviation akan bernilai `NaN`. Hal tersebut menandakan variasi antar-run belum dapat dihitung.

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
        parameters=("parameters", "first"),
        runs=("seed", "count"),
    )
)
summary_df["parameters_M"] = summary_df["parameters"] / 1e6
display(summary_df.round(4))
```

---

```python
fig, axes = plt.subplots(1, 2, figsize=(17, 6))
for ax, model_name in zip(axes, MODEL_BUILDERS):
    candidates = per_run_df[per_run_df["model"] == model_name]
    best_row = candidates.loc[candidates["best_val_accuracy"].idxmax()]
    key = f"{model_name}_seed{int(best_row['seed'])}"
    y_true, y_pred, _ = predictions[key]
    cm = confusion_matrix(y_true, y_pred, normalize="true")
    sns.heatmap(cm, annot=True, fmt=".2f", cmap="Blues", cbar=False,
                xticklabels=CLASS_NAMES, yticklabels=CLASS_NAMES, ax=ax)
    ax.set(title=model_name, xlabel="Prediksi", ylabel="Label sebenarnya")
plt.tight_layout()
plt.show()
```

---

```python
for model_name in MODEL_BUILDERS:
    candidates = per_run_df[per_run_df["model"] == model_name]
    best_row = candidates.loc[candidates["best_val_accuracy"].idxmax()]
    key = f"{model_name}_seed{int(best_row['seed'])}"
    y_true, y_pred, _ = predictions[key]
    print("\n", "=" * 70, "\n", model_name)
    report = classification_report(
        y_true, y_pred, target_names=CLASS_NAMES, digits=4, output_dict=True
    )
    display(pd.DataFrame(report).T.round(4))
```

---

## 23. Benchmark Latensi Dasar

Pengukuran ini menggunakan input 32×32. Lakukan warm-up dan sinkronisasi CUDA agar timer tidak terlalu optimistis. Hasil hanya berlaku untuk perangkat dan konfigurasi yang dicatat.

---

```python
@torch.inference_mode()
def benchmark(model, batch_size=1):
    model = model.to(DEVICE).eval()
    x = torch.randn(batch_size, 3, 32, 32, device=DEVICE)
    warmup = 10 if DEVICE.type == "cuda" else 3
    repeats = 30 if DEVICE.type == "cuda" else 10
    for _ in range(warmup):
        _ = model(x)
    if DEVICE.type == "cuda":
        torch.cuda.synchronize()
    elapsed = []
    for _ in range(repeats):
        start = time.perf_counter()
        _ = model(x)
        if DEVICE.type == "cuda":
            torch.cuda.synchronize()
        elapsed.append(time.perf_counter() - start)
    return np.asarray(elapsed), model.cpu()


benchmark_rows = []
for model_name, model in best_models.items():
    latency, model = benchmark(model, batch_size=1)
    batch = min(64, CONFIG["batch_size"])
    throughput_time, model = benchmark(model, batch_size=batch)
    best_models[model_name] = model
    benchmark_rows.append({
        "model": model_name,
        "latency_ms": latency.mean() * 1000,
        "latency_std_ms": latency.std() * 1000,
        "throughput_images_s": batch / throughput_time.mean(),
        "device": str(DEVICE),
    })
benchmark_df = pd.DataFrame(benchmark_rows)
display(benchmark_df.round(3))
```

---

## 24. Visualisasi Feature Map CNN

Feature map dari convolution kedua dirata-ratakan pada dimensi channel. Area terang menunjukkan respons rata-rata yang tinggi, tetapi tidak otomatis menjadi penjelasan kausal prediksi.

---

```python
def cnn_feature_map(model, image):
    activation = {}
    target_layer = model.features[4]  # Conv2d kedua

    def hook(_module, _inputs, output):
        activation["map"] = output.detach().cpu()

    handle = target_layer.register_forward_hook(hook)
    model = model.to(DEVICE).eval()
    with torch.inference_mode():
        prediction = model(image.unsqueeze(0).to(DEVICE)).argmax(1).item()
    handle.remove()
    fmap = activation["map"][0].mean(dim=0)
    fmap = F.interpolate(fmap[None, None], size=(32, 32), mode="bilinear",
                         align_corners=False)[0, 0].numpy()
    fmap = (fmap - fmap.min()) / (fmap.max() - fmap.min() + 1e-8)
    return fmap, prediction, model.cpu()


sample_image, sample_target = test_dataset[0]
fmap, cnn_prediction, cnn_model = cnn_feature_map(best_models["SimpleCNN"], sample_image)
best_models["SimpleCNN"] = cnn_model
image_np = denormalize(sample_image).permute(1, 2, 0).numpy()

fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(image_np)
axes[0].set_title(f"True: {CLASS_NAMES[sample_target]}")
axes[1].imshow(fmap, cmap="magma")
axes[1].set_title("Feature map")
axes[2].imshow(image_np)
axes[2].imshow(fmap, cmap="jet", alpha=0.45)
axes[2].set_title(f"Pred: {CLASS_NAMES[cnn_prediction]}")
for ax in axes:
    ax.axis("off")
plt.tight_layout()
plt.show()
```

---

## 25. Visualisasi Attention Tiny Image Transformer

Attention dari token `[CLS]` menuju 64 patch diambil dari blok terakhir, dirata-ratakan pada seluruh head, dibentuk menjadi grid 8×8, lalu diperbesar ke 32×32.

Ingat: **attention bukan otomatis explanation**. Visualisasi ini menunjukkan bobot internal pada satu bagian model.

---

```python
@torch.inference_mode()
def transformer_attention_map(model, image, layer=-1):
    model = model.to(DEVICE).eval()
    features, attention_maps = model.forward_features(
        image.unsqueeze(0).to(DEVICE), return_attention=True
    )
    prediction = model.classifier(features).argmax(1).item()
    attention = attention_maps[layer][0]  # head × query × key
    cls_to_patches = attention[:, 0, 1:].mean(dim=0)
    grid = cls_to_patches.reshape(1, 1, model.grid_size, model.grid_size)
    heatmap = F.interpolate(grid, size=(32, 32), mode="bilinear",
                            align_corners=False)[0, 0].cpu().numpy()
    heatmap = (heatmap - heatmap.min()) / (heatmap.max() - heatmap.min() + 1e-8)
    return heatmap, prediction, model.cpu()


attn_map, transformer_prediction, transformer_model = transformer_attention_map(
    best_models["TinyImageTransformer"], sample_image
)
best_models["TinyImageTransformer"] = transformer_model

fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(image_np)
axes[0].set_title(f"True: {CLASS_NAMES[sample_target]}")
axes[1].imshow(attn_map, cmap="magma")
axes[1].set_title("[CLS] → patch attention")
axes[2].imshow(image_np)
axes[2].imshow(attn_map, cmap="jet", alpha=0.45)
axes[2].set_title(f"Pred: {CLASS_NAMES[transformer_prediction]}")
for ax in axes:
    ax.axis("off")
plt.tight_layout()
plt.show()
```

---

## 26. Error Analysis

Jangan berhenti pada accuracy. Periksa contoh yang salah dan kelompokkan kemungkinan penyebabnya: objek kecil, background dominan, kemiripan kelas, pose, warna, atau resolusi rendah.

---

```python
def show_errors(model_name, maximum=10):
    candidates = per_run_df[per_run_df["model"] == model_name]
    best_row = candidates.loc[candidates["best_val_accuracy"].idxmax()]
    key = f"{model_name}_seed{int(best_row['seed'])}"
    y_true, y_pred, y_prob = predictions[key]
    wrong = np.where(y_true != y_pred)[0][:maximum]
    if len(wrong) == 0:
        print(f"{model_name}: tidak ada salah klasifikasi pada subset test ini.")
        return
    rows = math.ceil(len(wrong) / 5)
    fig, axes = plt.subplots(rows, 5, figsize=(14, 3 * rows))
    axes = np.atleast_1d(axes).flat
    for ax, index in zip(axes, wrong):
        image, _ = test_dataset[index]
        ax.imshow(denormalize(image).permute(1, 2, 0))
        ax.set_title(
            f"T: {CLASS_NAMES[y_true[index]]}\n"
            f"P: {CLASS_NAMES[y_pred[index]]} ({y_prob[index, y_pred[index]]:.2f})",
            fontsize=8,
        )
        ax.axis("off")
    for ax in axes:
        ax.axis("off")
    plt.suptitle(f"Salah klasifikasi — {model_name}", y=1.01)
    plt.tight_layout()
    plt.show()


for model_name in MODEL_BUILDERS:
    show_errors(model_name)
```

---

## 27. Analisis yang Harus Diisi Mahasiswa

### A. CNN

- Apa pola yang direspons kernel manual?
- Bagaimana ukuran feature map berubah?
- Bagaimana receptive field membesar?
- Apa fungsi pooling, BatchNorm, dan global average pooling?
- Apakah kurva CNN menunjukkan overfitting atau underfitting?

### B. Transformer

- Apa arti satu baris pada attention matrix?
- Mengapa attention perlu dibagi dengan $\sqrt{d_k}$?
- Mengapa positional encoding diperlukan?
- Bagaimana citra 32×32 berubah menjadi 64 token?
- Apa fungsi token `[CLS]`?
- Apakah kurva Transformer lebih lambat atau lebih cepat stabil?

### C. Perbandingan

- Model mana yang memperoleh accuracy dan macro-F1 lebih tinggi?
- Apakah jumlah parameternya sebanding?
- Apakah satu konfigurasi optimizer adil untuk keduanya?
- Kelas apa yang sering tertukar?
- Apakah pola feature map dan attention terlihat berbeda?
- Mengapa hasil ini belum membuktikan pemenang universal?

---

## 28. Eksperimen Pengembangan

Pilih minimal dua eksperimen.

1. **Ukuran patch:** bandingkan patch 2, 4, dan 8. Catat jumlah token dan biaya komputasi.
2. **Tanpa positional embedding:** nolkan positional embedding dan ukur perubahan kinerja.
3. **Jumlah head:** bandingkan 2, 4, dan 8 head dengan dimensi embedding tetap.
4. **Kedalaman Transformer:** bandingkan 1, 2, dan 4 block.
5. **Kedalaman CNN:** tambah atau kurangi satu blok convolution.
6. **Tanpa augmentasi:** ukur perubahan train-validation gap.
7. **Ukuran data:** gunakan 10%, 25%, 50%, dan 100% data.
8. **Residual CNN:** ubah CNN sederhana menjadi small ResNet.

Ubah satu faktor utama pada satu waktu. Catat setiap run, termasuk run yang gagal.

---

## 29. Research Log

| Waktu | Run ID | Model | Seed | Data | Konfigurasi utama | LR | Epoch | Val Acc | Test Acc | Macro-F1 | Waktu | Catatan |
|---|---|---|---:|---:|---|---:|---:|---:|---:|---:|---:|---|
| | | | | | | | | | | | | |
| | | | | | | | | | | | | |
| | | | | | | | | | | | | |

Catat juga:

- versi Python, PyTorch, dan torchvision;
- jenis CPU/GPU;
- perubahan dari notebook awal;
- error dan penyelesaiannya;
- alasan memilih konfigurasi berikutnya;
- run yang dikeluarkan beserta alasannya.

---

## 30. Ekspor Hasil

Sel berikut menyimpan hasil per-run, ringkasan, benchmark, konfigurasi, dan environment. Bobot model bersifat opsional.

---

```python
per_run_df.to_csv(OUTPUT_DIR / "per_run_results.csv", index=False)
summary_df.to_csv(OUTPUT_DIR / "summary_results.csv", index=False)
benchmark_df.to_csv(OUTPUT_DIR / "benchmark_results.csv", index=False)

with open(OUTPUT_DIR / "config.json", "w", encoding="utf-8") as file:
    json.dump(CONFIG, file, indent=2)

environment = {
    "python": platform.python_version(),
    "pytorch": torch.__version__,
    "torchvision": torchvision.__version__,
    "device": str(DEVICE),
    "gpu": torch.cuda.get_device_name(0) if torch.cuda.is_available() else None,
}
with open(OUTPUT_DIR / "environment.json", "w", encoding="utf-8") as file:
    json.dump(environment, file, indent=2)

SAVE_WEIGHTS = False
if SAVE_WEIGHTS:
    for name, model in best_models.items():
        torch.save(model.state_dict(), OUTPUT_DIR / f"{name}_best.pt")

print("Output:", OUTPUT_DIR.resolve())
for path in sorted(OUTPUT_DIR.iterdir()):
    print("-", path.name)
```

---

## 31. Tugas Laporan Praktikum

Susun laporan 5–8 halaman:

1. tujuan dan hipotesis;
2. penjelasan convolution dan hasil kernel manual;
3. arsitektur SimpleCNN dan perubahan bentuk tensor;
4. penjelasan Q/K/V dan attention matrix;
5. patch tokenization dan arsitektur Tiny Image Transformer;
6. protokol dataset, split, augmentasi, seed, dan hardware;
7. kurva, accuracy, macro-F1, confusion matrix, waktu, dan parameter;
8. feature map CNN dan attention map Transformer;
9. error analysis;
10. dua eksperimen pengembangan;
11. keterbatasan dan hubungan dengan Praktikum 03.

### Rubrik

| Aspek | Bobot |
|---|---:|
| Pemahaman CNN | 20% |
| Pemahaman attention dan Transformer | 25% |
| Ketepatan implementasi dan protokol | 20% |
| Analisis hasil dan kesalahan | 20% |
| Reproduksibilitas dan kualitas laporan | 15% |

---

## 32. Checklist Penyelesaian

### Fondasi citra dan CNN

- [ ] Dapat menjelaskan bentuk tensor citra dan batch.
- [ ] Berhasil menerapkan empat kernel manual.
- [ ] Dapat menghitung ukuran output convolution.
- [ ] Memahami stride, padding, pooling, dan receptive field.
- [ ] Berhasil membangun dan menjalankan SimpleCNN.
- [ ] Dapat menjelaskan residual connection.

### Fondasi Transformer

- [ ] Dapat menjelaskan token dan embedding.
- [ ] Berhasil menghitung Q, K, dan V.
- [ ] Dapat membaca attention matrix.
- [ ] Memahami scaled dot-product dan multi-head attention.
- [ ] Dapat menjelaskan kebutuhan positional encoding.
- [ ] Berhasil membagi citra menjadi patch token.
- [ ] Berhasil membangun Tiny Image Transformer.

### Eksperimen

- [ ] Split dan transformasi data sudah benar.
- [ ] Kedua model dilatih menggunakan protokol yang terdokumentasi.
- [ ] Mode final menggunakan minimal tiga seed.
- [ ] Accuracy, macro-F1, kurva, dan confusion matrix dianalisis.
- [ ] Parameter, latensi, dan throughput dicatat.
- [ ] Feature map dan attention map diperiksa secara hati-hati.
- [ ] Error analysis dilakukan.
- [ ] Dua eksperimen pengembangan diselesaikan.

### Pelaporan

- [ ] Research log lengkap.
- [ ] Environment dan konfigurasi disimpan.
- [ ] Klaim tidak melebihi bukti.
- [ ] Keterbatasan eksperimen dibahas.
- [ ] Hubungan ke ResNet, DeiT, dan ViT dijelaskan.

---

## 33. Jembatan Menuju Praktikum 03

Setelah modul ini, istilah pada Praktikum 03 seharusnya tidak lagi menjadi kotak hitam:

| Praktikum 02 | Praktikum 03 |
|---|---|
| Convolution manual dan SimpleCNN | ResNet-18 pretrained |
| Residual block sederhana | Residual architecture modern |
| Q/K/V dan attention buatan | Attention pada DeiT |
| Patch embedding 4×4 | Patch embedding 16×16 |
| Tiny Image Transformer dari nol | DeiT-Tiny pretrained |
| Feature map dan attention dasar | Analisis representasi CNN–ViT |
| Training dari nol | Linear probing dan fine-tuning |

Praktikum 02 menjawab **bagaimana komponen bekerja**. Praktikum 03 melanjutkan ke pertanyaan **bagaimana membandingkan arsitektur modern secara ilmiah**.

---

## 34. Kesimpulan

CNN membangun representasi melalui operasi lokal, weight sharing, dan receptive field yang membesar secara bertahap. Transformer mengubah input menjadi token dan menggunakan self-attention untuk menghubungkan seluruh token secara langsung. Positional encoding diperlukan karena attention tidak memahami lokasi secara bawaan.

Pada citra, patch menjadi jembatan antara grid piksel dan sekuens token. Dengan memahami convolution, feature map, Q/K/V, attention matrix, positional encoding, patch embedding, residual connection, serta training dari nol, mahasiswa memiliki fondasi yang cukup untuk mempelajari ResNet, DeiT, dan Vision Transformer pada Praktikum 03.

### Referensi utama

1. LeCun et al. (1998), *Gradient-Based Learning Applied to Document Recognition*.
2. Krizhevsky et al. (2012), *ImageNet Classification with Deep Convolutional Neural Networks*.
3. He et al. (2016), *Deep Residual Learning for Image Recognition*.
4. Vaswani et al. (2017), *Attention Is All You Need*.
5. Dosovitskiy et al. (2021), *An Image Is Worth 16×16 Words*.
6. Dokumentasi resmi PyTorch dan torchvision.
