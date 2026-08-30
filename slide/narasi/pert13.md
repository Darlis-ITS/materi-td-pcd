# Narasi TD Pengolahan Citra Digital - Pertemuan 13

## Formulasi Research Question, Hipotesis, dan Novelty

Sumber: markdown/pert13-formulasi-research-question-hipotesis-dan-novelty.md

---

## Slide 000 - Cover

### Narasi

Slide ini membuka diskusi inti untuk pertemuan ke-13. Pada jenjang doktoral, fokus perkuliahan telah beralih sepenuhnya dari pemahaman teknis algoritma menuju konstruksi penelitian yang rigor, terukur, dan berkontribusi pada state-of-the-art. Topik hari ini, Formulasi Research Question, Hipotesis, dan Novelty, menjadi fondasi struktural sebelum Anda merancang metodologi disertasi.

Merumuskan research question yang tepat bukan sekadar memilih topik yang sedang tren, melainkan mengidentifikasi celah pengetahuan yang signifikan dan dapat diuji. Pertanyaan penelitian harus bersifat spesifik, terarah, dan mampu mengendalikan seluruh pipeline eksperimen, mulai dari strategi augmentasi data, pemilihan arsitektur representasi visual, hingga definisi metrik evaluasi yang relevan dengan domain aplikasi.

Hipotesis berfungsi sebagai pernyataan awal yang dapat diverifikasi secara empiris. Dalam konteks pengolahan citra modern, hipotesis biasanya menghubungkan intervensi metodologis dengan perubahan performa model. Misalnya, apakah integrasi mekanisme attention cross-modal pada CLIP architecture secara signifikan meningkatkan robustness terhadap domain shift dibandingkan baseline supervised learning? Klaim ini harus dirancang agar dapat diuji melalui benchmarking yang reproducible dan analisis statistik yang ketat.

Novelty menuntut positioning yang jelas terhadap literatur terkini. Mahasiswa dituntut melakukan critical paper review yang sistematis untuk menemukan research gap yang belum dieksplorasi oleh pendekatan mutakhir seperti Vision Transformer, self-supervised learning, foundation models, atau generative diffusion architectures. Kontribusi ilmiah yang diajukan harus menjawab pertanyaan mendasar: apa yang baru, mengapa penting, dan bagaimana hal tersebut memperluas batas pengetahuan di bidang computer vision.

Untuk melihat bagaimana ketiga elemen ini menyatu dalam alur akademik Anda, mari kita tinjau peta besar penyusunan proposal. Slide berikutnya akan memaparkan posisi pertemuan ke-13 dalam roadmap RPS, menunjukkan bagaimana sesi ini menjadi penghubung kritis antara fase eksplorasi riset, desain eksperimen, dan persiapan seminar proposal disertasi.

---

## Slide 001 - Posisi Pertemuan 13 dalam RPS

### Narasi

Slide ini menempatkan Pertemuan 13 dalam peta besar perjalanan akademik menuju penyusunan proposal disertasi. Berdasarkan tabel yang ditampilkan, mata kuliah ini dirancang secara bertahap. Dua pertemuan awal difokuskan pada eksplorasi dan pemetaan riset melalui critical paper reading untuk mengidentifikasi research gap. Selanjutnya, pertemuan 3 hingga 11 membangun fondasi teknis dengan mendalami representasi visual, foundation models, generative vision, hingga trustworthy computer vision. Pertemuan 12 membahas desain eksperimen dan reproducible benchmarking. Pada rentang pertemuan 13 dan 14, kita memasuki fase krusial yaitu formulasi proposal, di mana mahasiswa dituntut untuk merumuskan research question, hipotesis, novelty, serta metodologi disertasi secara terstruktur. Tahap penutup akan diisi oleh seminar proposal, evaluasi state-of-the-art, dan rencana publikasi.

Pertemuan 13 merupakan titik balik akademis yang signifikan. Fokus diskusi bergeser dari kebiasaan memahami atau mengimplementasikan metode yang sudah ada, menuju kemampuan menyusun klaim penelitian yang tajam, orisinal, dan dapat diuji secara empiris. Transisi ini sangat menentukan kualitas kontribusi ilmiah yang akan dihasilkan pada jenjang doktor.

Aktivitas inti pada sesi ini mencakup research clinic individual, diskusi mendalam mengenai draft proposal, serta mekanisme peer feedback. Mahasiswa didorong untuk saling meninjau dan memberikan masukan kritis terhadap kerangka penelitian masing-masing. Target keluaran yang diharapkan adalah tersusunnya concept note penelitian yang utuh. Dokumen ini harus mencakup elemen-elemen berikut:
- Definisi masalah yang jelas dan kontekstual.
- Identifikasi research gap yang spesifik dan terdokumentasi.
- Perumusan research question yang terukur dan dapat diuji.
- Hipotesis yang dapat diverifikasi secara empiris.
- Pernyataan novelty yang kuat dan berbeda dari pendekatan existing.
- Estimasi kontribusi ilmiah yang relevan dengan state-of-the-art terkini.

Pemahaman posisi strategis pertemuan ini akan menjadi landasan ketika kita meninjau kembali materi eksperimen dari pertemuan sebelumnya. Desain eksperimen yang telah dipelajari tidak berdiri sendiri, melainkan berfungsi sebagai alat verifikasi bagi pertanyaan penelitian yang akan kita bangun. Mari kita lanjutkan ke penjelasan lebih lanjut mengenai bagaimana hasil Pertemuan 12 menjadi jembatan konseptual untuk memastikan empat hal mendasar: gap yang hendak diisi, alasan pentingnya gap tersebut, bukti yang akan mengonfirmasi hipotesis, serta manfaat ilmiah yang diperoleh.

---

## Slide 002 - Recap Pertemuan 12 dan Jembatan ke Pertemuan 13

### Narasi

Pada pertemuan sebelumnya, kita telah menyusun protokol eksperimen yang komprehensif. Hal ini mencakup pembagian dataset, pemilihan baseline, rancangan ablation study, pengaturan random seed, penentuan hyperparameter, pemilihan metrik evaluasi, serta standar pelaporan hasil. Prinsip utamanya adalah reproduktibilitas dan keadilan perbandingan. Desain eksperimen yang baik bukan sekadar langkah teknis, melainkan fondasi verifikasi empiris untuk setiap klaim penelitian yang akan kita ajukan di tingkat doktoral.

Namun, protokol eksperimen yang sangat rapi tidak akan bermakna jika pertanyaan penelitiannya tidak tajam. Pertemuan ini menjadi jembatan kritis antara desain teknis dan formulasi intelektual. Kita akan memastikan empat hal mendasar:
- Identifikasi research gap yang hendak diisi secara eksplisit.
- Justifikasi mengapa gap tersebut penting secara ilmiah dan praktis.
- Jenis bukti atau data yang akan digunakan untuk mengonfirmasi hipotesis.
- Manfaat akademik yang diperoleh dari pengisian gap tersebut.

Tanpa kejelasan pada keempat poin ini, bahkan benchmarking paling ketat sekalipun akan kehilangan arah. Sesuai dengan peta kurikulum, pertemuan ini menandai pergeseran fase dari kebiasaan memahami metode menjadi kemampuan membangun klaim penelitian yang dapat diuji. Hasil perumusan pada sesi ini akan langsung diterjemahkan menjadi metodologi disertasi dan rancangan eksperimen yang lebih konkret pada pertemuan berikutnya.

Untuk mencapai target keluaran ini, capaian pembelajaran mata kuliah menuntut kita merumuskan research problem, hipotesis, novelty, dan positioning terhadap state-of-the-art secara sistematis. Anda akan dilatih mengubah topik umum menjadi masalah penelitian yang spesifik dan terukur, menyusun hubungan logis antar komponen penelitian, serta menulis contribution statement yang membedakan karya Anda dari literatur yang sudah ada. Output utama yang ditargetkan adalah concept note awal beserta peta literatur dan tabel perbandingan metode terkini. Mari kita mulai membedah struktur formulasi pertanyaan penelitian yang solid.

---

## Slide 003 - Tujuan Pembelajaran dan Capaian Terkait

### Narasi

Capaian Pembelajaran Mata Kuliah pada jenjang doktor menekankan dua kompetensi inti yang menjadi fondasi utama pertemuan ini: CPMK-5 dan CPMK-6. CPMK-5 menuntut kemampuan merumuskan research problem, research question, hipotesis, research gap, novelty, serta positioning terhadap state-of-the-art secara sistematis dan kritis. CPMK-6 mengarahkan mahasiswa pada penyusunan proposal awal disertasi yang mencakup kontribusi ilmiah yang terukur, metodologi yang valid, experimental design yang robust, identifikasi risiko penelitian, serta rencana diseminasi hasil kepada komunitas akademik.

Tujuan spesifik pertemuan ke-13 dirancang untuk menerjemahkan kompetensi tersebut ke dalam praktik penelitian tingkat doktor:
- Mengubah topik umum menjadi masalah penelitian yang spesifik, terukur, dan dapat diuji secara empiris.
- Menyusun hubungan logis yang ketat antara research gap, research question, hipotesis, metode yang dipilih, hingga klaim kontribusi ilmiah.
- Menulis contribution statement yang jelas, sehingga perbedaan antara novelty karya Anda dengan publikasi yang telah ada dapat dibedakan secara eksplisit.
- Menyusun concept note awal yang dilengkapi dengan peta literatur dan tabel perbandingan state-of-the-art sebagai dasar evaluasi kelayakan penelitian.

Poin-poin ini merupakan kelanjutan langsung dari pembahasan sebelumnya mengenai desain eksperimen. Protokol eksperimen yang rapi tidak akan menghasilkan temuan yang bermakna jika pertanyaan penelitiannya tidak tajam. Pertemuan ini memastikan empat hal mendasar yang harus terpenuhi sebelum masuk ke tahap implementasi: gap yang hendak diisi, alasan pentingnya gap tersebut, bukti yang akan mengonfirmasi hipotesis, serta manfaat ilmiah yang diperoleh. Hasil perumusan ini akan menjadi bahan mentah yang kemudian diterjemahkan secara operasional pada pertemuan berikutnya menjadi metodologi disertasi dan rancangan eksperimen yang konkret.

Untuk mengevaluasi ketajaman formulasi tersebut, mahasiswa harus siap menjawab empat pertanyaan kunci yang akan dibahas pada slide berikutnya. Setiap pertanyaan memiliki target evaluasi yang spesifik, mulai dari penentuan positioning gap, justifikasi signifikansi masalah, kesiapan desain eksperimen untuk pengujian hipotesis, hingga identifikasi penerima manfaat ilmiah. Jika keempat pertanyaan ini belum terjawab dengan solid, concept note berisiko hanya menjadi daftar pustaka tanpa alur penelitian yang koheren, reviewer akan kesulitan menilai kebaruan dan kelayakan, dan eksperimen lanjutan akan kehilangan landasan metodologis yang kuat.

---

## Slide 004 - Pertanyaan Kunci Pertemuan Ini

### Narasi

Slide ini menyajikan empat pertanyaan kunci yang wajib dijawab secara eksplisit oleh setiap mahasiswa doktoral sebelum memasuki tahap penyusunan metodologi dan eksperimen. Pertanyaan-pertanyaan ini berfungsi sebagai filter kritis untuk memastikan bahwa riset yang Anda bangun memiliki struktur logis, signifikansi jelas, dan dapat diuji secara empiris.

Berikut adalah penjabaran masing-masing pertanyaan beserta implikasinya dalam konteks penelitian pengolahan citra digital tingkat lanjut:

- **Gap apa yang hendak diisi?** Jawaban ini menentukan posisi riset Anda terhadap state-of-the-art. Hindari pernyataan umum seperti "metode konvensional sudah usang". Tunjukkan secara spesifik batasan arsitektur, asumsi data, atau kondisi inferensi yang belum ditangani optimal oleh karya terkini.
- **Mengapa gap tersebut penting?** Bagian ini membentuk problem statement dan signifikansi penelitian. Kaitkan gap dengan dampak nyata, apakah berupa peningkatan akurasi diagnostik pada citra medis, efisiensi inference untuk perangkat edge, atau peningkatan robustness pada domain shift visual.
- **Bukti apa yang akan mengonfirmasi hipotesis?** Hipotesis riset S3 harus bersifat falsifiable dan terukur. Rancang metrik evaluasi yang relevan, tentukan baseline pembanding, dan pastikan desain eksperimen mampu memisahkan pengaruh variabel independen terhadap performa model secara statistik.
- **Siapa yang memperoleh manfaat ilmiah?** Klaim kontribusi harus terarah. Identifikasi secara eksplisit apakah kontribusi Anda bernilai teoretis bagi komunitas akademik, praktis bagi industri, atau aplikatif bagi bidang spesifik seperti computer vision otonom atau analisis patologi digital.

