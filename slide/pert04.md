# Slide 00 - Cover

EF256129 - TD PCD

Pertemuan 04

## Self-Supervised Learning dan Foundation Vision Models

Dr. Darlis Herumurti

Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 04 dalam RPS

| Pertemuan | Topik | Keterkaitan |
|---|---|---|
| 03 | Representasi Visual Modern: CNN, Attention, dan Vision Transformer | Memberikan fondasi arsitektur backbone dan mekanisme representasi visual yang menjadi dasar model self-supervised |
| **04** | **Self-Supervised Learning dan Foundation Vision Models** | **Mempelajari cara melatih representasi visual tanpa label eksplisit pada arsitektur modern** |
| 05 | Vision-Language Models dan Multimodal Representation | Memanfaatkan representasi visual yang dipelajari secara self-supervised sebagai salah satu modalitas |

- Pertemuan ini menghubungkan pemahaman arsitektur (Pertemuan 03) dengan pembelajaran representasi tanpa label.
- Representasi yang dipelajari pada pertemuan ini akan menjadi fondasi untuk memahami model multimodal seperti CLIP pada Pertemuan 05.

---

# Slide 02 - Tujuan Pembelajaran

Setelah mengikuti pertemuan ini, mahasiswa diharapkan mampu:

- Menjelaskan konsep self-supervised learning (SSL) dan perbedaannya dengan supervised learning.
- Menganalisis mekanisme contrastive learning, teacher-student learning, momentum encoder, dan masked image modeling.
- Mengevaluasi representasi visual yang dipelajari oleh model self-supervised melalui linear probing dan visualisasi embedding.
- Merancang eksperimen transferability DINO atau DINOv2 terhadap domain, resolusi, jumlah label, dan distribusi data baru.
- Memberikan rekomendasi penggunaan foundation vision model yang sesuai untuk domain penelitian masing-masing.

---

# Slide 03 - Agenda Pertemuan

1. Paradigma pembelajaran: supervised, unsupervised, dan self-supervised.
2. Konsep inti self-supervised learning untuk visi komputer.
3. Contrastive learning: SimCLR, MoCo, dan momentum encoder.
4. Teacher-student learning dan DINO.
5. Masked image modeling: MAE.
6. DINOv2: representasi visual tanpa supervisi.
7. Linear probing sebagai protokol evaluasi representasi.
8. Desain eksperimen transferability.
9. Praktikum: ekstraksi embedding, visualisasi, dan linear probing.
10. Diskusi seminar paper dan target keluaran.

---

# Slide 04 - Mengapa Self-Supervised Learning?

- Label anotasi mahal, lambat, dan rawan inkonsistensi.
- Data citra tanpa label tersedia dalam skala sangat besar di internet, medis, satelit, dan industri.
- Model yang dilatih dengan label terbatas sering gagal digeneralisasi ke domain baru.

**Pertanyaan fundamental:**

> Bagaimana model dapat mempelajari representasi yang bermakna tanpa memerlukan label eksplisit untuk setiap data?

- Self-supervised learning menjawab pertanyaan ini dengan membangun **pretext task** dari data itu sendiri.
- Supervisi datang dari sebagian data yang disembunyikan, diprediksi, atau dibandingkan.

---

# Slide 05 - Perbandingan Paradigma Pembelajaran

| Paradigma | Sumber Supervisi | Contoh | Kelebihan | Keterbatasan |
|---|---|---|---|---|
| Supervised Learning | Label manual | Klasifikasi ImageNet | Akurasi tinggi pada tugas yang sama | Butuh anotasi, kurang transferable |
| Unsupervised Learning | Tidak ada label, hanya struktur data | Clustering, PCA | Tidak butuh label | Sulit dievaluasi, tidak langsung untuk prediksi |
| Self-Supervised Learning | Label dibuat otomatis dari data (pretext task) | Prediksi bagian citra, kontras antar augmentasi | Skalabel, transferable, tanpa anotasi | Desain pretext task kritis, boros komputasi |

