# Slide 00 - Cover

EF256129 - TD PCD

Pertemuan 01

## Peta Riset Mutakhir dan Evolusi Pengolahan Citra Digital

### Dari Representasi Piksel hingga Foundation Models dan Agenda Riset Computer Vision

Dr. Darlis Herumurti

Departemen Teknik Informatika - ITS

---

# Slide 01 - Tujuan Pembelajaran dan Arah Perkuliahan

Setelah mengikuti pertemuan ini, mahasiswa diharapkan mampu:

- Menjelaskan evolusi pengolahan citra digital dan computer vision dari pendekatan klasik hingga foundation models.
- Memetakan area riset mutakhir dalam pengolahan citra digital dan computer vision.
- Menjelaskan perubahan representasi visual dari piksel, handcrafted feature, learned feature, hingga embedding.
- Menjelaskan peran baseline dalam penelitian computer vision.
- Mengaitkan peta riset dengan minat awal disertasi.
- Menggunakan Python, NumPy, OpenCV, scikit-image, scikit-learn, dan Matplotlib untuk eksplorasi awal dataset citra dan reproduksi baseline sederhana.
- Menginterpretasikan hasil eksperimen sebagai bahan awal untuk menyusun asumsi, risiko, dan pertanyaan penelitian.

Pertemuan ini menjadi pintu masuk menuju seluruh perkuliahan.

Target akhir mata kuliah adalah proposal awal disertasi yang relevan dengan state-of-the-art dan memiliki positioning ilmiah yang jelas.

---

# Slide 02 - Posisi Pertemuan 01 dalam RPS

Mata kuliah dirancang sebagai perjalanan dari pemetaan riset hingga proposal disertasi.

| Fase | Pertemuan | Fokus |
|---|---|---|
| Pemetaan riset | 1-2 | Evolusi bidang, peta riset, critical paper reading, research gap |
| Metode mutakhir | 3-11 | CNN, transformer, foundation model, multimodal, restoration, detection, segmentation, generative, 3D, trustworthy CV |
| Desain penelitian | 12-14 | Experimental design, research question, metodologi |
| Proposal | 15-16 | Seminar proposal dan konsolidasi rencana disertasi |

Pertemuan 01 tidak memiliki prasyarat material.

Pertemuan berikutnya akan membahas critical paper reading dan identifikasi research gap.

---

# Slide 03 - Alur Pembelajaran Pertemuan 01

Pertemuan ini menggunakan alur dari konteks bidang menuju eksperimen sederhana.

```text
Peta perkembangan bidang
        ↓
Evolusi representasi visual
        ↓
Classical pipeline dan baseline
        ↓
Peta riset mutakhir
        ↓
Benchmark dan masalah terbuka
        ↓
Eksplorasi dataset
        ↓
Baseline eksperimen
        ↓
Failure analysis
        ↓
Research interpretation
        ↓
Research log dan pertanyaan awal
```

Tujuan utamanya bukan menguasai satu algoritma, tetapi memahami bagaimana keputusan metodologis dihubungkan dengan bukti eksperimen dan posisi penelitian.

---

# Slide 04 - Mengapa Perlu Memahami Evolusi Bidang Ini?

## Alasan Konseptual

- Penelitian doktoral menuntut kemampuan melihat posisi metodologi dalam lanskap yang lebih luas.
- Pilihan metode tidak boleh hanya berdasarkan tren, tetapi berdasarkan asumsi, kekuatan, keterbatasan, data, dan tujuan penelitian.
- Research gap yang baik sering muncul dari keterbatasan pada transisi antar-paradigma.

## Alasan Praktis

- Banyak persoalan computer vision modern masih berakar pada konsep klasik seperti filtering dan feature engineering.
- Benchmark yang sehat memerlukan baseline yang masuk akal.
- Model yang lebih kompleks belum tentu diperlukan jika metode sederhana sudah menyelesaikan problem dengan baik.

## Pertanyaan Kunci

- Apa yang berubah dari pendekatan klasik menuju deep learning dan foundation models?
- Apa yang tetap relevan?
- Di mana posisi riset Anda dalam peta bidang ini?

---

