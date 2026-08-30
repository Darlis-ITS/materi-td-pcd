# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 10

## 3D Vision, Multi-View Geometry, dan Neural Rendering

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Posisi Pertemuan 10 dalam Rangkaian Perkuliahan

## Dari Representasi 2D Menuju Pemahaman 3D

- Pertemuan 1-8 membangun fondasi representasi visual, deteksi, segmentasi, dan pemahaman semantik pada citra 2D.
- Pertemuan 9 membahas generative vision dengan diffusion models, termasuk kemampuan model untuk menghasilkan citra baru.
- **Pertemuan 10** menggeser fokus: bagaimana informasi 3D dapat dipulihkan dari citra 2D dan bagaimana scene direkonstruksi secara geometris maupun neural.
- Pertemuan 11 akan membahas explainable, robust, dan trustworthy computer vision, yang relevan untuk mengevaluasi keandalan model 3D.

## Target Capaian Terkait

| CPMK | Fokus |
|---|---|
| CPMK-1 | Menganalisis paradigma 3D vision dan tantangan risetnya |
| CPMK-4 | Memanfaatkan tools untuk eksplorasi depth dan rekonstruksi sederhana |
| CPMK-5 | Merumuskan masalah penelitian 3D vision secara awal |

---

# Slide 02 - Tujuan Pembelajaran dan Target Keluaran

## Tujuan Pembelajaran

1. Memahami sumber informasi 3D dari citra 2D dan keterbatasannya.
2. Menjelaskan model kamera, epipolar geometry, stereo, dan depth estimation.
3. Membedakan representasi scene tradisional (point cloud, mesh) dengan representasi neural (NeRF).
4. Mengidentifikasi tantangan occlusion, sparse view, scale ambiguity, dan dynamic scene.

## Target Keluaran

- **Concept note** masalah 3D vision yang memuat:
  - data dan asumsi geometri,
  - baseline,
  - risiko teknis,
  - kemungkinan kontribusi.

---

# Slide 03 - Pertanyaan Kunci Pertemuan Ini

## Tiga Pertanyaan Utama

| Pertanyaan | Implikasi Riset |
|---|---|
| Informasi 3D apa yang dapat dipulihkan dari citra 2D? | Menentukan batas fundamental metode monocular vs multi-view |
| Bagaimana jumlah dan posisi view memengaruhi rekonstruksi? | Mendesain akuisisi data dan protokol eksperimen |
| Apa batasan representasi neural untuk scene 3D? | Memilih representasi yang sesuai dengan kebutuhan aplikasi |

## Pertanyaan Turunan

- Apa peran kalibrasi kamera dan asumsi geometri?
- Bagaimana menangani occlusion dan region yang tidak teramati?
- Kapan NeRF lebih unggul dibandingkan metode tradisional, dan sebaliknya?

---

# Slide 04 - Agendan dan Alur Materi

## Alur Pembelajaran

1. Motivasi 3D vision dan posisinya dalam computer vision.
2. Model kamera dan proyeksi 3D ke 2D.
3. Multi-view geometry: epipolar constraint, fundamental matrix, dan triangulasi.
4. Stereo matching dan depth estimation.
5. Point cloud dan multi-view reconstruction tradisional.
6. Neural rendering dan NeRF.
7. Tantangan penelitian dan perumusan masalah.
8. Praktikum eksplorasi depth dan rekonstruksi sederhana.

## Hubungan dengan Pertemuan Sebelumnya

- Diffusion models (Pertemuan 9) kini digunakan juga untuk **3D generation** dan **novel view synthesis**.
- Representasi visual dari CNN/ViT (Pertemuan 3) menjadi backbone untuk depth estimation dan NeRF.

---

# Slide 05 - Mengapa 3D Vision Penting?

## Aplikasi dan Dampak

| Domain | Kebutuhan 3D |
|---|---|
| Robotika dan navigasi | Estimasi kedalaman, obstacle avoidance, SLAM |
| AR/VR | Rekonstruksi scene, view synthesis, relighting |
| Medis | Rekonstruksi organ dari CT/MRI/USG |
| Otomotif | LiDAR+ kamera, depth estimation, 3D deteksi |
| E-commerce dan budaya | Digital twin, 3D scanning objek |

