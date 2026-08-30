# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 06
# Image Restoration dan Computational Imaging

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan dalam Rangkaian Perkuliahan

| Posisi | Topik |
|---|---|
| Sebelumnya | Vision-Language Models dan Multimodal Representation |
| Saat ini | Image Restoration dan Computational Imaging |
| Berikutnya | Object Detection Modern dengan YOLO dan Transformer |

Pertemuan ini berpindah dari **memahami makna citra** (representasi, multimodal) ke **memperbaiki kualitas citra** dari pengukuran yang rusak atau tidak lengkap.

Restoration memanfaatkan:
- representasi visual modern dari Pertemuan 03
- self-supervised learning dan foundation model dari Pertemuan 04
- keterampilan critical reading dan evaluasi klaim dari Pertemuan 02

Keterkaitan CPMK: CPMK-1, CPMK-3, CPMK-4.

---

# Slide 02 - Tujuan Pembelajaran dan Target Keluaran

## Tujuan Pembelajaran

- Merumuskan image restoration sebagai **inverse problem** dengan forward model yang eksplisit.
- Memahami perbedaan **fidelity numerik** dan **kualitas perseptual**.
- Mengevaluasi metode restoration secara **tidak bergantung pada satu metrik**.

## Target Keluaran

- Protokol evaluasi restoration.
- Analisis trade-off antara fidelity, perceptual quality, dan computational cost.
- Hasil eksperimen degradasi sintetis dan restoration baseline.

---

# Slide 03 - Pertanyaan Kunci Pertemuan Ini

Empat pertanyaan yang akan diuji sepanjang pertemuan:

1. Apakah model **memulihkan informasi asli** atau **menghasilkan detail yang meyakinkan**?
2. Bagaimana **ketidakpastian** dari solusi inverse problem diperlakukan?
3. Apakah **data sintetis** yang digunakan untuk melatih atau mengevaluasi benar-benar merepresentasikan degradasi nyata?
4. Bagaimana merancang evaluasi yang **tidak bias oleh satu metrik**?

Pertanyaan-pertanyaan ini menjadi dasar critical paper review dan perumusan proposal riset.

---

# Slide 04 - Apa Itu Image Restoration?

**Image restoration** memperkirakan citra bersih `x` dari observasi terdegradasi `y`.

```
x (clean) --forward model--> y (degraded) --> restoration --> x_hat (estimate)
```

Tugas penting:
- denoising
- deblurring
- super-resolution
- inpainting
- de-hazing / deraining

## Bedakan dengan Image Enhancement

| Aspek | Restoration | Enhancement |
|---|---|---|
| Titik awal | model degradasi | keinginan subjektif |
| Tujuan | membalikkan degradasi | memperbaiki tampilan |
| Ground truth | sering tersedia | tidak jelas |

---

# Slide 05 - Computational Imaging: Lebih dari Sekadar Filter

**Computational imaging** adalah desain bersama antara akuisisi dan algoritma rekonstruksi.

Contoh:
- coded aperture / coded exposure
- light field dan refocusing
- phase retrieval
- tomografi dan MRI reconstruction

| Perspektif | Image Restoration Klasik | Computational Imaging |
|---|---|---|
| Input | citra akhir yang terdegradasi | pengukuran mentah / sensor |
| Forward model | blur, noise, downsampling | proyeksi, kode, sampling |
| Hasil | citra bersih | citra atau informasi tersembunyi |

Image restoration menjadi inti computational imaging karena keduanya adalah **inverse problem**.

---

# Slide 06 - Forward Model dan Inverse Problem

Forward model: proses dari citra bersih ke observasi.

```
y = f(x, degradasi)
```

Inverse problem: diberikan `y`, estimasi `x`.

Contoh forward model linear:

```
y = H x + n
```

- `y`: citra terdegradasi / pengukuran
- `H`: operator degradasi (blur, downsampling, proyeksi)
- `n`: noise
- `x`: citra bersih yang ingin dipulihkan

Diagram sederhana:

```
x --(H)--> Hx --(+n)--> y
```

---

# Slide 07 - Formulasi Matematis Degradasi

Persamaan umum untuk beberapa tugas:

- Denoising

```
y = x + n
```

- Deblurring

```
y = k ⊛ x + n
```

- Super-resolution

```
y = (k ⊛ x) ↓_s + n
```

Keterangan:
- `k`: point spread function / blur kernel
- `⊛`: operasi konvolusi
- `↓_s`: downsampling dengan faktor `s`
- `n`: noise

Beberapa degradasi dapat digabung menjadi **degradation pipeline**.

---

# Slide 08 - Klasifikasi Degradasi: Noise, Blur, Downsampling

| Degradasi | Model sederhana | Contoh sumber nyata |
|---|---|---|
| Sensor noise | Gaussian / Poisson | low-light, ISO tinggi |
| Blur | Gaussian, motion kernel, defocus | gerakan kamera, fokus salah |
| Downsampling | decimation, aliasing | resolusi sensor rendah |
| Kompresi | JPEG artifact | penyimpanan, transmisi |

Setiap jenis degradasi memiliki karakteristik matematis yang berbeda dan membutuhkan penanganan yang berbeda.

---

# Slide 09 - Model Degradasi Sintetis: Contoh dan Sumber Nyata

## Gaussian Noise

```
n ~ N(0, σ²)
y = x + n
```

Sumber nyata: electronic noise, thermal noise.

## Gaussian Blur

```
kernel k = Gaussian(sigma)
y = k ⊛ x + n
```

Sumber nyata: defocus, atmosfer.

## Downsampling

```
y = (x ⊛ k) ↓_s + n
```

Sumber nyata: sensor dengan resolusi terbatas.

Memilih parameter degradasi yang realistis adalah keputusan penelitian, bukan sekadar teknis.

---

# Slide 10 - Mengapa Image Restoration Bersifat Ill-posed

**Ill-posed** berarti salah satu dari tiga kondisi berikut tidak terpenuhi:
1. solusi tidak selalu ada
2. solusi tidak unik
3. solusi tidak bergantung secara kontinu pada data

Pada restoration:
- jumlah pixel observasi bisa lebih kecil daripada jumlah variabel yang tidak diketahui, misalnya pada super-resolution.
- blur menghilangkan informasi frekuensi tinggi.
- noise membuat banyak solusi hampir sama baiknya.

Ilustrasi:

```
x1 --> y
x2 --> y   dengan x1 != x2
```

---

# Slide 11 - Konsekuensi Ill-posedness

Tanpa asumsi tambahan:
- inverse filter sederhana akan **memperkuat noise**.
- solusi bisa **tidak stabil**: perubahan kecil pada `y` menghasilkan perubahan besar pada `x_hat`.
- banyak solusi yang sama-sama cocok dengan data.

## Implikasi Penelitian

- Setiap metode restoration sebenarnya membawa **prior** atau **asumsi** tentang citra bersih.
- Klaim "metode ini memulihkan detail" harus diuji apakah detail itu berasal dari data atau dari prior model.
- Di sinilah konsep **hallucinated detail** lahir.

---

# Slide 12 - Pendekatan Dasar: Regularisasi dan Bayesian

## Formulasi Regularisasi

```
x_hat = argmin_x  || y - H x ||²  +  λ R(x)
```

- Term pertama: data fidelity
- Term kedua: regularisasi / prior
- `λ`: kekuatan prior

## Interpretasi Bayesian

```
p(x | y) ∝ p(y | x) p(x)
```

- `p(y|x)`: likelihood dari noise
- `p(x)`: prior distribusi citra bersih
- MAP: `x_hat = argmax_x p(x|y)`

Contoh prior klasik: Tikhonov, total variation, sparsity.

---

# Slide 13 - Dari Handcrafted Prior ke Learned Prior

