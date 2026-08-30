# Narasi TD Pengolahan Citra Digital - Pertemuan 12

## Experimental Design dan Reproducible Benchmarking

Sumber: markdown/pert12-experimental-design-dan-reproducible-benchmarking.md

---

## Slide 000 - Cover

### Narasi

Topik inti pada slide ini adalah *Experimental Design* dan *Reproducible Benchmarking*. Pada jenjang doktoral, validitas klaim penelitian sangat bergantung pada rigor desain eksperimen dan kemampuan pihak ketiga untuk mereplikasi hasilnya. Sebuah inovasi arsitektur, pipeline preprocessing, atau mekanisme attention tidak lagi dianggap signifikan hanya karena meningkatkan skor metrik tunggal pada satu subset data, melainkan harus dibuktikan melalui protokol pengujian yang terkontrol, transparan, dan bebas dari bias seleksi sampel.

Desain eksperimen yang robust memerlukan penentuan baseline yang kompetitif dan adil, pengelolaan variabel confounding, pemilihan metrik evaluasi yang selaras dengan tujuan penelitian, serta analisis statistik yang memadai untuk membedakan noise dari sinyal nyata. Di sisi lain, *reproducible benchmarking* menekankan standarisasi lingkungan komputasi, pencatatan random seed, dokumentasi kode yang modular, serta penggunaan dataset publik dengan split train-validation-test yang konsisten. Praktik ini bukan sekadar formalitas administratif, melainkan fondasi epistemologis yang memungkinkan akumulasi pengetahuan yang kumulatif dan meminimalkan klaim berlebihan yang sering muncul dalam literatur computer vision terkini.

Penguasaan terhadap kedua aspek ini akan menjadi landasan operasional ketika kita beralih ke tahap penyusunan proposal disertasi. Agar kerangka berpikirnya lebih terstruktur, mari kita tinjau bagaimana pembahasan mengenai desain eksperimen dan benchmarking ini tersusun secara sistematis dalam rencana pembelajaran semester, serta bagaimana ia menjembatani konsep-konsep sebelumnya dengan persiapan perumusan hipotesis pada sesi berikutnya.

---

## Slide 001 - Posisi Pertemuan 12 dalam RPS

### Narasi

Slide ini menampilkan peta kedudukan pertemuan ke-12 dalam alur Rencana Pembelajaran Semester (RPS). Kurikulum mata kuliah ini dirancang secara bertahap untuk membangun kompetensi penelitian tingkat doktoral secara sistematis, sehingga setiap topik memiliki fungsi spesifik dalam siklus penelitian ilmiah.

Pertemuan sebelumnya, pertemuan ke-11, telah membahas aspek fundamental mengenai apa yang harus diuji dalam sebuah studi computer vision modern, seperti interpretabilitas model, ketahanan terhadap gangguan, serta estimasi ketidakpastian prediksi. Fokus utamanya adalah identifikasi variabel, metrik evaluasi, dan fenomena yang perlu diobservasi sebelum masuk ke tahap implementasi teknis.

Pertemuan saat ini, pertemuan ke-12, mengalihkan fokus ke bagaimana menguji hal-hal tersebut secara valid dan dapat direproduksi. Pada jenjang doktor, validitas eksperimen tidak lagi hanya dinilai dari angka akurasi atau mAP, melainkan dari desain kontrol, manajemen random seed, isolasi kontribusi komponen arsitektur, serta standar pelaporan yang memungkinkan komunitas akademik memverifikasi temuan. Tanpa protokol benchmarking yang ketat, klaim peningkatan performa atau novelty sulit dipertanggungjawabkan secara ilmiah.

Sebagai kelanjutan logis, pertemuan ke-13 akan memanfaatkan protokol eksperimen yang telah kita bangun sebagai landasan struktural untuk merumuskan research question, hipotesis kerja, dan penentuan novelty. Alur ini memastikan bahwa setiap klaim inovasi didukung oleh bukti empiris yang terstruktur, terukur, dan dapat ditelusuri kembali dari kode hingga hasil akhir.

Dengan memahami posisi strategis pertemuan ini dalam ekosistem penelitian, kita siap melanjutkan ke tujuan pembelajaran spesifik dan target keluaran yang harus dicapai, yang akan menjadi panduan operasional langsung dalam menyusun proposal awal disertasi Anda.

---

## Slide 002 - Tujuan Pembelajaran dan Target Keluaran

### Narasi

Slide ini menetapkan arah akademik dan teknis untuk pertemuan ke-12 yang berfokus pada *Experimental Design* dan *Reproducible Benchmarking*. Sebagaimana dibahas di slide sebelumnya, pertemuan 11 telah menempatkan fondasi mengenai apa yang perlu diuji dalam konteks explainable, robust, dan trustworthy computer vision. Pertemuan ini menggeser fokus secara eksplisit ke bagaimana menguji hal tersebut secara metodologis, terukur, dan bebas dari bias implisit. Di jenjang doktoral, merancang eksperimen bukan sekadar menjalankan skrip Python, melainkan membangun kerangka verifikasi ilmiah yang mampu mengisolasi kontribusi nyata sebuah metode dari noise data, variasi implementasi, atau tuning konfigurasi.

Tujuan pembelajaran pada slide ini dibagi menjadi tiga ranah kompetensi inti. Pertama, mahasiswa dituntut mampu merancang eksperimen yang dapat membedakan kontribusi metodologis murni dari pengaruh eksternal seperti distribusi data, arsitektur backbone, atau hyperparameter default. Kedua, kita akan membekali diri dengan komponen standar benchmarking modern: pembangunan baseline yang kompetitif, analisis ablation yang sistematis, penentuan variabel kontrol yang ketat, serta pelaporan statistik yang mendukung klaim generalisasi. Ketiga, evaluasi kritis terhadap reproduktibilitas paper menjadi syarat mutlak, mengingat tingginya tingkat irreproducibility dalam publikasi computer vision dan deep learning terkini.

Target keluaran dirancang agar langsung terintegrasi dengan aktivitas penelitian disertasi. Mahasiswa diharapkan menghasilkan protokol eksperimen lengkap yang siap diadopsi sebagai dokumen referensi penelitian, dilengkapi dengan pipeline komputasional yang benar-benar reproducible melalui manajemen konfigurasi eksplisit dan pengendalian random seed secara konsisten. Selain itu, kemampuan melakukan *peer review* desain eksperimen akan dilatih secara intensif, sehingga Anda mampu mengidentifikasi celah metodologis, kelebihan, dan kelemahan dalam karya ilmiah orang lain maupun naskah penelitian Anda sendiri.

Pencapaian ini secara langsung menjembatani tiga Capaian Pembelajaran Mata Kuliah (CPMK) yang telah ditetapkan. CPMK-2 menuntut evaluasi paper berdasarkan validitas eksperimen, CPMK-3 mengarahkan perancangan eksperimen komputasional yang valid dan reproducible, sedangkan CPMK-6 memastikan seluruh proses metodologis ini terinternalisasi ke dalam penyusunan proposal awal penelitian. Ketiganya membentuk alur logis yang menghubungkan kajian literatur, implementasi teknis, dan kontribusi ilmiah yang terukur.

Untuk merealisasikan target tersebut, materi ini akan diteruskan ke agenda terstruktur yang mencakup workshop desain eksperimen, sesi *peer review* protokol, evaluasi reproduktibilitas beberapa paper pilihan, serta praktik langsung menyusun pipeline eksperimen menggunakan Python di Jupyter Notebook atau Google Colab. Alur kegiatan akan bergerak dari pemahaman konseptual, implementasi teknis, diskusi studi kasus, hingga penyusunan draf protokol eksperimen awal yang siap dikembangkan lebih lanjut pada pertemuan berikutnya terkait perumusan hipotesis dan identifikasi novelty.

---

## Slide 003 - Agenda Pertemuan

### Narasi

Pada pertemuan ini, kita langsung memasuki inti dari desain eksperimen dan benchmarking yang dapat direproduksi dalam konteks pengolahan citra digital dan computer vision tingkat lanjut. Mengacu pada tujuan pembelajaran yang telah dirumuskan pada slide sebelumnya, fokus kita adalah merancang protokol eksperimen yang mampu memisahkan kontribusi metodologis murni dari pengaruh bias data, variasi implementasi, atau konfigurasi sistem yang tidak terkontrol. Target keluaran yang diharapkan adalah tersedianya pipeline eksperimen yang lengkap, terdokumentasi secara transparan, dan siap diuji validitasnya secara kritis oleh komunitas akademik.

Agenda hari ini disusun secara terstruktur untuk menyeimbangkan pemahaman konseptual dengan penguatan keterampilan teknis. Kegiatan utama mencakup workshop desain eksperimen, sesi peer review terhadap protokol yang kalian susun, serta evaluasi reproduktibilitas beberapa paper terkini. Selain itu, terdapat komponen praktikum intensif untuk membangun pipeline eksperimen reproducible menggunakan konfigurasi parameter eksplisit dan pengaturan random seed yang konsisten di seluruh tahap preprocessing hingga evaluasi model.

Alur kegiatan dipetakan dalam empat tahap sistematis agar proses pembelajaran berjalan efisien dan terukur:
- Tahap pertama berfokus pada fondasi konsep experimental design dan prinsip reproducible benchmarking dalam riset computer vision.
- Tahap kedua dilaksanakan melalui praktik pipeline komputasional di Jupyter Notebook atau Google Colab, memanfaatkan ekosistem Python seperti NumPy, SciPy, OpenCV, scikit-image, Matplotlib, serta framework deep learning seperti PyTorch, torchvision, dan library augmentasi standar industri.
- Tahap ketiga berupa diskusi studi kasus nyata dilanjutkan dengan sesi peer review untuk mengidentifikasi celah metodologis, potensi data leakage, atau ketidakadilan dalam perbandingan baseline.
- Tahap keempat mengarah pada penyusunan draft protokol eksperimen awal yang akan menjadi fondasi solid bagi pengembangan proposal penelitian kalian.

Transisi ke materi ini sangat relevan mengingat pembahasan pertemuan lalu yang menyentuh aspek interpretabilitas model melalui saliency map, kalibrasi probabilitas, analisis robustness terhadap adversarial example, serta evaluasi fairness dan model card. Hasil atribusi dan plot kalibrasi yang dihasilkan selama pertemuan tersebut perlu diverifikasi lebih lanjut. Pertanyaan kritisnya adalah bagaimana memastikan bahwa performa dan visualisasi tersebut tidak bergantung pada kebetulan pemilihan random seed, adanya kontaminasi data antar split, atau ketidakseimbangan konfigurasi antara metode usulan dan baseline. Disiplin dalam experimental design dan reproducible benchmarking menjadi mekanisme kontrol utama untuk menjawab tantangan validitas tersebut.

Dengan agenda yang terarah ini, diharapkan kalian tidak hanya memahami teori desain eksperimen, tetapi juga mampu menerapkannya secara teknis dan metodologis. Langkah selanjutnya, kita akan melakukan recap singkat terhadap poin-poin kunci dari pertemuan sebelas sebagai fondasi kontekstual sebelum masuk ke detail teknis implementasi, analisis statistik, dan strategi validasi benchmarking.

---

## Slide 004 - Recap Singkat Pertemuan 11

### Narasi

Slide ini menyajikan tinjauan ulang singkat terhadap materi Pertemuan 11 yang menjadi fondasi metodologis bagi pembahasan hari ini. Fokus utama sebelumnya mencakup empat pilar evaluasi model *computer vision*:
- **Saliency map** dan *gradient attribution* untuk melacak kontribusi fitur spasial terhadap keputusan model.
- **Calibration** dan *predictive uncertainty* guna mengukur konsistensi antara probabilitas keluaran model dengan akurasi aktualnya.
- **Robustness** terhadap *adversarial example* dan *distribution shift* untuk menilai ketahanan model di bawah gangguan sinyal atau perubahan distribusi data.
- **Fairness** dan dokumentasi *model card* sebagai instrumen transparansi terkait bias, batasan aplikasi, dan dampak sosial model.

Hasil analisis seperti peta atribusi maupun kurva kalibrasi yang dihasilkan pada sesi sebelumnya tidak boleh dianggap sebagai kesimpulan akhir tanpa kerangka verifikasi yang ketat. Pertanyaan kritis yang harus kita jawab adalah bagaimana memastikan temuan tersebut benar-benar merepresentasikan kapabilitas arsitektur model, bukan sekadar artefak dari kebetulan pengaturan *random seed*, kebocoran informasi selama pelatihan (*data leakage*), atau konfigurasi eksperimen yang tidak setara antar metode. Tanpa kontrol metodologis yang ketat, perbandingan kinerja menjadi tidak valid dan klaim ilmiah kehilangan dasar empiris yang kuat.

Oleh karena itu, diperlukan disiplin ketat dalam merancang *experimental design* dan menerapkan *reproducible benchmarking*. Pertemuan ini akan membahas prinsip standarisasi dataset, protokol pengacakan, pencatatan hiperparameter, serta strategi isolasi variabel agar setiap langkah eksperimen dapat dilacak dan diverifikasi. Pendekatan ini bukan hanya soal teknis pelaporan, melainkan fondasi epistemologis dalam penelitian tingkat doktoral yang menuntut rigor, transparansi, dan objektivitas.

Langkah yang kita bangun melalui protokol eksperimental hari ini akan menjadi bukti empiris langsung untuk pertemuan berikutnya. Pada Pertemuan 13, kita akan bergerak ke tahap formulasi *Research Question*, hipotesis, dan penentuan *novelty*. Eksperimen yang dirancang dengan standar reproduktibilitas tinggi akan berfungsi sebagai alat uji validitas klaim kontribusi Anda. Protokol yang disusun akan membantu menjawab tiga pertanyaan inti: apakah argumen novelty didukung oleh data yang konsisten, apakah perbedaan performa benar-benar signifikan dibandingkan *baseline*, dan apakah seluruh prosedur dapat direplikasi secara independen oleh peneliti lain di komunitas akademik.

---

## Slide 005 - Jembatan Menuju Pertemuan 13

### Narasi

Slide ini berfungsi sebagai penghubung strategis antara perencanaan eksperimen yang akan kita bangun pada pertemuan ini dengan kerangka teoritis penelitian yang akan dibahas di pertemuan berikutnya. Pada Pertemuan 13, fokus utama akan beralih ke perumusan Research Question, Hipotesis, dan penentuan Novelty. Semua elemen konseptual tersebut tidak berdiri sendiri, melainkan harus diuji secara empiris melalui protokol eksperimen yang kita rancang hari ini.

