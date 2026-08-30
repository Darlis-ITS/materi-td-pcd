# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 07

## Object Detection Modern dengan YOLO dan Transformer

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 07 dalam RPS

## Posisi dalam Rangkaian Perkuliahan

| Posisi | Pertemuan | Topik |
|---|---|---|
| Sebelumnya | 06 | Image Restoration dan Computational Imaging |
| Saat ini | 07 | Object Detection Modern dengan YOLO dan Transformer |
| Berikutnya | 08 | Segmentasi Citra dan Promptable Foundation Models |

## Bahan Kajian dan CPMK

- Bahan kajian: BK-07 Object Detection dan Instance Understanding
- CPMK-2: Mengevaluasi paper ilmiah berdasarkan metodologi dan validitas klaim
- CPMK-3: Merancang eksperimen komputasional yang valid dan reproducible
- CPMK-4: Memanfaatkan framework dan foundation model modern untuk eksperimen

---

# Slide 02 - Tujuan Pembelajaran dan Agenda

## Tujuan Pembelajaran

- Memahami pipeline object detection: dari citra masukan hingga bounding box, kelas, dan confidence
- Membedakan one-stage detector dan transformer-based detector
- Mengevaluasi deteksi dengan metrik yang tepat: IoU, precision, recall, mAP
- Mengidentifikasi sumber kesalahan deteksi melalui confusion analysis, ukuran objek, dan domain shift

## Agenda Pertemuan

1. Recap singkat pertemuan sebelumnya dan posisi dalam RPS
2. Konsep dasar deteksi dan metrik evaluasi
3. Arsitektur YOLO dan transformer-based detector
4. Aspek penelitian: ketidakseimbangan kelas, threshold, anotasi, open-vocabulary
5. Workflow eksperimen, praktikum, dan analisis error

---

# Slide 03 - Recap Singkat Pertemuan 06

## Image Restoration dan Computational Imaging

- Restoration dimodelkan sebagai inverse problem: memperkirakan citra bersih dari citra terdegradasi
- PSNR dan SSIM sering tidak selaras dengan kualitas perseptual
- Diffusion-based restoration menjadi pendekatan mutakhir, tetapi memerlukan evaluasi yang hati-hati

## Kaitan dengan Deteksi

- Kualitas citra memengaruhi deteksi, namun fokus deteksi adalah lokasi dan kelas objek
- Pola pikir baseline yang adil, model degradasi, dan analisis kegagalan tetap relevan
- Di pertemuan ini, kita memindahkan perhatian dari pemulihan citra ke pemahaman isi citra

---

# Slide 04 - Jembatan Menuju Pertemuan 08

## Dari Deteksi ke Segmentasi

- Deteksi menghasilkan bounding box; segmentasi menghasilkan mask per piksel
- Pertemuan berikutnya membahas U-Net, transformer segmentation, SAM, dan promptable segmentation
- Konsep IoU, evaluasi area, dan error analysis yang dipelajari hari ini akan digunakan kembali pada IoU/Dice

## Benang Merah Perkuliahan

- Pertemuan 03-06 membangun representasi, restoration, dan multimodal
- Deteksi adalah salah satu tugas turunan yang menguji kualitas representasi
- Segmentasi kemudian menuntut pemahaman yang lebih halus daripada kotak

---

# Slide 05 - Apa itu Object Detection?

## Definisi

- Object detection = menemukan lokasi objek dalam citra dan mengenali kelasnya
- Output berupa bounding box, label kelas, dan confidence score
- Berbeda dengan klasifikasi citra yang hanya memberi label pada seluruh citra

## Representasi Output

```text
(x_min, y_min, x_max, y_max, class, score)
(320, 180, 512, 420, "person", 0.92)
```

- Sistem koordinat piksel dengan titik asal di kiri-atas
- Confidence score menyatakan tingkat keyakinan model

---

# Slide 06 - Representasi Bounding Box

## Format Bounding Box

| Format | Keterangan |
|---|---|
| xyxy | (x_min, y_min, x_max, y_max) |
| xywh | (x_min, y_min, width, height) |
| cxcywh | (center_x, center_y, width, height) |

## Konversi Antar Format

```python
def xyxy_to_cxcywh(box):
    x1, y1, x2, y2 = box
    cx = (x1 + x2) / 2
    cy = (y1 + y2) / 2
    w = x2 - x1
    h = y2 - y1
    return [cx, cy, w, h]
```

- Saat menggunakan dataset atau library baru, periksa format yang diharapkan

