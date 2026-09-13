# Modul Praktikum Pertemuan 05
## Vision-Language Models dan Multimodal Representation

**Mata kuliah:** EF256129 — Topik Dalam Pengolahan Citra Digital  
**Program:** Doktor/S3 Teknik Informatika  
**Dosen:** Dr. Darlis Herumurti  
**Departemen:** Teknik Informatika — ITS  
**Bahasa pemrograman:** Python 3.12  
**Platform:** Jupyter Notebook/JupyterLab atau Google Colab  
**Model utama:** `openai/clip-vit-base-patch32`  
**Dataset utama:** Oxford-IIIT Pet dan Flickr8k  

---

## Ringkasan Praktikum

Praktikum ini mempelajari bagaimana model **vision-language** menyelaraskan citra dan bahasa ke dalam ruang embedding bersama. Mahasiswa menggunakan CLIP sebagai model pretrained untuk menjalankan dua tugas utama:

1. **zero-shot image classification** pada Oxford-IIIT Pet;
2. **image-text retrieval** dua arah pada Flickr8k.

Eksperimen tidak berhenti pada menjalankan inference. Mahasiswa harus menguji pengaruh kanonikalisasi label, prompt template, prompt ensemble, mengukur top-1/top-5 dan macro-F1, menghitung Recall@K, median rank, dan mean reciprocal rank, memeriksa failure cases, serta membatasi klaim sesuai bukti.

Alur utama:

```text
Praktikum 04: visual embedding DINOv2
                     ↓
       Praktikum 05: shared image-text space
                     ↓
          ┌──────────┴──────────┐
          ↓                     ↓
 Oxford-IIIT Pet             Flickr8k
 zero-shot class.      image-text retrieval
          ↓                     ↓
 prompt sensitivity      Recall@K / MedR / MRR
          └──────────┬──────────┘
                     ↓
       error, bias, semantic-control analysis
                     ↓
       research gap dan rekomendasi domain
                     ↓
Praktikum 06: restoration & computational imaging
```

> Fokus praktikum bukan hanya “mencoba CLIP”, melainkan merancang dan mengevaluasi protokol multimodal yang valid, reproducible, dan relevan bagi penelitian doktoral.

---

## 1. Posisi dan Keterkaitan dengan Praktikum Sebelum dan Sesudahnya

### 1.1 Posisi dalam rangkaian Praktikum 01–06

| Praktikum | Fokus | Pertanyaan utama | Kontribusi terhadap Praktikum 05 |
|---|---|---|---|
| 01 | Baseline klasik | Seberapa jauh histogram/raw pixel mendukung klasifikasi? | Membentuk kebiasaan baseline, evaluasi, failure analysis, dan research log |
| 02 | Fondasi CNN dan Transformer | Bagaimana convolution, token, Q/K/V, dan attention membentuk fitur? | Menjelaskan komponen image encoder dan text Transformer |
| 03 | CNN vs ViT | Bagaimana pretrained ResNet dan DeiT dibandingkan? | Menyiapkan konsep fair comparison, frozen backbone, dan robustness |
| 04 | SSL dan foundation vision model | Seberapa transferable frozen visual embedding DINOv2? | Menyiapkan embedding, cosine similarity, retrieval visual, dan linear probing |
| **05** | **Vision-language model** | **Bagaimana bahasa menyelaraskan, memperluas, dan sekaligus membatasi representasi visual?** | **Zero-shot classification, prompt sensitivity, dan cross-modal retrieval** |
| 06 | Image restoration dan computational imaging | Bagaimana memulihkan citra dari degradasi dan menilai kualitasnya? | Memakai pemahaman semantik/perseptual untuk mengkritisi evaluasi hasil restorasi |

### 1.2 Hubungan khusus dengan Praktikum 04

Praktikum 04 menggunakan DINOv2 untuk memetakan:

```text
citra → visual embedding
```

Praktikum 05 memperluasnya menjadi:

```text
citra → image embedding ─┐
                         ├→ ruang embedding bersama → similarity
teks  → text embedding ──┘
```

| Aspek | Praktikum 04: DINOv2 | Praktikum 05: CLIP |
|---|---|---|
| Modalitas | Citra | Citra dan teks |
| Sinyal pretraining | Self-supervised visual | Pasangan citra–bahasa alami |
| Downstream utama | Linear probe, visual retrieval | Zero-shot classification, cross-modal retrieval |
| Bobot kelas | Dipelajari dari label oleh probe | Dibentuk dari embedding prompt teks |
| Kelas baru | Perlu melatih probe baru | Dapat ditambahkan melalui teks |
| Fokus kritis | Transferability visual | Alignment, prompt sensitivity, linguistic gap, bias |

Perbandingan skor DINOv2 linear probe dan CLIP zero-shot harus dilakukan hati-hati karena **jumlah supervisi downstream berbeda**. DINOv2 linear probe memakai contoh berlabel, sedangkan CLIP zero-shot hanya memakai nama/deskripsi kelas.

### 1.3 Jembatan menuju Praktikum 06

Praktikum 06 membahas inverse problem, denoising, deblurring, dan super-resolution. Keterkaitannya:

- embedding multimodal dapat membantu memeriksa apakah makna citra bertahan setelah restorasi;
- kesamaan CLIP dapat dibandingkan dengan PSNR/SSIM sebagai sinyal perseptual tambahan;
- prompt dapat digunakan untuk menguji apakah hasil restorasi mempertahankan atribut penting;
- CLIP score bukan pengganti fidelity metric karena dapat mengabaikan detail piksel atau hallucination.

Pertanyaan jembatan:

> Apakah citra hasil restorasi yang secara semantik cocok dengan teks pasti setia terhadap citra asli?

Jawabannya belum tentu. Praktikum 06 akan membedakan **semantic plausibility**, **perceptual quality**, dan **pixel fidelity**.

---

## 2. Capaian Pembelajaran Praktikum

Setelah menyelesaikan praktikum, mahasiswa mampu:

1. menjelaskan konsep multimodal representation dan image-text alignment;
2. menjelaskan arsitektur dual encoder CLIP;
3. menghitung cosine similarity dan memahami fungsi temperature;
4. menjelaskan perbedaan zero-shot classification dan classifier terlatih;
5. melakukan kanonikalisasi label menjadi bahasa alami;
6. membangun dan membandingkan beberapa prompt set;
7. menerapkan prompt ensemble secara benar;
8. menjalankan zero-shot classification tanpa melatih ulang CLIP;
9. mengukur top-1, top-5, balanced accuracy, dan macro-F1;
10. menghitung bootstrap confidence interval;
11. menjalankan image-to-text dan text-to-image retrieval;
12. menghitung Recall@K, median rank, mean rank, dan MRR;
13. menangani banyak caption benar untuk satu citra;
14. menganalisis kesalahan visual, semantik, linguistik, dan prompt-induced;
15. menguji semantic controls atau counterfactual captions;
16. membedakan kemampuan alignment dari reasoning;
17. mengaudit bias dan ancaman validitas;
18. merumuskan research gap multimodal yang dapat diuji.

---

## 3. Pertanyaan Penelitian dan Hipotesis

### 3.1 Research questions

- **RQ1 — Zero-shot capability:** Seberapa baik CLIP mengklasifikasikan 37 breed Oxford-IIIT Pet tanpa contoh berlabel downstream?
- **RQ2 — Prompt sensitivity:** Seberapa besar perubahan bentuk prompt memengaruhi top-1 dan macro-F1?
- **RQ3 — Hierarchical context:** Apakah penambahan superclass `cat`/`dog` membantu klasifikasi fine-grained?
- **RQ4 — Prompt ensemble:** Apakah perataan embedding beberapa prompt lebih stabil daripada satu prompt?
- **RQ5 — Cross-modal retrieval:** Seberapa baik CLIP menemukan caption atau citra pasangannya pada Flickr8k?
- **RQ6 — Semantic consistency:** Apakah CLIP membedakan kalimat yang semantik benar dari hard negative yang memiliki kata serupa?
- **RQ7 — Domain validity:** Keterbatasan apa yang muncul ketika protokol dipindahkan ke domain penelitian mahasiswa?

### 3.2 Hipotesis sebelum eksperimen

Isi sebelum menjalankan evaluasi:

| ID | Hipotesis | Dasar teori | Hasil mendukung/menolak |
|---|---|---|---|
| H1 | Prompt kalimat lebih baik daripada label mentah | ... | Diisi setelah eksperimen |
| H2 | Konteks superclass membantu breed ambigu | ... | Diisi setelah eksperimen |
| H3 | Prompt ensemble mengurangi sensitivitas template | ... | Diisi setelah eksperimen |
| H4 | Retrieval lebih sulit ketika kandidat bertambah | ... | Diisi setelah eksperimen |

> Hipotesis tidak diubah setelah hasil dilihat. Hasil yang menolak hipotesis tetap bernilai ilmiah.

---

## 4. Konsep Inti

### 4.1 Dual encoder dan ruang bersama

CLIP memiliki image encoder $f_I$ dan text encoder $f_T$:

$$
\mathbf{i}_n = \frac{f_I(x_n)}{\lVert f_I(x_n)\rVert_2}, \qquad
\mathbf{t}_m = \frac{f_T(s_m)}{\lVert f_T(s_m)\rVert_2}
$$

Setelah normalisasi L2, similarity adalah dot product:

$$
S_{nm}=\mathbf{i}_n^\top\mathbf{t}_m
$$

### 4.2 Contrastive image-text learning

