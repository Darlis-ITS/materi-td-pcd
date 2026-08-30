# Narasi TD Pengolahan Citra Digital - Pertemuan 06

## Image Restoration dan Computational Imaging

Sumber: markdown/pert06-image-restoration-dan-computational-imaging.md

---

## Slide 000 - Cover

### Narasi

Fokus utama pada babak ini adalah *Image Restoration* dan *Computational Imaging*, dua pilar yang mengubah cara kita memandang hubungan antara akuisisi sensor dan interpretasi visual. Dalam konteks penelitian doktoral, restorasi citra tidak lagi dipandang sebagai tahap pra-pemrosesan sederhana, melainkan sebagai masalah inversi terregularisasi yang menuntut integrasi mendalam antara fisika pencitraan, optimisasi numerik, dan representasi laten berbasis *deep learning*.

*Computational Imaging* mendorong pergeseran paradigma dari pencitraan pasif ke desain sistem aktif. Melalui rekayasa protokol akuisisi—seperti *coded aperture*, *single-pixel camera*, atau *multi-spectral integration*—kita dapat mengeksploitasi redundansi spasial-spektral untuk memulihkan informasi yang secara fisik hilang atau terdegradasi. Implementasi modernnya umumnya mengandalkan jaringan saraf yang dilatih secara end-to-end, memanfaatkan kerangka kerja diferensial seperti *PyTorch* untuk menyelesaikan persamaan invers yang non-linear dan *ill-posed*.

Kemajuan terkini dalam restorasi sangat dimungkinkan oleh ketersediaan *foundation model* dan teknik *self-supervised learning*. Model seperti *DINO/DINOv2* dan arsitektur *diffusion* memberikan prior struktural yang kuat tanpa memerlukan pasangan data *clean-corrupted* dalam skala masif. Hal ini memungkinkan pendekatan *blind restoration* dan *zero-shot enhancement* yang lebih robust terhadap variasi degradasi dunia nyata, sekaligus membuka ruang eksplorasi untuk:
- *Domain adaptation* lintas perangkat akuisisi
- Generalisasi pada kondisi pencahayaan dan atmosfer ekstrem
- Integrasi prior semantik dari model *vision-language* dalam tugas restorasi

Pembahasan ini secara strategis mengaitkan kembali representasi visual dan metode evaluasi kritis dari sesi sebelumnya, sekaligus menyiapkan kerangka metodologis untuk penelitian tingkat lanjut. Narasi selanjutnya akan memetakan posisi topik ini dalam alur kurikulum, menunjukkan bagaimana restorasi citra berfungsi sebagai fondasi krusial sebelum beralih ke deteksi objek modern dengan YOLO dan transformer, serta mengintegrasikannya dengan target capaian pembelajaran mahasiswa doktoral.

---

## Slide 001 - Posisi Pertemuan dalam Rangkaian Perkuliahan

### Narasi

Slide ini menempatkan Pertemuan 06 dalam peta besar rangkaian perkuliahan Topik Dalam Pengolahan Citra Digital. Sebelumnya, kita telah mendalami bagaimana mesin memahami makna citra melalui representasi visual dan pemodelan multimodal berbasis Vision-Language Models. Fokusnya adalah pada ekstraksi fitur semantik dan pencocokan lintas modalitas. Kini, kita beralih ke aspek yang lebih fundamental namun krusial dalam pipeline computer vision: memperbaiki kualitas citra dari pengukuran yang rusak, noisy, atau tidak lengkap. Bidang ini dikenal sebagai Image Restoration dan Computational Imaging.

Transisi ini merupakan evolusi logis dalam alur penelitian. Metode restoration modern memanfaatkan fondasi yang telah kita bangun pada pertemuan-pertemuan sebelumnya:
- Representasi visual tingkat tinggi dari Pertemuan 03 menjadi backbone arsitektur restoration.
- Prinsip self-supervised learning dan pemanfaatan foundation models dari Pertemuan 04 memungkinkan pelatihan model tanpa memerlukan pasangan data degraded-clean yang masif dan mahal.
- Kemampuan critical reading dan evaluasi klaim metodologis dari Pertemuan 02 menjadi kunci untuk membedah paper restoration terkini, mengidentifikasi kelemahan arsitektur, dan menilai validitas klaim peningkatan kualitas.

Keterkaitan dengan Capaian Pembelajaran Mata Kuliah (CPMK) juga terintegrasi secara eksplisit. CPMK-1 terpenuhi melalui pemahaman konseptual tentang inverse problem dalam imaging. CPMK-3 diaktifkan saat kita merancang eksperimen degradasi sintetis dan memilih kerangka metrik evaluasi yang tepat. Sementara CPMK-4 tercapai ketika mahasiswa mampu memposisikan metode restoration baru terhadap state-of-the-art dan menemukan research gap yang layak untuk kontribusi disertasi.

Sejalan dengan penempatan topik ini, slide berikutnya akan mengarahkan kita pada tujuan pembelajaran dan target keluaran yang lebih teknis. Kita akan mempelajari bagaimana merumuskan image restoration sebagai inverse problem dengan forward model yang eksplisit, serta membedakan secara kritis antara fidelity numerik dan kualitas perseptual. Evaluasi tidak lagi mengandalkan satu metrik tunggal seperti PSNR atau SSIM, melainkan protokol yang holistik. Target keluarannya meliputi penyusunan protokol evaluasi, analisis trade-off antara akurasi numerik, kualitas subjektif, dan beban komputasi, serta pelaksanaan eksperimen baseline restoration terhadap data degradasi sintetis. Semua elemen ini akan menjadi landasan penting sebelum kita membahas object detection modern pada pertemuan selanjutnya, mengingat kualitas input citra secara langsung menentukan performa detektor downstream.

---

## Slide 002 - Tujuan Pembelajaran dan Target Keluaran

### Narasi

Setelah pada pertemuan sebelumnya kita membahas bagaimana mesin memahami makna citra melalui representasi visual dan model multimodal, fokus kini bergeser ke aspek yang lebih fundamental dalam pengolahan sinyal: memperbaiki kualitas citra dari pengukuran yang rusak atau tidak lengkap. Slide ini menetapkan pondasi metodologis untuk topik Image Restoration dan Computational Imaging. Pada tingkat doktoral, restoration bukan sekadar penerapan filter konvensional, melainkan perumusan masalah matematis yang ketat.

Pertama, mahasiswa dituntut mampu merumuskan image restoration sebagai sebuah inverse problem dengan forward model yang eksplisit. Artinya, sebelum melakukan restorasi, kita harus mendefinisikan secara matematis bagaimana proses degradasi terjadi—mulai dari sensor noise, motion blur, hingga downsampling. Forward model menjadi kunci karena ketidakunikan solusi dalam inverse problem sering kali memerlukan regularisasi berbasis prior pengetahuan atau pembelajaran data.

Kedua, pemahaman mendalam mengenai perbedaan antara fidelity numerik dan kualitas perseptual menjadi wajib. Metrik tradisional seperti PSNR atau SSIM mengukur kesamaan piksel demi piksel, namun sering kali berkorelasi buruk dengan penilaian subjektif manusia. Model generasi modern seperti diffusion-based atau arsitektur transformer dapat menghasilkan detail tekstur yang sangat meyakinkan secara visual, meskipun secara numerik menyimpang dari ground truth. Kesenjangan inilah yang perlu dikaji secara kritis.

Ketiga, evaluasi metode restoration tidak boleh bergantung pada satu metrik tunggal. Pendekatan yang robust memerlukan protokol komprehensif yang menggabungkan ukuran objektif, studi persepsi manusia, dan analisis domain-specific. Hal ini sejalan dengan tuntutan penelitian mutakhir di mana klaim kinerja model harus diverifikasi dari berbagai perspektif agar tidak terjebak pada overfitting terhadap benchmark tertentu.

Sebagai target keluaran, Anda akan menyusun protokol evaluasi restoration yang terstruktur, menganalisis trade-off antara fidelity, perceptual quality, dan computational cost, serta menjalankan eksperimen baseline dengan degradasi sintetis. Hasil kerja ini akan langsung menjawab empat pertanyaan kunci pada slide berikutnya: apakah model benar-benar memulihkan informasi asli atau hanya menghasilkan detail yang meyakinkan, bagaimana menangani ketidakpastian solusi inverse problem, validitas data sintetis terhadap degradasi nyata, serta strategi evaluasi yang bebas bias metrik. Persiapan ini merupakan langkah strategis menuju critical paper review dan penyusunan proposal riset disertasi Anda.

---

## Slide 003 - Pertanyaan Kunci Pertemuan Ini

### Narasi

Slide ini menyajikan empat pertanyaan kunci yang akan menjadi lensa analitis utama sepanjang pertemuan ini. Pertanyaan-pertanyaan tersebut dirancang khusus untuk mengasah kemampuan kritik ilmiah dan menjadi fondasi perumusan masalah penelitian tingkat doktoral. Mari kita telaah satu per satu dengan pendekatan metodologis yang ketat.

1. Apakah model benar-benar memulihkan informasi asli, atau sekadar menghasilkan detail yang secara visual meyakinkan? Dalam konteks *image restoration* sebagai *inverse problem*, solusi sering kali non-uniq. Arsitektur *generative* seperti diffusion models atau GANs cenderung mengoptimalkan *perceptual quality* di atas *fidelity numerik*. Penelitian tingkat lanjut harus mampu membedakan antara rekonstruksi yang忠实 pada sinyal fisik versus sintesis tekstur yang hanya memenuhi harapan perseptual manusia.

2. Bagaimana ketidakpastian (*uncertainty*) dari solusi *inverse problem* diperlakukan secara eksplisit? Proses degradasi citra jarang bersifat deterministik. Pendekatan Bayesian, *Monte Carlo dropout*, atau *ensemble methods* diperlukan untuk mengkuantifikasi varians prediksi. Mengintegrasikan estimasi ketidakpastian ke dalam pipeline restoration bukan hanya meningkatkan keandalan sistem pada data *out-of-distribution*, tetapi juga membuka jalur riset baru dalam *trustworthy computer vision*.

3. Apakah data sintetis yang digunakan untuk melatih atau mengevaluasi benar-benar merepresentasikan degradasi nyata? Ini merupakan *research gap* yang sangat persisten. Benchmark konvensional sering mengandalkan degradasi matematis sederhana (Gaussian blur, additive white noise) yang mengabaikan karakteristik optik lensa, respons sensor, atau kompresi lossy. Validasi hanya pada data sintetis berisiko menghasilkan klaim kinerja yang overfit terhadap asumsi simulasi. Anda perlu merancang protokol augmentasi yang lebih *physics-aware* atau menerapkan teknik *domain adaptation*.

4. Bagaimana merancang evaluasi yang tidak bias oleh satu metrik? Metrik piksel tradisional seperti PSNR atau SSIM sering berkorelasi lemah dengan penilaian subjektif atau performa pada tugas *downstream*. Evaluasi yang robust harus bersifat multidimensi: menggabungkan metrik struktural, metrik berbasis fitur (LPIPS, FID), serta uji transferabilitas pada deteksi objek atau segmentasi. Desain evaluasi multi-aspek ini harus menjadi standar dalam publikasi internasional bereputasi.

Empat pertanyaan ini secara langsung menyambung dengan tujuan pembelajaran pada slide sebelumnya, khususnya terkait pembedaan konsep *fidelity numerik* versus *kualitas perseptual* serta kebutuhan protokol evaluasi yang independen terhadap metrik tunggal. Jawaban atas pertanyaan-pertanyaan ini akan menjadi kerangka kerja baku saat Anda melakukan *critical paper review* dan menyusun hipotesis riset.

Seiring kita beralih ke slide berikutnya, kita akan mendefinisikan secara teknis ruang lingkup *image restoration*, memformulasikannya melalui *forward model*, serta membedakannya secara tegas dari *image enhancement*. Pemahaman definisi dan batasan masalah ini akan memperkuat penerapan keempat pertanyaan kunci tadi ke dalam kerangka komputasional yang rigor.

---

## Slide 004 - Apa Itu Image Restoration?

### Narasi

Pada slide ini, kita mendefinisikan inti dari **Image Restoration**, yaitu upaya memperkirakan citra bersih atau asli, yang dinotasikan sebagai `x`, berdasarkan observasi yang telah terdegradasi, `y`. Konsep ini secara langsung merespons pertanyaan kunci dari slide sebelumnya mengenai apakah sebuah model benar-benar memulihkan informasi asli atau sekadar menghasilkan detail yang meyakinkan secara visual. Dalam konteks penelitian tingkat doktoral, pembedaan konseptual ini menjadi titik tolak kritis untuk mengevaluasi validitas arsitektur jaringan, pemilihan fungsi kerugian, serta generalisasi model terhadap distribusi data yang tidak terlihat.

Alur pemrosesan yang ditampilkan pada diagram merepresentasikan siklus matematis standar dalam domain ini. Pertama, citra ideal `x` mengalami transformasi melalui **forward model** sehingga menghasilkan observasi `y`. Kedua, tahap restorasi berusaha membalikkan proses tersebut untuk menghasilkan estimasi `x_hat`. Tantangan fundamentalnya terletak pada sifat masalah balik (*inverse problem*) yang umumnya bersifat ill-posed, di mana satu observasi `y` dapat berkorespondensi dengan множество kemungkinan citra `x`. Akibatnya, penanganan ketidakpastian, penerapan regularisasi, dan integrasi prior pengetahuan menjadi komponen wajib dalam perancangan solver.

Tugas-tugas spesifik dalam domain restorasi mencakup beberapa kategori utama yang sering menjadi fokus kajian literatur mutakhir:
- **Denoising**: menghilangkan noise acak yang merusak integritas sinyal gambar.
- **Deblurring**: mengembalikan ketajaman akibat gerakan kamera, defokus optik, atau keterbatasan bandwidth sensor.
- **Super-resolution**: meningkatkan resolusi spasial dari citra beresolusi rendah dengan mempertahankan struktur frekuensi tinggi.
- **Inpainting**: mengisi region yang hilang atau rusak berdasarkan konteks spasial dan semantik sekitarnya.
- **De-hazing / Deraining**: memulihkan kontras dan warna yang terdistorsi oleh partikel atmosfer atau fenomena cuaca.
Setiap tugas tersebut menuntut pemodelan degradasi yang spesifik, karena asumsi noise atau blur yang salah akan mengarah pada artefak rekonstruksi yang sistematis.

Pembedaan tegas antara **Restoration** dan **Enhancement** perlu ditekankan agar tidak terjadi kesalahan metodologis dalam perumusan penelitian. Restoration berangkat dari asumsi adanya model degradasi yang dapat diidentifikasi atau diaproksimasi, dengan tujuan membalikkan efek fisik atau teknis secara objektif. Ground truth-nya biasanya tersedia atau dapat direplikasi secara ketat. Sebaliknya, enhancement lebih bersifat subjektif dan bertujuan memperbaiki persepsi visual manusia tanpa mengacu pada model degradasi yang eksplisit. Perbedaan ini secara langsung mempengaruhi strategi evaluasi: restoration memerlukan metrik berbasis pixel atau fitur yang membandingkan `x_hat` terhadap referensi, sedangkan enhancement sering kali mengandalkan penilaian perceptual atau user study.

Konsep restorasi sebagai solusi *inverse problem* ini akan menjadi jembatan langsung menuju pembahasan **Computational Imaging** pada slide berikutnya. Kita akan melihat bagaimana paradigma bergeser dari sekadar memproses citra akhir terdegradasi, menuju desain bersama antara hardware akuisisi dan algoritma rekonstruksi, di mana pengukuran mentah sensor dimanipulasi secara sengaja untuk mengekstrak informasi yang sebelumnya tidak terjangkau oleh pipeline konvensional.

---

## Slide 005 - Computational Imaging: Lebih dari Sekadar Filter

### Narasi

Pada slide ini, kita beralih dari konsep dasar *image restoration* ke paradigma yang lebih komprehensif, yaitu *computational imaging*. Berbeda dengan pendekatan tradisional yang hanya mengandalkan filter pasca-pemrosesan pada citra terdegradasi, *computational imaging* menekankan desain bersama (*co-design*) antara proses akuisisi fisik dan algoritma rekonstruksi digital. Sensor atau sistem optik tidak lagi bekerja secara pasif, melainkan dirancang khusus agar data mentah yang dihasilkan dapat direkonstruksi menjadi informasi visual berkualitas tinggi melalui model matematika dan komputasi.

