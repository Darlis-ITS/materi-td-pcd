# Slide 00 - Cover

EF256129 - TD PCD  
Pertemuan 11

## Explainable, Robust, dan Trustworthy Computer Vision

Dr. Darlis Herumurti  
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 11 dalam RPS

## Kedudukan Materi

| Posisi | Pertemuan | Topik |
|---|---|---|
| Sebelumnya | 10 | 3D Vision, Multi-View Geometry, dan Neural Rendering |
| Saat ini | 11 | Explainable, Robust, dan Trustworthy Computer Vision |
| Berikutnya | 12 | Experimental Design dan Reproducible Benchmarking |

## Alur Berpikir

- Pertemuan 1–9 membangun kemampuan **membuat model** yang semakin canggih: CNN, ViT, SSL, multimodal, restoration, deteksi, segmentasi, dan generative model.
- Pertemuan 10 memperluas kemampuan model ke representasi 3D dan neural rendering.
- Pertemuan 11 berhenti sejenak untuk mengajukan pertanyaan kritis: **dapatkah model-model tersebut dipercaya?**

> Tanpa evaluasi kepercayaan, akurasi tinggi pada benchmark tidak cukup sebagai bukti ilmiah di tingkat doktoral.

---

# Slide 02 - Tujuan Pembelajaran dan Target Keluaran

## Capaian yang Diharapkan

Setelah pertemuan ini, mahasiswa mampu:

1. Menjelaskan konsep interpretabilitas, robustness, uncertainty, calibration, fairness, dan adversarial risk pada computer vision.
2. Mengevaluasi apakah explanation yang dihasilkan benar-benar faithful terhadap alasan prediksi model.
3. Membuat attribution map, calibration plot, dan melakukan perturbation test secara praktis.
4. Menyusun daftar risiko model dan mengkomunikasikannya melalui model card.

## Target Keluaran Pertemuan

- **Trustworthiness audit singkat** untuk satu model atau pipeline computer vision.
- Bukti eksperimen berupa:
  - Attribution map.
  - Reliability / calibration plot.
  - Hasil perturbation sederhana.
  - Analisis contoh kegagalan model.

---

# Slide 03 - Pertanyaan Kunci Pertemuan Ini

## Tiga Pertanyaan yang Harus Dijawab

| Pertanyaan | Implikasi Riset |
|---|---|
| Apakah explanation berkorelasi dengan alasan sebenarnya model memproduksi prediksi? | Menentukan validitas metode interpretability. |
| Bagaimana performa model berubah pada data di luar distribusi? | Menentukan batas generalisasi dan robustness. |
| Metrik kepercayaan apa yang relevan untuk dilaporkan kepada pengguna? | Menentukan standar pelaporan ilmiah yang jujur. |

## Fokus Penelitian

- Menguji **faithfulness** explanation, bukan sekadar visualisasi yang tampak masuk akal.
- Mengukur perubahan performa saat **distribusi data berubah**.
- Mengkomunikasikan **risiko model** secara eksplisit kepada pengguna.

---

# Slide 04 - Jembatan: Dari 3D Vision dan Generative Models ke Trustworthy Vision

## Mengapa Materi Ini Muncul Sekarang

- NeRF dan 3D reconstruction menghasilkan sintesis gambar yang fotorealistik, tetapi **belum tentu benar secara geometris**.
- Diffusion model dapat menghasilkan citra yang meyakinkan, tetapi juga dapat **halusinasi detail**.
- Detector dan segmenter modern mencapai akurasi tinggi pada benchmark, tetapi **tidak menyediakan ukuran ketidakpastian** pada prediksi.

## Peran Pertemuan 11

- Memberikan **lensa kritis** untuk mengevaluasi semua model yang telah dipelajari pada pertemuan 3–10.
- Membekali mahasiswa dengan alat untuk menjawab: *"Apakah model ini aman digunakan dalam konteks penelitian atau aplikasi nyata?"*
- Hasil audit ini menjadi input penting untuk merancang eksperimen yang ketat pada pertemuan 12.

---

# Slide 05 - Definisi Trustworthy Computer Vision

## Apa Itu Trustworthy Computer Vision?

