# Narasi TD Pengolahan Citra Digital - Pertemuan 03

## Representasi Visual Modern: CNN, Attention, dan Vision Transformer

Sumber: markdown/pert03-representasi-visual-modern-cnn-attention-dan-vision-transformer.md

---

## Slide 000 - Cover

### Narasi

Slide ini berfungsi sebagai pembuka topik inti hari ini, yaitu representasi visual modern yang mencakup perkembangan arsitektur CNN, mekanisme attention, dan Vision Transformer. Pada jenjang doktoral, pembahasan tidak berhenti pada deskripsi teknis layer atau konfigurasi hyperparameter, melainkan berfokus pada analisis kritis terhadap bagaimana masing-masing pendekatan membentuk ruang representasi fitur, menangani ketergantungan spasial, serta menyeimbangkan akurasi dengan efisiensi komputasi.

Evolusi dari konvolusi lokal menuju perhatian global menandai pergeseran paradigma dalam ekstraksi ciri citra. CNN tetap relevan karena invariansi translasi dan hierarki fitur yang efektif, namun memiliki batasan inherent dalam menangkap dependensi jarak jauh. Mekanisme attention mengatasi hal ini dengan memberikan bobot dinamis pada seluruh region citra, sementara Vision Transformer mengadopsi struktur encoder murni berbasis self-attention yang memungkinkan pemodelan konteks holistik tanpa induksi bias konvolusional. Bedah arsitektur ini akan kita lakukan dengan menyandingkan persamaan matematis, visualisasi attention map, serta analisis kompleksitas komputasi terhadap dataset standar.

Pemahaman mendalam terhadap ketiga fondasi ini menjadi prasyarat mutlak untuk mengevaluasi model-model mutakhir yang akan kita pelajari selanjutnya. Kesiapan menganalisis arsitektur dasar sangat krusial karena akan menentukan seberapa tajam kita dapat mengidentifikasi research gap, menilai validitas metodologi eksperimen, dan memposisikan karya ilmiah kita secara tepat terhadap state-of-the-art. Tanpa fondasi ini, evaluasi terhadap teknik self-supervised learning, multimodal alignment, atau generative vision akan kehilangan kedalaman analitis yang diharapkan pada level doktor.

Pada slide berikutnya, kita akan meninjau peta perjalanan perkuliahan secara ringkas untuk menempatkan materi hari ini dalam konteks keseluruhan kurikulum. Pembahasan ini juga akan menegaskan bagaimana keterampilan critical paper reading dari pertemuan sebelumnya akan langsung diaplikasikan pada bedah paper arsitektur CNN dan Vision Transformer, sekaligus menyiapkan landasan konseptual yang kokoh untuk topik self-supervised learning dan foundation vision models pada sesi-sesi berikutnya.

---

## Slide 001 - Posisi Pertemuan dalam Perkuliahan

### Narasi

Slide ini menempatkan pertemuan ketiga dalam peta perjalanan mata kuliah. Setelah dua sesi awal yang membahas peta riset mutakhir dan kerangka *critical paper reading*, fokus kini beralih ke fondasi teknis representasi visual modern. Tabel pada slide secara eksplisit menunjukkan bahwa topik ini berfungsi sebagai jembatan antara pemahaman konseptual dan implementasi arsitektur inti yang mendominasi literatur computer vision terkini.

Keterkaitan antar pertemuan dirancang untuk membangun alur kumulatif yang terstruktur. Prinsip kritis yang dipelajari pada pertemuan kedua langsung dioperasionalkan pada pembahasan kali ini melalui beberapa poin kunci:
- Membedah asumsi induktif CNN versus pendekatan global Vision Transformer.
- Mengevaluasi bagaimana mekanisme *attention* mengubah cara model memodelkan dependensi spasial dan semantik.
- Mengidentifikasi batasan komputasi dan kebutuhan data pada masing-masing keluarga arsitektur.

Pemahaman mendalam tentang evolusi representasi visual ini menjadi landasan wajib untuk materi lanjutan. Self-supervised learning, model vision-language multimodal, hingga *foundation models* seperti SAM, CLIP, dan DINOv2 sangat bergantung pada kualitas fitur yang diekstrak oleh backbone arsitektur dasar. Tanpa analisis kritis terhadap transisi dari konvolusi lokal menuju perhatian global, penentuan posisi riset terhadap *state-of-the-art* akan kehilangan dasar metodologis yang kuat.

Seiring masuk ke diskusi teknis yang lebih mendalam, perlu ditekankan bahwa evaluasi arsitektur tidak boleh terjebak pada perbandingan akurasi akhir saja. Seperti yang akan kita rekam dan perdalam pada slide berikutnya, perbandingan harus selalu mempertimbangkan baseline yang adil, kontrol eksperimen yang ketat, serta validitas klaim efisiensi atau generalisasi. Pendekatan evaluatif inilah yang membedakan kajian tingkat doktoral dengan tinjauan pustaka konvensional.

---

## Slide 002 - Recap Pertemuan 1 dan 2

### Narasi

Pada slide ini, kita akan meninjau kembali fondasi analitis yang telah dibangun pada dua pertemuan sebelumnya, sebagai landasan langsung untuk membedah representasi visual modern. Sesuai dengan peta perjalanan mata kuliah, fokus kita kini beralih dari identifikasi masalah penelitian menuju evaluasi mendalam terhadap arsitektur model yang menjadi tulang punggung riset terkini di bidang computer vision.

Pertemuan pertama menyoroti pergeseran paradigma mendasar dalam pengolahan citra digital, yaitu transisi sistematis dari metode klasik berbasis fitur manual menuju era deep learning dan foundation models. Pada jenjang doktoral, perhatian tidak hanya tertuju pada performa akhir, tetapi juga pada standar benchmark yang ketat, isu reproduktibilitas hasil eksperimen, serta katalog masalah terbuka yang masih menuntut solusi metodologis baru.

Pertemuan kedua kemudian membekali kita dengan kerangka pembacaan paper secara kritis. Kita telah membahas cara mengurai struktur paper, memverifikasi klaim kontribusi, mengevaluasi bukti empiris, serta mengidentifikasi keterbatasan desain eksperimental. Poin krusial yang ditekankan adalah kemampuan membedakan research gap yang substantif dengan sekadar variasi implementasi, penyesuaian hiperparameter dangkal, atau penggantian dataset tanpa nilai tambah konseptual.

Relevansi poin-poin tersebut sangat menentukan pendekatan kita terhadap perbandingan antara Convolutional Neural Network dan Vision Transformer pada sesi ini. Membandingkan kedua arsitektur tidak dapat direduksi hanya pada angka akurasi validasi atau testing. Sebagai peneliti tingkat S3, kita wajib memeriksa kualitas baseline yang digunakan, konsistensi kontrol eksperimen, serta validitas klaim arsitektural yang diajukan oleh penulis paper asli sebelum menarik kesimpulan umum.

Transisi logis ini akan mengarahkan kita langsung ke tujuan pembelajaran pertemuan ketiga. Kita akan menganalisis perbedaan fundamental terkait inductive bias antara CNN dan Vision Transformer, serta mengevaluasi implikasi praktisnya terhadap kebutuhan data, beban komputasi, dan kemampuan generalisasi lintas domain. Secara teknis, targetnya adalah kemampuan melakukan fine-tuning menggunakan koleksi model dari torchvision atau timm, membandingkan metrik evaluasi, waktu inferensi, jumlah parameter, serta dinamika kurva pembelajaran. Laporan komparatif yang dihasilkan harus melampaui sekadar tabel akurasi, dan berfokus pada analisis metodologis yang rigor sesuai standar publikasi internasional bereputasi.

---

## Slide 003 - Tujuan Pembelajaran Pertemuan 3

### Narasi

Setelah pada pertemuan sebelumnya kita menyoroti pentingnya evaluasi eksperimen yang ketat—bukan sekadar membandingkan angka akurasi akhir—slide ini menegaskan tujuan pembelajaran spesifik untuk Pertemuan 3. Fokusnya bergeser ke analisis mendalam terhadap representasi visual modern, khususnya perbedaan mendasar antara Convolutional Neural Networks (CNN) dan Vision Transformer (ViT), serta bagaimana pilihan arsitektur membentuk seluruh pipeline penelitian Anda.

Berdasarkan capaian sesuai RPS, terdapat tiga pilar analitis yang harus dicapai:
- Menganalisis perbedaan *inductive bias* antara CNN dan Vision Transformer. CNN mengandalkan prior spasial lokal melalui operasi konvolusi, sedangkan ViT mengadopsi mekanisme *self-attention* global yang secara inheren lebih fleksibel namun menuntut strategi regularisasi dan inisialisasi yang berbeda.
- Mengevaluasi implikasi pemilihan arsitektur terhadap kebutuhan data, beban komputasi, dan kemampuan generalisasi. Pemahaman ini menjadi landasan krusial dalam merancang *experimental design* yang valid, terkontrol, dan dapat direproduksi.
- Memilih representasi visual yang paling sesuai dengan karakteristik masalah penelitian disertasi, mengingat tidak ada arsitektur tunggal yang optimal untuk semua domain, dan keputusan arsitektural harus sejalan dengan hipotesis penelitian.

Dari sisi target praktis, mahasiswa ditargetkan mampu melakukan *fine-tuning* model standar dari ekosistem `torchvision` atau `timm` secara mandiri. Kegiatan ini akan diikuti oleh perbandingan sistematis yang mencakup metrik performa, waktu inferensi, jumlah parameter, serta dinamika kurva pembelajaran (*learning curves*). Laporan komparatif yang dihasilkan harus menghindari reduksionisme berbasis akurasi semata, melainkan menyajikan analisis multidimensi yang mencerminkan kedalaman kajian kritis khas penelitian S3.

Seluruh tujuan ini akan diuji melalui serangkaian pertanyaan kunci yang akan kita bahas pada slide berikutnya. Pertanyaan-pertanyaan tersebut dirancang untuk mengarahkan Anda pada identifikasi *research gap* yang konkret, khususnya terkait kapan arsitektur tertentu unggul, bagaimana skala prapelatihan memengaruhi klaim kinerja, dan bagaimana memastikan fairness dalam perbandingan eksperimental. Persiapan teknis dan konseptual pada slide ini merupakan fondasi langsung untuk menjawab tantangan metodologis tersebut.

---

## Slide 004 - Pertanyaan Kunci yang Akan Diuji

### Narasi

Pada slide tujuan pembelajaran sebelumnya, kita telah menetapkan fokus analitis untuk pertemuan ini: menganalisis perbedaan inductive bias antara CNN dan Vision Transformer, mengevaluasi dampaknya terhadap kebutuhan data dan komputasi, serta memilih representasi yang tepat berdasarkan karakteristik masalah penelitian. Slide ini akan menguraikan pertanyaan-pertanyaan kunci yang menjadi landasan untuk merancang eksperimen tingkat doktor dan mengidentifikasi research gap yang valid.

Berikut adalah pertanyaan utama yang akan kita uji sepanjang pertemuan ini:
- Kapan CNN lebih efektif daripada transformer?
- Bagaimana kebutuhan data memengaruhi kinerja masing-masing arsitektur?
- Apakah peningkatan akurasi berasal dari arsitektur atau dari skala pretraining?
- Bagaimana trade-off parameter, FLOPs, interpretasi, dan generalisasi?

Secara empiris, CNN tetap menunjukkan keunggulan pada dataset berukuran kecil hingga menengah yang memiliki struktur spasial lokal kuat, berkat inductive bias translational equivariance dan lokalisasi fitur yang inheren. Sebaliknya, transformer membutuhkan volume data yang jauh lebih besar dan diversifikasi augmentasi agar dapat mempelajari dependensi global secara stabil. Kebutuhan data ini bukan hanya kuantitas, melainkan juga kualitas label, konsistensi distribusi, dan strategi sampling yang menentukan kecepatan konvergensi serta stabilitas kurva pembelajaran.

Pertanyaan mengenai apakah kenaikan akurasi berasal dari inovasi arsitektur atau skala pretraining menuntut pendekatan kritis. Dalam literatur terkini, faktor skala corpus dan strategi masking sering kali mendominasi performa akhir. Oleh karena itu, desain eksperimen harus mengontrol variabel seperti ukuran dataset pretraining, teknik regularisasi, dan protocol optimasi. Evaluasi trade-off juga harus melampaui akurasi validation set, mencakup analisis FLOPs, latency inference, memori footprint, interpretabilitas melalui attention visualization, serta robustness terhadap domain shift.

Untuk menjawab pertanyaan-pertanyaan tersebut secara ilmiah, kita perlu merumuskan pertanyaan turunan yang selaras dengan proyek disertasi Anda. Pertama, tentukan representasi visual apa yang paling kompatibel dengan karakteristik domain target penelitian. Apakah masalah Anda menuntut deteksi objek halus, segmentasi semantik kompleks, atau pemetaan hubungan cross-modal? Kedua, pastikan protokol perbandingan arsitektur dilakukan secara adil dengan menggunakan pipeline preprocessing yang terstandarisasi, library referensi seperti timm atau torchvision, serta pelaporan metrik lengkap yang mencakup confidence calibration dan consistency score di bawah kondisi inferensi nyata.

Pemahaman atas kerangka pertanyaan ini akan menjadi fondasi ketika kita melanjutkan ke pembahasan evolusi representasi visual. Pada slide berikutnya, kita akan menelusuri transisi dari fitur yang dirancang secara manual seperti SIFT dan HOG, menuju fitur yang dipelajari secara end-to-end oleh CNN, hingga representasi berbasis attention global pada transformer. Setiap tahap perkembangan ini membawa implikasi metodologis yang signifikan terhadap cara Anda merumuskan hipotesis, memilih baseline, dan mengevaluasi novelty kontribusi penelitian tingkat doktoral.

---

## Slide 005 - Representasi Visual: Dari Fitur Manual ke Learned Representation

### Narasi

Slide ini menyoroti evolusi fundamental dalam pengolahan citra digital, yaitu pergeseran paradigma dari representasi visual yang dirancang secara manual menuju representasi yang dipelajari secara otomatis oleh model pembelajaran mesin. Secara definisi, representasi visual adalah transformasi sistematis dari data piksel mentah menjadi fitur berdimensi tinggi yang mampu mengekspresikan struktur semantik, sehingga siap digunakan untuk berbagai tugas visi komputer seperti klasifikasi, deteksi, segmentasi, atau estimasi pose.

Pada fase awal computer vision, ekstraksi fitur bersifat *hand-crafted*. Algoritma seperti SIFT, HOG, dan ORB mengandalkan keahlian domain untuk memodelkan tepi, gradien intensitas, atau distribusi orientasi. Pendekatan ini memiliki keterbatasan inheren karena tidak adaptif terhadap variasi iluminasi, deformasi, atau kompleksitas latar belakang di dunia nyata.

Perkembangan *Convolutional Neural Network* (CNN) mengubah lanskap tersebut dengan memperkenalkan *learned representation*. Arsitektur seperti VGG, ResNet, dan EfficientNet mempelajari hierarki fitur secara end-to-end langsung dari data. Karakteristik utamanya adalah bias lokal (*local receptive field*), yang memungkinkan ekstraksi pola spasial bertingkat mulai dari tekstur rendah hingga bentuk objek yang semakin abstrak. Namun, cakupan konteksnya tetap terbatas pada wilayah sekitar kernel konvolusi.

