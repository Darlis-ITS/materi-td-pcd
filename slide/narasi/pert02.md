# Narasi TD Pengolahan Citra Digital - Pertemuan 02

## Fondasi Representasi Visual: CNN, Transformer, dan Dataset Benchmark

Sumber: markdown/pert02-tambahan.md

---

## Slide 000 - Cover

### Narasi

Slide ini membuka Pertemuan Kedua dengan penekanan pada fondasi representasi visual, mencakup tiga pilar utama: arsitektur Convolutional Neural Networks (CNN), mekanisme Transformer, serta peran strategis dataset benchmark dalam ekosistem computer vision. Pada level doktoral, pembahasan tidak berhenti pada deskripsi arsitektur, melainkan berfokus pada bagaimana representasi visual dipelajari, diukur, dan divalidasi secara empiris. Pemahaman ini menjadi prasyarat kritis untuk melakukan perbandingan model yang ketat, mengidentifikasi keterbatasan generalisasi, dan merumuskan pertanyaan penelitian yang bermakna.

Kita akan menelusuri bagaimana CNN membangun representasi hierarkis melalui operasi konvolusi, pooling, dan non-linearitas, yang memungkinkan ekstraksi fitur lokal hingga semantik tingkat tinggi. Selanjutnya, transisi ke Transformer akan diurai melalui mekanisme *self-attention* yang menangkap dependensi jarak jauh dan konteks global tanpa batasan receptive field tetap. Kombinasi kedua paradigma ini membentuk landasan bagi perkembangan model vision terkini yang menggabungkan efisiensi spasial CNN dengan fleksibilitas kontekstual Transformer.

Selain arsitektur, pemilihan dataset benchmark menjadi aspek metodologis yang menentukan validitas klaim ilmiah. Kita akan membahas kriteria seleksi dataset, bias distribusi, metrik evaluasi yang sesuai, serta praktik reproduktibilitas dalam pelaporan hasil. Pembahasan ini berfungsi sebagai jembatan konseptual menuju pertemuan berikutnya, di mana arsitektur modern seperti Vision Transformer dan teknik attention akan dibandingkan secara eksperimental. Dengan fondasi ini, mahasiswa dilengkapi kerangka analitis untuk mengevaluasi state-of-the-art, menemukan research gap, dan mendesain eksperimen yang robust dalam konteks pengembangan sistem visi komputer tingkat lanjut.

---

## Slide 001 - Posisi Pertemuan dalam Perkuliahan

### Narasi

Slide ini menyajikan peta perjalanan perkuliahan untuk memberikan konteks strategis terhadap alur pembahasan. Pertemuan pertama telah menetapkan pondasi berupa identifikasi masalah, audit dataset, dan penyusunan baseline klasik. Fokus utamanya adalah menjawab pertanyaan mendasar mengenai kesesuaian masalah penelitian dan pemilihan titik awal eksperimen yang valid.

Pertemuan kedua, yang sedang kita diskusikan, secara spesifik menyoroti fondasi representasi visual melalui arsitektur CNN dan Transformer, serta evaluasi terhadap dataset benchmark. Pergeseran fokus ini membawa kita kepada pertanyaan metodologis baru: bagaimana model secara otomatis mempelajari dan mengonstruksi representasi visual yang bermakna?

Sebagai kelanjutan logis, pertemuan ketiga akan membahas CNN modern, mekanisme attention, dan Vision Transformer untuk analisis komparatif arsitektur terkini. Selanjutnya, pertemuan keempat akan mengeksplorasi self-supervised learning dan foundation vision models, yang menjawab tantangan pembelajaran representasi dalam skenario ketersediaan label yang terbatas.

Secara konseptual, pertemuan ini berperan sebagai jembatan kritis antara paradigma *handcrafted feature* dan era *learned representation*. Penguasaan terhadap mekanisme ekstraksi fitur otomatis ini sangat esensial bagi peneliti tingkat doktoral dalam merumuskan hipotesis, mendesain eksperimen, dan melakukan evaluasi kritis terhadap state-of-the-art.

Untuk memastikan transisi yang koheren, mari kita lakukan tinjauan singkat terhadap materi dan hasil praktikum dari pertemuan pertama sebelum memasuki penjelasan teknis mengenai arsitektur dan dataset.

---

## Slide 002 - Recap Pertemuan 01

### Narasi

Slide ini berfungsi sebagai pengingat terstruktur atas materi Pertemuan 01, sekaligus menyiapkan transisi konseptual menuju pembelajaran representasi visual secara mendalam. Pada pertemuan sebelumnya, kita telah menyusun peta riset secara keseluruhan, mulai dari identifikasi masalah hingga penentuan titik awal yang layak melalui audit dataset dan pembuatan baseline. Rekapan ini menegaskan kembali bahwa pemahaman metodologis yang kuat merupakan prasyarat sebelum memasuki arsitektur model modern.

Secara teoritis, kita telah membedah perbedaan ruang lingkup antara pengolahan citra digital dan computer vision, serta melacak evolusi pendekatan dari metode klasik menuju era deep learning dan foundation models. Poin kritis yang harus terus dipegang adalah disiplin dalam audit dataset, pemilihan benchmark yang valid, establishment baseline yang konsisten, dan komitmen terhadap reproducibility. Pipeline klasik yang terdiri dari preprocessing, ekstraksi fitur buatan manusia, hingga klasifikasi, menjadi kerangka pembanding untuk mengidentifikasi keterbatasan pendekatan manual. Selain itu, failure analysis diposisikan sebagai sumber utama perumusan hipotesis dan research question yang tajam, bukan sekadar laporan akurasi.

Dari sisi praktikum, eksperimen pada dataset digits memberikan implementasi langsung atas pipeline tersebut. Kita dimulai dengan baseline majority class untuk menetapkan batas bawah performa sistem, dilanjutkan dengan analisis distribusi intensitas gambar menggunakan histogram. Ekstraksi fitur dilakukan melalui k-Nearest Neighbors berbasis histogram dan Histogram of Oriented Gradients yang digabungkan dengan Linear SVM. Evaluasi akhir mencakup confusion matrix dan bedah kesalahan klasifikasi, yang secara eksplisit menghubungkan temuan empiris dengan langkah perbaikan metodologis atau pengembangan hipotesis baru.

Rekapan ini menjadi jembatan strategis menuju Pertemuan 03. Jika Pertemuan 01 berakhir pada pertanyaan mendasar mengenai keterbatasan fitur buatan manusia, maka Pertemuan 02 akan membuka “kotak hitam” arsitektur CNN dan Transformer. Dengan memahami operasi convolution, feature map, pooling, receptive field, residual connection, hingga konsep token, embedding, query-key-value, self-attention, multi-head attention, positional encoding, dan patch tokenization, mahasiswa akan memiliki landasan mekanistik yang solid. Hal ini memungkinkan evaluasi kritis terhadap model modern seperti ResNet, DeiT, dan Vision Transformer di pertemuan berikutnya, serta mempersiapkan eksplorasi foundation models dengan perspektif yang analitis dan berorientasi pada kontribusi ilmiah tingkat doktor.

---

## Slide 003 - Celah antara Pertemuan 01 dan 03

### Narasi

Pada pertemuan sebelumnya, kita telah menguji batas-batas pipeline klasik yang mengandalkan *handcrafted feature* seperti HOG atau histogram intensitas, dikombinasikan dengan classifier sederhana seperti kNN dan Linear SVM. Eksplorasi terhadap dataset *digits* dan analisis *confusion matrix* berhasil menyoroti keterbatasan fundamental pendekatan tersebut: fitur yang dirancang secara manual tidak mampu menangkap kompleksitas visual tingkat tinggi secara adaptif. Pertanyaan penutup dari sesi itu menjadi titik tolak krusial untuk pembahasan kali ini. Jika fitur tidak lagi bisa diturunkan secara eksplisit oleh manusia, maka mekanisme pembelajaran representasi harus diambil alih langsung oleh model melalui data.

Celah antara Pertemuan 01 dan Pertemuan 03 terletak pada kebutuhan mendesak untuk membongkar “kotak hitam” arsitektur modern sebelum kita langsung menerapkan model state-of-the-art seperti ResNet, DeiT, atau ViT. Tanpa pemahaman mekanistik yang mendalam, penggunaan framework canggih akan bersifat templatisasi dan menghambat kemampuan audit metodologis yang wajib dimiliki pada jenjang doktoral. Fungsi Pertemuan 02 secara eksplisit adalah membuka fondasi representasi visual yang menjadi tulang punggung dua paradigma utama dalam computer vision kontemporer. Sebelum masuk ke arsitektur spesifik, kita harus menguasai prinsip-prinsip inti berikut:

- Operasi konvolusi dan pembentukan *feature map*;
- Mekanisme *pooling* dan konsep *receptive field*;
- Peran *residual connection* dalam stabilisasi gradien;
- Konseptualisasi *token* dan *embedding*;
- Interaksi *query*, *key*, dan *value*;
- Mekanisme *self-attention* dan *multi-head attention*;
- Implementasi *positional encoding*;
- Proses *patch tokenization*.

Untuk CNN, pemahaman tentang bagaimana konvolusi secara komputasional membentuk *feature map* bukan sekadar hafalan layer, melainkan kunci untuk menganalisis bagaimana *stride*, *padding*, dan jenis *pooling* secara langsung mengatur ukuran *receptive field* serta hierarki abstraksi spasial. Pemahaman ini menentukan kemampuan Anda dalam merancang arsitektur yang robust terhadap variasi skala atau deformasi. Sementara itu, *residual connection* akan diurai sebagai solusi matematis terhadap masalah degradasi gradien, memungkinkan pelatihan jaringan yang sangat dalam tanpa kolapsnya sinyal pembelajaran.

Di sisi Transformer, paradigma bergeser dari lokalisasi spasial menuju dependensi global melalui *self-attention*. Kita akan membedah bagaimana citra dipotong menjadi *patch* lalu dikonversi menjadi *token embedding*, sehingga struktur grid piksel berubah menjadi urutan sekuensial yang kompatibel dengan arsitektur transformer. Matriks *query*, *key*, dan *value* akan dijelaskan sebagai mekanisme penguji relevansi kontekstual antar elemen, sementara *multi-head attention* meningkatkan kapasitas model untuk menangkap berbagai pola relasional secara paralel. *Positional encoding* hadir sebagai kompensasi struktural karena transformer murni bersifat permutasi-invarian.

Seluruh fondasi ini disusun sebagai prasyarat analitis menuju Tujuan Pembelajaran Pertemuan 02 yang tercantum pada slide berikutnya. Dengan menguasai mekanisme dasar tersebut, Anda tidak hanya akan mampu mengimpor dan menjalankan arsitektur CNN maupun Transformer, tetapi juga melakukan *ablation study*, merancang eksperimen kontrol, mengidentifikasi *research gap*, dan memposisikan karya Anda secara kritis terhadap perkembangan terkini. Mari kita lanjutkan ke rincian kompetensi spesifik yang harus dicapai setelah menyelesaikan sesi ini.

---

## Slide 004 - Tujuan Pembelajaran Pertemuan 02

### Narasi

Slide ini merumuskan tujuan pembelajaran spesifik untuk Pertemuan 02, yang berfungsi sebagai jembatan krusial antara landasan teori di Pertemuan 01 dan implementasi model lanjutan di Pertemuan 03. Pada tingkat doktoral, pemahaman mekanistik bukan sekadar menghafal arsitektur, melainkan kemampuan untuk membongkar “kotak hitam” sebelum mengadopsi model state-of-the-art seperti ResNet, DeiT, atau ViT.

Capaian kompetensi yang ditetapkan dapat dikelompokkan menjadi tiga ranah utama:
- **Representasi CNN**: Menjelaskan citra sebagai tensor, mekanisme pembentukan feature map melalui convolution, serta analisis mendalam terhadap stride, padding, pooling, dan konsep receptive field.
- **Arsitektur & Pelatihan**: Memahami alur training CNN dari nol, fungsi residual connection dalam stabilisasi gradien, serta mekanisme dasar self-attention, token, embedding, multi-head attention, dan posisi.
- **Manajemen Data Penelitian**: Mengubah citra menjadi patch token, memilih dataset berdasarkan task, anotasi, domain, dan research question, serta melakukan audit terhadap bias, data leakage, lisensi, dan keterbatasan benchmark.

Penguasaan poin-poin ini memastikan bahwa mahasiswa tidak hanya menggunakan library seperti PyTorch atau torchvision secara instan, tetapi memahami transformasi matematis dan statistik yang terjadi di balik setiap layer. Pemahaman tentang bagaimana kernel CNN belajar secara adaptif berbeda dengan filter manual, serta mengapa tokenisasi patch dan positional encoding menjadi syarat mutlak dalam Transformer, akan menjadi dasar analisis kritis terhadap paper-paper terkini.

Tujuan-tujuan ini secara langsung menutup celah yang diidentifikasi pada slide sebelumnya, sekaligus menyiapkan kerangka berpikir untuk mengeksplorasi pertanyaan kunci pada slide berikutnya. Fokus diskusi akan bergeser dari “bagaimana cara kerja” menuju “mengapa mekanisme ini dipilih”, sehingga mahasiswa terbiasa memposisikan setiap komponen arsitektur dan dataset dalam konteks novelty, validitas empiris, dan peluang kontribusi ilmiah baru.

---

## Slide 005 - Pertanyaan Kunci

### Narasi

Slide ini menyajikan rangkaian pertanyaan kunci yang menjadi kompas analitis bagi materi pertemuan kedua. Setelah menyelaraskan diri dengan tujuan pembelajaran sebelumnya, kita akan membedah tiga aspek fundamental: representasi visual pada CNN, mekanisme Transformer, serta strategi seleksi dan audit dataset. Pertanyaan-pertanyaan ini tidak bersifat faktual semata, melainkan dirancang untuk memicu eksplorasi kritis yang esensial dalam penyusunan penelitian tingkat doktoral.

### Representasi Visual pada CNN

- Apa yang sebenarnya dipelajari CNN dari piksel? Jaringan tidak sekadar mengenali tepi atau gradien, melainkan membangun hierarki abstraksi fitur secara bertahap melalui lapisan-lapisan konvolusi.
- Mengapa kernel CNN tidak sama dengan filter manual? Berbeda dengan filter handcrafted yang statis, kernel CNN memperoleh bobot optimal melalui optimisasi end-to-end, memungkinkan adaptasi kontekstual terhadap karakteristik dataset.
- Bagaimana hubungan lokal berkembang menjadi representasi objek? Receptive field yang meluas secara bertahap mengakumulasi informasi spasial, mengubah pola lokal menjadi struktur semantik yang lebih invariant terhadap gangguan visual.

### Mekanisme Transformer

- Mengapa citra perlu diubah menjadi token? Transformasi ini memetakan patch spasial ke dalam ruang vektor berdimensi tinggi, memungkinkan pemrosesan paralel dan manipulasi matematika yang konsisten dengan arsitektur sequence modeling.
- Bagaimana satu token memilih informasi dari token lain? Mekanisme self-attention menghitung skore kemiripan antar token, sehingga model dapat secara dinamis mengalokasikan fokus pada wilayah yang paling relevan secara semantik.
- Mengapa Transformer memerlukan positional encoding? Karena arsitektur attention bersifat permutation-invariant, penambahan sinyal posisi spasial menjadi wajib untuk mempertahankan struktur geometris citra asli.

### Seleksi dan Audit Dataset

- Dataset populer mana yang sesuai untuk classification, detection, atau segmentation? Kesesuaian bergantung pada granularitas anotasi, cakupan kelas, dan konsistensi protokol evaluasi yang ditetapkan dalam benchmark.
- Apakah benchmark populer otomatis sesuai untuk research question kita? Tidak selalu. Validitas eksternal, bias demografis atau domain, serta risiko data leakage harus diaudit ketat sebelum digunakan sebagai baseline penelitian.

Pembahasan pertanyaan-pertanyaan di atas akan segera diterjemahkan ke dalam bentuk matematis yang konkret. Pada slide berikutnya, kita akan beralih ke representasi numerik mendasar, di mana citra dipahami sepenuhnya sebagai struktur tensor yang menjadi substrat bagi seluruh operasi komputasi CNN maupun Transformer.

---

## Slide 006 - Citra adalah Tensor, Bukan Sekadar Gambar

### Narasi

Pada tingkat implementasi komputasional, citra tidak lagi dipandang sebagai objek visual yang dapat diamati secara langsung, melainkan sebagai struktur numerik multidimensi yang disebut tensor. Pemahaman fundamental ini menjadi prasyarat teknis sebelum kita membahas bagaimana arsitektur seperti CNN maupun Transformer memanipulasi informasi visual secara matematis.

Secara representasi, citra berwarna RGB disusun dalam format `Channel × Height × Width` atau secara ringkas ditulis sebagai `3 × H × W`. Setiap channel menyimpan distribusi intensitas warna merah, hijau, dan biru pada koordinat spasial tertentu. Ketika citra-citra tersebut dikumpulkan untuk proses pelatihan, mereka membentuk satu batch sehingga dimensi strukturnya berubah menjadi `Batch × Channel × Height × Width` atau `B × 3 × H × W`. Dimensi batch ini memungkinkan komputasi vektorisasi paralel pada GPU dan menjadi standar operasional dalam framework deep learning modern.

Nilai piksel pada citra digital umumnya direpresentasikan sebagai bilangan bulat 8-bit dengan rentang 0 hingga 255. Namun, transformasi seperti `ToTensor()` secara otomatis mengonversi nilai integer tersebut menjadi tipe data float dan menskalakannya ke interval 0–1. Langkah ini merupakan bagian integral dari normalisasi input yang menggeser mean dan menstandarisasi varians distribusi piksel. Normalisasi sangat krusial untuk mencegah saturasi aktivasi, menstabilkan propagasi gradien, dan mempercepat konvergensi optimizer selama pelatihan.

Pesan utama dari konsep ini menegaskan bahwa semua operasi pada CNN maupun Transformer, mulai dari convolusi, pooling, hingga mekanisme attention, pada akhirnya bekerja pada tensor numerik murni. Tidak ada manipulasi berbasis aturan eksplisit di lapisan bawah; seluruh pemrosesan bergantung pada aljabar linear dan kalkulus diferensial yang berjalan di atas struktur tensor.

Konsep ini memberikan jawaban teknis langsung terhadap pertanyaan kunci dari slide sebelumnya mengenai apa yang sebenarnya dipelajari CNN dari piksel dan mengapa kernel CNN berbeda secara fundamental dengan filter manual. Jika slide lalu menanyakan bagaimana representasi lokal berkembang menjadi pemahaman objek holistik, maka basisnya terletak pada bagaimana jaringan mengubah distribusi probabilitas dalam ruang vektor berdimensi tinggi melalui operasi tensor.

Sejalan dengan penjelasan ini, slide berikutnya akan menguraikan transisi paradigmatik dari image processing klasik menuju pembelajaran mendalam. Filter Gaussian, Sobel, atau Laplacian yang ditentukan secara eksplisit oleh manusia akan digantikan oleh kernel yang diinisialisasi secara acak dan kemudian disempurnakan melalui backpropagation untuk meminimalkan loss tugas target. Pergeseran dari `Manusia menentukan fitur` menjadi `Model mempelajari fitur dari data dan objective` inilah yang menjadi titik tolak perkembangan representasi visual kontemporer.

---

## Slide 007 - Dari Filter Manual ke Kernel yang Dipelajari

### Narasi

Pada slide sebelumnya, kita telah menegaskan bahwa citra pada dasarnya adalah struktur tensor numerik. Baik dalam dimensi Channel × Height × Width maupun Batch × Channel × Height × Width, semua operasi komputasi—termasuk konversi nilai piksel dari rentang 0–255 menjadi float 0–1 serta proses normalisasi—berjalan langsung pada representasi tensor ini. Dari fondasi numerik inilah, kita dapat memahami bagaimana model berevolusi dalam mengekstrak informasi visual.

Dalam pendekatan pengolahan citra klasik, respons terhadap pola gambar ditentukan sepenuhnya oleh manusia. Kita mendesain filter secara eksplisit berdasarkan pengetahuan domain dan asumsi statistik tentang data. Contoh filter tradisional yang masih relevan mencakup:
- Gaussian blur untuk penghalusan noise dan reduksi detail frekuensi tinggi;
- Operator Sobel untuk deteksi tepi berbasis gradien intensitas;
- Teknik sharpening yang memperkuat kontras batas objek;
- Laplacian yang sensitif terhadap perubahan intensitas orde kedua.