Contoh nyata penerapan *computational imaging* mencakup berbagai teknik canggih, antara lain:
- *Coded aperture* atau *coded exposure* untuk meningkatkan rasio signal-to-noise tanpa mengorbankan resolusi spasial.
- Pencitraan *light field* yang memungkinkan refokus dan estimasi kedalaman pasca-pengambilan gambar.
- *Phase retrieval* dalam pencitraan koheren dan optik difraktif.
- Rekonstruksi tomografi dan MRI, di mana sinyal proyeksi atau domain frekuensi diubah menjadi representasi anatomi.

Jika dibandingkan dengan *image restoration* klasik yang telah kita bahas pada slide sebelumnya, terdapat perbedaan fundamental dalam alur pemrosesan:
- **Input**: Restorasi klasik menerima citra akhir yang sudah terdegradasi, sedangkan *computational imaging* bekerja langsung dari pengukuran mentah atau respons sensor.
- **Forward Model**: Restorasi umumnya menangani blur, noise, atau downsampling sederhana, sementara *computational imaging* melibatkan proyeksi geometris, kode optik, atau pola sampling terstruktur.
- **Output**: Restorasi bertujuan mengembalikan citra bersih mendekati ground truth, sedangkan *computational imaging* sering kali mengekstrak informasi tersembunyi yang tidak terekam secara langsung oleh sensor.

Inti dari kedua pendekatan ini sebenarnya bersatu pada satu prinsip matematis: keduanya merupakan *inverse problem*. Dalam konteks perkuliahan sebelumnya, kita telah melihat bagaimana *image restoration* mencoba memulihkan citra bersih $x$ dari observasi $y$. Sekarang, kita perlu memahami bahwa dalam *computational imaging*, tantangan utamanya justru terletak pada perumusan forward model yang akurat dan penyelesaian masalah invers yang sering kali bersifat *ill-posed*. Hal ini akan kita bedah lebih mendalam pada slide berikutnya, di mana kita akan membahas formulasi matematis forward model, peran operator degradasi $H$, serta strategi regularisasi untuk menstabilkan solusi inverse problem.

---

## Slide 006 - Forward Model dan Inverse Problem

### Narasi

Pada slide ini, kita mengonfirmasi secara matematis konsep yang telah dibahas pada pertemuan sebelumnya mengenai keterkaitan fundamental antara *image restoration* dan *computational imaging*. Kedua bidang ini pada hakikatnya menyelesaikan masalah inversi, di mana tujuan utamanya adalah memulihkan informasi asli dari data yang teramati melalui sensor atau sistem akuisisi.

Proses transformasi dari citra ideal menuju data observasi disebut sebagai *forward model*. Secara umum, hubungan ini dapat dituliskan sebagai:
```
y = f(x, degradasi)
```
Di sini, `x` merepresentasikan citra bersih atau sinyal asli yang ingin kita estimasi, sedangkan `y` adalah hasil pengukuran atau citra terdegradasi yang tersedia. Fungsi `f` mencakup seluruh proses fisik, optik, maupun algoritmik yang mengubah `x` menjadi `y` selama tahap akuisisi.

Sebaliknya, *inverse problem* adalah tugas komputasional untuk mendapatkan perkiraan `x` hanya berdasarkan pengetahuan terhadap `y` dan karakteristik model degradasinya. Karena informasi hilang atau tercampur selama proses akuisisi, masalah inversi ini umumnya bersifat *ill-posed*. Dalam konteks penelitian doktoral, penyelesaian masalah ini menuntut penggunaan regularisasi, asumsi priors yang kuat, serta strategi optimasi yang stabil agar solusi yang dihasilkan tidak bias atau divergen.

Sebagai pendekatan awal, banyak formulasi degradasi dapat didekati menggunakan model linear:
```
y = H x + n
```
Pemahaman mendalam terhadap setiap komponen persamaan ini sangat krusial untuk desain eksperimen dan pemilihan arsitektur model:
- `y`: citra terdegradasi atau vektor pengukuran mentah.
- `H`: operator degradasi yang dapat berupa matriks konvolusi untuk *blur*, operator subsampling untuk *downsampling*, atau matriks proyeksi geometris pada pencitraan medis.
- `n`: noise aditif yang biasanya dimodelkan berdistribusi Gaussian, Poisson, atau Laplace tergantung karakteristik sensor dan kondisi pencahayaan.
- `x`: citra bersih target rekonstruksi yang menjadi fokus optimasi.

Alur pemrosesan ini dapat divisualisasikan secara sederhana sebagai berikut:
```
x --(H)--> Hx --(+n)--> y
```
Diagram ini menegaskan bahwa setiap tahap dalam rantai akuisisi citra harus dipetakan secara eksplisit ke dalam operator matematika. Akurasi dalam mendefinisikan struktur operator `H` dan distribusi statistik noise `n` menjadi fondasi utama dalam merumuskan *loss function*, memilih teknik regularisasi, hingga mengembangkan arsitektur *deep learning* khusus untuk tugas restorasi.

Pembahasan matematis umum ini akan langsung dispesifikasikan pada slide berikutnya, di mana bentuk operator `H` dan komponen noise `n` akan dirinci sesuai karakteristik masing-masing tugas restorasi, sekaligus memperkenalkan konsep *degradation pipeline* yang sering digunakan dalam sintesis data pelatihan model modern.

---

## Slide 007 - Formulasi Matematis Degradasi

### Narasi

Pada slide ini, kita menguraikan formulasi matematis spesifik untuk berbagai tugas *image restoration* yang merupakan penyempitan langsung dari model umum *forward model* pada slide sebelumnya. Ingat kembali bahwa secara umum hubungan antara citra bersih dan observasi dapat dituliskan sebagai `y = Hx + n`, di mana operator `H` berubah sepenuhnya tergantung pada jenis degradasi fisik yang terjadi pada sistem pencitraan.

Mari kita bedah tiga formulasi standar yang menjadi fondasi analisis dalam penelitian *computational imaging*:
- **Denoising**: Modelnya paling sederhana karena hanya melibatkan penambahan gangguan acak tanpa mengubah struktur spasial atau resolusi. Persamaannya ditulis sebagai `y = x + n`. Variabel `n` di sini merepresentasikan noise yang umumnya dimodelkan menggunakan distribusi Gaussian atau Poisson, tergantung pada karakteristik sensor dan kondisi pencahayaan.
- **Deblurring**: Operator degradasi berubah menjadi operasi konvolusi dengan sebuah kernel. Persamaannya menjadi `y = k ⊛ x + n`. Simbol `⊛` menandakan operasi konvolusi, sedangkan `k` adalah *point spread function* atau *blur kernel*. Kernel ini menentukan bagaimana cahaya tersebar akibat gerakan kamera, ketidaksempurnaan fokus, atau interferensi atmosfer.
- **Super-resolution**: Degradasi tidak hanya melibatkan pengaburan optik, tetapi juga penurunan resolusi digital. Model matematiknya digabungkan menjadi `y = (k ⊛ x) ↓_s + n`. Operator `↓_s` menunjukkan proses *downsampling* dengan faktor skala `s`, yang secara praktis merepresentasikan keterbatasan matriks piksel sensor atau kompresi data sebelum tahap pemrosesan lebih lanjut.

Perlu ditekankan bahwa dalam skenario dunia nyata maupun dalam pembuatan dataset sintetik untuk pelatihan model *deep learning*, degradasi jarang terjadi secara tunggal. Berbagai proses ini sering kali tumpang tindih dan dapat disusun menjadi sebuah **degradation pipeline**. Pipeline ini memungkinkan kita memodelkan rantai transformasi secara berurutan, mulai dari respons optik lensa, propagasi cahaya ke sensor, hingga proses *quantization* dan penyimpanan digital. 

Memahami urutan dan interaksi komponen dalam *pipeline* ini sangat krusial pada tingkat doktoral. Akurasi simulasi degradasi akan langsung mempengaruhi validitas eksperimen Anda, kemampuan mengidentifikasi *research gap*, serta desain arsitektur jaringan saraf tiruan yang mampu memisahkan sinyal asli dari artefak kompleks. Kesalahan dalam mendefinisikan pipeline sering menjadi penyebab utama kegagalan generalisasi model saat diuji pada data nyata.

Dengan pemahaman terhadap persamaan-persamaan dasar ini, kita siap untuk mengklasifikasikan setiap komponen degradasi berdasarkan karakteristik matematis dan sumber asalnya, yang akan kita bahas secara rinci pada slide berikutnya.

---

## Slide 008 - Klasifikasi Degradasi: Noise, Blur, Downsampling

### Narasi

Pada slide sebelumnya, kita telah menelaah formulasi matematis umum untuk berbagai tugas restorasi citra, mulai dari denoising, deblurring, hingga super-resolusi, serta konsep *degradation pipeline*. Dari persamaan-persamaan tersebut, jelas bahwa setiap proses degradasi memiliki struktur aljabar dan statistik yang unik. Slide ini akan mengklasifikasikan jenis-jenis degradasi berdasarkan karakteristik fisik dan matematisnya, lengkap dengan model penyederhanaan serta sumber asalnya di dunia nyata.

Berikut adalah rincian klasifikasi degradasi yang perlu dipahami secara mendalam:
- **Sensor noise**: Dimodelkan sebagai distribusi Gaussian atau Poisson. Muncul secara alami pada kondisi low-light atau penggunaan ISO tinggi, di mana fluktuasi elektronika sensor menghasilkan variasi intensitas piksel yang acak.
- **Blur**: Dapat direpresentasikan dengan *Gaussian kernel*, *motion kernel*, atau *defocus*. Sumber nyatanya mencakup gerakan kamera saat eksposur, kesalahan fokus optik, atau distorsi atmosfer. Proses ini menyebarkan energi sinyal ke tetangga spasialnya melalui konvolusi.
- **Downsampling**: Melibatkan *decimation* dan fenomena *aliasing* ketika resolusi sensor terbatas atau dilakukan reduksi ukuran tanpa filter anti-aliasing. Informasi frekuensi tinggi hilang permanen jika tidak ditangani dengan teknik interpolasi atau rekonstruksi yang tepat.
- **Kompresi**: Artefak JPEG muncul dari proses kuantisasi blok DCT dan *entropy coding*. Degradasi ini bersifat non-linear, terstruktur, dan sangat bergantung pada tingkat kompresi yang diterapkan selama penyimpanan atau transmisi.

Poin kritis yang harus ditekankan adalah bahwa setiap jenis degradasi memiliki karakteristik matematis berbeda, sehingga menuntut penanganan algoritmis yang spesifik. Pendekatan berbasis statistik mungkin efektif untuk *noise*, namun kurang optimal untuk *blur* atau artefak kompresi. Dalam konteks penelitian doktoral, pemahaman ini menjadi dasar dalam merancang arsitektur model, memilih representasi domain (spasial vs frekuensi), dan menentukan strategi *data augmentation* yang mencerminkan distribusi degradasi sebenarnya.

Menghubungkan dengan slide berikutnya, kita akan melihat contoh konkret model degradasi sintetik seperti *Gaussian noise*, *Gaussian blur*, dan *downsampling*, beserta persamaan dan sumber nyatanya. Perlu diingat bahwa pemilihan parameter degradasi yang realistis bukanlah keputusan teknis semata, melainkan langkah strategis dalam desain penelitian. Parameter seperti varians *noise*, ukuran *kernel*, atau faktor *scaling* harus dikalibrasi berdasarkan analisis empiris terhadap dataset target, karena hal ini akan menentukan validitas eksternal dan potensi kontribusi ilmiah dari metode restorasi yang Anda kembangkan.

---

## Slide 009 - Model Degradasi Sintetis: Contoh dan Sumber Nyata

### Narasi

Setelah pada slide sebelumnya kita mengklasifikasikan berbagai jenis degradasi berdasarkan karakteristik matematisnya, langkah selanjutnya adalah mengonkretkan klasifikasi tersebut ke dalam model sintesis yang umum dipakai dalam eksperimen image restoration dan computational imaging. Pemilihan model ini menjadi fondasi utama dalam mendesain pipeline restorasi dan mengevaluasi performa algoritma secara kritis.

Untuk kasus *Gaussian Noise*, proses degradasi dimodelkan sebagai penjumlahan variabel acak terhadap citra asli:
```
n ~ N(0, σ²)
y = x + n
```
Variabel $n$ mewakili gangguan statistik yang berdistribusi normal dengan mean nol dan varians $\sigma^2$, sedangkan $y$ adalah hasil observasi terdegradasi. Dalam praktiknya, model ini secara akurat merepresentasikan noise elektronik pada rangkaian penguat sensor atau noise termal yang meningkat seiring kenaikan suhu operasi perangkat pencitraan.

Pada *Gaussian Blur*, degradasi terjadi melalui operasi konvolusi spasial yang meredam detail halus:
```
kernel k = Gaussian(sigma)
y = k ⊛ x + n
```
Kernel $k$ dibentuk berdasarkan fungsi Gaussian dengan parameter $\sigma$ yang mengontrol tingkat penyebaran. Operasi konvolusi ($\circledast$) ini mensimulasikan fenomena optik seperti defokus lensa atau hamburan cahaya di atmosfer. Penambahan $n$ setelah konvolusi wajib dilakukan karena dalam kondisi nyata, degradasi spasial hampir selalu diikuti oleh gangguan statistik residual.

Model *Downsampling* menggabungkan reduksi resolusi dengan mitigasi aliasing:
```
y = (x ⊛ k) ↓_s + n
```
Simbol $\downarrow_s$ menandakan operasi *decimation* dengan faktor skala $s$, yang didahului oleh pra-filtering menggunakan kernel $k$ untuk mencegah artefak frekuensi tinggi. Model ini sangat relevan untuk mensimulasikan citra dari sensor beresolusi terbatas, sistem *computational imaging*, atau protokol transmisi bandwidth rendah.

Poin kritis pada level penelitian doktoral terletak pada pernyataan bahwa memilih parameter degradasi realistis adalah keputusan penelitian, bukan sekadar teknis. Nilai $\sigma$, ukuran kernel, atau faktor skaling harus diturunkan dari analisis distribusi fisika pencitraan target, karakterisasi hardware, atau benchmark publik yang terverifikasi. Penetapan parameter yang sembarangan dapat menghasilkan bias evaluasi yang masif dan menutup celah kontribusi metodologis yang sebenarnya bisa dieksplorasi.

Pemahaman mendalam terhadap formulasi sintetis ini membawa kita langsung ke pertanyaan fundamental mengapa pemulihan citra secara matematis sulit dijamin stabilnya. Slide berikutnya akan membahas sifat *ill-posed* pada image restoration, menguraikan tiga kondisi ketidakstabilan solusi, serta memberikan ilustrasi mengapa banyak konfigurasi $x$ berbeda dapat menghasilkan observasi $y$ yang identik.

---

## Slide 010 - Mengapa Image Restoration Bersifat Ill-posed

### Narasi

Setelah membahas model degradasi sintetik pada slide sebelumnya, kita kini menyoroti pertanyaan mendasar dalam restorasi citra: mengapa masalah ini secara matematis bersifat *ill-posed* atau tidak terdefinisi dengan baik. Pemahaman konsep ini menjadi prasyarat kritis sebelum merancang metodologi eksperimen di tingkat doktoral, karena menentukan batasan apa yang bisa dan tidak bisa dicapai oleh suatu algoritma.

Secara formal, sebuah masalah dikategorikan sebagai *ill-posed* ketika salah satu dari tiga kondisi berikut tidak terpenuhi:
- Solusi tidak selalu ada untuk setiap data observasi.
- Solusi tidak unik, artinya terdapat lebih dari satu citra bersih yang valid.
- Solusi tidak bergantung secara kontinu pada data, sehingga gangguan kecil pada input menyebabkan perubahan drastis pada keluaran.

Dalam praktik restorasi citra, ketiga kondisi ini hampir selalu dilanggar. Pada tugas seperti *super-resolution*, jumlah piksel observasi lebih sedikit daripada variabel tak diketahui, menjadikan sistem persamaan *underdetermined*. Proses *blur* secara inheren menghapus komponen frekuensi tinggi, sehingga informasi halus hilang permanen dari domain observasi. Keberadaan *noise* selanjutnya memperparah keadaan dengan menciptakan lanskap kesalahan rekonstruksi yang datar, di mana banyak kandidat solusi memiliki performa yang hampir identik terhadap data terdegradasi.