Ketika keempat pertanyaan ini belum terjawab dengan matang, konsep naskah penelitian berisiko berubah menjadi sekadar rangkuman literatur tanpa alur argumentasi yang koheren. Reviewer internasional akan kesulitan menilai novelty dan kelayakan teknis proposal Anda. Lebih kritis lagi, eksperimen yang akan kita kerjakan pada pertemuan berikutnya tidak akan memiliki landasan metodologis yang kuat, sehingga interpretasi hasil pengujian menjadi rentan terhadap bias dan overclaiming.

Penjelasan ini merupakan tindak lanjut langsung dari capaian pembelajaran pada slide sebelumnya, di mana kita telah menetapkan target untuk mengubah topik umum menjadi masalah penelitian yang spesifik, terukur, dan dapat diuji. Setelah kita memastikan keempat pertanyaan kunci ini terpenuhi, langkah selanjutnya adalah mempersempit ruang lingkup dari area minat hingga menghasilkan formulasi research question, hipotesis, dan klaim kontribusi yang presisi. Proses penyempitan tersebut akan kita bedah secara sistematis pada slide berikutnya.

---

## Slide 005 - Dari Topik Umum Menjadi Masalah Penelitian

### Narasi

Pada slide ini, kita membahas proses sistematis untuk menyempitkan area penelitian yang luas menjadi masalah penelitian yang spesifik, terukur, dan siap diuji. Alur yang ditampilkan menggambarkan bagaimana sebuah ide awal harus melalui serangkaian tahap filtrasi akademik sebelum akhirnya menghasilkan hipotesis dan klaim novelty yang valid.

Proses dimulai dari topik umum, misalnya "image restoration". Pada jenjang doktoral, topik semacam ini masih bersifat deskriptif dan terlalu lebar untuk langsung dijadikan landasan eksperimen. Langkah kritis berikutnya adalah melakukan tinjauan literatur yang komprehensif untuk menemukan celah pengetahuan atau *research gap*. Celah inilah yang berfungsi sebagai poros penghubung antara ranah disiplin ilmu dengan pertanyaan penelitian yang terfokus.

Setelah *gap* teridentifikasi secara empiris, kita merumuskan *research question* yang presisi. Pertanyaan tersebut kemudian dikonversi menjadi hipotesis kerja, yang pada akhirnya melahirkan klaim novelty dan pernyataan kontribusi ilmiah. Perhatikan contoh kontras pada slide: topik umum "image restoration" disempitkan menjadi masalah penelitian yang sangat spesifik, yaitu "bagaimana mengurangi detail halusinatif pada restorasi citra medis berdosis rendah menggunakan diffusion model?". Perbedaannya fundamental. Topik hanya menandai bidang studi, sedangkan masalah penelitian menyajikan pertanyaan yang dapat dijawab secara metodologis melalui desain eksperimen yang terstruktur.

Penyempitan ini bukan sekadar formalitas penulisan, melainkan fondasi logika penelitian Anda. Jika alur ini dilewati, seperti yang telah diingatkan pada slide sebelumnya mengenai risiko konsep penelitian yang berubah menjadi sekadar daftar pustaka, maka seluruh eksperimentasi di pertemuan-pertemuan lanjutan akan kehilangan pijakan yang kuat dan sulit dinilai kebaruan maupun kelayakannya oleh reviewer.

Memahami mekanisme penyempiran ini akan menjadi prasyarat utama ketika kita membedah anatomi formulasi riset pada slide berikutnya. Setiap komponen dalam kerangka penelitian saling bergantung secara hierarkis, dan konsistensi antar-komponen akan menentukan kekuatan argumen ilmiah Anda secara keseluruhan.

---

## Slide 006 - Anatomi Formulasi Riset

### Narasi

Setelah pada slide sebelumnya kita menelusuri alur penyempitan dari topik umum hingga menemukan celah penelitian, materi ini menyajikan kerangka struktural yang mengikat seluruh elemen tersebut menjadi satu kesatuan logis. Kita akan membedah anatomi formulasi riset, yaitu enam komponen utama yang saling terhubung dan harus dirancang secara koheren dalam setiap proposal disertasi tingkat doktoral.

Setiap komponen dalam tabel ini menjawab pertanyaan mendasar yang berbeda, namun memiliki peran strategis dalam membangun argumen ilmiah Anda:
- *Problem statement*: memberi alasan dan urgensi mengapa penelitian ini mendesak untuk dilakukan.
- *Research gap*: menentukan batas pengetahuan saat ini dan celah literatur yang belum tersentuh.
- *Research question*: mengarahkan cakupan penelitian agar tetap fokus dan terukur.
- *Hipotesis*: memberikan prediksi awal yang dapat diuji secara empiris melalui eksperimen.
- *Novelty*: menegaskan nilai tambah metodologis atau teoretis yang membedakan karya Anda dari state-of-the-art.
- *Contribution statement*: menegaskan dampak akhir dan sumbangan nyata terhadap perkembangan ilmu pengetahuan.

Konsistensi antar komponen ini adalah syarat mutlak validitas ilmiah. Perubahan pada definisi research gap akan langsung berdampak pada perumusan research question dan arah pengujian hipotesis. Dissonansi logis antar elemen ini sering menjadi kelemahan fatal dalam review proposal disertasi. Pada pertemuan ini, kita akan mendalami keenam pilar tersebut secara bertahap, dengan penekanan khusus pada perumusan pertanyaan yang tajam dan pengujian asumsi yang rigor. Pembahasan teknis mengenai desain eksperimen dan pemilihan arsitektur model akan kita lanjutkan pada pertemuan berikutnya.

Untuk memulai bedah komponen ini, mari kita buka pembahasan lebih mendalam pada elemen pertama. Di slide selanjutnya, kita akan mengurai kriteria keabsahan *problem statement*, mengidentifikasi kesalahan umum dalam perumusan masalah, dan memastikan bahwa setiap klaim ketidakcukupan metode yang ada didukung oleh konteks domain serta konsekuensi nyata yang terukur.

---

## Slide 007 - Problem Statement

### Narasi

Pada slide ini, kita masuk ke komponen pertama dari anatomi formulasi riset yang telah dipetakan pada slide sebelumnya, yaitu *problem statement*. Dalam penelitian tingkat doktoral, pernyataan masalah bukan sekadar deskripsi teknis atau keluhan terhadap performa model. Ia merupakan narasi ilmiah yang merumuskan kebutuhan nyata atau kesenjangan pengetahuan dalam bidang pengolahan citra digital, dilengkapi dengan konteks empiris serta konsekuensi yang timbul jika masalah tersebut tidak ditangani. Konsistensi antar komponen sangat krusial: perubahan pada *problem statement* akan berdampak langsung pada perumusan *research question*, hipotesis, hingga desain eksperimen.

Sebuah *problem statement* yang layak diajukan dalam proposal disertasi harus memenuhi empat kriteria berikut:
- Spesifik: menyebutkan objek, kondisi pengujian, atau domain aplikasi secara eksplisit, sehingga cakupan penelitian tidak melebar tanpa arah.
- Berbasis kebutuhan: terdapat justifikasi yang jelas mengapa masalah tersebut mendesak untuk dipecahkan, baik dari sisi kemajuan ilmu pengetahuan maupun aplikasi praktis.
- Menyebut kesenjangan: membandingkan secara kritis apa yang sudah terjawab oleh literatur terkini versus apa yang masih belum tuntas atau belum tervalidasi.
- Menyebut konsekuensi: menguraikan dampak negatif, baik secara metodologis, klinis, industri, atau kebijakan, apabila celah tersebut dibiarkan berlanjut.

Dalam praktiknya, penulisan *problem statement* sering kali terjebak pada tiga kesalahan umum yang melemahkan argumen penelitian:
- Klaim "akurasi model masih rendah" tanpa menjelaskan pada distribusi data, kondisi degradasi, atau skenario deployment tertentu kegagalan tersebut terjadi.
- Pernyataan "belum banyak penelitian" yang bersifat generik, tanpa menunjukkan secara analitis mengapa studi-studi terdahulu tidak memadai atau tidak relevan dengan konteks baru.
- Pengajuan "dataset terbatas" sebagai masalah inti, padahal seharusnya dikaitkan langsung dengan dampaknya terhadap generalisasi model, bias evaluasi, atau validitas eksternal temuan.

Dengan memahami batasan dan standar kualitas ini, kita dapat menghindari narasi yang dangkal dan beralih ke konstruksi kalimat yang presisi. Pada slide berikutnya, kita akan mempelajari kerangka penulisan *problem statement* yang kuat melalui template terstruktur, diikuti oleh contoh konkret dari domain restorasi citra medis dosis rendah dan segmentasi citra satelit resolusi tinggi, serta sesi latihan untuk menerapkannya langsung pada topik penelitian Anda.

---

## Slide 008 - Menulis Problem Statement yang Kuat

### Narasi

Pada slide ini, kita akan membahas cara menyusun *problem statement* yang kuat dan terstruktur. Setelah memahami definisi dan kriteria *problem statement* pada slide sebelumnya, langkah selanjutnya adalah menerapkannya dalam bentuk kalimat yang presisi dan akademis.

Template sederhana berikut dapat menjadi panduan awal penulisan: "Meskipun metode X telah mencapai hasil yang baik pada kondisi Y, metode tersebut masih memiliki keterbatasan pada kondisi Z, sehingga [dampak konkret]." Struktur ini memaksa peneliti untuk secara eksplisit menyebutkan keunggulan pendekatan yang sudah ada, mengidentifikasi batasannya secara spesifik, dan menghubungkan keterbatasan tersebut dengan konsekuensi nyata atau ilmiahnya.

Berikut adalah dua contoh penerapan template tersebut dalam konteks pengolahan citra digital mutakhir:
- "Meskipun *diffusion model* mampu menghasilkan citra restorasi berkualitas tinggi, metode tersebut belum stabil pada citra CT dosis rendah dengan degradasi campuran, sehingga dapat menghasilkan detail halusinatif yang menyesatkan diagnosis."
- "*Foundation model* segmentasi menunjukkan generalisasi tinggi pada citra natural, tetapi belum tervalidasi pada citra satelit resolusi tinggi dengan objek kecil, sehingga penerapannya pada pemantauan lingkungan masih berisiko."

Kedua contoh tersebut menunjukkan pola yang sama: pengakuan terhadap kemajuan terkini, identifikasi celah spesifik berdasarkan kondisi data atau tugas, serta penyebutan dampak operasional atau ilmiah jika celah tersebut tidak ditangani. Hindari klaim umum seperti "metode lebih baik" tanpa menjelaskan mekanisme perbaikan atau konteks kondisi di mana keunggulan itu berlaku.

Untuk latihan, silakan susun satu paragraf *problem statement* yang relevan dengan topik penelitian Anda. Pastikan setiap klaim didukung oleh kondisi teknis atau empiris yang jelas. Paragraf ini akan menjadi fondasi bagi perumusan *research question* dan hipotesis pada tahap berikutnya, sekaligus menjembatani materi *problem statement* dengan pembahasan *research gap* pada slide selanjutnya.

---

## Slide 009 - Research Gap

### Narasi

Setelah merumuskan problem statement yang kuat pada slide sebelumnya, langkah selanjutnya adalah mengidentifikasi research gap secara eksplisit. Research gap didefinisikan sebagai celah pengetahuan antara apa yang telah berhasil diteliti dengan apa yang masih belum diketahui atau belum terpecahkan, yang disertai alasan metodologis atau teoretis yang kuat untuk diisi. Pada jenjang doktoral, celah ini tidak boleh bersifat dangkal atau sekadar pengulangan eksperimen tanpa kontribusi konseptual yang jelas.

Sumber research gap dalam konteks pengolahan citra digital dan computer vision modern dapat berasal dari berbagai dimensi teknis maupun aplikatif:
- Keterbatasan arsitektur atau algoritma yang ada dalam menangani kompleksitas visual tertentu.
- Pergeseran distribusi data di lingkungan produksi yang membuat asumsi pelatihan awal menjadi tidak valid.
- Munculnya artefak atau kegagalan sistem akibat integrasi teknologi baru, seperti model generatif atau foundation model.
- Kebutuhan industri atau klinis yang belum terpenuhi oleh solusi komersial atau riset akademis terkini.
- Metrik evaluasi standar yang gagal menangkap aspek kritis seperti robustness, fairness, atau interpretability.
- Minimnya pemahaman mekanistik mengenai mengapa suatu pendekatan berhasil pada satu skenario namun gagal total pada skenario lain.

