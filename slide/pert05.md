# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 05

## Vision-Language Models dan Multimodal Representation

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 05 dalam Rangkaian Perkuliahan

## Koneksi dengan Pertemuan Sebelumnya

- Pertemuan 04 membahas **Self-Supervised Learning** dan **Foundation Vision Models**:
  - Contrastive learning, masked image modeling, DINO, DINOv2.
  - Representasi visual yang dipelajari tanpa label atau dengan label terbatas.
- Pertemuan 05 memperluas konsep tersebut dari **single modality (visual)** menjadi **multimodal (visual + bahasa)**.
- Alur logis: representasi visual yang baik → ditambahkan bahasa sebagai sinyal pengawas → **vision-language models**.

## Jembatan ke Pertemuan Berikutnya

- Pertemuan 06 membahas **Image Restoration dan Computational Imaging**.
- Representasi multimodal berguna untuk evaluasi kualitas persepsi dan pemulihan citra berbasis teks.
- Konsep **evaluasi zero-shot** yang dipelajari di pertemuan ini menjadi dasar untuk mengevaluasi model restoration pada domain baru.

---

# Slide 02 - Tujuan Pembelajaran dan Capaian Terkait

## Tujuan Pembelajaran

1. Memahami konsep **contrastive image-text learning** sebagai dasar pelatihan vision-language model.
2. Menganalisis arsitektur dan mekanisme kerja **CLIP** dalam menyelaraskan representasi citra dan teks.
3. Menjelaskan mekanisme **zero-shot recognition** melalui kanonikalisasi label dan prompt engineering.
4. Mengevaluasi **kekuatan dan keterbatasan** model multimodal, termasuk bias bahasa dan kesenjangan domain.
5. Merancang **protokol evaluasi zero-shot** dan **image-text retrieval** yang valid untuk kebutuhan penelitian.

## Capaian Pembelajaran Mata Kuliah

| CPMK | Kontribusi Pertemuan |
|---|---|
| CPMK-1 | Menganalisis kemampuan dan keterbatasan model multimodal (BK-05). |
| CPMK-4 | Memanfaatkan CLIP untuk eksperimen zero-shot dan retrieval. |
| CPMK-5 | Merumuskan gap penelitian terkait bias, prompt, dan generalisasi multimodal. |

---

# Slide 03 - Motivasi: Mengapa Menghubungkan Citra dan Teks?

## Keterbatasan Model Visual Konvensional

- Diberi label dari **closed-set** yang tetap.
- Kinerja hanya sebaik kualitas dan cakupan label pelatihan.
- Tidak dapat mengenali objek atau konsep yang belum pernah dilihat sebagai label.

## Keunggulan Supervisi Bahasa Alami

- Bahasa alami **kaya akan makna** dan dapat menggambarkan hampir semua konsep.
- Teks memiliki **struktur semantik** yang dapat disusun tanpa batas (combinations).
- Bahasa dapat bertindak sebagai **sinyal pengawas fleksibel** yang tidak terbatas pada kategori.

## Ide Inti

> Latih model untuk memahami hubungan antara **gambar** dan **deskripsinya dalam bahasa alami**, sehingga model dapat menalar konsep baru tanpa pelatihan ulang.

---

# Slide 04 - Apa yang Dimaksud dengan Multimodal Representation?

## Definisi

- **Multimodal representation** adalah representasi bersama (joint representation) yang memetakan data dari beberapa modalitas ke ruang vektor yang sama.
- Dalam konteks ini: **citra (visual)** dan **teks (linguistik)**.

## Tujuan Alignment

- Vektor representasi yang **berdekatan** dalam ruang embedding untuk data yang bermakna sama.
- Vektor representasi yang **berjauhan** untuk data yang bermakna berbeda.

## Ilustrasi Sederhana

```text
+---------------------------+
|     Ruang Embedding        |
|                           |
|  Citra "kucing"  ~  Teks "a photo of a cat" |
|  Citra "mobil"   ~  Teks "a car"            |
+---------------------------+
```

- Representasi multimodal memungkinkan model membandingkan citra dan teks secara langsung melalui **cosine similarity**.

---

# Slide 05 - Paradigma Supervisi: Dari Label Kategoris ke Bahasa Alami

## Supervisi Label Klasik

```text
Dataset: ImageNet
Label: "cat", "dog", "car"
Konversi: one-hot vector
Kelas tertutup, tidak fleksibel
```

## Supervisi Bahasa Alami

```text
Dataset: WebImageText
Teks deskriptif: "a photo of a tabby cat sitting on a window sill"
Representasi: teks asli diubah menjadi vektor melalui text encoder
Konsep tidak dibatasi jumlahnya
```

## Dampak Perubahan Paradigma

| Aspek | Label Kategoris | Bahasa Alami |
|---|---|---|
| Jumlah kelas | Tetap | Tidak terbatas |
| Semantik | Satu kata | Kalimat penuh |
| Hubungan antar kelas | Tidak tersedia | Ada struktur |
| Zero-shot | Tidak langsung | Alami |

