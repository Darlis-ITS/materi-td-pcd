# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 03
# Representasi Visual Modern: CNN, Attention, dan Vision Transformer

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan dalam Perkuliahan

## Peta Perjalanan Mata Kuliah

| Pertemuan | Topik | Status |
|---|---|---|
| 1 | Peta Riset Mutakhir Pengolahan Citra Digital | Sudah dibahas |
| 2 | Critical Paper Reading dan Identifikasi Research Gap | Sudah dibahas |
| 3 | Representasi Visual Modern: CNN, Attention, dan Vision Transformer | **Saat ini** |
| 4 | Self-Supervised Learning dan Foundation Vision Models | Berikutnya |

## Keterkaitan

- Pertemuan 2 memberikan cara membaca paper secara kritis; pertemuan ini mempraktikkan lensa kritis pada paper arsitektur CNN dan Vision Transformer.
- Representasi visual yang dibahas menjadi fondasi untuk self-supervised learning, multimodal, deteksi, segmentasi, dan generative vision pada pertemuan berikutnya.
- Pemahaman arsitektur diperlukan untuk menentukan posisi riset terhadap state-of-the-art.

---

# Slide 02 - Recap Pertemuan 1 dan 2

## Pertemuan 1: Peta Riset Mutakhir

- Perubahan paradigma dari pengolahan citra klasik menuju deep learning dan foundation models.
- Benchmark, reproducibility, dan masalah terbuka menjadi perhatian utama.

## Pertemuan 2: Critical Paper Reading

- Struktur paper, kontribusi, klaim, bukti, dan keterbatasan.
- Research gap dibedakan dari sekadar variasi implementasi atau penggantian dataset.

## Relevansi untuk Pertemuan 3

- Membandingkan CNN dan ViT tidak cukup dengan akurasi akhir.
- Kita harus menilai baseline, kontrol eksperimen, dan validitas klaim arsitektur.

---

# Slide 03 - Tujuan Pembelajaran Pertemuan 3

## Capaian Sesuai RPS

- Menganalisis perbedaan inductive bias CNN dan Vision Transformer.
- Mengevaluasi implikasi pemilihan arsitektur terhadap data, komputasi, dan generalisasi.
- Memilih representasi visual yang sesuai untuk karakteristik masalah penelitian.

## Target Praktis

- Mampu melakukan fine-tuning model dari torchvision atau timm.
- Mampu membandingkan metrik, waktu inferensi, jumlah parameter, dan kurva pembelajaran.
- Menghasilkan laporan komparatif yang tidak hanya berbasis akurasi.

---

# Slide 04 - Pertanyaan Kunci yang Akan Diuji

## Pertanyaan Utama

- Kapan CNN lebih efektif daripada transformer?
- Bagaimana kebutuhan data memengaruhi kinerja masing-masing arsitektur?
- Apakah peningkatan akurasi berasal dari arsitektur atau dari skala pretraining?
- Bagaimana trade-off parameter, FLOPs, interpretasi, dan generalisasi?

## Pertanyaan Turunan untuk Penelitian

- Representasi visual apa yang paling cocok untuk domain target disertasi?
- Bagaimana memastikan eksperimen perbandingan arsitektur dilakukan secara adil?

---

# Slide 05 - Representasi Visual: Dari Fitur Manual ke Learned Representation

## Definisi

Representasi visual adalah transformasi dari piksel menjadi fitur yang dapat digunakan untuk klasifikasi, deteksi, segmentasi, atau tugas visi lainnya.

## Perkembangan Pendekatan

| Pendekatan | Karakteristik | Contoh |
|---|---|---|
| Fitur manual | Dirancang berdasarkan pengetahuan domain | SIFT, HOG, ORB |
| CNN features | Dipelajari dari data dengan bias lokal | VGG, ResNet, EfficientNet |
| Transformer features | Dipelajari dengan attention global | ViT, DeiT, Swin |

## Catatan Penting

- Pemilihan arsitektur menentukan jenis representasi yang dipelajari.
- Perbedaan ini berdampak pada kebutuhan data, komputasi, dan kemampuan generalisasi.

---