| Era | Prior | Kekuatan | Kelemahan |
|---|---|---|---|
| Klasik | R(x) matematis (TV, Tikhonov) | analitis, bisa dijelaskan | kurang ekspresif |
| Deep learning | prior dipelajari dari data | ekspresif, detail natural | butuh data, bisa bias |
| Generative | distribusi citra dipelajari (diffusion) | sangat kuat, multimodal | mahal, potensi halusinasi |

## Cara Kerja Deep Restoration

- Mapping langsung: `x_hat = f_θ(y)` dengan CNN/transformer.
- Deep prior: menggunakan network untuk meregularisasi optimization.
- Generative prior: sampling dari `p(x|y)`.

Poin penting: learned prior adalah **sumber pengetahuan** sekaligus **sumber bias**.

---

# Slide 14 - Arsitektur Kunci Deep Restoration

| Pola | Contoh | Karakteristik |
|---|---|---|
| CNN dangkal | SRCNN-style | sederhana, cepat |
| Residual CNN | DnCNN-style | memprediksi noise/residual |
| Encoder-decoder | U-Net | multi-scale, skip connection |
| Transformer | SwinIR, Restormer | non-local, context besar |
| Diffusion backbone | UNet + attention | generative prior |

## Elemen Umum

- Residual learning
- Normalization
- Perceptual loss / adversarial loss
- Multi-scale feature extraction

---

# Slide 15 - Attention dan Transformer dalam Restoration

Konsep:
- self-attention mempelajari hubungan antar-pixel atau region yang jauh.
- pada citra terdegradasi, konteks global membantu membedakan tekstur dan noise.

Mengapa relevan:
- blur dapat bersifat non-lokal, seperti motion blur.
- repetitive texture bisa diperbaiki dengan informasi konteks.
- transformer menyediakan receptive field global tanpa menumpuk banyak layer.

Keterbatasan:
- kompleksitas kuadratik terhadap jumlah token.
- perlu desain window atau efficient attention.
- pretraining dan data tetap penting.

Catatan: sesuai Pertemuan 03, pilihan arsitektur harus disesuaikan dengan domain, bukan sekadar tren.

---

# Slide 16 - Super-Resolution: Dari Interpolasi ke Generative Models

## Klasik
- nearest, bilinear, bicubic
- cepat dan stabil
- hasil cenderung halus, kehilangan tekstur

## Deep Learning
- mempelajari mapping low-resolution ke high-resolution
- menghasilkan detail tajam
- rentan halusinasi pada skala tinggi

## Generative / Diffusion
- sampling dari distribusi high-resolution bersyarat
- detail tekstur natural
- banyak solusi plausible

| Metode | Fidelity | Perceptual | Biaya |
|---|---|---|---|
| Bicubic | rendah-sedang | rendah | sangat rendah |
| CNN SR | tinggi | sedang | rendah |
| Diffusion SR | sedang | tinggi | tinggi |

---

# Slide 17 - Fidelity vs Perceptual Quality

**Fidelity** = seberapa dekat hasil dengan ground truth pada level pixel.
**Perceptual quality** = seberapa natural hasil menurut persepsi manusia.

Keduanya sering bertentangan:
- optimasi PSNR menghasilkan citra **over-smoothed**.
- optimasi perceptual/GAN menghasilkan citra **tajam tetapi tidak selalu cocok dengan ground truth**.

```
        fidelity tinggi                 perceptual tinggi
  over-smooth <-----------------------> detail natural
                          trade-off
```

Untuk riset S3, penting menentukan **posisi** metode Anda pada trade-off ini dan membuktikannya dengan metrik ganda.

---

# Slide 18 - PSNR: Definisi dan Keterbatasan

## Definisi

```
MSE = (1/N) Σ (x_i - x_hat_i)²
PSNR = 10 log10 (MAX² / MSE)
```

- `MAX`: rentang nilai pixel, misalnya 255 atau 1.0

## Kelebihan

- sederhana, deterministik, banyak digunakan.

## Keterbatasan

- tidak berkorelasi kuat dengan persepsi manusia.
- menggunakan rata-rata global; lokasi kesalahan tidak dipertimbangkan.
- tidak menangkap struktur, tekstur, atau kontras lokal.

