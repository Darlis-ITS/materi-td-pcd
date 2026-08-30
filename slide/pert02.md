# Slide 00 - Cover

EF256129 - TD PCD

Pertemuan 02

## Critical Paper Reading dan Identifikasi Research Gap

Dr. Darlis Herumurti

Departemen Teknik Informatika - ITS

---

# Slide 01 - Tujuan Pembelajaran dan Posisi dalam Rangkaian Perkuliahan

## Tujuan Pertemuan 02

- Membaca paper ilmiah secara sistematis: problem, novelty, metode, eksperimen, klaim, dan keterbatasan.
- Membangun kemampuan evaluasi kritis terhadap kualitas evidence dan fair baseline.
- Membedakan research gap yang ilmiah dari sekadar variasi implementasi atau penggantian dataset.
- Menghasilkan dua critical paper review dan satu matriks literatur awal.

## Posisi dalam Course

| Pertemuan | Fokus | Kaitan dengan Pertemuan 02 |
|---|---|---|
| 01 | Peta Riset Mutakhir PCD | Menyediakan peta area riset dan masalah terbuka yang menjadi bahan pemilihan paper |
| 02 | Critical Paper Reading dan Research Gap | Membaca paper secara mendalam untuk menguji klaim dan menemukan celah ilmiah |
| 03 | Representasi Visual Modern | Menjadi objek kajian: CNN, Attention, Vision Transformer |

## Alur Belajar

Pertemuan 01 → memilih area dan daftar kandidat paper.

Pertemuan 02 → membaca kritis, memetakan literatur, menemukan gap.

Pertemuan 03 → memahami representasi visual untuk mengevaluasi metode secara teknis.

---

# Slide 02 - Mengapa Critical Paper Reading Menjadi Kompetensi Inti di Jenjang Doktor

## Tuntutan Keilmuan S3

- Kontribusi ilmiah harus melampaui penerapan metode pada data baru.
- Novelty harus diposisikan terhadap state-of-the-art secara eksplisit.
- Kemampuan mengkritisi paper orang lain menjadi bekal mengkritisi proposal sendiri.

## Kebiasaan yang Harus Dibangun

- Membaca bukan sekadar memahami isi paper, melainkan menguji apakah argumen paper konsisten.
- Setiap klaim dikaitkan dengan bukti eksperimen yang tersedia.
- Setiap keterbatasan paper dapat menjadi awal pertanyaan penelitian baru.

## Dampak pada Proposal Disertasi

- Penajaman research question.
- Pemilihan baseline yang adil.
- Rancangan eksperimen yang mampu membuktikan kontribusi.

---

# Slide 03 - Dari Peta Riset ke Daftar Paper yang Layak Dibaca Kritis

## Pertemuan 01 memberikan tiga hal

1. Peta paradigma: citra klasik, deep learning, foundation model, generative vision, trustworthy vision.
2. Daftar masalah terbuka yang relevan dengan minat disertasi.
3. Standar benchmark dan dataset yang umum digunakan.

## Seleksi Paper di Pertemuan 02

- Pilih paper dari sumber: arXiv, OpenReview, jurnal seperti TPAMI/IJCV, atau konferensi seperti CVPR/ICCV/ECCV/NeurIPS/ICLR/ICML.
- Perhatikan ketersediaan kode, data, dan reproduksibilitas.
- Sertakan paper lama yang menjadi fondasi dan paper baru yang mewakili state-of-the-art.

## Kriteria Seleksi

- Relevansi terhadap masalah riset.
- Kualitas venue dan sitasi komunitas.
- Ketersediaan artefak: kode, model, dataset.
- Potensi menimbulkan pertanyaan kritis dan gap.

---

# Slide 04 - Struktur Paper Ilmiah: Anatomi Argumen

## Bagian Utama Paper

| Bagian | Fungsi | Pertanyaan Kritis |
|---|---|---|
| Abstract | Ringkasan klaim dan kontribusi | Apa yang diklaim? Bukti apa yang ditawarkan? |
| Introduction | Motivasi, masalah, kontribusi | Mengapa masalah penting? Apa gap-nya? |
| Related Work | Positioning terhadap penelitian lain | Apakah pembanding dikutip secara adil? |
| Method | Deskripsi solusi | Apa asumsi? Mengapa desain ini dipilih? |
| Experiments | Validasi klaim | Apakah eksperimen dapat memisahkan pengaruh komponen? |
| Conclusion | Ringkasan dan keterbatasan | Apakah kesimpulan melebihi bukti? |