Untuk batch berisi $N$ pasangan citra–teks, dibentuk matriks similarity $N\times N$. Pasangan positif berada pada diagonal. Loss dihitung dua arah:

$$
\mathcal{L}=\frac{1}{2}\left(\mathcal{L}_{image\rightarrow text}+\mathcal{L}_{text\rightarrow image}\right)
$$

Model belajar menaikkan skor pasangan cocok dan menurunkan skor pasangan tidak cocok dalam batch.

### 4.3 Zero-shot classification

Setiap kelas $c_k$ diubah menjadi prompt $p(c_k)$ dan di-encode menjadi prototype teks $\mathbf{w}_k$. Prediksi citra $x$:

$$
\hat{y}=\arg\max_k\left(\mathbf{i}(x)^\top\mathbf{w}_k\right)
$$

Classifier tidak dilatih pada Oxford-IIIT Pet. “Bobot kelas” berasal dari teks.

### 4.4 Prompt ensemble

Untuk $M$ template:

$$
\bar{\mathbf{w}}_k = \operatorname{norm}\left(
\frac{1}{M}\sum_{m=1}^{M}\operatorname{norm}(f_T(p_m(c_k)))
\right)
$$

Normalisasi dilakukan sebelum dan sesudah rata-rata agar prototype tidak didominasi magnitudo tertentu.

### 4.5 Retrieval

- **Image-to-text (I2T):** satu citra menjadi query terhadap seluruh caption.
- **Text-to-image (T2I):** satu caption menjadi query terhadap seluruh citra.

Flickr8k memiliki lima caption benar per citra. Untuk I2T, keberhasilan terjadi jika minimal satu dari lima caption benar masuk top-$K$. Untuk T2I, setiap caption memiliki satu citra pasangan.

### 4.6 Alignment bukan reasoning penuh

Similarity tinggi menunjukkan kesesuaian statistik dalam embedding space, tetapi tidak otomatis membuktikan:

- pemahaman sebab-akibat;
- kemampuan menghitung objek;
- pemahaman relasi spasial yang presisi;
- factual grounding;
- bebas bias;
- fidelity terhadap detail citra.

---

## 5. Dataset

### 5.1 Dataset A — Oxford-IIIT Pet

Digunakan untuk zero-shot classification:

```python
torchvision.datasets.OxfordIIITPet
```

Karakteristik:

- 37 breed anjing dan kucing;
- split resmi `trainval` dan `test`;
- citra natural dengan pose, skala, latar, dan pencahayaan beragam;
- cocok untuk fine-grained recognition dan prompt hierarchy.

Alasan kesinambungan: dataset yang sama digunakan pada Praktikum 04. Dengan mempertahankan domain, perubahan yang diamati lebih mudah dikaitkan dengan perbedaan representasi dan protokol.

### 5.2 Dataset B — Flickr8k

Digunakan untuk cross-modal retrieval:

```python
datasets.load_dataset("jxie/flickr8k")
```

Karakteristik:

- sekitar 8.000 citra natural;
- lima caption manusia per citra;
- split train, validation, dan test;
- cocok untuk protokol retrieval skala kelas.

Pada modul ini hanya split `test` digunakan karena tidak ada training/fine-tuning. Mode `QUICK` memakai subset tetap, sedangkan mode `FULL` memakai seluruh test split.

### 5.3 Risiko dan keterbatasan dataset

- Oxford-IIIT Pet terbatas pada hewan peliharaan dan label breed berbahasa Inggris.
- Flickr8k relatif kecil dan caption cenderung pendek/deskriptif.
- Citra internet dapat membawa bias sosial, budaya, dan konteks.
- Kemungkinan overlap atau kemiripan dengan data pretraining CLIP sulit diaudit sepenuhnya.
- Hasil tidak otomatis berlaku pada bahasa Indonesia atau domain khusus.

### 5.4 Alternatif pengembangan

| Dataset | Tugas | Sumber umum |
|---|---|---|
| CIFAR-100 | zero-shot classification | `torchvision` |
| Food-101 | fine-grained zero-shot | `torchvision` |
| EuroSAT | remote-sensing zero-shot | Hugging Face/`torchgeo` |
| RSICD | remote-sensing retrieval/caption | Hugging Face/repositori resmi |
| MS COCO Captions | retrieval/caption benchmark | COCO/`torchvision` |
| Flickr30k | retrieval/caption | Hugging Face/repositori resmi |
| Dataset penelitian | domain transfer | repositori resmi masing-masing |

Jika mengganti dataset, dokumentasikan versi, lisensi, split, unit analisis, bahasa caption, duplikasi, potensi leakage, dan alasan ilmiahnya.

---

## 6. Desain Eksperimen dan Validitas

### 6.1 Dua eksperimen inti

| Eksperimen | Dataset | Input | Output | Metrik |
|---|---|---|---|---|
| A: zero-shot classification | Oxford-IIIT Pet | citra + prompt kelas | 37 kelas | top-1, top-5, balanced accuracy, macro-F1 |
| B: cross-modal retrieval | Flickr8k | citra dan caption | ranking kandidat | R@1/5/10, MedR, MeanR, MRR |

### 6.2 Prompt development tanpa test leakage

1. Gunakan subset `trainval` Oxford sebagai **development set**.
2. Definisikan seluruh prompt set sebelum melihat test result.
3. Pilih satu prompt final berdasarkan `development macro-F1`.
4. Kunci prompt tersebut.
5. Evaluasi satu kali pada official test set.
6. Prompt lain boleh tetap dibandingkan sebagai eksperimen sensitivity yang telah dipraregistrasi, bukan untuk memilih setelah melihat test.

### 6.3 Variabel

- **Bebas:** prompt set, ensemble, query direction, ukuran candidate pool, dan semantic control.
- **Terikat:** metrik klasifikasi, ranking, latency, dan error category.
- **Kontrol:** checkpoint, preprocessing, subset indices, seed sampling, batch size, dan device.

### 6.4 Baseline

- chance level Oxford-IIIT Pet: $1/37$ untuk top-1 pada kelas seimbang;
- raw-label prompt sebagai baseline prompt;
- random ranking sebagai baseline retrieval;
- hasil DINOv2 dari Praktikum 04 sebagai konteks, bukan perbandingan setara dengan CLIP zero-shot.

---

## 7. Struktur Folder

```text
praktikum-05-vision-language/
├── Praktikum_05_Vision_Language.ipynb
├── data/
├── cache_embeddings/
├── outputs_pertemuan_05/
│   ├── config.json
│   ├── environment.json
│   ├── prompt_definitions.json
│   ├── zero_shot_dev_results.csv
│   ├── zero_shot_test_results.csv
│   ├── per_class_results.csv
│   ├── retrieval_results.csv
│   ├── semantic_control_results.csv
│   ├── confusion_matrix.png
│   └── research_log.csv
└── laporan_praktikum_05.pdf
```

---

## 8. Setup Environment

### 8.1 Google Colab

Pilih GPU jika tersedia, lalu:

```python
!pip -q install -U torch torchvision transformers datasets scikit-learn pandas matplotlib seaborn tqdm
```

### 8.2 Local Python 3.12

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Linux/macOS:

```bash
source .venv/bin/activate
```

```bash
python -m pip install --upgrade pip
python -m pip install torch torchvision transformers datasets scikit-learn pandas matplotlib seaborn tqdm jupyterlab
jupyter lab
```

Unduhan pertama memerlukan koneksi internet dan ruang cache. Jangan menyimpan token atau credential dalam notebook.

---

## 9. Import, Environment, dan Reproducibility

```python
from pathlib import Path
from collections import Counter
import json
import os
import platform
import random
import time

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
import datasets
import sklearn
import torch
import torchvision
import transformers

from datasets import load_dataset
from IPython.display import display
from sklearn.metrics import (
    accuracy_score,
    balanced_accuracy_score,
    classification_report,
    confusion_matrix,
    f1_score,
    top_k_accuracy_score,
)
from torch.utils.data import DataLoader, Subset
from torchvision.datasets import OxfordIIITPet
from tqdm.auto import tqdm
from transformers import AutoProcessor, CLIPModel

print("Python       :", platform.python_version())
print("PyTorch      :", torch.__version__)
print("torchvision  :", torchvision.__version__)
print("transformers :", transformers.__version__)
print("scikit-learn:", sklearn.__version__)
print("CUDA aktif   :", torch.cuda.is_available())
if torch.cuda.is_available():
    print("GPU          :", torch.cuda.get_device_name(0))
```

```python
MODE = "QUICK"  # "QUICK", "FULL", atau "CUSTOM"

DATA_DIR = Path("./data")
CACHE_DIR = Path("./cache_embeddings")
OUTPUT_DIR = Path("./outputs_pertemuan_05")
for directory in [DATA_DIR, CACHE_DIR, OUTPUT_DIR]:
    directory.mkdir(parents=True, exist_ok=True)

if MODE == "QUICK":
    CONFIG = dict(
        oxford_dev_per_class=10,
        oxford_test_per_class=15,
        flickr_test_size=200,
        batch_size=32,
        bootstrap_iterations=300,
        seed=42,
    )
elif MODE == "FULL":
    CONFIG = dict(
        oxford_dev_per_class=30,
        oxford_test_per_class=None,
        flickr_test_size=None,
        batch_size=64,
        bootstrap_iterations=1000,
        seed=42,
    )
else:
    CONFIG = dict(
        oxford_dev_per_class=20,
        oxford_test_per_class=30,
        flickr_test_size=500,
        batch_size=32,
        bootstrap_iterations=500,
        seed=42,
    )

CONFIG.update(
    mode=MODE,
    model_id="openai/clip-vit-base-patch32",
    oxford_dataset="Oxford-IIIT Pet",
    flickr_dataset="jxie/flickr8k",
    flickr_split="test",
)

DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(json.dumps(CONFIG, indent=2))
print("Device:", DEVICE)
```