Trustworthy computer vision adalah pendekatan pengembangan dan evaluasi model yang menjamin bahwa model:

- **Dapat dijelaskan** — keputusan dapat dilacak dan dipahami.
- **Robust** — tidak mudah runtuh akibat gangguan kecil atau perubahan distribusi.
- **Terkalibrasi** — tingkat kepercayaan model sesuai dengan akurasi aktual.
- **Adil** — tidak mendiskriminasi kelompok tertentu secara sistematis.
- **Akuntabel** — risiko kegagalan terdokumentasi dan dikomunikasikan kepada pengguna.

## Konsekuensi untuk Penelitian

- Akurasi bukan satu-satunya klaim ilmiah yang valid.
- Proposal disertasi harus menyertakan analisis risiko dan keterbatasan model.
- Reproducibility hanya bermakna jika evaluasi mencakup aspek kepercayaan.

---

# Slide 06 - Empat Dimensi Utama Trustworthiness

## Peta Dimensi

```text
                    TRUSTWORTHY CV
                          |
        -------------------------------------
        |           |           |           |
   Explainability  Robustness  Uncertainty  Fairness
   "mengapa?"     "jika diuji" "seberapa yakin?" "untuk siapa?"
        |           |           |           |
   saliency map  distribution  calibration  bias audit
   gradient      shift         predictive   model card
   attribution   adversarial   uncertainty
   perturbation  robustness
```

## Kaitan Antar Dimensi

- Uncertainty yang buruk dapat memperbesar dampak adversarial example.
- Robustness yang diukur tanpa analisis kegagalan dapat menyembunyikan bias.
- Explainability harus dievaluasi bersama calibration agar tidak menyesatkan.

---

# Slide 07 - Explainability: Konsep dan Terminologi

## Interpretabilitas Intrinsik vs Post-Hoc

| Jenis | Deskripsi | Contoh |
|---|---|---|
| Intrinsik | Model dirancang agar mudah dipahami sejak awal | Linear regression, decision tree |
| Post-hoc | Model kompleks dijelaskan setelah dilatih | Grad-CAM, SHAP, LIME |

## Lokal vs Global

- **Local explanation**: menjelaskan satu prediksi tertentu (misal, mengapa gambar ini diklasifikasikan sebagai kucing?).
- **Global explanation**: menjelaskan perilaku model secara keseluruhan (misal, fitur apa yang paling sering digunakan?).

## Pertanyaan Kritis

- Explanation visual **belum tentu sama dengan mekanisme keputusan model**.
- Kita memerlukan uji tambahan untuk membuktikan bahwa penjelasan tersebut faithful.

---

# Slide 08 - Saliency Map dan Gradient Attribution

## Gagasan Utama

- Menghitung sensitivitas output model terhadap perubahan setiap piksel input.
- Piksel dengan gradien besar dianggap **penting** bagi keputusan model.

## Formulasi Sederhana

- Misal \( f_c(x) \) adalah logit atau skor untuk kelas \( c \).
- Saliency didefinisikan sebagai magnitudo gradien:

```text
S(x) = | ∂ f_c(x) / ∂ x |
```

- Nilai \( S(x) \) dipetakan ke \([0, 1]\) dan divisualisasikan sebagai peta panas.

## Keterbatasan Awal

- Gradien hanya sensitif **lokal** di sekitar titik input.
- Gradien dapat didominasi noise, sehingga perlu variasi seperti SmoothGrad atau Input × Gradient.

---

# Slide 09 - Grad-CAM dan Integrated Gradients

## Grad-CAM

- Menggunakan gradien dari skor kelas terhadap **feature map** pada lapisan konvolusi terakhir.
- Menghasilkan peta yang **lebih halus** dan semantik daripada saliency piksel.
- Cocok untuk CNN dan dapat diperluas ke ViT melalui attention rollout.

## Integrated Gradients

- Mengakumulasi gradien sepanjang garis lurus dari baseline ke input:

```text
IG_i(x) = (x_i - x'_i) × ∫_{α=0}^{1} ∂ f_c(x' + α(x - x')) / ∂ x_i dα
```

- Memenuhi beberapa aksioma seperti sensitivity dan implementation invariance.

