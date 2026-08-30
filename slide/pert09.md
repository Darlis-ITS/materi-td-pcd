# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 09

## Generative Vision dengan Diffusion Models

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 09 dalam Rangkaian Perkuliahan

## Konteks Mata Kuliah

- **Sebelumnya (Pertemuan 08):** Segmentasi Citra dan Promptable Foundation Models — fokus pada SAM, kualitas mask, dan prompt strategy.
- **Saat ini (Pertemuan 09):** Generative Vision dengan Diffusion Models — fokus pada mekanisme generasi, conditioning, synthetic data, dan risiko generatif.
- **Berikutnya (Pertemuan 10):** 3D Vision, Multi-View Geometry, dan Neural Rendering — membahas representasi 3D dan rekonstruksi dari citra.

## Peta RPS

| Pertemuan | Tema Besar | Keterkaitan dengan Pertemuan 09 |
|---|---|---|
| 06 | Image Restoration | Diffusion-based restoration menjadi jembatan menuju generative vision |
| 07 | Object Detection | Synthetic data dari diffusion dapat digunakan untuk augmentasi deteksi |
| 08 | Segmentasi dan Foundation Model | Promptable model dan generative model berbagi fondasi representasi visual |
| 09 | Generative Vision | Fokus pertemuan ini |
| 10 | 3D Vision dan Neural Rendering | Diffusion dapat digabungkan dengan representasi 3D untuk novel view synthesis |

---

# Slide 02 - Tujuan Pembelajaran dan Capaian Terkait

## Tujuan Pembelajaran

1. Memahami prinsip generasi berbasis noise schedule, forward diffusion, dan reverse denoising.
2. Menjelaskan peran conditioning dalam text-to-image, image-to-image, dan inpainting.
3. Membandingkan GAN dengan diffusion model dari sisi stabilitas latihan, mode coverage, dan kualitas sampel.
4. Mengevaluasi potensi dan risiko synthetic data: bias, memorization, hallucination, dan provenance.

## Capaian Pembelajaran Mata Kuliah

- **CPMK-1:** Menganalisis paradigma dan tantangan riset generative vision secara kritis.
- **CPMK-3:** Merancang eksperimen komputasional untuk evaluasi generasi citra.
- **CPMK-4:** Memanfaatkan pipeline Diffusers untuk eksperimen awal.

---

# Slide 03 - Agenda Pertemuan

## Alur Kuliah

1. Dari discriminative menuju generative vision
2. Konsep dasar generative model: likelihood, latent variable, dan distribusi data
3. GAN: generator dan discriminator
4. Diffusi: forward noising dan reverse denoising
5. Latent diffusion dan arsitektur U-Net
6. Conditioning: teks, citra, dan mask
7. Guidance: classifier-free guidance
8. Aplikasi: text-to-image, image-to-image, inpainting
9. Synthetic data dan diffusion-based augmentation
10. Risiko: bias, memorization, hallucination, provenance
11. Evaluasi kualitas dan keragaman
12. Praktikum: pipeline Diffusers dan prompt variation
13. Critical review dan diskusi penelitian

---

# Slide 04 - Pertanyaan Kunci Pertemuan Ini

## Pertanyaan yang Akan Diuji

| No | Pertanyaan Kunci | Relevansi Penelitian |
|---|---|---|
| 1 | Apakah citra sintetis meningkatkan generalisasi model atau justru memperkuat bias? | Penggunaan synthetic data dalam training |
| 2 | Bagaimana kualitas dan keragaman hasil generasi diukur secara valid? | Desain evaluasi generative vision |
| 3 | Bagaimana provenance data sintetis didokumentasikan? | Reproducibility dan etika riset |
| 4 | Apa perbedaan mendasar GAN dengan diffusion model dalam hal mode coverage? | Pemilihan model generatif |
| 5 | Apakah model diffusion menghafal data latih? | Risiko memorization dan privacy |

## Target Keluaran