- SSL berada di antara supervised dan unsupervised: ia memanfaatkan label palsu yang dihasilkan sendiri.
- Label tersebut tidak diberikan manusia, tetapi diturunkan dari struktur data.

---

# Slide 06 - Definisi Self-Supervised Learning

- Self-supervised learning adalah pendekatan pembelajaran representasi dengan membangun **pretext task** dari data tanpa label.
- Pretext task memaksa model memahami struktur data agar dapat melakukan prediksi terhadap data itu sendiri.
- Setelah pretext task selesai, representasi yang diperoleh digunakan untuk tugas hilir (downstream task) melalui linear probing atau fine-tuning.

```text
Input tanpa label -> Pretext task -> Representasi -> Downstream task
                                      (embedding)      (klasifikasi, deteksi, segmentasi)
```

**Syarat pretext task yang baik:**

- Harus membutuhkan pemahaman semantik, bukan sekadar pola permukaan.
- Harus lebih relevan terhadap downstream task daripada sekadar memecahkan teka-teki.
- Harus dapat dipelajari secara efisien pada skala besar.

---

# Slide 07 - Taksonomi Self-Supervised Learning untuk Citra

```text
Self-Supervised Learning
|
+-- Contrastive Methods
|   +-- SimCLR
|   +-- MoCo
|   +-- DINO (berbasis teacher-student + kontras)
|
+-- Masked Image Modeling
|   +-- MAE
|   +-- SimMIM
|
+-- Non-Contrastive Methods
    +-- BYOL (tanpa negative samples)
    +-- DINOv2 (gabungan beberapa objective)
```

- **Contrastive** menarik representasi positif berdekatan dan menjauhkan dari negatif.
- **Masked image modeling** merekonstruksi bagian citra yang ditutup.
- **Non-contrastive** hanya menggunakan positif, bergantung pada regularisasi atau bootstrap.

---

# Slide 08 - Mengapa SSL Penting untuk Riset Doktoral?

- Hampir semua foundation model modern menggunakan SSL sebagai tahap pretraining.
- Pemahaman SSL memungkinkan mahasiswa mengkritisi bagaimana representasi dibangun.
- Fokus riset dapat diarahkan pada:
  - **Transferability**: bagaimana representasi berpindah ke domain baru.
  - **Efisiensi label**: berapa banyak label yang dibutuhkan untuk mencapai kinerja tertentu.
  - **Robustness**: bagaimana representasi berperilaku terhadap resolusi dan distribusi berbeda.
- SSL membuka peluang **novelty** pada pretext task baru atau kombinasi objective baru.

---

# Slide 09 - Contrastive Learning: Intuisi Dasar

- Ide utama: **augmentasi berbeda dari citra yang sama** harus memiliki representasi yang mirip.
- Satu citra dibuat menjadi dua view melalui augmentasi acak (crop, flipping, color jitter, rotasi).
- Model dilatih agar kedua view tersebut menghasilkan embedding yang dekat, sementara embedding citra berbeda dijauhkan.

```text
        x (citra asli)
         |
   +-----+-----+
   aug1        aug2
   |           |
 view1       view2
   |           |
 encoder    encoder
   |           |
 embedding1  embedding2  ->  d(embedding1, embedding2) kecil
```

- Pasangan positif: dua view dari citra yang sama.
- Pasangan negatif: view dari citra berbeda.

---

# Slide 10 - Contrastive Loss: InfoNCE

- InfoNCE (Info Noise-Contrastive Estimation) adalah fungsi loss yang umum digunakan:

```text
L = -log( exp(sim(z_i, z_j) / tau) /
          sum_{k=1}^{N} exp(sim(z_i, z_k) / tau) )
```

| Simbol | Makna |
|---|---|
| z_i, z_j | Embedding dua view positif dari satu citra |
| z_k | Embedding view lain dalam batch (negatif) |
| sim | Fungsi kesamaan, misalnya cosine similarity |
| tau | Temperature parameter yang mengontrol kekontrasan |

- Loss mendorong kesamaan tinggi antara pasangan positif.
- Semua sampel lain dalam batch berperan sebagai negatif.