Ilustrasi pada slide ini merepresentasikan ketidakunikan solusi tersebut:
```
x1 --> y
x2 --> y   dengan x1 != x2
```
Dua citra bersih yang berbeda, $x_1$ dan $x_2$, dapat melalui transformasi degradasi yang sama persis hingga menghasilkan observasi $y$. Tanpa informasi tambahan, sistem inversi murni tidak memiliki mekanisme diskriminatif untuk memilih solusi yang sebenarnya.

Kondisi ini langsung mengarah pada konsekuensi algoritmik yang akan kita bedah pada slide berikutnya. Pendekatan inversi sederhana cenderung memperkuat *noise*, menghasilkan estimasi yang sangat sensitif terhadap fluktuasi data, dan membiarkan ruang pencarian solusi yang terlalu luas. Implikasi penelitiannya jelas: setiap metode restorasi, termasuk arsitektur *deep learning*, secara eksplisit atau implisit memperkenalkan *prior* atau asumsi struktural tentang citra bersih. Validasi klaim "pemulihan detail" harus selalu mempertanyakan sumber detail tersebut—apakah berasal dari redundansi dalam data observasi, atau justru merupakan artefak dari *prior* model yang dikenal sebagai *hallucinated detail*.

---

## Slide 011 - Konsekuensi Ill-posedness

### Narasi

Setelah memahami mengapa image restoration secara inheren bersifat ill-posed pada slide sebelumnya, kita sekarang perlu menelaah konsekuensi langsung dari kondisi matematis tersebut. Ketika masalah inversi tidak memenuhi syarat well-poseded, implikasinya sangat nyata dalam praktik pemrosesan citra. Tanpa adanya asumsi atau batasan tambahan, penerapan inverse filter yang naif akan secara drastis memperbesar komponen noise yang terdapat pada citra teramati. Hal ini terjadi karena operator invers sering kali memiliki gain yang sangat besar pada frekuensi tinggi, di mana sinyal asli sudah melemah namun noise justru mendominasi.

Selain itu, solusi yang dihasilkan menjadi sangat tidak stabil. Perubahan kecil pada input observasi, misalnya akibat kompresi atau sensor noise, dapat menghasilkan variasi besar pada hasil rekonstruksi. Akibatnya, terdapat banyak kemungkinan citra bersih yang sama-sama konsisten dengan data teramati. Tidak ada satu pun solusi yang secara mutlak benar hanya berdasarkan informasi dari $y$.

Dari perspektif penelitian tingkat doktoral, konsekuensi ini membuka pertanyaan kritis tentang validitas klaim metodologis. Setiap algoritma restoration, baik berbasis model klasik maupun deep learning, sebenarnya menyuntikkan prior atau asumsi tertentu tentang struktur citra alami. Prior ini bisa berupa asumsi kelancaran, sparsity pada domain transform, atau bahkan distribusi statistik yang dipelajari dari dataset besar. Ketika sebuah metode mengklaim mampu memulihkan detail halus, kita harus secara eksplisit menguji apakah detail tersebut memang terkandung dalam data observasi, atau justru merupakan produk sampingan dari prior model. Fenomena inilah yang dalam literatur modern dikenal sebagai *hallucinated detail*—rekonstruksi fitur yang tampak realistis secara visual tetapi tidak memiliki korespondensi fisik dengan objek asli.

Pemahaman tentang konsekuensi ill-posedness ini menjadi fondasi penting sebelum kita membahas bagaimana para peneliti dan praktisi mengatasi tantangan tersebut. Pada slide berikutnya, kita akan melihat dua kerangka kerja fundamental untuk menstabilkan masalah inversi: regularisasi deterministik dan pendekatan probabilistik Bayesian. Keduanya pada dasarnya adalah cara formal untuk memasukkan prior ke dalam formulasi optimasi, sehingga solusi yang dihasilkan tidak lagi ambigu atau tidak stabil.

---

## Slide 012 - Pendekatan Dasar: Regularisasi dan Bayesian

### Narasi

Pada slide sebelumnya, kita telah membahas konsekuensi mendasar dari sifat ill-posed dalam masalah inversi citra. Tanpa asumsi tambahan, penerapan inverse filter sederhana akan secara drastis memperkuat noise, menghasilkan solusi yang tidak stabil di mana perubahan kecil pada observasi $y$ menyebabkan fluktuasi besar pada estimasi $x\_hat$, serta memungkinkan banyak kandidat solusi yang sama-sama memenuhi batasan data. Implikasi kritisnya adalah setiap metode restorasi pada hakikatnya membawa prior atau asumsi implisit mengenai struktur citra bersih. Klaim bahwa suatu metode berhasil memulihkan detail harus selalu diuji empiris: apakah detail tersebut benar-benar terkandung dalam data observasi, atau hanya produk sampingan dari prior model. Ketegangan inilah yang melahirkan konsep *hallucinated detail*.

Untuk menstabilkan solusi dan mengarahkannya ke ruang yang bermakna secara visual, pendekatan formal dimulai dengan formulasi regularisasi. Persamaan pada slide mendefinisikan estimasi sebagai hasil minimisasi fungsi objektif yang terdiri dari dua suku utama. Suku pertama, $\| y - H x \|^2$, merepresentasikan *data fidelity*, yang mengukur kesesuaian fisik antara pengamatan $y$ dan rekonstruksi model forward $H x$. Suku kedua, $\lambda R(x)$, berfungsi sebagai regularisasi atau prior matematis yang membatasi ruang pencarian solusi agar sesuai dengan karakteristik citra alami. Parameter skalar $\lambda$ bertindak sebagai tuas penyetel keseimbangan (*trade-off*) antara akurasi pencocokan data dan kekuatan prior. Dalam konteks penelitian tingkat doktor, memilih bentuk $R(x)$ bukan sekadar langkah numerik, melainkan pernyataan eksplisit tentang hipotesis Anda mengenai properti statistik atau geometris citra target.

Secara teoretis, kerangka regularisasi memiliki ekuivalensi langsung dengan interpretasi Bayesian melalui estimasi Maximum A Posteriori (MAP). Distribusi posterior $p(x | y)$ sebanding dengan perkalian likelihood $p(y | x)$ dan prior $p(x)$. Likelihood merepresentasikan model noise yang menghubungkan citra bersih dengan pengamatan, sementara prior mendeskripsikan keyakinan awal kita tentang distribusi citra alami. Ketika kita mengambil logaritma negatif dari posterior dan mengabaikan konstanta normalisasi, turunan aljabar secara persis mengembalikan formulasi regularisasi $\arg\min_x \| y - H x \|^2 + \lambda R(x)$. Prior klasik seperti Tikhonov berkorespondensi dengan asumsi Gaussian pada koefisien citra, Total Variation mengimplikasikan distribusi Laplace pada gradien (menghasilkan edge-preserving smoothing), dan sparsity sering dikaitkan dengan prior heavy-tailed. Memahami jembatan probabilistik ini penting karena memungkinkan peneliti merancang prior berbasis distribusi yang lebih fleksibel daripada fungsi matematis statis.

Namun, keterbatasan prior handcrafted matematis mulai terlihat jelas ketika dihadapkan pada kompleksitas citra dunia nyata yang memiliki tekstur multi-skala, struktur semantik tinggi, dan degradasi non-linear. Hal ini membawa kita secara alami ke pergeseran paradigma yang akan dibahas pada slide berikutnya: transisi dari prior yang dirancang manual menuju prior yang dipelajari secara otomatis (*learned prior*). Arsitektur deep learning dan model generatif modern memungkinkan estimasi $p(x)$ langsung dari data skala besar, menggantikan fungsi matematis kaku dengan representasi dinamis yang jauh lebih ekspresif. Penting untuk dicatat bahwa dalam riset mutakhir, learned prior berfungsi ganda: ia merupakan sumber pengetahuan komputasi yang sangat kuat, namun sekaligus berpotensi menjadi sumber bias sistematis jika tidak dievaluasi dengan rigor metodologis. Kita akan membedah mekanisme kerja deep restoration, perbedaan mapping langsung versus deep prior, serta bagaimana prior terpelajar ini mengubah lanskap evaluasi state-of-the-art.

---

## Slide 013 - Dari Handcrafted Prior ke Learned Prior

### Narasi

Pada slide sebelumnya, kita telah menelaah formulasi dasar image restoration melalui lensa regularisasi dan inferensi Bayesian. Di sana, solusi rekonstruksi diperoleh dengan meminimalkan fungsi objektif yang menggabungkan *data fidelity* dan *prior* matematis eksplisit seperti Total Variation atau Tikhonov. Meskipun pendekatan ini memberikan jaminan teoretis dan stabilitas optimisasi, representasi prior yang kaku sering kali gagal menangkap kompleksitas statistik citra alami secara memadai.

Slide ini memperkenalkan pergeseran paradigma dari *handcrafted prior* menuju *learned prior*. Jika pada era klasik prior dirumuskan secara analitis, era *deep learning* memungkinkan prior diekstraksi langsung dari data. Tabel pada slide ini merangkum evolusi tiga pendekatan utama: pertama, *klasik* yang mengandalkan persamaan matematis dengan keunggulan interpretabilitas namun terbatas dalam ekspresivitas; kedua, *deep learning* yang memanfaatkan jaringan saraf untuk mempelajari prior secara implisit, menghasilkan rekonstruksi yang jauh lebih natural namun rentan terhadap bias domain dan kebutuhan data masif; ketiga, *generative* yang memodelkan distribusi citra penuh melalui mekanisme seperti *diffusion*, menawarkan fleksibilitas multimodal tertinggi namun menuntut biaya komputasi signifikan dan memerlukan kontrol sampling ketat untuk mencegah halusinasi struktur.

Implementasi *deep restoration* secara praktis terbagi ke dalam tiga skema arsitektural:
1. **Mapping langsung**: Jaringan dilatih end-to-end untuk memetakan citra terdegradasi $y$ ke hasil bersih $\hat{x}$ melalui fungsi parametrik $\hat{x} = f_\theta(y)$. Pendekatan ini efisien dan cepat, namun bergantung sepenuhnya pada kualitas pasangan data latih.
2. **Deep prior**: Arsitektur jaringan tidak digunakan sebagai pemetaian statis, melainkan diintegrasikan sebagai operator regularisasi dalam loop optimisasi iteratif. Network bertindak sebagai *implicit prior* yang menyesuaikan diri dengan setiap input spesifik selama proses inversi.
3. **Generative prior**: Restorasi diformulasikan sebagai proses pengambilan sampel dari distribusi kondisional $p(x|y)$. Model tidak hanya mengembalikan satu solusi deterministik, tetapi mengeksplorasi manifold citra yang valid secara statistik sesuai dengan degradasi yang diamati.

Poin kritis yang harus selalu diingat dalam konteks penelitian tingkat doktoral adalah bahwa *learned prior* bersifat dualistis: ia merupakan sumber pengetahuan visual yang powerful sekaligus sumber bias struktural. Karena prior ini dikondisikan oleh distribusi data pelatihan, generalisasi model akan menurun drastis jika terjadi *domain shift* atau degradasi yang tidak terwakili dalam data latih. Oleh karena itu, evaluasi model tidak boleh hanya berpatokan pada metrik numerik seperti PSNR atau SSIM, tetapi harus mencakup analisis stabilitas prior, keberagaman hasil, dan mitigasi bias representasional.

Transisi konseptual dari definisi prior ini secara alami mengarah pada implementasi teknisnya. Pada slide berikutnya, kita akan membedah arsitektur-arsitektur kunci yang menjadi tulang punggung *deep restoration* modern, mulai dari residual CNN, encoder-decoder berbasis skip connection, transformer non-local, hingga backbone diffusion, beserta elemen desain umum yang telah menjadi standar de facto dalam state-of-the-art.

---

## Slide 014 - Arsitektur Kunci Deep Restoration

### Narasi

Pada slide sebelumnya, kita telah membahas pergeseran paradigma dari *handcrafted prior* menuju *learned prior*, serta tiga cara kerja utama *deep restoration*: pemetaan langsung, regularisasi berbasis jaringan, dan sampling dari distribusi generatif. Langkah selanjutnya adalah membedah arsitektur jaringan yang secara spesifik dirancang untuk mengimplementasikan prior-prior tersebut secara efisien dan akurat.

Tabel pada slide ini mengelompokkan lima pola arsitektur kunci yang mendominasi riset restorasi citra terkini, masing-masing memiliki karakteristik dan trade-off tersendiri:

- **CNN dangkal (SRCNN-style)**: Struktur sederhana dengan jumlah parameter terbatas. Sangat cepat dalam inferensi dan mudah diimplementasikan, namun kapasitas representasinya sering kali kurang memadai untuk menangani degradasi kompleks atau noise tingkat tinggi.
- **Residual CNN (DnCNN-style)**: Tidak lagi memetakan citra terdegradasi ke citra bersih secara langsung, melainkan fokus mempelajari residual atau komponen noise. Pendekatan ini menstabilkan gradien selama pelatihan dan mempercepat konvergensi, menjadikannya baseline yang kuat untuk tugas denoising dan super-resolusi.
- **Encoder-decoder (U-Net)**: Menggabungkan jalur penyempitan (*bottleneck*) dengan jalur perluasan menggunakan *skip connection*. Mekanisme ini memungkinkan integrasi fitur halus dan detail spasial dari encoder ke decoder, sehingga sangat efektif untuk tugas yang memerlukan preservasi struktur geometri dan multi-scale degradation handling.
- **Transformer (SwinIR, Restormer)**: Memanfaatkan mekanisme *self-attention* untuk menangkap dependensi non-lokal antar-pixel atau region yang berjauhan. Receptive field yang bersifat global memungkinkan model membedakan tekstur berulang dari noise atau artefak dengan lebih akurat, meski menuntut manajemen memori yang cermat.
- **Diffusion backbone (UNet + attention)**: Menjadi tulang punggung model generatif modern. Prior citra dipelajari melalui proses denoising bertahap yang sangat ekspresif dan mampu menghasilkan detail realistis, namun memerlukan komputasi iteratif yang mahal dan berpotensi menimbulkan halusinasi jika kontrol kondisional tidak ketat.

Meskipun kerangka arsitekturnya berbeda, hampir semua model restorasi mutakhir berbagi empat elemen desain umum yang menjadi fondasi stabilitas dan performa:

- **Residual learning**: Tetap menjadi strategi inti untuk mencegah vanishing gradient dan memungkinkan jaringan fokus pada kesalahan restorasi, bukan replikasi sinyal input.
- **Normalization layers**: Batch normalization, Layer normalization, atau Group normalization digunakan untuk menstabilkan distribusi aktivasi, mempercepat pelatihan, dan meningkatkan generalisasi lintas dataset.
- **Perceptual loss / Adversarial loss**: Menambahkan regulasi berbasis fitur atau diskriminator agar hasil restorasi tidak hanya meminimalkan MSE/MAE, tetapi juga memenuhi persepsi visual manusia yang lebih natural.
- **Multi-scale feature extraction**: Ekstraksi fitur pada berbagai resolusi memungkinkan model menangani degradasi heterogen dan preservasi detail frekuensi tinggi secara simultan.

Poin kritis yang perlu ditekankan pada level doktoral adalah bahwa pemilihan arsitektur tidak boleh bersifat dogmatis atau sekadar mengikuti tren publikasi. Kesesuaian antara struktur jaringan, karakteristik degradasi, dan batasan komputasi harus diturunkan secara eksplisit dari formulasi masalah penelitian. Sebagaimana dicatat dalam Pertemuan 03, arsitektur harus disesuaikan dengan domain aplikasi, bukan sebaliknya. Pada slide berikutnya, kita akan mengevaluasi secara mendalam bagaimana mekanisme attention dan transformer diintegrasikan ke dalam pipeline restorasi, serta mengapa hal ini menjadi solusi strategis untuk mengatasi keterbatasan receptive field lokal pada arsitektur CNN konvensional.

---

## Slide 015 - Attention dan Transformer dalam Restoration

### Narasi

Pada slide sebelumnya kita telah menelaah berbagai arsitektur inti yang menjadi tulang punggung metode restorasi berbasis deep learning, mulai dari CNN dangkal, residual CNN, encoder-decoder U-Net, hingga backbone transformer seperti SwinIR dan Restormer. Fokus kita kali ini adalah memahami bagaimana mekanisme attention dan transformer secara spesifik berkontribusi dalam meningkatkan kualitas restorasi citra terdegradasi.