# Slide 06 - Arsitektur CNN Modern: Blok Dasar

## Alur Umum CNN

```text
Input -> Conv -> BN -> ReLU -> Pool -> (diulang) -> Global Pool -> FC -> Output
```

## Komponen Utama

- Konvolusi: ekstraksi fitur lokal dengan kernel.
- Batch Normalization: stabilisasi distribusi aktivasi.
- ReLU: aktivasi non-linear.
- Pooling: reduksi resolusi spasial.

## Model Modern

- ResNet memperkenalkan residual connection.
- DenseNet menghubungkan semua lapisan.
- EfficientNet menyeimbangkan depth, width, dan resolusi.

---

# Slide 07 - Operasi Konvolusi

## Ide Dasar

- Kernel kecil digeser melintasi citra untuk mendeteksi pola lokal.
- Parameter digunakan bersama di seluruh posisi spasial.

## Notasi

```text
Y[i, j] = sum_k sum_m sum_n X[i+m, j+n] * W[m, n, k] + b
```

## Kode Sederhana PyTorch

```python
import torch.nn as nn

conv = nn.Conv2d(
    in_channels=3,
    out_channels=16,
    kernel_size=3,
    stride=1,
    padding=1
)
## Input: (B, 3, H, W) -> Output: (B, 16, H, W)
```

---

# Slide 08 - Residual Connection

## Masalah Jaringan Dalam

- Semakin dalam CNN, akurasi dapat menurun karena degradasi training.
- Bukan hanya overfitting, tetapi kesulitan optimasi.

## Solusi ResNet

```text
Output = F(x) + x
```

- F(x) adalah blok konvolusi yang dipelajari.
- Jalur identity x memudahkan gradien mengalir.
- Jaringan dapat belajar fungsi identitas jika diperlukan.

## Dampak

- Memungkinkan training arsitektur sangat dalam.
- Menjadi standar pada hampir semua arsitektur modern.

---

# Slide 09 - Normalization Layer

## Fungsi Normalisasi

- Menstabilkan distribusi aktivasi antar lapisan.
- Mempercepat konvergensi.
- Mengurangi sensitivitas terhadap inisialisasi dan learning rate.

## Jenis Normalisasi

| Jenis | Rentang Normalisasi | Umum Digunakan Pada |
|---|---|---|
| Batch Normalization | Per batch dan per channel | CNN |
| Layer Normalization | Per sampel, seluruh channel | Transformer |
| Group Normalization | Per sampel, kelompok channel | CNN dengan batch kecil |

## Catatan untuk ViT

- Vision Transformer umumnya menggunakan Layer Normalization, bukan Batch Normalization.

---

# Slide 10 - Inductive Bias CNN

## Definisi

Inductive bias adalah asumsi bawaan yang dimiliki model mengenai data sebelum melihat data.

## Bias Utama CNN

- **Lokalitas**: piksel terdekat lebih relevan daripada piksel jauh.
- **Translasi ekuivariansi**: pola yang sama dikenali di posisi berbeda.
- **Hierarki spasial**: fitur dibangun dari tepi, lalu bagian, lalu objek.

## Dampak

- CNN dapat belajar dari lebih sedikit data karena asumsi yang kuat.
- Efektif pada citra alami dengan struktur spasial lokal.

---

# Slide 11 - Kekuatan dan Keterbatasan CNN

## Kekuatan

- Efisien secara parameter berkat weight sharing.
- Mudah dilatih pada dataset berukuran sedang.
- Receptive field bertahap, sehingga interpretasi spasial lebih mudah.
- Banyak pretrained model tersedia.

## Keterbatasan

- Receptive field kecil pada lapisan awal.
- Menangkap hubungan jarak jauh membutuhkan jaringan yang sangat dalam.
- Sulit memodelkan ketergantungan global antar bagian citra.
- Arsitektur yang dirancang untuk citra tidak langsung terapkan pada input multimodal.

---

# Slide 12 - Attention: Ide Dasar

## Motivasi

- Manusia tidak melihat semua piksel dengan bobot yang sama.
- Fokus, atau perhatian, dapat diarahkan ke objek penting meskipun letaknya jauh.

