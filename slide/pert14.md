# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 14
# Metodologi Disertasi dan Rancangan Eksperimen

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 14 dalam Rangkaian Perkuliahan

Pertemuan 14 merupakan titik kristalisasi proposal disertasi: dari ide dan research question menuju rancangan yang dapat diuji.

| Fase | Fokus | Keluaran |
|---|---|---|
| 1-11 | Peta riset, critical reading, model representasi, dan evaluasi | Pemahaman state-of-the-art |
| 12 | Experimental design dan reproducible benchmarking | Protokol eksperimen umum |
| 13 | Research question, hipotesis, dan novelty | Concept note |
| **14** | **Metodologi dan rancangan eksperimen** | **Desain eksperimen + hasil awal** |
| 15 | Seminar proposal awal dan evaluasi state-of-the-art | Proposal yang diuji |
| 16 | Konsolidasi proposal dan rencana publikasi | Roadmap dan manuskrip |

Tautan langsung:
- ke **Pertemuan 13**: concept note menjadi titik masuk utama.
- ke **Pertemuan 15**: hasil pertemuan ini akan dipresentasikan sebagai bukti kelayakan.

---

# Slide 02 - Recap Pertemuan 13 dan Kaitannya dengan Pertemuan 14

Pada Pertemuan 13, Anda telah menyusun:

- **Problem statement** yang mengidentifikasi kesenjangan.
- **Research question** yang spesifik dan dapat diuji.
- **Hipotesis** yang memprediksi hubungan variabel atau keunggulan metode.
- **Novelty** dan kontribusi yang dijanjikan.

Pertemuan 14 berfokus pada pertanyaan:

> Jika RQ dan hipotesis sudah jelas, bagaimana cara **membuktikannya** secara ilmiah?

Alur:

```
Concept note (P13)
       |
       v
Metodologi dan Rancangan Eksperimen (P14)
       |
       v
Proposal yang siap diseminarkan (P15)
```

---

# Slide 03 - Tujuan Pembelajaran dan Target Keluaran

**Tujuan pembelajaran**
- Menyusun metodologi yang konsisten dengan research question.
- Menghubungkan setiap elemen rancangan eksperimen dengan klaim.
- Menyiapkan bukti awal bahwa pendekatan layak dijalankan.

**Target keluaran pertemuan**
- Rancangan metodologi disertasi yang lengkap.
- Experimental matrix yang dapat diuji.
- Tabel hasil awal eksperimen pendahuluan.
- Log konfigurasi yang mendukung reproducibility.

**Keterkaitan CPMK**
- CPMK-3: merancang eksperimen yang valid dan reproducible.
- CPMK-4: memanfaatkan framework modern untuk eksperimen awal.
- CPMK-6: menyusun proposal awal disertasi.
- CPMK-7: mengkomunikasikan hasil kajian dan eksperimen.

---

# Slide 04 - Agenda dan Aktivitas Pertemuan 14

**Materi**
- Model konseptual dan desain komparatif.
- Experimental matrix dan komponennya.
- Validasi internal dan eksternal.
- Error analysis, limitation, ethical consideration.
- Risiko penelitian dan computational planning.

**Aktivitas**
- Workshop proposal dalam kelompok kecil.
- Konsultasi metodologi bersama dosen.
- Simulasi review: menelaah experimental matrix rekan.

**Praktikum**
- Menjalankan eksperimen pendahuluan menggunakan PyTorch.
- Menyusun tabel hasil awal dan log konfigurasi.

---

# Slide 05 - Mengapa Metodologi Menjadi Jantung Proposal Disertasi

- Ide bagus tanpa metodologi yang jelas hanyalah pernyataan aspiratif.
- Reviewer mencari **bukti yang dapat direplikasi** dan **interpretasi yang tidak berlebihan**.
- Metodologi menentukan:
  - data dan preprocessing yang digunakan,
  - bagaimana model dibangun dan dibandingkan,
  - bagaimana klaim ditarik dari hasil.