## Kontribusi Ilmiah

- 3D vision menjembatani **persepsi 2D** dan **aksi/representasi dunia nyata**.
- Banyak masalah 3D bersifat **ill-posed** sehingga membuka peluang riset berbasis asumsi, prior, atau representasi neural.

---

# Slide 06 - Dari Citra 2D ke Informasi 3D

## Sumber Informasi 3D

| Sumber | Deskripsi | Contoh Metode |
|---|---|---|
| Monocular cues | Bayangan, tekstur, perspektif, ukuran relatif | Depth from single image |
| Binocular stereo | Dua kamera dengan baseline diketahui | Stereo matching |
| Multi-view | Banyak gambar dari sudut berbeda | Structure-from-Motion, MVS, NeRF |
| Sensor aktif | Depth sensor atau LiDAR | RGB-D, point cloud langsung |

## Catatan Penting

- Informasi 3D dari citra tunggal bersifat **ambigu**: banyak scene 3D berbeda dapat menghasilkan citra 2D yang sama.
- Multi-view mengurangi ambiguitas, tetapi tetap memiliki tantangan seperti **occlusion** dan **area tak teramati**.

---

# Slide 07 - Representasi Scene 3D

## Berbagai Bentuk Representasi

| Representasi | Kelebihan | Kekurangan |
|---|---|---|
| Point cloud | Sederhana, langsung dari sensor | Tidak memiliki konektivitas permukaan |
| Mesh | Efisien untuk rendering dan simulasi | Memerlukan rekonstruksi permukaan yang akurat |
| Voxel | Cocok untuk CNN 3D | Resolusi terbatas karena memori kubik |
| Depth map | Representasi 2.5D, mudah diestimasi | Hanya satu sisi pandang |
| Neural field (NeRF) | Kontinu, fotorealistik | Training lambat, butuh banyak view |

## Arah Riset

- Peralihan dari representasi diskret ke **continuous neural representation**.
- Hybrid: point cloud + neural features untuk efisiensi.

---

# Slide 08 - Model Kamera: Proyeksi Pinhole

## Kamera Pinhole

```
Titik 3D: X = (X, Y, Z)
Titik 2D: x = (u, v)

Proyeksi:
    [u]   [fx  0  cx] [X/Z]
    [v] = [ 0 fy  cy] [Y/Z]
    [1]   [ 0  0   1] [ 1 ]
```

- `fx, fy`: focal length dalam piksel.
- `cx, cy`: principal point.
- `Z`: depth sepanjang sumbu optik.

## Matriks Kamera

- **Intrinsik** `K`: fokus, principal point, skew.
- **Ekstrinsik** `[R|t]`: rotasi dan translasi kamera terhadap dunia.
- Proyeksi lengkap: `x = K [R|t] X`.

---

# Slide 09 - Distorsi dan Kalibrasi Kamera

## Distorsi Lensa

- Distorsi radial: barrel/pincushion.
- Distorsi tangensial: lensa tidak sejajar sensor.
- Model umum: `x_distorted = x(1 + k1 r^2 + k2 r^4 + ...)`

## Kalibrasi Kamera

- Tujuan: memperoleh intrinsik `K` dan koefisien distorsi.
- Metode umum: chessboard calibration (OpenCV).
- **Mengapa penting untuk riset?**
  - Rekonstruksi 3D yang akurat memerlukan intrinsik yang benar.
  - Kesalahan kalibrasi berdampak langsung pada epipolar geometry dan triangulasi.

---

# Slide 10 - Asumsi dan Keterbatasan Model Kamera

## Asumsi Standar

- Kamera mengikuti model pinhole atau model lensa sederhana.
- Pencahayaan seragam, tidak ada refraksi atau refleksi internal.
- Sensor dan lensa tidak berubah selama akuisisi.

## Keterbatasan

| Situasi | Dampak |
|---|---|
| Lensa wide-angle extreme | Distorsi tinggi, model tidak cukup |
| Rolling shutter | Garis bengkok pada scene bergerak |
| Refleksi/transparansi | Melanggar asumsi permukaan Lambertian |
| Depth ambiguity pada area tekstur seragam | Stereo matching gagal |