Konsep fundamentalnya terletak pada self-attention yang mampu mempelajari hubungan jangka panjang antar-pixel atau region yang letaknya berjauhan. Berbeda dengan operasi konvolusi lokal yang terbatas pada kernel tetap, self-attention memberikan kemampuan untuk mengaitkan informasi dari seluruh area citra secara simultan. Hal ini sangat krusial karena pada citra yang terdegradasi, konteks global sering kali menjadi pembeda utama antara struktur tekstur asli dan artefak noise.

Relevansi pendekatan ini semakin jelas ketika kita melihat karakteristik degradasi itu sendiri. Banyak jenis blur, seperti motion blur, bersifat non-lokal dan memerlukan pemahaman konteks spasial yang luas untuk dipulihkan. Selain itu, pola tekstur berulang dapat direkonstruksi lebih akurat jika model memiliki akses ke informasi kontekstual di luar jendela lokal. Transformer menawarkan receptive field global tanpa harus menumpuk lapisan konvolusi yang sangat dalam, sehingga menghindari masalah representasi yang terfragmentasi sekaligus mempertahankan integritas sinyal jarak jauh.

Namun, implementasi transformer dalam restorasi tidak lepas dari tantangan komputasi. Kompleksitas kuadratik terhadap jumlah token masih menjadi hambatan utama, terutama pada resolusi tinggi atau batch size besar. Oleh karena itu, desain efisien seperti windowed attention, linear attention, atau arsitektur hibrida menjadi kebutuhan praktis. Seperti yang pernah ditekankan pada Pertemuan 03, pemilihan arsitektur harus selalu disesuaikan dengan karakteristik domain dan batasan komputasi, bukan sekadar mengikuti tren metodologi. Pretraining pada dataset besar dan strategi augmentasi tetap menjadi fondasi penting untuk mencapai generalisasi yang robust.

Dengan pemahaman tentang bagaimana attention dan transformer mengubah paradigma restorasi, langkah logis berikutnya adalah melihat bagaimana prinsip-prinsip ini berevolusi dalam tugas super-resolusi. Kita akan menguji pergeseran dari metode interpolasi klasik menuju model generatif dan diffusion yang mampu menghasilkan detail tekstural alami, meskipun dengan trade-off fidelity dan biaya komputasi yang berbeda.

---

## Slide 016 - Super-Resolution: Dari Interpolasi ke Generative Models

### Narasi

Setelah membahas bagaimana mekanisme attention dan transformer mampu menangkap konteks global untuk restorasi citra pada slide sebelumnya, kita kini beralih ke salah satu aplikasi paling konkret dan fundamental dari image restoration, yaitu super-resolution. Topik ini bukan sekadar meningkatkan dimensi piksel, melainkan merekonstruksi informasi spasial dan semantik yang hilang akibat degradasi optik, keterbatasan sensor, atau proses kompresi.

Pendekatan klasik seperti nearest neighbor, bilinear, dan bicubic masih menjadi baseline industri karena kecepatannya dan stabilitas komputasinya yang sangat tinggi. Namun, metode ini secara inheren bersifat interpolatif. Mereka hanya memetakan nilai intensitas piksel tetangga tanpa memahami struktur atau semantik gambar, sehingga hasil akhir cenderung halus secara artifisial dan kehilangan detail tekstur yang kompleks.

Dengan hadirnya deep learning, paradigma super-resolution bergeser dari interpolasi geometris menjadi pembelajaran pemetaan non-linier antara ruang low-resolution dan high-resolution. Arsitektur berbasis CNN dan residual learning mampu menghasilkan detail yang jauh lebih tajam dan terstruktur. Akan tetapi, pada skala pembesaran yang besar, model deterministik ini sering kali terjebak dalam optimasi loss berbasis pixel-wise seperti MSE atau MAE, yang berujung pada halus berlebihan atau bahkan halusinasi struktur yang tidak sesuai dengan konten asli.

Di sinilah model generatif, khususnya diffusion models, menawarkan langkah evolusioner. Alih-alih memprediksi satu nilai piksel pasti, diffusion SR melakukan sampling bertahap dari distribusi prior high-resolution yang dikondisikan oleh input low-resolution. Pendekatan ini memungkinkan generasi detail tekstur yang sangat natural dan beragam, meskipun konsekuensinya adalah munculnya banyak solusi yang secara statistik plausible namun mungkin berbeda dari ground truth.

Tabel perbandingan pada slide ini merangkum trade-off mendasar di antara ketiga era tersebut:
- **Bicubic**: menawarkan biaya komputasi yang sangat rendah, namun fidelitas dan kualitas perseptualnya terbatas.
- **CNN SR**: memberikan fidelitas tinggi terhadap ground truth dengan biaya yang masih terjangkau, namun kualitas perseptualnya berada di posisi menengah.
- **Diffusion SR**: menggeser keseimbangan secara signifikan; fidelitas relatif sedang karena sifat probabilistiknya, namun kualitas perseptualnya sangat tinggi, meski dibayar dengan biaya komputasi dan waktu inference yang jauh lebih besar.

Transisi ini membawa kita langsung ke diskusi kritis pada slide berikutnya mengenai ketegangan antara fidelity dan perceptual quality. Untuk penelitian tingkat doktoral, penting bagi Anda tidak hanya memilih arsitektur berdasarkan tren, tetapi juga secara eksplisit memposisikan kontribusi metodologis Anda pada spektrum trade-off tersebut. Evaluasi harus dilakukan menggunakan metrik ganda yang saling melengkapi, serta disertai analisis ablation yang jelas mengapa pendekatan tertentu dipilih untuk kasus penggunaan spesifik.

---

## Slide 017 - Fidelity vs Perceptual Quality

### Narasi

Slide ini menyoroti dua dimensi evaluasi yang menjadi inti dari hampir semua tugas restorasi citra dan komputasi pencitraan: *fidelity* dan *perceptual quality*. 

**Fidelity** mengkuantifikasi seberapa dekat hasil rekonstruksi dengan *ground truth* pada tingkat piksel individual. Ini merupakan ukuran objektif yang bersifat deterministik dan berbasis kesalahan numerik. Di sisi lain, **perceptual quality** menilai seberapa alami atau nyaman hasil tersebut dilihat oleh sistem visual manusia, yang sangat dipengaruhi oleh karakteristik psikofisik dan preferensi perseptual.

Kedua dimensi ini secara inheren sering kali saling bertolak belakang. Ketika fungsi kerugian (*loss function*) model dioptimalkan untuk memaksimalkan fidelitas—biasanya melalui minimisasi MSE atau maksimisasi PSNR—hasil akhir cenderung mengalami *over-smoothing*. Tekstur halus dan varians lokal tertekan demi kesamaan statistik global. Sebaliknya, optimasi berbasis *perceptual loss* atau arsitektur adversarial seperti GAN didorong untuk menghasilkan detail yang tajam dan struktur tekstur yang natural, namun sering kali mengorbankan kecocokan pixel-wise dengan referensi asli, sehingga berpotensi menimbulkan artefak atau ketidaksesuaian semantik.

Diagram konseptual pada slide merepresentasikan fenomena ini sebagai sebuah spektrum kontinu. Di satu ujung terdapat citra dengan fidelitas tinggi namun kehilangan karakter visual (*over-smooth*), sementara di ujung lainnya terdapat citra dengan kualitas perseptual tinggi yang kaya detail natural. Titik optimal terletak pada manajemen *trade-off* yang disengaja, bukan pencarian solusi mutlak di salah satu sisi.

Untuk konteks penelitian tingkat doktoral, klaim kinerja metode tidak dapat lagi bergantung pada single-metric benchmarking. Anda wajib secara eksplisit memposisikan kontribusi riset Anda pada spektrum *trade-off* ini. Validasi harus dirancang menggunakan metrik ganda: metrik struktural/fidelitas (seperti PSNR, SSIM, atau MS-SSIM) untuk menjamin konsistensi geometris, dan metrik perseptual (seperti LPIPS, BRISQUE, atau FRID) bersama evaluasi *human subjective study* untuk membuktikan kealamian visual. Pendekatan dual-evaluation ini menjadi standar kritis dalam membedakan karya yang sekadar meningkatkan skor benchmark versus karya yang benar-benar advances dalam representasi visual.

Pembahasan mengenai dilema evaluasi ini menjadi fondasi langsung menuju slide berikutnya, di mana kita akan membedah secara teknis definisi matematis PSNR, mekanisme perhitungannya, serta keterbatasan mendasarnya yang mengharuskan peneliti untuk melengkapinya dengan metrik modern yang lebih selaras dengan persepsi manusia.

---

## Slide 018 - PSNR: Definisi dan Keterbatasan

### Narasi

Pada slide ini kita akan mengupas secara kritis satu metrik yang menjadi standar de facto dalam evaluasi *image restoration* dan *computational imaging*, yaitu PSNR atau Peak Signal-to-Noise Ratio. Bagi peneliti tingkat doktoral, memahami batasan matematis dan persepsional dari metrik ini adalah prasyarat sebelum merancang protokol evaluasi yang valid dan reproducible.

Secara definisi, PSNR diturunkan langsung dari Mean Squared Error (MSE). Rumus MSE menghitung rata-rata kuadrat selisih antara piksel asli $x_i$ dan piksel hasil rekonstruksi $\hat{x}_i$, dibagi dengan jumlah total piksel $N$. Nilai MSE kemudian dikonversi ke skala logaritmik melalui rumus PSNR = $10 \log_{10}(\text{MAX}^2 / \text{MSE})$, di mana MAX merepresentasikan rentang dinamis nilai piksel, misalnya 255 untuk citra 8-bit atau 1.0 untuk data yang sudah dinormalisasi. Keunggulan utamanya terletak pada kesederhanaan komputasi, sifatnya yang deterministik, dan kemudahan interpretasi numerik, sehingga sangat lazim dijadikan baseline dalam publikasi.

Namun, sebagai peneliti S3, kita harus menyoroti keterbatasan fundamental dari pendekatan ini. Pertama, korelasi antara skor PSNR tinggi dan kualitas visual menurut manusia sering kali lemah. Optimasi untuk memaksimalkan PSNR cenderung mendorong model menghasilkan citra yang terlalu halus (*over-smoothed*), karena algoritma berfokus pada meminimalkan kesalahan kuadrat secara merata alih-alih mempertahankan detail tepi atau tekstur kompleks. Kedua, karena menggunakan rata-rata global, PSNR tidak mampu melokalisasi di mana kesalahan terjadi. Artefak lokal yang signifikan bisa tertutup oleh koreksi piksel massal di area homogen, padahal secara perseptual kerusakan tersebut sangat mengganggu. Ketiga, metrik ini buta terhadap struktur spasial, kontras lokal, dan karakteristik frekuensi yang justru menjadi inti dari tugas restorasi citra.

Kesimpulan metodologisnya jelas: PSNR tidak perlu dihapus dari pipeline evaluasi, melainkan harus diposisikan sebagai metrik pelengkap. Dalam desain penelitian Anda, gunakan PSNR untuk memantau stabilitas numerik, konvergensi loss function, dan reproduktibilitas eksperimen, tetapi wajib dikombinasikan dengan metrik berbasis struktur atau persepsi untuk klaim kontribusi ilmiah yang kuat. Pendekatan ini secara langsung menjawab tantangan *trade-off* antara fidelitas dan kualitas perseptual yang telah kita bahas pada slide sebelumnya.

Sebagai kelanjutan logis, ketika kita mengakui bahwa MSE dan PSNR gagal menangkap kesamaan struktural antar citra, komunitas riset mengembangkan indeks yang beroperasi pada skala lokal. Slide selanjutnya akan membahas SSIM atau Structural Similarity Index, yang secara eksplisit memodelkan komponen luminansi, kontras, dan struktur dalam jendela bergerak, menawarkan korelasi yang lebih dekat dengan persepsi manusia tanpa menghilangkan kebutuhan akan metrik berbasis piksel.

---

## Slide 019 - SSIM: Structural Similarity

### Narasi

Setelah membahas PSNR pada slide sebelumnya dan menggarisbawahi keterbatasannya dalam menangkap persepsi visual manusia melalui rata-rata kesalahan global, kita beralih ke SSIM atau Structural Similarity Index. Metrik ini dirancang khusus untuk memodelkan bagaimana sistem penglihatan manusia mengevaluasi kualitas citra dengan membandingkan tiga komponen secara lokal: luminositas, kontras, dan struktur.

Secara konseptual, SSIM dirumuskan sebagai fungsi gabungan dari ketiga komponen tersebut. Berbeda dengan pendekatan pixel-wise, perhitungan dilakukan menggunakan jendela geser lokal yang bergerak melintasi seluruh gambar. Nilai indeks yang dihasilkan berkisar antara -1 hingga 1, di mana nilai mendekati 1 menandakan kemiripan struktural yang tinggi. Pendekatan berbasis jendela ini memungkinkan SSIM mendeteksi degradasi spasial yang sering kali terlewat oleh metrik berbasis MSE.

Kelebihan utama SSIM terletak pada korelasinya yang jauh lebih kuat dengan penilaian subjektif manusia dibandingkan PSNR. Metrik ini sangat sensitif terhadap perubahan tepi objek, distorsi geometri, dan variasi kontras lokal, sehingga menjadi standar evaluasi yang lebih andal dalam tugas kompresi gambar dan algoritma restorasi berbasis filter klasik.

Namun, sebagai peneliti tingkat doktoral, kita harus kritis terhadap batasannya. SSIM masih memiliki kelemahan dalam menangkap tekstur kompleks dan detail frekuensi tinggi yang bersifat semantik. Selain itu, hasil yang terlalu halus atau over-smoothed dapat menghasilkan skor SSIM yang menyesatkan karena mempertahankan struktur makro dengan baik. Untuk menjamin reproduktibilitas penelitian, laporan eksperimen wajib mencantumkan nilai `data_range` dan ukuran jendela yang digunakan, mengingat kedua parameter ini secara signifikan mengubah distribusi skor akhir. Implementasi praktisnya akan kita uji langsung pada sesi praktikum.

Meskipun SSIM merepresentasikan lompatan signifikan dari metrik berbasis intensitas murni, tantangan evaluasi perseptual belum sepenuhnya teratasi. Kesenjangan antara skor numerik dan persepsi manusia semakin menyempit ketika kita memanfaatkan representasi fitur dari jaringan saraf tiruan. Pada slide berikutnya, kita akan mengupas metrik perseptual modern seperti LPIPS, DISTS, FID, serta pendekatan no-reference yang mengandalkan pembelajaran mendalam dan statistik scene alami untuk menutup celah evaluasi tersebut.

---

## Slide 020 - Perceptual Metrics Modern

### Narasi

Setelah membahas SSIM pada slide sebelumnya yang menekankan kesamaan struktur lokal namun masih memiliki keterbatasan dalam menangkap tekstur tingkat lanjut serta cenderung memprioritaskan hasil yang *over-smoothed*, kita beralih ke metrik perseptual modern. Pada level riset doktoral, evaluasi kualitas restorasi tidak lagi hanya bergantung pada MSE atau SSIM, melainkan memerlukan pendekatan yang lebih selaras dengan persepsi manusia dan representasi fitur mendalam.

Tabel pada slide ini merangkum lima metrik utama yang kini menjadi standar dalam literatur *image restoration* dan *computational imaging*:
- LPIPS dan DISTS merupakan metrik berbasis pembelajaran (*learned*) yang mengukur jarak fitur dari jaringan saraf tiruan yang telah dilatih sebelumnya. LPIPS berfokus pada kemiripan perseptual global, sedangkan DISTS mengevaluasi fidelitas struktur dan tekstur secara simultan. Keduanya sangat sensitif terhadap perubahan semantik dan sering kali berkorelasi lebih baik dengan penilaian manusia dibandingkan metrik piksel-per-piksel.
- FID (*Fréchet Inception Distance*) bersifat distribusi-based dan digunakan untuk evaluasi tingkat kumpulan data. Metrik ini membandingkan distribusi statistik fitur dari gambar asli dan gambar hasil restorasi, sehingga ideal untuk menilai konsistensi dan keberagaman hasil pada benchmark besar.
- Di sisi no-reference, NIQE mengandalkan statistik citra alamiah (*natural scene statistics*) untuk mendeteksi penyimpangan tanpa memerlukan *ground truth*. Sementara itu, MUSIQ dan NIMA menggunakan model terlatih untuk memprediksi skor kualitas berdasarkan opini manusia, cocok untuk aplikasi yang mengutamakan pengalaman pengguna akhir.

