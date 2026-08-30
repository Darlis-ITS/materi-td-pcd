# Narasi TD Pengolahan Citra Digital - Pertemuan 11

## Explainable, Robust, dan Trustworthy Computer Vision

Sumber: markdown/pert11-explainable-robust-dan-trustworthy-computer-vision.md

---

## Slide 000 - Cover

### Narasi

Pertemuan ini menempatkan topik explainable, robust, dan trustworthy computer vision sebagai fondasi kritis bagi penelitian tingkat doktoral. Selama perjalanan materi sebelumnya, penekanan utama telah diberikan pada pengembangan arsitektur dan metodologi pembuatan model yang semakin canggih. Namun, pada jenjang riset lanjutan, pencapaian skor akurasi tinggi pada benchmark konvensional tidak lagi cukup sebagai bukti validitas ilmiah. Model yang tidak dapat dijelaskan, rentan terhadap gangguan input, atau menunjukkan ketidakstabilan perilaku di luar distribusi training, memerlukan evaluasi yang lebih mendalam sebelum dapat diklaim sebagai kontribusi ilmiah yang layak.

Fokus utama sesi ini terbagi pada tiga pilar fundamental yang saling terkait:
- **Explainability** menuntut transparansi mekanistik, memungkinkan peneliti melacak jalur inferensi model dan memahami alasan di balik setiap prediksi visual tanpa bergantung pada pendekatan post-hoc yang dangkal.
- **Robustness** menguji ketahanan sistem terhadap variasi distribusi data, adversarial perturbations, dan kondisi lingkungan yang tidak terprediksi, yang sering kali terabaikan dalam protokol evaluasi tradisional namun krusial untuk generalisasi.
- **Trustworthiness** berfungsi sebagai kerangka holistik yang menggabungkan aspek statistik, etis, dan teknis untuk menjamin bahwa performa model konsisten, dapat dipertanggungjawabkan, dan aman untuk implementasi maupun replikasi akademis.

Pembahasan pada slide ini akan menjadi jembatan konseptual menuju diskusi metodologis yang lebih aplikatif. Pemahaman tentang kepercayaan model menjadi prasyarat mutlak sebelum memasuki tahap experimental design dan reproducible benchmarking, sebagaimana tercantum dalam alur kurikulum. Evaluasi trustworthiness harus tertanam sejak awal perancangan hipotesis, pemilihan dataset, dan definisi metrik, bukan sebagai langkah korektif pasca-eksperimen. Pendekatan ini memastikan bahwa setiap temuan penelitian memenuhi standar rigor yang diperlukan untuk publikasi di venue internasional bereputasi dan mendukung penyusunan proposal disertasi yang memiliki novelty serta positioning yang jelas terhadap state-of-the-art.

---

## Slide 001 - Posisi Pertemuan 11 dalam RPS

### Narasi

Slide ini memetakan posisi Pertemuan 11 secara eksplisit dalam struktur RPS mata kuliah. Seperti terlihat pada tabel, Pertemuan 1 hingga 9 telah membangun fondasi teknis berupa kemampuan merancang dan melatih model computer vision yang semakin kompleks, mencakup CNN, Vision Transformer, self-supervised learning, model multimodal, image restoration, deteksi, segmentasi, hingga generative model. Pertemuan 10 kemudian meluaskan cakupan tersebut ke domain representasi tiga dimensi dan neural rendering.

Pertemuan 11 sengaja dirancang sebagai titik jeda strategis sebelum masuk ke tahap evaluasi metodologis. Alur berpikir kita kini berhenti sejenak untuk mengajukan pertanyaan kritis: dapatkah seluruh model canggih yang telah dibangun selama ini benar-benar dipercaya? Kutipan pada slide menegaskan bahwa tanpa mekanisme evaluasi kepercayaan, angka akurasi tinggi pada benchmark standar tidak lagi memadai sebagai bukti ilmiah di tingkat doktoral. Validitas penelitian tidak hanya diukur dari performa numerik, melainkan dari transparansi, konsistensi, dan ketahanan model terhadap skenario dunia nyata.

Transisi ini akan langsung berlanjut pada Slide berikutnya yang merinci tujuan pembelajaran dan target keluaran spesifik. Mahasiswa diharapkan tidak hanya memahami konsep interpretabilitas, robustness, uncertainty, calibration, fairness, dan adversarial risk secara teoritis, tetapi juga mampu menerjemahkannya ke dalam praktik eksperimen. Evaluasi penjelasan model harus diuji kesetiaan (faithfulness)-nya terhadap alasan prediksi, sementara ketidakpastian model harus divisualisasikan melalui calibration plot dan diuji melalui perturbation test.

Secara operasional, capaian ini diterjemahkan menjadi dua komponen utama: audit singkat trustworthiness untuk satu pipeline computer vision, serta dokumentasi bukti eksperimen yang meliputi attribution map, reliability plot, hasil perturbation, dan analisis kasus kegagalan model. Pendekatan ini menyiapkan pondasi metodologis yang rigor untuk Pertemuan 12, yaitu experimental design dan reproducible benchmarking, sehingga langkah selanjutnya dapat dilakukan dengan kesadaran penuh terhadap batasan dan risiko sistem yang dikembangkan.

---

## Slide 002 - Tujuan Pembelajaran dan Target Keluaran

### Narasi

Pada slide ini, kita menetapkan secara eksplisit capaian pembelajaran dan bentuk konkret keluaran yang diharapkan setelah menyelesaikan pertemuan ke-11. Mengingat slide sebelumnya telah menempatkan topik ini sebagai titik balik kritis dalam alur kurikulum—di mana fokus bergeser dari pembangunan arsitektur model menuju pertanyaan fundamental tentang kepercayaan terhadap sistem—maka tujuan pada slide ini dirancang untuk menjembatani kesenjangan antara pemahaman konseptual dan implementasi evaluasi yang rigor.

Secara spesifik, Anda ditargetkan mampu menjelaskan enam pilar utama dalam evaluasi model modern: interpretabilitas, *robustness*, ketidakpastian (*uncertainty*), kalibrasi, keadilan (*fairness*), serta risiko adversarial dalam konteks computer vision. Penekanan doktoral ditempatkan pada kemampuan mengevaluasi apakah penjelasan yang dihasilkan model benar-benar *faithful* terhadap mekanisme internal yang mendasari prediksi, bukan sekadar menyajikan visualisasi yang secara intuitif tampak masuk akal bagi manusia.

Untuk mengoperasionalkan konsep tersebut, latihan praktis akan berfokus pada tiga teknik evaluasi inti: pembuatan *attribution map*, penyusunan *calibration plot*, dan pelaksanaan *perturbation test*. Selain itu, Anda juga akan dibekali kerangka kerja untuk menyusun inventarisasi risiko model dan mendokumentasikannya secara terstruktur melalui *model card*. Pendekatan ini memastikan bahwa klaim kinerja model didukung oleh auditable evidence, bukan hanya angka akurasi benchmark.

Sebagai target keluaran pertemuan, setiap mahasiswa wajib menghasilkan sebuah *trustworthiness audit* singkat yang diterapkan pada satu model atau pipeline computer vision pilihan Anda. Bukti eksperimen yang harus dikumpulkan mencakup empat elemen wajib:
- *Attribution map* yang memetakan kontribusi fitur terhadap keputusan model.
- Grafik reliabilitas atau *calibration plot* untuk mengukur kesesuaian antara skor kepercayaan dan akurasi empiris.
- Hasil *perturbation test* sederhana yang mengkuantifikasi degradasi performa akibat modifikasi input terkontrol.
- Analisis mendalam pada contoh kegagalan model (*failure cases*) untuk mengidentifikasi pola bias atau kerentanan struktural.

Audit yang Anda susun akan menjadi dasar langsung untuk menanggapi tiga pertanyaan kunci pada slide berikutnya. Kita akan menguji validitas korelasi antara penjelasan model dengan alasan prediksinya, mengukur sensitivitas performa terhadap pergeseran distribusi data, serta menentukan metrik kepercayaan mana yang paling etis dan informatif untuk dilaporkan kepada stakeholder. Dengan demikian, *trustworthiness audit* ini berfungsi sebagai batu loncatan metodologis menuju penelitian yang transparan, reproducible, dan siap diposisikan pada state-of-the-art literatur computer vision tingkat lanjut.

---

## Slide 003 - Pertanyaan Kunci Pertemuan Ini

### Narasi

Slide ini merangkum tiga pertanyaan penelitian fundamental yang menjadi poros analisis kita pada pertemuan ke-11. Jika pada slide sebelumnya kita telah menetapkan target keluaran berupa audit trustworthiness, pembuatan attribution map, hingga kalibrasi model, maka slide ini mempersempit fokus tersebut menjadi inquiry yang harus dijawab secara empiris dan metodologis.

Pertama, kita harus menguji apakah explanation yang dihasilkan benar-benar berkorelasi dengan mekanisme internal model dalam memproduksi prediksi. Validitas metode interpretability tidak dapat hanya dinilai dari estetika visualisasi heatmap. Kita perlu memastikan bahwa fitur yang disorot oleh algoritma attribution memang menjadi alasan kausal utama keputusan model, bukan sekadar artefak komputasi atau bias dataset. Tanpa korelasi ini, metode interpretability kehilangan nilai ilmiahnya.

Kedua, kita akan menelaah bagaimana performa model berubah ketika dihadapkan pada data yang berada di luar distribusi pelatihan. Pertanyaan ini menyentuh batas generalisasi dan robustness. Pada tingkat doktoral, identifikasi degradation curve saat terjadi domain shift atau covariate shift menjadi kunci untuk menentukan apakah sebuah arsitektur layak diadopsi untuk aplikasi nyata atau hanya overfit pada benchmark statis.

Ketiga, kita akan menentukan metrik kepercayaan apa yang paling relevan dan etis untuk dilaporkan kepada pengguna atau reviewer ilmiah. Transparansi pelaporan ketidakpastian (uncertainty) dan calibrated confidence score adalah standar baru yang menuntut kejujuran metodologis. Pelaporan yang rigor mencegah overclaiming terhadap akurasi model dan melindungi integritas publikasi ilmiah.

Fokus penelitian kita pada sesi ini tertuju pada tiga pilar eksperimental:
- Menguji **faithfulness** explanation melalui protokol evaluasi kuantitatif, bukan sekadar demonstrasi visual yang tampak masuk akal.
- Mengukur degradasi performa secara sistematis saat **distribusi data berubah**, sehingga batas keamanan dan generalisasi model terpetakan jelas.
- Mengkomunikasikan **risiko model** secara eksplisit kepada pengguna, menjembatani kesenjangan antara output teknis dan pemahaman stakeholder.

Penekanan pada pertanyaan-pertanyaan kunci ini berfungsi sebagai jembatan kritis. Materi ini akan menyaring kembali seluruh pembahasan mengenai 3D vision, generative models, serta arsitektur deteksi dan segmentasi yang telah dipelajari pada pertemuan sebelumnya. Dengan menjawab ketiga pertanyaan ini, kita akan memiliki lensa evaluatif yang ketat untuk menilai kesiapan model, sekaligus menyiapkan baseline eksperimen yang lebih disiplin dan terukur untuk tahap desain penelitian lanjutan pada pertemuan berikutnya.

---

## Slide 004 - Jembatan: Dari 3D Vision dan Generative Models ke Trustworthy Vision

### Narasi

Slide ini bertindak sebagai jembatan konseptual yang mengaitkan implementasi teknis sebelumnya dengan kerangka evaluasi kritis yang menjadi fokus utama pertemuan ini. Pada sesi-sesi awal, kita telah membahas berbagai arsitektur canggih mulai dari representasi 3D hingga model generatif. Namun, performa visual yang tinggi tidak otomatis mencerminkan keandalan sistem dalam skenario penelitian atau deployment nyata.

Terdapat tiga observasi penting yang mendorong urgensi pembahasan ini:
- Teknik seperti NeRF dan pipeline 3D reconstruction mampu mensintesis gambar yang sangat fotorealistik, yet validitas geometrisnya sering kali belum diverifikasi secara rigor.
- Diffusion model berhasil menghasilkan citra yang meyakinkan secara visual, tetapi tetap rentan terhadap halusinasi detail yang dapat mengubah interpretasi semantik.
- Detector dan segmenter modern memang mencatatkan akurasi tinggi pada benchmark standar, namun umumnya gagal menyediakan ukuran ketidakpastian atau confidence bounds yang transparan pada setiap prediksi.

Mengacu pada pertanyaan kunci yang telah kita rumuskan sebelumnya, ketiga celah ini menuntut kita untuk beralih dari sekadar optimisasi akurasi menuju pengukuran faithfulness explanation dan robustness terhadap perubahan distribusi data. Pertemuan 11 hadir untuk memberikan lensa kritis tersebut, sehingga kita dapat menilai ulang seluruh model yang telah dipelajari sejak pertemuan 3 hingga 10.

Tujuan utamanya adalah membekali mahasiswa dengan kerangka kerja untuk menjawab pertanyaan mendasar: apakah model ini aman dan siap digunakan dalam konteks penelitian atau aplikasi riil? Temuan dari audit kritis ini akan menjadi input struktural untuk merancang eksperimen yang lebih ketat pada pertemuan berikutnya. Dengan kata lain, evaluasi trustworthy vision bukan hanya pelengkap, melainkan prasyarat metodologis yang akan menentukan kualitas proposal disertasi dan desain riset Anda selanjutnya.

---

## Slide 005 - Definisi Trustworthy Computer Vision

### Narasi

