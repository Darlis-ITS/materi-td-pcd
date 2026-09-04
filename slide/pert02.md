# Slide 00 - Cover

EF256129 - TD PCD  
Pertemuan 02

# Fondasi Representasi Visual: CNN, Transformer, dan Dataset Benchmark

Dr. Darlis Herumurti  
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan dalam Perkuliahan

## Peta Perjalanan Awal

| Pertemuan | Fokus | Pertanyaan Utama |
|---|---|---|
| 1 | Peta riset, audit dataset, dan baseline klasik | Apa masalah dan titik awal yang layak? |
| 2 | Fondasi CNN, Transformer, dan dataset benchmark | Bagaimana model mempelajari representasi visual? |
| 3 | CNN modern, attention, dan Vision Transformer | Bagaimana memilih dan membandingkan arsitektur modern? |
| 4 | Self-supervised learning dan foundation vision models | Bagaimana belajar representasi dengan label terbatas? |

## Posisi Pertemuan 02

Pertemuan ini menjadi jembatan dari **handcrafted feature** menuju **learned representation**.

---

# Slide 02 - Recap Pertemuan 01

## Yang Sudah Dipelajari

- Perbedaan pengolahan citra digital dan computer vision.
- Evolusi dari image processing klasik menuju deep learning dan foundation models.
- Pentingnya audit dataset, benchmark, baseline, dan reproducibility.
- Pipeline klasik: preprocessing → handcrafted feature → classifier.
- Failure analysis sebagai sumber hipotesis dan research question.

## Praktikum 01

- Eksplorasi dataset `digits`.
- Baseline majority class.
- Histogram intensitas + kNN.
- HOG + Linear SVM.
- Confusion matrix dan analisis salah klasifikasi.

---

# Slide 03 - Celah antara Pertemuan 01 dan 03

## Pertemuan 01 Berakhir pada Pertanyaan

> Jika handcrafted feature terbatas, bagaimana fitur dapat dipelajari langsung dari data?

## Pertemuan 03 Memerlukan Fondasi

- operasi convolution dan feature map;
- pooling dan receptive field;
- residual connection;
- token dan embedding;
- query, key, dan value;
- self-attention dan multi-head attention;
- positional encoding;
- patch tokenization.

## Fungsi Pertemuan 02

Membuka isi “kotak hitam” CNN dan Transformer sebelum menggunakan ResNet, DeiT, dan ViT.

---

# Slide 04 - Tujuan Pembelajaran Pertemuan 02

Setelah mengikuti pertemuan ini, mahasiswa mampu:

- menjelaskan citra sebagai tensor;
- menjelaskan bagaimana convolution membentuk feature map;
- menganalisis stride, padding, pooling, dan receptive field;
- menjelaskan alur training CNN dari nol;
- menjelaskan fungsi residual connection;
- menghitung mekanisme dasar self-attention;
- menjelaskan token, embedding, multi-head attention, dan posisi;
- menjelaskan bagaimana citra dapat diubah menjadi patch token;
- memilih dataset berdasarkan task, anotasi, domain, dan research question;
- mengaudit bias, leakage, lisensi, serta keterbatasan benchmark.

---

# Slide 05 - Pertanyaan Kunci

## Representasi

- Apa yang sebenarnya dipelajari CNN dari piksel?
- Mengapa kernel CNN tidak sama dengan filter manual?
- Bagaimana hubungan lokal berkembang menjadi representasi objek?

## Transformer

- Mengapa citra perlu diubah menjadi token?
- Bagaimana satu token memilih informasi dari token lain?
- Mengapa Transformer memerlukan positional encoding?

## Dataset

- Dataset populer mana yang sesuai untuk classification, detection, atau segmentation?
- Apakah benchmark populer otomatis sesuai untuk research question kita?

---

# Slide 06 - Citra adalah Tensor, Bukan Sekadar Gambar

## Representasi Citra RGB

```text
Channel × Height × Width
3 × H × W
```

## Representasi Satu Batch

```text
Batch × Channel × Height × Width
B × 3 × H × W
```

## Nilai Piksel

- Citra 8-bit umumnya memiliki nilai 0–255.
- `ToTensor()` biasanya mengubahnya menjadi bilangan float 0–1.
- Normalisasi menggeser dan menskalakan distribusi input.

## Pesan Utama

Semua operasi CNN dan Transformer pada akhirnya bekerja pada tensor numerik.

---

# Slide 07 - Dari Filter Manual ke Kernel yang Dipelajari

## Pada Image Processing Klasik

Kernel ditentukan manusia:

- Gaussian blur;
- Sobel edge detector;
- sharpening;
- Laplacian.

