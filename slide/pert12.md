# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 12
# Experimental Design dan Reproducible Benchmarking

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 12 dalam RPS

| Posisi | Pertemuan | Topik |
|---|---|---|
| Sebelumnya | 11 | Explainable, Robust, dan Trustworthy Computer Vision |
| **Saat ini** | **12** | **Experimental Design dan Reproducible Benchmarking** |
| Berikutnya | 13 | Formulasi Research Question, Hipotesis, dan Novelty |

- Pertemuan 11 menekankan **apa yang harus diuji** (interpretabilitas, robustness, uncertainty).
- Pertemuan 12 menekankan **bagaimana menguji secara valid dan reproducible**.
- Pertemuan 13 akan memanfaatkan protokol eksperimen sebagai dasar perumusan hipotesis dan novelty.

---

# Slide 02 - Tujuan Pembelajaran dan Target Keluaran

**Tujuan Pembelajaran**

- Merancang eksperimen yang dapat membedakan kontribusi metode dari pengaruh data, implementasi, atau konfigurasi.
- Membangun baseline, ablation, kontrol, analisis statistik, dan pelaporan yang memungkinkan hasil diverifikasi.
- Mengevaluasi reproducibility paper secara kritis.

**Target Keluaran**

- Protokol eksperimen lengkap yang siap digunakan untuk proposal awal.
- Pipeline eksperimen reproducible dengan konfigurasi dan random seed.
- Kemampuan melakukan peer review desain eksperimen.

**Keterkaitan CPMK**

- CPMK-2: evaluasi paper berdasarkan validitas eksperimen.
- CPMK-3: merancang eksperimen komputasional yang valid dan reproducible.
- CPMK-6: menyusun proposal awal dengan experimental design.

---

# Slide 03 - Agenda Pertemuan

- Workshop desain eksperimen.
- Peer review protokol eksperimen.
- Evaluasi reproducibility beberapa paper.
- Praktikum: menyusun pipeline eksperimen reproducible dengan konfigurasi dan random seed.

**Alur Kegiatan**

1. Konsep experimental design dan reproducible benchmarking.
2. Praktik pipeline Python di Jupyter/Colab.
3. Diskusi studi kasus dan peer review.
4. Penyusunan protokol eksperimen awal.

---

# Slide 04 - Recap Singkat Pertemuan 11

Pertemuan sebelumnya membahas:

- **Saliency map** dan gradient attribution.
- **Calibration** dan predictive uncertainty.
- **Robustness** terhadap adversarial example dan distribution shift.
- **Fairness** dan model card.

**Jembatan ke Pertemuan 12**

- Eksperimen di pertemuan 11 menghasilkan attribution map dan calibration plot.
- Bagaimana memastikan hasil tersebut **tidak dipengaruhi kebetulan seed, leakage, atau konfigurasi tidak adil**?
- Oleh karena itu, diperlukan disiplin experimental design dan reproducible benchmarking.

---

# Slide 05 - Jembatan Menuju Pertemuan 13

- Pertemuan 13 akan membahas formulasi **Research Question, Hipotesis, dan Novelty**.
- Eksperimen yang dirancang pada pertemuan ini menjadi **bukti untuk menguji hipotesis**.
- Protokol eksperimen membantu menjawab:
  - Apakah klaim kontribusi didukung data?
  - Apakah novelty benar-benar berbeda dari baseline?
  - Apakah hasil dapat direplikasi peneliti lain?

---

# Slide 06 - Mengapa Experimental Design Krusial di Riset S3

**Masalah umum dalam riset deep learning**

- Peningkatan akurasi hanya karena **perbedaan seed**, bukan metode.
- Baseline dibuat **lemah** agar metode usulan terlihat unggul.
- **Hyperparameter** metode usulan dituning, baseline tidak.
- **Leakage** data menyebabkan hasil tidak valid.
- Eksperimen tidak dapat direplikasi karena konfigurasi tidak dilaporkan.

**Prinsip Dasar**

> Eksperimen adalah argumen ilmiah. Setiap keputusan desain harus memperkuat klaim bahwa perbedaan hasil disebabkan oleh kontribusi metode, bukan faktor lain.

---

# Slide 07 - Reproducible Benchmarking: Definisi dan Ruang Lingkup