```python
def set_seed(seed=42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False


set_seed(CONFIG["seed"])
```

---

## 10. Memuat CLIP

```python
MODEL_ID = CONFIG["model_id"]
processor = AutoProcessor.from_pretrained(MODEL_ID)
model = CLIPModel.from_pretrained(MODEL_ID).to(DEVICE).eval()

for parameter in model.parameters():
    parameter.requires_grad = False

total_params = sum(p.numel() for p in model.parameters())
print(f"Model: {MODEL_ID}")
print(f"Parameters: {total_params / 1e6:.2f} M")
print("Projection dimension:", model.config.projection_dim)
```

`AutoProcessor` menggabungkan tokenizer dan image processor yang sesuai dengan checkpoint. Jangan mengganti normalisasi/crop secara sembarang karena preprocessing merupakan bagian dari pipeline pretrained.

### 10.1 Fungsi normalisasi dan encoding

```python
def torch_l2_normalize(x, eps=1e-12):
    return x / x.norm(dim=-1, keepdim=True).clamp_min(eps)


@torch.inference_mode()
def encode_images(images, batch_size=None):
    batch_size = batch_size or CONFIG["batch_size"]
    all_features = []
    start = time.perf_counter()

    for start_idx in tqdm(range(0, len(images), batch_size), desc="Encode images"):
        batch = images[start_idx:start_idx + batch_size]
        inputs = processor(images=batch, return_tensors="pt")
        pixel_values = inputs["pixel_values"].to(DEVICE)
        vision_outputs = model.vision_model(pixel_values=pixel_values)
        features = model.visual_projection(vision_outputs.pooler_output)
        all_features.append(torch_l2_normalize(features).cpu())

    elapsed = time.perf_counter() - start
    return torch.cat(all_features, dim=0), elapsed


@torch.inference_mode()
def encode_texts(texts, batch_size=None):
    batch_size = batch_size or CONFIG["batch_size"]
    all_features = []
    start = time.perf_counter()

    for start_idx in tqdm(range(0, len(texts), batch_size), desc="Encode texts"):
        batch = texts[start_idx:start_idx + batch_size]
        inputs = processor(
            text=batch,
            return_tensors="pt",
            padding=True,
            truncation=True,
        )
        allowed_keys = {"input_ids", "attention_mask", "position_ids"}
        text_inputs = {
            k: v.to(DEVICE) for k, v in inputs.items() if k in allowed_keys
        }
        text_outputs = model.text_model(**text_inputs)
        features = model.text_projection(text_outputs.pooler_output)
        all_features.append(torch_l2_normalize(features).cpu())

    elapsed = time.perf_counter() - start
    return torch.cat(all_features, dim=0), elapsed
```

### 10.2 Sanity check ruang bersama

```python
demo_image = OxfordIIITPet(DATA_DIR, split="test", download=True)[0][0]
demo_texts = [
    "a photo of a pet",
    "a photo of a vehicle",
    "a bowl of food",
]

demo_image_features, _ = encode_images([demo_image])
demo_text_features, _ = encode_texts(demo_texts)
demo_scores = (demo_image_features @ demo_text_features.T).squeeze(0)

display(pd.DataFrame({"text": demo_texts, "cosine_similarity": demo_scores.numpy()}))
```

Sanity check tidak membuktikan kinerja model. Tujuannya memastikan image/text encoder menghasilkan dimensi yang sama dan similarity dapat dihitung.

---

## 11. Eksperimen A — Oxford-IIIT Pet Zero-Shot Classification

### 11.1 Memuat development dan test split

```python
oxford_dev_raw = OxfordIIITPet(
    root=DATA_DIR,
    split="trainval",
    target_types="category",
    download=True,
)
oxford_test_raw = OxfordIIITPet(
    root=DATA_DIR,
    split="test",
    target_types="category",
    download=True,
)

CLASS_NAMES_RAW = oxford_dev_raw.classes
NUM_CLASSES = len(CLASS_NAMES_RAW)
print("Classes:", NUM_CLASSES)
print("Development pool:", len(oxford_dev_raw))
print("Official test:", len(oxford_test_raw))
```

### 11.2 Label canonicalization

Nama internal dataset perlu diubah menjadi bentuk bahasa alami.

```python
def canonicalize_label(name):
    return name.replace("_", " ").lower().strip()


CLASS_NAMES = [canonicalize_label(name) for name in CLASS_NAMES_RAW]

CAT_BREEDS = {
    "abyssinian", "bengal", "birman", "bombay", "british shorthair",
    "egyptian mau", "maine coon", "persian", "ragdoll",
    "russian blue", "siamese", "sphynx",
}


def superclass_for(label):
    return "cat" if label in CAT_BREEDS else "dog"


label_table = pd.DataFrame({
    "class_id": range(NUM_CLASSES),
    "raw": CLASS_NAMES_RAW,
    "canonical": CLASS_NAMES,
    "superclass": [superclass_for(label) for label in CLASS_NAMES],
})
display(label_table)
```

Pemeriksaan manual wajib: pastikan setiap breed dipetakan ke superclass yang benar. Kesalahan kanonikalisasi adalah kesalahan protokol, bukan kegagalan model.

### 11.3 Fixed stratified subset

```python
def collect_labels(dataset):
    return np.asarray([dataset[i][1] for i in range(len(dataset))], dtype=np.int64)


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


dev_labels_all = collect_labels(oxford_dev_raw)
test_labels_all = collect_labels(oxford_test_raw)

dev_indices = fixed_per_class_indices(
    dev_labels_all, CONFIG["oxford_dev_per_class"], CONFIG["seed"]
)
test_indices = fixed_per_class_indices(
    test_labels_all, CONFIG["oxford_test_per_class"], CONFIG["seed"]
)

oxford_dev = Subset(oxford_dev_raw, dev_indices)
oxford_test = Subset(oxford_test_raw, test_indices)

print("Dev used :", len(oxford_dev))
print("Test used:", len(oxford_test))
```

### 11.4 Audit dan visualisasi

```python
fig, axes = plt.subplots(3, 4, figsize=(13, 10))
seen = set()
for image, label in oxford_dev:
    if label not in seen:
        ax = axes.flat[len(seen)]
        ax.imshow(image)
        ax.set_title(CLASS_NAMES[label])
        ax.axis("off")
        seen.add(label)
    if len(seen) == len(axes.flat):
        break
plt.suptitle("Contoh Oxford-IIIT Pet development set", y=1.01)
plt.tight_layout()
plt.show()
```

Pertanyaan audit:

1. Kelas mana yang mirip secara bentuk, warna, atau tekstur?
2. Apakah wajah/badan selalu terlihat utuh?
3. Apakah latar berpotensi menjadi shortcut?
4. Apakah label breed cukup natural untuk text encoder?

### 11.5 Ekstraksi image embedding

```python
dev_images = [oxford_dev[i][0].convert("RGB") for i in range(len(oxford_dev))]
dev_y = np.asarray([oxford_dev[i][1] for i in range(len(oxford_dev))])

test_images = [oxford_test[i][0].convert("RGB") for i in range(len(oxford_test))]
test_y = np.asarray([oxford_test[i][1] for i in range(len(oxford_test))])

dev_image_features, dev_image_seconds = encode_images(dev_images)
test_image_features, test_image_seconds = encode_images(test_images)

print("Dev features :", tuple(dev_image_features.shape))
print("Test features:", tuple(test_image_features.shape))
print(f"Test throughput: {len(test_images) / test_image_seconds:.2f} images/s")
```

```python
torch.save(
    {
        "features": dev_image_features,
        "labels": torch.tensor(dev_y),
        "indices": torch.tensor(dev_indices),
        "model_id": MODEL_ID,
    },
    CACHE_DIR / f"{MODE.lower()}_oxford_dev_clip.pt",
)
torch.save(
    {
        "features": test_image_features,
        "labels": torch.tensor(test_y),
        "indices": torch.tensor(test_indices),
        "model_id": MODEL_ID,
    },
    CACHE_DIR / f"{MODE.lower()}_oxford_test_clip.pt",
)
```

---

## 12. Mendefinisikan Prompt Sets

```python
PROMPT_SETS = {
    "raw_label": ["{label}"],
    "generic_photo": ["a photo of a {label}."],
    "hierarchical": ["a photo of a {label}, a breed of {superclass}."],
    "ensemble": [
        "a photo of a {label}.",
        "a close-up photo of a {label}.",
        "a photo of a {label}, a breed of {superclass}.",
        "a clear photo of the face of a {label}.",
        "a natural photo of a {label} pet.",
    ],
    "mismatched_style_control": ["a pencil drawing of a {label}."],
}

# Hanya prompt kandidat yang boleh dipilih sebagai protokol final.
# Control tetap dievaluasi, tetapi tidak ikut proses pemilihan.
SELECTABLE_PROMPT_SETS = [
    "raw_label",
    "generic_photo",
    "hierarchical",
    "ensemble",
]


def render_prompt(template, label):
    return template.format(label=label, superclass=superclass_for(label))


for set_name, templates in PROMPT_SETS.items():
    print("\n", set_name)
    for template in templates:
        print(" -", render_prompt(template, CLASS_NAMES[0]))
```