## Pada CNN

- Nilai kernel dimulai dari inisialisasi tertentu.
- Backpropagation memperbarui nilai kernel.
- Kernel dipelajari untuk menurunkan loss tugas target.

## Perubahan Paradigma

```text
Manusia menentukan fitur
          ↓
Model mempelajari fitur dari data dan objective
```

---

# Slide 08 - Operasi Convolution

Kernel digeser di atas citra dan menghasilkan feature map.

```text
Input patch × Kernel
       ↓
Perkalian elemen dan penjumlahan
       ↓
Satu nilai pada feature map
```

## Persamaan Sederhana

```text
Y[i,j] = Σm Σn X[i+m,j+n] W[m,n] + b
```

## Weight Sharing

Kernel yang sama digunakan di seluruh posisi sehingga:

- jumlah parameter lebih kecil;
- pola dapat dikenali pada lokasi berbeda;
- model memiliki inductive bias spasial.

---

# Slide 09 - Channel Input dan Output

## Contoh Layer

```python
nn.Conv2d(
    in_channels=3,
    out_channels=32,
    kernel_size=3,
    padding=1
)
```

## Interpretasi

- Tiga input channel berasal dari RGB.
- Terdapat 32 kernel yang dipelajari.
- Setiap kernel menghasilkan satu output channel.
- Output menjadi 32 feature map.

```text
B × 3 × 32 × 32
        ↓ Conv2d
B × 32 × 32 × 32
```

---

# Slide 10 - Stride dan Padding Mengatur Resolusi

## Ukuran Output

```text
O = floor((I + 2P - K) / S) + 1
```

| Simbol | Makna |
|---|---|
| `I` | Ukuran input |
| `K` | Ukuran kernel |
| `P` | Padding |
| `S` | Stride |
| `O` | Ukuran output |

## Contoh

| Input | Kernel | Padding | Stride | Output |
|---:|---:|---:|---:|---:|
| 32 | 3 | 0 | 1 | 30 |
| 32 | 3 | 1 | 1 | 32 |
| 32 | 3 | 1 | 2 | 16 |

---

# Slide 11 - Aktivasi Membuat Model Nonlinear

## Tanpa Aktivasi

Beberapa transformasi linear yang disusun tetap ekuivalen dengan satu transformasi linear.

## ReLU

```text
ReLU(x) = max(0, x)
```

## Fungsi

- menambahkan non-linearitas;
- memungkinkan model mempelajari pola kompleks;
- sederhana dan efisien;
- membantu optimasi dibandingkan aktivasi jenuh tertentu.

## Catatan

Aktivasi bukan sekadar langkah tambahan. Tanpa non-linearitas, kedalaman jaringan kehilangan sebagian besar manfaat representasionalnya.

---

# Slide 12 - Pooling Mereduksi Resolusi

## Max Pooling

Memilih nilai terbesar pada suatu wilayah.

```text
Feature map 32 × 32
       ↓ MaxPool 2 × 2
Feature map 16 × 16
```

## Tujuan

- mengurangi ukuran spasial;
- mengurangi biaya komputasi;
- memperbesar receptive field efektif;
- memberikan toleransi terhadap pergeseran kecil.

## Trade-off

Reduksi terlalu agresif dapat menghilangkan objek kecil dan detail lokasi.

---

# Slide 13 - Hierarki Feature Map

## Lapisan Awal

- tepi;
- orientasi;
- perubahan intensitas;
- warna dan tekstur sederhana.

## Lapisan Menengah

- pola tekstur lebih kompleks;
- sudut;
- bagian objek;
- kombinasi fitur lokal.

## Lapisan Dalam

- konfigurasi bagian;
- bentuk tingkat tinggi;
- fitur yang lebih terkait dengan kelas atau tugas.

## Catatan

Hierarki ini merupakan kecenderungan umum, bukan aturan bahwa setiap channel selalu memiliki arti tunggal yang mudah dinamai.

---

# Slide 14 - Receptive Field Membesar Bertahap

## Definisi

Receptive field adalah wilayah input yang dapat memengaruhi satu aktivasi.

```text
Layer awal  → melihat wilayah lokal
Layer tengah → menggabungkan beberapa wilayah lokal
Layer dalam → menerima konteks yang lebih luas
```

## Implikasi

- CNN tidak langsung menghubungkan semua lokasi citra.
- Hubungan jarak jauh memerlukan beberapa lapisan.
- Pooling atau stride mempercepat pertumbuhan receptive field.
- Receptive field teoretis tidak selalu sama dengan pengaruh efektif aktual.

---

# Slide 15 - Arsitektur Simple CNN