## Implikasi Riset

- Pilih model kamera yang sesuai dengan domain aplikasi.
- Dokumentasikan asumsi kalibrasi secara eksplisit pada concept note.

---

# Slide 11 - Epipolar Geometry: Dua Kamera dan Satu Titik 3D

## Konsep Dasar

- Diberikan titik 3D `X` yang terlihat di dua kamera: `x1` dan `x2`.
- `X`, pusat kamera `C1`, dan `C2` berada pada satu bidang: **epipolar plane**.
- Proyeksi `x1` pada kamera 2 terletak pada garis yang disebut **epipolar line**.

## Epipolar Constraint

- Untuk titik `x1`, pencarian pasangan `x2` cukup dilakukan di sepanjang epipolar line.
- Ini menyederhanakan **stereo matching** dari pencarian 2D menjadi 1D.

```
[ u2 v2 1 ] F [ u1 v1 1 ]^T = 0
```

- `F` = fundamental matrix (piksel).
- `E` = essential matrix (setelah normalisasi intrinsik).

---

# Slide 12 - Fundamental Matrix dan Essential Matrix

## Definisi

| Matriks | Input | Output | Kebutuhan |
|---|---|---|---|
| Fundamental `F` | Pixel coordinates | Pixel coordinates | Tidak perlu kalibrasi |
| Essential `E` | Normalized coordinates | Normalized coordinates | Perlu kalibrasi intrinsik |

## Hubungan

- `E = K2^T F K1`
- `E` mengandung rotasi `R` dan translasi `t` relatif antar kamera.
- Dari `E`, dapat dilakukan **dekomposisi** untuk memperoleh `R` dan `t` (dengan empat kemungkinan solusi).

## Kegunaan dalam Riset

- Validasi geometri dua view.
- Pencarian correspondences yang konsisten secara geometris.
- Dasar untuk *Structure-from-Motion* dan *Visual Odometry*.

---

# Slide 13 - Estimasi Fundamental Matrix: 8-Point Algorithm

## Ide Dasar

- Setiap pasangan titik korespondensi menghasilkan satu persamaan linear.
- Dengan 8 titik atau lebih, `F` dapat diestimasi menggunakan **linear least squares**.
- Normalisasi koordinat (Hartley) penting untuk stabilitas numerik.

## Pseudocode

```python
import numpy as np
## x1, x2: array of corresponding points (N, 2)
## 1. Normalize points
## 2. Build design matrix A from x1 and x2
## 3. Solve SVD: A = U S Vt
## 4. F = last row of Vt, reshaped to 3x3
## 5. Enforce rank-2 constraint via SVD
## 6. Denormalize F
```

## Catatan

- Gunakan **RANSAC** untuk robust estimation terhadap outlier.
- Implementasi tersedia pada OpenCV: `cv2.findFundamentalMat`.

---

# Slide 14 - Triangulasi: Menghitung Titik 3D dari Dua Ray

## Prinsip

- Diketahui `x1`, `x2`, serta proyeksi kamera `P1`, `P2`.
- Titik 3D `X` harus memenuhi:
  - `x1 = P1 X`
  - `x2 = P2 X`
- Gabungan persamaan membentuk sistem linear `A X = 0`.

## Metode

- **Linear triangulation** (DLT).
- **Midpoint method**: jarak terdekat antara dua sinar.
- **Optimal triangulation**: minimasi error geometrik (Sampson distance).

## Faktor yang Memengaruhi Akurasi

| Faktor | Pengaruh |
|---|---|
| Baseline sempit | Ketidakpastian depth besar |
| Sudut pandang ekstrem | Degenerasi numerik |
| Noise pada korespondensi | Error propagasi |
| Kalibrasi tidak akurat | Bias sistematis |

---

# Slide 15 - Structure-from-Motion dan Multi-View Reconstruction

## Alur SfM

1. Deteksi fitur (SIFT, ORB, SuperPoint).
2. Pencocokan fitur antar gambar.
3. Estimasi `F`/`E` serta `R`, `t` antar view.
4. Triangulasi titik 3D.
5. Bundle adjustment: optimasi bersama posisi kamera dan titik 3D.