---

# Slide 06 - Model CLIP: Gagasan Utama

## Latar Belakang

- CLIP = **Contrastive Language-Image Pre-training** (Radford et al., 2021).
- Tujuan: mempelajari representasi visual yang dapat **ditransfer** melalui supervisi bahasa alami.
- Mengatasi keterbatasan label kategoris dengan memanfaatkan pasangan **gambar-teks** dalam jumlah besar.

## Sumber Data

- Kumpulan data dari internet berisi pasangan **gambar dan teks** (400 juta pasangan).
- Tidak memerlukan anotasi manual terstruktur untuk setiap kelas.

## Ide Dasar

1. Encode gambar menjadi vektor.
2. Encode teks menjadi vektor.
3. Pelajari kesamaan (similarity) pasangan yang benar.
4. Gunakan objective kontrastif untuk memisahkan pasangan yang benar dari pasangan yang salah.

---

# Slide 07 - Arsitektur CLIP

## Komponen Utama

- **Image Encoder**: mengubah citra menjadi vektor representasi (dimensi tetap).
- **Text Encoder**: mengubah teks menjadi vektor representasi (dimensi tetap).
- **Proyeksi (projection)**: memetakan hasil encoder ke ruang bersama.

## Pilihan Arsitektur pada CLIP

| Komponen | Pilihan Arsitektur |
|---|---|
| Image Encoder | ResNet atau Vision Transformer (ViT) |
| Text Encoder | Transformer (GPT-style) |
| Proyeksi | Linear layer untuk menyelaraskan dimensi |
| Normalisasi | L2 normalization terhadap embedding |

## Representasi Akhir

- Setiap citra direpresentasikan sebagai vektor **I**.
- Setiap teks direpresentasikan sebagai vektor **T**.
- Kesamaan dihitung dengan **cosine similarity** antara **I** dan **T**.

---

# Slide 08 - Proses Pelatihan CLIP

## Dataset Pelatihan

- Kumpulan pasangan `(citra, teks)` berukuran batch `N`.
- Contoh: `(gambar kucing, "a photo of a cat" )`, `(gambar mobil, "a car on the road")`.

## Contrastive Objective

- Matriks kesamaan berukuran `N x N` dibangun antara semua embedding citra dan semua embedding teks.
- Pasangan yang benar berada pada diagonal.
- Pasangan yang salah berada pada off-diagonal.
- Model dilatih untuk memaksimalkan kesamaan pada diagonal dan meminimalkan kesamaan pada off-diagonal.

## Pseudocode

```python
## image_features, text_features: hasil encode batched
## logits = cosine_similarity(image_features, text_features) / temperature
## loss = cross_entropy(logits, target_diagonal)
```

---

# Slide 09 - Diagram Alur Pelatihan CLIP

```text
 Citra (N)                        Teks (N)
    |                                |
    v                                v
Image Encoder                  Text Encoder
    |                                |
    v                                v
Image Embedding (N,d)          Text Embedding (N,d)
    |                                |
    +----------> Normalize <---------+
                 |
                 v
        Matriks Kesamaan (N x N)
                 |
                 v
     Cross-Entropy Loss (diagonal)
                 |
                 v
            Update Bobot
```

- Loss dihitung dua arah: image-to-text dan text-to-image.

---

# Slide 10 - Normalisasi dan Kesamaan Kosinus

## Mengapa Normalisasi dan Cosine Similarity?

- Cosine similarity hanya bergantung pada **sudut antar vektor**, bukan magnitudo.
- Normalisasi L2 menghasilkan vektor satuan sehingga jarak Euclidean sebanding dengan sudut.
- Skala embedding menjadi terkontrol dan stabil untuk pelatihan.

## Rumus

```text
Cosine Similarity = (I . T) / (||I|| * ||T||)

Setelah normalisasi: I' = I / ||I||, T' = T / ||T||
Cosine Similarity = I' . T'
```

## Peran Temperature

- Logits dibagi dengan parameter suhu `temperature` yang dapat dipelajari.
- Suhu mengontrol **kecerunan distribusi kesamaan**.
- Nilai suhu kecil membuat distribusi lebih tajam dan mendorong model lebih percaya diri.

---

# Slide 11 - Zero-Shot Classification: Konsep Dasar

## Definisi

- **Zero-shot classification** adalah kemampuan mengklasifikasikan gambar ke kelas yang belum pernah dilihat saat pelatihan, tanpa contoh visual tambahan.
- Model hanya diberikan **deskripsi teks** untuk setiap kelas.

## Alur Kerja Zero-Shot dengan CLIP