## Cara Membaca

- Baca abstract dan conclusion terlebih dahulu untuk menangkap klaim.
- Baca experiments untuk menguji seberapa kuat bukti.
- Baca method untuk memahami apa yang sebenarnya dikerjakan.
- Baca related work terakhir untuk melihat positioning.

---

# Slide 05 - Empat Lapis Strategi Membaca Paper

## 1. Membaca Cepat

- Memindai judul, abstract, gambar utama, dan kesimpulan.
- Tujuan: memutuskan apakah paper relevan.

## 2. Membaca Kerangka

- Memahami struktur argumen: masalah → metode → eksperimen → klaim.
- Tujuan: mendapat peta logika paper.

## 3. Membaca Mendalam

- Menganalisis formula, arsitektur, protokol eksperimen, dan metrik.
- Tujuan: memahami detail teknis dan asumsi.

## 4. Membaca Reviewer-Style

- Menguji kelemahan, pertanyaan, dan potensi perbaikan.
- Tujuan: menilai apakah paper layak menjadi fondasi riset.

## Rekomendasi

- Jangan selalu membaca paper secara linear dari awal hingga akhir.
- Sesuaikan kedalaman dengan tujuan: mencari gap vs mempelajari metode.

---

# Slide 06 - Fase Awal: Klarifikasi Problem Statement

## Sebelum Menilai Metode, Pahami Masalahnya

- Apa fenomena atau kebutuhan yang memotivasi paper?
- Mengapa solusi yang sudah ada dianggap belum memadai?
- Apa tujuan akhir yang ingin dicapai?

## Rumusan Problem Statement yang Baik

- Spesifik: tidak terlalu luas.
- Terukur: ada indikator keberhasilan.
- Berkonteks: terkait dengan domain PCD tertentu.

## Contoh Pertanyaan Klarifikasi

- Apakah masalahnya bersifat teknis? Misalnya representasi fitur belum cukup semantik.
- Apakah masalahnya bersifat empiris? Misalnya model gagal pada domain tertentu.
- Apakah masalahnya bersifat teoretis? Misalnya tidak ada jaminan konvergensi.

---

# Slide 07 - Mengidentifikasi Problem dan Motivasi pada Paper

## Elemen Motivasi

- Kelemahan metode sebelumnya.
- Kebutuhan aplikasi nyata.
- Perubahan karakteristik data.
- Keterbatasan sumber daya komputasi.

## Tanda Problem Tidak Jelas

- Motivasi ditulis umum dan tidak mengarah pada pertanyaan spesifik.
- Tidak ada penjelasan mengapa pendekatan lama gagal.
- Kontribusi tidak berkorespondensi langsung dengan masalah.

## Latihan Saat Membaca

Catat dalam satu atau dua kalimat:

- Problem utama paper adalah ...
- Keterbatasan metode sebelumnya yang menjadi motivasi adalah ...
- Kebutuhan domain yang mendorong paper adalah ...

---

# Slide 08 - Taksonomi Kontribusi Ilmiah

## Kontribusi Tidak Hanya Metode Baru

| Jenis Kontribusi | Deskripsi |
|---|---|
| Teori | Analisis formal, pemahaman sifat model, batas teoretis |
| Algoritma/Metode | Arsitektur, prosedur, atau strategi optimasi baru |
| Dataset | Data baru, anotasi, atau protokol pengumpulan data |
| Evaluasi/Metrik | Ukuran kualitas baru yang lebih sesuai |
| Framework/Pipeline | Sistem yang menyatukan komponen secara baru |
| Analisis/Empiris | Studi sistematis yang mengubah pemahaman bidang |

## Pertanyaan untuk Paper

- Kontribusi utama termasuk kategori apa?
- Apakah ada kontribusi sekunder yang tidak dinyatakan penulis?
- Apakah kontribusi yang dinyatakan benar-benar baru?

---

# Slide 09 - Contoh Taksonomi Kontribusi dalam PCD

## Dimensi Kontribusi

- Representasi visual: patch embedding, attention, fitur self-supervised.
- Arsitektur: residual connection, transformer encoder, decoder segmentation.
- Prosedur pelatihan: pretext task, kontrastif, masked image modeling.
- Data: dataset domain medis, citra satelit, atau kurasi benchmark.
- Evaluasi: metrik perseptual untuk restoration, protokol evaluasi bias.

## Pembacaan Kritis