Filter-filter ini bersifat statis dan handcrafted. Kinerjanya sangat bergantung pada seberapa akurat peneliti memprediksi karakteristik data target, sehingga sering kali memerlukan tuning manual yang melelahkan dan kurang generalizable.

Ketika kita masuk ke ranah Convolutional Neural Network, mekanisme ekstraksi fitur berubah secara fundamental. Nilai-nilai dalam kernel tidak lagi ditetapkan secara manual. Prosesnya berjalan sebagai berikut:
- Inisialisasi: Nilai kernel dimulai dari distribusi tertentu (misalnya He atau Xavier initialization).
- Optimasi: Backpropagation memperbarui bobot kernel secara iteratif berdasarkan gradien loss.
- Adaptasi: Kernel belajar secara langsung dari data dan objective function yang didefinisikan, sehingga mampu menangkap pola hierarkis yang relevan dengan tugas.

Pergeseran ini menandai perubahan paradigma yang mendasar:
```text
Manusia menentukan fitur
          ↓
Model mempelajari fitur dari data dan objective
```
Dengan demikian, beban desain fitur berpindah dari insinyur ke algoritma optimasi, membuka ruang bagi penemuan representasi yang jauh lebih kompleks dan adaptif.

Untuk memahami secara teknis bagaimana kernel yang telah dipelajari ini berinteraksi dengan tensor input, pada slide berikutnya kita akan mengurai operasi convolution secara matematis dan konseptual. Kita akan membahas bagaimana kernel digeser melintasi patch input, melakukan perkalian elemen demi elemen, dan menghasilkan feature map, lengkap dengan persamaan konvolusi sederhana serta implikasi strategis dari weight sharing terhadap efisiensi parameter, invariansi lokasi, dan inductive bias spasial yang menjadi ciri khas arsitektur CNN modern.

---

## Slide 008 - Operasi Convolution

### Narasi

Setelah pada slide sebelumnya kita mengidentifikasi pergeseran paradigma dari filter yang ditentukan secara manual menjadi kernel yang dipelajari melalui optimisasi gradien, kini kita akan mengurai secara mekanistik bagaimana operasi convolution tersebut bekerja di tingkat komputasi. Inti dari convolusi adalah proses geser-kali-tambah antara patch input citra dengan kernel, yang secara iteratif menghasilkan feature map sebagai representasi respons spasial model terhadap pola tertentu.

Secara matematis, interaksi ini dapat diringkas dalam persamaan diskrit berikut:
```text
Y[i,j] = Σm Σn X[i+m,j+n] W[m,n] + b
```
Pada formulasi ini, $X$ merepresentasikan nilai intensitas piksel pada jendela input, $W$ adalah matriks bobot kernel yang telah dipelajari, dan $b$ adalah bias skalar. Indeks $i$ dan $j$ menandai posisi spasial pada feature map keluaran, sementara $m$ dan $n$ menelusuri dimensi kernel. Setiap langkah pergeseran melakukan perkalian elemen-per-elemen diikuti penjumlahan, sehingga nilai $Y[i,j]$ mencerminkan skor korelasi lokal antara kernel dan region citra yang sedang dipindai.

Keunggulan arsitektural utama yang muncul dari mekanisme ini terletak pada weight sharing, di mana satu set bobot kernel diterapkan secara konsisten di seluruh wilayah citra. Implementasi ini memberikan tiga implikasi strategis bagi pemodelan visual:
- Jumlah parameter yang harus dioptimalkan menjadi jauh lebih kecil, sehingga mengurangi risiko overfitting dan mempercepat konvergensi selama pelatihan;
- Model mampu mendeteksi pola atau tekstur yang sama meskipun berada di lokasi berbeda, membangun sifat invarian translasi yang esensial untuk tugas klasifikasi dan deteksi;
- Mekanisme ini memperkenalkan inductive bias spasial yang selaras dengan struktur natural image, di mana ketergantungan informasi bersifat lokal dan bertetangga, berbeda dengan asumsi i.i.d pada data tabular konvensional.

Memahami operasi convolution dua dimensi ini menjadi prasyarat analitis sebelum kita memperluasnya ke ruang dimensi yang lebih kompleks. Pada slide berikutnya, kita akan melihat bagaimana operasi ini menangani input berganda seperti channel RGB, serta bagaimana konfigurasi lapisan `nn.Conv2d` di PyTorch mengatur transformasi dimensi tensor dari batch × channel × tinggi × lebar menjadi representasi feature map multidimensi yang siap diteruskan ke lapisan jaringan selanjutnya.

---

## Slide 009 - Channel Input dan Output

### Narasi

Setelah membahas mekanisme dasar operasi convolution dan konsep weight sharing pada slide sebelumnya, kita kini mengonkretkan konsep tersebut ke dalam implementasi arsitektural menggunakan framework deep learning modern. Pada tingkat doktoral, pemahaman mendalam tentang bagaimana dimensi tensor berubah selama propagasi maju sangat krusial untuk merancang arsitektur yang efisien dan dapat diskalakan.

Berikut adalah contoh definisi layer konvolusi dua dimensi dalam PyTorch:

```python
nn.Conv2d(
    in_channels=3,
    out_channels=32,
    kernel_size=3,
    padding=1
)
```

Parameter-parameter ini memiliki makna arsitektural yang presisi:
- `in_channels=3` merepresentasikan ruang warna RGB standar pada citra masukan. Setiap kernel akan melakukan konvolusi secara paralel terhadap ketiga channel ini sebelum dijumlahkan menjadi satu nilai aktivasi.
- `out_channels=32` menentukan jumlah filter atau kernel yang akan dipelajari oleh layer. Ini secara langsung mengkapasitas representasional layer, karena setiap channel keluaran menangkap pola fitur yang berbeda-beda.
- `kernel_size=3` menetapkan ukuran receptive field lokal sebesar 3×3 piksel.
- `padding=1` menambahkan border nol di sekeliling citra agar informasi tepi tidak hilang saat kernel bergeser.

Transformasi dimensi tensor mengikuti konvensi NCHW (Batch, Channel, Height, Width) yang digunakan secara native oleh PyTorch:

```text
B × 3 × 32 × 32
        ↓ Conv2d
B × 32 × 32 × 32
```

Pada diagram ini, `B` menyatakan ukuran batch. Dimensi spasial tetap berukuran 32×32 karena kombinasi `kernel_size=3` dan `padding=1` saling mengimbangi secara matematis, sehingga tidak terjadi penurunan resolusi. Hasil akhir berupa 32 feature map independen, masing-masing merepresentasikan respons jaringan terhadap pola spesifik yang telah dioptimalkan selama pelatihan.

Perlu dicatat bahwa meskipun padding berhasil mempertahankan dimensi spasial pada contoh ini, kontrol atas resolusi output sebenarnya diatur oleh interaksi antara padding dan stride. Parameter stride yang belum disetel secara eksplisit pada kode di atas akan menggunakan nilai default 1, namun pengaturannya secara sengaja akan mengubah skala feature map secara drastis. Pembahasan mengenai rumus transformasi dimensi dan strategi pengaturan stride serta padding akan diuraikan secara komprehensif pada slide berikutnya.

---

## Slide 010 - Stride dan Padding Mengatur Resolusi

### Narasi

Pada slide ini, kita membahas bagaimana dua parameter fundamental dalam operasi konvolusi, yaitu *stride* dan *padding*, secara eksplisit mengontrol resolusi spasial dari feature map yang dihasilkan. Setelah pada slide sebelumnya kita menelaah bagaimana jumlah channel input dan output ditentukan oleh konfigurasi kernel, langkah logis berikutnya adalah memahami bagaimana dimensi tinggi dan lebar citra berevolusi sepanjang kedalaman jaringan.

Rumus inti untuk menghitung ukuran output spasial dapat dituliskan sebagai berikut:
```text
O = floor((I + 2P - K) / S) + 1
```
Di mana `I` menyatakan ukuran input, `K` adalah ukuran kernel, `P` mewakili jumlah padding yang disisipkan di setiap tepi, `S` adalah stride atau jarak lompatan kernel saat bergerak, dan `O` merupakan ukuran output akhir. Operator `floor` menjamin pembulatan ke bawah, mengingat dimensi spasial harus berupa bilangan bulat diskrit.

Mari kita bedah implikasi numeriknya melalui tiga skenario yang disajikan. Dengan input berukuran 32, kernel 3, tanpa padding (`P=0`), dan stride 1 (`S=1`), output menyusut menjadi 30. Ini mencerminkan karakteristik alami konvolusi valid yang secara konsisten memangkas tepi informasi spasial pada setiap lapisan.

Ketika padding sebesar 1 diterapkan (`P=1`) sementara stride tetap 1, ukuran output kembali dipertahankan pada 32. Praktik *same padding* ini sangat krusial dalam arsitektur modern seperti Residual Network atau arsitektur encoder-decoder, karena memungkinkan informasi hierarkis mengalir lebih dalam tanpa kehilangan resolusi spasial awal.

Perubahan drastis terjadi ketika stride dinaikkan menjadi 2 (`S=2`). Dengan konfigurasi `P=1` dan `K=3`, output mengecil menjadi 16. Penggunaan stride lebih besar dari satu berfungsi sebagai mekanisme downsampling yang efisien, menggabungkan fungsi pooling tradisional sekaligus mengurangi kompleksitas komputasi dan kebutuhan memori untuk blok lapisan berikutnya.

Pemahaman kuantitatif terhadap interaksi antara stride, padding, dan kernel ini menjadi landasan utama dalam merancang arsitektur yang seimbang antara cakupan reseptif (*receptive field*) dan preservasi detail spasial. Setelah dimensi spasial dan kanal distabilkan melalui konvolusi, jaringan memerlukan komponen yang memecah sifat linear transformasi tersebut agar mampu memodelkan pola visual yang kompleks, sebagaimana akan kita eksplorasi pada pembahasan fungsi aktivasi non-linear.

---

## Slide 011 - Aktivasi Membuat Model Nonlinear

### Narasi

Setelah pada slide sebelumnya kita membahas bagaimana stride dan padding mengatur resolusi spasial melalui operasi konvolusi, kini kita masuk ke komponen krusial berikutnya dalam arsitektur jaringan saraf tiruan: fungsi aktivasi. Tanpa fungsi ini, setiap lapisan yang ditumpuk hanya akan melakukan transformasi linear. Secara matematis, komposisi beberapa operasi linear tetap ekuivalen dengan satu transformasi linear tunggal. Artinya, jaringan berlapis tanpa aktivasi tidak akan memiliki kapasitas representasional yang lebih baik dibandingkan jaringan satu lapisan saja.

Untuk mengatasi keterbatasan ini, fungsi aktivasi memperkenalkan non-linearitas ke dalam model. Salah satu fungsi yang paling banyak digunakan dan menjadi standar de facto adalah ReLU, atau Rectified Linear Unit. Rumusnya sangat sederhana:
```text
ReLU(x) = max(0, x)
```
Setiap nilai input positif dipertahankan, sedangkan nilai negatif di-nol-kan.

Penggunaan ReLU memberikan beberapa keuntungan strategis bagi pelatihan model:
- menambahkan non-linearitas, sehingga memungkinkan model mempelajari pola kompleks yang tidak dapat direpresentasikan oleh kombinasi linear murni;
- sangat sederhana dan efisien dalam komputasi, baik saat forward pass maupun backpropagation;
- membantu optimasi karena tidak mengalami saturasi untuk rentang input positif yang luas, sehingga mengurangi masalah vanishing gradient yang sering mengganggu aktivasi jenuh tertentu.

Perlu ditekankan bahwa aktivasi bukan sekadar langkah tambahan. Non-linearitaslah yang sebenarnya memberi makna pada kedalaman jaringan. Tanpa non-linearitas, menambah jumlah lapisan hanya akan meningkatkan kompleksitas komputasi tanpa meningkatkan kemampuan model untuk memetakan hubungan non-linear dalam data citra.

Dengan fitur non-linear yang telah "diaktifkan", jaringan siap untuk mengekstrak hierarki fitur yang semakin abstrak. Langkah selanjutnya setelah ekstraksi fitur ini adalah mengelola ukuran representasi tersebut, yang akan kita bahas pada slide berikutnya mengenai pooling. Operasi pooling akan mereduksi resolusi spasial secara terkontrol, menyeimbangkan antara efisiensi komputasi dan preservasi informasi penting sebelum diteruskan ke lapisan berikutnya.

---

## Slide 012 - Pooling Mereduksi Resolusi

### Narasi

Setelah aktivasi memperkenalkan sifat non-linear yang memungkinkan jaringan mempelajari pola kompleks, langkah struktural berikutnya dalam arsitektur CNN adalah pengendalian resolusi spasial melalui operasi pooling. Slide ini membahas bagaimana Max Pooling berfungsi sebagai mekanisme downsample yang fundamental, sekaligus menyoroti implikasi desainnya terhadap representasi visual.

Secara operasional, Max Pooling bekerja dengan menggeser jendela sliding window berukuran tetap—umumnya 2×2 atau 3×3—melalui feature map, lalu hanya mempertahankan nilai terbesar di setiap wilayah tersebut. Ilustrasi pada slide menunjukkan transformasi dari feature map 32×32 menjadi 16×16 setelah diterapkan Max Pooling 2×2. Proses ini berjalan secara paralel dan independen pada setiap channel, sehingga dimensi kedalaman (depth) tetap terjaga sementara dimensi spasial menyusut setengahnya.

Pengurangan resolusi ini bukan sekadar teknik kompresi, melainkan memiliki tujuan arsitektural yang spesifik:
- Mengurangi ukuran spasial secara drastis, yang secara langsung menurunkan beban komputasi dan memori pada lapisan-lapisan downstream.
- Memperbesar *receptive field* efektif neuron pada lapisan berikutnya, memungkinkan model mengintegrasikan konteks lingkungan yang lebih luas tanpa menambah parameter trainable.
- Memberikan toleransi terhadap pergeseran kecil (*translation invariance*), sehingga model menjadi lebih robust terhadap variasi posisi objek dalam frame.
- Memfilter noise dan redundansi pixel dengan hanya menonjolkan respons tertinggi di setiap lokalitas.

Namun, setiap keputusan arsitektural membawa *trade-off*. Reduksi yang terlalu agresif, misalnya melalui kernel pooling berukuran besar atau stride yang tidak dioptimalkan, berisiko mengaburkan objek berukuran kecil atau menghilangkan presisi lokalisasi spasial. Dalam konteks tugas yang menuntut akurasi posisi tinggi seperti segmentasi semantik atau deteksi objek multi-skala, kehilangan detail ini dapat menjadi bottleneck performa. Oleh karena itu, konfigurasi pooling harus dipertimbangkan secara kritis berdasarkan distribusi skala objek dalam dataset target.

Perlu dicatat bahwa meskipun Max Pooling masih menjadi standar de facto, literatur terkini sering menggantinya atau melengkapinya dengan *strided convolution* atau *average pooling* untuk menjaga kelancaran gradien dan preservasi informasi halus. Mekanisme downsampling ini juga menjadi jembatan penting menuju pembentukan representasi bertingkat. Setelah resolusi dikendalikan dan fitur distilasi, jaringan mulai menyusun kombinasi fitur yang semakin abstrak, yang akan kita bedah lebih lanjut pada pembahasan hierarki feature map di slide berikutnya.

---

## Slide 013 - Hierarki Feature Map

### Narasi

Operasi pooling yang telah kita bahas pada slide sebelumnya secara fundamental mengubah cara jaringan memproses informasi spasial. Reduksi resolusi ini bukan sekadar penghematan komputasi, melainkan mekanisme arsitektural yang memungkinkan lapisan-lapisan berikutnya membangun representasi yang semakin abstrak. Konsekuensi langsung dari susunan berlapis tersebut adalah terbentuknya hierarki feature map, di mana karakteristik aktivasi berubah secara sistematis seiring bertambahnya kedalaman jaringan.

- **Lapisan Awal**: Filter didominasi oleh detektor elemen geometris dan intensitas dasar. Responsnya kuat terhadap tepi, orientasi garis, gradien intensitas, serta pola warna dan tekstur sederhana. Fitur ini bersifat sangat lokal dan belum mengandung makna semantik yang signifikan.
- **Lapisan Menengah**: Terjadi komposisi fitur lokal menjadi struktur yang lebih koheren. Lapisan ini mulai mendeteksi pola tekstur kompleks, sudut, hingga bagian-bagian objek seperti roda, daun, atau wajah. Representasi mulai bergeser dari piksel murni menuju komponen struktural parsial.
- **Lapisan Dalam**: Abstraksi mencapai tingkat konfigurasi bagian dan bentuk global. Neuron pada lapisan ini merespons proporsi, siluet, atau hubungan spasial antar komponen yang secara statistik sangat berkorelasi dengan kelas atau tugas spesifik. Ciri-ciri ini jauh lebih invariant terhadap variasi iluminasi dan deformasi kecil.

Perlu ditekankan bahwa hierarki ini merupakan kecenderungan empiris yang konsisten diamati melalui teknik visualisasi dan probing, bukan hukum kaku yang menjamin setiap channel selalu memiliki makna tunggal yang mudah dinamisasi. Pada arsitektur modern yang sangat dalam, banyak neuron justru merepresentasikan kombinasi multi-konsep atau pola yang sulit diuraikan secara intuitif. Kesadaran kritis ini penting dalam evaluasi model, khususnya ketika melakukan interpretasi hasil atau debugging representasi internal pada penelitian tingkat lanjut.

Dinamika hierarki feature map ini secara langsung beririsan dengan konsep wilayah pengaruh neuron. Bagaimana lapisan awal hanya menangkap konteks lokal, sementara lapisan dalam membutuhkan integrasi informasi dari area yang jauh lebih luas, akan kita bedah lebih lanjut pada pembahasan mengenai pertumbuhan receptive field secara bertahap.

---

## Slide 014 - Receptive Field Membesar Bertahap

### Narasi

Konsep *receptive field* atau bidang reseptif merujuk pada wilayah pada citra input yang secara langsung maupun tidak langsung memengaruhi satu nilai aktivasi tertentu di lapisan jaringan saraf. Jika pada slide sebelumnya kita membahas bagaimana fitur visual berkembang secara hierarkis dari tepi sederhana hingga bentuk kompleks, maka konsep ini menjelaskan mekanisme struktural di balik perkembangan tersebut. Setiap neuron pada lapisan konvolusi hanya “melihat” sebagian kecil dari citra asli, dan informasi tersebut harus diteruskan melalui beberapa tahap pemrosesan untuk mencapai konteks yang lebih luas.

Proses pembesaran *receptive field* terjadi secara bertahap seiring bertambahnya kedalaman arsitektur. Seperti yang digambarkan dalam skema slide, lapisan awal hanya mengintegrasikan informasi lokal seperti tekstur atau gradien intensitas. Saat data mengalir ke lapisan tengah, operasi konvolusi menggabungkan beberapa wilayah lokal menjadi representasi yang lebih abstrak. Di lapisan dalam, gabungan dari banyak operasi bertumpuk memungkinkan satu neuron mengakses konteks spasial yang jauh lebih luas, sehingga mampu merepresentasikan bagian objek atau bahkan seluruh struktur gambar.

Implikasi penting dari sifat ini adalah bahwa arsitektur CNN tidak dirancang untuk menghubungkan semua lokasi citra secara langsung pada satu lapisan. Ketergantungan jarak jauh (*long-range dependency*) harus dibangun secara bertingkat melalui penumpukan lapisan. Penggunaan operasi *pooling* atau peningkatan *stride* secara eksplisit mempercepat pertumbuhan *receptive field* karena mengurangi resolusi spasial sambil mempertahankan cakupan area input yang lebih besar. Hal ini menjadi pertimbangan desain krusial dalam menyeimbangkan antara preservasi detail halus dan ekstraksi konteks global.

Perlu dicatat bahwa *receptive field* teoretis yang dihitung berdasarkan rumus geometris tidak selalu mencerminkan pengaruh aktual dalam praktik. Kontribusi masing-masing piksel dalam wilayah *receptive field* sangat bergantung pada bobot yang dipelajari selama pelatihan, serta mekanisme regulasi dan aliran gradien. Pada tingkat penelitian doktoral, pemahaman ini menjadi dasar untuk mengevaluasi mengapa arsitektur modern sering menambahkan komponen seperti *dilated convolution*, *attention mechanism*, atau *skip connection* guna mengatasi keterbatasan pertumbuhan *receptive field* yang lambat pada CNN murni.

