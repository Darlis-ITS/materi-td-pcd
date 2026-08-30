# Narasi TD Pengolahan Citra Digital - Pertemuan 05

## Vision-Language Models dan Multimodal Representation

Sumber: markdown/pert05-vision-language-models-dan-multimodal-representation.md

---

## Slide 000 - Cover

### Narasi

Slide pembuka ini mengantar kita pada pembahasan inti mengenai *Vision-Language Models* dan representasi multimodal. Pada jenjang doktoral, topik ini bukan sekadar penerapan arsitektur, melainkan fondasi kritis untuk memahami bagaimana model dapat memetakan ruang fitur visual ke dalam ruang semantik bahasa secara terstruktur dan dapat diinterpretasikan.

Kita akan mengeksplorasi tiga pilar utama dalam materi ini:
- Mekanisme *cross-modal alignment* dan pencocokan fitur spasial-linguistik.
- Teknik *contrastive learning* antar pasangan gambar-teks serta optimasi fungsi kehilangan silang modalitas.
- Konstruksi *joint embedding space* yang memungkinkan korespondensi makna lintas modalitas untuk tugas *zero-shot classification* dan *retrieval*.

Pembahasan ini merupakan kelanjutan logis dari konsep *self-supervised learning* dan *foundation vision models*. Ketika representasi visual telah dipelajari secara mandiri melalui sinyal kontrastif atau pemaskeran, integrasi sinyal linguistik berfungsi sebagai pengawas struktural yang meningkatkan generalisasi, robustness, dan kemampuan transfer pengetahuan ke domain yang belum pernah dilihat.

Di slide berikutnya, kita akan memetakan posisi topik ini dalam alur perkuliahan, menunjukkan bagaimana transisi dari *single-modality* ke *multimodality* menjadi jembatan konseptual menuju aplikasi lanjutan seperti *image restoration* berbasis teks dan evaluasi *zero-shot* pada domain baru. Mari kita mulai eksplorasi teknis dan implikasi risetnya untuk pengembangan proposal disertasi Anda.

---

## Slide 001 - Posisi Pertemuan 05 dalam Rangkaian Perkuliahan

### Narasi

Pada slide ini, kita akan memetakan posisi Pertemuan 05 dalam keseluruhan alur perkembangan materi mata kuliah. Pemahaman kontekstual ini penting untuk menjaga koherensi konseptual, terutama mengingat bahwa topik vision-language models tidak berdiri sendiri, melainkan merupakan evolusi natural dari fondasi yang telah kita bangun sebelumnya.

Pertemuan 04 telah menjabarkan prinsip-prinsip self-supervised learning dan foundation vision models. Kita telah menganalisis bagaimana mekanisme contrastive learning dan masked image modeling memungkinkan model mempelajari representasi visual yang robust tanpa ketergantungan pada anotasi label manual. Arsitektur seperti DINO dan DINOv2 menunjukkan bahwa sinyal pengawas dapat dihasilkan secara internal dari augmentasi data dan konsistensi antar-view, menghasilkan embedding visual yang kaya makna struktural dan semantik.

Pertemuan 05 melakukan perluasan paradigmatik dari representasi unimodal tersebut ke ranah multimodal. Integrasi bahasa sebagai modalitas kedua mengubah cara model memproses informasi. Bahasa tidak lagi hanya berfungsi sebagai deskripsi pasif, melainkan sebagai sinyal pengawas aktif yang menyelaraskan ruang vektor visual dengan struktur linguistik. Proses penyelarasan ini melahirkan vision-language models, di mana kesamaan geometris antar-embedding citra dan teks menjadi dasar utama untuk tugas-tugas penalaran visual yang kompleks.

Konsep ini juga menjadi jembatan metodologis yang krusial menuju Pertemuan 06 tentang image restoration dan computational imaging. Representasi multimodal memberikan kerangka kerja baru untuk mengevaluasi kualitas persepsi manusia dan memandu pemulihan citra berbasis instruksi teks. Lebih jauh, protokol evaluasi zero-shot yang akan kita pelajari secara teknis pada slide-slide berikutnya akan menjadi standar baku untuk mengukur generalisasi model restoration pada domain data yang out-of-distribution, sebuah tantangan nyata dalam riset tingkat doktoral.

Dengan menempatkan topik ini dalam peta perkuliahan, diharapkan mahasiswa dapat melihat bagaimana transisi dari SSL ke multimodal learning bukan sekadar pergantian topik, melainkan langkah strategis dalam merumuskan research question yang relevan. Kemampuan melacak keterkaitan antar-metode ini akan menjadi fondasi utama saat kita memasuki fase analisis kritis paper dan perancangan eksperimen pada pertemuan-pertemuan selanjutnya.

---

## Slide 002 - Tujuan Pembelajaran dan Capaian Terkait

### Narasi

Pada slide ini, kita akan menguraikan tujuan pembelajaran spesifik yang menjadi fokus pertemuan kelima, serta bagaimana capaian tersebut selaras dengan kompetensi inti mata kuliah. Setelah pada pertemuan sebelumnya kita membahas representasi visual yang dipelajari secara *self-supervised* melalui DINO dan DINOv2, kini kita beralih ke paradigma multimodal di mana sinyal pengawas tidak lagi berasal dari label gambar itu sendiri, melainkan dari korespondensi antara citra dan teks alami.

Tujuan pertama adalah memahami konsep *contrastive image-text learning*. Mekanisme ini menjadi fondasi pelatihan vision-language model modern, di mana model diajarkan untuk memaksimalkan kesamaan fitur antara pasangan gambar-teks yang relevan dan meminimalkannya terhadap pasangan yang tidak sesuai. Dari sini, kita akan membedah arsitektur dan mekanisme kerja CLIP secara mendalam, termasuk bagaimana encoder visual dan encoder teks diselaraskan dalam ruang embedding bersama melalui fungsi kerugian kontrastif.

Selanjutnya, kita akan menganalisis mekanisme *zero-shot recognition* yang dimungkinkan oleh penyelarasan multimodal ini. Konsep kanonikalisasi label dan teknik *prompt engineering* akan dijelaskan sebagai jembatan antara representasi numerik dan makna linguistik, memungkinkan model melakukan inferensi pada kategori yang sama sekali tidak terlihat selama pelatihan. Di tingkat doktoral, analisis ini tidak berhenti pada penggunaan praktis, melainkan mencakup evaluasi kritis terhadap kekuatan dan keterbatasan model multimodal, seperti bias bahasa yang melekat pada data training, serta kesenjangan domain (*domain gap*) ketika model diaplikasikan pada distribusi data yang berbeda.

Untuk mendukung penelitian tingkat lanjut, mahasiswa juga dilatih merancang protokol evaluasi yang valid, baik untuk tugas *zero-shot classification* maupun *image-text retrieval*. Capaian pembelajaran ini secara langsung berkontribusi pada tiga kompetensi inti: CPMK-1 melalui analisis kritis terhadap performa dan kelemahan model multimodal, CPMK-4 melalui implementasi eksperimental memanfaatkan CLIP untuk skenario zero-shot dan retrieval, serta CPMK-5 yang mendorong identifikasi celah penelitian terkait bias, optimasi prompt, dan generalisasi lintas domain.

Pembahasan objektif ini akan menjadi landasan logis sebelum kita masuk ke motivasi mendasar mengapa menghubungkan citra dan teks menjadi paradigma yang transformatif, sekaligus menjawab keterbatasan fundamental dari model visual konvensional yang hanya bergantung pada himpunan label tertutup dan tidak mampu menalar konsep baru tanpa pelatihan ulang.

---

## Slide 003 - Motivasi: Mengapa Menghubungkan Citra dan Teks?

### Narasi

Pada slide ini, kita akan membahas motivasi mendasar di balik pengembangan model vision-language, atau mengapa dalam penelitian terkini kita perlu menghubungkan representasi visual dengan teks alami. Sebagaimana disinggung pada slide tujuan pembelajaran sebelumnya, pemahaman terhadap konsep *contrastive image-text learning* dan arsitektur model seperti CLIP menjadi prasyarat analitis untuk capai pembelajaran mata kuliah ini. Namun, sebelum masuk ke detail arsitektural, kita perlu mengidentifikasi celah metodologis yang mendorong lahirnya paradigma baru ini.

Model visual konvensional, baik yang berbasis klasifikasi tradisional maupun *deep learning* bersupervisi penuh, sangat bergantung pada label dari *closed-set*. Kinerja model secara inheren dibatasi oleh kualitas, kelengkapan, dan cakupan label yang tersedia selama fase pelatihan. Akibatnya, model secara fundamental tidak mampu mengenali atau menalar objek dan konsep yang tidak pernah muncul sebagai kategori pelatihan. Batasan ini menjadi hambatan signifikan ketika kita merancang sistem yang diharapkan dapat beradaptasi dengan dinamika dunia nyata yang selalu berkembang.

Di sinilah keunggulan supervisi menggunakan bahasa alami menjadi poin kritis. Bahasa alami menawarkan kekayaan semantik yang jauh melampaui daftar label statis. Struktur linguistik memungkinkan pembentukan makna yang bersifat kombinatorial dan hampir tak terbatas, sehingga teks dapat bertindak sebagai sinyal pengawas yang fleksibel dan terbuka (*open-ended*). Dengan memanfaatkan bahasa sebagai jembatan semantik, model tidak lagi terjebak pada kategori tertutup, melainkan mampu mempelajari relasi konseptual yang lebih abstrak dan generatif.

Ide inti dari pendekatan ini terletak pada pelatihan model untuk memetakan hubungan langsung antara gambar dan deskripsinya dalam bahasa alami. Ketika model berhasil mempelajari korespondensi ini, ia memperoleh kapasitas penalaran bawaan. Konsep baru yang belum pernah dilihat selama pelatihan dapat dipahami hanya melalui deskripsi linguistiknya, tanpa memerlukan proses *fine-tuning* atau pengumpulan data tambahan yang memakan biaya komputasi tinggi.

Penjelasan motivasi ini menjadi landasan konseptual yang esensial sebelum kita beralih ke implementasi teknisnya. Pada slide berikutnya, kita akan mendefinisikan apa itu *multimodal representation*, bagaimana ruang embedding bersama dibangun, serta bagaimana kesamaan makna antar modalitas diukur secara matematis melalui operasi seperti *cosine similarity*. Transisi ini akan memperjelas bagaimana ide teoretis tadi diwujudkan dalam bentuk vektor yang dapat diproses oleh jaringan saraf.

---

## Slide 004 - Apa yang Dimaksud dengan Multimodal Representation?

### Narasi

Pada slide ini, kita akan membahas konsep fundamental yang menjadi tulang punggung model vision-language, yaitu **multimodal representation**. Setelah pada slide sebelumnya kita mengidentifikasi keterbatasan model visual konvensional yang terjebak dalam *closed-set* label, langkah logis selanjutnya adalah memahami bagaimana kita dapat memetakan data dari modalitas yang berbeda ke dalam satu kesatuan matematis yang koheren.

Secara definisi, **multimodal representation** merujuk pada representasi bersama (*joint representation*) yang memproyeksikan data dari berbagai modalitas—dalam konteks kuliah ini khususnya citra dan teks—ke dalam ruang vektor yang sama. Tujuannya bukan sekadar mengekstrak fitur masing-masing modalitas secara terpisah, melainkan menciptakan ruang embedding di mana kesamaan semantik menjadi pengatur utama struktur data.

Prinsip inti dari konstruksi ruang ini terletak pada **tujuan alignment**. Dalam ruang embedding tersebut, vektor representasi untuk data yang memiliki makna serupa harus ditempatkan sangat berdekatan. Sebaliknya, vektor untuk data yang bermakna berbeda harus dijauhkan satu sama lain. Proses penataan jarak vektor inilah yang memungkinkan model belajar hubungan lintas modalitas secara otomatis, biasanya melalui optimisasi fungsi kerugian seperti contrastive loss atau InfoNCE.

Untuk memvisualisasikan konsep ini, perhatikan ilustrasi sederhana berikut:
```text
+---------------------------+
|     Ruang Embedding        |
|                           |
|  Citra "kucing"  ~  Teks "a photo of a cat" |
|  Citra "mobil"   ~  Teks "a car"            |
+---------------------------+
```
Ilustrasi ini menunjukkan bahwa ketika sebuah gambar kucing dan deskripsi tekstualnya diproses oleh encoder masing-masing, hasil akhirnya adalah dua titik vektor yang saling mendekat dalam ruang yang sama. Hal yang sama berlaku untuk pasangan citra mobil dan teksnya. Jarak geometris antar titik ini secara implisit merepresentasikan tingkat kesamaan semantik tanpa memerlukan anotasi manual.

Kemampuan ini mengubah cara kita melakukan pencocokan atau pencarian informasi. Alih-alih mengandalkan aturan hard-coded atau sistem berbasis kata kunci yang kaku, model kini dapat membandingkan citra dan teks secara langsung melalui metrik matematika sederhana, yaitu **cosine similarity**. Nilai kosinus yang mendekati 1 menandakan kesamaan makna yang tinggi, sementara nilai mendekati 0 atau negatif menandakan ketidakcocokan. Mekanisme ini menjadi dasar kerja dari model-model seperti CLIP dan ALIGN yang banyak digunakan dalam riset terkini.

Pemahaman tentang representasi multimodal dan mekanisme alignment ini menjadi fondasi kritis sebelum kita beralih ke perubahan paradigma supervisi. Jika pada slide sebelumnya kita telah melihat mengapa label kategoris tradisional tidak lagi memadai untuk menangkap kompleksitas dunia nyata, maka konsep ruang vektor bersama ini menjawab pertanyaan teknis tentang *bagaimana* bahasa alami dapat menggantikan label tersebut secara fungsional. Pada slide berikutnya, kita akan mendalami pergeseran konkret dari supervisi label klasik menuju supervisi berbasis bahasa alami, lengkap dengan dampak metodologisnya terhadap desain dataset, arsitektur encoder, dan strategi evaluasi model.

---

## Slide 005 - Paradigma Supervisi: Dari Label Kategoris ke Bahasa Alami

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana representasi multimodal memetakan citra dan teks ke dalam ruang vektor yang sama, sehingga kesamaan makna dapat diukur melalui kedekatan geometris atau cosine similarity. Langkah logis berikutnya adalah memahami bagaimana mekanisme supervisi dirancang untuk mendorong penyelarasan tersebut. Di sinilah terjadi pergeseran paradigma mendasar dari label kategoris tradisional menuju supervisi berbasis bahasa alami.

Secara historis, pembelajaran mesin vision bergantung pada skema supervisi klasik yang diilustrasikan oleh struktur dataset ImageNet. Gambar dikaitkan dengan label tunggal seperti "cat" atau "dog", yang kemudian dikonversi menjadi vektor one-hot. Skema ini memiliki keterbatasan inheren: jumlah kelas bersifat tertutup, fleksibilitas semantik sangat terbatas, dan model hanya belajar membedakan kategori diskrit tanpa memahami nuansa kontekstual.

Paradigma baru muncul ketika peneliti mulai memanfaatkan pasangan citra-teks berskala besar dari internet, sebagaimana direpresentasikan dalam blok kode kedua. Alih-alih memaksa gambar ke dalam kotak kategori statis, deskripsi teks alami seperti "a photo of a tabby cat sitting on a window sill" digunakan sebagai sinyal supervisi. Teks ini diproses oleh text encoder untuk menghasilkan vektor representasi yang kaya konteks. Akibatnya, konsep visual tidak lagi dibatasi oleh kamus kelas yang tetap, melainkan terbuka terhadap ekspresi linguistik yang komposisional dan tak terhingga.

Pergeseran ini membawa dampak metodologis yang nyata, yang dapat dirangkum dalam empat aspek kunci:
- **Jumlah kelas**: beralih dari himpunan tertutup yang tetap menjadi ruang konseptual yang tidak terbatas.
- **Tingkat semantik**: berkembang dari token tunggal menjadi kalimat lengkap yang memuat atribut, aksi, dan konteks spasial.
- **Struktur relasional**: hubungan antar kelas yang sebelumnya hilang kini tersirat melalui koherensi linguistik dan dependensi sintaksis.
- **Kemampuan zero-shot**: berubah dari pendekatan ad-hoc menjadi sifat bawaan model berkat generalisasi lintas modalitas.