Perlu diperhatikan bahwa klaim “metode ini belum pernah diterapkan pada domain X” tidak serta merta memenuhi syarat sebagai gap ilmiah yang layak untuk disertasi. Adaptasi domain saja tidak cukup jika tidak dibarengi dengan analisis mendalam tentang mengapa karakteristik domain tersebut menghadirkan tantangan fundamental yang berbeda. Apakah terdapat pergeseran kovariat? Apakah constraint komputasi, noise karakteristik sensor, atau bias annotator memaksa modifikasi arsitektur? Tanpa justifikasi seperti ini, novelty penelitian akan dianggap marginal dan kurang siap untuk publikasi tingkat internasional.

Setelah Anda berhasil memetakan celah pengetahuan yang valid, langkah logis berikutnya adalah mengklasifikasikan jenis gap tersebut ke dalam kategori yang lebih terstruktur. Pada slide berikutnya, kita akan membahas taksonomi research gap secara sistematis, mulai dari empirical, theoretical, methodological, hingga evaluation gap, sehingga positioning penelitian Anda terhadap state-of-the-art dapat dikomunikasikan dengan presisi akademik yang tinggi dan mudah dievaluasi oleh reviewer.

---

## Slide 010 - Taksonomi Gap

### Narasi

Setelah mengidentifikasi adanya celah penelitian pada slide sebelumnya, langkah selanjutnya adalah mengklasifikasikan celah tersebut ke dalam taksonomi yang sistematis. Klasifikasi ini bukan sekadar formalitas administratif, melainkan fondasi kritis untuk merumuskan research question dan hipotesis yang terukur serta berkontribusi secara nyata terhadap state-of-the-art di bidang pengolahan citra digital dan computer vision.

Klasifikasi gap penelitian dapat dipetakan menjadi enam kategori utama yang sering kali saling beririsan dalam praktik riset tingkat doktoral:
- **Empirical gap**: Belum ada bukti empiris yang memverifikasi kinerja atau perilaku model pada kondisi eksperimen, populasi data, atau skenario dunia nyata tertentu.
- **Theoretical gap**: Kerangka konseptual atau prinsip matematis yang ada masih belum mampu menjelaskan mekanisme internal mengapa suatu metode berhasil atau gagal secara komprehensif.
- **Methodological gap**: Algoritma atau pipeline yang tersedia sudah tidak relevan, terlalu terbatas, atau sama sekali belum dikembangkan untuk menangani kompleksitas masalah yang muncul.
- **Domain gap**: Metode yang terbukti efektif pada satu tipe data mengalami degradasi performa signifikan saat diterapkan pada domain dengan karakteristik fisik, sensor, atau distribusi statistik yang berbeda.
- **Data gap**: Dataset yang dibutuhkan belum tersedia, belum cukup berkualitas, atau tidak merepresentasikan variasi kondisi yang esensial untuk pelatihan dan validasi model.
- **Evaluation gap**: Metrik evaluasi standar yang ada gagal menangkap aspek kritis kinerja sistem, seperti presisi batas objek kecil, robustness terhadap noise, atau konsistensi semantik dalam konteks multimodal.

Implikasi praktis dari taksonomi ini sangat menentukan arah penelitian. Satu kontribusi ilmiah yang solid jarang mengisi hanya satu jenis gap secara isolatif. Kombinasi dua hingga tiga jenis gap justru sering menjadi indikator novelty yang lebih bernilai tinggi dan memiliki daya tawar publikasi yang lebih kuat. Oleh karena itu, dalam penulisan proposal disertasi atau naskah jurnal bereputasi, Anda wajib menyebutkan secara eksplisit jenis gap mana yang Anda targetkan. Kejelasan ini memudahkan reviewer memposisikan karya Anda relatif terhadap literatur terkini dan menilai potensi kontribusi ilmunya secara objektif.

Untuk melihat bagaimana taksonomi ini diterjemahkan ke dalam praktik penelitian nyata, kita akan langsung beralih ke contoh konkret pada slide berikutnya. Kita akan menelusuri lintasan berpikir dari identifikasi keterbatasan model foundation seperti SAM, pemetaan multi-gap yang muncul, hingga perumusan peluang penelitian yang terstruktur dan siap diuji secara empiris.

---

## Slide 011 - Contoh Pemetaan Gap pada Literatur

### Narasi

Mari kita terjemahkan taksonomi gap yang telah dibahas pada slide sebelumnya menjadi contoh konkret dalam pemetaan literatur. Pendekatan ini menunjukkan bagaimana observasi kritis terhadap state-of-the-art dapat diarahkan secara sistematis menuju celah penelitian yang terukur dan berkontribusi.

Lintasan berpikir dalam studi kasus ini mengikuti empat langkah berurutan:
1. Kajian literatur mencatat bahwa SAM menunjukkan kemampuan generalisasi yang kuat pada citra natural.
2. Pengamatan empiris mengungkap bahwa pada citra satelit resolusi sangat tinggi, objek berukuran kecil dan tepi yang tidak tegas menyebabkan mask segmentasi kurang presisi.
3. Evaluasi standar yang lazim digunakan terbukti tidak cukup sensitif terhadap boundary error pada objek skala kecil.
4. Anotasi manual dalam cakupan luas secara praktis tidak feasible untuk domain tersebut.

Dari rangkaian pengamatan tersebut, tiga jenis gap teridentifikasi secara eksplisit:
- Domain gap: SAM belum memadai untuk menangani karakteristik unik citra satelit resolusi tinggi.
- Evaluation gap: Protokol metrik yang ada perlu diperluas agar lebih responsif terhadap kesalahan batas tepi dan objek kecil.
- Data gap: Belum tersedianya benchmark yang secara akurat merepresentasikan kondisi nyata tersebut.
Perhatikan bahwa satu fenomena dapat sekaligus mengisi beberapa jenis gap, sebagaimana diingatkan pada taksonomi sebelumnya. Penjelasan eksplisit mengenai jenis gap ini sangat krusial agar novelty penelitian mudah dikomunikasikan kepada reviewer dan positioned dengan tepat.

Identifikasi gap ini langsung membuka peluang riset yang terarah dan memberikan nilai tambah metodologis:
- Mengembangkan strategi prompt engineering atau adaptasi arsitektur SAM khusus untuk objek kecil pada citra satelit.
- Menyediakan benchmark baru serta protokol evaluasi yang lebih adil dan representatif.
Langkah ini memastikan bahwa kontribusi ilmiah tidak hanya bersifat teknis, tetapi juga memperkuat ekosistem validasi dan ketersediaan data untuk komunitas penelitian terkait.

Setelah gap berhasil dipetakan dan divalidasi melalui tinjauan literatur, langkah logis berikutnya adalah merumuskan pertanyaan penelitian yang presisi. Slide selanjutnya akan membahas definisi, karakteristik, serta contoh perbaikan research question agar pertanyaan tersebut benar-benar dapat diuji, relevan dengan gap yang ditemukan, dan memiliki kejelasan konseptual yang kuat untuk tahap desain eksperimen dan penulisan proposal disertasi.

---

## Slide 012 - Research Question

### Narasi

Setelah pada slide sebelumnya kita mengidentifikasi celah penelitian—misalnya keterbatasan SAM dalam menangani objek kecil pada citra satelit resolusi tinggi—langkah selanjutnya yang krusial adalah merumuskan Research Question (RQ) yang tepat. RQ bukan sekadar pertanyaan umum, melainkan pernyataan interrogatif yang dapat dijawab melalui penyelidikan sistematis, didukung oleh data empiris dan metode yang dirancang secara eksplisit.

Sebuah RQ yang baik untuk tingkat doktoral harus memenuhi empat karakteristik utama:
- Fokus: tidak terlalu luas hingga kehilangan kedalaman analitis, maupun terlalu sempit sehingga sulit memberikan kontribusi signifikan.
- Dapat diuji: harus ada rancangan eksperimen atau analisis yang memungkinkan pengumpulan bukti valid dan reproduktibel.
- Relevan: secara langsung menjawab gap yang telah dipetakan dan memiliki implikasi ilmiah yang jelas bagi komunitas peneliti.
- Jelas: menghindari ambiguitas atau makna ganda agar arah penelitian tetap terukur sejak awal.

Perhatikan contoh perbaikan yang disajikan. Pertanyaan seperti “Apakah deep learning dapat memperbaiki pencitraan?” terlalu umum dan sulit diuji secara spesifik. Sebaliknya, formulasi yang lebih tajam seperti “Sejauh mana penambahan constraint fisik pada diffusion model mengurangi detail halusinatif pada citra CT dosis rendah?” sudah mencakup variabel independen, konteks aplikasi, metrik evaluasi tersirat, dan batasan domain. Formulasi semacam ini jauh lebih siap dioperasionalkan menjadi hipotesis kerja dan desain eksperimen yang ketat.

Dengan RQ yang telah dikualifikasi, kita dapat mengelompokkannya ke dalam kategori tertentu sesuai tujuan investigasi. Hal ini akan kita bahas secara rinci pada slide berikutnya, di mana berbagai jenis RQ—mulai dari deskriptif, komparatif, relasional, hingga mekanistik atau kausal—akan dianalisis relevansinya terhadap standar riset jenjang S3. Pemilihan jenis RQ yang tepat akan menentukan kedalaman analisis, strategi evaluasi, serta potensi novelty yang dapat dihasilkan dalam disertasi.

---

## Slide 013 - Jenis-Jenis Research Question

### Narasi

Setelah pada slide sebelumnya kita membahas definisi dan karakteristik research question yang baik, kini kita beralih ke klasifikasi jenis-jenis pertanyaan penelitian. Dalam konteks riset pengolahan citra digital dan computer vision tingkat doktoral, pemilihan jenis RQ yang tepat akan menentukan arah metodologi, strategi eksperimen, hingga kontribusi ilmiah yang dapat Anda tawarkan.

Secara garis besar, pertanyaan penelitian dapat dikelompokkan menjadi empat kategori utama. Pertama, deskriptif, yang bertujuan memetakan fenomena atau pola data, misalnya menanyakan bagaimana distribusi karakteristik degradasi pada domain citra tertentu. Kedua, komparatif, yang berfokus pada keunggulan relatif antar metode atau arsitektur di bawah kondisi operasional spesifik. Ketiga, relasional, yang mengeksplorasi korelasi atau hubungan antar variabel, seperti pengaruh ukuran objek terhadap kualitas hasil segmentasi. Keempat, mekanistik atau kausal, yang berusaha mengungkap mengapa atau bagaimana penambahan modul tertentu mengubah perilaku model secara fundamental.

Untuk jenjang S3, penting untuk menyadari bahwa RQ deskriptif umumnya terlalu dangkal untuk memenuhi standar disertasi. Penelitian doctoral menuntut kedalaman analitis yang melampaui sekadar pemetaan atau perbandingan permukaan. Oleh karena itu, RQ yang kuat biasanya bersifat komparatif atau relasional, namun wajib dilengkapi dengan penjelasan mekanisme di balik temuan empiris. Anda juga disarankan merumuskan pertanyaan menggunakan konstruksi seperti *to what extent* atau *under what condition*, karena struktur ini secara alami mengarah pada eksplorasi batas performa, trade-off, dan kondisi batasan yang sangat relevan dalam pengembangan sistem visi komputer mutakhir.

Pemahaman mengenai jenis RQ ini menjadi prasyarat langsung sebelum kita mengevaluasi kelayakan pengujian pada slide berikutnya. Sebuah pertanyaan yang telah diklasifikasikan dengan tepat harus segera diperiksa apakah ia memenuhi empat syarat utama: spesifik, terukur, dapat dijawab dengan sumber daya yang tersedia, serta falsifiable. Penentuan jenis RQ yang sesuai akan secara langsung menuntun Anda dalam memilih baseline eksperimen, menetapkan metrik evaluasi, hingga merancang strategi ablation study yang koheren dengan tujuan penelitian.

---

## Slide 014 - RQ yang Dapat Diuji

### Narasi

Setelah membahas berbagai jenis pertanyaan penelitian pada slide sebelumnya, kita kini masuk ke tahap kritis dalam merumuskan Research Question yang layak untuk tingkat doktoral. Sebuah RQ tidak boleh sekadar terdengar menarik secara konseptual, tetapi harus memenuhi empat syarat utama agar dapat diuji secara ilmiah. Pertama, **spesifik**, artinya harus ada batasan objek, kondisi eksperimen, dan cakupan domain yang jelas. Kedua, **measurable**, outcome yang diharapkan harus bisa diukur menggunakan metrik kuantitatif atau prosedur evaluasi yang terstandarisasi. Ketiga, **answerable**, pertanyaan ini harus feasible diteliti dengan ketersediaan data, infrastruktur komputasi, dan sumber daya yang Anda miliki. Keempat, **falsifiable**, prinsip dasar metode ilmiah yang menuntut adanya kemungkinan hasil eksperimen dapat menggugurkan prediksi awal. Tanpa falsifiability, sebuah klaim penelitian akan jatuh ke ranah opini, bukan sains.