Katalog eksperimen generatif dengan analisis konsistensi, keragaman, keterbatasan, dan potensi penggunaan.

---

# Slide 05 - Dari Diskriminatif ke Generatif: Perubahan Pertanyaan Penelitian

## Model Diskriminatif (Pertemuan 1-8)

- Mempelajari batas keputusan atau pemetaan dari citra ke label.
- Contoh: klasifikasi, deteksi, segmentasi.
- Pertanyaan: *Apa label atau struktur dari citra ini?*

## Model Generatif

- Mempelajari distribusi data p(x), atau distribusi bersyarat p(x|c).
- Contoh: GAN, diffusion, text-to-image.
- Pertanyaan: *Bagaimana menghasilkan citra baru yang sesuai dengan data atau kondisi tertentu?*

## Implikasi untuk Penelitian Doktoral

- Diskriminatif: kuat pada tugas tertutup dengan label.
- Generatif: membuka kemungkinan data baru, tetapi menuntut evaluasi yang hati-hati.

---

# Slide 06 - Peta Konsep Generative Vision

## Diagram Alur Konsep

```
Data latih x0
    |
    v
Forward diffusion: x0 -> x1 -> ... -> xT
    (menambahkan noise secara bertahap)
    |
    v
Reverse denoising: xT -> ... -> x1 -> x0_hat
    (belajar menghilangkan noise)
    |
    v
Conditioning: teks, mask, citra referensi
    |
    v
Output: citra baru, inpainted region, edited image
```

## Cakupan Materi

- Representasi laten
- Noise schedule
- Denoising process
- Guidance
- Evaluasi

---

# Slide 07 - Definisi Generative Model

## Gagasan Dasar

Model generatif mempelajari distribusi probabilitas data:

- p(x): distribusi data tanpa kondisi.
- p(x|c): distribusi data dengan kondisi c.

## Tujuan Pembelajaran

1. **Sampling:** menghasilkan sampel baru yang realistis.
2. **Likelihood:** mengukur seberapa mungkin data tertentu muncul.
3. **Representasi:** mempelajari struktur laten data.

## Contoh Distribusi Data Citra

- Citra wajah: p(x) terkonsentrasi pada manifold wajah manusia.
- Citra medis: p(x) terkonsentrasi pada pola anatomi tertentu.
- Citra satelit: p(x) terkonsentrasi pada struktur geografis.

---

# Slide 08 - Klasifikasi Keluarga Generative Model

## Taksonomi Sederhana

| Keluarga | Contoh | Sampling | Kelebihan | Keterbatasan |
|---|---|---|---|---|
| Autoregressive | PixelCNN, Transformer | Seq | Eksplisit likelihood | Lambat, panjang korelasi |
| Variational | VAE | Laten -> data | Representasi laten | Sampel cenderung blur |
| Implicit | GAN | Laten -> data | Sampel tajam | Mode collapse, sulit stabil |
| Diffusion | DDPM, Stable Diffusion | Noise -> data | Mode coverage baik | Iteratif, komputasi tinggi |

## Posisi Diffusion

Diffusion berada pada keluarga yang memodelkan proses penghilangan noise secara bertahap, menggabungkan kelebihan likelihood dan kualitas sampel.

---

# Slide 09 - Recap Singkat GAN

## Komponen GAN

- **Generator G(z):** memetakan vektor laten z menjadi citra.
- **Discriminator D(x):** membedakan citra asli dan citra buatan.

## Proses Latihan

- Generator berusaha menipu discriminator.
- Discriminator berusaha tidak tertipu.
- Solusi: keseimbangan Nash (secara teoretis).

## Masalah Utama

- Mode collapse: generator hanya menghasilkan sebagian variasi data.
- Ketidakstabilan latihan: keseimbangan sulit dicapai.
- Evaluasi: tidak ada likelihood yang dapat dihitung langsung.

---

# Slide 10 - Mengapa Diffusion Models? Perbedaan dengan GAN

## Perbandingan Konseptual