---

# Slide 11 - SimCLR: Framework Contrastive Sederhana

- SimCLR (Chen et al., 2020) menggunakan pipeline:

```text
Citra x -> Augmentasi T -> x1, x2 -> Encoder f -> h1, h2 -> Projection g -> z1, z2 -> InfoNCE
```

- **Encoder** f: backbone CNN atau ViT yang menghasilkan embedding.
- **Projection head** g: MLP kecil yang memetakan embedding ke ruang kontras.
- Setelah training, projection head dibuang dan encoder digunakan sebagai representasi.

**Faktor penting dalam SimCLR:**

- Augmentasi yang kuat dan beragam sangat penting.
- Batch size besar dibutuhkan untuk mendapatkan cukup negative samples.
- Normalization pada embedding membantu stabilitas training.

---

# Slide 12 - Momentum Encoder dan Negative Samples

- Contrastive learning membutuhkan banyak negative samples agar representasi tidak runtuh.
- Pendekatan sederhana: memperbesar batch size. Namun, hal ini mahal secara komputasi.
- Solusi: **momentum encoder** digunakan untuk menghasilkan representasi dari sampel yang disimpan dalam queue.

```text
queue (sampel lama) -> momentum encoder -> negative keys
                          |
                          |  (diupdate perlahan)
                          v
sampel saat ini -> online encoder -> query  -> contrastive loss dengan negative keys dari queue
```

- Momentum encoder adalah salinan lambat dari encoder utama.
- Parameter momentum encoder di-update sebagai eksponensial rata-rata dari encoder online.

---

# Slide 13 - MoCo: Momentum Contrast

- MoCo (He et al., 2020) memperkenalkan momentum encoder dan dynamic dictionary.

| Komponen | Keterangan |
|---|---|
| Query encoder | Memproses view positif dari satu sampel |
| Key encoder | Memproses seluruh sampel lain dalam dictionary |
| Dictionary queue | Menyimpan embedding dari mini-batch sebelumnya |
| Momentum update | Parameter key encoder mengikuti query encoder secara perlahan |

- Keuntungan: negative samples dapat berjumlah besar tanpa memerlukan batch size besar.
- Konsistensi representasi pada dictionary dijaga oleh momentum encoder.
- MoCo menjadi dasar banyak metode SSL berbasis kontras selanjutnya.

---

# Slide 14 - Teacher-Student Learning dalam SSL

- Teacher-student learning adalah bentuk knowledge distillation:
  - Model **student** belajar meniru output dari model **teacher**.
  - Teacher dan student dapat berupa arsitektur yang sama tetapi dengan parameter berbeda.
- Dalam konteks SSL, teacher tidak dilatih dengan label, melainkan dengan **momentum** atau **stop-gradient**.
- Student belajar menghasilkan representasi yang konsisten dengan teacher.

```text
Input -> student (backbone + head) -> prediksi
  |
  +----> teacher (momentum backbone + head) -> prediksi target
  student berusaha menyamai target teacher
```

- Loss dihitung antara output student dan output teacher.
- Teacher mengumpulkan pengetahuan secara perlahan dari student.

---

# Slide 15 - DINO: Self-Distillation with No Labels

- DINO (Caron et al., 2021) menggabungkan teacher-student learning dengan objective kontrastif tanpa negative samples.
- Nama DINO merupakan singkatan dari **DIstillation with NO labels**.
- DINO dilatih agar output dari student cocok dengan output teacher untuk semua augmentasi dari citra yang sama.

**Arsitektur DINO:**

```text
x -> view1 (local crop) -> student -> softmax center -> cross-entropy loss
x -> view2 (global crop) -> teacher -> softmax center -> (target)
```

- Teacher di-update dengan momentum dari student.
- Center centering dan sharpening mencegah representasi runtuh.

---

# Slide 16 - Komponen DINO: Centering dan Sharpening

- **Center centering**: mengurangi rata-rata output teacher agar tidak semua data mengarah ke satu cluster.