1. Tentukan label kelas yang diinginkan, misal: `"cat"`, `"dog"`, `"car"`.
2. Ubah setiap label menjadi teks kalimat, misal dengan template: `"a photo of a {label}"`.
3. Encode semua teks label menjadi embedding teks.
4. Encode citra menjadi embedding gambar.
5. Hitung kesamaan antara embedding gambar dan semua embedding teks.
6. Pilih kelas dengan kesamaan tertinggi.

---

# Slide 12 - Peran Prompt Template pada Zero-Shot

## Mengapa Teks Label Perlu Diubah Menjadi Kalimat?

- Model dilatih pada **kalimat deskriptif**, bukan label tunggal.
- Templat membantu model mengenali konteks: `"a photo of a"` memberi sinyal struktur kalimat yang umum pada data pelatihan.
- Tanpa templat, label mentah seperti `"cara"` mungkin kurang bermakna.

## Contoh Template

| Templat | Efek |
|---|---|
| `"a photo of a {label}"` | Umum, cukup baik untuk banyak kasus |
| `"a photo of a {label}, a type of {superclass}"` | Menambahkan konteks kelas atas |
| `"a satellite image of {label}"` | Cocok untuk citra penginderaan jauh |
| `"a medical image of {label}"` | Cocok untuk domain medis |
| Templat yang diadaptasi | Dapat meningkatkan kinerja pada domain spesifik |

---

# Slide 13 - Rumus Zero-Shot Classification

## Representasi

- Kelas-kelas yang mungkin: `C` = {c1, c2, ..., cK}.
- Untuk setiap kelas `ci`, buat deskripsi teks `ti` (misal dengan template).
- Encode semua `ti` menjadi `W = [w1, w2, ..., wK]` (berukuran `K x d`).

## Prediksi

```text
Probabilitas kelas untuk citra x:

p(y = ci | x) = softmax( f(x) . w_i / temperature )

prediksi = argmax_i p(y = ci | x)
```

## Perbedaan dengan Klasifikasi Konvensional

| Aspek | Klasifikasi Konvensional | Zero-Shot CLIP |
|---|---|---|
| Bobot kelas | Dipelajari dari data | Dibuat dari teks |
| Kelas baru | Perlu retraining | Langsung bisa ditambah |
| Contoh visual | Diperlukan | Tidak diperlukan |

---

# Slide 14 - Image-Text Retrieval: Definisi dan Arah

## Definisi

- **Image-to-text retrieval**: diberikan citra, temukan teks yang paling sesuai di antara kumpulan teks.
- **Text-to-image retrieval**: diberikan teks, temukan citra yang paling sesuai di antara kumpulan citra.

## Alur Umum

```text
Citra/teks query  ->  Encode  ->  Bandingkan dengan seluruh kandidat
                                     |
                                     v
                          Urutkan berdasarkan cosine similarity
```

## Perbedaan dengan Zero-Shot Classification

- Retrieval tidak membutuhkan label kelas eksplisit.
- Kandidat bisa berupa kalimat deskriptif, caption, atau dokumen.
- Cocok untuk **pencarian berbasis konten** dan **dataset curation**.

---

# Slide 15 - Representasi Embedding sebagai Jembatan Modalitas

## Ruang Embedding Bersama

- Citra dan teks dari pasangan yang sesuai diproyeksikan ke **wilayah yang berdekatan**.
- Data dengan makna serupa tetapi berbeda modalitas tetap memiliki vektor yang dekat.
- Model dapat membandingkan secara langsung antar modalitas.

## Sifat yang Diharapkan

1. **Alignment**: pasangan yang benar memiliki kesamaan tinggi.
2. **Uniformity**: vektor menyebar cukup merata sehingga tidak semua data mengumpul.
3. **Compositionality**: kombinasi teks sederhana memetakan ke makna yang komposisional.

## Implikasi

- Dengan ukuran tersebut, model dapat melakukan berbagai tugas multimodal:
  - Klasifikasi zero-shot.
  - Retrieval.
  - Pencarian berdasarkan deskripsi.
  - Evaluasi kesesuaian.

---

# Slide 16 - Dari Sinyal Bahasa ke Pemahaman Semantik

## Pertanyaan Kunci Penelitian

> Apakah kedekatan antara citra dan teks benar-benar menunjukkan **pemahaman semantik**, atau hanya menangkap korelasi permukaan dan bias statistik?

## Indikator Pemahaman Semantik

- Prediksi yang konsisten terhadap beberapa kalimat berbeda dengan makna sama.
- Model mengenali konsep atribut (warna, bentuk, material) secara terpisah.
- Model dapat menolak pasangan yang secara visual mirip tetapi makna berbeda.

## Indikator yang Belum Tentu Pemahaman

- Model dapat memilih label yang benar karena **korelasi dengan konteks** yang mungkin bias.
- Model belum tentu memahami hubungan sebab akibat atau kuantitas.
- Kinerja tinggi pada benchmark belum tentu berarti penalaran mendalam.

---

# Slide 17 - Prompt Engineering: Memahami Sensitivitas Template

## Definisi