Pembahasan ini menjadi landasan langsung untuk arsitektur CNN sederhana yang akan kita implementasikan pada slide berikutnya. Desain yang terdiri dari blok konvolusi, normalisasi batch, aktivasi non-linear, dan *pooling* secara sistematis memanfaatkan prinsip pembesaran *receptive field* ini. Pada sesi praktikum, mahasiswa akan mengamati bagaimana perubahan dimensi tensor mengikuti pola tersebut, serta membedakan karakteristik *handcrafted feature* dengan representasi yang benar-benar dipelajari oleh jaringan. Visualisasi *feature map* dan kurva pembelajaran akan memberikan bukti empiris terhadap hubungan antara kedalaman arsitektur, ukuran *receptive field*, dan kapasitas model dalam menangkap pola visual.

---

## Slide 015 - Arsitektur Simple CNN

### Narasi

Slide ini menyajikan arsitektur dasar Convolutional Neural Network yang akan menjadi kerangka kerja utama dalam eksperimen praktikum kita. Diagram menunjukkan alur pemrosesan data secara hierarkis, dimulai dari input citra RGB yang kemudian melewati tiga blok konvolusi berulang. Setiap blok terdiri dari lapisan konvolusi untuk ekstraksi fitur spasial, diikuti oleh Batch Normalization guna menstabilkan distribusi aktivasi, fungsi aktivasi ReLU untuk introduksi non-linearitas, dan operasi pooling untuk reduksi dimensi spasial. Desain ini merupakan implementasi langsung dari konsep yang telah dibahas pada slide sebelumnya, yaitu bagaimana receptive field membesar secara bertahap melalui akumulasi lapisan konvolusi dan pooling, sehingga jaringan mampu menggabungkan konteks lokal menjadi representasi yang lebih global.

Setelah melewati tiga blok konvolusi, arsitektur ini menghindari penggunaan fully connected layer tradisional. Sebagai gantinya, kita menerapkan Global Average Pooling yang merangkum seluruh informasi spasial menjadi satu vektor per channel. Vektor ringkas ini kemudian dilewatkan ke linear classifier untuk menghasilkan distribusi probabilitas kelas akhir. Pemilihan Global Average Pooling bukan sekadar alternatif teknis, melainkan keputusan desain yang secara signifikan mengurangi jumlah parameter trainable, menekan risiko overfitting, dan meningkatkan robustness model terhadap variasi posisi objek dalam citra.

Tujuan eksperimental yang harus dicapai dalam praktikum ini mencakup empat aspek metodologis:
- Melacak perubahan bentuk tensor di setiap tahap pipeline untuk memahami dinamika reduksi spasial dan ekspansi kanal;
- Menganalisis learning curve selama training untuk mengevaluasi stabilitas konvergensi, kecepatan pembelajaran, dan deteksi dini fenomena underfitting atau overfitting;
- Melakukan visualisasi feature map pada lapisan tengah guna menginterpretasi karakteristik representasi yang dipelajari, mulai dari deteksi tepi, tekstur, hingga komponen objek parsial;
- Melakukan analisis komparatif kritis antara handcrafted feature yang dirancang secara manual versus learned feature yang diekstraksi secara otomatis, menyoroti efisiensi dan adaptabilitas representasi berbasis deep learning.

Arsitektur sederhana ini sengaja dipilih sebagai baseline agar mekanisme propagasi forward dan transformasi representasi dapat dilacak secara transparan tanpa noise dari kompleksitas berlebihan. Prinsip desain yang sama tetap menjadi fondasi bagi arsitektur state-of-the-art yang akan kita tinjau pada kajian paper lanjutan. Keluaran probabilistik dari linear classifier ini nantinya akan menjadi variabel dependen dalam perhitungan error, yang akan kita bahas secara mendalam pada slide berikutnya mengenai mekanisme pembelajaran berbasis loss function dan algoritma optimasi parameter.

---

## Slide 016 - Model Belajar dari Loss

### Narasi

Pada slide ini, kita membahas mekanisme fundamental bagaimana model jaringan saraf, termasuk arsitektur CNN yang telah diuraikan pada slide sebelumnya, sebenarnya belajar dari data. Proses pembelajaran ini tidak terjadi secara otomatis, melainkan didorong oleh optimisasi fungsi kerugian (*loss function*). Ingat kembali tujuan praktikum pada slide 15 mengenai pengamatan *learning curve* dan pembedaan antara fitur buatan tangan (*handcrafted*) dengan fitur yang dipelajari (*learned feature*). Fitur yang dipelajari tersebut adalah hasil akhir dari iterasi berulang di mana model menyesuaikan bobotnya untuk meminimalkan nilai kerugian.

Mari kita bedah alur komputasi yang ditampilkan dalam diagram *Training Loop*:
```text
Input + Label
    ↓
Forward pass
    ↓
Prediction
    ↓
Hitung loss
    ↓
Backpropagation
    ↓
Update parameter
```
Alur ini membentuk siklus tertutup yang menjadi inti dari pelatihan model. Pada tahap *forward pass*, tensor citra masuk melalui lapisan-lapisan arsitektur CNN untuk menghasilkan prediksi. Hasil prediksi ini kemudian dibandingkan dengan label *ground truth* melalui perhitungan *loss*. Nilai kerugian yang dihasilkan akan diteruskan ke tahap *backpropagation*, di mana aturan rantai (*chain rule*) digunakan untuk menghitung gradien setiap parameter terhadap fungsi kerugian tersebut. Terakhir, optimizer menggunakan gradien ini untuk melakukan *update parameter*, biasanya melalui metode seperti Stochastic Gradient Descent (SGD) atau Adam. Siklus ini diulang selama ribuan hingga jutaan langkah hingga konvergensi tercapai.

Untuk tugas klasifikasi visual, fungsi kerugian yang paling umum digunakan adalah *Cross-Entropy*. Rumusnya dapat dituliskan sebagai berikut:
```text
L = -log p(y | x)
```
Di sini, $p(y | x)$ merepresentasikan probabilitas yang diberikan model terhadap kelas yang benar ($y$) berdasarkan input citra ($x$). Secara matematis, fungsi logaritma negatif memberikan penalti yang sangat besar ketika model memberikan probabilitas rendah pada kelas yang seharusnya benar, namun penaltinya mendekati nol jika model sudah yakin dan benar. Sifat ini memaksa model untuk tidak hanya menebak benar, tetapi juga meningkatkan kepercayaan (*confidence*) prediksinya. Dalam konteks penelitian tingkat doktoral, pemahaman mendalam tentang sifat kurva *cross-entropy* ini penting ketika Anda merancang eksperimen atau mengembangkan varian loss baru, misalnya untuk menangani ketidakseimbangan kelas (*class imbalance*), *label noise*, atau masalah kalibrasi probabilitas pada model deteksi dan segmentasi.

Penting untuk dicatat bahwa meskipun *training loop* ini mengoptimalkan kerugian pada data pelatihan, penurunan nilai *loss* yang terus-menerus tidak selalu menjamin peningkatan kemampuan generalisasi model. Justru, tren penurunan *loss* pada satu set data harus dipantau secara kritis terhadap performa pada set data lain. Hal ini membawa kita secara natural ke pembahasan berikutnya, yaitu mengapa pemisahan data menjadi *training*, *validation*, dan *test* memiliki fungsi yang berbeda dan tidak boleh saling menggantikan, serta risiko metodologis yang sering muncul jika prinsip ini dilanggar dalam desain eksperimen.

---

## Slide 017 - Training, Validation, dan Test Berbeda Fungsi

### Narasi

Setelah memahami mekanisme pembelajaran model melalui perhitungan loss pada slide sebelumnya, kita perlu menggarisbawahi bahwa pembagian data menjadi tiga subset—training, validation, dan test—memiliki fungsi yang secara fundamental berbeda. Tabel pada slide ini merangkum peran masing-masing split beserta batasannya dalam memengaruhi model. Set training berfungsi untuk memperbarui parameter model secara langsung melalui backpropagation. Sebaliknya, set validation tidak mengubah bobot model melalui gradien, melainkan berperan sebagai kompas untuk memilih epoch terbaik dan menyetel hyperparameter. Sementara itu, set test hanya digunakan sekali di akhir proses untuk memberikan estimasi objektif terhadap kemampuan generalisasi model ke data yang benar-benar belum pernah dilihat.

Dalam praktik penelitian tingkat lanjut, kesalahan umum sering kali muncul ketika batas antara ketiga subset ini kabur. Risiko kritis yang perlu diwaspadai meliputi:
- memilih model berdasarkan akurasi test set;
- mencoba banyak konfigurasi secara berulang pada data test;
- melakukan preprocessing sebelum pembagian data dilakukan;
- membiarkan sampel dari subjek yang sama tersebar antarsplit.

Prinsip utamanya sangat tegas: test set bukan merupakan validation set tambahan. Setiap keputusan arsitektural atau pemilihan hyperparameter harus sepenuhnya didasarkan pada feedback dari validation set, sehingga test set tetap terjaga kemurniannya sebagai benchmark akhir. Evaluasi yang bocor atau bias akan menghasilkan klaim novelty yang lemah dan sulit direproduksi, sebuah masalah fatal dalam desain eksperimen riset doktoral.

Pemahaman ketat mengenai isolasi data ini menjadi fondasi krusial sebelum kita membahas teknik modifikasi data seperti augmentasi. Pada slide berikutnya, kita akan mengeksplorasi bagaimana augmentasi membawa asumsi implisit tentang struktur data dan domain aplikasi. Validitas setiap transformasi harus dipertimbangkan secara kritis agar tidak melanggar prinsip independensi data maupun memperkenalkan bias yang justru merusak performa model pada tahap evaluasi akhir.

---

## Slide 018 - Augmentasi Membawa Asumsi

### Narasi

Setiap teknik augmentasi data sebenarnya membawa serangkaian asumsi implisit tentang struktur dan sifat dunia visual yang ingin dimodelkan. Augmentasi bukan sekadar trik untuk menambah jumlah sampel, melainkan pernyataan eksplisit mengenai invariansi yang kita harapkan dari model.

Operasi standar yang sering kita terapkan meliputi:
- horizontal flip;
- random crop;
- rotasi;
- color jitter;
- blur atau noise.

Secara umum, tujuan penerapannya adalah menambah variasi tampilan, mengurangi overfitting, dan menyatakan invariance yang diharapkan. Namun, di tingkat penelitian, kita harus menyadari bahwa augmentasi yang valid sangat bergantung pada karakteristik domain.

Berikut adalah peringatan kritis yang perlu dipertimbangkan saat merancang pipeline preprocessing:
- flip mungkin tidak valid untuk tulisan;
- rotasi besar mungkin tidak valid untuk citra medis tertentu;
- perubahan warna dapat merusak informasi diagnostik.

Kesalahan dalam memilih asumsi augmentasi dapat menggeser distribusi data secara diam-diam, sehingga mengaburkan evaluasi generalisasi model. Hal ini memperkuat prinsip dari slide sebelumnya bahwa test set bukan validation set tambahan, dan setiap manipulasi data harus dilakukan dengan transparansi penuh setelah proses split untuk mencegah kebocoran informasi.

Setelah memastikan bahwa transformasi data sesuai dengan asumsi domain, tantangan berikutnya bergeser ke arsitektur jaringan itu sendiri. Agar model mampu mengekstrak fitur secara robust dari data yang telah di-augmentasi, diperlukan mekanisme yang menjaga stabilitas aliran gradien pada lapisan yang lebih dalam. Pembahasan mengenai residual connection akan menjadi fondasi penting untuk menjawab tantangan tersebut, sebagaimana akan kita bahas pada slide berikutnya.

---

## Slide 019 - Residual Connection Membantu Jaringan Dalam

### Narasi

Pada pembahasan sebelumnya, kita telah menyoroti bahwa augmentasi data membawa serangkaian asumsi mengenai invariansi yang diharapkan dari model. Namun, ketika upaya peningkatan kapasitas model dilakukan dengan menambah kedalaman jaringan, muncul hambatan mendasar yang tidak selalu teratasi hanya melalui teknik regularisasi atau manipulasi data. Masalah utamanya terletak pada fakta bahwa jaringan yang lebih dalam tidak serta-merta lebih mudah dioptimalkan. Sebaliknya, tanpa mekanisme khusus, proses pelatihan sering kali mengalami degradation problem, di mana error pelatihan justru meningkat seiring bertambahnya layer, meskipun kapasitas representasional model seharusnya lebih tinggi.

Untuk memecahkan kebuntuan optimasi ini, residual connection diperkenalkan sebagai struktur arsitektural yang mengubah cara informasi mengalir maju dan mundur. Blok residual didefinisikan secara matematis sebagai:

```text
y = F(x) + x
```

Dalam formulasi ini, `x` merupakan sinyal input yang dilewatkan langsung, sedangkan `F(x)` mewakili transformasi nonlinier yang dipelajari oleh rangkaian lapisan di dalam blok. Operasi penjumlahan ini menciptakan jalur bypass yang memastikan sinyal asli tetap tersampaikan ke lapisan berikutnya, terlepas dari seberapa kompleks transformasi `F(x)` yang dihasilkan.

Keberadaan jalur identitas ini memberikan dampak signifikan terhadap dinamika pelatihan jaringan dalam:
- Mempercepat dan menstabilkan aliran gradien selama backpropagation, sehingga mitigasi masalah vanishing gradient menjadi lebih robust.
- Menggeser tujuan pembelajaran dari pemetaan input-output penuh menjadi pembelajaran residual atau koreksi terhadap representasi input.
- Menjadikan fungsi identitas sebagai kasus trivial yang dapat dicapai dengan cepat, karena model cukup mengarahkan `F(x)` menuju nilai nol jika transformasi tambahan tidak diperlukan.
- Membangun fondasi arsitektural yang memungkinkan pembangunan jaringan dengan ratusan hingga ribuan layer tanpa kehilangan kemampuan konvergensinya.

Prinsip residual connection ini bukan sekadar perbaikan teknis, melainkan pergeseran paradigma dalam desain arsitektur deep learning. Implementasinya secara sistematis diadopsi dalam arsitektur standar seperti ResNet-18 dan ResNet-50, yang akan kita analisis lebih mendalam pada pertemuan berikutnya. Pemahaman tentang bagaimana koneksi residual menstabilkan optimasi juga menjadi kunci untuk mengevaluasi trade-off antara kedalaman model dan efisiensi komputasi. Lebih lanjut, mekanisme ini berinteraksi erat dengan asumsi bawaan arsitektur itu sendiri, yang akan kita diskusikan pada slide selanjutnya mengenai inductive bias CNN dan implikasinya terhadap generalisasi model.

---

## Slide 020 - Inductive Bias CNN

### Narasi

Setelah membahas bagaimana residual connection mengatasi masalah degradasi pelatihan pada jaringan yang sangat dalam, kita kini beralih ke fondasi arsitektural yang membuat Convolutional Neural Network secara inheren efektif untuk pemrosesan visual. Efisiensi ini tidak muncul secara kebetulan, melainkan didorong oleh serangkaian asumsi bawaan yang secara akademis disebut sebagai inductive bias. Asumsi ini secara eksplisit memandu cara jaringan mempelajari representasi dari data piksel mentah tanpa memerlukan penalaan manual yang masif.

Empat asumsi utama membentuk tulang punggung desain CNN:
- **Lokalitas:** Tetangga spasial dianggap penting, sehingga operasi konvolusi hanya memproses jendela piksel yang berdekatan untuk mengekstrak fitur kontekstual.
- **Weight sharing:** Pola visual seperti tepi, tekstur, atau bentuk dasar dapat muncul di lokasi berbeda, sehingga parameter filter yang sama diterapkan secara universal di seluruh citra.
- **Translasi ekuivariansi:** Pergeseran input secara langsung menggeser feature map yang dihasilkan, menjaga konsistensi representasi fitur terlepas dari posisi absolut objek.
- **Hierarki:** Pola kompleks tersusun secara progresif dari pola sederhana pada lapisan awal menuju abstraksi tingkat tinggi pada lapisan akhir.

Konsekuensi langsung dari penerapan inductive bias ini sangat signifikan terhadap efisiensi pembelajaran model. CNN terbukti relatif data-efficient karena tidak perlu mempelajari hubungan antar-piksel secara acak, melainkan mengikuti struktur geometris dan statistik citra alami. Hal ini menjadikannya baseline yang sangat kuat bahkan ketika tersedia dataset berukuran kecil hingga menengah. Namun, mekanisme ini juga membawa implikasi struktural yang perlu disadari: hubungan konteks jarak jauh tidak dimodelkan secara instan, melainkan harus dirakit secara bertahap melalui propagasi berlapis dan akumulasi receptive field.

Pendekatan bertahap inilah yang nantinya akan kita evaluasi batasannya secara kritis. Ketika suatu tugas memerlukan pemahaman konteks yang meluas di luar jangkauan lokal, akumulasi lapisan konvolisasi saja sering kali kurang optimal atau membutuhkan kedalaman jaringan yang tidak efisien. Pada slide berikutnya, kita akan menguji mengapa mekanisme global menjadi diperlukan, serta bagaimana konsep self-attention dalam Transformer menawarkan solusi langsung untuk menangkap dependensi jarak jauh tanpa bergantung pada rantai komputasi berlapis.

---

## Slide 021 - Mengapa Kita Memerlukan Mekanisme Global?

### Narasi

Pada slide sebelumnya, kita telah menguraikan bagaimana bias induktif Convolutional Neural Network—seperti lokalitas, *weight sharing*, ekuivariansi translasi, dan hierarki fitur—memberikan efisiensi data yang signifikan dan menjadi baseline yang kuat untuk banyak tugas pengolahan citra konvensional. Namun, asumsi-asumsi ini juga membawa batasan struktural ketika kita menghadapi representasi visual yang memerlukan pemahaman hubungan jarak jauh atau konteks spasial yang tersebar luas. Di sinilah pertanyaan mendasar muncul: mengapa arsitektur berbasis konvolusi saja tidak lagi cukup untuk memenuhi tuntutan model komputer visi generasi terbaru?

Kebutuhan akan mekanisme pemrosesan global terlihat jelas dalam berbagai skenario nyata yang menuntut integrasi informasi lintas wilayah:
- Bagian objek yang terpisah secara spasial namun tetap membentuk satu entitas semantik.
- Konteks antara objek utama dengan lingkungan sekitarnya untuk disambiguasi makna.
- Struktur global pada citra medis, seperti keterkaitan antar organ atau lesi yang tersebar.
- Pola wilayah luas pada citra satelit yang memerlukan pemahaman topologi regional.
- Hubungan antarelemen dokumen yang saling terkait meskipun letaknya berjauhan.

Meskipun CNN secara teoretis mampu menjangkau area global melalui penumpukan lapisan (*stacking*), hal ini memerlukan kedalaman jaringan yang masif. Informasi harus melewati banyak operasi konvolusi berturut-turut untuk membangun representasi kontekstual, yang secara praktis memperlambat konvergensi, meningkatkan beban komputasi, dan rentan terhadap degradasi sinyal. Transformator menawarkan paradigma alternatif dengan memperkenalkan mekanisme *self-attention* yang memungkinkan setiap elemen dalam urutan visual berinteraksi langsung dengan elemen lainnya tanpa bergantung pada propagasi berlapis. Pendekatan ini secara fundamental mengubah cara model membangun dependensi, menjadikannya lebih efisien untuk menangkap konteks holistik.

Pergeseran dari bias induktif lokal ke konektivitas global ini bukan sekadar perubahan arsitektural, melainkan fondasi kritis bagi pengembangan Vision Transformer, self-supervised learning, hingga foundation models yang kini mendominasi benchmark state-of-the-art. Untuk memahami bagaimana mekanisme global ini diimplementasikan secara teknis, kita perlu melihat unit dasar yang diproses oleh transformator. Slide berikutnya akan membahas bagaimana transformator bekerja pada token, mencakup bentuk tensor input standar, definisi token sebagai unit informasi diskrit, serta cara embedding numerik merepresentasikan setiap token tersebut dalam domain visi komputer.

---