## Hasil

- **Sparse point cloud** + pose kamera.
- Dilanjutkan dengan **Multi-View Stereo (MVS)** untuk menghasilkan dense point cloud.

---

# Slide 16 - Bundle Adjustment

## Apa yang Dioptimasi

- Parameter kamera: intrinsik, ekstrinsik.
- Koordinat titik 3D.
- Meminimalkan **reprojection error**:

```
min Σ_i Σ_j || x_ij - P_j X_i ||^2
```

## Mengapa Penting

- Menghilangkan drift dan error akumulatif.
- Menghasilkan pose kamera dan struktur 3D yang konsisten secara global.
- Merupakan komponen inti dari COLMAP, OpenMVG, dan sistem SLAM.

## Kaitannya dengan NeRF

- NeRF memerlukan **pose kamera yang akurat**.
- COLMAP sering digunakan sebagai preprocessing untuk NeRF.
- Ketepatan pose sangat memengaruhi kualitas novel view synthesis.

---

# Slide 17 - Stereo Matching dan Depth Estimation

## Konsep Stereo

- Dua kamera sejajar (rectified) memiliki epipolar line horizontal.
- Disparity `d = u_left - u_right` berbanding terbalik dengan depth:

```
Z = (f * B) / d
```

- `f`: focal length, `B`: baseline.

## Alur Stereo Matching

1. Rectifikasi: menyelaraskan epipolar line.
2. Matching: mencari korespondensi piksel.
3. Disparity map: untuk setiap piksel kiri, cari pasangan di kanan.
4. Depth map: konversi disparity ke depth.

---

# Slide 18 - Metode Stereo Matching: Dari Klasik ke Modern

## Pendekatan Klasik

| Metode | Deskripsi |
|---|---|
| Block matching (BM) | Sum of Absolute Differences (SAD) pada window |
| Semi-Global Matching (SGM) | Optimasi energi dengan smoothness penalty |
| Graph cut / belief propagation | Global optimization via MRF |

## Pendekatan Deep Learning

- **DispNet**: encoder-decoder untuk disparity.
- **PSMNet**: pyramid stereo matching network.
- **RAFT-Stereo**: iterative updates berbasis recurrent unit.
- Umumnya dilatih dengan supervised loss pada data sintetis (Scene Flow) lalu diadaptasi ke data nyata.

---

# Slide 19 - Monocular Depth Estimation

## Tantangan

- Satu citra 2D dapat berasal dari banyak scene 3D.
- Skala depth bersifat ambigu: tanpa informasi kalibrasi, hanya depth relatif yang dapat diperoleh.

## Pendekatan

- **Supervised**: memerlukan depth ground truth dari sensor.
- **Self-supervised**: memanfaatkan rekonstruksi foto-metrik dari urutan video (monodepth2).
- **Foundation model**: DINOv2 sebagai backbone, lalu depth head.

## Arah Riset

- Scale-consistent depth untuk video.
- Depth dan pose estimasi secara bersama-sama.
- Adaptasi dari data sintetis ke domain nyata.

---

# Slide 20 - Point Cloud: Representasi dan Visualisasi

## Apa Itu Point Cloud

- Himpunan titik `(x, y, z)` yang merepresentasikan permukaan scene.
- Dapat disertai warna `(r, g, b)` atau fitur tambahan.

## Sumber

| Sumber | Karakteristik |
|---|---|
| LiDAR | Akurat, sparse, mahal |
| Depth camera | Dense, noise pada tepi |
| SfM/MVS | Dense jika banyak view, error pada area tekstur rendah |
| Estimasi depth monocular | Relatif, tidak memiliki skala benar |

## Tool di Python

- `open3d`: visualisasi, transformasi, ICP.
- `pyvista`, `matplotlib` untuk plotting sederhana.
- `numpy` untuk manipulasi koordinat.

---

# Slide 21 - Visualisasi Point Cloud dengan Python

## Contoh Kode