| Istilah | Makna |
|---|---|
| **Reproducibility** | Kode, data, dan konfigurasi yang sama menghasilkan hasil yang sama. |
| **Replicability** | Implementasi ulang berdasarkan deskripsi menghasilkan kesimpulan yang sama. |
| **Benchmarking** | Protokol evaluasi standar untuk perbandingan metode secara adil. |

**Ruang Lingkup**

- Dataset dan split.
- Preprocessing pipeline.
- Model dan pelatihan.
- Metrik dan statistik.
- Lingkungan komputasi.
- Artefak dan log.

---

# Slide 08 - Komponen Utama Eksperimen Komputasional

```
Dataset → Split → Preprocessing → Training → Evaluation → Reporting
                              ↑
                    Konfigurasi & Seed
```

**Komponen yang Harus Dikontrol**

1. Data: sumber, lisensi, distribusi kelas, duplikasi.
2. Split: train/validation/test, stratifikasi.
3. Preprocessing: normalisasi, augmentasi, resizing.
4. Model: arsitektur, inisialisasi, pretrained weights.
5. Training: optimizer, learning rate, batch size, epoch.
6. Evaluasi: metrik, threshold, protokol inference.
7. Pelaporan: tabel, interval kepercayaan, analisis statistik.

---

# Slide 09 - Dataset Split: Train/Validation/Test

| Split | Fungsi | Kapan Digunakan |
|---|---|---|
| **Train** | Memperbarui parameter model | Setiap iterasi pelatihan |
| **Validation** | Memilih hyperparameter, early stopping, model selection | Berkala selama training |
| **Test** | Evaluasi akhir performa model | Sekali saja, setelah semua keputusan selesai |

**Aturan Praktis**

- Jangan menyentuh test set berulang kali.
- Jika test digunakan untuk iterasi, hasil menjadi **overfit terhadap test set**.
- Pisahkan test set secara ketat sejak awal.

---

# Slide 10 - K-Fold Cross-Validation dan Stratifikasi

**K-Fold Cross-Validation**

- Data dibagi menjadi K lipatan.
- Model dilatih pada K-1 lipatan, dievaluasi pada 1 lipatan.
- Diulang K kali, hasil dirata-rata.

**Stratifikasi**

- Memastikan proporsi kelas sama di setiap lipatan.
- Penting untuk dataset tidak seimbang.

**Grouped Split**

- Data dari subjek/video yang sama tidak boleh tersebar di train dan test.
- Gunakan `GroupKFold` atau `LeaveOneGroupOut`.

---

# Slide 11 - Data Leakage: Definisi dan Jenis

**Definisi**

- Informasi dari luar training set bocor ke proses pelatihan atau pemilihan model.
- Menyebabkan hasil evaluasi **optimistis secara palsu**.

**Jenis Leakage**

| Jenis | Contoh |
|---|---|
| Target leakage | Fitur yang mengandung informasi label |
| Train-test contamination | Statistik dihitung dari seluruh data |
| Duplicate images | Gambar sama atau hampir sama di train dan test |
| Temporal leakage | Data masa depan dipakai untuk memprediksi masa lalu |
| Preprocessing leakage | Normalisasi/PCA fit sebelum split |

---

# Slide 12 - Sumber Leakage pada Pipeline PCD

- **Normalisasi global**: mean/std dihitung dari seluruh dataset sebelum split.
- **Augmentasi tidak hati-hati**: transformasi membuat sample test mirip sample train.
- **Fine-tuning dengan pretrained model**: dataset pretraining mungkin mengandung gambar yang mirip dengan test set.
- **Feature selection sebelum split**: fitur dipilih berdasarkan seluruh data.
- **Masking/interpolasi** menggunakan statistik global dari citra.
- **Duplicate atau near-duplicate** pada dataset publik seperti CIFAR-10, ImageNet, atau medical imaging.

---

# Slide 13 - Menghindari Leakage: Praktik Aman

**Urutan Pipeline yang Benar**

```
Split data terlebih dahulu
→ Fit preprocessing hanya pada training fold
→ Terapkan transformasi ke validation/test secara terpisah
→ Baru lakukan training dan evaluasi
```

**Checklist**

- [ ] Cek duplikat dan near-duplicate antar split.
- [ ] Gunakan GroupKFold untuk data berkelompok.
- [ ] Simpan statistik preprocessing per fold.
- [ ] Dokumentasikan versi dataset dan filter yang digunakan.
- [ ] Jangan melihat test set selama pengembangan.

---