| Aspek | GAN | Diffusion Model |
|---|---|---|
| Mekanisme | Adversarial | Denoising bertahap |
| Stabilitas latihan | Rentan collapse | Lebih stabil |
| Mode coverage | Sering tidak lengkap | Lebih baik |
| Kualitas sampel | Tajam, kadang tidak realistis | Mulai tajam, kokoh |
| Komputasi | Cepat saat sampling | Lambat karena iteratif |
| Likelihood | Tidak eksplisit | Variational lower bound |
| Conditioning | Umumnya perlu arsitektur tambahan | Natural melalui guidance |

## Konsekuensi Penelitian

- Diffusion unggul pada keragaman.
- GAN unggul pada kecepatan sampling.
- Penelitian dapat menggabungkan keduaya.

---

# Slide 11 - Forward Diffusion: Menambahkan Noise Secara Bertahap

## Gagasan

Mulai dari citra bersih x0, lalu tambahkan noise Gaussian kecil pada setiap langkah:

- x1 = sqrt(1-beta1) x0 + sqrt(beta1) epsilon1
- x2 = sqrt(1-beta2) x1 + sqrt(beta2) epsilon2
- ...
- xT mendekati distribusi Gaussian murni.

## Noise Schedule

- beta_t: varians noise pada langkah ke-t.
- Nama lain: variance schedule.

## Sifat Penting

- Proses ini markovian dan dapat ditulis dalam bentuk tertutup.
- x_t dapat dihitung langsung dari x0 tanpa simulasi bertahap.

---

# Slide 12 - Matematika Dasar Forward Diffusion

## Definisi

q(x_t | x_{t-1}) = N(x_t; sqrt(1-beta_t) x_{t-1}, beta_t I)

## Bentuk Tertutup

Misalkan alpha_t = 1 - beta_t dan alpha_bar_t = produk dari alpha_1 sampai alpha_t, maka:

q(x_t | x_0) = N(x_t; sqrt(alpha_bar_t) x_0, (1 - alpha_bar_t) I)

## Interpretasi

- Saat t kecil, x_t masih mirip x0.
- Saat t besar, x_t mendekati noise murni.
- Noise schedule menentukan laju transisi.

---

# Slide 13 - Reverse Denoising: Belajar Menghilangkan Noise

## Gagasan

Jika kita mengetahui distribusi q(x_{t-1} | x_t, x0), kita dapat mundur dari xT menuju x0.

## Masalah

- Distribusi q(x_{t-1} | x_t) tidak diketahui secara umum.
- Kita belajar model p_theta(x_{t-1} | x_t) untuk mengaproksimasinya.

## Bentuk Model

p_theta(x_{t-1} | x_t) = N(x_{t-1}; mu_theta(x_t, t), Sigma_theta(x_t, t))

## Intuisi

- Model diminta memprediksi noise yang harus dihilangkan.
- Proses berulang T kali menghasilkan citra bersih.

---

# Slide 14 - Denoising Diffusion Probabilistic Models (DDPM)

## Kontribusi Utama DDPM

- Ho et al., 2020.
- Menunjukkan bahwa training dapat disederhanakan dengan memprediksi noise epsilon.
- Menggunakan U-Net sebagai arsitektur.

## Objective Training (Sederhana)

L = E_{t, x0, epsilon} [ || epsilon - epsilon_theta(x_t, t) ||^2 ]

## Interpretasi

Model epsilon_theta belajar memprediksi noise yang ditambahkan pada langkah t, sehingga proses denoising menjadi akurat.

---

# Slide 15 - U-Net sebagai Backbone Denoising

## Mengapa U-Net?

- Arsitektur encoder-decoder dengan skip connection.
- Dapat menangkap detail lokal dan konteks global.
- Telah terbukti kuat pada segmentasi (Pertemuan 08).

## Struktur U-Net dalam Diffusion

- Encoder: mengecilkan resolusi, menambah channel.
- Bottleneck: representasi paling padat.
- Decoder: mengembalikan resolusi.
- Skip connection: menjaga detail halus.