Eksperimen yang dirancang dalam sesi ini nantinya akan berperan sebagai bukti empiris untuk menguji hipotesis yang diajukan. Tanpa desain eksperimen yang ketat, klaim kontribusi ilmiah hanya akan bersifat spekulatif. Protokol eksperimen yang sistematis memungkinkan kita menjawab tiga pertanyaan kritis:
- Apakah klaim kontribusi metode benar-benar didukung oleh data kuantitatif?
- Apakah novelty yang ditawarkan memang memberikan peningkatan signifikan dibandingkan baseline?
- Apakah hasil eksperimen dapat direplikasi oleh peneliti lain di luar tim?

Sebagai kelanjutan dari diskusi Pertemuan 11, hasil atribusi dan kalibrasi yang telah kita produksi perlu divalidasi melalui konfigurasi yang terkontrol. Hal ini sekaligus menjadi dasar logis untuk pembahasan pada slide berikutnya, yang menyoroti mengapa disiplin experimental design menjadi hal yang mutlak di tingkat riset doktoral. Di sana akan diuraikan masalah umum seperti pengaruh random seed, bias penyiapan baseline, hingga praktik hyperparameter tuning yang selektif. Dengan demikian, narasi ini menegaskan bahwa eksperimen bukan sekadar kumpulan kode dan angka, melainkan argumen ilmiah yang harus dibangun secara transparan, terukur, dan siap uji ulang.

---

## Slide 006 - Mengapa Experimental Design Krusial di Riset S3

### Narasi

Pada tingkat doktoral, desain eksperimen bukan sekadar langkah teknis, melainkan fondasi utama yang menentukan validitas klaim ilmiah Anda. Seperti yang telah disinggung pada slide sebelumnya, setiap eksperimen yang Anda rancang berfungsi sebagai bukti empiris untuk menguji hipotesis dan membuktikan novelty metode usulan. Tanpa protokol yang ketat, sulit bagi reviewer atau komunitas akademik untuk menilai apakah peningkatan kinerja benar-benar berasal dari kontribusi metodologis Anda, atau hanya akibat variasi acak dan bias dalam pelaksanaan penelitian.

Dalam praktik riset deep learning dan computer vision saat ini, terdapat beberapa masalah sistematis yang sering meruntuhkan kredibilitas hasil penelitian:
- Peningkatan akurasi yang dilaporkan kadang hanya disebabkan oleh perbedaan *random seed*, bukan karena keunggulan arsitektur atau algoritma yang diusulkan.
- Baseline dibuat secara tidak seimbang atau sengaja dilemahkan agar metode usulan terlihat lebih unggul.
- Terjadi bias dalam penyetelan *hyperparameter*, di mana metode baru dituning secara ekstensif sementara baseline dibiarkan dengan konfigurasi default.
- Kebocoran data (*data leakage*) yang merusak integritas pembagian set pelatihan, validasi, dan pengujian.
- Eksperimen tidak dapat direplikasi karena konfigurasi lingkungan, versi library, dan protokol pelatihan tidak dilaporkan secara lengkap.

Prinsip dasar yang harus selalu dipegang adalah bahwa eksperimen itu sendiri merupakan argumen ilmiah. Setiap keputusan dalam desain eksperimen—mulai dari pemilihan dataset, strategi sampling, pipeline preprocessing, hingga metrik evaluasi—harus secara eksplisit memperkuat klaim bahwa perbedaan performa yang terukur semata-mata disebabkan oleh kontribusi metode yang Anda kembangkan, bukan oleh faktor eksternal atau bias implementasi. Di level S3, transparansi dan rigor dalam desain eksperimen sama pentingnya dengan kebaruan ide itu sendiri.

Memahami mengapa desain eksperimen begitu krusial membawa kita langsung ke konsep yang akan dibahas pada slide berikutnya, yaitu Reproducible Benchmarking. Kita akan membedah definisi operasional dari *reproducibility*, *replicability*, dan *benchmarking*, serta menelaah ruang lingkup lengkap yang harus dicakup dalam protokol evaluasi standar agar perbandingan antar metode tetap adil, transparan, dan siap diverifikasi oleh komunitas riset global.

---

## Slide 007 - Reproducible Benchmarking: Definisi dan Ruang Lingkup

### Narasi

Pada slide ini, kita membahas fondasi metodologis yang menentukan validitas klaim ilmiah dalam riset pengolahan citra digital tingkat lanjut, yaitu *Reproducible Benchmarking*. Dalam literatur computer vision dan machine learning, istilah ini sering kali digunakan secara longgar, padahal memiliki definisi teknis yang spesifik dan implikasi metodologis yang berbeda.

Mari kita bedah tiga konsep inti yang tercantum dalam tabel. Pertama, *Reproducibility* berarti bahwa eksekusi ulang dengan kode, data, dan konfigurasi yang identik akan menghasilkan keluaran numerik yang sama persis. Ini adalah batas bawah standar reproduktibilitas. Kedua, *Replicability* menguji konsistensi temuan ketika peneliti lain membangun implementasi baru berdasarkan deskripsi metodologi yang dipublikasikan, tanpa bergantung pada kode asli. Ketiga, *Benchmarking* merujuk pada protokol evaluasi yang distandardisasi agar perbandingan antar metode dilakukan secara adil, transparan, dan bebas dari bias seleksi.

Ruang lingkup *reproducible benchmarking* mencakup enam dimensi yang harus dikendalikan secara sistematis. Dimensi pertama adalah pengelolaan dataset dan strategi *split* data. Kedua, pipeline pra-pemrosesan yang mencakup augmentasi, normalisasi, dan transformasi geometri. Ketiga, spesifikasi model beserta skema pelatihan. Keempat, pemilihan metrik evaluasi beserta prosedur analisis statistiknya. Kelima, lingkungan komputasi yang mendokumentasikan versi pustaka, dependensi, dan infrastruktur hardware. Terakhir, penyimpanan artefak eksperimen dan log pelatihan yang berfungsi sebagai jejak audit untuk verifikasi independen.

Konsep ini merupakan jawaban struktural terhadap masalah-masalah yang telah kita identifikasi pada slide sebelumnya. Ketika peningkatan performa hanya berasal dari variasi *random seed*, baseline sengaja dilatih secara suboptimal, atau terjadi kontaminasi data akibat kesalahan partisi, maka argumen ilmiah penelitian menjadi tidak sah. Penerapan *reproducible benchmarking* memaksa peneliti untuk mengisolasi pengaruh arsitektur atau mekanisme usulan dari noise acak dan bias konfigurasi, sehingga setiap klaim keunggulan didukung oleh bukti yang dapat diverifikasi.

Transisi ke slide berikutnya akan mengoperasionalkan keenam dimensi tersebut ke dalam alur kerja eksperimental konkret. Kita akan menelusuri komponen utama eksperimen komputasional mulai dari akuisisi data, stratifikasi *split*, normalisasi, inisialisasi bobot, hingga protokol inference dan pelaporan interval kepercayaan, sambil memperhatikan bagaimana konfigurasi global dan nilai *seed* berperan sebagai pengendali sentral yang menyatu di seluruh tahapan pipeline.

---

## Slide 008 - Komponen Utama Eksperimen Komputasional

### Narasi

Pada slide sebelumnya, kita telah menguraikan perbedaan mendasar antara *reproducibility* dan *replicability*, serta cakupan ruang lingkup benchmarking yang meliputi dataset, lingkungan komputasi, hingga manajemen artefak. Langkah selanjutnya dalam membangun metodologi penelitian yang solid adalah merangkai seluruh elemen tersebut ke dalam sebuah alur eksperimen komputasional yang terstruktur, transparan, dan bebas dari bias implisit.

Slide ini menyajikan kerangka kerja standar yang wajib diikuti dalam penelitian pengolahan citra digital tingkat lanjut. Rantai prosesnya dapat digambarkan sebagai berikut:

```
Dataset → Split → Preprocessing → Training → Evaluation → Reporting
                              ↑
                    Konfigurasi & Seed
```

Perhatikan bahwa blok `Konfigurasi & Seed` tidak berada di ujung alur, melainkan bersifat transversal. Penetapan *seed* yang konsisten pada generator pseudo-random (seperti NumPy, PyTorch, atau Albumentations) serta konfigurasi hardware dan versi library menentukan determinisme seluruh proses. Tanpa kontrol ini, variasi acak pada inisialisasi bobot, urutan shuffling data, atau augmentasi gambar akan membuat hasil eksperimen tidak dapat diverifikasi oleh peneliti lain.

Untuk menjamin validitas internal dan eksternal, terdapat tujuh komponen kritis yang harus dikontrol secara eksplisit:

1. **Data**: Verifikasi sumber, lisensi, keseimbangan distribusi kelas, serta deteksi duplikasi antar subset mutlak diperlukan untuk mencegah *data leakage* dan bias seleksi.
2. **Split**: Pembagian subset harus menerapkan stratifikasi jika terdapat *class imbalance*. Ukuran rasio split harus justifiable secara statistik, bukan sekadar kebiasaan empiris.
3. **Preprocessing**: Pipeline normalisasi, augmentasi, dan resizing harus didefinisikan secara eksplisit. Inconsistensi transformasi antara fase training dan inference akan merusak generalisasi model secara sistematis.
4. **Model**: Dokumentasi arsitektur, mekanisme inisialisasi parameter, serta status *pretrained weights* harus dicatat lengkap. Perubahan kecil pada titik awal optimasi dapat mengarah pada konvergensi ke minimum lokal yang berbeda.
5. **Training**: Parameter optimizer, skema *learning rate scheduling*, ukuran batch, jumlah epoch, serta teknik stabilisasi seperti gradient clipping atau mixed precision wajib dilaporkan secara detail.
6. **Evaluasi**: Protokol inference, ambang batas (*threshold*) pengambilan keputusan, dan metrik statistik yang digunakan harus ditetapkan sebelum proses pengujian dimulai untuk menghindari *p-hacking* atau *metric shopping*.
7. **Pelaporan**: Presentasi hasil tidak boleh terbatas pada angka tunggal. Standar doktoral menuntut penyertaan interval kepercayaan, uji signifikansi statistik, analisis ablation, dan visualisasi error cases untuk memberikan konteks kritis atas performa model.

Kontrol ketat terhadap ketujuh aspek ini bukan sekadar administratif teknis, melainkan fondasi epistemologis yang membedakan penelitian yang dapat direplikasi dari eksperimen yang hanya bersifat insidental. Dengan memahami peta kontrol eksperimental ini, kita siap untuk mendalami komponen pertama yang paling rentan terhadap kesalahan manusia dan kebocoran informasi, yaitu strategi pembagian subset data. Pembahasan tersebut akan kita lanjutkan pada slide berikutnya mengenai prinsip, fungsi, dan aturan ketat dalam penerapan Train, Validation, dan Test split.

---

## Slide 009 - Dataset Split: Train/Validation/Test

### Narasi

Merujuk pada slide sebelumnya, kita telah mengidentifikasi tujuh komponen inti yang wajib dikontrol dalam eksperimen komputasional, termasuk manajemen data, konfigurasi model, hingga protokol evaluasi. Dari rangkaian tersebut, pembagian dataset atau *dataset split* menempati posisi strategis karena langsung menentukan validitas generalisasi model. Tanpa protokol pemisahan yang ketat, seluruh pipeline pelatihan kehilangan landasan pengukuran yang objektif.

Setiap bagian dataset memiliki fungsi temporal dan operasional yang berbeda. Set *training* berperan aktif dalam memperbarui parameter model melalui backpropagation pada setiap iterasi. Di sisi lain, set *validation* berfungsi sebagai mekanisme seleksi. Anda menggunakannya secara berkala selama pelatihan untuk menyetel hyperparameter, menerapkan *early stopping*, dan membandingkan varian arsitektur tanpa mengubah bobot jaringan. Terakhir, set *test* dirancang khusus untuk evaluasi akhir. Akses terhadap set ini harus diblokir sepenuhnya hingga semua keputusan eksperimen telah final, sehingga hasilnya mencerminkan performa sesungguhnya pada distribusi data yang belum pernah dilihat.

Aturan praktis dalam pemisahan ini sangat krusial untuk menjaga integritas penelitian tingkat lanjut. Hindari kebiasaan mengakses *test set* berulang kali untuk penyesuaian model atau pemilihan fitur. Jika hal ini terjadi, Anda berisiko mengalami *overfitting terhadap test set* atau *data leakage*, yang menyebabkan metrik evaluasi menjadi terlalu optimistis dan tidak merepresentasikan kemampuan generalisasi ke domain nyata. Pisahkan *test set* secara fisik dan logis sejak tahap awal persiapan data, serta dokumentasikan rasio pembagian secara transparan dalam laporan metodologi.

| Split | Fungsi Utama | Waktu Penggunaan |
|---|---|---|
| **Train** | Memperbarui parameter model | Setiap iterasi pelatihan |
| **Validation** | Memilih hyperparameter, early stopping, model selection | Berkala selama training |
| **Test** | Evaluasi akhir performa model | Sekali saja, setelah semua keputusan selesai |

Prinsip pemisahan statis ini menjadi dasar yang kuat, namun akan menghadapi tantangan ketika ukuran dataset terbatas atau distribusi kelas sangat tidak seimbang. Oleh karena itu, pada slide berikutnya kita akan membahas alternatif yang lebih robust, yaitu *K-Fold Cross-Validation* dan teknik stratifikasi, yang memungkinkan pemanfaatan data secara maksimal sambil tetap mempertahankan batas ketat antara informasi pelatihan dan evaluasi akhir.

---

## Slide 010 - K-Fold Cross-Validation dan Stratifikasi

### Narasi

Pada slide sebelumnya, kita telah membahas pembagian dataset statis menjadi tiga bagian: training, validation, dan testing. Meskipun pendekatan satu kali split cukup untuk prototipe awal, dalam konteks penelitian tingkat doktoral yang menuntut validitas statistik dan reproduktibilitas tinggi, metode ini sering kali terlalu rentan terhadap bias seleksi data. Oleh karena itu, kita perlu beralih ke mekanisme evaluasi yang lebih sistematis, yaitu K-Fold Cross-Validation.

Prinsip K-Fold adalah membagi seluruh data menjadi K subset atau lipatan yang berukuran hampir sama. Proses pelatihan dan evaluasi diulang sebanyak K kali. Pada setiap iterasi, satu lipatan berfungsi sebagai validation set, sementara K-1 lipatan lainnya digabung menjadi training set. Skor performa dari setiap fold kemudian dirata-ratakan. Pendekatan ini mengurangi varians akibat kebetongan dalam pemilihan split tunggal, sehingga memberikan estimasi generalisasi model yang jauh lebih stabil dan dapat dipercaya.