Transisi dari label kategoris ke bahasa alami ini bukan sekadar perubahan format anotasi, melainkan fondasi arsitektural bagi model foundation modern. Dengan memahami bahwa supervisi linguistik membuka ruang representasi yang lebih halus dan scalable, kita siap menelaah implementasi konkretnya. Slide selanjutnya akan mengupas gagasan utama dari model CLIP, yang secara elegan mengoperasionalkan prinsip ini melalui pembelajaran kontrastif pada ratusan juta pasangan gambar-teks.

---

## Slide 006 - Model CLIP: Gagasan Utama

### Narasi

Pada slide sebelumnya, kita telah menelaah pergeseran paradigma supervisi dari label kategoris yang statis menuju pemanfaatan bahasa alami sebagai sinyal pembelajaran. Transformasi ini memungkinkan model menangkap struktur semantik yang lebih kompleks dan mendukung kemampuan zero-shot secara inheren. Sebagai manifestasi konkret dari konsep tersebut, slide ini memperkenalkan gagasan inti di balik model CLIP (*Contrastive Language-Image Pre-training*), karya seminal dari Radford dkk. pada tahun 2021.

CLIP dikembangkan dengan tujuan mempelajari representasi visual yang bersifat highly transferable melalui supervisi berbasis bahasa alami. Model ini secara efektif mengatasi batasan dataset dengan label tertutup dengan memanfaatkan pasangan gambar-teks dalam skala besar. Sumber data CLIP diperoleh melalui proses *web scraping* yang menghasilkan sekitar empat ratus juta pasangan gambar dan deskripsi teks terkait. Pendekatan ini menghilangkan kebutuhan akan anotasi manual yang terstruktur per kelas, sehingga model terpapar pada keragaman linguistik dan konteks visual yang jauh lebih luas.

Prinsip pembelajaran CLIP dapat diuraikan menjadi empat langkah konseptual yang saling berkaitan:
- Gambar diproses oleh sebuah encoder untuk diubah menjadi vektor representasi berdimensi tetap.
- Teks deskriptif yang sesuai juga di-encode menjadi vektor dalam ruang dimensi yang setara.
- Model dilatih untuk memaksimalkan nilai kesamaan (*similarity*) antara pasangan gambar dan teks yang benar-benar berkorespondensi.
- Objective kontrastif diterapkan untuk mendorong pemisahan yang tegas antara pasangan yang relevan dan pasangan yang tidak relevan di ruang embedding bersama.

Dengan memahami motivasi, skala data, dan mekanisme kontrastif yang mendasari CLIP, kita kini memiliki fondasi konseptual yang kuat. Pada slide berikutnya, kita akan mengonkretkan gagasan ini ke dalam detail implementasi teknis dengan membahas arsitektur CLIP, meliputi spesifikasi encoder, lapisan proyeksi, normalisasi L2, serta perhitungan cosine similarity yang menjadi tulang punggung pencocokan multimodal.

---

## Slide 007 - Arsitektur CLIP

### Narasi

Pada slide sebelumnya, kita telah membahas gagasan fundamental dari model CLIP, yaitu penggunaan supervisi kontrastif pada pasangan gambar dan teks dalam skala besar untuk menghasilkan representasi yang dapat ditransfer. Slide ini akan mengurai arsitektur teknis yang mendasari ide tersebut, sehingga kita memahami bagaimana ruang representasi bersama sebenarnya dibangun secara komputasional.

Arsitektur CLIP terdiri dari tiga komponen utama yang bekerja secara terintegrasi:
- **Image Encoder**: mengubah citra input menjadi vektor representasi berdimensi tetap. Implementasinya dapat menggunakan arsitektur klasik seperti ResNet atau Vision Transformer (ViT).
- **Text Encoder**: memproses input linguistik menjadi vektor semantik, umumnya diimplementasikan sebagai Transformer bertipe GPT-style.
- **Proyeksi**: lapisan linear layer yang menyelaraskan dimensi hasil encoding dari kedua modalitas agar berada dalam ruang bersama.

Setelah proses encoding selesai, setiap vektor hasil embedding mengalami **L2 normalization**. Normalisasi ini memastikan bahwa semua vektor berada pada hypersphere dengan norma satu, sehingga perhitungan kesamaan antar modalitas menjadi stabil dan tidak terpengaruh oleh magnitudo vektor. Representasi akhir dari citra ditandai sebagai vektor **I**, sedangkan representasi teks ditandai sebagai vektor **T**. Kesamaan antara kedua modalitas kemudian dihitung menggunakan metrik **cosine similarity**, yang secara matematis mengukur sudut antara dua vektor dalam ruang berdimensi tinggi.

Dengan arsitektur dan mekanisme normalisasi yang telah dijelaskan, kita siap melangkah ke tahap operasionalnya. Pada slide berikutnya, kita akan membahas secara rinci proses pelatihan CLIP, mulai dari pembentukan batch pasangan `(citra, teks)`, konstruksi matriks kesiman `N x N`, hingga penerapan pseudocode yang menggabungkan temperature scaling dan fungsi cross-entropy untuk mengoptimalkan objective kontrastif tersebut.

---

## Slide 008 - Proses Pelatihan CLIP

### Narasi

Setelah sebelumnya kita menguraikan komponen arsitektural CLIP, mulai dari Image Encoder berbasis ResNet atau ViT, Text Encoder bertipe Transformer, hingga proses normalisasi L2 pada embedding, slide ini menyoroti bagaimana kedua modalitas tersebut disatukan secara optimal selama fase pelatihan. Fokus utamanya beralih dari struktur statis ke dinamika optimasi yang memungkinkan model belajar korespondensi semantik antar domain visual dan linguistik.

Data pelatihan disajikan dalam bentuk batch berisi pasangan `(citra, teks)` dengan ukuran `N`. Setiap sampel dalam batch merupakan korespondensi langsung antara konten visual dan caption tekstualnya, seperti contoh pasangan `(gambar kucing, "a photo of a cat")`. Kualitas dan skalabilitas dataset berpasangan ini menjadi determinan utama dalam kemampuan model menangkap variasi makna yang kompleks.

Inti dari proses pembelajaran terletak pada *contrastive objective*. Model secara simultan menghitung matriks kesamaan berukuran `N x N` antara seluruh embedding citra dan seluruh embedding teks dalam batch yang sama. Pasangan yang benar secara semantik menempati posisi diagonal matriks, sedangkan pasangan yang tidak relevan tersebar pada area *off-diagonal*. Strategi optimasinya menuntut model untuk memaksimalkan nilai kesamaan pada diagonal sekaligus menekan nilai pada elemen off-diagonal, sehingga ruang embedding terdistribusi secara diskriminatif.

Implementasi komputasionalnya dapat direpresentasikan melalui pseudocode berikut:
```python

### image_features, text_features: hasil encode batched

logits = cosine_similarity(image_features, text_features) / temperature
loss = cross_entropy(logits, target_diagonal)
```
Parameter `temperature` berfungsi sebagai skalar penyetel ketajaman distribusi probabilitas pada matriks logit. Suhu yang lebih kecil akan menghasilkan distribusi yang lebih ekstrem, mempercepat konvergensi dengan memberi bobot dominan pada pasangan yang benar dan menekan distraktor. Fungsi *cross-entropy* kemudian diterapkan dengan vektor target yang hanya menunjuk ke indeks diagonal, memastikan gradien backpropagation fokus sepenuhnya pada penyelarasan pasangan yang valid.

Pendekatan ini memanfaatkan *implicit negative sampling* dari seluruh batch tanpa memerlukan pasangan negatif buatan, sehingga efisiensi komputasional meningkat signifikan. Mekanisme ini juga menjadi jembatan konseptual untuk slide berikutnya, di mana alur propagasi forward-backward akan divisualisasikan secara eksplisit, termasuk penjelasan mengapa loss dihitung secara simetris dalam dua arah: *image-to-text* dan *text-to-image*.

---

## Slide 009 - Diagram Alur Pelatihan CLIP

### Narasi

Diagram pada slide ini menyajikan representasi visual lengkap dari pipeline pelatihan CLIP, yang merupakan kelanjutan logis dari penjelasan objektif kontrasitif pada slide sebelumnya. Alur ini menegaskan bahwa pembelajaran multimodal dilakukan melalui dua jalur komputasi yang berjalan secara paralel dan sinkron, tanpa adanya interaksi dini antara domain citra dan teks.

Input berupa batch berisi N citra dan N kalimat diproses secara independen melalui Image Encoder dan Text Encoder. Kedua encoder biasanya diimplementasikan menggunakan arsitektur deep learning yang mapan, seperti CNN atau Vision Transformer untuk citra, serta Transformer berbasis self-attention untuk teks. Output dari masing-masing encoder adalah vektor embedding berdimensi d yang merepresentasikan fitur semantik dan struktural dari setiap sampel.

Sebelum masuk ke tahap perhitungan kesamaan, semua embedding mengalami normalisasi L2. Langkah ini memproyeksikan setiap vektor ke permukaan bola satuan, sehingga perhitungan kesamaan selanjutnya hanya bergantung pada orientasi sudut antarvektor, bukan pada skala atau magnitudonya. Hasil normalisasi kemudian dikalikan secara dot-product untuk menghasilkan matriks kesamaan berukuran N kali N, di mana elemen diagonal merepresentasikan pasangan citra-teks yang benar, sedangkan elemen off-diagonal menunjukkan pasangan tidak relevan.

Loss dihitung secara dua arah: image-to-text dan text-to-image. Artinya, model secara simultan mendorong kesamaan antara gambar dan caption yang sesuai, sekaligus menekan kesamaan silang dari perspektif teks terhadap gambar lain. Gradien dari kedua arah loss tersebut digabungkan dan dialirkan mundur untuk memperbarui bobot Image Encoder maupun Text Encoder secara bersamaan, sehingga ruang embedding terus menyatu dan semakin diskriminatif.

Untuk mendalami landasan matematis dari langkah normalisasi dan perhitungan cosine similarity yang menjadi inti dari matriks kesamaan ini, slide berikutnya akan menguraikan formulasi eksplisit serta mekanisme pengaturan parameter suhu. Pembahasan tersebut akan memberikan kerangka analitis yang diperlukan untuk menelaah stabilitas numerik, laju konvergensi, dan potensi penyesuaian hyperparameter pada eksperimen tingkat penelitian doktoral.

---

## Slide 010 - Normalisasi dan Kesamaan Kosinus

### Narasi

Pada slide ini kita akan menguraikan secara matematis mengapa normalisasi vektor dan kesamaan kosinus menjadi fondasi kritis dalam representasi multimodal. Langkah ini merupakan kelanjutan langsung dari blok `Normalize` pada diagram alur pelatihan CLIP di slide sebelumnya, yang kini akan kita bedah mekanisme optimasinya.

Kesamaan kosinus secara fundamental hanya bergantung pada sudut antar vektor, bukan pada magnitudo atau panjangnya. Dengan menerapkan normalisasi L2, setiap embedding gambar maupun teks diubah menjadi vektor satuan. Hal ini menyebabkan jarak Euclidean antara dua vektor yang telah dinormalisasi menjadi sebanding langsung dengan sudut di antara mereka. Akibatnya, skala embedding menjadi terkontrol dan stabil selama proses backpropagation, sehingga gradien tidak mengalami eksploding atau vanishing akibat perbedaan magnitudo yang ekstrem antar batch.

Secara formal, perhitungan kesamaan kosinus dinyatakan sebagai hasil kali titik dibagi dengan perkalian norma masing-masing vektor. Namun, setelah kedua vektor dinormalisasi menjadi I' dan T', rumusnya menyederhana menjadi produk titik langsung antara I' dan T'. Penyederhanaan ini tidak hanya mempercepat komputasi matriks kesamaan berukuran N x N, tetapi juga memastikan bahwa semua nilai berada dalam rentang [-1, 1], yang sangat ideal untuk fungsi softmax dan cross-entropy pada lapisan head classifier.

Parameter suhu atau temperature memainkan peran strategis dalam tahap ini. Logits kesamaan yang dihasilkan sebelum softmax akan dibagi dengan nilai temperature yang dapat dipelajari (learnable parameter). Temperatur berfungsi sebagai pengatur kecerunan distribusi probabilitas. Ketika nilai temperatur kecil, distribusi menjadi lebih tajam, memaksa model untuk memberikan bobot lebih besar pada pasangan yang paling mirip dan menekan noise dari pasangan yang kurang relevan. Sebaliknya, temperatur yang lebih besar menghasilkan distribusi yang lebih halus, yang berguna pada tahap awal pelatihan agar model tidak terlalu cepat overconfident terhadap pola yang belum optimal.

Pemahaman mendalam tentang normalisasi dan pengaturan suhu ini menjadi prasyarat penting sebelum kita beralih ke penerapan praktisnya. Pada slide berikutnya, kita akan melihat bagaimana skor kesamaan kosinus yang telah distabilkan ini diimplementasikan secara langsung dalam klasifikasi zero-shot, di mana model harus mencocokkan embedding citra dengan deskripsi teks kelas tanpa pernah melihat contoh visualnya selama pelatihan.

---

## Slide 011 - Zero-Shot Classification: Konsep Dasar

### Narasi

Pada slide ini, kita membahas konsep dasar dari *zero-shot classification*, sebuah paradigma yang memungkinkan model mengklasifikasikan gambar ke dalam kategori yang sama sekali tidak muncul selama proses pelatihan. Berbeda dengan pendekatan tradisional yang bergantung pada data berlabel secara eksplisit, *zero-shot* mengandalkan representasi semantik yang dipelajari dari teks deskriptif. Kemampuan ini menjadi fondasi penting bagi model *vision-language*, karena menghilangkan ketergantungan pada pengumpulan dan anotasi dataset yang masif untuk setiap kelas baru.

Alur kerja *zero-shot classification* menggunakan CLIP dapat diuraikan secara sistematis sebagai berikut:
1. Tentukan label kelas target, misalnya `"cat"`, `"dog"`, atau `"car"`.
2. Ubah setiap label menjadi kalimat lengkap menggunakan *prompt template*, seperti `"a photo of a {label}"`.
3. Masukkan teks tersebut ke dalam *text encoder* untuk menghasilkan embedding teks.
4. Proses citra input melalui *image encoder* untuk mendapatkan embedding gambar.
5. Hitung kesamaan antar vektor menggunakan metrik seperti *cosine similarity*, yang telah dijelaskan pada slide sebelumnya.
6. Pilih kelas dengan skor kesamaan tertinggi sebagai prediksi akhir.

Langkah transformasi label menjadi kalimat bukan sekadar formalitas sintaksis, melainkan mekanisme krusial untuk menyelaraskan input dengan ruang vektor yang telah dibentuk selama pra-pelatihan. Tanpa penyesuaian struktur teks, model kesulitan memetakan makna semantik dari kata tunggal ke dalam representasi multimodal yang kompleks.

Karena kedua encoder beroperasi dalam ruang fitur yang sejajar, perhitungan kesamaan dapat dilakukan secara langsung tanpa memerlukan lapisan classifier terlatih. Mekanisme ini mendemonstrasikan bagaimana *cross-modal alignment* menggantikan fungsi jaringan klasifikasi konvensional, sekaligus membuka peluang generalisasi ke domain yang belum pernah dilihat.

Keberhasilan seluruh pipeline ini sangat bergantung pada desain teks input. Penggunaan templat yang tepat dapat meningkatkan diskriminasi antar kelas secara signifikan. Pembahasan lebih lanjut mengenai strategi pemilihan dan adaptasi *prompt template* akan kita bahas pada slide berikutnya, yang menyoroti bagaimana variasi konteks linguistik mempengaruhi akurasi prediksi.

---

## Slide 012 - Peran Prompt Template pada Zero-Shot

### Narasi

