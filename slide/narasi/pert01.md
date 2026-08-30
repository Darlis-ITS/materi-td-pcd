# Narasi TD Pengolahan Citra Digital - Pertemuan 01

## Peta Riset Mutakhir Pengolahan Citra Digital

Sumber: markdown/pert01-peta-riset-mutakhir-pengolahan-citra-digital.md

---

## Slide 000 - Cover

### Narasi

Slide ini berfungsi sebagai pembuka visual yang menegaskan tema inti perkuliahan: peta riset mutakhir dan evolusi disiplin ilmu pengolah citra digital. Fokus utamanya terletak pada judul besar dan subjudul yang secara eksplisit menandai pergeseran paradigma dari representasi piksel tradisional menuju foundation models dan agenda riset computer vision kontemporer. Pada jenjang doktoral, penekanan ini bukan sekadar kronologi sejarah, melainkan landasan analitis untuk memahami mengapa metode tertentu masih relevan, kapan pendekatan klasik mulai jenuh, dan di mana peluang kontribusi ilmiah baru dapat dikembangkan.

Evolusi yang disebutkan dalam subjudul mencakup empat tahap representasi visual yang akan menjadi lensa analisis kita: piksel, handcrafted feature, learned feature, hingga embedding semantik. Pemahaman terhadap transisi ini penting karena menentukan bagaimana kita memilih arsitektur, mendesain loss function, dan mengevaluasi generalisasi model pada data yang kompleks. Slide ini sekaligus menyiapkan mentalitas akademik bahwa setiap teknik yang dibahas ke depan harus dikaitkan dengan posisi metodologisnya dalam peta riset global.

Sebagai titik awal perkuliahan, slide ini mengarahkan perhatian pada arah pembelajaran yang berorientasi pada produksi pengetahuan asli. Mahasiswa akan dilatih untuk tidak hanya mengonsumsi literatur, tetapi juga memetakan area riset aktif, mengidentifikasi research gap, dan merumuskan hipotesis yang teruji secara empiris. Kesiapan komputasi melalui ekosistem Python, Jupyter, Google Colab, serta library standar seperti NumPy, OpenCV, scikit-image, dan Matplotlib akan segera diaktifkan untuk mendukung eksplorasi data dan replikasi baseline secara mandiri.

Pembahasan selanjutnya akan langsung menjabarkan kompetensi spesifik yang harus dikuasai, termasuk cara mengaitkan peta riset dengan minat disertasi awal, interpretasi hasil eksperimen sebagai bahan penyusunan asumsi dan risiko penelitian, serta strategi positioning terhadap state-of-the-art. Seluruh komponen ini dirancang agar pada akhir semester, mahasiswa mampu menghasilkan proposal awal disertasi yang matang, memiliki novelty yang jelas, dan siap dikembangkan menjadi publikasi internasional bereputasi.

---

## Slide 001 - Tujuan Pembelajaran dan Arah Perkuliahan

### Narasi

Slide ini menetapkan tujuan pembelajaran dan arah strategis perkuliahan Topik Dalam Pengolahan Citra Digital pada jenjang doktor. Fokus utamanya adalah membangun fondasi konseptual sekaligus kesiapan metodologis yang diperlukan untuk menavigasi lanskap riset computer vision yang berkembang sangat cepat dan kompetitif.

Setelah mengikuti pertemuan ini, mahasiswa diharapkan mampu menguasai sejumlah kompetensi inti yang terbagi dalam tiga ranah utama:

- **Pemahaman Konseptual:** Menjelaskan evolusi pengolahan citra digital dari pendekatan klasik hingga foundation models, serta menguraikan pergeseran representasi visual dari piksel, handcrafted feature, learned feature, hingga embedding modern yang mendominasi arsitektur terkini.
- **Keterampilan Praktis:** Menggunakan ekosistem Python seperti NumPy, OpenCV, scikit-image, scikit-learn, dan Matplotlib untuk melakukan eksplorasi awal terhadap dataset citra serta mereproduksi baseline penelitian sederhana secara mandiri.
- **Orientasi Penelitian:** Memahami peran fundamental baseline dalam evaluasi model computer vision, memetakan area riset mutakhir, dan mengaitkan peta tersebut dengan minat awal disertasi. Hasil eksperimen awal tidak hanya dilihat sebagai metrik performa, tetapi diinterpretasikan sebagai bahan baku untuk merumuskan asumsi kerja, mengidentifikasi risiko teknis, dan menyusun pertanyaan penelitian yang tajam.

Pertemuan ini berfungsi sebagai pintu masuk yang terstruktur menuju seluruh rangkaian perkuliahan. Seluruh aktivitas di sini dirancang untuk mengarahkan Anda pada target akhir mata kuliah, yaitu penyusunan proposal awal disertasi yang relevan dengan state-of-the-art dan memiliki positioning ilmiah yang jelas. Untuk memahami bagaimana kompetensi-kompetensi ini terintegrasi dalam alur akademik, kita akan melihat posisi pertemuan ini dalam kerangka Rencana Pembelajaran Semester pada slide berikutnya.

---

## Slide 002 - Posisi Pertemuan 01 dalam RPS

### Narasi

Slide ini memetakan posisi materi hari ini dalam kerangka besar Rencana Pembelajaran Semester (RPS). Mata kuliah ini disusun sebagai perjalanan terstruktur yang membawa mahasiswa dari tahap pemetaan konseptual menuju penyusunan proposal disertasi yang siap uji. Struktur pembelajaran dibagi menjadi empat fase utama yang saling berkesinambungan:

- **Fase Pemetaan Riset (Pertemuan 1–2):** Membangun fondasi melalui pemahaman evolusi bidang, pemetaan area riset mutakhir, praktik *critical paper reading*, dan identifikasi *research gap*.
- **Fase Metode Mutakhir (Pertemuan 3–11):** Pendalaman teknis dan teoretis terhadap arsitektur CNN, Vision Transformer, *foundation models*, pendekatan multimodal, serta topik spesifik seperti *image restoration*, deteksi objek, segmentasi, model generatif, visi 3D, dan *trustworthy computer vision*.
- **Fase Desain Penelitian (Pertemuan 12–14):** Transisi dari pemahaman materi ke perancangan penelitian, mencakup perumusan pertanyaan penelitian, pemilihan metodologi, dan perencanaan eksperimen yang rigor.
- **Fase Proposal (Pertemuan 15–16):** Konsolidasi akhir melalui seminar proposal dan penyempurnaan rencana disertasi berdasarkan umpan balik akademis.

Poin krusial yang perlu ditekankan adalah bahwa materi ini tidak memiliki prasyarat teknis sebelumnya. Hal ini disengaja agar seluruh peserta dapat memulai dari titik nol yang setara, tanpa terbebani oleh beban pembelajaran sebelumnya. Fokus awal memang bukan pada implementasi kode atau penguasaan algoritma, melainkan pada pembangunan peta konseptual yang akurat. Pada pertemuan berikutnya, peta ini akan langsung ditransformasikan ke dalam praktik konkret melalui *critical paper reading* dan teknik sistematis untuk mengekstraksi celah penelitian yang layak dikembangkan menjadi kontribusi ilmiah baru.

Alur yang ditampilkan pada slide ini merupakan jembatan penghubung antara tujuan pembelajaran di awal perkuliahan dan aktivitas praktis yang akan dilakukan. Setelah memahami posisi dalam RPS, kita akan mengikuti lintasan logis yang dimulai dari konteks perkembangan bidang, berlanjut ke evolusi representasi visual dari piksel klasik hingga embedding modern, kemudian meninjau *classical pipeline* dan fungsi *baseline*. Dari sana, fokus bergeser ke pemetaan riset mutakhir, evaluasi benchmark, serta identifikasi masalah terbuka. Seluruh tahapan teoritis ini kemudian dioperasionalkan melalui eksplorasi dataset, pelaksanaan eksperimen *baseline*, analisis kegagalan (*failure analysis*), interpretasi hasil, dan akhirnya pencatatan dalam jurnal penelitian bersama formulasi pertanyaan awal.

Tujuan pedagogis dari alur ini sangat jelas: keberhasilan penelitian tingkat doktoral tidak diukur dari hafalan algoritma, melainkan dari kapasitas mahasiswa dalam menghubungkan keputusan metodologis dengan bukti empiris. Dengan memahami bagaimana setiap fase dalam RPS saling menopang, mahasiswa dapat mempersiapkan diri untuk terlibat aktif dalam konstruksi pengetahuan yang kritis, berbasis data, dan berorientasi pada inovasi ilmiah yang relevan dengan *state-of-the-art*.

---

## Slide 003 - Alur Pembelajaran Pertemuan 01

### Narasi

Pada pertemuan ini, kita akan mengikuti alur pembelajaran yang dirancang khusus untuk membangun pemahaman sistematis dari konteks bidang menuju eksperimen konkret. Seperti yang telah dipetakan pada slide sebelumnya, fase pemetaan riset menjadi fondasi utama sebelum kita masuk ke pembahasan metode mutakhir dan desain penelitian. Tidak ada prasyarat material yang menghambat, sehingga fokus kita sepenuhnya pada pembangunan kerangka berpikir penelitian sejak hari pertama.

Alur yang ditampilkan di sini bukan sekadar urutan topik, melainkan sebuah kerangka kerja metodologis yang terstruktur. Kita mulai dengan memetakan perkembangan bidang secara makro, kemudian menelusuri bagaimana representasi visual berevolusi dari teknik tradisional hingga pendekatan berbasis data. Dari sana, kita akan menguji classical pipeline dan establish baseline yang solid sebagai titik referensi komparatif.

Setelah memahami fondasi tersebut, peta riset mutakhir akan kita eksplorasi bersama. Di tahap ini, kita akan mengidentifikasi benchmark standar serta masalah terbuka yang masih belum terpecahkan. Langkah selanjutnya adalah eksplorasi dataset yang relevan, diikuti oleh implementasi baseline eksperimen sederhana menggunakan tools standar seperti Python, NumPy, OpenCV, atau PyTorch sesuai kebutuhan.

Fokus kritis muncul pada tahap failure analysis dan research interpretation. Di sinilah mahasiswa doktoral dilatih untuk tidak hanya melaporkan metrik, tetapi menganalisis mengapa suatu metode gagal atau berhasil, lalu menghubungkan temuan tersebut dengan posisi penelitian dalam lanskap ilmu pengetahuan. Seluruh proses ini akan didokumentasikan dalam research log yang berisi pertanyaan awal, asumsi, dan hipotesis kerja.

Tujuan utama dari alur ini jelas: kita tidak sedang mengejar penguasaan satu algoritma tertentu. Yang lebih penting adalah memahami bagaimana keputusan metodologis harus selalu didukung oleh bukti eksperimen, serta bagaimana setiap langkah menempatkan riset Anda secara strategis dalam peta bidang pengolahan citra digital. Pendekatan ini memastikan bahwa setiap klaim penelitian memiliki dasar empiris yang kuat dan dapat direproduksi.

Pemahaman terhadap evolusi bidang ini akan menjadi kunci ketika kita memasuki diskusi tentang alasan konseptual dan praktis mengapa transisi paradigma terjadi, apa yang tetap relevan, dan bagaimana Anda dapat menemukan celah penelitian yang bermakna. Hal ini akan kita bedah lebih lanjut pada slide berikutnya, di mana kita akan mengaitkan sejarah perkembangan teknik dengan strategi identifikasi research gap yang valid untuk tingkat doktoral.

---

## Slide 004 - Mengapa Perlu Memahami Evolusi Bidang Ini?

### Narasi

Slide ini menjawab pertanyaan mendasar mengapa pemahaman terhadap evolusi bidang Pengolahan Citra Digital menjadi fondasi krusial dalam penelitian doktoral. Sebagaimana alur pembelajaran pada slide sebelumnya menekankan, perjalanan riset dimulai dari pemetaan konteks bidang, berlanjut ke eksperimen, analisis kegagalan, dan akhirnya sintesis pertanyaan penelitian. Di tengah rangkaian tersebut, posisi metodologis Anda tidak dapat berdiri sendiri. Penelitian tingkat doktor menuntut kemampuan menempatkan setiap pilihan teknik dalam lanskap perkembangan ilmu yang lebih luas, sehingga keputusan eksperimen dapat dipertanggungjawabkan secara konseptual maupun empiris.

Secara konseptual, pemilihan metode dalam penelitian harus melampaui sekadar mengikuti tren terkini. Setiap pendekatan memiliki asumsi dasar, kekuatan, dan keterbatasan spesifik yang harus selaras dengan karakteristik data serta tujuan penelitian. Justru di sinilah letak peluang identifikasi *research gap* yang kuat. Seringkali, celah penelitian paling bermakna muncul tepat pada titik transisi antar-paradigma, ketika batasan metode lama mulai terlihat jelas namun belum sepenuhnya teratasi oleh pendekatan baru.

Dari sisi praktis, banyak tantangan *computer vision* modern masih berakar pada prinsip-prinsip klasik seperti pemfilteran citra dan rekayasa fitur. Hal ini menegaskan bahwa benchmark evaluasi yang sehat wajib menyertakan baseline yang masuk akal dan relevan. Model arsitektur yang semakin kompleks atau model besar berbasis *foundation* belum tentu memberikan peningkatan performa signifikan jika masalah yang dihadapi sebenarnya dapat diselesaikan secara efisien dengan metode yang lebih sederhana. Prinsip parsimoni tetap menjadi pedoman penting dalam desain eksperimen yang rigor.

Untuk mengarahkan analisis kritis Anda, terdapat tiga pertanyaan kunci yang perlu terus dipegang selama proses penelitian:
- Apa saja perubahan fundamental dari pendekatan klasik menuju *deep learning* dan *foundation models*?
- Aspek apa yang justru tetap bertahan dan relevan hingga kini?
- Di mana tepatnya posisi riset Anda berada dalam peta perkembangan bidang ini?

Pertanyaan-pertanyaan ini akan menjadi kompas saat kita memasuki fase eksplorasi dataset, pembuatan baseline, hingga analisis kegagalan (*failure analysis*) pada eksperimen selanjutnya. Pemahaman ini akan langsung diterjemahkan ke dalam gambaran historis dan teknis pada slide berikutnya, yang memetakan perjalanan paradigma pengolahan citra digital. Kita akan melihat bagaimana pergeseran dari era pemrosesan sinyal dan aturan manual menuju era representasi yang dipelajari secara otomatis mengubah cara informasi visual direpresentasikan, diekstraksi, dan dievaluasi. Transisi ini bukan sekadar pergantian algoritma, melainkan transformasi epistemologis dalam memahami data gambar itu sendiri.

---

## Slide 005 - Perjalanan Paradigma Pengolahan Citra

### Narasi

Slide ini memetakan evolusi metodologis dalam pengolahan citra digital, yang dapat dibagi menjadi dua fase dominan: era klasik dan era representasi yang dipelajari. Pada era klasik, pemrosesan citra bersifat deterministik dan sangat bergantung pada intervensi manusia. Pendekatan ini mengandalkan filtering manual, rekayasa fitur handcrafted, aturan heuristik, serta analisis statistik piksel untuk mengekstrak pola. Kekuatan utamanya terletak pada interpretabilitas dan efisiensi komputasi, namun kelemahannya tampak jelas saat menghadapi variasi iluminasi, deformasi, atau kompleksitas scene di dunia nyata.

Pergeseran ke era representasi yang dipelajari mengubah fondasi tersebut secara fundamental. Berikut adalah peta perkembangan metode yang mendominasi riset terkini:
- **CNN**: Menggantikan feature engineering dengan ekstraksi fitur hierarkis otomatis melalui konvolusi.
- **Vision Transformer**: Memperkenalkan mekanisme self-attention untuk menangkap dependensi global dan konteks jangka panjang.
- **Self-supervised Learning**: Mengurangi ketergantungan pada anotasi label dengan memanfaatkan struktur intrinsik data.
- **Foundation Models**: Menyediakan representasi serbaguna yang dapat diadaptasi ke berbagai downstream tasks tanpa fine-tuning masif.
- **Vision-Language Models**: Menyatukan modalitas visual dan linguistik untuk pemahaman kontekstual yang lebih kaya.
- **Generative Models & 3D Neural Rendering**: Membuka jalur sintesis konten realistis dan rekonstruksi spasial diferensial.
- **Trustworthy CV**: Memastikan transparansi, robustness, dan akuntabilitas keputusan model di lingkungan kritis.