## Catatan Praktis

- Pilihan baseline, layer, dan referensi kelas sangat memengaruhi hasil.
- Attribution map harus dilaporkan bersama **parameter teknis** agar reprodusibel.

---

# Slide 10 - Perturbation Test dan Occlusion Sensitivity

## Prinsip

- Jika suatu region diklaim penting, menghilangkan atau mengganggu region tersebut seharusnya **mengubah prediksi secara signifikan**.
- Metode sederhana: geser jendela buram (occlusion patch) melintasi gambar dan amati perubahan skor kelas.

## Alur Pengujian

```text
Input -> Pilih patch (misal 16x16, stride 16)
      -> Tutup patch (zero / blur / noise)
      -> Hitung skor kelas target
      -> Catat perubahan skor
      -> Bangun peta pentingnya berdasarkan penurunan skor
```

## Interpretasi

- Region yang menyebabkan penurunan skor besar dianggap **esensial**.
- Attribution map yang baik harus konsisten dengan hasil occlusion test.

---

# Slide 11 - Evaluasi Attribution: Faithfulness vs Plausibility

## Plausibility

- Secara visual tampak masuk akal bagi manusia.
- Contoh: peta panas menunjuk ke objek, bukan latar belakang.
- **Belum tentu benar**: manusia mudah percaya pada visualisasi yang rapi.

## Faithfulness

- Sejauh mana atribusi benar-benar mencerminkan sebab prediksi model.
- Diuji dengan cara **menghapus atau mempertahankan** fitur yang dianggap penting lalu mengamati perubahan prediksi.

## Metrik Sederhana

- **Deletion**: hapus piksel paling penting secara bertahap, ukur penurunan akurasi. Penurunan tajam = atribusi faithful.
- **Insertion**: tambahkan piksel paling penting secara bertahap, ukur kenaikan akurasi. Kenaikan tajam = atribusi informatif.

> Faithfulness adalah standar yang lebih kuat daripada plausibility untuk klaim ilmiah.

---

# Slide 12 - Skenario Gagal: Attribution yang Menyesatkan

## Contoh Umum

- Model diklasifikasikan sebagai "anjing" karena latar belakang rumput dan sofanya, bukan karena bentuk anjing.
- Grad-CAM yang dihasilkan tetap menunjuk ke arah objek karena gradien positif dari region objek, sehingga secara visual "terlihat benar".
- Occlusion test mengungkap bahwa menutup objek tidak mengubah prediksi, tetapi menutup latar belakang justru mengubah prediksi.

## Pelajaran untuk Penelitian

- Attribution map tidak boleh dilaporkan tanpa **uji kontrafaktual**.
- Setiap klaim "model menggunakan fitur X" harus didukung oleh eksperimen perturbasi.
- Auditor harus mencari **contoh kegagalan** secara aktif, bukan hanya contoh keberhasilan.

---

# Slide 13 - Predictive Uncertainty: Aleatoric vs Epistemic

## Dua Sumber Ketidakpastian

| Jenis | Arti | Contoh pada Citra | Dapat Dikurangi dengan Data? |
|---|---|---|---|
| Aleatoric | Ketidakpastian intrinsik dari data | Kabut, oklusi, citra blur, label ambigu | Tidak langsung |
| Epistemic | Ketidakpastian model | Data uji jauh dari distribusi pelatihan | Ya, dengan data baru atau model lebih baik |

## Implikasi

- Model perlu **menyatakan ketidakpastian** tinggi saat input tidak dikenal.
- Hanya memberikan probabilitas softmax tidak cukup, karena sering kali overconfident.
- Estimasi epistemic uncertainty memerlukan pendekatan Bayesian, ensemble, atau test-time augmentation.

---

# Slide 14 - Calibration: Apakah Confidence Dapat Dipercaya?

## Masalah Overconfidence

- Model deep learning sering memberikan probabilitas tinggi, bahkan saat prediksi salah.
- Akurasi rata-rata 90% tidak berarti setiap prediksi dengan confidence 0,9 juga akurat 90%.
- Ketidakcocokan ini disebut **miscalibration**.

## Definisi Calibration

Model dikatakan terkalibrasi dengan baik jika:

```text
P(prediksi benar | confidence = p) ≈ p
```

## Cara Memperbaiki

- **Temperature scaling**: membagi logit dengan parameter suhu \( T > 0 \) lalu softmax.
- **MC-Dropout**: jalankan dropout pada saat inferensi beberapa kali, gunakan varians prediksi sebagai ketidakpastian.
- **Ensemble**: rata-ratakan prediksi beberapa model untuk mendapatkan estimasi ketidakpastian yang lebih stabil.

---

# Slide 15 - Reliability Diagram dan Calibration Metrics

## Reliability Diagram

- Kelompokkan seluruh prediksi berdasarkan confidence ke dalam 10 bin.
- Plot rata-rata confidence terhadap akurasi aktual tiap bin.
- Jika model terkalibrasi sempurna, titik-titik berada pada garis diagonal.

## Interpretasi

- Titik di bawah garis diagonal → model **overconfident**.
- Titik di atas garis diagonal → model **underconfident**.

## ECE (Expected Calibration Error)

```text
ECE = Σ_{m=1}^{M} (|B_m| / N) × |acc(B_m) - conf(B_m)|
```

- Bobot adalah proporsi jumlah sampel pada tiap bin.
- ECE rendah berarti calibration baik.

## Metrik Pendukung

- **MCE**: error maksimum pada satu bin.
- **Brier Score**: rata-rata kuadrat selisih probabilitas dan label aktual.

---

# Slide 16 - Distribution Shift: Definisi

## Apa Itu Distribution Shift?

Perubahan distribusi data antara waktu pelatihan dan waktu inferensi.

```text
Data training:  (x, y) ~ P_train(x, y)
Data uji:       (x, y) ~ P_test(x, y)
Distribution shift:  P_train ≠ P_test
```

## Contoh pada Computer Vision

- Foto diambil dengan kamera berbeda dari dataset pelatihan.
- Kondisi pencahayaan, cuaca, atau musim berubah.
- Domain medis: data dari rumah sakit A vs rumah sakit B.
- Citra sintetis dari diffusion model yang digunakan sebagai data uji.

## Mengapa Penting

- Akurasi pada test set standar tidak menjamin performa di lapangan.
- Trustworthiness audit harus secara eksplisit menguji perubahan distribusi.

---

# Slide 17 - Jenis Distribution Shift

## Tiga Tipe Utama

| Tipe | Perubahan | Contoh |
|---|---|---|
| Covariate shift | \( P(x) \) berubah, \( P(y \mid x) \) tetap | Gambar lebih gelap, sensor berbeda |
| Label shift | \( P(y) \) berubah, \( P(x \mid y) \) tetap | Proporsi kelas tumor berubah |
| Concept shift | \( P(y \mid x) \) berubah | Kriteria diagnosis berubah antar waktu |

## Implikasi Evaluasi

- Model yang robust terhadap covariate shift belum tentu robust terhadap concept shift.
- Setiap pertanyaan penelitian harus menentukan **jenis shift mana** yang paling relevan dengan domainnya.
- Pengujian tidak boleh hanya satu jenis gangguan; perlu matriks shift.

---

# Slide 18 - Mengukur Robustness terhadap Distribution Shift

## Protokol Evaluasi Sistematis

1. Definisikan jenis shift yang akan diuji.
2. Sediakan dataset uji yang sesuai, baik dataset publik maupun versi yang dimodifikasi.
3. Apakah model memakai domain adaptation atau langsung zero-shot?
4. Laporkan performa pada distribusi asli dan distribusi yang bergeser.
5. Hitung **degradation score**:

```text
Degradation = Akurasi_clean - Akurasi_shifted
```

## Yang Harus Dilaporkan

- Akurasi per kelompok data, bukan hanya rata-rata.
- Confidence dan calibration pada data shifted.
- Contoh kegagalan yang representatif.

## Kesalahan Umum

- Menggunakan augmentasi random sebagai proxy distribution shift.
- Menguji hanya satu jenis noise.

---

# Slide 19 - Adversarial Example: Definisi dan Motivasi

## Definisi

Adversarial example adalah input yang telah dimodifikasi secara halus namun menyebabkan model membuat kesalahan besar.

