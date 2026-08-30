# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 13
# Formulasi Research Question, Hipotesis, dan Novelty

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 13 dalam RPS

## Tempat Pertemuan 13 dalam Perjalanan Menuju Proposal Disertasi

| Tahap | Rentang Pertemuan | Fokus |
|---|---|---|
| Eksplorasi dan pemetaan riset | 1–2 | Peta riset, critical paper reading, research gap |
| Fondasi teknis dan model modern | 3–11 | Representasi visual, foundation models, generative vision, 3D vision, trustworthy CV |
| Desain eksperimen | 12 | Experimental design, reproducible benchmarking |
| **Formulasi proposal** | **13–14** | **Research question, hipotesis, novelty, metodologi disertasi** |
| Seminar dan konsolidasi | 15–16 | Seminar proposal, evaluasi SOTA, rencana publikasi |

- Pertemuan 13 adalah titik kritis: dari kebiasaan memahami metode menjadi kemampuan membangun klaim penelitian yang dapat diuji.
- Aktivitas utama: research clinic individual, diskusi proposal, dan peer feedback.
- Target keluaran: concept note penelitian berisi problem, gap, research question, hipotesis, novelty, dan calon kontribusi.

---

# Slide 02 - Recap Pertemuan 12 dan Jembatan ke Pertemuan 13

## Hasil Pertemuan 12: Experimental Design dan Reproducible Benchmarking

- Protokol eksperimen lengkap: dataset split, baseline, ablation, random seed, hyperparameter, metrik, dan pelaporan.
- Prinsip penting: eksperimen harus dapat direproduksi dan hasilnya dapat dibandingkan secara adil.
- Pada pertemuan 13, protokol tersebut menjadi alat verifikasi untuk pertanyaan penelitian yang akan dirumuskan.

## Jembatan Menuju Pertemuan 13

- Pertanyaan penelitian yang tidak tajam akan membuat desain eksperimen yang baik tetap kehilangan arah.
- Pertemuan 13 memastikan empat hal:
  1. gap yang hendak diisi;
  2. alasan gap itu penting;
  3. bukti yang akan mengonfirmasi hipotesis;
  4. manfaat ilmiah yang diperoleh.
- Pertemuan 14 akan menerjemahkan formulasi ini menjadi metodologi disertasi dan rancangan eksperimen.

---

# Slide 03 - Tujuan Pembelajaran dan Capaian Terkait

## Capaian Pembelajaran Mata Kuliah

- CPMK-5: merumuskan research problem, research question, hipotesis, research gap, novelty, dan positioning terhadap state-of-the-art.
- CPMK-6: menyusun proposal awal disertasi yang mencakup kontribusi ilmiah, metodologi, experimental design, risiko, dan rencana diseminasi.

## Tujuan Pertemuan 13

1. Mengubah topik umum menjadi masalah penelitian yang spesifik, terukur, dan dapat diuji.
2. Menyusun hubungan logis antara gap, research question, hipotesis, metode, dan klaim kontribusi.
3. Menulis contribution statement yang jelas dan membedakan novelty dari karya yang sudah ada.
4. Menyusun concept note awal beserta peta literatur dan tabel perbandingan SOTA.

---

# Slide 04 - Pertanyaan Kunci Pertemuan Ini

## Empat Pertanyaan yang Harus Dijawab Mahasiswa S3

| Pertanyaan | Ditujukan untuk |
|---|---|
| Gap apa yang hendak diisi? | Research gap dan positioning |
| Mengapa gap tersebut penting? | Problem statement dan significance |
| Bukti apa yang akan mengonfirmasi hipotesis? | Hipotesis yang dapat diuji dan desain eksperimen |
| Siapa yang memperoleh manfaat ilmiah? | Significance dan kontribusi |

## Konsekuensi Jika Belum Terjawab

- Concept note akan terasa seperti daftar pustaka, bukan proposal penelitian.
- Reviewer akan sulit menilai kebaruan dan kelayakan.
- Eksperimen di pertemuan selanjutnya tidak memiliki dasar yang kuat.