# Slide 05 - Perjalanan Paradigma Pengolahan Citra

Pengolahan citra digital berkembang dari pemrosesan sinyal menjadi sistem visi yang belajar representasi dari data.

```text
Era klasik                 Era learned representation
filtering                   CNN
feature engineering         Vision Transformer
rule-based                  Self-supervised learning
pixel statistics            Foundation models
                            Vision-language models
                            Generative models
                            3D neural rendering
                            Trustworthy CV
```

Perubahan utama bukan sekadar pergantian algoritma.

Yang berubah adalah **cara informasi visual direpresentasikan, dipelajari, dan dievaluasi**.

---

# Slide 06 - Pengolahan Citra dan Computer Vision

## Pengolahan Citra Digital

- Berfokus pada manipulasi citra sebagai sinyal dua dimensi.
- Input berupa citra dan output sering berupa citra atau fitur tingkat rendah.
- Contoh: enhancement, filtering, restoration, morphology, edge detection.

## Computer Vision

- Berfokus pada pemberian makna atau interpretasi semantik terhadap citra/video.
- Output dapat berupa kelas, bounding box, mask, depth, pose, atau deskripsi.

| Aspek | Pengolahan Citra | Computer Vision |
|---|---|---|
| Input | Citra | Citra / video |
| Output | Citra / fitur tingkat rendah | Interpretasi semantik |
| Contoh | Denoising | Object recognition |
| Fokus | Transformasi sinyal visual | Pemahaman visual |

Computer vision modern sering memanfaatkan pengolahan citra sebagai bagian dari pipeline, tetapi tidak terbatas pada preprocessing.

---

# Slide 07 - Fondasi Klasik: Filtering dan Feature Engineering

Pendekatan klasik menekankan manipulasi piksel dan ekstraksi fitur buatan tangan.

## Filtering

- Perbaikan kontras
- Penghalusan dan penajaman
- Reduksi noise
- Deteksi tepi
- Transformasi morfologi

## Feature Engineering

- Fitur warna
- Fitur tekstur
- Fitur bentuk
- Fitur lokal berbasis gradien

Fondasi klasik tetap relevan untuk:

- memahami struktur citra;
- membangun baseline;
- menangani data terbatas;
- menghasilkan pipeline yang interpretabel;
- mengembangkan metode hybrid klasik-modern.

---

# Slide 08 - Keterbatasan Pendekatan Klasik

Pendekatan klasik memiliki keterbatasan yang mendorong lahirnya paradigma baru.

- Fitur buatan tangan tidak selalu mudah digeneralisasi ke domain baru.
- Kinerja bergantung pada pengetahuan peneliti terhadap domain.
- Sulit menangani variasi besar dalam sudut pandang, pencahayaan, skala, rotasi, dan oklusi.
- Feature engineering menjadi semakin sulit ketika objek dan konteks visual semakin kompleks.
- Pipeline terpisah tidak mengoptimalkan seluruh komponen secara end-to-end.

Namun, keterbatasan tidak berarti metode klasik kehilangan nilai ilmiahnya.

Justru baseline klasik membantu menjawab:

> Apakah kompleksitas metode modern benar-benar memberikan keuntungan yang bermakna?

---

# Slide 09 - Dari Piksel ke Representasi Visual

Citra dapat direpresentasikan pada berbagai tingkat abstraksi.

```text
Piksel
  ↓
Statistik intensitas / warna
  ↓
Handcrafted features
  ↓
Learned features
  ↓
Semantic embeddings
  ↓
Multimodal representations
```

## Representasi Piksel

- Menyimpan intensitas pada posisi tertentu.
- Sangat sensitif terhadap translasi, pencahayaan, noise, skala, dan rotasi.

## Representasi yang Lebih Baik Diharapkan Memiliki

- Invariansi terhadap variasi yang tidak relevan.
- Informasi yang diskriminatif.
- Dimensi yang efisien.
- Kemampuan generalisasi.

Pertanyaan penting dalam computer vision adalah:

> Representasi apa yang paling sesuai untuk masalah yang sedang dipelajari?

---

# Slide 10 - Handcrafted Feature sebagai Representasi