## Slide 022 - Transformer Bekerja pada Token

### Narasi

Pada slide sebelumnya, kita telah menelaah mengapa mekanisme global menjadi kebutuhan mendesak dalam arsitektur pengolahan citra modern. Keterbatasan jalur lokal pada jaringan konvolusional dapat diatasi dengan memungkinkan setiap elemen citra berinteraksi secara langsung. Transformator menjawab tantangan ini melalui self-attention, namun sebelum masuk ke mekanisme perhitungan attention, kita harus memahami bagaimana data visual pertama-tama dikonstruksi agar kompatibel dengan model.

Transformator tidak menerima piksel mentah atau grid gambar secara langsung. Model ini bekerja pada unit-unit diskrit yang disebut token. Setiap token berfungsi sebagai satuan informasi dasar yang akan melewati serangkaian lapisan transformator. Secara struktural, tensor input standar mengikuti notasi Batch × Jumlah Token × Dimensi Embedding, atau disingkat B × N × D. Dimensi B merepresentasikan ukuran batch selama training atau inference, N adalah jumlah total token yang diekstrak per sampel, dan D menentukan dimensi ruang vektor tempat setiap token dikodekan.

Token itu sendiri hanyalah wadah informasi, sedangkan embedding adalah vektor numerik berdimensi D yang mengonversi token tersebut menjadi representasi kontinu. Representasi ini memungkinkan operasi aljabar linear berjalan stabil dan memberikan fondasi numerik bagi seluruh lapisan feed-forward maupun attention di dalam jaringan.

Dalam domain pengolahan citra digital, strategi pembentukan token bersifat fleksibel dan sangat bergantung pada karakteristik data serta tujuan penelitian. Pendekatan yang umum digunakan meliputi:
- Partisi citra menjadi patch berukuran tetap yang kemudian di-flatten dan diproyeksikan ke ruang embedding;
- Penggunaan region proposal atau bounding box dari detektor sebagai token spasial terstruktur;
- Ekstraksi feature map dari tahap akhir CNN untuk dimanfaatkan sebagai token bertingkat;
- Integrasi token visual dengan token teks atau modalitas lain dalam kerangka multimodal.

Pemilihan skema tokenisasi ini akan berdampak langsung pada trade-off antara resolusi spasial, beban komputasi, dan kapasitas model menangkap dependensi jangka panjang. Setelah seluruh komponen visual dikonversi menjadi embedding berdimensi D, transformator siap menerapkan proyeksi linear untuk menghasilkan matriks Query, Key, dan Value. Formulasi matematis beserta intuisi di balik ketiga komponen tersebut akan kita bedah secara mendalam pada slide berikutnya.

---

## Slide 023 - Query, Key, dan Value

### Narasi

Pada slide ini, kita mengurai komponen fundamental dari mekanisme *self-attention*, yaitu Query, Key, dan Value. Sebagai penghubung dari slide sebelumnya, ingat bahwa input citra telah dikonversi menjadi sekumpulan token atau patch yang direpresentasikan sebagai matriks embedding $X$ dengan dimensi $B \times N \times D$. Dari representasi awal inilah, model membangun tiga jalur pemrosesan paralel.

Secara matematis, setiap baris vektor pada $X$ diproyeksikan ke tiga ruang fitur berbeda menggunakan perkalian matriks linear:
```text
Q = XWQ
K = XWK
V = XWV
```
Di sini, $WQ$, $WK$, dan $WV$ adalah matriks bobot yang ukurannya disesuaikan dengan dimensi target (biasanya $D \times dk$ atau $D \times dv$). Bobot-bobot ini tidak ditetapkan secara manual, melainkan dioptimalkan secara end-to-end bersama seluruh arsitektur jaringan selama fase pelatihan.

Untuk memahami fungsi masing-masing komponen, perhatikan intuisi berikut:
- **Query** mewakili pertanyaan internal model: *“Informasi apa yang sedang saya cari pada posisi ini?”*
- **Key** berperan sebagai indeks atau ciri pembeda: *“Informasi seperti apa yang saya tawarkan kepada elemen lain?”*
- **Value** menyimpan konten aktual: *“Informasi apa yang akan saya lempar ke output setelah tingkat relevansinya dihitung?”*

Pesan teknis yang krusial untuk kajian tingkat doktoral adalah bahwa Q, K, dan V bukan metadata statis yang dirancang oleh peneliti. Ketiganya merupakan proyeksi adaptif yang dipelajari langsung dari data. Selama pelatihan, bobot $WQ$, $WK$, dan $WV$ akan berkembang untuk mengekstrak aspek fungsional yang berbeda—mulai dari deteksi tepi, struktur geometris, hingga konteks semantik—tergantung pada distribusi data dan tugas yang dihadapi.

Mekanisme proyeksi ini menyiapkan landasan bagi langkah selanjutnya yang akan dibahas pada slide berikutnya, yaitu perhitungan *Scaled Dot-Product Attention*. Setelah Q, K, dan V terbentuk, model akan melakukan operasi dot-product antar-query dan key, menerapkan faktor penskalaan $\sqrt{dk}$, lalu menormalisasikannya dengan softmax untuk menghasilkan matriks bobot perhatian. Pemahaman bahwa ketiganya bersifat *learnable* menjadi kunci dalam menganalisis kapasitas Transformer menangkap ketergantungan jangka panjang (*long-range dependencies*), serta menjadi dasar evaluasi kritis terhadap arsitektur Vision Transformer, model berbasis DINO/DINOv2, dan pendekatan *self-supervised learning* lainnya dalam riset pengolahan citra mutakhir.

---

## Slide 024 - Scaled Dot-Product Attention

### Narasi

Setelah memahami bahwa Query, Key, dan Value merupakan proyeksi linear yang dipelajari secara end-to-end dari input, langkah selanjutnya adalah menggabungkan ketiganya melalui mekanisme Scaled Dot-Product Attention. Rumus fundamental yang mendefinisikan operasi ini adalah `Attention(Q,K,V) = softmax(QKᵀ / √dk) V`. Ekspresi ini bukan sekadar manipulasi aljabar, melainkan jantung dari cara arsitektur Transformer mengekspresikan dependensi kontekstual antar elemen representasi visual secara paralel.

Perhitungan dapat dipecah menjadi empat tahap operasional yang berurutan:
1. Bandingkan query dengan seluruh key melalui operasi dot product untuk mengukur tingkat kesamaan atau relevansi spasial-semantik.
2. Skala skor hasil dot product dengan faktor `1/√dk`, di mana `dk` menyatakan dimensi vektor key.
3. Normalisasi skor yang telah diskalakan menggunakan fungsi softmax agar menghasilkan distribusi probabilitas yang valid.
4. Gunakan bobot probabilitas tersebut sebagai koefisien dalam penjumlahan tertimbang terhadap matriks Value.

Hasil komputasi ini membentuk struktur yang dikenal sebagai Attention Matrix. Setiap baris pada matriks merepresentasikan satu query spesifik, sementara setiap kolom menunjukkan key yang mendapat perhatian dari query tersebut. Karena sifat normalisasi softmax, jumlah total bobot pada setiap baris akan selalu bernilai tepat satu. Perlu ditekankan bahwa hubungan yang terekam bersifat terarah; fokus perhatian dari token A ke token B tidak diwajibkan sama dengan arah sebaliknya, sehingga matriks ini umumnya tidak simetris.

Mekanisme pengskalan pada `√dk` ini memegang peranan kritis terhadap stabilitas pelatihan model. Ketika dimensi fitur `dk` bernilai besar, magnitudo dot product cenderung meluas secara eksponensial. Tanpa faktor skala, fungsi softmax akan terjebak di daerah ekor distribusi yang sangat tajam, memicu masalah gradien yang hilang atau meledak selama backpropagation. Pembahasan lebih lanjut mengenai justifikasi numerik scaling dan dampaknya terhadap konvergensi optimasi akan diurai secara detail pada slide berikutnya.

---

## Slide 025 - Mengapa Skor Perlu Diskalakan?

### Narasi

Pada slide sebelumnya, kita telah menguraikan rumus dasar *Scaled Dot-Product Attention*, di mana kesamaan antara query dan key dihitung melalui operasi dot product, lalu digunakan sebagai bobot untuk menggabungkan value. Namun, penerapan langsung dari operasi ini pada dimensi vektor yang besar menimbulkan tantangan numerik yang signifikan. Ketika dimensi key ($d_k$) meningkat, magnitudo hasil dot product cenderung membesar secara proporsional, yang secara langsung memengaruhi perilaku fungsi aktivasi berikutnya.

Tanpa mekanisme penskalaan, distribusi probabilitas yang dihasilkan oleh softmax akan menjadi sangat tajam. Sebagian besar bobot perhatian akan terkonsentrasi mendekati nilai satu, sementara sisanya terdorong mendekati nol. Kondisi saturasi ini menyebabkan gradien yang mengalir selama backpropagation menjadi sangat kecil atau bahkan hilang, sehingga proses optimisasi menjadi tidak stabil dan konvergensi pelatihan terhambat.

Solusi yang diterapkan adalah membagi skor dot product dengan faktor $\sqrt{d_k}$ sebelum dimasukkan ke dalam softmax. Pembagian ini berfungsi menormalkan varians skor, sehingga menjaga sebagian besar nilai probabilitas berada pada rentang sensitif di mana turunan fungsi softmax masih bermakna. Dengan demikian, aliran gradien tetap informatif dan dinamika pembelajaran dapat distabilkan meskipun dimensi representasi visual semakin tinggi.

Penting untuk ditegaskan bahwa penskalaan ini bukan merupakan mekanisme seleksi atau penentuan token mana yang lebih penting secara semantik. Fungsi utamanya murni bersifat teknis-operasional: menjaga stabilitas numerik dan memfasilitasi optimisasi yang lebih efisien. Pemahaman ini menjadi fondasi krusial sebelum kita beralih ke struktur arsitektur yang lebih kompleks untuk menangkap ketergantungan spasial maupun semantik antar patch citra.

Setiap unit perhatian yang telah distabilkan ini kemudian menjadi blok penyusun fundamental. Pada slide berikutnya, kita akan melihat bagaimana blok-blok tersebut dikembangkan menjadi *Multi-Head Attention*. Mekanisme ini memecah embedding awal menjadi beberapa subspesies paralel, masing-masing mempelajari hubungan yang berbeda, sebelum hasilnya digabungkan dan diproyeksikan kembali. Perlu dicatat bahwa meskipun arsitektur ini meningkatkan kapasitas representasi, tidak setiap head secara otomatis menghasilkan interpretasi yang mudah dipahami secara intuitif.

---

## Slide 026 - Multi-Head Attention Menangkap Banyak Hubungan

### Narasi

Setelah kita memastikan stabilitas numerik pada skor perhatian melalui penskalaan $\sqrt{d_k}$, langkah logis selanjutnya adalah meningkatkan kapasitas representasi model dengan mengadopsi *Multi-Head Attention*. Mekanisme ini mengubah paradigma dari satu jalur proyeksi tunggal menjadi paralelisasi subruang yang lebih kaya. Embedding input secara dinamis dibagi menjadi beberapa *head*, di mana setiap *head* mempertahankan matriks proyeksi *Query*, *Key*, dan *Value* yang independen. Pendekatan ini memungkinkan model mengeksploitasi berbagai dimensi fitur secara simultan tanpa saling mengganggu.

Alur komputasinya dapat dipahami melalui skema berikut:
```text
Input
 ├─ Head 1 → hubungan tertentu
 ├─ Head 2 → hubungan lain
 ├─ Head 3 → hubungan lain
 └─ Head 4 → hubungan lain
       ↓
Concatenate → Linear Projection
```
Keluaran dari masing-masing *head* diproses secara terpisah melalui fungsi *scaled dot-product attention*, kemudian digabungkan (*concatenate*) sepanjang dimensi fitur. Hasil penggabungan ini akhirnya dilewatkan ke lapisan linear tunggal untuk mengembalikan dimensi output sesuai dengan input awal. Struktur ini memberikan fleksibilitas arsitektural yang tinggi, di mana setiap *head* dapat secara implisit belajar untuk mendeteksi pola yang berbeda, mulai dari ketergantungan semantik lokal hingga relasi struktural global.

Namun, perlu adanya kewaspadaan analitis mengingat kompleksitas yang muncul. Tidak setiap *head* secara otomatis mempelajari fungsi atau pola yang mudah diinterpretasikan secara intuitif. Dalam literatur terkini, banyak *head* yang menunjukkan redundansi fungsional, sementara yang lain mengkodekan representasi yang sangat terspesialisasi namun sulit dilacak. Untuk penelitian tingkat doktoral, evaluasi peran masing-masing *head* sebaiknya didukung oleh metode analisis mekanistik seperti *attention rollout*, *probing classifiers*, atau dekomposisi singular value, agar klaim arsitektural dapat dibuktikan secara empiris dan teoretis.

Meskipun *Multi-Head Attention* berhasil memperluas cakupan relasional antar token, mekanisme ini tetap memiliki kelemahan fundamental terkait urutan spasial. Karena sifatnya yang *permutation-invariant*, model tidak secara bawaan mengetahui posisi relatif atau absolut setiap patch atau token. Keterbatasan ini akan segera kita bahas pada slide berikutnya, di mana kita akan mengeksplorasi berbagai strategi *positional encoding* yang diperlukan agar representasi visual dapat memahami struktur geometri citra secara utuh.

---

## Slide 027 - Posisi Tidak Tersedia Secara Bawaan

### Narasi

Merujuk pada pembahasan slide sebelumnya tentang multi-head attention, kita telah melihat bagaimana mekanisme ini mampu menangkap beragam hubungan kontekstual antar token secara paralel. Namun, terdapat satu karakteristik mendasar yang perlu disadari: arsitektur self-attention bersifat *permutation invariant*. Model tidak secara bawaan mengetahui urutan atau koordinat spasial dari setiap token yang dilewatkan. Tanpa informasi tambahan mengenai lokasi, token yang sama akan diproses identik terlepas dari posisinya dalam sekuens, padahal dalam representasi visual, urutan dan letak spasial sangat menentukan makna struktural.

Untuk mengisi celah ini, diperlukan mekanisme *positional encoding* yang menyuntikkan informasi koordinat ke dalam vektor embedding. Pendekatan yang lazim digunakan meliputi:
- *Sinusoidal positional encoding*, yang memanfaatkan fungsi gelombang sinus dan kosinus untuk menghasilkan vektor posisi unik tanpa memerlukan parameter trainable.
- *Learnable positional embedding*, di mana vektor posisi dipelajari langsung selama pelatihan melalui optimisasi gradien.
- *Relative positional bias*, yang menekankan pada jarak relatif antar token daripada posisi absolut dalam grid.
- *Rotary positional embeddings* atau variasinya, yang mengintegrasikan informasi posisi ke dalam ruang proyeksi Q dan K melalui transformasi rotasi, sehingga meningkatkan kapasitas model dalam menangkap dependensi jarak jauh.

Dalam konteks pengolahan citra digital, penyediaan informasi posisi menjadi sangat kritis karena data visual memiliki struktur grid dua dimensi yang terdefinisi jelas. Mekanisme ini membantu model membedakan patch kiri dan kanan, bagian atas dan bawah, serta memahami kedekatan spasial antar region. Akibatnya, representasi yang dihasilkan mampu merekonstruksi susunan objek dan pola global secara lebih akurat, yang merupakan prasyarat utama untuk tugas-tugas vision tingkat lanjut.

Pemahaman mengenai integrasi posisi ini menjadi landasan penting sebelum kita menelaah arsitektur lengkap pada slide berikutnya. Transformer encoder block akan menggabungkan layer normalization, multi-head self-attention, residual connection, dan feed-forward network dengan aktivasi GELU. Informasi posisi umumnya ditambahkan segera setelah tahap embedding awal, memastikan bahwa setiap blok komputasi tetap mempertahankan jejak spasial sambil mengekstrak fitur hierarkis yang semakin abstrak.

---

## Slide 028 - Transformer Encoder Block

### Narasi

Slide ini menguraikan arsitektur inti dari Transformer Encoder Block, yang berfungsi sebagai unit komputasi dasar dalam Vision Transformer dan arsitektur berbasis self-attention lainnya. Setelah pada slide sebelumnya kita membahas bagaimana informasi urutan dan posisi disisipkan melalui positional encoding, kini kita melihat bagaimana token-token tersebut diproses secara bertahap di dalam setiap lapisan encoder.

Alur pemrosesan data mengikuti struktur berurutan berikut:
1. **Input Token** masuk ke **LayerNorm** untuk menstabilkan distribusi aktivasi dan mempercepat konvergensi selama pelatihan.
2. Hasil normalisasi dilewatkan ke **Multi-Head Self-Attention**, yang menghitung bobot ketergantungan global antar semua token dalam sekuens, sehingga model mampu menangkap konteks holistik tanpa dibatasi oleh receptive field lokal.
3. Output attention ditambahkan kembali ke input awal melalui **Add Residual**. Skema ini mencegah degradasi gradien dan memungkinkan aliran informasi yang lebih lancar pada jaringan sangat dalam.
4. Blok kedua dimulai dengan **LayerNorm** lagi, diikuti oleh **MLP + GELU**. Fungsi aktivasi GELU memberikan non-linearitas yang kaya, sementara MLP berperan sebagai proyektor dimensi yang memperkaya kapasitas representasi.
5. Hasil akhirnya kembali melalui **Add Residual** sebelum keluar sebagai **Output Token** yang siap diteruskan ke lapisan berikutnya.

Perlu dicatat bahwa arsitektur ini menunjukkan kesamaan mendasar dengan CNN modern seperti ResNet atau ConvNeXt. Keduanya mengandalkan residual connection untuk memfasilitasi pelatihan jaringan dalam, normalisasi untuk stabilitas optimasi, serta penumpukan banyak blok yang menghasilkan representasi semakin abstrak dan semantik. Konvergensi desain ini menegaskan bahwa prinsip-prinsip arsitektural efektif tidak terikat pada satu paradigma komputasi, melainkan dapat diadaptasi lintas domain pengolahan sinyal visual.

Untuk memahami bagaimana citra asli dapat memasuki blok ini, kita perlu melihat mekanisme konversi dari grid piksel menjadi sekuens token. Pada slide berikutnya, akan dijelaskan bagaimana teknik patching berfungsi sebagai jembatan fundamental antara struktur gambar 2D dan arsitektur sequence-based transformer. Konsep embedding patch ini tidak hanya esensial secara teoretis, tetapi juga langsung diterapkan dalam implementasi praktis seperti DeiT dan berbagai varian Vision Transformer yang akan kita eksplorasi lebih lanjut.

---

## Slide 029 - Patch Menghubungkan Citra dan Transformer

### Narasi

Setelah menelaah struktur internal blok encoder Transformer pada slide sebelumnya, kini kita fokus pada mekanisme transisi yang menjembatani domain piksel dengan arsitektur berbasis attention: pemotongan citra menjadi patch. Berbeda dengan CNN yang secara inheren beroperasi pada grid piksel melalui filter lokal, Transformer membutuhkan transformasi eksplisit agar setiap wilayah citra dapat diperlakukan sebagai elemen diskrit dalam sebuah urutan sekuensial.

Sebagai contoh numerik, jika kita mengambil citra berukuran 32×32 dan membaginya menggunakan patch berukuran 4×4, perhitungan yang terjadi adalah `(32 / 4) × (32 / 4) = 8 × 8 = 64 patch`. Hasil bagi ini menunjukkan bahwa citra asal akan direduksi menjadi 64 token spasial yang saling terpisah. Reduksi resolusi ini bukan sekadar kompresi, melainkan langkah fundamental untuk menyamakan dimensi input dengan kapasitas sequence length yang diharapkan oleh lapisan self-attention.

Proses konversi tersebut dikenal sebagai Patch Embedding, yang alurnya dapat diuraikan sebagai berikut:
- Citra asli dipotong menjadi patch-patch non-overlapping.
- Setiap patch dilakukan flatten menjadi vektor berdimensi tinggi.
- Dilakukan linear projection untuk memetakan vektor hasil flatten ke dalam ruang embedding model.
- Hasil akhirnya adalah patch token siap masuk ke blok Transformer.