```python
import numpy as np
import matplotlib.pyplot as plt

## Generate synthetic point cloud (hemisphere)
phi = np.random.uniform(0, 2*np.pi, 5000)
theta = np.random.uniform(0, np.pi/2, 5000)
r = 1.0
x = r * np.sin(theta) * np.cos(phi)
y = r * np.sin(theta) * np.sin(phi)
z = r * np.cos(theta)
points = np.stack([x, y, z], axis=1)
## color by height
colors = plt.cm.viridis(z / z.max())

fig = plt.figure(figsize=(6,6))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(points[:,0], points[:,1], points[:,2],
           c=colors, s=1)
ax.set_title('Synthetic Point Cloud')
plt.show()
```

## Catatan Praktikum

- Gunakan `open3d` untuk interaksi rotasi dan pengukuran.
- Simpan point cloud dalam format `.ply` atau `.xyz`.

---

# Slide 22 - Multi-View Reconstruction Tradisional

## Alur COLMAP

```
Input gambar
   -> fitur dan matching
   -> SfM: estimasi kamera + sparse points
   -> MVS: dense reconstruction
   -> output point cloud / mesh
```

## Kelebihan

- Open-source dan banyak digunakan.
- Mendukung kalibrasi dan bundle adjustment.
- Kualitas tinggi jika jumlah view cukup.

## Keterbatasan

- Butuh banyak view dengan overlap yang baik.
- Gagal pada area reflektif, transparan, tekstur rendah.
- Lambat pada scene besar.

---

# Slide 23 - Kualitas Rekonstruksi vs Jumlah View

## Observasi Empiris

| Jumlah View | Karakteristik Hasil |
|---|---|
| 2-3 view | Sparse, banyak hole, sensitif terhadap noise |
| 5-15 view | Cukup untuk object-centric, mulai padat |
| 20+ view | Baik untuk scene statis tanpa oklusi parah |

## Faktor Lain

- **Distribusi sudut view** lebih penting daripada sekadar jumlah.
- View dengan baseline terlalu sempit tidak menambah informasi.
- View dengan sudut ekstrem menghasilkan triangulasi tidak stabil.

## Implikasi Eksperimen

- Lakukan **ablation** terhadap jumlah view pada konsep riset.
- Catat distribusi pose kamera, bukan hanya jumlah.

---

# Slide 24 - Representasi Neural untuk Scene 3D

## Mengapa Neural?

- Representasi tradisional (point cloud, mesh) bersifat **diskret** dan bergantung pada kualitas rekonstruksi permukaan.
- Neural field memodelkan scene sebagai **fungsi kontinu**:

```
F : (x, y, z, view direction) -> (RGB, density)
```

## Jenis Neural Scene Representation

| Nama | Ide utama |
|---|---|
| NeRF | Radiance field dengan volume rendering |
| SDF-based (NeuS, VolSDF) | Signed distance function + rendering |
| 3D Gaussian Splatting | Gaussian kernels yang dioptimasi untuk rendering cepat |
| Tri-plane / feature grid | Akselerasi dengan representasi hibrid |

---

# Slide 25 - NeRF: Konsep Dasar

## Radiance Field

- Setiap titik 3D `(x, y, z)` memiliki:
  - warna `c = (r, g, b)`,
  - kepadatan `σ` (opacity/density).
- Warna juga bergantung pada arah pandang `d` untuk menangkap efek view-dependent (specular).

## Volume Rendering

- Warna piksel dihitung dengan mengintegrasikan sepanjang sinar kamera:

```
C(r) = ∫ T(t) σ(r(t)) c(r(t), d) dt
```

- `T(t)`: transmittance (probabilitas sinar tidak terblokir sampai `t`).

## MLP

- NeRF memetakan `(x, y, z, θ, φ)` ke `(RGB, σ)` menggunakan MLP.
- Positional encoding membantu MLP merepresentasikan frekuensi tinggi.

---

# Slide 26 - Arsitektur dan Training NeRF

## Arsitektur Sederhana

```
Input: (x, y, z) + view direction
        -> positional encoding
        -> MLP (8 layers, 256 units)
        -> output: sigma (density), feature vector
        -> additional MLP layer dengan view direction
        -> output: RGB
```

## Training

- Untuk setiap piksel, sampel titik sepanjang sinar.
- Render warna prediksi menggunakan volume rendering.
- Minimasi MSE antara warna render dan warna ground truth.

## Kebutuhan