Pendekatan klasik merancang fitur untuk menangkap karakteristik tertentu.

| Fitur | Informasi Utama |
|---|---|
| Intensity / Color Histogram | Distribusi intensitas atau warna |
| HOG | Orientasi gradien dan struktur bentuk |
| LBP | Tekstur lokal |
| SIFT | Keypoint dan deskriptor lokal |

## Kelebihan

- Interpretabel.
- Efisien secara komputasi.
- Dapat bekerja dengan data terbatas.
- Cocok sebagai baseline.

## Keterbatasan

- Representasi ditentukan manusia.
- Informasi yang tidak dirancang sejak awal dapat hilang.
- Tidak selalu cukup untuk tugas semantik kompleks.

Praktikum pertemuan ini akan menggunakan **histogram intensitas** dan, sebagai ekstensi, **HOG** untuk memperlihatkan pengaruh representasi terhadap classifier.

---

# Slide 11 - Pipeline Klasik: Dari Citra ke Prediksi

Pipeline computer vision klasik bersifat modular.

```text
Input Image
   ↓
Preprocessing
   ↓
Feature Extraction
   ↓
Feature Vector
   ↓
Machine Learning Classifier
   ↓
Prediction
   ↓
Evaluation
```

Contoh:

```text
Digit Image
   ↓
Intensity Histogram
   ↓
Feature Vector
   ↓
kNN
   ↓
Digit Class
```

atau:

```text
Digit Image
   ↓
HOG
   ↓
Feature Vector
   ↓
Linear SVM
   ↓
Digit Class
```

Kinerja pipeline bergantung pada kualitas **data, representasi, classifier, dan evaluasi**.

---

# Slide 12 - Mengapa Baseline Penting dalam Penelitian?

Baseline bukan sekadar model sederhana.

Baseline menyediakan titik pembanding untuk menilai apakah metode baru benar-benar memberikan kontribusi.

```text
Baseline 0
Majority Class
      ↓
Baseline 1
Simple Feature + kNN
      ↓
Baseline 2
Stronger Classical Feature + SVM
      ↓
Modern Model
CNN / ViT / Foundation Model
```

Baseline membantu menjawab:

- Apakah dataset memiliki sinyal yang cukup kuat?
- Seberapa sulit problem sebenarnya?
- Apakah representasi sederhana sudah memadai?
- Apakah peningkatan berasal dari representasi, classifier, data, atau tuning?
- Berapa biaya komputasi tambahan untuk peningkatan performa?

Di tingkat doktoral, klaim metode baru harus dibandingkan dengan baseline yang relevan dan adil.

---

# Slide 13 - Deep Learning: Representasi yang Dipelajari dari Data

Deep learning membawa perubahan fundamental pada computer vision.

- CNN mempelajari fitur bertingkat dari data.
- Representasi dan classifier dapat dioptimalkan bersama secara end-to-end.
- Lapisan awal cenderung menangkap struktur sederhana seperti edge dan texture.
- Lapisan lebih dalam menangkap bagian objek dan representasi semantik.
- Transfer learning memungkinkan representasi pralatih digunakan pada domain dengan data terbatas.

Peran peneliti bergeser dari hanya merancang fitur menjadi merancang:

- data;
- arsitektur;
- objective function;
- strategi pembelajaran;
- evaluasi.

Detail CNN dibahas pada pertemuan khusus berikutnya.

---

# Slide 14 - Attention dan Vision Transformer

Vision Transformer menunjukkan bahwa self-attention dapat menjadi operator utama untuk representasi visual.

- Citra dibagi menjadi patch.
- Patch diproyeksikan menjadi token embedding.
- Positional information mempertahankan informasi posisi.
- Self-attention memodelkan hubungan antar-patch secara global.

## Konsekuensi Riset

- Kebutuhan data dan komputasi meningkat.
- Inductive bias berbeda dari CNN.
- Pretraining berskala besar menjadi semakin penting.
- Pertanyaan CNN versus transformer tidak dapat dijawab hanya dengan satu benchmark.

Detail arsitektur dibahas pada pertemuan berikutnya.

---

# Slide 15 - Self-Supervised Learning dan Foundation Vision Models