```text
center_t = mean(output_teacher)  # diperbarui eksponensial
prediksi_teacher = softmax(output_teacher - center_t)
```

- **Sharpening**: meningkatkan kekontrasan distribusi output teacher dengan temperature rendah pada softmax.
- Kombinasi centering dan sharpening menjaga keseimbangan antara **invariance** (posisi sama) dan **semantic clustering** (kelas berbeda terpisah).

| Mekanisme | Fungsi |
|---|---|
| Centering | Mengurangi mode collapse |
| Sharpening | Mendorong prediksi yang tegas |
| Multi-crop | Memperkaya konteks lokal dan global |
| Momentum teacher | Menstabilkan target |

---

# Slide 17 - Emergent Properties pada DINO

- DINO menghasilkan beberapa sifat menarik yang tidak diminta secara eksplisit:

1. **Self-attention maps** menunjukkan segmentasi objek secara implisit.
2. Representasi DINO dapat digunakan untuk segmentasi tanpa label (unsupervised segmentation).
3. Feature bersifat **part-based**: perhatian menyoroti bagian-bagian objek (kepala, kaki, badan).
4. Kinerja transfer ke downstream task sangat kuat, terutama pada model Vision Transformer.

**Pertanyaan penting:**

> Apakah properti ini muncul karena arsitektur ViT, atau karena objective DINO?

- Eksperimen DINO menunjukkan bahwa interaksi antara ViT dan objective SSL menghasilkan properti tersebut.

---

# Slide 18 - Semantic Clustering Otomatis

- Embedding DINO cenderung membentuk cluster yang bermakna semantik tanpa label.
- Dengan k-means sederhana pada embedding DINO, diperoleh pseudo-classes yang koheren.

```text
Embedding DINO -> k-means -> pseudo-label -> evaluasi dengan label sebenarnya
```

- Pseudo-label tersebut tidak sempurna, tetapi cukup berguna untuk:
  - Inisialisasi clustering.
  - Pseudo-label untuk self-training.
  - Analisis struktur data.

**Hubungan dengan riset:**

- Semantic clustering menjadi dasar untuk memahami apakah model belajar konsep objek.
- Kita dapat menguji seberapa baik cluster bertahan ketika data berasal dari domain yang berbeda.

---

# Slide 19 - Masked Image Modeling (MIM)

- Pendekatan lain SSL: menyembunyikan sebagian input dan meminta model memprediksi bagian yang hilang.
- Analogi dengan BERT pada NLP: token sentence ditutup, model memprediksi token yang hilang.
- Pada citra, sebagian patch di-mask, model harus merekonstruksi pixel atau representasi patch tersebut.

```text
Citra asli -> [x1 x2 x3 x4] -> mask x2,x4 -> model -> prediksi x2,x4 -> loss vs asli
```

**Contoh metode MIM:**

| Metode | Prediksi yang dipelajari |
|---|---|
| MAE | Pixel asli pada patch yang di-mask |
| SimMIM | Pixel asli dengan decoder ringan |
| BEiT | Token diskret dari dVAE |

---

# Slide 20 - MAE: Masked Autoencoders

- MAE (He et al., 2022) menggunakan encoder yang hanya melihat patch yang tidak di-mask.
- Patch yang di-mask diprediksi oleh decoder ringan.

```text
Citra -> tokenize patch -> mask 75% patch -> encoder (hanya visible patch)
      -> representasi visible -> decoder -> rekonstruksi semua patch -> MSE loss
```

- Rasio masking tinggi (75%) membuat tugas lebih sulit dan mendorong pemahaman semantik.
- MAE efisien karena encoder hanya memproses 25% patch.
- Representasi yang dihasilkan kuat untuk fine-tuning, tetapi **linear probing** tidak sekuat metode kontrastif.

---

# Slide 21 - Perbandingan Contrastive Learning vs Masked Image Modeling