- Paper sering mencampur kontribusi arsitektur dan kontribusi pelatihan.
- Tanyakan: apakah kontribusi utama terletak pada desain model, data, atau analisis?
- Status S3 membutuhkan kontribusi yang dapat dipertanggungjawabkan secara ilmiah, bukan sekadar akurasi.

---

# Slide 10 - Kualitas Related Work: Positioning, Bukan Daftar Pustaka

## Ciri Related Work yang Lemah

- Hanya menyebut nama metode dan hasil tanpa analisis.
- Tidak menjelaskan perbedaan dengan pendekatan penulis.
- Mengabaikan baseline penting yang seharusnya dibandingkan.

## Ciri Related Work yang Kuat

- Mengelompokkan pendekatan berdasarkan ide utama.
- Menjelaskan kelebihan dan keterbatasan masing-masing kelompok.
- Menegaskan posisi paper terhadap kelompok tersebut.
- Menunjukkan gap yang belum diselesaikan.

## Kegunaan untuk Identifikasi Gap

- Related work yang baik memetakan ruang solusi.
- Ruang kosong dalam pemetaan menunjukkan calon research gap.
- Paper yang mengaburkan perbedaan biasanya ingin menyembunyikan keterbatasan.

---

# Slide 11 - Positioning Paper: Comparing vs. Situating

## Dua Cara Memosisikan Penelitian

| Cara | Orientasi | Tujuan |
|---|---|---|
| Comparing | Membandingkan performa dengan metode lain | Menunjukkan keunggulan kuantitatif |
| Situating | Menempatkan dalam peta konseptual bidang | Menjelaskan posisi ilmiah dan asumsi |

## Dalam Critical Reading

- Perhatikan apakah paper menjelaskan hubungan konseptual dengan pendekatan lama.
- Apakah paper menjelaskan kapan pendekatannya cocok dan kapan tidak?
- Paper yang hanya "lebih baik dari X" belum tentu memberikan pemahaman baru.

## Untuk Penelitian S3

- Positioning yang kuat membantu menyusun argumentasi novelty.
- Positioning juga membantu memilih eksperimen pembanding secara adil.

---

# Slide 12 - Baseline: Titik Rujukan Perbandingan

## Definisi Baseline

- Metode sederhana atau metode yang sudah mapan sebagai pembanding.
- Baseline digunakan untuk mengetahui apakah kontribusi benar-benar memberi perbaikan.

## Jenis Baseline

| Jenis Baseline | Contoh dalam PCD |
|---|---|
| Sederhana | Rata-rata nilai piksel, interpolasi, mayoritas kelas |
| Klasik | Filter, feature engineering, SVM |
| Modern | CNN ResNet, ViT standar, YOLO |
| Upper bound | Anotasi manusia, oracle, model dengan informasi tambahan |

## Pertanyaan Kritis

- Apakah baseline dipilih karena paling merepresentasikan state-of-the-art?
- Apakah baseline dilatih ulang dengan konfigurasi yang adil?
- Apakah baseline yang kuat dihilangkan agar selisih performa terlihat besar?

---

# Slide 13 - Fair Baseline dan Kesalahan Umum Perbandingan

## Prinsip Perbandingan yang Adil

- Semua metode menggunakan data train/val/test yang sama.
- Preprocessing dan augmentasi tidak berbeda antara baseline dan metode usulan.
- Hyperparameter baseline dioptimalkan, bukan hanya diambil dari default.
- Computational budget dilaporkan agar selisih performa tidak berasal dari komputasi.

## Kesalahan Umum

- Menggunakan checkpoint publik tanpa fine-tuning yang setara.
- Tidak menyebut jumlah parameter dan FLOPs.
- Melaporkan hasil terbaik metode usulan vs hasil rata-rata baseline.
- Menggunakan metrik yang menguntungkan metode usulan.

## Implikasi untuk Research Gap

- Jika baseline tidak adil, gap performa yang dilaporkan bisa menyesatkan.
- Membaca baseline secara kritis adalah bagian dari identifikasi gap.

---

# Slide 14 - Ablation Study: Membuktikan Kontribusi Setiap Komponen

## Definisi

- Eksperimen pengurangan atau penambahan komponen untuk mengukur pengaruhnya terhadap hasil.

## Ciri Ablation yang Baik

- Setiap komponen utama diuji dampaknya.
- Hasil dilaporkan pada metrik yang sama dan data yang sama.
- Kesimpulan konsisten dengan arah perubahan performa.

## yang Harus Dicari Saat Membaca