## Peran Time Embedding

- Model menerima informasi t (langkah noise).
- t di-embedding dan disuntikkan ke setiap blok.

---

# Slide 16 - Peran Time Embedding

## Mengapa Model Perlu Tahu t?

- Pada t kecil, noise sedikit, model harus bekerja halus.
- Pada t besar, noise banyak, model harus lebih agresif menghilangkan noise.
- Informasi t membantu model menyesuaikan perilaku.

## Implementasi

- t diubah menjadi vektor melalui sinusoidal embedding.
- Vektor ditambahkan pada embedding pada tiap blok.

## Implikasi

- Arsitektur dan conditioning yang baik sangat menentukan kualitas denoising.

---

# Slide 17 - Latent Diffusion Models: Motivasi

## Masalah Diffusi pada Citra Resolusi Tinggi

- Denoising dalam ruang piksel membutuhkan komputasi besar.
- U-Net pada resolusi tinggi lambat dan memori besar.
- Iterasi T kali membuat biaya semakin tinggi.

## Gagasan Latent Diffusion

- Kompresi citra ke ruang laten berdimensi lebih rendah.
- Lakukan proses difusi di ruang laten.
- Dekode hasil laten menjadi citra.

## Keuntungan

- Komputasi lebih efisien.
- Fokus pada informasi semantik, bukan detail piksel yang tidak perlu.

---

# Slide 18 - Arsitektur Latent Diffusion Model (LDM)

## Komponen Utama

1. **Encoder E:** memetakan citra x ke representasi laten z.
2. **Decoder D:** memetakan laten z kembali ke citra.
3. **Diffusion U-Net:** bekerja pada ruang laten.
4. **Kondisioning (mis. teks):** dimasukkan melalui cross-attention.

## Alur

```
x -> E(z) -> [diffusion in latent] -> z_hat -> D(x_hat)
```

## Contoh Stabil

- Stable Diffusion menggunakan VAE sebagai encoder-decoder.
- U-Net bekerja pada laten 64x64 atau sejenisnya.

---

# Slide 19 - Peran VAE dalam Latent Diffusion

## VAE sebagai Kompresor

- VAE mempelajari representasi laten yang padat.
- Encoder memampatkan citra.
- Decoder merekonstruksi citra dari laten.
- Kualitas rekonstruksi menentukan batas atas kualitas akhir.

## Hubungan dengan Pertemuan Ini

- LDM tidak melakukan difusi pada piksel, tetapi pada laten VAE.
- VAE memberi ruang representasi yang lebih sederhana.

## Implikasi

- Kualitas dan keragaman sampel dibatasi decoder VAE.
- Fine-tuning VAE dapat meningkatkan kualitas.

---

# Slide 20 - Conditioning: Membawa Informasi Eksternal

## Bentuk Kondisi

- **Teks:** deskripsi semantik.
- **Citra:** referensi visual.
- **Mask:** region inpaint.
- **Segitiga/label:** informasi kelas.

## Cara Memasukkan Kondisi

1. **Concatenation:** kondisi digabung dengan input pada kanal atau ruang laten.
2. **Cross-Attention:** kondisi diubah menjadi key/value untuk query dari U-Net.
3. **AdaIN/Modul Adaptif:** injeksi melalui normalisasi.

## Mengapa Penting

- Kondisi menentukan kendali pengguna terhadap hasil generasi.

---

# Slide 21 - Text-to-Image dengan Cross-Attention

## Alur Text-to-Image

- Teks diproses oleh text encoder (misal CLIP).
- Representasi teks menjadi kondisi c.
- U-Net menerima c melalui cross-attention setiap bloknya.

## Ilustrasi Alur

```
Text -> text encoder -> embedding teks
                            |
Citra laten -> U-Net -> cross-attention -> denoised latent
                            |
                         VAE decoder -> citra
```

## Mengapa CLIP

- Representasi teks-gambar yang sudah diselaraskan.
- Memungkinkan pemahaman semantik yang baik.