```text
Input RGB
  ↓
Conv → BatchNorm → ReLU → Pool
  ↓
Conv → BatchNorm → ReLU → Pool
  ↓
Conv → BatchNorm → ReLU → Pool
  ↓
Global Average Pooling
  ↓
Linear Classifier
  ↓
Probabilitas Kelas
```

## Tujuan Praktikum

- melihat perubahan bentuk tensor;
- mengamati learning curve;
- memvisualisasikan feature map;
- membedakan handcrafted feature dan learned feature.

---

# Slide 16 - Model Belajar dari Loss

## Training Loop

```text
Input + Label
    ↓
Forward pass
    ↓
Prediction
    ↓
Hitung loss
    ↓
Backpropagation
    ↓
Update parameter
```

## Cross-Entropy

```text
L = -log p(y | x)
```

Model mendapat penalti besar ketika probabilitas kelas yang benar rendah.

---

# Slide 17 - Training, Validation, dan Test Berbeda Fungsi

| Split | Fungsi | Boleh Memengaruhi Model? |
|---|---|---|
| Training | Memperbarui parameter | Ya |
| Validation | Memilih epoch dan hyperparameter | Tidak langsung melalui gradien |
| Test | Estimasi akhir generalisasi | Tidak |

## Risiko Umum

- memilih model berdasarkan test accuracy;
- mencoba banyak konfigurasi pada test set;
- melakukan preprocessing sebelum split;
- membiarkan sampel dari subjek yang sama tersebar antarsplit.

## Prinsip

Test set bukan validation set tambahan.

---

# Slide 18 - Augmentasi Membawa Asumsi

## Contoh

- horizontal flip;
- random crop;
- rotasi;
- color jitter;
- blur atau noise.

## Tujuan

- menambah variasi tampilan;
- mengurangi overfitting;
- menyatakan invariance yang diharapkan.

## Peringatan

Augmentasi yang valid bergantung pada domain:

- flip mungkin tidak valid untuk tulisan;
- rotasi besar mungkin tidak valid untuk citra medis tertentu;
- perubahan warna dapat merusak informasi diagnostik.

---

# Slide 19 - Residual Connection Membantu Jaringan Dalam

## Masalah

Jaringan lebih dalam tidak otomatis lebih mudah dilatih. Optimasi dapat mengalami degradation problem.

## Residual Block

```text
y = F(x) + x
```

## Fungsi Identity Path

- mempermudah aliran gradien;
- memungkinkan blok belajar koreksi terhadap input;
- memudahkan pembelajaran fungsi identitas;
- menjadi fondasi ResNet.

## Jembatan ke Pertemuan 03

ResNet-18 dan ResNet-50 menggunakan prinsip ini dalam skala lebih besar.

---

# Slide 20 - Inductive Bias CNN

## Asumsi Bawaan

- **Lokalitas:** tetangga spasial dianggap penting.
- **Weight sharing:** pola yang sama dapat muncul di lokasi berbeda.
- **Translasi ekuivariansi:** pergeseran input menggeser feature map.
- **Hierarki:** pola kompleks tersusun dari pola sederhana.

## Konsekuensi

- relatif data-efficient;
- cocok untuk banyak citra alami;
- baseline kuat pada dataset kecil dan menengah;
- hubungan global dibangun secara bertahap.

---

# Slide 21 - Mengapa Kita Memerlukan Mekanisme Global?

## Contoh Hubungan Jarak Jauh

- bagian objek yang terpisah;
- konteks objek dan lingkungan;
- struktur global pada citra medis;
- pola wilayah luas pada citra satelit;
- hubungan antarelemen dokumen.

## Keterbatasan Jalur Lokal

CNN dapat menangkap hubungan global, tetapi informasi harus melewati beberapa lapisan.

## Gagasan Transformer

Mengizinkan setiap elemen melihat elemen lain secara langsung melalui self-attention.

---

# Slide 22 - Transformer Bekerja pada Token

## Bentuk Input

```text
Batch × Jumlah Token × Dimensi Embedding
B × N × D
```

## Token

Unit informasi yang diproses Transformer.

## Embedding

Vektor numerik berdimensi `D` yang merepresentasikan sebuah token.

## Dalam Vision

Token dapat berasal dari:

- patch citra;
- region atau proposal objek;
- feature map CNN;
- token multimodal.

---

# Slide 23 - Query, Key, dan Value

```text
Q = XWQ
K = XWK
V = XWV
```

## Intuisi

| Komponen | Pertanyaan Intuitif |
|---|---|
| Query | Informasi apa yang sedang saya cari? |
| Key | Informasi seperti apa yang saya miliki? |
| Value | Informasi apa yang akan saya kirim? |