Prompt `mismatched_style_control` adalah kontrol untuk menguji ketidakcocokan gaya pada foto. Jangan mengasumsikan bahwa hasilnya pasti lebih buruk sebelum diuji.

---

## 13. Membuat Text Prototypes dan Prompt Ensemble

```python
def build_text_prototypes(class_names, templates):
    prototypes = []
    rendered_prompts = {}

    for class_id, label in enumerate(class_names):
        prompts = [render_prompt(template, label) for template in templates]
        features, _ = encode_texts(prompts)
        prototype = features.mean(dim=0, keepdim=True)
        prototype = torch_l2_normalize(prototype)
        prototypes.append(prototype)
        rendered_prompts[class_id] = prompts

    return torch.cat(prototypes, dim=0), rendered_prompts


text_prototypes = {}
rendered_prompt_log = {}

for set_name, templates in PROMPT_SETS.items():
    prototypes, prompt_log = build_text_prototypes(CLASS_NAMES, templates)
    text_prototypes[set_name] = prototypes
    rendered_prompt_log[set_name] = prompt_log
    print(set_name, tuple(prototypes.shape))
```

Sanity checks:

```python
for set_name, prototypes in text_prototypes.items():
    assert prototypes.shape == (NUM_CLASSES, model.config.projection_dim)
    assert torch.isfinite(prototypes).all()
    norms = prototypes.norm(dim=1)
    assert torch.allclose(norms, torch.ones_like(norms), atol=1e-5)
print("Seluruh text prototypes valid.")
```

---

## 14. Fungsi Evaluasi Zero-Shot

```python
def zero_shot_scores(image_features, prototypes):
    return (image_features @ prototypes.T).numpy()


def evaluate_zero_shot(y_true, scores, labels):
    y_pred = scores.argmax(axis=1)
    return {
        "top1_accuracy": accuracy_score(y_true, y_pred),
        "top5_accuracy": top_k_accuracy_score(
            y_true, scores, k=5, labels=labels
        ),
        "balanced_accuracy": balanced_accuracy_score(y_true, y_pred),
        "macro_f1": f1_score(y_true, y_pred, average="macro"),
    }, y_pred
```

### 14.1 Development-set evaluation

```python
dev_rows = []
dev_predictions = {}

for set_name, prototypes in text_prototypes.items():
    scores = zero_shot_scores(dev_image_features, prototypes)
    metrics, predictions = evaluate_zero_shot(
        dev_y, scores, labels=np.arange(NUM_CLASSES)
    )
    dev_rows.append({"prompt_set": set_name, **metrics})
    dev_predictions[set_name] = predictions

dev_results = pd.DataFrame(dev_rows).sort_values("macro_f1", ascending=False)
display(dev_results.round(4))
```

### 14.2 Mengunci prompt final

```python
selectable_dev = dev_results[
    dev_results["prompt_set"].isin(SELECTABLE_PROMPT_SETS)
].sort_values("macro_f1", ascending=False)
LOCKED_PROMPT_SET = selectable_dev.iloc[0]["prompt_set"]
print("Prompt dipilih hanya dari development set:", LOCKED_PROMPT_SET)
```

Catat prompt yang terkunci sebelum test evaluation. Untuk eksperimen terkontrol, semua kandidat prompt sudah didefinisikan sebelumnya.

### 14.3 Test-set sensitivity dan evaluasi final

```python
test_rows = []
test_objects = {}

for set_name, prototypes in text_prototypes.items():
    scores = zero_shot_scores(test_image_features, prototypes)
    metrics, predictions = evaluate_zero_shot(
        test_y, scores, labels=np.arange(NUM_CLASSES)
    )
    test_rows.append({
        "prompt_set": set_name,
        "locked_from_dev": set_name == LOCKED_PROMPT_SET,
        **metrics,
    })
    test_objects[set_name] = {"scores": scores, "predictions": predictions}

test_results = pd.DataFrame(test_rows).sort_values("macro_f1", ascending=False)
display(test_results.round(4))
```

Interpretasi harus memisahkan:

- **hasil final:** prompt terkunci dari development set;
- **prompt-sensitivity study:** seluruh prompt set yang telah ditentukan sebelum test;
- **post-hoc prompt:** eksperimen baru setelah melihat test, yang tidak boleh diklaim sebagai hasil final tanpa test set baru.

---

## 15. Bootstrap Confidence Interval

Zero-shot inference deterministik untuk checkpoint, preprocessing, data, dan prompt yang sama. Oleh karena itu, multi-seed inference bukan ukuran variasi yang bermakna. Gunakan bootstrap pada unit citra untuk mengestimasi ketidakpastian sampel.

```python
def bootstrap_metric_ci(y_true, y_pred, metric_fn, n_boot=1000, seed=42):
    rng = np.random.default_rng(seed)
    n = len(y_true)
    values = []
    for _ in range(n_boot):
        idx = rng.integers(0, n, size=n)
        values.append(metric_fn(y_true[idx], y_pred[idx]))
    low, high = np.percentile(values, [2.5, 97.5])
    return float(np.mean(values)), float(low), float(high)


locked_pred = test_objects[LOCKED_PROMPT_SET]["predictions"]

acc_boot = bootstrap_metric_ci(
    test_y,
    locked_pred,
    accuracy_score,
    n_boot=CONFIG["bootstrap_iterations"],
    seed=CONFIG["seed"],
)
f1_boot = bootstrap_metric_ci(
    test_y,
    locked_pred,
    lambda yt, yp: f1_score(
        yt,
        yp,
        labels=np.arange(NUM_CLASSES),
        average="macro",
        zero_division=0,
    ),
    n_boot=CONFIG["bootstrap_iterations"],
    seed=CONFIG["seed"],
)

print("Accuracy bootstrap mean, 95% CI:", acc_boot)
print("Macro-F1 bootstrap mean, 95% CI:", f1_boot)
```

Catatan: bootstrap mengestimasi ketidakpastian terhadap sampling test, bukan ketidakpastian seluruh proses pretraining CLIP.

---

## 16. Analisis Per Kelas dan Confusion Matrix

```python
locked_scores = test_objects[LOCKED_PROMPT_SET]["scores"]
locked_pred = test_objects[LOCKED_PROMPT_SET]["predictions"]

report = classification_report(
    test_y,
    locked_pred,
    labels=np.arange(NUM_CLASSES),
    target_names=CLASS_NAMES,
    output_dict=True,
    zero_division=0,
)

per_class_df = (
    pd.DataFrame(report).T
    .iloc[:NUM_CLASSES]
    .reset_index(names="class_name")
    .sort_values("f1-score")
)
display(per_class_df)
```

```python
cm = confusion_matrix(test_y, locked_pred, normalize="true")
plt.figure(figsize=(16, 14))
sns.heatmap(cm, cmap="Blues", vmin=0, vmax=1)
plt.title(f"Normalized confusion matrix — {LOCKED_PROMPT_SET}")
plt.xlabel("Predicted class")
plt.ylabel("True class")
plt.tight_layout()
plt.savefig(OUTPUT_DIR / "confusion_matrix_locked_prompt.png", dpi=180)
plt.show()
```

### Pasangan kelas paling sering tertukar

```python
def top_confusions(y_true, y_pred, class_names, top_n=10):
    matrix = confusion_matrix(y_true, y_pred)
    np.fill_diagonal(matrix, 0)
    order = np.argsort(matrix.ravel())[::-1]
    rows = []
    for flat_idx in order:
        true_id, pred_id = np.unravel_index(flat_idx, matrix.shape)
        count = int(matrix[true_id, pred_id])
        if count == 0 or len(rows) == top_n:
            break
        rows.append({
            "true": class_names[true_id],
            "predicted": class_names[pred_id],
            "count": count,
        })
    return pd.DataFrame(rows)


display(top_confusions(test_y, locked_pred, CLASS_NAMES))
```

---

## 17. Failure-Case Analysis Zero-Shot

```python
def show_zero_shot_failures(n=12):
    wrong = np.flatnonzero(test_y != locked_pred)
    if len(wrong) == 0:
        print("Tidak ada failure case pada subset ini.")
        return
    rng = np.random.default_rng(CONFIG["seed"])
    chosen = rng.choice(wrong, size=min(n, len(wrong)), replace=False)

    rows = int(np.ceil(len(chosen) / 4))
    fig, axes = plt.subplots(rows, 4, figsize=(15, 3.8 * rows))
    axes = np.atleast_1d(axes).ravel()

    for ax, pos in zip(axes, chosen):
        top3 = locked_scores[pos].argsort()[-3:][::-1]
        top3_text = ", ".join(CLASS_NAMES[i] for i in top3)
        ax.imshow(test_images[pos])
        ax.set_title(
            f"T: {CLASS_NAMES[test_y[pos]]}\n"
            f"P: {CLASS_NAMES[locked_pred[pos]]}\n"
            f"Top-3: {top3_text}",
            fontsize=8,
        )
        ax.axis("off")

    for ax in axes[len(chosen):]:
        ax.axis("off")
    plt.tight_layout()
    plt.show()


show_zero_shot_failures()
```

Kategorikan minimal 15 kesalahan:

| Kategori | Indikator |
|---|---|
| Visual ambiguity | breed mirip, objek kecil, blur, occlusion |
| Fine-grained semantic confusion | superclass benar tetapi breed salah |
| Linguistic gap | nama breed jarang/tidak natural |
| Prompt-induced | prediksi berubah akibat template |
| Background shortcut | model tampak mengikuti latar |
| Pose/crop mismatch | template menekankan wajah tetapi citra tubuh penuh |
| Label ambiguity/noise | label sulit diverifikasi secara visual |