---

# Slide 05 - Dari Topik Umum Menjadi Masalah Penelitian

## Alur Penyempitan

```
Area minat
   |
   v
Topik umum
   |
   v
Literatur dan identifikasi gap
   |
   v
Research gap
   |
   v
Research question
   |
   v
Hipotesis
   |
   v
Novelty dan kontribusi
```

- Topik umum: "image restoration".
- Masalah penelitian: "bagaimana mengurangi detail halusinatif pada restorasi citra medis berdosis rendah menggunakan diffusion model?"
- Topik menunjukkan bidang; masalah menunjukkan pertanyaan yang dapat dijawab secara sistematis.

---

# Slide 06 - Anatomi Formulasi Riset

## Komponen Utama yang Saling Terhubung

| Komponen | Pertanyaan yang Dijawab | Peran dalam Proposal |
|---|---|---|
| Problem statement | Mengapa penelitian ini perlu dilakukan? | Memberi alasan dan urgensi |
| Research gap | Apa yang belum diketahui? | Menentukan celah literatur |
| Research question | Apa yang ingin dijawab? | Mengarahkan cakupan penelitian |
| Hipotesis | Apa jawaban sementara yang dapat diuji? | Memberikan prediksi |
| Novelty | Apa yang baru? | Menunjukkan kontribusi ilmiah |
| Contribution statement | Apa yang disumbangkan kepada ilmu pengetahuan? | Menegaskan hasil akhir |

## Catatan

- Seluruh komponen harus konsisten: jika gap berubah, RQ dan hipotesis ikut berubah.
- Pertemuan ini fokus pada komponen pertama sampai keenam; detail metode dibahas pada pertemuan 14.

---

# Slide 07 - Problem Statement

## Definisi

Problem statement adalah pernyataan yang menjelaskan masalah nyata atau kebutuhan ilmiah yang belum teratasi dalam bidang pengolahan citra digital, dengan disertai konteks dan konsekuensinya.

## Kriteria Problem Statement yang Baik

- Spesifik: menyebut objek, kondisi, atau domain secara jelas.
- Berbasis kebutuhan: ada alasan mengapa masalah itu penting.
- Menyebut kesenjangan: apa yang diketahui dan apa yang hilang.
- Menyebut konsekuensi: apa yang terjadi jika masalah dibiarkan.

## Kesalahan Umum

- "Akurasi model masih rendah" tanpa menjelaskan mengapa dan pada kondisi apa.
- "Belum banyak penelitian" tanpa menunjukkan mengapa literatur yang ada tidak memadai.
- "Dataset terbatas" tanpa mengaitkan dengan dampak ilmiah atau praktis.

---

# Slide 08 - Menulis Problem Statement yang Kuat

## Template Sederhana

> Meskipun metode X telah mencapai hasil yang baik pada kondisi Y, metode tersebut masih memiliki keterbatasan pada kondisi Z, sehingga [dampak konkret].

## Contoh

- "Meskipun diffusion model mampu menghasilkan citra restorasi berkualitas tinggi, metode tersebut belum stabil pada citra CT dosis rendah dengan degradasi campuran, sehingga dapat menghasilkan detail halusinatif yang menyesatkan diagnosis."
- "Foundation model segmentasi menunjukkan generalisasi tinggi pada citra natural, tetapi belum tervalidasi pada citra satelit resolusi tinggi dengan objek kecil, sehingga penerapannya pada pemantauan lingkungan masih berisiko."

## Latihan

- Tulis satu paragraf problem statement untuk topik penelitian Anda.
- Hindari frasa "lebih baik" tanpa penjelasan mekanisme atau kondisi.

---

# Slide 09 - Research Gap

## Definisi Research Gap

Research gap adalah celah antara apa yang sudah diteliti dan apa yang belum diketahui, yang memiliki alasan kuat untuk diisi.

## Sumber Research Gap