Kesimpulan: PSNR perlu dilengkapi metrik lain, bukan dihapus.

---

# Slide 19 - SSIM: Structural Similarity

SSIM membandingkan tiga komponen lokal:

```
SSIM = f(luminance, contrast, structure)
```

- dihitung pada window lokal
- rentang -1 sampai 1, semakin tinggi semakin mirip

## Kelebihan

- lebih dekat ke persepsi daripada MSE.
- sensitif terhadap perubahan struktur, edge, dan kontras.

## Keterbatasan

- tetap tidak menangkap tekstur tingkat lanjut.
- dapat terlihat baik untuk hasil yang over-smoothed.
- perlu `data_range` dan parameter window yang dilaporkan agar reproducible.

Implementasi praktis ada pada slide praktikum.

---

# Slide 20 - Perceptual Metrics Modern

| Metrik | Jenis | Konsep | Kegunaan |
|---|---|---|---|
| LPIPS | learned, full-reference | jarak fitur deep network | menilai kemiripan perseptual |
| DISTS | learned, full-reference | struktur dan tekstur | texture fidelity |
| FID | distribution-based | kesesuaian distribusi fitur | evaluasi level kumpulan |
| NIQE | no-reference | statistik natural scene | kualitas tanpa ground truth |
| MUSIQ / NIMA | learned, no-reference | prediksi opini manusia | penilaian kualitas |

Catatan:
- metrik learned memakai jaringan pretrained; hasil bergantung pada representasi.
- bedakan evaluasi per-citra (PSNR/SSIM/LPIPS) dan per-kumpulan (FID).
- gunakan metrik yang sesuai dengan domain aplikasi.

---

# Slide 21 - Studi Kasus: PSNR Tinggi vs Kualitas Visual

## Skenario

- Metode A: PSNR 31.2 dB, SSIM 0.92, tetapi permukaan tekstur menjadi "plastik".
- Metode B: PSNR 29.8 dB, SSIM 0.89, tetapi tekstur lebih natural dan tajam.

Jika hanya membaca tabel PSNR, A dianggap lebih baik. Secara visual, B sering lebih disukai manusia.

## Pertanyaan Diskusi

1. Metode mana yang memulihkan informasi asli?
2. Detail mana yang benar-benar ada di ground truth?
3. Untuk aplikasi Anda, kesalahan seperti apa yang paling mahal?
4. Apakah metrik tambahan seperti LPIPS atau human study mengubah kesimpulan?

Ini adalah pola critical review yang harus diterapkan pada paper restoration.

---

# Slide 22 - Hallucinated Detail: Memulihkan atau Menciptakan?

**Hallucinated detail** = struktur atau tekstur pada hasil yang tidak memiliki padanan di citra asli, tetapi tampak meyakinkan.

Contoh:
- super-resolution menambahkan pori-pori kulit yang tidak ada di data asli.
- deblurring menambahkan tepi yang sebenarnya bukan bagian objek.
- diffusion menghasilkan tekstur baru yang plausible.

## Mengapa Berbahaya?

- di bidang medis atau forensik, detail palsu bisa menyesatkan keputusan.
- metrik seperti PSNR/SSIM tidak dapat mendeteksi halusinasi.

## Cara Mitigasi

- laporkan uncertainty.
- lakukan inspeksi visual pada region sulit.
- evaluasi dampak pada tugas hilir, misalnya klasifikasi atau segmentasi.

---

# Slide 23 - Degradasi Sintetis vs Degradasi Nyata

| Aspek | Sintetis | Nyata |
|---|---|---|
| Ground truth | tersedia | jarang tersedia |
| Kontrol parameter | penuh | tidak diketahui |
| Reproducibility | tinggi | rendah |
| Relevansi aplikasi | sedang | tinggi |

## Masalah Utama

- model yang dilatih dengan degradasi sintetis sering gagal pada degradasi nyata.
- parameter degradasi seperti blur kernel dan noise level tidak diketahui saat inferensi.