- 20-100+ gambar per scene.
- **Pose kamera yang akurat** (biasanya dari COLMAP).
- Scene statis dan pencahayaan konsisten.

---

# Slide 27 - Kelebihan dan Keterbatasan NeRF

## Kelebihan

| Aspek | Keunggulan |
|---|---|
| Kualitas visual | Novel view synthesis fotorealistik |
| Representasi | Kontinu, resolusi tidak terbatas oleh grid |
| Fleksibilitas | Dapat digabung dengan condition, editing, relighting |

## Keterbatasan

| Masalah | Dampak |
|---|---|
| Butuh banyak view | Tidak cocok untuk sparse-view input |
| Rendering lambat | Setiap piksel memerlukan banyak sampel |
| Scene statis | Sulit untuk dynamic scene atau perubahan pencahayaan |
| Training lama | Jam hingga hari per scene |
| Generalisasi | NeRF per scene, bukan model general |

---

# Slide 28 - Varian dan Perkembangan NeRF

## Kategorisasi Perkembangan

| Tujuan | Contoh |
|---|---|
| Akselerasi training/rendering | Instant-NGP, Plenoxels |
| Generalisasi antar scene | PixelNeRF, IBRNet |
| Dynamic scene | D-NeRF, HyperNeRF |
| Sparse view | DietNeRF, RegNeRF, FreeNeRF |
| 3D generation | DreamFusion, Score Distillation Sampling |
| Relighting/editing | NeRF in the Wild, Ref-NeRF |
| Representasi alternatif | 3D Gaussian Splatting |

## Implikasi Riset

- NeRF bukan satu metode, melainkan **framework** untuk scene representation.
- Kontribusi bisa pada data, representasi, efisiensi, atau robustness.

---

# Slide 29 - 3D Gaussian Splatting

## Ide Kunci

- Scene direpresentasikan sebagai kumpulan **3D Gaussian primitives**.
- Setiap Gaussian memiliki posisi, skala, rotasi, opacity, dan warna (spherical harmonics).
- Rendering dilakukan dengan **splatting** Gaussian ke citra, bukan volume rendering sepanjang sinar.

## Keunggulan

- Rendering jauh lebih cepat daripada NeRF (realtime).
- Training relatif cepat (menit hingga puluhan menit).
- Kualitas visual tinggi.

## Tantangan

- Memori tinggi untuk scene besar.
- Belum sepenuhnya stabil pada kasus pencahayaan kompleks.
- Area riset: kompresi, anti-aliasing, dynamic 3DGS, dan sparse view.

---

# Slide 30 - Perbandingan Representasi Neural

## Tabel Perbandingan

| Kriteria | NeRF | 3D Gaussian Splatting | SDF-based |
|---|---|---|---|
| Kecepatan training | Lambat | Sedang | Sedang |
| Kecepatan rendering | Lambat | Sangat cepat | Sedang |
| Kualitas permukaan | Tidak eksplisit | Point-based | Baik, implicit surface |
| Hardware requirement | Tinggi | Tinggi | Tinggi |
| Kematangan tooling | Matang | Berkembang cepat | Berkembang |

## Pemilihan Berdasarkan Masalah

- Butuh surface akurat untuk simulasi/animasi? → SDF-based.
- Butuh rendering realtime? → 3DGS.
- Butuh baseline klasik untuk perbandingan? → NeRF.

---

# Slide 31 - Tantangan Utama 3D Vision untuk Riset

## Tantangan Ilmiah

| Tantangan | Deskripsi | Peluang Riset |
|---|---|---|
| Occlusion | Bagian scene tidak terlihat di beberapa view | Resampling, multi-modal fusion, prior semantik |
| Sparse view | Rekonstruksi dari 2-5 view | Regularisasi, pretraining, generative prior |
| Scale ambiguity | Kedalaman monocular tanpa skala benar | Calibration-free, metric depth, multi-sensor fusion |
| Dynamic scene | Objek bergerak, deformasi, perubahan pencahayaan | Temporal modeling, non-rigid registration |
| Dataset 3D | Ground truth sulit diperoleh | Synthetic data, simulation, weak supervision |
| Generalisasi | Model per-scene tidak praktis | Cross-scene learning, foundation model 3D |