Pada slide sebelumnya, kita telah menguraikan alur kerja klasifikasi zero-shot menggunakan model CLIP, di mana citra dan deskripsi teks kelas dipetakan ke ruang embedding bersama. Salah satu komponen kritis dalam pipeline tersebut adalah transformasi label kelas tunggal menjadi kalimat deskriptif sebelum proses encoding. Langkah ini bukan sekadar konvensi penulisan, melainkan kebutuhan arsitektural yang bersumber dari cara model vision-language dilatih pada pasangan gambar-teks berskala besar.

Model seperti CLIP tidak mempelajari representasi dari kata kunci terisolasi. Sebaliknya, jaringan menyerap pola linguistik yang mendominasi corpus pelatihan, di mana frasa pembuka seperti `"a photo of a"` atau `"an image of"` menciptakan prior struktural yang stabil. Tanpa templat, input teks mentah seperti `"mobil"` atau `"kucing"` akan menghasilkan embedding yang kurang koheren dan sulit sejajar dengan manifold visual yang dipelajari selama pretraining. Templat berfungsi sebagai jembatan semantik yang menstabilkan aktivasi neuron dan meningkatkan kesamaan kosinus antara fitur gambar dan fitur teks.

Berikut adalah variasi templat yang umum digunakan beserta dampak selektifnya terhadap representasi:
- `"a photo of a {label}"`: Templat umum yang cukup robust untuk dataset standar dan baseline zero-shot.
- `"a photo of a {label}, a type of {superclass}"`: Menambahkan konteks hierarkis untuk memperkuat disambiguasi semantik antar kelas yang mirip.
- `"a satellite image of {label}"` dan `"a medical image of {label}"`: Spesifik domain, sangat efektif ketika domain target menyimpang signifikan dari distribusi data pretraining.
- Templat yang diadaptasi secara empiris: Seringkali memberikan peningkatan metrik pada tugas niche, meskipun memerlukan validasi silang ketat untuk menghindari overfitting linguistik.

Dari perspektif penelitian tingkat doktoral, eksplorasi prompt template bukan hanya praktik tuning heuristik, melainkan bagian fundamental dari studi tentang multimodal alignment dan bias bahasa dalam foundation models. Memahami bagaimana sintaks teks memengaruhi distribusi embedding membuka peluang untuk merancang mekanisme prompting yang adaptif atau bahkan teroptimasi secara diferensial. Ketika konsep ini dipahami secara kualitatif, langkah logis berikutnya adalah merumuskannya secara matematis. Pada slide berikutnya, kita akan membahas rumus formal probabilitas kelas dalam zero-shot classification, serta perbedaan mendasar antara pendekatan berbasis teks ini dengan klasifikasi konvensional yang bergantung pada bobot terlatih.

---

## Slide 013 - Rumus Zero-Shot Classification

### Narasi

Setelah pada slide sebelumnya kita membahas bagaimana prompt template mengubah label tunggal menjadi kalimat deskriptif yang selaras dengan distribusi data pelatihan, kini kita akan melihat bagaimana deskripsi teks tersebut secara matematis dioperasionalkan untuk melakukan klasifikasi zero-shot. Pendekatan ini menghilangkan ketergantungan pada bobot kelas yang dipelajari secara empiris, dan menggantinya dengan pencocokan semantik di ruang representasi bersama.

Untuk setiap himpunan kelas potensial $C = \{c_1, c_2, ..., c_K\}$, kita menyusun deskripsi teks $t_i$ menggunakan templat yang telah dikonfigurasi. Setiap $t_i$ kemudian di-encode oleh bagian teks dari model vision-language, menghasilkan vektor embedding $w_i$. Kumpulan vektor ini membentuk matriks bobot kelas $W = [w_1, w_2, ..., w_K]$ dengan dimensi $K \times d$, di mana $d$ merepresentasikan dimensi ruang fitur bersama. Matriks $W$ bersifat statis pasca-encoding dan tidak mengalami pembaruan gradien selama fase inferensi.

Pada tahap prediksi, fitur citra $x$ diekstraksi oleh encoder gambar menjadi vektor $f(x)$. Skor kecocokan antara citra dan setiap kelas dihitung melalui operasi dot product yang diskalakan oleh parameter suhu, sebagaimana dirumuskan pada slide:
```text
Probabilitas kelas untuk citra x:

p(y = ci | x) = softmax( f(x) . w_i / temperature )

prediksi = argmax_i p(y = ci | x)
```
Pembagian dengan nilai *temperature* berperan sebagai pengontrol ketajaman distribusi probabilitas, mencegah overconfidence ketika kesamaan semantik rendah. Fungsi softmax kemudian menormalisasi seluruh skor menjadi peluang yang saling melengkapi, dan operasi argmax memilih indeks kelas dengan skor tertinggi sebagai prediksi akhir.

Mekanisme ini menunjukkan perbedaan mendasar dibandingkan klasifikasi konvensional. Pada arsitektur tradisional, bobot kelas dipelajari langsung dari data melalui backpropagation, sehingga penambahan kategori baru mewajibkan proses retraining lengkap dan pengambilan data tambahan. Sebaliknya, pada skema zero-shot berbasis CLIP, bobot kelas sepenuhnya diturunkan dari representasi teks. Hal ini memungkinkan perluasan vocabulari secara instan tanpa contoh visual, menjadikannya sangat relevan untuk aplikasi dengan dinamika kelas tinggi atau domain dengan biaya anotasi yang prohibitif.

Prinsip pencocokan skor antar fitur citra dan embedding teks ini juga menjadi fondasi struktural untuk tugas retrieval multimodal. Sebagaimana akan kita kaji pada slide berikutnya, mekanisme perbandingan kesamaan ini tidak terikat pada label eksplisit, melainkan dapat diperluas ke pencarian berbasis konten di mana kandidat berupa caption, metadata, atau dokumen bebas. Transisi dari klasifikasi terstruktur menuju retrieval akan mengungkap bagaimana representasi bersama ini mendukung fleksibilitas sistem dalam skala besar.

---

## Slide 014 - Image-Text Retrieval: Definisi dan Arah

### Narasi

Pada slide ini, kita menggeser fokus dari klasifikasi zero-shot yang telah dibahas pada slide sebelumnya ke salah satu aplikasi inti dalam arsitektur vision-language, yaitu *image-text retrieval*. Jika pada klasifikasi zero-shot model dipaksa memilih satu label dari himpunan kelas tertutup menggunakan fungsi softmax, retrieval bekerja dengan prinsip yang lebih terbuka dan fleksibel. Tugas ini tidak memerlukan label eksplisit, melainkan mengandalkan pencocokan semantik langsung antara query dan kumpulan kandidat.

Secara definisi, terdapat dua arah komputasi yang harus dipahami. Pertama, *image-to-text retrieval*, di mana citra berfungsi sebagai query untuk menemukan teks paling relevan dari basis data teks. Kedua, *text-to-image retrieval*, yang beroperasi secara terbalik: teks menjadi query untuk menyaring gambar yang paling sesuai. Kedua arah ini memanfaatkan representasi vektor yang dihasilkan oleh encoder modalitas masing-masing agar dapat dibandingkan dalam satu ruang kesamaan.

Alur pemrosesan pada kedua arah tersebut mengikuti pipeline retrieval standar. Query, baik berupa citra maupun teks, terlebih dahulu di-*encode* menjadi vektor fitur. Vektor query kemudian dibandingkan dengan seluruh kandidat dalam koleksi melalui metrik kesamaan, umumnya *cosine similarity*. Hasil perbandingan diurutkan berdasarkan skor tertinggi, sehingga kandidat dengan jarak geometris terdekat di ruang vektor muncul di posisi teratas. Tidak ada mekanisme probabilitas kelas atau normalisasi antar-kelas seperti pada klasifikasi konvensional.

Perbedaan mendasar dengan zero-shot classification terletak pada sifat kandidat dan tujuan aplikasinya. Pada retrieval, kandidat dapat berupa kalimat deskriptif bebas, caption natural, hingga paragraf dokumenter, sehingga tidak terikat pada skema kelas statis. Karakteristik ini menjadikan retrieval sebagai komponen kritis untuk *content-based search*, kurasi dataset skala besar, serta validasi kesesuaian semantik antar modalitas. Dari perspektif riset doktoral, performa retrieval sering dijadikan proxy metric untuk mengevaluasi kualitas representasi multimodal sebelum model diterapkan pada downstream task yang lebih spesifik.

Konsep bagaimana ruang vektor bersama ini dibentuk, distabilkan, dan dioptimalkan akan kita bahas secara mendalam pada slide berikutnya. Di sana, kita akan mengurai tiga properti fundamental—*alignment*, *uniformity*, dan *compositionality*—yang menjadi landasan matematis agar model mampu melakukan retrieval, klasifikasi zero-shot, dan penelusuran berbasis deskripsi secara simultan dan akurat.

---

## Slide 015 - Representasi Embedding sebagai Jembatan Modalitas

### Narasi

Pada slide sebelumnya, kita telah menguraikan mekanisme image-text retrieval yang mengandalkan proses encoding dan perhitungan cosine similarity untuk mencocokkan query dengan kandidat. Agar pembandingan lintas modalitas ini berjalan efisien dan akurat, kita memerlukan fondasi matematis yang konsisten, yaitu pembentukan ruang embedding bersama.

Konsep intinya terletak pada proyeksi vektor: citra dan teks dari pasangan yang semantik cocok harus dipetakan ke wilayah yang berdekatan di dalam ruang vektor yang sama. Ketika model berhasil mempelajari representasi ini, data dengan makna serupa namun berasal dari modalitas berbeda akan tetap memiliki jarak geometris yang rapat. Kondisi ini memungkinkan model melakukan operasi perbandingan langsung antar modalitas tanpa memerlukan layer transformasi tambahan yang kompleks.

Agar ruang embedding tersebut fungsional, terdapat tiga properti fundamental yang harus dicapai oleh arsitektur vision-language modern. Pertama, **Alignment**, yang menjamin pasangan gambar-teks yang relevan memiliki skor kesamaan vektor yang tinggi. Kedua, **Uniformity**, memastikan distribusi vektor menyebar secara merata di seluruh manifold sehingga mencegah kolapsnya representasi menjadi klaster padat yang mengurangi daya diskriminatif. Ketiga, **Compositionality**, di mana gabungan frasa atau kata sederhana dapat direpresentasikan sebagai kombinasi linear atau non-linear yang mempertahankan makna komposisional asli.

Dampak dari penguasaan ketiga properti ini sangat signifikan bagi ekosistem computer vision. Satu model yang telah dilatih dengan prinsip-prinsip tersebut mampu menjalankan berbagai tugas multimodal secara simultan. Mulai dari klasifikasi zero-shot tanpa parameter tuning, retrieval berbasis konten, pencarian menggunakan deskripsi natural language, hingga scoring kesesuaian visual-linguistik. Inilah alasan mengapa framework seperti CLIP dan arsitektur foundation model lainnya menjadi standar de facto dalam riset terkini.

Namun, keberhasilan konstruktif ruang embedding ini membuka celah kritik metodologis yang akan kita eksplorasi pada slide berikutnya. Kedekatan vektor antara representasi visual dan linguistik apakah benar-benar menandakan pemahaman semantik yang mendalam, atau hanya merupakan cerminan dari korelasi permukaan dan bias statistik dalam corpus pelatihan? Pertanyaan epistemologis inilah yang menjadi batas antara implementasi rekayasa dan kontribusi ilmiah tingkat doktoral dalam pengembangan vision-language models.

---

## Slide 016 - Dari Sinyal Bahasa ke Pemahaman Semantik

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana ruang embedding bersama berfungsi sebagai jembatan antara modalitas citra dan teks. Proyeksi vektor yang berdekatan memungkinkan perbandingan langsung antar modalitas. Namun, pertanyaan mendasar yang muncul adalah apakah kedekatan spasial dalam ruang vektor tersebut benar-benar mencerminkan pemahaman semantik yang mendalam, atau sekadar menangkap korelasi permukaan dan bias statistik dari data pelatihan.

Untuk membedakan antara korelasi dangkal dan pemahaman semantik yang sesungguhnya, peneliti perlu memperhatikan indikator-indikator berikut:
- **Konsistensi Parafrestasi**: Model harus memberikan respons yang stabil ketika menghadapi berbagai kalimat dengan makna identik namun struktur linguistik berbeda.
- **Disentanglement Atribut**: Kemampuan model untuk mengidentifikasi konsep seperti warna, bentuk, atau material secara terpisah, tanpa ketergantungan berlebihan pada konteks global.
- **Penolakan Pasangan Ambigu**: Model seharusnya mampu menolak pasangan citra-teks yang secara visual serupa tetapi memiliki perbedaan makna substantif.

Sebaliknya, terdapat beberapa indikator kinerja yang sering disalahartikan sebagai bukti pemahaman mendalam:
- Pemilihan label yang benar akibat **korelasi kontekstual** atau bias dataset, bukan karena inferensi semantik.
- Ketidakmampuan dalam menelusuri hubungan kausalitas atau melakukan penalaran kuantitatif sederhana.
- Skor benchmark yang tinggi, yang seringkali menunjukkan overfitting terhadap pola permukaan daripada generalisasi konseptual.

Evaluasi kritis semacam ini menjadi prasyarat metodologis yang wajib dilakukan sebelum mengevaluasi stabilitas model terhadap variasi input tekstual. Pembahasan selanjutnya akan mengarah pada bagaimana desain prompt secara langsung memengaruhi output dan representasi model, yang akan kita bedah lebih lanjut pada materi tentang sensitivitas template prompt.

---

## Slide 017 - Prompt Engineering: Memahami Sensitivitas Template

### Narasi

Pada slide sebelumnya, kita telah mengkritisi apakah kedekatan representasi antara citra dan teks pada model vision-language benar-benar mencerminkan pemahaman semantik, atau sekadar menangkap korelasi permukaan dan bias statistik dalam data pelatihan. Diskusi kritis ini membawa kita secara alami ke aspek metodologis yang sangat menentukan kinerja model di lapangan, yaitu bagaimana kita merancang input teks tersebut. Di sinilah konsep *prompt engineering* menjadi variabel kontrol yang tidak boleh diabaikan dalam eksperimen berbasis multimodal.

*Prompt engineering* dalam konteks model seperti CLIP atau arsitektur vision-language lainnya bukan sekadar penulisan kalimat bebas, melainkan proses sistematis untuk merancang teks input agar model menghasilkan prediksi atau representasi fitur yang paling optimal. Prompt berfungsi sebagai penyedia konteks yang mengarahkan cara model memetakan konten visual ke dalam ruang vektor bersama. Karena model-model ini dilatih pada korpus teks alami yang sangat besar dan heterogen, representasi yang mereka pelajari menjadi sangat sensitif terhadap bentuk permukaan (*surface form*) dari kata-kata yang digunakan.

Mengapa sensitivitas ini terjadi? Ada dua alasan fundamental yang perlu dipahami oleh peneliti tingkat doktoral. Pertama, distribusi linguistik dalam data pelatihan tidak seragam. Frasa pengantar atau modifier tertentu muncul dengan frekuensi jauh lebih tinggi daripada yang lain. Jika kita menggunakan format yang menyimpang dari distribusi tersebut, performa bisa menurun signifikan meskipun makna semantiknya sama. Kedua, kelas yang kurang umum atau memiliki variasi deskripsi tinggi sering kali membutuhkan kontekstualisasi tambahan agar embedding teksnya tetap sejalan dengan manifold representasi citra yang relevan.

Berikut adalah beberapa temuan empiris yang konsisten dilaporkan dalam literatur terkait pengaruh variasi template prompt:
- Menggunakan `"a photo of a {label}"` umumnya memberikan hasil yang stabil dan baik secara umum karena frekuensi kemunculan tinggi dalam data pelatihan.
- Menambahkan kata sifat negatif seperti `"a bad photo of a {label}"` cenderung menurunkan akurasi, karena model belum tentu mengenali nuansa evaluatif tersebut dalam ruang vektor bersama.
- Hanya menggunakan `"{label}"` tanpa frasa pengantar sering kali kurang efektif untuk kelas tertentu, terutama yang memiliki makna ganda atau konteks visual yang spesifik.
- Mengubah kata benda menjadi media lain seperti `"a drawing of a {label}"` akan mengarahkan model mencari representasi gaya gambar atau ilustrasi, yang mungkin tidak sesuai jika targetnya adalah foto realistis.