## Pesan Penting

Q, K, dan V bukan metadata yang diberikan manusia. Ketiganya merupakan proyeksi yang dipelajari.

---

# Slide 24 - Scaled Dot-Product Attention

```text
Attention(Q,K,V) = softmax(QKᵀ / √dk) V
```

## Langkah

1. Bandingkan query dengan seluruh key.
2. Skala skor dengan `√dk`.
3. Normalisasi menggunakan softmax.
4. Gunakan bobot untuk menggabungkan value.

## Interpretasi Attention Matrix

- Setiap baris mewakili satu query.
- Setiap kolom mewakili key yang diperhatikan.
- Jumlah bobot pada satu baris adalah satu.
- Hubungan bersifat terarah dan tidak harus simetris.

---

# Slide 25 - Mengapa Skor Perlu Diskalakan?

Ketika dimensi key besar, dot product cenderung memiliki magnitudo lebih besar.

## Tanpa Skala

- softmax dapat menjadi sangat tajam;
- sebagian probabilitas mendekati nol atau satu;
- gradien menjadi kurang informatif;
- training lebih sulit distabilkan.

## Dengan `√dk`

Skala skor lebih terkendali sebelum masuk softmax.

## Catatan

Scaling bukan mekanisme untuk menentukan token penting. Scaling menjaga stabilitas numerik dan optimasi.

---

# Slide 26 - Multi-Head Attention Menangkap Banyak Hubungan

## Mekanisme

- Embedding dibagi menjadi beberapa head.
- Setiap head memiliki proyeksi Q, K, dan V.
- Hasil head digabungkan dan diproyeksikan kembali.

```text
Input
 ├─ Head 1 → hubungan tertentu
 ├─ Head 2 → hubungan lain
 ├─ Head 3 → hubungan lain
 └─ Head 4 → hubungan lain
       ↓
Concatenate → Linear Projection
```

## Peringatan

Tidak setiap head otomatis memiliki fungsi yang mudah diinterpretasikan.

---

# Slide 27 - Posisi Tidak Tersedia Secara Bawaan

Self-attention memproses hubungan antartoken, tetapi tidak mengetahui urutan spasial tanpa informasi tambahan.

## Positional Encoding

- sinusoidal;
- learnable positional embedding;
- relative positional bias;
- rotary atau variasi lain.

## Dalam Citra

Posisi membantu membedakan:

- patch kiri dan kanan;
- bagian atas dan bawah;
- kedekatan spasial;
- struktur susunan objek.

---

# Slide 28 - Transformer Encoder Block

```text
Input Token
    ↓
LayerNorm
    ↓
Multi-Head Self-Attention
    ↓
Add Residual
    ↓
LayerNorm
    ↓
MLP + GELU
    ↓
Add Residual
    ↓
Output Token
```

## Persamaan dengan CNN Modern

- residual connection;
- normalization;
- tumpukan beberapa blok;
- representasi semakin abstrak.

---

# Slide 29 - Patch Menghubungkan Citra dan Transformer

## Contoh

Citra 32×32 dibagi menjadi patch 4×4:

```text
(32 / 4) × (32 / 4) = 8 × 8 = 64 patch
```

## Patch Embedding

```text
Citra
→ Potong menjadi patch
→ Flatten setiap patch
→ Linear projection
→ Patch token
```

Implementasi efisien dapat menggunakan `Conv2d` dengan kernel dan stride sebesar patch size.

## Jembatan

Praktikum 03 menggunakan konsep yang sama pada DeiT dan Vision Transformer modern.

---

# Slide 30 - Tiny Image Transformer sebagai Model Pendidikan

## Komponen

- patch embedding;
- token `[CLS]`;
- learnable positional embedding;
- beberapa Transformer encoder block;
- LayerNorm;
- linear classifier.

## Alur

```text
Image → Patch Tokens
      → [CLS] + Position
      → Transformer Blocks
      → [CLS] Representation
      → Class Prediction
```

## Batasan

Model kecil ini digunakan untuk memahami mekanisme, bukan mereproduksi keseluruhan strategi training ViT atau DeiT.

---

# Slide 31 - CNN dan Transformer Membawa Bias Berbeda

| Aspek | CNN | Transformer |
|---|---|---|
| Unit awal | Piksel dan tetangga lokal | Token |
| Operasi utama | Convolution | Self-attention |
| Bias lokal | Kuat | Lebih lemah |
| Relasi global | Bertahap | Langsung antartoken |
| Informasi posisi | Tersirat pada grid | Perlu encoding/bias posisi |
| Normalisasi umum | BatchNorm | LayerNorm |
| Kebutuhan data dari nol | Relatif lebih kecil | Umumnya lebih besar |