Revolusi berikutnya diwakili oleh arsitektur berbasis *Transformer*, seperti ViT, DeiT, dan Swin Transformer. Melalui mekanisme *self-attention*, setiap token visual dapat berinteraksi dengan seluruh bagian citra secara simultan. Hal ini menghasilkan representasi yang sangat kaya akan konteks jangka panjang, sehingga sering kali menunjukkan keunggulan signifikan pada tugas yang menuntut pemahaman hubungan antar objek atau pemisahan foreground-background yang kompleks.

Pemilihan jenis arsitektur secara langsung menentukan sifat representasi yang dihasilkan, yang kemudian berdampak pada tiga dimensi kritis dalam riset tingkat doktor: kebutuhan volume data, beban komputasi, serta kemampuan generalisasi lintas domain. Pertanyaan kunci yang diajukan pada slide sebelumnya mengenai kapan CNN lebih efektif daripada transformer, atau bagaimana trade-off antara akurasi, FLOPs, dan interpretabilitas, akan terus menjadi acuan dalam merancang eksperimen yang ketat.

Memasuki slide berikutnya, kita akan membedah blok-blok konstruksi dasar arsitektur CNN modern. Analisis terhadap komponen seperti lapisan konvolusi, normalisasi batch, fungsi aktivasi non-linear, dan operasi pooling akan memberikan fondasi teknis untuk memahami mengapa modifikasi seperti residual connection atau dense connectivity mampu meningkatkan stabilitas pelatihan dan kualitas representasi, sekaligus menjawab pertanyaan terkait efisiensi dan skalabilitas model.

---

## Slide 006 - Arsitektur CNN Modern: Blok Dasar

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi pergeseran fundamental dalam representasi visual, dari fitur yang dirancang secara manual berbasis pengetahuan domain menuju representasi yang dipelajari secara end-to-end oleh jaringan saraf. Paradigma yang paling dominan dan menjadi fondasi awal perkembangan *deep learning* untuk visi komputer adalah Convolutional Neural Network atau CNN. Sebelum kita mendalami operasi matematika di balik konvolusi, penting bagi kita untuk memahami bagaimana blok-blok dasar ini dirangkai menjadi arsitektur modern yang mampu mengekstrak hierarki fitur yang semakin abstrak.

Alur umum pemrosesan dalam CNN dapat dipahami sebagai rangkaian transformasi bertahap. Input citra pertama-tama melewati lapisan konvolusi yang berfungsi sebagai ekstraktor fitur lokal. Hasilnya kemudian distabilkan distribusinya melalui Batch Normalization, dilanjutkan dengan fungsi aktivasi non-linear seperti ReLU untuk memperkenalkan kapasitas pemodelan yang kompleks. Tahapan reduksi resolusi spasial dilakukan oleh operasi pooling, dan pola ini diulang beberapa kali hingga fitur mencapai tingkat abstraksi yang tinggi. Di tahap akhir, Global Average Pooling sering digunakan untuk mereduksi dimensi spasial sebelum dilewatkan ke lapisan fully connected untuk menghasilkan prediksi akhir.

Setiap komponen dalam alur tersebut memiliki peran krusial yang tidak bisa dipisahkan:
- Konvolusi memanfaatkan kernel yang bergerak melintasi citra untuk menangkap pola geometris dan tekstur pada skala tertentu.
- Batch Normalization tidak hanya mempercepat konvergensi selama pelatihan, tetapi juga berperan sebagai regularisasi halus yang membantu model generalize lebih baik pada data yang terdistribusi tidak seragam.
- ReLU memecahkan masalah vanishing gradient pada jaringan dalam, sekaligus menjaga sparsity aktivasi.
- Pooling memberikan invariansi translasi parsial dan mengurangi beban komputasi serta memori untuk lapisan berikutnya.

Inovasi pada arsitektur CNN modern muncul ketika peneliti menyadari bahwa penambahan kedalaman jaringan secara naif justru menghambat pembelajaran. ResNet mengatasi hal ini dengan memperkenalkan residual connection, yang memungkinkan gradien mengalir langsung melalui skip connection sehingga jaringan sangat dalam tetap dapat dilatih dengan stabil. DenseNet memperluas konsep ini dengan menghubungkan setiap lapisan ke setiap lapisan berikutnya, memaksimalkan reuse fitur dan efisiensi parameter. Sementara itu, EfficientNet menawarkan pendekatan scaling yang sistematis dengan menyeimbangkan tiga dimensi: kedalaman (*depth*), lebar (*width*), dan resolusi input, menggunakan compound scaling coefficient yang dioptimalkan melalui neural architecture search.

Pemahaman terhadap blok dasar dan inovasi arsitektural ini menjadi prasyarat penting sebelum kita membedah mekanisme inti yang menjadikannya bekerja. Pada slide berikutnya, kita akan menyoroti operasi konvolusi secara eksplisit, mulai dari ide dasar penggeseran kernel, notasi matematis formal, hingga implementasinya dalam PyTorch. Hal ini akan memberikan landasan teknis yang solid untuk menganalisis bagaimana bias dan bobot berkontribusi pada pembentukan representasi visual yang kita bahas hari ini.

---

## Slide 007 - Operasi Konvolusi

### Narasi

Setelah membahas blok-blok dasar arsitektur CNN pada slide sebelumnya, kita kini akan menyoroti operasi fundamental yang menjadi tulang punggung ekstraksi fitur tersebut, yaitu operasi konvolusi. Pada tingkat doktoral, penting untuk memahami bahwa kekuatan CNN tidak hanya terletak pada kedalaman jaringan, melainkan pada mekanisme berbagi parameter secara spasial melalui kernel kecil yang digeser melintasi citra. Pendekatan ini memungkinkan deteksi pola lokal yang konsisten, sekaligus mengurangi kompleksitas komputasi dibandingkan fully connected layers tradisional.

Secara matematis, operasi ini dapat direpresentasikan sebagai penjumlahan hasil kali titik antara nilai piksel input dan bobot kernel pada setiap posisi spasial. Perhatikan notasi berikut:
```text
Y[i, j] = sum_k sum_m sum_n X[i+m, j+n] * W[m, n, k] + b
```
Di sini, $X$ merepresentasikan tensor input, $W$ adalah filter atau kernel dengan dimensi spasial $(m, n)$ dan kanal $k$, sedangkan $b$ adalah bias. Indeks $i$ dan $j$ menunjukkan koordinat spasial pada feature map output $Y$. Penjumlahan atas $k$ mengindikasikan akumulasi respons dari seluruh kanal input, yang kemudian digabungkan sebelum penambahan bias. Pemahaman notasi ini krusial untuk menganalisis propagasi gradien dan desain arsitektur lanjutan.

Dalam praktikum menggunakan PyTorch, operasi ini diimplementasikan secara efisien melalui modul `torch.nn.Conv2d`. Berikut adalah contoh konfigurasi standar untuk memproses gambar RGB:
```python
import torch.nn as nn

conv = nn.Conv2d(
    in_channels=3,
    out_channels=16,
    kernel_size=3,
    stride=1,
    padding=1
)

### Input: (B, 3, H, W) -> Output: (B, 16, H, W)

```
Parameter `in_channels=3` menyesuaikan dengan tiga kanal warna RGB, sementara `out_channels=16` menentukan jumlah filter yang akan menghasilkan enam belas representasi fitur berbeda. Dengan `kernel_size=3`, `stride=1`, dan `padding=1`, dimensi spasial $(H, W)$ tetap dipertahankan setelah konvolusi. Ini sangat berguna ketika kita ingin membangun blok berlapis tanpa kehilangan resolusi spasial awal, yang nantinya akan menjadi prasyarat penting untuk mekanisme residual connection.

Menjaga dimensi spasial seperti ini bukan sekadar trik implementasi, melainkan strategi desain yang langsung berkaitan dengan tantangan optimasi pada jaringan sangat dalam. Ketika kita menumpuk banyak lapisan konvolusi tanpa mekanisme khusus, aliran gradien cenderung melemah atau mengalami degradasi performa training, sebagaimana akan kita bahas pada slide berikutnya mengenai residual connection. Oleh karena itu, pemahaman mendalam tentang bagaimana konvolusi memanipulasi ruang fitur dan mempertahankan struktur spasial menjadi fondasi kritis sebelum memasuki arsitektur transformer-based atau model generatif mutakhir.

---

## Slide 008 - Residual Connection

### Narasi

Setelah membahas operasi konvolusi pada slide sebelumnya sebagai mekanisme dasar ekstraksi fitur lokal, kita kini berhadapan dengan batasan fundamental ketika lapisan-lapisan tersebut ditumpuk secara masif. Semakin dalam arsitektur jaringan saraf konvolusional, penurunan akurasi yang teramati selama pelatihan bukanlah semata-mata akibat overfitting, melainkan indikasi kuat adanya degradasi optimasi. Jaringan mengalami kesulitan belajar representasi yang lebih baik karena gradien menjadi semakin sulit untuk merambat mundur secara stabil melalui banyak lapisan non-linear.

Untuk memecah hambatan optimasi ini, konsep *Residual Connection* diperkenalkan sebagai struktur jembatan yang mengubah dinamika aliran informasi dalam jaringan. Secara matematis, keluaran dari sebuah blok residual dirumuskan sebagai:

```text
Output = F(x) + x
```

Di sini, $F(x)$ merepresentasikan transformasi non-linear yang dipelajari oleh serangkaian lapisan konvolusi dan aktivasi, sedangkan $x$ adalah jalur identitas (*identity shortcut*) yang menghubungkan input langsung ke output. Penambahan ini bersifat element-wise dan tidak menambah parameter baru pada jaringan.

Mekanisme ini memiliki dampak optimasi yang sangat kritis. Jalur identitas memungkinkan gradien mengalir secara langsung tanpa harus melewati deretan fungsi aktivasi atau konvolusi, sehingga mitigasi masalah *vanishing gradient* dapat dilakukan secara alami. Selain itu, arsitektur ini memberikan fleksibilitas bagi jaringan untuk mempelajari fungsi identitas jika memang diperlukan, artinya lapisan tambahan tidak akan merusak representasi yang sudah terbentuk sebelumnya.

Dampak dari penerapan residual connection jauh melampaui perbaikan akurasi pada dataset tertentu. Teknik ini secara fundamental membuka kemungkinan untuk melatih arsitektur dengan ratusan bahkan ribuan lapisan, seperti keluarga ResNet atau model-modern yang lebih kompleks. Karena kemampuannya yang konsisten meningkatkan stabilitas pelatihan dan mempercepat konvergensi, residual connection telah menjadi komponen standar yang hampir selalu hadir dalam setiap arsitektur visual modern, termasuk baseline untuk transformer dan arsitektur hibrida.

Dengan fondasi kedalaman yang stabil melalui residual connection, langkah logis berikutnya adalah mengatur distribusi aktivasi di dalam jaringan agar proses pembelajaran tetap efisien dan robust. Pada slide selanjutnya, kita akan mengupas peran *Normalization Layer*, khususnya bagaimana teknik seperti Batch Normalization dan Layer Normalization bekerja berdampingan dengan residual connection untuk menstabilkan pelatihan model skala besar dan mempersiapkan transisi menuju arsitektur berbasis attention.

---

## Slide 009 - Normalization Layer

### Narasi

Setelah membahas residual connection pada slide sebelumnya yang memungkinkan gradien mengalir lebih lancar melalui jalur identitas, kita kini memasuki komponen krusial lain yang menjadi standar dalam arsitektur jaringan saraf modern: normalisasi lapisan. Tanpa mekanisme stabilisasi distribusi aktivasi, penambahan kedalaman jaringan—even dengan residual connection—cenderung mengalami ketidakstabilan numerik selama pelatihan. Normalisasi layer hadir untuk mengatasi masalah ini dengan menstabilkan distribusi aktivasi di setiap lapisan, mempercepat laju konvergensi, serta mengurangi sensitivitas model terhadap pilihan inisialisasi bobot dan nilai learning rate.

Terdapat tiga varian utama normalisasi yang umum digunakan dalam praktik pengolahan citra digital dan computer vision:
- **Batch Normalization**: melakukan normalisasi secara per batch dan per channel. Variabel ini sangat efektif pada arsitektur CNN konvensional karena memanfaatkan statistik global dari satu mini-batch.
- **Layer Normalization**: melakukan normalisasi per sampel melintasi seluruh channel. Pendekatan ini tidak bergantung pada ukuran batch, sehingga lebih robust ketika dimensi batch terbatas.
- **Group Normalization**: membagi channel ke dalam beberapa kelompok dan melakukan normalisasi per sampel per kelompok. Teknik ini sering diadopsi pada tugas dengan resolusi tinggi atau batch size kecil, seperti segmentasi medis atau pemrosesan video.

Untuk konteks Vision Transformer (ViT), penggunaan Layer Normalization menjadi pilihan dominan dibandingkan Batch Normalization. Hal ini disebabkan oleh cara kerja transformer yang memproses sekuen patch gambar secara independen melalui mekanisme attention. Layer Normalization sejalan dengan paradigma pemrosesan berbasis sampel ini, menjaga konsistensi skala fitur tanpa terpengaruh oleh variasi statistik antar-batch yang mungkin tidak konsisten pada urutan spasial patch.

Integrasi antara residual connection dan normalisasi layer membentuk fondasi stabil bagi pelatihan arsitektur dalam. Namun, stabilitas numerik saja tidak cukup; struktur arsitektur itu sendiri membawa asumsi bawaan mengenai bagaimana data harus dipelajari. Asumsi inilah yang dikenal sebagai inductive bias, yang akan kita bedah lebih lanjut pada slide berikutnya, khususnya terkait bagaimana CNN menginternalisasikan lokalitas, ekuivariansi translasi, dan hierarki spasial sebagai prior mendasar sebelum melihat data.

---

## Slide 010 - Inductive Bias CNN

### Narasi

Setelah membahas mekanisme normalisasi pada slide sebelumnya, khususnya perbedaan mendasar antara Batch Normalization yang lazim pada CNN dan Layer Normalization yang menjadi standar di arsitektur Transformer, kita kini beralih ke konsep fundamental yang membedakan kedua paradigma representasi visual ini: *inductive bias*. Inductive bias merujuk pada sekumpulan asumsi struktural dan matematis yang melekat pada arsitektur model mengenai sifat data target, bahkan sebelum proses optimisasi dimulai. Dalam konteks penelitian tingkat doktoral, memahami bias ini penting untuk menilai mengapa suatu arsitektur mampu menggeneralisasi dari sampel terbatas, serta kapan asumsi bawaan tersebut justru menjadi kendala ketika menghadapi distribusi data yang tidak sesuai dengan prior model.

Pada arsitektur CNN, terdapat tiga indikasi bias utama yang menjadi fondasi pemrosesan visualnya:
- **Lokalitas**: asumsi bahwa piksel yang berdekatan secara spasial memiliki ketergantungan semantik yang jauh lebih kuat daripada piksel yang berjauhan.
- **Translasi ekuivariansi**: prinsip yang menjamin deteksi pola atau fitur tertentu tetap konsisten, terlepas dari posisi objek dalam frame citra.
- **Hierarki spasial**: mekanisme pembangunan representasi secara bertahap, mulai dari tepi rendah (*low-level edges*), berkembang menjadi bagian objek (*parts*), hingga mencapai abstraksi tingkat tinggi (*high-level concepts*) pada lapisan yang lebih dalam.