Namun, pembagian acak biasa pada K-Fold berpotensi menimbulkan masalah serius jika dataset Anda mengalami ketidakseimbangan kelas. Dalam riset computer vision modern, seperti deteksi anomali atau klasifikasi patologi langka, sampel kelas minoritas bisa tereliminasi sepenuhnya dari salah satu fold. Untuk mencegah hal ini, kita menerapkan stratifikasi. Stratifikasi memastikan proporsi setiap kelas tetap konsisten di setiap lipatan, sehingga setiap fold merepresentasikan distribusi populasi secara utuh. Ini menjadi wajib dilakukan sebelum mengevaluasi arsitektur deep learning pada domain dengan label tidak merata.

Selain stratifikasi, ada aspek struktural data yang sering diabaikan dalam benchmarking standar. Jika dataset Anda terdiri dari gambar atau video yang berasal dari subjek, kamera, atau sesi pengambilan yang sama, asumsi independensi antar sampel otomatis gugur. Memisahkan sampel secara acak akan menyebabkan data dari subjek yang sama tersebar di training dan validation, yang secara artifisial meningkatkan skor evaluasi. Solusi rigorusnya adalah menggunakan Grouped Split, seperti `GroupKFold` atau `LeaveOneGroupOut` di ekosistem Python. Parameter grouping harus diidentifikasi melalui metadata unik seperti ID subjek, hash folder sumber, atau timestamp sesi, sehingga seluruh data dari satu grup hanya muncul di fold yang sama.

Penerapan strategi splitting yang ketat bukan sekadar teknik pra-pemrosesan, melainkan fondasi utama dari desain eksperimen yang solid. Ketika struktur data tidak dipisahkan dengan benar, risiko kebocoran informasi meningkat drastis dan hasil benchmarking menjadi menyesatkan. Hal ini membawa kita langsung ke pembahasan berikutnya, yaitu bagaimana mendefinisikan, mengklasifikasikan, dan menghindari berbagai jenis data leakage yang sering menggerogoti integritas publikasi di bidang pengolahan citra digital terkini.

---

## Slide 011 - Data Leakage: Definisi dan Jenis

### Narasi

Pada slide ini, kita membahas konsep fundamental yang sering menjadi sumber kesalahan fatal dalam desain eksperimen pengolahan citra digital, yaitu *data leakage*. Secara definisi, kebocoran data terjadi ketika informasi dari luar set pelatihan berhasil masuk ke dalam proses pelatihan atau seleksi model. Konsekuensi utamanya adalah menghasilkan metrik evaluasi yang terlihat sangat baik, namun sebenarnya bersifat optimistis secara palsu. Dalam konteks penelitian tingkat doktoral, hal ini dapat menyesatkan klaim kontribusi ilmiah karena performa model tidak mencerminkan kemampuan generalisasi yang sesungguhnya di lingkungan produksi atau aplikasi nyata.

Untuk mengidentifikasi akar masalahnya, kita perlu memahami lima jenis kebocoran yang paling sering muncul dalam literatur computer vision:
- *Target leakage*: Terjadi ketika fitur input mengandung informasi yang secara langsung atau tidak langsung sudah mengetahui label target. Contoh pada PCD adalah menyertakan bounding box ground truth atau mask segmentasi sebagai fitur tambahan saat melatih classifier.
- *Train-test contamination*: Statistik deskriptif, histogram, atau parameter estimasi dihitung menggunakan seluruh dataset sebelum dilakukan pemisahan train dan test, sehingga informasi test "bocor" ke fase training.
- *Duplicate images*: Gambar yang identik atau hampir identik tersebar di kedua split. Hal ini sangat kritis pada dataset visual karena model cenderung menghafal tekstur, watermark, atau artefak kamera spesifik, bukan mempelajari pola semantik yang relevan.
- *Temporal leakage*: Menggunakan data masa depan untuk memprediksi kondisi masa lalu, umum ditemui pada analisis video, drone imagery, atau studi longitudinal pencitraan medis.
- *Preprocessing leakage*: Tahap normalisasi, reduksi dimensi seperti PCA, atau estimasi distribusi dilakukan pada seluruh data sebelum split, sehingga statistik global test set secara implisit mempengaruhi transformasi data training.

Pembahasan ini merupakan kelanjutan logis dari slide sebelumnya mengenai K-Fold Cross-Validation dan stratifikasi. Meskipun teknik validasi silang yang tepat membantu mengurangi varians evaluasi dan meningkatkan stabilitas metrik, metode tersebut tidak otomatis mencegah *data leakage* jika pipeline preprocessing atau augmentasi tidak dirancang dengan ketat. Tanpa isolasi yang benar-benar terjaga antar lipatan, bahkan strategi stratifikasi sekalipun bisa gagal mendeteksi kebocoran informasi, terutama pada dataset yang memiliki struktur hierarkis atau dependensi spasial.

Memahami jenis-jenis kebocoran ini menjadi prasyarat metodologis sebelum kita menelusuri implementasinya secara praktis. Pada slide berikutnya, kita akan mengaitkan setiap jenis kebocoran tersebut dengan sumber spesifik yang sering muncul dalam pipeline pengolahan citra digital modern, mulai dari normalisasi global, fine-tuning model pretrained, hingga duplikasi tersembunyi pada dataset publik besar. Pendekatan ini akan membantu Anda merancang protokol eksperimen yang lebih robust, transparan, dan siap direproduksi oleh komunitas riset internasional.

---

## Slide 012 - Sumber Leakage pada Pipeline PCD

### Narasi

Pada slide sebelumnya, kita telah menguraikan definisi data leakage dan klasifikasinya mulai dari target leakage hingga preprocessing leakage. Fokus kita kini beralih ke implementasi praktis: di mana tepatnya kebocoran informasi tersebut最常 terjadi dalam pipeline pengolahan citra digital dan computer vision modern. Identifikasi sumber leakage ini menjadi prasyarat mutlak bagi desain eksperimen yang rigor, terutama pada level penelitian doktor yang menuntut transparansi metodologis dan reproduktibilitas hasil.

Pertama, normalisasi global sering kali diterapkan secara naif. Ketika mean dan standar deviasi dihitung dari seluruh dataset sebelum dilakukan train-validation-test split, distribusi statistik dari fold validation maupun test secara tidak langsung ikut terbawa ke dalam proses penyesuaian parameter. Model seolah-olah telah "melihat" karakteristik distribusi data uji, sehingga skor evaluasi yang dihasilkan bersifat optimistis secara palsu.

Kedua, augmentasi data yang tidak dikontrol ketat dapat menciptakan kontaminasi implisit. Transformasi deterministik atau parameter augmentasi yang terlalu agresif dapat menghasilkan sampel uji yang secara geometris atau tekstural hampir identik dengan sampel latih. Akibatnya, peningkatan metrik performa lebih mencerminkan kemampuan model menghafal variasi augmentasi daripada menangkap pola semantik yang generalizable.

Ketiga, fine-tuning model pretrained memerlukan audit overlap dataset yang cermat. Banyak backbone modern seperti CLIP, DINOv2, atau timm architectures dilatih pada corpus publik berskala besar. Jika dataset riset Anda mengandung gambar yang visually atau semantically tumpang tindih dengan data pretraining tersebut, maka transfer knowledge yang terjadi sebenarnya memanfaatkan informasi yang seharusnya tidak tersedia saat inference nyata. Ini merupakan bentuk leakage kontekstual yang sulit terdeteksi tanpa protokol deduplication yang ketat.

Keempat, feature selection atau pemilihan representasi sebelum split data melanggar prinsip independensi evaluasi. Menentukan subset fitur atau channel mana yang paling informatif berdasarkan seluruh kumpulan data berarti Anda telah menggunakan informasi dari fold uji untuk memandu arsitektur atau seleksi model. Cross-validation yang dijalankan setelahnya akan menghasilkan estimasi bias yang sistematis.

Kelima, teknik pra-pemrosesan citra seperti masking, inpainting, atau interpolasi yang mengandalkan statistik global dari seluruh citra atau dataset dapat menimbulkan kebocoran halus. Ketika nilai piksel yang hilang atau noise diestimasi berdasarkan distribusi keseluruhan gambar, informasi dari region yang seharusnya diisolasi sebagai data uji justru tersimpan dalam proses rekonstruksi itu sendiri, mengaburkan batas antara sinyal asli dan artefak pemrosesan.

Keenam, keberadaan duplikat atau near-duplicate images dalam dataset publik seperti CIFAR-10, ImageNet, hingga koleksi medical imaging harus selalu diverifikasi. Random split konvensional sering kali gagal memisahkan gambar yang berasal dari subjek, lokasi pengambilan, atau sesi kamera yang sama. Tanpa mekanisme grouping atau filtering berbasis hash/perceptual similarity, model dapat mencapai akurasi tinggi hanya dengan menghafal variasi minor dari objek yang sama, bukan kemampuan generalisasi yang sesungguhnya.

Dengan memahami keenam sumber leakage ini, kita telah membangun kesadaran kritis terhadap setiap tahap dalam alur kerja eksperimen. Pada slide berikutnya, kita akan membahas urutan pipeline yang aman serta checklist praktis untuk memastikan setiap komponen preprocessing, splitting, dan evaluasi berjalan secara independen, sehingga hasil benchmarking Anda memenuhi standar reproducible, trustworthy, dan siap dipertanggungjawabkan dalam publikasi internasional bereputasi.

---

## Slide 013 - Menghindari Leakage: Praktik Aman

### Narasi

Pada slide sebelumnya telah diidentifikasi berbagai sumber kebocoran data atau *data leakage* yang sering kali menggerogoti validitas eksperimen dalam pipeline pengolahan citra digital. Untuk memastikan integritas metodologis pada tingkat penelitian doktor, langkah selanjutnya adalah menerapkan praktik pencegahan yang ketat dan sistematis. Slide ini menyajikan urutan eksekusi pipeline yang benar serta daftar periksa (*checklist*) operasional untuk menjamin bahwa setiap komponen evaluasi berjalan secara independen dan bebas dari kontaminasi informasi.

Urutan eksekusi harus dipatuhi secara kaku. Pemisahan dataset menjadi *training*, *validation*, dan *test* harus dilakukan terlebih dahulu sebelum operasi apa pun. Setelah pemisahan, proses *fitting* pada tahap pra-pemrosesan—seperti perhitungan mean dan standar deviasi untuk normalisasi, estimasi parameter augmentasi adaptif, atau kompresi berbasis statistik—hanya boleh dijalankan pada lipatan *training*. Transformasi yang telah dipelajari tersebut kemudian diterapkan secara terpisah ke set validasi dan uji tanpa melakukan *re-fitting*. Hanya setelah isolasi ini tercapai, proses pelatihan model dan evaluasi dapat dimulai. Pola ini mencegah kebocoran statistik maupun distribusi dari set uji ke dalam proses pembelajaran.

Untuk implementasi lapangan, berikut adalah poin-poin kritis yang wajib diverifikasi sebelum peluncuran benchmarking:
- Verifikasi duplikat atau near-duplicate antar split menggunakan teknik pencitraan hash atau metrik kesamaan visual, terutama pada dataset publik seperti ImageNet, CIFAR, atau koleksi medis.
- Manfaatkan `GroupKFold` ketika data memiliki struktur berkelompok alami, misalnya beberapa frame video dari subjek yang sama, multi-view kamera, atau slice MRI dari pasien yang sama, agar kelompok tidak terpecah antar lipatan.
- Simpan dan version control statistik pra-pemrosesan per lipatan guna menjamin reproduktibilitas penuh dan menghindari drift parameter antar eksperimen.
- Dokumentasikan secara rinci versi dataset, tanggal unduhan, skrip pembersihan, dan filter yang diterapkan. Dalam riset tingkat doktoral, jejak data (*data provenance*) adalah fondasi utama klaim ilmiah.
- Jaga isolasi mutlak terhadap set uji selama fase pengembangan. Set uji hanya boleh diakses sekali pada akhir siklus eksperimen untuk laporan hasil akhir.

Dengan menerapkan protokol pencegahan kebocoran ini, fondasi eksperimental menjadi kokoh dan siap untuk tahap perbandingan metodologis. Pada slide berikutnya, kita akan membahas prinsip pemilihan baseline yang kuat dan wajar, termasuk bagaimana menetapkan metode state-of-the-art sebagai pembanding yang adil, menyetel konfigurasi secara setara, serta menilai signifikansi statistik perbedaan performa. Hal ini akan melengkapi kerangka desain eksperimen yang rigor untuk publikasi internasional bereputasi.

---

## Slide 014 - Baseline Selection: Prinsip Baseline Terkuat yang Wajar

### Narasi

Setelah kita memastikan integritas data melalui pencegahan *data leakage* pada slide sebelumnya, langkah kritis berikutnya dalam desain eksperimen adalah menetapkan *baseline* yang kredibel. Pemilihan *baseline* bukan sekadar pembanding biasa, melainkan fondasi evaluasi yang menentukan validitas klaim kontribusi ilmiah Anda.

Sebuah *baseline* yang baik harus memenuhi empat kriteria utama:
- Metode harus merupakan state-of-the-art yang relevan langsung dengan tugas pengolahan citra yang diteliti.
- Kode sumber harus tersedia atau dapat diimplementasikan ulang dengan akurasi tinggi tanpa modulasi sepihak.
- Penyetelan *hyperparameter* harus dilakukan secara adil, mengikuti protokol validasi yang identik dengan metode usulan.
- Hindari sepenuhnya pembuatan *strawman*, yaitu baseline yang sengaja dikonfigurasi lemah agar metode baru terlihat lebih unggul. Praktik ini bertentangan dengan standar etika penelitian tingkat doktoral.

Untuk menguji ketahanan metodologi Anda, jawab tiga pertanyaan kunci secara eksplisit:
- Apa metode terbaik yang sudah mapan untuk masalah spesifik ini?
- Apakah baseline dijalankan dengan konfigurasi dan beban komputasi yang sama adilnya dengan metode usulan?
- Apakah perbedaan hasil yang diperoleh signifikan secara statistik, atau hanya noise dalam distribusi data?

Pemahaman prinsip seleksi ini akan langsung dioperasionalkan pada slide berikutnya, di mana kita akan menyusun daftar baseline standar untuk berbagai tugas inti dalam Pengolahan Citra Digital, mencakup klasifikasi, deteksi, segmentasi, restorasi, serta representasi dan model multimodal.

