# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 08
# Segmentasi Citra dan Promptable Foundation Models

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan dalam Rangkaian Perkuliahan

## Kedudukan Pertemuan 08

| Fase | Pertemuan | Fokus |
|---|---|---|
| Sebelumnya | 01–07 | Peta riset, critical reading, representasi visual, SSL, vision-language, restoration, deteksi objek |
| **Saat ini** | **08** | **Segmentasi citra dan promptable foundation models** |
| Berikutnya | 09–16 | Generative vision, 3D vision, trustworthy, desain eksperimen, proposal disertasi |

## Hubungan dengan Materi Lain

- Pertemuan 07 membahas object detection modern dengan YOLO dan transformer.
- Pertemuan 08 melanjutkan dari **deteksi berbasis kotak** menuju **prediksi mask per-pixel**.
- Konsep Vision Transformer dan self-supervised learning dari Pertemuan 03 dan 04 menjadi fondasi arsitektur Segment Anything Model (SAM).
- Pertemuan 09 akan membahas diffusion models untuk generative vision, yang dapat memanfaatkan segmentasi sebagai conditioning.

---

# Slide 02 - Tujuan Pembelajaran dan Capaian

## Capaian yang Diharapkan

- Membedakan **semantic segmentation**, **instance segmentation**, dan **panoptic segmentation**.
- Menjelaskan arsitektur **U-Net**, **transformer segmentation**, dan **SAM**.
- Memahami peran prompt pada **promptable foundation model**.
- Mengevaluasi kualitas mask menggunakan **IoU** dan **Dice**.
- Menganalisis generalisasi segmentasi lintas domain dan pengaruh **domain shift**.

## Keterkaitan Capaian

- **CPMK-1**: analisis perkembangan mutakhir segmentasi.
- **CPMK-2**: evaluasi kritis paper SAM dan metode segmentasi lain.
- **CPMK-3**: merancang eksperimen evaluasi prompt.
- **CPMK-4**: memanfaatkan SAM dalam ekosistem PyTorch/Hugging Face.

---

# Slide 03 - Agenda Pertemuan

## Alur Pembahasan

1. Konsep dasar dan taksonomi segmentasi citra.
2. Metrik evaluasi: IoU, Dice, dan mask quality.
3. Arsitektur U-Net dan transformer segmentation.
4. Promptable segmentation dan arsitektur SAM.
5. Perbandingan supervised segmentation vs promptable segmentation.
6. Eksperimen SAM dengan berbagai tipe prompt.
7. Diskusi penelitian: domain shift, kualitas prompt, dan keterbatasan evaluasi.

## Target Keluaran

- Evaluasi strategi prompt pada SAM.
- Rekomendasi penggunaan SAM dalam pipeline riset masing-masing.

---

# Slide 04 - Dari Deteksi ke Segmentasi

## Recap Singkat Pertemuan 07

- Object detection menghasilkan **bounding box** dan label kelas.
- YOLO melakukan deteksi one-stage secara langsung.
- Detector berbasis transformer seperti DETR memanfaatkan attention secara global.

## Pertanyaan Transisi

- Bounding box hanya memberikan batas kasar objek.
- Banyak aplikasi membutuhkan **batas presisi per-pixel**, misalnya pemisahan sel, lesi, kendaraan, atau area bangunan.

## Kontinuum Representasi

```text
Bounding box  ->  Instance mask  ->  Semantic mask  ->  Panoptic mask
(kasar)            (presisi)          (per-kelas)         (semua piksel)
```

Segmentasi adalah tugas prediksi **di level piksel**, bukan hanya level objek.

---

# Slide 05 - Deteksi vs Segmentasi: Output yang Dihasilkan

## Perbandingan Umum

| Aspek | Object Detection | Segmentation |
|---|---|---|
| Output | Bounding box + label | Mask per-pixel + label |
| Batas objek | Kasar, berbentuk kotak | Presisi mengikuti kontur |
| Metrik umum | mAP, precision-recall | IoU, Dice, PQ |
| Kompleksitas anotasi | Relatif ringan | Lebih mahal dan detail |
| Penggunaan | Menghitung objek | Analisis bentuk, area, kontur |