---

# Slide 32 - Menghubungkan dengan Diffusion Models dan Foundation Models

## Persimpangan dengan Materi Sebelumnya

| Teknologi | Peran pada 3D Vision |
|---|---|
| Diffusion models (Pertemuan 9) | Prior generatif untuk sparse view, 3D generation (DreamFusion, Score Distillation Sampling) |
| DINOv2 (Pertemuan 4) | Fitur semantik untuk correspondences, dense matching, depth |
| CLIP (Pertemuan 5) | Text-guided 3D generation dan editing |
| SAM (Pertemuan 8) | Segmentasi objek untuk masking dan scene decomposition |
| ViT (Pertemuan 3) | Backbone untuk depth, MVS, dan Neural Radiance Fields |

## Implikasi

- Riset 3D vision saat ini **tidak berdiri sendiri**: banyak metode memanfaatkan model 2D yang kuat.
- Kombinasi geometric constraint dan neural representation menjadi arah yang menjanjikan.

---

# Slide 33 - Kajian Paper: Apa yang Harus Diperhatikan?

## Struktur Kajian Paper 3D Vision

1. **Problem**: apa yang ingin diselesaikan? (rekonstruksi, view synthesis, depth, dsb.)
2. **Data**: dataset apa yang digunakan? (DTU, NeRF Synthetic, ScanNet, KITTI, RealEstate10K)
3. **Metode**: representasi apa, optimasi apa, apakah memerlukan pose kamera?
4. **Baseline**: dibandingkan dengan metode apa? Adil atau tidak?
5. **Metrik**: PSNR, SSIM, LPIPS, accuracy depth, F-score rekonstruksi?
6. **Keterbatasan**: apa yang tidak diuji? Apakah klaim sesuai bukti?

## Pertanyaan Kritis

- Apakah peningkatan metrik bermakna secara visual?
- Apakah skenario eksperimen mencerminkan aplikasi nyata?
- Apakah metode membutuhkan GPU yang tidak realistis?

---

# Slide 34 - Metrik Evaluasi untuk 3D Vision

## Untuk Depth Estimation

| Metrik | Makna |
|---|---|
| Abs Rel | Absolute relative error |
| RMSE | Root mean square error |
| δ1, δ2, δ3 | Persentase piksel dengan rasio error di bawah threshold |

## Untuk Rekonstruksi

| Metrik | Makna |
|---|---|
| Chamfer Distance | Jarak rata-rata antar point cloud |
| F-score | Precision/recall pada threshold jarak |
| Accuracy | Jarak dari prediksi ke ground truth |

## Untuk Novel View Synthesis (NeRF)

| Metrik | Makna |
|---|---|
| PSNR | Fidelity piksel |
| SSIM | Struktur dan luminance |
| LPIPS | Perceptual similarity (lebih sesuai persepsi manusia) |

---

# Slide 35 - Desain Eksperimen 3D: Data dan Baseline

## Elemen Desain Eksperimen

| Komponen | Contoh Keputusan |
|---|---|
| Dataset | Pilih scene statis vs dinamis, indoor vs outdoor, objek vs scene |
| Split | Scene untuk training dan testing tidak boleh overlap |
| Jumlah view | Variasikan 2, 4, 8, 16, 32 untuk mempelajari sensitivitas |
| Baseline | Metode klasik (COLMAP), NeRF, 3DGS, atau metode monocular |
| Metrik | Kombinasikan geometris dan perseptual |

## Risiko yang Harus Dicatat

- Overfitting pada scene tertentu.
- Ketergantungan pada pose kamera yang tidak akurat.
- Bias pada dataset sintetis yang terlalu bersih.

---

# Slide 36 - Praktikum: Eksplorasi Depth dan Multi-View Reconstruction

## Tujuan Praktikum

1. Memperoleh pengalaman langsung dengan depth estimation atau rekonstruksi.
2. Memvisualisasikan point cloud.
3. Membandingkan kualitas rekonstruksi berdasarkan jumlah view.

## Alur Kerja