# Slide 14 - Baseline Selection: Prinsip Baseline Terkuat yang Wajar

**Kriteria Baseline yang Baik**

- Metode state-of-the-art yang relevan dengan tugas.
- Kode tersedia atau dapat diimplementasi ulang dengan benar.
- Hyperparameter dituning secara adil.
- Bukan *strawman*: versi yang sengaja dibuat lemah.

**Pertanyaan Kunci**

- Apa metode terbaik yang sudah ada untuk masalah ini?
- Apakah baseline dijalankan dengan konfigurasi yang sama adilnya dengan metode usulan?
- Apakah perbedaan hasil signifikan secara statistik?

---

# Slide 15 - Menyusun Baseline untuk Tugas PCD

| Tugas | Contoh Baseline |
|---|---|
| Klasifikasi citra | ResNet, EfficientNet, ViT |
| Object detection | YOLO, DETR, Faster R-CNN |
| Segmentasi | U-Net, DeepLab, SAM |
| Image restoration | BM3D, SwinIR, diffusion restoration |
| Self-supervised representation | DINO, DINOv2, MAE |
| Multimodal | CLIP, BLIP |

**Catatan Penting**

- Gunakan pretrained weights yang sah dan laporkan sumbernya.
- Jika baseline tidak tersedia, implementasi ulang harus divalidasi dengan reproduksi hasil paper.
- Laporkan jumlah parameter dan FLOPs untuk fairness komputasi.

---

# Slide 16 - Ablation Study: Konsep dan Peran

**Definisi**

- Eksperimen yang menghilangkan atau mengganti komponen metode untuk mengukur kontribusinya terhadap performa.

**Jenis Ablation**

| Jenis | Deskripsi |
|---|---|
| **Subtractive** | Hapus komponen dari model penuh |
| **Additive** | Tambahkan komponen ke model dasar |
| **Replacement** | Ganti satu komponen dengan alternatif |

**Contoh**

- Model penuh = Encoder + Attention Module + Loss B.
- Ablation 1: Encoder + Loss B (tanpa Attention).
- Ablation 2: Encoder + Attention + Loss A.

---

# Slide 17 - Desain Ablation yang Valid

**Prinsip**

- Hanya **satu faktor** yang berubah antar kondisi.
- Semua konfigurasi lain dikontrol: seed, optimizer, learning rate, jumlah epoch.
- Ablation harus menguji **klaim kontribusi** yang dinyatakan di paper.

**Hindari**

- Mengubah banyak komponen sekaligus.
- Menyetel hyperparameter berbeda untuk tiap varian.
- Menyimpulkan kontribusi dari satu seed saja.
- Melakukan banyak perbandingan tanpa koreksi statistik.

---

# Slide 18 - Hyperparameter dan Sensitivitas Konfigurasi

**Masalah Fairness**

- Metode usulan dituning 100 percobaan, baseline hanya 1 percobaan.
- Hasil bisa bias terhadap metode usulan.

**Praktik yang Disarankan**

- Gunakan budget tuning yang sama untuk semua metode.
- Laporkan rentang hyperparameter yang dicoba.
- Gunakan random search atau Bayesian optimization.
- Ukur sensitivitas metode terhadap perubahan hyperparameter.
- Laporkan hyperparameter terbaik beserta nilai validation.

---

# Slide 19 - Random Seed dan Variabilitas Training

**Sumber Variabilitas**

- Inisialisasi bobot.
- Urutan batch dan shuffling data.
- Dropout dan augmentasi acak.
- Non-determinisme GPU.

**Mengapa Satu Run Tidak Cukup**

- Perbedaan seed dapat mengubah urutan peringkat metode.
- Hasil tunggal tidak memberikan informasi ketidakpastian.
- Evaluasi yang adil membutuhkan beberapa seed.

**Kewajiban Pelaporan**

- Laporkan mean ± std dari beberapa seed.
- Simpan seed yang digunakan untuk setiap run.

---

# Slide 20 - Berapa Banyak Seed yang Diperlukan?

**Aturan Praktis**

| Situasi | Jumlah Seed |
|---|---|
| Eksperimen awal / debugging | 1 |
| Eksperimen utama | 3–5 |
| Perbedaan hasil tipis / varians tinggi | 5–10 |
| Meta-analisis / klaim kuat | 10+ |

**Pertimbangan**

- Semakin banyak seed, semakin stabil estimasi mean.
- Semakin besar varians, semakin banyak seed dibutuhkan.
- Sesuaikan dengan **computational budget**.