- **Prompt engineering** adalah proses merancang teks input agar model multimodal menghasilkan prediksi atau representasi terbaik.
- Prompt memberikan **konteks** yang dapat mengarahkan interpretasi citra.

## Mengapa Prompt Berpengaruh?

- Model dilatih pada teks alami yang beragam; representasi sangat bergantung pada teks.
- Kata permukaan (surface form) label memengaruhi hasil.
- Kelas yang tidak umum mungkin perlu dikontekstualisasikan agar sesuai dengan bahasa data pelatihan.

## Contoh Sensitivitas

| Prompt | Hasil yang Sering Dilaporkan |
|---|---|
| `"a photo of a {label}"` | Baik secara umum |
| `"a bad photo of a {label}"` | Menurunkan akurasi |
| `"{label}"` | Sering kurang baik untuk kelas tertentu |
| `"a drawing of a {label}"` | Mengarah ke gaya citra tertentu |

---

# Slide 18 - Teknik Ensembel Prompt

## Ide Dasar

- Alih-alih satu prompt, gunakan **banyak template prompt** untuk satu set label kelas.
- Embedding teks dari semua template dirata-ratakan sebelum dibandingkan dengan citra.

## Contoh Implementasi

```python
templates = [
    "a photo of a {}.",
    "a photo of the {}.",
    "a photo of one {}.",
    "a picture of a {}.",
    "a blurry photo of a {}.",
]
```

## Keuntungan Ensembel

- Mengurangi sensitivitas terhadap satu template.
- Menangkap variasi bahasa pada data pelatihan.
- Meningkatkan robustness dan akurasi zero-shot pada banyak dataset.

## Catatan

- Ensembel menambah biaya komputasi, tetapi tekstur ini dihitung sekali per kelas dan dapat di-cache.

---

# Slide 19 - Zero-Shot Classification: Alur Praktis dengan Python

## Langkah Praktis

1. Muat model CLIP (`open_clip` atau modul dari Hugging Face).
2. Siapkan daftar label kelas.
3. Bangun prompt untuk setiap label.
4. Encode semua tagar teks label.
5. Encode citra yang akan diprediksi.
6. Hitung kesamaan kosinus.
7. Ambil kelas dengan skor tertinggi.

## Contoh Instalasi

```text
pip install open_clip_torch
```

## Contoh Pseudo-Langkah

```python
## muat model, preprocess
## tokenize dan encode teks label
## encode citra
## scores = image_embedding @ text_embeddings.T
## prediksi = argmax
```

---

# Slide 20 - Contoh Kode Python: Zero-Shot dengan CLIP

```python
import torch
import open_clip

model, _, preprocess = open_clip.create_model_and_transforms(
    "ViT-B-32", pretrained="openai"
)
tokenizer = open_clip.get_tokenizer("ViT-B-32")

labels = ["cat", "dog", "car"]
prompts = [f"a photo of a {label}" for label in labels]

text_tokens = tokenizer(prompts)
with torch.no_grad():
    text_features = model.encode_text(text_tokens)
    image = preprocess(image).unsqueeze(0)
    image_features = model.encode_image(image)

image_features /= image_features.norm(dim=-1, keepdim=True)
text_features /= text_features.norm(dim=-1, keepdim=True)

similarity = (image_features @ text_features.T).softmax(dim=-1)
predicted_label = labels[similarity.argmax().item()]
```

---

# Slide 21 - Image-Text Retrieval: Contoh Implementasi Python

```python
import torch
import open_clip

model, _, preprocess = open_clip.create_model_and_transforms(
    "ViT-B-32", pretrained="openai"
)
tokenizer = open_clip.get_tokenizer("ViT-B-32")

## database teks (captions) dan citra query
captions = [
    "a man walking a dog on the street",
    "two people sitting in a cafe",
    "a red car parked beside a building",
]

text_tokens = tokenizer(captions)
with torch.no_grad():
    text_features = model.encode_text(text_tokens)
    image_features = model.encode_image(preprocess(image).unsqueeze(0))

text_features /= text_features.norm(dim=-1, keepdim=True)
image_features /= image_features.norm(dim=-1, keepdim=True)

similarity = (image_features @ text_features.T).squeeze()
ranking = similarity.argsort(descending=True)
```

---

# Slide 22 - Evaluasi Zero-Shot: Metrik yang Dipakai

## Klasifikasi Zero-Shot

| Metrik | Deskripsi |
|---|---|
| Accuracy | Proporsi prediksi benar dari total sampel |
| Top-1 / Top-5 | Akurasi pada 1 atau 5 prediksi teratas |
| Mean per-class accuracy | Rata-rata akurasi tiap kelas, tidak timpang kelas |

## Retrieval

| Metrik | Deskripsi |
|---|---|
| Recall@K | Proporsi query yang memiliki pasangan benar di K hasil teratas |
| Median Rank | Median peringkat pasangan benar |
| Mean Reciprocal Rank (MRR) | Rata-rata 1/peringkat pasangan benar |