Ketersediaan label merupakan bottleneck penting dalam computer vision.

Self-supervised learning membangun sinyal pembelajaran dari data itu sendiri.

- Contrastive learning
- Masked image modeling
- Teacher-student learning
- Representation prediction

Model seperti DINO dan DINOv2 menunjukkan bahwa representasi visual yang kuat dapat dipelajari tanpa anotasi manual untuk setiap tugas.

Pertanyaan riset penting:

- Apa yang sebenarnya dipelajari model?
- Kapan fine-tuning diperlukan?
- Seberapa baik transfer ke domain khusus?
- Apakah representasi tetap robust terhadap distribution shift?

---

# Slide 16 - Multimodal Vision-Language Models

Riset mutakhir tidak lagi memandang citra terpisah dari bahasa.

Vision-language models menyelaraskan representasi visual dan teks.

- Image-text contrastive learning
- Zero-shot recognition
- Image-text retrieval
- Prompt-based adaptation
- Multimodal reasoning

CLIP menunjukkan bahwa bahasa alami dapat digunakan sebagai supervision untuk menghasilkan representasi visual yang fleksibel.

Isu terbuka:

- bias bahasa dan budaya;
- sensitivitas terhadap prompt;
- domain shift;
- reliability pada domain khusus.

---

# Slide 17 - Generative Vision dan Diffusion Models

Generative vision mengubah fokus dari hanya menganalisis citra menjadi juga menghasilkan dan memodifikasi citra.

Aplikasi:

- Text-to-image generation
- Image editing dan inpainting
- Super-resolution dan restoration
- Synthetic data generation
- Controlled generation

Pertanyaan penelitian:

- Apakah synthetic data meningkatkan generalisasi?
- Apakah synthetic data memperkuat bias?
- Bagaimana fidelity dan diversity dievaluasi?
- Bagaimana provenance dan lisensi dikelola?

---

# Slide 18 - 3D Vision dan Neural Rendering

Citra 2D merupakan proyeksi dari dunia 3D.

Riset 3D vision mencoba memulihkan atau merepresentasikan struktur ruang.

- Depth estimation
- Stereo dan multi-view geometry
- Point cloud processing
- Neural Radiance Fields (NeRF)
- Neural scene representation

Pertanyaan terbuka:

- Informasi 3D apa yang dapat dipulihkan dari satu atau beberapa citra?
- Bagaimana menangani oklusi?
- Bagaimana merepresentasikan scene dinamis?
- Bagaimana menggabungkan geometric prior dengan learned representation?

---

# Slide 19 - Trustworthy Computer Vision

Model yang akurat belum tentu dapat dipercaya.

Computer vision modern perlu mempertimbangkan:

- Explainability dan attribution
- Predictive uncertainty
- Calibration
- Robustness
- Distribution shift
- Fairness dan bias

Pertanyaan penting:

- Apakah explanation benar-benar merepresentasikan alasan prediksi?
- Bagaimana model berperilaku pada data di luar distribusi training?
- Apakah performa konsisten pada kelompok data yang berbeda?
- Bagaimana ketidakpastian dilaporkan kepada pengguna?

---

# Slide 20 - Peta Lanskap Riset Pengolahan Citra

| Area | Fokus | Waktu RPS |
|---|---|---|
| Representasi visual | CNN, attention, transformer | Pertemuan 3 |
| Pembelajaran representasi | Self-supervised, foundation models | Pertemuan 4 |
| Multimodal | Vision-language models | Pertemuan 5 |
| Restorasi | Image restoration, computational imaging | Pertemuan 6 |
| Deteksi | Object detection modern | Pertemuan 7 |
| Segmentasi | Foundation model segmentation | Pertemuan 8 |
| Generatif | Diffusion models | Pertemuan 9 |
| 3D | Multi-view geometry, neural rendering | Pertemuan 10 |
| Kepercayaan | Explainable, robust, trustworthy CV | Pertemuan 11 |

Peta ini bukan daftar teknologi.

Tujuannya adalah membantu mahasiswa menemukan **lokasi kontribusi ilmiah** yang potensial.

---

# Slide 21 - Satu Problem, Banyak Paradigma