- Disertasi S3 menuntut kemampuan merancang eksperimen, bukan sekadar menjalankan kode.

Diagram sederhana:

```
RQ / Hipotesis
     |
     v
Metodologi & Desain Eksperimen
     |        |
     v        v
  Dataset   Metode & Baseline
     |        |
     +---v----+
     Analisis & Klaim
```

---

# Slide 06 - Alur Berpikir: Dari Research Question ke Bukti

Berpikir mundur dari klaim:

1. Tulis klaim yang ingin dipertahankan.
   - Contoh: "Metode X memperbaiki akurasi segmentasi pada data medis."
2. Tentukan hipotesis yang lebih spesifik.
   - Contoh: "U-Net berbasis DINOv2 unggul dibandingkan U-Net berbasis ResNet50."
3. Tentukan bukti yang diterima.
   - Contoh: perbedaan IoU yang signifikan pada 5 seed.
4. Rancang eksperimen untuk menghasilkan bukti tersebut.
5. Identifikasi ancaman yang dapat membatalkan bukti.

Bentuk umum:

```
RQ -> Hipotesis -> Klaim -> Eksperimen -> Bukti -> Interpretasi
```

Setiap anak panah harus dapat dijelaskan secara eksplisit dalam proposal.

---

# Slide 07 - Pertanyaan Kunci Pertemuan 14

Tiga pertanyaan yang harus mampu Anda jawab untuk proposal:

1. **Eksperimen minimum apa yang dapat menguji hipotesis?**
   - Tidak semua eksperimen perlu dilakukan sebelum seminar.
2. **Bagaimana membuktikan novelty?**
   - Novelty harus tampak dari desain dan hasil, bukan hanya klaim.
3. **Apa yang dilakukan apabila hasil utama tidak tercapai?**
   - Rencana mitigasi harus sudah disiapkan sejak awal.

Ketiga pertanyaan ini akan dibahas kembali pada slide 31-33.

---

# Slide 08 - Komponen Metodologi Disertasi

| Kelompok | Komponen |
|---|---|
| Landasan | Model konseptual, RQ, hipotesis, klaim |
| Data | Dataset, preprocessing, split, augmentasi |
| Metode | Model yang diusulkan, baseline, pelatihan |
| Evaluasi | Metrik, eksperimen, ablation, analisis statistik |
| Kualitas | Validasi internal/eksternal, error analysis, limitation |
| Rencana | Risiko, mitigasi, computational planning, etika |

Setiap komponen tidak berdiri sendiri; seluruhnya harus dapat ditelusuri kembali ke RQ dan klaim.

---

# Slide 09 - Model Konseptual Penelitian

- Model konseptual menjelaskan **variabel, konstruk, dan hubungan** yang menjadi asumsi dasar penelitian.
- Dalam Pengolahan Citra Digital, model konseptual dapat berupa:
  - persamaan degradasi dan rekonstruksi,
  - alur representasi citra dari piksel ke fitur semantik,
  - kerangka kerja optimasi atau pembelajaran mesin.

Contoh untuk image restoration:

```
x  ------>  H(y)  ------>  y  ------>  F  ------>  x_hat
asli       degradasi      observasi     model      rekonstruksi
```

Fungsi model konseptual:
- membuat keputusan eksperimen konsisten,
- membantu menemukan komponen yang benar-benar diuji,
- menghindari klaim yang melampaui asumsi model.

---

# Slide 10 - Hubungan RQ, Hipotesis, Klaim, dan Bukti

Setiap klaim kontribusi harus dapat dipetakan ke satu atau lebih bukti eksperimen.

| RQ | Hipotesis | Eksperimen | Metrik | Klaim yang didukung |
|---|---|---|---|---|
| RQ1 | H1 | E1, E2 | IoU, Dice | Kontribusi C1 |
| RQ2 | H2 | E3 | PSNR, SSIM | Kontribusi C2 |
| RQ1 | H1 | E4 (ablation) | IoU | Peran komponen baru |