## Arah Solusi

- blind restoration: estimasi degradasi dan citra bersih bersama-sama.
- self-supervised / zero-shot restoration.
- synthetic-to-real transfer.

---

# Slide 24 - Menuju Model Degradasi Realistis

Membuat degradasi sintetis yang lebih realistis:

- gunakan kombinasi blur: Gaussian, motion, defocus.
- gunakan beberapa jenis noise: Gaussian, Poisson, speckle.
- tambahkan kompresi JPEG, sensor artifact, demosaicing error.
- variasi parameter secara acak pada setiap sampel.

Contoh pipeline sederhana:

```
clean -> blur -> downscale -> noise -> JPEG -> degraded
```

Prinsip penting:
- kecocokan antara degradasi training dan target deployment menentukan performa.
- melaporkan model degradasi secara eksplisit adalah bagian dari protokol evaluasi.

---

# Slide 25 - Diffusion-Based Restoration

Diffusion model belajar **distribusi citra bersih** melalui proses denoising bertahap.

Untuk restoration:
- diinginkan sampel dari `p(x | y)`, bukan hanya satu estimasi point.
- diffusion menyediakan prior kuat dan multimodal.

Keunggulan:
- detail natural dan beragam.
- dapat menangani berbagai inverse problem dengan conditioning yang sama.

Tantangan:
- komputasi berat.
- ketidakpastian sampling.
- risiko halusinasi tinggi.
- perlu tuning guidance untuk menjaga fidelity.

---

# Slide 26 - Prinsip Kerja Conditional Diffusion

Konsep dua proses:

```
Forward:  x_0 = x  ->  x_1 -> ... -> x_T (noise)
Reverse:  x_T (noise) -> ... -> x_1 -> x_0 (estimate)
```

Kondisi `y` disuntikkan pada reverse process:

```
x_{t-1} = denoise(x_t, y, t)
```

Cara conditioning:
- concat `y` dengan `x_t`
- feature injection pada encoder-decoder
- guidance berbasis gradien dari data fidelity

Catatan: restoration generatif mencari keseimbangan antara **plausibilitas prior** dan **kesetiaan pada `y`**.

---

# Slide 27 - Workflow Diffusion Restoration

```
Input y
   |
   v
Encode degradasi / conditioning
   |
   v
Iterative reverse sampling (T steps)
   |
   v
Beberapa sampel x_hat
   |
   v
Aggregate / pilih / ukur uncertainty
```

- guidance strength tinggi -> fidelity naik, diversity turun.
- guidance strength rendah -> detail lebih bervariasi, risiko tidak cocok dengan `y`.

Dalam praktikum, bandingkan satu hasil deterministik seperti CNN dengan beberapa sampel diffusion untuk melihat trade-off.

---

# Slide 28 - Uncertainty dalam Inverse Problem

Karena ill-posed, solusi tidak tunggal. Uncertainty muncul dari:
- noise pada pengukuran
- informasi yang hilang, misalnya frekuensi tinggi
- prior yang tidak pasti

## Cara Mengukur

- generate `M` sampel dari model generatif.
- hitung mean dan variance per pixel.
- variance tinggi = region tidak pasti.

## Contoh Visualisasi

| Region | Variance | Interpretasi |
|---|---|---|
| Flat area | rendah | solusi stabil |
| Texture kompleks | tinggi | banyak solusi plausible |
| Edge | sedang | struktur penting |

Untuk aplikasi kritis, sajikan uncertainty map bersama hasil restorasi.

---

# Slide 29 - Merancang Evaluasi yang Adil

Prinsip:
- gunakan **banyak metrik** yang saling melengkapi.
- pilih **baseline** yang relevan dan sebanding.
- lakukan **ablation** untuk membuktikan kontribusi.
- laporkan **variabilitas** dari seed, split, dan inisialisasi.
- **inspeksi visual** tidak boleh dilewatkan.

## Checklist Sederhana