Satu problem dapat diselesaikan dengan paradigma yang berbeda.

Contoh: klasifikasi citra.

```text
Dataset
 ├── Intensity / Color Histogram + kNN
 ├── HOG / LBP + SVM
 ├── CNN end-to-end
 ├── Pretrained CNN + fine-tuning
 ├── Vision Transformer
 └── Foundation Model / Vision-Language Model
```

Pertanyaan doktoral bukan hanya:

> Metode mana yang menghasilkan akurasi tertinggi?

Tetapi juga:

- Mengapa metode tersebut lebih baik?
- Pada kondisi apa keunggulan itu muncul?
- Apa biaya komputasinya?
- Apakah peningkatan tetap terjadi pada domain lain?
- Apakah metode kompleks benar-benar diperlukan?

---

# Slide 22 - Trade-off Antar Paradigma

Tidak ada metode yang terbaik untuk semua kondisi.

| Paradigma | Kekuatan | Keterbatasan |
|---|---|---|
| Handcrafted feature | Interpretabel, efisien | Representasi terbatas |
| CNN | Inductive bias lokal kuat | Konteks global memerlukan kedalaman |
| ViT | Hubungan global | Pretraining dan data besar penting |
| SSL | Mengurangi ketergantungan label | Sangat bergantung objective/augmentasi |
| VLM | Zero-shot dan fleksibel | Bias bahasa, domain shift, biaya tinggi |

Pemilihan metode harus mempertimbangkan:

- karakteristik data;
- jumlah label;
- sumber daya komputasi;
- tujuan aplikasi;
- reliability;
- kebutuhan interpretasi.

---

# Slide 23 - Peran Benchmark dalam Riset

Benchmark adalah salah satu mesin penggerak kemajuan computer vision.

Benchmark menyediakan:

- Dataset standar
- Task yang jelas
- Metrik evaluasi
- Baseline pembanding
- Prosedur eksperimen yang dapat direproduksi

Contoh task benchmark:

- Image classification
- Object detection
- Semantic segmentation
- Image restoration
- Image-text retrieval

Benchmark memungkinkan klaim kemajuan diuji secara kuantitatif.

Namun benchmark harus diperlakukan sebagai **alat ukur**, bukan tujuan akhir penelitian.

---

# Slide 24 - Mengapa Benchmark Saat Ini Belum Memadai

Benchmark memiliki banyak keterbatasan.

- Dataset mungkin tidak mewakili kompleksitas dunia nyata.
- Label dapat mengandung bias dan kesalahan.
- Satu metrik tidak menangkap seluruh kualitas sistem.
- Evaluasi sering dilakukan pada satu domain.
- Model dapat memanfaatkan shortcut atau artefak dataset.
- Skor tinggi belum tentu berarti kontribusi ilmiah yang kuat.

Pertanyaan doktoral:

- Apakah peningkatan metrik bermakna secara ilmiah?
- Apakah benchmark mengukur kemampuan yang sebenarnya ingin dipelajari?
- Apakah hasil dapat direproduksi?
- Apakah klaim tetap berlaku pada distribution shift?

---

# Slide 25 - Kontribusi Teknis versus Kontribusi Ilmiah

Kontribusi teknis dan kontribusi ilmiah saling berkaitan, tetapi tidak identik.

## Contoh Kontribusi Teknis

- Arsitektur baru
- Loss function baru
- Strategi augmentasi baru
- Optimasi hyperparameter
- Penerapan metode pada dataset baru

## Contoh Kontribusi Ilmiah

- Menjelaskan mengapa suatu metode bekerja.
- Menguji asumsi yang sebelumnya tidak diperiksa.
- Mengidentifikasi failure mode penting.
- Merumuskan problem baru.
- Menawarkan kerangka konseptual baru.

Disertasi doktoral biasanya membutuhkan lebih dari sekadar peningkatan angka pada satu benchmark.

---

# Slide 26 - Mengidentifikasi Masalah Terbuka

Masalah terbuka dapat berasal dari:

- Ketidaksesuaian asumsi model dengan kondisi dunia nyata.
- Kegagalan pada kasus tepi atau subgroup tertentu.
- Domain khusus yang tidak terwakili oleh model general-purpose.
- Biaya komputasi yang terlalu tinggi.
- Evaluasi yang tidak menangkap aspek penting.
- Bias dataset atau annotation artifact.
- Ketergantungan pada jumlah label yang besar.

Latihan:

1. Pilih satu area computer vision.
2. Tuliskan tiga masalah yang menurut Anda belum terselesaikan.
3. Jelaskan bukti atau pengamatan yang mendukung dugaan tersebut.

---

# Slide 27 - Memetakan Area Riset dan Minat Disertasi

| No | Langkah | Pertanyaan |
|---|---|---|
| 1 | Identifikasi minat | Area apa yang paling menarik dan relevan? |
| 2 | Kenali masalah | Masalah apa yang ingin dipecahkan? |
| 3 | Cari pemain utama | Siapa peneliti dan kelompok riset penting? |
| 4 | Petakan metode | Metode apa yang dominan dan apa keterbatasannya? |
| 5 | Kenali benchmark | Dataset dan metrik apa yang digunakan? |
| 6 | Tentukan posisi | Kontribusi apa yang unik dan layak diuji? |

Pemetaan awal tidak perlu sempurna.

Peta akan diperbarui sepanjang semester berdasarkan paper, eksperimen, dan diskusi.

---

# Slide 28 - Research Log: Tujuan dan Format

Research log adalah catatan berpikir dan bekerja sepanjang proses penelitian.

## Tujuan

- Mendokumentasikan keputusan penelitian.
- Melacak perkembangan ide.
- Menyimpan pertanyaan yang belum terjawab.
- Mencatat hasil eksperimen, kegagalan, dan pelajaran.
- Menjadi bahan diskusi dengan dosen dan kolega.

## Format

- Markdown
- Jupyter Notebook
- Repository Git
- Dokumen bersama

Yang terpenting adalah konsistensi dan kemampuan menelusuri kembali alasan sebuah keputusan.

---

# Slide 29 - Workflow Research Log

Gunakan format sederhana berikut.

```text
1. Area/topik yang sedang dieksplorasi
2. Dataset dan sumbernya
3. Pertanyaan penelitian sementara
4. Temuan dari eksplorasi data
5. Baseline dan hasil awal
6. Failure cases
7. Asumsi yang digunakan
8. Risiko metodologis
9. Interpretasi
10. Langkah eksperimen berikutnya
```

Research log tidak harus formal.

Namun keputusan penting, perubahan metode, dan kegagalan eksperimen harus dapat ditelusuri.

---

# Slide 30 - Praktikum 01: Tujuan dan Alur Eksperimen

## Judul

**Image Dataset Exploration, Bias Analysis, and Classical Baseline**

## Tujuan

- Memahami struktur dan karakteristik awal dataset citra.
- Mengidentifikasi distribusi kelas, variasi visual, kualitas, dan potensi bias.
- Membangun baseline klasik sebagai titik pembanding eksperimen berikutnya.
- Menginterpretasikan hasil dan failure cases sebagai bahan pertanyaan penelitian awal.

## Alur Praktikum

```text
Dataset
  ↓
Dataset Exploration & Audit
  ↓
Classical Baseline
  ↓
Evaluation & Failure Analysis
  ↓
Research Interpretation
```

> Langkah teknis, kode lengkap, dan tugas eksperimen dijelaskan pada modul praktikum terpisah.

---

# Slide 31 - Praktikum 01: Eksplorasi dan Audit Dataset

## Fokus Eksplorasi

- Struktur dataset: jumlah data, kelas, ukuran, channel, dan format citra.
- Distribusi kelas dan potensi class imbalance.
- Visualisasi sampel dari setiap kelas.
- Variasi visual: intensitas, kontras, blur, noise, pencahayaan, dan oklusi.
- Potensi bias atau shortcut yang berkorelasi dengan label.

## Tools dan Dataset Demo

- Python, NumPy, Matplotlib, scikit-learn, scikit-image, dan OpenCV.
- Demo dapat menggunakan `load_digits()` dari scikit-learn.
- Dataset riset mahasiswa dapat digunakan sebagai perluasan.