```text
x' = x + δ,   ||δ||_p ≤ ε,   f(x') ≠ y_true
```

- δ sangat kecil sehingga tidak terlihat oleh manusia.
- Model mungkin mengklasifikasikan panda sebagai gibbon, atau "stop sign" sebagai "speed limit".

## Motivasi Penelitian

- Memahami apakah model benar-benar memahami konten visual atau hanya memakai pola yang rapuh.
- Menguji batas keamanan pada sistem seperti kendaraan otonom, pengenalan wajah, dan diagnosis medis.
- Adversarial example adalah salah satu bukti bahwa akurasi benchmark tidak cukup untuk klaim kepercayaan.

---

# Slide 20 - Serangan Adversarial: FGSM dan PGD

## FGSM (Fast Gradient Sign Method)

- Metode one-step menggunakan tanda gradien:

```text
x' = x + ε · sign(∇_x L(f(x), y))
```

- Cepat dan sederhana, tetapi tidak selalu optimal.

## PGD (Projected Gradient Descent)

- Iteratif, melakukan beberapa langkah kecil dan proyeksi kembali ke bola ε:

```text
x_{t+1} = clip_{x, ε}(x_t + α · sign(∇_x L(f(x_t), y)))
```

## Black-Box vs White-Box

- White-box: penyerang memiliki akses penuh ke model dan gradien.
- Black-box: penyerang hanya dapat mengamati output, sering memakai transferable attack.
- Adversarial example yang dihasilkan pada satu model sering dapat menipu model lain.

---

# Slide 21 - Pertahanan Adversarial dan Gap Evaluasi

## Strategi Pertahanan Umum

- **Adversarial training**: melatih model pada contoh serangan yang dihasilkan setiap iterasi.
- **Preprocessing**: denoising, kompresi, atau randomisasi input.
- **Certified robustness**: memberikan jaminan matematis bahwa model stabil dalam radius ε.

## Gap Evaluasi

- Pertahanan yang tampak berhasil pada satu jenis serangan sering **gagal terhadap serangan adaptif**.
- Evaluasi harus menggunakan **adaptive attacker** yang menyesuaikan serangan dengan pertahanan.
- Robustness dan akurasi sering berada dalam trade-off; laporan harus menyertakan keduanya.

> Aturan penting: klaim robustness hanya sah jika diuji oleh penyerang yang sekuat mungkin.

---

# Slide 22 - Fairness: Definisi dan Relevansi pada CV

## Definisi

Fairness adalah tidak adanya bias sistematis yang merugikan kelompok tertentu berdasarkan atribut sensitif seperti gender, usia, etnis, atau kondisi fisik.

## Contoh pada Computer Vision

- Face recognition memiliki akurasi lebih rendah pada kulit gelap dibanding kulit terang.
- Sistem rekrutmen berbasis video lebih sering mendiskualifikasi kandidat dari kelompok tertentu.
- Segmentation model medis bekerja lebih buruk pada kelompok usia atau jenis kelamin tertentu.

## Konsekuensi Ilmiah

- Rata-rata metrik yang tinggi dapat menyembunyikan kesenjangan antar kelompok.
- Trustworthiness audit harus melaporkan metrik **per kelompok**, bukan hanya metrik agregat.

---

# Slide 23 - Sumber Bias pada Pipeline Computer Vision

## Lokasi Bias

| Tahap | Contoh Sumber Bias |
|---|---|
| Dataset | Distribusi kelas tidak seimbang, under-representasi kelompok tertentu |
| Anotasi | Labeler memiliki standar berbeda, label ambigu, crowdsourcing tidak konsisten |
| Augmentasi | Transformasi yang memperkuat pola tertentu, menghilangkan variasi alami |
| Model | Arsitektur atau loss yang lebih cocok untuk satu mode data |
| Evaluasi | Test set tidak mewakili populasi dunia nyata |

## Pertanyaan Audit

- Dari mana data berasal dan bagaimana memilihnya?
- Siapa yang membuat anotasi dan bagaimana konsistensinya diukur?
- Kelompok mana yang mungkin tidak terwakili dalam data latih?

---