## Konsep Attention

- Model memilih informasi relevan dari seluruh elemen input.
- Bobot perhatian ditentukan dari kesesuaian antara query dan key.
- Nilai informasi diambil dari value.

## Contoh Analogi

- Mencari buku di perpustakaan: query adalah topik, key adalah label buku, value adalah isi buku.

---

# Slide 13 - Self-Attention: Query, Key, Value

## Transformasi Linear

```text
Q = X W_Q
K = X W_K
V = X W_V
```

## Skor Perhatian

```text
Attention(Q, K, V) = softmax(Q K^T / sqrt(d_k)) V
```

## Interpretasi

- Setiap token memperhatikan semua token lain.
- Bobot perhatian menunjukkan relevansi antar token.
- Tidak ada bias lokal yang memaksa token hanya melihat tetangga terdekat.

---

# Slide 14 - Multi-Head Attention dan Skala

## Multi-Head Attention

- Beberapa head dijalankan secara paralel.
- Setiap head dapat menangkap jenis hubungan berbeda.
- Hasil semua head digabungkan dan diproyeksikan.

## Diagram Sederhana

```text
Input
  |
  +-- Head 1 --> attention lokal/tekstur
  +-- Head 2 --> attention global/konteks
  +-- Head 3 --> attention antar wilayah
  |
  Concatenate -> Linear -> Output
```

## Skala d_k

- Pembagian dengan sqrt(d_k) menjaga agar skor tidak terlalu besar.
- Mencegah gradien softmax jenuh.

---

# Slide 15 - Dari NLP ke Vision: Attention Is All You Need

## Asal-usul Transformer

- Vaswani et al. (2017) memperkenalkan Transformer untuk machine translation.
- Berbasis self-attention tanpa recurrent atau convolution.
- Arsitektur ini sukses besar di NLP.

## Adaptasi ke Citra

- Citra tidak dapat langsung diproses sebagai sekuens kata.
- Perlu strategi untuk mengubah citra menjadi token.
- ViT membagi citra menjadi patch dan memperlakukan patch seperti kata.

## Titik Masuk

- Setelah patch tokenization, mekanisme Transformer dapat diterapkan langsung.

---

# Slide 16 - Vision Transformer: Arsitektur

## Alur Umum ViT

```text
Image
  |
  Patch Embedding
  |
  [CLS] + Positional Encoding
  |
  Transformer Encoder (L blok)
  |
  MLP Head
  |
  Class Prediction
```

## Sumber

- Dosovitskiy et al. (2021) mengusulkan ViT.
- Token khusus [CLS] digunakan sebagai representasi agregat citra untuk klasifikasi.

## Poin Kunci

- Tidak menggunakan konvolusi.
- Semua hubungan antar patch dipelajari melalui attention.

---

# Slide 17 - Patch Tokenization

## Cara Kerja

- Citra ukuran H x W x C dibagi menjadi patch berukuran P x P.
- Setiap patch di-flatten dan diproyeksikan linear menjadi vektor token berdimensi D.
- Contoh: 224 x 224 x 3 dengan P=16 menghasilkan 196 token.

## Kode Sederhana

```python
import torch.nn as nn

P, D = 16, 768
patch_embed = nn.Conv2d(
    in_channels=3,
    out_channels=D,
    kernel_size=P,
    stride=P
)

## Input: (B, 3, 224, 224) -> Output: (B, D, 14, 14)
## Lalu flatten menjadi (B, 196, D)
```

## Catatan

- Patch embedding dapat dipandang sebagai konvolusi non-overlap.
- Meskipun ada kemiripan, perlakuan selanjutnya sangat berbeda dari CNN.

---

# Slide 18 - Positional Encoding

## Masalah Urutan Token

- Self-attention tidak memiliki konsep urutan secara bawaan.
- Permutasi token menghasilkan representasi yang sama tanpa informasi posisi.

## Solusi

- Menambahkan positional encoding ke setiap token.
- Dapat berupa learnable embedding atau fungsi sinusoidal.
- Dalam ViT, positional embedding dipelajari bersama model.

## Implikasi