Kombinasi ketiga bias ini memberikan dampak langsung terhadap efisiensi pembelajaran. CNN dapat belajar secara efektif dari dataset yang relatif kecil karena arsitekturnya sudah diprioritaskan untuk menangkap struktur spasial lokal yang dominan pada citra alami. Namun, kekuatan ini juga bersifat ganda. Ketika kita bergerak menuju domain yang memerlukan pemahaman konteks global atau integrasi multimodal, asumsi lokalitas dan hierarki bertahap ini mulai menunjukkan batasannya. Pembahasan mengenai keunggulan teknis sekaligus keterbatasan fundamental dari pendekatan berbasis CNN akan kita uraikan secara kritis pada slide berikutnya.

---

## Slide 011 - Kekuatan dan Keterbatasan CNN

### Narasi

Merujuk pada konsep *inductive bias* yang telah dibahas sebelumnya, asumsi bawaan seperti lokalitas dan translasi ekuivariansi pada CNN ternyata menghasilkan implikasi praktis yang signifikan. Berikut adalah uraian mendalam mengenai kekuatan dan keterbatasan arsitektur ini dalam konteks penelitian tingkat doktoral:

**Kekuatan Utama:**
- Efisiensi parameter tinggi berkat mekanisme *weight sharing* yang meminimalkan redundansi pembelajaran dan mengurangi risiko overfitting.
- Stabilitas pelatihan yang baik pada dataset berukuran menengah, didukung oleh regularisasi implisit dari struktur konvolusi.
- Pertumbuhan *receptive field* yang bertahap memudahkan interpretasi spasial dan pelacakan propagasi fitur dari lapisan rendah ke tinggi.
- Ekosistem *pretrained model* yang sangat matang mempercepat iterasi eksperimen, transfer learning, dan establishment baseline yang solid.

**Keterbatasan Struktural:**
- *Receptive field* pada lapisan awal bersifat sangat lokal, sehingga konteks global tidak dapat diakses secara langsung tanpa propagasi berlapis.
- Penangkapan hubungan jarak jauh menuntut kedalaman jaringan yang ekstrem, berpotensi memicu degradasi gradien dan meningkatkan beban komputasi secara kuadratik.
- Mekanisme konvolusi murni sulit memodelkan ketergantungan non-lokal antar region citra secara eksplisit dan adaptif.
- Arsitektur berbasis grid piksel tidak kompatibel langsung dengan representasi multimodal atau data non-Euclidean tanpa transformasi dan rekayasa arsitektur tambahan.

Keterbatasan dalam pemodelan dependensi global ini secara historis dan metodologis mengarah pada kebutuhan akan mekanisme seleksi informasi yang lebih fleksibel. Berbeda dengan filter konvolusi yang bergerak secara lokal dan statis, pendekatan baru memungkinkan penimbangan bobot dinamis berdasarkan kesesuaian semantik antar elemen input. Pada slide berikutnya, kita akan membedah ide dasar *attention*, mulai dari motivasi kognitif manusia, formulasi *query-key-value*, hingga analogi fungsionalnya sebagai batu loncatan teoretis menuju Vision Transformer dan fondasi model modern lainnya.

---

## Slide 012 - Attention: Ide Dasar

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi bahwa arsitektur CNN memiliki keterbatasan struktural dalam memodelkan ketergantungan global akibat mekanisme *receptive field* yang bersifat lokal dan bertahap. Untuk mengatasi celah representasi ini, konsep *attention* diperkenalkan sebagai paradigma yang memungkinkan model secara dinamis menyeleksi informasi relevan dari seluruh elemen input, tanpa terikat oleh struktur grid spasial yang kaku. Pergeseran ini menandai evolusi fundamental dari pemrosesan berbasis lokalisasi menuju pemrosesan berbasis relasi kontekstual.

Secara konseptual, mekanisme *attention* beroperasi melalui tiga komponen inti: *query*, *key*, dan *value*. Ketika sebuah *query* diberikan, model menghitung tingkat kesesuaian atau kompatibilitas antara *query* tersebut dengan setiap *key* yang tersedia. Hasil kesesuaian ini kemudian dinormalisasi menjadi distribusi probabilitas menggunakan fungsi *softmax*, yang berfungsi sebagai bobot perhatian. Bobot inilah yang menentukan seberapa besar kontribusi masing-masing *value* terhadap representasi output. Dengan demikian, model tidak lagi memperlakukan seluruh piksel atau patch citra secara setara, melainkan memberikan penekanan selektif pada wilayah yang paling informatif bagi tugas yang sedang dipelajari.

Analogi pencarian buku di perpustakaan dapat membantu memetakan mekanisme abstrak ini ke konteks yang lebih konkret. Dalam skenario tersebut, *query* merepresentasikan topik atau kata kunci pencarian, katalog perpustakaan menyediakan *key* berupa judul atau metadata buku, dan *value* adalah konten aktual dari buku yang dipilih. Sistem pencocokan akan menghubungkan *query* dengan *key* yang paling relevan, lalu mengekstrak informasi dari *value* yang bersesuaian. Dalam domain pengolahan citra digital, setiap token atau *patch* citra berperan sebagai entitas yang saling berkomunikasi melalui pasangan *query-key-value* ini, sehingga hubungan semantik antar wilayah citra yang terpisah secara spasial dapat dimodelkan secara eksplisit.

Konsep dasar ini menjadi landasan teoretis sebelum kita masuk ke formulasi matematis yang akan dibahas pada slide berikutnya. Transformasi linear untuk menghasilkan *Q*, *K*, dan *V*, serta persamaan *scaled dot-product attention*, merupakan implementasi komputasional dari prinsip seleksi informasi yang baru saja dijelaskan. Pemahaman intuitif mengenai bagaimana bobot perhatian terbentuk dari kesesuaian *query-key* akan memudahkan analisis kritis terhadap perilaku model *self-attention* dan implikasinya terhadap efisiensi komputasi serta kapasitas representasi pada arsitektur modern seperti Vision Transformer.

---

## Slide 013 - Self-Attention: Query, Key, Value

### Narasi

Pada slide sebelumnya, kita telah membahas motivasi konseptual dari mekanisme attention, yaitu perlunya model untuk memfokuskan komputasinya pada subset informasi yang paling relevan daripada memperlakukan seluruh elemen input secara ekuivalen. Slide ini akan menguraikan formulasi matematis konkret yang mengimplementasikan ide tersebut, yaitu Self-Attention berbasis triplet Query, Key, dan Value.

Proses dimulai dengan transformasi linear pada representasi input awal $X$. Melalui matriks bobot yang dipelajari secara end-to-end, yaitu $W_Q$, $W_K$, dan $W_V$, setiap token atau patch citra diproyeksikan ke dalam tiga ruang vektor yang berbeda. Proyeksi ini menghasilkan matriks Query ($Q$), Key ($K$), dan Value ($V$). Meskipun ketiganya diturunkan dari sumber data yang sama, masing-masing memegang peran fungsional yang spesifik: Query mewakili pertanyaan pencarian, Key berfungsi sebagai indeks pencocokan, dan Value menyimpan konten informasi aktual.

Setelah proyeksi selesai, skor kesesuaian dihitung menggunakan operasi dot product antara $Q$ dan transpose $K$, kemudian distabilkan dan dinormalisasi melalui fungsi softmax. Rumus inti yang diterapkan adalah:
```text
Attention(Q, K, V) = softmax(Q K^T / sqrt(d_k)) V
```
Faktor skala $\sqrt{d_k}$ bukan sekadar penyederhanaan aljabar, melainkan komponen kritis untuk menjaga stabilitas numerik. Ketika dimensi fitur $d_k$ bernilai besar, nilai dot product cenderung melonjak, sehingga distribusi softmax menjadi sangat tajam (mendekati one-hot encoding). Kondisi ini memicu vanishing gradient yang menghambat pembelajaran. Pembagian dengan akar kuadrat dimensi memastikan varian skor tetap terkendal, sehingga gradien dapat mengalir lancar selama backpropagation.

Dari perspektif arsitektural, mekanisme ini memberikan sifat global receptive field secara inheren. Setiap token secara langsung berinteraksi dengan seluruh token lain dalam sekuens, tanpa dibatasi oleh ukuran kernel atau struktur grid lokal seperti pada arsitektur CNN konvensional. Bobot attention yang dihasilkan merepresentasikan tingkat ketergantungan semantik atau struktural antar elemen input, memungkinkan model menangkap relasi jarak jauh secara eksplisit dan adaptif terhadap konten gambar.

Dengan fondasi single-head self-attention yang telah dipahami, kita siap beralih ke slide berikutnya. Di sana akan dibahas mengapa satu head seringkali belum mampu mengekspresikan kompleksitas representasi visual secara optimal, serta bagaimana multi-head attention memanfaatkan paralelisme untuk menangkap berbagai pola hubungan secara simultan, lengkap dengan strategi skalasi yang mencegah degradasi performa.

---

## Slide 014 - Multi-Head Attention dan Skala

### Narasi

Setelah membahas mekanisme self-attention dasar pada slide sebelumnya, kita kini mengonsepkan bagaimana mekanisme ini diperkuat melalui arsitektur *Multi-Head Attention*. Menjalankan satu head saja seringkali tidak cukup untuk menangkap kompleksitas representasi visual yang tinggi. Oleh karena itu, beberapa head dijalankan secara paralel. Setiap head memiliki ruang proyeksi bobot yang berbeda, sehingga secara alami mereka cenderung fokus pada aspek representasi yang beragam. Hasil dari semua head tersebut kemudian digabungkan (*concatenate*) dan diproyeksikan kembali melalui lapisan linear untuk menghasilkan output akhir yang kaya informasi.

Secara operasional, proses ini dapat diilustrasikan sebagai jalur paralel yang memproses input yang sama:
- Satu head dapat menangkap hubungan lokal atau pola tekstur halus.
- Head lain merespons konteks global atau ketergantungan semantik skala besar.
- Head ketiga fokus pada interaksi spasial antar wilayah yang terpisah jauh.

Setelah tahap *concatenation*, transformasi linear terakhir berfungsi untuk menyintesis informasi dari berbagai perspektif tersebut menjadi vektor representasi yang kohesif. Pendekatan ini secara signifikan meningkatkan kapasitas model tanpa menambah beban komputasi yang proporsional terhadap dimensi fitur asli, sekaligus menjaga fleksibilitas arsitektur.

Aspek krusial lain yang perlu diperhatikan adalah faktor penskalaan pada pembagi *sqrt(d_k)*. Ketika dimensi ruang proyeksi (*d_k*) bertambah besar, hasil perkalian dot product antara *Query* dan *Key* cenderung menghasilkan nilai skalar yang sangat besar. Nilai ekstrem ini menyebabkan fungsi *softmax* bekerja di ekor distribusi, di mana gradiennya mendekati nol. Kondisi ini dikenal sebagai saturasi gradien, yang menghambat propagasi kesalahan selama backpropagation dan membuat pelatihan jaringan menjadi tidak stabil. Dengan membagi skor perhatian dengan akar kuadrat dari dimensi kunci, magnitudo nilai input ke *softmax* dijaga agar tetap berada pada rentang yang optimal, memastikan gradien yang bermakna tetap mengalir dan konvergensi model berjalan lancar.

Mekanisme *multi-head* dan penskalaan ini bukan sekadar trik teknis, melainkan fondasi desain yang memungkinkan transformer menangani data berdimensi tinggi secara efisien. Prinsip-prinsip inilah yang menjadi jembatan ketika kita mengadaptasi arsitektur yang awalnya lahir untuk pemrosesan bahasa alami ke dalam domain visi komputer. Pada slide berikutnya, kita akan melihat bagaimana konsep *patch tokenization* dan arsitektur *Vision Transformer* memanfaatkan fondasi ini untuk merevolusi cara kita merepresentasikan dan memahami konten visual.

---

## Slide 015 - Dari NLP ke Vision: Attention Is All You Need

### Narasi

Pada slide sebelumnya, kita telah menguraikan mekanisme multi-head attention dan urgensi pembagian skala $\sqrt{d_k}$ untuk mencegah saturasi gradien pada fungsi softmax. Pembahasan teknis ini menjadi landasan penting sebelum kita menelaah bagaimana arsitektur berbasis attention berhasil melakukan transisi paradigma dari pemrosesan bahasa alami menuju visi komputer.

Transformer pertama kali dipublikasikan oleh Vaswani dkk. pada tahun 2017 melalui makalah *Attention Is All You Need*. Secara fundamental, arsitektur ini menghilangkan ketergantungan pada lapisan recurrent maupun convolutional yang lazim digunakan dalam pemrosesan sekuensial. Seluruh kapasitas model untuk menangkap ketergantungan jangka panjang dialihkan sepenuhnya ke mekanisme self-attention. Inovasi ini membuktikan bahwa struktur attention murni mampu melampaui performa arsitektur tradisional dalam tugas machine translation dan segera menjadi standar baru di komunitas NLP.

Tantangan utama muncul ketika upaya adaptasi dilakukan ke domain citra digital. Berbeda dengan teks yang secara inheren berbentuk sekuens kata berurutan, matriks piksel pada citra bersifat spasial dan tidak memiliki urutan natural yang kompatibel dengan input Transformer. Solusi yang diperlukan adalah konversi representasi visual menjadi format sekuensial melalui strategi patch tokenization. Citra dibagi menjadi sejumlah patch berukuran tetap, lalu setiap patch diproyeksikan menjadi vektor embedding yang bertindak sebagai unit dasar pemrosesan, analog dengan token kata dalam NLP.

Setelah tahap patch tokenization selesai, data visual telah dikonversi ke ruang representasi yang siap menerima blok Transformer Encoder. Transisi ini secara langsung membuka jalan bagi implementasi Vision Transformer yang akan kita analisis pada slide berikutnya. Pada pembahasan selanjutnya, kita akan mendalami bagaimana patch embeddings digabungkan dengan positional encoding, serta bagaimana token agregat `[CLS]` dimanfaatkan untuk menghasilkan fitur global guna prediksi kelas.

---

## Slide 016 - Vision Transformer: Arsitektur

### Narasi

Setelah pada slide sebelumnya kita membahas bagaimana mekanisme *self-attention* yang awalnya dirancang untuk pemrosesan bahasa alami berhasil diadaptasi ke domain visi komputer melalui konsep dasar pembagian citra menjadi token, kini kita akan mendalami arsitektur lengkap dari Vision Transformer atau ViT. Slide ini menyajikan alur pemrosesan end-to-end yang diusulkan oleh Dosovitskiy dkk. pada tahun 2021, sebuah karya yang secara fundamental mengubah paradigma representasi visual tanpa bergantung pada operasi konvolusi tradisional.

Alur pemrosesan dimulai dari input citra yang langsung dipetakan ke dalam serangkaian vektor melalui tahap *patch embedding*. Hasil embedding tersebut kemudian ditambahkan dengan *positional encoding* untuk mempertahankan informasi spasial yang hilang akibat proses flattening. Tahap ini diikuti oleh serangkaian blok *Transformer Encoder* yang berulang sebanyak L kali. Setiap blok encoder menerapkan mekanisme multi-head self-attention dan feed-forward network, memungkinkan setiap patch berinteraksi secara global dengan seluruh patch lainnya dalam citra.

Pada akhir rangkaian encoder, output dari token khusus `[CLS]` diekstrak sebagai representasi agregat dari seluruh konten citra. Token ini dipilih karena posisinya yang konsisten di awal sekuens dan sifatnya yang mengakumulasi informasi kontekstual setelah melewati beberapa lapisan attention. Representasi `[CLS]` kemudian dilewatkan melalui *MLP Head* untuk menghasilkan prediksi kelas akhir. Pendekatan ini menghilangkan kebutuhan akan pooling layer atau struktur hierarkis yang umum ditemukan pada arsitektur CNN konvensional.