# Slide 24 - Metrik Fairness

## Metrik yang Umum Digunakan

| Metrik | Ide Dasar |
|---|---|
| Demographic Parity | Probabilitas prediksi positif sama untuk semua kelompok |
| Equalized Odds | False positive rate dan false negative rate sama antar kelompok |
| Equal Opportunity | True positive rate sama antar kelompok |
| Calibration by Group | Confidence memiliki akurasi yang sama antar kelompok |

## Langkah Praktis

1. Definisikan atribut sensitif yang relevan.
2. Kelompokkan sample berdasarkan atribut tersebut.
3. Hitung metrik performa utama untuk tiap kelompok.
4. Hitung selisih antar kelompok (max gap).
5. Laporkan dalam model card.

> Fairness tidak berarti menghilangkan semua perbedaan; harus dinilai sesuai konteks aplikasi.

---

# Slide 25 - Fairness pada Detection dan Segmentation

## Perbedaan dengan Klasifikasi

- Detection dan segmentation menghasilkan banyak prediksi per gambar.
- Fairness dapat dievaluasi pada beberapa level:
  - Per gambar: apakah setiap orang terdeteksi?
  - Per objek: presisi dan recall per kelas atau per kelompok.
  - Per area: kualitas mask segmentasi pada kelompok tertentu.

## Contoh Kasus

- Detector mobil lebih sering mendeteksi objek pada wilayah terang.
- Model segmentasi bangunan salah memisahkan area permukiman pada citra kota tertentu.
- SAM yang dilatih pada data internet mungkin memiliki kinerja tidak konsisten pada objek langka.

## Implikasi

- Laporan evaluasi harus memisahkan metrik per kelompok atribut.
- Analisis kegagalan harus menyertakan visualisasi contoh kegagalan per kelompok.

---

# Slide 26 - Model Card: Komunikasi Risiko Model

## Apa Itu Model Card?

Dokumen ringkas yang menyertai model dan menjelaskan:

- Tujuan model.
- Konteks penggunaan yang dimaksudkan.
- Data pelatihan dan evaluasi.
- Metrik performa utama.
- Keterbatasan dan risiko yang diketahui.

## Mengapa Diperlukan

- Pengguna model sering tidak membaca paper teknis.
- Model card menjembatani peneliti, pengembang, dan pembuat kebijakan.
- Memaksa peneliti untuk jujur tentang keterbatasan model.
- Menjadi bukti bahwa trustworthiness telah dipertimbangkan secara sistematis.

---

# Slide 27 - Contoh Komponen Model Card

## Bagian Utama

| Komponen | Isi yang Disarankan |
|---|---|
| Model details | Arsitektur, versi, framework, lisensi |
| Intended use | Domain aplikasi, pengguna target, bukan tujuan penggunaan |
| Factors | Atribut sensitif, domain, kondisi pencahayaan |
| Metrics | Accuracy, ECE, robustness degradation, fairness gap |
| Training data | Sumber, jumlah, rentang waktu, preprocessing |
| Evaluation data | Protokol split, deskripsi test set |
| Ethical considerations | Risiko penyalahgunaan, bias, mitigasi |
| Caveats | Rekomendasi penggunaan yang tidak disarankan |

## Prinsip

- Setiap klaim pada model card harus didukung bukti eksperimen.
- Model card adalah artefak audit, bukan hiasan dokumentasi.

---

# Slide 28 - Trustworthiness Audit: Workflow

## Alur Audit Sistematis

```text
1. Pilih model + task + dataset uji
        |
2. Definisikan pertanyaan audit
   (explainability? robustness? calibration? fairness?)
        |
3. Jalankan pengukuran:
   - attribution map + occlusion test
   - calibration plot + ECE
   - uji distribution shift/adversarial
   - metrik per kelompok
        |
4. Analisis kegagalan:
   - kumpulkan contoh salah
   - kategorikan pola kegagalan
        |
5. Susun daftar risiko dan model card
        |
6. Dokumentasikan di research log dan repositori
```

## Hasil Audit

- Daftar risiko yang spesifik: misal, "model gagal pada gambar malam hari, ECE naik 0,15 pada citra kabur".
- Rekomendasi penggunaan atau kebutuhan perbaikan.