## Ilustrasi Sederhana

```text
Deteksi:   [------]
           | mobil|
           [------]

Segmentasi:  /~~~~~\
            | mobil |
             \_____/
```

Segmentasi menyediakan informasi yang lebih kaya untuk pengukuran ilmiah.

---

# Slide 06 - Taksonomi Segmentasi Citra

## Tiga Paradigma Utama

```text
                       Segmentasi Citra
                              |
        +---------------------+----------------------+
        |                     |                      |
 Semantic              Instance                Panoptic
 Segmentation         Segmentation             Segmentation
        |                     |                      |
   semua piksel       setiap objek           stuff + things
   dikelompokkan      diberi mask            digabungkan
   berdasarkan        terpisah per           dalam satu
   kelas              individu               keluaran
```

## Penjelasan Singkat

- **Semantic segmentation**: label per-pixel tanpa membedakan individu.
- **Instance segmentation**: mask per objek, hanya untuk objek yang dapat dihitung.
- **Panoptic segmentation**: menggabungkan semantic dan instance dalam satu prediksi.

---

# Slide 07 - Semantic Segmentation

## Definisi

- Setiap piksel pada citra diberi satu label kelas.
- Semua piksel dengan kelas yang sama memiliki label yang sama.
- Tidak membedakan dua objek berbeda yang berada pada kelas yang sama.

## Contoh Tugas

- Segmentasi area jalan, bangunan, kendaraan, pohon pada citra perkotaan.
- Segmentasi organ atau jaringan pada citra medis.

## Ilustrasi

```text
Citra           Label per-pixel
[RGB]    ->     [jalan][jalan][pohon]
                [jalan][mobil][pohon]
```

## Metrik Khas

- Mean IoU (mIoU) antar kelas.
- Akurasi per-pixel dapat menyesatkan jika kelas tidak seimbang.

---

# Slide 08 - Instance Segmentation

## Definisi

- Prediksi mask untuk **setiap individu objek**.
- Berbeda dengan semantic segmentation, dua mobil yang berdampingan memiliki mask terpisah.
- Umumnya hanya mencakup objek tipe "things", bukan area latar seperti langit atau jalan.

## Contoh Tugas

- Segmentasi setiap orang dalam kerumunan.
- Segmentasi setiap sel dalam citra mikroskop.

## Ilustrasi

```text
Citra dengan 2 mobil

Mobil kiri:  mask A
Mobil kanan: mask B

Tidak digabung menjadi satu label "mobil".
```

## Metrik Khas

- Average Precision (AP) berbasis mask.
- IoU per instance.

---

# Slide 09 - Panoptic Segmentation

## Definisi

- Menggabungkan semantic dan instance segmentation dalam satu representasi.
- **Stuff**: area yang tidak memiliki individu jelas, misalnya langit, jalan, tembok.
- **Things**: objek yang dapat dihitung dan memiliki identitas individu, misalnya mobil, orang.

## Keluaran

- Setiap piksel mendapatkan label semantik.
- Setiap piksel pada objek "things" juga mendapatkan ID instance.

## Metrik Khas

- **Panoptic Quality (PQ)**.
- PQ menggabungkan recognition quality dan segmentation quality.

## Contoh

```text
[langit][bangunan][mobil-1][mobil-2][jalan]
  stuff     stuff    thing    thing   stuff
```

---

# Slide 10 - Perbandingan Tiga Paradigma Segmentasi

## Tabel Ringkas

| Aspek | Semantic | Instance | Panoptic |
|---|---|---|---|
| Unit prediksi | Piksel | Objek | Piksel + objek |
| Membedakan instance | Tidak | Ya | Ya |
| Mencakup stuff | Ya | Tidak | Ya |
| Jenis kelas | Semua | Things | Stuff + things |
| Contoh metode | U-Net, DeepLab | Mask R-CNN | Mask2Former, Panoptic FPN |