---

# Slide 22 - Image-to-Image dan Inpainting

## Image-to-Image

- Kondisi berupa citra sumber.
- Model mengubah citra sesuai instruksi atau gaya.
- Contoh: sketch to image, style transfer, restorasi.

## Inpainting

- Mask menunjukkan region yang boleh diisi.
- Model mengisi region tersebut dengan konten baru yang konsisten.
- Aplikasi: menghapus objek, restorasi foto rusak.

## Prosedur pada Diffusers

- Gabungkan citra input + mask pada ruang laten.
- Jalankan denoising pada region yang terkena mask.
- Pertahankan region lain agar tetap utuh.

---

# Slide 23 - Guidance: Mengarahkan Proses Denoising

## Masalah

- Tanpa guidance, hasil generasi sering tidak sesuai keinginan.
- Model menghasilkan sampel acak dari distribusi.

## Jenis Guidance

1. **Classifier Guidance**
   - Menggunakan classifier yang membedakan kelas.
   - Gradien classifier memandu denoising.
   - Membutuhkan classifier terpisah.

2. **Classifier-Free Guidance**
   - Melatih model dengan dan tanpa kondisi.
   - Saat sampling, interpolasi antara prediksi bersyarat dan tanpa kondisi.
   - Lebih sederhana dan efektif.

---

# Slide 24 - Classifier-Free Guidance: Mekanisme

## Gagasan Inti

- Latih model untuk memprediksi noise dalam dua mode:
  - Dengan kondisi: epsilon_theta(x_t, t, c)
  - Tanpa kondisi: epsilon_theta(x_t, t, empty)
- Saat sampling, gunakan kombinasi:

epsilon_total = epsilon_uncond + w * (epsilon_cond - epsilon_uncond)

## Interpretasi

- w = 0: tanpa kondisi.
- w > 0: semakin spesifik terhadap kondisi.
- w terlalu besar: kualitas turun, beragam menurun.

## Peran w pada Penelitian

- Menentukan trade-off antara kesesuaian teks dan keragaman.

---

# Slide 25 - Noise Schedule dan Sampling

## Noise Schedule

- Linear schedule, cosine schedule, sqrt schedule.
- Mempengaruhi kecepatan transisi menuju noise.
- Berdampak pada stabilitas training dan kualitas sampling.

## Denoising Sampling

- DDPM sampling: T langkah bertahap.
- DDIM sampling: mempercepat dengan langkah lebih sedikit.
- Scheduler lain: Euler, DPM-Solver.

## Implikasi

- Scheduler adalah hyperparameter penting.
- Eksperimen harus mencantumkan scheduler dan jumlah langkah.

---

# Slide 26 - Integrasi dengan Pertemuan Sebelumnya: Restoration dan Augmentasi

## Diffusion untuk Image Restoration (Pertemuan 06)

- Denoising, deblurring, super-resolution dapat dirumuskan sebagai reverse diffusion dengan kondisi citra rusak.
- Hasil perseptual cenderung lebih baik daripada baseline MSE.
- Risiko: detail halusinasi bukan asli.

## Diffusion-based Augmentation

- Hasilkan citra sintetis untuk memperbanyak data latih.
- Terkait erat dengan pertemuan 07 dan 08: deteksi, segmentasi.
- Perlu evaluasi apakah augmentasi meningkatkan atau menurunkan generalisasi.

---

# Slide 27 - Synthetic Data: Peluang dan Tantangan

## Peluang

- Mengatasi kelangkaan data berlabel.
- Membuat variasi yang tidak ada di dataset asli.
- Mengurangi biaya anotasi manual.
- Memungkinkan pengujian domain langka.

## Tantangan

- Bias pada data latih akan tercermin pada data sintetis.
- Risiko memorization: model menyalin sampel latih.
- Hallucination: detail yang tidak ada di realitas.
- Provenance sulit dilacak.

## Pertanyaan Penelitian