- [ ] Degradasi didefinisikan eksplisit
- [ ] Dataset dan split dilaporkan
- [ ] Baseline setara dalam data, komputasi, dan training budget
- [ ] Metrik minimal tiga: fidelity, struktural, perceptual
- [ ] Contoh visual ditampilkan
- [ ] Keterbatasan dan risiko halusinasi dibahas

---

# Slide 30 - Workflow Praktikum 06

```
1. Pilih citra bersih
2. Buat degradasi sintetis
3. Jalankan restoration baseline
4. Hitung PSNR, SSIM, LPIPS
5. Inspeksi visual terstruktur
6. Tulis analisis trade-off
```

## Bahan

- Python, NumPy, SciPy, scikit-image, Matplotlib
- jika tersedia, model deep/diffusion sederhana
- format laporan: notebook, tabel, visualisasi

## Keluaran

- tabel perbandingan metrik antar baseline
- montase citra
- paragraf analisis fidelity vs perceptual

---

# Slide 31 - Membuat Degradasi Sintetis di Python

```python
import numpy as np
from scipy.ndimage import gaussian_filter
from skimage import util
from skimage.transform import resize

## x: clean image, float, rentang [0, 1]
x = ...

## 1. Additive Gaussian noise
noisy = util.random_noise(x, mode='gaussian', var=0.01)

## 2. Gaussian blur
blurred = gaussian_filter(x, sigma=1.5)

## 3. Downscale + upscale (simulasi low-res)
h, w = x.shape[:2]
low = resize(x, (h // 2, w // 2), anti_aliasing=True)
degraded = resize(low, (h, w), anti_aliasing=True)
```

Simpan parameter degradasi untuk setiap percobaan.

---

# Slide 32 - Implementasi PSNR dan SSIM di Python

```python
from skimage.metrics import peak_signal_noise_ratio as psnr
from skimage.metrics import structural_similarity as ssim

## x: clean, x_hat: hasil restorasi
p = psnr(x, x_hat, data_range=1.0)
s = ssim(x, x_hat, data_range=1.0, channel_axis=-1)

print(f"PSNR: {p:.2f} dB")
print(f"SSIM: {s:.4f}")
```

Catatan:
- pastikan `x` dan `x_hat` memiliki rentang dan tipe data yang sama.
- gunakan `channel_axis` untuk citra berwarna.
- untuk LPIPS dan FID, gunakan library atau implementasi yang sesuai.

---

# Slide 33 - Restoration Baseline

Baseline yang wajar:

| Baseline | Tugas | Sifat |
|---|---|---|
| Bicubic interpolation | super-resolution | sederhana, sering dipakai |
| Median filter | denoising | mengurangi noise impulsif |
| Gaussian filter | denoising | smoothing |
| Wiener filter | deblurring | linear, perlu estimasi kernel |
| Richardson-Lucy | deblurring | iteratif, butuh kernel |

Mengapa baseline penting:
- membuktikan bahwa metode baru memberi peningkatan nyata.
- menetapkan kualitas minimum dan biaya komputasi.
- menghindari klaim berlebihan pada dataset yang mudah.

---

# Slide 34 - Menjalankan Baseline dan Mencatat Hasil

Contoh tabel hasil:

| Method | PSNR (dB) | SSIM | LPIPS | Runtime (s) |
|---|---|---|---|---|
| Degraded | 24.10 | 0.72 | 0.45 | - |
| Bicubic | 26.40 | 0.81 | 0.33 | 0.01 |
| Wiener | 27.20 | 0.83 | 0.31 | 0.05 |
| CNN baseline | 29.80 | 0.88 | 0.21 | 1.20 |
| Metode Anda | 30.10 | 0.89 | 0.19 | 1.50 |

Laporkan konfigurasi:
- jumlah seed
- versi library
- GPU/CPU
- hyperparameter

---

# Slide 35 - Analisis Visual Terstruktur

Jangan puas dengan angka.

## Prosedur