## Implikasi Penelitian

- Pemilihan paradigma bergantung pada kebutuhan ilmiah.
- Segmentasi medis umumnya semantik atau instance.
- Segmentasi scene understanding cenderung panoptic.
- Foundation model seperti SAM berfokus pada **mask objek** dan dapat digunakan untuk mendukung semua paradigma.

---

# Slide 11 - Metrik Evaluasi: IoU

## Definisi

IoU mengukur tumpang tindih antara mask prediksi dan mask ground truth.

```text
IoU = |A ∩ B| / |A ∪ B|
```

- `A`: himpunan piksel hasil prediksi.
- `B`: himpunan piksel ground truth.
- Nilai berkisar antara 0 dan 1.

## Karakteristik

- Sensitif terhadap kesalahan kecil pada batas objek.
- Tidak membedakan false positive dan false negative secara terpisah.
- Umum digunakan pada semantic dan instance segmentation.

## Variasi

- **mIoU**: rata-rata IoU seluruh kelas.
- **IoU per instance**: untuk evaluasi mask pada instance segmentation.

---

# Slide 12 - Metrik Evaluasi: Dice

## Definisi

Dice similarity coefficient mengukur kesamaan dua himpunan piksel.

```text
Dice = 2|A ∩ B| / (|A| + |B|)
```

## Contoh Ilustrasi

```text
|A| = 10 piksel, |B| = 10 piksel, |A ∩ B| = 6

IoU  = 6 / (10 + 10 - 6) = 6/14  ≈ 0.429
Dice = 2*6 / (10 + 10)   = 12/20 = 0.600
```

## Karakteristik

- Dice lebih "generous" daripada IoU untuk overlap yang sama.
- Sering digunakan pada segmentasi biomedis karena data tidak seimbang.
- Hubungan: `Dice = 2IoU / (1 + IoU)`.

---

# Slide 13 - Mask Quality dan Kualitas Anotasi

## Apa yang Diukur IoU/Dice?

- Overlap antar piksel, bukan kehalusan kontur.
- Dua mask dapat memiliki IoU sama tetapi kualitas batas berbeda.

## Sumber Kesalahan pada Mask

- Boundary error pada tepi objek.
- Anotasi kasar atau tidak konsisten antar annotator.
- Label tidak sempurna pada objek kecil atau tepi yang samar.

## Pertanyaan Kunci untuk Penelitian

- Apakah peningkatan IoU mencerminkan pemahaman objek yang lebih baik?
- Bagaimana anotasi tidak sempurna memengaruhi evaluasi?
- Apakah IoU/Dice cukup untuk mengukur kualitas mask pada domain ilmiah?

---

# Slide 14 - U-Net: Arsitektur Encoder-Decoder

## Latar Belakang

- Diusulkan oleh Ronneberger et al. untuk segmentasi biomedis.
- Populer karena bekerja baik dengan data pelatihan terbatas.

## Struktur Utama

```text
Encoder (kontraksi)          Decoder (ekspansi)
   [Conv + Pool]      [Skip]   [UpConv + Conv]
        |                |           |
   fitur resolusi       fitur      fitur
   rendah +             dari       resolusi
   semantik            encoder     tinggi +
                       detail      semantik
```

## Ide Kunci

- Encoder menangkap konteks semantik.
- Decoder memulihkan resolusi spasial.
- Skip connection mempertahankan detail tepi.

---

# Slide 15 - U-Net: Skip Connection dan Peranannya

## Mengapa Skip Connection Penting?

- Informasi lokasi hilang saat downsampling.
- Skip connection mengalirkan fitur resolusi tinggi dari encoder ke decoder.
- Decoder dapat memanfaatkan detail tepi dan lokasi objek.

## Ilustrasi