Poin krusial dari arsitektur ini adalah keseragaman pemrosesan. Tidak ada satu pun operasi konvolusi yang digunakan di seluruh jaringan. Semua hubungan spasial dan semantik antar patch dipelajari murni melalui matriks attention, yang memberikan fleksibilitas tinggi dalam menangkap dependensi jarak jauh. Meskipun sederhana secara konseptual, desain ini menunjukkan bahwa skala data dan komputasi yang memadai dapat membuat model berbasis attention mengalahkan arsitektur CNN yang telah lama mendominasi bidang computer vision.

Untuk memahami bagaimana transformasi dari piksel menjadi token ini terjadi secara teknis, kita akan menguraikan lebih lanjut pada slide berikutnya mengenai mekanisme *patch tokenization*. Di sana akan dibahas detail proyeksi linear menggunakan operasi konvolusi non-overlap sebagai implementasi efisien, serta bagaimana dimensi token dan ukuran patch mempengaruhi kapasitas representasi model sebelum masuk ke tahap pembelajaran attention.

---

## Slide 017 - Patch Tokenization

### Narasi

Pada slide sebelumnya kita telah menelusuri arsitektur umum Vision Transformer secara makro, mulai dari input citra hingga prediksi kelas melalui MLP head. Langkah krusial yang menjembatani representasi piksel tradisional dengan mekanisme *self-attention* adalah proses tokenisasi patch, yang akan kita bedah pada slide ini. Berbeda dengan CNN yang memproses seluruh peta fitur secara hierarkis, ViT menguraikan citra menjadi unit-unit diskrit yang setara posisinya sebelum dimasukan ke dalam blok transformer.

Secara teknis, alur tokenisasi berjalan melalui beberapa tahap terstruktur:
- Citra berdimensi $H \times W \times C$ dipotong secara non-overlapping menggunakan ukuran patch $P \times P$.
- Setiap patch di-*flatten* menjadi vektor satu dimensi.
- Vektor tersebut diproyeksikan secara linear ke ruang berdimensi $D$.
Sebagai ilustrasi konkret, jika kita menggunakan citra RGB berukuran $224 \times 224$ dengan $P=16$, maka jumlah total patch yang dihasilkan adalah $(224/16) \times (224/16) = 196$ token. Setiap token merepresentasikan wilayah lokal citra yang akan diperlakukan sebagai elemen urutan dalam rangkaian transformer.

Implementasi praktis dari tahap ini sering kali memanfaatkan lapisan konvolusi untuk efisiensi komputasi, sebagaimana ditunjukkan pada potongan kode berikut:
```python
import torch.nn as nn

P, D = 16, 768
patch_embed = nn.Conv2d(
    in_channels=3,
    out_channels=D,
    kernel_size=P,
    stride=P
)
```
Perhatikan bahwa parameter `kernel_size` dan `stride` diatur sama dengan nilai $P$. Operasi ini menghasilkan tensor keluaran dengan bentuk $(B, D, 14, 14)$ dari input $(B, 3, 224, 224)$. Bentuk spasial $(14, 14)$ kemudian di-*flatten* pada dimensi kedua menjadi $(B, 196, D)$ agar siap dilewatkan ke lapisan *positional embedding* dan blok encoder transformer. Penggunaan `Conv2d` di sini bukan berarti kita kembali ke paradigma CNN, melainkan sekadar trik implementasi yang sangat efisien untuk melakukan pemotongan dan proyeksi dimensi secara bersamaan dalam satu operasi terdiferensiasi.

Penting untuk dicatat bahwa meskipun secara matematis operasi ini dapat dipandang sebagai konvolusi non-overlapping, perlakuan terhadap hasil keluarannya sangat berbeda dari jaringan konvolusional tradisional. Pada CNN, peta fitur diproses secara hierarkis melalui lapisan-lapisan konvolusi berturut-turut untuk menangkap konteks spasial bertingkat. Sebaliknya, pada ViT, setiap token patch berdiri sendiri dan hubungan antar patch hanya dibangun sepenuhnya melalui mekanisme *multi-head self-attention* setelah tahap ini. Ini menegaskan filosofi dasar ViT: menggantikan induksi bias konvolusi dengan pembelajaran hubungan global secara eksplisit.

Setelah token berhasil dibentuk, tantangan berikutnya muncul karena mekanisme *self-attention* bersifat permutasi-invarian, artinya model tidak memiliki pemahaman bawaan mengenai posisi relatif antar patch. Hal inilah yang membawa kita langsung ke pembahasan slide berikutnya, yaitu *Positional Encoding*, di mana kita akan membahas bagaimana informasi spasial reintroduksi ke dalam rangkaian token agar transformer mampu memahami struktur geometris citra secara utuh.

---

## Slide 018 - Positional Encoding

### Narasi

Setelah proses patch tokenization mengubah citra menjadi sekumpulan vektor token pada slide sebelumnya, kita menghadapi tantangan fundamental dalam arsitektur berbasis attention. Mechanism self-attention secara inheren bersifat permutation-invariant, artinya ia memproses seluruh token secara simultan tanpa mekanisme bawaan untuk mengenali urutan atau posisi relatif antar patch. Jika susunan token diacak sebelum dimasukkan ke dalam layer attention, output representasinya akan tetap identik. Hal ini jelas bertentangan dengan sifat alami citra digital di mana konteks spasial menentukan makna semantik; patch di pojok kiri atas tidak dapat diperlakukan sama dengan patch di kanan bawah hanya karena keduanya memiliki fitur tekstur serupa.

Untuk mengatasi keterbatasan struktural ini, informasi posisi harus disuntikkan ke setiap token. Solusi yang lazim digunakan dalam literatur computer vision modern adalah positional encoding. Pendekatan ini dapat berbentuk fungsi matematis deterministik seperti sinusoidal, atau lebih dominan dalam praktik terkini berupa learnable positional embedding yang dipelajari bersama parameter model selama fase training. Pada implementasi Vision Transformer standar, vektor posisi ini ditambahkan secara element-wise ke vektor patch embedding sebelum memasuki blok transformer pertama, sehingga setiap token membawa sinyal lokasi absolut sekaligus konteks konten.

Implikasi dari desain ini bersifat kritis terhadap generalisasi dan efisiensi komputasi. Model harus secara eksplisit mempelajari diskriminasi spasial melalui kombinasi antara konten patch dan sinyalnya. Dalam skenario praktis, perubahan resolusi input akan mengubah jumlah total token, sehingga dimensi positional embedding juga berubah. Strategi standar yang diterapkan adalah interpolasi bilinear atau metode resampling terhadap vektor posisi yang telah dikonvergensi, memungkinkan adaptasi lintas resolusi tanpa memerlukan pelatihan ulang dari awal. Hal ini menunjukkan mengapa pemilihan jenis positional encoding sering kali menjadi hyperparameter sensitif dalam eksperimen tingkat lanjut.

Dengan representasi token yang kini terenkripsi secara spasial, arsitektur siap memasuki tahap pemrosesan relasional yang sebenarnya. Blok selanjutnya, yaitu Transformer Encoder Block, akan memanfaatkan token-token ini untuk melakukan interaksi global antar seluruh wilayah citra. Berbeda dengan filter konvolusi CNN yang terbatas pada receptive field lokal, mekanisme attention pada blok ini memungkinkan propagasi informasi jarak jauh secara langsung, sambil mempertahankan pola residual connection dan layer normalization yang telah terbukti stabil dalam optimasi jaringan saraf dalam skala besar.

---

## Slide 019 - Transformer Encoder Block

### Narasi

Setelah sebelumnya kita menambahkan positional encoding ke setiap patch image untuk mengatasi ketiadaan konsep urutan bawaan pada self-attention, langkah selanjutnya adalah memproses representasi tersebut melalui unit komputasi inti Transformer, yaitu Transformer Encoder Block. Blok ini merupakan komponen yang diulang secara bertingkat untuk membangun representasi semantik yang semakin abstrak.

Struktur satu blok encoder mengikuti alur komputasi yang terstruktur dan simetris. Input yang telah dilengkapi positional embedding pertama-tama dilewatkan melalui Layer Normalization untuk menstabilkan distribusi statistik fitur. Sinyal yang sudah dinormalisasi kemudian masuk ke modul Multi-Head Self-Attention yang menghitung bobot ketergantungan antar semua token secara paralel. Output attention ditambahkan kembali ke input awal melalui residual connection, dilanjutkan dengan Layer Normalization kedua. Tahap berikutnya adalah MLP atau Feed-Forward Network yang terdiri dari dua lapisan linear dengan fungsi aktivasi GELU di antaranya. Sama seperti pada tahap attention, output MLP juga digabungkan dengan jalur residual sebelum akhirnya keluar sebagai output blok ini.

Secara arsitektural, terdapat kesamaan prinsip dengan CNN modern yang menjadikan keduanya stabil untuk pelatihan skala besar. Kedua pendekatan sama-sama mengandalkan residual connection untuk mitigasi vanishing gradient, serta menggunakan normalisasi layer untuk mempercepat konvergensi dan meningkatkan robustness numerik. Namun, perbedaan mendasar terletak pada cara mereka memodelkan hubungan spasial. Self-attention bersifat global, sehingga setiap token dapat mengakses informasi dari seluruh wilayah citra tanpa batasan radius kernel. Di sisi lain, MLP dalam ViT beroperasi secara independen per token, berbeda dengan konvolusi CNN yang menyebarkan informasi secara lokal melalui sliding window dan weight sharing.

Karakteristik struktural ini bukan sekadar perbedaan implementasi, melainkan cerminan dari asumsi fundamental tentang bagaimana struktur visual dipelajari. Mekanisme global versus lokal, serta pemrosesan per token versus spasial, akan secara langsung menentukan bias induktif masing-masing arsitektur. Implikasi dari perbedaan ini—mulai dari kebutuhan volume data, strategi augmentasi, hingga pola pretraining—akan kita bandingkan secara eksplisit pada slide berikutnya melalui tabel komparatif antara CNN dan Vision Transformer.

---

## Slide 020 - Inductive Bias Berbeda: CNN vs ViT

### Narasi

Setelah membahas struktur blok encoder transformer pada slide sebelumnya, kini kita perlu menyoroti perbedaan fundamental yang mendasari kedua arsitektur ini, yaitu *inductive bias*. Dalam konteks pembelajaran mendalam, *inductive bias* merujuk pada serangkaian asumsi struktural atau preferensi pemrosesan yang melekat pada model sebelum proses pelatihan dimulai. Perbedaan bias inilah yang secara langsung menentukan bagaimana CNN dan Vision Transformer mengorganisir informasi visual, memanipulasi parameter, dan menggeneralisasi pola dari data empiris.

Mari kita bedah lima aspek kunci perbandingan antara CNN dan Vision Transformer berdasarkan tabel yang ditampilkan:
- **Bias lokal**: CNN memiliki bias lokal yang sangat kuat karena operasi konvolusi secara inheren membatasi interaksi pada wilayah spasial tetangga. Vision Transformer memiliki bias lokal yang lemah karena mekanisme *self-attention* bersifat global sejak lapisan pertama.
- **Weight sharing**: CNN memanfaatkan pembagian bobot yang identik pada seluruh area citra melalui kernel yang digeser, sehingga menekan jumlah parameter. Vision Transformer tidak diwajibkan melakukan *weight sharing* antar token, memberikan kapasitas ekspresif lebih besar namun meningkatkan beban komputasi.
- **Hubungan antar piksel**: Pada CNN, ketergantungan spasial dibangun secara bertahap melalui pertumbuhan *receptive field*. Pada ViT, semua patch dapat saling berkomunikasi langsung dalam satu langkah komputasi tanpa perlu akumulasi berlapis.
- **Kebutuhan data**: Karena struktur hierarkis dan ketergantungan spasial sudah tertanam pada arsitektur CNN, model ini cenderung lebih stabil dan membutuhkan dataset yang relatif lebih kecil untuk konvergensi optimal. ViT harus mempelajari struktur citra sepenuhnya dari data, sehingga umumnya menuntut volume data yang jauh lebih masif.
- **Fleksibilitas arsitektur**: CNN terikat oleh asumsi grid 2D yang kaku, menyulitkan adaptasi ke representasi non-Euclidean atau multimodal tanpa modifikasi substansial. ViT menawarkan fleksibilitas lebih tinggi karena berbasis urutan token yang dapat dipetakan ke berbagai modalitas.

Implikasi praktis dari perbedaan *inductive bias* ini sangat relevan dengan strategi eksperimen tingkat riset. Karena CNN sudah membawa prior struktural sejak desainnya, peneliti sering kali dapat fokus pada optimasi head deteksi atau segmentasi dengan dataset yang lebih terkendala. Sebaliknya, ViT menuntut perhatian ekstra pada fase *pretraining* dan teknik augmentasi data yang agresif untuk menggantikan prior spasial yang tidak dimiliki model. Tanpa strategi regularisasi dan kurasi data yang matang, ViT rentan terhadap overfitting atau kegagalan menangkap dependensi jarak jauh yang halus.

Pemahaman mengenai trade-off *inductive bias* ini menjadi landasan kritis sebelum kita mengimplementasikan kedua arsitektur secara konkret. Pada slide berikutnya, kita akan membedah alur pemrosesan data secara skematis untuk memperjelas bagaimana CNN mengekstrak pola lokal secara bertahap, sementara ViT langsung mengakses informasi global, serta implikasi efisiensi komputasi dan kebutuhan data yang muncul dari kedua paradigma tersebut.

---

## Slide 021 - Perbandingan Skematis CNN dan ViT

### Narasi

Pada slide ini, kita akan membedah secara skematis bagaimana arsitektur CNN dan Vision Transformer memproses informasi visual, yang merupakan konsekuensi langsung dari perbedaan *inductive bias* yang telah kita diskusikan pada slide sebelumnya. Perhatikan alur pemrosesan pada CNN: serangkaian operasi konvolusi ukuran 3x3 diikuti oleh pooling, kemudian berlanjut ke lapisan fully connected. Struktur ini memastikan bahwa ekstraksi fitur bersifat lokal sejak awal, di mana *receptive field* akan membesar secara bertahap seiring kedalaman jaringan. Artinya, CNN membangun representasi hierarkis secara eksplisit, mulai dari tepi dan tekstur sederhana menuju pola yang lebih kompleks dan semantik.

Sebaliknya, arsitektur Vision Transformer mengikuti alur yang sangat berbeda. Setelah tahap *patch embedding*, input langsung dilewatkan melalui blok *self-attention* dan *MLP*. Tidak ada mekanisme pooling atau konvolusi yang membatasi cakupan informasi. Sejak blok pertama, setiap *token* gambar saling terhubung secara penuh (*all-to-all*). Hal ini memungkinkan ViT mengakses konteks global secara instan, tanpa perlu menunggu propagasi sinyal melalui banyak lapisan berurutan.

Perbedaan skematis ini menghasilkan implikasi representasional dan komputasional yang perlu dikaji kritis:
- CNN mengandalkan pola lokal yang dikenali terlebih dahulu, sehingga sangat efisien untuk tugas dengan struktur spasial yang kuat dan membutuhkan data relatif lebih sedikit.
- ViT memberikan akses langsung ke informasi global pada lapisan pertama, yang memberikan fleksibilitas tinggi dalam menangkap dependensi jarak jauh.
- *Trade-off* utama muncul pada efisiensi komputasi akibat kompleksitas kuadratik dari *self-attention*, serta ketergantungan pada volume data yang masif untuk mempelajari struktur spasial yang sebelumnya sudah di-*encode* secara eksplisit pada CNN.