Failure analysis adalah hipotesis penyebab. Lanjutkan dengan kontrol sebelum menyatakan mekanisme kausal.

---

## 18. Analisis Sensitivitas Prompt

```python
metric_columns = ["top1_accuracy", "top5_accuracy", "balanced_accuracy", "macro_f1"]
plot_df = test_results.set_index("prompt_set")[metric_columns]
plot_df.plot(kind="bar", figsize=(11, 6))
plt.ylabel("Score")
plt.title("Prompt sensitivity pada Oxford-IIIT Pet")
plt.ylim(0, 1)
plt.grid(axis="y", alpha=0.3)
plt.tight_layout()
plt.savefig(OUTPUT_DIR / "prompt_sensitivity.png", dpi=180)
plt.show()
```

### Prediction disagreement

```python
prompt_names = list(test_objects)
disagreement = np.zeros((len(prompt_names), len(prompt_names)))

for i, name_i in enumerate(prompt_names):
    for j, name_j in enumerate(prompt_names):
        pred_i = test_objects[name_i]["predictions"]
        pred_j = test_objects[name_j]["predictions"]
        disagreement[i, j] = np.mean(pred_i != pred_j)

plt.figure(figsize=(8, 7))
sns.heatmap(
    disagreement,
    annot=True,
    fmt=".2f",
    xticklabels=prompt_names,
    yticklabels=prompt_names,
    cmap="Reds",
)
plt.title("Proporsi prediction disagreement antar-prompt")
plt.tight_layout()
plt.show()
```

Pertanyaan:

1. Apakah prompt terbaik di development juga terbaik di test?
2. Berapa proporsi citra yang berubah prediksi ketika prompt berubah?
3. Kelas mana yang paling sensitif?
4. Apakah prompt ensemble meningkatkan mean score atau hanya mengubah kelas tertentu?
5. Apakah kontrol gaya yang salah selalu menurunkan skor?

---

## 19. Eksperimen B — Flickr8k Cross-Modal Retrieval

### 19.1 Memuat test split

```python
flickr = load_dataset(
    CONFIG["flickr_dataset"],
    split=CONFIG["flickr_split"],
)

print(flickr)
print(flickr.column_names)
```

Expected columns:

```text
image, caption_0, caption_1, caption_2, caption_3, caption_4
```

Jika schema berubah, hentikan dan inspeksi dataset card. Jangan menebak nama kolom.

### 19.2 Fixed subset

```python
required_columns = {"image", *[f"caption_{i}" for i in range(5)]}
missing = required_columns - set(flickr.column_names)
if missing:
    raise ValueError(f"Kolom Flickr8k tidak sesuai dokumentasi: {missing}")

rng = np.random.default_rng(CONFIG["seed"])
all_indices = np.arange(len(flickr))
rng.shuffle(all_indices)

n_flickr = CONFIG["flickr_test_size"] or len(flickr)
flickr_indices = np.sort(all_indices[:n_flickr])
flickr_eval = flickr.select(flickr_indices.tolist())

print("Flickr8k retrieval images:", len(flickr_eval))
```

### 19.3 Audit pasangan citra-caption

```python
fig, axes = plt.subplots(2, 3, figsize=(15, 9))
for ax, idx in zip(axes.flat, range(6)):
    row = flickr_eval[idx]
    ax.imshow(row["image"].convert("RGB"))
    ax.set_title(row["caption_0"][:90], fontsize=9)
    ax.axis("off")
plt.tight_layout()
plt.show()
```

Periksa:

- apakah lima caption benar-benar mendeskripsikan citra;
- variasi detail antar-caption;
- potensi caption ambigu atau tidak lengkap;
- bias bahasa dan kesalahan ejaan.

### 19.4 Menyusun gallery dan positive mapping

```python
flickr_images = []
flickr_captions = []
caption_to_image = []
image_to_caption_indices = []

for image_id, row in enumerate(flickr_eval):
    flickr_images.append(row["image"].convert("RGB"))
    positive_caption_ids = []
    for caption_number in range(5):
        caption_id = len(flickr_captions)
        flickr_captions.append(row[f"caption_{caption_number}"])
        caption_to_image.append(image_id)
        positive_caption_ids.append(caption_id)
    image_to_caption_indices.append(positive_caption_ids)

caption_to_image = np.asarray(caption_to_image, dtype=np.int64)
image_to_caption_indices = np.asarray(image_to_caption_indices, dtype=np.int64)

print("Images  :", len(flickr_images))
print("Captions:", len(flickr_captions))
print("Mapping :", image_to_caption_indices.shape)
```

### 19.5 Ekstraksi embedding

```python
flickr_image_features, flickr_image_seconds = encode_images(flickr_images)
flickr_text_features, flickr_text_seconds = encode_texts(flickr_captions)

assert flickr_image_features.shape[0] == len(flickr_images)
assert flickr_text_features.shape[0] == len(flickr_captions)
assert flickr_image_features.shape[1] == flickr_text_features.shape[1]

similarity_matrix = (flickr_image_features @ flickr_text_features.T).numpy()
print("Similarity matrix:", similarity_matrix.shape)
```

---

## 20. Metrik Retrieval Dua Arah

### 20.1 Menghitung rank

```python
def image_to_text_ranks(scores, positive_caption_ids):
    ranks = []
    for image_id in range(scores.shape[0]):
        order = np.argsort(scores[image_id])[::-1]
        positions = np.flatnonzero(
            np.isin(order, positive_caption_ids[image_id])
        )
        ranks.append(int(positions.min()) + 1)
    return np.asarray(ranks)


def text_to_image_ranks(scores, target_image_ids):
    ranks = []
    text_to_image_scores = scores.T
    for caption_id in range(text_to_image_scores.shape[0]):
        order = np.argsort(text_to_image_scores[caption_id])[::-1]
        position = np.flatnonzero(order == target_image_ids[caption_id])[0]
        ranks.append(int(position) + 1)
    return np.asarray(ranks)


i2t_ranks = image_to_text_ranks(similarity_matrix, image_to_caption_indices)
t2i_ranks = text_to_image_ranks(similarity_matrix, caption_to_image)
```

### 20.2 Ringkasan metrik

```python
def summarize_ranks(ranks, direction):
    return {
        "direction": direction,
        "queries": len(ranks),
        "R@1": np.mean(ranks <= 1),
        "R@5": np.mean(ranks <= 5),
        "R@10": np.mean(ranks <= 10),
        "median_rank": np.median(ranks),
        "mean_rank": np.mean(ranks),
        "MRR": np.mean(1.0 / ranks),
    }


retrieval_results = pd.DataFrame([
    summarize_ranks(i2t_ranks, "image_to_text"),
    summarize_ranks(t2i_ranks, "text_to_image"),
])
display(retrieval_results.round(4))
```

### 20.3 Random-ranking baseline dan confidence interval

Untuk $N$ citra dan $5N$ caption, baseline acak membantu membaca apakah skor berada jauh di atas ranking tanpa informasi.

```python
def random_retrieval_baselines(n_images, captions_per_image=5):
    rows = []
    n_captions = n_images * captions_per_image
    for k in [1, 5, 10]:
        # Peluang minimal satu dari lima caption positif terambil tanpa replacement.
        miss_probability = 1.0
        for draw in range(min(k, n_captions)):
            miss_probability *= (
                (n_captions - captions_per_image - draw)
                / (n_captions - draw)
            )
        rows.append({
            "K": k,
            "random_I2T_R@K": 1.0 - miss_probability,
            "random_T2I_R@K": min(k / n_images, 1.0),
        })
    return pd.DataFrame(rows)


display(random_retrieval_baselines(len(flickr_images)).round(4))
```

```python
def bootstrap_recall_ci(ranks, k, n_boot=1000, seed=42):
    rng = np.random.default_rng(seed)
    successes = (np.asarray(ranks) <= k).astype(float)
    estimates = []
    for _ in range(n_boot):
        idx = rng.integers(0, len(successes), size=len(successes))
        estimates.append(successes[idx].mean())
    low, high = np.percentile(estimates, [2.5, 97.5])
    return successes.mean(), float(low), float(high)


retrieval_ci_rows = []
for direction, ranks in [("I2T", i2t_ranks), ("T2I", t2i_ranks)]:
    for k in [1, 5, 10]:
        estimate, low, high = bootstrap_recall_ci(
            ranks,
            k,
            n_boot=CONFIG["bootstrap_iterations"],
            seed=CONFIG["seed"],
        )
        retrieval_ci_rows.append({
            "direction": direction,
            "K": k,
            "recall": estimate,
            "ci95_low": low,
            "ci95_high": high,
        })

retrieval_ci = pd.DataFrame(retrieval_ci_rows)
display(retrieval_ci.round(4))
```

Interpretasi:

- R@K semakin tinggi semakin baik.
- Median/mean rank semakin rendah semakin baik.
- MRR semakin tinggi semakin baik.
- I2T dan T2I tidak harus identik karena jumlah query/positive berbeda.
- Nilai bergantung pada candidate-pool size; selalu laporkan jumlah kandidat.

---

## 21. Visualisasi Hasil Retrieval

### 21.1 Image-to-text