- buat grid: original, degraded, baseline, hasil.
- pilih region yang diperbesar pada:
  - tekstur
  - edge / kontur
  - area flat
  - area dengan detail kecil

| Region | Hasil baseline | Hasil metode | Catatan |
|---|---|---|---|
| Rambut | halus | tajam | detail mungkin halusinasi |
| Latar flat | bersih | noise kecil | perlu cek |
| Tepi objek | ringing | bersih | lebih baik |

Diskusikan: apakah perbaikan visual nyata atau hanya efek sharpening?

---

# Slide 36 - Trade-off: Fidelity, Perceptual Quality, Computational Cost

| Pendekatan | Fidelity | Perceptual | Cost |
|---|---|---|---|
| Filter klasik | sedang | rendah | sangat rendah |
| CNN / transformer | tinggi | sedang | rendah-sedang |
| Diffusion | sedang | tinggi | tinggi |

## Implikasi untuk Penelitian

- tidak ada metode yang unggul di semua sumbu.
- pilih titik operasi sesuai kebutuhan aplikasi.
- laporkan trade-off secara eksplisit.

## Contoh untuk Proposal Disertasi

- domain medis: prioritaskan fidelity dan uncertainty.
- domain fotografi: prioritaskan perceptual dan efisiensi.
- domain real-time: prioritaskan cost dan robustness.

---

# Slide 37 - Reproducibility dan Benchmarking

- gunakan dataset benchmark yang lazim, misalnya Set5, Set14, BSD100, Urban100, DIV2K.
- tuliskan skema degradasi yang digunakan.
- laporkan semua hyperparameter dan random seed.
- sertakan kode dan instruksi menjalankan eksperimen.
- hindari cherry-picking pada visual.

Reproducibility adalah bagian dari protokol evaluasi restoration. Prinsip ini akan diperdalam pada Pertemuan 12 tentang experimental design.

---

# Slide 38 - Riset Gap yang Bisa Dieksplorasi

Beberapa celah untuk kajian S3:

- **real-world degradation**: model degradasi apa yang paling tepat untuk domain tertentu?
- **blind restoration**: estimasi kernel dan noise secara bersama tanpa ground truth.
- **uncertainty-aware restoration**: memberikan peta kepercayaan pada hasil.
- **hallucination detection**: membedakan detail asli dan detail buatan.
- **task-driven evaluation**: mengukur dampak restoration pada deteksi atau segmentasi.
- **efficient diffusion restoration**: mengurangi biaya sampling untuk aplikasi praktis.

Pilih satu gap, lalu formulasikan research question yang dapat diuji.

---

# Slide 39 - Rangkuman dan Pesan Kunci

- image restoration adalah **inverse problem**; solusi tidak unik dan butuh prior.
- **forward model** menentukan apa yang diketahui dan apa yang hilang.
- **PSNR/SSIM** penting tetapi tidak cukup; tambahkan metrik perseptual dan inspeksi visual.
- model dapat **memulihkan** atau **menciptakan** detail; pahami bedanya.
- **degradasi sintetis** harus dirancang agar relevan dengan target nyata.
- **diffusion model** menawarkan prior kuat dan uncertainty, tetapi mahal serta berisiko halusinasi.
- evaluasi harus **multi-metrik, fair baseline, dan reproducible**.

---

# Slide 40 - Kaitan dengan Pertemuan Berikutnya

Pertemuan 07 akan membahas **Object Detection Modern dengan YOLO dan Transformer**.

Keterkaitan:
- kualitas citra memengaruhi deteksi; restoration dapat menjadi preprocessing.
- metodologi evaluasi seperti mAP, confusion analysis, dan robustness melanjutkan prinsip evaluasi multi-metrik.
- pertanyaan "apakah peningkatan metrik berarti peningkatan kinerja nyata?" muncul kembali pada deteksi.

Selain itu, diffusion-based restoration akan terhubung dengan Generative Vision pada Pertemuan 09.

---

# Slide 41 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Object Detection Modern dengan YOLO dan Transformer**