---

## Slide 015 - Menyusun Baseline untuk Tugas PCD

### Narasi

Pada slide sebelumnya, kita telah membahas prinsip-prinsip pemilihan baseline yang kuat dan wajar, termasuk pentingnya menghindari strawman dan memastikan konfigurasi eksperimen yang adil. Langkah selanjutnya dalam desain eksperimen yang rigor adalah menerjemahkan prinsip tersebut ke dalam praktik konkret sesuai dengan jenis tugas pengolahan citra digital yang Anda kerjakan. Slide ini menyajikan panduan penyusunan baseline berdasarkan kategori tugas utama dalam computer vision modern.

Panduan penyusunan baseline ini perlu disesuaikan dengan kategori tugas spesifik dalam penelitian Anda. Berikut adalah rekomendasi baseline yang relevan dengan perkembangan terkini:
- **Klasifikasi citra**: ResNet, EfficientNet, dan Vision Transformer (ViT) sebagai acuan arsitektur klasik hingga transformer-based.
- **Object detection**: YOLO, DETR, dan Faster R-CNN yang mewakili spektrum dari deteksi real-time hingga end-to-end transformer.
- **Segmentasi**: U-Net untuk arsitektur dasar, DeepLab untuk konteks multi-scale, dan SAM sebagai foundation model segmentasi generik.
- **Image restoration**: BM3D sebagai referensi klasik, SwinIR untuk modeling hierarkis, serta restorasi berbasis diffusion yang mendominasi state-of-the-art.
- **Self-supervised representation**: DINO, DINOv2, dan MAE sebagai standar baru dalam pembelajaran tanpa label.
- **Multimodal**: CLIP dan BLIP untuk evaluasi alignment vision-language.

Implementasi baseline ini menuntut disiplin tinggi dalam hal reproduksibilitas dan transparansi komputasi. Selalu gunakan pretrained weights resmi dari sumber terpercaya, dan cantumkan secara eksplisit lokasi unduhan atau repository aslinya dalam metodologi penelitian. Jika baseline tertentu tidak menyediakan kode publik yang dapat dipercaya, implementasi ulang harus divalidasi secara ketat dengan mereproduksi hasil numerik dari paper asli sebelum digunakan sebagai pembanding. Selain itu, laporkan jumlah parameter dan FLOPs setiap baseline. Hal ini bukan sekadar formalitas, melainkan prasyarat fundamental untuk menilai fairness komputasi dan memastikan bahwa peningkatan akurasi yang Anda klaim tidak diperoleh dengan mengorbankan efisiensi yang tidak proporsional.

Dengan baseline yang telah tersusun rapi, terverifikasi, dan dicatat metrik komputasinya, langkah logis berikutnya adalah menguji kontribusi masing-masing komponen dalam model usulan Anda. Ini akan membawa kita pada pembahasan tentang ablation study, di mana kita akan membedah bagaimana penghilangan, penambahan, atau penggantian modul memengaruhi performa akhir secara kuantitatif dan membantu membangun argumen ilmiah yang kuat untuk kontribusi penelitian tingkat doktor.

---

## Slide 016 - Ablation Study: Konsep dan Peran

### Narasi

Setelah kita menetapkan baseline yang solid pada slide sebelumnya, langkah kritis berikutnya dalam desain eksperimen adalah melakukan ablation study. Ablation study merupakan metode eksperimental yang dirancang untuk mengisolasi dan mengukur kontribusi spesifik dari setiap komponen dalam sebuah arsitektur atau pipeline pengolahan citra digital. Dalam konteks penelitian tingkat doktoral, pendekatan ini bukan sekadar praktik rutin, melainkan fondasi utama untuk membuktikan klaim novelti dan validitas metodologi yang Anda ajukan.

Secara definisi, ablation study dilakukan dengan cara menghilangkan atau mengganti satu atau lebih komponen model, lalu mengamati dampaknya terhadap metrik performa. Hal ini memungkinkan peneliti membedakan antara peningkatan hasil yang benar-benar berasal dari ide baru versus yang hanya disebabkan oleh faktor lain seperti kapasitas model yang lebih besar atau tuning hyperparameter yang tidak terkontrol.

Terdapat tiga jenis pendekatan ablation yang umum digunakan dalam literatur computer vision terkini:
- **Subtractive**: Komponen diusulkan dihilangkan dari model lengkap untuk melihat apakah performanya tetap stabil atau justru menurun signifikan.
- **Additive**: Komponen tambahan ditambahkan ke model dasar (baseline) untuk menguji apakah penambahan tersebut memberikan gain yang relevan.
- **Replacement**: Satu komponen diganti dengan alternatif lain yang memiliki fungsi serupa namun mekanisme berbeda, misalnya mengganti aktivasi standar dengan varian non-linear yang diusulkan.

Sebagai ilustrasi konkret, misalkan model lengkap Anda terdiri dari Encoder, Attention Module, dan Loss Function B. Pada ablation pertama, Anda mengevaluasi Encoder bersama Loss B tanpa Attention Module, guna mengukur seberapa besar perhatian spasial atau kanal tersebut berkontribusi pada akurasi tugas. Pada ablation kedua, Anda mengganti Loss B dengan Loss A sambil mempertahankan Attention Module, sehingga isolasi efek loss function dapat dianalisis secara terpisah.

Penting untuk dicatat bahwa contoh di atas hanyalah skema konseptual. Dalam praktiknya, setiap varian ablation harus dieksekusi dengan protokol yang ketat agar perbandingan tetap adil. Pembahasan mengenai bagaimana merancang konfigurasi ablation yang valid, termasuk prinsip pengendalian variabel, manajemen seed, serta kesalahan metodologis yang sering mengancam integritas benchmark, akan kita bahas secara mendalam pada slide berikutnya.

---

## Slide 017 - Desain Ablation yang Valid

### Narasi

Merujuk pada pembahasan slide sebelumnya mengenai definisi dan klasifikasi ablation study, kita kini masuk ke aspek metodologis yang paling menentukan kredibilitas hasil penelitian: desain ablation yang valid. Pada jenjang doktoral, validitas eksperimen menuntut isolasi variabel yang ketat. Prinsip fundamentalnya sangat jelas: hanya satu faktor yang boleh diubah antar kondisi eksperimen. Seluruh konfigurasi lain harus dikontrol secara konsisten, mencakup initialization seed, pilihan optimizer, jadwal learning rate, hingga total epoch training. Tanpa kontrol ini, fluktuasi performa tidak dapat secara meyakinkan dikaitkan dengan komponen yang sedang diuji.

Lebih lanjut, setiap skema ablation harus secara eksplisit dirancang untuk menguji klaim kontribusi yang diajukan dalam paper. Jika sebuah modul diklaim memberikan peningkatan kinerja karena mekanisme tertentu, maka ablasi harus secara spesifik menonaktifkan atau mengganti modul tersebut tanpa mengubah arsitektur dasar lainnya. Hal ini memastikan bahwa metrik evaluasi benar-benar mencerminkan dampak dari komponen target, bukan artefak dari perubahan struktur model yang tidak terkontrol.

Untuk menjaga integritas ilmiah, hindari beberapa praktik metodologis yang umum terjadi namun merusak validitas eksperimen:
- Mengubah lebih dari satu komponen sekaligus, sehingga isolasi efek menjadi mustahil.
- Menyetel hyperparameter secara berbeda untuk tiap varian model, yang mengacaukan fair comparison.
- Menarik kesimpulan kontribusi hanya berdasarkan satu kali run dengan seed tunggal, mengingat varians acak dalam pelatihan deep learning cukup signifikan.
- Melakukan perbandingan performa tanpa menerapkan koreksi statistik yang memadai.

Pendekatan desain ablation yang ketat ini menjadi prasyarat mutlak sebelum memasuki tahap evaluasi sensitivitas konfigurasi. Sebagaimana akan dibahas pada slide berikutnya, ketidakseimbangan dalam proses tuning—seperti metode usulan yang dioptimalkan melalui ratusan percobaan sementara baseline hanya dijalankan sekali—dapat menciptakan bias sistematis. Konsistensi dalam pengendalian variabel dan transparansi dalam pelaporan konfigurasi akan menjadi fondasi utama bagi reproducible benchmarking yang diakui dalam literatur computer vision terkini.

---

## Slide 018 - Hyperparameter dan Sensitivitas Konfigurasi

### Narasi

Pada slide ini, kita membahas aspek krusial dalam benchmarking eksperimental yang sering kali terlewatkan namun memiliki dampak signifikan terhadap validitas klaim penelitian: kesetaraan dalam penyetelan hyperparameter dan analisis sensitivitas konfigurasi. Masalah utama yang perlu diwaspadai adalah bias akibat ketidakseimbangan budget tuning. Ketika metode usulan dituning melalui ratusan percobaan untuk mencari performa optimal, sementara baseline hanya dijalankan sekali dengan konfigurasi default, perbandingan menjadi tidak adil. Hasil akhir cenderung menguntungkan metode usulan bukan karena keunggulan arsitektur atau algoritmanya, melainkan karena over-tuning yang tidak sebanding.

Untuk menjaga integritas evaluasi, praktik terbaik yang harus diterapkan meliputi beberapa poin kunci:
- Gunakan alokasi komputasi dan usaha tuning yang sama untuk semua metode yang dibandingkan.
- Laporkan secara transparan seluruh rentang hyperparameter yang dicoba selama proses pencarian.
- Manfaatkan random search atau Bayesian optimization untuk efisiensi dan coverage ruang pencarian yang lebih baik.
- Ukur sensitivitas metode terhadap perubahan hyperparameter untuk menilai stabilitas model.
- Selalu cantumkan pasangan hyperparameter terbaik beserta nilai metrik validasi yang dicapai, bukan hanya skor uji akhir.

Pendekatan ini memastikan bahwa keunggulan yang dilaporkan benar-benar berasal dari desain model atau strategi pembelajaran, bukan dari keberuntungan dalam pencarian konfigurasi. Model yang robust akan menunjukkan fluktuasi performa yang minimal meskipun terjadi variasi kecil pada learning rate, weight decay, atau ukuran batch. Transparansi dalam pelaporan konfigurasi juga memudahkan replikasi oleh peneliti lain dan memperkuat posisi karya Anda dalam literatur terkini.

Pembahasan mengenai kesetaraan tuning ini merupakan kelanjutan logis dari prinsip ablation study pada slide sebelumnya. Jika ablation bertujuan mengisolasi kontribusi komponen tertentu dengan mengubah satu faktor saja, maka kontrol hyperparameter memastikan bahwa perbandingan antar varian dilakukan di bawah kondisi yang setara. Setelah konfigurasi distabilkan, kita masih harus menghadapi sumber variabilitas lain yang tak terhindarkan dalam pelatihan model deep learning, yaitu pemilihan random seed, yang akan kita bahis secara mendalam pada slide berikutnya.

---

## Slide 019 - Random Seed dan Variabilitas Training

### Narasi

Setelah membahas pentingnya kesetaraan dalam alokasi budget tuning hyperparameter pada slide sebelumnya, kita kini beralih ke sumber variabilitas lain yang sering kali tersembunyi namun sangat menentukan validitas eksperimen: ketidakkonsistenan akibat penggunaan random seed selama proses pelatihan model.

Variabilitas dalam pelatihan deep learning tidak muncul dari satu penyebab tunggal, melainkan akumulasi dari beberapa mekanisme stokastik yang berjalan simultan:
- Inisialisasi bobot jaringan yang berbeda akan menghasilkan titik awal ruang loss landscape yang unik, sehingga jalur konvergensi optimizer berubah.
- Urutan pengacakan (shuffling) dataset dan pembentukan mini-batch memengaruhi estimasi gradien pada setiap iterasi.
- Lapisan regulasi seperti dropout serta pipeline augmentasi citra yang bersifat probabilistik menambah variasi representasi fitur antar epoch.
- Selain aspek perangkat lunak, implementasi CUDA dan operasi paralel pada GPU modern sering kali bersifat non-deterministik, menyebabkan perbedaan numerik kecil yang dapat terakumulasi hingga mengubah hasil akhir.

Mengandalkan satu kali run saja dengan seed tertentu sama sekali tidak memadai untuk klaim penelitian tingkat doktoral. Pergeseran nilai seed dapat mengubah urutan peringkat antar metode, menutupi interval ketidakpastian performa, dan berisiko menghasilkan kesimpulan palsu mengenai superioritas suatu pendekatan. Benchmarking yang rigor memerlukan pengukuran stabilitas metode terhadap fluktuasi acak ini, bukan sekadar pencatatan angka tunggal.

Oleh karena itu, standar pelaporan dalam publikasi computer vision terkini mewajibkan penyajian hasil sebagai mean ± standar deviasi yang dihitung dari sejumlah percobaan independen. Kewajiban teknis yang setara pentingnya adalah dokumentasi eksplisit: catat, version control, dan publikasikan nilai seed spesifik yang digunakan untuk setiap run agar replikasi eksperimen dapat dilakukan secara transparan oleh komunitas ilmiah.

Prinsip pelaporan statistik ini secara alami mengarah pada pertanyaan metodologis berikutnya: berapa sebenarnya jumlah seed minimum yang harus dijalankan agar estimasi performa tersebut signifikan secara empiris? Pembahasan mengenai aturan praktis penentuan jumlah seed, hubungan antara varians data dengan kebutuhan sampel, serta strategi optimalisasi computational budget akan kita telaah pada slide selanjutnya.

---

## Slide 020 - Berapa Banyak Seed yang Diperlukan?

### Narasi

Pada slide sebelumnya, kita telah menegaskan bahwa satu kali eksekusi training tidak pernah cukup untuk menilai kinerja model secara objektif. Sumber variabilitas seperti inisialisasi bobot, urutan batch, dropout, hingga non-determinisme GPU dapat menggeser ranking metode secara signifikan. Karena itu, standar pelaporan wajib menggunakan rata-rata dan deviasi standar dari beberapa seed. Pertanyaan logis yang muncul setelahnya adalah berapa banyak seed yang benar-benar diperlukan agar hasil benchmarking Anda memiliki kekuatan statistik yang memadai?