## Pertanyaan Kunci

- Apakah dataset cukup representatif?
- Apakah ada kelas yang dominan atau langka?
- Apakah terdapat artefak visual yang dapat menjadi shortcut bagi model?

---

# Slide 32 - Praktikum 01: Baseline Klasik

## Baseline yang Dibangun

```text
Baseline 0
Majority Class
      ↓
Baseline 1
Intensity Histogram + kNN
      ↓
Baseline 2
HOG + Linear SVM
```

## Tujuan Perbandingan

- **Majority baseline** menunjukkan performa minimum tanpa representasi visual.
- **Intensity histogram** menangkap distribusi intensitas, tetapi tidak struktur spasial.
- **HOG** menangkap informasi gradien dan bentuk lokal.

## Prinsip Eksperimen

Gunakan pembagian data dan metrik yang sama agar perbandingan adil dan reproducible.

> Baseline ini bukan tujuan akhir, tetapi titik referensi untuk menilai apakah model yang lebih kompleks benar-benar diperlukan.

---

# Slide 33 - Praktikum 01: Evaluasi, Failure Analysis, dan Refleksi Riset

## Evaluasi

Bandingkan baseline menggunakan:

- Accuracy dan balanced accuracy.
- Confusion matrix.
- Kesalahan antar kelas.
- Contoh failure cases.

## Interpretasi

Jangan berhenti pada angka performa.

Pertanyakan:

1. Representasi mana yang paling informatif dan mengapa?
2. Kelas mana yang paling sering tertukar?
3. Apakah error berasal dari kualitas data, representasi, classifier, atau bias dataset?
4. Apakah model yang lebih kompleks berpotensi memberikan manfaat yang berarti?
5. Eksperimen berikutnya apa yang paling informatif?

## Luaran Praktikum

```text
Observation
   ↓
Failure / Limitation
   ↓
Assumption
   ↓
Research Question
   ↓
Next Experiment
```

Catat hasil utama pada **research log** sebagai bahan menuju identifikasi research gap pada pertemuan berikutnya.

---

# Slide 34 - Menyusun Asumsi dan Risiko Awal

Setiap eksperimen dibangun di atas asumsi.

## Contoh Asumsi

- Dataset cukup mewakili domain target.
- Label cukup benar dan konsisten.
- Train-test split tidak mengandung leakage.
- Representasi yang dipilih relevan dengan signal utama.
- Metrik evaluasi sesuai dengan tujuan aplikasi.

## Contoh Risiko

- Dataset terlalu kecil.
- Distribusi data berubah.
- Class imbalance menyesatkan accuracy.
- Model memanfaatkan shortcut.
- Evaluasi hanya pada satu dataset.
- Sumber daya komputasi membatasi reproduksibilitas.

Tuliskan asumsi dan risiko secara eksplisit dalam research log.

---

# Slide 35 - Dari Failure Case ke Research Question

Failure case bukan hanya kesalahan model.

Failure case dapat menjadi petunjuk masalah penelitian.

```text
Observation
"Digit tertentu sering tertukar"
        ↓
Possible cause
"Representasi kurang menangkap struktur bentuk"
        ↓
Hypothesis
"Representasi dengan spatial structure lebih robust"
        ↓
Experiment
Histogram vs HOG vs learned representation
        ↓
Research question
"Kapan representasi yang lebih kompleks memberi keuntungan?"
```

Di domain riset nyata, logika yang sama dapat digunakan untuk masalah:

- domain shift;
- low-light imaging;
- medical imaging;
- remote sensing;
- rare classes;
- multimodal data.

---

# Slide 36 - Target Luaran Pertemuan 01

Pada akhir pertemuan, mahasiswa diharapkan memiliki empat luaran.

## Luaran 1 - Peta Awal Area Riset

- Area utama yang diminati.
- Subarea yang relevan.
- Kandidat metode dan paper penting.

## Luaran 2 - Dataset Audit dan Baseline Notebook

- Struktur dan distribusi data.
- Visualisasi sampel.
- Potensi quality issue dan bias.
- Baseline sederhana.
- Evaluasi dan failure cases.

## Luaran 3 - Tiga sampai Lima Masalah Potensial