| Aspek | Contrastive (SimCLR/MoCo/DINO) | Masked Image Modeling (MAE) |
|---|---|---|
| Objective | Menyamakan representasi antar view | Merekonstruksi bagian yang di-mask |
| Informasi yang dipelajari | Invariance terhadap augmentasi | Struktur dan tekstur lokal |
| Linear probing | Kuat | Sedang |
| Fine-tuning | Kuat | Sangat kuat |
| Sifat representasi | Semantik global | Spasial dan tekstural |
| Biaya komputasi | Bergantung pada batch/negatif | Efisien karena masking |

**Implikasi riset:**

- Pilihan metode bergantung pada downstream task yang dituju.
- Kombinasi kontrastif dan MIM dapat saling melengkapi.

---

# Slide 22 - DINOv2: Robust Visual Features without Supervision

- DINOv2 (Oquab et al., 2023) adalah pengembangan dari DINO yang menghasilkan representasi visual universal.
- DINOv2 menggabungkan:
  - Objective contrastif DINO.
  - Masked image modeling (patch-level reconstruction).
  - SwAV loss untuk clustering.
  - Koordinat dan informasi patch untuk membantu lokalisasi.

```text
DINOv2 = DINO loss + iBOT loss (masked) + SwAV loss + Koordinat patch
```

- Model dilatih pada dataset besar yang dikurasi sendiri: **LVD-142M**.

---

# Slide 23 - DINOv2: Strategi Pengumpulan Data

- DINOv2 menggunakan pipeline otomatis untuk membangun dataset besar dari internet:

```text
Web crawl -> deduplikasi -> filtering -> retrieval berbasis embedding -> kurasi -> LVD-142M
```

- **Deduplikasi** menghilangkan citra yang sama atau hampir sama.
- **Filtering** membuang citra berkualitas rendah atau tidak relevan.
- **Retrieval** menggunakan embedding untuk memilih citra yang mirip dengan dataset target.
- Tujuan: membangun dataset yang mencakup distribusi visual dunia secara luas.

**Pelajaran untuk riset:**

- Kualitas kurasi data sama pentingnya dengan arsitektur model.
- Bias pada data akan terekam dalam representasi model.

---

# Slide 24 - Evaluasi Representasi: Linear Probing

- Linear probing adalah protokol standar untuk mengevaluasi kualitas representasi tanpa fine-tuning.
- Model backbone dibekukan, kemudian sebuah **klasifier linear** dilatih pada embedding.

```text
Embedding backbone (frozen) -> Linear classifier -> Prediksi label
```

**Prosedur:**

1. Ekstrak embedding untuk seluruh data training.
2. Latih regresi logistik atau linear layer pada embedding.
3. Evaluasi akurasi pada data test.

**Interpretasi:**

- Akurasi tinggi menunjukkan representasi sudah memisahkan kelas secara linear.
- Tidak ada bobot backbone yang dimodifikasi, sehingga hasil murni mencerminkan kualitas embedding.

---

# Slide 25 - Mengapa Linear Probing Penting?

- Linear probing membedakan **kualitas representasi** dari **kemampuan adaptasi** model.
- Jika fine-tuning bebas dilakukan, model besar selalu dapat menyesuaikan diri, sehingga sulit mengetahui representasi yang sebenarnya dipelajari.
- Linear probing mengungkap:
  - Apakah fitur sudah **linearly separable**.
  - Apakah informasi kelas tersimpan secara eksplisit.
  - Apakah representasi benar-benar semantik atau hanya tekstural.

```text
Akurasi linear probing tinggi -> fitur semantik yang baik.
Akurasi linear probing rendah -> informasi memerlukan transformasi non-linear.
```

- MAE rendah pada linear probing, tinggi pada fine-tuning: representasinya kaya secara spasial tetapi tidak linearly separable.

---

# Slide 26 - Transfer Learning dengan Foundation Vision Models

- Foundation vision model adalah model pretraining berskala besar yang dapat digunakan ulang pada berbagai tugas.
- Transfer learning dilakukan dengan dua mode:

| Mode | Deskripsi | Kapan Digunakan |
|---|---|---|
| Frozen backbone + linear probing | Backbone tidak diubah, hanya klasifier yang dilatih | Jumlah label sedikit, komputasi terbatas |
| Fine-tuning | Seluruh atau sebagian backbone di-update | Dataset cukup besar, tugas berbeda secara domain |

- DINOv2 dirancang agar representasi frozen sudah kuat.
- Namun, domain yang sangat berbeda (misalnya medis, satelit) mungkin tetap membutuhkan fine-tuning.

---

# Slide 27 - Kapan Fine-tuning Diperlukan?

- Tidak ada jawaban universal. Keputusan bergantung pada:

| Faktor | Indikasi Fine-tuning |
|---|---|
| Jumlah label | Banyak label (>10rb) lebih aman untuk fine-tuning |
| Jarak domain | Semakin jauh domain target dari data pretraining, semakin perlu adaptasi |
| Resolusi | Perbedaan resolusi besar dapat membutuhkan penyesuaian positional embedding |
| Komputasi | Fine-tuning model besar mahal; perlu GPU dengan memori cukup |
| Risiko overfitting | Dataset kecil dengan fine-tuning berisiko overfitting |

**Eksperimen yang disarankan:**

- Bandingkan linear probing vs fine-tuning pada berbagai subset label (1%, 10%, 100%).
- Amati kurva akurasi terhadap jumlah label.

---

# Slide 28 - Pertanyaan Kunci: Semantik vs Tekstural

- Apakah representasi yang dipelajari model benar-benar semantik, atau hanya pola tekstural?
- Beberapa studi menemukan bahwa model CNN cenderung sangat bergantung pada tekstur.
- Perilaku DINO dan DINOv2 dapat diuji dengan:

1. **Dataset dengan tekstur berbeda**: citra objek dengan tekstur yang diubah.
2. **Perturbasi frekuensi**: menghilangkan komponen frekuensi tinggi.
3. **Probing dengan atribut**: menguji apakah embedding memisahkan kelas objek atau kelas tekstur.

**Pertanyaan riset:**

> Pada domain tertentu, apakah DINOv2 mengandalkan bentuk objek atau tekstur permukaan?

- Jawaban akan menentukan di mana fine-tuning diperlukan.

---

# Slide 29 - Fokus Riset: Transferability DINO dan DINOv2

- Transferability berarti kemampuan representasi untuk tetap berguna pada data baru.
- Dimensi transferability yang akan dievaluasi:

| Dimensi | Pertanyaan Riset |
|---|---|
| Domain | Apakah representasi yang dilatih pada citra natural berguna pada domain medis, satelit, atau industri? |
| Resolusi | Apakah perubahan resolusi mengubah kualitas embedding? |
| Jumlah label | Berapa label minimum untuk mencapai kinerja yang dapat diterima? |
| Distribusi data baru | Bagaimana representasi berperilaku pada data yang tidak terwakili saat pretraining? |

**Keluaran:**

- Analisis perbandingan DINO vs DINOv2 (misalnya backbone ViT-S vs ViT-B).
- Rekomendasi penggunaannya untuk domain penelitian mahasiswa.

---

# Slide 30 - Desain Eksperimen Transferability

```text
Input domain X / resolusi R
      |
      v
Frozen foundation model (DINO/DINOv2)
      |
      +--> Embedding D_train -> Linear probe -> Akurasi
      |
      +--> Embedding D_test  -> Evaluasi
```

**Protokol eksperimen:**

1. Pilih dataset target yang relevan dengan penelitian.
2. Ekstrak embedding DINO/DINOv2 pada resolusi asli dan bervariasi.
3. Latih linear probe pada subset label: 1%, 10%, 50%, 100%.
4. Ukur akurasi, F1, atau metrik spesifik domain.
5. Bandingkan dengan fitur supervised (misalnya ResNet-50 pretrained ImageNet).

**Kontrol penting:**

- Seed acak, split data, dan konfigurasi optimizer harus dicatat.
- Setidaknya 3 kali ulangan per kondisi untuk estimasi varians.

---

# Slide 31 - Praktikum: Ekstraksi Embedding DINO atau DINOv2