- Apakah peningkatan akurasi berasal dari generalisasi atau kebocoran data?
- Bagaimana memastikan synthetic data etis dan sah digunakan?

---

# Slide 28 - Bias pada Generative Vision

## Sumber Bias

- Dataset latih tidak seimbang.
- Representasi teks bias (misal kata "dokter" lebih sering dengan pria).
- Evaluasi yang hanya mengukur kecocokan prompt, bukan keadilan.

## Dampak

- Model menghasilkan citra yang memperkuat stereotip.
- Penggunaan synthetic data dapat menyalin bias ke downstream model.
- Risiko sosial dan hukum.

## Langkah Mitigasi

- Audit distribusi output pada berbagai kelompok.
- Dokumentasikan komposisi data latih.
- Laporkan bias pada model card.

---

# Slide 29 - Memorization: Model Menghafal Data Latih

## Definisi

- Model menghasilkan sampel yang sangat mirip dengan data latih tertentu.
- Dapat membocorkan data pribadi.
- Sering terjadi pada data langka atau outlier.

## Cara Mendeteksi

- Cari sampel yang identik atau hampir identik dengan data latih.
- Gunakan metrik kesamaan (misal LPIPS, FID vs data asli).
- Uji dengan prompt yang menargetkan entitas spesifik.

## Implikasi Etis

- Model generatif publik dapat melanggar privasi.
- Harus ada protokol penggunaan data yang jelas.

---

# Slide 30 - Hallucination pada Citra Sintetis

## Definisi

- Model menghasilkan detail yang tidak ada dalam realitas atau kondisi.
- Contoh: teks pada poster menjadi tidak terbaca, jari tangan salah jumlah, wajah aneh.

## Penyebab

- Inkonsistensi representasi pada data latih.
- Error pada cross-attention.
- Keterbatasan decoder.

## Dampak

- Citra sintetis tidak dapat dipercaya untuk domain kritis seperti medis atau forensik.
- Harus ada evaluasi fakta atau struktur.

---

# Slide 31 - Provenance Data dan Dokumentasi

## Mengapa Provenance Penting

- Penelitian harus dapat melacak asal data.
- Data sintetis membutuhkan keterangan label, generator, versi model, seed, dan parameter.
- Menghindari klaim yang tidak dapat diverifikasi.

## Informasi yang Harus Dicatat

| Item | Contoh |
|---|---|
| Model | Stable Diffusion v2.1 |
| Pipeline | Diffusers 0.27 |
| Prompt | "foto produk dengan latar putih" |
| Seed | 42 |
| Scheduler | DDIM, 50 steps |
| Tanggal | 2026-09-15 |
| Hardware/software | CUDA 12.1, Python 3.11 |

---

# Slide 32 - Evaluasi Kualitas: Metrik Kuantitatif

## Kebutuhan

- Tidak cukup hanya melihat visual.
- Evaluasi harus objektif dan reproducible.

## Metrik Umum

- **FID (Fréchet Inception Distance):** mengukur kesamaan distribusi sampel dengan data asli.
- **IS (Inception Score):** mengukur kualitas dan keragaman.
- **CLIP Score:** kesesuaian teks-gambar.
- **LPIPS:** kesamaan persepsi antar citra.
- **KID:** kernel inception distance.

## Keterbatasan

- Metrik bergantung pada ekstraktor fitur.
- Nilai FID yang kecil tidak menjamin visual bagus.
- Harus disertai inspeksi visual dan analisis kasus.

---

# Slide 33 - Evaluasi Keragaman dan Konsistensi

## Keragaman

- Seberapa bervariasi output untuk variasi seed?
- Jika semua output hampir sama, terjadi mode collapse atau deterministik.
- Dapat diukur dengan jarak antar sampel pada ruang fitur.

## Konsistensi

- Apakah output konsisten dengan prompt?
- Apakah gaya, objek, dan komposisi sesuai?
- Dapat diukur dengan CLIP score atau human evaluation.

## Desain Eksperimen