Perlu diperhatikan bahwa kinerja metrik *learned* sangat bergantung pada arsitektur dan domain pelatihan jaringan dasar. Jika representasi fitur tidak sesuai dengan karakteristik data Anda, skor metrik dapat menyesatkan. Selain itu, pembedaan antara evaluasi per-citra dan per-kumpulan harus selalu dipertahankan dalam metodologi penelitian. Pemilihan metrik harus disesuaikan dengan tujuan aplikasi spesifik, apakah fokus pada fidelitas piksel, konsistensi distribusi, atau penilaian subjektif.

Pemahaman tentang batasan metrik tradisional dan keunggulan metrik perseptual ini menjadi fondasi kritis sebelum memasuki studi kasus pada slide berikutnya. Kita akan melihat langsung bagaimana skor metrik konvensional seperti PSNR dan SSIM bisa bertentangan dengan kualitas visual yang dirasakan manusia, serta bagaimana integrasi metrik modern mengubah interpretasi hasil restorasi dalam tinjauan paper.

---

## Slide 021 - Studi Kasus: PSNR Tinggi vs Kualitas Visual

### Narasi

Slide ini menyajikan ketegangan fundamental dalam evaluasi *image restoration*: divergensi antara metrik numerik tradisional dan persepsi visual manusia. Skenario yang diberikan membandingkan dua pendekatan restorasi dengan hasil yang saling bertolak belakang. Metode A mencetak skor PSNR 31.2 dB dan SSIM 0.92, namun menghasilkan permukaan tekstur yang terasa kaku atau "plastik". Sebaliknya, Metode B mencatat nilai lebih rendah, yaitu PSNR 29.8 dB dan SSIM 0.89, tetapi secara visual menawarkan detail yang lebih alami dan tajam. Jika hanya membaca tabel metrik, kesimpulan awal akan secara mutlak mengarah pada keunggulan Metode A. Namun, preferensi subjektif dan pengamatan empiris sering kali menunjukkan pola yang berkebalikan.

Ketidaksesuaian ini terjadi karena PSNR dan SSIM mengukur kesamaan piksel atau struktur lokal berdasarkan asumsi statistik tertentu, yang tidak selalu mencerminkan cara sistem visual manusia memproses informasi kompleks. Di sinilah peran metrik perseptual modern yang telah dibahas pada slide sebelumnya menjadi krusial. Metrik seperti LPIPS mengukur jarak fitur pada jaringan saraf dalam yang telah dilatih untuk menangkap kemiripan semantik dan struktural, sehingga sering kali memberikan skor yang lebih selaras dengan penilaian manusia meskipun nilai PSNR-nya lebih rendah.

Untuk melatih kemampuan *critical review* pada level doktoral, mari kita bedah skenario ini melalui empat pertanyaan strategis:
1. Metode mana yang benar-benar memulihkan informasi asli, dan mana yang sekadar meminimalkan fungsi kerugian tanpa memperhatikan semantik gambar?
2. Detail apa yang sebenarnya terdapat pada *ground truth*, dan apakah variasi tersebut merupakan noise yang harus dihilangkan atau sinyal penting yang justru perlu dipertahankan?
3. Mengacu pada konteks aplikasi spesifik Anda, jenis kesalahan apa yang paling mahal secara operasional atau klinis?
4. Apakah penambahan metrik tambahan seperti LPIPS atau pelaksanaan *human study* terkontrol akan mengubah kesimpulan awal kita?

Pola analisis ini bukan sekadar latihan akademis, melainkan kerangka kerja standar saat mengevaluasi publikasi di bidang restorasi citra. Peneliti diharapkan tidak hanya bergantung pada angka, tetapi juga mengidentifikasi diskrepansi antara optimisasi numerik dan kualitas subjektif, serta merumuskan hipotesis yang kuat mengenai penyebabnya. Pemahaman ini menjadi landasan langsung untuk slide berikutnya, di mana kita akan menginvestigasi fenomena *hallucinated detail*. Kita akan membahas bagaimana model restorasi dapat menghasilkan struktur yang tampak meyakinkan namun tidak memiliki padanan di data asli, serta metodologi rigor untuk mendeteksi dan memitigasi risiko halusinasi tersebut dalam penelitian tingkat lanjut.

---

## Slide 022 - Hallucinated Detail: Memulihkan atau Menciptakan?

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana metrik kuantitatif seperti PSNR dan SSIM tidak selalu berkorelasi dengan persepsi visual manusia. Kasus studi tersebut menggarisbawahi pentingnya evaluasi kritis dalam penelitian restorasi citra. Melanjutkan poin itu, slide ini menyoroti fenomena yang lebih fundamental namun sering terabaikan: *hallucinated detail*.

*Hallucinated detail* merujuk pada struktur atau tekstur yang muncul pada hasil restorasi tanpa memiliki padanan eksak di citra asli, meskipun secara visual tampak meyakinkan. Dalam praktik modern, hal ini sangat umum terjadi. Model super-resolution sering kali menambahkan pori-pori kulit atau serat kain yang tidak ada pada data asli. Proses deblurring dapat menciptakan tepi tajam yang sebenarnya bukan bagian dari objek. Sementara itu, model berbasis diffusion cenderung menghasilkan tekstur baru yang sangat *plausible* secara statistik, tetapi sepenuhnya fiktif.

Fenomena ini berbahaya karena menyentuh aspek validitas ilmiah dan etika penerapan. Di bidang medis atau forensik, detail palsu dapat mengarah pada diagnosis yang salah atau interpretasi bukti yang keliru. Yang lebih kritis lagi, metrik tradisional seperti PSNR maupun SSIM sama sekali tidak mampu mendeteksi halusinasi ini, karena mereka hanya mengukur kesamaan piksel atau struktur global, bukan kebenaran semantik atau faktual dari konten yang dipulihkan.

Untuk menangani masalah ini, diperlukan protokol evaluasi yang lebih ketat sesuai standar riset tingkat doktor. Pertama, model harus melaporkan tingkat ketidakpastian (*uncertainty*) pada region yang dipulihkan. Kedua, inspeksi visual harus dilakukan secara sistematis pada area yang rentan terhadap artefak. Ketiga, dampak halusinasi harus dievaluasi melalui tugas hilir (*downstream task*), seperti akurasi klasifikasi atau segmentasi, untuk memastikan bahwa detail yang dihasilkan tidak mengganggu performa aplikasi nyata.

Pembahasan mengenai keandalan hasil restorasi ini membawa kita secara alami ke tantangan mendasar berikutnya: sumber data pelatihan. Jika model menghasilkan detail yang halusinatif, sering kali akar masalahnya terletak pada kesenjangan antara degradasi sintetis yang digunakan saat training dan degradasi nyata yang dihadapi saat inferensi. Slide selanjutnya akan membedah perbandingan komprehensif antara kedua jenis degradasi tersebut, serta strategi mitigasi seperti *blind restoration* dan pendekatan *self-supervised* yang relevan untuk menjawab pertanyaan penelitian tingkat lanjut.

---

## Slide 023 - Degradasi Sintetis vs Degradasi Nyata

### Narasi

Pada slide ini, kita membahas perbedaan fundamental antara degradasi sintetis dan degradasi nyata dalam konteks restorasi citra digital. Tabel di atas merangkum empat dimensi kritis yang memisahkan kedua pendekatan tersebut. Untuk degradasi sintetis, ground truth umumnya tersedia secara eksplisit, parameter kontrol dapat diatur sepenuhnya, reproduktibilitas eksperimen sangat tinggi, namun relevansi aplikatifnya cenderung terbatas. Sebaliknya, pada degradasi nyata, ground truth hampir mustahil diperoleh secara sempurna, parameter degradasi tidak diketahui, reproduktibilitas rendah, tetapi relevansi untuk implementasi dunia nyata justru menjadi prioritas utama.

Kesenjangan distribusi antara kedua domain ini menimbulkan masalah utama yang sering disebut sebagai domain gap. Model yang dilatih secara eksklusif menggunakan degradasi sintetis cenderung mengalami penurunan performa signifikan saat diujikan pada citra nyata. Penyebab utamanya adalah ketidaktahuan parameter degradasi seperti ukuran blur kernel, arah motion, atau distribusi noise saat tahap inferensi. Tanpa informasi ini, mekanisme restorasi model kehilangan referensi yang diperlukan untuk memetakan kembali ke ruang fitur citra bersih.

Untuk menutup kesenjangan tersebut, literatur terkini mengarahkan penelitian ke tiga jalur solusi utama. Pertama, blind restoration menekankan estimasi degradasi dan pemulihan citra bersih secara simultan, tanpa bergantung pada pengetahuan awal tentang proses degradasi. Kedua, pendekatan self-supervised atau zero-shot restoration memungkinkan adaptasi model langsung pada citra target tanpa memerlukan pasangan data bersih-degraded. Ketiga, teknik synthetic-to-real transfer memanfaatkan domain adaptation, adversarial training, atau augmentasi berbasis fisika untuk menyelaraskan distribusi feature space antara data sintetik dan real-world.

Pembahasan ini merupakan konsekuensi logis dari isu hallucinated detail yang telah dibahas pada slide sebelumnya. Ketika model hanya terpapar pada degradasi sintetis yang terlalu terkontrol, jaringan saraf cenderung belajar pola artifisial yang memicu halusinasi tekstur atau struktur saat menghadapi ketidakpastian pada data nyata. Dengan mengakui keterbatasan degradasi sintetis, kita dapat merancang protokol validasi yang lebih ketat, termasuk pelaporan uncertainty dan evaluasi dampak pada tugas hilir seperti segmentasi atau deteksi objek.

Sebagai tindak lanjut, slide berikutnya akan membahas strategi konkret untuk membangun model degradasi sintetis yang lebih realistis. Pendekatan ini mencakup komposisi multi-stage degradation, integrasi artifact sensor dan kompresi JPEG, serta variasi parameter stokastik per sampel. Tujuannya jelas: mencocokkan distribusi training sedekat mungkin dengan kondisi deployment, sehingga performa model tetap robust dan interpretabilitas hasil restorasi dapat dipertanggungjawabkan secara ilmiah.

---

## Slide 024 - Menuju Model Degradasi Realistis

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi kesenjangan fundamental antara degradasi sintetis yang lazim digunakan dalam pelatihan model dan degradasi nyata yang ditemui di lapangan. Model yang hanya dikenalkan dengan degradasi sederhana sering kali mengalami *domain shift* yang signifikan saat diterapkan pada data asli. Untuk mengatasi hal ini, langkah krusial berikutnya adalah merancang model degradasi sintetis yang lebih mendekati kompleksitas fisik dan statistik citra dunia nyata.

Pendekatan realistis tidak lagi mengandalkan satu jenis filter atau noise tunggal. Sebaliknya, kita perlu menggabungkan berbagai operasi optik dan sensor secara bersamaan. Misalnya, kombinasi blur dapat mencakup Gaussian untuk aberrasi lensa, motion blur akibat getaran kamera, serta defocus blur dari kesalahan fokus. Untuk komponen noise, integrasi Gaussian, Poisson yang merepresentasikan shot noise pada kondisi pencahayaan rendah, dan speckle noise umum pada pencitraan medis atau radar akan menghasilkan distribusi error yang lebih akurat. Selain itu, artefak kompresi JPEG, keterbatasan dinamis range sensor, hingga kesalahan proses demosaicing harus disertakan agar simulasi mencerminkan rantai pemrosesan kamera secara utuh.

Variasi parameter secara acak pada setiap sampel juga menjadi wajib. Dalam implementasi praktis, parameter seperti sigma blur, intensitas noise, atau kualitas JPEG tidak boleh bersifat statis. Randomisasi ini memaksa model untuk belajar invariansi terhadap variasi degradasi, sehingga meningkatkan generalisasi. Sebagai ilustrasi, pipeline degradasi yang umum digunakan dalam riset terkini dapat disederhanakan sebagai berikut:
```
clean -> blur -> downscale -> noise -> JPEG -> degraded
```
Setiap tahap dalam pipeline ini merepresentasikan operasi fisik atau digital yang berurutan. Penting untuk dicatat bahwa urutan dan komposisi operasi ini sangat mempengaruhi karakteristik frekuensi dan struktur spasial dari citra terdegradasi.

Prinsip utama yang perlu ditekankan adalah kecocokan antara distribusi degradasi selama pelatihan dan target deployment. Jika protokol evaluasi melaporkan model degradasi secara eksplisit, termasuk parameter distribusi dan chain of operations, maka replikasi hasil penelitian menjadi lebih transparan dan dapat dibandingkan secara objektif antar studi. Hal ini juga menjadi fondasi penting sebelum beralih ke metode restorasi tingkat lanjut yang bergantung pada prior kuat, seperti pendekatan berbasis diffusion model yang akan kita bahas pada slide berikutnya. Dengan degradasi sintetis yang terkontrol dan realistis, mekanisme conditioning pada diffusion process dapat dioptimalkan untuk menghasilkan estimasi citra bersih yang lebih setia pada konten asli tanpa terjebak dalam halusinasi struktural.

---

## Slide 025 - Diffusion-Based Restoration

### Narasi

Pada slide ini, kita membahas penerapan diffusion model khusus untuk tugas image restoration. Berbeda dengan pendekatan deterministik tradisional yang langsung memetakan citra terdegradasi ke hasil restorasi, diffusion model secara fundamental mempelajari distribusi probabilitas dari citra bersih itu sendiri melalui proses denoising bertahap. Pendekatan ini sangat relevan dalam konteks penelitian tingkat doktoral karena sifatnya yang probabilistik dan mampu menangkap kompleksitas struktur visual yang sulit dimodelkan secara eksplisit.

Untuk restoration, target utamanya adalah menghasilkan sampel dari distribusi bersyarat `p(x | y)`, di mana `x` merepresentasikan citra bersih dan `y` adalah observasi terdegradasi. Kita tidak lagi mengejar satu estimasi titik tunggal, melainkan memanfaatkan kemampuan diffusion dalam menyediakan prior visual yang kuat sekaligus bersifat multimodal. Hal ini memungkinkan model untuk menghasilkan variasi hasil yang secara perceptual masuk akal, meskipun inputnya identik.

Keunggulan utama dari arsitektur berbasis diffusion terletak pada kemampuannya menghasilkan detail tekstur yang lebih natural dan beragam dibandingkan metode konvensional atau GAN. Selain itu, framework ini sangat fleksibel; satu model yang sama dapat diarahkan untuk menyelesaikan berbagai jenis inverse problem—seperti deblurring, inpainting, super-resolution, atau denoising—hanya dengan mengubah mekanisme conditioning-nya tanpa perlu mengubah arsitektur inti secara signifikan. Fleksibilitas ini menjadikan diffusion sebagai kandidat kuat untuk unified restoration frameworks.

Namun, implementasinya menghadapi sejumlah tantangan metodologis dan komputasional yang perlu menjadi fokus kajian kritis. Pertama, beban komputasi tetap menjadi hambatan utama karena memerlukan ratusan hingga ribuan langkah iteratif denoising selama inference. Kedua, adanya ketidakpastian inherent dalam proses sampling stokastik yang bisa menghasilkan varians hasil yang cukup lebar. Ketiga, risiko halusinasi struktur atau tekstur yang tidak sesuai dengan konten asli masih tinggi, mengingat sifat generatifnya yang bebas.

Oleh karena itu, strategi tuning guidance menjadi krusial untuk menyeimbangkan antara plausibilitas prior dan kesetiaan pada data observasi `y`. Dalam praktik eksperimental, peneliti sering mengimplementasikan classifier-free guidance atau menambahkan gradient-based fidelity term untuk menekan deviasi dari input terdegradasi. Keseimbangan antara kebebasan generatif dan akurasi terhadap `y` inilah yang menentukan keberhasilan sebuah model restoration. Konsep penyuntikan kondisi `y` ke dalam reverse process akan kita bedah lebih teknis pada slide berikutnya, setelah kita meninjau kembali bagaimana degradasi realistis dari slide sebelumnya membentuk ruang pencarian `p(x | y)`.

---

## Slide 026 - Prinsip Kerja Conditional Diffusion

### Narasi