Pemahaman skematis ini menjadi fondasi penting sebelum kita menelaah aspek empirisnya. Seperti yang akan kita bahas pada slide berikutnya, kelemahan *inductive bias* pada ViT dapat diatasi melalui strategi training khusus, regularisasi, dan augmentasi data, sebagaimana dibuktikan oleh penelitian DeiT. Oleh karena itu, dalam konteks penelitian tingkat doktoral, evaluasi arsitektur tidak boleh berhenti pada diagram strukturnya saja, melainkan harus memperhitungkan interaksi antara desain model, protokol pelatihan, dan skala data untuk mengidentifikasi celah penelitian yang valid dan berkontribusi terhadap state-of-the-art.

---

## Slide 022 - ViT dan Kebutuhan Data

### Narasi

Pada slide sebelumnya, kita telah membandingkan skema arsitektur CNN dan Vision Transformer secara visual. Perbedaan mendasar terletak pada cara mereka memproses informasi spasial: CNN mengandalkan pola lokal yang berkembang bertahap melalui mekanisme *receptive field*, sedangkan ViT langsung mengakses hubungan global antar *patch* sejak blok pertama. Namun, keunggulan teoretis ini membawa konsekuensi praktis yang sangat signifikan, yaitu ketergantungan ekstrem terhadap volume dan kualitas data pelatihan.

Temuan kunci dari paper awal ViT menunjukkan bahwa arsitektur ini hanya mampu mengungguli CNN ketika dilatih pada dataset berskala masif seperti JFT-300M. Ketika dievaluasi hanya pada ImageNet-1k tanpa pretraining eksternal, performa ViT justru tertinggal jauh dibandingkan ResNet. Fenomena ini bukan kebetulan, melainkan cerminan dari lemahnya *inductive bias* bawaan ViT. Berbeda dengan CNN yang sudah memiliki prior kuat untuk mengenali struktur hierarkis dan lokalisasi citra, ViT bersifat lebih *agnostic* dan memerlukan data dalam jumlah besar untuk mempelajari representasi yang setara.

Untuk mengatasi tantangan ini, penelitian lanjutan seperti DeiT (Data-efficient image Transformers) oleh Touvron dkk. (2021) menawarkan solusi strategis. Mereka membuktikan bahwa kombinasi strategi berikut dapat membuat ViT menjadi kompetitif bahkan pada dataset seukuran ImageNet-1k:
- *Data augmentation* yang agresif dan konsisten.
- Regularisasi model yang ketat untuk mencegah *overfitting*.
- Teknik *knowledge distillation* dari model CNN yang sudah matang.

Hasil ini menegaskan bahwa strategi pelatihan (*training strategy*) dapat secara efektif menggantikan sebagian kekurangan *inductive bias* arsitektural. Paradigma ini mengubah cara kita menilai model: keunggulan tidak lagi semata bergantung pada desain jaringan, tetapi juga pada protokol eksperimental yang ketat dan terukur.

Pesan kritis untuk kegiatan penelitian tingkat doktoral adalah jangan pernah menyimpulkan keunggulan suatu arsitektur tanpa mengontrol variabel skala data dan metodologi pelatihan. Perbandingan model harus dilakukan di bawah kondisi eksperimen yang adil, termasuk penggunaan teknik augmentasi, regularisasi, dan protokol pretraining yang setara. Kesalahan umum dalam literatur sering kali muncul karena mengabaikan faktor-faktor kontekstual ini, sehingga menghasilkan klaim novelty yang tidak robust dan sulit direplikasi.

Implikasi dari diskusi ini akan berlanjut pada slide berikutnya, di mana kita akan membahas bagaimana *transfer learning* dan *pretraining* pada dataset besar menjadi jembatan praktis untuk mengatasi keterbatasan data. Kita akan menelusuri alur *fine-tuning*, manfaat ekstraksi fitur dasar, serta ekosistem ketersediaan *pretrained weights* melalui library seperti `torchvision`, `timm`, dan Hugging Face, yang menjadi fondasi penting dalam merancang eksperimen computer vision modern.

---

## Slide 023 - Transfer Learning dan Pretraining

### Narasi

Setelah membahas bagaimana Vision Transformer sangat bergantung pada skala data dan strategi pelatihan seperti yang ditunjukkan oleh DeiT pada slide sebelumnya, kita beralih ke mekanisme praktis untuk mengatasi keterbatasan data tersebut, yaitu transfer learning dan pemanfaatan model yang telah dilatih sebelumnya. Pada jenjang doktoral, penguasaan atas alur ini bukan sekadar keterampilan implementasi, melainkan fondasi metodologis krusial dalam merancang eksperimen yang valid dan reproducible.

Alur transfer learning dapat dipetakan secara sistematis melalui tiga tahapan berturut-turut:
- Tahap pertama adalah pretraining pada dataset berskala besar, di mana model mempelajari representasi visual hierarkis mulai dari tepi, tekstur, pola geometris, hingga konsep semantik tingkat tinggi.
- Tahap kedua adalah fine-tuning pada dataset target, di mana bobot model yang sudah terbentuk disesuaikan dengan distribusi data dan tugas spesifik yang lebih sempit.
- Tahap ketiga adalah evaluasi pada test set target untuk mengukur kinerja akhir model dalam konteks aplikasi atau benchmark yang ditetapkan.

Manfaat strategis dari pendekatan ini terletak pada efisiensi pembelajaran dan mitigasi overfitting. Model tidak lagi mempelajari fitur dasar dari nol, melainkan mengadopsi representasi yang sudah matang dan generalisasi baik. Hal ini menjadi sangat efektif ketika peneliti berhadapan dengan dataset berukuran kecil atau domain khusus yang memiliki keterbatasan annotasi label. Dalam konteks riset S3, strategi ini memungkinkan eksplorasi arsitektur alternatif, modifikasi head deteksi/sementasi, atau integrasi modul attention tanpa memerlukan infrastruktur komputasi raksasa sejak awal.

Untuk mendukung ekosistem riset ini, ketersediaan weight yang telah dilatih tersebar rapi di berbagai library standar industri dan akademik. Di torchvision, kita dapat mengakses arsitektur klasik dan efisien seperti ResNet, MobileNet, hingga EfficientNet. Library timm menawarkan variasi yang jauh lebih luas, mencakup ratusan arsitektur modern termasuk ViT, Swin Transformer, dan ConvNeXt. Sementara itu, Hugging Face Hub menjadi pusat distribusi terpusat untuk model-model berbasis transformer seperti ViT, DeiT, BEiT, serta berbagai varian self-supervised learning lainnya. Integrasi seamless library-library ini ke dalam workflow Jupyter Notebook atau Google Colab secara signifikan mempercepat iterasi eksperimen dan validasi hipotesis.

Namun, kemudahan akses terhadap weight pretrained ini membawa implikasi metodologis yang harus dikritisi secara ketat. Sebelum kita melanjutkan ke pembahasan berikutnya, perhatikan bahwa keunggulan akurasi yang sering dilaporkan dalam literatur belum tentu murni berasal dari inovasi arsitektur. Apakah peningkatan performa tersebut benar-benar disebabkan oleh desain arsitektur yang baru, ataukah semata-mata karena skema pretraining yang masif dan strategi regularisasi? Pertanyaan kritis ini akan menjadi fokus diskusi kita di slide berikutnya, di mana kita akan mendalami pentingnya kontrol eksperimen, pembandingan setup yang setara, serta pelaporan hasil yang transparan antara pengaruh arsitektur versus pengaruh skala pretraining.

---

## Slide 024 - Peran Pretraining vs Arsitektur

### Narasi

Pada pembahasan slide sebelumnya, kita telah menguraikan alur transfer learning, manfaat penggunaan bobot pretrained, serta ekosistem penyedia arsitektur siap pakai seperti torchvision, timm, dan Hugging Face. Namun, keberhasilan empiris tersebut menuntut kita untuk mengajukan pertanyaan kritis yang sering kali terabaikan dalam evaluasi model: apakah peningkatan akurasi yang kita catat sebenarnya berasal dari keunggulan desain arsitektur, atau justru didorong oleh skala dan kualitas proses pretraining? Jawaban atas pertanyaan ini tidak dapat bersifat asumsi, melainkan harus dibuktikan melalui eksperimen yang dikontrol secara ketat.

Dalam literatur dan praktik penelitian terkini, pola kinerja model menunjukkan variasi yang jelas tergantung pada kombinasi arsitektur dan volume data pretraining. Vision Transformer yang dilatih dengan pretraining pada dataset berskala besar cenderung mendominasi benchmark karena kemampuan representasinya yang kuat. Sebaliknya, ketika arsitektur yang sama dijalankan dengan pelatihan dari awal pada dataset kecil, performanya mengalami penurunan signifikan. Pola serupa juga terlihat pada arsitektur CNN konvensional; jika diberi akses ke pretraining pada data besar, CNN mampu bersaing secara kompetitif, membuktikan bahwa faktor skala data pretraining memiliki pengaruh yang setara bahkan lebih dominan daripada jenis arsitektur itu sendiri.

Untuk memastikan rigor metodologis dalam penelitian tingkat doktor, kita harus menerapkan protokol kontrol eksperimental yang sistematis:
- Bandingkan model-model hanya ketika kondisi pretraining-nya disetarakan.
- Jalankan dua skenario pelatihan secara paralel: pelatihan dari nol dan fine-tuning terhadap bobot pretrained.
- Laporkan kedua hasil eksperimen tersebut secara terpisah dalam publikasi atau laporan penelitian.

Pemisahan pelaporan ini memungkinkan komunitas akademik menilai kontribusi marginal masing-masing komponen secara transparan, sekaligus menghindari bias konfirmasi dalam klaim novelty. Dengan memahami bahwa skala pretraining dan arsitektur saling berinteraksi secara kompleks, langkah logis berikutnya adalah mengevaluasi beban komputasi yang dihasilkan oleh masing-masing pendekatan tersebut. Pada slide berikutnya, kita akan beralih ke analisis efisiensi parameter dan FLOPs, termasuk bagaimana mengukur kompleksitas model secara kuantitatif menggunakan kode Python sederhana, serta mengapa metrik jumlah parameter saja tidak cukup untuk menggambarkan kebutuhan memori GPU atau latensi inferensi dalam skenario deployment nyata.

---

## Slide 025 - Efisiensi Parameter dan FLOPs

### Narasi

Setelah mengevaluasi peran relatif antara skala pretraining dan struktur arsitektur pada slide sebelumnya, kita kini masuk ke dimensi kuantitatif yang menentukan kelayakan implementasi: efisiensi parameter dan beban komputasi (FLOPs). Pada level penelitian doktoral, angka-angka ini tidak boleh dipandang sebagai indikator tunggal performa, melainkan sebagai variabel kontrol dalam desain eksperimen.

Berikut adalah perbandingan ukuran model yang umum digunakan sebagai baseline:
- ResNet-50 mewakili arsitektur CNN klasik dengan sekitar 25,6 juta parameter.
- ViT-Base membutuhkan sekitar 86 juta parameter.
- ViT-Large melonjak signifikan ke 307 juta parameter.
- Swin-Base berada di kisaran 88 juta parameter, namun memanfaatkan struktur hierarkis yang mengurangi kompleksitas pemrosesan visual dibandingkan ViT standar.

Perlu ditekankan bahwa jumlah parameter dan FLOPs teoritis tidak selalu berkorelasi linear dengan kebutuhan memori aktual maupun latensi inferensi. FLOPs pada Transformer sangat bergantung pada resolusi input karena mekanisme attention menghitung interaksi antar token secara kuadratik terhadap panjang sekuens. Sebaliknya, CNN memiliki skabilitas linear terhadap resolusi akibat sifat lokal filter konvolusional. Oleh karena itu, pengukuran empiris seperti throughput batch, utilisasi GPU memory, dan waktu inferensi harus dicatat secara langsung selama validasi model.

Untuk mencatat kompleksitas model secara otomatis selama pelatihan atau ablasi, gunakan fungsi berikut:

```python
def count_parameters(model):
    return sum(p.numel() for p in model.parameters() if p.requires_grad)
```

Kode ini mengiterasi semua tensor parameter dalam objek model, menyaring hanya yang memerlukan gradien (`requires_grad=True`), lalu menjumlahkan banyaknya elemen (`numel()`). Implementasi ini sangat berguna untuk melacak perubahan bobot saat Anda melakukan modifikasi lapisan, mengubah width/depth factor, atau mengganti backbone tanpa harus menghitung manual. Angka hasil fungsi ini sebaiknya dilog bersama metrik loss dan akurasi agar reproducible.

Memahami profil komputasi ini menjadi fondasi penting sebelum kita menyoroti aspek perilaku model. Di slide berikutnya, kita akan membahas bagaimana trade-off antara generalisasi dan robustness sering kali berbanding terbalik dengan efisiensi arsitektur, serta mengapa distribusi shift harus diuji secara spesifik sesuai konteks domain target.

---

## Slide 026 - Trade-off Generalisasi dan Robustness

### Narasi

Setelah membahas efisiensi parameter dan FLOPs pada slide sebelumnya, kita kini beralih ke aspek evaluasi yang tak kalah fundamental, yaitu trade-off antara generalisasi dan robustness. Kapasitas model yang diukur dari jumlah parameter atau biaya komputasi saja tidak serta-merta menjamin kinerja di lingkungan produksi, karena adaptabilitas model terhadap variasi input sering kali memiliki hubungan non-linear dengan kompleksitas arsitekturnya.

Temuan empiris dari literatur terkini menunjukkan perbedaan mendasar dalam cara arsitektur memproses informasi visual. CNN secara inheren cenderung mengandalkan tekstur lokal dan pola frekuensi tinggi akibat struktur *receptive field* yang bersifat hierarkis dan terbatas. Sebaliknya, Vision Transformer, khususnya yang dilatih dalam skala besar, memanfaatkan mekanisme *self-attention* untuk membangun konteks global, sehingga lebih efektif menangkap struktur bentuk (*shape bias*) objek secara holistik. Dalam sejumlah benchmark, ViT memang dilaporkan menunjukkan ketahanan yang lebih baik terhadap *occlusion* parsial, namun konsistensi hasil tersebut sangat sensitif terhadap protokol augmentasi data dan skema pretraining yang diterapkan.

Implikasi kritis untuk riset tingkat doktoral adalah bahwa tidak ada arsitektur yang unggul secara universal. Keputusan pemilihan model harus diposisikan sebagai respons langsung terhadap jenis *distribution shift* atau degradasi data yang spesifik pada domain target Anda. Evaluasi rigor memerlukan pengujian eksplisit pada skenario perubahan distribusi yang relevan, bukan hanya mengandalkan metrik akurasi standar pada set validasi statis. Pembahasan ini menjadi jembatan logis menuju slide berikutnya, yang akan mengurai kondisi-kondisi strategis di mana arsitektur CNN justru menawarkan efisiensi dan efektivitas yang lebih unggul dibandingkan pendekatan berbasis transformer.

---

## Slide 027 - Kapan CNN Lebih Efektif?

### Narasi

Pada slide sebelumnya, kita telah menyoroti bahwa tidak ada arsitektur yang secara mutlak menang dalam segala kondisi. CNN cenderung mengandalkan pola tekstur lokal, sementara Vision Transformer mampu menangkap dependensi jangka panjang dan struktur bentuk global. Hasil evaluasi ini sangat bergantung pada strategi augmentasi, kualitas pretraining, serta jenis distribution shift yang dihadapi model. Implikasi utamanya bagi peneliti adalah bahwa pilihan arsitektur harus selaras dengan karakteristik perubahan data pada domain target, bukan mengikuti tren semata.