```python
def show_i2t(query_image_id, k=5):
    order = np.argsort(similarity_matrix[query_image_id])[::-1][:k]
    positives = set(image_to_caption_indices[query_image_id].tolist())

    plt.figure(figsize=(5, 4))
    plt.imshow(flickr_images[query_image_id])
    plt.axis("off")
    plt.title(f"Image query {query_image_id}")
    plt.show()

    rows = []
    for rank, caption_id in enumerate(order, start=1):
        rows.append({
            "rank": rank,
            "caption": flickr_captions[caption_id],
            "score": similarity_matrix[query_image_id, caption_id],
            "is_ground_truth": caption_id in positives,
        })
    display(pd.DataFrame(rows))


show_i2t(query_image_id=0, k=5)
```

### 21.2 Text-to-image

```python
def show_t2i(query_caption_id, k=5):
    order = np.argsort(similarity_matrix[:, query_caption_id])[::-1][:k]
    target = caption_to_image[query_caption_id]

    print("Query:", flickr_captions[query_caption_id])
    fig, axes = plt.subplots(1, k, figsize=(3.2 * k, 3.5))
    for rank, (ax, image_id) in enumerate(zip(axes, order), start=1):
        ax.imshow(flickr_images[image_id])
        ax.set_title(
            f"rank {rank}\nscore={similarity_matrix[image_id, query_caption_id]:.3f}\n"
            f"correct={image_id == target}",
            fontsize=8,
        )
        ax.axis("off")
    plt.tight_layout()
    plt.show()


show_t2i(query_caption_id=0, k=5)
```

Tampilkan minimal:

- lima query berhasil pada rank 1;
- lima query gagal dengan rank tinggi;
- satu kasus ketika hasil non-ground-truth tetap semantik relevan.

---

## 22. Retrieval Failure Analysis

```python
worst_i2t = np.argsort(i2t_ranks)[-10:][::-1]
worst_t2i = np.argsort(t2i_ranks)[-10:][::-1]

display(pd.DataFrame({
    "image_query": worst_i2t,
    "best_positive_rank": i2t_ranks[worst_i2t],
}))

display(pd.DataFrame({
    "caption_query": worst_t2i,
    "target_image": caption_to_image[worst_t2i],
    "target_rank": t2i_ranks[worst_t2i],
    "caption": [flickr_captions[i] for i in worst_t2i],
}))
```

Kategori analisis:

| Kategori | Contoh |
|---|---|
| Generic caption | “a person outside” cocok dengan banyak citra |
| Missing detail | caption tidak menyebut objek pembeda |
| Fine-grained attribute | warna/jumlah/aksi tidak tertangkap |
| Relational error | subjek–objek atau posisi tertukar |
| Hard negative | kandidat lain sangat mirip secara semantik |
| Annotation incompleteness | hasil non-ground-truth sebenarnya masuk akal |
| Dataset noise | caption salah atau ambigu |

> Pada retrieval, ground truth tidak selalu lengkap. Ranking “salah” dapat tetap semantik benar jika kandidat relevan tidak dianotasi sebagai pasangan.

---

## 23. Semantic-Control Experiment

Tujuan eksperimen ini adalah memeriksa apakah model peka terhadap perubahan atribut atau aksi, bukan hanya kata benda utama.

```python
SEMANTIC_CONTROLS = [
    # Isi setelah memeriksa citra secara manual. Contoh struktur:
    # {
    #     "image_id": 0,
    #     "positive": flickr_captions[0],
    #     "hard_negative": "<kalimat yang salah pada satu atribut terkontrol>",
    #     "control_type": "action",
    # },
]


semantic_rows = []
for item in SEMANTIC_CONTROLS:
    image_feature = flickr_image_features[item["image_id"]:item["image_id"] + 1]
    texts = [item["positive"], item["hard_negative"]]
    text_features, _ = encode_texts(texts)
    scores = (image_feature @ text_features.T).squeeze(0).numpy()
    semantic_rows.append({
        **item,
        "positive_score": scores[0],
        "hard_negative_score": scores[1],
        "margin": scores[0] - scores[1],
        "passes_control": scores[0] > scores[1],
    })

semantic_control_results = pd.DataFrame(semantic_rows)
if semantic_control_results.empty:
    print("TODO: tambahkan minimal 20 semantic controls yang diverifikasi manual.")
else:
    display(semantic_control_results)
```

Mahasiswa wajib menambah minimal 20 kontrol terverifikasi, mencakup:

1. objek sama, aksi berbeda;
2. objek sama, jumlah berbeda;
3. objek sama, warna berbeda;
4. subjek–objek tertukar;
5. paraphrase dengan makna sama.

Jangan membuat hard negative yang sebenarnya mungkin benar untuk citra. Setiap kontrol harus diperiksa manual.

---

## 24. Candidate-Pool Size Ablation

Retrieval menjadi lebih sulit ketika jumlah kandidat meningkat.

```python
def retrieval_at_pool_sizes(scores, image_to_caps, caption_to_img, pool_sizes):
    rows = []
    total_images = scores.shape[0]

    for pool_size in pool_sizes:
        pool_size = min(pool_size, total_images)
        image_ids = np.arange(pool_size)
        caption_ids = np.arange(pool_size * 5)
        sub_scores = scores[np.ix_(image_ids, caption_ids)]
        sub_i2c = image_to_caps[:pool_size]
        sub_c2i = caption_to_img[:pool_size * 5]

        i_ranks = image_to_text_ranks(sub_scores, sub_i2c)
        t_ranks = text_to_image_ranks(sub_scores, sub_c2i)
        rows.append({"pool_size": pool_size, **summarize_ranks(i_ranks, "I2T")})
        rows.append({"pool_size": pool_size, **summarize_ranks(t_ranks, "T2I")})

    return pd.DataFrame(rows)


pool_sizes = [25, 50, 100, len(flickr_images)]
pool_ablation = retrieval_at_pool_sizes(
    similarity_matrix,
    image_to_caption_indices,
    caption_to_image,
    pool_sizes,
)
display(pool_ablation.round(4))
```

Catatan: pemilihan urutan pool dapat memengaruhi hasil. Untuk laporan lebih kuat, ulangi sampling pool dengan beberapa seed dan laporkan mean ± std.

---

## 25. Analisis Efisiensi

```python
efficiency_df = pd.DataFrame([
    {
        "stage": "Oxford dev image encoding",
        "items": len(dev_images),
        "seconds": dev_image_seconds,
        "items_per_second": len(dev_images) / dev_image_seconds,
    },
    {
        "stage": "Oxford test image encoding",
        "items": len(test_images),
        "seconds": test_image_seconds,
        "items_per_second": len(test_images) / test_image_seconds,
    },
    {
        "stage": "Flickr image encoding",
        "items": len(flickr_images),
        "seconds": flickr_image_seconds,
        "items_per_second": len(flickr_images) / flickr_image_seconds,
    },
    {
        "stage": "Flickr caption encoding",
        "items": len(flickr_captions),
        "seconds": flickr_text_seconds,
        "items_per_second": len(flickr_captions) / flickr_text_seconds,
    },
])
display(efficiency_df.round(3))

embedding_dim = model.config.projection_dim
float32_bytes_per_embedding = embedding_dim * 4
print("Float32 bytes/embedding:", float32_bytes_per_embedding)
print("Approx MB for 1M embeddings:", float32_bytes_per_embedding * 1_000_000 / 1024**2)
```

Diskusikan perbedaan biaya:

- text prototypes kelas dapat di-cache sekali;
- image gallery embedding dapat dihitung offline;
- online query hanya membutuhkan satu encoder dan similarity search;
- brute-force matrix multiplication tidak selalu cocok untuk jutaan kandidat;
- approximate nearest-neighbor index dapat diperlukan pada deployment.

---

## 26. Menghubungkan Hasil DINOv2 dan CLIP

Isi tabel menggunakan hasil Praktikum 04 dan 05:

| Dimensi | DINOv2 | CLIP | Interpretasi hati-hati |
|---|---:|---:|---|
| Oxford linear-probe macro-F1 | ... | Tidak diuji pada inti P05 | Memerlukan label untuk melatih probe |
| Oxford zero-shot macro-F1 | Tidak langsung | ... | CLIP dapat membentuk kelas dari teks |
| Visual nearest-neighbor P@5 | ... | Opsional | Mengukur struktur image embedding |
| Cross-modal Flickr8k R@5 | Tidak tersedia langsung | ... | Hanya CLIP memiliki text encoder sejajar |
| Embedding dimension | ... | ... | Bukan ukuran kualitas tunggal |
| Image throughput | ... | ... | Pastikan perangkat/batch sama |

Klaim yang tidak valid:

> “CLIP kalah dari DINOv2 karena zero-shot accuracy lebih rendah daripada DINOv2 linear probe.”

Kedua skor memakai informasi downstream yang berbeda. Eksperimen yang lebih setara memerlukan linear probe pada image embedding kedua model dengan split dan label fraction yang sama.

---

## 27. Analisis yang Harus Diisi Mahasiswa

### A. RQ1 — Zero-shot capability

- Top-1/top-5 locked prompt: **...**
- Balanced accuracy/macro-F1: **...**
- Bootstrap 95% CI: **...**
- Selisih terhadap chance baseline: **...**

### B. RQ2–RQ4 — Prompt

- Prompt terbaik di development: **...**
- Apakah tetap terbaik pada test? **...**
- Prediction disagreement terbesar: **...**
- Kelas paling sensitif: **...**
- Dampak hierarchical context: **...**
- Dampak ensemble: **...**

### C. RQ5 — Retrieval

- I2T R@1/5/10, MedR, MRR: **...**
- T2I R@1/5/10, MedR, MRR: **...**
- Arah yang lebih sulit: **...**
- Pengaruh candidate-pool size: **...**

### D. RQ6 — Semantic controls