- Variasikan seed, prompt, guidance scale.
- Dokumentasikan setiap parameter.
- Gunakan pengulangan untuk estimasi varians.

---

# Slide 34 - Evaluasi Kualitatif Terstruktur

## Rubrik Penilaian Kualitatif

| Kriteria | Pertanyaan Evaluasi |
|---|---|
| Relevansi | Apakah citra sesuai prompt? |
| Realisme | Apakah citra terlihat natural? |
| Keragaman | Apakah hasil berbeda antar seed? |
| Detail | Apakah detail halus konsisten? |
| Artefak | Apakah ada distorsi, teks aneh, atau geometri salah? |

## Langkah

1. Buat grid output untuk setiap kondisi.
2. Evaluasi per kriteria dengan skor.
3. Identifikasi pola kegagalan.
4. Hubungkan dengan parameter eksperimen.

---

# Slide 35 - Praktikum: Pipeline Diffusers

## Tujuan Praktikum

- Menjalankan inferensi text-to-image dengan Hugging Face Diffusers.
- Melakukan prompt variation dan seed variation.
- Melakukan evaluasi kualitatif terstruktur.

## Code Snippet

```python
from diffusers import StableDiffusionPipeline
import torch

pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
)
pipe = pipe.to("cuda")

prompt = "landscape photograph, mountain lake at sunrise"
image = pipe(
    prompt,
    num_inference_steps=30,
    guidance_scale=7.5,
    seed=42
).images[0]
image.save("output.png")
```

---

# Slide 36 - Praktikum: Variasi Prompt dan Seed

## Protokol Eksperimen Minimal

| Kondisi | Prompt | Seed | Guidance Scale |
|---|---|---|---|
| 1 | "bendungan dengan latar pegunungan" | 1 | 7.5 |
| 2 | "bendungan dengan latar pegunungan" | 2 | 7.5 |
| 3 | "bendungan dengan latar kota" | 1 | 7.5 |
| 4 | "bendungan dengan latar kota" | 2 | 9.0 |
| 5 | "bendungan gaya isometrik" | 3 | 5.0 |

## Output yang Dicatat

- Gambar hasil.
- Seed dan prompt.
- Waktu inferensi.
- Skor kualitatif per kriteria.
- Catatan kegagalan.

---

# Slide 37 - Praktikum: Image-to-Image dan Inpainting

## Image-to-Image dengan Diffusers

```python
from diffusers import StableDiffusionImg2ImgPipeline
import requests
from PIL import Image

pipe = StableDiffusionImg2ImgPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5"
).to("cuda")

img = Image.open("sketch.png").convert("RGB")
result = pipe(
    prompt="foto realistis dari sketsa",
    image=img,
    strength=0.6,
    guidance_scale=7.5
).images[0]
result.save("output_img2img.png")
```

## Inpainting

- Gunakan pipeline `StableDiffusionInpaintPipeline`.
- Sediakan citra input dan mask.
- Tentukan strength sesuai kebutuhan.

---

# Slide 38 - Critical Review Paper: Pertanyaan Penuntun

## Struktur Review

1. Problem apa yang dipecahkan?
2. Apa kontribusi utama paper?
3. Bagaimana metode berbeda dari sebelumnya?
4. Apakah eksperimen memadai untuk klaim?
5. Apa keterbatasan dan risiko?
6. Apa pertanyaan penelitian lanjutan?

## Contoh Kasus Paper

- LDM / Stable Diffusion
- Perbandingan GAN vs Diffusion
- Evaluasi synthetic data pada benchmark
- Risiko memorization pada diffusion

---

# Slide 39 - Diskusi Etika dan Reproducibility

## Etika

- Hak cipta data latih.
- Konten yang menyesatkan (deepfake).
- Penggunaan citra tokoh publik.
- Dampak sosial dari citra sintetis.

## Reproducibility

- Simpan konfigurasi lengkap.
- Gunakan random seed.
- Catat versi library dan hardware.
- Bagikan script dan hasil sebagai artefak.