Slide ini melanjutkan pembahasan pada slide sebelumnya mengenai bagaimana model difusi dapat dimanfaatkan sebagai prior kuat untuk menyelesaikan masalah inversi dalam restorasi citra. Jika pada slide 25 kita telah melihat bahwa diffusion model mampu menghasilkan sampel dari distribusi kondisional $p(x \mid y)$ dengan detail yang natural dan beragam, maka slide 26 mengupas mekanisme fundamental di balik kemampuan tersebut, yaitu prinsip kerja conditional diffusion.

Inti dari pendekatan ini terletak pada dua proses yang berlawanan arah, yang secara matematis merepresentasikan degradasi terkontrol dan rekonstruksi bertahap:

```
Forward:  x_0 = x  ->  x_1 -> ... -> x_T (noise)
Reverse:  x_T (noise) -> ... -> x_1 -> x_0 (estimate)
```

Pada proses forward, citra bersih $x_0$ secara bertahap ditambahkan noise Gaussian hingga mencapai distribusi noise standar $x_T$. Proses reverse berfungsi sebagai kebalikannya, di mana model belajar memprediksi dan mengurangi noise secara iteratif untuk mengembalikan struktur citra. Namun, tanpa kondisi, hasil reverse hanya akan menghasilkan sampel dari distribusi marginal citra bersih, bukan restorasi yang sesuai dengan input terdegradasi.

Untuk mengatasi hal ini, informasi observasi terdegradasi $y$ disuntikkan langsung ke dalam reverse process melalui formulasi:

```
x_{t-1} = denoise(x_t, y, t)
```

Implementasi conditioning ini umumnya dilakukan dengan tiga strategi utama:
- **Concatenation**: menggabungkan tensor representasi $y$ bersama $x_t$ pada channel input awal atau intermediate layer.
- **Feature Injection**: menyuntikkan embedding $y$ ke dalam blok encoder-decoder menggunakan mekanisme cross-attention atau adaptive normalization (misalnya AdaIN atau FiLM).
- **Gradient-based Guidance**: menghitung gradien log-likelihood $\nabla_{x_t} \log p(y \mid x_t)$ untuk menarik sampel menuju wilayah yang konsisten dengan data observasi selama sampling.

Poin krusial yang perlu ditekankan adalah bahwa restorasi berbasis generatif tidak sekadar meminimalkan error pixel-wise, melainkan mencari keseimbangan optimal antara *plausibilitas prior* (struktur dan tekstur yang realistis menurut distribusi citra alami) dan *kesetiaan pada $y$* (fidelity terhadap informasi yang benar-benar ada pada input terdegradasi). Ketidakseimbangan pada kedua aspek ini sering menjadi sumber artefak halusinasi atau over-smoothing, yang menjadi fokus analisis kritis dalam penelitian tingkat doktoral.

Konsep kondisioning ini langsung menerjemahkan diri ke dalam alur praktis yang akan dibahas pada slide berikutnya. Ketika kita menerapkan prinsip di atas dalam implementasi nyata, kekuatan conditioning atau guidance strength menjadi hyperparameter penentu trade-off antara keragaman hasil dan akurasi struktural. Pada praktikum dan eksperimen lanjutan, mahasiswa diharapkan dapat membandingkan pendekatan deterministik berbasis CNN dengan ensemble sampel dari conditional diffusion, lalu menganalisis bagaimana variasi guidance strength mempengaruhi ketidakpastian sampling serta konsistensi terhadap ground truth.

---

## Slide 027 - Workflow Diffusion Restoration

### Narasi

Slide ini merangkum alur kerja atau *workflow* restorasi citra menggunakan model difusi kondisional. Alur dimulai dari input citra terdegradasi $y$, yang kemudian diproses melalui tahap encoding degradasi atau *conditioning*. Tahap ini menentukan bagaimana informasi kerusakan disuntikkan ke dalam jaringan, melanjutkan konsep kondisionalisasi dari slide sebelumnya—baik melalui concatenation, feature injection, maupun gradient-based guidance.

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

Setelah kondisi terenkripsi, model memasuki iteratif reverse sampling selama $T$ langkah. Berbeda dengan arsitektur restorasi tradisional yang bersifat deterministik dan menghasilkan satu keluaran tunggal, model difusi secara inheren bersifat stokastik. Pada tahap ini, proses sampling diulang atau dijalankan dengan seed berbeda untuk menghasilkan beberapa sampel hasil restorasi $\hat{x}$ dari input $y$ yang identik.

Kumpulan sampel tersebut kemudian masuk ke tahap agregasi, seleksi, atau pengukuran ketidakpastian. Di sinilah parameter *guidance strength* menjadi variabel kontrol utama. Jika *guidance strength* diatur tinggi, model akan memprioritaskan kesetiaan terhadap data terdegradasi $y$, sehingga fidelitas meningkat namun keragaman (*diversity*) hasil menurun. Sebaliknya, pengaturan *guidance strength* yang rendah memungkinkan eksplorasi ruang prior yang lebih luas, menghasilkan detail tekstur yang lebih bervariasi, namun berisiko menyimpang dari konten asli $y$.

Dalam konteks praktikum dan riset tingkat doktoral, mahasiswa diinstruksikan untuk melakukan komparasi sistematis antara satu hasil restorasi deterministik (seperti CNN atau U-Net standar) dengan sekumpulan sampel dari model difusi. Eksperimen ini tidak hanya menilai kualitas visual, tetapi juga membuka ruang analisis kuantitatif terhadap stabilitas, konsistensi, dan trade-off antara akurasi struktural versus kekayaan detail generatif.

Hasil dari pengagregasian dan pemilihan sampel pada alur ini menjadi jembatan langsung menuju pembahasan slide berikutnya mengenai ketidakpastian dalam masalah inversi. Ketika beberapa sampel $\hat{x}$ berhasil dikumpulkan, kita dapat menghitung statistik per-pixel untuk memetakan wilayah mana yang memiliki konsistensi tinggi versus wilayah yang masih ambigu, yang akan kita jabarkan secara teknis pada diskusi tentang *uncertainty quantification*.

---

## Slide 028 - Uncertainty dalam Inverse Problem

### Narasi

Pada slide sebelumnya kita telah membahas workflow restorasi berbasis diffusion, di mana proses sampling iteratif menghasilkan beberapa sampel hasil rekonstruksi. Keberadaan banyak sampel ini secara alami membawa kita pada konsep fundamental dalam restorasi citra, yaitu ketidakpastian atau *uncertainty*.

Masalah restorasi citra pada dasarnya merupakan masalah invers yang bersifat *ill-posed*. Artinya, dari satu pengamatan terdegradasi, tidak ada satu solusi tunggal yang pasti benar. Ketidakpastian muncul dari tiga sumber utama:
- Noise pada proses pengukuran yang tidak dapat dipisahkan sepenuhnya dari sinyal asli.
- Informasi yang hilang secara fisik, misalnya komponen frekuensi tinggi akibat *blurring* atau kompresi lossy.
- Prior model yang tidak pasti, terutama ketika regularisasi yang digunakan terlalu lemah atau tidak sesuai dengan distribusi data target.

Untuk mengkuantifikasi hal ini secara empiris, pendekatan standar adalah memanfaatkan sifat generatif dari model. Kita dapat melakukan sampling sebanyak $M$ kali dari model yang sama dengan kondisi input yang identik. Dari $M$ sampel tersebut, kita hitung mean dan varians per piksel. Varians yang rendah menunjukkan konsistensi antar sampel, sedangkan varians yang tinggi secara langsung mencerminkan ketidakpastian estimasi model pada region tersebut.

Secara visual, pola varians ini memberikan interpretasi yang sangat informatif:
- Area datar (*flat area*) memiliki varians rendah, menandakan solusi stabil dan mudah direkonstruksi.
- Area dengan tekstur kompleks memiliki varians tinggi, karena terdapat banyak konfigurasi tekstur yang secara matematis *plausible*.
- Tepi objek (*edge*) menunjukkan varians sedang, yang mengindikasikan bahwa meskipun posisi piksel tepi mungkin sedikit bergeser, strukturnya tetap dianggap penting oleh model.

Implikasi praktisnya sangat krusial, terutama untuk aplikasi kritis seperti diagnostik medis, analisis satelit, atau forensik digital. Hasil restorasi saja tidak cukup; kita wajib menyajikan peta ketidakpastian (*uncertainty map*) bersama gambar yang direstorasi. Ini memungkinkan pengguna akhir untuk menilai kepercayaan terhadap setiap region, sekaligus menghindari pengambilan keputusan berdasarkan artefak halusinasi model.

Pembahasan mengenai kuantifikasi ketidakpastian ini harus berjalan beriringan dengan metodologi evaluasi yang ketat. Ketika kita melaporkan hasil restorasi beserta peta ketidakpastiannya, kita perlu memastikan bahwa evaluasi dilakukan secara adil, transparan, dan dapat direproduksi, yang akan menjadi fokus pada slide berikutnya terkait prinsip-prinsip perancangan evaluasi yang robust.

---

## Slide 029 - Merancang Evaluasi yang Adil

### Narasi

Setelah membahas bagaimana ketidakpastian muncul secara alami dari sifat ill-posed pada masalah inversi, langkah selanjutnya yang krusial adalah merancang protokol evaluasi yang mampu menangkap kompleksitas tersebut secara adil dan komprehensif. Pada tingkat penelitian doktoral, satu metrik tunggal tidak cukup untuk memvalidasi klaim novelty atau keunggulan metodologis. Oleh karena itu, prinsip evaluasi harus dibangun di atas fondasi multi-metrik yang saling melengkapi, baseline yang relevan, serta transparansi penuh terhadap variabilitas eksperimen.

Penggunaan banyak metrik yang berbeda domain sangat penting. Metrik fidelitas seperti PSNR atau MSE hanya mengukur kesamaan pixel-wise, namun sering kali gagal mencerminkan persepsi manusia atau kualitas struktural. Metrik struktural seperti SSIM atau MS-SSIM menangkap korelasi spasial, sementara metrik perceptual berbasis deep features seperti LPIPS atau FID lebih selaras dengan penilaian subjektif. Kombinasi ketiganya memberikan gambaran holistik tentang performa model, terutama ketika berhadapan dengan trade-off antara akurasi numerik dan realisme visual.

Pemilihan baseline juga harus ketat. Baseline tidak boleh dipilih secara arbitrer, melainkan harus setara dalam hal data preprocessing, komputasi, dan training budget. Ini memastikan bahwa peningkatan performa benar-benar berasal dari kontribusi arsitektur atau loss function baru, bukan dari bias eksperimental. Ablation study menjadi wajib untuk mengisolasi pengaruh setiap komponen model, mulai dari modul attention, mekanisme regularization, hingga desain decoder. Tanpa ablasi yang terstruktur, klaim kontribusi ilmiah akan lemah secara metodologis.

Variabilitas eksperimen harus dilaporkan secara eksplisit. Hasil penelitian tidak boleh hanya menyajikan angka rata-rata dari satu run. Laporan harus mencakup deviasi standar atau interval kepercayaan yang dihitung dari variasi seed acak, split data train-val-test, serta inisialisasi bobot. Hal ini mencerminkan robustness model dan memungkinkan replikasi oleh peneliti lain, yang merupakan standar emas dalam publikasi internasional bereputasi.

Inspeksi visual tetap menjadi pilar utama yang tidak bisa digantikan oleh angka. Tabel metrik saja tidak cukup untuk mendeteksi artefak halus, over-smoothing, atau fenomena hallucination yang khas pada model generatif dan diffusion-based restoration. Visualisasi harus dilakukan secara terstruktur, misalnya dengan membandingkan region flat, edge, dan texture kompleks, lalu dikaitkan dengan interpretasi uncertainty map yang telah dibahas pada slide sebelumnya.

Untuk memastikan konsistensi dan reproduktibilitas, berikut adalah checklist evaluasi yang dapat dijadikan pedoman dalam penulisan laporan penelitian atau naskah jurnal:
- Degradasi didefinisikan secara eksplisit, termasuk parameter noise, blur kernel, atau downsampling factor.
- Dataset dan train-val-test split dilaporkan lengkap, termasuk sumber dan kriteria seleksi sampel.
- Baseline setara dalam data augmentasi, komputasi, dan training budget agar perbandingan bersifat adil.
- Minimal tiga metrik digunakan: fidelity, struktural, dan perceptual, masing-masing dengan justifikasi pemilihan.
- Contoh visual ditampilkan secara berdampingan untuk memudahkan identifikasi artefak dan struktur yang dipulihkan.
- Keterbatasan model dan risiko halusinasi dibahas secara kritis, termasuk kondisi di mana metode mungkin gagal atau menghasilkan output yang misleading.

Prinsip-prinsip ini akan langsung diterjemahkan ke dalam implementasi praktis. Pada slide berikutnya, kita akan melihat alur kerja konkret Praktikum 06 yang menghubungkan teori evaluasi ini ke dalam kode, mulai dari pemilihan citra bersih, pembuatan degradasi sintetis, eksekusi baseline, perhitungan metrik multidimensi, hingga penulisan analisis trade-off antara fidelitas dan persepsi visual. Pendekatan sistematis ini dirancang untuk membiasakan mahasiswa doktoral dalam merancang eksperimen yang rigor, reproducible, dan siap dikembangkan menjadi proposal disertasi yang solid.

---

## Slide 030 - Workflow Praktikum 06

### Narasi

Slide ini menyajikan alur kerja atau *workflow* terstruktur untuk praktikum keenam. Rancangan ini dibangun secara langsung dari prinsip evaluasi yang adil yang telah kita bahas pada slide sebelumnya, memastikan bahwa setiap eksperimen restorasi citra yang kalian lakukan tidak hanya menghasilkan angka, tetapi juga kesimpulan yang valid dan dapat direplikasi.

Alur praktikum ini terdiri dari enam tahapan kunci yang harus diikuti secara berurutan:
- Pilih citra bersih sebagai referensi ground truth. Pastikan representatif terhadap domain masalah yang ingin kalian teliti.
- Buat degradasi sintetis secara terkontrol. Simulasi ini harus merepresentasikan kondisi nyata yang ingin kalian restorasi, dengan parameter yang tercatat rapi.
- Jalankan algoritma atau model restorasi baseline. Pada jenjang doktoral, eksplorasi tidak berhenti pada metode klasik; bandingkan hasilnya dengan pendekatan *deep learning* atau *diffusion* jika infrastruktur mendukung.
- Hitung metrik kuantitatif secara komprehensif. Gunakan kombinasi PSNR untuk kesetiaan sinyal, SSIM untuk kesamaan struktur, dan LPIPS untuk kualitas perseptual guna menghindari bias metrik tunggal.
- Lakukan inspeksi visual terstruktur. Amati artefak, kehilangan tekstur, atau distorsi semantik yang sering kali tidak tertangkap oleh metrik numerik.
- Tulis analisis *trade-off* secara eksplisit. Diskusikan dinamika antara fidelitas sinyal versus kualitas perseptual, serta implikasinya terhadap desain sistem di aplikasi nyata.

Untuk pelaksanaan, gunakan ekosistem Python standar seperti NumPy, SciPy, scikit-image, dan Matplotlib. Format laporan disarankan dalam bentuk Jupyter Notebook yang mengintegrasikan kode, tabel hasil, dan visualisasi montase citra. Keluaran wajib yang harus kalian kumpulkan meliputi tabel perbandingan metrik antar-baseline, montase citra sebelum dan sesudah restorasi, serta paragraf analisis kritis yang menyoroti dikotomi fidelitas-perseptual. Hasil analisis ini nantinya akan menjadi fondasi metodologis ketika kalian merumuskan hipotesis dan *research gap* untuk proposal disertasi.

Sebagai eksekusi teknis dari langkah kedua dalam *workflow* ini, slide berikutnya akan langsung menampilkan implementasi pembuatan degradasi sintetis menggunakan Python. Kita akan menguraikan kode untuk menambahkan derau Gaussian, menerapkan *Gaussian blur*, serta mensimulasikan penurunan resolusi melalui operasi *downscaling* dan *upscaling*. Parameter yang digunakan dalam kode tersebut haruslah yang sama persis yang kalian laporkan, sehingga reproduktibilitas eksperimen terjaga.

---

## Slide 031 - Membuat Degradasi Sintetis di Python

### Narasi

Berikut adalah implementasi kode untuk membuat degradasi sintetis sesuai dengan langkah kedua dalam workflow praktikum sebelumnya.