```text
Encoder level 2: fitur tepi halus  ----+
                                        |  concatenation
Decoder level 2: fitur semantik  -------+----> prediksi mask
```

## Keterbatasan

- Receptive field encoder bergantung pada kedalaman jaringan.
- U-Net murni CNN memiliki keterbatasan dalam menangkap ketergantungan jarak jauh.
- Arsitektur transformer kemudian digunakan untuk mengatasi keterbatasan ini.

---

# Slide 16 - Dari CNN ke Transformer untuk Segmentasi

## Recap Pertemuan 03

- CNN bekerja dengan filter lokal dan inductive bias spasial.
- Vision Transformer menggunakan patch embedding dan self-attention global.

## Motivasi Transformer pada Segmentasi

- Segmentasi membutuhkan pemahaman konteks global dan hubungan antar objek.
- Self-attention dapat menghubungkan piksel berjauhan secara langsung.

## Contoh Pendekatan

- ViT-based encoder untuk menggantikan backbone CNN.
- TransUNet: menggabungkan U-Net dengan transformer.
- Swin Transformer: perhatian dalam window dengan hierarki.
- Mask2Former: framework universal untuk semantic, instance, dan panoptic segmentation.

---

# Slide 17 - Arsitektur Transformer Segmentation Modern

## Komponen Umum

```text
Citra
  |
  v
Backbone (ViT/Swin/CNN)
  |
  v
Transformer Decoder / Pixel Decoder
  |
  v
Mask Prediction
```

## Peran Masing-Masing

| Komponen | Fungsi |
|---|---|
| Backbone | Mengekstrak representasi visual |
| Transformer decoder | Menghubungkan query objek dengan fitur |
| Pixel decoder | Memetakan fitur ke resolusi penuh |
| Mask head | Menghasilkan mask biner per objek atau per kelas |

## Catatan

- Transformer memberi fleksibilitas untuk segmentasi universal.
- Biaya komputasi lebih tinggi daripada CNN murni.
- Pretraining dan skala data sangat menentukan performa.

---

# Slide 18 - Supervised Segmentation: Pipeline dan Transfer Learning

## Pipeline Umum

```text
Dataset berlabel mask
       |
       v
Pretrained backbone + segmentation head
       |
       v
Loss: Cross-entropy + Dice / Focal
       |
       v
Evaluasi: mIoU, Dice, PQ
```

## Transfer Learning pada Segmentasi

- Backbone sering diinisialisasi dari ImageNet atau model self-supervised seperti DINOv2.
- Head segmentasi dilatih dari awal.

## Praktik yang Perlu Diperhatikan

- Jangan membandingkan metode dengan backbone berbeda tanpa kontrol.
- Ablation diperlukan untuk memisahkan kontribusi arsitektur, loss, dan strategi augmentasi.
- Baseline terkuat harus dipertimbangkan sebelum mengklaim kebaruan.

---

# Slide 19 - Keterbatasan Segmentasi Supervised

## Kebutuhan Anotasi

- Mask per-pixel jauh lebih mahal daripada bounding box.
- Dataset besar seperti COCO dan Cityscapes membutuhkan waktu anotasi besar.

## Closed-Set Limitation

- Model dilatih pada daftar kelas tertentu.
- Kelas baru membutuhkan data dan pelatihan ulang.
- Evaluasi hanya mencakup kelas yang ada pada set pelatihan.

## Generalisasi

- Performa menurun ketika domain berubah, misalnya dari foto alami ke citra satelit.
- Model supervised cenderung menghafal pola domain pelatihan.
- Hal ini menjadi motivasi utama pengembangan promptable foundation model.

---

# Slide 20 - Promptable Segmentation: Gagasan Dasar

## Definisi

- Model segmentasi yang menerima **prompt** sebagai masukan tambahan.
- Prompt dapat berupa titik, kotak, mask awal, atau kombinasi.
- Model menghasilkan mask yang sesuai dengan prompt.

## Perbedaan dengan Supervised Segmentation