Secara implementasi, proses flatten dan linear projection ini dapat dijalankan secara sangat efisien dengan memanfaatkan layer `Conv2d`. Dengan menetapkan ukuran kernel dan stride sama dengan patch size, operasi konvolusi berjalan tanpa overlap dan secara simultan melakukan downsampling serta proyeksi linear. Pendekatan ini mengurangi overhead memori dan mempercepat komputasi dibandingkan menerapkan operasi flattening manual diikuti oleh `nn.Linear` terpisah.

Konsep patch embedding ini menjadi landasan utama pada Praktikum 03, di mana kalian akan mengimplementasikan arsitektur seperti DeiT dan Vision Transformer modern. Pemahaman mendalam tentang bagaimana representasi visual diubah menjadi sequence of tokens akan menentukan keberhasilan kalian dalam menangani masalah kontekstualitas global versus lokal. Pada slide berikutnya, kita akan merangkai seluruh komponen ini ke dalam arsitektur lengkap bernama Tiny Image Transformer, yang berfungsi sebagai model edukatif untuk memvisualisasikan alur end-to-end sebelum beralih ke strategi training skala penuh yang lebih kompleks.

---

## Slide 030 - Tiny Image Transformer sebagai Model Pendidikan

### Narasi

Slide ini memperkenalkan Tiny Image Transformer sebagai arsitektur penyederhanaan yang berfungsi sebagai media edukasi untuk memahami mekanisme inti Vision Transformer. Setelah pada slide sebelumnya kita membahas bagaimana citra dipotong menjadi patch dan diembed menggunakan operasi konvolusi yang efisien, kini kita merangkai komponen-komponen tersebut menjadi alur pemrosesan end-to-end yang utuh.

Arsitektur ini terdiri dari enam elemen utama yang bekerja secara berurutan. Pertama, *patch embedding* mentransformasi blok spasial menjadi vektor fitur berdimensi tetap. Kedua, ditambahkan token khusus `[CLS]` yang posisinya tetap di awal urutan token. Ketiga, setiap token diperkaya dengan *learnable positional embedding* karena arsitektur attention murni tidak memiliki indikasi lokasi spasial bawaan. Keempat, seluruh token diproses melalui beberapa blok *Transformer encoder* yang mengandalkan mekanisme *multi-head self-attention*. Kelima, keluaran melewati *LayerNorm* untuk stabilisasi distribusi aktivasi. Keenam, representasi akhir diteruskan ke *linear classifier* untuk menghasilkan prediksi kelas.

Alur pemrosesan data dapat diikuti sebagai berikut:
- Citra asli dikonversi menjadi sekumpulan *patch tokens*.
- Token `[CLS]` dan vektor posisi digabungkan sebagai input awal.
- Seluruh token melewati rangkaian *Transformer blocks*.
- Vektor output pada indeks `[CLS]` diekstrak sebagai representasi kontekstual global.
- Representasi tersebut diproyeksikan ke ruang kelas melalui lapisan linear.

Perlu ditekankan bahwa model berskala kecil ini sengaja dibatasi fungsinya. Tujuannya bukan untuk mereplikasi strategi pelatihan lengkap dari ViT standar maupun DeiT, melainkan untuk mengisolasi perilaku setiap komponen arsitektural. Dalam konteks riset tingkat doktoral, pemahaman mekanistik ini memungkinkan peneliti melacak propagasi gradien, menganalisis ketergantungan antar token, dan mengevaluasi dampak hyperparameter tanpa noise komputasi dari model skala besar.

Pembahasan mengenai struktur dan tujuan edukatif Tiny Image Transformer ini menjadi jembatan alami menuju perbandingan arsitektural yang lebih luas. Pada slide berikutnya, kita akan membedah perbedaan bias bawaan antara CNN dan Transformer, mencakup aspek unit pemrosesan awal, operasi inti, penanganan posisi, hingga kebutuhan data. Pertanyaan kritis yang akan muncul dari tabel perbandingan tersebut adalah apakah disparitas performa lebih disebabkan oleh karakteristik arsitektur itu sendiri, atau oleh faktor eksternal seperti kualitas dataset, skema pretraining, dan protokol optimasi.

---

## Slide 031 - CNN dan Transformer Membawa Bias Berbeda

### Narasi

Pada slide sebelumnya, kita telah membedah arsitektur Tiny Image Transformer sebagai model edukatif untuk memahami mekanisme patch embedding, token `[CLS]`, dan blok encoder Transformer. Namun, dalam praktik penelitian tingkat lanjut, penting untuk menyadari bahwa arsitektur yang berbeda membawa *inductive bias* yang secara fundamental berbeda. Slide ini menyoroti kontras sistematis antara Convolutional Neural Networks (CNN) dan Vision Transformer, yang sering kali menjadi sumber kebingungan ketika membandingkan performa atau menginterpretasi hasil eksperimen tanpa kontrol metodologis yang ketat.

Berikut adalah kontras mendasar yang perlu diperhatikan dalam desain eksperimen:
- **Unit awal & Operasi utama**: CNN bekerja pada piksel dan tetangga lokal melalui operasi konvolusi, sedangkan Transformer memulai dari token-tokoh patch independen yang diproses oleh mekanisme self-attention.
- **Bias lokal vs Global**: CNN memiliki bias spasial yang kuat karena weight sharing dan receptive field terbatas. Relasi global dibangun secara bertahap melalui penumpukan lapisan. Transformer, sebaliknya, mampu membangun interaksi global secara langsung antartoken, sehingga bias lokalnya jauh lebih lemah.
- **Informasi posisi**: Pada CNN, struktur grid dan operasi shift membuat informasi posisi tersirat secara alami. Transformer tidak memiliki prior spasial bawaan, sehingga memerlukan explicit positional encoding atau bias posisi agar urutan dan lokasi token tetap terjaga.
- **Normalisasi**: CNN umumnya mengandalkan BatchNorm yang menormalisasi statistik spasial dan channel, sedangkan Transformer lebih konsisten dengan LayerNorm yang menormalisasi fitur per sampel.
- **Kebutuhan data**: Karena bias lokal yang kuat, CNN cenderung lebih stabil dan membutuhkan data relatif lebih kecil saat dilatih dari nol (*from scratch*). Transformer murni biasanya menuntut volume data yang jauh lebih besar untuk mempelajari representasi yang bermakna tanpa bantuan prior eksternal.

Pertanyaan penelitian di bagian bawah slide ini sangat krusial untuk desain eksperimental tingkat doktoral. Ketika sebuah model mencapai akurasi lebih tinggi, apakah keunggulan tersebut benar-benar berasal dari arsitektur yang lebih unggul, perbedaan skala dan distribusi data, strategi pretraining seperti masked autoencoding atau contrastive learning, atau sekadar optimasi hyperparameter dan jadwal pelatihan? Memisahkan variabel-variabel ini bukan sekadar teknis evaluasi, melainkan prasyarat untuk klaim kontribusi ilmiah yang valid. Tanpa ablation study yang komprehensif dan kontrol dataset yang ketat, atribusi keberhasilan hanya akan bersifat spekulatif.

Diskusi mengenai bias arsitektur ini secara alami mengarah pada faktor eksternal yang tak kalah menentukan: dataset. Seperti yang akan kita bahas pada slide berikutnya, dataset bukanlah bahan bakar pasif bagi model, melainkan komponen metodologis aktif yang membatasi makna hasil penelitian. Pilihan dataset menentukan fenomena apa yang dapat dipelajari, jenis bias yang mungkin terinternalisasi, serta klaim generalisasi yang sah untuk diajukan. Oleh karena itu, pemahaman mendalam tentang interaksi antara inductive bias arsitektur dan karakteristik dataset menjadi fondasi kritis sebelum merancang benchmark atau mengevaluasi state-of-the-art.

---

## Slide 032 - Dataset adalah Bagian dari Metode

### Narasi

Slide ini memberikan respons langsung terhadap pertanyaan penelitian yang diangkat pada pembahasan sebelumnya mengenai perbedaan bias antara arsitektur CNN dan Transformer. Ketika kita mengamati variasi performa model, faktor yang paling sering luput dari perhatian bukanlah hanya pilihan arsitektur atau hyperparameter, melainkan dataset itu sendiri. Pada jenjang penelitian doktoral, kita harus menempatkan dataset sebagai bagian integral dari metodologi, bukan sekadar koleksi gambar pasif yang diberikan kepada model.

Pemilihan dataset secara langsung menentukan fondasi empiris dari seluruh eksperimen:
- Fenomena apa yang sebenarnya dapat dipelajari oleh model;
- Label dan task yang valid untuk dievaluasi;
- Populasi yang benar-benar diwakili dalam data;
- Jenis bias sistematis yang berpotensi masuk sejak tahap pengumpulan;
- Klaim generalisasi yang sah untuk diajukan dalam publikasi;
- Benchmark pembanding yang tersedia dan relevan secara akademis.

Prinsip kunci yang perlu ditekankan adalah bahwa dataset membatasi arti dan ruang lingkup hasil penelitian. Model yang unggul pada satu benchmark spesifik belum tentu menunjukkan robustness ketika dihadapkan pada domain baru atau distribution shift. Performa tinggi pada kondisi terkontrol sering kali mencerminkan adaptasi terhadap pola artifisial atau shortcut dalam data, bukan penguasaan representasi visual yang mendasar. Oleh karena itu, validasi eksternal dan analisis kegagalan menjadi wajib sebelum menyimpulkan kontribusi ilmiah suatu metode.

Untuk menjamin rigoritas dan transparansi penelitian, langkah kritis berikutnya adalah melakukan audit struktural terhadap dataset sebelum proses modeling dimulai. Slide berikutnya akan membedah anatomi dataset computer vision secara sistematis, mencakup unit data, modalitas input, granularitas label, strategi splitting, sumber data, lisensi, serta risiko seperti data duplication dan leakage. Pemahaman komprehensif atas komponen-komponen ini akan memungkinkan Anda merancang experimental design yang solid, meminimalkan confounding variables, dan memposisikan riset Anda secara tepat di tengah perkembangan state-of-the-art.

---

## Slide 033 - Anatomi Dataset Computer Vision

### Narasi

Slide ini mengajak kita membedah struktur internal sebuah dataset computer vision sebelum menggunakannya sebagai fondasi eksperimen. Seperti yang telah disinggung pada slide sebelumnya, dataset bukan sekadar kumpulan gambar, melainkan kerangka metodologis yang membatasi klaim penelitian. Oleh karena itu, setiap komponen dalam anatomi dataset harus diaudit secara kritis untuk memastikan validitas ilmiah dan reproduktibilitas hasil.

Pertama, tentukan unit data Anda. Apakah eksperimen berbasis satu citra statis, rangkaian video, frame individual, subjek tertentu, atau studi longitudinal? Penentuan unit ini akan mempengaruhi desain arsitektur model dan cara validasi dilakukan. Kedua, identifikasi modalitas input. Apakah data berupa RGB standar, grayscale, multispektral, depth map, atau kombinasi multimodal? Pilihan modalitas menentukan kompleksitas representasi visual yang harus dipelajari jaringan saraf.

Ketiga, evaluasi jenis label dan granularitasnya. Label bisa berupa klasifikasi kelas, bounding box, segmentasi mask, keypoint, caption teks, atau pasangan visual-tulisan. Granularitasnya pun bervariasi mulai dari image-level, object-level, pixel-level, hingga sequence-level. Kesesuaian antara granularitas label dengan tugas penelitian sangat krusial untuk menghindari misalignment antara tujuan dan implementasi.

Keempat, perhatikan strategi split data. Pembagian acak sering kali tidak memadai untuk penelitian tingkat lanjut. Anda perlu mempertimbangkan subject-wise split untuk menghindari data leakage antar subjek, temporal split untuk data berurutan, geographical split untuk generalisasi spasial, atau cross-domain split untuk menguji robustness terhadap distribution shift. Kelima, telusuri sumber data. Data dari web, sensor langsung, rekam medis, satelit, laboratorium terkontrol, atau simulasi memiliki karakteristik noise dan bias yang berbeda secara fundamental.

Keenam, verifikasi lisensi dan risiko etis. Pastikan hak penggunaan sesuai dengan scope riset, apakah diperbolehkan untuk komersialisasi atau redistribusi. Terakhir, lakukan audit risiko sistematis. Identifikasi potensi bias demografis atau sensor, pelanggaran privasi, duplikasi konten, shortcut learning di mana model mengandalkan heuristik dangkal, serta data leakage yang dapat menginflasi metrik evaluasi secara artifisial.

Pendekatan anatomi ini menjadi landasan praktis ketika kita mengevaluasi dataset spesifik. Sebagai contoh, pada slide berikutnya kita akan melihat bagaimana dataset low-level image processing seperti BSD500, DIV2K, atau SIDD dirancang dengan spesifikasi input, label, dan split yang sangat ketat untuk tugas restoration dan super-resolution. Pemahaman anatomi ini memungkinkan Anda menilai mengapa benchmark kecil seperti Set5 hanya berfungsi sebagai test set pembanding, bukan data training, sekaligus membantu Anda merancang dataset baru yang benar-benar menjawab research gap di bidang pengolahan citra digital.

---

## Slide 034 - Dataset Low-Level Image Processing

### Narasi

Setelah membahas anatomi dan komponen audit dataset pada slide sebelumnya, kita kini beralih ke klasifikasi spesifik berdasarkan tugas pengolahan citra. Slide ini menyoroti dataset yang menjadi fondasi utama dalam *low-level image processing*, yaitu tugas-tugas yang berfokus pada perbaikan kualitas piksel atau struktur dasar citra tanpa mengubah semantik tingkat tinggi.

Tabel pada slide ini merangkum beberapa benchmark standar yang wajib dikenal dalam riset restorasi dan enhancement citra:
- **BSD500/BSDS500**: Berisi citra alam dengan anotasi batas (*boundary*), sering digunakan sebagai referensi untuk edge detection dan segmentasi berbasis gradien.
- **DIV2K**: Mengandung 1.000 citra resolusi 2K yang menjadi standar de facto untuk pelatihan model super-resolution dan restoration modern.
- **Set5 dan Set14**: Merupakan himpunan uji berukuran kecil yang dirancang khusus sebagai pembanding cepat antar metode, bukan untuk proses pelatihan.
- **BSD100 dan Urban100**: Fokus pada struktur alam dan arsitektur perkotaan, sangat berguna untuk menguji generalisasi model terhadap pola geometris berulang atau tekstur kompleks.
- **SIDD**: Menyediakan pasangan citra noisy-clean yang dikumpulkan dari kamera ponsel nyata, menjadikannya acuan utama untuk denoising realistis.
- **GoPro**: Mengandalkan pasangan blurred-sharp untuk evaluasi motion deblurring dalam kondisi dinamis.
- **REDS**: Dataset video resolusi tinggi yang mendukung tugas deblurring, super-resolution, dan restoration secara temporal.

Penting untuk dicatat bahwa benchmark kecil seperti Set5 atau Set14 tidak boleh digunakan sebagai data pelatihan. Fungsinya murni sebagai *test set* pembanding yang memastikan *fair comparison* antar publikasi. Penggunaan yang salah dapat menyebabkan overfitting tersembunyi dan klaim performa yang tidak valid secara ilmiah, terutama dalam konteks evaluasi metodologi S3 yang menuntut reproduktibilitas tinggi dan transparansi data splitting.

Pembahasan selanjutnya pada slide berikutnya akan memperluas cakupan ke dataset yang berfokus pada *image quality assessment* dan restoration lanjutan. Di sana, kita akan mengaitkan karakteristik dataset ini dengan pemilihan metrik evaluasi seperti PSNR, SSIM, hingga LPIPS, serta pentingnya validasi perseptual melalui human evaluation. Penguasaan atas pembagian peran antara dataset training, validation, dan testing pada kategori low-level processing ini menjadi prasyarat kritis sebelum merancang eksperimen atau melakukan *critical paper review* pada literatur computer vision terkini.

---

## Slide 035 - Dataset Image Quality dan Restoration

### Narasi

Melanjutkan pembahasan mengenai dataset untuk pemrosesan citra tingkat rendah, kita kini menyoroti aspek evaluasi kualitas dan restorasi citra yang lebih menekankan pada dimensi perseptual maupun teknis. Slide ini memperkenalkan sejumlah benchmark yang secara spesifik dirancang untuk mengukur degradasi, melakukan restorasi, serta mengevaluasi hasil enhancement dalam berbagai kondisi pengamatan nyata.

Beberapa dataset acuan yang perlu Anda pahami antara lain:
- Kodak24: Standar evaluasi kompresi dan restorasi berbasis citra natural berkualitas tinggi.
- LIVE IQA, TID2013, dan KADID-10k: Dataset terstruktur berisi variasi distorsi sintetis lengkap dengan anotasi skor subjektif manusia, sangat relevan untuk pengembangan model Image Quality Assessment (IQA).
- DPED: Fokus pada transfer gaya dan enhancement antar perangkat kamera.
- LOL dan LOL-v2: Pasangan citra low-light dan normal-light yang menjadi dasar penelitian low-light enhancement dan color correction.

Dalam merancang eksperimen pada tingkat doktoral, pemilihan metrik evaluasi menentukan seberapa kuat klaim kontribusi ilmiah Anda. Metrik konvensional seperti PSNR dan SSIM menghitung kesamaan pixel-wise dan struktural terhadap reference, namun sering kali gagal berkorelasi sempurna dengan persepsi manusia. Sebaliknya, metrik berbasis representasi seperti LPIPS memanfaatkan embedding dari jaringan pre-trained untuk mengukur kesemestaan semantik dan tekstur. Integrasi metrik objektif dengan human evaluation menjadi praktik wajib ketika penelitian mengklaim peningkatan kualitas perseptual atau user experience, mengingat kesenjangan antara nilai numerik dan interpretasi visual manusia.

Pemahaman terhadap dataset evaluasi ini menjadi prasyarat kritis sebelum masuk ke diskursus arsitektural. Kinerja model tidak hanya ditentukan oleh kapasitas representasi CNN atau Transformer, melainkan juga oleh keselarasan antara tugas, distribusi data, dan metrik validasi yang dipilih. Slide berikutnya akan menggeser fokus ke dataset fondasi dan klasifikasi seperti MNIST, CIFAR, hingga ImageNet-1K, yang memberikan perspektif berbeda mengenai skalabilitas data, transfer learning, dan batas generalisasi model pada tugas pengenalan objek berskala besar.

---

## Slide 036 - Dataset Fondasi dan Klasifikasi

### Narasi

Setelah menelaah dataset yang berfokus pada kualitas citra dan restoration pada slide sebelumnya, kita kini beralih ke fondasi representasi visual melalui dataset klasifikasi dan benchmark dasar. Pemilihan dataset ini tidak hanya bersifat teknis, melainkan mencerminkan evolusi kebutuhan evaluasi model dari domain terkontrol menuju kompleksitas distribusi dunia nyata.

Berikut adalah karakteristik dan peran strategis setiap dataset dalam tabel:
- **MNIST & Fashion-MNIST**: Terdiri dari 70 ribu citra grayscale berukuran 28×28. Sangat efektif sebagai playground untuk debugging pipeline, validasi arsitektur awal, dan pengujian teknik augmentasi data tanpa membebani sumber daya komputasi.
- **CIFAR-10 & CIFAR-100**: Resolusi 32×32 dengan 10 dan 100 kelas. Sering dijadikan medium untuk mengevaluasi robustness model terhadap variasi kecil dalam tekstur, pencahayaan, dan pose objek.
- **ImageNet-1K**: Sekitar 1,28 juta gambar pelatihan yang terbagi ke dalam 1.000 kategori. Berfungsi sebagai standar emas untuk pretraining dan benchmark klasifikasi, sekaligus katalisator utama dalam perkembangan arsitektur deep visual representation.

Perlu ditekankan bahwa kinerja superior pada dataset kecil seperti MNIST atau CIFAR tidak serta merta menjamin generalisasi yang kuat pada skenario riset lanjutan atau implementasi industri. Keterbatasan variasi distribusi, potensi bias anotasi, dan kesederhanaan latar belakang dapat menghasilkan overfitting implisit yang tertutup oleh metrik akurasi permukaan. Oleh karena itu, klaim performa model harus selalu dikontekstualisasikan dengan kesesuaian dataset terhadap problem statement, ketersediaan data domain-specific, dan protokol evaluasi yang transparan.