| Komponen | Pertanyaan |
|---|---|
| Modul baru | Apakah performa turun saat modul dihapus? |
| Loss / objective | Apakah pemilihan loss dibuktikan? |
| Data / augmentasi | Apakah pengaruh data dipisahkan dari pengaruh metode? |
| Hyperparameter | Apakah sensitivitas parameter dilaporkan? |

## Tanpa Ablation, Klaim Kontribusi Tidak Kuat

- Tidak dapat diketahui apakah peningkatan berasal dari komponen yang diklaim.
- Kemungkinan besar peningkatan hanya berasal dari engineering detail.

---

# Slide 15 - Experimental Setup: Dataset, Split, Metrik, dan Detail Implementasi

## Elemen yang Wajib Diperhatikan

- Dataset: ukuran, asal, lisensi, karakteristik domain.
- Split: train/validation/test, cara sampling, potensi data leakage.
- Metrik: definisi, keunggulan, keterbatasan, kapan metrik cocok.
- Implementasi: framework, versi library, resolusi input, jumlah epoch, learning rate, batch size, random seed.

## Tanda Reproducibility Buruk

- Tidak ada kode publik.
- Hyperparameter tidak dilaporkan.
- Tidak ada seed yang digunakan.
- Konfigurasi hardware tidak disebutkan.

## Dampak pada Penilaian Klaim

- Jika setup tidak lengkap, pembaca tidak dapat memverifikasi eksperimen.
- Hasil yang tidak reproducible tidak dapat menjadi dasar research gap yang kuat.

---

# Slide 16 - Mengevaluasi Klaim versus Bukti

## Prinsip Dasar

Setiap klaim harus diperiksa ketersediaan buktinya.

## Tabel Kecocokan Klaim dan Bukti

| Klaim Paper | Bukti yang Dibutuhkan | Bukti yang Sering Ditemukan | Penilaian |
|---|---|---|---|
| Metode lebih baik | Perbandingan dengan baseline pada metrik yang sama | Tabel perbandingan | Cek signifikansi statistik |
| Komponen perlu | Ablation study | Tabel ablation | Cek apakah semua komponen diuji |
| Umum/domain luas | Evaluasi lintas domain | Evaluasi satu domain | Cek generalisasi |
| Cepat/ringan | Ukuran parameter dan waktu | Tidak dilaporkan | Cek kelengkapan |
| Pemahaman lebih baik | Analisis interpretability | Hanya visualisasi contoh | Cek kedalaman analisis |

## Pertanyaan Kunci

- Apakah klaim dalam abstract didukung oleh eksperimen pada body paper?
- Apakah kesimpulan analogi atau spekulasi dinyatakan sebagai fakta?

---

# Slide 17 - Threats to Validity: Sistematika Kelemahan Paper

## Empat Kategori Umum

| Jenis Threat | Fokus | Contoh dalam PCD |
|---|---|---|
| Internal Validity | Apakah hubungan sebab-akibat benar? | Peningkatan akurasi karena kontribusi atau karena tuning? |
| External Validity | Apakah hasil dapat digeneralisasi? | Model hanya diuji pada satu dataset |
| Construct Validity | Apakah ukuran sesuai dengan konsep? | Metrik akurasi tidak mencerminkan kualitas persepsi visual |
| Conclusion Validity | Apakah kesimpulan statistik benar? | Tidak ada multiple run atau uji statistik |

## Kegunaan untuk Research Gap

- Ancaman validitas menunjukkan area yang belum diselesaikan.
- Ketidaklengkapan evaluasi dapat menjadi peluang penelitian.

---

# Slide 18 - Reproducibility: Syarat Kontribusi Dapat Dipercaya

## Reproducibility Bukan Sekadar Kode Tersedia

- Kode harus dapat dijalankan pada lingkungan yang terdokumentasi.
- Versi dependency dan library harus dicatat.
- Random seed, data split, dan preprocessing harus eksplisit.

## Tingkatan Reproducibility

1. Dokumentasi: penjelasan konfigurasi eksperimen.
2. Artefak: kode, model, dataset tersedia.
3. Verifikasi: eksperimen dapat dijalankan ulang dengan hasil serupa.
4. Replikasi: penelitian ulang pada data baru untuk menguji generalisasi.

## Pertanyaan Kritis

- Apakah semua hyperparameter dilaporkan?
- Apakah hasil adalah rata-rata dari beberapa kali run?
- Apakah konfigurasi baseline tersedia?

---

# Slide 19 - Pertanyaan Kunci Saat Membaca Paper