- Pass rate kontrol: **...**
- Kontrol paling sulit: **...**
- Apakah model lebih peka pada objek, atribut, aksi, jumlah, atau relasi? **...**

### E. RQ7 — Domain validity

- Kesamaan domain benchmark dan domain penelitian: **...**
- Linguistic/domain gap: **...**
- Data eksternal yang diperlukan: **...**
- Klaim yang masih aman: **...**

---

## 28. Ablation Study — Pilih Minimal Satu

| Ablation | Yang diubah | Yang dikontrol | Pertanyaan |
|---|---|---|---|
| Prompt count | 1, 3, 5 template | kelas/model/data | Kapan ensemble mencapai diminishing return? |
| Label wording | raw, canonical, synonym | template/model | Seberapa sensitif text prototype? |
| Superclass | tanpa vs cat/dog | template lain | Apakah hierarchy membantu? |
| Language | English vs Indonesian | arti prompt | Apakah multilingual gap muncul? |
| CLIP backbone | ViT-B/32 vs model lain | data/metrik | Apakah skala sepadan dengan biaya? |
| Candidate pool | 25/50/100/full | query/model | Bagaimana retrieval menurun? |
| Caption count | 1 vs 5 positif/citra | images/model | Bagaimana annotation coverage memengaruhi R@K? |
| Image degradation | clean vs blur/noise | prompt/model | Apakah alignment bertahan? |

Ubah satu faktor utama pada satu waktu. Untuk eksperimen post-hoc, gunakan validation atau test set baru.

---

## 29. Challenge Doktoral

### A. Domain-specific zero-shot benchmark

Bangun benchmark minimal 10 kelas dari domain riset. Susun label card yang mencatat definisi kelas, sinonim, superclass, dan kemungkinan ambiguity.

### B. Bahasa Indonesia dan multilingual prompts

Bandingkan English, Indonesian, dan bilingual ensemble. Jangan menerjemahkan istilah domain secara otomatis tanpa verifikasi pakar.

### C. Learnable prompt atau adapter

Bandingkan zero-shot, linear probe, prompt learning, dan partial fine-tuning dengan label budget yang sama.

### D. Bias audit

Rancang subgroup evaluation atau counterfactual control yang relevan dan etis. Hindari menyimpulkan fairness dari contoh kecil.

### E. Restoration bridge

Degradasikan citra dengan blur/noise, lalu ukur PSNR/SSIM (jika ada referensi) dan CLIP similarity terhadap caption. Analisis kondisi ketika skor semantik tinggi tetapi fidelity rendah.

### F. Scalable retrieval

Bangun FAISS/ANN index dan bandingkan exact search vs approximate search pada latency, memory, dan recall.

---

## 30. Paper Discussion

Baca paper CLIP dan minimal dua paper pendukung.

| Paper | Fokus pembacaan kritis |
|---|---|
| CLIP | dataset, contrastive objective, prompt ensemble, transfer protocol, bias/limitation |
| ALIGN | noisy data scale dan perbandingan dengan kurasi |
| BLIP/BLIP-2 | bootstrapping, filtering, generation, frozen encoders |
| Paper domain | external validity dan linguistic/domain adaptation |

### Template critical reading

1. Masalah: **...**
2. Klaim utama: **...**
3. Data dan sumber supervisi: **...**
4. Image/text architecture: **...**
5. Alignment objective: **...**
6. Downstream protocol: **...**
7. Baseline dan fairness: **...**
8. Metrik: **...**
9. Ablation terkuat: **...**
10. Failure case/bias: **...**
11. Ancaman validitas: **...**
12. Research gap: **...**

---

## 31. Research Log

Isi setiap run, termasuk run gagal atau prompt yang buruk.

| Waktu | Run ID | Dataset | Model | Prompt set | Split/pool | Seed | Metric utama | Latency | Status | Catatan/anomali |
|---|---|---|---|---|---|---:|---|---:|---|---|
| | | | | | | | | | | |
| | | | | | | | | | | |

Informasi wajib:

- Python/runtime/OS: **...**
- CPU/GPU: **...**
- versi PyTorch, torchvision, transformers, datasets: **...**
- exact model ID dan revision jika dikunci: **...**
- dataset ID, split, dan subset indices: **...**
- seluruh prompt template: **...**
- perubahan konfigurasi: **...**
- error dan penyelesaian: **...**

---

## 32. Ekspor Hasil

```python
dev_results.to_csv(OUTPUT_DIR / "zero_shot_dev_results.csv", index=False)
test_results.to_csv(OUTPUT_DIR / "zero_shot_test_results.csv", index=False)
per_class_df.to_csv(OUTPUT_DIR / "per_class_results.csv", index=False)
retrieval_results.to_csv(OUTPUT_DIR / "retrieval_results.csv", index=False)
retrieval_ci.to_csv(OUTPUT_DIR / "retrieval_confidence_intervals.csv", index=False)
pool_ablation.to_csv(OUTPUT_DIR / "retrieval_pool_ablation.csv", index=False)
semantic_control_results.to_csv(
    OUTPUT_DIR / "semantic_control_results.csv", index=False
)
efficiency_df.to_csv(OUTPUT_DIR / "efficiency_results.csv", index=False)

with open(OUTPUT_DIR / "config.json", "w", encoding="utf-8") as file:
    json.dump(CONFIG, file, indent=2)

with open(OUTPUT_DIR / "prompt_definitions.json", "w", encoding="utf-8") as file:
    json.dump(
        {
            "templates": PROMPT_SETS,
            "rendered": rendered_prompt_log,
            "locked_prompt_set": LOCKED_PROMPT_SET,
        },
        file,
        indent=2,
    )

environment = {
    "python": platform.python_version(),
    "pytorch": torch.__version__,
    "torchvision": torchvision.__version__,
    "transformers": transformers.__version__,
    "datasets": datasets.__version__,
    "sklearn": sklearn.__version__,
    "device": str(DEVICE),
    "gpu": torch.cuda.get_device_name(0) if torch.cuda.is_available() else None,
}
with open(OUTPUT_DIR / "environment.json", "w", encoding="utf-8") as file:
    json.dump(environment, file, indent=2)

print("Output:", OUTPUT_DIR.resolve())
for path in sorted(OUTPUT_DIR.iterdir()):
    print("-", path.name)
```

---

## 33. Pertanyaan Diskusi Kritis

1. Apakah CLIP benar-benar zero-shot jika konsep Oxford-IIIT Pet mungkin muncul saat pretraining?
2. Apa perbedaan zero-shot transfer dan open-set recognition?
3. Mengapa label mentah dapat menghasilkan skor berbeda dari kalimat natural?
4. Apakah prompt ensemble merupakan peningkatan model atau perubahan protokol evaluasi?
5. Mengapa prompt tidak boleh dipilih berdasarkan test set?
6. Apakah similarity dapat ditafsirkan sebagai probabilitas terkalibrasi?
7. Mengapa top-5 tinggi tetapi top-1 rendah dapat terjadi pada fine-grained classes?
8. Apakah hasil non-ground-truth pada retrieval selalu salah?
9. Mengapa candidate-pool size harus dilaporkan?
10. Apa bedanya alignment, grounding, compositionality, dan reasoning?
11. Bagaimana menguji sensitivitas terhadap jumlah atau relasi spasial?
12. Bagaimana linguistic gap memengaruhi domain Indonesia?
13. Apa risiko memakai caption internet sebagai supervisi?
14. Bagaimana mengaudit overlap downstream dan pretraining data?
15. Apakah CLIP score layak menjadi metrik kualitas restorasi tunggal?
16. Bagaimana membandingkan DINOv2 linear probe dan CLIP zero-shot secara adil?
17. Kapan fine-tuning/adaptation lebih tepat daripada prompt engineering?
18. Research gap apa yang muncul dari failure patterns?

---

## 34. Tugas Praktikum

### Tugas inti

1. Audit Oxford-IIIT Pet dan kanonikalisasi seluruh label.
2. Ekstrak CLIP image embedding pada development dan test set.
3. Definisikan minimal empat prompt set sebelum melihat test result.
4. Pilih prompt final menggunakan development set.
5. Evaluasi test dengan top-1, top-5, balanced accuracy, dan macro-F1.
6. Hitung bootstrap 95% CI untuk locked prompt.
7. Analisis confusion matrix dan minimal 15 failure cases.
8. Ukur prediction disagreement antar-prompt.
9. Jalankan I2T dan T2I retrieval pada Flickr8k.
10. Laporkan R@1/5/10, median rank, mean rank, dan MRR.
11. Analisis minimal 10 retrieval failures.
12. Tambahkan minimal 20 semantic controls.
13. Jalankan satu ablation study.
14. Tulis hubungan hasil dengan Praktikum 04 dan 06.
15. Rumuskan research gap serta eksperimen lanjutan.

### Deliverable

- notebook `.ipynb` yang dapat dijalankan ulang;
- laporan PDF 7–12 halaman;
- folder output CSV/JSON/gambar;
- research log;
- satu slide ringkasan hasil;
- daftar prompt lengkap;
- referensi paper sesuai ketentuan hak cipta institusi.

---

## 35. Struktur Laporan