Jika sebuah klaim tidak memiliki eksperimen yang jelas, klaim tersebut belum siap.
Jika sebuah eksperimen tidak terkait dengan klaim apa pun, pindahkan ke bagian pendukung atau buang.

---

# Slide 11 - Desain Komparatif dan Kontrol

Rancangan eksperimen disertasi umumnya bersifat **komparatif**:

- metode yang diusulkan vs baseline,
- metode yang diusulkan vs state-of-the-art,
- variasi metode untuk menguji kontribusi.

Prinsip kontrol:

- gunakan dataset dan split yang sama,
- gunakan preprocessing yang identik,
- gunakan metrik dan protokol evaluasi yang sama,
- kendalikan random seed dan hyperparameter.

Jangan membandingkan angka dari paper dengan angka hasil Anda sendiri tanpa replikasi yang adil.

---

# Slide 12 - Experimental Matrix: Definisi dan Isi

**Experimental matrix** adalah tabel yang memetakan seluruh kombinasi eksperimen yang direncanakan.

Isi umum:

| Aspek | Deskripsi |
|---|---|
| Dataset | nama, jumlah data, split |
| Kondisi eksperimen | hyperparameter, seed, augmentasi, resolusi |
| Model dan baseline | identitas model, pretrained weights, jumlah parameter |
| Tujuan | hipotesis atau klaim yang diuji |
| Metrik | metrik primer dan sekunder |
| Kriteria keberhasilan | target atau expected outcome |

Matrix membantu:
- menghindari eksperimen yang berlebihan,
- menemukan celah pada desain,
- memudahkan komunikasi dengan reviewer.

---

# Slide 13 - Contoh Experimental Matrix

Contoh penelitian: *DINOv2-based representation untuk semantic segmentation pada data medis*.

| ID | Eksperimen | Dataset | Variabel | Baseline | Metrik |
|---|---|---|---|---|---|
| E1 | Fitting baseline | Data-Mini | tanpa modifikasi | U-Net + ResNet50 | IoU, Dice |
| E2 | Fine-tune DINOv2 | Data-Mini | pretrained vs scratch | U-Net + DINOv2 | IoU, Dice |
| E3 | Ablasi layer | Data-Mini | pemilihan layer 1, 3, 6 | U-Net + DINOv2 | IoU, Dice |
| E4 | Uji transfer | Data-Out | domain shift | U-Net + DINOv2 | IoU, Dice |

Setiap baris memiliki tujuan eksperimen yang eksplisit. Matrix ini juga akan digunakan sebagai alat komunikasi pada seminar proposal.

---

# Slide 14 - Dataset dan Preprocessing

- Pilih dataset yang sesuai dengan RQ, bukan sekadar mudah diakses.
- Nyatakan:
  - lisensi dan provenance data,
  - ukuran dataset dan jumlah kelas,
  - kondisi akuisisi dan keterbatasan data.
- Preprocessing harus dilaporkan dan diterapkan identik untuk semua metode.
- Hindari data leakage:
  - lakukan split sebelum normalisasi yang menggunakan statistik global,
  - jangan menggunakan data uji untuk augmentasi atau tuning hyperparameter.
- Detail leakage dan split sudah dibahas pada Pertemuan 12; terapkan pada proposal.

---

# Slide 15 - Baseline dan Model yang Diusulkan

**Baseline**
- Minimal dua jenis baseline:
  - baseline kuat: metode state-of-the-art yang sudah dipublikasikan,
  - baseline sederhana: model linear, nearest neighbor, atau U-Net sederhana.
- Baseline sederhana berfungsi memastikan dataset dapat dipelajari.

**Model yang diusulkan**
- Jelaskan arsitektur lengkap.
- Nyatakan komponen baru yang menjadi kontribusi.
- Cantumkan pretrained weights dan sumbernya.

**Pelaporan**
- jumlah parameter,
- inference time,
- FLOPs jika relevan.

---