Sebagai pedoman praktis, jumlah seed yang dijalankan harus dikalibrasi berdasarkan fase dan ambiguitas eksperimen. Untuk tahap debugging atau validasi pipeline, satu seed sudah mencukupi. Namun, untuk eksperimen inti yang menjadi fondasi klaim penelitian, minimal tiga hingga lima seed harus dijalankan. Jika selisih metrik antar metode sangat tipis atau varians hasil tinggi, perluasan ke lima hingga sepuluh seed menjadi keharusan. Sementara itu, untuk meta-analisis atau klaim yang akan diajukan ke konferensi/jurnal top-tier, penggunaan sepuluh seed atau lebih telah menjadi praktik standar di komunitas riset mutakhir.

Pertimbangan teknis lainnya terletak pada trade-off antara stabilitas estimasi dan biaya komputasi. Semakin banyak seed yang digunakan, semakin rapat estimasi mean terhadap distribusi performa sebenarnya. Sebaliknya, ketika varians hasil besar, Anda memerlukan sampel acak yang lebih banyak untuk menutupi rentang ketidakpastian tersebut. Dalam praktik nyata, jumlah ini harus selalu disesuaikan dengan computational budget yang tersedia. Riset tingkat doktoral menuntut efisiensi, bukan pengorbanan rigor ilmiah; artinya, Anda merancang protokol yang optimal tanpa pemborosan sumber daya GPU yang tidak perlu.

Setelah jumlah seed ditetapkan, langkah kritis berikutnya adalah mengkuantifikasi seberapa andal estimasi tersebut. Slide berikutnya akan membahas Confidence Interval sebagai alat ukur ketidakpastian. Kita akan melihat bagaimana menghitung interval kepercayaan melalui pendekatan normal maupun bootstrap, serta mengapa pelaporan error bar berbasis CI jauh lebih informatif daripada sekadar menyajikan nilai p atau rata-rata tunggal dalam tabel dan grafik hasil benchmarking.

---

## Slide 021 - Confidence Interval: Mengukur Ketidakpastian Estimasi

### Narasi

Setelah menentukan jumlah replikasi atau seed yang memadai pada slide sebelumnya, langkah metodologis berikutnya adalah mengkuantifikasi ketidakpastian dari estimasi performa model kita. Di sinilah Confidence Interval atau selang kepercayaan menjadi komponen wajib dalam desain eksperimen yang rigor. Secara definisi, CI merupakan interval yang diperkirakan memuat nilai parameter populasi sebenarnya dengan tingkat kepercayaan tertentu. Misalnya, laporan CI 95% mengindikasikan bahwa jika eksperimen diulang secara independen berkali-kali, sekitar 95% dari selang yang dihitung akan mencakup nilai metrik evaluasi yang sesungguhnya di populasi data.

Untuk menghitung CI secara praktis dalam pipeline benchmarking, terdapat dua pendekatan utama yang dapat dipilih berdasarkan karakteristik data dan sumber daya komputasi:
- **Normal approximation**: Menggunakan rumus `mean ± 1.96 * (std / sqrt(n))`. Pendekatan ini efisien dan cukup akurat ketika jumlah replikasi sudah besar serta distribusi error mendekati normal.
- **Bootstrap**: Dilakukan dengan sampling ulang dengan penggantian terhadap kumpulan skor metrik dari berbagai seed. Batas bawah dan atas diambil dari persentil ke-2.5 dan ke-97.5. Metode ini lebih robust terhadap distribusi non-normal dan varians tinggi yang sering muncul pada arsitektur deep learning kompleks.

Dalam standar publikasi tingkat S3 dan konferensi internasional, pelaporan hasil harus selalu menyertakan error bar atau confidence interval pada setiap tabel perbandingan dan grafik visualisasi. Menyajikan nilai rata-rata tunggal tanpa indikator dispersi dianggap kurang informatif dan rentan terhadap misinterpretasi. Confidence interval memberikan gambaran presisi estimasi yang jauh lebih bermakna dibandingkan hanya mengandalkan nilai p, sekaligus menegaskan transparansi dalam reproduktibilitas eksperimen.

Dengan ketidakpastian yang telah terukur dan dilaporkan secara eksplisit, fondasi statistik untuk tahap perbandingan model sudah terbangun kuat. Langkah logis selanjutnya adalah memilih uji statistik yang sesuai dengan struktur data dan asumsi distribusi, sehingga klaim keunggulan suatu metode dapat dibuktikan secara signifikan dan dapat dipertanggungjawabkan secara ilmiah.

---

## Slide 022 - Statistical Testing: Memilih Uji yang Tepat

### Narasi

Setelah kita mempelajari cara mengukur ketidakpastian estimasi melalui Confidence Interval pada slide sebelumnya, langkah logis berikutnya dalam desain eksperimen adalah menentukan uji statistik yang tepat untuk memvalidasi perbedaan performa antar model. Pemilihan uji ini tidak boleh dilakukan secara sembarangan, melainkan harus didasarkan pada struktur data, jumlah kelompok yang dibandingkan, dan asumsi distribusi yang terpenuhi.

Berikut adalah panduan pemilihan uji statistik berdasarkan situasi umum dalam benchmarking pengolahan citra:
- **Dua model dengan data terpasang (paired):** Gunakan Paired t-test jika data berdistribusi normal, atau Wilcoxon signed-rank test sebagai alternatif non-parametriknya.
- **Perbandingan beberapa model:** Terapkan ANOVA untuk deteksi awal adanya perbedaan signifikan, yang kemudian wajib dilanjutkan dengan uji post-hoc seperti Tukey atau Bonferroni.
- **Tabel klasifikasi 2x2:** Uji McNemar adalah standar de facto untuk membandingkan dua classifier pada dataset yang sama.
- **Proporsi atau akurasi:** Pendekatan Bootstrap CI atau uji binomial umumnya lebih robust karena tidak mengandalkan asumsi distribusi normal pada metrik diskrit.

Prinsip fundamental yang harus selalu dipegang adalah bahwa uji statistik tidak pernah menggantikan ukuran efek. Nilai p saja tidak memberikan informasi tentang magnitudo perbedaan praktis. Oleh karena itu, selalu laporkan effect size bersama Confidence Interval-nya. Selain itu, ketika melakukan banyak pengujian simultan, koreksi untuk multiple comparisons seperti Bonferroni, Tukey, atau Holm wajib diterapkan untuk mengendalikan family-wise error rate dan meminimalkan risiko false positive.

Penerapan kerangka kerja statistik yang ketat ini akan memastikan bahwa klaim kinerja model Anda memiliki validitas empiris yang kuat. Namun, dalam praktik penelitian tingkat lanjut, tidak semua perbandingan akan menghasilkan signifikansi statistik. Pada slide berikutnya, kita akan membahas strategi interpretasi terhadap hasil negatif atau non-significant, termasuk langkah diagnostik sistematis dan etika pelaporan yang berkontribusi pada reproduktibilitas penelitian.

---

## Slide 023 - Interpretasi Hasil Negatif dan Non-Significant

### Narasi

Setelah kita menentukan uji statistik yang tepat pada slide sebelumnya, langkah kritis berikutnya adalah memahami cara menginterpretasikan hasil yang keluar dari eksperimen tersebut. Dalam penelitian tingkat doktoral, penting untuk menegaskan bahwa hasil negatif atau non-signifikant bukanlah kegagalan metodologis, melainkan temuan ilmiah yang sah. Metode yang kita usulkan tidak dijamin selalu melampaui baseline, dan penerimaan terhadap fakta ini justru menjadi cerminan integritas penelitian yang matang.

Pelaporan hasil negatif secara sistematis berkontribusi langsung dalam mengurangi *publication bias* yang sering kali mendominasi literatur computer vision. Ketika hanya pencapaian positif yang dipublikasikan, komunitas ilmiah kehilangan peta batas-batas kemampuan suatu pendekatan. Beberapa penyebab umum hasil negatif meliputi baseline yang sudah sangat kompetitif, daya statistik yang rendah akibat jumlah *seed* atau replikasi yang minim, potensi *bug* dalam implementasi, atau bahkan hipotesis awal yang kurang sesuai dengan karakteristik data target.

Jika eksperimen menghasilkan nilai non-signifikant, hindari impuls untuk mengubah metode secara sembarangan. Lakukan audit sistematis melalui langkah-langkah berikut:
1. Periksa kembali *pipeline* preprocessing dan training untuk memastikan tidak ada *data leakage*.
2. Tinjau ulang konfigurasi hiperparameter serta alokasi *budget* komputasi yang telah ditetapkan.
3. Evaluasi daya statistik (*statistical power*) dengan mempertimbangkan penambahan jumlah *seed* atau sampel replikasi.

Apabila setelah investigasi menyeluruh hasil tetap negatif, laporkan temuan tersebut secara transparan. Diskusikan implikasi teoretis dan praktisnya, misalnya mengapa baseline bertahan unggul atau apakah asumsi fundamental model perlu ditinjau ulang. Transparansi seperti ini justru memperkuat posisi karya Anda saat menghadapi proses *peer-review* di jurnal atau konferensi bereputasi.

Pembahasan mengenai interpretasi hasil ini secara inheren terkait dengan manajemen sumber daya eksperimen. Pada slide berikutnya, kita akan menyoroti bagaimana *computational budget* yang terukur memengaruhi keputusan desain eksperimen, pemilihan baseline, hingga klaim efisiensi algoritma. Dokumentasi lengkap mengenai spesifikasi perangkat keras, kompleksitas model, durasi eksekusi, dan total beban komputasi bukan sekadar pelengkap, melainkan prasyarat mutlak untuk mewujudkan *reproducible benchmarking* yang menjadi standar emas dalam publikasi riset terkini.

---

## Slide 024 - Computational Budget dan Reproducibility

### Narasi

Setelah membahas strategi menginterpretasikan hasil negatif atau non-signifikans pada slide sebelumnya, kita kini menyoroti variabel desain eksperimen yang sering kali menjadi akar ketidakreproduksibelan dalam penelitian pengolahan citra dan computer vision, yaitu *computational budget*. Anggaran komputasi bukan sekadar kendala logistik, melainkan faktor kritis yang menentukan kualitas dan kredibilitas temuan ilmiah.

Ketersediaan sumber daya secara langsung memengaruhi pilihan baseline dan jumlah *seed* yang dapat diuji. Ketika anggaran terbatas, peneliti sering kali terpaksa memangkas variasi eksperimen atau menggunakan baseline yang kurang kompetitif, yang berpotensi menurunkan daya statistik dan meningkatkan varians hasil. Di sisi lain, klaim efisiensi sebuah metode baru juga harus dievaluasi secara proporsional terhadap biaya komputasinya. Peningkatan akurasi yang marginal tidak selalu membenarkan kebutuhan memori GPU atau waktu pelatihan yang jauh lebih besar, terutama jika tujuannya adalah pengembangan model yang scalable untuk aplikasi dunia nyata.

Untuk memastikan transparansi dan kemudahan replikasi oleh peneliti lain, pelaporan aspek komputasi harus dilakukan secara sistematis. Berikut adalah elemen kunci yang wajib disertakan dalam laporan eksperimen:
- Spesifikasi perangkat keras: tipe GPU/CPU, model chipset, dan kapasitas RAM.
- Durasi pelatihan dan total jam komputasi untuk seluruh rangkaian eksperimen.
- Metrik efisiensi model: jumlah parameter, estimasi FLOPs, serta waktu inferensi per sampel.
- Alat pemantauan energi atau jejak karbon komputasi, jika tersedia, untuk mendukung praktik *green AI*.

Dokumentasi teknis yang rinci ini akan menjadi fondasi struktural ketika kita masuk ke penyusunan protokol eksperimen pada slide berikutnya. Dengan environment, hardware, dan alokasi sumber daya yang terdokumentasi dengan baik, mahasiswa dapat merancang benchmarking yang tidak hanya robust secara statistik, tetapi juga memenuhi standar reproduksibilitas tinggi yang diharapkan dalam publikasi tingkat internasional.

---

## Slide 025 - Protokol Eksperimen: Struktur dan Isi

### Narasi

Setelah menelaah pentingnya alokasi *computational budget* dan metrik pelaporan pada slide sebelumnya, kita kini masuk ke tahap operasionalisasi penelitian melalui struktur protokol yang ketat. Pada jenjang doktoral, klaim ilmiah hanya dapat dipertanggungjawabkan jika seluruh alur eksperimen didokumentasikan secara sistematis. Slide ini menyajikan kerangka kerja standar yang terdiri dari dua belas komponen esensial untuk menjamin transparansi dan kemampuan replikasi.

Komponen pertama hingga keempat membangun fondasi metodologis. Anda harus merumuskan tujuan yang secara eksplisit menjawab *research question* dan menguji hipotesis spesifik. Dokumentasi dataset meliputi sumber resmi, lisensi, jumlah sampel, serta skema pembagian data (*split*). Tahap preprocessing dan augmentasi gambar harus dijabarkan secara teknis, karena transformasi spasial atau domain adaptation dapat mengubah distribusi data secara signifikan. Spesifikasi model mencakup arsitektur inti, mekanisme inisialisasi bobot, serta status *pretrained weights* yang digunakan.

Komponen kelima hingga kedelapan mengatur proses validasi dan perbandingan. Detail pelatihan mencakup pilihan optimizer, *learning rate schedule*, ukuran batch, dan jumlah epoch. Daftar baseline pembanding wajib dicantumkan untuk memastikan evaluasi yang adil terhadap klaim efisiensi atau akurasi. Rancangan ablation study harus menjelaskan varian konfigurasi mana saja yang diuji untuk mengisolasi kontribusi masing-masing modul. Pemilihan metrik evaluasi perlu dipisahkan antara metrik utama sebagai indikator keberhasilan novelitas, dan metrik sekunder untuk analisis komplementer.

Komponen kesembilan hingga kedua belas menangani aspek statistik, lingkungan, dan manajemen proyek. Jumlah *random seed* yang digunakan, prosedur uji signifikansi statistik, serta laporan *confidence interval* harus dinyatakan eksplisit untuk menghindari bias acak. Spesifikasi lingkungan komputasi, mulai dari hardware, versi pustaka, hingga seed awal, harus diarsipkan lengkap. Seluruh artefak penelitian—kode sumber, log eksekusi, dan checkpoint model—harus disimpan dengan rapi. Estimasi jadwal dan alokasi sumber daya memberikan peta jalan yang realistis bagi keberlanjutan riset.

Kerangka protokol ini berfungsi sebagai kontrak ilmiah yang menghubungkan perencanaan teoretis dengan eksekusi teknis. Ketika struktur dokumen telah mapan, tantangan berikutnya adalah menerjemahkan protokol tersebut ke dalam sistem yang otomatis, terkontrol, dan bebas dari kesalahan ketik manusia. Pembahasan selanjutnya akan mengarah pada praktik *configuration management* dan *versioning*, di mana penggunaan file YAML, integrasi Git, serta isolasi lingkungan eksekusi akan diuraikan secara teknis untuk memastikan setiap variabel eksperimen dapat dilacak hingga ke baris kode paling dasar.