---

# Slide 21 - Confidence Interval: Mengukur Ketidakpastian Estimasi

**Definisi**

- Interval yang diperkirakan memuat nilai parameter populasi dengan tingkat kepercayaan tertentu.
- CI 95% berarti jika eksperimen diulang, interval akan memuat nilai sebenarnya pada 95% percobaan.

**Cara Menghitung**

- Normal approximation: `mean ± 1.96 * (std / sqrt(n))`
- Bootstrap: sampling ulang dengan penggantian, lalu ambil persentil 2.5 dan 97.5.

**Pelaporan**

- Selalu tampilkan error bar atau CI pada tabel dan grafik.
- CI lebih informatif daripada nilai p saja.

---

# Slide 22 - Statistical Testing: Memilih Uji yang Tepat

| Situasi | Uji yang Sesuai |
|---|---|
| Dua model, data terpasang (paired) | Paired t-test / Wilcoxon signed-rank |
| Perbandingan beberapa model | ANOVA / Friedman + post-hoc |
| Tabel klasifikasi 2x2 | McNemar test |
| Proporsi / akurasi | Bootstrap CI / uji binomial |

**Prinsip**

- Uji statistik tidak menggantikan ukuran efek.
- Laporkan effect size dan CI.
- Koreksi untuk multiple comparisons (Bonferroni, Tukey, Holm).

---

# Slide 23 - Interpretasi Hasil Negatif dan Non-Significant

**Hasil Negatif adalah Hasil Valid**

- Metode usulan tidak selalu lebih baik dari baseline.
- Hasil negatif berkontribusi menghindari publikasi bias.
- Sering kali disebabkan oleh:
  - Baseline yang kuat.
  - Daya statistik kurang (jumlah seed sedikit).
  - Bug implementasi.
  - Hipotesis yang salah.

**Langkah jika Hasil Negatif**

1. Periksa pipeline dan leakage.
2. Periksa hiperparameter dan budget.
3. Periksa daya statistik.
4. Jika tetap negatif, laporkan secara jujur dan diskusikan implikasinya.

---

# Slide 24 - Computational Budget dan Reproducibility

**Mengapa Budget Penting**

- Memengaruhi pilihan baseline dan jumlah seed.
- Menentukan apakah eksperimen dapat direplikasi peneliti lain.
- Mempengaruhi klaim efisiensi metode.

**Yang Harus Dilaporkan**

- Tipe GPU/CPU, RAM, dan durasi training.
- Jumlah parameter dan FLOPs.
- Waktu inferensi per sampel.
- Total jam komputasi untuk seluruh eksperimen.
- Tools monitoring energi jika tersedia.

---

# Slide 25 - Protokol Eksperimen: Struktur dan Isi

**Template Protokol**

1. **Tujuan**: research question dan hipotesis yang diuji.
2. **Dataset**: sumber, lisensi, jumlah sampel, split.
3. **Preprocessing**: transformasi, augmentasi.
4. **Model**: arsitektur, inisialisasi, pretrained.
5. **Training**: optimizer, schedule, batch, epoch.
6. **Baseline**: daftar metode pembanding.
7. **Ablation**: daftar varian yang diuji.
8. **Metrik**: metrik utama dan sekunder.
9. **Statistik**: jumlah seed, uji statistik, CI.
10. **Environment**: hardware, software, seed.
11. **Artefak**: kode, log, checkpoint.
12. **Jadwal**: estimasi waktu dan sumber daya.

---

# Slide 26 - Configuration Management dan Versioning

**Praktik**

- Simpan konfigurasi dalam file YAML/JSON.
- Versioning kode dengan Git.
- Catat hash commit untuk setiap eksperimen.
- Gunakan environment management: `requirements.txt`, `environment.yml`, atau Docker.

**Contoh Config YAML**

```yaml
dataset:
  name: cityscapes
  split: [0.7, 0.15, 0.15]
  seed: 42

model:
  architecture: resnet50
  pretrained: true

training:
  optimizer: adamw
  learning_rate: 0.0001
  batch_size: 32
  epochs: 100

evaluation:
  metrics: [accuracy, f1]
  seeds: [42, 123, 7]
```

---

# Slide 27 - Pipeline Eksperimen Reproducible dengan Python