Dalam konteks pengolahan citra digital dan computer vision, keempat syarat ini diterjemahkan secara konkret. Alih-alih bertanya apakah "model generatif menghasilkan gambar lebih realistis", RQ yang spesifik dan measurable akan mengarah pada perbandingan arsitektur diffusion tertentu terhadap baseline pada dataset X, diukur menggunakan metrik FID atau KID, dengan protokol augmentasi yang dikontrol ketat. Aspek answerable menjadi sangat krusial di jenjang S3 karena ruang lingkup disertasi memiliki batas waktu dan kapasitas komputasi yang terbatas. Oleh karena itu, pemotongan scope harus dilakukan sejak fase perumusan RQ, bukan setelah eksperimen berjalan.

Untuk memastikan kualitas RQ sebelum memasuki fase implementasi, gunakan tiga pertanyaan refleksi diri sebagai filter akhir. Tanyakan apa observasi empiris yang secara langsung akan menjawab RQ tersebut. Siapkan skenario jika hasil observasi justru bertolak belakang dengan ekspektasi awal, karena penolakan hipotesis pun merupakan kontribusi ilmiah yang valid. Terakhir, verifikasi apakah investigasi ini benar-benar dapat diselesaikan dalam kerangka waktu penyusunan disertasi tanpa mengorbankan kedalaman analisis.

Perlu diingat bahwa RQ yang telah melalui proses penyaringan ini akan menjadi fondasi langsung bagi desain eksperimen yang dibahas pada pertemuan sebelumnya, termasuk pemilihan baseline yang relevan, penentuan metrik evaluasi, dan strategi ablation study yang terarah. Selanjutnya, ketika RQ sudah solid dan teruji, langkah logis berikutnya adalah menerjemahkannya menjadi pernyataan hipotesis yang terstruktur, yang akan kita bahas pada slide selanjutnya.

---

## Slide 015 - Dari Research Question ke Hipotesis

### Narasi

Setelah research question disaring hingga memenuhi empat kriteria utama—spesifik, measurable, answerable, dan falsifiable—langkah logis berikutnya adalah menerjemahkan pertanyaan tersebut menjadi hipotesis yang terstruktur. Pada jenjang doktoral, hipotesis bukan sekadar dugaan intuitif, melainkan pernyataan sementara yang grounded pada teori, sintesis literatur kritis, atau observasi awal terhadap fenomena visual. Pernyataan ini harus memiliki jalur verifikasi eksperimental yang jelas, sehingga memungkinkan pembuktian atau penyangkalan berbasis data.

Secara umum, hipotesis dalam penelitian pengolahan citra dan computer vision mengikuti pola logika yang ketat: jika kondisi atau intervensi tertentu diterapkan, maka outcome spesifik akan terjadi, karena mekanisme atau prinsip dasar yang diusulkan. Struktur tiga bagian ini memaksa peneliti untuk tidak hanya menyatakan hubungan antar variabel, tetapi juga mengartikulasikan landasan arsitektural atau matematis di balik prediksi tersebut. Tanpa komponen penjelasan mekanistik, hipotesis cenderung kehilangan daya uji dan sulit dipetakan ke dalam desain eksperimen.

Mari kita bedah contoh pada slide ini. Pertanyaan riset menyoroti apakah penambahan geometri guidance pada diffusion model meningkatkan konsistensi struktur pada area yang ter-occlude. Hipotesisnya dirumuskan bahwa penambahan guidance tersebut akan meningkatkan konsistensi struktur dibandingkan baseline tanpa guidance, karena model memperoleh constraint tambahan yang menjaga kesesuaian representasi dengan geometri scene. Perhatikan keselarasan setiap elemen: perlakuan (geometri guidance), outcome terukur (konsistensi struktur di region occluded), dan mekanisme (constraint geometri scene). Formulasi ini secara langsung mengarahkan pemilihan metrik evaluasi, strategi ablation study, serta protokol kontrol variabel dalam eksperimen.

Ketika hipotesis telah matang, ia akan memasuki fase pengujian formal yang dibahas pada slide berikutnya. Penting untuk membedakan antara hipotesis riset yang bersifat konseptual dengan hipotesis statistik operasional seperti nol (H0) dan alternatif (H1). Untuk disertasi di bidang PCD, bobot argumentatif lebih terletak pada kekuatan hipotesis riset itu sendiri, bukan semata-mata pada hasil uji signifikansi statistik. Eksperimen harus dirancang dengan fair comparison, sehingga perbedaan performa benar-benar mencerminkan kontribusi metode yang diteliti, bukan artefak dari distribusi data, konfigurasi hyperparameter, atau baseline yang tidak setara. Presisi dalam merumuskan hipotesis, oleh karena itu, menjadi fondasi validitas internal seluruh pipeline penelitian Anda.

---

## Slide 016 - Hipotesis dalam Konteks Eksperimen Sains

### Narasi

Pada slide sebelumnya, kita telah membahas kerangka umum perumusan hipotesis berdasarkan research question yang telah diidentifikasi. Kini, kita akan menempatkan hipotesis tersebut dalam konteks eksperimen ilmiah yang ketat, sesuai standar penelitian tingkat doktoral di bidang pengolahan citra digital dan computer vision.

Dalam praktik riset sains, penting untuk memisahkan konsep hipotesis riset dari hipotesis statistik. Hipotesis riset merupakan klaim substantif yang menjawab pertanyaan penelitian Anda. Sementara itu, hipotesis nol atau H0 menyatakan bahwa tidak terdapat perbedaan atau efek signifikan dari intervensi yang diteliti. Sebaliknya, hipotesis alternatif atau H1 menyatakan adanya perbedaan atau efek yang diharapkan. Pembagian ini bukan sekadar formalitas uji statistik, melainkan fondasi logis untuk merancang validasi yang bermakna.

Hipotesis juga dapat diklasifikasikan berdasarkan arah prediksinya. Hipotesis non-directional hanya menyatakan bahwa terdapat perbedaan antara dua pendekatan pada metrik tertentu, tanpa memprediksi arah perbedaannya. Sebaliknya, hipotesis directional secara eksplisit memprediksi bahwa pendekatan A akan lebih unggul daripada pendekatan B pada kondisi atau skenario spesifik. Pemilihan jenis ini harus didasari oleh tinjauan literatur atau mekanisme teoritis yang kuat, sehingga prediksi Anda memiliki pijakan yang jelas.

Untuk penelitian doktoral di bidang TD PCD, penekanan utamanya terletak pada kedalaman hipotesis riset itu sendiri, bukan sekadar pencapaian nilai signifikansi statistik. Eksperimen yang rigor harus mampu mengisolasi pengaruh metode yang Anda usulkan dari faktor pengganggu lain. Hal ini mencakup kontrol terhadap bias dataset, kesetaraan konfigurasi hyperparameter, dan pemilihan baseline yang adil. Tanpa isolasi variabel yang tepat, hasil pengujian hipotesis akan sulit dipertanggungjawabkan secara ilmiah.

Pemahaman tentang struktur dan arah hipotesis ini menjadi prasyarat langsung sebelum masuk ke tahap operasionalisasi. Pada slide berikutnya, kita akan mengurai komponen teknis agar hipotesis tersebut benar-benar dapat diuji secara empiris, meliputi identifikasi variabel bebas, variabel terikat, kondisi eksperimen, serta prediksi kuantitatif atau kualitatif yang terukur dan siap diterjemahkan ke dalam rancangan eksperimen.

---

## Slide 017 - Merumuskan Hipotesis yang Dapat Diuji

### Narasi

Melanjutkan pembahasan pada slide sebelumnya tentang perbedaan hipotesis riset dan statistik serta arah hipotesis, slide ini membawa kita ke langkah praktis merumuskannya agar benar-benar dapat diuji secara empiris. Untuk penelitian tingkat doktoral, hipotesis tidak boleh bersifat ambigu atau hanya berupa pernyataan kualitatif tanpa kerangka eksperimental yang ketat. Agar hipotesis tersebut valid, terukur, dan siap dieksekusi dalam environment komputasi, ia harus dibangun atas empat komponen inti berikut:

- Variabel bebas: faktor utama yang akan dimanipulasi, divariasikan, atau dibandingkan antar baseline dan proposed method.
- Variabel terikat: outcome atau performa sistem yang diukur sebagai respons langsung terhadap perubahan pada variabel bebas.
- Kondisi eksperimen: spesifikasi domain data, skenario pengujian, atau batasan lingkungan tempat metode tersebut diterapkan.
- Prediksi kuantitatif atau kualitatif: pernyataan arah dan besaran efek yang diharapkan, yang biasanya diturunkan dari literatur atau analisis teoretis awal.

Tabel perancangan hipotesis pada slide ini berfungsi sebagai peta alur logika yang mengunci setiap elemen desain penelitian. Setiap kolom saling bergantung untuk memastikan konsistensi antara pertanyaan riset dan alat ukurnya. Ambil contoh kasus pemodelan degradasi gambar: pertanyaan riset menyoroti peningkatan generalisasi, yang diterjemahkan menjadi hipotesis bahwa model mampu memisahkan karakteristik degradasi dari semantik objek. Variabel bebasnya adalah strategi pemodelan degradasi, sedangkan variabel terikatnya adalah robustness pada unseen degradation. Prediksi kuantitatifnya dinyatakan dalam perbaikan skor PSNR dan LPIPS, yang sekaligus menjadi acuan metrik wajib dalam pelaporan hasil eksperimen.

Struktur formulasi ini memberikan dua dampak operasional yang signifikan. Pertama, tabel ini bertindak sebagai blueprint teknis saat kita masuk ke tahap implementasi eksperimen pada pertemuan berikutnya, sehingga setiap baris kode, augmentasi, atau pipeline preprocessing memiliki target evaluasi yang jelas. Kedua, format ini memandu kita dalam menentukan apa yang harus dilaporkan sebagai bukti ilmiah. Dalam standar publikasi bereputasi, kejelasan komponen hipotesis mencegah bias seleksi data dan memastikan bahwa klaim novelty didukung oleh evidence yang transparan, terukur, dan mudah direproduksi.

Dengan hipotesis yang telah terstruktur dan teroperasionalkan seperti ini, fondasi logis penelitian sudah siap. Langkah selanjutnya adalah menempatkan seluruh elemen ini ke dalam rantai argumen penelitian, di mana hipotesis akan diuji melalui metodologi spesifik untuk menghasilkan bukti yang selaras dengan research gap dan pertanyaan riset awal, sebagaimana akan dibahas pada tautan logis antara RQ, hipotesis, metode, dan bukti.

---

## Slide 018 - Hubungan RQ, Hipotesis, Metode, dan Bukti

### Narasi

Setelah pada pertemuan sebelumnya kita menyusun hipotesis yang terstruktur berdasarkan variabel bebas, variabel terikat, dan prediksi kuantitatif atau kualitatif, langkah selanjutnya adalah memastikan bahwa setiap komponen penelitian tersusun dalam satu alur logika yang koheren. Hipotesis tidak berdiri sendiri; ia harus menjadi bagian integral dari sebuah rantai argumen yang ketat, terutama pada level doktoral di mana validitas metodologis dan kejelasan kontribusi sangat ditekankan.

Slide ini menyajikan Rantai Argumen sebagai fondasi desain penelitian Anda. Alurnya dimulai dari Research Gap yang telah diidentifikasi, yang secara langsung melahirkan Research Question. Pertanyaan penelitian tersebut kemudian memandu perumusan Hipotesis, yang selanjutnya menentukan pilihan Metode atau Eksperimen. Pelaksanaan eksperimen menghasilkan Observasi atau Bukti empiris, yang akhirnya menjadi dasar untuk merumuskan Klaim Kontribusi. Perhatikan bahwa hubungan antar-node bersifat kausal dan hierarkis; jika salah satu elemen lemah atau tidak selaras, seluruh klaim penelitian akan kehilangan daya persuasifnya.

Konsistensi menjadi prinsip utama yang harus dijaga sepanjang rantai tersebut. Setiap penyesuaian pada research gap wajib diikuti oleh revisi terhadap RQ, hipotesis, dan klaim kontribusi agar tidak terjadi disonansi logis. Metode eksperimen hanyalah alat untuk menghasilkan bukti yang kredibel, bukan tujuan akhir dari disertasi. Lebih penting lagi, klaim kontribusi tidak boleh melampaui kekuatan bukti yang sebenarnya dihasilkan; overclaiming adalah kesalahan fatal yang sering menjadi alasan penolakan pada tinjauan kritis paper atau proposal disertasi.