## Catatan

- Gunakan **acuan yang sama** antara model dan baseline.
- Laporkan **interval kepercayaan** atau variasi antar seed jika memungkinkan.

---

# Slide 23 - Kanalisasi dan Normalisasi Label untuk Zero-Shot

## Tantangan

- Nama kelas tidak selalu berupa kata tunggal yang natural.
- Beberapa kelas memiliki sinonim atau varian bahasa.
- Kelas bisa bersifat hierarkis (misal `"Persian cat"` vs `"cat"`).

## Praktik Baik

1. Gunakan nama kelas yang **umum dipahami** oleh model (natural language).
2. Sertakan **konteks kelas atas** jika perlu, misal `"a photo of a {label}, a type of {superclass}"`.
3. Gunakan **banyak template** dan rata-ratakan embedding-nya.
4. Validasi pilihan nama label pada dataset development sebelum evaluasi final.

## Risiko

- Nama label yang tidak umum dapat menyebabkan prediksi bias ke kelas lain.
- Perubahan kecil pada nama label dapat mengubah hasil secara signifikan.

---

# Slide 24 - Sensitivitas Prompt: Analisis dan Evaluasi

## Tujuan Eksperimen

- Mengukur seberapa besar perubahan prompt mengubah akurasi.
- Mengidentifikasi prompt yang robust dan prompt yang menyebabkan penurunan.

## Desain Eksperimen

1. Ambil dataset yang sudah memiliki label (misal CIFAR-100 atau domain spesifik).
2. Tentukan beberapa set prompt:
   - Sederhana: `"a photo of a {label}"`.
   - Beragam: tambahkan konteks, lokasi, gaya, kualitas.
   - Berbeda: tanpa templat, dengan templat negatif.
3. Hitung akurasi untuk setiap set prompt.
4. Bandingkan rata-rata dan deviasi akurasi.

## Interpretasi

- Deviasi kecil menunjukkan model robust terhadap bahasa.
- Deviasi besar menunjukkan model sensitif terhadap kata permukaan, bukan sepenuhnya semantik.

---

# Slide 25 - Analisis Kesalahan pada Zero-Shot

## Tujuan Analisis Kesalahan

- Menentukan jenis kesalahan yang paling dominan.
- Mengidentifikasi apakah kesalahan bersifat visual, semantik, atau linguistik.

## Kategori Kesalahan Umum

| Jenis Kesalahan | Contoh Penyebab |
|---|---|
| Visual ambiguity | Gambar samar, objek kecil, oklusi |
| Label semantically similar | Kelas mirip seperti "truck" vs "car" |
| Linguistic gap | Label tidak sesuai bahasa pelatihan model |
| Bias dataset | Korelasi konteks, misal "kuda" sering di padang rumput |
| Prompt effect | Prompt yang kurang tepat menekankan konsep yang salah |

## Output Analisis

- Tabel frekuensi kesalahan per kategori.
- Visualisasi contoh prediksi salah yang representatif.
- Rekomendasi perbaikan prompt atau prosedur evaluasi.

---

# Slide 26 - Bias Bahasa dan Bias Visual pada Model Multimodal

## Sumber Bias

- **Bias data**: distribusi pasangan citra-teks dari internet tidak mewakili populasi dunia nyata.
- **Bias label**: pemilihan nama kelas dan superclass dapat menimbulkan prior yang salah.
- **Bias bahasa**: kata-kata tertentu memiliki asosiasi kuat akibat frekuensi pada data pelatihan.

## Contoh

- Jika data pelatihan didominasi citra "dokter" yang berkorelasi dengan pria, model dapat salah mengaitkan konsep profesi dengan gender.
- Jika objek "gadget" lebih sering muncul dengan latar perkotaan, model dapat memanfaatkan konteks tersebut.

## Implikasi Penelitian

- Evaluasi menggunakan akurasi saja tidak cukup.
- Perlu pengujian **fairness** dan **debiasing**.
- Protokol evaluasi harus memisahkan performa pada subkelompok data tertentu.

---

# Slide 27 - Studi Kasus Bias: Bagaimana Bias Masuk ke Model?

## Skema Perpindahan Bias

```text
Internet text-image pairs
       |
       v
Statistik korelasi tidak seimbang
       |
       v
Representasi embedding menyerap korelasi
       |
       v
Prediksi dipengaruhi korelasi, bukan semantik
```

## Contoh Analisis

| Pertanyaan | Yang Harus Diperiksa |
|---|---|
| Apakah model memilih `"cook"` karena alat masak, atau karena gender yang sering muncul? | Disagregasi berdasar atribut |
| Apakah model mengenali objek di luar konteks umum? | Uji pada citra dan lokasi tak umum |
| Apakah perubahan jenis kelamin pada citra mengubah label profesi? | Uji kontrol atribut |

---

# Slide 28 - Kesenjangan Kemampuan Benchmark dengan Kebutuhan Domain