```
Data Loader → Preprocessing → Split → Training → Evaluation
     ↑            ↑            ↑         ↑            ↑
  config.yaml   transform   GroupKFold  seed & log   metrics & CI
```

**Langkah Praktis**

1. Tulis skrip modular: `data.py`, `train.py`, `evaluate.py`.
2. Gunakan argparse atau config untuk parameter.
3. Set seed di awal setiap run.
4. Simpan log ke file terpisah per eksperimen.
5. Commit kode dan config setiap kali menjalankan eksperimen.

---

# Slide 28 - Contoh Kode: Konfigurasi dan Seed

```python
import random, numpy as np, torch

def set_seed(seed):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False

config = {
    "seed": 42,
    "epochs": 100,
    "batch_size": 32,
    "lr": 1e-4,
    "model": "resnet50"
}

def main():
    set_seed(config["seed"])
    # lanjut ke data loading, training, evaluasi
```

**Catatan**

- `deterministic=True` memperlambat training tetapi meningkatkan reproducibility.
- Untuk GPU non-deterministik, laporkan versi CUDA dan PyTorch.

---

# Slide 29 - Contoh Kode: Menyimpan Log dan Metrik

```python
import json, csv, datetime

def save_config(config, path):
    with open(path, "w") as f:
        json.dump(config, f, indent=2)

def append_metric(log_path, metrics, seed):
    with open(log_path, "a", newline="") as f:
        writer = csv.DictWriter(f, fieldnames=metrics.keys())
        if f.tell() == 0:
            writer.writeheader()
        writer.writerow(metrics)

## Contoh penggunaan
metrics = {"seed": 42, "accuracy": 0.913, "f1": 0.901}
save_config(config, "config.json")
append_metric("metrics.csv", metrics, seed=42)
```

**Manfaat**

- Riwayat eksperimen tersimpan.
- Analisis antar seed menjadi mudah.
- Artefak siap dibagikan.

---

# Slide 30 - Contoh Kode: Cross-Validation dan Confidence Interval

```python
import numpy as np
from sklearn.model_selection import GroupKFold
from sklearn.metrics import accuracy_score

def bootstrap_ci(scores, n_bootstrap=1000, ci=95):
    rng = np.random.default_rng(0)
    boot_means = []
    for _ in range(n_bootstrap):
        sample = rng.choice(scores, size=len(scores), replace=True)
        boot_means.append(np.mean(sample))
    lower = np.percentile(boot_means, (100 - ci) / 2)
    upper = np.percentile(boot_means, 100 - (100 - ci) / 2)
    return lower, upper

scores = [0.91, 0.89, 0.92, 0.90, 0.93]
print("Mean:", np.mean(scores))
print("CI 95%:", bootstrap_ci(scores))
```

**Catatan**

- Gunakan GroupKFold untuk data berkelompok.
- Simpan skor per fold, lalu hitung CI dari skor tersebut.

---

# Slide 31 - Checklist Reproducibility Paper

| Aspek | Pertanyaan |
|---|---|
| Kode | Apakah kode tersedia dan berlisensi jelas? |
| Data | Apakah dataset dijelaskan dan dapat diakses? |
| Environment | Apakah versi library dan hardware dilaporkan? |
| Konfigurasi | Apakah hyperparameter dan config tersedia? |
| Seed | Apakah seed dilaporkan untuk setiap run? |
| Metrik | Apakah metrik dihitung dengan protokol yang jelas? |
| Statistik | Apakah ada CI atau uji signifikansi? |
| Baseline | Apakah baseline dituning secara adil? |
| Ablation | Apakah semua kontribusi diuji? |

**Latihan**: gunakan checklist ini untuk menilai 2–3 paper pada pertemuan ini.

---

# Slide 32 - Evaluasi Reproducibility: Studi Kasus

Studi kasus paper dengan klaim kuat namun tanpa detail:

- Paper melaporkan akurasi 98,2% mengalahkan baseline 96,5%.
- Tidak ada kode, tidak ada seed, tidak ada CI.
- Baseline hanya 1 konfigurasi default.
- Tidak ada ablation.

**Analisis**

- Apakah klaim dapat dipercaya?
- Eksperimen apa yang seharusnya dilakukan?
- Bagaimana protokol dapat diperbaiki?

**Tugas**: tulis ulang desain eksperimen paper tersebut menjadi protokol yang valid.

---

# Slide 33 - Peer Review Protokol Eksperimen