Perubahan ini bukan sekadar pergantian algoritma atau peningkatan metrik akurasi secara marginal. Yang berubah secara esensial adalah cara informasi visual direpresentasikan, dipelajari, dan dievaluasi. Representasi tidak lagi dirancang secara eksplisit oleh peneliti, melainkan muncul dari optimisasi fungsi loss pada skala data besar. Evaluasi pun bergeser dari metrik pixel-wise tradisional seperti PSNR atau SSIM, menuju ukuran generalisasi lintas domain, alignment semantik, ketahanan terhadap adversarial perturbation, dan konsistensi temporal pada video.

Transisi ini secara langsung menjawab pertanyaan kunci dari slide sebelumnya mengenai apa yang berubah dari pendekatan klasik menuju deep learning dan foundation models. Meskipun arsitektur dan kerangka optimasinya berbeda, prinsip dasar pemrosesan sinyal, statistika ruang-waktu, dan teori sampling tetap relevan sebagai fondasi teoretis. Posisi riset doktoral Anda kini terletak pada kemampuan mengidentifikasi research gap di persimpangan transisi paradigma ini, misalnya bagaimana mengintegrasikan efisiensi filtering klasik dengan kapasitas representasi foundation models, atau bagaimana merancang protokol evaluasi yang lebih holistik untuk model vision-language dan generatif.

Pembahasan ini akan berlanjut pada slide berikutnya yang membedah secara eksplisit ranah pengolahan citra digital versus computer vision. Memahami batas dan sinergi kedua bidang tersebut krusial karena banyak kontribusi ilmiah mutakhir justru lahir di area intersection, di mana manipulasi sinyal tingkat rendah berfungsi sebagai komponen strategis dalam pipeline pemahaman visual tingkat tinggi, bukan sekadar tahap preprocessing konvensional.

---

## Slide 006 - Pengolahan Citra dan Computer Vision

### Narasi

Slide ini menegaskan batas konseptual sekaligus hubungan fungsional antara Pengolahan Citra Digital dan Computer Vision. Meskipun sering dibahas dalam satu payung disiplin, keduanya memiliki orientasi komputasi dan target keluaran yang berbeda secara fundamental.

Pengolahan Citra Digital memperlakukan gambar sebagai sinyal dua dimensi. Aktivitas utamanya berpusat pada manipulasi nilai piksel untuk meningkatkan kualitas representasi atau mengekstrak ciri tingkat rendah. Contoh konkretnya mencakup enhancement kontras, filtering frekuensi, restorasi citra terdegradasi, operasi morfologi, serta deteksi tepi. Dalam skema ini, input dan output umumnya berada pada domain visual atau statistik piksel, tanpa melibatkan penafsiran makna di baliknya.

Computer Vision melangkah lebih jauh ke ranah semantik. Bidang ini bertujuan memberikan interpretasi terhadap konten visual yang diamati. Output-nya bersifat deskriptif atau struktural: label kelas, bounding box, mask segmentasi, peta kedalaman, pose skeletal, atau caption berbasis bahasa. Fokusnya bergeser dari transformasi sinyal menuju pemahaman konteks, relasi spasial, dan pengambilan keputusan berbasis visi.

Perbedaan tersebut dapat dipetakan pada tiga dimensi kunci: jenis input, jenis output, dan fokus pemrosesan. Pengolahan citra mengutamakan stabilitas sinyal dan preservasi struktur lokal, sedangkan computer vision mengutamakan generalisasi representasi dan ketahanan terhadap variabilitas dunia nyata. Penting untuk dicatat bahwa dalam arsitektur computer vision modern, teknik pengolahan citra klasik tidak ditinggalkan, melainkan diintegrasikan sebagai modul pendukung dalam pipeline. Fungsi-nya kini lebih spesifik: normalisasi domain, reduksi noise adaptif, augmentasi terkontrol, atau pra-pemrosesan yang disesuaikan dengan karakteristik arsitektur neural.

Dari perspektif penelitian tingkat doktoral, pembedaan ini menjadi dasar strategis dalam mendesain metodologi. Ketika Anda mengembangkan framework baru, misalnya foundation model atau generative vision system, Anda perlu menentukan secara eksplisit bagian mana yang memerlukan penanganan manipulatif klasik dan bagian mana yang dapat dilatih end-to-end. Integrasi yang tepat antara prinsip fisika optik, statistik citra, dan pembelajaran representasi sering kali menghasilkan model yang lebih robust, efisien secara komputasi, dan mudah diinterpretasi.

Pemahaman pembagian peran ini menyiapkan landasan untuk meninjau kembali mengapa pendekatan klasik masih menjadi rujukan utama dalam evaluasi baseline dan analisis ablation study. Pada slide berikutnya, kita akan membedah bagaimana filtering dan feature engineering klasik tetap menjadi pilar metodologis, terutama ketika menangani data terbatas, kebutuhan interpretabilitas tinggi, atau pengembangan arsitektur hibrida klasik-modern.

---

## Slide 007 - Fondasi Klasik: Filtering dan Feature Engineering

### Narasi

Slide ini membahas fondasi klasik dalam pengolahan citra digital, yang menjadi landasan historis sekaligus masih sangat relevan secara metodologis hingga tingkat penelitian doktoral. Pendekatan klasik menekankan manipulasi piksel secara eksplisit dan ekstraksi fitur buatan tangan (*handcrafted features*). Berbeda dengan paradigma pembelajaran mendalam yang mempelajari representasi secara otomatis dari data, metode klasik mengandalkan pengetahuan ahli domain untuk merancang operasi matematis, statistik, dan geometris tertentu.

### Filtering

- Perbaikan kontras untuk mengoptimalkan dinamika intensitas piksel agar informasi tersembunyi lebih terbaca.
- Penghalusan dan penajaman guna mengontrol detail spasial serta menyeimbangkan antara reduksi noise dan preservasi tepian.
- Reduksi noise yang sering menjadi langkah preprocessing wajib sebelum analisis kuantitatif lebih lanjut.
- Deteksi tepi menggunakan operator turunan parsial atau kernel khusus untuk mengisolasi perubahan intensitas tajam.
- Transformasi morfologi yang memanipulasi struktur geometris citra berdasarkan operasi himpunan dan erosi/dilasi.

### Feature Engineering

- Fitur warna yang mengeksploitasi distribusi spektral pada ruang RGB, HSV, atau LAB.
- Fitur tekstur yang menangkap pola repetitif dan heterogenitas permukaan melalui analisis ko-okurensi atau filter Gabor.
- Fitur bentuk yang mendeskripsikan kontur, luas, keliling, dan momen geometris objek.
- Fitur lokal berbasis gradien seperti HOG atau LBP yang merepresentasikan orientasi perubahan intensitas di sekitar titik kunci.

Meskipun era *deep learning* telah mendominasi literatur terkini, fondasi klasik tetap memiliki nilai strategis yang tinggi. Metode ini sangat efektif untuk memahami struktur citra secara fundamental, membangun *baseline* yang ketat dalam evaluasi eksperimen, serta menangani skenario dengan data terbatas atau kendala komputasi. Selain itu, pipeline berbasis metode klasik menawarkan tingkat interpretabilitas yang jauh lebih transparan dibandingkan model *black-box*. Tren riset mutakhir juga menunjukkan kebangkitan pendekatan hibrida yang mengintegrasikan filtering klasik sebagai lapisan regularisasi atau mekanisme attention dalam arsitektur neural.

Pembahasan ini melanjutkan diskursus dari slide sebelumnya yang memetakan perbedaan mendasar antara pengolahan citra dan computer vision, dengan menegaskan bahwa manipulasi sinyal visual adalah lapisan pertama dalam pipeline pemrosesan data. Namun, sebagaimana akan diuraikan pada slide berikutnya, keterbatasan inheren dari pendekatan klasik—terutama terkait generalisasi, variasi kondisi akuisisi, dan ketidakmampuan optimasi end-to-end—menjadi katalisator transisi menuju paradigma pembelajaran representasi. Kritisisme terhadap fondasi ini justru menjadi prasyarat esensial bagi perumusan pertanyaan penelitian doktor yang inovatif dan berkontribusi pada state-of-the-art.

---

## Slide 008 - Keterbatasan Pendekatan Klasik

### Narasi

Setelah menelaah fondasi klasik yang mengandalkan filtering dan rekayasa fitur buatan tangan pada slide sebelumnya, kita perlu mengevaluasi secara kritis mengapa pendekatan tersebut mulai menghadapi batas teoretis dan praktisnya. Pada tingkat doktoral, pemahaman ini menjadi landasan penting untuk mengidentifikasi research gap dan merumuskan pertanyaan penelitian yang selaras dengan perkembangan metodologi terkini.

Keterbatasan utama metode klasik terletak pada ketidakmampuannya dalam melakukan generalisasi lintas domain. Fitur yang dirancang secara manual sangat bergantung pada keahlian peneliti dalam memahami karakteristik visual spesifik suatu masalah. Ketika dihadapkan pada variasi ekstrem seperti perubahan sudut pandang, pencahayaan, skala, rotasi, atau oklusi parsial, kinerja sistem klasik cenderung menurun signifikan. Selain itu, kompleksitas objek dan konteks visual yang semakin tinggi membuat proses feature engineering menjadi tidak efisien dan sulit diskalakan. Struktur pipeline yang terpisah-pisah juga menghalangi optimasi end-to-end, sehingga kesalahan pada tahap pra-pemrosesan akan terakumulasi dan menurunkan akurasi akhir secara keseluruhan.

Namun, keberadaan keterbatasan ini tidak serta-merta meniadakan nilai ilmiah metode klasik. Justru dalam ekosistem penelitian tingkat lanjut, baseline klasik berfungsi sebagai titik referensi esensial untuk mengukur signifikansi kemajuan metodologis. Pertanyaan mendasar yang harus selalu dijawab oleh setiap peneliti adalah apakah penambahan kompleksitas arsitektur modern benar-benar menghasilkan peningkatan performa yang bermakna, atau hanya menambah overhead komputasi tanpa kontribusi substantif terhadap kualitas representasi. Evaluasi kritis semacam ini menjadi inti dari praktik penelitian computer vision yang rigor dan berbasis bukti.

Pergeseran paradigma yang dipicu oleh batasan tersebut membawa kita pada transisi fundamental: dari manipulasi piksel dan fitur statis menuju pembelajaran representasi yang adaptif dan kontekstual. Slide berikutnya akan membahas bagaimana citra dapat direpresentasikan pada berbagai tingkat abstraksi, mulai dari intensitas piksel mentah hingga embedding semantik dan representasi multimodal. Fokus analisis akan bergeser ke pencarian representasi yang invarian terhadap variasi noise, diskriminatif, efisien, dan mampu generalisasi—sebuah tujuan yang menjadi pendorong utama evolusi deep learning, self-supervised learning, serta foundation models yang akan kita eksplorasi lebih lanjut dalam perkuliahan ini.

---

## Slide 009 - Dari Piksel ke Representasi Visual

### Narasi

Slide ini membahas evolusi representasi visual dalam pengolahan citra digital, yang menjadi fondasi metodologis sebelum kita memasuki arsitektur deep learning, Vision Transformer, atau model multimodal. Pada tingkat doktoral, memahami bagaimana informasi visual ditransformasi dari data mentah menjadi bentuk yang dapat dioptimalkan oleh algoritma adalah langkah pertama dalam merumuskan desain eksperimen yang rigor.

Diagram hierarki yang ditampilkan menggambarkan jalur abstraksi bertingkat. Di lapisan paling bawah terdapat piksel sebagai data raw berdimensi tinggi. Dari sana, informasi dapat dipadatkan menjadi statistik intensitas atau warna, kemudian berevolusi menjadi fitur buatan tangan (*handcrafted features*), fitur yang dipelajari secara end-to-end (*learned features*), hingga mencapai embedding semantik dan representasi multimodal. Setiap transisi tingkat abstraksi ini membawa trade-off yang perlu dievaluasi secara kritis antara kapasitas diskriminatif, beban komputasi, dan potensi generalisasi.

Representasi piksel murni menyimpan nilai intensitas pada koordinat spasial tertentu. Meskipun strukturnya sederhana, pendekatan ini sangat sensitif terhadap variasi non-informatif seperti translasi, fluktuasi pencahayaan, noise akuisisi, perubahan skala, dan rotasi. Kondisi ini secara langsung memperkuat argumen pada slide sebelumnya, di mana ketergantungan pada pipeline terpisah dan feature engineering manual menjadi bottleneck utama yang mendorong pergeseran paradigma.

Oleh karena itu, representasi yang lebih robust harus dirancang untuk memenuhi empat properti esensial. Pertama, invariansi terhadap variasi yang tidak relevan dengan tugas inti. Kedua, karakter diskriminatif yang mampu memisahkan kategori atau objek dengan margin yang jelas. Ketiga, efisiensi dimensi untuk mencegah curse of dimensionality dan mempercepat konvergensi model. Keempat, kemampuan generalisasi lintas distribusi data dan kondisi lingkungan yang berbeda.

Pertanyaan penutup pada slide ini menjadi landasan kritis bagi setiap peneliti computer vision: representasi apa yang paling sesuai dengan masalah spesifik yang sedang diteliti? Jawaban atas pertanyaan ini akan menentukan arah pemilihan arsitektur, strategi augmentasi, dan metrik evaluasi. Pembahasan selanjutnya akan menguraikan bagaimana *handcrafted features* seperti histogram intensitas dan HOG diimplementasikan sebagai baseline representasi, sekaligus menyiapkan kerangka kerja untuk praktikum pertemuan ini yang akan menguji pengaruh pilihan representasi terhadap performa classifier.

---

## Slide 010 - Handcrafted Feature sebagai Representasi

### Narasi

Pada slide sebelumnya, kita telah membahas hierarki representasi visual yang berkembang dari piksel mentah hingga embedding multimodal. Pertanyaan kunci yang muncul adalah bagaimana memilih representasi yang tepat agar sistem dapat bersifat invarian terhadap variasi tidak relevan, diskriminatif, efisien, dan mampu menggeneralisasi dengan baik. Slide ini secara spesifik menyoroti salah satu fondasi penting dalam computer vision klasik, yaitu *handcrafted feature* sebagai representasi.

Pendekatan klasik mengandalkan perancangan fitur oleh manusia untuk menangkap karakteristik tertentu dari citra. Terdapat beberapa metode yang telah menjadi standar de facto dalam literatur. Histogram intensitas atau warna memberikan gambaran distribusi statistik tanpa memperhatikan informasi spasial. HOG (*Histogram of Oriented Gradients*) menangkap orientasi gradien sehingga sangat efektif untuk mendeteksi struktur bentuk dan siluet objek. LBP (*Local Binary Patterns*) berfokus pada pola tekstur lokal melalui perbandingan intensitas piksel tetangga, sementara SIFT (*Scale-Invariant Feature Transform*) menghasilkan *keypoint* dan deskriptor yang robust terhadap perubahan skala, rotasi, dan pencahayaan.

Keunggulan utama dari fitur buatan manusia terletak pada interpretabilitas yang tinggi, efisiensi komputasi yang ringan, serta kemampuannya memberikan hasil yang stabil bahkan ketika ketersediaan data pelatihan terbatas. Dalam konteks penelitian tingkat doktoral, fitur-fitur ini sering dijadikan *baseline* yang kuat untuk membandingkan kinerja model-model kompleks yang lebih baru. Namun, keterbatasan mendasarnya tetap ada karena representasi sepenuhnya ditentukan oleh pengetahuan dan asumsi manusia. Informasi yang tidak sengaja diabaikan selama proses perancangan akan hilang permanen, dan fitur ini umumnya kurang memadai untuk menangani tugas semantik yang rumit atau domain yang sangat bervariasi.