## Realita

- CLIP menunjukkan kinerja tinggi pada beberapa benchmark umum:
  - ImageNet, CIFAR-100, dan dataset web.
- Namun, kinerja dapat turun drastis pada **data domain spesifik**:
  - Citra medis, satelit, industri, artefak budaya, dll.

## Penyebab

- Konsep yang jarang muncul pada pasangan citra-teks di internet.
- Gaya citra yang berbeda dari foto natural.
- Bahasa yang digunakan oleh domain berbeda dari bahasa internet umum.

## Implikasi untuk Disertasi

- Validasi domain sangat penting.
- Model umum (general-purpose) tidak otomatis siap pakai untuk masalah penelitian Anda.
- Pelaporan harus menyertakan pengukuran pada data domain target.

---

# Slide 29 - Toolbox Utama untuk Eksperimen Multimodal

| Library / Alat | Fungsi Utama |
|---|---|
| `open_clip_torch` | Memuat CLIP dengan berbagai arsitektur dan pretrained weights |
| `transformers` (Hugging Face) | Memuat berbagai model vision-language (CLIP, BLIP, dll.) |
| `torch` / `torchvision` | Operator tensor, transformasi citra, dan arsitektur dasar |
| `matplotlib` | Visualisasi citra, embedding, kurva evaluasi |
| `numpy` | Operasi numerik dan manipulasi data |
| `scikit-learn` | Metrik evaluasi, reduksi dimensi, clustering |

## Catatan

- Gunakan versi library yang konsisten dan dokumentasikan environment.
- Setel `random seed` agar eksperimen reproducible.

---

# Slide 30 - Workflow Praktikum Pertemuan 05

```text
+-----------+------------+------------------------+-----------------+
| 1. Setup  | 2. Muat     | 3. Zero-shot           | 4. Retrieval    |
| dataset   | model CLIP | classification          | test            |
| target    |            | dengan variasi prompt   |                 |
+-----------+------------+------------------------+-----------------+
     |             |                   |                     |
     v             v                   v                     v
+-----------+------------+------------------------+-----------------+
| pilih     | load       | hitung akurasi,        | recall@K,       |
| dataset   | pretrained | analisis kesalahan,    | visualisasi     |
| yang      | weights    | bandingkan prompt      | hasil retrieval |
| relevan   |            |                        |                 |
```

## Target Luaran

- Laporan eksperimen multimodal berisi:
  - Akurasi zero-shot.
  - Variasi prompt dan dampaknya.
  - Contoh keberhasilan dan kegagalan.
  - Analisis bias atau keterbatasan model.

---

# Slide 31 - Perancangan Protokol Evaluasi Zero-Shot

## Langkah Perancangan

1. **Tentukan pertanyaan penelitian**: Apa yang ingin diuji? (misal, apakah model memahami konsep domain).
2. **Pilih dataset**: pastikan representatif dan memiliki label yang dapat dikonversi menjadi teks.
3. **Definisikan set prompt**: buat variasi yang sistematis, bukan hanya satu template.
4. **Tentukan metrik**: Accuracy, per-class accuracy, dll.
5. **Tetapkan aturan evaluasi**: berapa sampel, bagaimana split, apakah ada warm-up prompt.
6. **Dokumentasikan semua konfigurasi**: seed, versi model, daftar prompt, dan preprocessing.

## Protokol yang Baik

- Jelas dan reproducible.
- Tidak mengubah prompt setelah melihat hasil pada dataset test (bila memungkinkan).
- Memiliki analisis kesalahan terstruktur.

---

# Slide 32 - Analisis Pengaruh Prompts: Benchmark Sederhana

## Contoh Notasi Hasil

| Set Prompt | Akurasi | Catatan |
|---|---|---|
| Tanpa template (`"cat"`) | 57.2% | Banyak kelas tidak terbaca |
| `"a photo of a {label}"` | 68.5% | Peningkatan signifikan |
| `"a photo of a {label}, a type of {superclass}"` | 70.1% | Konteks hierarkis membantu |
| Ensembel 5 template | 72.3% | Deviasi turun |
| `"a drawing of a {label}"` | 51.8% | Tidak cocok untuk foto |

## Interpretasi

- Templat yang lebih dekat dengan data pelatihan umumnya lebih baik.
- Konteks superclass dapat membantu saat label ambigu.
- Prompt yang salah gaya dapat menurunkan kinerja hingga di bawah baseline.

---

# Slide 33 - Menghubungkan dengan Pertemuan Sebelumnya: DINO vs CLIP

| Aspek | DINO/DINOv2 (Pertemuan 04) | CLIP (Pertemuan 05) |
|---|---|---|
| Sinyal pengawas | Self-supervised (tanpa label) | Natural language supervision |
| Input | Citra saja | Citra + teks |
| Representasi | Visual-centric | Multimodal |
| Kemampuan zero-shot text | Tidak langsung | Langsung |
| Transferability | Sangat kuat | Kuat untuk konsep umum |