## Pertanyaan Inti Pertemuan 02

1. Apakah klaim paper didukung bukti yang memadai?
2. Apakah baseline yang digunakan adil?
3. Apa eksperimen tambahan yang diperlukan untuk menguji klaim tersebut?
4. Kapan metode ini berlaku dan kapan gagal?
5. Apa asumsi yang tidak dinyatakan penulis?

## Pertanyaan Tambahan

- Apakah metrik yang dipilih selaras dengan tujuan masalah?
- Apakah perbedaan performa signifikan secara praktis?
- Apakah komputasi yang diperlukan sepadan dengan peningkatan hasil?

## Catatan untuk Research Log

- Jawab pertanyaan ini dalam tulisan, bukan hanya dalam kepala.
- Simpan jawaban sebagai anotasi pada paper.

---

# Slide 20 - Checklist Critical Paper Reading

## Tahap Analisis

### Problem

- [ ] Masalah dinyatakan secara eksplisit.
- [ ] Motivasi didukung data atau contoh nyata.

### Method

- [ ] Ide utama dijelaskan dengan jelas.
- [ ] Asumsi dan batasan dinyatakan.

### Experiments

- [ ] Dataset dan split dijelaskan.
- [ ] Baseline adil dan representatif.
- [ ] Metrik sesuai dan tidak dipilih sepihak.
- [ ] Ablation study menutupi semua komponen utama.

### Claim

- [ ] Klaim tidak melampaui hasil.
- [ ] Keterbatasan dibahas.

### Reproducibility

- [ ] Kode dan lingkungan tersedia atau memungkinkan untuk direplikasi.

---

# Slide 21 - Research Gap: Definisi dan Ciri

## Definisi

- Research gap adalah masalah atau pertanyaan ilmiah yang belum terjawab secara memadai oleh literatur yang ada.

## Ciri Gap yang Layak Diteliti

- Belum ada solusi yang memuaskan.
- Relevan bagi komunitas ilmiah atau kebutuhan nyata.
- Dapat dirumuskan menjadi pertanyaan penelitian yang teruji.
- Memungkinkan dirancangnya eksperimen untuk memperoleh bukti.

## yang Bukan Research Gap

- Belum pernah dicoba pada dataset tertentu.
- Implementasi metode X padahal domainnya bisa memakai Y tanpa analisis.
- Mengganti backbone tanpa pertanyaan ilmiah baru.

---

# Slide 22 - Gap Ilmiah vs Variasi Implementasi vs Pengganti Dataset

## Perbandingan

| Jenis | Pertanyaan Ilmiah | Nilai Kontribusi |
|---|---|---|
| Gap ilmiah | Mengapa metode lama gagal, dan bagaimana prinsip baru dapat mengatasinya? | Tinggi |
| Variasi implementasi | Bagaimana merekayasa komponen agar bekerja lebih cepat atau lebih akurat? | Sedang |
| Pengganti dataset | Apakah metode X bekerja pada data Y? | Rendah/tergantung domain |

## Contoh Gap Ilmiah vs Pengganti Dataset

- Pengganti dataset: menerapkan ViT pada dataset medis dan melaporkan akurasi.
- Gap ilmiah: menganalisis mengapa representasi ViT kurang robust pada domain medis dan merancang representasi baru berbasis pemahaman domain.

## Pesan Utama

- Gunakan pertanyaan "mengapa" dan "bagaimana" untuk mengangkat variasi implementasi menjadi gap ilmiah.

---

# Slide 23 - Tipologi Research Gap

## Jenis Gap yang Dapat Diidentifikasi

| Tipe Gap | Fokus | Contoh dalam PCD |
|---|---|---|
| Knowledge gap | Pengetahuan yang belum ada | Bagaimana fitur self-supervised berperilaku pada domain spesifik |
| Methodological gap | Metode belum memadai | Belum ada pendekatan yang menggabungkan struktur 3D dan representasi semantik |
| Empirical gap | Bukti empiris kurang | Evaluasi lintas domain belum dilakukan |
| Theoretical gap | Teori belum menjelaskan | Tidak ada analisis tentang batas performa model generative |
| Population/domain gap | Domain tertentu belum terwakili | Citra bawah air, citra satelit, citra medis langka |

## Catatan

- Gap dapat berada pada lebih dari satu tipe.
- Tuliskan tipe gap secara eksplisit untuk memperjelas novelty.

---

# Slide 24 - Sumber Research Gap yang Dapat Ditemukan dari Paper

## Lokasi Gap dalam Paper