Dengan memastikan konsistensi ini, Anda telah menyiapkan landasan yang solid sebelum mengevaluasi aspek paling krusial berikutnya: novelty. Pada slide berikutnya, kita akan membedah bagaimana membedakan inovasi ilmiah yang substantif dari sekadar variasi teknis, serta menggunakan uji sederhana untuk memvalidasi apakah kontribusi Anda benar-benar memiliki nilai tambah yang dapat dipertanggungjawabkan di mata reviewer internasional.

---

## Slide 019 - Novelty

### Narasi

Pada slide ini, kita membahas konsep inti yang menjadi penentu validitas penelitian tingkat doktoral, yaitu *novelty*. Novelty didefinisikan sebagai aspek baru yang membedakan penelitian Anda dari karya-karya sebelumnya, sekaligus membawa nilai ilmiah yang terukur. Dalam rantai argumen yang telah kita bahas pada slide sebelumnya, novelty berfungsi sebagai benang merah yang menghubungkan *research gap*, formulasi pertanyaan penelitian, hipotesis, hingga klaim kontribusi akhir. Tanpa novelty yang jelas, seluruh struktur penelitian kehilangan landasan keberadaannya.

Penting untuk secara tegas mengidentifikasi praktik yang *bukan* merupakan novelty, karena kesalahan konseptual ini sering kali merendahkan kualitas publikasi. Mengganti dataset tanpa justifikasi ilmiah yang kuat, menggabungkan modul atau blok arsitektur yang sudah ada secara *ad-hoc* tanpa analisis mendasar, atau sekadar menambah parameter tuning tanpa menjelaskan mekanisme dan dampaknya terhadap perilaku model, bukanlah kontribusi ilmiah. Demikian pula, menyajikan pipeline yang hanya berupa komposisi mekanis dari berbagai metode yang ada, atau mengklaim hasil "lebih baik" tanpa analisis kausal mengapa pendekatan Anda unggul, akan dianggap sebagai rekayasa teknis belaka.

Untuk menguji ketajaman novelty, terapkan tiga kriteria verifikasi sederhana. Pertama, lakukan penelusuran literatur ketat: apakah ide atau pendekatan ini sudah pernah dipublikasikan secara eksplisit? Kedua, pastikan perbedaan yang Anda tawarkan bersifat fundamental dalam prinsip atau mekanisme, bukan hanya penyempurnaan detail teknis atau optimasi hiperparameter. Ketiga, coba nyatakan novelty tersebut dalam satu kalimat yang lugas dan dapat dipahami oleh reviewer di luar subbidang spesifik Anda. Jika novelitas Anda tidak lolos uji ini, maka perlu disederhanakan, diperdalam, atau difokuskan ulang agar lebih tajam dan terukur.

Pemahaman tentang esensi novelty ini menjadi fondasi langsung menuju klasifikasi pada slide berikutnya. Kita akan melihat bagaimana novelty dapat dikategorikan ke dalam lima tingkat kontribusi, mulai dari teoretis, algoritmik, empiris, dataset, hingga sistem/aplikasi. Pada jenjang S3, disertasi diharapkan minimal menghasilkan satu kontribusi yang bersifat metodologis atau teoretis, meskipun kontribusi empiris maupun berbasis data tetap sangat relevan apabila ditempatkan dalam kerangka evaluasi kritis terhadap state-of-the-art.

---

## Slide 020 - Tingkat Novelty

### Narasi

Setelah membedah definisi novelty dan mengidentifikasi praktik yang bukan merupakan kebaruan ilmiah pada slide sebelumnya, kita kini beralih ke klasifikasi tingkat kontribusi penelitian. Pada jenjang doktoral, novelty tidak bersifat mutlak tunggal, melainkan memiliki gradasi berdasarkan kedalaman dan jenis sumbangannya terhadap kemajuan bidang Pengolahan Citra Digital maupun Computer Vision.

Berikut adalah rincian lima tingkatan kontribusi yang lazim digunakan sebagai acuan evaluasi akademik:
- **Teoretis/Analitis**: Memberikan pemahaman atau prinsip baru melalui analisis mendalam. Contoh konkretnya adalah menjelaskan mengapa mekanisme attention sering gagal pada objek berukuran kecil dalam arsitektur Vision Transformer.
- **Algoritmik/Metodologis**: Mengusulkan metode, arsitektur, atau algoritma baru. Misalnya, merancang modul adaptasi prompt khusus untuk Segment Anything Model (SAM) agar lebih robust terhadap variasi kontekstual.
- **Empiris**: Menemukan bukti baru mengenai perilaku sistem. Studi mengenai generalisasi foundation model seperti DINOv2 atau CLIP pada domain medis yang memiliki distribusi data sangat berbeda dari data natural termasuk kategori ini.
- **Dataset/Anotasi**: Menyediakan data atau prosedur anotasi baru yang belum tersedia secara publik. Penyusunan benchmark terstandarisasi untuk deteksi objek kecil pada citra satelit dengan anotasi hierarkis adalah contoh representatif.
- **Sistem/Aplikasi**: Membangun sistem atau workflow yang memecahkan masalah praktis. Pipeline deteksi dini berbasis drone yang mengintegrasikan multi-sensor dan edge computing jatuh dalam ranah ini.

Perlu ditekankan bahwa untuk disertasi S3, minimal harus terdapat satu kontribusi yang bersifat metodologis atau teoretis. Meskipun kontribusi empiris dan dataset sangat bernilai bagi komunitas riset, keduanya cenderung kurang kuat jika berdiri sendiri tanpa landasan analisis mendalam atau inovasi struktural. Ekspektasi ini sejalan dengan standar reviewer di jurnal bereputasi tinggi yang menuntut kedalaman kontributif, bukan sekadar penerapan ulang atau komparasi permukaan.

Pemahaman mengenai gradasi tingkat novelty ini akan menjadi fondasi krusial ketika kita mengevaluasi sumber-sumber kebaruan yang spesifik dalam domain Pengolahan Citra Digital pada slide berikutnya. Kita akan menelusuri titik-titik masuk kebaruan yang potensial, sekaligus menerapkan prinsip seleksi agar novelty yang dirumuskan benar-benar feasible untuk diuji dalam timeline disertasi.

---

## Slide 021 - Sumber Novelty dalam Pengolahan Citra Digital

### Narasi

Pada slide sebelumnya, kita telah mengklasifikasikan berbagai tingkat kontribusi penelitian, mulai dari teoretis, algoritmik, empiris, hingga sistem atau dataset. Pembahasan tersebut menegaskan bahwa untuk jenjang S3, kontribusi metodologis atau teoretis menjadi fondasi utama yang wajib dipenuhi. Langkah logis selanjutnya setelah memahami jenis kontribusi adalah menentukan dari mana kebaruan itu sebenarnya dapat digali. Slide ini akan menguraikan sumber potensial novelitas dalam domain Pengolahan Citra Digital dan Computer Vision, serta prinsip strategis dalam memilihnya agar relevan dengan standar penelitian doktoral.

Mari kita telusuri titik masuk kebaruan yang dapat Anda eksplorasi. Pertama, Anda dapat mengangkat domain atau masalah aplikasi yang belum terpetakan secara memadai oleh literatur terkini, misalnya adaptasi foundation model pada kondisi pencahayaan ekstrem atau domain medis dengan keterbatasan anotasi expert. Kedua, fokus pada karakteristik degradasi atau gangguan citra yang masih jarang ditangani secara komprehensif, seperti noise non-stasioner, artefak kompresi adaptif, atau distorsi optik kompleks. Ketiga, eksplorasi representasi visual atau arsitektur jaringan saraf yang berbeda secara prinsip dari pendekatan konvensional, misalnya integrasi geometri diferensial ke dalam arsitektur CNN atau modifikasi mekanisme attention untuk pemrosesan multi-resolusi.

Selain aspek struktural, strategi pembelajaran dan evaluasi juga menjadi lahan inovatif yang sangat menjanjikan. Anda dapat mengembangkan metode self-supervised learning yang lebih efisien, merancang protokol prompting yang adaptif untuk model besar seperti SAM atau CLIP, atau mengeksplorasi teknik fine-tuning ringan yang mempertahankan performa tanpa beban komputasi berlebihan. Aspek evaluasi pun sering kali terlewatkan, padahal menyusun protokol benchmark yang lebih adil, transparan, dan informatif terhadap bias model merupakan bentuk kontribusi yang sangat dihargai di komunitas riset. Terakhir, kemampuan menjelaskan mekanisme di balik keberhasilan atau kegagalan model—melalui interpretability, error analysis, atau causal reasoning—sering kali membuka jalan bagi kebaruan yang berdampak signifikan dan dapat direplikasi.

Namun, memiliki banyak ide kebaruan tidak berarti Anda harus mengejar semuanya sekaligus. Prinsip pemilihan novelty menuntut realisme akademik dan fokus strategis. Pastikan kebaruan yang Anda pilih benar-benar dapat diuji secara empiris dalam kerangka waktu, infrastruktur komputasi, dan ketersediaan data yang tersedia selama masa studi S3. Hindari ambisi yang terlalu luas dengan mencoba menggabungkan seluruh dimensi inovasi sekaligus, karena hal ini justru berisiko mengaburkan kontribusi inti dan melemahkan validitas eksperimen Anda. Kuncinya adalah menghubungkan setiap klaim novelty secara langsung dengan research gap yang telah Anda identifikasi, divalidasi, dan dipetakan melalui tinjauan literatur kritis. Dengan demikian, setiap langkah pengembangan metodologi atau desain eksperimen akan memiliki justifikasi ilmiah yang kuat, terukur, dan siap diuji.

Pemahaman tentang sumber dan prinsip seleksi novelty ini akan menjadi landasan konseptual sebelum kita turun ke tahap operasionalisasi penelitian. Pada slide berikutnya, kita akan membahas bagaimana menerjemahkan gap literatur menjadi kebaruan yang konkret melalui sebuah workflow sistematis, lengkap dengan ilustrasi pseudocode yang merepresentasikan proses iteratif penyusunan concept note, serta pentingnya validasi silang terhadap state-of-the-art terkini.

---

## Slide 022 - Menemukan Novelty dari Gap

### Narasi

Setelah pada slide sebelumnya kita mengidentifikasi berbagai titik masuk potensial untuk kebaruan—mulai dari domain masalah baru, karakteristik degradasi yang belum terpetakan, hingga strategi pembelajaran mutakhir—langkah selanjutnya adalah menyaring ide-ide tersebut melalui proses yang lebih ketat dan sistematis. Menghadapi jenjang doktoral, sekadar memiliki ide yang terdengar menarik tidak cukup. Kita perlu mengubah observasi literatur menjadi celah penelitian yang dapat diuji secara empiris dan berkontribusi nyata terhadap perkembangan bidang ini.

Workflow yang disajikan pada slide ini berfungsi sebagai panduan operasional untuk menjembatani kesenjangan antara tinjauan kritis terhadap paper dan perumusan kontribusi orisinal. Prosesnya terdiri dari enam langkah terstruktur: pertama, identifikasi gap spesifik dari hasil critical paper review. Kedua, jelaskan secara meyakinkan mengapa gap tersebut tidak sepele dan berdampak pada kemajuan metodologi. Ketiga, susun pendekatan atau sudut pandang yang berbeda secara prinsip, bukan sekadar penyesuaian hiperparameter minor. Keempat, tulis klaim kontribusi awal dalam bentuk pernyataan yang jelas dan dapat diukur. Kelima, lakukan pengecekan silang terhadap state-of-the-art terkini untuk memastikan orisinalitas. Keenam, jika ditemukan kemiripan, perbesar diferensiasi metodologis atau kontekstualnya hingga benar-benar unik.

Ilustrasi pseudocode pada slide ini merepresentasikan alur logika tersebut secara komputasional. Meskipun tidak dimaksudkan untuk dieksekusi langsung sebagai skrip Python, struktur `for setiap gap dalam literatur:` menekankan bahwa pencarian novelty harus bersifat eksploratif namun terarah. Kondisi `if gap memiliki justifikasi kuat:` menuntut validasi signifikansi masalah sebelum melangkah ke tahap usulan pendekatan. Bagian `bandingkan dengan SOTA` dan `if bukan duplikasi:` berfungsi sebagai mekanisme filtering untuk mencegah redundansi ilmiah. Hasil akhirnya adalah kumpulan kandidat kontribusi yang telah disaring, siap dimasukkan ke dalam concept note penelitian.