---

# Slide 29 - Trustworthiness Audit: Daftar Risiko Model

## Contoh Format Daftar Risiko

| Dimensi | Pengujian | Hasil yang Dilaporkan | Risiko yang Teridentifikasi |
|---|---|---|---|
| Attribution | Grad-CAM + occlusion | Peta panas, deletion/insertion score | Model bergantung pada tekstur, bukan bentuk |
| Calibration | Reliability plot, ECE | ECE = 0,08 (clean), 0,21 (OOD) | Model overconfident pada data OOD |
| Robustness | Gaussian noise, blur, brightness | Akurasi turun 20% pada blur kuat | Rawan pada kondisi sensor buruk |
| Adversarial | FGSM ε=0.05 | Akurasi turun 78% | Perlu adversarial training |
| Fairness | Ukur per kelompok | Gap akurasi 12% antar kelompok | Dataset tidak seimbang |

## Kegunaan

- Daftar ini menjadi lampiran proposal disertasi yang memperkuat argumen kontribusi.
- Menunjukkan bahwa mahasiswa memahami keterbatasan model yang diusulkan.

---

# Slide 30 - Praktikum 1: Menghasilkan Attribution Map

## Kode Sederhana dengan PyTorch

```python
import torch
import matplotlib.pyplot as plt

## x = tensor input berukuran (1, 3, H, W), requires_grad=True
model.eval()
x.requires_grad_(True)

out = model(x)
pred = out.argmax(dim=1).item()

## Backprop dari skor kelas prediksi atau kelas target
out[0, pred].backward()

## Saliency: max abs gradien pada kanal warna
saliency = x.grad.abs().squeeze(0).max(dim=0).values
saliency = (saliency - saliency.min()) / (saliency.max() - saliency.min())

plt.imshow(saliency.cpu().detach().numpy(), cmap="hot")
plt.axis("off")
plt.title(f"Saliency untuk kelas {pred}")
plt.show()
```

## Tugas

- Ulangi untuk beberapa gambar dan beberapa kelas.
- Bandingkan dengan occlusion test.
- Catat kesesuaian antara saliency dan region yang benar-benar mengubah prediksi.

---

# Slide 31 - Praktikum 2: Calibration Plot dan Reliability

## Kode Membuat Reliability Diagram

```python
import numpy as np
import matplotlib.pyplot as plt

def reliability_curve(proba, y_true, n_bins=10):
    conf = np.max(proba, axis=1)
    acc = (proba.argmax(axis=1) == y_true).astype(float)
    bins = np.linspace(0, 1, n_bins + 1)
    centers, means, accs = [], [], []
    for i in range(n_bins):
        mask = (conf >= bins[i]) & (conf < bins[i+1])
        if mask.sum() > 0:
            centers.append((bins[i] + bins[i+1]) / 2)
            means.append(conf[mask].mean())
            accs.append(acc[mask].mean())
    return centers, means, accs

centers, means, accs = reliability_curve(proba, y_true)
plt.plot([0, 1], [0, 1], "--", label="Perfect")
plt.plot(means, accs, "o-", label="Model")
plt.xlabel("Confidence")
plt.ylabel("Accuracy")
plt.legend()
plt.show()
```

## Pertanyaan

- Pada bin mana model paling overconfident?
- Bagaimana ECE berubah jika dievaluasi pada data dengan distribution shift?

---

# Slide 32 - Praktikum 3: Perturbation Test dan Analisis Kegagalan

## Occlusion Sensitivity

```python
def occlusion_test(model, x, patch=16, stride=16, class_id=None):
    model.eval()
    with torch.no_grad():
        base = model(x)[0, class_id].item()
    _, _, H, W = x.shape
    map_occ = torch.zeros(H, W)
    for y in range(0, H - patch + 1, stride):
        for x0 in range(0, W - patch + 1, stride):
            x_occ = x.clone()
            x_occ[0, :, y:y+patch, x0:x0+patch] = 0
            score = model(x_occ)[0, class_id].item()
            map_occ[y:y+patch, x0:x0+patch] = base - score
    return map_occ
```

## Langkah Analisis Kegagalan