## Pertanyaan Penelitian

Apakah perbedaan hasil berasal dari arsitektur, data, pretraining, atau strategi training?

---

# Slide 32 - Dataset adalah Bagian dari Metode

Dataset menentukan:

- fenomena apa yang dapat dipelajari;
- label dan task yang dapat dievaluasi;
- populasi yang direpresentasikan;
- jenis bias yang mungkin masuk;
- klaim generalisasi yang dapat dibuat;
- benchmark pembanding yang tersedia.

## Prinsip

> Dataset bukan sekadar bahan bakar model; dataset membatasi arti hasil penelitian.

Model yang unggul pada satu benchmark belum tentu unggul pada domain atau distribution shift lain.

---

# Slide 33 - Anatomi Dataset Computer Vision

| Komponen | Pertanyaan Audit |
|---|---|
| Unit data | Satu citra, video, frame, subjek, atau studi? |
| Input | RGB, grayscale, multispektral, depth, atau multimodal? |
| Label | Kelas, bounding box, mask, keypoint, caption, atau pair? |
| Granularitas | Image-level, object-level, pixel-level, atau sequence-level? |
| Split | Acak, subject-wise, temporal, geographical, atau cross-domain? |
| Sumber | Web, sensor, klinik, satelit, laboratorium, atau simulasi? |
| Lisensi | Boleh untuk riset, komersial, redistribusi, atau terbatas? |
| Risiko | Bias, privasi, duplikasi, shortcut, dan leakage? |

---

# Slide 34 - Dataset Low-Level Image Processing

| Dataset | Fokus | Penggunaan Umum |
|---|---|---|
| BSD500/BSDS500 | Natural images + boundary annotation | Edge detection dan segmentation |
| DIV2K | 1.000 citra resolusi 2K | Super-resolution dan restoration |
| Set5 dan Set14 | Test set berukuran kecil | Benchmark klasik super-resolution |
| BSD100 dan Urban100 | Natural/urban structures | Evaluasi super-resolution |
| SIDD | Pasangan noisy-clean dari kamera ponsel | Real image denoising |
| GoPro | Pasangan blurred-sharp | Motion deblurring |
| REDS | Video resolusi tinggi | Deblurring, super-resolution, dan restoration |

## Catatan

Benchmark kecil seperti Set5 bukan data training yang memadai; fungsinya terutama sebagai test set pembanding.

---

# Slide 35 - Dataset Image Quality dan Restoration

| Dataset | Jenis Data | Task |
|---|---|---|
| Kodak24 | Citra natural berkualitas tinggi | Compression dan restoration evaluation |
| LIVE IQA | Distorsi sintetis + human score | Full-reference image quality assessment |
| TID2013 | Banyak jenis dan tingkat distorsi | Image quality assessment |
| KADID-10k | Distorsi sintetis berskala lebih besar | IQA dan perceptual quality |
| DPED | Foto perangkat berbeda | Photo enhancement |
| LOL/LOL-v2 | Pasangan low-light dan normal-light | Low-light enhancement |

## Pemilihan Metrik

- PSNR dan SSIM mengukur kesamaan tertentu terhadap reference.
- LPIPS dan perceptual metrics mencoba mendekati kesamaan representasional.
- Human evaluation tetap penting ketika kualitas perseptual menjadi klaim utama.

---

# Slide 36 - Dataset Fondasi dan Klasifikasi

| Dataset | Karakteristik | Umum Digunakan untuk |
|---|---|---|
| MNIST | 70 ribu digit grayscale 28×28 | Pengenalan digit dan pembelajaran awal |
| Fashion-MNIST | 70 ribu citra pakaian grayscale | Baseline klasifikasi sederhana |
| CIFAR-10 | 60 ribu citra 32×32, 10 kelas | CNN kecil, augmentasi, robustness |
| CIFAR-100 | 60 ribu citra, 100 kelas | Klasifikasi lebih granular |
| ImageNet-1K | Sekitar 1,28 juta train, 1.000 kelas | Pretraining dan benchmark klasifikasi |

## Catatan

- MNIST dan CIFAR cocok untuk pembelajaran serta debugging.
- ImageNet berpengaruh besar dalam perkembangan deep visual representation.
- Dataset kecil tidak cukup untuk menyimpulkan kinerja pada citra dunia nyata secara luas.

---

# Slide 37 - Dataset Detection dan Segmentation