```python
import numpy as np
from scipy.ndimage import gaussian_filter
from skimage import util
from skimage.transform import resize

### x: clean image, float, rentang [0, 1]

x = ...

### 1. Additive Gaussian noise

noisy = util.random_noise(x, mode='gaussian', var=0.01)

### 2. Gaussian blur

blurred = gaussian_filter(x, sigma=1.5)

### 3. Downscale + upscale (simulasi low-res)

h, w = x.shape[:2]
low = resize(x, (h // 2, w // 2), anti_aliasing=True)
degraded = resize(low, (h, w), anti_aliasing=True)
```

Kode ini memanfaatkan pustaka standar komputasi ilmiah untuk mensimulasikan tiga kategori degradasi yang paling umum dijumpai dalam masalah inverse imaging. Pertama, `util.random_noise` dengan mode `'gaussian'` dan parameter `var=0.01` menambahkan gangguan acak berdistribusi normal pada setiap piksel. Varians ini mengendalikan intensitas noise dan secara langsung memengaruhi rasio sinyal-terhadap-derau (SNR) citra, yang menjadi variabel kontrol utama dalam uji robustness model.

Kedua, `gaussian_filter` dari SciPy menerapkan konvolusi linear dengan kernel Gaussian ber-sigma 1.5. Nilai sigma ini merepresentasikan deviasi standar distribusi probabilitas kernel, yang secara fisik meniru efek defokus optik atau keterbatasan aperture lensa. Blur ini bersifat isotropik dan menghilangkan frekuensi tinggi tanpa mengubah struktur global citra secara drastis.

Ketiga, simulasi resolusi rendah dilakukan melalui pipeline downscale followed by upscale. Fungsi `resize` dipanggil berturut-turut untuk mengurangi dimensi spasial menjadi setengahnya, lalu mengembalikan ukuran ke dimensi awal. Flag `anti_aliasing=True` wajib diaktifkan pada tahap downscaling untuk mencegah artefak moiré atau aliasing akibat undersampling, sehingga degradasi yang dihasilkan murni merepresentasikan kehilangan informasi resolusi tinggi.

Dalam konteks penelitian tingkat doktoral, penyimpanan eksplisit setiap parameter degradasi sebelum eksekusi eksperimen merupakan praktik wajib. Dokumentasi ini menjadi dasar untuk reproduktibilitas ilmiah, desain ablation study yang ketat, serta penempatan kontribusi metodologis Anda terhadap state-of-the-art. Tanpa jejak parameter yang transparan, klaim peningkatan performa pada metrik fidelity versus perceptual tidak dapat diverifikasi secara kritis.

Setelah proses degradasi selesai, evaluasi kuantitatif akan dilakukan pada tahap berikutnya. Slide selanjutnya akan membahas implementasi perhitungan PSNR dan SSIM menggunakan `skimage.metrics`, yang akan berfungsi sebagai baseline numerik untuk mengukur seberapa baik model restorasi mendekati citra referensi.

---

## Slide 032 - Implementasi PSNR dan SSIM di Python

### Narasi

```python
from skimage.metrics import peak_signal_noise_ratio as psnr
from skimage.metrics import structural_similarity as ssim

### x: clean, x_hat: hasil restorasi

p = psnr(x, x_hat, data_range=1.0)
s = ssim(x, x_hat, data_range=1.0, channel_axis=-1)

print(f"PSNR: {p:.2f} dB")
print(f"SSIM: {s:.4f}")
```

Setelah kita berhasil merancang berbagai skenario degradasi sintetis pada slide sebelumnya, langkah metodologis berikutnya adalah mengukur efektivitas algoritma restorasi yang telah dikembangkan. Pada jenjang penelitian doktoral, klaim peningkatan kualitas citra tidak dapat mengandalkan penilaian visual subjektif semata. Kita memerlukan metrik kuantitatif yang rigor, transparan, dan dapat direproduksi secara lintas eksperimen. Dua indikator standar yang paling dominan dalam literatur pengolahan citra digital adalah PSNR dan SSIM.

Kode yang ditampilkan memanfaatkan fungsionalitas native dari `scikit-image` untuk menghitung kedua metrik tersebut secara komputasional efisien. Variabel `x` merepresentasikan citra referensi atau ground truth, sedangkan `x_hat` menyimpan keluaran dari pipeline restorasi Anda. Fungsi `psnr` menghitung rasio daya sinyal puncak terhadap daya noise, menghasilkan nilai dalam satuan desibel. Sebaliknya, `ssim` mengevaluasi kesamaan struktural dengan mempertimbangkan komponen luminance, kontras, dan korelasi struktural, sehingga lebih selaras dengan persepsi manusia dibandingkan metrik berbasis MSE murni.

Ada beberapa praktik terbaik implementasi yang wajib diperhatikan agar hasil evaluasi tetap valid secara statistik. Parameter `data_range=1.0` harus disesuaikan dengan normalisasi tensor input Anda. Karena kita bekerja dengan citra float bertipe `float32` pada rentang `[0, 1]`, penyetelan eksplisit ini mencegah kesalahan skala numerik. Penggunaan `channel_axis=-1` juga sangat krusial untuk citra berwarna. Argumen ini memastikan perhitungan SSIM dilakukan secara independen pada setiap channel warna sebelum digabungkan menjadi satu skor agregat, sehingga menghindari bias antar-channel.

Konsistensi tipe data dan rentang nilai antara `x` dan `x_hat` harus dijaga ketat selama preprocessing. Pencamporan tipe data seperti `uint8` dan `float32` tanpa konversi yang tepat dapat memicu overflow atau clipping yang mendistorsi skor metrik secara signifikan. Untuk penelitian yang menuntut evaluasi lebih mendalam mengenai kualitas perseptual, pertimbangkan integrasi LPIPS dan FID. Metrik tersebut tidak tersedia di `skimage`, sehingga memerlukan implementasi berbasis PyTorch atau library pendukung seperti `torchmetrics` dan `pytorch-fid` untuk menangkap representasi fitur tingkat tinggi yang tidak tertangkap oleh metrik berbasis piksel.

Hasil kuantitatif dari PSNR dan SSIM ini akan menjadi landasan objektif untuk membandingkan kinerja metode Anda dengan pendekatan konvensional. Pada slide berikutnya, kita akan membahas pentingnya menetapkan baseline restorasi yang wajar, mulai dari interpolasi bicubic hingga filter Richardson-Lucy. Tanpa perbandingan terhadap baseline yang solid dan terdokumentasi, sulit bagi reviewer jurnal internasional bereputasi untuk menilai novelty dan kontribusi nyata dari arsitektur deep learning yang Anda usulkan. Evaluasi metrik yang tepat dan positioning terhadap baseline yang ketat merupakan syarat mutlak untuk membangun proposal disertasi yang matang dan memiliki kontribusi ilmiah yang jelas.

---

## Slide 033 - Restoration Baseline

### Narasi

Setelah pada slide sebelumnya kita mengimplementasikan metrik evaluasi seperti PSNR dan SSIM menggunakan pustaka `scikit-image`, langkah selanjutnya dalam pipeline penelitian restorasi citra adalah menetapkan baseline yang solid. Sebelum mengevaluasi model arsitektur kompleks atau metode berbasis deep learning, kita harus memastikan bahwa setiap pendekatan dibandingkan terhadap standar yang wajar dan mudah direproduksi.

Tabel ini merangkum lima baseline klasik yang lazim dijadikan acuan awal. Interpolasi bicubic menjadi titik tolak standar untuk tugas super-resolusi karena kesederhanaannya dan performa yang konsisten. Untuk denoising, filter median efektif menangani noise impulsif seperti salt-and-pepper, sementara filter Gaussian memberikan smoothing linear yang lebih halus. Di ranah deblurring, filter Wiener bekerja secara optimal apabila estimasi kernel blur telah diketahui, dan Richardson-Lucy menawarkan pendekatan iteratif berbasis statistik Poisson yang sangat sesuai untuk citra dengan karakteristik noise atau degradasi tertentu.

Pemilihan baseline bukan sekadar rutinitas komputasi, melainkan fondasi validitas klaim ilmiah. Baseline berfungsi untuk membuktikan apakah peningkatan metrik yang dilaporkan berasal dari kontribusi metodologis nyata, bukan dari bias dataset atau konfigurasi yang tidak adil. Selain itu, baseline menetapkan batas bawah kualitas hasil dan biaya komputasi, sehingga peneliti dapat menghindari klaim berlebihan pada dataset yang terlalu mudah atau kondisi degradasi yang sebenarnya sudah dapat dipecahkan secara konvensional.

Langkah logis berikutnya adalah menjalankan seluruh baseline tersebut secara sistematis dan mencatat hasilnya ke dalam tabel terstruktur seperti yang akan kita bahas pada slide selanjutnya. Pencatatan harus mencakup metrik kuantitatif, waktu eksekusi, serta konfigurasi eksperimen lengkap termasuk jumlah seed, versi library, perangkat keras, dan hyperparameter demi transparansi, akuntabilitas, dan reproduktibilitas penelitian tingkat doktor.

---

## Slide 034 - Menjalankan Baseline dan Mencatat Hasil

### Narasi

Setelah menetapkan baseline yang relevan pada slide sebelumnya, langkah kritis berikutnya adalah mengeksekusi setiap metode secara konsisten dan mendokumentasikan hasilnya dengan standar reproduktibilitas yang ketat. Pada jenjang doktoral, pencatatan hasil eksperimen bukan sekadar mengisi tabel metrik, melainkan membangun landasan transparansi yang memungkinkan verifikasi independen oleh komunitas peneliti.

Tabel contoh pada slide ini membandingkan lima entri: citra terdegradasi sebagai referensi bawah, interpolasi bicubic, filter Wiener, CNN baseline, serta metode usulan Anda. Perhatikan bahwa ketiga metrik utama saling melengkapi. PSNR mengukur deviasi kuadrat rata-rata pada domain piksel dan sensitif terhadap outlier lokal. SSIM menilai kesamaan struktur spasial dan lebih selaras dengan persepsi kontras manusia. Sementara itu, LPIPS mengekstrak fitur hierarkis dari jaringan saraf pra-latih untuk mengukur jarak perseptual, sehingga sering kali lebih baik dalam membedakan artefak halus dari restorasi yang alami. Kolom runtime juga wajib dicatat karena trade-off antara akurasi dan beban komputasi menjadi penentu kelayakan implementasi di aplikasi nyata.

Agar hasil dapat dipertanggungjawabkan secara ilmiah, konfigurasi eksperimen harus dilaporkan secara eksplisit. Elemen-elemen berikut merupakan standar minimal yang harus disertakan:
- Jumlah random seed yang digunakan, untuk menjamin konsistensi shuffling data, inisialisasi bobot, dan augmentasi.
- Versi pustaka inti seperti PyTorch, torchvision, OpenCV, atau scikit-image, mengingat pembaruan minor dapat mengubah perilaku optimizer atau pipeline preprocessing.
- Spesifikasi lingkungan komputasi, termasuk tipe GPU atau CPU, kapasitas VRAM, serta versi driver CUDA jika menggunakan akselerasi hardware.
- Daftar lengkap hyperparameter, mencakup learning rate schedule, weight decay, momentum, batch size, jumlah epoch, serta protokol early stopping jika diterapkan.

Dengan dokumentasi yang rinci, klaim peningkatan performa tidak lagi bersifat anekdotal, melainkan berbasis bukti yang dapat direplikasi. Hal ini sekaligus memperkuat posisi penelitian Anda ketika berhadapan dengan reviewer jurnal atau konferensi internasional yang menuntut rigor metodologis tinggi.

Angka metrik yang telah tercatat hanyalah tahap pertama evaluasi. Numerik tanpa konteks spasial berisiko menimbulkan interpretasi yang keliru, misalnya menganggap noise ringing sebagai detail tekstur atau menganggap oversmoothing sebagai peningkatan SSIM. Pada slide berikutnya, kita akan beralih ke analisis visual terstruktur untuk memverifikasi apakah perbaikan numerik benar-benar mencerminkan pemulihan informasi yang bermakna, atau sekadar efek post-processing yang menyesatkan.

---

## Slide 035 - Analisis Visual Terstruktur

### Narasi

Setelah mencatat metrik kuantitatif seperti PSNR, SSIM, dan LPIPS pada slide sebelumnya, penting untuk diingat bahwa angka statistik saja tidak cukup menggambarkan keberhasilan restorasi citra. Skor tinggi dapat menutupi cacat perseptual atau bahkan mengindikasikan artifak buatan yang dihasilkan oleh model. Oleh karena itu, evaluasi harus dilengkapi dengan protokol analisis visual terstruktur yang sistematis.

Prosedur analisis visual yang direkomendasikan meliputi langkah-langkah berikut:
- Susun grid perbandingan standar yang menampilkan empat kolom berdampingan: referensi asli, input terdegradasi, hasil baseline, dan output metode usulan Anda.
- Pilih region spesifik yang paling sensitif terhadap kualitas restorasi, yaitu area tekstur kompleks, transisi edge atau kontur, bidang datar (flat area), serta struktur detail halus.
- Buat tampilan zoom-in pada masing-masing region untuk memeriksa perilaku piksel secara mikroskopis sebelum menarik kesimpulan.

Hasil observasi dapat didokumentasikan dalam tabel komparatif yang membandingkan kinerja baseline versus metode Anda. Misalnya, meskipun metode Anda menghasilkan detail rambut yang lebih tajam dibandingkan baseline, Anda wajib menilai apakah hal ini merepresentasikan pemulihan informasi sejati atau justru potensi hallucinasi fitur. Area latar belakang yang seharusnya rata harus bebas dari noise residual, sementara transisi tepi objek perlu diperiksa keberadaan efek ringing yang sering kali menandakan keterbatasan model.

Diskusi kritis yang harus dijawab adalah apakah peningkatan visual yang terlihat benar-benar mencerminkan restorasi informasi yang valid, atau semata-mata akibat teknik sharpening berlebihan yang hanya memperkuat frekuensi tinggi tanpa memperbaiki kesalahan struktural. Pada tingkat doktoral, membedakan antara rekonstruksi setia dan overfitting perseptual menjadi kunci utama dalam memvalidasi novelty metodologis.

Setelah audit visual ini selesai, langkah selanjutnya adalah menempatkan temuan tersebut ke dalam kerangka evaluasi yang lebih luas. Kita akan membahas bagaimana trade-off antara fidelitas, kualitas perseptual, dan biaya komputasi saling berinteraksi antar paradigma arsitektur, sehingga Anda dapat menentukan titik operasi optimal untuk eksperimen disertasi Anda.

---

## Slide 036 - Trade-off: Fidelity, Perceptual Quality, Computational Cost

### Narasi

Pada slide sebelumnya, kita telah membahas prosedur analisis visual terstruktur untuk memverifikasi apakah perbaikan citra benar-benar substantif atau sekadar efek sharpening artifisial. Evaluasi berbasis mata saja rentan terhadap bias persepsi, sehingga diperlukan kerangka kerja evaluasi yang lebih objektif dan multidimensi. Di sinilah konsep *trade-off* antara *Fidelity*, *Perceptual Quality*, dan *Computational Cost* menjadi fondasi kritis dalam merancang eksperimen restorasi citra tingkat lanjut.

Mari kita bedah tiga sumbu evaluasi tersebut berdasarkan perbandingan pendekatan yang ada:
- **Filter klasik**: Memiliki biaya komputasi yang sangat rendah, namun kualitas perseptualnya cenderung rendah karena sering menghaluskan detail tekstur asli. Fidelitasnya hanya berada di tingkat sedang.
- **CNN / Transformer**: Menawarkan fidelitas tinggi yang tercermin dari skor metrik objektif seperti PSNR atau SSIM. Namun, kualitas perseptualnya masih berada di tingkat sedang karena model deterministik ini cenderung menghasilkan artefak repetitif atau tekstur yang kurang natural.
- **Diffusion**: Memberikan kualitas perseptual yang sangat tinggi berkat kemampuannya memodelkan distribusi data kompleks secara probabilistik. Sayangnya, hal ini dibayar dengan biaya komputasi yang tinggi akibat proses denoising bertahap yang membutuhkan iterasi inference panjang.