# Slide 16 - Metrik Evaluasi Primer dan Sekunder

- Pilih **metrik primer** yang paling sesuai dengan tujuan penelitian.
- Gunakan **metrik sekunder** untuk menangkap aspek yang tidak diukur metrik primer.

| Tugas | Contoh metrik |
|---|---|
| Klasifikasi | Accuracy, balanced accuracy, F1, calibration error |
| Segmentasi | IoU, Dice, boundary IoU |
| Deteksi | mAP, precision-recall, AP per kelas |
| Restoration | PSNR, SSIM, LPIPS, evaluasi perseptual |

- Tentukan kondisi penghitungan secara eksplisit.
- Sertakan analisis visual sebagai bukti pelengkap, bukan pengganti metrik.

---

# Slide 17 - Ablation Study

Ablation study digunakan untuk **mengisolasi kontribusi komponen** terhadap kinerja akhir.

Prinsip:

- mulai dari metode lengkap,
- lepaskan satu komponen pada satu waktu,
- jaga komponen lain tetap konstan,
- catat pengaruhnya terhadap metrik.

Contoh:

| Variasi | Komponen A | Komponen B | Komponen C | IoU |
|---|---|---|---|---|
| Full | ✓ | ✓ | ✓ | 78.2 |
| - A | ✗ | ✓ | ✓ | 75.1 |
| - B | ✓ | ✗ | ✓ | 76.4 |
| - C | ✓ | ✓ | ✗ | 77.0 |

Interpretasi: jika metrik turun saat komponen dihapus, komponen memberikan kontribusi nyata.

---

# Slide 18 - Validasi Internal dan Eksternal

**Validasi internal**
- Mengukur apakah perbedaan hasil disebabkan oleh perlakuan, bukan konflik atau bias prosedur.
- Contoh: menggunakan protokol yang sama untuk semua metode, seed yang sama, evaluasi yang sama.

**Validasi eksternal**
- Mengukur apakah hasil berlaku pada data, domain, atau kondisi lain.
- Contoh: menguji pada dataset tambahan yang tidak digunakan saat pengembangan.

Disertasi yang baik memisahkan kedua jenis validasi.
Jika hanya menggunakan satu dataset, nyatakan keterbatasan generalisasi secara eksplisit.

---

# Slide 19 - Analisis Statistik dan Confidence Interval

- Hasil eksperimen tidak cukup disajikan sebagai satu angka.
- Gunakan:
  - beberapa seed,
  - mean dan standar deviasi,
  - confidence interval,
  - uji signifikansi bila asumsi terpenuhi.

Contoh dengan SciPy:

```python
import numpy as np
from scipy import stats

hasil_a = [78.1, 78.5, 78.3, 78.0, 78.4]
hasil_b = [76.2, 76.8, 76.5, 76.0, 76.6]

t_stat, p_value = stats.ttest_ind(hasil_a, hasil_b)
print(f"p = {p_value:.4f}")

print("CI A:", stats.t.interval(0.95, len(hasil_a)-1,
      np.mean(hasil_a), stats.sem(hasil_a)))
```

Statistical testing telah dibahas pada Pertemuan 12; di sini hasilnya diintegrasikan ke dalam argumen proposal.

---

# Slide 20 - Error Analysis

Error analysis membantu menemukan keterbatasan metodologi secara sistematis.

Langkah:

- kumpulkan sampel prediksi yang salah,
- kategorikan pola kesalahan,
- hitung frekuensi tiap kategori,
- hubungkan dengan konteks data.

Contoh tabel:

| Failure Mode | Konteks | Frekuensi | Dampak |
|---|---|---|---|
| False positive pada latar | objek kecil | 32% | menurunkan precision |
| False negative kelas langka | data tidak seimbang | 25% | menurunkan recall |

Error analysis juga menjadi bahan untuk iterasi perbaikan dan argumen kontribusi.

---

# Slide 21 - Limitation dan Threats to Validity

**Limitation** harus dinyatakan secara jujur:

- ukuran dataset,
- cakupan domain,
- biaya komputasi,
- asumsi model,
- ketidakpastian evaluasi.

**Threats to validity** dapat dirumuskan sebagai pertanyaan:

- apakah hasil hanya berlaku pada dataset ini?
- apakah baseline dievaluasi secara adil?
- apakah metrik benar-benar mengukur klaim?
- apakah ada faktor lain yang tidak dikontrol?

Kejujuran terhadap keterbatasan justru meningkatkan kredibilitas proposal.

---

# Slide 22 - Risiko Penelitian dan Rencana Mitigasi

Risiko harus dinilai sejak awal, bukan setelah eksperimen gagal.

| Risiko | Dampak | Probabilitas | Mitigasi | Rencana cadangan |
|---|---|---|---|---|
| Metode tidak mengungguli baseline | Tinggi | Sedang | Evaluasi awal cepat | Ubah fokus pada aspek lain |
| Dataset tidak tersedia | Tinggi | Rendah | Hubungi pemilik data | Gunakan dataset surrogate |
| Komputasi tidak mencukupi | Sedang | Sedang | Estimasi budget awal | Perkecil resolusi atau durasi |
| Hasil tidak signifikan | Sedang | Sedang | Hitung effect size | Laporkan sebagai temuan |

Mitigasi harus terintegrasi ke dalam timeline disertasi.

---

# Slide 23 - Computational Planning

Perkirakan kebutuhan komputasi seluruh eksperimen.

Komponen perhitungan:

- jumlah model yang akan dilatih,
- ukuran dataset dan resolusi input,
- jumlah epoch dan durasi per run,
- GPU yang tersedia.

Contoh estimasi:

| Eksperimen | Durasi/run | Runs | Total GPU jam |
|---|---|---|---|
| Baseline | 4 jam | 5 seeds | 20 |
| Metode utama | 6 jam | 5 seeds | 30 |
| Ablation | 6 jam | 5 seeds | 30 |
| **Total** | | | **80** |

Jika total melebihi kapasitas, rancang ulang eksperimen atau kurangi jumlah seed, bukan menghilangkan komponen penting.

---

# Slide 24 - Ethical Consideration

Pertimbangan etika berlaku untuk data dan model.

**Data**
- izin penggunaan dataset,
- anonimisasi data sensitif,
- bias dan representasi kelompok.

**Model**
- dampak kesalahan prediksi di dunia nyata,
- kemungkinan penyalahgunaan,
- transparansi dan akuntabilitas.

Dokumentasikan provenance setiap dataset dan model.
Jika menggunakan data sekunder, nyatakan lisensi dan pembatasan penggunaannya.

---

# Slide 25 - Praktikum: Eksperimen Pendahuluan dengan PyTorch

Tujuan praktikum:

- membuktikan bahwa pipeline dasar berjalan,
- memperoleh estimasi awal kinerja,
- menemukan hambatan teknis lebih dini.

Lingkup cukup kecil:

- satu dataset mini,
- satu baseline sederhana,
- satu prototipe metode yang diusulkan.

Contoh kerangka kode:

```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader

train_loader = DataLoader(train_dataset, batch_size=16, shuffle=True)

model = nn.Sequential(
    nn.Conv2d(3, 32, 3, padding=1), nn.ReLU(),
    nn.AdaptiveAvgPool2d(1), nn.Flatten(),
    nn.Linear(32, num_classes)
).to(device)

optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()
```

Eksperimen pendahuluan tidak harus sempurna, tetapi harus terdokumentasi.

---

# Slide 26 - Menyusun Tabel Hasil Awal

Tabel hasil awal harus memuat:

- identifikasi eksperimen,
- metode yang dibandingkan,
- dataset dan split,
- konfigurasi utama,
- metrik dalam bentuk mean ± std,
- status eksperimen.

Gunakan format yang sama untuk semua eksperimen agar mudah dibandingkan.