Sensitivitas terhadap pilihan prompt ini mengungkap bahwa kinerja zero-shot pada benchmark tidak selalu mencerminkan generalisasi model yang kuat, melainkan sering kali bergantung pada kecocokan linguistik antara query dan data pelatihan. Ketergantungan pada satu template tertentu justru dapat menjadi celah validitas dalam evaluasi model. Untuk mengatasi masalah ini, pendekatan selanjutnya yang banyak diadopsi dalam penelitian state-of-the-art adalah teknik ensembel prompt, yang akan kita bahas pada slide berikutnya. Dengan menggabungkan beberapa variasi template dan merata-ratakan embedding teksnya, kita dapat mengurangi bias linguistik dan meningkatkan robustness representasi multimodal secara sistematis.

---

## Slide 018 - Teknik Ensembel Prompt

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi bahwa representasi multimodal sangat sensitif terhadap bentuk permukaan teks atau *surface form* yang diberikan sebagai prompt. Perbedaan kecil seperti menambahkan artikel `"a"` atau mengubah kata benda `"photo"` menjadi `"picture"` dapat menggeser posisi embedding dalam ruang vektor bersama. Untuk mitigasi bias ini, pendekatan standar yang efektif adalah menerapkan teknik ensembel prompt.

Konsep dasarnya adalah mengganti ketergantungan pada satu template tunggal dengan kumpulan beberapa template yang memetakan makna kelas yang sama melalui struktur linguistik berbeda. Setiap template di-*tokenize* dan di-*encode* secara independen menggunakan komponen teks dari model vision-language. Hasilnya berupa sekumpulan embedding vektor yang kemudian di-*average* atau di-*pool* untuk menghasilkan satu vektor representasi kelas yang lebih stabil dan umum.

Berikut adalah contoh definisi kumpulan template dalam Python:
```python
templates = [
    "a photo of a {}.",
    "a photo of the {}.",
    "a photo of one {}.",
    "a picture of a {}.",
    "a blurry photo of a {}.",
]
```
Placeholder `{}` akan diisi dinamis oleh nama kelas target saat proses *batching*. Saat dieksekusi, setiap string dalam daftar ini melewati tokenizer dan encoder teks CLIP, menghasilkan tensor embedding berdimensi tinggi. Operasi *mean* dilakukan sepanjang sumbu template untuk menggabungkan informasi semantik dari seluruh variasi sintaks menjadi satu vektor kelas.

Strategi ensembel memberikan tiga keuntungan metodologis yang signifikan. Pertama, sensitivitas terhadap pilihan template tunggal berkurang drastis karena model tidak lagi bergantung pada satu jalur linguistik spesifik. Kedua, representasi yang dihasilkan mampu menangkap keragaman distribusi bahasa yang ada dalam data pra-pelatihan model foundation. Ketiga, peningkatan konsistensi embedding secara empiris menaikkan akurasi *zero-shot classification* dan memperkuat generalisasi lintas domain, yang menjadi indikator penting dalam evaluasi state-of-the-art.

Dari sisi infrastruktur komputasi, penambahan jumlah template memang meningkatkan biaya encoding awal. Namun, karena embedding teks bersifat statis untuk setiap kelas, proses ini hanya perlu dijalankan sekali. Hasil averaging dapat di-*cache* ke memori atau disimpan sebagai file `.pt`, sehingga overhead tambahan saat inference praktis mendekati nol. Pendekatan ini sangat direkomendasikan dalam desain eksperimen tingkat doktor yang menuntut reproduktibilitas dan stabilitas metrik.

Langkah teoritis ini akan segera diterjemahkan ke dalam implementasi kode yang lengkap. Pada slide berikutnya, kita akan membahas alur praktis klasifikasi *zero-shot* menggunakan Python, mencakup instalasi paket `open_clip_torch`, pipeline preprocessing citra, tokenisasi batch, serta perhitungan kesamaan kosinus antara embedding citra dan embedding teks terensembel untuk pengambilan keputusan akhir.

---

## Slide 019 - Zero-Shot Classification: Alur Praktis dengan Python

### Narasi

Setelah membahas teknik ensembel prompt pada slide sebelumnya, di mana kita menggabungkan beberapa variasi kalimat untuk menstabilkan representasi ruang teks, kini kita beralih ke implementasi praktisnya dalam klasifikasi zero-shot. Alur ini menjadi jembatan metodologis yang menghubungkan teori multimodal representation dengan eksekusi eksperimen yang dapat direproduksi.

Implementasi klasifikasi zero-shot mengikuti pipeline komputasi yang sistematis dan dapat diukur:
1. Muat model CLIP (`open_clip` atau modul dari Hugging Face).
2. Siapkan daftar label kelas target yang tidak pernah dilihat selama pelatihan model.
3. Bangun prompt standar untuk setiap label menggunakan template yang konsisten.
4. Tokenize dan encode seluruh prompt teks label menjadi embedding vektor berdimensi tinggi.
5. Lakukan preprocessing dan encode citra input yang akan diprediksi.
6. Hitung kesamaan kosinus antara embedding citra dan embedding teks.
7. Ambil kelas dengan skor kesamaan tertinggi sebagai prediksi akhir.

Sebelum memasuki tahap coding, pastikan lingkungan kerja Anda telah terinstalasi dependensi yang diperlukan. Untuk mengakses arsitektur resmi OpenAI, perintah instalasi dasar dapat dijalankan melalui terminal atau cell notebook:
```text
pip install open_clip_torch
```
Paket ini menyediakan antarmuka ringan untuk memuat model ViT-B/32 beserta transformasi gambar dan tokenizer bawaannya, sehingga fokus eksperimen dapat dialihkan ke validasi hipotesis daripada konfigurasi infrastruktur.

Pada tingkat abstraksi algoritma, langkah-langkah tersebut dapat direpresentasikan dalam bentuk pseudo-code yang menekankan operasi tensor inti:
```python

### prediksi = argmax

```
Kode pseudo ini menyoroti tiga operasi kritis: konversi modalitas berbeda ke ruang embedding yang sejajar, perkalian matriks dot product untuk menghitung relevansi silang, serta pengambilan indeks maksimum melalui argmax. Normalisasi vektor sebelum perkalian dot product merupakan prasyarat matematis agar nilai hasil setara dengan kesamaan kosinus, menghindari bias akibat magnitudo vektor yang berbeda.

Pseudo-code ini akan dijabarkan secara eksplisit pada slide berikutnya melalui implementasi lengkap menggunakan PyTorch dan `open_clip`. Dalam konteks penelitian doktoral, penguasaan pipeline ini memungkinkan Anda melakukan modifikasi pada tahap encoding, mengganti backbone vision-language, atau mengintegrasikan mekanisme cross-modal attention sesuai kebutuhan desain eksperimen. Pemahaman mendalam terhadap alur praktis ini juga menjadi fondasi untuk mengidentifikasi bottleneck komputasi dan merancang optimasi inference yang relevan dengan kontribusi ilmiah baru.

---

## Slide 020 - Contoh Kode Python: Zero-Shot dengan CLIP

### Narasi

Kode pada slide ini merupakan realisasi eksplisit dari alur zero-shot classification yang telah dirangkum secara konseptual pada slide sebelumnya. Menggunakan pustaka `open_clip`, skrip ini menunjukkan bagaimana ruang vektor citra dan teks diselaraskan dalam satu embedding space bersama, memungkinkan prediksi kelas tanpa fine-tuning tambahan.

Inisialisasi dimulai dengan `open_clip.create_model_and_transforms("ViT-B-32", pretrained="openai")`. Pemilihan arsitektur Vision Transformer berukuran menengah dengan bobot pra-latih OpenAI menjamin representasi semantik yang kuat. Fungsi `get_tokenizer` kemudian menyiapkan pipeline tokenisasi yang konsisten dengan arsitektur model, memastikan mapping karakter ke indeks vocabulary berjalan tepat sebelum masuk ke lapisan embedding.

Daftar label dikemas ulang menjadi prompt natural language dengan pola `"a photo of a {label}"`. Konvensi prompting ini bukan sekadar rutinitas string, melainkan strategi empiris yang mengurangi ambiguity representasi teks dan meningkatkan robustness model terhadap variasi deskripsi objek. `tokenizer(prompts)` mengeksekusi batching tokenisasi sekaligus, menghasilkan tensor integer yang siap diumpankan ke encoder teks.

Ekstraksi fitur dibungkus dalam `torch.no_grad()` untuk menonaktifkan autograd selama inferensi, sehingga mengoptimalkan penggunaan memori GPU dan mempercepat komputasi. Gambar input dilewatkan melalui `preprocess` yang menangani resizing, padding, normalisasi channel RGB, dan konversi ke tensor float. `.unsqueeze(0)` menambahkan dimensi batch agar kompatibel dengan expectasi input model neural network.

Normalisasi L2 diterapkan pada kedua vektor fitur melalui operasi pembagian dengan normanya masing-masing. Langkah ini memproyeksikan semua vektor ke permukaan hypersphere, sehingga perkalian dot product secara matematis ekuivalen dengan kesamaan kosinus. Normalisasi menghilangkan distorsi akibat perbedaan magnitudo vektor dan memastikan bahwa skor kesamaan murni mencerminkan kedekatan arah semantik.

Operasi matriks `(image_features @ text_features.T)` menghitung similarity score antara satu gambar query dengan seluruh kandidat teks. `.softmax(dim=-1)` menormalkan skor mentah menjadi distribusi probabilitas, memudahkan interpretasi statistik hasil prediksi. `.argmax().item()` mengambil indeks maksimum dan memetakannya kembali ke string label asli, menyelesaikan pipeline zero-shot secara deterministik.

Implementasi ini menegaskan prinsip dasar alignment multimodal yang menjadi pondasi bagi aplikasi vision-language lebih lanjut. Pada slide berikutnya, konsep kesamaan kosinus yang sama akan diperluas dari perbandingan dengan label diskrit menuju pencarian berbasis ranking pada kumpulan caption panjang, membuka diskusi mengenai image-text retrieval dan evaluasi top-k accuracy.

---

## Slide 021 - Image-Text Retrieval: Contoh Implementasi Python

### Narasi

```python
import torch
import open_clip

model, _, preprocess = open_clip.create_model_and_transforms(
    "ViT-B-32", pretrained="openai"
)
tokenizer = open_clip.get_tokenizer("ViT-B-32")

### database teks (captions) dan citra query

captions = [
    "a man walking a dog on the street",
    "two people sitting in a cafe",
    "a red car parked beside a building",
]

text_tokens = tokenizer(captions)
with torch.no_grad():
    text_features = model.encode_text(text_tokens)
    image_features = model.encode_image(preprocess(image).unsqueeze(0))

text_features /= text_features.norm(dim=-1, keepdim=True)
image_features /= image_features.norm(dim=-1, keepdim=True)

similarity = (image_features @ text_features.T).squeeze()
ranking = similarity.argsort(descending=True)
```

Slide ini melanjutkan pembahasan dari contoh klasifikasi zero-shot sebelumnya, namun menggeser fokus ke tugas *image-text retrieval*. Alih-alih memilih satu label dari kategori tertutup, model sekarang diminta mencocokkan sebuah citra query terhadap kumpulan deskripsi teks yang lebih panjang dan kontekstual. Pendekatan ini merefleksikan skenario nyata dalam sistem pencarian visual, anotasi gambar otomatis, atau pemetaan dataset berlabel lemah.

Inisialisasi model tetap menggunakan arsitektur ViT-B-32 dari OpenCLIP dengan bobot pretrained OpenAI. Basis data teks didefinisikan sebagai list string yang mewakili caption potensial. Proses tokenisasi dilakukan secara batch agar seluruh caption dapat dipadatkan menjadi tensor integer yang siap dilewatkan ke encoder teks.

Ekstraksi fitur dilakukan di dalam blok `torch.no_grad()` untuk menonaktifkan autograd. Hal ini tidak hanya mengurangi konsumsi memori GPU secara signifikan, tetapi juga mempercepat inferensi karena tidak ada operasi backpropagation yang perlu disimpan. Fitur citra terlebih dahulu melalui `preprocess()` yang menerapkan augmentasi standar (resize, normalize, channel permutation) sebelum ditambahkan dimensi batch dengan `.unsqueeze(0)`.

Normalisasi vektor fitur menjadi langkah metodologis yang krusial. Pembagian vektor dengan norma L2-nya memastikan bahwa semua embedding berada di permukaan hipersfer satuan. Akibatnya, operasi perkalian dot product `(image_features @ text_features.T)` secara matematis ekuivalen dengan kosinus kemiripan (*cosine similarity*). Tanpa normalisasi, magnitudo vektor dapat mendominasi skor dan mengaburkan informasi arah semantik.

Hasil perkalian matriks menghasilkan vektor skor kesamaan sepanjang dimensi teks. `.squeeze()` menghilangkan dimensi batch yang redundan, sedangkan `.argsort(descending=True)` mengembalikan indeks caption yang diurutkan berdasarkan relevansi tertinggi. Indeks inilah yang menjadi dasar penyusunan peringkat retrieval, siap digunakan untuk pengambilan keputusan atau evaluasi lebih lanjut.

Dalam konteks penelitian tingkat doktoral, implementasi retrieval semacam ini harus diposisikan sebagai komponen sistemik yang memerlukan validasi rigor. Slide berikutnya akan membahas metrik evaluasi standar untuk tugas retrieval, termasuk Recall@K, Median Rank, dan Mean Reciprocal Rank, serta praktik pelaporan interval kepercayaan yang diperlukan untuk memastikan reproducible research dan positioning yang kuat terhadap state-of-the-art.

---

## Slide 022 - Evaluasi Zero-Shot: Metrik yang Dipakai

### Narasi

Setelah pada slide sebelumnya kita membahas implementasi kode untuk menghitung similarity antara embedding citra dan teks serta melakukan ranking hasil retrieval, langkah selanjutnya yang tak kalah kritis adalah mengevaluasi performa tersebut secara kuantitatif. Pada konteks zero-shot, skor similarity mentah saja tidak cukup untuk menarik kesimpulan ilmiah. Kita memerlukan metrik evaluasi yang standar, transparan, dan dapat dibandingkan lintas studi, terutama mengingat level penelitian doktor menuntut rigor metodologis yang ketat.

Untuk tugas klasifikasi zero-shot, tiga metrik utama yang menjadi acuan umum adalah:
- **Accuracy**: Proporsi prediksi benar terhadap total sampel. Cocok untuk dataset seimbang, namun rentan terhadap class imbalance.
- **Top-1 / Top-5**: Mengukur apakah label ground truth muncul di posisi pertama atau lima teratas dari daftar prediksi. Memberi toleransi terhadap ketidakpastian model saat menghadapi kategori yang mirip secara semantik.
- **Mean per-class accuracy**: Rata-rata akurasi dihitung per kelas lalu dirata-ratakan kembali. Metrik ini menghilangkan bias dominasi kelas mayoritas dan lebih merepresentasikan kemampuan generalisasi model pada kategori langka.

Sementara itu, untuk tugas retrieval berbasis gambar-teks, fokus evaluasi bergeser ke kemampuan menemukan pasangan yang relevan dari basis data besar:
- **Recall@K**: Proporsi query yang berhasil menemukan pasangan benar di antara K hasil teratas. Indikator langsung untuk presisi sistem pencarian.
- **Median Rank**: Nilai peringkat tengah dari pasangan benar. Lebih robust terhadap outlier dibanding mean rank.
- **Mean Reciprocal Rank (MRR)**: Rata-rata nilai 1/peringkat pasangan benar. Memberi bobot signifikan pada hasil yang muncul di urutan paling atas, sehingga sensitif terhadap kesalahan fatal model.

Secara praktik penelitian, validitas perbandingan metrik ini sangat bergantung pada konsistensi protokol. Gunakan acuan dataset, split data, dan pipeline preprocessing yang identik antara model baseline dan arsitektur yang Anda usulkan. Selain itu, selalu laporkan interval kepercayaan atau variasi skor antar random seed. Praktik ini bukan sekadar kelengkapan laporan, melainkan standar reproduktibilitas yang diwajibkan oleh reviewer jurnal dan konferensi top-tier di bidang computer vision.