| Dataset | Anotasi Utama | Penggunaan |
|---|---|---|
| PASCAL VOC 2007/2012 | Kelas, bounding box, segmentation | Benchmark klasik detection/segmentation |
| MS COCO | Box, instance mask, keypoint, caption | Detection, instance segmentation, pose, captioning |
| Open Images | Label citra, box, relation, mask pada subset | Detection skala besar dan long-tail |
| ADE20K | Semantic mask berbagai scene | Semantic segmentation |

## Perbedaan Task

- Classification: apa yang ada dalam citra?
- Detection: apa dan di mana objek berada?
- Semantic segmentation: kelas apa pada setiap piksel?
- Instance segmentation: piksel mana milik setiap objek individual?

---

# Slide 38 - Dataset Scene, Street, dan Autonomous Vision

| Dataset | Fokus | Catatan |
|---|---|---|
| Cityscapes | Street scene perkotaan | Semantic/instance segmentation dan disparity |
| KITTI | Kendaraan dan jalan | Stereo, optical flow, detection, tracking |
| BDD100K | Video berkendara beragam | Detection, lane, segmentation, tracking |
| nuScenes | Multi-sensor autonomous driving | Kamera, lidar, radar, 3D detection/tracking |
| Waymo Open Dataset | Sensor kendaraan skala besar | Perception dan motion research |

## Risiko Evaluasi

- kondisi kota dan cuaca tidak seimbang;
- frame berdekatan dapat bocor antarsplit;
- model dapat gagal ketika berpindah negara, sensor, atau musim.

---

# Slide 39 - Dataset Medis dan Biomedical Vision

| Dataset | Modalitas atau Task | Catatan Penting |
|---|---|---|
| ISIC Archive | Dermoscopy dan lesi kulit | Classification/segmentation; distribusi populasi perlu diaudit |
| CheXpert | Chest radiograph | Multi-label findings dan label uncertainty |
| MIMIC-CXR | Chest radiograph + report | Memerlukan credentialing dan kepatuhan penggunaan |
| BraTS | MRI tumor otak | Segmentasi tumor multisequence |
| CAMELYON | Histopathology | Deteksi metastasis pada whole-slide image |

## Prinsip Split

Gunakan **patient-wise split**, bukan random image split, agar citra pasien yang sama tidak masuk ke train dan test.

## Klaim

Kinerja benchmark bukan pengganti validasi klinis.

---

# Slide 40 - Dataset Remote Sensing dan Earth Observation

| Dataset | Sumber atau Anotasi | Penggunaan |
|---|---|---|
| EuroSAT | Sentinel-2, 10 kelas | Land-use/land-cover classification |
| BigEarthNet | Patch Sentinel multispektral | Multi-label land-cover representation |
| SpaceNet | Citra satelit + anotasi geospasial | Bangunan, jalan, dan mapping |
| xView | Citra resolusi tinggi + bounding box | Deteksi objek skala kecil |
| LoveDA | Urban dan rural scenes | Domain adaptation dan segmentation |

## Risiko Khusus

- spatial leakage antartile berdekatan;
- perbedaan sensor dan resolusi;
- perubahan musim dan waktu;
- ketidakseimbangan wilayah geografis.

---

# Slide 41 - Dataset Manusia, Video, Dokumen, dan Multimodal

## Wajah dan Aktivitas Manusia

- LFW dan CelebA: face verification, attributes, dan representation.
- COCO Keypoints dan MPII: human pose estimation.
- Market-1501: person re-identification.

## Video

- UCF101 dan HMDB51: action recognition skala pendidikan.
- Kinetics: pretraining dan benchmark action recognition.
- Something-Something: interaksi objek dan pemahaman temporal.

## Dokumen

- RVL-CDIP: document image classification.
- DocVQA: visual question answering pada dokumen.

## Multimodal

- Conceptual Captions, LAION, dan DataComp: image-text representation.

## Wajib Dipertimbangkan

Privasi, consent, bias demografis, hak cipta, lisensi, dan konten berbahaya.

---

# Slide 42 - Memilih Dataset dari Research Question

## Mulai dari Pertanyaan, Bukan Popularitas

| Research Question | Kebutuhan Dataset |
|---|---|
| Apakah model mengenali kelas? | Image-level label |
| Di mana objek berada? | Bounding box atau mask |
| Bagaimana model menghadapi domain shift? | Beberapa domain/sensor/lokasi |
| Bagaimana kinerja pada data terbatas? | Kurva terhadap ukuran label |
| Apakah model robust terhadap corruption? | Clean + corruption protocol |
| Apakah model adil lintas kelompok? | Metadata kelompok yang sah dan etis |
| Apakah representasi dapat ditransfer? | Pretraining dan downstream dataset |

## Prinsip

Dataset harus memungkinkan research question dijawab secara valid.

---

# Slide 43 - Audit Dataset Sebelum Training