Seiring meningkatnya granularitas tugas visi komputer, fokus eksplorasi akademis pun bergeser dari sekadar mengenali konten citra utuh menuju pemahaman spasial yang lebih presisi. Pada slide berikutnya, kita akan membahas dataset deteksi dan segmentasi seperti PASCAL VOC, MS COCO, Open Images, dan ADE20K, serta membedah perbedaan fundamental antara classification, detection, semantic segmentation, dan instance segmentation dalam hal tipe anotasi, tantangan komputasional, dan adaptasi arsitektur model.

---

## Slide 037 - Dataset Detection dan Segmentation

### Narasi

Pada slide sebelumnya, kita telah membahas dataset fondasi yang berfokus pada klasifikasi citra, mulai dari MNIST, CIFAR, hingga ImageNet. Langkah logis berikutnya dalam evolusi *computer vision* adalah memperluas cakupan tugas dari sekadar mengenali konten gambar, menuju pemahaman spasial dan struktural yang lebih mendalam. Slide ini menyoroti empat dataset utama yang menjadi pilar standar untuk tugas deteksi dan segmentasi.

Mari kita bedah karakteristik masing-masing dataset. PASCAL VOC 2007 dan 2012 merupakan benchmark klasik yang memperkenalkan anotasi kelas, *bounding box*, serta mask segmentasi. Dataset ini sangat krusial sebagai titik awal validasi arsitektur deteksi objek modern. Kemudian, MS COCO menetapkan standar baru dengan anotasi yang jauh lebih kaya, mencakup *instance mask*, *keypoint*, hingga *image caption*. Kekayaan anotasi ini menjadikan COCO standar emas untuk evaluasi deteksi, *instance segmentation*, bahkan generasi deskripsi visual. Untuk skala yang lebih masif dan tantangan *long-tail distribution*, Open Images menyediakan miliaran label, *bounding box*, dan hubungan relasional antar objek, meskipun subsetnya sering dipakai untuk melatih model yang robust terhadap variasi konteks. Terakhir, ADE20K secara khusus dirancang untuk *semantic segmentation* pada berbagai skenario *scene*, dengan ribuan kategori objek dan latar belakang yang menuntut model memahami struktur ruang secara holistik.

Perbedaan mendasar antara keempat tugas ini terletak pada tingkat granularitas dan jenis jawaban yang diharapkan dari model:
- **Klasifikasi**: hanya menjawab pertanyaan "apa yang ada dalam citra?".
- **Deteksi**: melangkah lebih jauh dengan menjawab "apa dan di mana objek berada?", yang memerlukan prediksi koordinat spasial.
- **Semantic segmentation**: melakukan pemetaan piksel demi piksel ke dalam kelas tertentu, sehingga setiap piksel diidentifikasi berdasarkan semantiknya tanpa membedakan objek identik.
- **Instance segmentation**: menggabungkan deteksi dan segmentasi, menjawab "piksel mana yang milik setiap objek individual?", yang berarti model harus memisahkan batas antar objek yang memiliki kelas sama.

Pemahaman tentang perbedaan tugas ini menjadi fondasi kritis sebelum kita mengevaluasi performa model pada domain yang lebih spesifik. Jika dataset klasifikasi dan deteksi umum memberikan kerangka kerja dasar, maka aplikasi dunia nyata seperti kendaraan otonom atau analisis lingkungan perkotaan menuntut dataset yang lebih kompleks dan kontekstual. Hal ini akan kita lanjutkan pada pembahasan mengenai dataset *scene*, jalan raya, dan visi otonom, di mana kita juga perlu mengantisipasi risiko evaluasi seperti bias kondisi lingkungan, kebocoran data antar split, serta fenomena *domain shift* ketika model diterapkan di luar kondisi pelatihan.

---

## Slide 038 - Dataset Scene, Street, dan Autonomous Vision

### Narasi

Pada slide ini, kita beralih dari dataset umum untuk deteksi dan segmentasi ke kumpulan data yang lebih spesifik pada konteks scene perkotaan, jalan raya, serta sistem otonom. Jika pada slide sebelumnya kita membahas dataset seperti PASCAL VOC, MS COCO, dan ADE20K sebagai fondasi klasik untuk tugas klasifikasi, deteksi, hingga segmentasi semantik, maka slide ini menyoroti bagaimana kebutuhan aplikasi dunia nyata menuntut representasi visual yang lebih kompleks dan terstruktur secara spasial-temporal.

Tabel pada slide ini merangkum lima dataset utama yang mendominasi riset autonomous vision. Cityscapes berfokus pada pemetaan scene perkotaan dengan anotasi semantic dan instance segmentation, serta menyediakan data disparity untuk estimasi kedalaman. KITTI merupakan dataset pionir yang mencakup stereo vision, optical flow, deteksi kendaraan, dan tracking dalam lingkungan jalan raya. BDD100K memperluas cakupan dengan video berkendara dalam kondisi geografis dan cuaca yang beragam, mendukung tugas deteksi, lane detection, segmentasi, dan tracking. nuScenes menghadirkan pendekatan multi-sensor yang mengintegrasikan kamera, lidar, dan radar untuk deteksi dan tracking 3D, sementara Waymo Open Dataset menawarkan skala besar dengan fokus pada persepsi lingkungan dan prediksi gerak kendaraan otonom.

Bagian risiko evaluasi pada slide ini sangat krusial untuk kajian kritis di tingkat doktoral. Perhatikan tiga tantangan metodologis berikut:
- Kondisi kota dan cuaca yang tidak seimbang dalam dataset dapat menimbulkan bias pelatihan, sehingga model sulit menggeneralisasi ke lingkungan baru.
- Frame berdekatan yang bocor antar split train-val-test menyebabkan data leakage, di mana model sebenarnya memanfaatkan korelasi temporal alih-alih mempelajari fitur visual yang invariant.
- Model sering gagal ketika berpindah negara, jenis sensor, atau musim, yang mengindikasikan keterbatasan benchmark konvensional dalam mengukur robustness sejati dan menyoroti perlunya teknik domain adaptation atau self-supervised pretraining pada tahap penelitian lanjutan.

Poin-poin ini menjadi jembatan analitis menuju diskusi pada slide berikutnya tentang dataset medis dan biomedical vision. Di domain spesialis seperti pencitraan medis, prinsip evaluasi dan strategi split data bahkan lebih ketat, misalnya penerapan patient-wise split untuk mencegah kebocoran informasi antar pasien, serta peringatan tegas bahwa metrik benchmark tidak menggantikan validasi klinis. Pemahaman mendalam mengenai batasan dataset ini akan menjadi landasan bagi mahasiswa dalam merancang eksperimen yang rigor, mengidentifikasi research gap yang relevan, dan memposisikan kontribusi ilmiah Anda terhadap state-of-the-art di bidang computer vision.

---

## Slide 039 - Dataset Medis dan Biomedical Vision

### Narasi

Setelah menelaah dataset untuk visi otonom dan lingkungan perkotaan pada slide sebelumnya, kita kini memasuki domain medis dan biomedical vision yang menuntut standar rigor metodologis dan etika yang jauh lebih ketat. Berbeda dengan data scene atau street-level, data medis memiliki karakteristik unik terkait privasi, heterogenitas perangkat pencitraan, serta dampak langsung terhadap keputusan klinis. Berikut adalah analisis kritis terhadap lima benchmark utama yang sering menjadi acuan dalam literatur terkini:

- **ISIC Archive**: Fokus pada dermoskopi dan lesi kulit untuk tugas klasifikasi dan segmentasi. Peneliti wajib melakukan audit distribusi populasi karena bias demografi dapat secara signifikan membatasi generalisasi model ke kelompok etnis atau geografi lain.
- **CheXpert**: Menyediakan chest radiograph dengan anotasi multi-label dan metrik ketidakpastian label. Dataset ini sangat relevan untuk mengembangkan model yang mampu menangani ambiguitas diagnostik, namun memerlukan penanganan noise label yang cermat selama training.
- **MIMIC-CXR**: Menggabungkan citra rontgen dada dengan laporan klinis terstruktur. Aset ini sangat kuat untuk riset vision-language dan reasoning, namun penggunaannya tunduk pada proses credentialing ketat dan kepatuhan regulasi kesehatan.
- **BraTS**: Berbasis MRI otak multisequence untuk segmentasi tumor glioma. Tantangan utamanya terletak pada variabilitas intensitas antar-scanner dan kebutuhan akan arsitektur yang mampu mengintegrasikan informasi dari berbagai sequence secara optimal.
- **CAMELYON**: Berfokus pada whole-slide histopathology untuk deteksi metastasis kanker payudara. Ukuran file yang masif dan kompleksitas struktur jaringan menuntut strategi augmentasi dan teknik patch-based processing yang efisien.

Prinsip pembagian data menjadi aspek paling krusial dalam evaluasi model medis. Kita harus menerapkan **patient-wise split**, bukan random image split. Jika citra dari pasien yang sama secara acak masuk ke set pelatihan dan pengujian, terjadi fenomena data leakage yang menghasilkan performa artifisial. Model pada dasarnya hanya menghafal fitur anatomi atau artefak spesifik pasien tersebut, bukan pola patologis yang sesungguhnya. Pemisahan berbasis identifier pasien memastikan bahwa evaluasi benar-benar mengukur kemampuan generalisasi model terhadap subjek baru.

Selain itu, klaim bahwa kinerja benchmark menggantikan validasi klinis merupakan kesalahpahaman fundamental yang perlu dikoreksi sejak tahap desain penelitian. Skor akurasi atau Dice coefficient pada dataset tertutup tidak menjamin keamanan atau efektivitas model di lingkungan rumah sakit nyata. Variasi protokol pencitraan, perbedaan merek mesin, serta heterogenitas populasi pasien sering kali menyebabkan degradasi performa yang drastis. Oleh karena itu, transisi dari eksperimen laboratorium ke implementasi klinis memerlukan validasi prospektif, uji coba real-world, dan kolaborasi multidisiplin antara ilmuwan data dan praktisi medis.

Karakteristik tantangan pada dataset medis ini memiliki paralel metodologis yang kuat dengan domain remote sensing dan earth observation yang akan kita bahas pada slide berikutnya. Meskipun konteks aplikasinya berbeda, kedua domain ini sama-sama menghadapi risiko spatial leakage, variasi sensor, perubahan musim/waktu, serta kebutuhan adaptasi domain yang ketat untuk memastikan robustness model di luar kondisi laboratorium.

---

## Slide 040 - Dataset Remote Sensing dan Earth Observation

### Narasi

Setelah membahas dataset medis yang menuntut pemisahan berbasis pasien dan validasi klinis yang ketat, kita beralih ke domain penginderaan jauh dan observasi bumi. Benchmark pada slide ini menjadi acuan standar dalam penelitian computer vision untuk aplikasi geospasial, namun masing-masing membawa karakteristik distribusi data yang unik:

- **EuroSAT**: Memanfaatkan data Sentinel-2 untuk klasifikasi tutupan lahan dengan sepuluh kelas kategori.
- **BigEarthNet**: Memperluas cakupan menjadi representasi multi-label pada patch multispektral, mencerminkan kompleksitas lingkungan nyata di mana satu area bisa memiliki beberapa fungsi sekaligus.
- **SpaceNet & xView**: Fokus pada ekstraksi fitur infrastruktur seperti bangunan dan jalan. xView secara khusus menguji kemampuan deteksi objek berukuran kecil pada citra resolusi tinggi.
- **LoveDA**: Dirancang khusus untuk mengatasi tantangan adaptasi domain antara skenario perkotaan dan pedesaan melalui tugas segmentasi semantik.

Implementasi model pada dataset ini menghadapi risiko teknis yang spesifik dan sering terabaikan dalam evaluasi awal:

- **Spatial leakage antartile**: Pola spasial yang mirip bocor ke set validasi atau uji akibat pembagian grid yang tidak independen.
- **Perbedaan sensor dan resolusi**: Menciptakan domain shift yang signifikan antar sumber data, sehingga arsitektur model harus tahan terhadap variasi frekuensi spasial.
- **Perubahan musim dan waktu**: Mengubah karakteristik spektral permukaan, menuntut mekanisme adaptasi temporal yang eksplisit.
- **Ketidakseimbangan wilayah geografis**: Membuat generalisasi model terbatas pada wilayah tertentu saja dan rentan terhadap bias sampling.

Bagi peneliti tingkat doktoral, tantangan ini bukan sekadar masalah preprocessing, melainkan fondasi desain eksperimen. Evaluasi kinerja wajib menyertakan protokol leave-one-region-out atau temporal holdout, bukan hanya random k-fold. Validasi statistik harus menguji robustness terhadap pergeseran distribusi dan bias geografis. Ketika kita berpindah ke dataset manusia, video, dokumen, dan multimodal pada slide berikutnya, pertimbangan etika seperti privasi, persetujuan subjek, bias demografis, hak cipta, dan konten berbahaya akan menjadi faktor penentu yang sama kritisnya dengan integritas data teknis.

---

## Slide 041 - Dataset Manusia, Video, Dokumen, dan Multimodal

### Narasi

Pada slide sebelumnya kita telah menyoroti karakteristik unik dan risiko teknis pada dataset remote sensing seperti spatial leakage, perbedaan sensor, serta ketidakseimbangan geografis. Kini kita beralih ke empat kategori dataset yang mendominasi perkembangan computer vision modern: representasi manusia, data video, dokumen visual, dan pasangan teks-citra multimodal.

- **Wajah dan Aktivitas Manusia**: LFW dan CelebA menjadi standar de facto untuk verifikasi wajah, ekstraksi atribut, dan evaluasi representasi laten. COCO Keypoints dan MPII menyediakan anotasi pose yang esensial untuk human pose estimation, sedangkan Market-1501 fokus pada person re-identification yang menuntut robustness tinggi terhadap oklusi parsial dan variasi pencahayaan.
- **Video**: UCF101 dan HMDB51 masih relevan sebagai benchmark pendidikan karena struktur labelnya yang terorganisir, namun Kinetics mendominasi fase pretraining skala besar berkat volume dan diversitas klipnya. Something-Something menawarkan tantangan temporal halus yang menguji kemampuan model memahami interaksi objek tanpa bergantung sepenuhnya pada konteks semantik luas.
- **Dokumen**: RVL-CDIP menyediakan kerangka klasifikasi citra dokumen multi-format, sementara DocVQA mendorong kemampuan visual question answering langsung pada layout kompleks, tabel, dan diagram teknis. Keduanya menggeser fokus dari deteksi objek umum ke interpretasi struktur informasi berbasis teks visual.
- **Multimodal**: Conceptual Captions, LAION, dan DataComp menyediakan pasangan teks-citra berskala besar yang menjadi fondasi self-supervised learning dan alignment vision-language. Kualitas kurasi, noise filtering, dan strategi sampling dari kumpulan ini secara langsung menentukan transferability representasi ke downstream tasks seperti CLIP atau DINOv2.

Di samping pertimbangan arsitektural dan komputasional, setiap pemilihan dataset wajib melibatkan audit etika dan legalitas secara ketat. Privasi subjek, persetujuan informed consent, bias demografis dalam distribusi kelas, hak cipta konten, lisensi redistribusi, serta potensi paparan konten berbahaya harus didokumentasikan secara transparan. Pada level doktoral, protokol pembersihan data, analisis bias sistematis, dan justifikasi representativitas sampel menjadi bagian integral dari metodologi penelitian Anda.

Pilihan dataset yang tepat tidak boleh didasarkan pada popularitas benchmark, melainkan pada kesesuaiannya dengan pertanyaan penelitian. Slide selanjutnya akan membahas prinsip pemetaan research question ke kebutuhan dataset, memastikan bahwa setiap eksperimen dirancang untuk menjawab hipotesis secara valid, terukur, dan dapat direplikasi.

---

## Slide 042 - Memilih Dataset dari Research Question

### Narasi

Setelah pada slide sebelumnya kita menelaah berbagai jenis dataset mulai dari wajah, aktivitas manusia, video, dokumen, hingga multimodal, langkah krusial selanjutnya adalah menentukan bagaimana memilih dataset yang tepat berdasarkan pertanyaan penelitian Anda. Di tingkat doktoral, kesalahan metodologis sering kali terjadi ketika peneliti terjebak pada popularitas benchmark tertentu tanpa mempertimbangkan kesesuaian struktural dengan tujuan riset. Prinsip dasarnya sangat tegas: mulailah dari research question, bukan dari tren atau kemudahan akses dataset.

Mari kita bedah hubungan antara jenis pertanyaan penelitian dengan karakteristik dataset yang diperlukan melalui poin-poin berikut:
- Untuk pertanyaan mengenai pengenalan kelas, dataset dengan label image-level sudah memadai.
- Jika fokusnya adalah pelokalan objek, Anda wajib menyediakan anotasi bounding box atau segmentation mask.
- Studi domain shift menuntut dataset multi-domain yang mencakup variasi sensor, lokasi, atau kondisi lingkungan.
- Evaluasi kinerja pada data terbatas harus dipantau melalui kurva performa terhadap ukuran label.
- Pengujian robustness terhadap korupsi data memerlukan protokol yang menggabungkan set bersih dan set terkorupsi.
- Analisis keadilan lintas kelompok membutuhkan metadata demografis yang sah dan etis.
- Investigasi transferabilitas representasi mengharuskan adanya pasangan dataset pretraining dan downstream yang terdefinisi jelas.

Prinsip inti yang mengatur seluruh proses ini adalah validitas eksperimen. Sebuah dataset hanya bernilai ilmiah jika ia memungkinkan research question dijawab secara rigor, transparan, dan dapat direproduksi. Pemilihan yang tidak selaras akan menghasilkan metrik yang menyesatkan, analisis bias yang tidak terdeteksi, atau klaim kontribusi yang lemah. Oleh karena itu, seleksi dataset harus menjadi bagian integral dari desain metodologi penelitian, bukan sekadar langkah administratif sebelum memasuki fase implementasi kode.

Setelah dataset terpilih dan kriteria pemetaannya jelas, tahap berikutnya adalah verifikasi kualitas data tersebut secara sistematis. Pada slide berikutnya, kita akan membahas checklist audit minimum yang wajib dilakukan sebelum memulai pelatihan arsitektur apa pun, termasuk pemeriksaan distribusi kelas, integritas file, duplikasi, kualitas label, serta aspek lisensi. Audit ini merupakan fondasi praktis yang juga menjadi fokus utama dalam Praktikum 01, memastikan bahwa setiap eksperimen dimulai dari data yang sehat, terdokumentasi, dan siap untuk dianalisis secara kritis sesuai standar publikasi internasional.

---

## Slide 043 - Audit Dataset Sebelum Training

### Narasi

Setelah menyelaraskan kebutuhan dataset dengan pertanyaan penelitian pada slide sebelumnya, langkah kritis berikutnya adalah melakukan audit menyeluruh sebelum proses training dimulai. Pada jenjang doktoral, asumsi bahwa dataset publik atau koleksi pribadi sudah bersih dan siap pakai sering kali menjadi sumber bias metodologis yang fatal. Audit berfungsi sebagai validasi eksperimental pertama untuk memastikan integritas data sebelum arsitektur model yang lebih kompleks diuji.

Checklist audit minimum mencakup sepuluh poin fundamental yang harus diverifikasi secara sistematis:
- Jumlah sampel dan distribusi kelas, untuk mengidentifikasi ketidakseimbangan yang dapat mendominasi fungsi loss dan mendistorsi arah gradient descent.
- Resolusi, channel, dan format file, karena inkonsistensi teknis ini akan mengganggu pipeline preprocessing dan augmentasi otomatis.
- Sampel visual acak per kelas, untuk memastikan konsistensi semantik dan mendeteksi dini outlier atau anotasi yang tidak sesuai dengan definisi kelas.
- File yang hilang atau korup, yang dapat memutus batch training atau menghasilkan error tak terduga selama iterasi optimasi.

Poin kelima hingga kesepuluh berfokus pada kualitas statistik, label, dan legalitas data:
- Deteksi duplikasi exact maupun near-duplicate, yang dapat menyebabkan overfitting artifisial jika subjek atau patch identik muncul di split berbeda.
- Statistik intensitas piksel dan distribusi warna per kelas, karena normalisasi yang tidak akurat akan memperlambat konvergensi dan menurunkan generalisasi lintas domain.
- Evaluasi kualitas label melalui spot-check manual atau metrik konsistensi annotator, mengingat kesalahan label pada penelitian tingkat lanjut dapat mengubah arah hipotesis secara signifikan.
- Dokumentasi metadata sumber, pemeriksaan lisensi dan batasan penggunaan, serta pemahaman hubungan antarunit data seperti ketergantungan spasial, temporal, atau subjektif.