Slide ini merumuskan definisi operasional dari *Trustworthy Computer Vision*. Dalam konteks penelitian tingkat doktoral, pendekatan ini menuntut lima jaminan fundamental yang harus dipenuhi selama siklus pengembangan dan evaluasi model:
- **Dapat dijelaskan** — keputusan model harus dapat dilacak, ditafsirkan, dan dipahami secara logis oleh peneliti maupun pengguna.
- **Robust** — model harus tahan terhadap gangguan minor pada input maupun pergeseran distribusi data saat diterapkan di lingkungan nyata.
- **Terkalibrasi** — skor kepercayaan (*confidence score*) yang dihasilkan jaringan harus selaras secara matematis dengan akurasi prediksinya.
- **Adil** — model tidak boleh mendiskriminasi kelompok data tertentu secara sistematis akibat bias pelatihan atau desain arsitektur.
- **Akuntabel** — potensi risiko kegagalan harus didokumentasikan, dipantau, dan dikomunikasikan secara transparan kepada pemangku kepentingan.

Konsekuensi akademis dari kerangka ini mengubah paradigma penilaian karya ilmiah. Klaim akurasi tinggi saja sudah tidak lagi dianggap sebagai bukti validitas model. Proposal disertasi dan publikasi tingkat lanjut wajib menyertakan analisis risiko, pengujian ketahanan, serta dokumentasi keterbatasan model secara eksplisit. Lebih jauh, konsep *reproducibility* atau keterulangan eksperimen hanya akan bermakna jika protokol evaluasinya secara ketat mengukur aspek-aspek kepercayaan tersebut. Tanpa komponen ini, hasil penelitian berisiko menjadi temuan yang rapuh dan sulit digeneralisasi.

Pemahaman ini merupakan respons langsung terhadap celah yang diidentifikasi pada slide sebelumnya. Kita telah membahas bagaimana model *generative* dan teknik rekonstruksi 3D sering kali menghasilkan sintesis yang fotorealistik tanpa jaminan kebenaran geometris, sementara detektor dan segmenter canggih kerap beroperasi sebagai kotak hitam tanpa ukuran ketidakpastian. Dengan menetapkan standar *trustworthiness* ini, kita menyiapkan lensa kritis untuk mengaudit semua arsitektur yang telah dibahas pada pertemuan tiga hingga sepuluh.

Langkah berikutnya adalah menerjemahkan prinsip-prinsip konseptual ini menjadi metrik dan prosedur yang dapat diukur. Pada slide berikutnya, kita akan membedah empat dimensi utama *trustworthiness*: *Explainability*, *Robustness*, *Uncertainty*, dan *Fairness*. Setiap dimensi akan dipetakan ke pertanyaan riset spesifik, dilengkapi dengan teknik evaluasi seperti *saliency map*, pengujian pergeseran distribusi, kalibrasi prediktif, serta audit bias melalui *model cards*. Keterkaitan antar-dimensi juga akan dianalisis untuk menunjukkan mengapa perbaikan pada satu aspek sering kali berdampak langsung pada aspek lainnya.

---

## Slide 006 - Empat Dimensi Utama Trustworthiness

### Narasi

Pada slide ini, kita memetakan empat dimensi utama yang menyusun kerangka *trustworthy computer vision*. Mengacu pada definisi yang telah dibahas pada slide sebelumnya—di mana model vision dituntut untuk dapat dijelaskan, robust, terkalibrasi, adil, dan akuntabel—slide ini berfungsi sebagai struktur operasionalisasi klaim-klaim tersebut ke dalam variabel penelitian yang terukur.

Peta dimensi menyajikan empat pilar yang berjalan paralel namun saling bergantung. *Explainability* menjawab pertanyaan "mengapa?" melalui teknik seperti *saliency map*, *gradient attribution*, dan *perturbation-based analysis*. *Robustness* menguji ketahanan model terhadap gangguan dengan memantau *distribution shift* dan respons terhadap *adversarial examples*. *Uncertainty* mengukur "seberapa yakin?" model melalui *calibration* dan estimasi *predictive uncertainty*. Sementara itu, *fairness* memastikan keadilan sistematis dengan melakukan *bias audit* dan pendokumentasian risiko melalui *model card*.

Yang menjadi fokus kajian kritis di tingkat doktoral adalah interaksi antar dimensi tersebut. Ketiganya tidak dapat dievaluasi secara isolatif. Estimasi *uncertainty* yang lemah akan memperbesar kerentanan terhadap *adversarial example*, karena model gagal mendeteksi ketika input berada di luar domain pelatihan. Pengukuran *robustness* tanpa bedah kegagalan mendalam justru berpotensi menutupi bias tersembunyi pada subset data marginal. Selain itu, output *explainability* wajib divalidasi bersama metrik kalibrasi; tanpa sinkronisasi ini, interpretasi visual berisiko menyesatkan dan menciptakan ilusi kepercayaan yang tidak sesuai dengan performa aktual model.

Pemahaman relasional ini menjadi fondasi metodologis sebelum kita menyorot salah satu pilar secara teknis. Pada slide berikutnya, kita akan mengurai terminologi kunci dalam *explainability*, membedakan pendekatan intrinsik versus *post-hoc*, serta cakupan penjelasan lokal dan global. Diskusi akan dilanjutkan dengan kritik terhadap asumsi bahwa visualisasi penjelasan otomatis mencerminkan mekanisme keputusan model, serta protokol verifikasi *faithfulness* yang diperlukan untuk menjamin integritas ilmiah dalam riset tingkat lanjut.

---

## Slide 007 - Explainability: Konsep dan Terminologi

### Narasi

Setelah sebelumnya membahas empat dimensi utama trustworthiness dalam computer vision, kini kita fokus pada dimensi pertama, yaitu explainability. Dimensi ini menjawab pertanyaan fundamental “mengapa?” model mengambil keputusan tertentu. Memahami terminologi dan klasifikasi explainability sangat penting sebelum kita mengevaluasi kualitas atau keandalan suatu metode interpretasi.

Berikut adalah dua pasangan konsep kunci yang harus dibedakan secara tegas:

- **Interpretabilitas Intrinsik vs Post-Hoc**
  - *Intrinsik*: Model dirancang agar transparan sejak fase arsitektur, contohnya regresi linear atau decision tree. Mekanisme keputusannya dapat dibaca langsung tanpa alat bantu eksternal.
  - *Post-Hoc*: Diterapkan pada model kompleks yang sudah selesai dilatih, seperti CNN atau Vision Transformer. Metode ini menjelaskan perilaku model setelah training, dengan contoh populer seperti Grad-CAM, SHAP, dan LIME.

- **Penjelasan Lokal vs Global**
  - *Lokal*: Berfokus pada satu instance prediksi spesifik, misalnya mengapa gambar A diklasifikasikan sebagai kucing. Cocok untuk debugging error case atau audit kasus individual.
  - *Global*: Mengungkap pola perilaku model secara keseluruhan, seperti fitur visual atau kanal mana yang paling dominan digunakan di seluruh dataset. Berguna untuk validasi bias dataset atau alignment dengan domain knowledge.

Poin kritis yang perlu ditekankan adalah bahwa penjelasan visual belum tentu sama dengan mekanisme keputusan aktual model. Visualisasi heatmap atau overlay atribusi sering kali hanya mencerminkan korelasi statistik atau sensitivitas numerik, bukan causal reasoning internal jaringan. Karena itu, kita wajib melakukan uji tambahan untuk memverifikasi *faithfulness*, yaitu sejauh mana penjelasan tersebut benar-benar merepresentasikan logika pengambilan keputusan model. Tanpa verifikasi ini, interpretasi berisiko menyesatkan dan justru melemahkan aspek trustworthy dari sistem.

Untuk mengoperasionalkan konsep faithfulnes ini, slide berikutnya akan membahas implementasi teknis melalui saliency map dan gradient attribution. Kita akan menelusuri bagaimana sensitivitas output dihitung terhadap setiap piksel input, melihat formulasi matematis dasarnya, serta mengidentifikasi keterbatasan awal yang mendorong pengembangan variasi seperti SmoothGrad dan Input × Gradient.

---

## Slide 008 - Saliency Map dan Gradient Attribution

### Narasi

Setelah membahas kerangka konseptual explainability pada slide sebelumnya, termasuk dikotomi antara metode intrinsik dan post-hoc serta cakupan lokal versus global, kita kini memasuki salah satu teknik post-hoc paling fundamental: peta salien berbasis atribusi gradien. Pendekatan ini menjadi batu loncatan krusial sebelum mengevaluasi metode yang lebih canggih.

Gagasan intinya bersifat langsung dan terukur secara matematis. Kita menghitung sensitivitas skor keluaran model terhadap perubahan infinitesimal di setiap piksel input. Prinsipnya sederhana: jika modifikasi pada suatu piksel tertentu menyebabkan pergeseran signifikan pada skor prediksi kelas, maka piksel tersebut dianggap memiliki bobot pengaruh terbesar terhadap keputusan model saat itu.

Secara formulasi, sensitivitas ini diekspresikan melalui turunan parsial dari fungsi skor kelas \( f_c(x) \) terhadap vektor input \( x \). Peta salien \( S(x) \) didefinisikan sebagai magnitudo absolut dari gradien tersebut:
```text
S(x) = | ∂ f_c(x) / ∂ x |
```
Dalam praktik eksperimental, nilai gradien mentah dinormalisasi ke interval \([0, 1]\), kemudian divisualisasikan sebagai peta panas yang ditumpangkan pada citra asli. Daerah dengan intensitas warna lebih hangat secara otomatis menandakan tingkat atribusi yang lebih tinggi bagi model.

Namun, dalam konteks penelitian tingkat lanjut, kita harus kritis terhadap keterbatasan inherent dari pendekatan gradien naif ini. Pertama, sifatnya murni lokal, sehingga hanya menangkap kemiringan permukaan loss di sekitar titik input spesifik tanpa mempertimbangkan struktur global. Kedua, gradien pada jaringan saraf modern cenderung sangat berisik akibat aktivasi ReLU, dropout, dan interaksi bobot yang non-linear. Hal ini sering menghasilkan peta panas yang mendominasi noise atau artefak kompresi, bukan region semantik yang sebenarnya.

Untuk memitigasi masalah stabilitas dan interpretabilitas ini, komunitas riset mengembangkan varian seperti SmoothGrad, yang mengurangi varians gradien dengan melakukan averaging atas multiple perturbed copies, serta Input × Gradient, yang mengalikan input dengan gradiennya untuk menekan respons pada area bernilai nol atau noise. Membangun dari fondasi atribusi gradien ini, slide berikutnya akan memperkenalkan dua metode yang dirancang khusus untuk mengatasi kelemahan lokalitas dan inkonsistensi teoretis, yaitu Grad-CAM yang memanfaatkan feature map konvolusi, serta Integrated Gradients yang mengakumulasi gradien sepanjang jalur integrasi untuk memenuhi aksioma penjelasan yang ketat.

---

## Slide 009 - Grad-CAM dan Integrated Gradients

### Narasi

Pada slide sebelumnya, kita telah membahas dasar-dasar saliency map berbasis gradien piksel. Meskipun intuitif, pendekatan tersebut memiliki keterbatasan mendasar: sensitivitasnya bersifat lokal dan sangat rentan terhadap noise, sehingga sering menghasilkan peta atribusi yang berisik dan kurang semantik. Untuk mengatasi hal ini, kita beralih ke metode yang memanfaatkan representasi fitur tingkat tinggi, yaitu Grad-CAM dan Integrated Gradients.

Grad-CAM (Gradient-weighted Class Activation Mapping) mengubah paradigma dari gradien piksel menjadi gradien pada feature map lapisan konvolusi terakhir. Alih-alih menghitung turunan langsung terhadap input gambar, metode ini mengambil gradien dari skor kelas target terhadap setiap channel feature map, lalu melakukan weighted average berdasarkan signifikansi masing-masing channel. Hasilnya adalah peta aktivasi yang jauh lebih halus, kohesif secara spasial, dan secara semantik relevan dengan objek atau region yang diprediksi model. Pendekatan ini awalnya dirancang untuk arsitektur CNN, namun kini dapat diadaptasi ke Vision Transformer melalui mekanisme attention rollout, menjadikannya alat yang serbaguna dalam ekosistem deep learning modern.

Di sisi lain, Integrated Gradients menawarkan fondasi teoretis yang lebih kuat dengan memenuhi sejumlah aksioma penting seperti sensitivity dan implementation invariance. Metode ini tidak hanya mengandalkan gradien pada titik input tunggal, melainkan mengintegrasikan perubahan gradien sepanjang jalur lurus dari sebuah baseline ke input aktual. Perhatikan formulasi berikut:

```text
IG_i(x) = (x_i - x'_i) × ∫_{α=0}^{1} ∂ f_c(x' + α(x - x')) / ∂ x_i dα
```