Perlu ditekankan bahwa seluruh workflow ini bersifat iteratif dan sangat bergantung pada validasi eksternal. Masukan dari dosen pembimbing serta diskusi dengan rekan sejawat berperan penting untuk mempertajam argumen dan menghindari bias konfirmasi. Setelah satu atau beberapa kandidat kontribusi berhasil diformulasikan dengan solid, langkah natural berikutnya adalah menempatkan proposal Anda secara eksplisit dalam peta penelitian yang ada. Hal ini akan kita bahas pada slide selanjutnya, yaitu bagaimana menyusun tabel positioning terhadap state-of-the-art untuk memetakan kekuatan, kelemahan, dan posisi strategis penelitian Anda dibandingkan metode-metode yang sudah terpublikasi.

---

## Slide 023 - Positioning terhadap State-of-the-Art

### Narasi

Setelah menyelesaikan langkah-langkah sistematis untuk menemukan novelty dari celah literatur, fokus kita sekarang bergeser ke pemetaan posisi penelitian Anda secara eksplisit terhadap state-of-the-art. Slide ini menyajikan struktur tabel perbandingan yang dirancang untuk menempatkan usulan metode di antara pendekatan yang telah dipublikasikan, seperti Paper A yang mengandalkan arsitektur U-Net konvensional, maupun Paper B yang memanfaatkan SAM dengan mekanisme box prompt. Tabel ini berfungsi sebagai jembatan analitis antara ide awal dan validasi empiris.

Kolom-kolom dalam tabel diposisikan untuk memaksa kejelasan akademis. Setiap baris harus diisi berdasarkan bukti publikasi yang nyata, mencakup problem spesifik, arsitektur atau pipeline yang digunakan, dataset benchmark, metrik evaluasi, hasil kuantitatif, hingga keterbatasan yang diakui oleh penulis asli. Perhatikan bahwa kolom Keterbatasan pada baris usulan sengaja dibiarkan kosong atau ditandai dengan tanda tanya. Ini bukan kelalaian, melainkan refleksi dari sikap ilmiah yang sehat: setiap metode baru pasti memiliki trade-off, dan pengakuan dini atas batas tersebut justru meningkatkan kredibilitas proposal Anda di tingkat doktoral.

Fungsi strategis tabel perbandingan ini dapat diringkas sebagai berikut:
- Menjelaskan posisi penelitian secara transparan, menghindari klaim berlebihan yang sering menjadi kelemahan naskah awal.
- Membuktikan pemahaman mendalam terhadap kekuatan dan kelemahan metodologi sebelumnya, sehingga usulan Anda benar-benar menargetkan celah yang spesifik dan terukur.
- Berfungsi sebagai blueprint langsung untuk penyusunan contribution statement dan desain eksperimen yang akan menguji claim Anda secara ketat.

Dengan tabel ini, diskusi tidak lagi bersifat konseptual semata, melainkan sudah berakar pada data dan literatur terkini. Posisi yang telah dipetakan secara objektif ini akan menjadi bahan baku utama untuk slide berikutnya, yaitu penulisan contribution statement. Dari sel-sel tabel, Anda akan mengekstrak elemen inti—objek kontribusi, prinsip pembeda, dan metrik validasi—untuk kemudian dirangkum menjadi satu pernyataan penelitian yang padat, teruji, dan siap diajukan dalam forum akademik internasional.

---

## Slide 024 - Menulis Contribution Statement

### Narasi

Setelah kita memetakan posisi penelitian terhadap state-of-the-art melalui tabel perbandingan pada slide sebelumnya, langkah logis berikutnya adalah merangkum analisis tersebut ke dalam sebuah *contribution statement* yang terstruktur dan terukur. Pernyataan kontribusi ini berfungsi sebagai inti dari proposal atau naskah penelitian Anda, karena ia secara eksplisit menyatakan apa yang baru, mengapa hal itu penting, dan bagaimana klaim tersebut akan divalidasi secara empiris.

Gunakan template satu paragraf berikut sebagai kerangka penulisan. Isi setiap variabel secara spesifik agar tidak terjadi ambiguitas saat direview. Bagian pertama harus mendefinisikan jenis kontribusi dan objek penelitiannya. Bagian kedua menyoroti celah atau *research gap* yang dituju, dilanjutkan dengan prinsip atau ide teknis utama yang menjadi pembeda. Selanjutnya, tunjukkan secara tegas perbedaan dengan metode *state-of-the-art* yang telah ada. Terakhir, sebutkan desain eksperimen atau bukti validasi yang akan digunakan untuk menguji klaim tersebut.

Sebagai ilustrasi konkret, perhatikan contoh berikut. Penelitian ini menyumbang metode segmentasi adaptif prompt untuk citra satelit resolusi tinggi yang mengatasi ketidakstabilan SAM pada objek kecil melalui mekanisme fokus multi-skala. Kontribusi ini berbeda dari pendekatan *fine-tuning* penuh yang membutuhkan komputasi GPU besar, dan diuji pada benchmark baru dengan metrik yang sensitif terhadap batas objek kecil. Perhatikan bagaimana setiap komponen template terpenuhi secara berurutan, sehingga reviewer langsung memahami novelty dan feasibility penelitian Anda.

Untuk mengasah ketajaman penulisan, coba susun *contribution statement* Anda dalam dua hingga tiga kalimat saja. Pembatasan panjang ini memaksa Anda untuk membuang jargon berlebihan dan memilih diksi yang presisi. Setelah draf selesai, minta *peer feedback* secara langsung. Fokuskan pertanyaan pada apakah kalimat pembuka sudah cukup membedakan penelitian Anda dari publikasi terkini. Jika jawabannya ragu, lakukan iterasi penyuntingan hingga pernyataan tersebut benar-benar tajam dan siap diuji.

Pernyataan kontribusi yang padat dan jelas akan menjadi fondasi natural untuk membahas dampak penelitian. Pada slide berikutnya, kita akan menguraikan cara merumuskan *significance* dan manfaat ilmiah dari kontribusi tersebut, baik dalam konteks pengembangan metodologi pengolahan citra digital maupun implikasi aplikatifnya di industri dan riset lanjutan.

---

## Slide 025 - Significance dan Manfaat Ilmiah

### Narasi

Setelah pada slide sebelumnya kita menyusun *contribution statement* yang padat dan terstruktur, langkah selanjutnya adalah memperluas perspektif dengan menjelaskan *significance* atau signifikansi penelitian. Significance bukan sekadar pengulangan kontribusi teknis, melainkan penjelasan mengenai dampak nyata dari temuan Anda terhadap komunitas ilmiah, praktisi di lapangan, maupun masyarakat luas. Pada jenjang doktoral, penekanan ini sangat krusial karena menunjukkan bahwa riset Anda tidak hanya mengisi celah pengetahuan (*research gap*), tetapi juga memberikan nilai tambah yang dapat diukur, direplikasi, dan diaplikasikan.

Untuk merumuskan signifikansi secara tajam, gunakan serangkaian pertanyaan pemandu berikut sebagai acuan penulisan:
- Siapa yang akan menggunakan hasil penelitian ini? Identifikasi target pengguna, apakah peneliti lain, praktisi *computer vision*, atau pemangku kepentingan industri.
- Keputusan apa yang dapat berubah berdasarkan hasil ini? Tinjau bagaimana temuan Anda mengubah praktik standar atau mendukung pengambilan keputusan berbasis data.
- Penelitian lanjutan apa yang menjadi terbuka? Lihat peluang eksplorasi metodologi, arsitektur model, atau aplikasi baru yang lahir dari pendekatan Anda.
- Bagaimana penelitian ini memperluas pemahaman di bidang PCD? Evaluasi kontribusi teoretis atau konseptual terhadap perkembangan pengolahan citra digital dan visi komputer.

Sebagai ilustrasi konkret, manfaat ilmiah bisa berupa penyediaan *benchmark* baru untuk evaluasi objek kecil, atau justru menjelaskan keterbatasan mendasar dari *foundation model* seperti SAM atau CLIP ketika dihadapkan pada domain citra satelit tertentu. Sementara itu, manfaat praktisnya sangat terasa ketika metode yang dikembangkan membantu analis citra memantau perubahan lingkungan dengan akurasi dan efisiensi yang jauh melampaui metode konvensional. Pastikan contoh manfaat ini selaras dengan *contribution statement* yang telah disusun, sehingga narasi penelitian tetap koheren dan tidak bertele-tele.

Signifikansi yang terdefinisi dengan jelas juga menjadi fondasi penting sebelum kita beralih ke materi berikutnya. Pada slide selanjutnya, kita akan menyusun *kerangka integratif satu halaman* yang memetakan rantai logika dari *gap*, *research question*, hipotesis, hingga klaim kontribusi, memastikan bahwa setiap pernyataan signifikansi didukung oleh desain eksperimen dan bukti empiris yang valid serta proporsional.

---

## Slide 026 - Kerangka Integratif Satu Halaman

### Narasi

Pada slide ini, kita akan membahas kerangka integratif satu halaman yang menjadi tulang punggung konsistensi logika penelitian Anda. Setelah sebelumnya mendefinisikan signifikansi dan manfaat ilmiah dari kontribusi yang ingin Anda berikan, langkah selanjutnya adalah memastikan bahwa seluruh elemen penelitian tersusun dalam alur kausal yang ketat dan dapat dipertanggungjawabkan secara akademis.

Diagram pada slide ini menggambarkan rantai konsep yang harus saling terhubung secara linear maupun siklik. Alurnya dimulai dari identifikasi research gap, yang kemudian langsung melahirkan research question. Dari pertanyaan penelitian tersebut, Anda merumuskan hipotesis sebagai prediksi terukur yang akan diuji. Selanjutnya, metode dan desain eksperimen disusun khusus untuk mengumpulkan bukti empiris. Bukti ini kemudian digunakan untuk mendukung atau menolak hipotesis, yang akhirnya menghasilkan klaim kontribusi penelitian. Perhatikan juga adanya umpan balik dari eksperimen dan observasi ke dalam metode serta klaim, yang menegaskan bahwa validitas klaim sepenuhnya bergantung pada kualitas data dan analisis yang dihasilkan.

Ada empat prinsip kunci yang perlu Anda pegang dalam kerangka ini:
- Research question tidak boleh muncul secara tiba-tiba; ia harus merupakan konsekuensi langsung dari celah pengetahuan yang telah Anda identifikasi.
- Hipotesis harus bersifat falsifiable atau dapat dibantah melalui pengujian empiris.
- Desain eksperimen dan metode teknis harus selaras dengan variabel yang ingin diuji dalam hipotesis.
- Klaim novelty atau kontribusi Anda tidak boleh melampaui batas bukti yang berhasil dikumpulkan selama eksperimen. Overclaiming adalah kesalahan metodologis yang sering menjadi kritik utama dalam review jurnal bereputasi tingkat Q1 atau konferensi top-tier.

Sebagai latihan praktis, saya meminta Anda menggambar ulang diagram ini secara manual untuk proposal disertasi Anda sendiri. Gunakan kertas kosong atau canvas digital, lalu hubungkan setiap komponen dengan panah. Jika Anda menemukan bagian mana pun yang tidak bisa dijelaskan hubungan sebab-akibatnya—misalnya, mengapa hipotesis tertentu dipilih, atau bagaimana metode tertentu menjawab pertanyaan penelitian—maka formulasi awal Anda masih perlu diperbaiki. Konsistensi internal inilah yang membedakan penelitian doktoral yang matang dengan sekadar kumpulan eksperimen yang terfragmentasi.

Kerangka satu halaman ini akan segera kita terapkan pada slide berikutnya, di mana kita akan mengubah topik umum menjadi research question yang spesifik melalui pemetaan gap secara sistematis. Pastikan Anda sudah menyiapkan draf awal diagram ini sebelum melanjutkan ke tahap konversi topik.

---

## Slide 027 - Latihan: Mengubah Topik Umum Menjadi RQ

### Narasi

Slide ini menerjemahkan kerangka konseptual slide sebelumnya menjadi praktik operasional. Ingat kembali rantai logika pada slide 26, di mana setiap transisi dari gap ke RQ, lalu ke hipotesis dan desain eksperimen, harus memiliki justifikasi yang dapat dipertanggungjawabkan. Di sinilah kita menguji kemampuan Anda mempersempit topik riset yang masih bersifat generik menjadi pertanyaan penelitian yang tajam dan terukur.

Tabel pada slide menyajikan empat pola umum yang sering ditemui dalam penulisan proposal tingkat doktor. Setiap baris menunjukkan transformasi bertahap dari topik luas menuju formulasi RQ yang spesifik. Mari kita tinjau mekanismenya secara singkat.