- Bagian Future Work: penulis menyatakan apa yang belum dikerjakan.
- Bagian Limitation: keterbatasan yang diakui penulis.
- Bagian Related Work: celah yang tidak dibahas atau tidak dijawab.
- Bagian Experiments: hasil yang hanya diuji pada kondisi sempit.

## Sumber Lain

- Kegagalan replikasi atau ketidakstabilan hasil.
- Pergeseran karakteristik data dari waktu ke waktu.
- Kemajuan teknologi yang membuat asumsi lama tidak berlaku.

## Latihan

- Untuk setiap paper yang dibaca, catat: "Penulis mengakui belum menangani ..."
- Jika keterbatasan paper menjawab masalah yang relevan, gap mulai terlihat.

---

# Slide 25 - Gap, Research Question, Hipotesis, dan Novelty

## Alur Konseptual

Literatur → Gap → Research Question → Hipotesis → Novelty → Kontribusi

## Definisi

| Istilah | Makna |
|---|---|
| Research Gap | Masalah yang belum terjawab |
| Research Question | Pertanyaan spesifik yang akan dijawab |
| Hipotesis | Dugaan ilmiah yang dapat diuji |
| Novelty | Kebaruan yang membedakan dari penelitian sebelumnya |

## Hubungan dengan Pertemuan 13

- Pertemuan 02 berfokus pada menemukan gap.
- Pertemuan 13 akan memperdalam formulasi research question, hipotesis, dan novelty.
- Matriks literatur yang dibuat sekarang menjadi bahan utama pertemuan 13.

---

# Slide 26 - Workflow Identifikasi Research Gap

## Diagram Alur

```text
Kumpulkan paper
      |
      v
Anotasi problem, metode, klaim, limitasi
      |
      v
Susun matriks literatur
      |
      v
Bandingkan klaim vs bukti
      |
      v
Temukan kelemahan / ruang kosong
      |
      v
Tulis gap candidates
      |
      v
Validasi gap: relevan? baru? dapat diuji?
      |
      v
Rumuskan research question awal
```

## Prinsip

- Gap yang baik lahir dari akumulasi pemahaman literatur.
- Satu paper dapat memicu banyak kandidat gap, tetapi hanya sedikit yang layak diteliti.

---

# Slide 27 - Matriks Literatur: Struktur Perbandingan Paper

## Fungsi Matriks Literatur

- Membandingkan paper secara konsisten.
- Melihat tren metode, dataset, dan hasil.
- Menemukan area yang belum dieksplorasi.

## Kolom yang Direkomendasikan

| Kolom | Isi |
|---|---|
| ID | Nomor urut |
| Paper | Judul, penulis, tahun, venue |
| Problem | Masalah yang diangkat |
| Method | Pendekatan utama |
| Dataset | Data yang digunakan |
| Baseline | Pembanding yang dilaporkan |
| Metric | Metrik evaluasi |
| Result | Hasil utama singkat |
| Limitation | Keterbatasan yang diakui |
| Our Notes | Catatan kritis pribadi |

## Tips

- Gunakan satu baris per paper.
- Tambahkan kolom thematic category untuk memudahkan analisis lintas paper.

---

# Slide 28 - Membuat Matriks Literatur pada Notebook

## Strategi Penyimpanan

- Gunakan pandas DataFrame pada Jupyter Notebook.
- Simpan salinan dalam format CSV agar mudah dibagikan.
- Perbarui matriks di setiap pertemuan.

## Contoh Struktur dengan Markdown Table

```markdown
| ID | Paper | Problem | Method | Dataset | Metric | Limitation |
|---|-------|---------|--------|---------|--------|------------|
| 001 | ... | ... | ... | ... | ... | ... |
```

## Pertimbangan

- Untuk puluhan paper, tabel Markdown mulai sulit dikelola.
- Gunakan pandas dan simpan sebagai CSV untuk analisis lanjutan.
- Gunakan kolom `tags` untuk kategori tematik.

---

# Slide 29 - Contoh Kode Awal Matriks Literatur dengan Python

```python
import pandas as pd

columns = [
    "id", "title", "year", "venue", "problem",
    "method", "dataset", "metric", "result",
    "limitation", "tags", "notes"
]

matrix = pd.DataFrame(columns=columns)

matrix.loc[0] = [
    1,
    "Contoh Paper Judul",
    2026,
    "CVPR",
    "Masalah utama yang dikaji",
    "Metode yang diusulkan",
    "Nama dataset",
    "PSNR / mAP / Accuracy",
    "Hasil singkat",
    "Keterbatasan yang diakui",
    "self-supervised; segmentation",
    "Catatan kritis: baseline belum adil"
]

## Simpan
matrix.to_csv("matriks_literatur.csv", index=False)
```