Untuk praktikum pertemuan ini, kita akan menerapkan dua representasi tersebut, yaitu histogram intensitas dan HOG, untuk mengamati secara langsung bagaimana perubahan representasi memengaruhi performa classifier. Hasil eksperimen ini akan menjadi bagian integral dari pemahaman kita tentang komponen-komponen dalam pipeline klasik. Sebagaimana akan dibahas pada slide berikutnya, ekstraksi fitur hanyalah satu modul dalam alur kerja yang lebih besar. Pipeline komputer visi klasik bersifat modular, mulai dari pra-pemrosesan, ekstraksi fitur, pembentukan vektor fitur, klasifikasi berbasis mesin pembelajaran, hingga evaluasi. Kinerja akhir sistem sangat bergantung pada sinergi antara kualitas data, ketajaman representasi, kesesuaian algoritma classifier, serta rigor dalam metrik evaluasi.

---

## Slide 011 - Pipeline Klasik: Dari Citra ke Prediksi

### Narasi

Setelah membahas karakteristik dan batasan fitur buatan manusia pada slide sebelumnya, kini kita akan melihat bagaimana fitur-fitur tersebut diintegrasikan ke dalam alur pemrosesan yang terstruktur. Pipeline computer vision klasik dirancang secara modular, memungkinkan setiap tahapan dipisahkan, dioptimalkan, dan dievaluasi secara independen. Struktur ini bukan sekadar warisan historis, melainkan kerangka kerja analitis yang sangat relevan untuk merancang eksperimen penelitian tingkat doktoral.

Mari kita telusuri diagram alur yang terdapat pada slide ini. Proses dimulai dari **Input Image**, dilanjutkan dengan **Preprocessing** untuk normalisasi, reduksi noise, atau penyesuaian skala. Tahap **Feature Extraction** kemudian mengubah sinyal visual menjadi representasi numerik yang terkompresi. Hasilnya berupa **Feature Vector** yang disuplai ke **Machine Learning Classifier** untuk menghasilkan **Prediction**. Siklus ditutup oleh **Evaluation** yang mengukur konsistensi dan generalisasi model. Setiap panah menunjukkan ketergantungan kausal antar-tahap, di mana degradasi pada satu modul akan berdampak langsung pada output akhir.

Sebagai ilustrasi konkret, slide menyajikan dua varian pipeline untuk tugas klasifikasi digit. Pada contoh pertama, citra digit diproses melalui **Intensity Histogram** untuk membentuk feature vector, lalu diklasifikasikan menggunakan **kNN**. Pada contoh kedua, citra yang sama diolah menggunakan **HOG**, menghasilkan vektor yang menangkap orientasi gradien, dan dipasangkan dengan **Linear SVM**. Perbedaan representasi secara fundamental mengubah ruang fitur yang diakses oleh classifier, yang pada akhirnya menentukan decision boundary, margin, dan kapasitas generalisasi model.

Kinerja keseluruhan pipeline ini bergantung pada empat pilar utama: kualitas **data**, ketajaman **representasi**, kesesuaian **classifier**, dan rigor dalam **evaluasi**. Di tingkat riset mutakhir, pemahaman mendalam tentang interaksi antar komponen ini menjadi prasyarat sebelum mengadopsi arsitektur end-to-end seperti CNN, Vision Transformer, atau foundation models. Tanpa dekomposisi modular yang jelas, peneliti kesulitan melakukan ablation study, mengisolasi bottleneck, atau merumuskan hipotesis yang dapat diuji secara empiris.

Dengan memahami dinamika pipeline klasik ini, kita siap menjawab pertanyaan metodologis yang lebih kritis. Bagaimana cara menetapkan standar pembandingan yang adil ketika memperkenalkan metode baru? Apakah peningkatan performa benar-benar berasal dari inovasi representasi, atau sekadar tuning hyperparameter? Diskusi mengenai hierarki baseline dan kriteria validasi ilmiah akan kita lanjutkan pada slide berikutnya.

---

## Slide 012 - Mengapa Baseline Penting dalam Penelitian?

### Narasi

Pada slide sebelumnya, kita telah menguraikan bagaimana pipeline computer vision klasik bekerja secara modular, mulai dari preprocessing citra, ekstraksi fitur manual, hingga klasifikasi menggunakan algoritma machine learning tradisional. Namun, dalam konteks penelitian tingkat doktoral, validitas sebuah metode tidak dapat dinilai secara isolatif. Di sinilah konsep baseline memegang peranan krusial sebagai fondasi evaluasi yang objektif dan terukur.

Baseline bukanlah sekadar model sederhana yang dipilih secara asal untuk dibandingkan. Ia berfungsi sebagai titik pembanding fundamental untuk memastikan bahwa kontribusi metode baru benar-benar signifikan secara empiris. Struktur hierarki yang ditampilkan menunjukkan eskalasi kompleksitas representasi dan komputasi:
- **Baseline 0 (Majority Class)**: Menetapkan batas bawah absolut performa tanpa pembelajaran pola.
- **Baseline 1 (Simple Feature + kNN)**: Menguji apakah pola dasar dalam data dapat ditangkap oleh jarak geometris sederhana.
- **Baseline 2 (Stronger Classical Feature + SVM)**: Mengevaluasi kekuatan representasi fitur handcrafted ketika digabungkan dengan pemisah linear/non-linear yang robust.
- **Modern Model (CNN / ViT / Foundation Model)**: Merepresentasikan state-of-the-art yang memanfaatkan representasi otomatis dan skala arsitektur besar.

Penggunaan baseline yang terstruktur memungkinkan peneliti menjawab pertanyaan-pertanyaan kritis sebelum mengklaim inovasi:
- Apakah dataset mengandung sinyal yang cukup kuat sehingga model apa pun mampu belajar?
- Seberapa sulit problem yang diteliti, atau apakah representasi sederhana sudah memadai?
- Apakah peningkatan performa berasal dari perbaikan representasi, penyesuaian classifier, augmentasi data, atau sekadar fine-tuning hyperparameter?
- Berapa besar biaya komputasi tambahan yang diperlukan untuk memperoleh kenaikan metrik evaluasi?

Pada jenjang doktor, klaim novelty dan kontribusi ilmiah harus selalu diuji terhadap baseline yang relevan, adil, dan merepresentasikan praktik terbaik terkini. Tanpa perbandingan yang ketat, risiko overclaiming atau underestimating complexity metodologi sangat tinggi. Dengan memahami pentingnya baseline, transisi menuju pendekatan modern menjadi lebih terukur dan berdasar. Hal ini membuka jalan bagi pembahasan berikutnya mengenai bagaimana deep learning mengubah paradigma representasi visual melalui pembelajaran bertingkat langsung dari data, yang akan kita bedah pada slide selanjutnya.

---

## Slide 013 - Deep Learning: Representasi yang Dipelajari dari Data

### Narasi

Setelah memahami mengapa baseline menjadi fondasi evaluasi yang kritis dalam penelitian, kita beralih ke bagaimana paradigma representasi visual telah berevolusi secara fundamental. Pada slide ini, kita membahas pergeseran besar yang dibawa oleh deep learning ke dalam computer vision, di mana fitur tidak lagi dirancang secara manual, melainkan dipelajari langsung dari data melalui proses optimisasi numerik.

Convolutional Neural Networks (CNN) memungkinkan pembelajaran fitur bertingkat secara otomatis melalui optimasi end-to-end antara representasi dan classifier. Lapisan awal jaringan cenderung menangkap pola geometris sederhana seperti tepi, gradien, dan tekstur. Seiring kedalaman arsitektur meningkat, representasi yang terbentuk semakin abstrak, mencakup bagian-bagian objek spesifik hingga makna semantik tingkat tinggi yang relevan dengan tugas klasifikasi atau deteksi.

Kemampuan transfer learning menjadi katalis utama yang mempercepat adopsi deep learning, terutama pada domain dengan keterbatasan data berlabel. Representasi yang telah dipelajari pada dataset skala besar dapat diadaptasi dengan fine-tuning atau feature extraction untuk tugas target. Akibatnya, peran peneliti bergeser secara signifikan dari sekadar merancang fitur menjadi fokus pada elemen-elemen strategis berikut:
- Perancangan dan kurasi data training;
- Pemilihan dan modifikasi arsitektur model;
- Formulasi objective function yang sesuai dengan karakteristik tugas;
- Strategi pembelajaran seperti scheduling, augmentation, dan regularisasi;
- Protokol evaluasi yang ketat, terukur, dan reproducible.

Meskipun CNN mendominasi evolusi awal deep learning untuk visi komputer, pendekatan berbasis attention mulai menunjukkan keunggulan dalam memodelkan dependensi jangka panjang antar wilayah citra tanpa batasan receptive field lokal. Hal ini membawa kita secara alami ke pembahasan mengenai Vision Transformer, yang menggeser asumsi inductif dari konvolusi menuju pemrosesan global melalui mekanisme self-attention. Detail arsitektur dan implementasinya akan kita bedah lebih lanjut pada pertemuan berikutnya, namun dampaknya terhadap kebutuhan komputasi, skema pretraining berskala besar, dan perdebatan CNN versus transformer sudah terasa sangat signifikan dalam peta riset mutakhir.

---

## Slide 014 - Attention dan Vision Transformer

### Narasi

Pada pertemuan sebelumnya, kita telah menelusuri bagaimana arsitektur deep learning berbasis CNN mampu mempelajari representasi fitur secara hierarkis, mulai dari pola geometris sederhana hingga abstraksi semantik tingkat tinggi. Pergeseran fokus peneliti dari rekayasa fitur manual menuju desain data, arsitektur, fungsi objektif, dan strategi optimasi membuka ruang bagi eksplorasi operator representasi alternatif. Di sinilah mekanisme attention dan Vision Transformer muncul sebagai respons terhadap keterbatasan konvolusi dalam menangkap dependensi jarak jauh secara efisien.

Vision Transformer mendemonstrasikan bahwa self-attention dapat berfungsi sebagai operator inti dalam pemrosesan visual. Pipeline arsitektur ini memulai dengan partisi citra menjadi grid patch yang seragam. Setiap patch kemudian diproyeksikan secara linear ke dalam ruang dimensi tertentu, membentuk token embedding. Karena proses flattening menghilangkan koordinat spasial asli, informasi posisi disuntikkan kembali melalui positional encoding agar model tetap peka terhadap tata letak gambar. Blok self-attention kemudian beroperasi pada seluruh token secara paralel, memungkinkan setiap patch berinteraksi langsung dengan patch lainnya tanpa batasan receptive field lokal. Mekanisme ini memberikan cakupan konteks global sejak lapisan pertama.

Adopsi prinsip ini melahirkan implikasi riset yang perlu dikaji secara kritis. Kebutuhan data dan kapasitas komputasi melonjak signifikan karena kompleksitas perhitungan attention tumbuh kuadratik terhadap jumlah patch, berbeda dengan CNN yang memiliki inductive bias kuat berupa translasi equivariance dan sparsity. Tanpa bias tersebut, Vision Transformer sangat bergantung pada pretraining berskala besar untuk mempelajari prior visual yang stabil. Selain itu, klaim superioritas CNN versus transformer tidak dapat divalidasi secara absolut melalui satu benchmark tunggal; hasil evaluasi sangat sensitif terhadap protokol augmentasi, skema regularisasi, dan metrik generalisasi lintas domain. Pertanyaan mendasarnya adalah kapan inductive bias justru menghambat adaptasi, dan kapan fleksibilitas global menjadi keunggulan kompetitif.

Transisi arsitektural ini secara alami mengarah pada tantangan infrastruktur dan etiket penelitian di era modern. Ketika model menuntut data masif dan komputasi intensif, ketersediaan anotasi manual menjadi bottleneck utama yang memperlambat iterasi riset. Kondisi inilah yang memicu perkembangan self-supervised learning dan fondasi model visual (*foundation vision models*) yang mengeksploitasi sinyal intrinsik dari data mentah. Pada slide berikutnya, kita akan membedah mekanisme contrastive learning, masked image modeling, dan teacher-student distillation yang mendasari model seperti DINO dan DINOv2, sekaligus menguji pertanyaan riset krusial mengenai stabilitas transfer, ketahanan terhadap distribution shift, dan batas-batas fine-tuning pada domain khusus.

---

## Slide 015 - Self-Supervised Learning dan Foundation Vision Models

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana arsitektur Vision Transformer mengganti operator konvolusi dengan mekanisme self-attention untuk memodelkan dependensi global antar patch citra. Namun, kinerja optimal ViT sangat bergantung pada dataset berlabel skala besar dan infrastruktur komputasi yang masif. Di sinilah muncul tantangan mendasar dalam computer vision kontemporer: ketersediaan anotasi manusia yang akurat, konsisten, dan relevan menjadi bottleneck utama yang menghambat skalabilitas model.

Self-supervised learning menjawab tantangan ini dengan membangun sinyal pembelajaran langsung dari struktur intrinsik data, tanpa ketergantungan pada label eksternal. Pendekatan ini umumnya direalisasikan melalui empat mekanisme utama:
- Contrastive learning memaksa model membedakan representasi augmented dari gambar yang sama versus gambar berbeda, sehingga mendorong diskriminasi fitur yang tajam.
- Masked image modeling meniru prinsip language modeling dengan menyembunyikan sebagian patch citra dan melatih encoder untuk merekonstruksi informasi yang hilang.
- Teacher-student learning memanfaatkan distilasi pengetahuan antara dua jaringan yang dilatih secara sinkron, di mana student belajar meniru output soft dari teacher tanpa label keras.
- Representation prediction fokus pada menjaga konsistensi embedding lintas augmentasi atau view, sehingga model menangkap invariansi semantik yang esensial.

Implementasi mutakhir seperti DINO dan DINOv2 membuktikan bahwa representasi visual berkualitas tinggi dapat dipelajari secara murni melalui self-distillation dan contrastive objectives. Model foundation ini tidak hanya menghasilkan embedding yang kaya semantik, tetapi juga secara emergent mengekstrak knowledge implisit seperti segmentasi semantic, deteksi objek, dan pemahaman geometri scene, semuanya tanpa satu pun anotasi bounding box atau mask.

Untuk level doktoral, pertanyaan riset yang tercantum pada slide ini bukan sekadar tinjauan teoretis, melainkan kerangka kerja eksperimental yang harus diuji secara kritis. Kita perlu menganalisis apa sebenarnya yang dipelajari oleh representasi tersebut di balik metrik evaluasi standar. Kapan strategi fine-tuning tradisional masih memberikan ROI terbaik, dan kapan harus beralih ke linear probing atau prompt adaptation? Bagaimana generalisasi model terhadap domain khusus seperti radiologi atau pemetaan satelit? Serta seberapa robust representasi tersebut menghadapi distribution shift akibat perubahan sensor, pencahayaan, atau konteks geografis. Jawaban atas pertanyaan-pertanyaan ini akan menjadi dasar positioning penelitian dan desain eksperimen disertasi Anda.

Representasi yang dihasilkan oleh foundation models berbasis self-supervised learning kemudian menjadi fondasi natural untuk evolusi arsitektural berikutnya. Ketika fitur visual telah memiliki struktur semantik yang stabil dan umum, integrasi dengan modalitas linguistik menjadi langkah logis selanjutnya. Hal ini membuka transisi langsung menuju multimodal vision-language models, yang akan kita bedah pada slide berikutnya, di mana penyelarasan cross-modal akan mengubah paradigma machine understanding dari isolasi pixel menjadi interpretasi kontekstual yang holistik.

---

## Slide 016 - Multimodal Vision-Language Models

### Narasi