Perlu ditekankan bahwa perhitungan metrik di atas mengasumsikan representasi label sudah dioptimalkan. Jika penamaan kelas terlalu teknis, mengandung sinonim, atau tidak natural bagi model bahasa, skor evaluasi bisa menurun drastis terlepas dari kekuatan arsitektur. Hal ini secara langsung mengarah pada pembahasan slide berikutnya, yaitu kanalisasi dan normalisasi label, di mana strategi prompting, penggunaan template, dan validasi nama kelas akan dibahas sebagai fondasi sebelum eksekusi evaluasi metrik.

---

## Slide 023 - Kanalisasi dan Normalisasi Label untuk Zero-Shot

### Narasi

Setelah membahas metrik evaluasi zero-shot pada slide sebelumnya seperti Accuracy, Top-1/Top-5, Recall@K, dan Mean Reciprocal Rank, langkah kritis berikutnya adalah memastikan bahwa representasi label kelas telah disiapkan dengan tepat. Tanpa kanalisasi dan normalisasi label yang konsisten, metrik-metrik tersebut dapat menghasilkan skor yang menyesatkan dan mengaburkan kemampuan sebenarnya dari model vision-language.

Tantangan utama dalam zero-shot classification sering kali terletak pada kesenjangan antara penamaan kelas di dataset dan cara model memahaminya secara semantik. Nama kelas tidak selalu berupa kata tunggal yang natural, sering kali mengandung sinonim, varian bahasa, atau bersifat hierarkis seperti perbedaan halus antara `"Persian cat"` dan `"cat"`. Jika label tidak dikanalisasi ke bentuk yang paling umum dipahami oleh model, embedding multimodal yang dihasilkan akan terdistorsi.

Untuk mengatasi tantangan ini, terapkan praktik baik berikut dalam pipeline preprocessing Anda:
- Gunakan nama kelas dalam bentuk natural language yang paling sering muncul dan paling umum dipahami oleh model.
- Sertakan konteks kelas atas jika diperlukan, misalnya dengan format `"a photo of a {label}, a type of {superclass}"`.
- Manfaatkan banyak template prompt dan rata-ratakan embedding-nya untuk mengurangi varians akibat pemilihan kata spesifik.
- Validasi ketat pilihan nama label pada subset development sebelum menjalankan evaluasi final.

Proses ini juga membawa risiko yang perlu diantisipasi. Penggunaan nama label yang terlalu teknis atau jarang digunakan dapat menyebabkan prediksi model secara sistematis bias ke kelas lain yang lebih familiar. Selain itu, perubahan kecil pada wording label pun berpotensi mengubah hasil evaluasi secara signifikan, yang mengindikasikan kerentanan pipeline zero-shot terhadap variasi linguistik.

Isu sensitivitas terhadap wording ini akan menjadi fokus eksperimental pada slide berikutnya. Kita akan merancang pengujian untuk mengukur seberapa besar perubahan prompt mempengaruhi akurasi, mulai dari prompt sederhana hingga yang mengandung konteks kompleks. Analisis deviasi akurasi antar set prompt ini penting untuk menilai apakah model benar-benar menangkap semantik visual, atau hanya bergantung pada pola permukaan bahasa. Dengan demikian, kanalisasi label bukan sekadar tahap teknis, melainkan fondasi metodologis yang menentukan validitas dan reproducibility evaluasi zero-shot.

---

## Slide 024 - Sensitivitas Prompt: Analisis dan Evaluasi

### Narasi

Pada slide ini, kita melanjutkan diskusi dari kanalisasi dan normalisasi label yang telah dibahas sebelumnya, dengan beralih ke tahap evaluasi empiris terhadap stabilitas model multimodal. Fokus utama adalah mengukur sensitivitas prompt dalam skenario zero-shot classification. Eksperimen ini dirancang untuk mengkuantifikasi seberapa besar variasi formulasi teks dapat menggeser performa akurasi, sekaligus memetakan prompt mana yang bersifat robust versus yang rentan menyebabkan degradasi kinerja secara signifikan.

Desain eksperimen yang direkomendasikan memanfaatkan dataset berlabel terstruktur seperti CIFAR-100 atau domain spesifik yang relevan dengan fokus penelitian Anda. Langkah implementasinya meliputi penyiapan tiga varian prompt yang sistematis: pertama, prompt sederhana menggunakan templat dasar `"a photo of a {label}"`; kedua, prompt beragam yang memperkaya konteks melalui penambahan deskripsi lokasi, gaya visual, atau kualitas render; ketiga, prompt divergen yang menguji ketahanan model tanpa templat standar maupun dengan frasa negatif. Untuk setiap set prompt, hitung akurasi klasifikasi secara terpisah, lalu bandingkan nilai rata-rata dan deviasi standarnya.

Interpretasi hasil deviasi menjadi inti analisis tingkat lanjut. Deviasi akurasi yang kecil mengindikasikan bahwa model telah mempelajari representasi semantik yang stabil dan robust terhadap variasi permukaan bahasa. Sebaliknya, deviasi yang besar menandakan ketergantungan model pada kata kunci permukaan rather than makna konseptual, yang sering kali mengungkap kelemahan dalam alignment antara ruang visual dan linguistik. Temuan ini sangat krusial untuk merancang protokol evaluasi yang ketat dalam penelitian doktor, karena sensitivitas prompt berfungsi sebagai proxy penting untuk menilai generalisasi model di luar distribusi pelatihan.

Hasil analisis sensitivitas ini akan menjadi fondasi langsung untuk langkah dekonstruksi kesalahan prediksi pada slide berikutnya. Ketika kita telah mengidentifikasi prompt mana yang paling stabil atau paling rapuh, kita dapat mengarahkan analisis kesalahan ke kategori yang lebih spesifik, apakah bias muncul dari ambiguitas visual, kesamaan semantik antar kelas, gap linguistik, hingga efek prompt itu sendiri. Rangkaian evaluasi ini membentuk siklus iteratif yang ketat, memastikan bahwa setiap klaim performa model didukung oleh bukti eksperimental yang transparan dan reproducible.

---

## Slide 025 - Analisis Kesalahan pada Zero-Shot

### Narasi

Setelah kita mengevaluasi sensitivitas prompt terhadap perubahan akurasi pada slide sebelumnya, langkah analitis berikutnya adalah melakukan bedah kesalahan secara sistematis pada skenario zero-shot. Fokusnya bergeser dari pengukuran metrik agregat ke identifikasi pola kegagalan prediktif ketika model multimodal menghadapi data未见 tanpa proses fine-tuning.

Dalam konteks penelitian tingkat doktoral, pengklasifikasian kesalahan menjadi lima kategori utama diperlukan untuk mengisolasi variabel yang mendominasi noise prediksi:
- *Visual ambiguity*: kesalahan muncul karena representasi citra memiliki ambiguitas tinggi akibat oklusi, resolusi rendah, atau skala objek yang terlalu kecil.
- *Label semantically similar*: model kesulitan memisahkan kelas yang memiliki overlap deskriptif atau fitur hierarkis yang tumpang tindih.
- *Linguistic gap*: terjadi ketidakselarasan antara label tugas dengan distribusi bahasa yang pernah dipelajari selama pretraining, sehingga embedding teks tidak sejalan dengan ruang fitur visual.
- *Bias dataset*: model memanfaatkan korelasi kontekstual atau spasial sebagai shortcut, misalnya mengasosiasikan objek tertentu hanya dengan latar belakang spesifik.
- *Prompt effect*: formulasi prompt yang kurang tepat justru mengalihkan fokus attention mechanism ke fitur sekunder yang menyesatkan.

Output dari analisis ini harus dikonversi menjadi artefak penelitian yang terukur. Anda diminta menyusun tabel frekuensi kesalahan per kategori untuk memetakan distribusi error, kemudian melengkapinya dengan galeri visualisasi contoh prediksi salah yang representatif. Dari temuan tersebut, dapat dirumuskan rekomendasi perbaikan yang bersifat teknis, mulai dari penyempurnaan prompt engineering, penyesuaian prosedur augmentasi, hingga revisi protokol evaluasi agar lebih robust terhadap domain shift.

Hasil kategorisasi kesalahan ini akan menjadi fondasi empiris sebelum kita membahas sumber bias yang lebih mendasar pada slide berikutnya. Ketika titik kegagalan sudah terpetakan, pertanyaan kritis selanjutnya adalah bagaimana bias bahasa dan bias visual berinteraksi dalam arsitektur multimodal, serta mengapa evaluasi berbasis akurasi tunggal tidak lagi memadai untuk menjamin keadilan dan keandalan model di dunia nyata.

---

## Slide 026 - Bias Bahasa dan Bias Visual pada Model Multimodal

### Narasi

Pada slide ini, kita beralih dari identifikasi permukaan kesalahan prediksi menuju penggalian akar masalah sistemik, yaitu bias dalam representasi multimodal. Setelah sebelumnya kita mengklasifikasikan kesalahan zero-shot menjadi kategori visual, semantik, dan linguistik, langkah metodologis selanjutnya adalah memahami bagaimana bias bahasa dan bias visual terbentuk selama fase pra-pelatihan dan fine-tuning.

Sumber bias dalam model multimodal umumnya terbagi menjadi tiga dimensi kritis. Pertama, bias data, di mana kumpulan pasangan citra-teks yang diskrapping dari internet tidak mencerminkan distribusi populasi dunia nyata secara proporsional. Kedua, bias label, yang muncul akibat pemilihan nama kelas atau struktur hierarki superclass yang menanamkan prior semantik bias sejak awal. Ketiga, bias bahasa, di mana frekuensi kemunculan kata dalam korpus pelatihan menciptakan asosiasi statistik yang kuat, namun sering kali bersifat stereotip atau terbatas pada konteks linguistik tertentu.

Manifestasi bias ini dapat diamati melalui contoh korelasi artifisial dalam data. Jika korpus pelatihan didominasi gambar subjek pria pada label "dokter", model akan belajar mengaitkan konsep profesi tersebut dengan gender, bukan dengan atribut fungsional seperti alat medis atau lingkungan klinis. Demikian pula, objek seperti "gadget" yang sering muncul bersamaan dengan latar perkotaan akan mendorong model memanfaatkan konteks latar belakang sebagai shortcut prediksi, alih-alih mengekstrak fitur objek secara mandiri.

Implikasi penelitian pada jenjang doktoral menuntut pergeseran paradigma evaluasi. Metrik akurasi global sudah tidak memadai karena dapat menyembunyikan disparitas performa antar-subkelompok. Penelitian wajib mengintegrasikan pengujian fairness dan strategi debiasing. Protokol evaluasi harus dirancang untuk melakukan disaggregasi performa berdasarkan atribut demografis, variasi linguistik, atau kondisi konteks agar validitas model dapat diukur secara komprehensif dan reproduktif.

Pemahaman mengenai sumber dan manifestasi bias ini menjadi landasan metodologis sebelum kita menelusuri mekanisme internalnya. Pada slide berikutnya, kita akan membedah skema perpindahan bias mulai dari korpus mentah hingga ke dalam ruang embedding, serta merancang protokol kontrol atribut untuk mengisolasi pengaruh bias terhadap keputusan akhir model.

---

## Slide 027 - Studi Kasus Bias: Bagaimana Bias Masuk ke Model?

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi bahwa bias dalam model multimodal dapat bersumber dari distribusi data yang tidak representatif, pemilihan label yang memprioritaskan kategori tertentu, serta asosiasi linguistik yang terbentuk akibat frekuensi kemunculan kata dalam korpus pelatihan. Langkah selanjutnya adalah memahami mekanisme bagaimana bias-bias tersebut secara sistematis masuk dan terinternalisasi ke dalam arsitektur model. Proses ini bukan sekadar kesalahan acak, melainkan hasil langsung dari optimisasi fungsi loss yang mendorong model untuk menangkap korelasi statistik apa pun yang tersedia di dalam data, tanpa memandang apakah korelasi tersebut bersifat semantik atau artifaktual.

Skema perpindahan bias pada slide ini menggambarkan alur kausalitas yang perlu dipahami secara mendalam. Dimulai dari pasangan teks-citra yang dikumpulkan dari internet, data tersebut mengandung korelasi statistik yang sangat tidak seimbang karena didominasi oleh konten viral, stereotip budaya, atau pola komersial. Ketika model melakukan pembelajaran representasi, proses embedding akan menyerap korelasi-korelasi ini sebagai fitur utama karena memberikan penurunan error yang paling cepat selama training. Akibatnya, saat inference, prediksi model lebih banyak dipengaruhi oleh korelasi permukaan daripada pemahaman semantik yang mendalam. Fenomena ini menjelaskan mengapa model yang tampak cerdas pada benchmark umum sering kali gagal ketika dihadapkan pada konteks yang melanggar prior data.

Untuk mendeteksi dan menganalisis fenomena ini secara rigor, diperlukan protokol evaluasi yang terstruktur sesuai tabel analisis pada slide. Pertama, lakukan disagregasi berdasar atribut untuk memastikan model memilih suatu kelas berdasarkan karakteristik intrinsik objek, bukan karena faktor pendamping yang berkorelasi kuat di data latih. Kedua, uji model pada citra dengan latar belakang atau lokasi yang jarang muncul dalam dataset umum, guna mengukur ketahanan model terhadap distribusi out-of-distribution. Ketiga, terapkan uji kontrol atribut dengan mengubah properti subjek (seperti gender, usia, atau pakaian) dan amati dampaknya terhadap label keluaran. Jika label berubah hanya karena atribut sekunder, hal ini mengindikasikan ketergantungan model pada bias permukaan yang harus diintervensi.

Analisis kritis terhadap mekanisme bias ini menjadi fondasi penting sebelum melangkah ke evaluasi performa pada domain spesifik. Seperti yang akan dibahas pada slide berikutnya, meskipun model multimodal menunjukkan akurasi tinggi pada benchmark standar, kinerja tersebut sering kali tidak generalisasi ke data domain khusus seperti medis, satelit, atau industri. Ketidaksesuaian antara distribusi data internet yang digunakan untuk training dan kebutuhan nyata penelitian doctoral inilah yang menciptakan kesenjangan validasi. Oleh karena itu, desain eksperimen disertasi harus selalu menyertakan pengukuran ketat pada data target domain, serta strategi mitigasi bias yang transparan dan terukur.

---

## Slide 028 - Kesenjangan Kemampuan Benchmark dengan Kebutuhan Domain

### Narasi

Pada slide sebelumnya, kita telah menguraikan mekanisme bagaimana bias dapat terserap ke dalam representasi model melalui korelasi statistik yang tidak seimbang pada pasangan citra dan teks di internet. Diskusi mengenai bias ini secara alami mengarah pada pertanyaan kritis berikutnya: seberapa jauh kinerja model pada benchmark standar benar-benar mencerminkan kemampuannya dalam menyelesaikan masalah nyata di lapangan.

Berikut adalah realita yang perlu dipahami secara mendalam:
- Model vision-language seperti CLIP memang menunjukkan skor tinggi pada benchmark umum seperti ImageNet, CIFAR-100, atau dataset web berskala besar.
- Namun, performa tersebut sering kali mengalami penurunan drastis ketika diterapkan pada data domain spesifik, seperti citra medis, penginderaan jauh, inspeksi industri, atau dokumentasi artefak budaya.

Kesenjangan ini muncul karena beberapa faktor struktural yang perlu diidentifikasi:
- Konsep atau objek yang relevan di domain tertentu sangat jarang muncul dalam pasangan citra-teks yang digunakan untuk pelatihan awal.
- Gaya visual pada data domain berbeda jauh dari foto natural yang mendominasi corpus internet, misalnya berupa gambar mikroskopis, peta satelit, atau sketsa teknis.
- Terminologi dan struktur bahasa yang digunakan oleh pakar di domain tersebut cenderung lebih spesifik, formal, atau mengandung jargon yang tidak lazim dalam bahasa internet umum.