- Keterbatasan metode yang ada.
- Perubahan distribusi data di dunia nyata.
- Masalah baru yang muncul akibat teknologi baru.
- Kebutuhan aplikasi yang belum terdukung.
- Evaluasi yang tidak memadai untuk kondisi penting.
- Kurangnya pemahaman tentang mengapa sebuah metode berhasil atau gagal.

## Perhatian

- "Belum pernah diterapkan di domain X" bukan otomatis menjadi gap ilmiah.
- Gap yang kuat harus didukung oleh alasan mengapa domain atau kondisi X memberikan tantangan berbeda.

---

# Slide 10 - Taksonomi Gap

## Jenis-Jenis Gap Penelitian

| Jenis Gap | Deskripsi |
|---|---|
| Empirical gap | Belum ada bukti empiris pada kondisi atau populasi tertentu. |
| Theoretical gap | Teori atau konsep belum cukup menjelaskan fenomena. |
| Methodological gap | Metode yang ada belum tepat atau belum ada sama sekali. |
| Domain gap | Metode belum diuji pada domain yang memiliki karakteristik berbeda. |
| Data gap | Data yang dibutuhkan belum tersedia atau belum cukup berkualitas. |
| Evaluation gap | Evaluasi yang ada belum mampu menangkap aspek penting dari kinerja sistem. |

## Implikasi

- Satu penelitian dapat mengisi kombinasi dua atau tiga jenis gap.
- Sebutkan jenis gap secara eksplisit agar novelty lebih mudah dikomunikasikan.

---

# Slide 11 - Contoh Pemetaan Gap pada Literatur

## Lintasan Berpikir

1. Kajian paper: SAM menunjukkan generalisasi kuat pada citra natural.
2. Pengamatan: pada citra satelit resolusi sangat tinggi, objek kecil dan tepi tidak tegas membuat mask kurang presisi.
3. Evaluasi standar tidak cukup sensitif terhadap boundary error pada objek kecil.
4. Anotasi manual pada skala luas tidak praktis.

## Gap yang Teridentifikasi

- Domain gap: SAM belum memadai untuk citra satelit resolusi tinggi.
- Evaluation gap: perlu protokol metrik yang memperhatikan objek kecil dan batas tepi.
- Data gap: belum ada benchmark yang merepresentasikan kondisi tersebut.

## Peluang

- Mengembangkan strategi prompt atau adaptasi SAM untuk objek kecil pada citra satelit.
- Menyediakan benchmark dan evaluasi yang lebih adil.

---

# Slide 12 - Research Question

## Definisi

Research question adalah pertanyaan yang dapat dijawab melalui penyelidikan sistematis, biasanya dengan data dan metode yang dirancang secara eksplisit.

## Karakteristik RQ yang Baik

- Fokus: tidak terlalu luas atau terlalu sempit.
- Dapat diuji: ada cara untuk mengumpulkan bukti.
- Relevan: berhubungan dengan gap dan kepentingan ilmiah.
- Jelas: tidak bermakna ganda.

## Contoh Perbaikan

- Terlalu luas: "Apakah deep learning dapat memperbaiki pencitraan?"
- Lebih tajam: "Sejauh mana penambahan constraint fisik pada diffusion model mengurangi detail halusinatif pada citra CT dosis rendah?"

---

# Slide 13 - Jenis-Jenis Research Question

## Kategori RQ

| Jenis | Contoh Format |
|---|---|
| Deskriptif | Bagaimana distribusi karakteristik degradasi pada citra domain X? |
| Komparatif | Apakah metode A lebih unggul daripada B pada kondisi C? |
| Relasional | Bagaimana hubungan antara ukuran objek dan kualitas segmentasi? |
| Mekanistik/Kausal | Mengapa penambahan modul tertentu mengubah perilaku model? |

## Catatan untuk Jenjang S3

- RQ deskriptif umumnya terlalu dangkal untuk disertasi.
- RQ yang kuat biasanya bersifat komparatif atau relasional dengan penjelasan mekanisme.
- RQ juga dapat dirumuskan sebagai "to what extent" atau "under what condition" agar hasilnya informatif.