- Model harus belajar bahwa patch kiri-atas berbeda dari patch kanan-bawah.
- Perubahan resolusi memerlukan interpolasi positional embedding.

---

# Slide 19 - Transformer Encoder Block

## Struktur Satu Blok

```text
Input
  -> LayerNorm
  -> Multi-Head Self-Attention
  -> Add Residual
  -> LayerNorm
  -> MLP (2 lapis dengan GELU)
  -> Add Residual
  -> Output
```

## Persamaan dengan CNN Modern

- Residual connection digunakan di kedua arsitektur.
- Normalization digunakan di kedua arsitektur.

## Perbedaan Inti

- Attention bersifat global, bukan lokal.
- MLP bekerja per token, bukan konvolusi spasial.

---

# Slide 20 - Inductive Bias Berbeda: CNN vs ViT

## Tabel Perbandingan

| Aspek | CNN | Vision Transformer |
|---|---|---|
| Bias lokal | Kuat | Lemah |
| Weight sharing | Ya, pada kernel | Tidak wajib |
| Hubungan antar piksel | Bertahap melalui receptive field | Langsung melalui attention |
| Kebutuhan data | Relatif lebih sedikit | Relatif lebih besar |
| Fleksibilitas arsitektur | Terbatas oleh asumsi citra 2D | Lebih fleksibel |

## Implikasi

- ViT harus mempelajari struktur citra dari data.
- CNN sudah memiliki struktur tersebut sejak awal.
- Perbedaan ini menentukan kebutuhan pretraining dan augmentasi.

---

# Slide 21 - Perbandingan Skematis CNN dan ViT

## CNN

```text
Conv 3x3 -> Conv 3x3 -> Pool -> Conv 3x3 -> ... -> FC
Lokal dari awal, receptive field membesar bertahap
```

## ViT

```text
Patch Embedding -> Self-Attention -> MLP -> Self-Attention -> ... -> MLP Head
Semua token saling terhubung sejak blok pertama
```

## Perbandingan Visual

- CNN: pola lokal dikenali lebih dulu.
- ViT: informasi global dapat diakses pada lapisan pertama.
- Trade-off muncul pada efisiensi komputasi dan kebutuhan data.

---

# Slide 22 - ViT dan Kebutuhan Data

## Temuan dari Paper ViT

- ViT dilatih pada dataset sangat besar (mis. JFT-300M) mampu mengungguli CNN.
- Pada ImageNet-1k saja, ViT tanpa pretraining besar kurang kompetitif dibanding ResNet.
- Dibutuhkan data dalam jumlah besar untuk mengkompensasi lemahnya inductive bias.

## Peran DeiT

- Touvron et al. (2021) menunjukkan bahwa data augmentation, regularisasi, dan knowledge distillation membuat ViT kompetitif pada ImageNet-1k.
- DeiT adalah contoh bahwa training strategy dapat menggantikan sebagian inductive bias.

## Pesan untuk Penelitian

- Jangan menyimpulkan arsitektur unggul tanpa memperhitungkan skala data dan strategi training.

---

# Slide 23 - Transfer Learning dan Pretraining

## Alur Transfer Learning

```text
Pretraining pada dataset besar
        |
        v
Fine-tuning pada dataset target
        |
        v
Evaluasi pada test set target
```

## Manfaat

- Model tidak belajar dari nol.
- Fitur dasar seperti tepi, tekstur, dan bentuk sudah tersedia.
- Efektif untuk dataset kecil dan domain dengan label terbatas.

## Ketersediaan Pretrained Weights

- torchvision: ResNet, MobileNet, EfficientNet, dll.
- timm: ratusan arsitektur termasuk ViT, Swin, ConvNeXt.
- Hugging Face: ViT, DeiT, BEiT, dan model lain.

---

# Slide 24 - Peran Pretraining vs Arsitektur

## Pertanyaan Kritis

- Apakah peningkatan akurasi berasal dari arsitektur atau dari skala pretraining?
- Jawaban memerlukan eksperimen terkontrol.

## Kasus yang Sering Terjadi