Pada slide sebelumnya, kita telah menelaah bagaimana self-supervised learning dan foundation vision models seperti DINO berhasil mengekstrak representasi visual yang kuat tanpa bergantung pada anotasi manual. Langkah logis berikutnya dalam peta riset mutakhir adalah mengintegrasikan representasi tersebut ke dalam ruang semantik yang lebih kompleks, yaitu bahasa alami. Pergeseran paradigma ini menjadikan multimodal vision-language models sebagai salah satu area paling strategis untuk eksplorasi penelitian tingkat doktoral.

Pendekatan ini menggeser fokus pengolahan citra dari pemrosesan yang terisolasi menuju penyelarasan representasi visual dengan deskripsi linguistik. Integrasi ini memungkinkan model memahami konteks, nuansa, dan hubungan semantik yang jauh melampaui fitur piksel murni. Teknik-teknik inti yang mendominasi literatur terkini meliputi:
- Image-text contrastive learning, yang memaksa model untuk memaksimalkan korelasi antara pasangan gambar dan teks yang relevan sambil meminimalkan kesamaan dengan pasangan negatif.
- Zero-shot recognition, yang memanfaatkan keluwesan representasi multimodal untuk mengenali kategori baru tanpa proses fine-tuning tambahan.
- Image-text retrieval, yang menguji kemampuan model dalam menjembatani pencarian visual berdasarkan query linguistik dan sebaliknya.
- Prompt-based adaptation, yang mengubah cara model menerima input agar lebih adaptif terhadap downstream tasks.
- Multimodal reasoning, yang mengembangkan kemampuan inferensi bertahap melalui gabungan modalitas visual dan teks.

Model CLIP menjadi tonggak empiris yang membuktikan bahwa bahasa alami dapat difungsikan sebagai sinyal supervisi yang sangat efektif. Dengan memanfaatkan korpus gambar-teks berskala besar, CLIP menghasilkan embedding ruang bersama yang memungkinkan fleksibilitas tinggi dalam berbagai tugas computer vision. Kemampuan ini secara fundamental mengubah cara kita merancang arsitektur pembelajaran mesin, di mana batas antara supervised dan unsupervised learning semakin kabur.

Meskipun performanya impresif, masih terdapat sejumlah isu terbuka yang menuntut kajian kritis dan desain eksperimen yang rigor:
- Bias bahasa dan budaya yang tertanam dalam dataset pelatihan dapat memperkuat stereotip dan mengurangi fairness model.
- Sensitivitas ekstrem terhadap formulasi prompt masih menjadi tantangan metodologis yang menghambat replikasi hasil.
- Masalah domain shift menunjukkan penurunan performa signifikan ketika model diterapkan pada data di luar distribusi pelatihan.
- Reliabilitas pada domain khusus seperti medis, geospasial, atau industri manufaktur masih memerlukan mekanisme verifikasi dan grounding yang lebih ketat.

Kajian mendalam terhadap isu-isu di atas akan menjadi landasan bagi transisi ke topik berikutnya. Setelah memahami bagaimana model multimodal menyelaraskan dan menginterpretasi informasi, langkah selanjutnya dalam peta riset adalah memanfaatkan representasi tersebut untuk menghasilkan, memodifikasi, dan mensintesis konten visual secara terkendali. Hal ini akan mengarahkan diskusi kita langsung ke slide berikutnya mengenai generative vision dan diffusion models.

---

## Slide 017 - Generative Vision dan Diffusion Models

### Narasi

Paradigma pengolahan citra digital kini bergeser signifikan dari analisis pasif menuju generasi dan modifikasi visual secara aktif. Generative vision, yang didorong oleh perkembangan arsitektur diffusion models, telah membuka dimensi baru dalam riset computer vision. Model tidak lagi terbatas pada klasifikasi atau deteksi, melainkan mampu menghasilkan konten visual baru yang koheren berdasarkan kondisi input tertentu, seperti deskripsi teks, mask geometris, atau referensi gaya.

Aplikasi utama yang menjadi fokus kajian mutakhir meliputi:
- Text-to-image generation untuk sintesis konten berbasis prompt alami.
- Image editing dan inpainting untuk manipulasi lokal tanpa merusak semantik global.
- Super-resolution dan restoration guna memulihkan detail yang hilang akibat degradasi sensor atau kompresi.
- Synthetic data generation sebagai solusi atas kelangkaan data berlabel berkualitas tinggi.
- Controlled generation yang memungkinkan intervensi terarah pada atribut spesifik tanpa mengganggu struktur dasar gambar.

Dari perspektif penelitian tingkat doktoral, slide ini menyoroti empat pertanyaan penelitian kritis yang harus dijawab melalui desain eksperimen yang rigor:
- Apakah synthetic data benar-benar meningkatkan generalisasi model pada domain target, atau justru memperkuat bias struktural yang inheren dalam data pelatihan?
- Bagaimana metrik fidelity dan diversity dievaluasi secara objektif, mengingat evaluasi visual sering kali bersifat subjektif dan memerlukan triangulasi dengan perceptual loss, Fréchet Inception Distance, serta human-centered assessment?
- Bagaimana provenance dan lisensi konten generatif dikelola dalam ekosistem akademik dan industri?
- Aspek traceability, watermarking, dan compliance terhadap regulasi hak cipta menjadi pertimbangan metodologis yang tidak boleh diabaikan dalam pipeline penelitian.

Konteks ini terhubung erat dengan slide sebelumnya tentang Multimodal Vision-Language Models. Mekanisme conditioning pada diffusion models sangat bergantung pada representasi joint space yang diselaraskan oleh model seperti CLIP. Tanpa alignment visual-teks yang robust, kontrol atas proses generasi akan kehilangan presisi dan stabilitas. Selanjutnya, ketika kita melangkah ke slide berikutnya mengenai 3D Vision dan Neural Rendering, teknik restorasi dan generasi citra dapat diperluas ke domain spasial, misalnya untuk mengisi oklusi pada point cloud, merekonstruksi view yang hilang, atau menginisialisasi neural scene representation yang lebih konsisten secara geometris.

Untuk eksplorasi implementasi dan penulisan proposal disertasi, mahasiswa disarankan menelaah arsitektur DDPM, latent diffusion, serta mekanisme kontrol modern seperti ControlNet dan IP-Adapter. Evaluasi empiris terhadap dataset sintetis harus mempertimbangkan trade-off antara kualitas perceptual, keberagaman distribusi, dan dampak downstream task. Integrasi analisis etis, reproducibility, dan positioning terhadap state-of-the-art wajib menjadi fondasi metodologi penelitian tingkat doktoral dalam ranah generative vision.

---

## Slide 018 - 3D Vision dan Neural Rendering

### Narasi

Pada slide sebelumnya, kita telah menelaah bagaimana generative vision dan diffusion models menggeser fokus pengolahan citra dari analisis pasif menuju sintesis dan manipulasi konten visual. Evolusi natural dari kemampuan menghasilkan citra adalah memahami dan merekonstruksi struktur ruang yang mendasarinya. Slide ini akan membahas fondasi serta perkembangan mutakhir di bidang 3D Vision dan Neural Rendering sebagai respons terhadap kebutuhan representasi spasial yang lebih autentik.

Secara fundamental, setiap citra yang kita tangkap hanyalah proyeksi 2D dari dunia fisik 3D. Tantangan inti dalam computer vision modern adalah membalikkan proses proyeksi ini untuk memulihkan atau merepresentasikan struktur ruang secara akurat. Riset di area ini telah meninggalkan pendekatan geometri klasik yang kaku, dan beralih ke framework berbasis pembelajaran mendalam yang mampu menangkap non-linearitas scene secara end-to-end.

Pilar-pilar utama yang mendominasi peta riset terkini meliputi:
- **Depth Estimation**: Pergeseran dari metode supervised tradisional menuju arsitektur self-supervised yang memanfaatkan dataset skala besar, sering kali dikombinasikan dengan constraint fisika cahaya untuk menjaga konsistensi geometri.
- **Stereo dan Multi-View Geometry**: Pemanfaatan korelasi epipolar dan feature matching lintas pandangan, dengan tren terbaru mengintegrasikan attention mechanism untuk pencocokan semantik yang robust terhadap variasi iluminasi.
- **Point Cloud Processing**: Operasi langsung pada data titik 3D yang bersifat sparse dan irregular, didorong oleh arsitektur seperti PointNet++, Graph Neural Networks, dan transformer-based architectures yang menangani densitas variabel.
- **Neural Radiance Fields (NeRF)**: Paradigma representasi scene implicit yang memetakan koordinat spasial (x,y,z) dan vektor pandang ke nilai warna serta kerapatan volumetrik, memungkinkan novel view synthesis dengan kualitas fotorealistik.
- **Neural Scene Representation**: Generalisasi beyond NeRF, mencakup explicit-implicit hybrids, 3D Gaussian Splatting, dan deformable representations yang menyeimbangkan fidelity visual, kecepatan inferensi, dan skalabilitas memori.

Meskipun capaian teknisnya sangat pesat, masih terdapat sejumlah pertanyaan terbuka yang menjadi frontier penelitian tingkat doktor. Pertama, batasan teoretis apa yang membatasi informasi 3D yang dapat dipulihkan dari satu atau beberapa citra terbatas? Kajian ulang terhadap ill-posed nature problem ini diperlukan dalam konteks model neural modern. Kedua, bagaimana merancang mekanisme inference yang robust terhadap oklusi parsial maupun total tanpa mengorbankan konsistensi topologi global? Ketiga, representasi scene dinamis menuntut pemodelan perubahan temporal yang koheren, baik melalui parameter latent space maupun dekomposisi eksplisit komponen statis-dinamis. Keempat, integrasi antara geometric prior yang ketat dengan learned representation yang fleksibel masih menghadapi tantangan optimasi dan generalisasi cross-domain.

Penguasaan terhadap konsep-konsep ini bukan hanya relevan untuk membangun pipeline rekonstruksi 3D yang handal, tetapi juga menjadi landasan kritis untuk memahami bagaimana representasi spasial mempengaruhi downstream tasks seperti robotics navigation dan augmented reality. Setelah kita memahami bagaimana merepresentasikan dunia 3D secara neural, langkah selanjutnya adalah memastikan bahwa model-model tersebut dapat diandalkan secara etis dan teknis. Hal ini akan membawa kita secara alami ke pembahasan tentang Trustworthy Computer Vision pada slide berikutnya, di mana aspek explainability, predictive uncertainty, calibration, dan robustness terhadap distribution shift akan menjadi fokus utama.

---

## Slide 019 - Trustworthy Computer Vision

### Narasi

Akurasi numerik semata tidak lagi menjadi satu-satunya metrik keberhasilan dalam computer vision modern. Pada tingkat penelitian doktoral, kita harus mengakui bahwa model yang mencapai skor tinggi pada benchmark standar belum tentu dapat diandalkan dalam skenario dunia nyata atau lingkungan kritis. Transisi dari pendekatan yang hanya berorientasi pada performa menuju trustworthy AI menuntut evaluasi multidimensi yang melampaui akurasi klasifikasi atau IoU segmentasi.

Kerangka trustworthy computer vision mencakup enam pilar utama yang harus dipertimbangkan secara ketat dalam desain metodologi penelitian:
- **Explainability dan attribution**: Memastikan bahwa fitur atau region yang diaktivasi model benar-benar relevan dengan tugas visual, bukan sekadar korelasi artifisial.
- **Predictive uncertainty**: Mengkuantifikasi tingkat keyakinan model terhadap setiap prediksi, yang menjadi prasyarat untuk pengambilan keputusan berisiko tinggi.
- **Calibration**: Menjamin konsistensi antara probabilitas keluaran model dengan akurasi empirisnya, sehingga output dapat diinterpretasikan secara statistik.
- **Robustness**: Menguji ketahanan arsitektur terhadap adversarial perturbations, noise sensor, atau variasi iluminasi yang ekstrem.
- **Distribution shift**: Menilai degradasi performa ketika data inference menyimpang signifikan dari distribusi training, termasuk tantangan domain adaptation dan generalization.
- **Fairness dan bias**: Memverifikasi bahwa model tidak mendiskriminasi kelompok demografis, kategori objek, atau kondisi lingkungan tertentu secara sistematis.

Pertanyaan-pertanyaan kritis pada slide ini berfungsi sebagai landasan perumusan research question dan kerangka evaluasi eksperimental:
- Apakah mekanisme explanation yang dihasilkan benar-benar merepresentasikan alasan prediksi, atau hanya post-hoc approximation yang menyesatkan?
- Bagaimana perilaku model ketika dihadapkan pada data out-of-distribution yang tidak terlihat selama pelatihan?
- Apakah performa tetap konsisten dan stabil pada berbagai kelompok data atau kondisi operasional yang berbeda?
- Bagaimana ketidakpastian model dilaporkan dan dikomunikasikan kepada pengguna akhir tanpa menimbulkan overconfidence atau underutilization?

Pembahasan mengenai trustworthy computer vision ini melengkapi diskusi teknis sebelumnya yang mencakup kompleksitas representasi ruang pada 3D vision. Pada slide berikutnya, kita akan mengintegrasikan seluruh topik—termasuk aspek kepercayaan ini—ke dalam peta lanskap riset pengolahan citra secara menyeluruh. Peta tersebut dirancang bukan sebagai daftar teknologi statis, melainkan sebagai panduan strategis untuk membantu Anda menemukan lokasi kontribusi ilmiah potensial, memposisikan karya Anda terhadap state-of-the-art, dan menyusun experimental design yang memenuhi standar publikasi internasional bereputasi.

---

## Slide 020 - Peta Lanskap Riset Pengolahan Citra

### Narasi

Slide ini menyajikan peta lanskap riset pengolahan citra digital yang berfungsi sebagai panduan struktural untuk keseluruhan perkuliahan. Tabel di atas memetakan sembilan area penelitian inti ke dalam fokus bahasan teknis serta alokasi waktu dalam Rencana Pembelajaran Semester. Rentang topik dimulai dari representasi visual yang membahas transisi dari CNN konvensional hingga arsitektur attention-based, dilanjutkan dengan pembelajaran representasi yang berfokus pada self-supervised learning dan foundation models, kemudian berkembang ke arah model vision-language, restorasi citra, deteksi objek modern, segmentasi berbasis foundation model, model generatif diffusion, visi tiga dimensi, dan ditutup dengan trustworthy computer vision pada pertemuan terakhir.

Perlu ditegaskan bahwa peta ini bukanlah katalog teknologi atau daftar framework yang sedang viral. Tujuannya bersifat strategis: membantu mahasiswa doktoral mengidentifikasi *lokasi kontribusi ilmiah* yang potensial. Setiap baris dalam tabel mewakili ruang masalah yang masih terbuka, menuntut mahasiswa untuk melakukan pemetaan state-of-the-art secara kritis, menemukan research gap yang valid, dan merumuskan pertanyaan ilmiah yang memiliki nilai novelty serta dampak akademis yang jelas.

Kaitannya dengan slide sebelumnya, pembahasan tentang trustworthy computer vision pada Pertemuan 11 ditempatkan bukan sebagai topik tambahan, melainkan sebagai fondasi evaluasi akhir. Setelah mendalami berbagai arsitektur dan teknik ekstraksi fitur, mahasiswa diharapkan mampu menilai apakah model yang dibangun sudah memenuhi standar explainability, kalibrasi ketidakpastian, robustness terhadap distribution shift, serta fairness. Kepercayaan sistem tidak lagi bersifat opsional, melainkan menjadi metrik keberhasilan yang setara dengan performa akurasi itu sendiri.

Seiring kita memasuki setiap area dalam peta ini, akan terlihat bahwa satu masalah penelitian jarang sekali hanya memiliki satu solusi tunggal. Slide berikutnya akan mengilustrasikan bagaimana sebuah tugas klasifikasi citra sederhana dapat didekati melalui beragam paradigma, mulai dari pipeline tradisional berbasis histogram dan SVM, hingga end-to-end CNN, fine-tuning pretrained network, Vision Transformer, dan akhirnya foundation model vision-language. Diskusi tingkat doktoral pun tidak berhenti pada pertanyaan mana metode yang menghasilkan skor tertinggi, tetapi bergeser ke analisis fundamental: mengapa keunggulan itu muncul, pada kondisi distribusi data apa metode tersebut gagal, berapa biaya komputasi dan skalabilitasnya, serta apakah penambahan kompleksitas arsitektural benar-benar justified secara ilmiah.