---

# Slide 14 - RQ yang Dapat Diuji

## Empat Syarat Utama

- **Spesifik**: ada objek, kondisi, dan cakupan yang jelas.
- **Measurable**: outcome yang diamati dapat diukur dengan metrik atau prosedur.
- **Answerable**: data dan sumber daya yang tersedia memungkinkan penyelidikan.
- **Falsifiable**: ada hasil yang mungkin dan dapat menggugurkan prediksi.

## Pertanyaan Diri

- Apa observasi yang akan menjawab RQ ini?
- Apa yang saya lakukan jika hasil observasi tidak mendukung prediksi?
- Apakah RQ ini dapat dijawab dalam kerangka waktu disertasi?

## Hubungan dengan Pertemuan 12

- RQ yang baik menuntun pemilihan baseline, metrik, dan ablation pada desain eksperimen.

---

# Slide 15 - Dari Research Question ke Hipotesis

## Apa Itu Hipotesis?

Hipotesis adalah jawaban sementara terhadap research question yang dirumuskan berdasarkan teori, analisis literatur, atau observasi awal, dan dapat diuji dengan eksperimen.

## Format Umum

> Jika [perlakuan/kondisi diterapkan], maka [outcome tertentu akan terjadi], karena [mekanisme yang diusulkan].

## Contoh

- RQ: "Apakah penambahan geometri guidance pada diffusion model meningkatkan konsistensi struktur pada area yang ter-occlude?"
- Hipotesis: "Penambahan geometri guidance akan meningkatkan konsistensi struktur pada area yang ter-occlude dibandingkan baseline tanpa guidance, karena model memiliki constraint yang menjaga kesesuaian dengan geometri scene."

---

# Slide 16 - Hipotesis dalam Konteks Eksperimen Sains

## Hipotesis Riset dan Hipotesis Statistik

- Hipotesis riset: klaim ilmiah yang menjadi fokus penelitian.
- Hipotesis nol (H0): tidak ada perbedaan atau efek.
- Hipotesis alternatif (H1): ada perbedaan atau efek yang diharapkan.

## Arah Hipotesis

| Jenis | Contoh |
|---|---|
| Non-directional | Metode A dan B berbeda pada metrik M. |
| Directional | Metode A lebih unggul daripada B pada metrik M dalam kondisi C. |

## Perhatian

- Untuk disertasi PCD, hipotesis riset lebih penting daripada sekadar uji statistik.
- Eksperimen harus mampu membedakan pengaruh metode dari faktor lain seperti data, konfigurasi, atau baseline yang tidak adil.

---

# Slide 17 - Merumuskan Hipotesis yang Dapat Diuji

## Komponen Hipotesis yang Dapat Diuji

- Variabel bebas: faktor yang dimanipulasi atau dibedakan.
- Variabel terikat: outcome yang diukur.
- Kondisi eksperimen: domain data atau skenario yang digunakan.
- Prediksi kuantitatif atau kualitatif: hasil yang diharapkan.

## Tabel Perancangan Hipotesis

| RQ | Hipotesis | Variabel Bebas | Variabel Terikat | Prediksi | Metrik |
|---|---|---|---|---|---|
| Apakah eksplisit modeling degradasi meningkatkan generalisasi? | Ya, karena model belajar memisahkan degradasi dari konten | Metode degradasi modeling | Generalisasi pada unseen degradation | PSNR dan LPIPS lebih baik | PSNR, SSIM, LPIPS |

## Manfaat

- Memudahkan perancangan eksperimen pada pertemuan 14.
- Membantu menentukan apa yang harus dilaporkan sebagai bukti.

---

# Slide 18 - Hubungan RQ, Hipotesis, Metode, dan Bukti

## Rantai Argumen

```
  Research Gap
       |
       v
Research Question
       |
       v
    Hipotesis
       |
       v
Metode/Eksperimen
       |
       v
   Observasi/Bukti
       |
       v
 Klaim Kontribusi
```