Implikasi langsungnya bagi penelitian doktoral adalah pemahaman bahwa tidak ada metode tunggal yang unggul secara mutlak di semua sumbu. Peneliti wajib memilih titik operasi yang selaras dengan kebutuhan aplikasi target dan melaporkannya secara eksplisit dalam publikasi. Contoh penerapannya dalam penyusunan proposal disertasi meliputi:
- **Domain medis**: Prioritaskan fidelitas diagnostik dan integrasi *uncertainty quantification*, karena akurasi struktural bersifat non-negotiable untuk keputusan klinis.
- **Domain fotografi & seni**: Fokuskan pada kualitas perseptual dan efisiensi rendering, dengan tujuan menjaga estetika visual tanpa overhead komputasi berlebihan.
- **Domain real-time**: Utamakan latensi komputasi dan *robustness*, mengingat keterbatasan sumber daya pada perangkat tepi (*edge devices*) atau sistem embedded yang menuntut respons cepat.

Penentuan titik optimal dalam ruang evaluasi multidimensi ini akan menjadi landasan metodologis sebelum kita memasuki tahap validasi empiris. Setelah Anda menetapkan pendekatan dan batasan komputasi yang tepat, klaim performa tersebut harus dapat diverifikasi secara ketat oleh komunitas ilmiah. Hal ini akan membawa kita secara alami ke pembahasan mengenai protokol benchmarking standar, skema degradasi yang transparan, serta prinsip *reproducibility* yang akan kita dalami pada slide berikutnya.

---

## Slide 037 - Reproducibility dan Benchmarking

### Narasi

Setelah membahas trade-off antara fidelitas, kualitas perseptual, dan biaya komputasi pada slide sebelumnya, langkah kritis berikutnya dalam evaluasi metode *image restoration* adalah memastikan bahwa setiap klaim kinerja dapat diverifikasi melalui benchmarking yang standar dan reproduksibilitas yang ketat. Reproduksibilitas bukan sekadar kelengkapan administratif, melainkan fondasi metodologis yang menentukan validitas ilmiah dan potensi adopsi suatu pendekatan baru di komunitas riset.

Standarisasi evaluasi dimulai dari pemilihan dataset benchmark yang telah diakui secara luas. Dataset seperti Set5, Set14, BSD100, Urban100, dan DIV2K menjadi acuan utama karena masing-masing mewakili karakteristik degradasi dan kompleksitas visual yang berbeda. Misalnya, Urban100 sangat sensitif terhadap artefak pada struktur geometris dan garis tepi, sementara DIV2K memberikan tantangan pada dinamika warna, tekstur halus, dan resolusi tinggi. Pemilihan dataset harus selaras dengan domain aplikasi yang ditargetkan agar metrik evaluasi benar-benar merefleksikan performa di kondisi nyata.

Spesifikasi skema degradasi juga harus didokumentasikan secara eksplisit. Setiap eksperimen wajib mendefinisikan jenis noise (Gaussian, Poisson, atau sensor-specific), profil kernel blur, faktor downscaling, serta urutan transformasi non-linear yang diterapkan sebelum proses restorasi. Tanpa deskripsi degradasi yang transparan, perbandingan antar-arsitektur menjadi tidak adil karena perbedaan kondisi input dapat mengaburkan kontribusi sebenarnya dari mekanisme pembelajaran model.

Untuk menjamin reproduksibilitas teknis, ikuti praktik pelaporan berikut secara konsisten:
- Tuliskan semua hyperparameter, termasuk ukuran batch, laju pembelajaran, strategi weight decay, dan konfigurasi optimizer.
- Cantumkan nilai *random seed* untuk inisialisasi bobot, pengacakan data, dan generator pseudo-random library.
- Sertakan kode lengkap, skrip preprocessing, dan instruksi eksekusi yang terverifikasi di lingkungan standar seperti Google Colab atau Jupyter Notebook.
- Gunakan versi library yang spesifik (PyTorch, torchvision, Albumentations, dll.) untuk menghindari inkonsistensi perilaku API.

Hindari praktik *cherry-picking* visual, yaitu hanya menyajikan hasil yang paling menarik secara subjektif tanpa menyertakan analisis kuantitatif menyeluruh. Evaluasi harus didasarkan pada metrik objektif yang dihitung secara seragam di seluruh set uji, dilengkapi dengan interval kepercayaan atau uji signifikansi jika diperlukan. Reproduksibilitas dan benchmarking yang disiplin merupakan bagian tak terpisahkan dari protokol evaluasi *restoration*, dan prinsip ini akan diperdalam lebih sistematis pada Pertemuan 12 ketika kita membahas *experimental design* untuk penelitian doktoral.

Dengan kerangka evaluasi yang solid, langkah logis selanjutnya adalah mengarahkan kajian kritis ke area yang masih terbuka. Slide berikutnya akan menguraikan beberapa *research gap* strategis—mulai dari pemodelan degradasi dunia nyata, restorasi buta (*blind restoration*), hingga evaluasi berbasis tugas dan deteksi halusinasi—yang dapat Anda pilih dan dirumuskan menjadi pertanyaan penelitian yang teruji untuk proposal disertasi.

---

## Slide 038 - Riset Gap yang Bisa Dieksplorasi

### Narasi

Pada jenjang doktoral, identifikasi *research gap* bukan sekadar langkah administratif, melainkan fondasi strategis sebelum merancang metodologi eksperimen. Slide ini menyajikan enam celah penelitian yang sangat relevan dengan perkembangan mutakhir dalam *image restoration* dan *computational imaging*. Setiap poin dirancang untuk mendorong pendekatan kritis terhadap batasan metode konvensional dan menemukan peluang kontribusi ilmiah yang terukur.

Berikut adalah area celah penelitian yang dapat Anda kaji lebih mendalam:
- **Real-world degradation**: Model degradasi sintetis sering kali terlalu ideal. Tantangannya terletak pada penentuan model degradasi yang paling tepat untuk domain spesifik, seperti citra medis, penginderaan jauh, atau fotografi *low-light*.
- **Blind restoration**: Dalam skenario tanpa *ground truth*, estimasi kernel blur dan noise harus dilakukan secara simultan. Pendekatan ini menuntut integrasi antara pemodelan statistik dan arsitektur jaringan yang mampu merekonstruksi representasi degradasi langsung dari data observasi.
- **Uncertainty-aware restoration**: Output tidak hanya berupa gambar yang diperbaiki, tetapi juga dilengkapi dengan peta kepercayaan (*confidence map*) per piksel. Mekanisme ini vital untuk aplikasi kritis di mana ketidakpastian model harus dapat dilokalisasi.
- **Hallucination detection**: Arsitektur generatif, khususnya *diffusion model*, rentan menciptakan detail yang tidak terdapat pada aslinya. Diperlukan mekanisme deteksi yang mampu memisahkan informasi yang dipulihkan secara akurat dari artifak sintetik.
- **Task-driven evaluation**: Metrik berbasis piksel seperti PSNR atau SSIM tidak selalu berkorelasi dengan kinerja *downstream*. Evaluasi harus mengukur dampak langsung restorasi terhadap akurasi deteksi objek atau segmentasi semantik.
- **Efficient diffusion restoration**: Meski menawarkan prior yang kuat, biaya komputasi sampling *diffusion model* masih menjadi hambatan praktis. Fokus riset di sini adalah akselerasi inferensi dan reduksi langkah sampling tanpa mengorbankan stabilitas konvergensi.

Pilih satu celah yang paling selaras dengan minat akademik dan ketersediaan infrastruktur komputasi. Setelah topik terpilih, segera rumuskan *research question* yang spesifik, memiliki batasan masalah yang jelas, serta kriteria keberhasilan yang dapat diuji secara empiris. Pastikan hipotesis Anda terhubung langsung dengan metrik evaluasi dan desain eksperimen yang akan Anda bangun.

Pembahasan mengenai celah penelitian ini merupakan tindak lanjut logis dari prinsip reproduktibilitas dan standar benchmark yang telah kita diskusikan pada slide sebelumnya. Ketika Anda menetapkan arah riset, konsistensi dalam pelaporan skema degradasi, *hyperparameter*, dan kode eksperimen akan menjadi penentu validitas temuan Anda. Selanjutnya, materi akan ditutup dengan rangkuman pesan kunci yang menyatukan seluruh prinsip teknis dan filosofis dalam *image restoration*, termasuk pemahaman tentang sifat masalah invers, keterbatasan metrik tradisional, serta peran model generasi dalam konteks restorasi yang bertanggung jawab.

---

## Slide 039 - Rangkuman dan Pesan Kunci

### Narasi

Slide ini berfungsi sebagai penutup sekaligus rangkuman strategis dari seluruh pembahasan Pertemuan 06 mengenai *Image Restoration* dan *Computational Imaging*. Sebelumnya, kita telah menguraikan berbagai celah penelitian yang masih terbuka, mulai dari *blind restoration*, estimasi ketidakpastian, hingga deteksi halusinasi pada output model. Pada slide ini, kita akan menyatukan kembali fondasi metodologis yang wajib menjadi acuan dalam merancang eksperimen, memilih prior, hingga mengevaluasi hasil restorasi secara kritis.

Pertama, ingatlah bahwa *image restoration* secara inheren merupakan masalah balik (*inverse problem*). Solusi untuk masalah ini jarang bersifat unik, sehingga kehadiran *prior* atau asumsi struktural menjadi penentu utama dalam membatasi ruang pencarian solusi. Apa yang diketahui versus apa yang hilang sepenuhnya bergantung pada *forward model* yang Anda definisikan. Kesalahan dalam memodelkan proses degradasi akan langsung merusak validitas prior dan berujung pada hasil yang bias atau tidak generalisasi.

Kedua, evaluasi kuantitatif harus diperluas melampaui PSNR dan SSIM. Meskipun kedua metrik tersebut berguna sebagai indikator kesamaan piksel, mereka tidak cukup menangkap kualitas perseptual atau utilitas citra untuk tugas downstream. Peneliti tingkat doktor diharapkan melengkapi analisis dengan metrik berbasis persepsi, pengukuran struktur, serta inspeksi visual yang sistematis. Penting juga untuk membedakan secara eksplisit antara pemulihan detail yang sebenarnya ada versus kreasi detail baru yang secara statistik masuk akal namun tidak merepresentasikan realitas asli.

Ketiga, desain degradasi sintetis harus selalu dikalibrasi terhadap karakteristik degradasi nyata pada domain target. Menggunakan model degradasi yang terlalu idealis atau tidak representatif akan menghasilkan arsitektur yang hanya unggul pada data laboratorium, bukan pada skenario dunia nyata. Di sisi lain, meskipun *diffusion model* menawarkan prior yang sangat kuat serta kemampuan bawaan dalam mengkuantifikasi ketidakpastian, biaya komputasi sampling yang tinggi dan kerentanan terhadap halusinasi detail tetap menjadi pertimbangan metodologis yang serius.

Keempat, standar evaluasi dalam penelitian doktoral harus mengadopsi prinsip tiga pilar: penggunaan multi-metrik yang saling melengkapi, perbandingan terhadap baseline yang adil dan terkini, serta reproduktibilitas penuh dari seluruh pipeline eksperimen. Konsistensi dalam pelaporan metrik dan transparansi dalam konfigurasi prior akan menentukan kredibilitas kontribusi ilmiah yang Anda ajukan.

Prinsip-prinsip evaluasi dan pemahaman mendalam tentang batasan model restorasi ini akan langsung diterjemahkan ke dalam konteks tugas-tugas visi komputer lainnya. Pada pertemuan berikutnya, kita akan membahas *Object Detection Modern dengan YOLO dan Transformer*. Kualitas citra yang keluar dari tahap restorasi akan langsung memengaruhi akurasi deteksi, menjadikan restorasi sebagai komponen preprocessing yang strategis. Metodologi evaluasi seperti mAP, analisis confusion matrix, dan pengujian robustness akan melanjutkan prinsip evaluasi multi-metrik yang kita diskusikan hari ini. Pertanyaan kritis mengenai apakah peningkatan skor metrik benar-benar mencerminkan peningkatan kinerja sistem nyata akan kembali muncul, bahkan semakin kompleks saat dikaitkan dengan arsitektur transformer dan detektor modern.

---

## Slide 040 - Kaitan dengan Pertemuan Berikutnya

### Narasi

Slide ini berfungsi sebagai jembatan konseptual antara materi *Image Restoration* yang baru saja kita tutup dengan topik deteksi objek modern pada pertemuan berikutnya. Seperti yang telah dirangkum pada slide sebelumnya, restorasi citra merupakan masalah inversi yang menuntut prior kuat, dan evaluasi kinerjanya tidak boleh bergantung semata pada metrik tradisional seperti PSNR atau SSIM. Prinsip evaluasi multi-metrik serta kehati-hatian dalam menafsirkan peningkatan skor numerik akan menjadi fondasi metodologis yang langsung diterjemahkan ke dalam konteks deteksi objek.

Pada Pertemuan 07, kita akan membahas *Object Detection Modern dengan YOLO dan Transformer*. Keterkaitan utamanya terletak pada kualitas representasi fitur dari citra input. Restorasi citra sering kali berperan sebagai tahap *preprocessing* strategis, khususnya dalam skenario *real-world deployment* di mana degradasi akibat noise, *motion blur*, atau kompresi berlebihan dapat mendegradasi tepi objek dan tekstur halus. Arsitektur detektor mutakhir sangat sensitif terhadap kehilangan informasi frekuensi tinggi tersebut. Oleh karena itu, pemahaman mendalam mengenai bagaimana pipeline restorasi memulihkan struktur spasial akan secara langsung mempengaruhi stabilitas prediksi *bounding box*, kalibrasi skor kepercayaan, dan generalisasi model pada domain target.

Dari perspektif desain eksperimen tingkat doktoral, prinsip evaluasi yang kita tekankan pada restorasi akan berlanjut secara inheren. Deteksi objek memperkenalkan metrik seperti mAP, analisis *confusion matrix*, dan pengujian *robustness* terhadap variasi kondisi pencahayaan, resolusi, atau oklusi. Pertanyaan kritis yang sama akan muncul kembali: apakah peningkatan nilai metrik secara statistik benar-benar mencerminkan peningkatan kinerja sistem dalam skenario aplikasi nyata? Mahasiswa diharapkan mampu merancang ablation study yang mengisolasi pengaruh modul restorasi terhadap performa akhir-to-end, serta mengkuantifikasi trade-off antara overhead komputasi, latensi inferensi, dan akurasi deteksi.

Selain keterkaitan dengan deteksi objek, perlu disorot bahwa pendekatan restorasi berbasis *diffusion model* yang kita bahas akan menjadi benang merah menuju materi *Generative Vision* pada Pertemuan 09. Kapabilitas *diffusion* dalam memodelkan distribusi data kompleks dan menghasilkan sampel realistis akan membuka diskusi lanjutan mengenai kontrol generasi, konsistensi struktural, serta integrasinya dengan tugas *downstream* seperti segmentasi semantik atau deteksi multi-skala. Silakan persiapkan literatur terkini mengenai *joint restoration-detection pipelines* dan mekanisme evaluasi yang menguji dampak restorasi terhadap bias detektor sebelum kita masuk ke sesi praktikum minggu depan.

---

## Slide 041 - Penutup

### Narasi

Sesi ini ditutup dengan ringkasan atas konsep Image Restoration dan Computational Imaging. Pada level doktoral, fokus bergeser dari sekadar perbaikan visual menuju integrasi sistematis antara pembatasan akuisisi hardware dan algoritma pemrosesan perangkat lunak. Teknik restorasi kontemporer, termasuk framework berbasis optimisasi, deep learning, serta diffusion models, kini menjadi elemen fundamental dalam membangun pipeline data yang robust sebelum memasuki tahap analisis lanjutan.

Kualitas output restorasi memiliki dampak langsung pada performa tugas downstream seperti deteksi objek. Pendekatan preprocessing berbasis restorasi dapat meningkatkan ketahanan model terhadap degradasi citra, sekaligus membuka peluang penelitian mengenai optimalisasi joint training antara restoration dan detection modules. Prinsip evaluasi multi-metrik yang telah dibahas sebelumnya akan terus diterapkan, dengan penekanan khusus pada hubungan kausal antara peningkatan metrik restorasi dan peningkatan akurasi deteksi di lapangan.

Eksplorasi terhadap metode diffusion-based restoration hari ini juga akan menemukan kesinambungan metodologis pada pembahasan Generative Vision di sesi mendatang. Fondasi teknis dan kerangka berpikir kritis yang telah disusun hari ini diharapkan dapat langsung diadaptasi ke dalam rancangan eksperimen dan formulasi research question Anda. Terima kasih atas kontribusi diskusi yang sangat bermakna. Sampai jumpa pada pembahasan Object Detection Modern dengan YOLO dan Transformer.