---

# Slide 07 - Intersection over Union (IoU)

## Definisi

- IoU mengukur tumpang tindih antara dua bounding box
- IoU = |A ∩ B| / |A ∪ B|

```text
+--------+
|   A    |
|   +----+----+
|   |    B    |
+---+----+    |
    |         |
    +---------+
```

## Peran dalam Evaluasi

- Ambang IoU menentukan prediksi dianggap true positive atau false positive
- Evaluasi COCO menggunakan IoU 0.50 sampai 0.95 dengan langkah 0.05

```python
def iou(box1, box2):
    x1 = max(box1[0], box2[0])
    y1 = max(box1[1], box2[1])
    x2 = min(box1[2], box2[2])
    y2 = min(box1[3], box2[3])
    inter = max(0, x2 - x1) * max(0, y2 - y1)
    area1 = (box1[2]-box1[0]) * (box1[3]-box1[1])
    area2 = (box2[2]-box2[0]) * (box2[3]-box2[1])
    union = area1 + area2 - inter
    return inter / union if union > 0 else 0
```

---

# Slide 08 - Precision dan Recall

## Definisi

- True positive (TP): prediksi benar dengan IoU cukup dan kelas tepat
- False positive (FP): prediksi salah, tidak ada objek atau kelas salah
- False negative (FN): objek aktual tidak terdeteksi

- Precision = TP / (TP + FP)
- Recall = TP / (TP + FN)

## Interpretasi

- Precision tinggi berarti sebagian besar prediksi memang benar
- Recall tinggi berarti sebagian besar objek aktual berhasil ditemukan
- Menaikkan jumlah prediksi dapat menaikkan recall tetapi menurunkan precision

---

# Slide 09 - Precision-Recall Curve dan Average Precision

## Kurva Precision-Recall

- Urutkan prediksi berdasarkan confidence tertinggi ke terendah
- Evaluasi kumulatif setiap prediksi menghasilkan pasangan precision-recall
- AP adalah luas area di bawah kurva precision-recall

## Pseudo-code Perhitungan AP

```text
predictions sorted by confidence descending
for each prediction:
    if IoU >= threshold and class matches:
        mark TP
    else:
        mark FP
    compute precision and recall at each rank
AP = area under the precision-recall curve
```

---

# Slide 10 - mAP dan Variannya

## Mean Average Precision

- mAP adalah rata-rata AP dari seluruh kelas
- Merupakan metrik utama pada benchmark COCO

| Metrik | Keterangan |
|---|---|
| AP@IoU=0.5 | Kriteria longgar, gaya PASCAL VOC |
| AP@IoU=0.75 | Kriteria ketat untuk lokalisasi |
| AP@IoU=0.5:0.95 | Rata-rata AP dari IoU 0.50 hingga 0.95, default COCO |

## Catatan Kritis

- mAP menggabungkan lokalisasi dan klasifikasi dalam satu angka
- Tidak menunjukkan di mana dan mengapa model salah
- Perlu analisis tambahan untuk memahami perilaku model

---

# Slide 11 - Keterbatasan mAP dan Pentingnya Error Analysis

## Mengapa mAP Tidak Cukup

- Dua model dengan mAP sama dapat memiliki pola kesalahan berbeda
- Peningkatan mAP dapat didominasi kelas mayoritas atau objek besar
- Perbaikan kecil pada lokalisasi tidak selalu terlihat pada perubahan mAP

## Arah Analisis yang Diperlukan

- Distribusi false positive: kesalahan lokalisasi, kesalahan klasifikasi, atau background
- Distribusi false negative: objek kecil, objek tertutup, domain sulit
- Evaluasi per subset ukuran objek: small, medium, large

---

# Slide 12 - Taksonomi Detektor

## Kategori Besar

| Kategori | Contoh | Ciri |
|---|---|---|
| Two-stage | Faster R-CNN | Region proposal lalu klasifikasi |
| One-stage | YOLO, SSD | Prediksi langsung tanpa proposal |
| Transformer-based | DETR | Set prediction dengan attention global |

## Peta Konsep

```text
Detektor
├── Two-stage: Faster R-CNN
├── One-stage: YOLO, SSD, RetinaNet
└── Transformer: DETR, Deformable DETR
```

- Fokus pertemuan ini: YOLO sebagai one-stage, DETR sebagai transformer-based

---

# Slide 13 - Anchor dan Anchor-Free

## Anchor-Based