---

## Slide 021 - Satu Problem, Banyak Paradigma

### Narasi

Peta lanskap riset yang telah kita paparkan sebelumnya berfungsi sebagai kompas arah untuk memetakan area-area potensial pengembangan ilmu. Namun, di balik pemetaan makro tersebut, terdapat prinsip metodologis yang harus menjadi landasan berpikir Anda sebagai peneliti tingkat doktoral: satu masalah penelitian dapat diselesaikan melalui berbagai paradigma komputasi yang berbeda.

Perhatikan struktur hierarki pada slide ini sebagai ilustrasi konkret untuk masalah klasifikasi citra. Pendekatan dapat dimulai dari representasi statistik sederhana seperti histogram intensitas atau warna yang dipasangkan dengan k-Nearest Neighbors. Selanjutnya, fitur terstruktur seperti HOG atau LBP sering dikombinasikan dengan Support Vector Machine untuk menangkap pola geometris dan tekstur lokal. Perkembangan arsitektur jaringan saraf kemudian memperkenalkan CNN end-to-end yang menghilangkan kebutuhan rekayasa fitur manual. Ketika ekosistem open-source berkembang, fine-tuning model CNN pra-latih menjadi praktik standar. Arsitektur Vision Transformer kemudian mengubah paradigma pemodelan dependensi spasial melalui mekanisme attention global. Pada puncak evolusi saat ini, foundation model dan vision-language model memungkinkan penyelesaian tugas melalui alignment semantik atau prompting tanpa pelatihan ulang yang intensif.

Pertanyaan doktoral yang harus Anda bangun tidak berhenti pada perbandingan metrik akurasi semata. Analisis kritis harus mencakup dimensi-dimensi berikut:
- Mengapa metode tertentu menunjukkan kinerja superior dalam konteks spesifik?
- Pada kondisi distribusi data atau noise apa keunggulan tersebut muncul atau justru menurun?
- Bagaimana trade-off antara biaya komputasi, kebutuhan memori, dan latency inference?
- Apakah peningkatan performa tetap konsisten ketika dilakukan transfer ke domain atau modalitas lain?
- Apakah kompleksitas arsitektural yang tinggi benar-benar esensial, ataukah pendekatan yang lebih parsimonius sudah memadai?

Jawaban atas pertanyaan-pertanyaan ini akan menentukan kualitas desain eksperimen, validitas hipotesis, dan kekuatan positioning penelitian Anda terhadap state-of-the-art. Setiap pilihan paradigma membawa karakteristik representasi dan asumsi matematis yang berbeda, sehingga pemilihan metode harus didasarkan pada pertimbangan sistematis. Pembahasan mengenai pertukaran strategis antar paradigma, termasuk bagaimana karakteristik data, kelimpahan label, sumber daya komputasi, serta kebutuhan interpretabilitas dan reliabilitas mempengaruhi keputusan metodologis, akan kita bahas secara mendalam pada slide berikutnya.

---

## Slide 022 - Trade-off Antar Paradigma

### Narasi

Setelah kita menelaah bahwa satu masalah penelitian dapat diatasi melalui berbagai paradigma komputasi, langkah kritis berikutnya adalah menyadari bahwa tidak ada metode tunggal yang optimal untuk semua kondisi. Setiap pendekatan membawa pertukaran fungsional yang harus dievaluasi secara rigor oleh peneliti tingkat doktoral sebelum memutuskan arah metodologis.

Mari kita perhatikan perbandingan pada tabel slide ini. Pendekatan berbasis *handcrafted feature* tetap relevan karena interpretabilitasnya yang tinggi dan beban komputasi yang ringan, namun kapasitas representasinya sering kali mentok pada kompleksitas visual kontemporer. Arsitektur CNN mengkompensasi hal ini dengan *inductive bias* lokal yang kuat, memungkinkan ekstraksi fitur hierarkis secara efisien, meskipun menangkap dependensi global memerlukan kedalaman jaringan yang meningkat secara eksponensial.

Di sisi lain, Vision Transformer menghilangkan batasan lokal tersebut dengan memodelkan hubungan antar-patch secara global sejak lapisan pertama, namun kinerjanya sangat bergantung pada ketersediaan data berskala besar dan protokol *pretraining* yang matang. Metode *Self-Supervised Learning* berhasil menekan ketergantungan pada anotasi manual, namun stabilitas konvergensinya sangat sensitif terhadap desain *objective function* dan strategi augmentasi. Sementara itu, *Vision-Language Models* menawarkan fleksibilitas ekstrem termasuk kemampuan *zero-shot*, namun rentan terhadap bias linguistik, *domain shift*, serta menuntut infrastruktur komputasi dan memori yang signifikan.

Pemilihan paradigma dalam proposal disertasi Anda tidak boleh bersifat intuitif atau mengikuti tren semata. Keputusan tersebut harus dipertimbangkan berdasarkan karakteristik intrinsik data, kelimpahan atau kelangkaan label supervisi, batas sumber daya komputasi yang tersedia, tujuan aplikasi akhir, tingkat keandalan yang ditargetkan, serta kebutuhan akan interpretabilitas hasil. Kriteria-kriteria ini berfungsi sebagai filter metodologis untuk memastikan bahwa kompleksitas model yang dipilih sebanding dengan kontribusi ilmiah yang diharapkan.

Pahamnya terhadap dinamika *trade-off* ini menjadi prasyarat sebelum kita masuk ke tahap validasi empiris. Pada slide berikutnya, kita akan membahas bagaimana *benchmark* berperan sebagai standar objektif dalam mengukur kemajuan, sekaligus mengingatkan bahwa metrik dan dataset tersebut seharusnya diposisikan sebagai alat ukur, bukan tujuan akhir dari penelitian doktor.

---

## Slide 023 - Peran Benchmark dalam Riset

### Narasi

Merujuk pada pembahasan trade-off antar paradigma pada slide sebelumnya, pemilihan metode yang tepat selalu bergantung pada karakteristik data, ketersediaan label, sumber daya komputasi, serta kebutuhan akan interpretabilitas dan keandalan sistem. Untuk memastikan bahwa keputusan tersebut benar-benar menghasilkan peningkatan kinerja yang nyata, kita memerlukan standar perbandingan yang objektif dan konsisten. Di sinilah benchmark berperan sebagai mesin penggerak kemajuan computer vision.

Sebuah benchmark yang valid dan bermanfaat bagi riset tingkat lanjut harus mencakup lima elemen inti yang saling melengkapi:
- Dataset standar yang telah dikurasi, didistribusikan secara terbuka, dan memiliki lisensi yang jelas.
- Task yang didefinisikan dengan presisi, menghindari ambiguitas dalam formulasi masalah penelitian.
- Metrik evaluasi yang konsisten, relevan, dan sensitif terhadap perubahan performa model.
- Baseline pembanding yang menjadi acuan kinerja awal untuk mengukur lompatan inovatif.
- Prosedur eksperimen yang dapat direproduksi, sehingga hasil dapat divalidasi ulang oleh peneliti lain di seluruh dunia.

Contoh task yang umum diimplementasikan dalam benchmark meliputi image classification, object detection, semantic segmentation, image restoration, hingga image-text retrieval. Melalui kerangka kerja ini, setiap klaim kemajuan metodologi—baik itu modifikasi arsitektur, teknik self-supervised learning, maupun integrasi multimodal—dapat diuji secara kuantitatif terhadap kondisi yang seragam. Hal ini sangat selaras dengan tuntutan riset doktoral, di mana rigoritas eksperimental, desain eksperimen yang matang, dan kemampuan memposisikan karya terhadap state-of-the-art menjadi prasyarat mutlak sebelum mengajukan kontribusi ilmiah baru.

Namun, penting untuk menekankan bahwa benchmark harus senantiasa dipandang sebagai alat ukur, bukan tujuan akhir dari sebuah penelitian. Skor tinggi pada leaderboard tertentu belum secara otomatis mencerminkan kedalaman kontribusi ilmiah, generalisasi model, atau kebaruan konseptual. Pandangan kritis terhadap keterbatasan benchmark inilah yang akan menjadi fondasi diskusi pada slide berikutnya, di mana kita akan mengurai mengapa infrastruktur evaluasi yang ada saat ini masih belum memadai dan pertanyaan-pertanyaan doktoral apa yang perlu dijawab untuk mendorong batas pengetahuan di bidang pengolahan citra digital.

---

## Slide 024 - Mengapa Benchmark Saat Ini Belum Memadai

### Narasi

Setelah membahas peran krusial benchmark sebagai mesin penggerak kemajuan computer vision pada slide sebelumnya, kita perlu menyoroti sisi lain dari ekosistem evaluasi ini. Benchmark memang menyediakan kerangka standar yang memungkinkan perbandingan kuantitatif dan reproduktibilitas eksperimen. Namun, dalam konteks penelitian tingkat doktoral, kita tidak boleh terjebak pada asumsi bahwa performa tinggi secara otomatis mencerminkan terobosan ilmiah yang substantif.

Kenyataannya, banyak benchmark yang beredar saat ini masih memiliki keterbatasan mendasar yang perlu dikritisi:
- Dataset mungkin tidak mewakili kompleksitas dunia nyata secara utuh.
- Label dapat mengandung bias dan kesalahan annotasi yang signifikan.
- Satu metrik tidak cukup untuk menangkap seluruh dimensi kualitas sistem.
- Evaluasi sering kali dilakukan pada satu domain saja, mengabaikan generalisasi lintas konteks.
- Model dapat memanfaatkan shortcut atau artefak dataset alih-alih mempelajari representasi visual yang esensial.
- Skor tinggi belum tentu berarti kontribusi ilmiah yang kuat.

Keterbatasan-keterbatasan ini memaksa peneliti doktoral untuk mengajukan pertanyaan-pertanyaan mendasar mengenai validitas evaluasi:
- Apakah peningkatan metrik tersebut bermakna secara ilmiah atau hanya noise statistik?
- Apakah benchmark yang ada benar-benar mengukur kemampuan yang ingin dipelajari?
- Apakah hasil eksperimen dapat direproduksi di lingkungan yang berbeda?
- Apakah klaim kinerja model tetap berlaku ketika menghadapi distribution shift?

Pertanyaan-pertanyaan kritis ini menjadi fondasi bagi pergeseran paradigma evaluasi dalam penelitian tingkat lanjut. Kita tidak lagi puas dengan sekadar mengejar angka di leaderboard, melainkan fokus pada kedalaman analisis metodologi dan signifikansi temuan. Pada slide berikutnya, kita akan membedah lebih lanjut bagaimana membedakan kontribusi teknis murni dari kontribusi ilmiah yang sejati, serta mengapa disertasi doktor menuntut lebih dari sekadar optimasi metrik pada benchmark konvensional.

---

## Slide 025 - Kontribusi Teknis versus Kontribusi Ilmiah

### Narasi

Setelah membahas keterbatasan berbagai benchmark yang ada pada slide sebelumnya, kita perlu memahami bahwa skor tinggi atau peningkatan metrik saja tidak otomatis mencerminkan kemajuan ilmiah yang substantif. Di sinilah perbedaan antara kontribusi teknis dan kontribusi ilmiah menjadi sangat krusial untuk dipahami, terutama dalam konteks penelitian doktoral.

Kontribusi teknis biasanya bersifat implementatif dan terukur secara langsung. Cakupannya mencakup hal-hal seperti:
- Perancangan arsitektur jaringan saraf baru.
- Pengembangan fungsi loss yang lebih efektif.
- Strategi augmentasi data yang inovatif.
- Penyetelan hyperparameter yang lebih presisi.
- Penerapan metode yang sudah ada ke dalam dataset domain tertentu.

Semua poin tersebut jelas bermanfaat dan sering kali menghasilkan peningkatan angka pada evaluasi standar. Namun, kontribusi ilmiah menuntut tingkat abstraksi dan kedalaman analisis yang berbeda. Sebuah karya dianggap memiliki kontribusi ilmiah ketika mampu:
- Menjelaskan mekanisme di balik keberhasilan suatu metode.
- Menguji asumsi dasar yang selama ini luput dari perhatian.
- Mengidentifikasi failure mode kritis yang mengungkap kelemahan sistem.
- Merumuskan masalah penelitian baru yang belum pernah dipetakan.
- Menawarkan kerangka konseptual yang mengubah cara kita memandang suatu tantangan.

Untuk tingkat doktor, disertasi tidak akan cukup hanya dengan menumpuk peningkatan metrik pada satu benchmark tunggal. Penelitian harus mampu menjembatani celah antara perbaikan teknis dan pemahaman ilmiah yang lebih luas. Pemahaman ini menjadi fondasi penting sebelum kita melangkah ke tahap identifikasi masalah terbuka, yang akan menjadi fokus pembahasan pada slide berikutnya.

---

## Slide 026 - Mengidentifikasi Masalah Terbuka

### Narasi

Setelah pada slide sebelumnya kita membedah perbedaan mendasar antara kontribusi teknis dan kontribusi ilmiah, langkah logis selanjutnya dalam perjalanan penelitian tingkat doktoral adalah mengidentifikasi masalah terbuka yang layak diinvestigasi. Tanpa celah pengetahuan yang terdefinisi dengan jelas, sebuah studi berisiko hanya menjadi implementasi ulang atau penyesuaian minor tanpa nilai tambah konseptual yang signifikan.

Masalah terbuka dalam pengolahan citra digital dan computer vision umumnya bermula dari beberapa sumber kritis yang perlu dikurasi secara selektif:
- Ketidaksesuaian asumsi model dengan kondisi dunia nyata yang kompleks dan dinamis.
- Kegagalan sistematis pada kasus tepi atau subgroup populasi tertentu yang jarang muncul di data pelatihan.
- Domain khusus seperti pencitraan medis, astronomi, atau pertanian presisi yang kurang terwakili oleh model general-purpose.
- Biaya komputasi dan kebutuhan infrastruktur yang menghambat reproduktibilitas dan adopsi praktis.
- Metrik evaluasi konvensional yang gagal menangkap aspek penting seperti konsistensi semantik, robustness, atau fairness.
- Bias dataset atau artefak anotasi yang menyebabkan model mempelajari pola artifisial alih-alih fitur intrinsik.
- Ketergantungan ekstrem pada jumlah label besar yang bertentangan dengan prinsip efisiensi data dan membuka ruang bagi pendekatan semi-supervised atau weakly supervised.

Untuk melatih sensitivitas riset Anda, kerjakan latihan berikut secara mandiri. Pilih satu area spesifik dalam computer vision yang paling relevan dengan minat doktor Anda. Tuliskan tiga masalah yang menurut Anda belum terselesaikan hingga saat ini. Kemudian, dukung setiap pernyataan tersebut dengan bukti konkret, seperti temuan dari paper terbaru, observasi eksperimen awal, atau analisis kritis terhadap laporan benchmark. Tujuan latihan ini adalah memastikan bahwa masalah yang Anda pilih benar-benar terdefinisi, terukur, dan memiliki potensi kontribusi ilmiah yang substansial.

Identifikasi masalah terbuka ini akan langsung diterjemahkan ke dalam strategi pemetaan riset. Pada slide berikutnya, kita akan membahas kerangka sistematis untuk memetakan area riset dan minat disertasi, mulai dari menentukan fokus utama, mengidentifikasi peneliti kunci, hingga menempatkan posisi kontribusi unik Anda di tengah lanskap publikasi yang sudah sangat kompetitif.

---

## Slide 027 - Memetakan Area Riset dan Minat Disertasi

### Narasi