Dalam beberapa skenario spesifik, CNN tetap menunjukkan efektivitas yang lebih tinggi. Kondisi pertama adalah ketika dataset pelatihan berukuran kecil dengan label terbatas. Induksi bias spasial bawaan pada lapisan konvolusi memungkinkan model belajar representasi bermakna dengan sample size yang jauh lebih kecil dibandingkan arsitektur berbasis attention murni. Kondisi kedua muncul ketika sumber daya komputasi terbatas atau target deployment berada di perangkat edge seperti sistem embedded, drone, atau perangkat IoT. Komputasi konvolusi bersifat lokal, dapat di-quantize dengan stabil, dan memiliki footprint memori yang ringan, sehingga ideal untuk inferensi real-time dengan latency rendah.

Kondisi ketiga relevan untuk tugas yang secara inheren mengandalkan pola lokal. Contoh konkretnya meliputi segmentasi objek kecil dengan tekstur dominan, inspeksi defect industri, atau analisis histopatologi di mana fitur diagnostik terkonsentrasi pada wilayah mikroskopis. Selain itu, kebutuhan akan interpretasi spasial berbasis fitur juga mendukung penggunaan CNN. Mekanisme filter dan pooling menghasilkan aktivasi neuron yang lebih mudah divisualisasikan, memberikan jejak komputasi yang transparan untuk studi explainable AI atau validasi klinis.

Poin kritis yang perlu ditekankan adalah efektivitas CNN dalam kondisi-kondisi tersebut tidak boleh diasumsikan tanpa verifikasi empiris. Sebagai peneliti tingkat doktoral, Anda wajib menguji baseline CNN secara ketat terhadap arsitektur modern lainnya, bahkan pada dataset yang tampak sederhana. Variabel seperti augmentasi data, teknik regularisasi, warmup scheduler, dan strategi optimasi dapat mengubah hasil secara signifikan. Desain eksperimen yang terkontrol dan metrik evaluasi yang relevan diperlukan sebelum menarik kesimpulan tentang keunggulan relatif suatu arsitektur.

Transisi ke slide berikutnya akan mengarahkan kita pada kondisi sebaliknya: kapan Vision Transformer justru menunjukkan dominasi yang jelas. Pemahaman dualitas ini akan menjadi fondasi dalam merancang experimental design yang presisi, memilih arsitektur dasar yang sesuai dengan constraint riset, dan memposisikan novelty kontribusi Anda terhadap state-of-the-art yang terus berevolusi.

---

## Slide 028 - Kapan Transformer Lebih Efektif?

### Narasi

Beralih dari efektivitas CNN, slide ini menyoroti kondisi spesifik di mana Vision Transformer (ViT) memberikan keunggulan arsitektural yang signifikan. Keunggulan ini tidak bersifat mutlak, melainkan bergantung pada keselarasan antara karakteristik data, kompleksitas tugas, dan ketersediaan sumber daya komputasi.

Kondisi yang paling mendukung penerapan ViT meliputi:
- Ketersediaan dataset berskala besar atau akses ke model pretrained yang sudah terkalibrasi secara luas.
- Tugas yang menuntut pemodelan dependensi jangka panjang antar wilayah citra, melampaui batas receptive field lokal.
- Input yang berupa patch atau token dari sumber heterogen, memungkinkan fleksibilitas representasi non-Euclidean.
- Budget komputasi yang memadai untuk menangani overhead kalkulasi matriks attention.
- Strategi transfer learning yang ditujukan untuk multiple downstream task secara simultan.

Mekanisme self-attention memungkinkan ViT menangkap konteks global secara native, sehingga sangat efektif untuk kasus seperti deteksi objek kecil yang memerlukan disambiguasi berbasis lingkungan, segmentasi semantik pada citra satelit atau medis dengan struktur kompleks, serta pembangunan joint embedding untuk vision-language model. Investasi komputasi yang diperlukan sebanding dengan skalabilitas dan kemampuan generalisasi lintas domain yang ditawarkan.

Penting untuk menekankan bahwa keputusan arsitektural dalam penelitian tingkat doktor harus didasarkan pada analisis trade-off yang ketat. Sebagaimana ditunjukkan pada pembahasan sebelumnya, CNN tetap menjadi pilihan rasional ketika constraint komputasi ketat atau pola lokal mendominasi. Sebaliknya, ViT unggul ketika fokus penelitian bergeser ke fondasi model, multimodal alignment, atau ekstraksi fitur hierarkis yang invariant terhadap transformasi spasial.

Klaim teoretis ini harus divalidasi secara empiris. Pada slide berikutnya, kita akan menguji dinamika ini melalui studi kasus dataset kecil tanpa pretraining. Simulasi tersebut akan membuktikan bahwa pada skala data terbatas, kualitas representasi awal sering kali lebih determinan daripada pilihan arsitektur murni. Validasi eksperimental ini menjadi prasyarat kritis sebelum merumuskan metodologi penelitian atau menyusun proposal disertasi.

---

## Slide 029 - Studi Kasus Dataset Kecil

### Narasi

Setelah mengidentifikasi kondisi di mana arsitektur Transformer menunjukkan keunggulan komputasional dan kontekstual pada slide sebelumnya, kita kini beralih ke skenario yang lebih menantang dalam praktik penelitian: penerapan model pada dataset terbatas. Dalam riset tingkat doktoral, asumsi ketersediaan data masif jarang menjadi kenyataan mutlak. Oleh karena itu, memahami dinamika pembelajaran ketika sampel sangat terbatas menjadi kunci dalam merancang metodologi yang valid.

Mari kita tinjau skenario eksperimen dengan dataset berukuran 1.000 citra yang terbagi ke dalam 10 kelas, tanpa memanfaatkan inisialisasi dari model pra-latih. Berdasarkan literatur terkini dan pengamatan empiris, berikut adalah proyeksi perilaku arsitektur dalam kondisi tersebut:
- **CNN kecil dilatih dari awal**: Cenderung mengalami *overfitting* akibat rasio parameter terhadap sampel yang tidak seimbang. Dengan regularisasi ketat, model masih dapat membentuk batas keputusan yang fungsional, namun generalisasinya lemah.
- **CNN pra-latih yang dilakukan *fine-tuning***: Menjadi baseline yang sangat robust. Representasi hierarkis yang telah dipelajari dari dataset skala besar memberikan fitur awal yang stabil, sehingga konvergensi lebih cepat dan akurasi lebih tinggi.
- **ViT dilatih dari awal**: Sangat rentan terhadap *overfitting*. Mekanisme *self-attention* yang murni membutuhkan volume token dan variasi data yang jauh lebih besar untuk mempelajari dependensi spasial yang bermakna.
- **ViT pra-latih yang dilakukan *fine-tuning***: Menunjukkan kinerja yang kompetitif, bahkan sering menyamai atau melampaui CNN pra-latih. Hal ini menegaskan bahwa kualitas inisialisasi bobot lebih dominan daripada arsitektur dasar ketika data terbatas.

Kesimpulan kritis dari studi kasus ini adalah bahwa pada dataset kecil, keberadaan mekanisme *pretraining* umumnya lebih menentukan daripada pilihan arsitektur CNN versus Vision Transformer. Transfer pengetahuan dari domain sumber yang luas mampu menutupi celah statistik yang sempit. Klaim ini wajib diverifikasi secara empiris melalui desain eksperimen yang terkontrol, karena karakteristik distribusi data spesifik dapat mengubah pola konvergensi dan kebutuhan regulasi.

Poin ini menjadi landasan langsung untuk langkah implementatif pada slide berikutnya. Kita akan menerjemahkan konsep *fine-tuning* tersebut ke dalam kode menggunakan ekosistem PyTorch dan `torchvision`, termasuk modifikasi lapisan klasifikasi, pemilihan bobot pra-latih, serta penyesuaian transformasi input agar konsisten dengan asumsi distribusi model yang digunakan.

---

## Slide 030 - Praktikum: Fine-tuning dengan torchvision

### Narasi

Pada slide sebelumnya, kita telah menyimpulkan bahwa pada dataset berukuran kecil, keberadaan pretraining sering kali lebih menentukan performa akhir dibandingkan sekadar memilih arsitektur CNN atau Vision Transformer. Temuan ini menegaskan bahwa representasi visual yang telah dipelajari dari skala besar berfungsi sebagai fondasi robust sebelum dilakukan adaptasi ke tugas spesifik. Untuk menguji dan mengimplementasikan prinsip tersebut secara langsung, kita akan beralih ke sesi praktikum menggunakan library `torchvision`.

Langkah awal dalam fine-tuning adalah menyiapkan dataset citra beserta transformasinya. Pastikan pipeline transformasi Anda konsisten dengan statistik mean dan standar deviasi yang digunakan selama proses pretraining model. Setelah data siap, langkah selanjutnya adalah memuat model pretrained. Alih-alih melatih jaringan dari nol, kita mengambil bobot yang telah belajar fitur tingkat rendah hingga tinggi dari dataset referensi seperti ImageNet. Tahap krusial berikutnya adalah mengganti lapisan klasifikasi akhir sesuai dengan jumlah kelas pada dataset target, kemudian memutuskan apakah akan melatih hanya head baru tersebut atau melakukan fine-tuning pada seluruh lapisan jaringan.

Perhatikan potongan kode berikut untuk implementasinya:
```python
import torchvision.models as models
import torch.nn as nn

model = models.resnet50(
    weights=models.ResNet50_Weights.IMAGENET1K_V2
)
model.fc = nn.Linear(model.fc.in_features, num_classes=10)
```
Baris pertama mengimpor modul yang diperlukan. Pada baris ketiga, fungsi `models.resnet50()` dipanggil dengan parameter `weights` yang mengarahkan PyTorch untuk mengunduh dan memuat bobot resmi dari ImageNet versi kedua. Secara otomatis, lapisan konvolusi dan bottleneck akan terkunci dengan bobot pretrained. Baris terakhir menggantikan atribut `fc` (fully connected layer) asli dengan `nn.Linear` baru yang menyesuaikan dimensi output menjadi `num_classes=10`. Struktur ini memungkinkan gradient hanya mengalir kuat ke lapisan baru saat tahap training awal.

Terdapat dua catatan praktis yang wajib diperhatikan. Pertama, transformasi gambar harus diselaraskan dengan normalisasi ImageNet (mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]). Ketidaksesuaian akan menyebabkan domain shift yang signifikan dan menurunkan akurasi. Kedua, mulailah fine-tuning dengan learning rate yang jauh lebih kecil dibandingkan training dari awal, misalnya di kisaran 1e-4 hingga 1e-5, agar bobot pretrained tidak terdistorsi secara drastis pada iterasi awal. Jika tujuan Anda adalah baseline cepat, cukup latih lapisan klasifikasi baru sambil membekukan lapisan konvolusi.

Setelah menguasai alur fine-tuning melalui `torchvision`, pada slide berikutnya kita akan memperluas ekosistem pemodelan menggunakan library `timm`. Library ini menawarkan antarmuka seragam yang memudahkan perbandingan langsung antara arsitektur CNN konvensional dan Vision Transformer, sekaligus menyediakan ribuan bobot pretrained yang telah diverifikasi. Kita akan melihat bagaimana struktur kode dapat dibuat lebih ringkas tanpa mengorbankan fleksibilitas eksperimen.

---

## Slide 031 - Praktikum: Fine-tuning dengan timm

### Narasi

Setelah kita menyelesaikan praktikum fine-tuning menggunakan ekosistem `torchvision` pada slide sebelumnya, kini kita beralih ke library `timm` (TorchVision Models) sebagai alternatif yang sangat dominan dalam riset computer vision terkini. Library ini menawarkan pendekatan yang lebih terstandarisasi untuk mengakses ratusan arsitektur state-of-the-art, mulai dari CNN klasik hingga Vision Transformer terbaru, tanpa perlu menulis ulang struktur inisialisasi model secara manual.

Penggunaan `timm` memberikan beberapa keuntungan strategis bagi peneliti dan praktisi:
- Menyediakan banyak arsitektur dalam satu API yang seragam, sehingga mengurangi kompleksitas boilerplate code dan meminimalkan error implementasi.
- Mempermudah perbandingan langsung antara arsitektur CNN dan ViT karena pola inisialisasi, loading weight, dan preprocessing yang konsisten.
- Pretrained weights tersedia untuk mayoritas model, mempercepat iterasi eksperimen dan memastikan baseline representasi fitur yang kuat dan teruji.

Berikut adalah implementasi praktisnya:
```python
import timm

model_vit = timm.create_model(
    "vit_base_patch16_224",
    pretrained=True,
    num_classes=10
)

model_cnn = timm.create_model(
    "resnet50",
    pretrained=True,
    num_classes=10
)
```
Pada potongan kode di atas, fungsi `create_model` menangani seluruh proses inisialisasi arsitektur secara otomatis. Parameter `"vit_base_patch16_224"` memanggil arsitektur Vision Transformer dengan konfigurasi patch size 16 dan resolusi input standar 224 piksel. Sementara itu, `"resnet50"` mewakili arsitektur CNN konvensional. Flag `pretrained=True` secara otomatis mengunduh dan memuat bobot yang telah dilatih pada dataset skala besar, sedangkan `num_classes=10` langsung menyesuaikan lapisan klasifikasi akhir untuk tugas multi-kelas kita. Pendekatan ini jauh lebih ringkas dibandingkan manipulasi atribut `.fc` secara eksplisit seperti pada contoh `torchvision`.

Terdapat dua catatan teknis kritis yang perlu diperhatikan saat bekerja dengan library ini. Pertama, resolusi input harus selalu disesuaikan dengan spesifikasi resmi model. Mengubah dimensi tensor input secara paksa dapat menggeser distribusi statistik batch normalization dan merusak kemampuan generalisasi fitur yang telah dipelajari selama pretraining. Kedua, `timm` dilengkapi dengan helper functions untuk konfigurasi data dan augmentasi yang telah dioptimalkan khusus untuk masing-masing arsitektur. Utilitas ini sangat berharga ketika kita menyiapkan pipeline data yang konsisten sebelum memasuki fase evaluasi komparatif.

Dengan fondasi kode dan konfigurasi yang telah distandarisasi melalui `timm`, langkah logis berikutnya adalah merancang protokol eksperimen yang ketat. Pada slide berikutnya, kita akan membahas elemen-elemen kunci dalam protokol eksperimen komparatif, termasuk kontrol variabel, konsistensi random seed, dan prinsip reproduktibilitas. Rancangan ini memastikan bahwa setiap selisih performa yang terukur benar-benar mencerminkan keunggulan arsitektural, bukan bias dalam setup training atau variasi acak.

---

## Slide 032 - Protokol Eksperimen Komparatif

### Narasi

Setelah kita berhasil memuat arsitektur CNN dan Vision Transformer secara seragam melalui library `timm` pada praktikum sebelumnya, langkah kritis berikutnya adalah merancang protokol eksperimen yang ketat. Pada jenjang doktoral, perbandingan model tidak boleh dilakukan secara ad-hoc atau berdasarkan satu kali percobaan yang kebetulan menghasilkan angka tinggi. Kita memerlukan kerangka kerja eksperimental yang terkontrol agar setiap selisih performa benar-benar mencerminkan karakteristik struktural model, bukan artefak dari konfigurasi yang berbeda.