- Ditulis sebagai problem atau gap awal.
- Disertai alasan mengapa problem penting.

## Luaran 4 - Asumsi, Risiko, dan Next Experiment

- Asumsi utama.
- Risiko metodologis.
- Satu eksperimen lanjutan yang logis.

---

# Slide 37 - Refleksi dan Diskusi

Pertanyaan refleksi:

- Bagian peta riset mana yang paling relevan dengan minat Anda?
- Mengapa Anda memilih metode tertentu?
- Apakah baseline klasik masih relevan untuk problem Anda?
- Jika model sederhana sudah sangat baik, apa justifikasi menggunakan model kompleks?
- Failure case apa yang paling menarik?
- Benchmark apa yang seharusnya digunakan?
- Bukti apa yang diperlukan untuk mempercayai suatu klaim?
- Risiko apa yang berpotensi membuat kesimpulan eksperimen salah?

Pada jenjang doktoral, kualitas pertanyaan dan argumen sama pentingnya dengan kemampuan implementasi.

---

# Slide 38 - Tugas dan Bukti Belajar

## Tugas

Pilih satu dataset citra yang relevan dengan minat riset.

Lakukan:

1. Dataset audit.
2. Visualisasi distribusi kelas.
3. Visualisasi sampel tiap kelas.
4. Analisis kualitas citra.
5. Identifikasi potensi bias atau shortcut.
6. Bangun minimal satu baseline klasik yang sesuai.
7. Evaluasi dengan metrik yang relevan.
8. Tampilkan confusion matrix atau failure cases.
9. Tulis interpretasi hasil.
10. Rumuskan minimal dua pertanyaan penelitian awal.

## Bukti Belajar

- Jupyter Notebook / Google Colab.
- Research log.
- Ringkasan hasil dan rekomendasi eksperimen berikutnya.

---

# Slide 39 - Persiapan untuk Pertemuan Berikutnya

Pertemuan 02 membahas **critical paper reading dan identifikasi research gap**.

Sebelum pertemuan berikutnya:

- Pilih satu paper penting pada area yang diminati.
- Baca abstrak, pendahuluan, metode utama, eksperimen, dan kesimpulan.
- Catat klaim utama.
- Identifikasi baseline yang digunakan.
- Periksa dataset dan metrik evaluasi.
- Catat satu failure case, limitation, atau asumsi yang belum diuji.

Gunakan hasil eksplorasi dataset dan baseline pertemuan ini untuk mempertajam pertanyaan saat membaca paper.

---

# Slide 40 - Kaitan dengan Capaian Pembelajaran

| CPMK | Kontribusi Pertemuan 01 |
|---|---|
| CPMK-1 | Menganalisis evolusi dan perkembangan riset pengolahan citra / computer vision |
| CPMK-4 | Menggunakan tool untuk eksplorasi dan eksperimen awal |
| CPMK-5 | Mulai merumuskan masalah penelitian dan research gap |
| CPMK-6 | Membangun peta literatur dan positioning awal penelitian |

Peta riset memberikan konteks.

Praktikum memberikan bukti empiris.

Research log menghubungkan keduanya menjadi proses penelitian yang dapat ditelusuri.

---

# Slide 41 - Ringkasan Materi Pertemuan 01

Pesan utama:

- Pengolahan citra berkembang dari manipulasi piksel menuju learned representation dan foundation models.
- Evolusi metode juga merupakan evolusi cara merepresentasikan informasi visual.
- Handcrafted feature tetap penting sebagai fondasi dan baseline.
- Model kompleks harus dibandingkan dengan baseline yang masuk akal.
- Dataset harus diaudit sebelum modeling.
- Class distribution, image quality, bias, dan shortcut dapat memengaruhi kesimpulan ilmiah.
- Evaluation harus dilanjutkan dengan failure analysis.
- Failure case dapat menjadi sumber hypothesis dan research question.
- Research log membantu mengubah eksperimen menjadi proses penelitian yang sistematis.

---

# Slide 42 - Penutup

TERIMA KASIH

Pertemuan berikutnya:

**Critical Paper Reading dan Identifikasi Research Gap**

---