1. Judul dan identitas
2. Posisi terhadap Praktikum 04 dan 06
3. Latar belakang dan research questions
4. Hipotesis yang ditulis sebelum eksperimen
5. Teori CLIP, zero-shot, prompt ensemble, dan retrieval
6. Dataset, split, sampling, lisensi, dan audit
7. Checkpoint, preprocessing, prompt protocol, dan hardware
8. Development-set prompt selection
9. Final zero-shot test result dan confidence interval
10. Prompt-sensitivity dan per-class analysis
11. Confusion matrix dan failure cases
12. I2T/T2I retrieval metrics dan qualitative examples
13. Semantic controls dan ablation
14. Bias, validitas, dan keterbatasan
15. Hubungan DINOv2–CLIP dan jembatan ke restoration
16. Research gap, proposed experiment, kesimpulan
17. Research log dan referensi

---

## 36. Rubrik Penilaian

| Aspek | Bobot |
|---|---:|
| Setup, audit data, preprocessing, dan reproducibility | 15% |
| Ketepatan zero-shot dan prompt protocol | 20% |
| Ketepatan retrieval serta metrik dua arah | 20% |
| Prompt sensitivity, semantic controls, dan ablation | 15% |
| Failure analysis, bias, dan validitas klaim | 15% |
| Research gap, keterkaitan antarmodul, dan kualitas laporan | 15% |
| **Total** | **100%** |

---

## 37. Checklist Penyelesaian

### Setup dan model

- [ ] Python, package, OS, dan hardware dicatat.
- [ ] Exact CLIP model ID dicatat.
- [ ] Image/text encoder dibekukan.
- [ ] Preprocessing resmi checkpoint digunakan.
- [ ] Image dan text embedding memiliki dimensi sama.
- [ ] Embedding dinormalisasi L2.

### Oxford-IIIT Pet

- [ ] Development dan official test dipisahkan.
- [ ] Label dikanonikalisasi dan diaudit.
- [ ] Mapping cat/dog diverifikasi.
- [ ] Fixed subset indices disimpan jika mode QUICK/CUSTOM.
- [ ] Prompt set didefinisikan sebelum test.
- [ ] Locked prompt dipilih dari development set.
- [ ] Top-1/top-5/balanced accuracy/macro-F1 dihitung.
- [ ] Bootstrap 95% CI dihitung.
- [ ] Per-class result dan confusion matrix dianalisis.
- [ ] Minimal 15 zero-shot failures dikategorikan.
- [ ] Prediction disagreement antar-prompt dihitung.

### Flickr8k retrieval

- [ ] Schema dataset diverifikasi.
- [ ] Lima caption per citra dipertahankan.
- [ ] Mapping image-caption benar.
- [ ] Similarity matrix memiliki bentuk yang benar.
- [ ] I2T dan T2I ranks dihitung terpisah.
- [ ] R@1/5/10, MedR, MeanR, MRR dilaporkan.
- [ ] Candidate-pool size dilaporkan.
- [ ] Minimal 10 failure cases dianalisis.
- [ ] Minimal 20 semantic controls dibuat dan diverifikasi.

### Analisis ilmiah

- [ ] Satu ablation diselesaikan.
- [ ] Hasil post-hoc dipisahkan dari final result.
- [ ] Alignment tidak disamakan dengan reasoning.
- [ ] Potensi pretraining overlap dibahas.
- [ ] Bias visual/bahasa dan linguistic gap dibahas.
- [ ] DINOv2 linear probe tidak dibandingkan secara naif dengan CLIP zero-shot.
- [ ] Jembatan ke image restoration dijelaskan.
- [ ] Research gap dan desain eksperimen ditulis.

### Pengumpulan

- [ ] Notebook dapat dijalankan dari awal sampai akhir.
- [ ] Output penting dipertahankan.
- [ ] CSV, JSON, visualisasi, dan research log tersedia.
- [ ] Seluruh prompt dicantumkan.
- [ ] Klaim tidak melebihi bukti.

---

## 38. Troubleshooting

### `ModuleNotFoundError`

```bash
python -m pip install -U torch torchvision transformers datasets scikit-learn pandas matplotlib seaborn tqdm
```

Pastikan kernel notebook memakai environment yang sama.

### Model atau dataset gagal diunduh

- periksa internet dan ruang penyimpanan;
- periksa exact model/dataset ID;
- jangan menaruh credential di notebook;
- simpan pesan error di research log;
- jangan mengganti dataset diam-diam tanpa mendokumentasikannya.

### CUDA out of memory

- turunkan `batch_size` menjadi 16, 8, atau 4;
- encode image dan text secara terpisah;
- simpan embedding ke CPU/cache;
- gunakan `QUICK` terlebih dahulu.

### `top_k_accuracy_score` error

Pastikan `labels=np.arange(NUM_CLASSES)` diberikan dan score matrix berisi kolom untuk seluruh kelas.

### Flickr8k schema berubah

Cetak `flickr.column_names`, periksa dataset card, dan ubah parser secara eksplisit. Jangan menebak mapping caption.

### Retrieval kehabisan memori

- kurangi `flickr_test_size`;
- simpan embedding sebagai float32/float16 dengan dokumentasi;
- hitung similarity secara chunked;
- jangan membentuk matriks kandidat sangat besar sekaligus.

### Hasil tidak berubah antar-seed

Zero-shot inference deterministik. Seed hanya memengaruhi sampling subset/bootstrap, bukan bobot CLIP. Gunakan bootstrap atau beberapa subset, bukan mengulang inference identik.

### Prompt ensemble lebih buruk

Itu hasil yang valid. Periksa apakah ada template tidak cocok, superclass salah, atau averaging dilakukan sebelum normalisasi akhir.

### Similarity dianggap probabilitas

Cosine similarity mentah bukan probabilitas terkalibrasi. Softmax hanya menghasilkan distribusi relatif terhadap candidate set tertentu.

---

## 39. Validitas, Bias, dan Etika

### Validitas internal

- preprocessing, checkpoint, dan subset harus tetap;
- prompt selection hanya menggunakan development set;
- mapping label/superclass harus benar;
- image-caption indexing harus diverifikasi.

### Validitas eksternal

- dua benchmark natural tidak mewakili semua domain;
- caption English tidak mewakili seluruh bahasa;
- external validation diperlukan untuk domain penelitian.

### Validitas konstruk

- zero-shot accuracy mengukur kecocokan prototype teks, bukan pemahaman umum;
- retrieval ground truth dapat tidak lengkap;
- similarity bukan confidence terkalibrasi;
- semantic control kecil tidak membuktikan reasoning menyeluruh.

### Bias dan etika

- pasangan web dapat membawa stereotip dan ketimpangan representasi;
- hindari inferensi atribut sensitif yang tidak relevan;
- gunakan subgroup/counterfactual audit dengan desain etis;
- periksa lisensi dataset dan model;
- jangan mengunggah data sensitif ke layanan publik;
- dokumentasikan model card, data card, dan batas penggunaan.

---

## 40. Kesimpulan Konseptual

Praktikum 05 memperluas fondasi Praktikum 04 dari visual embedding menuju ruang citra–teks. Bahasa memungkinkan kelas dan query disusun secara fleksibel, tetapi sekaligus memperkenalkan sensitivitas prompt, linguistic gap, dan bias baru.

Pesan utama:

1. CLIP menyelaraskan image dan text embedding melalui contrastive learning;
2. zero-shot classifier dibentuk dari text prototypes, bukan bobot kelas yang dilatih pada dataset target;
3. prompt adalah bagian dari protokol eksperimen dan harus dilaporkan;
4. prompt selection memerlukan development set agar test tidak bocor;
5. cross-modal retrieval harus dievaluasi dua arah dan menangani banyak caption benar;
6. candidate-pool size dan kelengkapan anotasi memengaruhi interpretasi;
7. alignment kuat belum membuktikan reasoning atau bebas bias;
8. failure analysis dan semantic controls dapat menghasilkan research gap;
9. CLIP similarity dapat membantu analisis semantik restorasi, tetapi bukan pengganti fidelity metric.

---

## 41. Referensi Utama

1. Radford, A., et al. (2021). *Learning Transferable Visual Models From Natural Language Supervision*. ICML. <https://arxiv.org/abs/2103.00020>
2. Jia, C., et al. (2021). *Scaling Up Visual and Vision-Language Representation Learning With Noisy Text Supervision*. ICML. <https://arxiv.org/abs/2102.05918>
3. Li, J., et al. (2022). *BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation*. ICML. <https://arxiv.org/abs/2201.12086>
4. Li, J., et al. (2023). *BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models*. ICML. <https://arxiv.org/abs/2301.12597>
5. OpenAI CLIP repository: <https://github.com/openai/CLIP>
6. Hugging Face CLIP documentation: <https://huggingface.co/docs/transformers/model_doc/clip>
7. OpenAI CLIP checkpoint: <https://huggingface.co/openai/clip-vit-base-patch32>
8. Oxford-IIIT Pet documentation: <https://docs.pytorch.org/vision/stable/generated/torchvision.datasets.OxfordIIITPet.html>
9. Flickr8k dataset card: <https://huggingface.co/datasets/jxie/flickr8k>
10. Flickr8k original paper: Hodosh, M., Young, P., & Hockenmaier, J. (2013). *Framing Image Description as a Ranking Task*. JAIR.

---

## Penutup

Gunakan vision-language model sebagai objek evaluasi ilmiah, bukan sekadar mesin pencocokan otomatis. Selalu tanyakan:

- bagaimana label diubah menjadi bahasa;
- pasangan data apa yang membentuk embedding;
- konsep mana yang stabil terhadap paraphrase;
- atribut atau relasi mana yang gagal;
- apakah ground truth retrieval lengkap;
- bagaimana bahasa, budaya, dan domain memengaruhi hasil;
- bukti apa yang diperlukan sebelum model digunakan dalam domain nyata.