- ViT pretrained pada data besar unggul.
- ViT dilatih dari awal pada dataset kecil kalah.
- CNN pretrained juga unggul jika dilatih pada data besar.

## Cara Mengontrol

- Bandingkan model dengan pretraining yang setara.
- Lakukan eksperimen dari awal dan eksperimen fine-tuning.
- Laporkan keduanya secara terpisah.

---

# Slide 25 - Efisiensi Parameter dan FLOPs

## Ukuran Model

| Model | Kategori | Perkiraan Parameter |
|---|---|---|
| ResNet-50 | CNN | 25,6 juta |
| ViT-Base | Transformer | 86 juta |
| ViT-Large | Transformer | 307 juta |
| Swin-Base | Transformer | 88 juta |

## Catatan

- Parameter dan FLOPs berbeda dengan kebutuhan memori dan latensi.
- FLOPs bergantung pada resolusi input dan jumlah token.
- Ukur throughput, GPU memory, dan waktu inferensi secara langsung.

## Kode Menghitung Parameter

```python
def count_parameters(model):
    return sum(p.numel() for p in model.parameters() if p.requires_grad)
```

---

# Slide 26 - Trade-off Generalisasi dan Robustness

## Temuan Umum dari Berbagai Studi

- CNN sering mengandalkan tekstur lokal.
- ViT, terutama yang dilatih besar, dapat memanfaatkan bentuk global.
- ViT dilaporkan lebih robust terhadap occlusion pada beberapa pengujian.
- Hasil sangat bergantung pada augmentasi dan pretraining.

## Implikasi untuk Riset

- Tidak ada pemenang mutlak.
- Pilihan arsitektur harus disesuaikan dengan jenis perubahan data yang dihadapi.
- Uji model pada distribution shift yang relevan dengan domain target.

---

# Slide 27 - Kapan CNN Lebih Efektif?

## Kondisi yang Mendukung CNN

- Dataset kecil dengan label terbatas.
- Sumber daya komputasi terbatas.
- Kebutuhan inferensi cepat pada perangkat edge.
- Tugas yang sangat bergantung pada pola lokal.
- Interpretasi spasial berbasis fitur penting.

## Contoh Kasus

- Segmentasi objek kecil dengan tekstur lokal.
- Mobile dan embedded vision.
- Baseline untuk dataset medis atau industri dengan sampel terbatas.

## Catatan

- Efektivitas CNN tetap harus diuji, bukan diasumsikan.

---

# Slide 28 - Kapan Transformer Lebih Efektif?

## Kondisi yang Mendukung ViT

- Dataset besar atau pretrained model yang kuat tersedia.
- Tugas membutuhkan hubungan jangka panjang antar wilayah citra.
- Input berupa patch atau token dari berbagai sumber.
- Budget komputasi cukup besar.
- Transfer learning ke banyak downstream task.

## Contoh Kasus

- Deteksi objek kecil dengan konteks global.
- Segmentasi semantik pada citra satelit atau medis.
- Representasi bersama untuk vision-language model.

---

# Slide 29 - Studi Kasus Dataset Kecil

## Skenario

- Dataset 1.000 citra, 10 kelas, tanpa pretraining.

## Prediksi Hasil

| Arsitektur | Training | Perkiraan Hasil |
|---|---|---|
| CNN kecil | Dari awal | Mungkin overfit, tetapi bisa berfungsi |
| CNN pretrained | Fine-tuning | Baseline kuat |
| ViT dari awal | Dari awal | Sangat rawan overfit |
| ViT pretrained | Fine-tuning | Kompetitif dengan CNN pretrained |

## Kesimpulan

- Pada dataset kecil, keberadaan pretraining sering lebih menentukan daripada pilihan arsitektur.
- Ini harus diuji secara empiris pada praktikum.

---

# Slide 30 - Praktikum: Fine-tuning dengan torchvision

## Langkah

- Siapkan dataset citra dan transform.
- Load model pretrained.
- Ganti lapisan klasifikasi dengan jumlah kelas baru.
- Latih hanya head atau seluruh lapisan.

## Kode Contoh