Implikasi langsung untuk penyusunan disertasi Anda adalah sebagai berikut:
- Validasi berbasis domain menjadi prasyarat mutlak sebelum mengklaim keberhasilan sebuah arsitektur atau metode.
- Model *general-purpose* tidak dapat serta-merta dianggap siap pakai (*plug-and-play*) untuk menjawab pertanyaan penelitian Anda.
- Laporan eksperimen wajib menyertakan pengukuran kinerja secara eksplisit pada data domain target, lengkap dengan analisis kegagalan (*failure cases*) dan batas kemampuan model.

Mengingat pentingnya validasi domain yang ketat dan eksperimen yang terukur, persiapan infrastruktur perangkat lunak menjadi langkah operasional berikutnya. Pada slide selanjutnya, kita akan membahas toolbox utama yang diperlukan untuk menjalankan eksperimen multimodal secara efisien dan reproduktif. Library seperti `open_clip_torch`, `transformers` dari Hugging Face, serta ekosistem PyTorch dan scikit-learn akan menjadi fondasi teknis untuk memuat model, memanipulasi tensor, mengevaluasi metrik, hingga memvisualisasikan hasil embedding. Konsistensi versi library, dokumentasi lingkungan komputasi, dan pengaturan *random seed* harus menjadi standar baku agar setiap temuan empiris dapat diverifikasi dan dikembangkan lebih lanjut.

---

## Slide 029 - Toolbox Utama untuk Eksperimen Multimodal

### Narasi

Setelah memahami bahwa kinerja model multimodal sangat rentan terhadap pergeseran domain, validasi empiris harus didukung oleh lingkungan komputasi yang terkontrol dan terdokumentasi dengan baik. Slide ini merinci toolbox utama yang menjadi fondasi eksperimen praktikum pertemuan ini.

Implementasi eksperimen vision-language akan mengandalkan ekosistem library berikut:
- `open_clip_torch`: Memuat model CLIP dengan berbagai arsitektur encoder-decoder dan pretrained weights yang telah distandarisasi untuk inference dan fine-tuning.
- `transformers` (Hugging Face): Menyediakan API seragam untuk mengakses tidak hanya CLIP, tetapi juga keluarga model vision-language lainnya seperti BLIP, Flamingo, atau LLaVA.
- `torch` dan `torchvision`: Menangani operasi tensor tingkat rendah, augmentasi citra, serta konstruksi arsitektur neural network secara native dan efisien.
- `numpy` dan `scikit-learn`: Digunakan untuk pra-pemrosesan data numerik, ekstraksi metrik evaluasi, reduksi dimensi embedding, serta teknik clustering untuk analisis ruang fitur multimodal.
- `matplotlib`: Berfungsi sebagai alat visualisasi utama untuk plotting distribusi embedding, kurva evaluasi, heatmap attention, dan contoh pasangan citra-teks.

Pada jenjang doktoral, konsistensi lingkungan komputasi sama pentingnya dengan desain metodologi penelitian. Pastikan semua eksekusi menggunakan versi library yang identik dan dokumentasikan konfigurasi environment secara lengkap. Setel `random seed` secara eksplisit di awal setiap script eksperimen untuk menjamin reproduktibilitas hasil, sehingga temuan Anda dapat diverifikasi oleh reviewer jurnal atau konferensi internasional tanpa ambiguity.

Dengan toolbox yang telah dikonfigurasi, langkah selanjutnya adalah menerapkan alur kerja sistematis. Slide berikutnya akan menguraikan workflow praktikum yang terstruktur, mulai dari setup dataset target, pemuatan model, uji klasifikasi zero-shot dengan variasi prompt, hingga pengujian retrieval dan analisis kegagalan model sesuai target luaran yang telah ditetapkan.

---

## Slide 030 - Workflow Praktikum Pertemuan 05

### Narasi

Setelah mengonfirmasi ketersediaan dan kompatibilitas library pendukung pada slide sebelumnya, kita kini beralih ke alur eksekusi praktis untuk pertemuan ini. Workflow yang disajikan merupakan peta jalan sistematis untuk menjalankan eksperimen vision-language model, dengan CLIP sebagai baseline representasi multimodal. Alur ini dirancang agar setiap fase eksperimen berjalan terstruktur, terukur, dan siap di-reproduce.

Tahap pertama berfokus pada persiapan dataset target. Mahasiswa diminta memilih kumpulan data yang relevan dengan domain penelitian, memastikan bahwa struktur label kategorikalnya dapat dipetakan secara semantik ke dalam deskripsi teks yang koheren. Tahap kedua adalah inisialisasi model, di mana pretrained weights dimuat melalui interface yang telah disiapkan. Penetapan `random seed` dan pencatatan versi library menjadi kewajiban mutlak pada fase ini untuk menjamin konsistensi numerik dan transparansi lingkungan komputasi.

Inti eksperimen berada pada tahap ketiga, yaitu klasifikasi zero-shot dengan variasi prompt. Alih-alih mengandalkan satu template statis, mahasiswa harus merancang sekumpulan prompt yang sistematis untuk menguji ketahanan model terhadap perubahan formulasi linguistik. Proses ini tidak hanya menghitung akurasi global, tetapi juga menuntut analisis kesalahan (*error analysis*), pemetaan distribusi embedding, serta perbandingan dampak semantik antar prompt terhadap performa inferensi.

Tahap keempat mengarah pada pengujian retrieval, di mana model dievaluasi kemampuannya menjembatani query teks dengan pasangan citra yang sesuai melalui metrik seperti `recall@K`. Seluruh rangkaian eksekusi ini harus dikompilasi ke dalam laporan eksperimen yang memenuhi standar riset tingkat doktor. Laporan wajib mencakup akurasi zero-shot, analisis kuantitatif terhadap variasi prompt, dokumentasi kasus keberhasilan dan kegagalan, serta refleksi kritis mengenai bias sistematis atau keterbatasan arsitektural model.

Agar seluruh tahapan ini menghasilkan temuan yang valid dan dapat dipertanggungjawabkan secara ilmiah, perencanaan metodologis harus diselesaikan sebelum kode dieksekusi. Pada slide berikutnya, kita akan mendalami perancangan protokol evaluasi zero-shot yang menekankan pada formulasi pertanyaan penelitian, seleksi dataset representatif, konstruksi set prompt yang terkontrol, serta penetapan aturan evaluasi yang ketat. Pendekatan metodologis inilah yang akan membedakan praktikum teknis dari sebuah kajian penelitian yang matang dan berkontribusi pada state-of-the-art.

---

## Slide 031 - Perancangan Protokol Evaluasi Zero-Shot

### Narasi

Pada slide ini, kita membahas perancangan protokol evaluasi zero-shot yang menjadi fondasi metodologis dalam eksperimen Anda. Setelah pada slide sebelumnya kita menyusun alur kerja praktis mulai dari persiapan dataset hingga pengujian klasifikasi zero-shot, langkah selanjutnya adalah merancang protokol yang ketat dan terukur agar hasil eksperimen dapat dipertanggungjawabkan secara ilmiah.

Perancangan protokol ini mengikuti enam langkah sistematis:
1. Tentukan pertanyaan penelitian yang spesifik, misalnya kemampuan model memahami konsep domain baru atau sensitivitas representasi terhadap variasi linguistik.
2. Pilih dataset yang representatif, dengan memastikan labelnya dapat dikonversi secara natural ke dalam format teks untuk sinkronisasi dengan text encoder.
3. Definisikan set prompt secara sistematis dengan berbagai variasi, menghindari ketergantungan pada satu template tunggal.
4. Tentukan metrik evaluasi yang komprehensif, mencakup akurasi global maupun per-class accuracy untuk deteksi bias kelas.
5. Tetapkan aturan evaluasi yang jelas, termasuk jumlah sampel, strategi pembagian data, dan penanganan warm-up prompt.
6. Dokumentasikan seluruh konfigurasi teknis, meliputi seed acak, versi model, daftar prompt, dan pipeline preprocessing.

Sebuah protokol evaluasi yang berkualitas harus memenuhi tiga karakteristik utama. Pertama, desainnya harus transparan dan sepenuhnya reproducible oleh peneliti lain. Kedua, hindari modifikasi prompt pasca-pengamatan pada dataset uji, karena praktik tersebut dapat memicu data leakage dan overfitting implisit. Ketiga, sertakan kerangka analisis kesalahan yang terstruktur, sehingga kegagalan prediksi dapat dikategorikan berdasarkan pola error daripada sekadar laporan numerik.

Protokol yang telah dirumuskan pada slide ini akan langsung diimplementasikan pada tahap benchmark empiris di slide berikutnya. Di sana, kita akan mengukur dampak langsung dari variasi prompt terhadap akurasi zero-shot, sekaligus memverifikasi apakah aturan evaluasi yang kita buat mampu menangkap dinamika kinerja model foundation vision-language secara akurat.

---

## Slide 032 - Analisis Pengaruh Prompts: Benchmark Sederhana

### Narasi

Pada slide ini, kita akan membahas bagaimana variasi prompt secara langsung memengaruhi kinerja model dalam skenario zero-shot classification. Tabel yang ditampilkan menyajikan contoh benchmark sederhana untuk mengkuantifikasi dampak pemilihan template teks terhadap akurasi akhir, sekaligus mengilustrasikan prinsip alignment antara ruang teks dan visual.

Ketika kita menggunakan label mentah tanpa template, misalnya hanya kata `"cat"`, akurasinya tercatat sebesar 57,2%. Model kesulitan memetakan token tunggal ke ruang representasi yang kompleks, sehingga banyak kelas tidak terbaca dengan optimal. Penggunaan template dasar seperti `"a photo of a {label}"` meningkatkan akurasi secara signifikan menjadi 68,5%. Frasa ini memberikan konteks visual yang lebih selaras dengan distribusi data pelatihan model, sehingga embedding teks yang dihasilkan lebih relevan dengan fitur citra.

Penambahan konteks hierarkis melalui `"a photo of a {label}, a type of {superclass}"` mendorong peningkatan lebih lanjut menjadi 70,1%. Dalam tugas klasifikasi tingkat lanjut, konteks superclass sering kali membantu model mengatasi ambiguitas antar-kelas yang memiliki fitur visual tumpang tindih. Jika kita menerapkan teknik ensembel dengan menggabungkan lima template berbeda, akurasi mencapai 72,3% dengan deviasi yang lebih rendah, menunjukkan stabilitas prediksi yang lebih konsisten. Sebaliknya, prompt dengan gaya tidak sesuai, seperti `"a drawing of a {label}"` pada dataset foto, justru menurunkan kinerja hingga 51,8%, bahkan di bawah baseline tanpa template.

Dari hasil benchmark ini, terdapat tiga poin interpretasi kritis yang harus dipertimbangkan dalam perancangan eksperimen:
- Templat yang paling dekat dengan karakteristik dan domain data pelatihan umumnya menghasilkan representasi yang lebih optimal.
- Penyertaan konteks superclass dapat menjadi strategi efektif ketika label tunggal bersifat ambigu atau terlalu umum.
- Pemilihan prompt yang salah gaya atau tidak selaras dengan modalitas input dapat merusak cross-modal alignment, sehingga kinerja model anjlok drastis.

Temuan ini secara langsung mengoperasionalkan protokol evaluasi yang telah kita diskusikan pada slide sebelumnya. Setelah merancang set prompt yang sistematis dan menetapkan metrik akurasi, langkah validasi empiris terhadap variasi template menjadi wajib sebelum mengadopsi satu prompt tetap untuk pengujian skala penuh. Pendekatan ini memastikan bahwa perbedaan kinerja yang diamati benar-benar berasal dari desain prompt, bukan noise acak atau bias preprocessing.

Pemahaman mendalam tentang pengaruh prompt ini akan menjadi fondasi analitis saat kita membandingkan arsitektur model vision-language pada slide berikutnya. Diskusi akan mengarah pada perbandingan langsung antara DINO/DINOv2 dan CLIP, menyoroti bagaimana sinyal supervisi berbasis bahasa versus sinyal visual murni membentuk representasi multimodal yang berbeda, serta kapan masing-masing pendekatan paling tepat diimplementasikan dalam riset computer vision mutakhir.

---

## Slide 033 - Menghubungkan dengan Pertemuan Sebelumnya: DINO vs CLIP

### Narasi

Sensitivitas terhadap prompt yang kita bahas pada slide sebelumnya mengungkap karakteristik fundamental dari model berbasis alignment. Ketika model bergantung pada pencocokan ruang fitur visual dan linguistik, variasi gaya penulisan prompt dapat menggeser distribusi embedding dan menurunkan kinerja zero-shot. Fenomena ini menjadi titik tolak penting untuk membedakan mengapa DINOv2 dan CLIP, meskipun sama-sama menghasilkan representasi berkualitas tinggi, memiliki filosofi pelatihan dan cakupan aplikasi yang berbeda.

Perbandingan pada tabel ini menyoroti dua paradigma representasi yang saling melengkapi. DINOv2 mengandalkan sinyal pengawas self-supervised tanpa label eksternal, sehingga fokusnya sepenuhnya pada struktur visual. Representasi yang dihasilkan bersifat visual-centric, sangat kuat secara spasial maupun semantik, dan memiliki transferability yang luar biasa untuk tugas computer vision murni. Sebaliknya, CLIP dilatih dengan supervisi bahasa alami melalui jutaan pasangan gambar-teks. Pendekatan ini menghasilkan representasi multimodal yang langsung kompatibel dengan teks, memungkinkan zero-shot classification berbasis prompt, serta sangat efektif untuk konsep-konsep umum yang dapat dideskripsikan secara linguistik.

Berdasarkan karakteristik tersebut, pemilihan model harus dipetakan sesuai kebutuhan riset:
- **DINOv2**: Ideal untuk tugas yang menuntut representasi spasial detail atau ekstraksi fitur semantik tanpa ketergantungan pada deskripsi tekstual. Sangat cocok untuk segmentasi, deteksi, atau clustering visual.
- **CLIP**: Lebih unggul ketika tugas melibatkan interaksi bahasa, retrieval berbasis teks, atau klasifikasi zero-shot dengan kategori yang fleksibel dan mudah diubah melalui prompt.

Penting untuk dipahami bahwa CLIP bukanlah endpoint dari perkembangan model multimodal. Ia berfungsi sebagai fondasi alignment yang relatif sederhana namun sangat modular. Pada slide berikutnya, kita akan melihat bagaimana ekosistem model seperti BLIP, ALIGN, Flamingo, hingga LLaVA membangun di atas prinsip dasar CLIP dengan menambahkan kapabilitas generasi, reasoning berurutan, dan instruction tuning. Pemahaman ini akan membantu Anda memposisikan metodologi yang diusulkan dalam proposal disertasi terhadap state-of-the-art terkini.

---

## Slide 034 - Perbandingan dengan Model Multimodal Lain

### Narasi

Setelah kita membedah perbedaan mendasar antara DINO yang bersifat self-supervised dan CLIP yang memanfaatkan supervisi bahasa alami, langkah selanjutnya adalah menempatkan CLIP dalam ekosistem model multimodal yang lebih luas. Pada slide ini, kita akan membandingkan arsitektur dan filosofi desain CLIP dengan beberapa pendekatan multimodal terkemuka lainnya untuk memahami posisi strategisnya dalam peta riset terkini.

Tabel pada slide ini merangkum lima model kunci beserta ide utama dan kekuatan masing-masing:
- **CLIP**: Mengandalkan contrastive learning antara gambar dan teks, sehingga unggul dalam klasifikasi zero-shot dan retrieval berbasis teks.
- **BLIP**: Memperkenalkan mekanisme bootstrapping untuk menghasilkan caption secara otomatis dan memfilter data berkualitas tinggi, menjadikannya kuat untuk tugas generasi dan pemahaman visual.
- **ALIGN**: Mengadopsi skala data yang sangat besar dengan toleransi terhadap noise, memberikan robustness yang luar biasa terhadap data yang tidak bersih.
- **Flamingo**: Menggabungkan interleaved image-text dengan large language model, memungkinkan reasoning multimodal few-shot yang kontekstual.
- **LLaVA**: Fokus pada integrasi langsung ke LLM untuk mengikuti instruksi visual secara natif.