## Prinsip Konsistensi

- Setiap perubahan pada gap harus diikuti penyesuaian RQ, hipotesis, dan klaim.
- Metode adalah alat untuk menghasilkan bukti, bukan tujuan utama.
- Klaim kontribusi tidak boleh melampaui kekuatan bukti yang dihasilkan.

---

# Slide 19 - Novelty

## Definisi Novelty

Novelty adalah aspek baru yang membedakan penelitian Anda dari hasil yang sudah ada, dan memiliki nilai ilmiah.

## Yang Bukan Novelty

- Mengganti dataset tanpa alasan ilmiah yang kuat.
- Menggabungkan modul yang sudah ada secara ad-hoc tanpa analisis.
- Menambahkan parameter tanpa menjelaskan mengapa dan dampaknya.
- Menyajikan pipeline hasil kompilasi dari banyak metode.
- Mengklaim "lebih baik" tanpa menganalisis mengapa.

## Uji Sederhana

- Apakah ada paper yang sudah memublikasikan ide ini sebelumnya?
- Apakah perbedaan pendekatan cukup fundamental, bukan hanya detail teknis?
- Dapatkah novelty dinyatakan dalam satu kalimat yang dimengerti oleh reviewer di luar subbidang?

---

# Slide 20 - Tingkat Novelty

## Tabel Tingkat dan Contoh Kontribusi

| Tingkat | Deskripsi | Contoh dalam PCD |
|---|---|---|
| Teoretis/Analitis | Memberikan pemahaman atau prinsip baru | Analisis mengapa attention gagal pada objek kecil |
| Algoritmik/Metodologis | Mengusulkan metode, arsitektur, atau algoritma baru | Modul adaptasi prompt untuk SAM |
| Empiris | Menemukan bukti baru tentang perilaku sistem | Studi generalisasi foundation model pada domain medis |
| Dataset/Anotasi | Menyediakan data atau prosedur anotasi baru | Benchmark objek kecil untuk citra satelit |
| Sistem/Aplikasi | Membangun sistem atau workflow yang memecahkan masalah praktis | Pipeline deteksi dini berbasis drone |

## Catatan

- Disertasi S3 sebaiknya memiliki minimal satu kontribusi metodologis atau teoretis.
- Kontribusi empiris dan dataset tetap penting, tetapi kurang kuat apabila berdiri sendiri.

---

# Slide 21 - Sumber Novelty dalam Pengolahan Citra Digital

## Titik Masuk Kebaruan

- Domain atau masalah baru yang belum terpetakan.
- Karakteristik degradasi atau gangguan baru.
- Representasi atau arsitektur yang berbeda secara prinsip.
- Strategi pembelajaran baru: self-supervised, prompting, fine-tuning efisien.
- Protokol evaluasi yang lebih adil dan informatif.
- Penjelasan mekanisme mengapa model berhasil atau gagal.

## Prinsip Pemilihan

- Pilih novelty yang benar-benar dapat Anda uji dalam waktu disertasi.
- Jangan mencoba semua dimensi sekaligus.
- Hubungkan novelty dengan gap yang telah dipetakan dari literatur.

---

# Slide 22 - Menemukan Novelty dari Gap

## Workflow Sistematis

1. Identifikasi gap dari critical paper review.
2. Jelaskan mengapa gap tersebut tidak sepele.
3. Susun pendekatan atau sudut pandang yang secara prinsip berbeda.
4. Tulis klaim kontribusi awal yang dapat diuji.
5. Periksa kembali terhadap SOTA: apakah sudah ada yang memublikasikan ide serupa?
6. Jika masih orisinal, pertahankan; jika tidak, perbesar perbedaan.

## Ilustrasi Pseudocode

```
for setiap gap dalam literatur:
    if gap memiliki justifikasi kuat:
        usulkan pendekatan
        tulis contribution candidate
        bandingkan dengan SOTA
        if bukan duplikasi:
            masukkan ke concept note
```