Praktik audit ini bukan sekadar prosedur administratif, melainkan fondasi desain eksperimen yang ketat. Sesuai dengan arahan Praktikum 01, audit data harus diselesaikan tuntas sebelum mencoba arsitektur transformer, model self-supervised, atau framework generatif. Dengan demikian, evaluasi kinerja benar-benar mencerminkan kapasitas representasi model, bukan artefak dari data yang belum diverifikasi.

Temuan audit ini secara langsung menentukan strategi pembagian data yang akan dibahas pada slide berikutnya. Ketika struktur dan ketergantungan internal dataset diketahui, kita dapat merancang split yang mencegah data leakage, baik melalui pendekatan patient-wise, region-wise, maupun time-based, tergantung pada karakteristik domain penelitian yang dipilih.

---

## Slide 044 - Data Leakage Membuat Hasil Tampak Lebih Baik

### Narasi

Setelah melakukan audit dataset secara sistematis pada slide sebelumnya, langkah metodologis berikutnya yang tak kalah krusial adalah memastikan integritas pembagian data. Kesalahan paling umum yang sering meruntuhkan validitas eksperimen adalah *data leakage*. Fenomena ini secara artifisial meningkatkan skor evaluasi, sehingga hasil penelitian tampak lebih unggul padahal model sebenarnya mengalami kebocoran informasi dari set uji ke proses pelatihan.

### Bentuk Umum Data Leakage

Kebocoran data dapat terjadi melalui beberapa mekanisme yang sering luput dari perhatian peneliti:
- Subjek atau pasien yang sama muncul sekaligus di set pelatihan dan set pengujian.
- Frame video yang berdekatan secara temporal tersebar di split yang berbeda.
- Tile citra satelit yang bertetangga secara spasial masuk ke split berbeda.
- Augmentasi atau turunan citra justru dimasukkan ke dalam set uji.
- Operasi normalisasi dihitung menggunakan seluruh data sebelum pemisahan.
- Set uji dipakai berulang kali untuk memilih model atau menyesuaikan hiperparameter.

### Strategi Split yang Sesuai Domain

Untuk memitigasi risiko tersebut, pembagian data harus dirancang sesuai dengan struktur intrinsik domain aplikasi:
- **Medis**: Gunakan pendekatan *patient-wise* agar semua studi dari satu pasien tetap berada di split yang sama.
- **Video**: Terapkan pemisahan berbasis *video* atau *subject-wise* untuk memutus kontinuitas temporal.
- **Satelit**: Gunakan pembagian *geographic* atau *region-wise* guna menghindari korelasi lingkungan yang bocor.
- **Temporal**: Terapkan pembagian berbasis waktu (*time-based*) untuk mensimulasikan prediksi masa depan.
- **Multi-center**: Pastikan pemisahan dilakukan berdasarkan lokasi atau institusi asal data (*site/hospital-wise*).

Penerapan protokol *splitting* yang ketat ini akan menjadi landasan utama ketika kita mengevaluasi kemajuan model melalui benchmark standar. Namun, perlu diwaspadai bahwa bahkan dengan pembagian data yang benar, benchmark itu sendiri masih rentan memicu interpretasi yang keliru jika tidak dikaji secara kritis. Pembahasan mengenai potensi jebakan benchmark dan pertanyaan-pertanyaan fundamental yang harus diajukan sebelum mengklaim kontribusi ilmiah akan kita lanjutkan pada slide berikutnya.

---

## Slide 045 - Benchmark Dapat Mendorong Kesimpulan Keliru

### Narasi

Setelah membahas bagaimana kebocoran data dapat secara artifisial meningkatkan metrik evaluasi pada slide sebelumnya, kita beralih ke masalah yang lebih sistemik dalam komunitas riset: bagaimana penggunaan benchmark itu sendiri dapat mendorong kesimpulan yang keliru. Bahkan ketika split data telah dirancang ketat, praktik evaluasi yang tidak kritis tetap berisiko menghasilkan klaim penelitian yang tidak robust dan sulit direproduksi.

Dalam literatur terkini, beberapa pola umum sering terabaikan saat membandingkan model-model vision. Berikut adalah tantangan kritis yang perlu diwaspadai:
- Mengejar perbedaan akurasi yang sangat kecil sering kali tidak memiliki signifikansi statistik maupun praktis, namun tetap mendominasi publikasi tanpa validasi yang memadai.
- Test set berubah fungsi menjadi target optimasi komunitas, di mana arsitektur dan pipeline tuning secara tidak langsung menyesuaikan diri dengan pola distribusi benchmark tertentu.
- Dataset benchmark jarang mewakili kondisi deployment nyata, terutama pada domain sensitif seperti medis atau satelit di mana noise, missing data, dan heterogenitas perangkat jauh lebih kompleks.
- Label ground truth biasanya bersifat diskrit dan simplistik, sehingga gagal menangkap seluruh nuansa fenomena visual atau ambiguitas intrinsik dalam citra.
- Model deep learning modern sangat rentan memanfaatkan shortcut atau korelasi latar belakang (background bias) daripada fitur semantik yang benar-benar relevan dengan tugas.
- Biaya komputasi, durasi training, dan skema pretraining sering kali tidak dilaporkan secara transparan, sehingga perbandingan antar metode kehilangan dimensi fairness yang esensial.

Untuk memastikan rigor ilmiah, setiap eksperimen harus melewati serangkaian pertanyaan kritis sebelum novelty diklaim:
- Apakah baseline yang digunakan cukup kuat? Baseline yang lemah akan membuat improvement terlihat dramatis meskipun sebenarnya hanya mengikuti tren umum.
- Apakah perbandingan menggunakan data dan pretraining yang setara? Perbedaan sumber data awal atau checkpoint foundation model dapat mendominasi hasil akhir.
- Apakah hasil stabil pada beberapa seed? Variasi acak dalam initialization dan shuffling dapat mengubah ranking model secara signifikan jika tidak divalidasi secara statistik.
- Apakah ada external validation atau pengujian distribution shift? Performa pada train/test split saja tidak cukup menjamin generalisasi ke domain lain.
- Apakah klaim dibatasi oleh karakteristik dataset? Generalisasi berlebihan tanpa menyebutkan batasan dataset merupakan red flag dalam penulisan paper tingkat doktor.

Prinsip-prinsip evaluasi kritis ini akan langsung diuji secara empiris pada Praktikum 02 yang akan kita bahas pada slide berikutnya. Mahasiswa akan membangun fondasi CNN dan Transformer dari nol menggunakan PyTorch, kemudian melatihnya pada CIFAR-10. Melalui implementasi manual convolution, mekanisme attention, serta visualisasi feature map dan attention matrix, diharapkan mahasiswa dapat merasakan langsung mengapa kontrol eksperimen, pemahaman mendalam tentang representasi visual, dan interpretasi hasil yang hati-hati menjadi kunci utama dalam merancang penelitian computer vision yang berkontribusi signifikan.

---

## Slide 046 - Praktikum 02: Dari Mekanisme ke Eksperimen

### Narasi

Slide ini menandai transisi dari analisis kritis terhadap keterbatasan benchmark menuju implementasi eksperimental yang terstruktur. Praktikum 02 dirancang khusus untuk menjembatani pemahaman mekanistik arsitektur dengan validasi empiris, sehingga mahasiswa dapat menguji langsung bagaimana representasi visual terbentuk, diekstraksi, dan diverifikasi selama proses pembelajaran.

Praktikum dibagi menjadi dua bagian inti yang saling melengkapi:
- **Bagian A — Fondasi CNN**: Mahasiswa akan memanipulasi citra sebagai tensor multidimensi, mengimplementasikan konvolusi secara manual, serta mengatur parameter stride, padding, pooling, dan menghitung receptive field. Arsitektur SimpleCNN akan dibangun bertahap, dilengkapi dengan residual block untuk mengatasi degradasi gradien, dan didukung dengan teknik visualisasi feature map untuk interpretasi representasi internal.
- **Bagian B — Fondasi Transformer**: Fokus bergeser ke tokenisasi patch, embedding, serta mekanisme self-attention yang terdiri dari query, key, dan value. Mahasiswa akan menyusun attention matrix, menerapkan multi-head attention, mengintegrasikan positional encoding, dan akhirnya merakit Tiny Image Transformer. Visualisasi attention matrix akan digunakan sebagai lensa diagnostik untuk menilai apakah model benar-benar menangkap konteks global atau terjebak pada pola artifaktual.

Pemilihan CIFAR-10 sebagai dataset bersifat strategis dan disengaja. Skala data yang terbatas memungkinkan kedua model dibangun dan dilatih sepenuhnya dari nol dalam lingkungan praktikum, tanpa bergantung pada weight pretraining eksternal. Pendekatan ini meminimalkan confounding variable, sekaligus menjawab kritik pada slide sebelumnya bahwa perbandingan akurasi sering kali menyesatkan ketika kondisi pretraining, distribusi data, dan infrastruktur tidak disejajarkan.

Dengan fondasi mekanistik dan dataset yang telah ditetapkan, langkah selanjutnya adalah memastikan bahwa setiap eksperimen berjalan di bawah kontrol ketat dan reproducible. Slide berikutnya akan membahas protokol eksperimen secara rinci, mencakup variabel yang dikontrol, spesifikasi model, serta catatan kritis mengenai kesetaraan konfigurasi dan batasan fairness dalam perbandingan arsitektur.

---

## Slide 047 - Protokol Eksperimen Praktikum 02

### Narasi

Merujuk pada pembahasan slide sebelumnya mengenai implementasi arsitektur CNN dan Transformer dari nol, kita kini memasuki tahap penyusunan protokol eksperimen. Pada tingkat doktoral, desain eksperimen yang terkontrol merupakan prasyarat fundamental sebelum melakukan analisis kritis terhadap performa model.

Untuk menjaga validitas perbandingan, sejumlah variabel harus dikendalikan secara ketat:
- Pembagian indeks data untuk training, validation, dan test harus identik.
- Teknik augmentasi citra diterapkan seragam untuk meminimalkan bias distribusi.
- Konfigurasi optimisasi meliputi jumlah epoch, optimizer, learning rate, dan batch size disamakan.
- Random seed ditetapkan tetap guna menjamin reproduktibilitas hasil.
- Perangkat evaluasi (device) disesuaikan agar tidak terjadi distorsi akibat perbedaan spesifikasi hardware.

Model yang akan dievaluasi adalah SimpleCNN dan Tiny Image Transformer, keduanya dibangun tanpa memanfaatkan weight pre-trained. Pendekatan ini memungkinkan peneliti menyelami mekanisme internal setiap layer, mulai dari operasi konvolusi lokal hingga perhitungan attention global, sebelum beralih ke fondasi model skala industri.

Penting untuk dipahami bahwa kesamaan konfigurasi ini hanya memberikan kontrol awal. Hal tersebut belum menjamin optimalitas hyperparameter, apalagi menyamakan jumlah parameter antara arsitektur CNN dan Transformer. Perbedaan kapasitas representasi dan kompleksitas komputasi inherent dari kedua pendekatan ini menuntut interpretasi hasil yang kontekstual. Protokol ini berfungsi sebagai baseline yang terstandarisasi, bukan sebagai pernyataan kesetaraan mutlak.

Setelah kerangka eksperimen ditetapkan, langkah logis berikutnya adalah menentukan instrumen pengukuran yang relevan. Slide selanjutnya akan menguraikan metrik yang harus diobservasi, mencakup kinerja prediksi, dinamika konvergensi loss, efisiensi komputasi, serta analisis kualitatif terhadap feature map dan attention mechanism.

---

## Slide 048 - Apa yang Harus Diukur?

### Narasi

Setelah protokol eksperimen ditetapkan dengan kontrol variabel yang ketat pada slide sebelumnya, langkah metodologis berikutnya adalah mendefinisikan metrik evaluasi yang komprehensif. Pada jenjang doktoral, penilaian model tidak dapat disederhanakan hanya pada satu angka agregat, melainkan harus menangkap multidimensi kinerja, stabilitas, efisiensi, dan interpretabilitas representasi visual.

Untuk **kinerja prediksi**, akurasi berfungsi sebagai baseline, namun wajib dilengkapi dengan macro-F1 score agar ketidakseimbangan kelas terakomodasi secara eksplisit. Classification report memberikan granularitas performa per-kelas, sedangkan confusion matrix menjadi instrumen diagnostik utama untuk mengidentifikasi pola kesalahan sistematis, misalnya kebingungan antar-kelas yang berbagi tekstur atau struktur visual serupa.

Dari perspektif **proses belajar**, monitoring training loss dan validation loss sepanjang epoch menjadi indikator primer konvergensi. Training-validation gap mengungkap potensi overfitting atau underfitting, sementara pengujian berulang dengan berbagai random seed mengkuantifikasi stabilitas model terhadap variasi inisialisasi bobot. Stabilitas ini sering kali menjadi pembeda antara model yang benar-benar robust versus yang hanya lucky initialization.

Aspek **efisiensi** harus dilaporkan secara transparan untuk menilai kelayakan praktis dan skalabilitas. Jumlah parameter total merepresentasikan kompleksitas arsitektur, sedangkan waktu training, latensi inference, dan throughput (samples per detik) menentukan kompatibilitas model dengan constraint hardware. Dalam riset computer vision terkini, optimasi trade-off antara akurasi dan beban komputasi sering menjadi sumber novelty kontribusi ilmiah.

Komplemen esensial dari metrik kuantitatif adalah **analisis kualitatif**. Visualisasi feature map membantu melacak bagaimana CNN mengekstrak hierarki abstraksi, sedangkan attention map pada transformer mengungkap region fokus spasial saat pengambilan keputusan. Review manual terhadap contoh salah klasifikasi memungkinkan identifikasi bias dataset, artefak augmentasi, atau blind spot arsitektural yang tidak terpampang jelas pada metrik agregat.

Poin fairnes yang ditekankan pada slide sebelumnya—bahwa kesetaraan konfigurasi belum menjamin kesetaraan kapasitas model—menjadi landasan interpretasi seluruh metrik ini. Tanpa desain eksperimen yang terkontrol, perbedaan skor hanyalah noise metodologis. Ketika kumpulan data dari keempat dimensi tersebut telah terbentuk, pembacaannya memerlukan kerangka analitis yang hati-hati, menghindari generalisasi prematur, dan selalu menyertakan kondisi batas serta alternatif penjelasan, sebagaimana akan diuraikan pada pembahasan selanjutnya.

---

## Slide 049 - Membaca Hasil Secara Hati-Hati

### Narasi

Setelah kita mengidentifikasi metrik apa saja yang perlu diukur—mulai dari kinerja prediksi, proses belajar, efisiensi komputasi, hingga analisis kualitatif seperti feature map dan confusion matrix—langkah krusial berikutnya adalah menafsirkan angka-angka tersebut dengan kritis. Slide ini menekankan bahwa pada level penelitian doktoral, interpretasi hasil eksperimen tidak boleh bersifat permukaan. Angka statistik hanyalah titik awal; makna ilmiahnya muncul ketika kita memahami konteks, batasan, dan mekanisme di balik performa model.

Pernyataan seperti “CNN lebih baik karena accuracy-nya lebih tinggi” sering muncul sebagai kesimpulan awal, namun secara metodologis ini sangat lemah. Accuracy tunggal tidak mencerminkan keseimbangan antar-kelas, sensitivitas terhadap threshold, atau konsistensi performa di bawah distribusi data yang berbeda. Klaim semacam itu juga mengabaikan faktor confounding seperti preprocessing pipeline, augmentasi, atau inisialisasi bobot yang mungkin tidak setara antara baseline dan model baru.

Sebuah kesimpulan yang valid harus dirumuskan dengan presisi kontekstual. Perhatikan contoh narasi yang tepat: pada CIFAR-10 dengan training dari nol dan konfigurasi yang diuji, SimpleCNN memang mencapai kinerja lebih tinggi dalam batas epoch tertentu. Hasil ini konsisten dengan manfaat bias lokal pada data terbatas, tetapi belum membuktikan dominasi mutlak CNN karena parameter dan hyperparameter kedua model belum sepenuhnya disejajarkan. Dengan kata lain, keunggulan yang teramati bersifat kondisional, bukan universal.

Untuk menjaga integritas ilmiah dan menghindari overclaiming, setiap pelaporan hasil harus mengikuti prinsip berikut:
- Sebutkan kondisi eksperimental secara eksplisit (dataset, split, hardware, framework).
- Cantumkan ukuran efek (effect size) dan selang kepercayaan, bukan hanya nilai rata-rata.
- Laporkan variasi lintas-seed atau cross-validation untuk mengukur stabilitas.
- Akui keterbatasan desain eksperimen dan potensi bias sampling.
- Sediakan alternatif penjelasan yang masuk akal sebelum menarik kesimpulan kausal.

Pendekatan ini mengubah hasil eksperimen dari sekadar angka menjadi fondasi argumentasi penelitian yang robust. Ketika model menunjukkan pola kesalahan sistematis, justru itulah momen paling bernilai untuk iterasi ilmiah. Pada slide berikutnya, kita akan membahas alur terstruktur dari observasi failure case hingga perumusan klaim yang terbatas namun dapat dipertanggungjawabkan secara empiris.

---

## Slide 050 - Dari Failure Case ke Eksperimen Berikutnya

### Narasi

Setelah pada slide sebelumnya menekankan pentingnya membaca hasil eksperimen secara hati-hati dan menghindari generalisasi berlebihan, kita kini beralih ke langkah operasional berikutnya: mengubah temuan atau kegagalan model menjadi kerangka eksperimen yang terstruktur dan dapat direplikasi. Slide ini menyajikan alur kerja ilmiah yang harus menjadi standar setiap kali kita menemukan anomali, penurunan kinerja, atau *failure case* dalam evaluasi model.

Alur tersebut dapat dijabarkan sebagai berikut:
- Mulai dari observasi empiris terhadap perilaku model pada subset data tertentu.
- Formulasikan dugaan penyebab berdasarkan mekanisme arsitektur, karakteristik distribusi data, atau kesalahan pra-pemrosesan.
- Turunkan hipotesis yang spesifik dan dapat diuji secara kuantitatif.
- Tentukan variabel independen dan dependen, serta identifikasi variabel yang harus dikontrol agar isolasi efek menjadi valid.
- Rancang eksperimen yang terkontrol untuk mengumpulkan bukti.
- Analisis *evidence* menggunakan metrik yang tepat, bukan hanya akurasi global.
- Rumuskan klaim yang dibatasi secara ketat pada kondisi, dataset, dan konfigurasi yang diuji.

Sebagai ilustrasi konkret, perhatikan contoh pengamatan bahwa model sering tertukar antara kelas *cat* dan *dog*. Dugaan awal dapat mengarah pada resolusi input yang rendah, sehingga detail bentuk, tekstur bulu, atau proporsi anatomi khas masing-masing kelas hilang atau terdistorsi. Hipotesisnya adalah peningkatan resolusi input atau augmentasi berbasis skala akan memperbaiki diskriminasi antar kelas tersebut. Untuk mengujinya, kita perlu merancang eksperimen yang membandingkan performa pada berbagai resolusi dan ukuran subset data, sambil menjaga learning rate, optimizer, dan arsitektur tetap konstan. Bukti yang diperlukan mencakup perubahan per-class recall, pola pada matriks kebingungan (*confusion matrix*), serta visualisasi *feature map* atau *attention map* yang menunjukkan apakah fokus model telah bergeser ke region yang lebih informatif. Klaim yang dihasilkan harus selalu dibungkus dengan batasan eksperimental, misalnya menyatakan bahwa perbaikan hanya teramati pada konfigurasi tertentu dan belum tentu general ke domain lain atau perangkat keras berbeda.

Pendekatan ini sangat krusial dalam penelitian tingkat doktor karena mencegah terjebak pada optimisasi buta, tuning hiperparameter tanpa dasar teoritis, atau klaim yang tidak berdasar. Setiap *failure case* sebenarnya adalah sinyal berharga yang mengarahkan kita pada *research gap*, apakah itu terkait representasi lokal-global, bias dataset, ketidakseimbangan kelas, atau ketidaksesuaian kapasitas model dengan kompleksitas tugas. Dengan menerapkan alur observasi-hipotesis-evidence-klaim terbatas secara konsisten, kita membangun fondasi metodologis yang kuat, transparan, dan siap dipertanggungjawabkan dalam penulisan artikel ilmiah atau proposal disertasi.