Penting untuk dipahami bahwa CLIP bukan sekadar salah satu dari banyak model, melainkan fondasi alignment yang sederhana namun sangat efektif. Banyak model modern justru mengambil representasi CLIP sebagai titik awal, lalu memperluasnya dengan kemampuan generasi, reasoning tingkat lanjut, atau instruction tuning. Pendekatan modular ini menunjukkan pergeseran paradigma dari alignment statis menuju interaksi dinamis antara modalitas visual dan linguistik.

Dari perspektif penelitian doktoral, pemilihan model harus didasarkan pada jenis tugas spesifik dan seberapa kritis kebutuhan integrasi bahasa dalam aplikasi target. Namun, sebelum memutuskan arsitektur, peneliti perlu menyadari bahwa kekuatan multimodalitas memiliki batas-batas fundamental. Keterbatasan ini akan menjadi fokus diskusi pada slide berikutnya, di mana kita akan mengidentifikasi celah performa seperti kesalahan atribut, sensitivitas terhadap prompt, hingga risiko evaluasi yang menyesatkan akibat shortcut statistik.

---

## Slide 035 - Keterbatasan Multimodal Model yang Perlu Diketahui

### Narasi

Setelah menempatkan CLIP sebagai fondasi alignment sederhana dan melihat bagaimana model lain memperluas kemampuannya ke arah generation, reasoning, atau instruction tuning, kita perlu menggeser fokus dari arsitektur menuju batas fundamental yang masih menghambat keandalan sistem multimodal di tingkat penelitian doktoral. Performa impresif pada benchmark standar tidak serta merta mencerminkan pemahaman semantik, kausalitas, atau generalisasi yang robust.

Berikut adalah enam keterbatasan kritis yang wajib dipertimbangkan dalam perancangan eksperimen dan evaluasi model:

1. **Kesalahan atribut**: model sering kali kurang peka terhadap jumlah objek, posisi spasial, serta relasi struktural antar elemen dalam citra.
2. **Sensitivitas prompt**: keluaran model dapat berubah drastis hanya karena variasi kecil pada permukaan teks, mengindikasikan ketidakstabilan representasi.
3. **Bias distribusi**: model cenderung mereplikasi stereotip sosial, gender, dan budaya yang melekat pada data latih berskala besar.
4. **Reasoning terbatas**: kemampuan penalaran masih bersifat korelasional; model belum sepenuhnya menguasai logika atau proses sebab-akibat yang mendasari fenomena visual.
5. **Keterbatasan domain**: kinerja turun signifikan saat menghadapi citra atau konstruksi linguistik yang berada di luar distribusi training (out-of-distribution).
6. **Evaluasi yang menyesatkan**: akurasi tinggi sering kali dicapai melalui shortcut statistik, bukan melalui alignment semantik yang genuine.

Implikasi langsung untuk riset tingkat doktor adalah menolak kesederhanaan interpretasi bahwa skor tinggi ekuivalen dengan pemahaman. Evaluasi harus dirancang secara eksplisit untuk membongkar mekanisme internal model. Pendekatan yang disarankan meliputi penggunaan pertanyaan kontrol yang mengisolasi variabel tertentu, pengujian dengan prompt adversarial yang mengeksploitasi bias permukaan, serta penyusunan dataset khusus yang memaksa model bergantung pada representasi konseptual, bukan heuristik dangkal.

Pembahasan mengenai batasan ini menjadi jembatan metodologis yang krusial sebelum kita memasuki protokol validasi empiris. Pada slide berikutnya, kita akan menguraikan cara menguji apakah model benar-benar memahami semantik, melalui manipulasi pasangan kalimat yang menjaga makna tetap stabil sementara permukaan linguistik bervariasi, atau sebaliknya. Desain pengujian semacam ini memungkinkan peneliti membedakan antara alignment superficial dan grounding konseptual yang sebenarnya, sekaligus membuka celah untuk kontribusi metodologis baru dalam literatur computer vision.

---

## Slide 036 - Cara Menguji Apakah Model Memahami Semantik

### Narasi

Merujuk pada pembahasan slide sebelumnya mengenai keterbatasan model multimodal, khususnya masalah evaluasi yang menyesatkan dan sensitivitas terhadap bentuk permukaan teks, kita perlu beralih ke metode verifikasi yang lebih rigor. Pengujian pemahaman semantik bukan sekadar melihat akurasi akhir, melainkan membedah bagaimana model merepresentasikan hubungan antara sinyal visual dan linguistik secara komposisional.

Strategi pengujianya dibangun melalui dua jenis pasangan kalimat yang dirancang khusus:
- Pasangan kalimat dengan makna sama tetapi berbeda permukaan (*paraphrase*).
- Pasangan kalimat dengan permukaan mirip tetapi makna berbeda (*minimal pair semantik*).
Skor kesamaan dari kedua pasangan ini kemudian dibandingkan secara paralel untuk mengungkap apakah model benar-benar menangkap makna esensial atau hanya mencocokkan token secara dangkal.

Perhatikan contoh kasus pada tabel slide ini. Untuk citra anjing yang sedang berlari, model ideal akan memberikan skor tinggi ketika membandingkannya dengan `"a dog running"` versus `"a dog is running on the grass"`, karena keduanya ekivalen secara semantik. Namun, skor harus menurun signifikan saat teks berubah menjadi `"a dog sitting"`. Pada kasus ketiga, perbandingan dengan `"an animal moving fast"` seharusnya menghasilkan skor menengah, mencerminkan pemahaman hierarkis konsep tanpa kehilangan konteks spesifik dari aksi utama.

Implikasi analisisnya cukup kritis. Jika model memberikan skor ekstrem tinggi pada teks yang secara logika atau visual tidak konsisten, maka model tersebut kemungkinan besar masih bergantung pada *surface form matching* atau korelasi statistik dalam data pelatihan. Kondisi ini menegaskan bahwa metrik evaluasi konvensional sering kali gagal mendeteksi kegagalan inferensial pada model multimodal, sehingga memerlukan protokol pengujian berbasis kontras semantik.

Hasil diagnostik semacam ini langsung mengarah pada langkah berikutnya, yaitu menghubungkan temuan evaluasi dengan identifikasi *research gap*. Dari celah pemahaman semantik yang terdeteksi, kita dapat merumuskan pertanyaan penelitian yang spesifik, merancang protokol eksperimen yang ketat, dan akhirnya menyusun proposal disertasi yang menawarkan *novelty* metodologis serta kontribusi ilmiah yang terukur.

---

## Slide 037 - Menghubungkan Konsep ke Research Gap

### Narasi

Pada slide ini, kita beralih dari mekanisme pengujian pemahaman semantik menuju identifikasi celah penelitian atau *research gap* yang konkret. Setelah memahami bagaimana menilai apakah model benar-benar menangkap makna di balik pasangan citra-teks, langkah selanjutnya adalah memetakan temuan tersebut ke dalam area-area yang masih memerlukan eksplorasi lebih mendalam dalam literatur terkini.

Berikut adalah beberapa contoh *research gap* yang sangat relevan dengan perkembangan *Vision-Language Models* pada jenjang doktoral:
- **Evaluasi domain**: Model seperti CLIP umumnya dievaluasi pada dataset umum, namun belum ada kajian sistematis mengenai kinerjanya pada domain spesifik yang dipilih peneliti.
- **Prompt engineering**: Metode adaptasi prompt secara otomatis untuk menyesuaikan data domain masih minim dikembangkan, padahal ketepatan prompt sangat menentukan performa *zero-shot*.
- **Bias bahasa-visual**: Pemetaan bias antara representasi teks dan citra pada dataset lokal atau domain tertentu belum banyak diteliti secara komprehensif.
- **Retrieval**: Mekanisme *image-text retrieval* untuk konten yang sangat spesifik sering kali belum diuji menggunakan metrik evaluasi yang tepat dan kontekstual.
- **Benchmark**: Belum terdapat *benchmark zero-shot* yang mampu merepresentasikan kebutuhan praktis industri atau aplikasi nyata secara holistik.

Untuk mengubah celah-celah tersebut menjadi proposal penelitian yang solid, diperlukan alur kerja yang terstruktur. Pertama, identifikasi pertanyaan evaluasi yang belum terjawab oleh studi sebelumnya. Kedua, rancang eksperimen yang secara langsung menguji hipotesis terkait celah tersebut. Ketiga, manfaatkan analisis kesalahan (*error analysis*) sebagai fondasi utama untuk merumuskan *novelty*, karena pola kegagalan model sering kali mengungkap kelemahan arsitektural atau kesenjangan data yang dapat dikontribusikan secara ilmiah.

Pendekatan ini akan membawa kita langsung ke implementasi praktis. Pada slide berikutnya, kita akan melihat contoh konkret bagaimana sebuah *research gap* diterjemahkan ke dalam konfigurasi eksperimen, khususnya dalam menguji sensitivitas prompt pada dataset domain tertentu menggunakan model CLIP.

---

## Slide 038 - Contoh Eksperimen: Menguji Sensitivitas Prompt pada Dataset Domain

### Narasi

Pada slide ini, kita menerjemahkan celah penelitian yang telah diidentifikasi sebelumnya menjadi desain eksperimen yang terukur. Fokusnya adalah menguji sensitivitas *prompt* terhadap performa model Vision-Language Model pada data domain spesifik, yang secara langsung menyentuh isu rekayasa *prompt* otomatis dan evaluasi domain yang belum banyak dieksplorasi secara sistematis.

Konfigurasi eksperimen dirancang untuk mensimulasikan kondisi nyata di mana data tidak mengikuti distribusi umum internet. Kita menggunakan dataset berisi sepuluh kelas dari domain target, misalnya objek industri atau flora lokal. Sebagai backbone, dipilih CLIP dengan arsitektur ViT-B/32 karena memberikan keseimbangan optimal antara kapasitas representasi multimodal dan overhead komputasi yang memungkinkan iterasi cepat selama tuning variabel.

Variabel utama dalam uji ini adalah empat set *prompt* yang disusun berdasarkan tingkat kedalaman semantik:
- P1 menggunakan template generik `"a photo of a {label}"` sebagai baseline zero-shot standar.
- P2 menyisipkan konteks lingkungan menjadi `"a {label} in the factory"` untuk menguji kemampuan adaptasi domain.
- P3 memanfaatkan hierarki kategorikal dengan format `"a {label}, a type of {superclass}"` guna mengevaluasi pengaruh pengetahuan superkelas.
- P4 menerapkan mekanisme ensembel dari P1 hingga P3 untuk mengukur potensi stabilisasi prediksi.

Prosedur evaluasi dilakukan secara granular dengan menghitung akurasi per kelas dan memetakan ruang fitur melalui visualisasi embedding. Pendekatan ini memungkinkan kita mengamati apakah batas keputusan antar kelas tetap tajam atau justru mengalami degenerasi akibat dominasi satu jenis prompt tertentu.

Dari perspektif analisis, tiga hipotesis kerja perlu diverifikasi secara empiris. Pertama, apakah penambahan konteks domain secara statistik signifikan meningkatkan akurasi pada kategori yang jarang muncul dalam data pra-latihan CLIP. Kedua, bagaimana distribusi kesalahan bergesir ketika prompt divariasikan; apakah kesalahan masih bersifat misalignment semantik atau berubah menjadi kerentanan terhadap noise visual. Ketiga, apakah strategi ensembel pada P4 berhasil mengurangi varians prediksi dan meningkatkan *robustness* dibandingkan penggunaan prompt tunggal.

Temuan dari pengujian sensitivitas ini akan menjadi bahan baku substantif untuk penulisan laporan eksperimen. Struktur pelaporan yang sistematis, sebagaimana akan diuraikan pada slide berikutnya, diperlukan agar temuan mengenai dinamika prompt dapat dikomunikasikan secara akademis, mudah direplikasi, dan siap diposisikan sebagai kontribusi metodologis tingkat doktor.

---

## Slide 039 - Laporan Eksperimen Multimodal: Format yang Diusulkan

### Narasi

Setelah kita menguji sensitivitas prompt pada berbagai skenario domain di slide sebelumnya, langkah selanjutnya adalah mendokumentasikan temuan tersebut secara terstruktur. Slide ini menyajikan format laporan eksperimen multimodal yang diusulkan sebagai standar penulisan akademik tingkat doktor. Format ini menjamin bahwa setiap aspek penelitian dapat ditelusuri, dievaluasi, dan dikembangkan lebih lanjut sesuai tuntutan publikasi internasional.

Struktur dimulai dari pendahuluan, yang harus memuat pertanyaan riset yang tajam dan motivasi yang kuat. Motivasi ini perlu secara eksplisit menunjukkan celah penelitian atau kelemahan pendekatan existing, sehingga posisi novelty karya Anda terlihat jelas di tengah perkembangan vision-language model dan foundation model terkini.

Bagian metode menuntut ketelitian teknis yang tinggi. Cantumkan arsitektur model, spesifikasi dataset, seluruh template prompt yang diuji, serta prosedur eksperimen secara berurutan. Detail seperti konfigurasi hyperparameter, strategi augmentasi, dan pipeline preprocessing wajib dirinci agar eksperimen dapat direplikasi tanpa ambiguitas oleh peneliti lain.

Penyajian hasil harus menggabungkan analisis kuantitatif dan kualitatif. Gunakan tabel akurasi per set prompt, grafik yang mengilustrasikan pengaruh variasi prompt terhadap performa, serta tabel metrik retrieval jika relevan. Sertakan juga contoh visual keberhasilan dan kegagalan model, karena studi kasus ekstrem sering kali mengungkap pola error sistemik yang tidak tertangkap oleh metrik agregat.

Bagian analisis merupakan jantung dari kajian tingkat S3. Kategorisasi kesalahan harus dilakukan secara sistematis, misalnya berdasarkan kesenjangan semantik, bias domain, atau ambiguitas linguistik. Diskusikan keterbatasan model secara kritis, lalu interpretasikan bagaimana representasi multimodal berhasil atau gagal menangkap makna kontekstual. Kaitkan temuan ini dengan teori atau paper foundational yang telah dibahas selama perkuliahan.

Kesimpulan harus menjawab pertanyaan riset secara langsung, merangkum implikasi teoretis maupun praktis, serta mengusulkan arah riset lanjutan yang spesifik dan terukur. Struktur pelaporan ini akan menjadi fondasi langsung bagi checklist reproduktibilitas yang akan kita bahas pada slide berikutnya, memastikan transparansi metodologi Anda sejak tahap perencanaan eksperimen hingga penyusunan proposal disertasi.

---

## Slide 040 - Reproducibility Checklist untuk Eksperimen Multimodal

### Narasi

Beralih dari struktur penulisan laporan eksperimen yang telah kita bahas pada slide sebelumnya, fokus kita kini bergeser ke aspek fundamental dalam penelitian tingkat doktoral: reproducibility atau kemampuan replikasi. Dalam ekosistem eksperimen multimodal, reproduktibilitas bukan sekadar kelengkapan administratif, melainkan prasyarat validitas ilmiah yang menentukan apakah temuan Anda dapat dipercaya, diverifikasi, dan dikembangkan oleh komunitas riset global.

Mari kita bedah komponen checklist ini secara terstruktur agar implementasinya tepat sasaran:
- **Versi lingkungan komputasi**: Catat secara eksplisit versi Python, PyTorch, dan library pendukung seperti `open_clip`. Variasi minor pada dependensi sering kali menyebabkan drift numerik pada embedding dan skor evaluasi.
- **Spesifikasi model**: Tuliskan nama arsitektur lengkap beserta path atau hash pretrained weights. Ini menjamin konsistensi representasi semantik yang digunakan selama inference maupun fine-tuning.
- **Template prompt**: Dokumentasikan seluruh variasi prompt yang diuji. Sensitivitas performa zero-shot classification dan image-text retrieval terhadap wording sangat tinggi, sehingga dokumentasi ini menjadi kunci analisis ablation study.
- **Random seed**: Tetapkan dan kunci seed untuk semua sumber keacakan, termasuk shuffling DataLoader, inisialisasi bobot acak, dan augmentasi gambar. Kondisi awal yang identik adalah syarat mutlak replikasi deterministik.
- **Dataset dan preprocessing**: Jelaskan pipeline transformasi, split train-val-test, serta teknik handling missing value atau class imbalance. Bias distribusi data sering kali menjadi akar kegagalan generalisasi ke domain baru.
- **Metrik dan rumus**: Paparkan formula matematis metrik evaluasi, terutama jika Anda memodifikasi metric standar atau menggabungkan multiple scoring functions untuk menangkap aspek semantik yang lebih kompleks.
- **Repositori kode dan hasil**: Simpan seluruh script, konfigurasi environment, dan artifact eksperimen dalam struktur folder yang rapi. Transparansi kode mempercepat proses peer-review dan memudahkan kolaborasi lintas tim.