## Catatan

- Ganti nilai contoh dengan hasil anotasi paper aktual.
- Gunakan kolom `tags` untuk memfilter paper berdasarkan topik.

---

# Slide 30 - Anotasi Paper pada Notebook: Template Terstruktur

## Tujuan Anotasi

- Mencatat pemahaman saat membaca.
- Menyimpan jawaban atas pertanyaan kritis.
- Menjadi bahan diskusi dan matriks literatur.

## Template Anotasi Berbasis Markdown

```markdown
## Anotasi Paper
- ID: 001
- Judul: ...
- Problem:
  - Masalah yang diangkat
- Novelty:
  - Klaim kebaruan penulis
- Metode:
  - Ide utama
- Eksperimen:
  - Dataset, metrik, baseline
- Klaim vs Bukti:
  - Apakah klaim didukung?
- Keterbatasan:
  - Yang diakui penulis
- Research Gap:
  - Ide pertanyaan lanjutan
```

---

# Slide 31 - Contoh Anotasi pada Paper Pilihan RPS

## Contoh: Paper tentang Self-Supervised Vision Model

```text
ID: 002
Judul: DINOv2 (contoh dari daftar RPS)
Problem: representasi visual tanpa supervisi
      masih kurang kaya untuk berbagai tugas turunan
Novelty: mempelajari fitur visual serbaguna
      tanpa label manual pada skala besar
Metode: self-supervised learning, teacher-student,
      regularisasi dan kurasi data
Eksperimen: transfer learning ke berbagai tugas
Baseline: model supervised dan model self-supervised sebelumnya
Limitasi: data pelatihan besar dan komputasi tinggi,
      tidak semua domain diuji
Research Gap: bagaimana meminimalkan kebutuhan komputasi
      pada domain spesifik tanpa kehilangan generalisasi
```

## Catatan

- Isi ini ilustrasi anotasi, bukan klaim angka atau hasil.
- Anotasi asli disesuaikan dengan paper yang dibaca.

---

# Slide 32 - Latihan Argumentasi Kritis ala Reviewer

## Pertanyaan Reviewer untuk Setiap Paper

### Strength

- Apa kekuatan utama paper?
- Bagian mana yang paling meyakinkan?

### Weakness

- Apa kelemahan metodologis?
- Apakah ada bias dalam pemilihan baseline?
- Apakah ada eksperimen yang hilang?

### Questions

- Apa satu eksperimen yang wajib diminta kepada penulis?
- Apa batas interpretasi dari hasil yang dilaporkan?

### Decision

- Accept, minor revision, major revision, atau reject?
- Jelaskan satu alasan utama dari keputusan tersebut.

## Latihan Berpasangan

- Satu mahasiswa berperan sebagai penulis.
- Mahasiswa lain berperan sebagai reviewer.
- Gunakan pertanyaan di atas untuk diskusi 15-20 menit.

---

# Slide 33 - Simulasi Critical Review pada Abstrak Hipotetis

## Contoh Abstrak Ilustratif

```text
Kami mengusulkan metode segmentasi baru dengan
menggabungkan transformer dan modul perhatian.
Metode mencapai akurasi lebih tinggi daripada
baseline U-Net pada dataset CT scan.
```

## Kritik yang Layak Dilontarkan

- Apa klaim utama? "Lebih tinggi daripada U-Net".
- Apakah baseline cukup kuat? U-Net dasar mungkin tidak representatif.
- Apakah dataset tunggal cukup untuk menyimpulkan keunggulan?
- Apakah modul perhatian dianalisis melalui ablation?
- Apakah ukuran model dan biaya komputasi dibandingkan?
- Apakah metrik yang digunakan tepat untuk segmentasi?

## Nilai Latihan

- Melatih kepekaan terhadap klaim dan bukti.
- Menghasilkan pertanyaan yang menjadi bahan eksperimen lanjutan.

---

# Slide 34 - Praktikum: Struktur Notebook Critical Paper Review

## Bagian Notebook

1. **Identitas Paper**: metadata dan link.
2. **Ringkasan**: abstract dalam bahasa sendiri.
3. **Problem**: masalah, motivasi, gap yang diklaim penulis.
4. **Metode**: alur diagram atau bullet.
5. **Eksperimen**: dataset, baseline, metrik, hasil.
6. **Evaluasi Kritis**: klaim vs bukti, threats to validity.
7. **Keterbatasan dan Gap**: peluang riset lanjutan.