- Merancang kotak referensi dengan berbagai skala dan rasio
- Model memprediksi offset dari anchor
- Kekurangan: banyak hyperparameter dan kurang fleksibel pada bentuk objek baru

## Anchor-Free

- Memprediksi titik pusat, ukuran, atau keypoint secara langsung
- Tidak memerlukan anchor
- YOLO modern dan sejumlah transformer detector menggunakan pendekatan ini

## Implikasi Penelitian

- Pilihan anchor atau anchor-free memengaruhi desain eksperimen dan interpretasi hasil

---

# Slide 14 - YOLO: Filosofi One-Stage Detection

## You Only Look Once

- YOLO asli membagi citra menjadi grid
- Setiap grid cell memprediksi bounding box, confidence, dan probabilitas kelas
- Seluruh objek diprediksi dalam satu forward pass

## Kelebihan dan Tantangan Awal

- Kecepatan sangat tinggi sehingga cocok untuk real-time application
- Versi awal kurang akurat untuk objek kecil dan objek berdekatan
- Evolusi YOLO memperbaiki arsitektur, loss, augmentasi, dan label assignment

---

# Slide 15 - Evolusi YOLO

## Linimasa Singkat

| Versi | Kontribusi Utama |
|---|---|
| YOLO v1 | Deteksi real-time berbasis grid |
| YOLO v2/v3 | Anchor, multi-scale, feature pyramid |
| YOLO v4/v5 | CSP backbone, mosaik augmentasi, training trick |
| YOLO v8 | Anchor-free, ekosistem Ultralytics |
| YOLO v9/v10 | Varian terbaru dengan fokus efisiensi dan NMS-free |

## Catatan Riset

- Perbandingan antar versi harus memeriksa konfigurasi pelatihan dan kode sumber
- Tidak semua peningkatan versi bersifat adil sebagai baseline
- Baca dokumentasi Ultralytics untuk versi dan detail implementasi

---

# Slide 16 - Pipeline YOLO Modern

## Arsitektur Umum

- Backbone: mengekstrak fitur visual, umumnya CSPDarknet
- Neck: menggabungkan fitur multi-skala, misalnya PAN-FPN
- Head: memprediksi bounding box, kelas, dan confidence

## Diagram Alir

```text
Input -> Backbone -> Neck (multi-scale) -> Head
                                            ├── box regression
                                            ├── class prediction
                                            └── confidence
```

## Multi-Scale Feature

- Fitur resolusi tinggi membantu objek kecil
- Fitur resolusi rendah memberi konteks semantik untuk objek besar

---

# Slide 17 - Contoh Fine-tuning YOLO dengan Ultralytics

## Struktur Dataset

```text
dataset/
  images/train/...
  images/val/...
  labels/train/*.txt
  labels/val/*.txt
```

- Format label YOLO: `class cx cy w h`, nilai dinormalisasi 0-1

## Kode Training

```python
from ultralytics import YOLO

model = YOLO("yolov8n.pt")
model.train(
    data="dataset.yaml",
    epochs=50,
    imgsz=640,
    batch=16,
    project="exp_deteksi"
)
```

- Bobot awal `yolov8n.pt` dilatih pada COCO, digunakan sebagai transfer learning

---

# Slide 18 - Evaluasi dan Visualisasi Prediksi

## Menghitung Metrik

```python
model = YOLO("best.pt")
metrics = model.val(data="dataset.yaml")
print(metrics.box.map)        # mAP@0.5:0.95
print(metrics.box.map50)      # mAP@0.5
print(metrics.box.map75)      # mAP@0.75
```

## Visualisasi Prediksi

```python
model.predict(
    source="gambar.jpg",
    conf=0.25,
    save=True,
    save_txt=True
)
```

- Hasil: citra berisi bounding box dan file label
- Inspeksi visual tetap wajib, jangan hanya mengandalkan angka mAP

---

# Slide 19 - Transformer Detector: DETR

## DETR (Detection Transformer)

- Memperlakukan deteksi sebagai masalah set prediction
- Tidak memerlukan anchor, proposal, atau NMS
- Encoder-decoder transformer memproses konteks global

## Komponen Utama

- CNN backbone menghasilkan fitur
- Transformer encoder memberi konteks antar region
- Decoder dengan object queries menghasilkan prediksi
- Bipartite matching menghitung loss antara prediksi dan ground truth

---

# Slide 20 - Object Query dan Bipartite Matching

## Object Query