| Aspek | Supervised | Promptable |
|---|---|---|
| Input | Citra | Citra + prompt |
| Kelas | Ditentukan pelatihan | Tidak perlu kelas tetap |
| Interaksi | Tidak ada | Dapat iteratif |
| Tujuan utama | Memetakan piksel ke kelas | Mengikuti keinginan pengguna |

## Kontribusi Ilmiah Potensial

- Bagaimana kualitas prompt memengaruhi hasil segmentasi?
- Bagaimana model menginterpretasi prompt yang ambigu?
- Bagaimana promptable model digunakan dalam pipeline anotasi?

---

# Slide 21 - Foundation Model untuk Segmentasi: SAM

## Apa itu SAM?

- **Segment Anything Model (SAM)**, Kirillov et al., 2023.
- Dikembangkan oleh Meta AI Research.
- Bertujuan menjadi foundation model untuk segmentasi gambar.

## Ide Utama

- Mendukung **promptable segmentation** dengan titik, kotak, dan mask.
- Memberikan solusi zero-shot untuk objek di luar kelas yang dipelajari.
- Dapat digunakan sebagai komponen dalam sistem yang lebih besar.

## Konsep "Segment Anything"

```text
Citra + prompt  ->  SAM  ->  Mask yang sesuai
```

Tidak terikat pada daftar kelas tertutup.

---

# Slide 22 - Arsitektur SAM: Ringkasan

## Tiga Komponen Utama

```text
Citra
  |
  v
Image Encoder (ViT)          Prompt (titik/kotak/mask)
  |                                     |
  v                                     v
Image Embedding                Prompt Encoder
  |                                     |
  +---------------> Mask Decoder <------+
                       |
                       v
                 Mask + IoU score
```

## Penjelasan

| Komponen | Fungsi |
|---|---|
| Image encoder | Mengekstrak embedding citra |
| Prompt encoder | Mengkodekan prompt pengguna |
| Mask decoder | Menggabungkan embedding dan prompt menjadi mask |

## Keunggulan Desain

- Image embedding dapat dihitung sekali untuk banyak prompt.
- Mask decoder ringan sehingga interaksi menjadi cepat.

---

# Slide 23 - Image Encoder SAM

## Spesifikasi

- Menggunakan **Vision Transformer (ViT)**.
- Dimodifikasi untuk resolusi tinggi, misalnya 1024x1024.
- Pretrained dengan pendekatan **MAE** (Masked Autoencoder).

## Hubungan dengan Materi Pertemuan 04

- MAE adalah salah satu pendekatan self-supervised learning.
- SAM memanfaatkan representasi visual yang kuat tanpa label segmentasi.

## Peran dalam Alur

```text
Citra input -> Image Encoder -> image embedding (large feature map)
```

- Embedding ini dapat digunakan ulang untuk banyak prompt.
- Karena berat secara komputasi, image encoding biasanya dilakukan lebih dulu.

---

# Slide 24 - Prompt Encoder SAM

## Jenis Prompt yang Dikodekan

| Prompt | Representasi |
|---|---|
| Point | Sparse embedding, termasuk label positif/negatif |
| Box | Sparse embedding, kode untuk kiri-atas dan kanan-bawah |
| Mask | Dense embedding, dimasukkan ke decoder sebagai informasi tambahan |

## Karakteristik

- Setiap prompt diubah menjadi vektor atau peta fitur.
- Prompt encoder relatif ringan.
- Beberapa prompt dapat dikombinasikan.

## Implikasi Penelitian

- Model tidak melihat teks label seperti CLIP.
- SAM menentukan objek dari **lokasi dan keinginan pengguna**, bukan semantik kelas.
- Prompt yang berbeda dapat menghasilkan mask yang berbeda untuk piksel yang sama.

---

# Slide 25 - Mask Decoder SAM

## Fungsi

- Menerima image embedding dan prompt embedding.
- Menghasilkan mask prediksi dan estimasi kualitas mask.
- Menggunakan mekanisme attention dan transposed convolution.