Mengapa elemen-elemen ini menjadi prioritas? Secara praktis, checklist ini menghilangkan ambiguitas yang biasa memicu ketidaksesuaian hasil antar replikasi. Dari perspektif akademik, dokumen ini memungkinkan evaluator dan reviewer melakukan audit metodologis secara mendalam, mengidentifikasi potensi overfitting terhadap benchmark tertentu, atau mengadaptasi protokol Anda ke skenario aplikasi nyata. Dalam konteks penyusunan proposal disertasi, ketelitian ini mencerminkan kematangan experimental design dan kesiapan Anda berkontribusi pada state-of-the-art dengan standar keterbukaan ilmiah yang ketat.

Penerapan checklist ini akan menjadi landasan eksekusi yang kokoh sebelum kita menutup pertemuan hari ini. Pada slide berikutnya, kita akan merangkum enam poin kunci dari Pertemuan 05, mulai dari mekanisme contrastive learning pada CLIP, strategi ensembel prompt, hingga batasan fundamental model multimodal. Integrasi antara protokol reproducible yang kita diskusikan sekarang dengan kesimpulan tersebut akan membentuk kerangka kerja penelitian yang siap diuji validitasnya dan mengarah pada identifikasi research gap yang potensial untuk kontribusi disertasi Anda.

---

## Slide 041 - Kesimpulan Utama Pertemuan 05

### Narasi

Pada slide sebelumnya, kita telah menelaah checklist reproduktibilitas yang menjadi prasyarat metodologis dalam setiap eksperimen multimodal. Dokumentasi versi library, arsitektur model, template prompt, random seed, serta protokol preprocessing dan metrik bukan sekadar formalitas administratif, melainkan fondasi yang memungkinkan replikasi dan evaluasi kritis oleh komunitas ilmiah. Dengan landasan transparansi tersebut, kita dapat merangkum enam poin kunci dari pertemuan kelima mengenai model vision-language dan representasi multimodal.

Pertama, arsitektur CLIP mempelajari representasi citra dan teks secara simultan melalui mekanisme contrastive learning. Proses optimasi ini memaksa model untuk memaksimalkan kesamaan antara pasangan gambar-kata yang relevan, sekaligus menekan kesamaan dengan pasangan yang tidak berkorespondensi. Hasilnya adalah ruang embedding terpadu yang memungkinkan klasifikasi zero-shot bekerja murni dengan membandingkan vektor embedding citra terhadap embedding teks label, tanpa memerlukan tahap fine-tuning tambahan.

Kedua, kinerja zero-shot sangat sensitif terhadap rekayasa prompt atau *prompt engineering*. Perubahan kecil pada struktur kalimat, pilihan kata sifat, atau penambahan konteks dapat menggeser distribusi embedding secara signifikan. Untuk mengurangi varians dan meningkatkan robustness, strategi ensembel prompt sering diimplementasikan dengan menggabungkan prediksi dari beberapa template berbeda. Selain klasifikasi, prinsip ruang embedding bersama ini juga menjadi inti dari image-text retrieval, di mana kesamaan kosinus (*cosine similarity*) digunakan sebagai proxy semantik untuk menjembatani dua modalitas yang berbeda.

Ketiga, kekuatan transfer learning multimodal tidak lepas dari keterbatasan inheren. Model cenderung mewarisi bias dari dataset pelatihan, menunjukkan sensitivitas tinggi terhadap variasi linguistik, serta mengalami degradasi performa ketika menghadapi kesenjangan domain antara data pretraining dan aplikasi target. Karena itu, evaluasi yang rigor mutlak diperlukan. Protokol eksperimen harus dirancang dengan metrik yang sesuai, dilengkapi analisis kesalahan yang sistematis untuk mengungkap pola kegagalan, misalignment, atau artefak generasi.

Dari perspektif penelitian tingkat doktoral, poin-poin ini menawarkan arah yang jelas. CLIP dapat diadaptasi sebagai baseline eksperimen untuk menguji generalisasi multimodal pada domain spesifik seperti diagnostik medis, pemantauan lingkungan, atau analisis konten industri. Lebih jauh, investigasi terhadap bias representasional, kerentanan linguistik, serta teknik mitigasi domain shift merupakan celah penelitian yang masih terbuka lebar dan berpotensi menjadi kontribusi orisinal dalam disertasi.

Kesimpulan ini akan segera diterjemahkan ke dalam aktivitas praktis pada slide berikutnya. Anda diminta untuk mendesain dan menjalankan eksperimen zero-shot classification pada minimal sepuluh kelas, melakukan image-text retrieval dengan puluhan pasang sampel, serta menguji tiga set variasi prompt. Seluruh akurasi, metrik retrieval, dan contoh visual keberhasilan maupun kegagalan harus dicatat secara komprehensif. Pastikan setiap konfigurasi didokumentasikan agar hasil tetap reproducible, dan siapkan refleksi kritis Anda untuk dibahas intensif selama sesi research clinic.

---

## Slide 042 - Tugas dan Bukti Belajar

### Narasi

Setelah menelaah kesimpulan utama mengenai mekanisme CLIP, strategi prompt engineering, serta protokol evaluasi zero-shot dan retrieval pada slide sebelumnya, kita kini masuk ke tahap operasionalisasi pengetahuan tersebut melalui eksperimen terstruktur. Pada jenjang doktoral, setiap konsep representasi multimodal harus dibuktikan melalui implementasi yang rigor, terukur, dan siap dikembangkan menjadi kajian penelitian orisinal.

Tugas praktikum kali ini dibagi menjadi empat komponen inti yang saling melengkapi:
- Lakukan zero-shot classification pada dataset minimal 10 kelas. Fokuskan analisis pada distribusi kesalahan per kelas dan identifikasi apakah performa menurun pada kategori dengan ambiguitas visual atau semantik tinggi.
- Kerjakan image-text retrieval menggunakan 20 hingga 50 pasang data teks dan citra. Hitung metrik retrieval standar seperti Precision@K, Recall@K, dan mAP untuk mengukur seberapa efektif ruang embedding bersama dalam menjembatani kedua modalitas.
- Uji variasi prompt dengan minimal tiga set template berbeda. Amati bagaimana perubahan struktur kalimat, penambahan konteks domain, atau teknik ensembel prompt memengaruhi stabilitas skor kesamaan dan robustness model.
- Catat seluruh metrik akurasi dan retrieval, lalu susun analisis kesalahan yang menghubungkan kegagalan prediksi dengan karakteristik data atau kelemahan arsitektur model.

Luaran yang harus dikumpulkan berupa laporan eksperimen akademik yang memenuhi standar reproduktibilitas ilmiah. Laporan wajib menyajikan hasil kuantitatif dalam bentuk tabel atau grafik, dilengkapi contoh visual kasus keberhasilan dan kegagalan. Pembahasan harus secara kritis mengaitkan kinerja model dengan konfigurasi prompt yang digunakan, serta merefleksikan batasan inherent seperti bias dataset, sensitivitas bahasa, dan kesenjangan domain. Seluruh konfigurasi lingkungan, versi library, hyperparameter, dan seed acak harus didokumentasikan secara transparan agar eksperimen dapat divalidasi ulang. Hasil awal ini akan menjadi bahan diskusi substantif selama sesi research clinic.

Pemahaman mendalam tentang alignment multimodal dan evaluasi berbasis embedding yang Anda bangun melalui tugas ini akan langsung bertransisi ke topik pertemuan berikutnya. Image Restoration dan Computational Imaging tidak lagi bergantung semata pada metrik pixel-wise seperti PSNR atau SSIM, melainkan mulai mengintegrasikan representasi bahasa untuk menilai kualitas persepsi secara otomatis. Kemampuan Anda dalam memanipulasi dan menganalisis ruang fitur multimodal hari ini akan menjadi fondasi metodologis ketika kita mengeksplorasi bagaimana deskripsi teks dapat berfungsi sebagai constraint semantik dalam proses inversi dan restorasi citra.

---

## Slide 043 - Persiapan Pertemuan Berikutnya

### Narasi

Slide ini berfungsi sebagai pengarah strategis menuju topik pertemuan berikutnya, yaitu Image Restoration dan Computational Imaging. Fokus utama pada sesi mendatang mencakup pemecahan masalah invers dalam pemulihan citra, karakterisasi berbagai bentuk degradasi, serta teknik inti seperti denoising, deblurring, dan super-resolusi. Dari sudut pandang penelitian doktoral, penekanan bergeser dari sekadar perbaikan piksel menuju integrasi representasi visual dan multimodal untuk mengevaluasi kualitas hasil restorasi secara lebih kontekstual dan human-aligned.

Keterkaitan materi ini dengan Vision-Language Models yang telah kita pelajari sangat signifikan. Arsitektur model multimodal dapat diadaptasi untuk mencocokkan deskripsi tekstual spesifik dengan citra yang telah direstorasi, sekaligus mengevaluasi kualitas persepsi secara otomatis. Mekanisme alignment antara ruang fitur visual dan linguistik menjadi fondasi kritis bagi pengembangan metrik evaluasi berbasis bahasa yang lebih robust, melampaui keterbatasan indikator numerik tradisional.

Sebagai persiapan teknis, tinjaulah kembali konsep degradasi citra serta prinsip perhitungan PSNR dan SSIM. Siapkan pula pertanyaan analitis mengenai perbandingan antara evaluasi berbasis persepsi manusia versus evaluasi numerik murni. Diskusi ini dirancang untuk menguji kemampuan Anda dalam memposisikan kriteria keberhasilan algoritma restorasi sesuai dengan kebutuhan aplikasi nyata dan standar publikasi internasional.

Alur ini melanjutkan eksplorasi praktis dari slide sebelumnya, di mana Anda telah melaksanakan zero-shot classification, image-text retrieval, dan variasi prompt. Langkah selanjutnya adalah mengkonversi kemampuan alignment tersebut menjadi instrumen evaluasi untuk computational imaging. Referensi fundamental yang akan kita bahas pada slide berikutnya, termasuk CLIP, ALIGN, BLIP, hingga BLIP-2, akan menjadi landasan konseptual utama dalam merancang metodologi penelitian dan identifikasi research gap Anda pada domain evaluasi multimodal untuk restorasi citra.

---

## Slide 044 - Referensi dan Bacaan Lanjutan

### Narasi

Slide ini menyajikan daftar referensi utama dan bacaan lanjutan yang menjadi fondasi akademis untuk pertemuan kelima mengenai *Vision-Language Models* dan representasi multimodal. Pada jenjang doktoral, penguasaan literatur tidak hanya bersifat komprehensif, tetapi harus diarahkan untuk melakukan kajian kritis, mengidentifikasi *research gap*, serta mengevaluasi metodologi state-of-the-art secara sistematis.

Paper inti yang wajib Anda telaah adalah karya Radford dkk. tentang CLIP. Model ini memperkenalkan paradigma pembelajaran representasi visual yang dapat ditransfer melalui supervisi bahasa alami, sehingga memungkinkan pencocokan semantik antara ruang fitur citra dan teks tanpa pelatihan task-specific yang intensif. Sebagai bahan pembanding, paper ALIGN dari Jia dkk. menyoroti bagaimana skalabilitas data dan penggunaan label noisy secara masif dapat meningkatkan kualitas embedding multimodal tanpa bergantung pada anotasi manual yang presisi.

Untuk evolusi arsitektur berikutnya, BLIP dan BLIP-2 dari Li dkk. menawarkan mekanisme bootstrap yang menyatukan tugas pemahaman dan generasi dalam satu kerangka terpadu. BLIP-2 secara khusus menonjol dengan strategi memanfaatkan encoder citra beku yang dihubungkan ke Large Language Model melalui komponen ringan, sehingga menekan beban komputasi sekaligus mempertahankan kinerja tinggi pada benchmark multimodal.

Secara implementasi, Anda dapat mengeksplorasi ketiga keluarga model tersebut melalui dokumentasi OpenCLIP dan ekosistem Hugging Face Transformers. Keduanya menyediakan antarmuka standar untuk loading checkpoint, inferensi batch, serta ekstraksi fitur yang kompatibel dengan PyTorch dan scikit-learn. Familiaritas dengan pipeline preprocessing dan struktur kode ini akan menjadi dasar praktis saat Anda merancang eksperimen awal untuk proposal disertasi.

Kaitannya dengan slide sebelumnya, referensi ini juga sangat relevan ketika kita membahas evaluasi kualitas pada *Image Restoration*. Model multimodal dapat berfungsi sebagai alternatif metrik numerik tradisional seperti PSNR atau SSIM, misalnya untuk mencocokkan deskripsi linguistik dengan hasil denoising atau super-resolution guna mengukur persepsi visual secara otomatis dan kontekstual.

Di slide berikutnya, kita akan menutup sesi ini dengan ucapan terima kasih dan memberikan transisi menuju topik pertemuan selanjutnya, yaitu *Image Restoration* dan *Computational Imaging*. Persiapkan diri Anda untuk mendiskusikan bagaimana representasi multimodal dapat diintegrasikan ke dalam kerangka *inverse problem* serta pergeseran paradigma evaluasi dari pengukuran piksel menuju penilaian berbasis bahasa.

---

## Slide 045 - TERIMA KASIH

### Narasi

Slide penutup ini menandai berakhirnya pembahasan mengenai *Vision-Language Models* dan representasi multimodal. Pada tingkat doktoral, penguasaan terhadap konsep *cross-modal alignment*, *contrastive pre-training*, dan arsitektur hybrid vision-language menjadi prasyarat penting untuk merumuskan masalah penelitian yang memiliki kontribusi signifikan. Fokus tidak lagi hanya pada penerapan model siap pakai, melainkan pada analisis kritis terhadap mekanisme pembelajaran, skalabilitas data teks berisik, serta keterbatasan generalisasi antar domain.

Dari perspektif implementasi, ekosistem tools seperti OpenCLIP dan Hugging Face Transformers telah mendemokratisasi akses ke model state-of-the-art. Namun, tantangan riset aktual terletak pada optimalisasi *parameter-efficient fine-tuning*, mitigasi bias representasi, serta pengembangan metrik evaluasi yang mampu mengukur koherensi semantik lintas modalitas secara objektif. Mahasiswa didorong untuk mengevaluasi trade-off antara kompleksitas arsitektur dan efisiensi komputasi, terutama ketika mengintegrasikan *frozen image encoders* dengan Large Language Models.

Referensi yang disajikan pada slide sebelumnya berfungsi sebagai peta literatur untuk eksplorasi lebih lanjut. Paper-paper inti seperti CLIP, BLIP, dan BLIP-2 menawarkan variasi strategis dalam *bootstrapping* dan *self-supervision* yang dapat dijadikan baseline atau inspirasi untuk mengembangkan arsitektur baru. Identifikasi *research gap* pada bagian abstrak dan diskusi masing-masing paper akan menjadi bahan baku utama dalam penyusunan proposal disertasi.

Untuk pertemuan berikutnya, kita akan memasuki topik **Image Restoration dan Computational Imaging**. Subtopik ini akan mengupas pemulihan citra terdegradasi melalui pendekatan fisika-informed optimization dan neural reconstruction, mencakup super-resolusi, denoising, deblurring, serta integrasi sensor komputasional dengan pipeline diferensiabel. Persiapkan diri Anda untuk mengeksplorasi hubungan antara regularisasi invers, model generatif, dan evaluasi kualitas citra secara kuantitatif maupun perseptual.

Terima kasih atas kontribusi dan diskusi intensif selama pertemuan ini. Silakan lanjutkan uji coba skrip dan telaah literatur pendukung sebelum sesi berikutnya dimulai.