Setelah mengidentifikasi masalah terbuka pada slide sebelumnya, langkah selanjutnya adalah mentransformasi kegelisahan riset tersebut menjadi peta arah yang terstruktur dan dapat ditindaklanjuti. Pada jenjang doktoral, minat penelitian tidak boleh sekadar mengikuti tren, melainkan harus berakar pada celah metodologis, teoretis, atau empiris yang nyata dan terdefinisi dengan jelas.

Proses pemetaan area riset dapat dioperasionalkan melalui enam pertanyaan panduan:
1. **Identifikasi minat**: Pilih area yang paling relevan dengan latar belakang, akses data, dan infrastruktur komputasi Anda. Pertimbangkan apakah area tersebut memiliki momentum perkembangan pesat dan potensi kontribusi jangka panjang.
2. **Kenali masalah**: Rumuskan masalah spesifik yang ingin dipecahkan. Masalah riset yang kuat harus memiliki batasan jelas, dapat diukur, dan menghindari klaim solusi universal tanpa validasi bertahap.
3. **Cari pemain utama**: Lakukan pemetaan ekosistem akademik. Identifikasi peneliti kunci, laboratorium terdepan, serta publikasi seminal yang mendefinisikan wacana. Langkah ini membantu Anda memahami posisi akademik, menghindari duplikasi, dan membangun jejaring kolaborasi strategis.
4. **Petakan metode**: Analisis paradigma yang dominan, apakah berbasis arsitektur konvolusional, Vision Transformer, model generatif, atau pendekatan hybrid. Evaluasi keterbatasan masing-masing secara kritis, seperti kerentanan terhadap domain shift, kebutuhan label masif, biaya inferensi tinggi, atau kurangnya interpretabilitas.
5. **Kenali benchmark**: Pahami standar evaluasi yang berlaku di area tersebut. Dataset mana yang menjadi acuan? Metrik apa yang digunakan, dan apakah metrik tersebut benar-benar menangkap aspek kualitas yang Anda targetkan? Perhatikan juga bias dalam annotation, distribusi data, dan kesenjangan antara skor benchmark dengan performa di dunia nyata.
6. **Tentukan posisi**: Definisikan kontribusi unik yang layak diuji dalam cakupan disertasi. Apakah Anda menawarkan arsitektur baru, skema pelatihan inovatif, analisis teoretis, atau adaptasi khusus untuk domain terbatas? Pastikan kontribusi tersebut dapat diverifikasi secara eksperimental dan memiliki baseline perbandingan yang solid.

Perlu ditekankan bahwa pemetaan awal bersifat dinamis dan tidak perlu sempurna sejak tahap perencanaan. Peta ini akan terus disempurnakan sepanjang semester seiring dengan tinjauan literatur mendalam, pelaksanaan eksperimen baseline, serta umpan balik dari diskusi penelitian dan klinik riset. Fleksibilitas dalam menyesuaikan arah riset berdasarkan temuan empiris merupakan ciri khas penelitian tingkat doktoral yang matang.

Untuk menjaga transparansi, akuntabilitas, dan konsistensi selama perjalanan pemetaan ini, diperlukan alat dokumentasi yang sistematis. Slide berikutnya akan membahas bagaimana menyusun Research Log sebagai catatan berpikir yang mencatat keputusan metodologis, perkembangan ide, hasil eksperimen, hingga kegagalan yang justru menjadi bahan pembelajaran berharga. Konsistensi pencatatan dan kemampuan menelusuri kembali alasan di balik setiap pilihan riset akan menjadi fondasi kuat bagi penulisan proposal disertasi dan publikasi ilmiah Anda.

---

## Slide 028 - Research Log: Tujuan dan Format

### Narasi

Research log merupakan instrumen fundamental dalam perjalanan penelitian tingkat doktoral. Berbeda dengan catatan biasa, dokumen ini berfungsi sebagai ruang berpikir yang terstruktur untuk mendokumentasikan setiap langkah, keputusan metodologis, dan perkembangan ide sepanjang proses penelitian. Merujuk pada slide sebelumnya tentang pemetaan area riset dan minat disertasi, langkah selanjutnya adalah mengabadikan pemetaan tersebut ke dalam format yang dapat ditelusuri secara konsisten.

Tujuan penelitian log bukan sekadar menjadi arsip pasif, melainkan berperan sebagai alat aktif untuk meningkatkan rigor akademik. Manfaat utamanya meliputi:
- Mendokumentasikan keputusan penelitian beserta justifikasinya.
- Melacak perkembangan ide dari hipotesis awal hingga implementasi.
- Menyimpan pertanyaan yang belum terjawab atau celah pengetahuan yang muncul.
- Mencatat hasil eksperimen, kegagalan, dan pelajaran teknis yang berharga.
- Menjadi bahan diskusi yang substantif dengan dosen pembimbing maupun kolega.

Dalam hal format, peneliti diberikan kebebasan menyesuaikan dengan ekosistem kerja masing-masing. Beberapa opsi yang direkomendasikan antara lain:
- File Markdown untuk dokumentasi teks yang ringan dan mudah dibaca.
- Jupyter Notebook untuk menggabungkan kode, eksekusi, visualisasi, dan narasi analisis.
- Repository Git untuk version control, kolaborasi, dan pelacakan perubahan.
- Dokumen bersama berbasis cloud untuk diskusi real-time dan feedback cepat.
Yang terpenting dari semua format tersebut adalah konsistensi penggunaan dan kemampuan menelusuri kembali alasan di balik setiap keputusan penelitian. Tanpa jejak yang jelas, reproduktibilitas dan validitas metodologi akan sulit dibuktikan.

Agar proses pencatatan tidak menjadi beban administratif, struktur konten dapat disederhanakan tanpa mengurangi kedalaman analisis. Pada slide berikutnya, akan diperkenalkan workflow research log yang terdiri dari sepuluh poin esensial, mulai dari identifikasi area eksplorasi, sumber dataset, pertanyaan penelitian sementara, temuan awal, baseline, failure cases, asumsi, risiko metodologis, interpretasi, hingga rencana eksperimen lanjutan. Pendekatan ini memastikan bahwa log tetap ringkas namun komprehensif, sehingga setiap perubahan arah riset atau kegagalan teknis dapat dipertanggungjawabkan secara akademis sepanjang semester.

---

## Slide 029 - Workflow Research Log

### Narasi

Pada slide sebelumnya, kita telah menetapkan tujuan dasar dan format fleksibel untuk *research log*. Kini, mari kita operasionalkan konsep tersebut menjadi alur kerja yang terstruktur agar pencatatan penelitian Anda tidak hanya menjadi catatan harian, melainkan peta perjalanan ilmiah yang dapat diaudit dan direplikasi.

Format yang ditampilkan di sini dirancang khusus untuk menangkap dinamika penelitian tingkat doktor. Setiap elemen dalam daftar ini berfungsi sebagai anchor bagi proses berpikir kritis dan desain eksperimen Anda:
- Area atau topik yang sedang dieksplorasi, sebagai konteks awal pencarian celah penelitian.
- Dataset dan sumbernya, mencakup skema akses, lisensi, serta karakteristik teknis dan etika data.
- Pertanyaan penelitian sementara, yang akan terus diuji dan disempurnakan seiring berjalannya iterasi.
- Temuan dari eksplorasi data, meliputi pola visual, anomali statistik, atau distribusi kelas yang tidak terduga.
- Baseline dan hasil awal, sebagai titik tolak objektif untuk mengukur peningkatan kinerja model.
- Failure cases, yang justru sering menjadi sumber insight paling bernilai untuk perbaikan arsitektur, preprocessing, atau definisi tugas.
- Asumsi yang digunakan, baik implisit maupun eksplisit, agar validitas logika eksperimen dapat dilacak.
- Risiko metodologis, seperti bias seleksi, kebocoran data, atau ketidaksesuaian metrik evaluasi dengan tujuan penelitian.
- Interpretasi awal, menghubungkan temuan empiris dengan teori yang ada dan literatur terkini.
- Langkah eksperimen berikutnya, yang menentukan arah iterasi selanjutnya secara sistematis dan terukur.

Perlu ditegaskan bahwa *research log* ini tidak memerlukan format kaku atau dokumen administratif. Fleksibilitas justru memungkinkan Anda mendokumentasikan ide eksperimental, kegagalan cepat, atau perubahan strategi tanpa hamburan birokrasi. Namun, konsistensi dalam menelusuri keputusan penting, modifikasi hiperparameter, dan kegagalan eksperimen adalah syarat mutlak di jenjang doktor. Kemampuan menjawab pertanyaan "mengapa saya memilih jalur ini?" sama krusialnya dengan "berapa akurasi yang dihasilkan?".

Struktur ini akan langsung diimplementasikan ketika Anda memasuki fase praktis pada slide berikutnya. Saat melakukan eksplorasi dataset, audit bias, dan pembangunan baseline klasik pada Praktikum 01, kerangka *workflow* ini akan menjadi pedoman evaluasi yang membantu Anda mengidentifikasi masalah penelitian nyata sebelum beralih ke pengembangan model berbasis deep learning atau foundation model.

---

## Slide 030 - Praktikum 01: Tujuan dan Alur Eksperimen

### Narasi

Slide ini membuka Praktikum 01 dengan judul proyek *Image Dataset Exploration, Bias Analysis, and Classical Baseline*. Sebelum beralih ke arsitektur deep learning, Vision Transformer, atau model foundation terkini, mahasiswa tingkat doktor dituntut untuk membangun fondasi metodologis yang kuat melalui pemahaman kritis terhadap data itu sendiri. Tahap ini bukan sekadar latihan implementasi kode, melainkan proses investigasi ilmiah untuk memvalidasi kelayakan dataset sebelum digunakan dalam eksperimen lanjutan.

Tujuan praktikum ini dirancang untuk mengembangkan kompetensi penelitian Anda melalui empat pilar utama:
- Memahami struktur dan karakteristik awal dataset citra secara komprehensif.
- Mengidentifikasi distribusi kelas, variasi visual, kualitas data, serta potensi bias sistematis yang dapat memicu shortcut pembelajaran pada model neural network.
- Membangun baseline klasik sebagai titik pembanding objektif untuk mengukur peningkatan performa di tahap berikutnya.
- Menginterpretasikan hasil evaluasi dan failure cases sebagai bahan perumusan pertanyaan penelitian awal yang relevan dengan research gap.

Alur eksperimen yang ditampilkan mengikuti pola kerja penelitian yang sistematis dan dapat direplikasi. Proses dimulai dari dataset, dilanjutkan dengan eksplorasi dan audit data, kemudian pembangunan baseline klasik, evaluasi, analisis kegagalan, hingga interpretasi penelitian. Setiap tahapan harus dicatat secara transparan. Sesuai dengan format *Research Log* yang telah dibahas pada slide sebelumnya, keputusan metodologis, perubahan parameter, maupun kegagalan awal wajib dilacak agar jejak penelitian Anda tetap auditable, reproducible, dan kokoh secara akademis.

Langkah teknis, implementasi kode lengkap, serta instruksi tugas eksperimen telah dipisahkan ke dalam modul praktikum terpisah agar ruang kuliah tetap fokus pada aspek desain penelitian, validasi hipotesis, dan analisis konseptual. Pada slide berikutnya, kita akan mendalami secara spesifik prosedur eksplorasi dan audit dataset, termasuk pemanfaatan tools standar seperti NumPy, Matplotlib, scikit-learn, scikit-image, dan OpenCV, serta menjawab pertanyaan kunci mengenai representativitas data, dominansi kelas, dan artefak visual yang berpotensi menjadi shortcut bagi model.

---

## Slide 031 - Praktikum 01: Eksplorasi dan Audit Dataset

### Narasi

Setelah memahami tujuan umum dan alur eksperimen pada praktikum sebelumnya, kita kini masuk ke tahap inti pertama, yaitu eksplorasi dan audit dataset. Pada tingkat doktoral, langkah ini bukan sekadar prosedur teknis, melainkan fondasi kritis untuk merumuskan masalah penelitian yang valid. Kita harus mengkuantifikasi dan memvisualisasikan karakteristik data sebelum menyentuh arsitektur model apa pun, karena kualitas insight awal akan menentukan arah hipotesis dan desain eksperimen selanjutnya.

Fokus eksplorasi mencakup beberapa dimensi yang harus dievaluasi secara sistematis:
- Struktur dataset: jumlah sampel, jumlah kelas, resolusi, channel, dan format file.
- Distribusi kelas: identifikasi potensi *class imbalance* yang dapat mendistorsi gradien selama pelatihan.
- Visualisasi sampel: sampling acak per kelas untuk verifikasi konsistensi label dan kualitas awal.
- Variasi visual: analisis intensitas, kontras, *blur*, *noise*, pencahayaan, dan oklusi.
- Deteksi bias: pencarian pola artifisial atau *shortcut* yang berkorelasi palsu dengan label target.

Untuk melaksanakan audit ini, kita memanfaatkan ekosistem Python standar dalam pengolahan citra, meliputi NumPy untuk manipulasi array multidimensi, Matplotlib untuk visualisasi statistik dan sampel, serta scikit-learn, scikit-image, dan OpenCV untuk ekstraksi fitur dan transformasi geometris. Sebagai demonstrasi awal, kita akan menjalankan `load_digits()` dari scikit-learn karena strukturnya yang terstruktur rapi namun tetap relevan untuk menguji pipeline audit. Namun, untuk konteks riset disertasi, Anda wajib menerapkan protokol yang sama pada dataset penelitian Anda sendiri. Selama proses ini, tiga pertanyaan kunci harus terus dijawab: apakah dataset cukup representatif terhadap domain masalah? Apakah terdapat kelas yang dominan atau langka secara signifikan? Dan apakah terdapat artefak visual yang dapat menjadi *shortcut* bagi model?

Temuan dari audit ini akan langsung menjadi landasan bagi tahap berikutnya, yaitu pembangunan baseline klasik. Ketika Anda telah memetakan distribusi kelas, variasi visual, dan potensi bias, pemilihan metode seperti *Majority Class*, histogram intensitas dengan kNN, atau HOG dikombinasikan dengan Linear SVM akan menjadi lebih strategis. Pendekatan ini bukan hanya untuk menetapkan batas bawah performa, tetapi juga untuk mengisolasi kontribusi representasi spasial versus statistik intensitas, sehingga Anda dapat merancang eksperimen yang lebih tajam, reproducible, dan berorientasi pada identifikasi *research gap* pada slide berikutnya.

---

## Slide 032 - Praktikum 01: Baseline Klasik

### Narasi

Setelah menyelesaikan tahap eksplorasi dan audit dataset pada slide sebelumnya, langkah metodologis berikutnya adalah membangun serangkaian baseline klasik sebagai landasan evaluasi kuantitatif. Pada praktikum ini, kita akan menyusun tiga tingkatan baseline yang diproyeksikan secara berurutan untuk mengukur peningkatan performa model seiring dengan tingkat kedalaman representasi visual yang digunakan.

Baseline pertama adalah *Majority Class*. Pendekatan ini hanya memprediksi kelas yang memiliki frekuensi tertinggi dalam data pelatihan tanpa mengekstrak ciri visual apa pun. Nilai akurasi dari baseline ini menetapkan batas bawah (*lower bound*) yang harus dilampaui oleh metode manapun. Jika model yang lebih sophisticated tidak menunjukkan peningkatan signifikan di atas angka ini, maka representasi visual yang dipilih belum memberikan sinyal diskriminatif yang bermakna.

Baseline kedua menggabungkan *Intensity Histogram* dengan algoritma *k-Nearest Neighbors (kNN)*. Histogram intensitas merekam distribusi statistik piksel secara global, namun masih mengabaikan struktur spasial, orientasi tepi, dan tekstur lokal. Penggunaan kNN pada fitur ini memungkinkan kita menguji seberapa efektif distribusi intensitas murni mampu memisahkan antar kategori. Tahap ini berfungsi sebagai jembatan analitis sebelum beralih ke deskriptor yang lebih kaya secara geometris.