```python
import torchvision.models as models
import torch.nn as nn

model = models.resnet50(
    weights=models.ResNet50_Weights.IMAGENET1K_V2
)
model.fc = nn.Linear(model.fc.in_features, num_classes=10)
```

## Catatan

- Gunakan transform yang sesuai dengan pretraining model.
- Mulai dengan learning rate kecil untuk fine-tuning.

---

# Slide 31 - Praktikum: Fine-tuning dengan timm

## Keuntungan timm

- Menyediakan banyak arsitektur dalam API seragam.
- Mempermudah perbandingan CNN dan ViT.
- Pretrained weights tersedia untuk mayoritas model.

## Kode Contoh

```python
import timm

model_vit = timm.create_model(
    "vit_base_patch16_224",
    pretrained=True,
    num_classes=10
)

model_cnn = timm.create_model(
    "resnet50",
    pretrained=True,
    num_classes=10
)
```

## Catatan

- Sesuaikan resolusi input dengan spesifikasi model.
- timm juga menyediakan helper untuk data config dan augmentasi.

---

# Slide 32 - Protokol Eksperimen Komparatif

## Elemen Protokol

- Dataset tetap dan split yang konsisten.
- Random seed tetap untuk setiap run.
- Optimizer, learning rate, batch size, dan jumlah epoch sama.
- Augmentasi identik untuk semua model.
- Jumlah run minimal tiga kali untuk melihat variasi.

## Hal yang Harus Dicatat

- Versi library dan perangkat keras.
- Waktu training dan waktu inferensi.
- Konfigurasi lengkap setiap model.

## Prinsip

- Eksperimen harus dapat direproduksi oleh orang lain.
- Perbandingan dilakukan dengan kontrol yang adil.

---

# Slide 33 - Metrik Evaluasi

## Metrik Klasifikasi

- Accuracy, macro-F1, weighted-F1, AUC.
- Confusion matrix untuk melihat pola kesalahan.

## Metrik Efisiensi

- Jumlah parameter.
- FLOPs.
- Latensi rata-rata per citra.
- Throughput citra per detik.
- Peak GPU memory.

## Contoh Tabel Laporan

| Model | Accuracy | F1 | Parameter | Latensi |
|---|---|---|---|---|
| ResNet-50 | 92,1 | 91,4 | 25,6M | 4,2 ms |
| ViT-B | 93,0 | 92,2 | 86M | 9,8 ms |

---

# Slide 34 - Kurva Pembelajaran dan Waktu Inferensi

## Kurva Pembelajaran

- Plot training loss dan validation accuracy terhadap epoch.
- Amati gejala overfit dan underfit.
- Perhatikan epoch terbaik, bukan hanya epoch terakhir.

## Waktu Inferensi

- Ukur dengan batch tetap dan beberapa kali pengulangan.
- Gunakan waktu rata-rata dan standar deviasi.
- Bedakan pengukuran pada CPU dan GPU.

## Interpretasi

- Model dengan akurasi sama tetapi latensi lebih rendah lebih cocok untuk real-time.
- Model dengan kurva lebih stabil lebih mudah dikendalikan.

---

# Slide 35 - Menghindari Kesimpulan Berdasarkan Akurasi Saja

## Masalah yang Sering Terjadi

- Akurasi berbeda tipis tetapi tidak signifikan secara statistik.
- Satu run beruntung karena seed tertentu.
- Hyperparameter tidak di-tuning secara adil untuk setiap model.

## Praktik yang Disarankan

- Laporkan mean dan standar deviasi dari beberapa run.
- Gunakan confidence interval.
- Lakukan error analysis untuk melihat sumber kesalahan.
- Dokumentasikan strategi pemilihan hyperparameter.

## Pertanyaan Refleksi

- Apakah perbedaan berasal dari arsitektur atau dari pretraining?
- Apakah hasil berlaku pada domain target penelitian?

---

# Slide 36 - Kerangka Critical Review Paper Arsitektur

## Pertanyaan Review

- Problem: apa masalah yang ingin diselesaikan paper?
- Metode: apakah kontribusinya pada arsitektur, training, atau data?
- Eksperimen: apakah baseline dan ablation study memadai?
- Validitas: apakah klaim didukung bukti eksperimen?
- Generalisasi: apakah hasil berlaku di luar dataset yang diuji?