## Catatan

- Proses ini iteratif dan membutuhkan masukan dari dosen serta peer.

---

# Slide 23 - Positioning terhadap State-of-the-Art

## Tabel Perbandingan SOTA

| Paper/Metode | Problem | Pendekatan | Dataset | Metrik | Hasil | Keterbatasan |
|---|---|---|---|---|---|---|
| Paper A | Segmentasi objek kecil | U-Net biasa | Dataset x | IoU | 0.72 | Kurang pada objek kecil |
| Paper B | Segmentasi objek kecil | SAM + box prompt | Dataset x | IoU | 0.78 | Sensitif terhadap prompt |
| Usulan | Segmentasi objek kecil | SAM + adaptor multi-skala | Dataset x | IoU, boundary IoU | ? | ? |

## Fungsi Tabel

- Menjelaskan posisi penelitian secara jujur.
- Menunjukkan bahwa Anda memahami kekuatan dan kelemahan metode sebelumnya.
- Menjadi dasar contribution statement dan perancangan eksperimen.

---

# Slide 24 - Menulis Contribution Statement

## Template Satu Paragraf

> Penelitian ini menyumbang [kontribusi] berupa [objek], yang mengatasi [gap] melalui [prinsip atau ide], berbeda dari [SOTA] pada [aspek], dan diuji dengan [bukti eksperimen].

## Contoh

- "Penelitian ini menyumbang metode segmentasi adaptif prompt untuk citra satelit resolusi tinggi yang mengatasi ketidakstabilan SAM pada objek kecil melalui mekanisme fokus multi-skala. Kontribusi ini berbeda dari pendekatan fine-tuning penuh yang membutuhkan GPU besar, dan diuji pada benchmark baru dengan metrik yang sensitif terhadap batas objek kecil."

## Latihan

- Tulis contribution statement dalam 2–3 kalimat.
- Minta peer feedback: apakah kalimat pertama sudah cukup membedakan penelitian Anda dari SOTA?

---

# Slide 25 - Significance dan Manfaat Ilmiah

## Definisi Significance

Significance menjelaskan dampak atau kegunaan kontribusi penelitian bagi komunitas ilmiah, praktisi, dan masyarakat.

## Pertanyaan yang Membantu

- Siapa yang akan menggunakan hasil penelitian ini?
- Keputusan apa yang dapat berubah berdasarkan hasil ini?
- Penelitian lanjutan apa yang menjadi terbuka?
- Bagaimana penelitian ini memperluas pemahaman di bidang PCD?

## Contoh

- Manfaat ilmiah: menyediakan benchmark baru untuk evaluasi objek kecil; menjelaskan keterbatasan foundation model pada domain tertentu.
- Manfaat praktis: membantu analis citra satelit mendeteksi perubahan lingkungan dengan lebih akurat.

---

# Slide 26 - Kerangka Integratif Satu Halaman

## Rantai Konsep yang Harus Terlihat

```
+--------+     +-------+     +-----------+     +----------------+     +--------+
|  Gap   | --> |  RQ   | --> | Hipotesis | --> | Metode/Desain  | --> | Klaim  |
+--------+     +-------+     +-----------+     +----------------+     +--------+
                                                |                |
                                                v                v
                                         Eksperimen        Observasi/Bukti
```

## Arti Diagram

- RQ harus dirumuskan langsung dari gap.
- Hipotesis memberikan prediksi yang dapat diuji.
- Metode dan desain eksperimen menghasilkan bukti untuk memvalidasi hipotesis.
- Klaim kontribusi tidak boleh lebih luas dari bukti.

## Latihan

- Gambarkan diagram ini untuk proposal Anda.
- Jika salah satu panah tidak dapat dijelaskan, maka formulasi perlu diperbaiki.

---

# Slide 27 - Latihan: Mengubah Topik Umum Menjadi RQ

## Contoh Hasil Latihan