**Peer review dilakukan terhadap protokol mahasiswa lain.**

Kriteria penilaian:

- Apakah research question jelas?
- Apakah baseline dipilih dengan adil?
- Apakah ablation sesuai dengan klaim kontribusi?
- Apakah pengendalian variabel memadai?
- Apakah analisis statistik direncanakan?
- Apakah dokumentasi dan artefak lengkap?

**Output**

- Umpan balik tertulis dengan saran perbaikan.
- Nilai kelayakan: layak / revisi / tidak layak.

---

# Slide 34 - Menangani Hasil Negatif dan Anomaly

**Troubleshooting Pipeline**

| Gejala | Kemungkinan Penyebab |
|---|---|
| Hasil sangat tinggi | Leakage, duplikat, evaluasi salah |
| Hasil sangat rendah | Learning rate salah, preprocessing salah |
| Varians antar seed besar | Batch kecil, augmentasi ekstrem |
| Baseline tidak masuk akal | Implementasi baseline salah |
| Ablation tidak menunjukkan perbedaan | Komponen tidak berpengaruh, konfigurasi kurang |

**Keputusan**

- Perbaiki pipeline.
- Ulangi eksperimen.
- Jika hasil tetap negatif, laporkan sebagai temuan ilmiah.

---

# Slide 35 - Reporting: Tabel Metrik dan Analisis Statistik

Contoh format pelaporan:

| Metode | Accuracy (%) | F1 (%) | CI 95% | p-value |
|---|---|---|---|---|
| Baseline ResNet-50 | 91,2 ± 0,3 | 90,1 ± 0,4 | [90,7; 91,7] | – |
| + Attention Module | 92,8 ± 0,2 | 91,9 ± 0,3 | [92,4; 93,2] | 0,01 |
| + Attention + Loss B | 93,5 ± 0,2 | 92,7 ± 0,2 | [93,1; 93,9] | 0,003 |

**Prinsip**

- Laporkan mean ± std dari beberapa seed.
- Sertakan CI dan uji statistik.
- Jelaskan metode uji dan koreksi multiple comparison.
- Jangan hanya menampilkan angka terbaik.

---

# Slide 36 - Dokumentasi Lingkungan dan Artefak

**Artefak yang Harus Disimpan**

- Kode sumber dan config.
- Log training dan metrics.
- Checkpoint model.
- Dataset split yang digunakan.
- File environment.

**Alat**

- `requirements.txt` atau `environment.yml`.
- Dockerfile untuk lingkungan penuh.
- Git untuk versioning.
- MLflow / Weights & Biases / TensorBoard untuk logging.
- Model card dan dataset card untuk dokumentasi.

---

# Slide 37 - Dari Eksperimen ke Proposal Disertasi

**Peran Protokol Eksperimen**

- Bagian metodologi disertasi.
- Dasar untuk menguji hipotesis.
- Bukti kelayakan penelitian.
- Landasan rencana publikasi.

**Yang Disiapkan pada Pertemuan 14**

- Experimental matrix.
- Jadwal komputasi.
- Rencana mitigasi risiko.
- Hasil awal untuk proposal.

---

# Slide 38 - Workshop: Menyusun Experimental Protocol

**Langkah Latihan**

1. Pilih topik dan tulis satu research question.
2. Tetapkan dataset dan split.
3. Tentukan baseline dan metode usulan.
4. Rancang ablation study.
5. Tentukan metrik, jumlah seed, dan uji statistik.
6. Dokumentasikan lingkungan dan artefak.
7. Tulis estimasi computational budget.
8. Presentasikan protokol untuk peer review.

**Template tersedia di notebook praktikum.**

---

# Slide 39 - Rubrik Penilaian Protokol Eksperimen

| Aspek | Skor 1–4 |
|---|---|
| Kejelasan research question dan tujuan | 1–4 |
| Kesesuaian dataset dan split | 1–4 |
| Keadilan pemilihan baseline | 1–4 |
| Kualitas desain ablation | 1–4 |
| Ketepatan metrik dan statistik | 1–4 |
| Dokumentasi reproducibility | 1–4 |
| Realisme computational budget | 1–4 |

**Kriteria**

- 4 = sangat baik dan siap digunakan
- 3 = baik dengan sedikit revisi
- 2 = perlu perbaikan signifikan
- 1 = belum memenuhi standar

---

# Slide 40 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Formulasi Research Question, Hipotesis, dan Novelty**