```python
import torch
from torchvision import transforms
from PIL import Image

## Muat backbone DINOv2 dari torch.hub
model = torch.hub.load('facebookresearch/dinov2', 'dinov2_vits14')
model.eval()

transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],
                         std=[0.229, 0.224, 0.225])
])

def extract_embedding(path):
    img = Image.open(path).convert('RGB')
    x = transform(img).unsqueeze(0)
    with torch.no_grad():
        feat = model(x)  # [1, 384] untuk ViT-S
    return feat.squeeze().numpy()
```

- Output `model(x)` adalah embedding global dari `[CLS]` token.
- Untuk DINO, gunakan `['dinov2_vits14']` atau `['dino_vits16']` dari torch.hub.

---

# Slide 32 - Praktikum: Visualisasi Embedding dengan Reduksi Dimensi

```python
import numpy as np
from sklearn.decomposition import PCA
from sklearn.manifold import TSNE
import matplotlib.pyplot as plt

## X: matriks embedding (n_samples, dim), y: label
pca = PCA(n_components=50, random_state=0)
X_pca = pca.fit_transform(X)

tsne = TSNE(n_components=2, random_state=0, perplexity=30)
X_2d = tsne.fit_transform(X_pca)

plt.figure(figsize=(8, 6))
scatter = plt.scatter(X_2d[:, 0], X_2d[:, 1], c=y, cmap='tab10', s=5, alpha=0.7)
plt.colorbar(scatter)
plt.title('Visualisasi Embedding dengan t-SNE')
plt.show()
```

- Gunakan PCA terlebih dahulu untuk mengurangi dimensi sebelum t-SNE.
- Perhatikan apakah embedding membentuk cluster yang sesuai dengan kelas.

---

# Slide 33 - Praktikum: Linear Probing

```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

## X: embedding, y: label
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=0, stratify=y
)

clf = LogisticRegression(max_iter=1000, C=1.0)
clf.fit(X_train, y_train)

y_pred = clf.predict(X_test)
acc = accuracy_score(y_test, y_pred)
print(f'Akurasi linear probing: {acc:.4f}')
```

- Pada subset label kecil, gunakan `class_weight='balanced'`.
- Bandingkan dengan fitur terakhir sebelum classifier pada model supervised.

---

# Slide 34 - Perbandingan Fitur Supervised vs Self-Supervised

| Aspek | Fitur Supervised (ImageNet) | Fitur Self-Supervised (DINOv2) |
|---|---|---|
| Sumber label | 1,2 juta label ImageNet | Tanpa label (LVD-142M) |
| Transferibility umum | Kuat pada domain natural | Lebih umum dan robust |
| Linear probing | Baik | Sangat baik |
| Fine-tuning | Baik | Sangat baik |
| Sensitivitas domain | Cenderung terbatas | Lebih adaptif, tetapi tetap ada bias |
| Biaya pretraining | Besar | Lebih besar |

**Praktik yang dianjurkan:**

- Jangan berasumsi bahwa supervised selalu kalah atau menang.
- Selalu uji keduanya pada dataset yang sama dan laporkan interval kepercayaan.

---

# Slide 35 - Analisis Hasil dan Interpretasi

- Setelah eksperimen selesai, buat tabel hasil:

| Model | Resolusi | % Label | Akurasi | Std Dev |
|---|---|---|---|---|
| DINO ViT-S | 224 | 1% | 72,1 | 1,2 |
| DINO ViT-S | 224 | 10% | 81,4 | 0,8 |
| DINOv2 ViT-S | 224 | 1% | 76,8 | 0,9 |
| DINOv2 ViT-S | 224 | 10% | 84,2 | 0,7 |

**Pertanyaan interpretasi:**

- Apakah selisih akurasi signifikan secara statistik?
- Pada kelas mana model gagal?
- Apakah error berubah ketika resolusi diturunkan?
- Apakah representasi DINOv2 lebih tahan terhadap perubahan domain?

---

# Slide 36 - Rekomendasi Penggunaan Foundation Vision Model