Elemen protokol komparatif ini harus diterapkan secara mutlak:
- Dataset tetap dan pembagian split train-val-test yang konsisten.
- Random seed dikunci di awal setiap run untuk menjamin determinisme dalam inisialisasi bobot dan pengacakan data.
- Hiperparameter inti: optimizer, learning rate, batch size, dan jumlah epoch disamakan persis.
- Pipeline augmentasi identik untuk semua model yang diuji.
- Minimal tiga kali run independen untuk mengukur variasi stokastik dan menghitung deviasi standar.

Dokumentasi teknis menjadi bagian tak terpisahkan dari protokol ini. Versi exact dari setiap library Python, framework deep learning, serta spesifikasi perangkat keras harus dicatat secara rinci. Waktu training dan waktu inferensi perlu diukur secara konsisten, begitu pula konfigurasi lengkap setiap model yang diuji. Prinsip intinya adalah reproduktibilitas penuh dan kontrol variabel yang adil. Peneliti lain harus mampu menjalankan ulang protokol ini di lingkungan mereka dan mendapatkan hasil yang setara. Tanpa disiplin ini, klaim keunggulan arsitektur tidak memiliki fondasi metodologis yang kuat.

Dengan protokol yang solid ini, kita siap memasuki tahap pengukuran hasil. Eksperimen yang terkontrol akan menghasilkan data empiris yang siap dievaluasi menggunakan metrik klasifikasi dan efisiensi yang standar, sesuai dengan kerangka laporan komparatif yang akan kita bahas pada slide berikutnya.

---

## Slide 033 - Metrik Evaluasi

### Narasi

Setelah kita menetapkan protokol eksperimen yang ketat pada slide sebelumnya, langkah selanjutnya adalah menentukan bagaimana kita mengukur keberhasilan setiap arsitektur visual. Pada tingkat doktoral, evaluasi tidak boleh hanya bergantung pada satu angka tunggal. Kita memerlukan kerangka metrik yang komprehensif untuk menangkap kinerja model secara multidimensi, baik dari sisi akurasi klasifikasi maupun efisiensi komputasi.

Untuk aspek klasifikasi, *accuracy* memberikan gambaran dasar, namun sering kali menyesatkan pada dataset yang tidak seimbang. Oleh karena itu, kita wajib melaporkan *macro-F1* dan *weighted-F1* untuk menilai performa per kelas secara adil. *AUC-ROC* juga menjadi indikator penting karena mengukur kemampuan diskriminasi model tanpa terikat pada ambang batas keputusan tertentu. Selain angka-angka tersebut, matriks kebingungan (*confusion matrix*) harus selalu dianalisis untuk mengidentifikasi pola kesalahan sistematis, apakah model cenderung membingungkan kelas tertentu atau mengalami bias terhadap kategori minoritas.

Di sisi efisiensi, model dengan akurasi tinggi belum tentu layak diimplementasikan dalam skenario nyata. Kita perlu mencatat jumlah parameter sebagai proksi kompleksitas model, serta *FLOPs* untuk memperkirakan beban komputasi teoritis. Dalam praktiknya, latensi rata-rata per citra dan *throughput* (jumlah citra per detik) menjadi penentu utama untuk aplikasi *real-time*. Jangan lupa mencatat *peak GPU memory*, karena hal ini sering menjadi bottleneck saat melatih atau melakukan inferensi dengan batch size besar pada hardware terbatas.

Contoh tabel laporan pada slide ini menunjukkan sintesis antara kinerja dan efisiensi. Perhatikan perbandingan antara ResNet-50 dan ViT-B. Meskipun ViT-B unggul sedikit dalam akurasi dan F1-score, ia membutuhkan empat kali lipat parameter dan latensi inferensi yang lebih tinggi. Keputusan memilih salah satu model tidak lagi murni berdasarkan akurasi tertinggi, melainkan harus mempertimbangkan *trade-off* antara presisi dan biaya komputasi sesuai konteks aplikasi target penelitian Anda.

Setelah metrik ditetapkan, langkah kritis berikutnya adalah melacak dinamika metrik tersebut selama proses pelatihan dan pengujian. Pada slide berikutnya, kita akan membahas cara menginterpretasi kurva pembelajaran untuk mendeteksi overfitting atau underfitting, serta metodologi pengukuran waktu inferensi yang konsisten. Kombinasi antara pelacakan epoch terbaik dan analisis stabilitas latensi akan menjadi fondasi kuat untuk menarik kesimpulan ilmiah yang valid dalam disertasi Anda.

---

## Slide 034 - Kurva Pembelajaran dan Waktu Inferensi

### Narasi

Setelah sebelumnya membahas berbagai metrik evaluasi seperti akurasi, macro-F1, AUC, hingga ukuran efisiensi seperti FLOPs, latensi, dan penggunaan memori GPU pada tabel perbandingan model, langkah selanjutnya adalah memahami dinamika internal pelatihan dan konsistensi komputasi model. Angka statis saja tidak cukup menggambarkan kualitas arsitektur modern seperti CNN, Attention mechanism, atau Vision Transformer. Kita perlu menelusuri bagaimana model belajar, kapan ia berhenti belajar optimal, dan seberapa cepat ia dapat menghasilkan prediksi dalam skenario nyata.

Mari kita fokus pada kurva pembelajaran terlebih dahulu. Plot training loss dan validation accuracy terhadap jumlah epoch berfungsi sebagai diagnostik utama kesehatan model. Amati perilaku kedua kurva secara bersamaan: jika training loss terus menurun tajam sementara validation accuracy plateau atau justru turun, model mengalami overfitting. Sebaliknya, jika kedua kurva bergerak lambat dan berada pada level error yang masih tinggi, indikasi underfitting sangat kuat. Dalam praktik penelitian tingkat doktoral, jangan pernah otomatis memilih checkpoint epoch terakhir. Identifikasi epoch terbaik berdasarkan validation metric, karena performa puncak sering kali tercapai jauh sebelum akhir proses training.

Di sisi lain, waktu inferensi memerlukan protokol pengukuran yang ketat agar hasil benchmark dapat direproduksi. Ukur latency dengan batch size yang tetap, jalankan inferensi beberapa kali, lalu laporkan rata-rata beserta standar deviasinya. Bedakan secara eksplisit pengukuran pada CPU dan GPU, karena karakteristik parallel processing dan memory bandwidth sangat mempengaruhi distribusi waktu eksekusi. Dokumentasikan juga versi library, driver CUDA, dan konfigurasi hardware, mengingat reproduktibilitas eksperimen menjadi fondasi utama publikasi internasional bereputasi.

Dari perspektif interpretasi, kombinasi antara stabilitas kurva dan efisiensi latensi menjadi penentu kelayakan deploy. Model yang mencapai akurasi setara tetapi memiliki latensi lebih rendah jelas lebih unggul untuk aplikasi real-time atau sistem edge. Selain itu, kurva validasi yang lebih stabil dan fluktuasi loss yang kecil menandakan model yang lebih mudah dikendalikan, kurang sensitif terhadap noise data, dan memiliki konvergensi yang lebih predictable. Hal ini sangat relevan ketika kita membandingkan arsitektur dengan kompleksitas berbeda, misalnya ResNet versus ViT-B, di mana trade-off antara representasi global dan beban komputasi harus dipertimbangkan secara objektif.

Pemahaman ini menjadi landasan kritis sebelum melangkah ke pembahasan berikutnya, di mana kita akan menekankan bahwa kesimpulan ilmiah tidak boleh dibangun atas dasar satu nilai akurasi tunggal. Evaluasi yang rigor memerlukan pengulangan eksperimen, pelaporan confidence interval, dan error analysis mendalam untuk memastikan bahwa perbedaan performa benar-benar mencerminkan keunggulan arsitektur atau strategi pretraining, bukan variasi acak dari seed initialization atau ketidakseimbangan hyperparameter tuning.

---

## Slide 035 - Menghindari Kesimpulan Berdasarkan Akurasi Saja

### Narasi

Setelah menelaah kurva pembelajaran dan karakteristik waktu inferensi pada slide sebelumnya, kita memasuki tahap evaluasi yang lebih kritis. Metrik akurasi tunggal memang mudah dilaporkan, namun dalam konteks penelitian tingkat doktoral, mengandalkan angka tersebut tanpa analisis pendukung berisiko menghasilkan kesimpulan yang lemah atau bahkan menyesatkan.

Berikut adalah masalah metodologis yang perlu diwaspadai saat interpretasi hasil eksperimen:
- **Perbedaan tipis yang tidak signifikan**: Selisih akurasi 0,5% atau 1% seringkali berada dalam noise eksperimen dan tidak memenuhi uji signifikansi statistik.
- **Ketergantungan pada seed acak**: Satu run yang menghasilkan performa puncak bisa jadi hanyalah keberuntungan distribusi random initialization atau data shuffling, bukan indikator robustness model.
- **Ketidakadilan dalam tuning hyperparameter**: Membandingkan model dengan intensitas tuning yang berbeda secara implisit menguntungkan salah satu arsitektur, sehingga perbandingan kehilangan nilai ilmiahnya.

Untuk membangun landasan evaluasi yang rigor, terapkan praktik berikut secara konsisten:
- Laporkan mean dan standar deviasi dari minimal tiga hingga lima run independen dengan seed berbeda. Angka tunggal tanpa dispersi tidak mencerminkan stabilitas model.
- Sertakan confidence interval untuk memberikan batas bawah dan atas performa yang realistis, memudahkan pembaca memahami rentang generalisasi model.
- Lakukan error analysis mendalam. Identifikasi kelas atau kategori visual mana yang paling sering gagal, lalu kaitkan dengan karakteristik dataset atau representasi fitur.
- Dokumentasikan seluruh strategi pemilihan dan pencarian hyperparameter. Reproduksi eksperimen oleh peneliti lain bergantung pada transparansi protokol ini.

Pertajam analisis Anda dengan pertanyaan reflektif berikut. Tanyakan apakah peningkatan performa benar-benar berasal dari inovasi arsitektur inti, atau sekadar keuntungan dari skema pretraining yang lebih matang. Evaluasi pula apakah temuan tersebut berlaku pada domain target penelitian Anda, mengingat transfer knowledge antar domain visual sering kali menunjukkan degradasi performa yang halus namun substantif.

Pemahaman evaluasi berbasis bukti ini akan langsung diterjemahkan ke dalam kerangka critical review paper arsitektur pada slide berikutnya. Di sana, kita akan menggunakan prinsip signifikansi statistik, ablation study, dan validasi domain yang sama untuk menilai kualitas publikasi terkini. Pendekatan inilah yang membedakan tinjauan literatur deskriptif dengan analisis kritis yang mampu mengidentifikasi research gap dan memposisikan kontribusi disertasi Anda secara tepat di tengah state-of-the-art computer vision.

---

## Slide 036 - Kerangka Critical Review Paper Arsitektur

### Narasi

Setelah pada slide sebelumnya menekankan pentingnya menghindari kesimpulan berdasarkan angka akurasi tunggal, kita kini beralih ke tahap evaluasi yang lebih mendalam: menyusun kerangka sistematis untuk melakukan *critical review* terhadap paper arsitektur model visi komputer. Pada jenjang doktoral, penilaian terhadap karya ilmiah tidak lagi berhenti pada laporan metrik, melainkan menuntut analisis struktural terhadap bagaimana arsitektur dirancang, divalidasi, dan diposisikan dalam lanskap state-of-the-art.

Framework yang disajikan berpusat pada lima pertanyaan inti yang harus dijawab secara rigor oleh setiap paper arsitektur yang ingin dijadikan referensi penelitian:
- **Problem**: Apakah paper tersebut benar-benar menjawab celah penelitian yang relevan, atau hanya menawarkan variasi minor tanpa justifikasi teoretis yang kuat?
- **Metode**: Di mana letak kontribusi utamanya? Apakah pada modifikasi arsitektur, strategi pelatihan, teknik augmentasi data, atau integrasi komponen multimodal?
- **Eksperimen**: Apakah baseline yang digunakan kompetitif dan fair? Apakah ablation study dilakukan secara komprehensif untuk mengisolasi dampak setiap modul yang diusulkan?
- **Validitas**: Apakah klaim keunggulan model didukung oleh bukti empiris yang transparan, termasuk analisis kesalahan (*error analysis*) dan pengujian signifikansi statistik?
- **Generalisasi**: Apakah performa konsisten ketika diuji pada domain, resolusi, atau distribusi data yang berbeda, atau hanya overfit terhadap benchmark tertentu?

Kerangka ini merupakan kelanjutan langsung dari matriks literatur yang telah kita bangun pada Pertemuan 2. Dengan menerapkan lensa reviewer ini, Anda dapat menyaring paper arsitektur mana yang memiliki fondasi metodologis kokoh dan layak menjadi pijakan penelitian disertasi, sekaligus mengidentifikasi kelemahan implisit yang bisa dikembangkan sebagai *research gap*. Penekanan pada ablation study dan validasi silang menjadi kunci untuk memastikan bahwa novelty yang Anda usulkan memang memberikan kontribusi substantif, bukan sekadar penyesuaian hiperparameter.

Pemahaman kritis terhadap kekuatan dan keterbatasan masing-masing representasi visual akan langsung menerjemahkan diri ke dalam keputusan desain eksperimen. Pada slide berikutnya, kita akan membahas bagaimana memetakan karakteristik spesifik masalah penelitian ke dalam rekomendasi arsitektur yang paling sesuai, serta melakukan analisis trade-off antara akurasi, efisiensi inferensi, interpretabilitas spasial, dan robustnes domain. Pendekatan ini memastikan bahwa pemilihan model dalam disertasi Anda didasarkan pada pertimbangan ilmiah yang terukur dan reproducible.

---

## Slide 037 - Analisis Trade-off untuk Penelitian Disertasi

### Narasi

Setelah menyelesaikan kerangka critical review arsitektur pada slide sebelumnya, langkah logis berikutnya dalam perjalanan penelitian disertasi adalah menerjemahkan temuan review tersebut menjadi keputusan teknis yang terukur. Slide ini menyajikan analisis trade-off yang menghubungkan karakteristik spesifik masalah penelitian dengan rekomendasi arsitektur yang paling tepat. Pada tingkat doktoral, pemilihan model tidak lagi bersifat empiris semata, melainkan harus didasari oleh evaluasi ketat terhadap constraint data, komputasi, dan tujuan ilmiah yang ingin dicapai.

Tabel keputusan pada slide ini merangkum enam skenario masalah umum beserta arsitektur yang direkomendasikan:
- Untuk dataset kecil dengan label terbatas, CNN pretrained tetap menjadi pilihan yang robust berkat kematangan transfer learning dan regularisasi bawaannya.
- Ketika hubungan spasial global atau konteks jarak jauh menjadi inti permasalahan, Vision Transformer atau varian hierarkis seperti Swin Transformer menawarkan kapasitas modeling kontekstual yang superior.
- Jika aplikasi menuntut inferensi real-time atau deployment di perangkat edge, CNN ringan atau arsitektur mobile-optimized harus diutamakan untuk menjaga latency rendah.
- Ketersediaan corpus pretraining skala besar secara langsung mendukung penggunaan ViT pretrained, mengingat arsitektur ini sangat sensitif terhadap volume data untuk mencapai representasi yang stabil.
- Ketika interpretasi spasial menjadi requirement kritis, CNN yang dipadukan dengan teknik saliency mapping masih memberikan transparansi decision-making yang lebih mudah diaudit.
- Untuk domain multimodal, Transformer atau ViT sering dipilih sebagai encoder visual karena keseragaman representasi vektornya yang kompatibel dengan modality linguistik atau sensorik lainnya.