- Kumpulan vektor yang dipelajari, misalnya 100 atau 300
- Setiap query dapat memprediksi satu objek
- Tidak ada korespondensi tetap antara query dan posisi objek

## Bipartite Matching

- Mencocokkan prediksi ke ground truth secara optimal menggunakan Hungarian algorithm
- Menghilangkan kebutuhan NMS
- Kekurangan: konvergensi lebih lambat, terutama untuk objek kecil

```text
Prediksi 1 ----┐
Prediksi 2 ----┼--- matching optimal
Prediksi 3 ----┘
```

---

# Slide 21 - Perkembangan Deformable DETR dan Detector DINO

## Perbaikan DETR

| Metode | Ide Utama |
|---|---|
| Deformable DETR | Attention hanya pada titik sampling di sekitar referensi |
| Conditional DETR | Memisahkan query posisi dan konten |
| DINO (detector) | Contrastive denoising, query selection, look-forward scheme |

## Catatan Penting

- DINO pada detector berbeda dari DINO self-supervised learning yang dibahas di pertemuan 04
- Selalu nyatakan varian dan konfigurasi saat membandingkan metode

---

# Slide 22 - Perbandingan One-Stage dan Transformer Detector

## Tabel Perbandingan

| Aspek | YOLO (one-stage) | DETR (transformer) |
|---|---|---|
| Paradigma | Grid/anchor-free + regresi langsung | Set prediction + bipartite matching |
| Komponen manual | FPN, NMS pada sebagian versi | Minimal, tanpa NMS |
| Kecepatan | Sangat cepat | Lebih lambat |
| Akurasi objek kecil | Baik setelah YOLO v8 | Lemah di DETR, membaik di Deformable DETR |
| Konvergensi | Relatif stabil | Butuh pelatihan khusus |

## Implikasi

- Pilihan tergantung kebutuhan aplikasi dan tujuan riset
- Perbandingan harus mengontrol jumlah parameter, FLOPs, dan protokol pelatihan

---

# Slide 23 - Transfer Learning dan Fine-tuning Detector

## Mengapa Transfer Learning

- Data deteksi dengan anotasi bounding box sangat mahal
- Bobot pretrained pada COCO memberikan representasi visual awal yang kuat
- Fine-tuning menyesuaikan representasi dengan domain target

## Strategi Fine-tuning

- Bekukan backbone pada awal training jika dataset sangat kecil
- Gunakan augmentasi data dan learning rate rendah
- Dokumentasikan sumber pretrained dan skema fine-tuning

## Kaitan dengan Pertemuan 03 dan 04

- Representasi dari CNN, ViT, dan self-supervised learning dapat menjadi backbone detector
- Backbone modern seperti DINOv2 dan CLIP dapat dipasang pada arsitektur deteksi

---

# Slide 24 - Dataset dan Benchmark Deteksi

## Benchmark Standar

| Dataset | Kelas | Ciri |
|---|---|---|
| COCO | 80 | Standar benchmark dengan variasi ukuran objek |
| PASCAL VOC | 20 | Benchmark historis |
| Open Images | Banyak | Skala besar, label lebih bervariasi |

## Dataset Domain Spesifik

- Medis, satelit, kendaraan otonom, industri, dan lain-lain
- Distribusi ukuran objek dan kelas sangat berbeda dari COCO
- Evaluasi harus disesuaikan dengan karakteristik domain

---

# Slide 25 - Protokol Eksperimen Fine-tuning Detector

## Komponen Protokol

- Dataset split: train/val/test dengan stratifikasi sesuai kebutuhan
- Preprocessing dan augmentasi: resize, mosaic, flip, color jitter
- Konfigurasi: optimizer, learning rate, batch size, epochs, seed
- Baseline: model pretrained resmi dan metode pembanding yang wajar

## Contoh dataset.yaml

```yaml
train: dataset/images/train
val: dataset/images/val
nc: 2
names: ["objek_a", "objek_b"]
```

## Reproduksibilitas

- Simpan seed, versi library, environment, dan konfigurasi lengkap
- Gunakan log eksperimen agar setiap hasil dapat dilacak

---

# Slide 26 - Confusion Analysis: Jenis Kesalahan Deteksi

## Kategori Kesalahan

| Kesalahan | Definisi |
|---|---|
| Lokalisasi | Kelas benar, tetapi IoU di bawah threshold |
| Klasifikasi | Lokasi benar, tetapi kelas salah |
| False positive background | Prediksi tidak bertepatan dengan objek aktual |
| False negative | Objek aktual tidak terdeteksi |