## Format

- Gunakan H2 untuk setiap bagian.
- Simpan satu notebook per paper.
- Gunakan sel Markdown untuk narasi dan sel Python untuk tabel/metrik.

---

# Slide 35 - Praktikum: Workflow Pembuatan Matriks Literatur

## Urutan Kerja

```text
1. Buat daftar 5-10 paper dari reading list.
2. Baca paper secara sistematis.
3. Isi template anotasi untuk setiap paper.
4. Pindahkan hasil anotasi ke DataFrame.
5. Tambahkan kolom tematik dan prioritas.
6. Buat visualisasi sederhana, misalnya sebaran
   paper berdasarkan topik atau dataset.
7. Tulis catatan gap di bagian akhir notebook.
```

## Kriteria Keberhasilan

- Matriks berisi evaluasi kritis, bukan sekadar rangkuman.
- Gap yang ditulis merujuk pada kombinasi keterbatasan beberapa paper.
- Notebook dapat diperbarui sepanjang semester.

---

# Slide 36 - Praktikum: Contoh Analisis Sederhana Matriks Literatur

```python
import pandas as pd

df = pd.read_csv("matriks_literatur.csv")

## Sebaran paper berdasarkan topik
print(df["tags"].value_counts())

## Paper yang menyebutkan keterbatasan tertentu
mask = df["limitation"].str.contains("domain", case=False)
print(df.loc[mask, ["title", "limitation"]])

## Perbandingan dataset yang dipakai
print(df["dataset"].value_counts())
```

## Interpretasi

- Jika banyak paper menggunakan dataset yang sama, ada risiko overfitting benchmark.
- Jika banyak paper mengakui limitasi domain yang sama, indikasi gap domain muncul.
- Catatan ini dijadikan bahan diskusi pertemuan 13.

---

# Slide 37 - Aktivitas Seminar Paper dan Diskusi Kelompok

## Format Seminar Paper

- Satu kelompok menyajikan satu paper secara kritis.
- Penyaji membahas problem, metode, eksperimen, klaim, dan keterbatasan.
- Peserta lain bertindak sebagai reviewer.

## Peran Reviewer

- Menyiapkan minimal tiga pertanyaan kritis.
- Menilai cukup tidaknya bukti untuk setiap klaim.
- Mengusulkan satu eksperimen tambahan.

## Keluaran Diskusi

- Catatan komentar dan sanggahan.
- Daftar masalah terbuka yang mungkin menjadi gap.
- Rekomendasi perbaikan paper atau arah riset lanjutan.

---

# Slide 38 - Target Keluaran Pertemuan 02

## Produk yang Harus Dihasilkan

1. **Dua critical paper review**: deskripsi sistematis, evaluasi kritis, dan rekomendasi riset lanjutan.
2. **Satu matriks literatur**: perbandingan problem, data, metode, metrik, hasil, dan keterbatasan.

## Bentuk Penyerahan

- Jupyter Notebook untuk setiap critical paper review.
- Satu notebook atau CSV berisi matriks literatur.
- Semua artefak disimpan dalam repositori Git yang rapi.

## Kualitas yang Dinilai

- Ketajaman analisis, bukan panjang dokumen.
- Kemampuan membedakan klaim dan bukti.
- Keterkaitan antara keterbatasan paper dan gap yang diidentifikasi.

---

# Slide 39 - Menuju Pertemuan Berikutnya

## Integrasi dengan Perjalanan Course

- Matriks literatur yang dibuat sekarang akan diperbarui sepanjang semester.
- Critical reading menjadi dasar untuk mengevaluasi metode pada pertemuan 03.
- Research gap yang diidentifikasi akan diperdalam menjadi research question pada pertemuan 13.

## Tugas Antara

- Selesaikan anotasi untuk dua paper yang dipilih.
- Masukkan paper tersebut ke dalam matriks literatur.
- Siapkan satu pertanyaan diskusi untuk seminar paper.

## Persiapan Pertemuan 03

- Topik: Representasi Visual Modern: CNN, Attention, dan Vision Transformer.
- Bacalah kembali struktur dasar CNN dan transformer agar diskusi perbandingan arsitektur lebih tajam.

---

# Slide 40 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Representasi Visual Modern: CNN, Attention, dan Vision Transformer**