Dalam persamaan ini, \(x\) merepresentasikan input asli, \(x'\) adalah baseline (biasanya vektor nol atau rata-rata dataset pelatihan), dan \(\alpha\) berperan sebagai parameter interpolasi yang bergerak dari 0 hingga 1. Dengan mengakumulasi kontribusi gradien di setiap langkah interpolasi, Integrated Gradients mengurangi bias yang muncul dari pemilihan titik evaluasi tunggal, sehingga memberikan estimasi atribusi yang lebih stabil dan konsisten secara matematis.

Secara praktis, keberhasilan kedua metode ini sangat bergantung pada pilihan konfigurasi eksperimental. Baseline yang dipilih, layer tempat ekstraksi gradien dilakukan, serta referensi kelas yang digunakan akan secara langsung memengaruhi bentuk dan interpretasi peta atribusi. Oleh karena itu, dalam konteks penelitian tingkat doktoral, laporan hasil atribusi wajib menyertakan parameter teknis lengkap—termasuk versi model, hyperparameter preprocessing, dan protokol baseline—agar eksperimen dapat direproduksi dan dibandingkan secara kritis dengan state-of-the-art lainnya.

Setelah memperoleh peta atribusi melalui Grad-CAM atau Integrated Gradients, langkah selanjutnya adalah memvalidasi apakah peta tersebut benar-benar mencerminkan region yang esensial bagi keputusan model. Hal ini mengarah pada diskusi pada slide berikutnya mengenai Perturbation Test dan Occlusion Sensitivity, di mana kita akan menguji konsistensi antara prediksi model dan region yang diidentifikasi oleh metode atribusi melalui manipulasi terkontrol pada input.

---

## Slide 010 - Perturbation Test dan Occlusion Sensitivity

### Narasi

Setelah membahas metode atribusi berbasis gradien seperti Grad-CAM dan Integrated Gradients pada slide sebelumnya, kita perlu menguji apakah peta penting yang dihasilkan benar-benar mencerminkan mekanisme internal model, atau sekadar artefak visual yang menarik perhatian. Di sinilah Perturbation Test dan Occlusion Sensitivity berperan sebagai validasi empiris yang wajib dilakukan sebelum klaim interpretabilitas dipublikasikan.

Prinsip dasarnya sangat intuitif namun rigor secara statistik. Jika suatu region pada input gambar memang menjadi penentu utama prediksi model, maka mengganggu atau menutupi region tersebut seharusnya menyebabkan perubahan skor kelas yang signifikan. Sebaliknya, jika atribusi hanya menangkap noise, bias dataset, atau korelasi dangkal, gangguan terkontrol pada region tersebut tidak akan mengubah keluaran model secara bermakna.

Implementasinya dapat dilakukan melalui alur pengujian sistematis berikut:
- Siapkan gambar input dan tentukan ukuran serta langkah geser (stride) untuk jendela occlusion, misalnya patch berukuran 16×16 piksel dengan stride 16.
- Geser patch tersebut melintasi seluruh area gambar secara bertahap.
- Pada setiap posisi, tutupi region yang bersangkutan menggunakan teknik masking seperti pengisian nol (zero-filling), Gaussian blur, atau noise acak.
- Jalankan inferensi model dan catat skor kelas target sebelum dan sesudah masking.
- Hitung selisih skor untuk membangun peta sensitivitas berdasarkan besarnya penurunan kepercayaan model.

Hasil dari proses ini memberikan interpretasi yang lebih objektif dibandingkan sekadar visualisasi heatmap. Region yang menyebabkan penurunan skor drastis saat di-occlude dapat dikategorikan sebagai fitur esensial bagi keputusan model. Untuk penelitian tingkat doktoral, konsistensi antara peta atribusi awal dengan hasil occlusion test menjadi indikator kualitas yang harus dilaporkan secara transparan. Inkonsistensi sering kali mengindikasikan bias dalam arsitektur, masalah dalam normalisasi data, atau overfitting terhadap pola artifaktual yang justru merugikan generalisasi model.

Validasi empiris ini juga menjadi jembatan alami menuju evaluasi metrik atribusi yang lebih formal. Pada slide berikutnya, kita akan membedah perbedaan mendasar antara plausibilitas visual dan faithfulness matematis, serta bagaimana metrik deletion-insertion digunakan untuk mengukur seberapa akurat atribusi tersebut benar-benar merepresentasikan sebab-sebab prediktif model.

---

## Slide 011 - Evaluasi Attribution: Faithfulness vs Plausibility

### Narasi

Pada slide ini, kita beralih dari pengujian sensitivitas region menuju kerangka evaluasi atribusi yang lebih rigor, dengan membedakan dua konsep kunci: *plausibilitas* dan *faithfulness*.

Konsep *plausibilitas* merujuk pada sejauh mana peta atribusi tampak masuk akal secara visual bagi pengamat manusia. Contoh klasiknya adalah peta panas yang terkonsentrasi pada objek utama, bukan pada latar belakang. Meskipun secara intuitif menarik, plausibilitas bersifat subjektif dan sangat rentan terhadap bias kognitif peneliti. Visualisasi yang rapi dan mudah dipahami belum tentu mencerminkan kebenaran mekanistik model.

Sebaliknya, *faithfulness* mengukur konsistensi kausal antara atribusi dengan proses pengambilan keputusan model. Atribusi dikatakan faithful jika menghapus fitur yang dianggap penting menyebabkan perubahan skor prediksi yang signifikan, sedangkan mempertahankan fitur tersebut menjaga stabilitas kelas target. Ini adalah standar empiris yang jauh lebih ketat dan wajib diadopsi dalam penelitian tingkat doktoral.

Untuk mengkuantifikasi *faithfulness*, kita menggunakan dua metrik perturbasi bertahap:
- **Deletion Metric**: piksel dengan skor atribusi tertinggi dihapus secara berurutan (biasanya diganti dengan nol atau noise), lalu akurasi atau skor kelas diamati. Penurunan skor yang tajam menandakan atribusi tersebut faithful.
- **Insertion Metric**: kebalikannya, piksel paling informatif ditambahkan secara bertahap dari kondisi awal acak atau gelap, lalu kenaikan skor dikumpulkan. Kenaikan yang cepat menunjukkan bahwa region tersebut benar-benar membawa sinyal prediktif yang relevan.

Penting untuk ditegaskan bahwa dalam konteks publikasi internasional dan kajian kritis, klaim interpretabilitas model harus didasarkan pada *faithfulness*, bukan sekadar *plausibilitas*. Metrik deletion dan insertion menyediakan landasan kuantitatif yang dapat direplikasi, sehingga meminimalkan risiko validasi subjektif yang sering menjebak peneliti pemula.

Diskusi ini merupakan kelanjutan langsung dari prinsip uji occlusion pada slide sebelumnya. Jika slide sepuluh memperkenalkan gagasan dasar gangguan region, slide sebelas memformalkannya menjadi protokol evaluasi metrik yang terstandarisasi. Selanjutnya, kita akan melihat bagaimana ketidakpatuhan pada prinsip ini menghasilkan skenario atribusi yang menyesatkan, yang akan diuraikan secara eksplisit pada slide berikutnya.

---

## Slide 012 - Skenario Gagal: Attribution yang Menyesatkan

### Narasi

Merujuk pada pembahasan slide sebelumnya mengenai perbedaan fundamental antara *plausibility* dan *faithfulness*, kita kini menghadapi realita empiris di mana teknik atribusi sering kali menghasilkan penjelasan yang menyesatkan. Dalam praktik riset maupun deployment, model tidak selalu mempelajari representasi fitur yang semestinya. Sebagai contoh klasik, sebuah model dapat mengklasifikasikan gambar sebagai "anjing" semata-mata karena adanya latar belakang berupa rumput hijau atau sofa, bukan karena morfologi atau ciri khas anjing itu sendiri.

Masalah kritis muncul ketika visualisasi atribusi seperti Grad-CAM tetap menunjukkan highlight positif pada region objek. Secara subjektif, peta panas tersebut tampak masuk akal bagi manusia, sehingga menciptakan ilusi validitas kausal. Namun, jika dilakukan *occlusion test*, menutup bagian objek utama justru tidak mengubah prediksi model, sedangkan menutup latar belakang menyebabkan perubahan kelas yang signifikan. Bukti ini menegaskan bahwa atribusi yang dihasilkan hanyalah *plausible*, bukan *faithful*.

Untuk standar publikasi dan penelitian tingkat doktoral, pelaporan atribusi tanpa uji kontrafaktual tidak dapat diterima sebagai bukti ilmiah. Setiap klaim bahwa model mengandalkan fitur tertentu harus divalidasi melalui eksperimen perturbasi yang ketat. Auditor dan reviewer harus menerapkan prinsip berikut:
- Atribusi map tidak boleh dilaporkan tanpa uji kontrafaktual.
- Klaim penggunaan fitur X wajib didukung eksperimen perturbasi.
- Pencarian contoh kegagalan harus dilakukan secara aktif, bukan hanya mengandalkan contoh keberhasilan yang diseleksi.

Ketika kita mengakui bahwa peta atribusi rentan terhadap bias dan model cenderung overconfident, pertanyaan natural berikutnya adalah bagaimana model seharusnya mengekspresikan keraguan atas prediksinya. Transisi ini mengarah langsung pada konsep ketidakpastian prediktif, di mana kita perlu memisahkan sumber noise intrinsik dari keterbatasan arsitektur model. Pembahasan mengenai dikotomi *aleatoric* versus *epistemic* uncertainty akan menguraikan kerangka kerja matematis dan eksperimental untuk menangani masalah ini pada slide berikutnya.

---

## Slide 013 - Predictive Uncertainty: Aleatoric vs Epistemic

### Narasi

Setelah membahas skenario kegagalan pada peta atribusi di slide sebelumnya, kita beralih ke dimensi fundamental lain yang menentukan keandalan sistem computer vision: estimasi ketidakpastian prediktif. Peta atribusi seperti Grad-CAM atau saliency map hanya menjawab pertanyaan “region mana yang paling berpengaruh?”, namun tidak memberikan informasi kuantitatif tentang “seberapa yakin model terhadap keputusan tersebut”. Tanpa mekanisme kuantifikasi ketidakpastian yang rigor, klaim performa model tetap rentan terhadap interpretasi yang keliru, terutama ketika di-deploy pada data dunia nyata yang noisy atau out-of-distribution.

Dalam kerangka kerja trustworthy AI modern, ketidakpastian prediktif dipilah menjadi dua sumber utama berdasarkan karakteristik dan sifatnya:
• **Ketidakpastian Aleatorik** merujuk pada varians intrinsik yang melekat pada proses generatif data itu sendiri. Pada domain citra, hal ini termanifestasi sebagai kondisi pencahayaan ekstrem, degradasi optik (kabut, hujan, motion blur), oklusi parsial, atau label ground-truth yang ambigu. Karena sifatnya inherent terhadap distribusi data observasi, penambahan volume data pelatihan tidak akan secara signifikan mengurangi jenis ketidakpastian ini.
• **Ketidakpastian Epistemik** berasal dari keterbatasan model dalam mempelajari pola yang mendasari data. Ini umumnya muncul ketika input uji berada jauh dari manifold distribusi pelatihan, atau ketika arsitektur belum cukup kompeten untuk menangkap variasi kelas tertentu. Berbeda dengan aleatorik, ketidakpastian epistemik bersifat reduksibel melalui pengumpulan data yang lebih representatif, peningkatan kapasitas model, atau strategi regularisasi yang tepat.

Implikasi metodologisnya sangat krusial untuk desain eksperimen tingkat doktor. Model tidak boleh lagi mengandalkan output probabilitas softmax tunggal tanpa disertai indikator keyakinan yang terkalibrasi. Softmax secara inheren cenderung menghasilkan nilai yang overconfident bahkan pada prediksi yang salah, sehingga menyesatkan pipeline downstream atau auditor manusia. Untuk mengestimasi ketidakpastian epistemik secara eksplisit, pendekatan yang telah divalidasi dalam literatur meliputi:
• Bayesian Neural Networks yang memodelkan bobot jaringan sebagai distribusi probabilitas posterior.
• Deep Ensembles yang menggabungkan prediksi dari beberapa model dengan inisialisasi atau subset data berbeda.
• Test-Time Augmentation (TTA) yang mengukur dispersi prediksi di bawah serangkaian transformasi augmentasi.
• Monte Carlo Dropout yang memanfaatkan dropout layer selama fase inferensi untuk melakukan sampling stochastik atas fungsi aktivasi.

Kuantifikasi ketidakpastian merupakan prasyarat teknis menuju sistem vision yang trustworthy, namun bukan jaminan akhir. Memiliki skor ketidakpastian saja tidak cukup jika skor confidence yang dilaporkan tidak konsisten dengan akurasi empiris di lapangan. Jika sebuah model mengklaim confidence 0.90 tetapi hanya benar 0.60 dari total sampel dengan confidence serupa, maka terjadi fenomena miscalibration yang dapat berakibat fatal pada pengambilan keputusan kritis. Pembahasan mengenai cara mendeteksi, mengukur, dan memperbaiki kesenjangan antara confidence score dengan akurasi aktual akan kita bedah secara komprehensif pada slide berikutnya.

---

## Slide 014 - Calibration: Apakah Confidence Dapat Dipercaya?

### Narasi

Pada slide sebelumnya, kita telah membedah dua sumber ketidakpastian dalam prediksi model, yaitu aleatoric yang bersumber dari noise intrinsik data, dan epistemic yang muncul akibat keterbatasan kapasitas atau cakupan data pelatihan. Ketika model berhasil mengestimasi kedua jenis ketidakpastian tersebut, langkah logis berikutnya adalah memvalidasi apakah skor kepercayaan atau confidence score yang dihasilkan benar-benar mencerminkan probabilitas kebenaran prediksi. Di sinilah konsep calibration menjadi fondasi utama dalam membangun computer vision yang trustworthy dan siap deploy di lingkungan produksi.

Masalah klasik pada arsitektur deep learning modern adalah overconfidence. Model sering kali memberikan nilai probabilitas softmax yang sangat tinggi, bahkan ketika prediksinya salah total. Akurasi rata-rata dataset sebesar 90 persen tidak serta-merta menjamin bahwa setiap sampel dengan confidence 0,9 memang memiliki peluang benar sebesar 90 persen. Ketidaksesuaian antara confidence score dan akurasi aktual ini disebut sebagai miscalibration, yang dapat berakibat fatal saat model diintegrasikan ke dalam sistem pengambilan keputusan kritis seperti deteksi objek otonom atau diagnosis citra medis.

Secara formal, sebuah model dikatakan terkalibrasi dengan baik jika memenuhi hubungan berikut:
```text
P(prediksi benar | confidence = p) ≈ p
```
Hubungan ini menyatakan bahwa ketika model menyatakan kepercayaannya berada di angka p, proporsi prediksi yang benar secara empiris harus mendekati nilai p tersebut. Jika persamaan ini terpenuhi, maka confidence score dapat dipertanggungjawabkan sebagai ukuran probabilistik yang valid untuk thresholding, early exit mechanism, atau routing decision pada pipeline inference.

Untuk memperbaiki miscalibration, terdapat tiga pendekatan utama yang sering diimplementasikan dalam praktik riset dan industri:
- **Temperature scaling**: membagi logit mentah oleh parameter suhu \( T > 0 \) sebelum fungsi softmax, sehingga distribusi probabilitas dapat disesuaikan agar lebih realistis tanpa mengubah urutan ranking kelas.
- **MC-Dropout**: menjalankan dropout secara aktif selama fase inferensi sebanyak beberapa kali, lalu menghitung varians dari distribusi prediksi sebagai proxy ketidakpastian model.
- **Ensemble**: menggabungkan output dari beberapa model independen melalui rata-rata probabilitas atau voting, yang secara alami mengurangi varians dan menghasilkan estimasi confidence yang lebih stabil.

Setelah memahami mekanisme perbaikan calibration, pada slide berikutnya kita akan beralih ke evaluasi kuantitatif dan visual. Kita akan mempelajari reliability diagram sebagai alat diagnostik berbasis binning confidence versus akurasi, serta metrik numerik seperti Expected Calibration Error (ECE), Maximum Calibration Error (MCE), dan Brier Score untuk mengukur deviasi kalibrasi secara presisi dan mendukung validasi metodologi penelitian tingkat lanjut.

---

## Slide 015 - Reliability Diagram dan Calibration Metrics

### Narasi

Setelah membahas definisi kalibrasi dan beberapa strategi perbaikan seperti temperature scaling, MC-Dropout, serta ensemble pada slide sebelumnya, langkah selanjutnya adalah mengkuantifikasi seberapa baik model kita sebenarnya terkalibrasi. Untuk tujuan evaluasi empiris yang rigor, kita mengandalkan visualisasi dan metrik numerik yang objektif.

Reliability diagram menjadi alat visual utama dalam audit kalibrasi. Prosesnya dimulai dengan mengelompokkan seluruh skor kepercayaan prediksi ke dalam sepuluh bin yang memiliki rentang confidence sama lebar. Pada sumbu horizontal, kita tempatkan rata-rata confidence dari setiap bin, sedangkan sumbu vertikal menunjukkan akurasi aktual atau fraksi positif yang benar diprediksi di bin tersebut. Jika model memiliki kalibrasi sempurna, titik-titik hasil pengelompokan akan jatuh tepat pada garis diagonal dari sudut kiri bawah ke kanan atas.

Interpretasi deviasi titik dari garis diagonal memberikan diagnosis langsung terhadap perilaku model. Ketika titik-titik berada di bawah garis diagonal, model menunjukkan sifat overconfident atau terlalu percaya diri, artinya probabilitas yang diberikan lebih tinggi daripada akurasi nyatanya. Sebaliknya, posisi titik di atas diagonal menandakan underconfident, di mana model cenderung meremehkan kemampuan prediksinya sendiri. Dalam konteks penelitian tingkat doktoral, memahami pola ini penting untuk menentukan apakah ketidakpastian model berasal dari bias sistematis atau keraguan yang wajar.

Untuk mengubah observasi visual menjadi angka yang dapat dibandingkan antar-eksperimen, kita menggunakan Expected Calibration Error atau ECE. Rumusnya dapat dituliskan sebagai berikut:
```text
ECE = Σ_{m=1}^{M} (|B_m| / N) × |acc(B_m) - conf(B_m)|
```
Komponen \(|B_m| / N\) berfungsi sebagai bobot proporsional yang merepresentasikan jumlah sampel dalam bin ke-\(m\) terhadap total dataset \(N\). Nilai \(acc(B_m)\) dan \(conf(B_m)\) masing-masing adalah akurasi aktual dan rata-rata confidence pada bin tersebut. ECE secara efektif menghitung rata-rata tertimbang dari selisih absolut antara performa nyata dan keyakinan model. Semakin rendah nilai ECE, semakin konsisten kalibrasi model, yang menjadi prasyarat dasar sebelum mengevaluasi robustness atau generalisasi lebih lanjut.

Selain ECE, dua metrik pendukung sering digunakan untuk melengkapi analisis:
- **MCE (Maximum Calibration Error)**: menangkap skenario terburuk dengan mengambil nilai error maksimum dari satu bin tertentu, sehingga sangat sensitif terhadap outlier kalibrasi.
- **Brier Score**: mengukur rata-rata kuadrat selisih antara probabilitas prediksi dan label aktual (biasanya dikodekan sebagai 0 atau 1). Metrik ini tidak hanya menghukum prediksi yang salah, tetapi juga memberikan penalti lebih besar ketika model sangat yakin namun keliru, menjadikannya indikator probabilistik yang ketat untuk trustworthy computer vision.

Evaluasi metrik-metrik ini biasanya dilakukan pada distribusi data uji statis terlebih dahulu. Namun, dalam skenario dunia nyata, kondisi input sering berubah seiring waktu. Perubahan ini membawa kita pada konsep fundamental berikutnya yang akan menguji ketahanan kalibrasi dan kepercayaan model, yaitu distribusi shift.

---

## Slide 016 - Distribution Shift: Definisi

### Narasi

Pada slide sebelumnya, kita telah membahas pengukuran kalibrasi model melalui diagram keandalan dan metrik seperti ECE serta Brier Score. Namun, seluruh metrik kalibrasi tersebut mengasumsikan bahwa data uji mengikuti distribusi yang sama dengan data pelatihan, atau setidaknya bersifat stasioner. Ketika asumsi independen dan terdistribusi secara identik (i.i.d.) ini dilanggar, kalibrasi yang tampak baik pada data standar dapat runtuh secara instan. Inilah alasan fundamental mengapa kita perlu mengintegrasikan analisis *distribution shift* ke dalam kerangka kerja *trustworthy computer vision*.

Secara formal, *distribution shift* didefinisikan sebagai perubahan distribusi probabilitas data antara fase pelatihan dan fase inferensi. Jika data pelatihan diasumsikan berasal dari distribusi joint $P_{train}(x, y)$ dan data uji berasal dari $P_{test}(x, y)$, maka pergeseran distribusi terjadi ketika $P_{train} \neq P_{test}$. Perbedaan ini tidak selalu seragam; ia dapat memengaruhi margin input, margin label, maupun struktur hubungan kondisional di antara keduanya.

Dalam praktik *Computer Vision*, fenomena ini sangat lazim dan berdampak langsung pada generalisasi model. Beberapa manifestasi konkretnya meliputi:
- Pergantian perangkat akuisisi, di mana model yang dilatih pada foto DSLR mengalami degradasi signifikan saat diuji pada citra smartphone atau kamera industri.
- Variasi lingkungan eksternal seperti perubahan intensitas pencahayaan, cuaca ekstrem, atau pergantian musim yang mengubah statistik piksel secara masif.
- Heterogenitas domain medis, di mana karakteristik histopatologi atau radiologi berbeda tajam antar rumah sakit akibat variasi protokol pencitraan dan demografi pasien.
- Penggunaan data sintetis dari model generatif seperti *diffusion models* sebagai proxy data uji, yang berpotensi memperkenalkan bias distribusi artifisial yang tidak ada di dunia nyata.

Pentingnya mendeteksi *distribution shift* terletak pada fakta bahwa akurasi tinggi pada benchmark standar tidak menjamin keandalan operasional. Audit kepercayaan model harus secara eksplisit menguji ketahanan arsitektur terhadap pergeseran distribusi sebelum diklaim siap untuk deployment. Tanpa pengujian berbasis shift ini, evaluasi model hanya bersifat deskriptif terhadap data historis dan kehilangan relevansi kritis untuk penelitian tingkat lanjut.

Pada slide berikutnya, kita akan membedah klasifikasi teknis *distribution shift* menjadi tiga kategori utama berdasarkan komponen distribusi yang terpengaruh. Pemahaman mengenai tipe-tipe pergeseran ini akan menjadi dasar metodologis untuk merancang matriks gangguan yang komprehensif dalam eksperimen robustness Anda.

---

## Slide 017 - Jenis Distribution Shift

### Narasi

Setelah pada slide sebelumnya kita mendefinisikan *distribution shift* sebagai ketidaksesuaian antara distribusi data pelatihan dan data inferensi, serta menegaskan bahwa akurasi pada set standar tidak menjamin keandalan di lingkungan nyata, kini kita perlu menguraikan taksonomi pergeseran tersebut secara lebih presisi. Pada jenjang doktoral, pembedaan jenis *shift* bukan sekadar terminologi, melainkan fondasi metodologis yang menentukan arah hipotesis, desain eksperimen, dan validitas klaim kontribusi ilmiah Anda.

Slide ini menyajikan tiga tipe utama *distribution shift* yang sering muncul dalam literatur *computer vision* mutakhir. Tabel tersebut membedakan berdasarkan komponen probabilitas mana yang mengalami perubahan. Pertama, *covariate shift* terjadi ketika distribusi marginal input $P(x)$ berubah, namun fungsi pemetaan dari citra ke label $P(y \mid x)$ tetap invariant. Contoh klasik meliputi variasi iluminasi, resolusi sensor, atau gaya rendering sintetis. Kedua, *label shift* ditandai dengan perubahan distribusi prior kelas $P(y)$, sementara karakteristik visual intrinsik tiap kelas $P(x \mid y)$ tidak bergeser. Fenomena ini umum pada domain medis atau agronomi di mana prevalensi kategori target fluktuatif antar wilayah atau musim. Ketiga, *concept shift* merupakan skenario paling menantang karena relasi semantik antara fitur visual dan label itu sendiri yang berubah, sehingga $P(y \mid x)$ tidak lagi stabil. Kasus ini muncul ketika pedoman klasifikasi diperbarui, atau ketika makna visual suatu objek berevolusi dalam konteks sosial-teknis yang dinamis.

Implikasi praktis dari klasifikasi ini terhadap evaluasi model sangat krusial. Robustness terhadap satu jenis pergeseran tidak dapat digeneralisasi ke jenis lainnya. Model yang tahan terhadap *covariate shift* bisa kolaps total saat menghadapi *concept shift*, begitu pula sebaliknya. Oleh karena itu, setiap proposal penelitian harus secara eksplisit menyatakan jenis *shift* mana yang menjadi fokus investigasi, dan alat ukur yang dipilih harus selaras dengan karakteristik pergeseran tersebut. Evaluasi tidak boleh lagi bersifat ad-hoc atau bergantung pada satu skenario gangguan saja; diperlukan matriks pengujian yang terstruktur untuk memetakan respons model terhadap berbagai dimensi pergeseran distribusi secara paralel.

Pemahaman tentang jenis-jenis *shift* ini akan langsung diterjemahkan ke dalam protokol evaluasi yang sistematis pada slide berikutnya. Kita akan membahas bagaimana merancang benchmark yang valid, menghitung skor degradasi performa secara kuantitatif, serta standar pelaporan hasil yang menghindari jebakan metodologis umum seperti penggunaan augmentasi acak sebagai proxy pergeseran distribusi yang sesungguhnya.

---

## Slide 018 - Mengukur Robustness terhadap Distribution Shift

### Narasi

Setelah pada slide sebelumnya kita menguraikan tiga tipe utama distribusi shift—covariate shift, label shift, dan concept shift—serta implikasinya terhadap desain evaluasi, langkah logis berikutnya adalah menerjemahkan pemahaman teoretis tersebut ke dalam protokol pengukuran yang terstandarisasi. Pada slide ini, kita akan membahas prosedur sistematis untuk mengukur robustness model terhadap pergeseran distribusi, yang menjadi prasyarat fundamental dalam penelitian tingkat doktoral.

Protokol evaluasi yang rigor harus mengikuti lima langkah berurutan:
1. Definisikan secara eksplisit jenis shift yang akan diuji berdasarkan hipotesis penelitian.
2. Siapkan dataset uji yang sesuai, baik menggunakan benchmark publik yang telah dimodifikasi maupun koleksi data domain-spesifik yang Anda kurasi.
3. Tentukan skenario inference: apakah model mengandalkan teknik domain adaptation selama fine-tuning, atau dievaluasi langsung dalam kondisi zero-shot tanpa penyesuaian bobot.
4. Laporkan performa secara terpisah pada distribusi training (clean) dan distribusi target (shifted).
5. Hitung metrik degradasi menggunakan rumus berikut:
```text
Degradation = Akurasi_clean - Akurasi_shifted
```
Nilai degradation score ini berfungsi sebagai indikator kuantitatif langsung mengenai seberapa besar kinerja model menurun ketika dihadapkan pada data yang keluar dari distribusi pembelajaran.

Dalam penyajian hasil, hindari hanya melaporkan akurasi agregat global. Pecah metrik evaluasi per sub-kelompok data atau per kategori kelas agar bias tersembunyi dapat terdeteksi. Selain itu, laporan wajib mencakup analisis confidence score dan calibration error pada data shifted. Model yang menghasilkan prediksi tinggi namun salah pada distribusi baru mengindikasikan miscalibration yang serius. Sertakan pula galeri contoh kegagalan representatif sebagai bukti kualitatif yang memperkuat temuan numerik.

Peneliti sering terjebak pada dua kesalahan metodologis yang mengurangi kredibilitas klaim robustness. Pertama, menggunakan augmentasi random sebagai proxy untuk mensimulasikan distribution shift. Augmentasi geometris atau warna bersifat deterministik dan tidak mereplikasi pergeseran statistik populasi yang sesungguhnya. Kedua, menguji hanya satu jenis noise atau gangguan tunggal. Untuk level S3, Anda harus merancang matriks pengujian multi-dimensi yang mencakup kombinasi shift nyata, bukan sekadar variasi artifisial.

Dengan menerapkan kerangka evaluasi ini, fondasi eksperimen Anda akan memenuhi standar reproduktibilitas dan ketelitian akademis. Pendekatan sistematis terhadap pengukuran degradasi juga membuka jalan bagi diskusi tentang bentuk perturbasi yang lebih halus namun secara struktural lebih merusak. Hal ini secara alami mengarah pada pembahasan adversarial example, di mana modifikasi input sengaja dioptimalkan untuk menjebak decision boundary model, sebagaimana akan kita bedah pada slide berikutnya.

---

## Slide 019 - Adversarial Example: Definisi dan Motivasi

### Narasi

Setelah membahas protokol evaluasi untuk mengukur ketahanan model terhadap pergeseran distribusi pada slide sebelumnya, kita kini beralih ke salah satu tantangan paling kritis dalam keandalan sistem computer vision modern, yaitu adversarial example. Berbeda dengan distribution shift yang umumnya terjadi secara alami atau akibat perubahan kondisi lingkungan, adversarial example melibatkan modifikasi input yang disengaja namun sangat halus.

Secara formal, adversarial example didefinisikan sebagai variasi dari input asli $x$ yang ditambahkan dengan gangguan kecil $\delta$, sehingga menghasilkan prediksi model $f(x')$ yang meleset jauh dari label sebenarnya $y_{true}$. Bentuk matematisnya dapat dituliskan sebagai berikut:
```text
x' = x + δ,   ||δ||_p ≤ ε,   f(x') ≠ y_true
```
Parameter $\varepsilon$ membatasi norma dari gangguan agar tetap berada dalam batas yang tidak terdeteksi oleh persepsi manusia. Meskipun perubahan pikselnya sangat minim, model dapat mengubah klasifikasinya secara drastis, seperti mengidentifikasi gambar panda sebagai gibbon, atau rambu berhenti sebagai rambu batas kecepatan. Fenomena ini menunjukkan bahwa representasi internal model mungkin belum sepenuhnya menangkap semantik visual yang sesungguhnya.

Motivasi penelitian di balik studi adversarial example sangat relevan dengan tujuan kita dalam membangun computer vision yang trustworthy:
- Memahami apakah model benar-benar memahami konten visual atau hanya mengandalkan pola permukaan yang rapuh.
- Menguji batas keamanan pada sistem aplikasi kritis seperti kendaraan otonom, pengenalan wajah, dan diagnosis medis.
- Menegaskan bahwa akurasi benchmark saja tidak cukup untuk klaim kepercayaan, karena model bisa sangat sensitif terhadap gangguan yang tidak terlihat.

Memahami definisi dan motivasi ini menjadi fondasi penting sebelum kita mendalami mekanisme serangan itu sendiri. Pada slide berikutnya, kita akan menguraikan dua metode generasi adversarial example yang paling fundamental, yaitu FGSM dan PGD, serta membedah perbedaan antara skenario white-box dan black-box dalam konteks penyerangan model.

---

## Slide 020 - Serangan Adversarial: FGSM dan PGD

### Narasi

Setelah membahas definisi dan motivasi di balik adversarial example pada slide sebelumnya, kita kini masuk ke mekanisme konstruktif bagaimana serangan tersebut direalisasikan. Dua algoritma yang menjadi pilar utama dalam literatur computer vision adalah FGSM dan PGD. Keduanya memanfaatkan informasi gradien dari fungsi loss untuk menghasilkan gangguan yang memaksimalkan kesalahan prediksi model.

FGSM atau Fast Gradient Sign Method merupakan pendekatan one-step yang sangat efisien secara komputasi. Metode ini menghitung gangguan berdasarkan tanda dari gradien loss terhadap input gambar. Rumus dasarnya dapat ditulis sebagai:
```text
x' = x + ε · sign(∇_x L(f(x), y))
```
Di sini, ε mengendalikan besaran gangguan agar tetap imperceptible, sedangkan sign(∇_x L(f(x), y)) mengarahkan perubahan pixel sesuai arah yang paling meningkatkan loss. Keunggulan FGSM terletak pada kecepatannya yang memungkinkan generasi contoh adversarial secara real-time. Namun, karena hanya melakukan satu langkah optimasi, hasilnya sering kali belum mencapai batas maksimal kegagalan model, sehingga kurang optimal dibandingkan metode iteratif.

Untuk mengatasi keterbatasan FGSM, PGD atau Projected Gradient Descent hadir sebagai varian yang jauh lebih agresif dan dianggap sebagai standar evaluasi robustness modern. PGD melakukan beberapa langkah kecil dengan ukuran langkah α, lalu memproyeksikan kembali hasil update ke dalam bola radius ε menggunakan operasi clipping. Implementasinya mengikuti rumus:
```text
x_{t+1} = clip_{x, ε}(x_t + α · sign(∇_x L(f(x_t), y)))
```
Proses berulang ini memastikan bahwa setiap iterasi tetap mempertahankan batasan visual yang tidak terlihat oleh manusia, sekaligus meningkatkan probabilitas model tertipu secara signifikan. PGD sering digunakan sebagai baseline dalam benchmark keamanan model deep learning karena konsistensinya dalam menghasilkan serangan yang kuat.

Selain dimensi waktu iterasi, konteks akses penyerang terhadap model menjadi variabel kritis yang menentukan strategi serangan. Pada skenario white-box, penyerang memiliki akses penuh ke arsitektur, bobot terlatih, dan kemampuan menghitung gradien secara eksplisit. Hal ini memungkinkan FGSM dan PGD bekerja secara optimal karena informasi gradien tersedia secara langsung. Sebaliknya, black-box attack terjadi ketika penyerang hanya dapat mengamati output model, misalnya melalui query API atau log prediksi tanpa mengetahui parameter internal. Dalam kondisi ini, penyerang biasanya mengandalkan fenomena transferability, yaitu karakteristik di mana adversarial example yang dirancang untuk satu model berhasil menipu model lain yang berbeda arsitekturnya, bahkan jika model target tidak pernah melihat data pelatihan serupa.

Pemahaman mendalam mengenai FGSM, PGD, serta dinamika white-box versus black-box ini menjadi prasyarat penting sebelum merancang mekanisme pertahanan. Karena kekuatan serangan terus berevolusi, klaim robustness tidak dapat lagi divalidasi secara statis. Slide berikutnya akan menguraikan bagaimana komunitas peneliti merespons ancaman ini melalui strategi seperti adversarial training dan certified robustness, serta mengapa terdapat kesenjangan evaluasi yang menuntut penggunaan adaptive attacker dalam protokol benchmark modern.

---

## Slide 021 - Pertahanan Adversarial dan Gap Evaluasi

### Narasi

Setelah membahas mekanisme serangan adversarial seperti FGSM dan PGD pada slide sebelumnya, kita kini beralih ke strategi pertahanan dan tantangan evaluasinya. Dalam konteks penelitian tingkat lanjut, pertahanan terhadap contoh adversarial tidak lagi bersifat ad-hoc, melainkan memerlukan kerangka kerja yang rigor, dapat direproduksi, dan divalidasi secara kritis.

Strategi pertahanan yang umum diimplementasikan dalam literatur terkini meliputi:
- **Adversarial training**: melatih model secara langsung pada contoh serangan yang dihasilkan setiap iterasi, sehingga jaringan mempelajari representasi yang lebih invariant terhadap perturbasi kecil.
- **Preprocessing**: menerapkan denoising, kompresi, atau randomisasi input untuk membersihkan sinyal noise yang dimanfaatkan penyerang sebelum data masuk ke model.
- **Certified robustness**: memberikan jaminan matematis bahwa performa model tetap stabil dalam radius gangguan ε tertentu, meskipun penerapannya masih terbatas pada skenario atau arsitektur spesifik.

Meskipun demikian, praktik penelitian sering kali menghadapi *gap evaluasi* yang signifikan. Pertahanan yang tampak efektif pada satu jenis serangan statis cenderung gagal total ketika dihadapkan pada serangan adaptif. Penyerang adaptif mampu mengamati struktur pertahanan dan menyesuaikan parameter serta alur serangannya secara dinamis. Oleh karena itu, protokol evaluasi wajib menyertakan *adaptive attacker* yang setara atau lebih kuat daripada metode yang diujikan. Aturan fundamental dalam bidang ini menegaskan bahwa klaim robustness hanya sah jika diuji oleh penyerang yang sekuat mungkin.

Perlu juga dicatat bahwa peningkatan robustness umumnya berada dalam hubungan *trade-off* dengan akurasi pada data bersih. Laporan penelitian yang kredibel harus melaporkan kedua metrik tersebut secara eksplisit, bukan hanya mengandalkan akurasi standar yang tinggi. Evaluasi yang transparan dan metodologis ini menjadi prasyarat penting sebelum kita menyoroti dimensi lain dari *trustworthy computer vision*, khususnya isu keadilan algoritmik yang akan dibahas pada slide berikutnya.

---

## Slide 022 - Fairness: Definisi dan Relevansi pada CV

### Narasi

Setelah membahas strategi pertahanan adversarial dan kesenjangan dalam evaluasi robustness pada slide sebelumnya, kita kini beralih ke dimensi lain dari trustworthiness dalam computer vision, yaitu fairness atau keadilan algoritmik. Dalam konteks pengolahan citra digital, fairness didefinisikan sebagai ketiadaan bias sistematis yang merugikan kelompok tertentu berdasarkan atribut sensitif seperti gender, usia, etnis, atau kondisi fisik. Konsep ini bukan sekadar isu etika sosial, melainkan parameter teknis yang harus diukur secara rigor dalam setiap pipeline vision modern.

Implementasi model computer vision sering kali mengungkap disparitas kinerja yang signifikan antar-demografi. Sebagai contoh, sistem face recognition historis menunjukkan akurasi yang lebih rendah pada subjek berkulit gelap dibandingkan kulit terang. Pada aplikasi industri, algoritma seleksi rekrutmen berbasis analisis video cenderung mendiskualifikasi kandidat dari latar belakang etnis tertentu secara tidak proporsional. Di bidang kesehatan, model segmentasi medis dapat mengalami penurunan performa pada kelompok usia atau jenis kelamin yang kurang terwakili dalam data pelatihan. Fenomena-fenomena ini membuktikan bahwa bias visual bukanlah anomali, melainkan pola sistemik yang perlu diidentifikasi sejak fase desain model.

Dari perspektif penelitian tingkat doktoral, konsekuensi ilmiahnya sangat krusial. Laporan metrik agregat seperti mean accuracy atau mAP rata-rata sering kali menutupi kesenjangan performa antar-subpopulasi. Sebuah model mungkin mencapai akurasi global yang tinggi, namun gagal total pada kelompok minoritas. Oleh karena itu, audit trustworthiness wajib melaporkan metrik secara disagregat atau per-kelompok. Pendekatan ini memungkinkan peneliti mengkuantifikasi trade-off antara efisiensi komputasi dan keadilan, serta memberikan dasar empiris untuk perbaikan arsitektur atau strategi sampling data.

Memahami definisi dan dampak fairness membawa kita langsung ke pertanyaan metodologis berikutnya: bagaimana bias tersebut terbentuk? Slide selanjutnya akan membedah sumber bias pada setiap tahap pipeline computer vision, mulai dari koleksi dataset, proses anotasi, augmentasi, hingga pemilihan arsitektur dan evaluasi. Analisis ini diperlukan untuk merancang mitigasi yang tepat sasaran sebelum model di-deploy ke lingkungan produksi.

---

## Slide 023 - Sumber Bias pada Pipeline Computer Vision

### Narasi

Setelah mendefinisikan fairnes dan memahami konsekuensi ilmiahnya pada slide sebelumnya, kita kini perlu menelusuri secara sistematis di mana bias sebenarnya masuk ke dalam sebuah sistem computer vision. Bias bukanlah fenomena yang muncul tiba-tiba saat inference, melainkan akumulasi dari berbagai keputusan desain dan operasional sepanjang pipeline pengolahan data hingga evaluasi model.

Pada tahap dataset, bias sering kali berakar dari distribusi kelas yang tidak seimbang atau under-representasi kelompok tertentu. Jika data latih didominasi oleh satu demografi atau kondisi lingkungan, model akan belajar pola yang tidak generalisasi. Tahap anotasi juga rentan terhadap subjektivitas. Ketidakseragaman standar antara labeler, ambiguitas definisi kategori, atau proses crowdsourcing yang tidak terkontrol dapat menghasilkan ground truth yang noise dan bias.

Proses augmentasi citra, yang biasanya dianggap sebagai teknik penguat generalisasi, justru bisa menjadi sumber bias jika transformasi yang dipilih memperkuat pola artifisial atau menghilangkan variasi alami yang penting untuk representasi kelompok minoritas. Di sisi arsitektur model, pemilihan loss function atau struktur jaringan mungkin secara implisit lebih cocok untuk mode data tertentu, sehingga mengabaikan karakteristik sampel lain. Terakhir, tahap evaluasi sering kali luput dari perhatian padahal test set yang tidak mewakili populasi dunia nyata akan memberikan gambaran performa yang menyesatkan.

Untuk melakukan audit yang rigor, kita harus mengajukan pertanyaan kritis di setiap titik tersebut:
- Dari mana data berasal dan bagaimana kriteria samplingnya?
- Siapa yang bertanggung jawab atas anotasi, dan metrik konsistensi antar-labeler seperti Cohen’s Kappa atau Fleiss’ Kappa sudah diterapkan?
- Kelompok demografis atau kontekstual mana yang berpotensi terpinggirkan dalam koleksi data?

Jawaban atas pertanyaan-pertanyaan ini akan membentuk dasar empiris sebelum kita bergerak ke kuantifikasi bias. Ketika lokasi bias telah diidentifikasi secara kualitatif, langkah selanjutnya adalah mengukur dampaknya secara numerik. Slide berikutnya akan membahas metrik-metrik fairness yang umum digunakan, seperti Demographic Parity, Equalized Odds, dan Calibration by Group, serta cara menerapkannya dalam workflow penelitian tingkat doktoral untuk memastikan bahwa klaim performa model benar-benar trustworthy dan robust.

---

## Slide 024 - Metrik Fairness

### Narasi

Setelah mengidentifikasi sumber bias pada setiap tahap pipeline computer vision, langkah selanjutnya adalah mengukur seberapa besar ketidakadilan yang muncul dalam performa model. Slide ini membahas metrik fairness yang menjadi standar evaluasi dalam penelitian trustworthy computer vision.

Terdapat empat metrik utama yang sering diadopsi dalam literatur terkini:
- **Demographic Parity**: Menuntut agar probabilitas prediksi positif tetap konsisten di seluruh kelompok sensitif.
- **Equalized Odds**: Memperketat syarat ini dengan memastikan false positive rate dan false negative rate tidak berbeda signifikan antar kelompok.
- **Equal Opportunity**: Berfokus hanya pada true positive rate, sehingga cocok untuk skenario di mana deteksi kasus positif merupakan prioritas mutlak.
- **Calibration by Group**: Menilai apakah tingkat kepercayaan model berkorelasi linear dengan akurasi aktual di setiap subgroup.

Implementasi metrik-metrik ini memerlukan protokol evaluasi yang sistematis:
1. Definisikan atribut sensitif yang relevan dengan konteks aplikasi.
2. Kelompokkan sampel berdasarkan atribut tersebut tanpa mengubah struktur dataset asli.
3. Hitung metrik performa utama untuk masing-masing kelompok.
4. Hitung selisih maksimum atau rasio antar kelompok sebagai indikator disparitas.
5. Laporkan temuan secara transparan dalam model card atau dokumen evaluasi teknis.

Perlu ditekankan bahwa fairness tidak berarti menghilangkan semua perbedaan numerik. Penilaian keadilan harus selalu dinilai sesuai konteks aplikasi, trade-off performa, dan implikasi etis di dunia nyata. Pada level doktoral, mahasiswa diharapkan mampu memilih metrik yang paling sesuai dengan risk profile sistem, serta merancang eksperimen yang menguji robustness metrik terhadap shift distribusi data.

Pembahasan metrik klasifikasi ini menjadi fondasi penting sebelum kita menyoroti tantangan unik pada tugas deteksi dan segmentasi. Karena outputnya bersifat spasial dan melibatkan banyak prediksi per gambar, penerapan fairness memerlukan adaptasi metrik dan strategi evaluasi yang lebih kompleks, yang akan kita bahas pada slide berikutnya.

---

## Slide 025 - Fairness pada Detection dan Segmentation

### Narasi

Pada slide sebelumnya, kita telah membahas metrik fairness standar yang umum diterapkan pada masalah klasifikasi citra, seperti Demographic Parity, Equalized Odds, hingga Calibration by Group. Namun, ketika kita beralih ke tugas deteksi objek dan segmentasi, struktur prediksi berubah secara fundamental. Berbeda dengan klasifikasi yang menghasilkan satu label per gambar, model deteksi dan segmentasi memunculkan banyak prediksi dalam satu frame. Hal ini menuntut penyesuaian mendasar dalam cara kita mendefinisikan dan mengukur fairness.

Evaluasi fairness pada konteks ini dapat dilakukan pada tiga level utama:
- **Evaluasi per gambar**: memeriksa apakah setiap individu atau objek dalam frame memiliki probabilitas terdeteksi yang setara antar kelompok atribut sensitif.
- **Evaluasi per objek**: menyoroti presisi dan recall spesifik untuk setiap kelas atau kelompok demografis yang muncul dalam dataset.
- **Evaluasi per area**: berfokus pada kualitas mask segmentasi, misalnya seberapa akurat batas objek dipisahkan untuk kelompok tertentu tanpa bias sistematis.

Dalam praktik penelitian, bias ini sering kali tersembunyi di balik metrik agregat yang tampak memuaskan. Contoh bias yang perlu diwaspadai meliputi:
- Detector mobil yang lebih sering mendeteksi objek pada wilayah terang dibandingkan area gelap atau dengan kontras rendah.
- Model segmentasi bangunan yang salah memisahkan area permukiman pada citra kota tertentu akibat distribusi geometri yang tidak seimbang.
- SAM yang dilatih pada data internet berskala besar yang menunjukkan kinerja tidak konsisten pada objek langka atau domain yang jarang terwakili.

Implikasi langsung dari fenomena ini adalah bahwa laporan evaluasi model tidak boleh lagi hanya menyajikan angka rata-rata global. Kita wajib memisahkan perhitungan metrik berdasarkan kelompok atribut sensitif, dan analisis kegagalan harus dilengkapi dengan visualisasi contoh konkret per kelompok. Pendekatan ini sangat krusial bagi peneliti tingkat doktoral, karena mengungkap bias spasial atau kontekstual sering kali menjadi pintu masuk identifikasi research gap yang relevan untuk pengembangan arsitektur baru, mekanisme attention masking, atau strategi data augmentation yang lebih robust.

Dengan demikian, pemahaman mendalam tentang fairness di ranah deteksi dan segmentasi menjadi fondasi penting sebelum kita merancang strategi komunikasi risiko model. Langkah selanjutnya akan mengarah pada bagaimana temuan-temuan evaluasi ini didokumentasikan secara transparan melalui Model Card, yang akan kita bahas pada slide berikutnya.

---

## Slide 026 - Model Card: Komunikasi Risiko Model

### Narasi

Setelah menelaah bagaimana fairness dievaluasi secara spesifik pada tugas deteksi dan segmentasi, kita kini beralih ke instrumen transparansi yang diperlukan untuk mengomunikasikan temuan tersebut kepada berbagai pemangku kepentingan. Pada tingkat doktoral, pemahaman mendalam tentang mekanisme akuntabilitas model sama pentingnya dengan peningkatan akurasi algoritma. Di sinilah Model Card hadir sebagai standar praktis dalam ekosistem computer vision modern.

Model Card merupakan dokumen ringkas yang dirancang khusus untuk menyertai setiap model yang dikembangkan atau dirilis. Dokumen ini tidak bertujuan menggantikan paper teknis, melainkan menyediakan ringkasan aksesibel yang mencakup empat pilar utama: tujuan model, konteks penggunaan yang dimaksudkan, karakteristik data pelatihan dan evaluasi, serta metrik performa inti. Yang paling krusial, Model Card wajib mendokumentasikan keterbatasan struktural dan risiko yang telah diidentifikasi selama siklus pengembangan.

Kebutuhan mendesak akan Model Card muncul akibat kesenjangan informasi antara peneliti dan pengguna akhir. Pengembang sistem, praktisi industri, maupun pembuat kebijakan umumnya tidak memiliki kapasitas untuk menelaah seluruh literatur teknis secara detail. Model Card menjembatani celah ini dengan menyajikan informasi kritis secara terstruktur dan terstandarisasi. Selain itu, proses penyusunannya memaksa tim riset untuk melakukan audit internal yang jujur terhadap kelemahan model, sehingga menjadi bukti empiris bahwa aspek trustworthiness telah diintegrasikan secara sistematis sejak awal.

Prinsip fundamental dari pendekatan ini adalah bahwa setiap klaim mengenai kinerja atau keamanan model harus didukung oleh bukti eksperimen yang dapat direproduksi. Model Card berfungsi sebagai artefak audit yang valid, sekaligus fondasi komunikasi risiko sebelum model di-deploy ke lingkungan produksi. Pembahasan selanjutnya akan mengurai komponen-komponen spesifik yang wajib termuat dalam struktur Model Card agar memenuhi standar transparansi akademik dan regulasi terkini.

---

## Slide 027 - Contoh Komponen Model Card

### Narasi

Slide ini menguraikan secara struktural komponen-komponen esensial yang wajib termuat dalam sebuah Model Card. Sebagai kelanjutan dari konsep komunikasi risiko pada slide sebelumnya, dokumen ini berfungsi sebagai jembatan teknis antara implikasi teoretis model dan implementasi praktisnya di lapangan. Setiap baris pada tabel merepresentasikan dimensi audit yang harus diverifikasi sebelum model dirilis atau dipublikasikan.

Bagian *model details* memerlukan spesifikasi teknis yang lengkap: arsitektur jaringan, versi hyperparameter, framework komputasi, dan skema lisensi. Presisi pada bagian ini mendukung reproduktibilitas penelitian, standar mutlak untuk publikasi tingkat doktoral. Selanjutnya, *intended use* harus mendefinisikan domain aplikasi, profil pengguna akhir, serta batasan eksplisit mengenai konteks yang tidak disarankan untuk digunakan. Kejelasan batas ini mencegah misaplikasi model di luar cakupan validitasnya.

Komponen *factors* mencatat variabel kontekstual atau atribut sensitif yang berpotensi memengaruhi distribusi data, seperti kondisi pencahayaan, resolusi sensor, atau karakteristik demografis. Pada bagian *metrics*, hindari ketergantungan tunggal pada akurasi. Integralkan Expected Calibration Error (ECE), laju degradasi robustness terhadap perturbasi, dan fairness gap antar subpopulasi. Metrik gabungan ini memberikan gambaran holistik tentang keandalan dan keadilan model.

Transparansi data menjadi fondasi validitas klaim riset. Kolom *training data* dan *evaluation data* menuntut dokumentasi sumber, volume sampel, rentang temporal pengumpulan, protokol preprocessing, serta strategi split dataset. Sementara itu, *ethical considerations* dan *caveats* berfungsi sebagai mekanisme mitigasi proaktif, mencatat potensi bias sistemik, risiko penyalahgunaan, serta skenario kegagalan yang perlu diwaspadai pengguna.

Prinsip utama yang mengatur isi Model Card adalah evidensi berbasis eksperimen. Setiap klaim performa atau keterbatasan harus disertai hasil uji empiris yang terdokumentasi. Dokumen ini dirancang sebagai artefak audit yang dapat diverifikasi independen, bukan sekadar lampiran administratif atau elemen dekoratif presentasi. 

Setelah struktur konten Model Card dipahami, langkah kritis berikutnya adalah menerapkannya dalam proses evaluasi yang terstandarisasi. Pada slide berikutnya, kita akan membahas workflow audit trustworthiness yang menghubungkan pengukuran atribusi, kalibrasi, robustness, dan fairness secara sistematis, lalu merangkum temuan tersebut ke dalam Model Card yang siap diaudit.

---

## Slide 028 - Trustworthiness Audit: Workflow

### Narasi

Slide ini menyajikan alur sistematis untuk melakukan *trustworthiness audit* pada model visi komputer. Pendekatan ini memastikan bahwa setiap klaim mengenai keandalan model didasarkan pada bukti empiris yang terstruktur, bukan sekadar observasi permukaan atau asumsi teoretis semata.

Proses dimulai dengan memilih model, tugas spesifik, dan dataset uji yang representatif. Setelah kerangka kerja ditetapkan, langkah selanjutnya adalah merumuskan pertanyaan audit yang tajam. Fokuskan pada dimensi yang paling kritis untuk penelitian doktor Anda, apakah itu aspek *explainability*, ketahanan terhadap gangguan (*robustness*), kalibrasi probabilitas, atau kesetaraan hasil antar kelompok. Pertanyaan ini akan menjadi kompas selama seluruh tahapan pengukuran.

Tahap eksekusi pengukuran memerlukan kombinasi metode kuantitatif dan kualitatif. Anda dapat menerapkan teknik seperti *attribution map* bersamaan dengan *occlusion test* untuk memvalidasi region visual yang benar-benar mendominasi keputusan model. Untuk kalibrasi, plot reliabilitas bersama metrik Expected Calibration Error (ECE) menjadi indikator standar. Lakukan pula pengujian *distribution shift* atau serangan adversarial, serta hitung metrik kinerja secara terpisah per sub-kelompok data jika terdapat variabel sensitif atau kondisi lingkungan yang bervariasi.

Setelah data terkumpul, lakukan analisis kegagalan secara mendalam. Kumpulkan semua kasus prediksi salah, lalu kategorikan pola kegagalannya. Identifikasi apakah kesalahan bersifat acak akibat noise sensor, atau merupakan bias sistematis terkait kondisi pencahayaan, resolusi rendah, atau distribusi domain yang berbeda. Kategorisasi ini menjadi kunci akademis untuk membedakan antara keterbatasan arsitektur jaringan dan keterbatasan kualitas data pelatihan.

Temuan audit harus diterjemahkan menjadi daftar risiko yang spesifik dan terukur. Contoh laporan yang efektif adalah "model gagal pada gambar malam hari dengan kenaikan ECE sebesar 0,15 pada citra kabur", atau "degradasi akurasi 20% saat terkena filter blur kuat". Dari temuan ini, Anda dapat menyusun rekomendasi penggunaan yang aman atau merancang iterasi perbaikan model yang tepat sasaran.

Seluruh dokumentasi audit wajib dicatat dalam *research log* dan diarsipkan di repositori kode. Praktik ini memperkuat transparansi penelitian dan secara langsung melengkapi komponen *Model Card* yang telah dibahas pada slide sebelumnya. Selain itu, struktur audit ini menjadi dasar teknis untuk menyusun daftar risiko formal yang akan kita bedah secara detail pada slide berikutnya.

---

## Slide 029 - Trustworthiness Audit: Daftar Risiko Model

### Narasi

Setelah menyelesaikan alur audit sistematis pada slide sebelumnya, kita kini beralih ke artefak kunci dari proses tersebut: Daftar Risiko Model. Dokumen ini berfungsi sebagai matriks pelaporan yang menghubungkan metode pengujian dengan temuan empiris serta implikasi risikonya. Berikut adalah rincian bagaimana setiap dimensi kepercayaan dipetakan berdasarkan tabel pada slide ini:

- **Attribution**: Menggunakan Grad-CAM dan occlusion test untuk menghasilkan peta panas. Jika skor deletion atau insertion menunjukkan ketergantungan tinggi pada tekstur lokal, risiko yang muncul adalah kegagalan generalisasi ketika bentuk objek berubah atau domain bergeser.
- **Calibration**: Memantau reliability plot dan Expected Calibration Error (ECE). Lonjakan ECE pada data out-of-distribution mengindikasikan overconfidence, yang berbahaya dalam aplikasi safety-critical karena model akan memberikan keyakinan tinggi pada prediksi yang salah.
- **Robustness**: Menguji ketahanan terhadap noise Gaussian, blur optik, dan variasi kecerahan. Penurunan akurasi yang signifikan pada kondisi sensor terdegradasi menyoroti kebutuhan augmentasi domain atau mekanisme denoising pada preprocessing.
- **Adversarial**: Mensimulasikan serangan FGSM dengan epsilon terbatas. Keruntuhan akurasi yang cepat menuntut penerapan adversarial training atau regularisasi gradien untuk meningkatkan margin decision boundary.
- **Fairness**: Mengukur disparitas performa antar sub-kelompok. Gap akurasi yang lebar biasanya berakar pada ketidakseimbangan distribusi pelatihan, sehingga memerlukan teknik reweighting loss atau stratified sampling.

Pendokumentasian daftar risiko ini memiliki nilai strategis tinggi untuk tingkat doktoral. Lampiran ini tidak hanya memperkuat argumen kontribusi penelitian, tetapi juga membuktikan bahwa Anda mampu melakukan evaluasi kritis terhadap batasan model yang diusulkan. Transparansi mengenai kelemahan awal justru menjadi fondasi yang solid untuk merancang eksperimen perbaikan dan positioning novel terhadap state-of-the-art.

Sejalan dengan fokus pada interpretabilitas, slide berikutnya akan memperkenalkan implementasi praktis melalui Python dan PyTorch. Anda akan mempelajari skrip untuk menghitung saliency map berbasis gradien input, melakukan normalisasi min-max, dan memvisualisasikan region penentu prediksi. Tugas praktikum meminta Anda membandingkan output kode ini dengan hasil occlusion test, serta mencatat sejauh mana aktivasi gradien benar-benar berkorelasi dengan region kausal yang mengubah keputusan model.

---

## Slide 030 - Praktikum 1: Menghasilkan Attribution Map

### Narasi

```python
import torch
import matplotlib.pyplot as plt

### x = tensor input berukuran (1, 3, H, W), requires_grad=True

model.eval()
x.requires_grad_(True)

out = model(x)
pred = out.argmax(dim=1).item()

### Backprop dari skor kelas prediksi atau kelas target

out[0, pred].backward()

### Saliency: max abs gradien pada kanal warna

saliency = x.grad.abs().squeeze(0).max(dim=0).values
saliency = (saliency - saliency.min()) / (saliency.max() - saliency.min())

plt.imshow(saliency.cpu().detach().numpy(), cmap="hot")
plt.axis("off")
plt.title(f"Saliency untuk kelas {pred}")
plt.show()
```

Pada slide sebelumnya, kita telah merangkum dimensi-dimensi audit kepercayaan model, termasuk atribusi sebagai indikator transparansi keputusan. Langkah praktis berikutnya adalah mengimplementasikan mekanisme atribusi berbasis gradien untuk memvisualisasikan region piksel mana yang paling dominan mendorong prediksi model. Kode di atas menyajikan protokol standar menggunakan PyTorch untuk menghasilkan *saliency map* tanpa memerlukan library tambahan selain `torch` dan `matplotlib`.

Alur komputasi dimulai dengan menyiapkan tensor input `x` berdimensi `(1, 3, H, W)` dan mengaktifkan autograd melalui `x.requires_grad_(True)`. Model dijalankan dalam mode evaluasi (`model.eval()`) untuk menonaktifkan dropout dan batch normalization yang bersifat stochastic. Forward pass menghasilkan logit, lalu indeks kelas prediksi diambil menggunakan `argmax`. Backpropagation dipicu secara eksplisit dari skor kelas prediksi via `out[0, pred].backward()`, yang mengisi buffer gradien pada tensor input.

Hasil gradien diekstrak melalui `x.grad`. Karena tensor memiliki tiga kanal warna, kode melakukan operasi `abs()` dilanjutkan dengan `.max(dim=0).values` untuk mengambil nilai gradien absolut tertinggi di sepanjang sumbu kanal pada setiap koordinat spasial. Nilai salienitas yang dihasilkan masih berupa besaran relatif, sehingga dilakukan normalisasi min-max agar skalanya terkunci antara 0 hingga 1. Visualisasi akhir menggunakan `imshow` dengan colormap `"hot"` untuk memberikan kontras tinggi pada region kritis, sekaligus memastikan tensor dipindahkan ke CPU dan detached dari graph sebelum konversi ke numpy.

Tugas praktikum ini menuntut pendekatan analitis tingkat riset. Ulangi prosedur pada beragam sampel gambar dan kelas target, lalu bandingkan peta salienitas dengan *occlusion test*. Salienitas berbasis gradien murni bersifat lokal dan sensitif terhadap noise, sehingga tidak selalu mencerminkan hubungan kausal antar piksel. Catat secara sistematis apakah region yang ditandai benar-benar mengubah prediksi ketika dihilangkan atau dikaburkan, serta identifikasi potensi bias representasi atau artefak sensor yang mungkin diadopsi model sebagai shortcut pembelajaran.

Temuan empiris dari eksperimen atribusi ini akan menjadi landasan untuk mengevaluasi konsistensi perilaku model. Setelah memahami *di mana* model fokus, langkah metodologis selanjutnya adalah mengukur *seberapa akurat* tingkat keyakinan model terhadap prediksi tersebut. Slide berikutnya akan membahas implementasi *reliability diagram* dan perhitungan Expected Calibration Error untuk mengungkap miscalibration, khususnya saat model menghadapi data dengan *distribution shift* atau domain yang berbeda.

---

## Slide 031 - Praktikum 2: Calibration Plot dan Reliability

### Narasi

Setelah pada praktikum sebelumnya kita mengeksplorasi attribution map untuk mengidentifikasi region spasial mana yang paling berkontribusi terhadap keputusan model, langkah logis berikutnya dalam kerangka trustworthy computer vision adalah mengevaluasi konsistensi antara skor kepercayaan diri model dan kebenaran prediksinya. Slide ini memperkenalkan calibration plot atau reliability diagram, sebuah alat diagnostik esensial untuk mengukur kualitas kalibrasi model klasifikasi modern.

Berikut adalah implementasi kode Python untuk menghitung dan memvisualisasikan reliability curve:

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

Fungsi `reliability_curve` bekerja dengan mengekstrak confidence score tertinggi dari vektor probabilitas tiap sampel menggunakan `np.max(proba, axis=1)`. Selanjutnya, akurasi biner dihitung dengan membandingkan indeks kelas prediksi (`argmax`) dengan label ground truth. Rentang confidence [0, 1] kemudian dibagi menjadi `n_bins` interval yang sama besar. Untuk setiap bin, kode menyaring sampel yang masuk ke interval tersebut, lalu menghitung rata-rata confidence (`means`) dan rata-rata akurasi (`accs`) secara terpisah. Hasil akhir berupa tiga array koordinat yang siap diplot.

Pada bagian visualisasi, garis diagonal putus-putus merepresentasikan kondisi kalibrasi sempurna (`Perfect`), di mana confidence sama persis dengan frekuensi akurasi empiris. Kurva model yang dihasilkan akan menunjukkan pola deviasi. Jika titik-titik kurva berada di bawah garis diagonal, model bersifat overconfident; jika di atas, model underconfident. Jarak vertikal antara kurva model dan garis ideal dapat diintegralkan untuk menghitung Expected Calibration Error (ECE), metrik ringkas yang sering dipakai dalam literatur top-tier untuk melaporkan tingkat miscalibration.

Sebagai bahan refleksi kritis pada level doktoral, perhatikan dua pertanyaan yang diajukan. Pertama, tentukan pada bin confidence mana model menunjukkan overconfidence paling ekstrem. Pola ini sering kali berakar pada ketidakseimbangan kelas selama training, saturasi softmax, atau dominasi fitur artifaktual yang tidak generalisasi. Kedua, bagaimana nilai ECE berubah ketika evaluasi dilakukan pada data yang mengalami distribution shift? Pergeseran kovariat atau konsep biasanya memperlebar gap antara confidence dan akurasi, menegaskan bahwa kalibrasi bukanlah properti statis, melainkan respons dinamik terhadap kesenjangan domain train-test. Strategi mitigasi seperti temperature scaling, Platt scaling, atau arsitektur berbasis ensemble perlu dipertimbangkan secara eksplisit dalam desain penelitian Anda.

Evaluasi kalibrasi ini akan menjadi landasan analitis sebelum kita beralih ke praktikum berikutnya, yaitu perturbation test dan analisis kegagalan melalui occlusion sensitivity, yang akan menguji stabilitas prediksi model ketika terjadi degradasi struktural pada input citra.

---

## Slide 032 - Praktikum 3: Perturbation Test dan Analisis Kegagalan

### Narasi

Pada praktikum kali ini, kita akan menguji ketahanan dan interpretabilitas model melalui *Perturbation Test*, khususnya dengan metode Occlusion Sensitivity. Pendekatan ini sangat relevan sebagai kelanjutan evaluasi kalibrasi pada slide sebelumnya, karena ketidakseimbangan antara kepercayaan diri (*confidence*) dan akurasi sering kali berakar pada region visual yang tidak dipelajari secara optimal oleh jaringan saraf.

Mari kita bedah implementasi fungsi `occlusion_test` yang tersedia. Fungsi ini menerima model dalam mode evaluasi, input tensor `x`, parameter ukuran patch, stride, serta indeks kelas target. Langkah pertama adalah menghitung skor referensi (*base score*) untuk kelas yang diminati tanpa gangguan. Selanjutnya, fungsi melakukan iterasi spasial berdasarkan grid yang dibentuk oleh patch dan stride. Pada setiap koordinat, sebagian wilayah citra di-*occlude* dengan nilai nol menggunakan operasi slicing, lalu model dijalankan ulang. Selisih antara skor referensi dan skor setelah penghalusan (`base - score`) diakumulasi ke dalam peta sensitivitas `map_occ`. Nilai tinggi pada peta ini mengindikasikan bahwa model sangat bergantung pada region tersebut untuk menghasilkan prediksi, sehingga memberikan wawasan kuantitatif sekaligus visual mengenai fokus pengambilan keputusan model.

Setelah memperoleh peta sensitivitas atau hasil uji perturbasi lainnya, langkah selanjutnya adalah melakukan analisis kegagalan secara sistematis. Ikuti empat langkah terstruktur berikut:
- Identifikasi seluruh sampel yang diprediksi salah pada himpunan data uji.
- Kelompokkan kesalahan tersebut berdasarkan pola umum, seperti citra blur, oklusi parsial, objek berukuran kecil, atau latar belakang yang mirip dengan objek target.
- Pilih tiga hingga lima contoh paling representatif dari setiap kelompok untuk didokumentasikan dalam laporan.
- Rumuskan hipotesis penyebab kegagalan secara spesifik, lalu rancang eksperimen verifikasi untuk menguji asumsi Anda.

Praktik ini bukan sekadar mencari tahu di mana model salah, melainkan membangun fondasi metodologis untuk meningkatkan keandalan sistem. Hasil analisis kegagalan akan menjadi bahan kritis ketika kita beralih ke tahap berikutnya, yaitu membaca dan mengaudit literatur computer vision dengan perspektif *trustworthiness*. Kita akan melihat bagaimana peneliti lain melaporkan metrik explainability, robustness, dan fairnes, serta bagaimana kita dapat menerapkan kerangka audit yang sama terhadap karya ilmiah terkini.

---

## Slide 033 - Membaca dan Mengaudit Paper dengan Kacamata Trustworthiness

### Narasi

Pada slide ini, kita beralih dari uji empiris yang telah kalian kerjakan pada praktikum sebelumnya menuju tahap evaluasi kritis terhadap literatur ilmiah. Setelah melakukan perturbation test dan memetakan area sensitivitas model melalui occlusion sensitivity, langkah logis berikutnya adalah menerapkan lensa trustworthiness saat membaca paper computer vision. Di jenjang doktor, kemampuan membedah metodologi penelitian orang lain secara sistematis menjadi fondasi utama untuk mengidentifikasi research gap dan merumuskan kontribusi ilmiah yang orisinal.

Tabel audit pada slide ini menyoroti lima dimensi krusial yang wajib diuji dalam setiap publikasi berkualitas. Untuk explainability, pastikan attribution tidak hanya disajikan secara visual, tetapi juga divalidasi menggunakan faithfulness metric seperti I-SHAPE atau deletion/insertion score. Aspek calibration menuntut pelaporan uncertainty estimate atau Expected Calibration Error alongside accuracy, karena prediksi akurat tanpa kepastian yang terkalibrasi sangat rentan gagal pada skenario dunia nyata. Robustness harus dibuktikan melalui pengujian distribution shift atau serangan adversarial, bukan hanya performa pada data training-test split konvensional. Fairness memerlukan breakdown metrik per sub-kelompok atau kategori objek, sedangkan reproducibility mensyaratkan ketersediaan seed acak, konfigurasi lingkungan, dan repositori kode yang dapat dieksekusi ulang.

Aktivitas seminar paper dirancang sebagai latihan terstruktur untuk mengasah ketelitian metodologis kalian. Setiap mahasiswa diminta memilih satu paper computer vision terkini, kemudian melakukan audit kecil berdasarkan lima pertanyaan tersebut. Presentasi tidak bertujuan menghakimi karya peneliti lain, melainkan melatih disiplin akademik dalam mengidentifikasi kekuatan bukti empiris, kelemahan desain evaluasi, dan merumuskan saran perbaikan yang berbasis literatur. Pendekatan ini akan membiasakan kalian membaca paper secara aktif, kritis, dan konstruktif.

Temuan dari audit ini akan langsung menjadi bahan baku untuk perencanaan eksperimen pada pertemuan berikutnya. Daftar risiko model yang teridentifikasi, bersama dengan metrik non-akurasi seperti degradation score atau fairness gap, akan menentukan strategi pemilihan baseline dan rancangan ablation study. Jika audit mengungkap kecenderungan model menjadi overconfident pada out-of-distribution data, maka protokol eksperimen di pertemuan 12 harus mencakup variasi seed untuk estimasi interval kepercayaan, perbandingan statistik dengan metode kalibrasi standar, serta dokumentasi ketat terhadap karakteristik dataset OOD yang digunakan. Dengan demikian, membaca paper dengan kacamata trustworthiness bukan sekadar tugas teoritis, melainkan langkah strategis menyusun experimental design yang valid, transparan, dan siap diuji secara rigor.

---

## Slide 034 - Kaitan dengan Pertemuan 12: Experimental Design

### Narasi

Slide ini berfungsi sebagai jembatan metodologis antara audit trustworthiness yang telah kita bahas pada slide 33 dengan persiapan teknis untuk pertemuan berikutnya. Pada slide 33, kita telah menyusun kerangka pertanyaan kritis dan metrik evaluasi untuk menguji aspek explainability, calibration, robustness, fairness, dan reproducibility dalam literatur computer vision. Langkah natural berikutnya adalah menerjemahkan pertanyaan audit tersebut menjadi protokol eksperimen yang ketat, reproducible, dan siap diujikan secara empiris.

Pertemuan 12 akan fokus pada perancangan eksperimental yang valid. Fokus bergeser dari identifikasi celah evaluasi ke implementasi desain penelitian yang mampu menjawab pertanyaan audit tadi dengan bukti kuantitatif yang kuat. Hal ini mencakup penentuan baseline yang relevan, strategi ablation study yang terarah, serta pengendalian variabel confounding agar klaim kinerja model benar-benak mencerminkan atribut algoritma, bukan artefak konfigurasi atau kebocoran data.

Beberapa elemen fundamental yang perlu Anda bawa dan persiapkan untuk sesi eksperimen meliputi:
- Daftar risiko model yang telah diidentifikasi, yang menjadi dasar objektif dalam memilih baseline dan menentukan titik ablation.
- Penguasaan metrik non-akurasi seperti Expected Calibration Error (ECE), degradation score, fairness gap, dan deletion score, karena metrik inilah yang mengukur dimensi trustworthiness yang sering terabaikan dalam pelaporan konvensional.
- Dokumentasi pola kegagalan model untuk merancang error analysis yang sistematis, bukan sekadar observasi deskriptif.
- Penerapan kontrol eksperimen ketat: konsistensi random seed, transparansi train-validation-test split, dan pencatatan kondisi lingkungan komputasi agar perbandingan antar metode bersifat adil dan dapat direplikasi.

Sebagai contoh praktis, jika analisis pada slide sebelumnya mengindikasikan bahwa model cenderung overconfident ketika menghadapi Out-of-Distribution (OOD) data, maka desain eksperimen di pertemuan 12 harus diarahkan untuk menguji hipotesis tersebut secara statistik. Protokol yang disarankan mencakup pengulangan eksperimen dengan variasi seed untuk menghitung interval kepercayaan, perbandingan baseline kalibrasi menggunakan uji signifikansi parametrik atau non-parametrik, serta penggunaan dataset OOD yang telah didokumentasikan secara rinci mengenai karakteristik domain shift-nya. Pendekatan ini memastikan bahwa temuan Anda memiliki bobot empiris yang dapat dipertanggungjawabkan di tingkat doktoral.

Transisi ini mempersiapkan Anda langsung menuju peluang kontribusi riset yang akan diuraikan pada slide berikutnya. Dengan fondasi pertanyaan audit yang tajam dan protokol eksperimen yang solid, Anda dapat memetakan area di mana metode state-of-the-art masih belum memadai, sehingga membuka ruang untuk inovasi berupa metrik evaluasi baru, benchmark auditing, analisis teoretis keterbatasan arsitektur, atau protokol trustworthiness yang dikustomisasi untuk domain spesifik.

---

## Slide 035 - Peluang Kontribusi Penelitian Doktoral

### Narasi

Slide ini mengalihkan fokus dari evaluasi teknis menuju peluang kontribusi ilmiah tingkat doktoral. Setelah pada pertemuan sebelumnya kita mengidentifikasi risiko model, menyusun daftar metrik non-akurasi seperti ECE, degradation score, fairness gap, dan deletion score, serta mencatat pengalaman kegagalan yang perlu dikontrol, kini saatnya merangkai ide riset yang memiliki novelty dan dampak akademis nyata.

Tabel pada slide ini menyoroti lima area terbuka yang sangat relevan dengan perkembangan mutakhir di bidang *trustworthy computer vision*. Pertama, pengembangan *faithfulness metric* baru untuk memastikan bahwa attribution map benar-benar mencerminkan sebab prediksi, bukan sekadar korelasi visual. Kedua, metode kalibrasi yang stabil di bawah *distribution shift*, mengingat banyak model gagal mempertahankan keandalan probabilitas ketika data uji menyimpang dari distribusi pelatihan. Ketiga, studi sistematis mengenai perilaku Vision Transformer terhadap serangan adversarial, yang masih menjadi celah penelitian karena arsitektur berbasis attention memiliki dinamika kerentanan yang berbeda dari arsitektur konvolusional tradisional. Keempat, audit bias pada segmentasi mask untuk domain kritis seperti medis atau perkotaan, di mana kesalahan alokasi wilayah dapat berdampak langsung pada keputusan klinis atau kebijakan publik. Kelima, otomatisasi *model card* melalui analisis kegagalan terstruktur, sehingga dokumentasi risiko model tidak lagi bersifat manual dan subjektif.

Penting untuk dipahami bahwa kontribusi disertasi tidak harus selalu berupa arsitektur jaringan baru. Pada tingkat doktoral, inovasi bisa hadir dalam bentuk metrik evaluasi yang lebih rigor, dataset atau benchmark khusus untuk auditing, analisis teoretis yang mengungkap keterbatasan fundamental suatu metode, maupun protokol *trustworthiness* yang disesuaikan dengan karakteristik domain tertentu. Pendekatan ini sejalan dengan tren literatur terbaru yang menekankan validasi empiris dan teoretis di atas klaim performa mentah.

Temuan dari eksplorasi ide riset ini akan langsung berkaitan dengan poin-poin kunci yang akan kita ringkas pada slide berikutnya. Ingat bahwa akurasi tinggi tidak sama dengan model yang dapat dipercaya, sehingga setiap metrik yang kita usulkan harus melewati uji *faithfulness*, kalibrasi, dan evaluasi *robustness* yang ketat. Hasil audit model yang Anda kerjakan pada praktikum akan menjadi bahan utama untuk merancang eksperimen yang terkontrol pada pertemuan berikutnya, di mana kita akan membahas protokol desain eksperimental, pemilihan baseline, dan strategi ablation study secara mendalam.

---

## Slide 036 - Ringkasan dan Takeaway

### Narasi

Mari kita tinjau kembali inti dari pertemuan ini melalui enam take-away kunci:

1. **Akurasi tinggi tidak sama dengan model yang dapat dipercaya.** Performa numerik pada dataset uji statis sering kali menutupi kerentanan sistem terhadap distribusi data yang berubah atau input yang dimanipulasi.
2. **Attribution map harus diuji faithfulness-nya**, bukan hanya dilihat visualnya. Peta atribusi perlu divalidasi secara kuantitatif untuk memastikan region yang disorot benar-benar berkontribusi kausal terhadap prediksi model, bukan sekadar korelasi semu.
3. **Confidence perlu divalidasi melalui kalibrasi.** Skor kepercayaan yang overconfident tanpa validasi probabilistik dapat menyesatkan pengambilan keputusan kritis dalam aplikasi nyata.
4. **Distribution shift dan adversarial attack adalah bagian wajib** dalam evaluasi robustness. Tanpa pengujian kedua aspek ini, klaim generalisasi model tetap bersifat teoretis dan belum siap untuk deployment.
5. **Fairness harus diukur per kelompok**, bukan hanya rata-rata global. Audit bias harus dilakukan secara granular, terutama pada domain sensitif seperti segmentasi medis atau deteksi objek perkotaan yang rentan terhadap skew data.
6. **Model card membantu mengkomunikasikan risiko secara ilmiah.** Dokumen ini menjadi standar transparansi untuk mendokumentasikan batasan, asumsi, serta potensi dampak negatif sebelum model diintegrasikan ke dalam pipeline produksi.

Sebagai aksi setelah pertemuan, selesaikan praktikum yang mencakup pembuatan attribution map, plotting calibration plot, dan perturbation test. Pilih satu model arsitektur yang telah dipelajari, lalu lakukan trustworthiness audit menyeluruh berdasarkan keenam prinsip di atas. Hasil audit ini akan menjadi bahan diskusi utama pada pertemuan berikutnya, di mana kita akan bersama-sama merancang eksperimen yang ketat dan reproducible.

Rangkuman ini menjadi jembatan langsung menuju materi pertemuan selanjutnya, yaitu Experimental Design dan Reproducible Benchmarking. Fokus kita kini bergeser dari identifikasi kelemahan model ke konstruksi protokol evaluasi yang rigor, sehingga kontribusi penelitian doktoral yang Anda kembangkan nanti memiliki fondasi metodologis yang solid, terstandarisasi, dan siap diverifikasi oleh komunitas ilmiah internasional.

---

## Slide 037 - Penutup

### Narasi

Kita telah menyelesaikan rangkaian pembahasan mengenai Explainable, Robust, dan Trustworthy Computer Vision. Pada jenjang doktoral, ketiga aspek ini bukan lagi fitur pelengkap, melainkan fondasi metodologis yang menentukan apakah sebuah model layak dianggap sebagai kontribusi ilmiah yang valid dan siap diadopsi ke dalam sistem kritis.

Seperti yang telah dirangkum pada slide sebelumnya, akurasi tinggi tidak otomatis menjamin kepercayaan. Validasi atribusi harus melewati uji faithfulness, confidence score memerlukan kalibrasi probabilistik, dan evaluasi robustness wajib mencakup stress test terhadap distribution shift serta adversarial perturbation. Penerapan fairness metric per subgroup, serta dokumentasi batasan dan risiko menggunakan model card, akan memperkuat posisi paper Anda di tengah literatur computer vision terkini.

Pertemuan berikutnya akan mengalihkan fokus ke tahap eksekusi penelitian yang lebih ketat, yaitu Experimental Design dan Reproducible Benchmarking. Sesi tersebut akan membahas kerangka kerja untuk merancang ablation study yang terkontrol, menetapkan baseline yang kompetitif, serta menerapkan protokol pelaporan hasil yang memenuhi standar open science. Pendekatan ini diperlukan agar temuan riset Anda dapat direplikasi, dibandingkan secara adil, dan dikembangkan lebih lanjut oleh komunitas akademik global.

Sebelum transisi ke topik tersebut, pastikan Anda telah menyelesaikan tiga komponen praktikum inti:
- Visualisasi attribution map beserta metrik faithfulness-nya.
- Calibration plot untuk mengukur kesesuaian antara confidence score dan akurasi aktual.
- Hasil perturbation test yang mendemonstrasikan degradasi performa di bawah noise atau adversarial input.

Siapkan laporan singkat audit trustworthiness pada satu model pilihan Anda. Data empiris dari audit ini akan menjadi bahan diskusi utama saat kita menyusun experimental matrix dan strategi benchmarking yang rigor. Terima kasih atas partisipasi dan kedalaman analisis selama pertemuan ini.