## Tujuan Analisis

- Mengetahui prioritas perbaikan: apakah lebih baik memperbaiki lokalisasi, klasifikasi, atau recall
- Jangan berhenti pada mAP; analisis error memberi penjelasan

---

# Slide 27 - Analisis Kesalahan Berdasarkan Ukuran Objek

## Definisi Subset Ukuran pada COCO

| Subset | Luas Area (piksel) |
|---|---|
| Small | area < 32x32 |
| Medium | 32x32 <= area < 96x96 |
| Large | area >= 96x96 |

## Pola yang Sering Ditemukan

- Objek kecil cenderung memiliki false negative tinggi
- Model umumnya lebih mudah mendeteksi objek besar
- Augmentasi multi-scale dan loss berbasis area dapat membantu

## Pertanyaan Riset

- Pada kombinasi kelas dan ukuran apa kegagalan terkonsentrasi?

---

# Slide 28 - Ketidakseimbangan Kelas

## Dampak pada Deteksi

- Kelas jarang sering memiliki AP rendah meskipun mAP keseluruhan baik
- Confidence threshold yang dipilih untuk deployment bisa merugikan kelas minoritas
- Evaluasi per kelas wajib dilaporkan

## Contoh Matriks Analisis

| Kelas | Jumlah Train | AP | Catatan |
|---|---|---|---|
| Kendaraan | 50.000 | 0,72 | Cukup baik |
| Helm | 3.000 | 0,41 | Kurang representasi |
| Pejalan kaki malam | 500 | 0,12 | Data sangat terbatas |

## Strategi

- Re-weighting loss atau oversampling kelas minoritas
- Augmentasi khusus atau data sintetis dapat dipertimbangkan

---

# Slide 29 - Threshold, Kalibrasi Confidence, dan Trade-off

## Confidence Threshold

- Threshold rendah menaikkan recall, tetapi menurunkan precision
- Threshold tinggi menaikkan precision, tetapi menurunkan recall
- Pemilihan threshold tergantung biaya kesalahan pada aplikasi

## Ilustrasi Trade-off

| Threshold | Precision | Recall |
|---|---|---|
| 0,1 | 0,55 | 0,90 |
| 0,25 | 0,70 | 0,78 |
| 0,5 | 0,84 | 0,61 |

## Catatan

- mAP dihitung tanpa threshold tunggal, tetapi deployment memerlukan threshold
- Perlu memeriksa kalibrasi confidence agar skor dapat ditafsirkan sebagai probabilitas

---

# Slide 30 - Domain Shift dan Robustness

## Definisi

- Model dilatih pada satu domain, lalu diuji pada domain yang berbeda
- Contoh: foto siang vs malam, citra satelit vs drone, data sintetis vs asli

## Eksperimen Robustness

- Gunakan test set yang mewakili variasi domain
- Laporkan penurunan mAP antar domain
- Visualisasikan prediksi yang gagal untuk memahami penyebab

## Pertanyaan Kunci

- Apakah detector memahami objek secara umum, atau hanya menghafal pola konteks?

---

# Slide 31 - Kualitas Anotasi

## Anotasi Tidak Sempurna

- Bounding box tidak presisi
- Label salah atau tidak konsisten
- Objek aktual tidak terlabel, sehingga terlihat sebagai false positive

## Praktik Penelitian yang Baik

- Audit kualitas anotasi: variasi antar annotator, konsistensi label, dan area
- Baca dokumentasi protokol label saat menggunakan dataset publik
- Jangan mengubah ground truth tanpa alasan eksplisit dan terdokumentasi

## Kaitan dengan Pertemuan 08

- SAM dapat membantu membuat anotasi mask, tetapi kualitas tetap harus divalidasi

---

# Slide 32 - Open-Vocabulary Detection

## Konsep

- Mendeteksi kelas yang tidak terlihat saat pelatihan
- Memanfaatkan representasi vision-language seperti CLIP yang dibahas pada pertemuan 05
- Contoh: model dilatih pada kategori dasar, lalu diuji pada kategori baru melalui teks

## Arsitektur Umum

- Detektor menghasilkan region proposal
- Region dibandingkan dengan embedding teks kelas
- Head klasifikasi tidak terbatas pada kelas tetap

## Tantangan

- Generalisasi ke domain baru
- Bias pada data caption
- Evaluasi yang adil untuk kelas yang tidak terlihat

---