- Image restoration: Masalahnya bukan sekadar ketidakstabilan diffusion model, melainkan kegagalan model menangani degradasi campuran secara simultan. RQ kemudian difokuskan pada efektivitas constraint fisik dalam menekan halusinasi struktur.
- Segmentasi sel: Foundation model umumnya robust, namun performanya drop drastis pada variasi kontras dan noise mikroskopis antar-institusi. RQ diarahkan pada mekanisme adaptasi instance-aware prompt, dengan IoU sebagai proxy keberhasilan.
- Deteksi objek kecil: Evaluasi berbasis mAP standar cenderung mendominasi skor keseluruhan dan mengaburkan performa pada skala sub-pixel. RQ menuntut eksplorasi metrik alternatif yang lebih sensitif terhadap distribusi ukuran objek ekstrem.
- Foundation model: Banyak studi melaporkan keunggulan transfer learning tanpa validasi ketat pada domain target. RQ menjadi pertanyaan komparatif langsung antara embedding DINOv2 dan arsitektur CNN konvensional pada domain X, dengan kontrol variabel yang eksplisit.

Tugas Anda sekarang adalah mengisi tabel serupa menggunakan topik penelitian disertasi masing-masing. Perhatikan bahwa bagian gap wajib didasari oleh bukti literatur, inkonsistensi empiris, atau batasan asumsi metodologis yang teridentifikasi. Hindari frasa seperti "belum ada penelitian" atau "kurang dieksplorasi" tanpa dukungan sitasi atau analisis kritis. Gap yang lemah akan menyebabkan hipotesis tidak dapat diuji dan klaim kontribusi menjadi overclaim.

Setelah Anda menyelesaikan latihan ini, proses penyempiran masalah akan kami lanjutkan ke konteks aplikasi nyata. Pada slide berikutnya, kita akan membedah studi kasus lengkap mengenai integrasi diffusion model dalam pencitraan medis, mulai dari identifikasi literatur, perumusan RQ utama dan turunan, hingga penyiapan protokol eksperimen yang siap divalidasi secara statistik.

---

## Slide 028 - Studi Kasus 1: Diffusion Model untuk Citra Medis

### Narasi

Mari kita terapkan kerangka kerja yang telah dipraktikkan pada latihan sebelumnya, di mana Anda mengubah topik penelitian yang masih bersifat umum menjadi pertanyaan penelitian yang terukur. Pada slide ini, kita akan melihat bagaimana proses tersebut dijalankan secara konkret melalui studi kasus di bidang pengolahan citra medis, khususnya pemanfaatan *diffusion model*.

Topik "diffusion model untuk citra medis" memang menarik, namun dalam konteks penelitian tingkat doktoral, pernyataan ini masih terlalu luas dan kurang tajam. Tinjauan literatur terkini menunjukkan bahwa meskipun arsitektur *diffusion* memberikan performa metrik seperti PSNR yang baik pada tugas restorasi, terdapat risiko signifikan berupa halusinasi detail pada area dengan rasio sinyal-terhadap-noise yang rendah. Celah penelitian atau *research gap* yang muncul adalah belum adanya metode yang secara eksplisit mengintegrasikan *constraint fisik* langsung ke dalam proses *sampling* denoising.

Dari identifikasi masalah dan *gap* tersebut, kita dapat merumuskan pertanyaan penelitian utama yang presisi. RQ utamanya adalah: sejauh mana penambahan *constraint konsistensi data* pada proses *denoising diffusion* mampu mengurangi detail halusinatif pada citra CT dosis rendah? Pertanyaan ini menuntut verifikasi empiris melalui desain eksperimen yang ketat dan terkontrol.

Selain RQ utama, diperlukan pula pertanyaan turunan untuk memetakan dinamika efek intervensi kita secara lebih granular. Salah satu RQ turunan yang relevan adalah pada tingkat noise berapakah pengaruh *constraint* tersebut mulai memberikan dampak yang signifikan terhadap kualitas restorasi? Pertanyaan turunan ini akan secara langsung mengarahkan pemilihan *noise schedule*, strategi augmentasi, serta titik-titik evaluasi metrik selama pelatihan dan inferensi model.

Dengan merumuskan RQ yang spesifik, berbasis literatur, dan terukur seperti ini, fondasi metodologis penelitian kita sudah terbangun dengan kokoh. Langkah logis berikutnya dalam alur penyusunan proposal disertasi adalah menerjemahkan RQ tersebut menjadi pernyataan hipotesis yang dapat diuji, serta mendefinisikan kontribusi ilmiah atau *novelty*-nya. Pembahasan mengenai perumusan hipotesis dan penentuan *novelty* akan kita lanjutkan pada slide berikutnya.

---

## Slide 029 - Studi Kasus 2: Hipotesis dan Novelty

### Narasi

Setelah merumuskan pertanyaan penelitian pada slide sebelumnya, langkah kritis berikutnya adalah menerjemahkan celah literatur tersebut menjadi hipotesis yang terukur dan spesifik. Pada studi kasus kedua ini, kita menguji apakah penambahan constraint konsistensi data pada setiap langkah reverse sampling benar-benar memberikan dampak positif yang dapat diobservasi secara kuantitatif.

Hipotesis yang diajukan menyatakan bahwa integrasi constraint tersebut akan menurunkan skor LPIPS sekaligus meningkatkan akurasi deteksi struktur anatomi berukuran kecil, apabila dibandingkan dengan baseline diffusion model tanpa constraint. Pernyataan ini dirancang agar variabel independen dan dependennya jelas, sehingga pengujian eksperimen dapat dilakukan secara objektif menggunakan metrik yang telah mapan dalam literatur restorasi citra medis.

Dari perspektif novelty, kontribusi utama penelitian terletak pada arsitektur metode yang menggabungkan prinsip fisika pencitraan CT dosis rendah ke dalam proses denoising diffusion. Selain itu, protokol evaluasi yang dikembangkan juga menjadi nilai tambah signifikan, karena secara eksplisit menilai tingkat detail halusinatif yang sering kali luput dari metrik tradisional seperti PSNR atau SSIM. Implikasi praktisnya sangat relevan bagi klinisi, khususnya radiolog, yang membutuhkan jaminan keandalan visual sebelum mengambil keputusan diagnostik.

Perlu dicatat bahwa desain ablasi untuk memisahkan pengaruh masing-masing komponen constraint akan dibahas lebih lanjut pada pertemuan berikutnya. Contoh studi kasus ini menegaskan bahwa sebuah research gap hanya bernilai ilmiah jika berhasil dikonversi menjadi hipotesis yang dapat diuji, terukur, dan memiliki signifikansi baik secara metodologis maupun aplikatif.

Transisi ke slide berikutnya akan membahas bagaimana merancang rangkaian eksperimen—mulai dari pemilihan baseline yang kuat, strategi ablasi, hingga kontrol seed dan dokumentasi konfigurasi—untuk memastikan bahwa pengujian hipotesis ini memenuhi standar rigoritas penelitian tingkat doktoral.

---

## Slide 030 - Merancang Eksperimen untuk Menguji Hipotesis

### Narasi

Setelah kita merumuskan hipotesis dan novelty pada studi kasus sebelumnya mengenai data-consistent diffusion untuk CT dosis rendah, langkah selanjutnya yang krusial adalah menerjemahkan klaim teoretis tersebut ke dalam desain eksperimen yang terukur. Slide ini membahas bagaimana menyusun rangkaian uji empiris yang secara langsung menjawab pertanyaan penelitian dan menguji validitas hipotesis yang telah kita susun.

Desain eksperimen yang rigor harus berpegang pada prinsip-prinsip metodologis yang telah dibahas sebelumnya. Pertama, pastikan baseline yang digunakan kuat dan representatif terhadap state-of-the-art terkini, bukan sekadar model sederhana yang mudah dikalahkan. Kedua, rancang uji ablation secara sistematis untuk mengisolasi kontribusi setiap komponen dalam arsitektur usulan. Ketiga, dokumentasikan konfigurasi lengkap termasuk pengaturan seed acak agar hasil dapat direproduksi oleh peneliti lain. Terakhir, jika variasi hasil signifikan, gunakan analisis statistik atau interval kepercayaan untuk memperkuat klaim performa, terutama pada jenjang doktoral di mana reproducible dan statistically sound menjadi standar utama.

Untuk memetakan kerangka pengujian, kita membagi eksperimen menjadi tiga kategori utama yang saling melengkapi:
- **Eksperimen A**: Evaluasi komparatif antara metode usulan dengan baseline kuat, bertujuan membuktikan adanya peningkatan metrik yang diharapkan.
- **Eksperimen B**: Uji ablasi pada komponen constraint konsistensi data, guna mengkuantifikasi kontribusi masing-masing blok arsitektur terhadap penurunan LPIPS dan peningkatan akurasi deteksi struktur halus.
- **Eksperimen C**: Uji robustness pada variasi tingkat noise atau distribusi data berbeda, untuk mengidentifikasi batas generalisasi dan potensi kegagalan model.

Perlu dicatat bahwa pertemuan ini hanya menargetkan penyusunan logika hipotesis dan rencana bukti pendukungnya. Detail teknis implementasi, penentuan hyperparameter, serta skrip pelatihan akan dibahas lebih lanjut pada pertemuan berikutnya. Dengan peta eksperimen yang jelas ini, kita siap melangkah ke tahap penentuan kriteria keberhasilan dan interpretasi hasil, baik berupa konfirmasi positif maupun temuan negatif yang tetap bernilai ilmiah.

---

## Slide 031 - Bukti yang Mengonfirmasi Hipotesis

### Narasi

Pada slide ini, kita membahas mekanisme penentuan bukti yang secara objektif dapat mengonfirmasi atau menolak hipotesis penelitian. Sebelum mengeksekusi eksperimen, Anda wajib menetapkan kriteria keberhasilan yang terukur, spesifik, dan dapat direplikasi. Sebagai contoh dalam konteks restorasi citra atau segmentasi medis, hipotesis dapat diformulasikan sebagai: "Hipotesis didukung jika skor LPIPS menurun minimal 0,02 dan akurasi deteksi struktur meningkat minimal 5% dibandingkan baseline, dengan varians performa yang stabil pada lima kali pengulangan menggunakan seed berbeda." Penetapan threshold eksplisit ini berfungsi sebagai guardrail metodologis untuk menghindari bias konfirmasi dan memastikan bahwa klaim kontribusi Anda berlandaskan data empiris yang robust.

Poin krusial lainnya adalah perlakuan terhadap hasil negatif. Dalam standar riset doktoral, temuan yang menunjukkan tidak adanya perbedaan signifikan atau bahkan degradasi performa bukanlah kegagalan, melainkan kontribusi ilmiah yang sah selama disertai analisis kausal yang mendalam. Apakah kegagalan tersebut disebabkan oleh mismatch arsitektur dengan karakteristik data, overfitting pada subset tertentu, atau batasan fundamental dari pendekatan yang digunakan? Mendokumentasikan skenario ini sejak fase perencanaan eksperimen akan memperkuat integritas penelitian. Hasil negatif yang terekstraksi dengan sistematis sering kali menjadi pintu masuk bagi identifikasi research gap baru atau pembatasan domain generalisasi model.

Kekuatan bukti secara langsung menentukan bobot klaim novelty Anda. Bukti yang lemah, seperti peningkatan marginal tanpa uji signifikansi statistik, kurangnya validasi silang, atau reproduksibilitas yang rendah, akan membatasi ruang lingkup kontribusi yang dapat Anda pertanggungjawabkan. Sebaliknya, bukti yang kuat, reproducible, dan divalidasi melalui multiple baselines serta ablation study akan memperkuat posisi novelty Anda terhadap state-of-the-art. Hal ini menjadi fondasi logis sebelum Anda mentranslasikan seluruh elemen penelitian ke dalam dokumen formal.

Sebagai kelanjutan dari perancangan eksperimen pada slide sebelumnya, penentuan kriteria keberhasilan dan interpretasi hasil negatif ini harus segera diintegrasikan ke dalam kerangka kerja tertulis. Slide berikutnya akan membahas struktur concept note, yaitu dokumen ringkas 3–5 halaman yang berfungsi sebagai blueprint awal untuk menguji kelayakan ide, memetakan hubungan antara research question, hipotesis, novelty, dan bukti yang diharapkan, sebelum memasuki tahap implementasi teknis yang lebih mendetail pada pertemuan selanjutnya.

---

## Slide 032 - Concept Note: Struktur dan Komponen

### Narasi

Setelah kita membahas penentuan kriteria keberhasilan hipotesis serta pentingnya menafsirkan hasil negatif maupun positif pada slide sebelumnya, langkah logis berikutnya adalah mengonsolidasikan seluruh elemen metodologis tersebut ke dalam satu dokumen ringkas. Dokumen ini disebut sebagai Concept Note. Pada tingkat doktoral, concept note berperan sebagai alat uji kelayakan ide yang memampukan peneliti memetakan alur pikir sebelum mengembangkan proposal disertasi yang lebih kompleks.