## Keluaran

```text
Mask biner: objek yang diminta pengguna
IoU score: perkiraan kualitas mask
```

## Catatan Penting

- Decoder dapat dipanggil berulang kali.
- Iterasi berikutnya dapat memanfaatkan mask sebelumnya sebagai prompt.
- Proses ini mendukung interaksi human-in-the-loop.

---

# Slide 26 - Ambiguity dan Multi-Mask Output

## Masalah Ambigu Prompt

- Satu titik dapat merujuk ke banyak objek yang valid.
- Satu titik pada gambar sebatang pohon dapat berarti seluruh batang, seluruh tajuk, atau satu cabang.

## Solusi SAM

- Mask decoder menghasilkan beberapa mask sekaligus.
- Setiap mask disertai skor IoU prediksi.
- Pengguna dapat memilih mask yang paling sesuai.

## Ilustrasi

```text
Point prompt di tengah objek

Output:
mask A: seluruh objek
mask B: sub-bagian objek
mask C: objek lebih besar yang memuat titik
```

## Pertanyaan Riset

- Bagaimana memilih mask yang tepat secara otomatis?
- Bagaimana ambiguitas ini memengaruhi evaluasi IoU terhadap ground truth?

---

# Slide 27 - Jenis Prompt pada SAM

## Perbandingan Tipe Prompt

| Tipe Prompt | Deskripsi | Sensitivitas |
|---|---|---|
| Point | Satu atau beberapa titik pada objek | Tinggi terhadap posisi |
| Box | Kotak seputar objek | Lebih stabil dibanding titik |
| Mask | Mask awal dari model lain | Bergantung pada kualitas input |
| Kombinasi | Titik + box + mask | Umumnya meningkat |

## Prompt Teks

- SAM asli tidak memproses teks.
- Prompt teks dapat dihubungkan melalui model grounding seperti Grounding DINO.
- Hasil grounding berupa box yang kemudian menjadi prompt SAM.

## Implikasi Eksperimen

- Eksperimen prompt harus memvariasikan posisi titik, ukuran box, dan kombinasi prompt.
- Perbandingan harus memperhitungkan random seed dan protokol yang konsisten.

---

# Slide 28 - Workflow Promptable Segmentation

## Alur Interaktif

```text
1. Muat citra
2. Hitung image embedding
3. Pengguna memberikan prompt awal
4. SAM menghasilkan mask + skor IoU
5. Pengguna mengevaluasi mask
6. Jika kurang tepat, tambahkan prompt koreksi
7. Ulangi hingga mask diterima
```

## Pseudocode Sederhana

```text
image_embedding = image_encoder(image)
loop:
    prompt = user_input()  # point, box, mask
    mask, score = mask_decoder(image_embedding, prompt)
    if user_accept(mask):
        break
```

## Kaitan dengan Praktikum

- Praktikum akan menerapkan alur ini pada subset data.
- Evaluasi dilakukan dengan IoU dan Dice terhadap referensi.

---

# Slide 29 - SA-1B: Dataset Segment Anything

## Skala Dataset

- SA-1B terdiri dari sekitar 11 juta citra.
- Berisi sekitar 1 miliar mask.
- Dibangun menggunakan pipeline model + manusia.

## Proses Pembuatan

- Model SAM awal digunakan untuk menghasilkan mask.
- Annotator mengoreksi mask melalui prompt interaktif.
- Iterasi ini meningkatkan kualitas dan jumlah label.

## Catatan Kritis

- Mask bukan ground truth sempurna.
- Beberapa mask dapat berasal dari prediksi model yang disetujui manusia.
- Distribusi objek tidak seimbang dan tidak mewakili semua domain.

## Implikasi untuk Riset

- Evaluasi menggunakan SA-1B atau dataset turunannya harus dilakukan secara hati-hati.
- Mask SAM pada data sendiri sebaiknya tidak langsung dianggap sebagai label emas tanpa validasi.