---

## Slide 026 - Configuration Management dan Versioning

### Narasi

Setelah pada slide sebelumnya kita menyusun kerangka lengkap protokol eksperimen mulai dari perumusan hipotesis, spesifikasi dataset, hingga estimasi sumber daya dan artefak yang diharapkan, langkah operasional berikutnya adalah menerjemahkan kerangka tersebut ke dalam praktik manajemen konfigurasi dan versioning yang ketat. Pada jenjang doktoral, reproduktibilitas bukanlah pilihan, melainkan standar metodologis yang harus dibuktikan melalui jejak digital yang transparan dan terstruktur.

Praktik inti yang harus diterapkan meliputi poin-poin berikut:
- Simpan seluruh parameter eksperimen dalam file konfigurasi eksternal berformat YAML atau JSON.
- Versioning seluruh kode sumber menggunakan Git sejak awal pengembangan.
- Catat hash commit secara eksplisit untuk setiap run eksperimen yang dilaporkan.
- Isolasi lingkungan komputasi menggunakan `requirements.txt`, `environment.yml`, atau container Docker.

Berikut adalah contoh implementasi konfigurasi dalam format YAML:

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

Struktur hierarkis ini mengelompokkan parameter ke dalam domain yang jelas: dataset, model, training, dan evaluation. Pemisahan nilai dari logika kode memungkinkan iterasi hyperparameter tanpa menyentuh basis kode inti, sekaligus meminimalkan risiko human error saat replikasi. Framework modern seperti Hydra atau OmegaConf dapat membaca struktur ini secara otomatis, sehingga mapping antara protokol tertulis dan eksekusi script menjadi satu-satu. Pastikan setiap field dalam file ini selaras dengan poin metrik, statistik, dan lingkungan pada template protokol Anda.

Di samping konfigurasi, versioning kode harus dipadukan dengan pencatatan hash commit untuk setiap eksperimen. Hash commit berfungsi sebagai identifier unik yang mengikat hasil evaluasi terhadap versi spesifik dari repositori. Hal ini menjadi krusial ketika melakukan ablation study, validasi silang, atau reproduksi hasil paper referensi, karena perbedaan minor pada implementasi layer atau utility function dapat menggeser distribusi metrik secara signifikan. Kombinasi dengan environment management seperti Docker atau Conda memastikan bahwa dependensi seperti PyTorch, torchvision, scikit-image, dan library pendukung lainnya berjalan pada versi yang persis sama lintas mesin, menghilangkan variabel ambien yang sering menjadi penyebab irreproducibility.

Dengan fondasi konfigurasi yang terdokumentasi dan kode yang terverifikasi melalui version control, kita telah menyelesaikan tahap persiapan administratif dan teknis. Pada slide berikutnya, materi akan berlanjut ke integrasi elemen-elemen ini ke dalam pipeline eksperimental yang sepenuhnya reproducible menggunakan skrip modular Python, pengaturan seed yang terpusat, serta mekanisme logging dan penyimpanan artefak yang terotomatisasi.

---

## Slide 027 - Pipeline Eksperimen Reproducible dengan Python

### Narasi

Slide ini memetakan arsitektur pipeline eksperimen yang dirancang khusus untuk menjamin reproduktibilitas penuh dalam penelitian pengolahan citra digital dan computer vision. Diagram horizontal menunjukkan lima tahapan berurutan: Data Loader, Preprocessing, Split, Training, dan Evaluation. Di bawah setiap tahapan terdapat dependensi teknis yang krusial. Tahap data loader dan preprocessing dikontrol oleh file konfigurasi eksternal, sementara transformasi visual diterapkan secara terpusat. Pembagian dataset memanfaatkan strategi seperti GroupKFold untuk mencegah kebocoran data antar split, terutama ketika citra memiliki ketergantungan spasial, temporal, atau subjektif. Proses training mengandalkan penetapan seed yang ketat dan pencatatan log yang sistematis, sedangkan evaluasi tidak hanya melaporkan metrik titik, melainkan juga interval kepercayaan untuk mengkuantifikasi variabilitas performa model.

Penerapan pipeline ini memerlukan disiplin metodologis melalui lima langkah praktis berikut:
1. Struktur proyek harus dimodularisasi menjadi skrip terpisah seperti `data.py`, `train.py`, dan `evaluate.py`. Pemisahan ini mempermudah isolasi variabel, pengujian komponen secara independen, serta replikasi oleh peneliti lain.
2. Parameter eksperimen dikelola melalui `argparse` atau file konfigurasi, memungkinkan variasi hyperparameter tanpa modifikasi kode sumber inti.
3. Seed ditetapkan di baris pertama setiap eksekusi untuk menstabilkan generator pseudo-random pada Python, NumPy, dan library deep learning.
4. Log pelatihan dan hasil evaluasi diekspor ke file teks atau JSON yang dinamai unik berdasarkan timestamp atau identifier eksperimen, menghindari pencampuran hasil antar run.
5. Setiap iterasi eksperimen disertai commit Git yang mencakup kode sumber beserta file konfigurasi, menciptakan jejak audit yang transparan. Praktik ini merupakan kelanjutan langsung dari konsep manajemen konfigurasi dan versioning yang telah dibahas pada slide sebelumnya.

Detail implementasi teknis, khususnya fungsi penyetelan seed lintas library dan pengaturan deterministik PyTorch, akan diuraikan secara eksplisit pada slide berikutnya. Dengan mengintegrasikan pipeline modular, konfigurasi terpusat, dan kontrol randomness yang ketat, peneliti dapat menyusun benchmarking yang memenuhi standar rigor akademik tingkat doktoral. Hasil eksperimen yang dihasilkan tidak hanya valid secara statistik, tetapi juga siap untuk diverifikasi, direplikasi, dan dijadikan baseline yang solid bagi pengembangan metode baru di bidang computer vision.

---

## Slide 028 - Contoh Kode: Konfigurasi dan Seed

### Narasi

Pada slide ini, kita masuk ke tahap implementasi teknis yang menjamin konsistensi dalam desain eksperimen: pengelolaan konfigurasi dan penyetelan seed. Langkah ini merupakan kelanjutan alami dari pipeline modular yang telah disusun pada slide sebelumnya. Tanpa kontrol yang ketat terhadap sumber keacakan dan parameter, perbedaan kecil pada urutan pembacaan data atau inisialisasi bobot dapat menghasilkan fluktuasi metrik yang signifikan, sehingga menyulitkan validasi klaim novelty atau perbaikan performa model.

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

Fungsi `set_seed` mengunci generator pseudorandom pada tiga lapisan komputasi yang dominan digunakan dalam pipeline pengolahan citra berbasis Python. Modul `random` standar menangani operasi seperti shuffling dataset atau augmentasi gambar acak. `numpy.random` mengendalikan sampling statistik dan manipulasi array numerik. Sementara itu, `torch.manual_seed` dan `torch.cuda.manual_seed_all` mengunci state PyTorch untuk eksekusi CPU dan GPU. Penambahan `torch.backends.cudnn.deterministic = True` bersama `benchmark = False` adalah langkah kritis yang sering terabaikan. Parameter ini memaksa cuDNN memilih algoritma konvolusi yang sepenuhnya deterministik, meskipun trade-off-nya adalah penurunan throughput training karena framework tidak lagi melakukan pencarian kernel optimal secara dinamis.

Struktur `config` berperan sebagai single source of truth bagi seluruh hyperparameter. Pendekatan dictionary ini jauh lebih scalable dibandingkan hardcoding nilai di dalam skrip terpisah seperti `data.py`, `train.py`, atau `evaluate.py`. Dengan memisahkan deklarasi parameter dari logika eksekusi, peneliti dapat melakukan grid search, sweep seed, atau migrasi arsitektur tanpa menyentuh kode inti. Pemanggilan `set_seed(config["seed"])` di dalam fungsi `main()` memastikan bahwa semua operasi stochastic terjadi setelah seed ditetapkan, sehingga urutan batch sampling, weight initialization, dan drop-out activation mengikuti pola yang terprediksi.

Catatan penting mengenai perilaku hardware perlu diperhatikan dalam konteks penelitian tingkat doktoral. Pengaktifan mode deterministik memang memperlambat iterasi training, namun hal ini diperlukan ketika tujuan penelitian menuntut reproduktibilitas mutlak antar mesin atau saat membandingkan metode secara head-to-head. Jika lingkungan komputasi Anda masih menunjukkan variasi kecil akibat operasi paralel atau reduksi floating-point yang bersifat nondeterministik, dokumentasi versi CUDA, driver GPU, dan PyTorch menjadi kewajiban. Metadata lingkungan ini memungkinkan replikasi eksak oleh reviewer atau peneliti lain, sesuai standar ketat publikasi di venue computer vision bereputasi tinggi.

Setelah konfigurasi dan seed berhasil dikunci, langkah selanjutnya adalah mendokumentasikan setiap variabel tersebut beserta metrik evaluasi yang dihasilkan selama eksperimen berjalan. Slide berikutnya akan membahas mekanisme persistensi log ke format JSON dan CSV, sehingga riwayat eksperimen tersusun sistematis, memudahkan analisis sensitivitas terhadap seed, dan menghasilkan artefak yang siap diarsipkan atau dibagikan ke komunitas penelitian.

---

## Slide 029 - Contoh Kode: Menyimpan Log dan Metrik

### Narasi

Setelah mengonfigurasi seed dan parameter eksperimen pada slide sebelumnya, langkah kritis berikutnya adalah memastikan setiap eksekusi kode dapat dilacak secara sistematis. Pada slide ini, kita membahas implementasi praktis untuk menyimpan konfigurasi dan metrik evaluasi ke dalam file yang terstruktur, sehingga mendukung prinsip reproduksibilitas dalam penelitian tingkat doktoral.

Kode pertama mendefinisikan fungsi `save_config` yang memanfaatkan modul `json` untuk mengekspor dictionary konfigurasi ke file `.json`. Penggunaan `indent=2` bertujuan agar file konfigurasi tetap mudah dibaca oleh manusia, sehingga memudahkan audit, review, atau replikasi eksperimen oleh peneliti lain. Fungsi kedua, `append_metric`, menangani pencatatan metrik evaluasi secara bertahap menggunakan format CSV. Logika `if f.tell() == 0: writer.writeheader()` memastikan bahwa header kolom hanya ditulis sekali saat file pertama kali dibuat, sementara baris berikutnya akan terus ditambahkan tanpa menimpa data lama. Pendekatan ini mencegah duplikasi header yang sering menjadi sumber error saat menggabungkan dataset hasil eksperimen.

Implementasi logging ini memberikan tiga manfaat strategis bagi workflow penelitian:
- Riwayat eksperimen tersimpan secara lengkap, memungkinkan pelacakan perubahan hyperparameter, arsitektur model, atau preprocessing pipeline.
- Analisis antar-seed menjadi lebih efisien karena semua skor akurasi, F1-score, atau metrik lainnya terpusat dalam satu tabel terstruktur yang siap diolah dengan NumPy atau Pandas.
- Artefak eksperimen siap dibagikan kepada reviewer atau kolaborator, memenuhi standar transparansi ilmiah modern dan mempercepat proses peer-review.

Perlu diperhatikan bahwa struktur log ini dirancang sebagai fondasi sebelum memasuki tahap validasi statistik. Ketika Anda telah mengumpulkan hasil dari berbagai konfigurasi seed atau split data, langkah selanjutnya adalah menghitung interval kepercayaan dan melakukan cross-validation, seperti yang akan dibahas pada slide berikutnya. Dengan pipeline logging yang konsisten, proses perhitungan confidence interval melalui bootstrap atau penggunaan `GroupKFold` akan berjalan lebih akurat, karena seluruh skor per-fold sudah terekam rapi dan bebas dari kesalahan manual dalam pengumpulan data.

---

## Slide 030 - Contoh Kode: Cross-Validation dan Confidence Interval

### Narasi

Pada slide ini, kita beralih dari sekadar menyimpan log eksperimen ke tahap analisis statistik yang lebih rigor, yaitu implementasi cross-validation dan perhitungan confidence interval. Setelah pada slide sebelumnya kita mencatat konfigurasi dan metrik ke dalam file JSON serta CSV, langkah selanjutnya adalah memastikan bahwa angka akurasi atau F1 score yang dilaporkan bukan hasil dari satu kali split data yang kebetulan menguntungkan.

Kode yang ditampilkan memanfaatkan `numpy` untuk simulasi bootstrap dan `sklearn.model_selection.GroupKFold` sebagai strategi validasi silang. Fungsi `bootstrap_ci` menerima sekumpulan skor, melakukan resampling dengan penggantian sebanyak 1.000 iterasi melalui `rng.choice`, lalu menghitung rata-rata setiap sampel bootstrap. Dari distribusi rata-rata tersebut, kita mengambil persentil ke-2,5 dan ke-97,5 untuk mendapatkan batas bawah dan atas confidence interval 95%. Contoh penggunaan menunjukkan bagaimana daftar skor `[0.91, 0.89, 0.92, 0.90, 0.93]` diproses untuk menghasilkan mean dan interval kepercayaan yang mencerminkan variabilitas model.

Penting untuk dicatat bahwa penggunaan `GroupKFold` sangat krusial ketika dataset memiliki struktur berkelompok, seperti multiple frame dari video yang sama, atau beberapa subjek dalam dataset biomedis. Split acak biasa dapat menyebabkan data leakage antar fold karena gambar dari subjek atau sesi yang sama muncul di training dan testing sekaligus. Dengan mengelompokkan berdasarkan ID subjek atau sesi, kita menjamin evaluasi yang independen dan realistis.

Setelah menjalankan cross-validation, jangan hanya menyimpan akurasi akhir secara agregat. Simpan skor per fold, lalu gunakan skor-skor tersebut untuk menghitung confidence interval. Pendekatan ini memberikan gambaran ketidakpastian model yang jauh lebih informatif dibandingkan laporan titik tunggal, terutama dalam konteks penelitian tingkat doktoral yang menuntut ketelitian metodologis dan transparansi pelaporan.