## Hubungan dengan Pertemuan 2

- Gunakan matriks literatur untuk membandingkan paper arsitektur.
- Gunakan kerangka reviewer untuk menilai apakah paper layak dijadikan state-of-the-art.

---

# Slide 37 - Analisis Trade-off untuk Penelitian Disertasi

## Tabel Keputusan

| Karakteristik Masalah | Rekomendasi Arsitektur |
|---|---|
| Dataset kecil, label terbatas | CNN pretrained |
| Hubungan global penting | ViT atau Swin |
| Inferensi real-time | CNN ringan |
| Pretraining besar tersedia | ViT pretrained |
| Interpretasi spasial | CNN dengan saliency |
| Domain multimodal | Transformer atau ViT sebagai encoder |

## Langkah

- Identifikasi karakteristik masalah penelitian.
- Tentukan prioritas: akurasi, interpretasi, kecepatan, atau robustnes.
- Bandingkan beberapa model pada eksperimen pendahuluan.

---

# Slide 38 - Menghubungkan ke Self-Supervised Learning

## Relevansi Pertemuan 4

- Self-supervised learning (SSL) mempelajari representasi tanpa label.
- DINO dan DINOv2 banyak menggunakan arsitektur ViT.
- Pretraining menjadi penentu kualitas representasi.

## Pertanyaan yang Muncul

- Representasi apa yang dipelajari ViT secara self-supervised?
- Apakah fitur yang dihasilkan bersifat semantik atau hanya tekstural?
- Kapan SSL lebih unggul daripada supervised pretraining?

## Tindak Lanjut

- Pertemuan 4 akan membahas contrastive learning, masked image modeling, dan linear probing.

---

# Slide 39 - Rangkuman

## Poin Utama

- CNN memiliki inductive bias lokal yang kuat sehingga efisien pada dataset kecil.
- ViT memiliki inductive bias lemah sehingga membutuhkan data besar atau pretraining kuat.
- Attention memungkinkan hubungan global antar patch secara langsung.
- Pretraining sering menjadi faktor penentu kinerja, bukan hanya arsitektur.
- Perbandingan arsitektur harus memperhitungkan parameter, FLOPs, data, dan protokol eksperimen.

## Implikasi

- Pilihan representasi visual harus disesuaikan dengan masalah dan sumber daya penelitian.
- Laporan komparatif yang baik menjelaskan mengapa, bukan sekadar menyatakan siapa pemenang.

---

# Slide 40 - Referensi Kunci

## Paper Arsitektur

- ResNet: He et al. (2016), Deep Residual Learning for Image Recognition.
- Transformer: Vaswani et al. (2017), Attention Is All You Need.
- ViT: Dosovitskiy et al. (2021), An Image Is Worth 16x16 Words.
- DeiT: Touvron et al. (2021), Training Data-Efficient Image Transformers and Distillation through Attention.
- Swin Transformer: Liu et al. (2021).

## Praktik Reproduksi

- PyTorch, torchvision: dokumentasi resmi.
- timm: pustaka model untuk computer vision.
- Gunakan referensi di RPS untuk pendalaman lebih lanjut.

---

# Slide 41 - Tugas dan Target Keluaran

## Tugas Praktikum

- Melakukan fine-tuning model torchvision atau timm pada dataset citra terkontrol.
- Membandingkan minimal satu CNN dan satu ViT.
- Mengumpulkan metrik, kurva pembelajaran, waktu inferensi, dan jumlah parameter.

## Target Keluaran

- Laporan komparatif CNN dan ViT.
- Analisis bukan hanya akurasi, tetapi juga faktor arsitektur, pretraining, dan data.
- Hubungkan hasil dengan research gap yang telah diidentifikasi pada pertemuan 2.

## Kriteria Penilaian

- Kesesuaian protokol eksperimen.
- Interpretasi hasil yang kritis.
- Kemampuan menjelaskan trade-off representasi visual.

---

# Slide 42 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Self-Supervised Learning dan Foundation Vision Models**