| Langkah | Tool |
|---|---|
| Ambil atau unduh image sequence | COLMAP, dataset |
| Estimasi pose kamera | COLMAP (feature matching + SfM) |
| Dense reconstruction | COLMAP (MVS) atau metode sederhana |
| Visualisasi point cloud | Open3D, Matplotlib |
| Depth estimation monocular | Model pretrained (MiDaS, DPT, Depth Anything) |

---

# Slide 37 - Pseudocode Praktikum dengan COLMAP

## Command-Line COLMAP

```bash
## 1. Feature extraction
colmap feature_extractor \
    --database_path database.db \
    --image_path images/

## 2. Feature matching
colmap exhaustive_matcher \
    --database_path database.db

## 3. Sparse reconstruction (SfM)
colmap mapper \
    --database_path database.db \
    --image_path images/ \
    --output_path sparse/

## 4. Dense reconstruction (MVS)
colmap image_undistorter \
    --image_path images/ \
    --input_path sparse/0 \
    --output_path dense/
colmap patch_match_stereo \
    --workspace_path dense/
colmap stereo_fusion \
    --workspace_path dense/ \
    --output_path dense/fused.ply
```

## Catatan

- Gunakan subset gambar untuk eksperimen 2, 4, 8 view.
- Bandingkan point cloud dari `fused.ply` secara visual dan metrik.

---

# Slide 38 - Pseudocode Depth Estimation Monocular

## Python dengan Model Pretrained

```python
import torch
from PIL import Image
import matplotlib.pyplot as plt

## Contoh: gunakan model DPT dari timm/torchhub
model = torch.hub.load("intel-isl/MiDaS", "DPT_Large")
model.eval()

img = Image.open("image.jpg").convert("RGB")
## preprocessing: resize, normalize
## inference: depth = model(input_tensor)
## visualize depth map dengan colormap
plt.imshow(depth_map, cmap="inferno")
plt.colorbar()
plt.show()
```

## Evaluasi Sederhana

- Bandingkan depth relatif dengan ground truth LiDAR atau stereo.
- Hitung Abs Rel dan RMSE jika tersedia.
- Amati area error: tepi objek, area tekstur rendah, refleksi.

---

# Slide 39 - Perumusan Masalah Penelitian 3D: Template Concept Note

## Struktur Concept Note

| Bagian | Isi |
|---|---|
| Judul masalah | Kalimat singkat yang menggambarkan gap |
| Data | Dataset, jumlah view, jenis scene, resolusi |
| Asumsi geometri | Kalibrasi, baseline, scene statis/dinamis |
| Baseline | Metode pembanding dan justifikasi |
| Kontribusi yang dibayangkan | Metode baru, representasi baru, dataset baru |
| Risiko teknis | Kegagalan training, keterbatasan GPU, kesulitan data |

## Contoh Pertanyaan Riset

- Bagaimana merekonstruksi scene dari sparse view dengan bantuan prior diffusion?
- Bagaimana menangani dynamic scene pada 3D Gaussian Splatting?
- Bagaimana mengintegrasikan semantic cues untuk mengurangi ambiguitas depth monocular?

---

# Slide 40 - Rangkuman dan Kaitan ke Pertemuan Berikutnya

## Rangkuman

- 3D vision memulihkan struktur 3D dari citra 2D melalui model kamera, epipolar geometry, stereo, dan multi-view reconstruction.
- NeRF dan 3D Gaussian Splatting memberikan representasi neural yang fleksibel dan fotorealistik.
- tantangan utama: occlusion, sparse view, scale ambiguity, dynamic scene, dan ketersediaan dataset 3D.
- Praktikum memberikan pengalaman langsung dalam estimasi depth dan rekonstruksi point cloud.

## Kaitan ke Pertemuan 11

- Model 3D yang dihasilkan perlu dievaluasi dari sisi **keandalan dan interpretabilitas**.
- Pertemuan berikutnya akan membahas explainable, robust, dan trustworthy computer vision.
- Pertanyaan yang dapat dibawa: bagaimana menjelaskan keputusan model depth? Bagaimana memastikan rekonstruksi robust terhadap perubahan distribusi data?

---

# Slide 41 - TERIMA KASIH

Pertemuan berikutnya

**Explainable, Robust, dan Trustworthy Computer Vision**