---

# Slide 30 - Human-in-the-Loop untuk Anotasi

## Konsep Dasar

- Manusia memberikan prompt, bukan membuat mask piksel demi piksel.
- Model menghasilkan mask kandidat.
- Manusia mengoreksi hanya pada bagian yang salah.

## Keuntungan

- Waktu anotasi jauh lebih cepat daripada menggambar mask manual.
- Memungkinkan anotasi objek baru tanpa pelatihan model baru.
- Cocok untuk membangun dataset domain spesifik.

## Tantangan

- Kualitas hasil tetap bergantung pada kejelian manusia.
- Kesalahan kecil pada awal dapat tersebar melalui iterasi.
- Anotasi dari foundation model dapat mengandung bias sistemik.

---

# Slide 31 - SAM sebagai Annotator: Peluang dan Risiko

## Peluang

- Mempercepat produksi dataset.
- Berguna untuk objek yang sulit diakses oleh annotator manusia.
- Dapat diintegrasikan dengan segmentasi supervised untuk meningkatkan produktivitas.

## Risiko

- Mask cenderung mengikuti kontur yang dipelajari dari data alami.
- Domain yang sangat berbeda dapat menghasilkan mask tidak akurat.
- Penggunaan mask SAM sebagai ground truth dapat membuat evaluasi menjadi bias.

## Rekomendasi Awal

- Selalu validasi mask hasil SAM pada sebagian subset data.
- Laporkan persentase mask yang perlu koreksi.
- Gunakan IoU/Dice terhadap anotasi manual untuk mengukur keandalan.

---

# Slide 32 - Supervised vs Promptable Segmentation

## Perbandingan untuk Kebutuhan Riset

| Aspek | Supervised Segmentation | Promptable Foundation Model |
|---|---|---|
| Data | Butuh mask berlabel | Butuh prompt, dapat tanpa label kelas |
| Kelas | Tertutup pada set pelatihan | Terbuka, ditentukan pengguna |
| Interaksi | Tidak interaktif | Interaktif dan iteratif |
| Komputasi | Pelatihan mahal | Inferensi relatif mahal, tanpa pelatihan |
| Kualitas | Tinggi pada domain sesuai | Bervariasi pada domain baru |
| Evaluasi | mIoU, Dice, PQ | IoU/Dice per prompt |

## Interpretasi

- Supervised segmentation kuat pada domain yang sama dengan data latih.
- Promptable segmentation unggul pada fleksibilitas dan kecepatan adaptasi.
- Keduanya dapat digabungkan: prompt digunakan untuk membuat label, model supervised dilatih pada label tersebut.

---

# Slide 33 - Seberapa Sensitif terhadap Prompt?

## Pertanyaan Kunci

- Seberapa sensitif hasil segmentasi terhadap jenis dan posisi prompt?
- Apakah model memahami objek secara utuh, atau hanya meniru kontur di sekitar prompt?
- Seberapa besar perbedaan mask ketika titik digeser sedikit?

## Eksperimen yang Dapat Dilakukan

- Variasikan posisi titik: tengah objek, tepi objek, dan di luar objek.
- Variasikan box: ketat, longgar, dan sedikit bergeser.
- Ulangi dengan seed berbeda untuk mengukur variabilitas.

## Kemungkinan Temuan

- Box prompt umumnya lebih stabil daripada point prompt.
- Point di tepi objek dapat menghasilkan mask yang keliru.
- Multi-mask output memberikan cara mengelola ambiguitas.

## Implikasi

- Protokol prompt harus didokumentasikan secara ketat dalam eksperimen.

---

# Slide 34 - Generalisasi Lintas Domain dan Domain Shift

## Definisi Domain Shift

- Distribusi citra target berbeda dari distribusi citra pelatihan.
- Contoh: SAM dilatih pada foto alami tetapi diterapkan pada citra medis, satelit, atau dokumen.

## Pertanyaan Riset