| Topik Umum | Gap | RQ |
|---|---|---|
| Image restoration | Diffusion model belum stabil pada degradasi campuran | Sejauh mana constraint fisik mengurangi halusinasi? |
| Segmentasi sel | Foundation model tidak robust terhadap variasi mikroskop | Bagaimana adaptasi instance-aware prompt meningkatkan IoU? |
| Deteksi objek kecil | Evaluasi mAP kurang sensitif pada objek kecil | Metrik apa yang lebih sesuai untuk objek kecil? |
| Foundation model | Embedding self-supervised belum dievaluasi pada domain X | Apakah DINOv2 superior dibanding CNN pada domain X? |

## Tugas

- Isi tabel untuk topik penelitian Anda sendiri.
- Pastikan gap yang dipilih memiliki alasan, bukan sekadar "belum ada".

---

# Slide 28 - Studi Kasus 1: Diffusion Model untuk Citra Medis

## Perumusan Masalah

- Topik: "Diffusion model untuk citra medis" masih terlalu umum.
- Literatur menunjukkan: diffusion restoration baik untuk PSNR, tetapi berisiko menghasilkan detail halusinatif pada area dengan sinyal rendah.
- Gap: belum ada metode yang mengintegrasikan constraint fisik secara langsung ke dalam proses sampling.

## RQ yang Dibentuk

- RQ utama: "Sejauh mana penambahan constraint konsistensi data pada proses denoising diffusion mengurangi detail halusinatif pada citra CT dosis rendah?"
- RQ turunan: "Pada tingkat noise berapakah efek constraint mulai signifikan?"

---

# Slide 29 - Studi Kasus 2: Hipotesis dan Novelty

## Hipotesis

- "Constraint konsistensi data pada setiap langkah reverse akan menurunkan LPIPS dan meningkatkan akurasi deteksi struktur kecil dibandingkan baseline tanpa constraint."

## Novelty

- Metode: data-consistent diffusion dengan physical constraint untuk CT dosis rendah.
- Kontribusi tambahan: protokol evaluasi yang menilai detail halusinatif.
- Significance: membantu radiolog menilai keandalan citra restorasi.

## Catatan

- Ablation yang diperlukan baru dirancang pada pertemuan 14.
- Contoh ini menunjukkan pentingnya memilih gap yang dapat diterjemahkan menjadi hipotesis terukur.

---

# Slide 30 - Merancang Eksperimen untuk Menguji Hipotesis

## Prinsip dari Pertemuan 12

- Gunakan baseline yang kuat dan wajar.
- Lakukan ablation untuk menunjukkan kontribusi tiap komponen.
- Atur seed dan dokumentasikan konfigurasi.
- Gunakan analisis statistik atau interval kepercayaan bila diperlukan.

## Pemetaan Eksperimen

| Eksperimen | Tujuan | Membuktikan |
|---|---|---|
| Eksperimen A | Perbandingan baseline vs metode usulan | Adanya perbedaan yang diharapkan |
| Eksperimen B | Ablasi komponen constraint | Kontribusi masing-masing komponen |
| Eksperimen C | Uji pada variasi noise atau data | Generalisasi dan batasan |

## Catatan

- Pertemuan 13 hanya menyusun hipotesis dan bukti yang diperlukan.
- Detail implementasi eksperimen menjadi bahan pertemuan 14.

---

# Slide 31 - Bukti yang Mengonfirmasi Hipotesis

## Menentukan Kriteria Keberhasilan

- Sebelum menjalankan eksperimen, tetapkan kriteria yang mendukung atau menolak hipotesis.
- Contoh: "Hipotesis didukung jika LPIPS menurun minimal 0,02 dan akurasi deteksi struktur meningkat minimal 5% dibanding baseline, stabil pada 5 seed."

## Hasil Negatif

- Jika hipotesis tidak didukung, tetap berharga.
- Hasil negatif dapat menjadi kontribusi empiris jika analisisnya menjelaskan penyebab kegagalan.
- Tentukan sejak awal bahwa "tidak ada perbedaan" adalah hasil yang mungkin.