## Kapan Menggunakan?

- **DINO/DINOv2**: tugas visual yang membutuhkan representasi spasial atau semantik, tanpa kebutuhan interaksi teks.
- **CLIP**: tugas yang melibatkan bahasa, retrieval, atau zero-shot classification dengan konsep fleksibel.

---

# Slide 34 - Perbandingan dengan Model Multimodal Lain

| Model | Ide Utama | Kekuatan |
|---|---|---|
| CLIP | Contrastive image-text | Zero-shot classification dan retrieval |
| BLIP | Bootstrapping captions dan filtering | Generation dan understanding |
| ALIGN | Skala data besar dengan noise | Robust terhadap noisy data |
| Flamingo | Interleaved image-text + LM | Few-shot multimodal reasoning |
| LLaVA | Connection ke LLM | Visual instruction following |

## Posisi CLIP

- CLIP adalah fondasi alignment yang sederhana dan banyak digunakan.
- Model lain memperluasnya dengan **generation**, **reasoning**, atau **instruction tuning**.

## Implikasi Penelitian

- Pemilihan model tergantung pada jenis tugas dan kebutuhan bahasa pada aplikasi.

---

# Slide 35 - Keterbatasan Multimodal Model yang Perlu Diketahui

## Keterbatasan Utama

1. **Kesalahan atribut**: kurang peka pada jumlah, posisi, dan hubungan antar objek.
2. **Sensitivitas prompt**: hasil berubah drastis oleh kata permukaan.
3. **Bias distribusi**: mereplikasi bias sosial dan budaya pada data latih.
4. **Reasoning terbatas**: tidak sepenuhnya memahami logika atau proses sebab-akibat.
5. **Keterbatasan domain**: kinerja turun pada citra atau bahasa yang tidak umum.
6. **Evaluasi yang menyesatkan**: akurasi tinggi dapat dicapai dengan shortcut statistik.

## Implikasi Riset

- Jangan menyimpulkan "pemahaman" hanya dari akurasi.
- Uji dengan pertanyaan kontrol, prompt adversial, dan data yang disusun untuk menyelidiki kelemahan model.

---

# Slide 36 - Cara Menguji Apakah Model Memahami Semantik

## Ide Pengujian

- Bentuk **pasangan kalimat yang bermakna sama** tetapi berbeda permukaan.
- Bentuk **pasangan kalimat yang permukaannya mirip tetapi makna berbeda**.
- Bandingkan skor kesamaan.

## Contoh

| Citra | Teks 1 | Teks 2 | Harapan |
|---|---|---|---|
| Anjing berlari | "a dog running" | "a dog is running on the grass" | Skor tinggi |
| Anjing berlari | "a dog running" | "a dog sitting" | Skor rendah |
| Anjing berlari | "a dog running" | "an animal moving fast" | Skor sedang |

## Analisis

- Jika model memberikan skor sangat tinggi untuk teks yang tidak konsisten dengan citra.
- Maka model mungkin hanya mengandalkan surface form, bukan pemahaman penuh.

---

# Slide 37 - Menghubungkan Konsep ke Research Gap

## Contoh Research Gap yang Dapat Dikaji

| Area | Gap Potensial |
|---|---|
| Evaluasi domain | CLIP belum dievaluasi secara sistematis pada [domain pilihan] |
| Prompt engineering | Metode adaptasi prompt otomatis untuk data domain tidak banyak |
| Bias | Bias bahasa-visual belum dipetakan pada dataset domain lokal |
| Retrieval | Image-text retrieval untuk konten spesifik belum diuji dengan metrik yang tepat |
| Benchmark | Belum ada benchmark zero-shot yang merepresentasikan kebutuhan praktis |

## Langkah Menuju Proposal

1. Identifikasi pertanyaan evaluasi yang belum terjawab.
2. Rancang eksperimen untuk menguji hipotesis.
3. Gunakan analisis kesalahan sebagai dasar novelty.

---

# Slide 38 - Contoh Eksperimen: Menguji Sensitivitas Prompt pada Dataset Domain

## Konfigurasi Eksperimen

- Dataset: 10 kelas dari domain target (misal: objek industri, flora lokal, dll).
- Model: CLIP ViT-B/32.
- Prompt sets:
  - P1: `"a photo of a {label}"`
  - P2: `"a {label} in the factory"` (domain-specific)
  - P3: `"a {label}, a type of {superclass}"`
  - P4: ensembel P1-P3
- Evaluasi: akurasi per kelas, visualisasi embedding.

## Analisis yang Diharapkan

- Apakah prompt domain membantu pada data yang tidak umum?
- Bagaimana distribusi kesalahan berubah?
- Apakah ensembel meningkatkan robustness?

---

# Slide 39 - Laporan Eksperimen Multimodal: Format yang Diusulkan

## Struktur Laporan