Baseline ketiga menerapkan *Histogram of Oriented Gradients (HOG)* yang diklasifikasikan menggunakan *Linear Support Vector Machine (SVM)*. HOG berhasil menangkap pola gradien arah dan kontur lokal, sehingga mampu merepresentasikan bentuk objek dengan presisi jauh lebih tinggi dibandingkan histogram intensitas. Kombinasi ini merupakan standar baku dalam computer vision klasik pra-deep learning, dan sering dijadikan patokan ketat untuk menilai apakah fitur buatan manusia sudah mencapai saturasi performa atau perlu digantikan oleh representasi laten berbasis neural network.

Prinsip eksperimen yang wajib dijaga adalah kontrol variabel yang ketat. Pastikan pembagian data (*train/validation/test split*), protokol augmentasi, normalisasi, dan metrik evaluasi tetap identik di seluruh baseline. Hanya dengan kondisi yang terstandarisasi, selisih performa dapat diatribusikan secara valid kepada perubahan strategi representasi fitur, bukan akibat kebocoran data, ketidakseimbangan prosedur, atau noise dalam pipeline preprocessing.

Perlu ditekankan bahwa rangkaian baseline ini bukanlah tujuan akhir dari investigasi doctoral Anda. Mereka berperan sebagai titik referensi kritis untuk menguji asumsi dasar: apakah arsitektur yang lebih kompleks benar-benar diperlukan, ataukah bottleneck terletak pada kualitas data, desain fitur, atau bias sampling? Temuan dari setiap tingkat baseline akan langsung diteruskan ke proses evaluasi komprehensif dan *failure analysis* pada slide berikutnya, di mana kita akan mengidentifikasi kelas yang paling rentan tertukar, menganalisis sumber error, serta merumuskan pertanyaan riset yang tajam untuk mengisi celah metodologis pada pertemuan-pertemuan selanjutnya.

---

## Slide 033 - Praktikum 01: Evaluasi, Failure Analysis, dan Refleksi Riset

### Narasi

Setelah kita membangun tiga baseline klasik pada slide sebelumnya—mulai dari majority class, histogram intensitas dengan kNN, hingga HOG dikombinasikan dengan linear SVM—langkah selanjutnya adalah mengevaluasi hasil tersebut secara sistematis. Evaluasi tidak berhenti pada perhitungan akurasi, melainkan menuntut pemahaman mendalam terhadap pola kegagalan model.

Gunakan kombinasi metrik seperti accuracy dan balanced accuracy, terutama ketika distribusi kelas tidak merata. Sajikan confusion matrix untuk memetakan kesalahan antar kelas secara granular. Lakukan inspeksi visual pada failure cases untuk membedakan apakah kesalahan berasal dari ambiguannya konten citra, keterbatasan representasi fitur, atau kelemahan classifier itu sendiri.

Interpretasi hasil eksperimen harus bersifat kritis dan melampaui angka performa. Tanyakan representasi mana yang paling informatif dan mengapa kelas tertentu cenderung tertukar. Identifikasi sumber error: apakah berasal dari kualitas data, pilihan representasi, arsitektur classifier, atau bias dalam dataset? Evaluasi juga apakah penambahan kompleksitas model memberikan peningkatan signifikansi yang nyata, atau hanya menangkap noise. Desain eksperimen berikutnya harus langsung menjawab pertanyaan-pertanyaan analitis ini.

Alur refleksi riset yang diharapkan mengikuti struktur berjenjang:
- Observation empiris dari hasil baseline
- Identifikasi failure atau limitation spesifik
- Penyusunan assumption yang mendasari eksperimen
- Perumusan research question yang terukur
- Penentuan next experiment yang informatif

Catat seluruh temuan utama secara terstruktur dalam research log. Catatan ini menjadi fondasi metodologis untuk mengidentifikasi research gap yang akan dieksplorasi lebih lanjut pada sesi berikutnya.

Pada slide berikutnya, kita akan mendalami bagaimana merumuskan asumsi dan risiko awal secara eksplisit. Setiap eksperimen dibangun di atas asumsi implisit seperti representativitas dataset, konsistensi label, tidak adanya train-test leakage, relevansi representasi, dan kesesuaian metrik evaluasi. Di sisi lain, risiko seperti ukuran dataset yang terbatas, distribusi data yang bergeser, class imbalance yang menyesakkan interpretasi akurasi, model yang memanfaatkan shortcut, evaluasi pada satu dataset saja, serta keterbatasan komputasi, harus diantisipasi sejak tahap perencanaan. Menuliskan asumsi dan risiko secara transparan dalam research log akan meningkatkan rigoritas penelitian dan mempersiapkan kerangka evaluasi yang lebih robust di tahap pengembangan metodologi lanjutan.

---

## Slide 034 - Menyusun Asumsi dan Risiko Awal

### Narasi

Setiap eksperimen dalam penelitian pengolahan citra digital dan computer vision dibangun di atas serangkaian asumsi yang sering kali tersirat namun harus ditegaskan secara eksplisit. Sebagaimana kita telaah pada analisis kegagalan dan refleksi metodologi sebelumnya, kekuatan sebuah klaim riset sangat bergantung pada validitas fondasi yang mendasari eksperimen tersebut. Tanpa pemetaan asumsi yang jelas, sulit bagi peneliti untuk membedakan antara kelemahan arsitektur model dengan keterbatasan desain eksperimen.

Berikut adalah beberapa contoh asumsi kritis yang wajib diverifikasi sebelum eksekusi:
- Apakah dataset yang dikumpulkan cukup representatif terhadap variasi domain target?
- Apakah anotasi label konsisten, akurat, dan bebas dari noise sistematis?
- Apakah proses train-test split telah memastikan tidak adanya data leakage antar subset?
- Apakah representasi fitur yang dipilih memang relevan menangkap sinyal utama dalam data?
- Apakah metrik evaluasi yang ditetapkan selaras dengan tujuan aplikasi praktis?

Di samping asumsi, setiap setup eksperimen juga membawa risiko intrinsik yang dapat mengancam validitas atau reproduksibilitas hasil. Risiko-risiko ini meliputi:
- Ukuran dataset yang terlalu kecil sehingga model rentan overfitting.
- Pergeseran distribusi data (*distribution shift*) saat deployment atau pengujian eksternal.
- Ketidakseimbangan kelas yang membuat akurasi tampak tinggi padahal performa pada minoritas kelas buruk.
- Kecenderungan model memanfaatkan *shortcut learning* alih-alih memahami struktur semantik gambar.
- Evaluasi yang hanya terpaku pada satu dataset tanpa validasi silang atau cross-domain testing.
- Keterbatasan sumber daya komputasi yang menghambat replikasi penuh atau pencarian hiperparameter yang memadai.

Catat seluruh asumsi dan risiko ini secara rinci dalam *research log*. Dokumentasi tertulis ini berfungsi sebagai baseline diagnostik ketika Anda menghadapi anomali atau *failure case*. Dengan asumsi dan risiko yang sudah terpetakan, proses penelusuran akar masalah menjadi lebih terarah dan sistematis. Hal ini akan menjadi jembatan metodologis yang kuat menuju langkah selanjutnya, yaitu mengubah observasi kegagalan menjadi pertanyaan penelitian yang spesifik dan berkontribusi, sesuai dengan alur logika yang akan kita kembangkan pada slide berikutnya.

---

## Slide 035 - Dari Failure Case ke Research Question

### Narasi

Pada slide ini, kita beralih dari identifikasi asumsi dan risiko yang telah dibahas pada slide sebelumnya, menuju pemanfaatan *failure case* sebagai sumber inspirasi penelitian. Dalam konteks riset tingkat doktor, kegagalan model tidak boleh dipandang sekadar sebagai bug atau penurunan akurasi semata. Sebaliknya, *failure case* merupakan sinyal empiris yang mengindikasikan adanya kesenjangan antara kapasitas representasi model dengan karakteristik data dunia nyata. Dengan menganalisis pola kegagalan secara sistematis, kita dapat merumuskan pertanyaan penelitian yang tajam, terukur, dan berpotensi mengisi celah metodologis yang belum terjamah.

Perhatikan alur logika yang disajikan dalam blok teks berikut:
```text
Observasi
"Digit tertentu sering tertukar"
        ↓
Kemungkinan penyebab
"Representasi kurang menangkap struktur bentuk"
        ↓
Hipotesis
"Representasi dengan spatial structure lebih robust"
        ↓
Eksperimen
Histogram vs HOG vs learned representation
        ↓
Pertanyaan penelitian
"Kapan representasi yang lebih kompleks memberi keuntungan?"
```
Proses ini dimulai dari pengamatan empiris, misalnya ketika model secara konsisten salah mengklasifikasikan digit tertentu. Langkah selanjutnya adalah mengisolasi kemungkinan penyebabnya, seperti ketidakmampuan representasi saat ini menangkap struktur bentuk secara memadai. Dari sana, kita merumuskan hipotesis bahwa representasi yang secara eksplisit memodelkan struktur spasial akan lebih robust. Hipotesis tersebut kemudian diuji melalui eksperimen komparatif, misalnya membandingkan histogram intensitas piksel, fitur HOG, dan representasi yang dipelajari secara end-to-end. Temuan eksperimen akhirnya mengarah pada pertanyaan penelitian yang bernilai ilmiah, seperti kapan representasi yang lebih kompleks memberikan keuntungan signifikan dibandingkan pendekatan tradisional, dan pada kondisi apa kompleksitas tersebut justru menimbulkan overfitting atau bias.

Kerangka kerja ini bersifat universal dan sangat relevan untuk berbagai tantangan riset mutakhir di bidang pengolahan citra digital dan computer vision. Di bidang *domain shift*, kegagalan model pada distribusi target dapat memicu investigasi tentang mekanisme adaptasi fitur tanpa label. Pada pencitraan cahaya rendah atau pencitraan medis, noise dan artefak sering menyebabkan *false positive* yang justru membuka peluang pengembangan metode denoising atau augmentasi berbasis self-supervised learning. Demikian pula pada *remote sensing*, kelas langka (*rare classes*) atau integrasi data multimodal seperti kombinasi spektral, termal, dan geometri menuntut eksplorasi arsitektur yang mampu menangkap ketergantungan lintas modalitas. Setiap kasus kegagalan yang terdokumentasi dengan baik berpotensi menjadi fondasi studi yang inovatif dan siap diuji validitasnya.

Analisis mendalam terhadap *failure case* ini akan langsung diterjemahkan ke dalam luaran konkret yang diharapkan pada akhir pertemuan pertama. Sesuai dengan target yang tercantum pada slide berikutnya, mahasiswa diminta untuk menyusun peta area riset awal, melakukan audit dataset beserta notebook baseline, mengidentifikasi tiga hingga lima masalah potensial, serta mendokumentasikan asumsi, risiko, dan eksperimen lanjutan. Dengan demikian, proses transformasi dari temuan empiris menjadi pertanyaan penelitian yang terstruktur menjadi langkah pertama yang krusial dalam penyusunan proposal disertasi yang rigor, reproducible, dan berorientasi pada state-of-the-art.

---

## Slide 036 - Target Luaran Pertemuan 01

### Narasi

Slide ini menetapkan empat luaran konkret yang harus diselesaikan pada akhir pertemuan pertama. Pada jenjang doktoral, kejelasan target awal berfungsi sebagai fondasi struktural untuk merancang penelitian yang terukur, kritis, dan siap dikembangkan menjadi proposal disertasi.

Luaran pertama menuntut penyusunan peta awal area riset. Mahasiswa tidak hanya mendaftar topik umum, tetapi harus mengidentifikasi area utama yang diminati, subarea spesifik yang selaras dengan perkembangan mutakhir, serta kandidat metode dan paper penting yang menjadi rujukan foundational maupun state-of-the-art. Peta ini akan menjadi kompas selama proses eksplorasi literatur dan penajaman fokus penelitian.

Luaran kedua berfokus pada audit dataset dan pembuatan notebook baseline. Mahasiswa diminta menganalisis struktur data, memvisualisasikan sampel representatif, serta mendeteksi potensi masalah kualitas data atau bias sistematis. Setelah itu, diimplementasikan baseline sederhana sebagai titik tolak evaluasi. Catatan performa terbatas atau pola kesalahan dari baseline ini akan langsung diterjemahkan menjadi bahan analisis kritis, sebagaimana telah dibahas pada slide sebelumnya mengenai transformasi failure case menjadi pertanyaan penelitian yang terstruktur.

Luaran ketiga meminta formulasi tiga hingga lima masalah potensial. Setiap masalah harus ditulis sebagai pernyataan gap atau problem statement awal, disertai justifikasi kuat mengapa isu tersebut signifikan secara ilmiah maupun praktis. Pada tingkat S3, ketajaman argumen tentang urgensi masalah sering kali menjadi penentu utama novelty dan posisi kontribusi penelitian terhadap state-of-the-art.

Luaran keempat menyangkut perumusan asumsi kerja, risiko metodologis, dan satu eksperimen lanjutan yang logis. Asumsi perlu diidentifikasi secara eksplisit karena ketidaksesuaian asumsi dengan karakteristik data dapat menggagalkan seluruh desain eksperimen. Risiko metodologis mencakup potensi bias evaluasi, overfitting, atau keterbatasan infrastruktur komputasi. Eksperimen lanjutan yang diusulkan harus merupakan langkah natural berikutnya setelah melihat hasil baseline dan peta riset awal.

Keluaran-keluaran ini dirancang agar langsung menyuburkan diskusi reflektif pada slide berikutnya. Pertanyaan kritis seperti relevansi peta riset, justifikasi pemilihan metode, kelayakan model kompleks versus pendekatan sederhana, atau bukti yang diperlukan untuk mempercayai suatu klaim, akan diuji melalui kelengkapan artefak penelitian yang telah dihasilkan. Dengan demikian, refleksi tidak hanya bersifat konseptual, tetapi grounded pada data, baseline, dan desain eksperimen yang telah dirumuskan bersama.

---

## Slide 037 - Refleksi dan Diskusi

### Narasi

Setelah menyelesaikan penyusunan peta riset awal dan melakukan audit dataset beserta eksperimen baseline pada sesi sebelumnya, kita kini memasuki tahap evaluasi kritis. Slide ini menyajikan rangkaian pertanyaan reflektif yang berfungsi sebagai filter akademis sebelum Anda mendalami topik penelitian secara lebih intensif.

- Bagian peta riset mana yang paling relevan dengan minat Anda?
- Mengapa Anda memilih metode tertentu?
- Apakah baseline klasik masih relevan untuk problem Anda?
- Jika model sederhana sudah sangat baik, apa justifikasi menggunakan model kompleks?
- Failure case apa yang paling menarik?
- Benchmark apa yang seharusnya digunakan?
- Bukti apa yang diperlukan untuk mempercayai suatu klaim?
- Risiko apa yang berpotensi membuat kesimpulan eksperimen salah?

Pertanyaan-pertanyaan ini dirancang untuk menguji kedalaman analisis dan kesiapan metodologis Anda. Mulailah dengan meninjau kembali peta riset yang telah Anda susun. Pilih area yang benar-benar menawarkan celah penelitian yang terukur, bukan sekadar mengikuti tren tanpa dasar teoretis yang kuat. Ketika mempertimbangkan pemilihan metode, pastikan keputusan Anda didorong oleh karakteristik data dan hipotesis kerja, bukan preferensi pribadi atau kemudahan implementasi semata.

Isu baseline klasik versus model kompleks memerlukan pertimbangan yang matang. Jika pendekatan tradisional seperti SVM, Random Forest, atau model berbasis handcrafted feature sudah mencapai performa kompetitif, beralih ke arsitektur deep learning hanya dapat dibenarkan jika terdapat peningkatan signifikansi statistik, kebutuhan skalabilitas, atau kemampuan ekstraksi fitur hierarkis yang tidak mampu ditangkap model linear. Evaluasi failure cases juga harus dilakukan secara sistematis. Identifikasi pola kesalahan prediksi, apakah terdistribusi acak atau menunjukkan bias sistematis terhadap kelas minoritas dan kondisi augmentasi tertentu.