Berdasarkan hasil eksperimen, buat rekomendasi:

- **Jika domain target dekat dengan data natural**:
  - DINOv2 frozen + linear probe sudah cukup untuk banyak kasus.
- **Jika domain target sangat jauh** (misalnya histopatologi, CT, atau SAR):
  - Evaluasi apakah fine-tuning pada sebagian layer diperlukan.
  - Uji juga apakah augmentasi khusus domain membantu.
- **Jika data label sangat sedikit**:
  - Prioritaskan linear probing dengan representasi DINOv2 daripada fine-tuning.
- **Jika resolusi berbeda jauh**:
  - Perhatikan interpolasi positional embedding dan uji beberapa resolusi.

> Rekomendasi harus berbasis bukti eksperimen, bukan sekadar preferensi.

---

# Slide 37 - Aktivitas Seminar Paper dan Diskusi Pretext Task

**Aktivitas kelas:**

1. Seminar paper: dua paper utama DINO dan DINOv2.
   - Bedakan kontribusi, metode, eksperimen, dan klaim.
   - Identifikasi keterbatasan yang tidak dibahas paper.
2. Diskusi desain pretext task untuk dataset penelitian mahasiswa.
   - Apa pretext task yang paling sesuai untuk domain Anda?
   - Mengapa?

**Pertanyaan diskusi:**

- Apakah objective pretraining harus cocok dengan downstream task?
- Apakah representasi yang dilatih untuk satu domain dapat dipakai lintas domain?
- Bagaimana cara mengukur representasi yang "semantik" tanpa label?

---

# Slide 38 - Target Keluaran dan Tugas

**Tugas praktikum:**

1. Ekstraksi embedding DINO atau DINOv2 pada dataset pilihan.
2. Visualisasi embedding dengan reduksi dimensi.
3. Evaluasi linear probing dengan variasi jumlah label.
4. Perbandingan dengan fitur supervised.

**Laporan yang harus diserahkan:**

- Deskripsi dataset dan domain.
- Kode notebook yang reproducible.
- Tabel hasil lintas kondisi.
- Interpretasi: apakah representasi DINOv2 transferable ke domain Anda?
- Rekomendasi penggunaan foundation model untuk riset Anda.

**Target keluaran pertemuan:**

- Analisis transferability dan rekomendasi penggunaan foundation vision model untuk satu domain penelitian.

---

# Slide 39 - Checklist Praktikum

- [ ] Dataset diunduh dan dibagi secara stratifikasi.
- [ ] Batch embedding diekstrak untuk beberapa resolusi.
- [ ] Visualisasi t-SNE/UMAP dibuat dan diamati.
- [ ] Linear probe dilatih pada 1%, 10%, 50%, 100% label.
- [ ] Baseline supervised (ResNet/ImageNet) diekstrak.
- [ ] Tabel hasil dan error analysis disusun.
- [ ] Random seed dan environment dicatat.
- [ ] Interpretasi dan rekomendasi ditulis dengan argumen berbasis bukti.

Jika checklist selesai, mahasiswa siap mempresentasikan hasil pada diskusi kelas.

---

# Slide 40 - Hubungan dengan Pertemuan Berikutnya

- Representasi yang dipelajari pada pertemuan ini akan menjadi modalitas visual bagi model multimodal.
- Pertemuan 05: **Vision-Language Models dan Multimodal Representation** menggunakan CLIP untuk menyelaraskan citra dan teks.
- CLIP menggunakan contrastive learning seperti SimCLR, tetapi pasangan positifnya adalah citra dan teks yang sesuai.
- Memahami SSL memudahkan analisis mengapa CLIP memiliki sifat zero-shot.

**Arah lanjutan:**

- Pada pertemuan selanjutnya, mahasiswa akan menguji apakah representasi visual dari DINOv2 dapat dikombinasikan dengan representasi teks.
- Pilih paper dan siapkan pertanyaan kritis untuk diskusi.

---

# Slide 41 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Vision-Language Models dan Multimodal Representation**