1. **Pendahuluan**: pertanyaan riset dan motivasi.
2. **Metode**: model, dataset, prompt, prosedur.
3. **Hasil**:
   - Tabel akurasi per set prompt.
   - Grafik pengaruh variasi prompt.
   - Tabel retrieval metrics.
   - Contoh visual keberhasilan dan kegagalan.
4. **Analisis**:
   - Kategorisasi kesalahan.
   - Diskusi bias atau keterbatasan.
   - Interpretasi terhadap pemahaman semantik.
5. **Kesimpulan**: implikasi dan arah riset lanjutan.

---

# Slide 40 - Reproducibility Checklist untuk Eksperimen Multimodal

## Checklist

- [ ] Versi Python, PyTorch, open_clip dicatat.
- [ ] Nama arsitektur model dan pretrained weights dicatat.
- [ ] Daftar template prompt seluruhnya dituliskan.
- [ ] Random seed ditetapkan untuk semua sumber acak.
- [ ] Dataset dan preprocessing dijelaskan.
- [ ] Metrik dan rumus dijelaskan.
- [ ] Kode dan hasil disimpan dalam repositori.

## Mengapa Penting?

- Menghindari perbedaan hasil antar replikasi.
- Memungkinkan evaluasi kritis oleh peneliti lain.
- Mendukung pelaporan yang transparan pada proposal disertasi.

---

# Slide 41 - Kesimpulan Utama Pertemuan 05

## Poin Kunci

1. **CLIP** mempelajari representasi citra dan teks melalui contrastive learning.
2. **Zero-shot classification** bekerja dengan membandingkan embedding citra dengan embedding teks label.
3. **Prompt engineering** berdampak besar terhadap hasil, dan ensembel prompt dapat meningkatkan robustness.
4. **Image-text retrieval** memanfaatkan kesamaan kosinus dalam ruang embedding bersama.
5. Model multimodal memiliki **keterbatasan**: bias, sensitivitas bahasa, dan kesenjangan domain.
6. Evaluasi yang baik memerlukan **protokol yang jelas**, metrik yang tepat, dan **analisis kesalahan**.

## Koneksi ke Penelitian

- CLIP dapat menjadi fondasi eksperimen yang menguji generalisasi multimodal pada domain spesifik.
- Analisis bias dan keterbatasan membuka peluang research gap untuk disertasi.

---

# Slide 42 - Tugas dan Bukti Belajar

## Tugas Praktikum / Eksperimen

1. Lakukan **zero-shot classification** pada dataset minimal 10 kelas.
2. Lakukan **image-text retrieval** dengan 20-50 pasang teks/citra.
3. Lakukan **variasi prompt** minimal 3 set template.
4. Catat **akurasi**, **retrieval metrics**, dan buat **analisis kesalahan**.

## Luaran yang Harus Dikumpulkan

- Laporan eksperimen berisi:
  - Hasil kuantitatif (tabel/grafik).
  - Contoh visual keberhasilan dan kegagalan.
  - Pembahasan pengaruh prompt.
  - Refleksi kritis terhadap keterbatasan model.

## Catatan

- Dokumentasikan seluruh konfigurasi agar reproducible.
- Diskusikan hasil dengan dosen atau rekan pada sesi research clinic.

---

# Slide 43 - Persiapan Pertemuan Berikutnya

## Topik Berikutnya

- **Image Restoration dan Computational Imaging**:
  - Inverse problem, degradasi, denoising, deblurring, super-resolution.
  - Peran representasi visual dan multimodal dalam evaluasi kualitas.

## Kaitan dengan Pertemuan Ini

- Model multimodal dapat digunakan untuk tugas seperti:
  - Mencocokkan deskripsi teks dengan citra hasil restorasi.
  - Mengevaluasi kualitas persepsi secara otomatis.
- Pemahaman alignment menjadi dasar untuk mengembangkan metrik evaluasi berbasis bahasa.

## Persiapan

- Tinjau konsep degradasi citra dan pengukuran PSNR/SSIM.
- Siapkan pertanyaan tentang peran evaluasi persepsi vs numerik.

---

# Slide 44 - Referensi dan Bacaan Lanjutan

## Paper Utama untuk Pertemuan 05

- Radford, A., et al. "Learning Transferable Visual Models From Natural Language Supervision." ICML 2021.

## Bacaan Pendukung

- Jia, C., et al. "Scaling Up Visual and Vision-Language Representation Learning With Noisy Text Supervision." ICML 2021.
- Li, J., et al. "BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation." ICML 2022.
- Li, J., et al. "BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models." ICML 2023.

## Dokumentasi

- OpenCLIP: https://github.com/mlfoundations/open_clip
- Hugging Face Transformers.

## Catatan

- Gunakan sumber yang relevan untuk kajian kritis dan penulisan proposal.

---

# Slide 45 - TERIMA KASIH

Pertemuan berikutnya

**Image Restoration dan Computational Imaging**