## Target

- Eksperimen yang dapat diulang oleh mahasiswa lain atau reviewer.

---

# Slide 40 - Hubungan dengan Pertemuan 10 dan Penelitian Disertasi

## Keterkaitan ke 3D Vision

- Diffusion dapat digunakan untuk novel view synthesis.
- Kondisi berupa pose kamera atau representasi 3D.
- Menjadi jembatan menuju NeRF pada pertemuan 10.

## Potensi Riset Generative Vision

- Synthetic data untuk domain langka.
- Conditioning yang lebih terkontrol.
- Evaluasi yang lebih baik dari sekadar FID.
- Mitigasi bias dan memorization.

## Refleksi untuk Disertasi

- Tentukan pertanyaan riset yang konkret.
- Rancang eksperimen yang memisahkan kontribusi teknis dari konfigurasi.
- Dokumentasikan semua keputusan.

---

# Slide 41 - Rangkuman Konsep Kunci

## Peta Ringkas

- Diffusion adalah proses noise bertahap dan denoising terbalik.
- LDM melakukan difusi di ruang laten melalui VAE.
- Conditioning dan guidance membuat output sesuai keinginan.
- Text-to-image, image-to-image, inpainting adalah tiga aplikasi inti.
- Evaluasi tidak cukup dengan satu metrik.
- Risiko: bias, memorization, hallucination, provenance.

## Yang Harus Diingat

- Synthetic data bukan pengganti data nyata tanpa audit.
- Setiap eksperimen harus reproducible.
- Penelitian generative vision membutuhkan desain evaluasi yang hati-hati.

---

# Slide 42 - Tugas dan Bukti Belajar

## Tugas Praktikum

- Menjalankan pipeline Diffusers pada satu model Stable Diffusion.
- Melakukan minimal 5 variasi prompt dan 3 variasi seed.
- Menyimpan output dalam katalog terstruktur.

## Tugas Analisis

- Evaluasi kualitatif setiap output menggunakan rubrik.
- Amati konsistensi, keragaman, kesesuaian prompt, dan artefak.
- Tuliskan kesimpulan tentang keterbatasan model.

## Bukti Belajar

- Kumpulkan notebook, output, dan laporan evaluasi.
- Sertakan tabel konfigurasi dan seed.

---

# Slide 43 - Checklist Sebelum Mengumpulkan Laporan

## Checklist

| No | Item | Status |
|---|---|---|
| 1 | Pipeline berhasil dijalankan | Ya / Tidak |
| 2 | Semua prompt dan seed dicatat | Ya / Tidak |
| 3 | Output disimpan dalam format PNG/JPG | Ya / Tidak |
| 4 | Rubrik evaluasi diisi | Ya / Tidak |
| 5 | Identifikasi pola kegagalan dituliskan | Ya / Tidak |
| 6 | Refleksi keterbatasan model dibuat | Ya / Tidak |

## Catatan

- Pastikan tidak ada prompt atau parameter yang hilang.
- Laporan harus dapat dipahami tanpa harus menjalankan ulang notebook.

---

# Slide 44 - Referensi dan Bacaan Lanjutan

## Paper Utama

- Ho et al., Denoising Diffusion Probabilistic Models
- Rombach et al., High-Resolution Image Synthesis with Latent Diffusion Models
- Goodfellow et al., Generative Adversarial Nets
- Song et al., Score-Based Generative Modeling through Stochastic Differential Equations (opsional)

## Dokumentasi

- Hugging Face Diffusers
- Model cards dan dokumentasi dataset
- Panduan evaluasi FID, CLIP Score

## Saran

- Baca kode sumber Diffusers untuk memahami pipeline.
- Bandingkan implementasi DDPM dan DDIM.
- Cari paper terbaru tentang synthetic data dan bias.

---

# Slide 45 - TERIMA KASIH

Pertemuan berikutnya

**3D Vision, Multi-View Geometry, dan Neural Rendering**