Proses seleksi ini memerlukan eksekusi sistematis melalui tiga langkah konkret. Pertama, identifikasi secara eksplisit karakteristik masalah penelitian Anda, termasuk batasan ketersediaan data, infrastruktur komputasi, dan kebutuhan interpretabilitas. Kedua, tetapkan prioritas utama secara hierarkis: apakah akurasi, kecepatan inferensi, kemampuan interpretasi, atau robustnes terhadap shift distribusi data. Ketiga, rancang dan jalankan eksperimen pendahuluan yang membandingkan beberapa kandidat model secara paralel. Hasil pilot study ini akan menjadi dasar justifikasi metodologis yang kuat dalam bab metodologi proposal disertasi Anda.

Keputusan arsitektur yang dipetakan hari ini akan langsung berimplikasi pada strategi pembelajaran representasi yang akan kita eksplorasi pada pertemuan berikutnya. Karena banyak arsitektur modern, khususnya ViT, sangat bergantung pada fase pretraining berkualitas tinggi untuk menghasilkan embedding bermakna, pemahaman mendalam tentang self-supervised learning menjadi prasyarat. Slide berikutnya akan mengaitkan topik ini dengan pertemuan 4, di mana kita akan membahas bagaimana DINO dan DINOv2 memanfaatkan arsitektur transformer untuk mengekstrak fitur tanpa label, serta kapan pendekatan self-supervised benar-benar melampaui paradigma supervised tradisional. Diskusi ini akan melengkapi kerangka pemilihan arsitektur dengan strategi ekstraksi representasi yang lebih efisien, scalable, dan relevan untuk penelitian cutting-edge.

---

## Slide 038 - Menghubungkan ke Self-Supervised Learning

### Narasi

Pada slide sebelumnya, kita telah menelaah analisis trade-off dalam pemilihan arsitektur representasi visual untuk konteks penelitian disertasi. Rekomendasi pada tabel tersebut menegaskan bahwa ketika masalah penelitian menuntut pemodelan hubungan global atau beroperasi pada domain multimodal, Vision Transformer (ViT) sering kali menjadi kandidat utama. Namun, keunggulan struktural ViT tidak otomatis menjamin performa optimal; faktor penentu utamanya justru terletak pada kualitas representasi yang berhasil dipelajari selama fase pretraining. Di sinilah konsep self-supervised learning (SSL) memasuki diskusi sebagai solusi strategis.

Self-supervised learning memungkinkan model mengekstrak representasi visual yang bermakna tanpa bergantung pada anotasi label manual yang biasanya mahal, lambat, dan terbatas skalanya. Dalam ekosistem computer vision mutakhir, arsitektur ViT telah menjadi backbone dominan untuk metode SSL terkini, termasuk DINO dan DINOv2. Hal ini menunjukkan pergeseran paradigma di mana keberhasilan model tidak lagi hanya diukur dari kedalaman atau lebar jaringan, melainkan dari efektivitas protokol pretraining dalam membentuk ruang fitur yang robust dan generalizable.

Dari perspektif penelitian tingkat doktor, implementasi SSL pada ViT membuka sejumlah pertanyaan kritis yang perlu dijawab melalui desain eksperimen yang rigor:
- Representasi apa yang sebenarnya dipelajari oleh ViT dalam skema self-supervised?
- Apakah fitur yang dihasilkan bersifat semantik tinggi, atau sekadar menangkap pola tekstural dan statistik permukaan citra?
- Kapan pendekatan SSL secara signifikan melampaui pretraining berbasis supervised, baik dari segi sample efficiency maupun transferability?

Sebagai tindak lanjut, pertemuan berikutnya akan membedah tiga mekanisme inti dalam SSL modern: contrastive learning, masked image modeling, dan linear probing untuk evaluasi representasi. Pembahasan ini akan memberikan landasan metodologis bagi Anda untuk merancang studi pendahuluan, memvalidasi hipotesis awal, serta mengidentifikasi research gap yang potensial dikontribusikan dalam publikasi internasional.

Rangkaian penjelasan ini juga menyiapkan transisi menuju rangkuman pada slide berikutnya, di mana kita akan menyimpulkan bahwa perbedaan inductive bias antara CNN dan ViT secara langsung mempengaruhi kebutuhan data dan strategi pelatihan. Lebih jauh, standar pelaporan penelitian doctoral menuntut penjelasan kausal mengapa suatu representasi unggul dalam konteks tertentu, bukan sekadar klaim kemenangan berdasarkan metrik akurasi semata.

---

## Slide 039 - Rangkuman

### Narasi

Slide ini berfungsi sebagai rangkuman strategis dari seluruh pembahasan mengenai representasi visual modern. Pada tingkat doktoral, pemahaman kritis tentang trade-off antara arsitektur CNN dan Vision Transformer menjadi prasyarat fundamental sebelum merancang metodologi penelitian yang inovatif dan reproducible.

Berikut adalah poin-poin kunci yang perlu menjadi acuan dalam merancang eksperimen dan penulisan paper:

- **CNN dan Inductive Bias Lokal**: Operasi konvolusi dan pooling memberikan bias spasial yang kuat, sehingga CNN sangat efisien pada dataset berukuran terbatas karena mampu mengekstrak pola hierarkis tanpa memerlukan volume data masif.
- **ViT dan Inductive Bias Lemah**: Pemecahan citra menjadi patch dan pemrosesan paralel menghilangkan bias lokal bawaan. Keunggulan ViT baru termanifestasi sepenuhnya ketika didukung oleh dataset berskala besar atau protokol *pretraining* yang sangat ketat.
- **Mekanisme Attention**: Kemampuan ViT membangun relasi global antar semua patch secara langsung memungkinkan pemodelan konteks jangka panjang yang lebih holistik dibandingkan receptive field terbatas pada CNN konvensional.
- **Dominasi Strategi Pretraining**: Dalam praktik state-of-the-art, kualitas representasi sering kali lebih ditentukan oleh skema *pretraining* (misalnya supervised vs self-supervised) daripada sekadar modifikasi arsitektur.
- **Protokol Komparasi yang Rigor**: Evaluasi model harus mempertimbangkan parameter, beban komputasi (*FLOPs*), ukuran dataset, dan konsistensi protokol evaluasi. Klaim kinerja tanpa kontrol variabel tersebut rentan terhadap bias seleksi.

Implikasi praktisnya bagi peneliti adalah pemilihan representasi visual harus selalu diselaraskan dengan ruang lingkup masalah, ketersediaan data, dan batas komputasi. Laporan komparatif yang berkualitas tidak boleh berhenti pada pernyataan "siapa yang menang", melainkan harus menjelaskan secara analitis mengapa suatu pendekatan unggul dalam konteks spesifik, termasuk diskresi atas *bias* komputasional dan potensi generalisasi.

Ringkasan ini juga menyiapkan transisi natural menuju materi *self-supervised learning*. Sebagaimana disebutkan pada slide sebelumnya, arsitektur ViT telah menjadi fondasi utama dalam metode modern seperti DINO dan DINOv2, di mana kualitas representasi sangat bergantung pada mekanisme *pretraining* tanpa label. Untuk menelusuri landasan teoretis, spesifikasi arsitektur asli, dan panduan reproduksi implementasi dari setiap model yang dibahas, silakan merujuk pada daftar literatur kunci yang akan kita paparkan pada slide berikutnya.

---

## Slide 040 - Referensi Kunci

### Narasi

Pada slide sebelumnya, kita telah merangkum perbedaan fundamental antara CNN dan Vision Transformer, khususnya mengenai inductive bias, cakupan receptive field, serta dominasi skema pretraining dalam menentukan kinerja akhir. Untuk memperkuat landasan teoretis dan memastikan reproduktibilitas eksperimen, slide ini menyajikan referensi kunci yang menjadi pilar utama dalam evolusi representasi visual modern.

Daftar berikut merupakan karya seminal yang wajib dikaji secara kritis pada tingkat doktor:
- **ResNet (He et al., 2016)** memperkenalkan residual connection yang memungkinkan pelatihan jaringan sangat dalam tanpa degradasi performa, sekaligus menetapkan baseline kuat untuk arsitektur berbasis konvolusi.
- **Transformer (Vaswani et al., 2017)** memisahkan pemodelan dependensi dari struktur rekurensi atau konvolusi, membuktikan bahwa self-attention mampu menangkap hubungan global secara paralel dan skalabel.
- **ViT (Dosovitskiy et al., 2021)** mentransfer arsitektur transformer murni ke domain citra dengan partisi patch, mengungkap bahwa kelemahan ViT awal bukanlah arsitekturnya, melainkan kurangnya inductive bias dan data training yang memadai.
- **DeiT (Touvron et al., 2021)** mengatasi ketergantungan data masif ViT melalui knowledge distilasi berbasis attention, menunjukkan bahwa transformator dapat mencapai efisiensi tinggi bahkan pada dataset menengah.
- **Swin Transformer (Liu et al., 2021)** mengintegrasikan hierarki fitur dan shifted window attention, menghasilkan representasi multi-skala yang lebih kompatibel dengan tugas dense prediction seperti segmentasi semantik dan deteksi objek.

Di sisi implementasi, reproduksi hasil penelitian menuntut penggunaan stack perangkat lunak yang stabil dan terdokumentasi dengan baik. **PyTorch** dan **torchvision** menyediakan infrastruktur tensor dan modul optimasi yang menjadi standar de facto, sedangkan **timm** menawarkan implementasi model terkini yang telah divalidasi secara empiris untuk kemudahan transfer learning dan benchmarking. Mahasiswa disarankan mencermati dokumentasi resmi serta menyelaraskan pemilihan library dengan protokol eksperimen yang akan dilaporkan.

Daftar referensi ini berfungsi sebagai peta jalan untuk melacak jejak metodologis dan mengidentifikasi celah penelitian. Pada slide berikutnya, pemahaman teoretis ini akan diterjemahkan ke dalam tugas praktikum. Anda diminta melakukan fine-tuning terhadap minimal satu model CNN dan satu model ViT, mengumpulkan metrik lengkap termasuk kurva pembelajaran, waktu inferensi, dan kompleksitas komputasi, lalu menyusun laporan yang tidak hanya membandingkan angka, tetapi juga mengevaluasi trade-off representasi visual sesuai dengan research gap yang telah dirumuskan sebelumnya.

---

## Slide 041 - Tugas dan Target Keluaran

### Narasi

Pada slide ini, kita beralih dari tinjauan literatur ke implementasi praktis yang menjadi inti dari pertemuan ketiga ini. Setelah sebelumnya membahas arsitektur kunci seperti ResNet, Transformer, ViT, hingga DeiT dan Swin Transformer beserta dokumentasi resmi PyTorch dan timm, kini saatnya menerjemahkan pemahaman teoretis tersebut ke dalam eksperimen terkontrol.

Tugas praktikum yang harus Anda kerjakan adalah melakukan *fine-tuning* pada model yang tersedia di pustaka `torchvision` atau `timm`. Pilihlah setidaknya satu arsitektur CNN klasik sebagai baseline, dan bandingkan secara langsung dengan satu model Vision Transformer. Pastikan Anda menggunakan dataset citra yang telah dikontrol agar variabel confounding dapat diminimalkan selama proses evaluasi.

Selama pelaksanaan eksperimen, kumpulkan data komprehensif yang mencakup:
- Metrik kinerja utama dan kurva pembelajaran (*learning curves*)
- Waktu inferensi per batch
- Jumlah parameter trainable

Data-data ini bukan sekadar angka untuk pelaporan, melainkan fondasi bagi analisis kritis tingkat doktor. Target keluaran yang diharapkan adalah sebuah laporan komparatif yang tidak hanya menyajikan akurasi, tetapi juga membedah faktor-faktor arsitektural, strategi *pretraining*, dan karakteristik data yang memengaruhi performa masing-masing model. Hubungkan temuan empiris Anda dengan *research gap* yang telah Anda identifikasi pada pertemuan kedua. Pertanyaan kunci yang harus dijawab adalah bagaimana representasi visual dari CNN dan ViT saling melengkapi atau bertolak belakang dalam konteks masalah penelitian Anda.

Kriteria penilaian akan berfokus pada tiga aspek utama:
- Kesesuaian protokol eksperimen dengan standar reproduktibilitas ilmiah
- Kedalaman interpretasi hasil yang menunjukkan kemampuan analisis kritis, bukan deskriptif semata
- Kejelasan penjelasan mengenai *trade-off* dalam representasi visual, seperti bias induktif CNN versus kebutuhan data besar pada ViT, serta implikasinya terhadap efisiensi komputasi dan generalisasi model

Hasil dari tugas ini akan menjadi bahan diskusi mendalam sebelum kita menutup pertemuan hari ini. Pada slide penutup, kita akan merangkum poin-poin penting, dan persiapan analitis Anda akan diteruskan langsung ke topik berikutnya, yaitu *Self-Supervised Learning* dan *Foundation Vision Models*, yang merupakan langkah logis setelah memahami batasan dan keunggulan arsitektur supervised tradisional maupun transformer-based.

---

## Slide 042 - Penutup

### Narasi

Kita mengakhiri pertemuan ketiga ini dengan merangkum evolusi representasi visual modern yang telah kita bedah bersama. Dari arsitektur CNN yang mengandalkan field reseptif lokal dan berbagi bobot, hingga mekanisme attention yang memungkinkan pemodelan konteks global, pergeseran paradigma menuju Vision Transformer menandai perubahan fundamental dalam cara model memproses informasi spasial. ViT menghilangkan bias induktif konvolusi dengan memperlakukan citra sebagai urutan token, sehingga mampu menangkap dependensi jarak jauh melalui mekanisme self-attention murni. Pemahaman mendalam terhadap perbedaan representasi ini menjadi fondasi kritis untuk evaluasi arsitektur pada tingkat penelitian doktoral.

Seiring penutupan sesi ini, pastikan Anda menindaklanjuti tugas praktikum yang telah dijabarkan pada slide sebelumnya. Eksperimen perbandingan antara CNN dan ViT memerlukan perhatian khusus pada aspek-aspek berikut:
- Melakukan fine-tuning menggunakan library `torchvision` atau `timm` pada dataset terkontrol.
- Mengumpulkan metrik lengkap meliputi kurva pembelajaran, waktu inferensi, dan jumlah parameter.
- Menganalisis trade-off arsitektural, pengaruh strategi pretraining, serta sensitivitas terhadap karakteristik dataset.
- Menghubungkan hasil komparatif secara eksplisit dengan research gap yang telah Anda identifikasi, guna memperkuat positioning metodologi dalam rancangan proposal disertasi awal.

Pada pertemuan berikutnya, fokus kajian akan bergeser ke Self-Supervised Learning dan Foundation Vision Models. Transisi ini sangat relevan mengingat keterbatasan paradigma supervised dalam skala data besar yang umumnya tidak berlabel. Kita akan mengeksplorasi bagaimana metode seperti DINO/DINOv2, pembelajaran kontrastif, dan masked autoencoders memungkinkan ekstraksi fitur visual yang robust tanpa annotasi manual. Lebih lanjut, kita akan membahas bagaimana model-model foundation ini berfungsi sebagai backbone yang dapat diadaptasi lintas tugas, mencerminkan arah perkembangan computer vision yang semakin modular dan general-purpose.

Persiapkan diri Anda untuk membaca paper-paper seminal terkait SSL dan arsitektur foundation model sebelum kelas berikutnya dimulai. Diskusi akan difokuskan pada analisis kritis terhadap mekanisme pretraining, skalabilitas, serta potensi bottleneck dalam transfer knowledge antar domain visual. Terima kasih atas partisipasi aktif selama sesi hari ini.