Hasil estimasi interval kepercayaan ini nantinya akan menjadi dasar objektif saat kita mengevaluasi literatur. Pada slide berikutnya, kita akan menerapkan prinsip-prinsip statistik dan pelaporan transparan ini ke dalam sebuah checklist reproduktibilitas paper. Checklist tersebut mencakup aspek kode, data, lingkungan komputasi, seed, hingga penulisan ablation study, sehingga kita dapat menilai kualitas metodologi paper secara sistematis sebelum mengidentifikasi research gap dan merumuskan kontribusi ilmiah baru.

---

## Slide 031 - Checklist Reproducibility Paper

### Narasi

Pada slide sebelumnya, kita telah membahas implementasi cross-validation dan perhitungan confidence interval melalui teknik bootstrap. Langkah ini menegaskan bahwa evaluasi model tidak boleh berhenti pada satu nilai akurasi tunggal, melainkan harus menyertakan estimasi variabilitas hasil. Namun, keandalan statistik saja belum cukup menjamin bahwa sebuah penelitian dapat direplikasi oleh komunitas ilmiah. Oleh karena itu, kita memerlukan kerangka kerja sistematis untuk menilai kesiapan publikasi dalam hal reproduktibilitas.

Slide ini menyediakan checklist komprehensif yang dapat digunakan sebagai standar audit metodologis. Mari kita tinjau setiap dimensi penilaiannya:
- **Kode**: Repository harus tersedia dengan lisensi terbuka yang jelas, memungkinkan eksekusi pipeline end-to-end tanpa hambatan hak cipta atau struktur direktori yang ambigu.
- **Data**: Dataset harus didokumentasikan lengkap, mencakup sumber resmi, skema izin akses, prosedur pembersihan, dan pembagian subset train, validation, dan test.
- **Environment**: Versi tepat dari semua library, framework, driver GPU, serta spesifikasi hardware harus dicantumkan, mengingat sensitivitas performa model terhadap perubahan dependency minor.
- **Konfigurasi**: Hyperparameter, arsitektur jaringan, dan strategi augmentasi harus disimpan dalam file konfigurasi yang terstruktur dan mudah dimodifikasi.
- **Seed**: Nilai seed untuk generator acak harus dilaporkan per sesi eksekusi, menjamin konsistensi pada pengacakan data, inisialisasi bobot, dan augmentasi gambar.
- **Metrik**: Protokol perhitungan skor harus didefinisikan secara eksplisit, termasuk cara penanganan edge case, normalisasi output, dan agregasi hasil antar kelas atau sampel.
- **Statistik**: Wajib menyertakan confidence interval atau uji signifikansi statistik untuk memisahkan peningkatan kinerja yang substantif dari fluktuasi acak.
- **Baseline**: Model pembanding harus dioptimalkan secara fair dengan tuning yang setara, bukan hanya dijalankan pada setting default, agar klaim keunggulan benar-benar valid.
- **Ablation**: Setiap modul atau inovasi yang diusulkan harus diuji secara terpisah melalui ablation study, membuktikan kontribusi marginal masing-masing komponen terhadap performa akhir.

Sebagai latihan mandiri, silakan terapkan checklist ini untuk mengevaluasi dua hingga tiga paper yang telah kita diskusikan pada pertemuan ini. Fokuskan pada identifikasi kesenjangan antara klaim novelty penulis dan bukti empiris yang sebenarnya disajikan. Hasil penilaian Anda akan menjadi dasar analisis kritis sebelum kita lanjut ke studi kasus konkret pada slide berikutnya, di mana kita akan membedah bagaimana ketiadaan transparansi metodologis dapat menggugurkan klaim akurasi tinggi, serta merancang ulang protokol eksperimen menjadi standar benchmarking yang memenuhi prinsip open science dan reproducible research.

---

## Slide 032 - Evaluasi Reproducibility: Studi Kasus

### Narasi

Setelah kita membahas checklist reproduktibilitas pada slide sebelumnya, kini saatnya menerapkannya secara langsung melalui studi kasus nyata. Slide ini menyajikan sebuah paper yang tampak memiliki klaim kinerja sangat kuat, namun jika diperiksa menggunakan lensa reproduktibilitas, terdapat sejumlah celah metodologis yang kritis.

Mari kita bedah fakta-fakta yang disajikan dalam studi kasus ini:
- Paper mengklaim akurasi 98,2 persen, mengalahkan baseline sebesar 96,5 persen.
- Tidak ada kode sumber yang tersedia, sehingga implementasi tidak dapat diverifikasi.
- Tidak ada pelaporan random seed, yang mengakibatkan ketidakmampuan untuk mereplikasi hasil secara deterministik.
- Tidak ada penyertaan Confidence Interval atau uji signifikansi statistik.
- Perbandingan hanya menggunakan satu konfigurasi default dari baseline, tanpa proses tuning yang adil.
- Tidak ada analisis ablation study untuk mengisolasi kontribusi masing-masing komponen model.

Dari kondisi tersebut, muncul tiga pertanyaan analitis yang harus kita jawab. Pertama, apakah klaim keberhasilan tersebut masih dapat dipercaya? Tanpa bukti transparansi dan pengujian statistik, klaim selisih 1,7 persen tersebut belum cukup kuat untuk diterima sebagai kontribusi ilmiah yang valid. Kedua, eksperimen apa yang seharusnya dilakukan? Kita memerlukan replikasi multi-seed, pelaporan interval kepercayaan, evaluasi baseline yang telah dioptimalkan secara fair, serta ablation study sistematis. Ketiga, bagaimana protokol dapat diperbaiki? Dengan menyusun ulang desain eksperimen agar memenuhi standar rigoritas dan transparansi yang kita harapkan dalam publikasi tingkat internasional.

Sebagai tindak lanjut, mahasiswa diminta untuk menulis ulang desain eksperimen dari studi kasus ini menjadi sebuah protokol yang valid dan siap dievaluasi. Tugas ini dirancang agar Anda tidak hanya pasif membaca paper, tetapi aktif memperbaiki kelemahan metodologis yang sering ditemukan dalam literatur terkini. 

Hasil penulisan protokol ini akan langsung kita diskusikan pada slide berikutnya melalui mekanisme peer review. Anda akan saling menilai kelayakan desain eksperimen rekan Anda berdasarkan kriteria transparansi, fairness baseline, kesesuaian ablation, dan perencanaan analisis statistik. Latihan ini merupakan langkah penting dalam membangun kompetensi riset tingkat doktoral, di mana kemampuan mendesain benchmarking yang reproducible sama pentingnya dengan inovasi algoritma itu sendiri.

---

## Slide 033 - Peer Review Protokol Eksperimen

### Narasi

Setelah pada slide sebelumnya kita mengkritisi sebuah studi kasus di mana klaim kinerja tinggi tidak didukung oleh detail reproduktibilitas yang memadai, kini kita beralih ke tahap validasi kolaboratif melalui peer review protokol eksperimen. Pada jenjang doktoral, kemampuan mengevaluasi desain penelitian orang lain sama krusialnya dengan merancang penelitian sendiri. Proses ini melatih Anda untuk mengidentifikasi celah metodologis sebelum sumber daya komputasi dialokasikan, sehingga meningkatkan efisiensi dan integritas ilmiah secara keseluruhan.

Peer review dalam konteks ini dilakukan secara silang antar mahasiswa. Setiap peserta akan menilai protokol eksperimen rekan kerjanya berdasarkan enam kriteria fundamental yang telah kita diskusikan:
- Apakah research question jelas, terfokus, dan terukur secara empiris?
- Apakah baseline dipilih dengan adil dan mewakili state-of-the-art yang relevan?
- Apakah rancangan ablation study selaras dengan klaim kontribusi utama?
- Apakah pengendalian variabel memadai, termasuk split data, augmentasi, dan kondisi lingkungan training?
- Apakah analisis statistik direncanakan sejak awal, seperti uji signifikansi, confidence interval, atau bootstrapping?
- Apakah dokumentasi dan artefak lengkap, mencakup kode, konfigurasi hyperparameter, random seed, dan spesifikasi environment?

Output dari sesi peer review ini bersifat konstruktif dan terstruktur. Mahasiswa wajib menghasilkan umpan balik tertulis yang memuat saran perbaikan spesifik dan actionable, bukan sekadar penilaian umum. Berdasarkan masukan tersebut, penilai akan menetapkan status kelayakan protokol menjadi tiga opsi: layak dieksekusi, perlu revisi, atau tidak layak karena memiliki flaw metodologis fatal. Keputusan ini harus disertai justifikasi teknis yang jelas dan merujuk pada prinsip benchmarking yang dapat direproduksi.

Perlu ditekankan bahwa protokol yang lolos review pun belum menjamin keberhasilan eksekusi. Setelah Anda menjalankan eksperimen berdasarkan protokol yang telah disempurnakan, kemungkinan besar Anda akan berhadapan dengan noise training, divergensi loss, atau bahkan performa yang justru menurun dibanding baseline. Pembahasan selanjutnya akan menyoroti strategi menangani hasil negatif dan anomaly, termasuk teknik troubleshooting pipeline, kriteria penghentian eksperimen, serta alasan mendasar mengapa pelaporan hasil negatif tetap diakui sebagai kontribusi ilmiah yang sah jika ditangani secara transparan dan metodologis.

---

## Slide 034 - Menangani Hasil Negatif dan Anomaly

### Narasi

Setelah menyelesaikan peer review terhadap protokol eksperimen pada slide sebelumnya, kita kini memasuki fase eksekusi dan pemantauan kinerja pipeline. Dalam riset tingkat doktoral, alur eksperimen jarang berjalan sempurna tanpa intervensi. Anomali atau hasil yang menyimpang dari ekspektasi bukanlah indikasi kegagalan mutlak, melainkan sinyal penting yang memerlukan diagnosa metodologis sebelum menarik kesimpulan ilmiah.

Mari kita tinjau gejala-gejala umum yang kerap muncul selama pelatihan model computer vision, beserta akar penyebabnya. Jika metrik evaluasi melonjak sangat tinggi hingga tidak realistis, kita harus segera memeriksa是否存在 data leakage, duplikasi sampel antara set training dan testing, atau kesalahan logika dalam perhitungan skor evaluasi. Sebaliknya, jika performa sangat rendah, kemungkinan besar terdapat inkonsistensi pada preprocessing, nilai learning rate yang tidak sesuai dengan skala gradien, atau inisialisasi bobot yang belum konvergen.

Fluktuasi varians yang besar antar seed acak sering mengindikasikan batch size yang terlalu kecil sehingga estimasi gradien menjadi noisy, atau augmentasi data yang terlalu agresif hingga merusak struktur fitur spasial. Ketika baseline standar tidak menghasilkan angka yang masuk akal, umumnya masalah terletak pada implementasi kode baseline yang belum terverifikasi atau konfigurasi hyperparameter yang menyimpang dari literatur. Terakhir, jika operasi ablasi tidak menunjukkan perbedaan metrik yang signifikan, hal ini bisa berarti komponen yang diusulkan memang belum memberikan gain fungsional, atau desain eksperimen kurang sensitif terhadap isolasi variabel tersebut.

Dari peta diagnosa ini, kita memiliki tiga jalur keputusan strategis. Pertama, lakukan perbaikan pada pipeline berdasarkan temuan penyebab. Kedua, jalankan ulang eksperimen dengan konfigurasi yang telah distabilkan untuk memverifikasi konsistensi. Ketiga, jika setelah audit menyeluruh hasil tetap negatif atau tidak mendukung hipotesis awal, jangan menghapusnya dari laporan. Laporkan sebagai temuan ilmiah. Dalam ekosistem riset S3, hasil negatif yang didokumentasikan secara transparan justru memperkaya peta pengetahuan tentang batas kapabilitas metode, sekaligus menjadi fondasi untuk merumuskan revisi arsitektur atau pertanyaan penelitian baru.

Proses penanganan anomali ini harus berujung pada pelaporan yang rigor dan transparan. Pada slide berikutnya, kita akan membahas standar penyajian tabel metrik, penghitungan confidence interval, penerapan uji signifikansi statistik, serta koreksi multiple comparison. Dengan demikian, setiap klaim novelty dan kontribusi penelitian dapat dipertanggungjawabkan secara empiris, reproducible, dan siap untuk diuji oleh komunitas akademik internasional.

---

## Slide 035 - Reporting: Tabel Metrik dan Analisis Statistik

### Narasi

Slide ini menetapkan standar pelaporan metrik dan analisis statistik yang wajib diterapkan dalam penelitian pengolahan citra digital tingkat lanjut. Hasil eksperimen tidak boleh disajikan sebagai angka tunggal atau nilai terbaik secara selektif. Format pelaporan yang rigor memerlukan penyajian mean beserta deviasi standar dari beberapa eksekusi independen, dilengkapi interval kepercayaan dan uji signifikansi statistik yang transparan.

Perhatikan contoh tabel pada slide. Kolom pertama menandai arsitektur atau modifikasi yang diuji. Kolom Accuracy dan F1 menampilkan rata-rata beserta variansnya, misalnya Baseline ResNet-50 mencapai 91,2 ± 0,3 persen untuk accuracy. Penambahan Attention Module pada baris kedua meningkatkan performa menjadi 92,8 ± 0,2 persen, disertai p-value sebesar 0,01 yang menandakan peningkatan signifikan terhadap baseline. Integrasi Loss B pada baris ketiga menghasilkan akurasi 93,5 ± 0,2 persen dengan p-value 0,003. Interval kepercayaan 95 persen memberikan rentang realistik di mana nilai sebenarnya diperkirakan berada, sehingga pembaca dapat menilai stabilitas model tanpa bergantung pada satu titik estimasi saja.

Prinsip pelaporan yang harus diikuti mencakup empat poin utama:
- Laporkan mean ± std yang diperoleh dari minimal tiga hingga lima eksekusi independen dengan seed acak berbeda.
- Sertakan confidence interval dan hasil uji statistik formal, seperti paired t-test atau Wilcoxon signed-rank test, disesuaikan dengan distribusi data.
- Jelaskan secara eksplisit metode uji yang digunakan dan bagaimana koreksi multiple comparison diterapkan, misalnya menggunakan Bonferroni atau Holm-Bonferroni, ketika membandingkan lebih dari dua metode sekaligus.
- Hindari praktik cherry-picking angka terbaik; transparansi terhadap variasi antar-seed justru memperkuat validitas klaim ilmiah Anda.