Pada slide berikutnya, Anda akan diberikan tugas pertemuan kedua yang dirancang khusus untuk melatih penerapan alur ini secara langsung. Tugas praktikum meminta Anda menjalankan arsitektur SimpleCNN dan Tiny Image Transformer, mendeskripsikan bentuk tensor pada setiap tahap pemrosesan, membandingkan kurva pelatihan, akurasi, macro-F1, jumlah parameter, dan waktu komputasi, serta melakukan inspeksi mendalam pada *feature map* dan *attention map*. Anda juga diminta mengidentifikasi minimal lima *failure case*, merancang dua eksperimen pengembangan berdasarkan skema yang dibahas, dan memilih satu dataset yang relevan dengan minat disertasi. Pemilihan dataset tersebut harus disertai analisis kritis mengenai jenis anotasi, unit data, strategi split, lisensi akses, potensi bias dan risiko *data leakage*, baseline yang layak, serta keterbatasan klaim generalisasi. Semua komponen ini akan menjadi landasan struktural bagi proposal awal disertasi yang Anda bangun selama perkuliahan.

---

## Slide 051 - Tugas Pertemuan 02

### Narasi

Beralih dari kerangka analisis kegagalan pada slide sebelumnya, kita kini memasuki fase eksekusi dan pemetaan metodologis untuk Pertemuan 02. Tugas ini dirancang agar Anda tidak hanya menjalankan kode, tetapi juga menginternalisasi mekanisme representasi visual pada arsitektur dasar sekaligus menyusun fondasi pemilihan dataset untuk penelitian disertasi Anda.

### Tugas Praktikum

Anda diminta mengimplementasikan dan melatih dua model sederhana: SimpleCNN dan Tiny Image Transformer. Selama proses komputasi, catat secara rinci bentuk tensor pada setiap tahap, mulai dari input, melalui layer konvolusi atau blok transformer, hingga ke head klasifikasi. Setelah pelatihan selesai, lakukan perbandingan sistematis berdasarkan:
- Kurva pembelajaran dan metrik evaluasi (accuracy dan macro-F1);
- Jumlah parameter dan waktu komputasi;
- Analisis feature map dari CNN versus attention map dari Transformer;
- Identifikasi minimal lima failure case;
- Perancangan dua eksperimen pengembangan berbasis temuan tersebut.

Pendekatan ini melanjutkan alur observasi-dugaan-eksperimen-evidence yang telah kita bahas. Fokuskan analisis pada bagaimana arsitektur lokal CNN berbeda dengan mekanisme dependensi global pada Transformer dalam menangani variasi intrakelas dan interkelas.

### Tugas Dataset

Secara paralel, pilih satu dataset yang relevan dengan minat topik disertasi Anda. Tuliskan spesifikasi teknis berikut secara eksplisit:
- Task dan jenis anotasi yang digunakan;
- Unit data serta strategi split (train/validation/test);
- Status lisensi dan jalur akses data;
- Potensi bias dan risiko data leakage;
- Baseline yang layak untuk dijadikan acuan;
- Keterbatasan klaim generalisasi model pada dataset tersebut.

Evaluasi kritis terhadap dataset bukan sekadar formalitas administratif, melainkan fondasi metodologis yang menentukan validitas eksternal dan reproducibility penelitian Anda di jenjang doktor.

Keluaran dari kedua tugas ini akan diterjemahkan secara terstruktur pada slide berikutnya. Anda diharapkan menghasilkan notebook yang dapat direproduksi sepenuhnya, tabel perbandingan kuantitatif, visualisasi kurva dan confusion matrix, peta fitur dan perhatian, analisis kesalahan yang mendalam, log penelitian lengkap, audit singkat dataset target, serta dua pertanyaan lanjutan yang mengarah pada arsitektur modern. Indikator keberhasilan utamanya terletak pada kemampuan Anda menjelaskan mengapa suatu hasil terjadi, serta menyadari batas bukti yang dimiliki oleh eksperimen yang dijalankan. Pastikan setiap klaim didukung oleh evidence yang terukur, sehingga langkah selanjutnya menuju riset state-of-the-art dapat dibangun di atas fondasi yang rigor dan teruji.

---

## Slide 052 - Target Keluaran

### Narasi

Setelah menyelesaikan tugas praktikum pada pertemuan sebelumnya yang mencakup implementasi SimpleCNN dan Tiny Image Transformer serta pemilihan dataset target, kita kini beralih ke standar kualitas yang harus dicapai dalam setiap eksperimen. Slide ini menegaskan bahwa hasil kerja akademik di jenjang doktor tidak hanya dinilai dari kelengkapan kode, melainkan dari kedalaman analisis dan transparansi metodologi.

Secara spesifik, mahasiswa diharapkan menghasilkan delapan komponen utama sebagai bukti ketercapaian kompetensi:
1. Notebook Praktikum 02 yang dapat dijalankan ulang secara reproduktif tanpa dependensi tersembunyi.
2. Tabel komparatif sistematis antara arsitektur CNN dan Transformer yang mencakup metrik akurasi, macro-F1, jumlah parameter, dan waktu komputasi.
3. Kurva pembelajaran beserta confusion matrix untuk memetakan performa model pada kelas-kelas tertentu.
4. Visualisasi feature map dan attention map guna menginterpretasikan mekanisme internal jaringan.
5. Error analysis yang mengidentifikasi pola kegagalan prediksi secara kuantitatif maupun kualitatif.
6. Research log yang mendokumentasikan seluruh iterasi eksperimen, termasuk hyperparameter tuning dan keputusan desain.
7. Audit singkat terhadap satu dataset target yang menyoroti bias, risiko data leakage, lisensi, dan strategi split.
8. Dua pertanyaan lanjutan yang mengarah pada eksplorasi arsitektur modern atau teknik representasi mutakhir.

Indikator keberhasilan pada slide ini menekankan aspek kausalitas dan batas validitas temuan. Mahasiswa tidak cukup hanya melaporkan angka atau grafik; mereka harus mampu menjelaskan mengapa suatu hasil terjadi berdasarkan karakteristik arsitektur, inductive bias, atau distribusi data. Selain itu, setiap klaim kinerja model harus disertai dengan pembatasan bukti yang jelas, seperti ukuran sampel, variasi augmentasi, atau kondisi inference yang belum teruji. Pendekatan ini sejalan dengan tuntutan penelitian tingkat doktor yang mengutamakan rigor metodologis dan kesadaran akan generalisasi terbatas.

Pencapaian target keluaran ini menjadi fondasi langsung untuk persiapan Pertemuan 03. Dengan pemahaman mendalam tentang convolution, residual connection, tokenization, Q/K/V mechanism, multi-head attention, positional encoding, patch embedding, serta peran karakteristik dataset, mahasiswa siap melanjutkan eksplorasi ke arsitektur modern seperti ResNet, EfficientNet, DeiT, ViT, dan Swin Transformer. Pertemuan berikutnya juga akan membahas pretrained model, transfer learning, kebutuhan data, trade-off antara parameter, FLOPs, latency, dan robustness, serta protokol standar untuk membandingkan arsitektur secara adil. Hasil audit dataset dan error analysis dari pertemuan ini akan menjadi bahan kritis dalam merancang eksperimen komparatif yang lebih kompleks dan relevan dengan arah penelitian disertasi masing-masing.

---

## Slide 053 - Persiapan Menuju Pertemuan 03

### Narasi

Setelah menyelesaikan Praktikum 02, mahasiswa telah menghasilkan deliverables yang tidak hanya bersifat teknis, melainkan juga analitis. Notebook yang dapat direproduksi, tabel perbandingan arsitektur, kurva pembelajaran, confusion matrix, hingga peta fitur dan attention map, semuanya dirancang untuk menguji hipotesis dan memvalidasi klaim empiris. Indikator keberhasilan pada slide sebelumnya menekankan pentingnya kemampuan menjelaskan mekanisme di balik setiap hasil eksperimen, serta menyadari batas bukti yang dimiliki sebelum melangkah ke tahap penelitian lebih lanjut.

Pada slide ini, kita merangkum fondasi representasi visual yang telah dibangun selama Pertemuan 02. Mahasiswa telah mendalami operasi convolution dan bagaimana residual connection memecah masalah optimasi pada jaringan dalam. Konsep feature map dan receptive field menjadi kunci untuk memahami bagaimana informasi spasial dipadatkan secara hierarkis. Di sisi transformer, pemahaman mengenai tokenisasi citra, mekanisme Query, Key, Value, serta self-attention memungkinkan kita melihat pola hubungan global antar region gambar. Multi-head attention memperkaya representasi dengan menangkap berbagai jenis dependensi secara paralel, sementara positional encoding menyuplai informasi lokasional yang hilang akibat sifat permutation-invariant dari attention mechanism. Patch embedding berperan sebagai jembatan antara domain piksel dan domain sequence, dan seluruh proses training dari nol memberikan gambaran eksplisit tentang bagaimana karakteristik dataset membentuk perilaku model.

Pertemuan 03 akan melanjutkan perjalanan ini dengan mengevaluasi arsitektur modern yang telah matang secara empiris. Kita akan membedah evolusi CNN seperti ResNet dan EfficientNet, yang mengoptimalkan trade-off antara akurasi dan efisiensi komputasi. Di ranah transformer, DeiT, Vision Transformer, dan Swin Transformer akan dianalisis struktur bloknya, strategi windowing, serta adaptasinya terhadap tugas computer vision. Transfer learning dan penggunaan pretrained model akan dibahas sebagai praktik standar untuk mengurangi kebutuhan data dan mempercepat konvergensi, sekaligus menyoroti peran inductive bias yang berbeda antara CNN dan transformer. Evaluasi komparatif tidak lagi berhenti pada akurasi semata, melainkan mencakup metrik parameter, FLOPs, latency inference, dan robustness terhadap distribusi shift. Protokol komparasi yang ketat diperlukan agar setiap klaim performa dapat direplikasi dan dibandingkan secara adil di literatur terkini.

Rangkuman pada slide berikutnya akan mengikat kembali semua konsep inti menjadi satu kerangka kohesif. Pemahaman bahwa CNN mengandalkan kernel lokal bertingkat, sedangkan transformer mengandalkan attention global yang diperkuat oleh posisi dan tokenisasi patch, menjadi dasar kritis untuk memilih atau merancang arsitektur baru. Audit dataset dan pembagian split yang rigor tetap menjadi pondasi yang setara pentingnya dengan pemilihan model. Dengan fondasi ini, persiapan menuju pertemuan berikutnya siap mengarahkan analisis Anda pada identifikasi research gap, evaluasi metodologi state-of-the-art, dan perumusan kontribusi ilmiah yang terukur.

---

## Slide 054 - Rangkuman

### Narasi

Mari kita rangkum fondasi representasi visual yang telah dibahas dalam pertemuan ini. Pada tingkat doktoral, pemahaman mendalam mengenai mekanisme pembelajaran fitur bukan sekadar hafalan arsitektur, melainkan kesadaran kritis terhadap bagaimana informasi spasial dan semantik diproses secara hierarkis. Convolutional Neural Networks (CNN) bekerja dengan mempelajari kernel konvolusi yang secara bertahap membangun representasi lokal, mulai dari tepi dan tekstur sederhana hingga pola kompleks pada lapisan yang lebih dalam. Mekanisme seperti stride, padding, pooling, serta konsep receptive field berperan krusial dalam mengatur aliran informasi spasial dan mengontrol trade-off antara resolusi fitur dan konteks global.

Di sisi lain, arsitektur modern mengandalkan residual connection untuk memitigasi masalah vanishing gradient dan mempercepat konvergensi selama optimasi jaringan yang sangat dalam. Sementara itu, pendekatan berbasis Transformer mengubah paradigma pemrosesan citra melalui tokenisasi patch, di mana setiap bagian citra diubah menjadi vektor yang diproses melalui mekanisme Query, Key, Value, dan self-attention. Multi-head attention memungkinkan model menangkap berbagai pola relasi paralel secara simultan, sedangkan positional encoding memberikan informasi lokasi yang secara inheren tidak dimiliki oleh operasi attention murni. Kombinasi patch tokenization dan mekanisme attention ini menjembatani representasi piksel dengan struktur sequence yang diperlukan Transformer.

Penting untuk ditekankan bahwa performa model tidak hanya ditentukan oleh pemilihan arsitektur, tetapi juga oleh karakteristik dataset. Dataset menentukan definisi tugas, memperkenalkan bias implisit, berfungsi sebagai benchmark standar, sekaligus menetapkan batas generalisasi model. Audit data yang ketat dan protokol pembagian train-validation-test yang rigor sama pentingnya dengan rekayasa arsitektur itu sendiri. Kesalahan dalam split atau ketidakseimbangan kelas dapat menghasilkan evaluasi yang menyesatkan, terutama dalam penelitian tingkat lanjut yang menuntut reproducibility dan validitas statistik yang kuat.

Rangkuman ini menjadi jembatan menuju pembahasan lanjutan di pertemuan berikutnya. Kita akan menguji fondasi ini dengan menganalisis arsitektur CNN modern seperti ResNet dan EfficientNet, serta evolusi Vision Transformer seperti DeiT dan Swin Transformer. Diskusi juga akan mencakup strategi transfer learning, kebutuhan inductive bias, serta metrik evaluasi komprehensif meliputi parameter, FLOPs, latency, dan robustness. Sebagai landasan literatur, referensi kunci yang tercantum pada slide berikutnya—mulai dari karya klasik LeCun dan Krizhevsky hingga Vaswani et al. dan Dosovitskiy et al.—akan menjadi acuan utama dalam membedah perkembangan metodologis dan benchmark standar industri maupun akademik.

---

## Slide 055 - Referensi Kunci

### Narasi

Slide ini menyajikan daftar referensi kunci yang menjadi landasan teoretis dan eksperimental dalam topik representasi visual. Pada jenjang doktoral, penguasaan literatur tidak bersifat kumulatif, melainkan berfungsi sebagai titik awal untuk melakukan kritik metodologis, melacak evolusi arsitektur, dan mengidentifikasi celah penelitian yang belum terjamah. Setiap publikasi yang tercantum merepresentasikan lompatan konseptual yang mengubah cara kita memandang pemrosesan informasi spasial.

Untuk kategori arsitektur dan representasi, kita mulai dari LeCun dkk. (1998) yang memperkenalkan pembelajaran berbasis gradien untuk pengenalan dokumen, menandai kelahiran konvolusi dalam jaringan saraf. Terobosan praktis muncul melalui Krizhevsky dkk. (2012) yang mendemonstrasikan dominasi deep convolutional neural networks pada klasifikasi ImageNet. Masalah degradasi akurasi pada jaringan sangat dalam kemudian diatasi oleh He dkk. (2016) melalui residual learning yang memanfaatkan koneksi shortcut. Pergeseran paradigma terjadi ketika Vaswani dkk. (2017) mempublikasikan *Attention Is All You Need*, yang meletakkan fondasi mekanismenya sepenuhnya di luar operasi konvolusi. Terakhir, Dosovitskiy dkk. (2021) membuktikan bahwa Vision Transformer mampu bersaing dan melampaui CNN pada skala besar, membuka era adopsi arsitektur berbasis attention dalam computer vision.

Di sisi dataset dan benchmark, validitas generalisasi setiap model sangat bergantung pada karakteristik data yang digunakan. CIFAR-10 dan CIFAR-100 berperan sebagai baseline komputasional yang efisien, sementara ImageNet menjadi standar de facto untuk klasifikasi skala besar dengan jutaan sampel. PASCAL VOC dan Microsoft COCO memperluas cakupan ke deteksi objek dan segmentasi semantik, dengan COCO menawarkan anotasi multi-task yang lebih kaya. Cityscapes menjadi rujukan utama untuk pemahaman adegan jalan raya otonom, sedangkan DIV2K, Smartphone Image Denoising Dataset (SIDD), dan GoPro menjadi benchmark esensial untuk tugas image restoration dan deblurring. TID2013 melengkapi ekosistem ini dengan fokus pada kualitas perseptual citra. Dokumentasi resmi dataset tetap menjadi sumber primer yang wajib dirujuk untuk memastikan reproduktibilitas dan transparansi eksperimen.

Daftar ini bukan sekadar bibliografi, melainkan peta konseptual yang menghubungkan teori representasi dengan evaluasi empiris. Memahami bagaimana masing-masing paper merumuskan hipotesis, mendesain eksperimen, dan melaporkan metrik akan membekali Anda dalam menyusun kerangka penelitian yang rigor. Hal ini juga selaras dengan rangkuman pada slide sebelumnya, di mana kita telah membahas bagaimana kernel CNN membangun representasi hierarkis, bagaimana token diproses melalui self-attention, serta bagaimana audit dan split dataset menentukan batas generalisasi model.

Sebelum memasuki perbandingan arsitektur modern, pemahaman mendalam tentang mekanisme pembentukan representasi dan batasan inherent dataset adalah prasyarat mutlak. Pada Pertemuan 03, Anda akan menggunakan fondasi ini untuk menjawab pertanyaan strategis: dalam kondisi data, komputasi, dan tujuan aplikasi tertentu, kapan CNN atau Vision Transformer menjadi pilihan yang lebih tepat? Jawaban atas pertanyaan tersebut akan bergantung pada kemampuan Anda menimbang trade-off antara inductive bias, skalabilitas, efisiensi komputasi, dan karakteristik dataset yang relevan dengan konteks penelitian Anda.

---

## Slide 056 - Penutup

### Narasi

Slide penutup ini menegaskan kembali pesan krusial dari seluruh pembahasan hari ini. Sebelum kita melakukan perbandingan langsung antar arsitektur model modern, kita harus terlebih dahulu memahami mekanisme pembangunan representasi visual dan menyadari sepenuhnya bagaimana karakteristik dataset menentukan batas makna dari setiap hasil eksperimen. Referensi arsitektur dan benchmark yang telah kita telaah pada slide sebelumnya bukan sekadar daftar pustaka, melainkan fondasi empiris yang menunjukkan bahwa performa model selalu berkorelasi erat dengan bias data, resolusi, anotasi, dan distribusi kelas yang tersedia.

Dari perspektif penelitian tingkat doktoral, kesadaran ini menjadi prasyarat utama dalam merancang studi yang rigor. Evaluasi arsitektur tidak boleh dipisahkan dari konteks pengujian. Ketika sebuah model mencapai skor tinggi pada ImageNet, COCO, atau dataset domain-spesifik, kita harus mampu mengurai apakah keberhasilan tersebut berasal dari kekuatan arsitektural murni, atau justru didorong oleh kelimpahan data, augmentasi, dan preprocessing yang unik. Transparansi dalam mencatat keterbatasan dataset dan asumsi inductive bias merupakan standar wajib untuk menghasilkan karya ilmiah yang dapat direproduksi dan berkontribusi nyata terhadap state-of-the-art.

Untuk Pertemuan 03, manfaatkan fondasi ini sebagai kerangka analisis kritis. Anda diminta menyusun jawaban terstruktur atas pertanyaan panduan berikut:

- Identifikasi kondisi ketersediaan data (terbatas vs melimpah) dan batasan komputasi yang relevan dengan konteks riset Anda.
- Tentukan tujuan aplikasi dan metrik evaluasi yang paling tepat untuk mengukur kesuksesan representasi.
- Bandingkan kapan Convolutional Neural Network lebih tepat dipilih dibandingkan Vision Transformer berdasarkan trade-off inductive bias, kebutuhan data, dan efisiensi inferensi.

Pertanyaan ini menuntut Anda menimbang secara objektif antara lokalitas dan efisiensi komputasi CNN versus cakupan konteks global dan skalabilitas ViT, sambil mempertimbangkan kesesuaiannya dengan masalah penelitian yang akan Anda kembangkan. Siapkan catatan mengenai karakteristik dataset benchmark yang selaras dengan minat riset, serta indikator performa apa yang paling adil untuk membandingkan kedua keluarga arsitektur tersebut. Diskusi pada pertemuan berikutnya akan menguji kedalaman analisis Anda dalam menempatkan pilihan arsitektur di tengah dinamika data, infrastruktur, dan novelty penelitian yang akan Anda ajukan dalam proposal disertasi.