## Audit Minimum

- jumlah sampel dan distribusi kelas;
- resolusi, channel, dan format;
- sampel visual per kelas;
- missing atau corrupted files;
- duplikasi dan near-duplicate;
- statistik intensitas dan warna;
- kualitas label;
- metadata sumber;
- lisensi dan batas penggunaan;
- hubungan antarunit data.

## Hubungan dengan Praktikum 01

Audit data harus dilakukan sebelum mencoba arsitektur yang lebih kompleks.

---

# Slide 44 - Data Leakage Membuat Hasil Tampak Lebih Baik

## Bentuk Leakage

- subjek yang sama berada pada train dan test;
- frame video berdekatan tersebar antarsplit;
- tile citra satelit bertetangga masuk ke split berbeda;
- augmentasi atau turunan citra masuk ke test;
- normalisasi dihitung menggunakan seluruh data;
- test set dipakai berulang untuk memilih model.

## Split yang Sesuai

| Domain | Split yang Disarankan |
|---|---|
| Medis | Patient-wise |
| Video | Video/subject-wise |
| Satelit | Geographic/region-wise |
| Temporal | Time-based |
| Multi-center | Site/hospital-wise |

---

# Slide 45 - Benchmark Dapat Mendorong Kesimpulan Keliru

## Masalah Umum

- mengejar perbedaan akurasi sangat kecil;
- test set menjadi target optimasi komunitas;
- dataset tidak mewakili kondisi deployment;
- label tidak menangkap seluruh fenomena;
- model memanfaatkan shortcut atau background;
- biaya komputasi dan pretraining tidak diperhitungkan.

## Pertanyaan Kritis

- Apakah baseline cukup kuat?
- Apakah perbandingan menggunakan data dan pretraining setara?
- Apakah hasil stabil pada beberapa seed?
- Apakah ada external validation atau distribution shift?
- Apakah klaim dibatasi oleh karakteristik dataset?

---

# Slide 46 - Praktikum 02: Dari Mekanisme ke Eksperimen

## Bagian A — Fondasi CNN

- citra sebagai tensor;
- convolution manual;
- stride, padding, pooling, dan receptive field;
- SimpleCNN;
- residual block;
- feature-map visualization.

## Bagian B — Fondasi Transformer

- token dan embedding;
- Q, K, dan V;
- attention matrix;
- multi-head attention;
- positional encoding;
- patch tokenization;
- Tiny Image Transformer;
- attention visualization.

## Dataset

CIFAR-10 digunakan agar kedua model dapat dibangun dan dilatih dari nol dalam skala praktikum.

---

# Slide 47 - Protokol Eksperimen Praktikum 02

## Variabel yang Dikontrol

- indeks train, validation, dan test;
- augmentasi;
- jumlah epoch;
- optimizer dan learning rate;
- batch size;
- random seed;
- perangkat evaluasi.

## Model

- SimpleCNN dari nol;
- Tiny Image Transformer dari nol.

## Catatan Fairness

Konfigurasi yang sama memberikan kontrol awal, tetapi belum menjamin hyperparameter optimal atau jumlah parameter setara.

---

# Slide 48 - Apa yang Harus Diukur?

## Kinerja Prediksi

- accuracy;
- macro-F1;
- classification report;
- confusion matrix.

## Proses Belajar

- training loss;
- validation loss;
- training-validation gap;
- stabilitas antar-seed.

## Efisiensi

- jumlah parameter;
- waktu training;
- latensi;
- throughput.

## Analisis Kualitatif

- feature map;
- attention map;
- contoh salah klasifikasi.

---

# Slide 49 - Membaca Hasil Secara Hati-Hati

## Kesimpulan yang Tidak Cukup

> CNN lebih baik karena accuracy-nya lebih tinggi.

## Kesimpulan yang Lebih Tepat

> Pada CIFAR-10, training dari nol, dan konfigurasi yang diuji, SimpleCNN mencapai kinerja lebih tinggi dalam budget epoch tertentu. Hasil ini konsisten dengan manfaat bias lokal pada data terbatas, tetapi belum membuktikan CNN selalu unggul karena parameter dan hyperparameter kedua model belum sepenuhnya disejajarkan.

## Prinsip

Sebutkan kondisi, ukuran efek, variasi, keterbatasan, dan alternatif penjelasan.

---

# Slide 50 - Dari Failure Case ke Eksperimen Berikutnya

```text
Observasi
→ Dugaan penyebab
→ Hipotesis
→ Variabel yang dikontrol
→ Eksperimen
→ Evidence
→ Klaim terbatas
```

## Contoh