# Slide 33 - Pertanyaan Kunci Penelitian

## Evaluasi Kritis terhadap mAP

- Apakah peningkatan mAP mencerminkan peningkatan deteksi objek penting?
- Bagaimana false positive dan false negative didistribusikan?
- Bagaimana robustness diuji pada domain yang berbeda?

## Refleksi untuk Disertasi

- Setiap klaim "lebih baik" harus diperkuat analisis kesalahan dan subset evaluasi
- Jawab pertanyaan mengapa, bukan hanya berapa
- Hubungkan dengan experimental design yang akan dibahas pada pertemuan 12

---

# Slide 34 - Workflow Analisis Error

## Langkah-langkah

1. Jalankan detektor pada test set
2. Hitung mAP global dan per kelas
3. Buat confusion matrix antar kelas
4. Kelompokkan false positive: lokalisasi, klasifikasi, background
5. Kelompokkan false negative berdasarkan ukuran dan okulasi
6. Visualisasikan contoh prediksi yang gagal

## Diagram Alir

```text
Prediksi -> Evaluasi mAP -> Per-kelas -> FP/FN -> Visualisasi -> Kesimpulan
```

---

# Slide 35 - Praktikum dan Tugas

## Eksperimen Fine-tuning Detector

- Pilih dataset kecil atau subset data yang tersedia
- Fine-tuning YOLOv8 dengan Ultralytics
- Hitung mAP@0.5, mAP@0.5:0.95, AP per kelas, dan AP per ukuran
- Visualisasikan prediksi pada subset validasi
- Susun confusion analysis berdasarkan ukuran objek dan kelas

## Bukti Belajar

- Laporan benchmark deteksi
- Konfigurasi eksperimen lengkap
- Contoh prediksi dan analisis kegagalan

---

# Slide 36 - Template Laporan Benchmark Deteksi

## Struktur Laporan

1. Pendahuluan dan tujuan
2. Dataset dan protokol
3. Konfigurasi model dan pelatihan
4. Hasil kuantitatif
5. Analisis error
6. Kesimpulan dan keterbatasan

## Contoh Tabel Hasil

| Model | mAP@0.5 | mAP@0.5:0.95 | AP small | AP large |
|---|---|---|---|---|
| Pretrained baseline | 0,72 | 0,45 | 0,22 | 0,61 |
| Fine-tuned | 0,84 | 0,58 | 0,38 | 0,72 |

- Sertakan gambar prediksi sebelum dan sesudah fine-tuning

---

# Slide 37 - Critical Review Paper Detector

## Pertanyaan Review

- Apa masalah yang dijawab dan mengapa penting?
- Apa kontribusi teknis dan ilmiah?
- Apakah baseline dan konfigurasi adil?
- Apakah metrik dan analisis error memadai?
- Apakah klaim didukung oleh data?

## Matriks Perbandingan Paper

| Paper | Metode | Dataset | mAP | Keterbatasan |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

## Kaitan dengan Pertemuan 02

- Gunakan kemampuan critical paper reading untuk menilai apakah klaim detector valid

---

# Slide 38 - Diskusi Fairness Baseline

## Mengapa Baseline Harus Adil

- Detector sering dibandingkan dengan ukuran model dan konfigurasi yang tidak setara
- Baseline yang lemah membuat metode baru terlihat lebih baik dari aktual
- Ablation study diperlukan untuk membuktikan kontribusi komponen

## Aturan Praktis

- Gunakan pretrained resmi dengan protokol yang sama
- Laporkan parameter, FLOPs, dan waktu inferensi
- Lakukan ablation untuk setiap komponen yang diklaim

## Hubungan dengan Pertemuan 12

- Fairness baseline merupakan bagian dari experimental design dan reproducible benchmarking

---

# Slide 39 - Koneksi ke Pertemuan Berikutnya

## Dari Deteksi ke Segmentasi

- Pertemuan 08 membahas semantic, instance, dan panoptic segmentation
- Perbandingan supervised segmentation dan promptable foundation model (SAM)
- Metrik IoU/Dice dan evaluasi domain shift pada mask

## Keterampilan yang Akan Digunakan

- Error analysis dan visualisasi prediksi yang dipelajari hari ini
- Pemahaman IoU dan evaluasi area akan diperluas ke mask

## Persiapan

- Baca paper Segment Anything
- Pahami konsep prompt: point, box, dan mask

---

TERIMA KASIH

Pertemuan berikutnya

**Segmentasi Citra dan Promptable Foundation Models**