Penentuan benchmark evaluasi harus selaras dengan tujuan penelitian dan sifat distribusi data. Hindari ketergantungan tunggal pada metrik akurasi global; manfaatkan precision, recall, F1-score, ROC-AUC, atau metrik berbasis probabilitas yang lebih robust terhadap ketidakseimbangan kelas dan noise. Selain itu, selalu tanyakan bukti empiris apa yang dibutuhkan untuk mendukung klaim Anda, serta risiko metodologis seperti data leakage, overfitting pada set validasi, atau distribusi sampel uji yang tidak representatif yang dapat meruntuhkan validitas eksternal eksperimen.

Pada jenjang doktoral, kualitas pertanyaan penelitian dan ketajaman argumen analitis memiliki bobot yang setara dengan kemampuan teknis implementasi. Refleksi mendalam ini akan langsung diterjemahkan ke dalam struktur tugas akhir dan bukti belajar yang akan kita bahas pada slide berikutnya, di mana Anda diminta mendokumentasikan seluruh proses audit, eksperimen baseline, interpretasi hasil, serta perumusan minimal dua pertanyaan penelitian awal secara sistematis dalam format Jupyter Notebook atau Google Colab.

---

## Slide 038 - Tugas dan Bukti Belajar

### Narasi

Setelah sesi refleksi dan diskusi mengenai relevansi peta riset serta justifikasi pemilihan metode, kini saatnya menerjemahkan minat akademik menjadi eksplorasi empiris yang terukur. Pilihlah satu dataset citra yang selaras dengan fokus penelitian Anda, lalu kerjakan serangkaian langkah sistematis berikut ini:

1. Lakukan *dataset audit* untuk memeriksa metadata, konsistensi label, resolusi, dan representasi domain.
2. Visualisasikan distribusi kelas guna mendeteksi ketidakseimbangan (*class imbalance*).
3. Tampilkan sampel acak per kelas untuk verifikasi kualitas visual dan anotasi.
4. Analisis kualitas citra secara menyeluruh, termasuk noise, artefak kompresi, dan variasi pencahayaan.
5. Identifikasi potensi bias atau *shortcut learning* yang dapat menyesatkan model.
6. Bangun minimal satu baseline klasik yang relevan dengan problem statement.
7. Evaluasi performa menggunakan metrik yang sesuai dengan jenis tugas.
8. Tampilkan *confusion matrix* atau dokumentasi *failure cases* secara detail.
9. Tulis interpretasi hasil yang menghubungkan temuan empiris dengan karakteristik data.
10. Rumuskan minimal dua pertanyaan penelitian awal sebagai pijakan eksperimen lanjutan.

Fokus utama pada tahap ini adalah membangun pemahaman mendalam tentang data sebelum terjun ke arsitektur model yang kompleks. *Dataset audit* bukan sekadar penghitungan statistik, melainkan investigasi kritis terhadap konsistensi anotasi, kemungkinan kebocoran data (*data leakage*), dan heterogenitas visual. Visualisasi distribusi kelas dan sampel per kelas akan mengungkap pola yang sering terlewatkan, seperti dominasi minoritas kelas atau adanya outlier visual yang dapat mendistorsi gradien selama pelatihan.

Analisis kualitas citra dan identifikasi bias memerlukan pendekatan yang ketat. Perhatikan apakah terdapat artefak kompresi, resolusi rendah, atau keterwakilan demografik yang terbatas. Potensi *shortcut learning* harus diwaspadai karena model dapat mencapai akurasi tinggi secara artifaktual tanpa mempelajari fitur semantik yang sebenarnya. Setelah karakteristik data dipahami, bangun baseline klasik yang solid. Baseline ini berfungsi sebagai *ground truth* empiris untuk membandingkan kinerja metode mutakhir seperti Vision Transformer, model *self-supervised*, atau arsitektur berbasis *diffusion*.

Evaluasi dilakukan dengan metrik yang tepat, diikuti oleh presentasi *confusion matrix* atau analisis *failure cases* untuk mengidentifikasi pola kesalahan sistematis. Interpretasi hasil harus bersifat kritis, menjelaskan mengapa model gagal pada kasus tertentu, dan bagaimana hal tersebut mengarah pada penyempurnaan metodologi. Dua pertanyaan penelitian awal yang dirumuskan akan menjadi kompas untuk merancang hipotesis dan desain eksperimen berikutnya.

Seluruh proses wajib direkam sebagai bukti belajar dalam bentuk Jupyter Notebook atau Google Colab yang terstruktur, dilengkapi *research log* yang mendokumentasikan setiap keputusan teknis dan iterasi eksperimen. Ringkasan hasil beserta rekomendasi eksperimen berikutnya akan menjadi bahan persiapan langsung untuk pertemuan kedua. Dengan fondasi data dan baseline yang kuat, Anda akan lebih tajam dalam melakukan *critical paper reading*, mengevaluasi klaim penulis, serta mengidentifikasi *research gap* yang substantif pada literatur terkini.

---

## Slide 039 - Persiapan untuk Pertemuan Berikutnya

### Narasi

Slide ini mengarahkan persiapan konkret untuk pertemuan berikutnya yang akan berfokus pada *critical paper reading* dan identifikasi *research gap*. Pada jenjang doktoral, kemampuan membaca literatur secara kritis menuntut lebih dari sekadar memahami alur argumen penulis. Mahasiswa dituntut untuk mengurai klaim metodologis, mengevaluasi ketatnya desain eksperimen, dan mencari celah pengetahuan yang belum terjamah.

Sebelum pertemuan kedua, setiap mahasiswa wajib memilih satu paper kunci yang selaras dengan minat riset masing-masing. Proses pembacaan harus dilakukan secara terstruktur. Abstrak, pendahuluan, metode inti, hasil eksperimen, dan kesimpulan perlu dicermati secara mendalam. Saat membaca, catat klaim utama peneliti, lalu bandingkan dengan *baseline* yang mereka gunakan. Evaluasi apakah klaim tersebut benar-benar didukung oleh bukti empiris atau justru bertumpu pada asumsi yang lemah.

Periksa juga dataset dan metrik evaluasi yang dilaporkan dalam paper. Tanyakan apakah dataset tersebut representatif terhadap kondisi dunia nyata, serta apakah metrik yang dipilih sensitif terhadap bias data atau fenomena *shortcut learning*. Catat minimal satu *failure case*, keterbatasan metodologis, atau hipotesis yang belum diuji. Catatan ini akan menjadi bahan diskusi kritis saat kita membedah literatur bersama di kelas.

Hasil eksplorasi dataset dan pembangunan *baseline* dari pertemuan ini dapat langsung Anda manfaatkan sebagai konteks ketika membaca paper. Jika sebelumnya Anda menemukan distribusi kelas yang tidak seimbang atau artefak teknis pada dataset pilihan Anda, gunakan temuan itu sebagai lensa analitis untuk menilai apakah paper yang Anda baca telah mengontrol variabel serupa. Integrasi antara bukti empiris awal dan literatur mutakhir inilah yang akan mempertajam pertanyaan penelitian Anda.

Persiapan ini akan langsung terhubung dengan capaian pembelajaran yang akan dibahas pada slide berikutnya. Peta riset memberikan kerangka konseptual, praktikum memberikan bukti empiris, dan *research log* menjembatani keduanya menjadi proses investigasi ilmiah yang transparan dan dapat ditelusuri. Fondasi yang Anda bangun melalui tugas audit dataset dan pemilihan paper ini akan menjadi landasan bagi analisis kritis, penentuan *novelty*, serta penyusunan proposal disertasi di tahap selanjutnya.

---

## Slide 040 - Kaitan dengan Capaian Pembelajaran

### Narasi

Pada slide ini, kita mengaitkan secara eksplisit aktivitas dan materi Pertemuan 01 dengan Capaian Pembelajaran Mata Kuliah atau CPMK yang telah ditetapkan. Tabel di atas merangkum bagaimana setiap komponen pembelajaran berkontribusi langsung terhadap tujuan akademik tingkat doktoral ini.

Untuk CPMK-1, fokusnya adalah menganalisis evolusi dan perkembangan riset pengolahan citra maupun computer vision. Melalui peta riset mutakhir yang telah kita bahas, mahasiswa dilatih untuk melacak pergeseran paradigma dari metode tradisional berbasis manipulasi piksel menuju representasi yang dipelajari secara mendalam hingga era foundation models. Pemahaman historis dan konseptual ini menjadi landasan kritis sebelum memasuki analisis metodologi spesifik.

CPMK-4 menekankan pada penggunaan tool untuk eksplorasi dan eksperimen awal. Di sini, keterampilan teknis tidak hanya dilihat sebagai kemampuan menjalankan kode, melainkan sebagai sarana untuk memvalidasi klaim teoretis. Penggunaan library seperti PyTorch, scikit-image, atau Hugging Face harus diarahkan untuk membangun pipeline dasar yang reproducible, sehingga hasil eksperimen dapat dijadikan referensi objektif dan mudah direplikasi.

Sementara itu, CPMK-5 dan CPMK-6 berfokus pada aspek penelitian tingkat lanjut. CPMK-5 menuntut mahasiswa untuk mulai merumuskan masalah penelitian dan mengidentifikasi research gap secara sistematis. Hal ini sejalan dengan arahan pada slide sebelumnya, di mana persiapan membaca paper kritis harus disertai dengan pencatatan limitation, asumsi yang belum teruji, atau failure case dari model baseline. CPMK-6 melengkapi hal tersebut dengan membangun peta literatur dan positioning awal penelitian, memastikan bahwa kontribusi ilmiah yang diusulkan berada dalam koridor state-of-the-art yang relevan.

Tiga kalimat penutup pada slide ini merangkum filosofi pembelajaran pertemuan ini. Peta riset memberikan konteks teoritis dan arah perkembangan bidang. Praktikum memberikan bukti empiris yang menguji validitas teori tersebut di dunia nyata. Research log berfungsi sebagai jembatan metodologis yang menghubungkan keduanya, mengubah serangkaian eksperimen ad-hoc menjadi proses penelitian yang transparan, terdokumentasi, dan dapat ditelusuri ulang.

Kaitan ini akan semakin kokoh saat kita masuk ke pertemuan berikutnya, di mana Anda akan menerapkan kerangka kerja ini pada pembacaan paper spesifik sesuai persiapan yang telah disebutkan. Selanjutnya, pada slide ringkasan, kita akan melihat kembali poin-poin kunci yang menjadi fondasi bagi seluruh perjalanan penelitian Anda selama mata kuliah ini berlangsung.

---

## Slide 041 - Ringkasan Materi Pertemuan 01

### Narasi

Slide ini menyajikan rangkuman inti dari seluruh materi Pertemuan 01. Sebagaimana ditekankan pada slide sebelumnya, peta riset memberikan konteks makro, praktikum menyediakan bukti empiris, dan research log menjadi pengikat sistematis di antara keduanya. Rangkuman berikut menegaskan prinsip-prinsip metodologis yang harus Anda jadikan acuan baku sebagai peneliti tingkat doktoral dalam bidang pengolahan citra digital dan computer vision.

Berikut adalah poin-poin kunci yang perlu Anda internalisasi:

- **Evolusi representasi visual:** Perkembangan metode pengolahan citra bukan sekadar pergantian algoritma, melainkan transformasi fundamental dalam cara kita merepresentasikan informasi visual. Pergeseran dari manipulasi piksel berbasis aturan eksplisit menuju learned representation, dan kini ke foundation models, menunjukkan bahwa kapasitas generalisasi model meningkat seiring kedalaman abstraksi fitur.
- **Peran handcrafted feature:** Meskipun arsitektur deep learning semakin dominan, ekstraksi fitur buatan tangan tetap berfungsi sebagai fondasi teoretis dan baseline komparatif yang sah. Tanpa baseline yang masuk akal, klaim novelty suatu model kehilangan bobot ilmiah.
- **Audit dataset sebelum modeling:** Kualitas keluaran model sangat bergantung pada integritas data masuk. Distribusi kelas yang tidak seimbang, degradasi kualitas gambar, bias sampling, serta shortcut pembelajaran dapat mendistorsi metrik evaluasi dan mengarah pada kesimpulan yang keliru. Inspeksi data adalah langkah wajib, bukan opsional.
- **Evaluasi berkelanjutan dengan failure analysis:** Metrik agregat seperti akurasi atau mAP tidak cukup untuk menilai kesiapan sebuah metode. Analisis kegagalan harus dilakukan secara sistematis karena pola error yang berulang justru sering menjadi sumber hipotesis baru dan perumusan research question yang tajam.
- **Research log sebagai infrastruktur penelitian:** Dokumentasi eksperimen secara berkala mengubah rangkaian uji coba ad-hoc menjadi proses penelitian yang transparan, terukur, dan dapat direplikasi. Catatan ini melacak jejak keputusan metodologis, konfigurasi hiperparameter, dan interpretasi hasil, sehingga memudahkan penyusunan argumen disertasi yang koheren.

Prinsip-prinsip ini membentuk siklus penelitian yang ketat: mulai dari pemahaman konteks literatur, validasi data, perbandingan baseline, evaluasi kritis, hingga dokumentasi sistematis. Penerapan disiplin ini akan menjadi pondasi utama saat Anda memasuki fase kajian literatur mendalam.

Dengan fondasi metodologis yang telah diringkas, kita siap melanjutkan ke tahap aplikasi praktis. Pada pertemuan berikutnya, kita akan membahas teknik critical paper reading dan strategi identifikasi research gap, di mana Anda akan langsung menerapkan prinsip audit data, analisis kegagalan, dan pembandingan baseline pada publikasi state-of-the-art terkini.

---

## Slide 042 - Penutup

### Narasi

Kita telah menyelesaikan pembahasan mengenai peta riset mutakhir dalam pengolahan citra digital. Evolusi metode dari manipulasi piksel konvensional menuju *learned representation* dan *foundation models* menegaskan bahwa cara kita merepresentasikan informasi visual terus berkembang seiring kemajuan komputasi dan arsitektur jaringan saraf. Meskipun model besar dan *deep learning* mendominasi publikasi terkini, teknik berbasis *handcrafted feature* tetap memegang peranan penting sebagai baseline yang masuk akal dan referensi fundamental dalam evaluasi empiris.

Sebelum memasuki tahap pemodelan, audit terhadap dataset merupakan langkah krusial yang tidak boleh dilewatkan. Distribusi kelas, kualitas gambar, adanya bias, hingga fenomena *shortcut learning* dapat secara signifikan memengaruhi validitas kesimpulan ilmiah. Oleh karena itu, evaluasi model tidak boleh berhenti pada pencapaian metrik numerik saja. Analisis kasus kegagalan (*failure analysis*) harus dilakukan secara sistematis, karena pola kesalahan yang konsisten sering kali menjadi sumber utama perumusan hipotesis dan pertanyaan penelitian yang bernilai novelty tinggi. Dokumentasi eksperimen melalui *research log* juga diperlukan untuk memastikan setiap iterasi pengujian berjalan transparan dan dapat direproduksi oleh peneliti lain.

Pemaparan pada slide sebelumnya telah merangkum poin-poin kunci tersebut sebagai landasan metodologis. Untuk pertemuan pertama ini, kita tutup dengan penekanan pada sikap kritis dan disiplin penelitian yang terstruktur. Pada pertemuan berikutnya, fokus kita akan beralih sepenuhnya ke praktik *critical paper reading* dan identifikasi *research gap*. Di sesi tersebut, mahasiswa akan dilatih untuk membedah metodologi paper terkini, memetakan celah penelitian yang belum tuntas, serta merumuskan *research question* yang memiliki posisi jelas terhadap *state-of-the-art* di bidang *computer vision* dan pengolahan citra digital.

Terima kasih atas perhatian dan partisipasi aktif Anda. Sampai jumpa pada diskusi paper berikutnya.