- Observasi: model sering tertukar antara cat dan dog.
- Dugaan: resolusi rendah menghilangkan detail bentuk.
- Eksperimen: bandingkan resolusi dan ukuran data.
- Evidence: perubahan per-class recall dan confusion.
- Klaim: dibatasi pada dataset dan kondisi yang diuji.

---

# Slide 51 - Tugas Pertemuan 02

## Tugas Praktikum

- Menjalankan SimpleCNN dan Tiny Image Transformer.
- Menjelaskan bentuk tensor pada setiap tahap.
- Membandingkan kurva, accuracy, macro-F1, parameter, dan waktu.
- Menganalisis feature map dan attention map.
- Menemukan minimal lima failure cases.
- Melakukan dua eksperimen pengembangan.

## Tugas Dataset

Pilih satu dataset yang relevan dengan minat disertasi dan tuliskan:

- task dan jenis anotasi;
- unit data dan strategi split;
- lisensi serta akses;
- bias dan risiko leakage;
- baseline yang layak;
- keterbatasan klaim generalisasi.

---

# Slide 52 - Target Keluaran

Mahasiswa menghasilkan:

1. notebook Praktikum 02 yang dapat dijalankan ulang;
2. tabel perbandingan CNN dan Transformer;
3. kurva pembelajaran dan confusion matrix;
4. feature map dan attention map;
5. error analysis;
6. research log seluruh eksperimen;
7. audit singkat satu dataset target;
8. dua pertanyaan lanjutan menuju arsitektur modern.

## Indikator Keberhasilan

Mahasiswa dapat menjelaskan **mengapa** suatu hasil terjadi dan **batas bukti** yang dimiliki.

---

# Slide 53 - Persiapan Menuju Pertemuan 03

## Setelah Pertemuan 02

Mahasiswa telah memahami:

- convolution dan residual connection;
- feature map dan receptive field;
- token, Q/K/V, dan self-attention;
- multi-head attention dan positional encoding;
- patch embedding;
- training model dari nol;
- peran karakteristik dataset.

## Pertemuan 03 Akan Melanjutkan

- ResNet, EfficientNet, dan CNN modern;
- DeiT, ViT, dan Swin Transformer;
- pretrained model dan transfer learning;
- kebutuhan data dan inductive bias;
- parameter, FLOPs, latency, dan robustness;
- protokol komparasi arsitektur modern.

---

# Slide 54 - Rangkuman

- CNN mempelajari kernel dan membangun representasi lokal secara hierarkis.
- Stride, padding, pooling, dan receptive field mengatur aliran informasi spasial.
- Residual connection membantu optimasi jaringan dalam.
- Transformer memproses token melalui Q, K, V, dan self-attention.
- Multi-head attention memungkinkan beberapa pola hubungan dipelajari paralel.
- Positional encoding memberi informasi posisi yang tidak tersedia secara bawaan.
- Patch tokenization menghubungkan citra dengan Transformer.
- Dataset menentukan task, bias, benchmark, dan batas generalisasi.
- Audit dan split yang benar sama pentingnya dengan pemilihan arsitektur.

---

# Slide 55 - Referensi Kunci

## Arsitektur dan Representasi

- LeCun et al. (1998), *Gradient-Based Learning Applied to Document Recognition*.
- Krizhevsky et al. (2012), *ImageNet Classification with Deep Convolutional Neural Networks*.
- He et al. (2016), *Deep Residual Learning for Image Recognition*.
- Vaswani et al. (2017), *Attention Is All You Need*.
- Dosovitskiy et al. (2021), *An Image Is Worth 16×16 Words*.

## Dataset dan Benchmark

- Krizhevsky (2009), CIFAR-10 dan CIFAR-100.
- Deng et al. (2009), ImageNet.
- Everingham et al., PASCAL VOC.
- Lin et al. (2014), Microsoft COCO.
- Cordts et al. (2016), Cityscapes.
- Timofte et al. (2017), DIV2K.
- Abdelhamed et al. (2018), Smartphone Image Denoising Dataset.
- Nah et al. (2017), GoPro dynamic scene deblurring dataset.
- Ponomarenko et al. (2015), TID2013.
- Dokumentasi resmi dataset yang digunakan.

---

# Slide 56 - Penutup

## Pesan Utama

> Sebelum membandingkan arsitektur modern, kita perlu memahami bagaimana representasi dibangun dan bagaimana dataset membatasi makna hasil eksperimen.

## Langkah Berikutnya

Gunakan fondasi ini untuk menjawab pada Pertemuan 03:

> Dalam kondisi data, komputasi, dan tujuan tertentu, kapan CNN atau Vision Transformer menjadi pilihan yang lebih tepat?