Struktur concept note yang solid harus mencakup sembilan komponen berikut secara berurutan:
1. Judul kerja yang merefleksikan fokus utama.
2. Problem statement yang mendefinisikan celah praktis atau teoretis.
3. Research gap dan positioning terhadap state-of-the-art terkini.
4. Research question utama beserta turunan operasionalnya.
5. Hipotesis yang dapat diuji secara empiris.
6. Pernyataan novelty dan kontribusi ilmiah yang diharapkan.
7. Signifikansi penelitian bagi komunitas akademik maupun aplikasi teknis.
8. Rancangan eksperimen awal yang feasible.
9. Daftar pustaka pendek sebagai landasan literatur.

Terkait panjang dokumen, concept note tidak perlu ditulis setebal proposal lengkap. Target idealnya adalah tiga hingga lima halaman. Batasan ruang ini justru menjadi kekuatan karena menuntut presisi argumentasi, eliminasi redudansi, dan kejelasan visi penelitian. Dengan format yang padat, concept note akan lebih efektif memicu diskusi kritis dan iterasi cepat bersama dosen pembimbing maupun peer reviewer.

Ketika sembilan komponen ini telah dirumuskan dengan matang, concept note siap digunakan sebagai dasar evaluasi kelayakan. Namun, klaim novelty dan positioning terhadap SOTA yang Anda tulis harus didukung oleh bukti literatur yang terstruktur dan transparan. Oleh karena itu, pada slide berikutnya kita akan mempelajari teknik penyusunan peta literatur dan tabel perbandingan state-of-the-art, yang berfungsi sebagai verifikasi objektif atas claim kontribusi penelitian Anda sebelum masuk ke tahap implementasi eksperimental.

---

## Slide 033 - Peta Literatur dan Tabel Perbandingan SOTA

### Narasi

Setelah merumuskan struktur concept note pada slide sebelumnya, langkah kritis berikutnya adalah memvalidasi ide penelitian melalui peta literatur dan tabel perbandingan state-of-the-art. Pada jenjang doktoral, tinjauan pustaka tidak boleh bersifat deskriptif atau sekadar rangkuman paper. Anda harus melakukan sintesis kritis untuk memetakan lanskap metodologis yang ada, sehingga posisi penelitian Anda dapat dibuktikan secara empiris dan teoretis.

Proses pembuatan peta literatur dimulai dengan mengumpulkan 15 hingga 30 publikasi terkini yang benar-benar relevan dengan topik Anda, utamakan dari venue bereputasi seperti CVPR, ICCV, ECCV, TPAMI, atau jurnal Q1 di bidang computer vision. Setelah terkumpul, kelompokkan paper tersebut ke dalam tema utama berdasarkan pendekatan arsitektur, jenis data, atau formulasi masalahnya. Identifikasi cluster yang masih memiliki keterbatasan atau celah kinerja, lalu hubungkan langsung dengan peluang kontribusi yang Anda tawarkan.

Untuk mendokumentasikan analisis ini, gunakan tabel perbandingan SOTA yang sistematis. Kolom-kolom seperti Problem, Metode, Dataset, Hasil, dan Keterbatasan dipaksa untuk diisi secara objektif. Perhatikan contoh baris usulan Anda: kolom hasil dan keterbatasan sengaja dikosongkan karena merupakan prediksi awal yang akan divalidasi melalui eksperimen. Namun, pengisian ini justru melatih Anda untuk tidak overclaim novelty sebelum bukti eksperimental tersedia. Tabel ini juga berfungsi sebagai alat diagnostik untuk memastikan bahwa metode yang Anda rancang benar-benar menjawab kelemahan paper sebelumnya secara prinsipil.

Manfaat utama dari artefak ini adalah kejelasan posisi penelitian saat berdiskusi dengan dosen pembimbing maupun rekan sejawat. Peta literatur mempercepat deteksi duplikasi ide dan memperkuat argumen novelty berbasis bukti, bukan asumsi. Pada sesi kelas berikutnya, tabel ini akan menjadi bahan utama dalam aktivitas Research Clinic dan peer feedback, di mana setiap klaim novelty dan hipotesis akan diuji ketangguhannya melalui pertanyaan kritis terstruktur. Pastikan tabel Anda sudah siap diekspos agar diskusi dapat fokus pada substansi metodologis dan kelayakan kontribusi ilmiah Anda.

---

## Slide 034 - Aktivitas Kelas: Research Clinic dan Peer Feedback

### Narasi

Setelah kita menyelesaikan peta literatur dan menyusun tabel perbandingan state-of-the-art pada slide sebelumnya, kini saatnya menguji validitas formulasi riset Anda melalui interaksi akademis langsung. Slide ini memperkenalkan mekanisme Research Clinic yang dirancang khusus untuk standar doktoral, di mana setiap ide konsep tidak hanya dinilai dari kelengkapan teknis, tetapi juga dari ketajaman argumen ilmiah dan kesiapan menghadapi kritik peer-review.

Alur kegiatan ini bersifat individual dan sangat terstruktur. Setiap mahasiswa menyampaikan ide konsep penelitiannya dalam batas waktu lima menit. Selama presentasi singkat tersebut, dosen dan rekan mahasiswa akan mengajukan pertanyaan kritis yang menguji fondasi penelitian Anda. Fokus diskusi diarahkan pada lima pilar utama: identifikasi research gap, kejelasan research question, kelayakan hipotesis, klaim novelty, serta signifikansi kontribusi. Seluruh masukan yang diterima wajib dicatat secara sistematis sebagai bahan revisi intensif sebelum memasuki tahap penulisan concept note.

Untuk menjaga objektivitas dan kedalaman evaluasi, kita menerapkan rubrik feedback berbasis pertanyaan pemantik berikut:
- **Problem statement**: Apakah konsekuensi masalah telah dijelaskan secara jelas dan berdampak nyata?
- **Research question**: Apakah pertanyaan penelitian dapat dijawab dengan metode, data, atau komputasi yang benar-benar Anda kuasai?
- **Hipotesis**: Apakah prediksi yang diajukan memenuhi syarat falsifiability, sehingga dapat diuji dan berpotensi dibuktikan salah?
- **Novelty**: Apakah pendekatan Anda berbeda secara prinsipil dari metode SOTA, bukan sekadar perbaikan marginal?
- **Significance**: Siapa yang akan diuntungkan secara ilmiah maupun praktis, dan bagaimana manfaat tersebut tersalurkan?

Prinsip peer feedback menjadi pondasi budaya akademik dalam sesi ini. Kritik harus selalu ditujukan pada struktur argumen, desain eksperimen, atau interpretasi hasil, bukan pada pribadi penyaji. Selain mengidentifikasi celah, setiap peserta diharapkan menawarkan alternatif metodologis atau perspektif analisis baru. Sikap konstruktif ini akan melatih ketahanan intelektual Anda dalam menyikapi review eksternal di konferensi atau jurnal bereputasi.

Hasil diskusi clinic ini akan langsung dioperasionalkan ke instrumen evaluasi pada slide berikutnya. Sebelum mengumpulkan concept note, gunakan checklist kualitas formulasi riset untuk melakukan self-audit menyeluruh. Penekanan pada konsistensi logis antar elemen—mulai dari gap, RQ, hipotesis, bukti, hingga positioning—akan menjadi penentu utama kelayakan dan potensi publikasi internasional dari proposal disertasi Anda.

---

## Slide 035 - Checklist Kualitas Formulasi Riset

### Narasi

Setelah kita menguji konsep penelitian melalui sesi *Research Clinic* dan rubrik umpan balik pada slide sebelumnya, langkah selanjutnya adalah memvalidasi setiap elemen formulasi riset secara sistematis. Slide ini menyajikan *Checklist Kualitas Formulasi Riset*, yang berfungsi sebagai standar kritis sebelum Anda menyerahkan *Concept Note*.

Berikut adalah parameter kualitas yang harus Anda verifikasi satu per satu:
- **Problem statement**: Harus spesifik, berbasis kebutuhan nyata, dan menjelaskan konsekuensi jelas jika masalah tidak ditangani.
- **Research gap**: Tidak cukup hanya menyatakan "belum ada"; diperlukan alasan metodologis atau teoretis yang kuat.
- **Research question**: Dirumuskan secara spesifik, terukur, dan benar-benar dapat dijawab dengan pendekatan yang tersedia.
- **Hipotesis**: Mengandung prediksi yang eksplisit, dapat diuji, dan bersifat *falsifiable* (dapat disangkal).
- **Novelty**: Jelas terlihat perbedaannya dengan *State-of-the-Art* (SOTA) secara prinsip, bukan sekadar peningkatan marginal.
- **Contribution statement**: Dinyatakan dalam satu kalimat utama yang kuat dan terukur dampaknya.
- **Significance**: Menyebutkan secara eksplisit manfaat ilmiah maupun praktis bagi komunitas peneliti atau aplikasi industri.
- **Positioning**: Dilengkapi dengan tabel atau analisis komparatif yang menempatkan penelitian Anda relatif terhadap metode SOTA.
- **Konsistensi**: Memastikan alur logis dari gap, RQ, hipotesis, bukti empiris, hingga klaim kontribusi saling terhubung tanpa jeda.

Checklist ini tidak boleh dipandang sebagai formalitas administratif semata. Gunakan sebagai panduan aktif selama proses penulisan, dan jadikan sebagai bahan diskusi substantif dalam *Research Clinic*. Jika terdapat ketidaksesuaian antar-elemen, revisi segera sebelum memasuki tahap implementasi.

Dengan formulasi yang telah terstandarisasi, fondasi teoritis dan konseptual penelitian Anda sudah matang. Kita akhiri pertemuan ini dengan kata-kata penutup, dan pada sesi berikutnya, kita akan langsung beralih ke topik Metodologi Disertasi dan Rancangan Eksperimen, di mana seluruh elemen checklist ini akan diterjemahkan menjadi protokol penelitian, desain studi, dan strategi evaluasi yang rigor.

---

## Slide 036 - Penutup

### Narasi

Kita kini berada di tahap penutup pembahasan mengenai formulasi Research Question, Hipotesis, dan Novelty. Pada slide sebelumnya, kita telah menyusun checklist kualitas yang berfungsi sebagai filter kritis sebelum Anda mengumpulkan concept note. Pastikan setiap komponen dalam draf riset Anda saling mengunci secara logis: problem statement yang spesifik harus mengarah pada research gap yang terjustifikasi secara literatur, yang kemudian diterjemahkan menjadi research question yang terukur. Hipotesis harus menyajikan prediksi yang dapat diuji secara empiris, sementara novelty harus jelas dibedakan dari state-of-the-art yang ada. Konsistensi antar elemen ini bukan sekadar formalitas penulisan, melainkan fondasi metodologis yang menentukan kelayakan penelitian tingkat doktoral.

Pada jenjang S3, novelty tidak selalu menuntut penemuan arsitektur atau algoritma yang benar-benar baru dari nol. Seringkali, kontribusi ilmiah yang signifikan justru terletak pada penyempurnaan metodologi eksisting untuk domain spesifik, integrasi teknik self-supervised learning ke dalam pipeline restorasi citra medis, atau adaptasi prinsip alignment vision-language untuk segmentasi objek dengan label terbatas. Statement kontribusi Anda harus mampu merangkum nilai tambah tersebut dalam satu kalimat yang tajam, terukur, dan mudah diposisikan terhadap karya-karya terkini. Gunakan checklist terakhir ini untuk melakukan stress-test terhadap alur logika riset Anda sebelum masuk ke fase implementasi.

Sebagai penutup sesi ini, saya mengajak Anda untuk merefleksikan kembali arah penelitian yang sedang Anda kerjakan. Jika terdapat celah antara gap yang diidentifikasi dengan hipotesis yang diajukan, luangkan waktu untuk menyelaraskannya melalui pemetaan literatur yang lebih mendalam atau diskusi dengan pembimbing. Ketajaman formulasi riset akan sangat menentukan efisiensi eksperimen dan kredibilitas temuan Anda di kemudian hari.

Terima kasih atas partisipasi dan kedalaman analisis yang telah ditampilkan selama pertemuan ini. Pada pertemuan berikutnya, kita akan beralih ke fase operasionalisasi dengan membahas Metodologi Disertasi dan Rancangan Eksperimen. Kita akan menguraikan bagaimana menerjemahkan research question yang telah diformulasikan menjadi desain eksperimen yang sistematis, mencakup strategi kurasi dataset, pemilihan baseline yang relevan, definisi metrik evaluasi yang tepat, perencanaan ablation study, serta validasi statistik untuk memastikan hasil yang reproduktibel dan siap bersaing di level publikasi internasional.