Pendekatan ini merupakan kelanjutan logis dari pembahasan sebelumnya mengenai penanganan hasil negatif dan anomaly. Ketika pipeline mengalami kegagalan atau menghasilkan outlier, langkah selanjutnya bukan menyembunyikan temuan tersebut, melainkan mendokumentasikan proses troubleshooting, memperbaiki konfigurasi, dan melaporkan hasil akhir secara statistik yang ketat. Jika perbaikan tetap tidak mengubah tren negatif, temuan itu sendiri dapat menjadi kontribusi ilmiah berupa batasan metodologis atau karakteristik dataset yang perlu diwaspadai oleh komunitas peneliti.

Setelah metrik dilaporkan dengan standar statistik yang jelas, reproduktibilitas penelitian akan sangat bergantung pada dokumentasi lingkungan komputasi. Slide berikutnya akan membahas artefak teknis yang harus disimpan secara sistematis, mulai dari kode sumber, konfigurasi hyperparameter, log training, checkpoint model, hingga file environment lengkap. Tanpa dokumentasi lingkungan yang terstruktur, bahkan hasil statistik yang paling rigor sekalipun sulit diverifikasi atau dikembangkan lebih lanjut oleh peneliti lain.

---

## Slide 036 - Dokumentasi Lingkungan dan Artefak

### Narasi

Setelah kita membahas pelaporan metrik dan analisis statistik pada slide sebelumnya, langkah kritis berikutnya dalam desain eksperimen adalah memastikan bahwa seluruh proses dapat direproduksi secara akurat. Reproduktibilitas bukan sekadar ideal akademis, melainkan fondasi utama untuk validitas penelitian di tingkat doktoral. Pada slide ini, kita akan menyoroti dokumentasi lingkungan dan pengelolaan artefak eksperimen sebagai komponen wajib dalam benchmarking yang kredibel.

Setiap eksperimen pengolahan citra atau computer vision menghasilkan sejumlah artefak digital yang harus dikelola dengan disiplin ketat. Artefak yang wajib disimpan meliputi:
- Kode sumber lengkap beserta konfigurasi hyperparameter.
- Log training serta catatan metrik per epoch.
- Checkpoint model pada interval tertentu.
- Split dataset yang digunakan, termasuk seed random-nya.
- File definisi lingkungan komputasi.

Tanpa penyimpanan terstruktur ini, upaya reproduksi hasil akan terhambat oleh ketidakpastian versi library atau perbedaan konfigurasi sistem operasi. Pengelolaan artefak yang rapi juga memudahkan proses audit metodologis saat penulisan bab metodologi disertasi.

Untuk mengelola artefak tersebut, kita memerlukan serangkaian alat standar riset komputasi modern:
- `requirements.txt` atau `environment.yml` untuk mendefinisikan dependensi Python secara eksplisit.
- Dockerfile guna menciptakan kontainer isolasi yang mereplikasi lingkungan komputasi secara identik.
- Git untuk versioning kode dan konfigurasi secara granular.
- Platform tracking seperti MLflow, Weights & Biases, atau TensorBoard untuk pencatatan metrik dan hiperparameter otomatis.
- Model card dan dataset card sebagai dokumen transparansi metodologis.

Model card dan dataset card sering kali terabaikan, padahal keduanya krusial untuk konteks penelitian S3. Model card menjelaskan arsitektur, tujuan penggunaan, batasan performa, dan pertimbangan etika, sedangkan dataset card mendeskripsikan asal data, prosedur anotasi, bias potensial, dan lisensi. Praktik ini sejalan dengan prinsip reproducible benchmarking yang menuntut keterbukaan penuh terhadap reviewer dan komunitas ilmiah.

Dengan artefak dan dokumentasi yang tertata rapi, hasil eksperimen Anda tidak hanya dapat diverifikasi ulang, tetapi juga siap diintegrasikan ke dalam kerangka penelitian lebih lanjut. Langkah ini menjadi jembatan langsung menuju pembahasan pada slide berikutnya, yaitu bagaimana protokol eksperimen yang solid diterjemahkan menjadi komponen metodologi dalam proposal disertasi Anda.

---

## Slide 037 - Dari Eksperimen ke Proposal Disertasi

### Narasi

Setelah kita membahas secara rinci dokumentasi lingkungan komputasi dan pengelolaan artefak pada slide sebelumnya, langkah selanjutnya adalah mengintegrasikan seluruh praktikum teknis ini ke dalam kerangka akademik yang lebih besar, khususnya penyusunan proposal disertasi. Slide ini menyoroti bagaimana protokol eksperimen yang terstruktur dan dapat direproduksi menjadi jembatan antara pelaksanaan laboratorium dan penulisan bab metodologi disertasi Anda.

Protokol eksperimen bukan sekadar catatan jalannya kode atau konfigurasi hyperparameter. Dalam konteks penelitian tingkat doktor, dokumen ini berfungsi sebagai bagian integral dari metodologi penelitian yang harus memenuhi standar rigor ilmiah. Setiap keputusan desain eksperimen—mulai dari pemilihan arsitektur model, strategi augmentasi data, hingga prosedur evaluasi—harus dapat dipertanggungjawabkan secara logis dan statistik. Hal ini sekaligus menjadi dasar empiris untuk menguji hipotesis penelitian yang telah Anda rumuskan sebelumnya.

Selain itu, hasil dari protokol yang dijalankan dengan ketat memberikan bukti kelayakan penelitian. Di jenjang S3, komite penguji akan sangat memperhatikan apakah pendekatan yang Anda usulkan memiliki fondasi eksperimental yang solid sebelum Anda berkomitmen pada pengembangan sistem skala penuh. Bukti awal ini juga menjadi landasan strategis untuk merancang rencana publikasi di konferensi atau jurnal bereputasi, karena alur eksperimen yang jelas memudahkan proses penulisan section *Experiments* dan *Ablation Studies*.

Untuk mempersiapkan pertemuan ke-14, Anda diminta menyusun empat komponen kunci berikut:
- Matriks eksperimen yang memetakan kombinasi variabel independen versus metrik evaluasi.
- Jadwal komputasi yang realistis, memperhitungkan ketersediaan GPU di Google Colab atau cluster kampus.
- Rencana mitigasi risiko, seperti strategi fallback jika model tidak konvergen atau terjadi bias pada dataset split.
- Hasil awal atau *proof-of-concept* yang akan langsung diintegrasikan ke dalam draf proposal disertasi.

Pada slide berikutnya, kita akan langsung menerjemahkan konsep-konsep teoretis ini ke dalam bentuk praktis melalui sesi workshop. Anda akan berlatih menyusun protokol eksperimen lengkap menggunakan template yang tersedia di notebook praktikum, mencakup perumusan research question, pemilihan baseline, rancangan ablation study, penetapan jumlah seed dan uji statistik, hingga estimasi computational budget. Persiapan materi pada slide ini akan sangat menentukan kelancaran diskusi peer review di sesi tersebut.

---

## Slide 038 - Workshop: Menyusun Experimental Protocol

### Narasi

Pada slide ini, kita beralih dari perencanaan teoretis menuju praktik langsung melalui workshop penyusunan protokol eksperimen. Setelah sebelumnya membahas bagaimana protokol ini akan menjadi tulang punggung metodologi disertasi dan dasar pengujian hipotesis, kini saatnya menerapkannya secara konkret melalui delapan langkah terstruktur.

Pertama, pilih topik penelitian yang relevan dengan perkembangan terkini di bidang *computer vision* atau *digital image processing*, lalu rumuskan satu *research question* yang tajam dan terukur. Pertanyaan ini harus secara eksplisit mengidentifikasi celah pengetahuan (*research gap*) yang ingin Anda isi. Kedua, tetapkan dataset yang akan digunakan beserta strategi *split*-nya, apakah berupa train-validation-test, k-fold cross-validation, atau pembagian khusus untuk domain adaptation. 

Ketiga, tentukan baseline yang kompetitif dan jelaskan metode usulan Anda. Pastikan baseline dipilih secara adil agar perbandingan hasil benchmarking benar-benar valid secara ilmiah. Keempat, rancang *ablation study* yang sistematis untuk mengisolasi kontribusi masing-masing komponen model Anda, sehingga klaim novelti dapat dibuktikan secara empiris.

Kelima, tentukan metrik evaluasi yang tepat sesuai konteks masalah, misalnya mAP untuk deteksi objek, IoU untuk segmentasi, atau skor FID dan CLIPScore untuk generasi gambar. Jangan lupa menetapkan jumlah *seed* acak yang konsisten dan rencanakan uji statistik seperti t-test berpasangan atau ANOVA untuk memastikan signifikansi perbedaan performa antar metode.

Keenam, dokumentasikan seluruh lingkungan komputasi, versi library, konfigurasi perangkat keras, serta artefak penting seperti kode pra-pemrosesan, skrip pelatihan, dan checkpoint model. Ketujuh, hitung estimasi *computational budget* berdasarkan kompleksitas arsitektur model dan ukuran dataset, termasuk alokasi waktu training dan inference. Kedelapan, presentasikan protokol lengkap Anda untuk mendapatkan masukan kritis dari rekan sejawat melalui sesi *peer review*.

Template panduan untuk menyusun protokol ini telah disediakan di notebook praktikum. Silakan buka file tersebut sebagai referensi struktural selama mengerjakan langkah-langkah di atas. Seluruh elemen yang Anda susun pada workshop ini akan segera dinilai menggunakan kriteria objektif yang akan kita bahas pada slide berikutnya.

---

## Slide 039 - Rubrik Penilaian Protokol Eksperimen

### Narasi

Setelah menyelesaikan latihan menyusun protokol eksperimen pada slide sebelumnya, kita kini beralih ke mekanisme evaluasi yang akan digunakan untuk menilai kualitas setiap rancangan tersebut. Rubrik ini dirancang secara sistematis untuk memastikan bahwa setiap komponen metodologis dalam protokol Anda memenuhi standar rigor akademik tingkat doktoral, sekaligus berfungsi sebagai alat self-review sebelum memasuki fase implementasi teknis.

Terdapat tujuh aspek kunci yang menjadi fokus penilaian, masing-masing diberi skala skor dari satu hingga empat:
- Kejelasan research question dan tujuan penelitian, yang harus tajam, terukur, dan secara eksplisit menunjukkan celah pengetahuan yang ingin diisi.
- Kesesuaian dataset dan split data, memastikan pembagian train-validation-test atau cross-validation menghindari data leakage dan mencerminkan distribusi dunia nyata.
- Keadilan pemilihan baseline, yang wajib merepresentasikan state-of-the-art terkini tanpa bias pengurangan performa baseline secara artifisial.
- Kualitas desain ablation study, harus isolatif, logis, dan mampu mengisolasi kontribusi spesifik dari setiap komponen yang Anda usulkan.
- Ketepatan metrik dan statistik, mencakup pemilihan indikator evaluasi yang selaras dengan tugas serta penggunaan uji signifikansi yang tepat.
- Dokumentasi reproducibility, meliputi pencatatan lengkap lingkungan komputasi, versi library, seed acak, konfigurasi hyperparameter, dan pipeline preprocessing.
- Realisme computational budget, berupa estimasi kebutuhan GPU hours, memori, dan waktu training yang seimbang dengan infrastruktur yang tersedia.

Kriteria pemberian skor empat poin memberikan ruang nuansa yang jelas untuk umpan balik konstruktif. Skor empat menandakan protokol sangat baik, matang, dan siap diimplementasikan tanpa revisi substansial. Skor tiga mengindikasikan kualitas baik namun memerlukan penyempurnaan minor pada detail teknis atau kelengkapan dokumentasi. Skor dua berarti terdapat kekurangan signifikan pada desain metodologi atau strategi evaluasi yang perlu diperbaiki sebelum eksekusi. Sementara skor satu menunjukkan bahwa protokol belum memenuhi standar kelayakan penelitian tingkat doktor dan memerlukan penataan ulang pada beberapa aspek fundamental.

Penerapan rubrik ini secara konsisten akan membiasakan Anda melakukan audit metodologis yang ketat, sehingga validitas internal dan eksternal penelitian semakin kuat. Evaluasi berbasis rubrik juga meminimalkan subjektivitas dalam peer review dan mempersiapkan Anda menghadapi proses double-blind review pada jurnal atau konferensi internasional. Setelah sesi penutup hari ini, pertemuan berikutnya kita akan melanjutkan ke tahap selanjutnya yaitu perumusan research question, hipotesis, dan novelty secara lebih mendalam, yang merupakan fondasi konseptual sebelum masuk ke fase desain eksperimen teknis.

---

## Slide 040 - Penutup

### Narasi

Kita telah menyelesaikan rangkaian pembahasan mengenai desain eksperimen dan benchmarking yang dapat direproduksi dalam konteks penelitian pengolahan citra digital tingkat lanjut. Pada slide sebelumnya, kita menelaah rubrik penilaian protokol eksperimen yang mencakup tujuh dimensi evaluasi, mulai dari kejelasan research question, kesesuaian dataset dan split, keadilan pemilihan baseline, kualitas desain ablation, ketepatan metrik dan analisis statistik, dokumentasi reproduktibilitas, hingga realisme anggaran komputasi. Rubrik ini berfungsi sebagai kerangka audit mandiri maupun mekanisme peer-review untuk memastikan bahwa setiap rancangan penelitian memenuhi standar rigor metodologis dan transparansi ilmiah.

Dalam praktik penelitian doktoral, reproduktibilitas bukan sekadar kelengkapan teknis, melainkan prasyarat validitas temuan. Pencatatan konfigurasi lingkungan, versi library, seed acak, hyperparameter tuning, serta pipeline preprocessing harus dilakukan secara sistematis. Hal ini memungkinkan peneliti lain mereplikasi hasil benchmarking, memverifikasi klaim kinerja model, dan membangun perbaikan secara iteratif sesuai prinsip open science yang kini menjadi standar publikasi di venue internasional bereputasi.

Dengan fondasi desain eksperimen yang solid dan protokol evaluasi yang terstandarisasi, langkah logis selanjutnya adalah mengarahkan energi riset pada perumusan masalah yang tepat. Pada pertemuan berikutnya, kita akan membahas formulasi research question, hipotesis, dan novelty. Pembahasan akan berfokus pada teknik identifikasi research gap dari tinjauan literatur mutakhir, penyusunan pertanyaan penelitian yang terukur dan falsifiable, serta strategi mendefinisikan klaim kontribusi ilmiah yang membedakan karya Anda dari state-of-the-art yang sudah ada.

Terima kasih atas fokus dan kontribusi aktif selama sesi ini. Persiapkan diri untuk membaca dua hingga tiga paper terbaru yang relevan dengan domain penelitian Anda, karena materi pertemuan depan akan menuntut kemampuan dekonstruksi argumen metodologis dan penyusunan kerangka konseptual yang tajam.