## Hubungan dengan Klaim

- Bukti yang lemah membatasi klaim kontribusi.
- Bukti yang kuat dan reproducible memperkuat novelty.

---

# Slide 32 - Concept Note: Struktur dan Komponen

## Struktur Concept Note

1. Judul kerja.
2. Problem statement.
3. Research gap dan positioning terhadap SOTA.
4. Research question utama dan turunan.
5. Hipotesis.
6. Novelty dan contribution statement.
7. Significance.
8. Rancangan eksperimen awal.
9. Daftar pustaka pendek.

## Target Panjang

- Concept note tidak perlu menjadi proposal penuh.
- Idealnya 3–5 halaman, cukup untuk mengundang diskusi dan menguji kelayakan ide.

---

# Slide 33 - Peta Literatur dan Tabel Perbandingan SOTA

## Langkah Membuat Peta Literatur

1. Kumpulkan 15–30 paper yang relevan dengan topik.
2. Kelompokkan ke dalam tema utama.
3. Buat tabel berisi problem, pendekatan, dataset, metrik, hasil, keterbatasan.
4. Tandai cluster yang masih memiliki gap.
5. Hubungkan gap dengan peluang kontribusi.

## Contoh Struktur Tabel

| Paper | Tema | Metode | Dataset | Hasil | Keterbatasan |
|---|---|---|---|---|---|
| Paper 1 | Super-resolution | Arsitektur CNN | DIV2K | PSNR 28.5 | Kurang pada degradasi nyata |
| Paper 2 | Super-resolution | Transformer | DIV2K | PSNR 29.0 | Komputasi tinggi |
| Usulan | Super-resolution | Efficient ViT + constraint | DIV2K + data nyata | ? | ? |

## Manfaat

- Membantu menjelaskan posisi penelitian kepada dosen dan peer.
- Memudahkan deteksi duplikasi ide.

---

# Slide 34 - Aktivitas Kelas: Research Clinic dan Peer Feedback

## Alur Research Clinic Individual

- Setiap mahasiswa menyampaikan ide konsep dalam 5 menit.
- Dosen dan mahasiswa lain memberikan pertanyaan kritis.
- Fokus diskusi: gap, RQ, hipotesis, novelty, significance.
- Setiap mahasiswa mencatat masukan untuk revisi.

## Rubrik Feedback

| Aspek | Pertanyaan Pemantik |
|---|---|
| Problem statement | Apakah konsekuensi masalah jelas? |
| Research question | Apakah dapat dijawab dengan metode yang dimiliki? |
| Hipotesis | Apakah prediksi dapat diuji dan disalahkan? |
| Novelty | Apakah berbeda dari SOTA secara prinsip? |
| Significance | Siapa yang diuntungkan dan bagaimana? |

## Prinsip Peer Feedback

- Kritik ditujukan pada argumen, bukan pribadi.
- Tawarkan alternatif, bukan hanya kelemahan.

---

# Slide 35 - Checklist Kualitas Formulasi Riset

## Sebelum Mengumpulkan Concept Note

| Aspek | Checklist |
|---|---|
| Problem statement | Spesifik, berbasis kebutuhan, ada konsekuensi |
| Research gap | Bukan sekadar "belum ada", ada alasan kuat |
| Research question | Spesifik, terukur, dapat dijawab |
| Hipotesis | Memiliki prediksi yang dapat diuji |
| Novelty | Jelas, dapat dibedakan dari SOTA |
| Contribution statement | Satu kalimat utama yang kuat |
| Significance | Menyebut manfaat ilmiah dan praktis |
| Positioning | Ada tabel/analisis perbandingan SOTA |
| Konsistensi | Gap-RQ-hipotesis-bukti-klaim saling terhubung |

## Penggunaan Checklist

- Gunakan checklist sebagai panduan menulis, bukan sekadar formalitas.
- Checklist juga menjadi bahan diskusi research clinic.

---

# Slide 36 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Metodologi Disertasi dan Rancangan Eksperimen**