- Seberapa baik mask SAM bertahan pada domain di luar data latihnya?
- Apakah penurunan kualitas seragam untuk semua ukuran dan jenis objek?
- Faktor apa yang paling menentukan: tekstur, kontras, atau kompleksitas latar?

## Metode Evaluasi

- Ambil subset data dari beberapa domain.
- Beri prompt yang identik secara protokol.
- Hitung IoU/Dice per domain dan bandingkan distribusinya.

## Keterkaitan

- Pertemuan 11 akan membahas robustness dan distribution shift secara lebih mendalam.

---

# Slide 35 - Anotasi Tidak Sempurna dan Evaluasi

## Masalah pada Ground Truth

- Mask referensi tidak selalu akurat.
- Anotasi manusia memiliki variasi antar-annotator.
- Objek kecil dan batas samar sering memiliki label tidak konsisten.

## Dampak pada Metrik

- IoU dan Dice menghitung perbedaan terhadap referensi.
- Jika referensi salah, metrik dapat menghukum model yang sebenarnya lebih baik.
- Perbandingan antar metode menjadi tidak adil jika referensi berbeda antar dataset.

## Strategi Mitigasi

- Gunakan beberapa annotator dan ukur inter-annotator agreement.
- Lakukan analisis sensitivitas terhadap variasi mask referensi.
- Evaluasi secara visual sebagai pelengkap metrik kuantitatif.

## Pertanyaan Penelitian

- Apakah mask otomatis dari SAM dapat menggantikan peran ground truth dalam eksperimen tertentu?
- Bagaimana cara melaporkan ketidakpastian anotasi pada evaluasi?

---

# Slide 36 - Praktikum: Eksperimen SAM di Google Colab

## Tujuan Praktikum

- Menjalankan SAM pada beberapa tipe prompt.
- Membandingkan kualitas mask antar prompt.
- Menghitung IoU dan Dice pada subset data.

## Alur

```text
1. Instal segment-anything dan dependensi
2. Unduh checkpoint SAM ViT-B
3. Muat gambar dan siapkan referensi mask
4. Jalankan prediksi dengan point prompt
5. Jalankan prediksi dengan box prompt
6. Jalankan prediksi dengan mask prompt
7. Hitung IoU/Dice untuk setiap mask
```

## Contoh Kode Ringkas

```python
from segment_anything import sam_model_registry, SamPredictor

sam = sam_model_registry["vit_b"](checkpoint="sam_vit_b_01ec64.pth")
predictor = SamPredictor(sam)
predictor.set_image(image)

mask, score, _ = predictor.predict(
    point_coords=np.array([[x, y]]),
    point_labels=np.array([1]),
    multimask_output=True
)
```

---

# Slide 37 - Evaluasi Prompt Strategy dan Rekomendasi Riset

## Contoh Format Pelaporan Hasil

| Prompt | IoU | Dice | Keterangan |
|---|---|---|---|
| Point tengah objek | 0.72 | 0.83 | Mask kehilangan bagian tepi |
| Point tepi objek | 0.55 | 0.70 | Mask tidak stabil |
| Box ketat | 0.85 | 0.92 | Konsisten dengan objek |
| Box longgar | 0.79 | 0.88 | Terdapat latar yang ikut |
| Mask awal | 0.88 | 0.94 | Perbaikan dari prompt lain |

Nilai pada tabel adalah ilustrasi, bukan hasil final.

## Rekomendasi Riset

- Dokumentasikan protokol prompt secara lengkap.
- Gunakan beberapa jenis prompt untuk mengukur sensitivitas.
- Jika SAM digunakan untuk anotasi, validasi terhadap anotasi manual pada subset data.

## Pertanyaan untuk Seminar

- Bagaimana hasil ini mengubah posisi riset Anda terhadap state-of-the-art?
- Metrik mana yang paling relevan untuk domain Anda?

---

# Slide 38 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Generative Vision dengan Diffusion Models**