1. Temukan semua prediksi salah pada test set.
2. Kelompokkan berdasarkan pola umum: blur, oklusi, objek kecil, latar mirip objek.
3. Ambil 3–5 contoh representatif untuk disajikan dalam laporan.
4. Tulis hipotesis penyebab kegagalan dan rencanakan eksperimen verifikasi.

---

# Slide 33 - Membaca dan Mengaudit Paper dengan Kacamata Trustworthiness

## Pertanyaan yang Diajukan Saat Membaca Paper

| Aspek | Pertanyaan Audit |
|---|---|
| Explainability | Apakah attribution dievaluasi dengan faithfulness metric? |
| Calibration | Apakah confidence dilaporkan bersama accuracy? |
| Robustness | Apakah model diuji terhadap distribution shift atau adversarial attack? |
| Fairness | Apakah metrik dilaporkan per kelompok? |
| Reproducibility | Apakah seed, konfigurasi, dan kode tersedia? |

## Aktivitas Seminar Paper

- Setiap mahasiswa memilih satu paper computer vision.
- Lakukan audit kecil terhadap paper tersebut.
- Presentasikan: kekuatan bukti, kelemahan evaluasi, dan saran perbaikan.

> Tujuan bukan menghakimi paper, tetapi melatih ketelitian metodologis.

---

# Slide 34 - Kaitan dengan Pertemuan 12: Experimental Design

## Hubungan Langsung

- Pertemuan 11 menyediakan **pertanyaan dan metrik**.
- Pertemuan 12 menyediakan **protokol eksperimen** untuk menjawabnya dengan valid.

## Yang Dibawa ke Pertemuan 12

- Daftar risiko model sebagai dasar pemilihan baseline dan ablation.
- Metrik yang tidak hanya akurasi: ECE, degradation score, fairness gap, deletion score.
- Pengalaman kegagalan yang membantu merancang error analysis.
- Kebutuhan kontrol: seed, split data, dan kondisi eksperimen yang adil.

## Contoh

- Jika model overconfident pada OOD, maka eksperimen di pertemuan 12 harus mencakup:
  - Variasi seed untuk estimasi interval confidence.
  - Baseline calibration yang dibandingkan secara statistik.
  - Dataset OOD yang terdokumentasi dengan baik.

---

# Slide 35 - Peluang Kontribusi Penelitian Doktoral

## Ide Riset yang Terbuka

| Area | Potensi Kontribusi |
|---|---|
| Faithfulness metric | Metrik baru untuk menguji apakah attribution benar-benar sebab prediksi |
| Calibration OOD | Metode kalibrasi yang stabil pada distribution shift |
| Robustness ViT | Studi sistematis perilaku transformer terhadap serangan adversarial |
| Fairness segmentation | Audit bias mask pada domain medis atau perkotaan |
| Model card otomatis | Dokumentasi risiko yang dihasilkan dari analisis kegagalan |

## Posisi terhadap State-of-the-Art

- Kontribusi tidak harus selalu arsitektur baru, tetapi bisa berupa:
  - Metrik evaluasi baru.
  - Dataset atau benchmark auditing.
  - Analisis teoretis tentang keterbatasan metode.
  - Protokol trustworthiness untuk domain tertentu.

---

# Slide 36 - Ringkasan dan Takeaway

## Inti Pertemuan

1. Akurasi tinggi tidak sama dengan model yang dapat dipercaya.
2. Attribution map harus diuji faithfulness-nya, bukan hanya dilihat visualnya.
3. Confidence perlu divalidasi melalui calibration.
4. Distribution shift dan adversarial attack adalah bagian wajib dalam evaluasi robustness.
5. Fairness harus diukur per kelompok, bukan hanya rata-rata.
6. Model card membantu mengkomunikasikan risiko secara ilmiah.

## Aksi Setelah Pertemuan

- Selesaikan praktikum: attribution map, calibration plot, perturbation test.
- Pilih satu model untuk dianalisis sebagai trustworthiness audit.
- Bawa hasil audit ke pertemuan 12 untuk dirancang eksperimen yang ketat.

---

# Slide 37 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Experimental Design dan Reproducible Benchmarking**