Simpan tabel dalam Markdown atau CSV agar dapat dipindahkan langsung ke proposal.

Jangan lupa menyimpan log penyerta: command, seed, versi library, environment.

---

# Slide 27 - Contoh Tabel Hasil Awal dan Log Konfigurasi

| ID | Metode | Dataset | Seed | Akurasi | IoU |
|---|---|---|---|---|---|
| E1 | Baseline U-Net | Data-Mini | 42 | 84.1 ± 0.4 | 61.2 |
| E2 | U-Net + DINOv2 | Data-Mini | 42 | 86.3 ± 0.5 | 64.7 |
| E3 | U-Net + DINOv2 (no aug) | Data-Mini | 42 | 85.0 ± 0.3 | 63.1 |

Contoh log konfigurasi:

```yaml
eksperimen: E2
model: unet
backbone: dinov2_vitb14
pretrain: facebook/dinov2-base
dataset: datamini
resolusi: 224
augmentasi: albumentations_v2
lr: 0.0003
batch_size: 16
seed: 42
gpu: 1xV100
waktu_total: 04:12:30
```

Log ini menjadi bahan audit reproducibility.

---

# Slide 28 - Reproducibility dan Random Seed

Pertemuan 12 telah menekankan reproducible benchmarking. Pada tahap ini, praktik tersebut diterapkan pada eksperimen disertasi.

Contoh pengaturan seed di PyTorch:

```python
import random
import numpy as np
import torch

def set_seed(seed: int = 42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False

set_seed(42)
```

Catat versi Python, PyTorch, CUDA, dan library.
Simpan model checkpoint dan log setiap run.

---

# Slide 29 - Simulasi Review: Perspektif Reviewer

Aktivitas: menelaah rancangan proposal rekan seolah-olah menjadi reviewer.

Pertanyaan yang diajukan reviewer:

- Apakah RQ dapat dijawab oleh eksperimen ini?
- Apakah baseline cukup kuat dan fair?
- Apakah metrik sesuai dengan klaim?
- Apakah ada variabel yang tidak dikontrol?
- Apakah kesimpulan yang direncanakan melebihi bukti yang ada?

Simulasi review membantu menemukan kelemahan sebelum seminar sesungguhnya.

---

# Slide 30 - Checklist Konsistensi Metodologi

Gunakan checklist berikut untuk memeriksa proposal:

- [ ] Metodologi eksplisit terkait dengan RQ.
- [ ] Setiap hipotesis memiliki eksperimen.
- [ ] Setiap klaim memiliki bukti yang direncanakan.
- [ ] Baseline adil dan tercantum.
- [ ] Metrik primer dan sekunder didefinisikan.
- [ ] Ablation study direncanakan.
- [ ] Validasi eksternal dipertimbangkan.
- [ ] Analisis statistik direncanakan.
- [ ] Risiko dan mitigasi terdokumentasi.
- [ ] Kebutuhan komputasi diestimasi.
- [ ] Etika dan lisensi data dicantumkan.

---

# Slide 31 - Menjawab Pertanyaan Kunci: Eksperimen Minimum

Eksperimen minimum adalah eksperimen paling sedikit yang cukup untuk:

- menguji hipotesis,
- menolak penjelasan alternatif,
- mendukung klaim awal pada proposal.

Langkah:

1. Tentukan klaim minimum yang akan dipertahankan.
2. Pilih dua sampai tiga eksperimen yang sangat diperlukan.
3. Tambahkan satu eksperimen kontrol atau kalibrasi.
4. Tunda eksperimen sekunder hingga fase setelah seminar.

Hindari *experiment sprawl*: banyak eksperimen tanpa prioritas yang jelas.

---

# Slide 32 - Menjawab Pertanyaan Kunci: Membuktikan Novelty

Novelty tidak dibuktikan dengan pernyataan, tetapi dengan bukti perbedaan terhadap state-of-the-art.

Mekanisme bukti:

- tabel perbandingan desain atau metode dengan paper terkait,
- eksperimen yang membandingkan metode usulan dengan metode terdekat,
- ablation yang menunjukkan komponen baru bekerja,
- analisis yang memperlihatkan keterbatasan metode sebelumnya.

Simulasi review akan menguji apakah novelty benar-benar tampak dari eksperimen, bukan hanya dari kata-kata.

---

# Slide 33 - Menjawab Pertanyaan Kunci: Jika Hasil Utama Tidak Tercapai

Rencana cadangan harus sudah disiapkan sejak awal.

Kemungkinan dan respons:

- hasil tidak unggul signifikan
  - periksa error analysis, ubah pengaturan, atau perbaiki hipotesis.
- hasil unggul pada satu dataset tetapi tidak pada dataset lain
  - laporkan sebagai temuan yang menarik, jangan disembunyikan.
- eksperimen tidak berjalan
  - minimal laporkan pipeline yang valid dan penyebab kegagalan.
- hasil negatif
  - tetap berharga jika analisisnya jujur dan metodologi jelas.

Rencana kontingensi ditulis di bagian risk and mitigation proposal.

---

# Slide 34 - Workflow Penyempurnaan Experimental Matrix

Alur yang disarankan:

1. Mulai dari RQ dan hipotesis.
2. Daftar semua eksperimen yang mungkin.
3. Kelompokkan berdasarkan prioritas: core, supporting, exploratory.
4. Pilih eksperimen minimum untuk proposal awal.
5. Buat experimental matrix dan hubungkan dengan klaim.
6. Simulasikan review untuk menemukan celah.
7. Jalankan eksperimen pendahuluan.
8. Perbarui matrix berdasarkan hasil dan kendala.

Diagram:

```
RQ -> Hipotesis -> Daftar Eksperimen -> Prioritas -> Matrix -> Review -> Jalankan
```

---

# Slide 35 - Template Pipeline Eksperimen

Pseudocode yang dapat diadaptasi:

```python
## train_evaluate.py
1. set_seed(seed)
2. load_config("config.yaml")
3. dataset = load_dataset(config)
4. split = make_split(dataset, config)
5. preprocess = build_preprocess(config)
6. model = build_model(config)
7. for epoch in range(config.epochs):
       train_one_epoch(model, train_loader)
       validate(model, val_loader)
8. metrics = evaluate(model, test_loader, config.metrics)
9. log_metrics(metrics)
10. save_checkpoint(model, config, metrics)
```

Poin penting: semua komponen, mulai dari split, preprocessing, model, hingga evaluasi, harus diterapkan secara identik untuk metode usulan dan baseline.

---

# Slide 36 - Menghubungkan ke Pertemuan 15 dan 16

**Pertemuan 15: Seminar Proposal Awal**
- Sajikan RQ, novelty, metodologi, dan hasil awal.
- Gunakan experimental matrix sebagai lampiran.
- Siapkan menjawab pertanyaan reviewer secara defensible.

**Pertemuan 16: Konsolidasi Proposal**
- Sintesis masukan dari seminar.
- Perbaiki metodologi dan finalisasi roadmap.
- Hasil awal dari pertemuan 14 menjadi bukti kelayakan.

Pertemuan 14 adalah fondasi bukti untuk dua pertemuan berikutnya.

---

# Slide 37 - Rangkuman dan Pesan Kunci

- Metodologi disertasi adalah jembatan antara ide dan bukti.
- Rancangan eksperimen harus eksplisit, adil, dan dapat direproduksi.
- Tiga pertanyaan kunci harus dapat dijawab:
  - eksperimen minimum,
  - bukti novelty,
  - rencana jika hasil utama tidak tercapai.
- Produk pertemuan ini:
  - rancangan metodologi,
  - tabel hasil awal,
  - log konfigurasi.

Lanjutkan ke seminar proposal dengan bukti yang jujur dan siap dikritik.

---

TERIMA KASIH

Pertemuan berikutnya

**Seminar Proposal Awal dan Evaluasi State-of-the-Art**