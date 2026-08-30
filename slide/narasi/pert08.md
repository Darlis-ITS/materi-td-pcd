# Narasi TD Pengolahan Citra Digital - Pertemuan 08

## Segmentasi Citra dan Promptable Foundation Models

Sumber: markdown/pert08-segmentasi-citra-dan-promptable-foundation-models.md

---

## Slide 000 - Cover

### Narasi

Pertemuan kedelapan ini menandai pergeseran fokus dari deteksi objek berbasis bounding box menuju segmentasi citra bergranularitas piksel, disertai eksplorasi mendalam terhadap model fondasi yang dapat diprompt (*promptable foundation models*). Topik ini menjadi jembatan penting antara arsitektur visi tradisional dan era model besar yang dirancang untuk menerima berbagai bentuk instruksi, mulai dari teks, koordinat, hingga mask kasar, sebagai input untuk menghasilkan segmentasi yang akurat dan kontekstual.

Posisi materi ini dalam kurikulum menunjukkan kesinambungan metodologis dari pertemuan sebelumnya. Setelah Pertemuan 01–07 membangun fondasi mengenai peta riset, teknik *critical reading*, representasi visual, *self-supervised learning*, model visi-bahasa, restorasi citra, serta deteksi objek menggunakan YOLO dan transformer, kini kita memasuki tahap prediksi mask per-pixel. Beberapa poin kunci yang perlu ditekankan:
- Transisi dari deteksi ke segmentasi menuntut pemahaman yang lebih mendalam tentang batas spasial, hierarki semantik, dan interaksi antar-objek dalam scene.
- Arsitektur Vision Transformer dan mekanisme *self-supervised learning* yang diperkenalkan pada Pertemuan 03 dan 04 menjadi tulang punggung model seperti Segment Anything Model (SAM), yang mampu menggeneralisasi ke domain未见 tanpa fine-tuning ekstensif.
- Pendekatan *promptable* mengubah cara kita mendesain antarmuka manusia-mesin, memungkinkan adaptasi cepat ke tugas spesifik hanya dengan mengubah format prompt, bukan arsitektur model.

Dari perspektif penelitian tingkat doktoral, segmentasi modern bukan sekadar peningkatan resolusi output, melainkan perubahan paradigma dalam merepresentasikan pengetahuan spasial-semantic. Model fondasi terkini memanfaatkan ruang fitur yang telah dilatih secara masif untuk mengekstrak struktur yang kaya, sehingga membuka peluang riset signifikan dalam hal efisiensi komputasi, robustness terhadap noise, adaptasi domain terbatas, serta integrasi multimodal. Mahasiswa diharapkan mampu mengidentifikasi celah metodologis, misalnya pada aspek skalabilitas prompt, konsistensi cross-domain, atau mitigasi bias data pelatihan, yang dapat dikembangkan menjadi pertanyaan penelitian yang tajam.

Materi ini juga menyiapkan landasan konseptual bagi Pertemuan 09, di mana kita akan membahas *generative vision* dan *diffusion models*. Segmentasi tidak lagi dipandang sebagai tugas akhiran semata, melainkan dapat berfungsi sebagai *conditioning signal* yang mengarahkan proses generasi gambar, meningkatkan koherensi struktural, dan mengurangi artefak umum pada model generatif. Dengan memahami bagaimana mask segmentasi dapat mengontrol distribusi latens atau guidance classifier-free, mahasiswa akan memiliki kerangka kerja yang kuat untuk merancang eksperimen rigor, memposisikan kontribusi ilmiah mereka terhadap state-of-the-art, dan menyusun proposal disertasi yang relevan dengan tren terkini dalam pengolahan citra digital dan computer vision.

---

## Slide 001 - Posisi Pertemuan dalam Rangkaian Perkuliahan

### Narasi

Slide ini memetakan kedudukan Pertemuan 08 dalam alur perkembangan mata kuliah Topik Dalam Pengolahan Citra Digital. Fase sebelumnya, pertemuan 01 hingga 07, telah menyelesaikan pembangunan fondasi metodologis mulai dari pemetaan riset, teknik critical reading, representasi visual, self-supervised learning, integrasi vision-language, image restoration, hingga deteksi objek modern. Fokus utama pada pertemuan kali ini secara eksplisit dialihkan ke segmentasi citra dan pengembangan promptable foundation models. Pergeseran ini menandai transisi paradigmatik dari pendekatan deteksi berbasis koordinat ruang ke prediksi mask yang bersifat per-pixel, sehingga meningkatkan resolusi semantik dan presisi batas objek secara signifikan.

Keterkaitan materi antar pertemuan dirancang untuk memastikan kesinambungan arsitektural dan konseptual. Berikut adalah poin kunci hubungan materi:
- Pertemuan 07 yang membahas object detection modern dengan YOLO dan transformer menjadi batu loncatan menuju prediksi mask.
- Konsep Vision Transformer dan self-supervised learning dari Pertemuan 03 dan 04 diintegrasikan langsung ke dalam desain arsitektur Segment Anything Model.
- Pengetahuan tentang conditioning dan kontrol generasi akan menjadi prasyarat penting untuk Pertemuan 09 yang membahas diffusion models.

Dengan memahami peta ini, kita melihat bahwa segmentasi bukan lagi sekadar tugas klasifikasi piksel, melainkan komponen kritis yang berfungsi sebagai spatial prior atau conditioning signal dalam pipeline generatif dan multimodal. Hal ini juga menyiapkan konteks empiris untuk slide berikutnya, di mana kita akan merinci capaian pembelajaran spesifik. Mahasiswa diharapkan mampu membedakan semantic, instance, dan panoptic segmentation secara konseptual, mengevaluasi arsitektur U-Net, transformer-based segmenters, dan SAM secara kritis, serta merancang eksperimen evaluasi menggunakan metrik IoU dan Dice. Semua capaian tersebut selaras dengan CPMK analisis mutakhir, evaluasi paper, perancangan eksperimen prompt, serta implementasi praktis dalam ekosistem PyTorch dan Hugging Face. Mari kita lanjutkan ke pembahasan teknis dan arsitektural foundation models yang mendefinisikan ulang state-of-the-art segmentasi saat ini.

---

## Slide 002 - Tujuan Pembelajaran dan Capaian

### Narasi

Pada slide ini, kita menetapkan tujuan pembelajaran dan capaian yang menjadi acuan utama untuk Pertemuan 08. Setelah pada pertemuan sebelumnya kita menelusuri peta riset, membaca paper secara kritis, serta membahas object detection berbasis YOLO dan transformer, fokus kini bergeser dari prediksi bounding box menuju prediksi mask per-pixel yang lebih presisi. Slide ini merangkum lima capaian inti yang harus dikuasai oleh mahasiswa doktoral dalam konteks segmentasi citra modern.

Pertama, mahasiswa diharapkan mampu membedakan secara konseptual dan teknis antara semantic segmentation, instance segmentation, dan panoptic segmentation. Perbedaan mendasar terletak pada bagaimana setiap metode menangani kelas objek, tumpang tindih antarobjek, serta representasi latar belakang. Pemahaman ini krusial karena pemilihan taksonomi segmentasi akan menentukan arsitektur model, fungsi loss, hingga strategi evaluasi yang tepat dalam penelitian Anda.

Kedua, kita akan membedah arsitektur kunci yang mendominasi evolusi segmentasi, mulai dari U-Net sebagai fondasi encoder-decoder dengan skip connection, hingga transformer-based segmentation yang memanfaatkan self-attention global. Lebih lanjut, kita akan mengupas arsitektur Segment Anything Model (SAM), sebuah promptable foundation model yang mengubah paradigma segmentasi dari task-specific menjadi task-agnostic. Arsitektur ini sangat bergantung pada konsep Vision Transformer dan self-supervised learning yang telah dibahas di awal perkuliahan, sehingga membentuk kesinambungan logis dalam rangkaian materi.

Ketiga, peran prompt pada promptable foundation model akan dianalisis secara mendalam. Prompt dapat berupa titik, kotak, teks, atau bahkan mask kasar, yang berfungsi sebagai conditioning signal untuk mengarahkan model menghasilkan segmentasi spesifik tanpa perlu fine-tuning ulang. Pada tingkat doktoral, Anda tidak hanya menggunakan prompt sebagai fitur, tetapi juga mengevaluasi stabilitas respons model terhadap variasi prompt, ambiguitas semantik, dan noise input.

Keempat, evaluasi kualitas mask akan dibekali dengan metrik standar industri dan akademik, yaitu Intersection over Union (IoU) dan Dice Coefficient. Kedua metrik ini mengukur overlap geometris antara prediksi dan ground truth, namun memiliki sensitivitas berbeda terhadap ketidakseimbangan kelas dan ukuran objek kecil. Mahasiswa didorong untuk memahami batasan metrik tersebut, terutama ketika diterapkan pada dataset medis, satelit, atau dokumen historis yang sering mengalami class imbalance ekstrem.

Kelima, analisis generalisasi lintas domain dan pengaruh domain shift menjadi fokus kritis. Foundation model seperti SAM memang menunjukkan zero-shot capability yang kuat, namun performanya dapat menurun signifikan ketika menghadapi distribusi data yang menyimpang dari training set, misalnya pada citra dengan pencahayaan ekstrem, resolusi rendah, atau domain aplikasi yang sama sekali baru. Diskusi ini akan mempersiapkan Anda untuk merancang eksperimen robust dan mengidentifikasi research gap terkait adaptasi domain dalam pipeline segmentasi.

Capaian-capaian tersebut selaras dengan empat CPMK mata kuliah. CPMK-1 menuntut analisis perkembangan mutakhir segmentasi, CPMK-2 menekankan evaluasi kritis terhadap paper SAM dan metode turunannya, CPMK-3 mengarahkan perancangan eksperimen evaluasi prompt yang terstruktur, dan CPMK-4 memastikan pemanfaatan praktis SAM dalam ekosistem PyTorch dan Hugging Face. 

Agenda pada slide berikutnya akan menerjemahkan capaian ini ke dalam alur pembelajaran konkret, dimulai dari taksonomi dasar, metrik evaluasi, arsitektur klasik hingga modern, perbandingan pendekatan supervised versus promptable, eksperimen hands-on dengan berbagai tipe prompt, hingga diskusi penelitian tentang domain shift dan keterbatasan evaluasi. Target keluaran akhir pertemuan ini adalah kemampuan Anda mengevaluasi strategi prompt pada SAM dan menyusun rekomendasi strategis penerapannya dalam pipeline riset disertasi masing-masing.

---

## Slide 003 - Agenda Pertemuan

### Narasi

Agenda pertemuan ini disusun untuk menuntun pemahaman dari fondasi teoretis menuju implementasi kritis pada *promptable foundation models*. Kita akan memulai dengan konsep dasar dan taksonomi segmentasi, mengingat kembali perbedaan mendasar antara *semantic*, *instance*, dan *panoptic segmentation* yang telah menjadi capaian pembelajaran sebelumnya. Pemahaman taksonomi ini penting karena menentukan bagaimana kita merancang *loss function*, memilih arsitektur, dan menyiapkan dataset label di tingkat piksel.

Selanjutnya, kita akan membahas metrik evaluasi seperti *Intersection over Union* (IoU) dan *Dice coefficient*, serta parameter kualitas *mask* lainnya. Pada jenjang doktoral, evaluasi tidak cukup hanya melihat skor agregat; kita perlu menganalisis distribusi kesalahan per-piksel, sensitivitas terhadap objek kecil, dan bias yang muncul akibat ketidakseimbangan kelas. Metrik ini akan menjadi acuan objektif saat kita membandingkan performa model di berbagai skenario.

Pembahasan teknis kemudian bergerak ke arsitektur, dimulai dari U-Net sebagai kerangka *encoder-decoder* klasik, lalu berkembang ke arsitektur berbasis *transformer* yang memanfaatkan mekanisme *attention* global untuk menangkap konteks jangka panjang. Dari sinilah kita memasuki inti materi: *promptable segmentation* dan arsitektur SAM. Di sini, *prompt* berfungsi sebagai sinyal kontrol yang memungkinkan satu model umum beradaptasi ke berbagai tugas tanpa fine-tuning ulang, mengubah paradigma dari *task-specific training* menjadi *inference-time conditioning*.

Untuk menguji kedalaman adaptasi tersebut, kita akan membandingkan pendekatan *supervised segmentation* konvensional dengan *promptable segmentation*. Perbandingan ini akan menyoroti trade-off antara akurasi terukur, kebutuhan data anotasi, dan skalabilitas lintas domain. Eksperimen langsung menggunakan SAM dengan berbagai tipe *prompt*—mulai dari titik (*point*), kotak (*box*), hingga teks (*text*)—akan dilakukan untuk mengamati sensitivitas model terhadap variasi input dan batas kemampuan generalisasinya.

Diskusi penelitian akan menutup rangkaian ini dengan fokus pada tiga isu strategis: mitigasi *domain shift* ketika model diterapkan pada data medis atau satelit yang berbeda distribusi, optimalisasi kualitas *prompt* dalam pipeline otomatis, serta keterbatasan metrik evaluasi saat menghadapi *edge cases* atau objek dengan ambiguitas tinggi. Target keluaran yang diharapkan adalah kemampuan Anda mengevaluasi strategi *prompt* secara empiris dan menyusun rekomendasi penggunaan SAM yang relevan dengan pipeline riset dan arah proposal disertasi masing-masing.

Alur pembahasan ini secara alami akan melanjutkan ke transisi dari representasi deteksi berbasis *bounding box* menuju prediksi segmentasi per-piksel, yang akan kita bedah lebih detail pada slide berikutnya.

---

## Slide 004 - Dari Deteksi ke Segmentasi

### Narasi

Pada slide ini, kita akan menelusuri pergeseran fundamental dari deteksi objek konvensional menuju segmentasi citra tingkat piksel. Mari kita mulai dengan mengingat kembali poin kunci dari pertemuan sebelumnya. Model object detection seperti YOLO menghasilkan bounding box beserta label kelas secara langsung dalam satu tahap inferensi. Sementara itu, arsitektur modern berbasis transformer seperti DETR memanfaatkan mekanisme attention global untuk memprediksi set objek secara simultan. Meskipun efisien untuk identifikasi awal, output berupa bounding box memiliki keterbatasan struktural yang signifikan.

Bounding box hanya memberikan batas geometris berbentuk persegi panjang yang sering kali menyertakan area latar belakang atau memotong bagian tepi objek. Banyak aplikasi riset dan industri menuntut presisi hingga tingkat piksel. Contoh konkretnya meliputi pemisahan sel biologis, analisis lesi medis, pengukuran luas area bangunan, atau segmentasi kendaraan otonom. Untuk kasus-kasus tersebut, kotak pembatas tidak lagi memadai karena kita membutuhkan batas yang mengikuti kontur alami objek secara akurat.

Perhatikan kontinuum representasi yang disajikan pada slide ini. Alurnya bergerak dari bounding box yang bersifat kasar, menuju instance mask yang mengisolasi setiap objek individu, kemudian semantic mask yang mengelompokkan piksel berdasarkan kategori kelas, dan akhirnya panoptic mask yang merangkum seluruh piksel dalam gambar tanpa terkecuali. Diagram berikut merepresentasikan hierarki granularitas tersebut:
```text
Bounding box  ->  Instance mask  ->  Semantic mask  ->  Panoptic mask
(kasar)            (presisi)          (per-kelas)         (semua piksel)
```
Setiap transisi dalam kontinuum ini menambah dimensi informasi spasial dan semantik yang lebih kaya, mempersiapkan data untuk analisis kuantitatif yang lebih mendalam.

Inti dari pembahasan ini terletak pada redefinisi tugas segmentasi. Berbeda dengan deteksi yang beroperasi pada level proposal region atau objek diskrit, segmentasi adalah tugas prediksi di level piksel. Setiap koordinat dalam citra harus dipetakan ke dalam kelas tertentu, baik itu background, foreground, maupun kategori spesifik lainnya. Pendekatan ini mengubah paradigma pengolahan citra dari sekadar pelokasian menjadi pemahaman struktural dan topologis yang lengkap.

Pemahaman tentang kontinuum representasi dan sifat prediksi piksel ini menjadi landasan kritis sebelum kita membahas perbandingan teknis lebih lanjut pada slide berikutnya. Di sana, kita akan membedah perbedaan output, metrik evaluasi, kompleksitas anotasi, serta implikasi praktis masing-masing pendekatan dalam konteks pipeline penelitian computer vision tingkat lanjut.

---

## Slide 005 - Deteksi vs Segmentasi: Output yang Dihasilkan

### Narasi

Melanjutkan konsep kontinuum representasi pada slide sebelumnya, slide ini memberikan perbandingan eksplisit antara keluaran (*output*) sistem *object detection* dan *segmentasi*. Deteksi objek beroperasi pada level objek diskrit, menghasilkan *bounding box* koordinat beserta label kelasnya. Pendekatan ini efisien secara komputasi namun bersifat aproksimasi, karena kotak pembatas tidak pernah benar-benar menempel pada kontur asli objek. Sebaliknya, segmentasi melakukan prediksi langsung pada domain piksel, menghasilkan *mask* biner atau multi-kelas yang mengikuti bentuk geometris aktual. Pergeseran dari level objek ke level piksel inilah yang mengubah seluruh kerangka evaluasi dan desain eksperimen dalam penelitian pengolahan citra digital.

Tabel perbandingan menyoroti lima dimensi kritis yang harus dipertimbangkan saat merumuskan metodologi penelitian:
- **Output & Batas Objek**: Deteksi memberikan batas kasar berbentuk persegi, sedangkan segmentasi menawarkan presisi spasial yang mengikuti tepi objek.
- **Metrik Evaluasi**: Deteksi mengandalkan *mean Average Precision* (mAP) dan kurva presisi-rekall. Segmentasi menuntut metrik berbasis tumpang tindih wilayah seperti *Intersection over Union* (IoU), koefisien Dice, atau *Panoptic Quality* (PQ) yang lebih sensitif terhadap kesalahan lokal pada batas objek.
- **Kompleksitas Anotasi**: Penandaan per piksel memerlukan konsistensi tinggi dan waktu annotator yang jauh lebih lama, sehingga berdampak langsung pada strategi pengumpulan dataset dan alokasi anggaran riset.
- **Penggunaan Ilmiah**: Deteksi cocok untuk agregasi statistik (misalnya menghitung jumlah objek), sementara segmentasi diperlukan untuk analisis morfologis, pengukuran luas area, ekstraksi kontur, dan pemetaan regional yang detail.

Ilustrasi ASCII pada slide ini secara visual memperkuat kontras tersebut. Garis lurus horizontal merepresentasikan batasan rigid dari *bounding box*, sedangkan garis lengkung menggambarkan bagaimana *mask* segmentasi menyesuaikan diri dengan kelengkungan dan irregularitas objek nyata. Dalam konteks penelitian tingkat doktoral, informasi tambahan ini bukan sekadar peningkatan estetika visual, melainkan variabel kuantitatif yang memungkinkan ekstraksi fitur geometri, analisis tekstur lokal, atau bahkan menjadi *ground truth* untuk melatih model generatif dan *diffusion-based image restoration*.

Meskipun segmentasi menawarkan informasi yang jauh lebih kaya, tantangan utamanya terletak pada biaya anotasi dan kebutuhan komputasi yang lebih tinggi. Perkembangan terkini dalam *promptable foundation models* seperti Segment Anything Model (SAM) dan arsitektur vision-language mulai mengatasi hambatan ini dengan memungkinkan segmentasi adaptif melalui teks, titik, atau kotak referensi tanpa memerlukan pelatihan ulang end-to-end. Transisi menuju paradigma yang lebih fleksibel ini menjadi landasan logis untuk memahami klasifikasi tugas segmentasi secara lebih terstruktur.

Pada slide berikutnya, kita akan membahas taksonomi segmentasi citra yang membagi tugas ini menjadi tiga paradigma utama: *semantic*, *instance*, dan *panoptic*. Pemahaman atas perbedaan output, metrik, dan trade-off anotasi pada slide ini akan menjadi acuan penting ketika kita mengevaluasi kapan masing-masing paradigma paling relevan untuk menjawab pertanyaan penelitian tertentu.

---

## Slide 006 - Taksonomi Segmentasi Citra

### Narasi

Setelah pada slide sebelumnya kita membahas perbedaan fundamental antara deteksi objek yang menghasilkan bounding box dengan segmentasi yang memberikan mask per-pixel, kini kita masuk ke taksonomi segmentasi citra itu sendiri. Dalam literatur computer vision tingkat lanjut, khususnya untuk penelitian doktoral, pemahaman klasifikasi ini menjadi fondasi penting dalam merumuskan masalah penelitian dan memilih arsitektur model yang tepat.

Taksonomi segmentasi dapat dibagi menjadi tiga paradigma utama, seperti yang terlihat pada diagram di slide ini:
- **Semantic Segmentation**: Setiap piksel diklasifikasikan ke dalam satu label kelas. Model tidak membedakan individu yang berbeda dalam kelas yang sama. Semua piksel yang termasuk kategori tertentu akan menerima label identik, terlepas dari jumlah atau posisi objeknya.
- **Instance Segmentation**: Fokus pada pemisahan setiap objek secara individual. Setiap instance objek diberi mask yang terpisah, sehingga sangat cocok untuk tugas yang membutuhkan penghitungan, pelacakan, atau analisis bentuk per objek.
- **Panoptic Segmentation**: Menggabungkan kedua pendekatan di atas dalam satu keluaran terpadu. Paradigma ini memisahkan prediksi menjadi dua kategori utama: *stuff* (area tekstural atau background seperti langit, jalan, rumput) dan *things* (objek diskrit yang dapat dihitung seperti orang, kendaraan, hewan). Semua piksel dalam citra harus dilabeli, baik sebagai stuff maupun things, tanpa ada piksel yang dibiarkan kosong.

Pemilihan taksonomi ini sangat bergantung pada tujuan aplikasi, kompleksitas scene, dan ketersediaan data anotasi. Untuk penelitian yang berorientasi pada foundation models atau promptable architectures seperti SAM (Segment Anything Model), pemahaman mendalam terhadap ketiga taksonomi ini menentukan bagaimana prompting strategy dirancang, apakah menggunakan point, box, text prompt, atau free-form mask, serta bagaimana model menggeneralisasi pengetahuan lintas domain.

Pada slide berikutnya, kita akan mendalami lebih lanjut semantic segmentation, mulai dari definisi teknis, contoh penerapan di domain perkotaan dan medis, hingga metrik evaluasi standar seperti mean IoU serta catatan kritis mengenai penggunaan akurasi per-pixel pada dataset yang tidak seimbang. Penjelasan ini akan menjadi dasar metodologis saat kita mengevaluasi arsitektur encoder-decoder modern atau transformer-based segmentation networks dalam konteks state-of-the-art.

---

## Slide 007 - Semantic Segmentation

### Narasi

Setelah pada slide sebelumnya kita menguraikan taksonomi segmentasi citra yang membagi bidang ini menjadi tiga paradigma utama, kini kita mendalami fondasi pertama, yaitu *semantic segmentation*. Pada pendekatan ini, setiap piksel dalam citra diklasifikasikan secara independen ke dalam satu label kategori. Karakteristik utamanya adalah pengelompokan berdasarkan makna visual atau semantik, tanpa mempertimbangkan apakah piksel-piksel tersebut berasal dari individu objek yang berbeda atau merupakan bagian kontinu dari suatu area.

Implementasi konsep ini sangat relevan dalam aplikasi yang memerlukan pemahaman konteks spasial secara menyeluruh. Pada pemetaan lingkungan perkotaan, model bertugas memisahkan area jalan, struktur bangunan, kendaraan, dan vegetasi secara konsisten di seluruh frame. Di ranah biomedis, teknik serupa digunakan untuk membatasi organ target atau lesi jaringan pada citra histopatologi dan radiologi, yang menjadi langkah kritis dalam ekstraksi fitur kuantitatif dan analisis longitudinal.

Ilustrasi pada slide menggambarkan transformasi langsung dari ruang warna RGB ke tensor label diskrit. Seluruh piksel yang membentuk permukaan jalan akan menerima nilai label identik, demikian pula dengan area mobil atau daun pohon. Secara komputasional, ini berarti masalah visi komputer direduksi menjadi tugas klasifikasi multikelas yang dieksekusi secara paralel di setiap koordinat piksel, biasanya diimplementasikan melalui arsitektur encoder-decoder seperti U-Net, DeepLab, atau FCN yang memanfaatkan skip connection untuk mempertahankan resolusi spasial.

Evaluasi kinerja pada *semantic segmentation* secara standar mengandalkan metrik Mean Intersection over Union (mIoU) yang mengukur rata-rata overlap antara prediksi dan anotasi referensi di seluruh kelas. Perlu ditekankan bahwa akurasi per-piksel sering kali bersifat menipu, terutama ketika distribusi kelas tidak seimbang. Dalam skenario dunia nyata, dominasi piksel latar belakang atau objek makro dapat mendominasi skor akurasi, sehingga menutupi kegagalan model dalam mengenali kelas minoritas yang justru sering menjadi fokus investigasi ilmiah tingkat lanjut.

Memahami batasan representasi dan evaluasi pada *semantic segmentation* menjadi landasan penting sebelum kita melangkah ke kompleksitas yang lebih tinggi. Pada slide berikutnya, kita akan membahas *instance segmentation*, di mana fokus bergeser dari pengelompokan semantik menjadi pemisahan mask individual untuk setiap entitas objek yang terdeteksi, bahkan jika mereka berbagi kelas yang sama.

---

## Slide 008 - Instance Segmentation

### Narasi

Pada slide sebelumnya, kita telah membahas *Semantic Segmentation* yang memberikan satu label kelas seragam untuk seluruh piksel yang termasuk dalam kategori tertentu. Pendekatan ini sangat efektif untuk pemetaan wilayah semantik, namun memiliki keterbatasan mendasar: ia tidak mampu membedakan dua objek berbeda yang berada pada kelas yang sama. Jika terdapat dua mobil berdampingan, *semantic segmentation* akan memberinya label yang identik tanpa mempertimbangkan batas fisik atau keberadaan independen masing-masing kendaraan.

Untuk mengatasi keterbatasan tersebut, *Instance Segmentation* hadir sebagai evolusi tugas segmentasi yang berfokus pada prediksi *mask* untuk setiap individu objek secara terpisah. Dalam paradigma ini, setiap entitas yang terdeteksi mendapatkan identitas mask yang unik, sehingga dua objek dengan kelas semantik sama tetap dipisahkan secara geometris dan topologis. Secara umum, tugas ini hanya menargetkan objek diskrit yang dapat dihitung atau dilacak, yang dalam literatur sering dikategorikan sebagai *things*, sementara area latar seperti langit atau jalan dikesampingkan dari prediksi mask.

Contoh aplikasi nyata dari pendekatan ini meliputi segmentasi setiap individu dalam kerumunan padat (*crowd segmentation*) maupun identifikasi dan penghitungan sel tunggal pada citra mikroskop biomedis. Ilustrasi pada slide menunjukkan bagaimana dua mobil yang berdampingan tidak lagi digabung menjadi satu label “mobil”, melainkan dipecah menjadi mask A dan mask B yang saling eksklusif. Pemisahan ini krusial untuk *downstream task* seperti pelacakan multi-objek (*multi-object tracking*), analisis kepadatan spasial, atau sistem otonom yang memerlukan estimasi jarak dan interaksi antar entitas.

Evaluasi performa *instance segmentation* umumnya menggeser metrik dari Mean IoU ke *Average Precision* (AP) berbasis mask. Metrik ini menghitung IoU antara mask prediksi dan *ground truth* untuk setiap instance, kemudian menerapkan ambang batas IoU (biasanya 0.5 hingga 0.75) untuk menentukan *true positive*. Hasil akhir dirata-ratakan melintasi berbagai tingkat keakuratan (*recall*) dan ambang IoU, sehingga memberikan gambaran yang lebih robust mengenai kemampuan model dalam mendeteksi sekaligus membatasi bentuk objek secara presisi. Pendekatan ini juga menuntut arsitektur model yang mampu menghasilkan *mask head* paralel terhadap klasifikasi dan regresi bounding box, seperti pada keluarga Mask R-CNN atau metode *anchor-free* terkini.

Meskipun powerful, pendekatan ini masih bersifat parsial karena mengabaikan komponen latar belakang non-diskrit. Keterbatasan inilah yang menjadi jembatan alami menuju konsep pada slide berikutnya, yaitu *Panoptic Segmentation*. Dengan menggabungkan keunggulan *semantic segmentation* untuk area *stuff* dan *instance segmentation* untuk objek *things*, panoptic segmentation menawarkan representasi pixel-per-pixel yang lengkap dan koheren. Kita akan membedah mekanisme penggabungan kedua domain tersebut serta metrik *Panoptic Quality*-nya pada diskusi selanjutnya.

---

## Slide 009 - Panoptic Segmentation

### Narasi

Setelah membahas instance segmentation pada slide sebelumnya yang berfokus pada prediksi mask terpisah untuk setiap individu objek, kita kini beralih ke paradigma yang lebih komprehensif: panoptic segmentation. Paradigma ini dirancang untuk menyelesaikan keterbatasan pendekatan semantik maupun instance secara terpisah, dengan cara mengintegrasikan keduanya ke dalam satu representasi terpadu.

Kunci utama dari panoptic segmentation terletak pada pembedaan eksplisit antara dua kategori visual: *stuff* dan *things*. 
- **Stuff**: Area latar atau tekstur yang tidak memiliki batas individu yang jelas, seperti langit, jalan aspal, atau dinding bangunan.
- **Things**: Objek diskrit yang dapat dihitung, memiliki identitas unik, dan sering kali memerlukan pemisahan antar-instance, contohnya mobil, pejalan kaki, atau hewan.

Dengan memisahkan kedua konsep ini, model tidak hanya memahami konteks scene secara global, tetapi juga melacak entitas spesifik di dalamnya tanpa menimbulkan tumpang tindih makna atau kehilangan informasi latar belakang.

Secara teknis, keluaran dari panoptic segmentation bersifat hierarkis pada level piksel. Setiap piksel dalam citra wajib mendapatkan label semantik yang sesuai dengan kategorinya. Namun, untuk piksel yang termasuk dalam kategori *things*, sistem menambahkan identifikasi unik berupa instance ID. Hal ini memungkinkan representasi yang sangat detail dan konsisten, sekaligus menjaga struktur data tetap terorganisir untuk tahap downstream processing atau analisis kuantitatif.

Untuk mengevaluasi performa model pada tugas ini, metrik standar yang digunakan adalah Panoptic Quality (PQ). Berbeda dengan IoU murni atau Average Precision biasa, PQ merupakan metrik gabungan yang menilai dua aspek sekaligus:
- **Recognition Quality**: Ketepatan klasifikasi semantik dan penghitungan jumlah instance yang benar.
- **Segmentation Quality**: Tingkat overlap geometris antara mask prediksi dan ground truth.

PQ menghitung kesesuaian secara holistik dengan mempertimbangkan false positives, false negatives, serta skor IoU per pasangan, sehingga memberikan gambaran akurat tentang kemampuan model dalam memahami scene secara utuh tanpa bias terhadap salah satu kategori.

Sebagai ilustrasi konkret, bayangkan sebuah citra jalanan yang diproses oleh model panoptic. Sistem akan memberikan label semantik seperti `[langit]`, `[bangunan]`, dan `[jalan]` yang semuanya dikategorikan sebagai *stuff*. Sementara itu, mobil pertama dan mobil kedua akan diberi label semantik `"mobil"` namun dibedakan melalui instance ID menjadi `[mobil-1]` dan `[mobil-2]`, yang masuk dalam kategori *things*. Struktur penamaan ini mencerminkan bagaimana data biasanya disusun dalam format JSON atau dictionary pada implementasi praktisnya, serta memudahkan pipeline evaluasi otomatis.

Memahami fondasi panoptic segmentation ini penting karena paradigmanya menjadi landasan bagi banyak arsitektur modern yang menggabungkan pemahaman konteks dan deteksi objek. Pada slide berikutnya, kita akan membandingkan ketiga paradigma segmentasi—semantic, instance, dan panoptic—secara sistematis melalui tabel perbandingan, serta mendiskusikan implikasi pemilihan masing-masing pendekatan terhadap desain penelitian dan pemanfaatan foundation model seperti SAM dalam konteks akademik tingkat doktor.

---

## Slide 010 - Perbandingan Tiga Paradigma Segmentasi

### Narasi

Setelah mendefinisikan konsep panoptic segmentation serta pembedaan antara kategori *stuff* dan *things* pada slide sebelumnya, kini kita perlu menempatkan ketiga paradigma segmentasi dalam perspektif yang lebih luas untuk menentukan arah metodologi penelitian.

Perbedaan mendasar antara semantic, instance, dan panoptic segmentation dapat dirangkum sebagai berikut:
- **Semantic segmentation** melakukan prediksi per piksel tanpa membedakan identitas objek yang berulang, sehingga ideal untuk pemetaan wilayah homogen.
- **Instance segmentation** berfokus pada objek individu (*things*), memberikan identifikasi unik per entitas meskipun memiliki kelas semantik yang sama.
- **Panoptic segmentation** menyatukan kedua pendekatan tersebut, menghasilkan representasi lengkap di mana setiap piksel mendapatkan label semantik sekaligus ID instance jika termasuk kategori objek.

Implikasi pemilihan paradigma ini sangat bergantung pada karakter data dan pertanyaan penelitian yang diajukan. Dalam domain segmentasi medis, pendekatan semantik atau instance umumnya lebih dominan karena fokusnya pada struktur anatomi atau patologi spesifik tanpa memerlukan pemahaman konteks ruang yang kompleks. Sebaliknya, tugas *scene understanding* untuk kendaraan otonom atau robotika navigasi cenderung mengadopsi panoptic segmentation guna menangkap hubungan spasial antar elemen lingkungan secara holistik.

Perkembangan *foundation model* seperti Segment Anything Model (SAM) telah mengubah cara kita mendekati evaluasi dan implementasi ketiga paradigma ini. Meskipun SAM dirancang khusus untuk menghasilkan mask objek berbasis prompt, fleksibilitas arsitekturnya memungkinkan adaptasi ke semua jenis tugas segmentasi. Melalui mekanisme *prompt engineering* dan penyesuaian *training head*, SAM dapat berfungsi sebagai baseline serbaguna yang mendukung eksperimen baik pada skenario semantik, instance, maupun panoptic, sesuai dengan kebutuhan novelitas penelitian.

Pemahaman terhadap kerangka kerja dan implikasi metodologis ini menjadi fondasi kritis sebelum memasuki tahap validasi kuantitatif. Pada slide berikutnya, kita akan mengkaji metrik evaluasi standar yang paling sering digunakan dalam literatur terkini, khususnya *Intersection over Union* (IoU), beserta karakteristik sensitivitas dan variasinya dalam konteks penelitian tingkat lanjut.

---

## Slide 011 - Metrik Evaluasi: IoU

### Narasi

Setelah menguraikan perbedaan mendasar antara paradigm semantic, instance, dan panoptic pada slide sebelumnya, langkah kritis berikutnya dalam pipeline evaluasi adalah memilih metrik yang tepat untuk mengukur akurasi prediksi mask. Intersection over Union atau IoU telah menjadi standar de facto dalam literatur computer vision karena sifatnya yang intuitif dan mudah diinterpretasikan secara geometris.

Secara formal, IoU menghitung proporsi area tumpang tindih antara mask prediksi dan mask ground truth relatif terhadap total area gabungan keduanya. Rumus dasarnya dapat dituliskan sebagai:

```text
IoU = |A ∩ B| / |A ∪ B|
```

Di sini, himpunan A merepresentasikan piksel yang diprediksi oleh model, sementara himpunan B adalah anotasi referensi yang sebenarnya. Hasil perhitungan selalu ternormalisasi dalam rentang `[0, 1]`, di mana nilai satu menandakan kesesuaian sempurna dan nol menunjukkan ketiadaan overlap.

Sebagai peneliti pada tingkat doktoral, pemahaman mendalam mengenai karakteristik teknis IoU menjadi prasyarat sebelum menerapkannya dalam eksperimen:
- Sensitivitas tinggi terhadap deviasi batas objek: pergeseran hanya beberapa piksel di edge mask dapat menurunkan skor secara drastis, sehingga IoU kurang toleran terhadap noise anotasi atau variasi resolusi input.
- Tidak memisahkan false positive dan false negative: karena bersifat ratio agregat, dua skenario error yang berbeda secara topologi dapat menghasilkan nilai IoU identik, yang memerlukan validasi visual atau metrik tambahan saat melakukan ablation study.
- Kompatibilitas luas: tetap menjadi benchmark utama baik untuk semantic segmentation, instance segmentation, maupun evaluasi awal foundation model seperti SAM sebelum beralih ke metrik berbasis ranking atau threshold-independence.

Dalam implementasi praktis, IoU hampir never digunakan dalam bentuk scalar tunggal. Untuk tugas multi-kelas, kita akan mengagregasikannya menjadi mIoU (mean IoU) dengan menghitung IoU per kelas terlebih dahulu, kemudian meratakannya. Sebaliknya, pada konteks instance segmentation atau evaluasi promptable model, kita menghitung IoU per-instance agar performa terhadap objek minor tidak tenggelam oleh dominasi objek besar.

Meskipun robust dan widely adopted, IoU memiliki kelemahan inheren terkait ketidakseimbangan kelas ekstrem, terutama pada domain biomedis di mana foreground mendominasi background atau sebaliknya. Keterbatasan ini membuka ruang metodologis untuk mempertimbangkan alternatif yang lebih sensitif terhadap overlap absolut, yaitu koefisien kemiripan Dice. Pada slide berikutnya, kita akan membedah rumus Dice, contoh numerik perbandingannya dengan IoU, serta hubungan aljabar langsung `Dice = 2IoU / (1 + IoU)` yang sering dimanfaatkan dalam perancangan loss function dan strategi optimasi model generasi maupun segmentasi presisi tinggi.

---

## Slide 012 - Metrik Evaluasi: Dice

### Narasi

Setelah membahas Intersection over Union pada slide sebelumnya, kini kita beralih ke metrik evaluasi alternatif yang sangat dominan dalam literatur segmentasi, yaitu Dice Similarity Coefficient atau sering disingkat sebagai Dice Score. Jika IoU mengukur rasio tumpang tindih terhadap union himpunan piksel, Dice memberikan perspektif yang sedikit berbeda dengan membandingkan dua kali irisan terhadap total jumlah piksel pada prediksi dan ground truth. Rumus dasarnya dapat dituliskan sebagai `Dice = 2|A ∩ B| / (|A| + |B|)`.

Untuk memahami perbedaan sensitivitas antara kedua metrik ini, mari kita telaah ilustrasi numerik pada slide. Dengan asumsi himpunan prediksi A berukuran 10 piksel, ground truth B juga 10 piksel, dan irisan keduanya sebesar 6 piksel, perhitungan IoU menghasilkan nilai sekitar 0,429. Namun, ketika menggunakan rumus Dice, nilai yang diperoleh adalah 0,600. Perbedaan angka ini bukan merupakan kesalahan kalkulasi, melainkan mencerminkan sifat matematis Dice yang cenderung menghasilkan nilai lebih tinggi untuk tingkat overlap yang sama dibandingkan IoU.

Karakteristik utama Dice terletak pada sifatnya yang lebih "generous" dalam menilai performa model. Hal ini menjadikan Dice sangat populer di bidang segmentasi biomedis, di mana masalah ketidakseimbangan kelas (class imbalance) sangat krusial karena objek target seperti lesi atau organ seringkali menempati proporsi piksel yang sangat kecil dibandingkan latar belakang. Secara matematis, hubungan antara kedua metrik ini bersifat deterministik dan dapat dikonversi satu sama lain melalui persamaan `Dice = 2IoU / (1 + IoU)`.

Meskipun secara teoritis keduanya saling terkait erat, penggunaan Dice versus IoU sering kali bergantung pada konvensi domain penelitian dan bagaimana peneliti menafsirkan penalization terhadap false positive maupun false negative. Pada konteks penelitian doktoral, pertanyaan kritisnya bukan sekadar menghitung skor, melainkan memahami apa sebenarnya yang diukur oleh metrik tersebut terhadap struktur spasial hasil segmentasi. Pembahasan ini akan membawa kita secara natural ke slide berikutnya, di mana kita akan mengupas batas-batas fundamental dari IoU dan Dice, khususnya terkait kualitas kontur mask, konsistensi anotasi manusia, serta apakah skor overlap piksel saja sudah cukup untuk menilai kemajuan model pada domain ilmiah yang menuntut presisi tinggi.

---

## Slide 013 - Mask Quality dan Kualitas Anotasi

### Narasi

Pada slide sebelumnya kita telah membahas koefisien Dice sebagai metrik evaluasi yang banyak diadopsi, khususnya dalam segmentasi biomedis, karena sifatnya yang lebih toleran terhadap ketidakseimbangan kelas dibandingkan IoU. Namun, penting untuk menekankan bahwa metrik berbasis tumpukan piksel ini memiliki batasan konseptual yang signifikan ketika diaplikasikan pada penilaian kualitas mask secara menyeluruh.

IoU dan Dice murni mengukur derajat tumpang tindih himpunan piksel antara prediksi model dan ground truth. Keduanya tidak menangkap karakteristik geometris seperti kehalusan kontur, kelengkapan topologi, atau akurasi batas objek. Dua mask dapat menghasilkan nilai IoU identik, namun satu mungkin memiliki tepi yang halus dan sesuai anatomi, sedangkan yang lain bergerigi, terfragmentasi, atau mengalami over-segmentasi. Dengan demikian, skor numerik tinggi tidak otomatis menjamin representasi spasial yang bermakna secara klinis atau ilmiah.

Dalam ekosistem penelitian dan aplikasi nyata, kualitas mask kerap terdegradasi oleh berbagai sumber kesalahan yang bersifat sistematis. *Boundary error* muncul ketika model gagal mendelineasi tepian objek dengan presisi tinggi, terutama pada region dengan kontras rendah, noise tinggi, atau tekstur repetitif. Selain itu, anotasi manusia sendiri jarang bersifat absolut. Variabilitas antar-anotator, inkonsistensi protokol labeling, serta label yang tidak lengkap pada struktur mikro atau tepi samar menciptakan *ground truth* yang mengandung noise inheren. Noise ini kemudian terserap selama pelatihan dan berpotensi menstabilkan evaluasi pada standar yang sebenarnya suboptimal.

Kondisi ini memicu serangkaian pertanyaan kritis yang sangat relevan untuk eksplorasi tingkat doktoral. Pertama, apakah peningkatan nilai IoU atau Dice secara linear mencerminkan pemahaman semantik objek yang lebih mendalam, atau sekadar penyesuaian statistik terhadap bias dataset? Kedua, bagaimana ketidaksempurnaan anotasi memengaruhi distorsi evaluasi, dan apakah metrik tradisional masih cukup robust untuk domain ilmiah yang menuntut akurasi geometris ketat? Ketiga, perlukah kita merumuskan kerangka evaluasi baru yang mengintegrasikan kesadaran batas (*boundary-aware metrics*), pemodelan ketidakpastian anotasi, atau alignment dengan persepsi ahli manusia?

Pertanyaan-pertanyaan inilah yang menjadi fondasi mengapa desain arsitektur segmentasi modern harus secara eksplisit mengakomodasi preservasi detail spasial. Sebagaimana akan kita telaah pada slide berikutnya, U-Net menjawab tantangan ini melalui mekanisme *skip connection* yang secara aktif mengalirkan fitur resolusi tinggi dari encoder ke decoder, sehingga degradasi tepi dapat diminimalkan sejak tahap konstruksi jaringan.

---

## Slide 014 - U-Net: Arsitektur Encoder-Decoder

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi bahwa metrik seperti IoU dan Dice hanya mengukur tumpang tindih piksel, bukan kehalusan kontur, serta bagaimana anotasi yang tidak konsisten atau noisy dapat menyesatkan evaluasi model. Dalam konteks ini, U-Net muncul sebagai arsitektur yang secara eksplisit dirancang untuk mengatasi ketidaksempurnaan data dan batasan jumlah sampel, khususnya pada domain segmentasi biomedis.

Arsitektur ini pertama kali diusulkan oleh Ronneberger dkk. dengan motivasi utama menangani masalah segmentasi sel dan jaringan biologis yang umumnya memiliki keterbatasan data pelatihan. Keunggulan fundamentalnya terletak pada kemampuan generalisasi yang kuat meskipun dataset sangat kecil, sebuah karakteristik kritis yang sering menjadi bottleneck dalam penelitian pengolahan citra digital tingkat lanjut.

Secara struktural, U-Net mengadopsi pola encoder-decoder yang simetris dan efisien. Jalur encoder berfungsi sebagai mekanisme kontraksi, di mana konvolusi dan pooling secara bertahap menurunkan resolusi spasial sambil memperkaya representasi semantik. Sebaliknya, decoder bertindak sebagai jalur ekspansi yang menggunakan up-convolution untuk mengembalikan dimensi spasial ke ukuran asli, sekaligus menghasilkan prediksi mask yang presisi.

```text
Encoder (kontraksi)          Decoder (ekspansi)
   [Conv + Pool]      [Skip]   [UpConv + Conv]
        |                |           |
   fitur resolusi       fitur      fitur
   rendah +             dari       resolusi
   semantik            encoder     tinggi +
                       detail      semantik
```

Ide kunci yang mendasari desain ini adalah pemisahan fungsi antara ekstraksi konteks dan pemulihan spasial. Encoder menangkap representasi global dan konteks semantik objek melalui downsampling berulang. Decoder, di sisi lain, bertugas merekonstruksi detail lokal dan batas objek dengan meningkatkan resolusi fitur. Kombinasi ini memungkinkan model memahami apa yang ada di dalam citra, sekaligus menentukan lokasi dan bentuknya secara akurat.

Mekanisme skip connection menjadi elemen penentu yang menghubungkan kedua jalur tersebut. Dengan mengalirkan fitur resolusi tinggi dari encoder ke decoder, arsitektur ini mempertahankan informasi tepi dan struktur halus yang biasanya terdegradasi selama proses downsampling. Pendekatan ini secara langsung mitigasi masalah boundary error yang sering gagal tertangkap oleh metrik overlap tradisional.

Meskipun awalnya dikembangkan sebagai CNN murni, prinsip encoder-decoder dengan skip connection tetap menjadi fondasi bagi banyak arsitektur segmentasi modern. Keterbatasan receptive field pada CNN konvensional akan mendorong evolusi menuju transformer-based dan foundation models yang mampu menangkap ketergantungan jarak jauh. Pembahasan mendalam mengenai bagaimana skip connection beroperasi dan mengapa ia krusial akan kita jabarkan pada slide berikutnya, sekaligus membuka perspektif kritis mengenai posisi arsitektur ini dalam peta perkembangan state-of-the-art computer vision.

---

## Slide 015 - U-Net: Skip Connection dan Peranannya

### Narasi

Pada slide sebelumnya, kita telah menguraikan arsitektur dasar U-Net yang terdiri dari jalur kontraksi sebagai encoder dan jalur ekspansi sebagai decoder. Desain simetris ini memang terbukti ampuh dalam memulihkan resolusi spasial untuk tugas segmentasi pixel-level. Namun, elemen kunci yang menjadikan U-Net unggul dibandingkan arsitektur encoder-decoder konvensional justru terletak pada mekanisme penghubung antara kedua jalur tersebut, yaitu skip connection.

Saat proses downsampling berlangsung di bagian encoder, informasi spasial mengenai lokasi piksel dan detail tepi objek cenderung terabstraksi atau hilang akibat penurunan dimensi tensor. Skip connection mengatasi hal ini dengan mengalirkan representasi fitur resolusi tinggi dari lapisan encoder secara langsung ke lapisan decoder yang setara. Melalui operasi concatenation, decoder dapat menggabungkan konteks semantik dari bottleneck dengan detail lokal yang masih terjaga, sehingga prediksi mask menjadi lebih presisi terutama pada batas objek.

Ilustrasi pada slide menunjukkan bagaimana fitur tepi halus dari encoder level dua digabung dengan fitur semantik dari decoder level dua sebelum diproses lebih lanjut. Penggabungan ini memungkinkan jaringan untuk melakukan refines lokal tanpa kehilangan jejak posisi awal piksel. Dalam praktik implementasi menggunakan PyTorch atau torchvision, mekanisme ini biasanya diimplementasikan dengan fungsi `torch.cat()` pada channel dimension, diikuti oleh layer konvolusi 3x3 untuk menyatukan kembali representasi sebelum tahap prediksi akhir.

Meskipun sangat efektif, arsitektur U-Net murni berbasis CNN memiliki batasan teoretis yang perlu disadari dalam konteks penelitian tingkat doktoral. Receptive field jaringan sangat bergantung pada kedalaman arsitektur dan jumlah operasi pooling, sehingga kemampuan model untuk menangkap ketergantungan jarak jauh (*long-range dependencies*) antar region citra tetap terbatas. Keterbatasan ini menjadi motivasi utama mengapa paradigma transformer mulai diadopsi dalam pipeline segmentasi modern. Pada slide berikutnya, kita akan membahas bagaimana integrasi self-attention dan arsitektur transformer mengatasi kelemahan receptive field CNN serta membuka jalan menuju framework segmentasi universal.

---

## Slide 016 - Dari CNN ke Transformer untuk Segmentasi

### Narasi

Beralih dari pembahasan sebelumnya mengenai arsitektur U-Net, kita telah melihat bagaimana skip connection berhasil mitigasi kehilangan informasi spasial selama proses downsampling. Namun, keterbatasan mendasar tetap ada pada backbone CNN itu sendiri, di mana receptive field-nya sangat bergantung pada kedalaman jaringan dan kesulitan menangkap ketergantungan jarak jauh antar piksel yang tidak bersebelahan. Pergeseran paradigma inilah yang membawa kita pada integrasi mekanisme Transformer ke dalam pipeline segmentasi citra.

Untuk memahami lompatan metodologis ini, mari kita tinjau kembali perbedaan fundamental antara CNN dan Vision Transformer. CNN beroperasi dengan filter lokal yang memiliki inductive bias spasial kuat, sehingga sangat efisien dalam mengekstrak pola tepi dan tekstur lokal. Sebaliknya, Vision Transformer memecah citra menjadi patch embedding dan mengandalkan mekanisme self-attention untuk memodelkan hubungan global secara langsung. Pada tingkat doktoral, penting untuk menyadari bahwa penghilangan inductive bias spasial ini bukan kelemahan, melainkan trade-off yang memungkinkan model belajar representasi kontekstual yang lebih adaptif terhadap variasi objek yang kompleks.

Motivasi utama penggunaan Transformer dalam tugas segmentasi terletak pada kebutuhan akan pemahaman konteks global dan hubungan semantik antar region yang terpisah jauh. Self-attention mampu menghubungkan piksel yang berjauhan tanpa melalui propagasi bertahap seperti pada konvolusi, sehingga prediksi mask dapat mempertahankan koherensi struktural meskipun objek tersebar atau memiliki bentuk irregular. Hal ini menjadi krusial terutama dalam skenario medical imaging atau remote sensing, di mana konteks lingkungan sering kali menentukan akurasi segmentasi.

Berbagai pendekatan terkini telah mengadopsi prinsip ini dengan cara yang berbeda-beda. TransUNet, misalnya, menggabungkan kekuatan U-Net dengan encoder berbasis ViT untuk menjaga hierarki resolusi sambil memanfaatkan representasi global. Swin Transformer memperkenalkan mekanisme attention dalam window yang bergelembung, menghasilkan fitur hierarkis dengan biaya komputasi yang lebih terkendali. Sementara itu, framework seperti Mask2Former mereformulasi segmentasi sebagai masalah query-based prediction, memungkinkan satu arsitektur universal menangani semantic, instance, hingga panoptic segmentation secara bersamaan.

Meskipun arsitektur berbasis Transformer menawarkan fleksibilitas dan performa state-of-the-art, implementasinya memerlukan pertimbangan matang terkait efisiensi dan desain sistem. Pada slide berikutnya, kita akan membedah komponen umum dari arsitektur segmentasi modern, mulai dari backbone, decoder, pixel decoder, hingga mask head, serta mengevaluasi bagaimana interaksi antar modul tersebut menentukan keseimbangan antara akurasi, skalabilitas data, dan beban komputasi.

---

## Slide 017 - Arsitektur Transformer Segmentation Modern

### Narasi

Slide ini menguraikan arsitektur umum yang menjadi fondasi model segmentasi berbasis transformer terkini. Jika pada slide sebelumnya kita membahas transisi dari CNN ke transformer serta motivasi penggunaan self-attention untuk menangkap konteks global, maka di sini kita akan membedah bagaimana komponen-komponen tersebut dirakit secara sistematis dalam pipeline modern.

Alur pemrosesan citra pada arsitektur modern dapat dilihat pada skema berikut:
```text
Citra
  |
  v
Backbone (ViT/Swin/CNN)
  |
  v
Transformer Decoder / Pixel Decoder
  |
  v
Mask Prediction
```
Setiap blok memiliki peran spesifik yang saling melengkapi. Backbone berfungsi sebagai ekstraktor representasi visual awal. Pada implementasi terkini, backbone tidak lagi terbatas pada CNN konvensional, melainkan banyak mengadopsi Vision Transformer atau varian hierarkis seperti Swin Transformer untuk menangkap multi-skala fitur secara efisien.

Setelah fitur diekstrak, tahap selanjutnya melibatkan decoder. Di sinilah transformer decoder atau pixel decoder bekerja. Transformer decoder bertugas menghubungkan query objek dengan fitur spasial yang telah diproses oleh backbone, memungkinkan model memahami hubungan semantik antar wilayah citra. Sementara itu, pixel decoder berperan dalam memetakan kembali fitur berdimensi tinggi ke resolusi asli input, sehingga menghasilkan peta fitur yang sesuai dengan ukuran piksel aslinya.

Output dari decoder kemudian diteruskan ke mask head atau prediction layer. Komponen ini bertanggung jawab menghasilkan mask biner per kelas atau per objek, tergantung pada formulasi masalah segmentasi yang dihadapi, apakah semantic, instance, atau panoptic.

Beberapa catatan kritis perlu diperhatikan dalam merancang atau mengevaluasi arsitektur semacam ini:
- Fleksibilitas framework berbasis transformer memungkinkan pendekatan universal yang dapat diadaptasi untuk berbagai jenis segmentasi tanpa mengubah struktur inti secara drastis.
- Kompleksitas komputasi memang lebih tinggi dibandingkan arsitektur CNN murni, terutama karena operasi self-attention yang bersifat kuadratik terhadap jumlah patch.
- Performa akhir sangat bergantung pada strategi pretraining dan skala dataset. Penggunaan model yang telah dilatih secara self-supervised seperti DINOv2 atau pretraining masif pada kumpulan data besar sering kali menjadi penentu utama keberhasilan transfer learning ke tugas segmentasi spesifik.

Memahami struktur arsitektur ini menjadi prasyarat metodologis sebelum masuk ke aspek pelatihan dan evaluasi empiris. Pada slide berikutnya, kita akan membahas pipeline supervised segmentation secara detail, termasuk pemilihan loss function, metrik evaluasi standar, serta praktik terbaik dalam menerapkan transfer learning dan desain eksperimen yang rigor untuk level penelitian doktor.

---

## Slide 018 - Supervised Segmentation: Pipeline dan Transfer Learning

### Narasi

Slide ini menguraikan pipeline standar untuk segmentasi berbasis supervised serta strategi transfer learning yang menjadi landasan eksperimen tingkat doktoral. Mari kita telusuri alur pemrosesan data hingga tahap evaluasi.

Pipeline dimulai dari dataset yang telah dilengkapi anotasi mask per-pixel. Data tersebut dipasangkan dengan arsitektur yang terdiri dari pretrained backbone dan segmentation head. Selama pelatihan, optimasi difokuskan pada kombinasi loss function seperti Cross-entropy dengan Dice atau Focal loss untuk menangani class imbalance dan ketajaman batas objek. Setelah konvergensi, model dievaluasi menggunakan metrik mIoU, Dice coefficient, dan Panoptic Quality (PQ) untuk mengukur akurasi semantik sekaligus konsistensi topologi hasil segmentasi.

Dalam praktik transfer learning, backbone umumnya diinisialisasi dari bobot ImageNet atau model self-supervised seperti DINOv2 yang telah mempelajari representasi visual yang robust. Sebaliknya, segmentation head dilatih sepenuhnya dari awal agar mampu memetakan fitur kontekstual kembali ke resolusi spasial penuh sesuai kebutuhan prediksi mask. Pendekatan ini secara signifikan mempercepat konvergensi dan meningkatkan stabilitas numerik, terutama ketika data anotasi target relatif terbatas.

Untuk menjaga rigoritas penelitian, perhatikan praktik metodologis berikut:
- Hindari membandingkan metode dengan backbone berbeda tanpa kontrol ketat, karena variasi kapasitas representasi dapat mengaburkan kontribusi nyata dari inovasi yang diajukan.
- Lakukan ablation study secara sistematis untuk mengisolasi dampak masing-masing komponen, mulai dari desain arsitektur, pemilihan loss, hingga strategi augmentasi data.
- Selalu establish baseline terkuat sebelum mengklaim kebaruan, sehingga kontribusi ilmiah dapat diposisikan secara objektif terhadap state-of-the-art yang sudah mapan.

Penjelasan mengenai konfigurasi training dan transfer learning ini merupakan kelanjutan langsung dari slide 17 tentang arsitektur transformer segmentation. Jika slide sebelumnya mendefinisikan peran teknis setiap modul backbone, decoder, dan pixel head, slide ini menyoroti bagaimana modul tersebut dikonfigurasi dan dioptimalkan secara empiris. Namun, meskipun pendekatan supervised sangat efektif, ia tetap menghadapi batasan fundamental yang akan kita bahas pada slide berikutnya terkait keterbatasan segmentasi supervised.

---

## Slide 019 - Keterbatasan Segmentasi Supervised

### Narasi

Merujuk pada pembahasan pipeline segmentasi terawasi dan strategi transfer learning pada slide sebelumnya, kita kini perlu mengevaluasi celah metodologis yang masih menghambat pendekatan konvensional tersebut. Meskipun penggunaan backbone pretrained seperti DINOv2 atau ImageNet telah meningkatkan representasi fitur secara signifikan, arsitektur segmentasi berbasis label tetap menghadapi tiga batasan fundamental yang menjadi fokus kajian kritis pada jenjang doktoral.

- **Biaya Anotasi Mask Per-Pixel:** Pembuatan ground truth pixel-wise jauh lebih intensif secara sumber daya dibandingkan anotasi bounding box. Pada dataset skala besar seperti COCO atau Cityscapes, proses tracing dan validasi mask membutuhkan waktu berbulan-bulan, menciptakan bottleneck khusus untuk domain niche atau kelas dengan frekuensi rendah.
- **Keterbatasan Closed-Set:** Model terlatih hanya beroperasi pada ruang kategori yang telah didefinisikan selama fase training. Munculnya kelas baru di lingkungan inferensi mengharuskan pengumpulan data tambahan dan siklus fine-tuning ulang, sementara metrik evaluasi seperti mIoU atau Panoptic Quality (PQ) secara inheren mengukur performa hanya pada distribusi kelas yang sudah ada.
- **Rendahnya Generalisasi Domain:** Arsitektur supervised cenderung overfit pada statistik visual data asal, sehingga performa menurun tajam saat terjadi pergeseran domain, misalnya transisi dari citra jalan raya ke citra satelit atau mikroskopis. Kakuannya dalam merespons variasi kontekstual menjadi pendorong utama evolusi menuju arsitektur yang lebih adaptif.

Ketiganya secara kolektif menunjukkan bahwa pendekatan statis berbasis kelas tetap tidak lagi memadai untuk menangani kompleksitas data vision modern. Keterbatasan inilah yang menjadi landasan konseptual bagi pengembangan promptable foundation model. Pada slide berikutnya, kita akan menguraikan mekanisme dasar segmentasi interaktif, di mana model menerima masukan fleksibel berupa titik, kotak, atau mask awal, sekaligus membuka ruang eksplorasi penelitian mengenai robustness prompt, disambiguasi semantik, dan integrasinya ke dalam workflow anotasi cerdas.

---

## Slide 020 - Promptable Segmentation: Gagasan Dasar

### Narasi

Sebagai respons terhadap keterbatasan segmentasi berbasis *supervised* yang telah dibahas sebelumnya, khususnya terkait biaya anotasi piksel-per-piksel dan ketidakmampuan model menangani kelas baru atau pergeseran domain, kita memasuki paradigma yang lebih adaptif: *promptable segmentation*. Pendekatan ini mengubah struktur input model dengan menambahkan lapisan interaktivitas. Alih-alih hanya menerima citra mentah, model kini memproses pasangan input berupa citra beserta *prompt* yang diberikan pengguna. Prompt tersebut dapat berupa titik (*point*), kotak pembatas (*bounding box*), mask awal, atau kombinasi geometris lainnya, yang secara dinamis mengarahkan model untuk mengekstrak region of interest yang relevan.

Perbedaan fundamental antara segmentasi konvensional dan *promptable* terletak pada fleksibilitas kelas, mekanisme interaksi, dan orientasi tujuannya. Model *supervised* diprogram secara eksplisit untuk memetakan setiap piksel ke salah satu dari set kelas tertutup yang telah ditentukan selama pelatihan. Sebaliknya, model *promptable* tidak bergantung pada label kategorikal statis. Ia dirancang untuk menafsirkan maksud pengguna secara kontekstual, memungkinkan sesi segmentasi yang iteratif dan dapat disesuaikan secara real-time. Tujuan utamanya bergeser dari klasifikasi otomatis menuju pemenuhan kebutuhan visual spesifik pengguna.

Dari sudut pandang riset tingkat doktoral, arsitektur dan perilaku model *promptable* menawarkan celah penelitian yang signifikan. Beberapa pertanyaan kritis yang dapat dikembangkan menjadi proposal disertasi meliputi: bagaimana resolusi dan presisi prompt memengaruhi stabilitas gradien serta konsistensi prediksi mask? Bagaimana mekanisme *attention* atau *cross-modal alignment* dalam model mengolah prompt yang ambigu atau bersifat relatif? Selain itu, bagaimana integrasi model ini ke dalam alur kerja anotasi semi-otomatis dapat mengurangi bias manusia sekaligus meningkatkan skalabilitas pembuatan dataset untuk domain khusus seperti medis atau satelit? Jawaban atas pertanyaan-pertanyaan ini memerlukan desain eksperimen yang ketat, analisis ablation, dan evaluasi metrik yang melampaui IoU standar.

Paradigma *promptable* bukan sekadar peningkatan antarmuka, melainkan fondasi arsitektural bagi generasi terbaru *foundation models* dalam visi komputer. Dengan kemampuan generalisasi lintas domain dan dukungan inferensi *zero-shot*, pendekatan ini membuka jalan bagi sistem segmentasi yang benar-benar universal. Implementasi paling matang dan telah mendefinisikan ulang standar industri maupun akademis adalah Segment Anything Model (SAM), yang akan kita telaah secara komprehensif pada slide berikutnya.

---

## Slide 021 - Foundation Model untuk Segmentasi: SAM

### Narasi

Pada slide sebelumnya, kita telah mendefinisikan konsep segmentasi yang dipandu oleh *prompt*, serta membedakannya secara fundamental dengan pendekatan segmentasi tradisional berbasis *supervised learning*. Kita juga telah mengidentifikasi beberapa celah penelitian strategis, mulai dari pengaruh kualitas prompt terhadap akurasi mask, mekanisme interpretasi model terhadap prompt yang ambigu, hingga strategi integrasi ke dalam pipeline anotasi semi-otomatis. Kini, kita akan menyoroti implementasi paling matang dari paradigma tersebut, yaitu *Segment Anything Model* atau SAM.

SAM diperkenalkan oleh Meta AI Research pada tahun 2023 dengan ambisi menjadi *foundation model* universal untuk tugas segmentasi gambar. Inti desainnya terletak pada dukungan multi-modalitas prompt, yang memungkinkan pengguna memberikan masukan berupa titik koordinat, kotak pembatas (*bounding box*), atau mask awal sebagai panduan spasial. Melalui mekanisme ini, SAM mampu menghasilkan solusi *zero-shot* untuk objek yang sama sekali tidak hadir dalam data pelatihan, sehingga彻底 menghilangkan ketergantungan pada daftar kelas tertutup yang menjadi ciri khas model segmentasi konvensional. Fleksibilitas ini menjadikan SAM bukan hanya sebagai alat segmentasi, tetapi sebagai blok bangunan (*building block*) yang dapat disematkan ke dalam arsitektur sistem computer vision yang lebih luas.

Logika operasional SAM dapat direpresentasikan secara ringkas sebagai berikut:
```text
Citra + prompt  ->  SAM  ->  Mask yang sesuai
```
Alur ini menegaskan bahwa keluaran segmentasi sepenuhnya dikondisikan oleh interaksi dinamis antara representasi fitur visual citra dan instruksi eksplisit dari pengguna. Karena tidak terikat pada ruang label diskrit, model dapat beradaptasi secara instan terhadap domain atau objek baru tanpa memerlukan fine-tuning ulang. Karakteristik *class-agnostic* ini secara signifikan mengubah cara kita merancang eksperimen evaluasi, karena metrik keberhasilan tidak lagi diukur berdasarkan akurasi per-kelas, melainkan konsistensi spasial dan kesesuaian semantik terhadap intent pengguna.

Dari perspektif penelitian tingkat doktoral, sifat adaptif SAM membuka jalur eksplorasi yang kaya. Misalnya, bagaimana distribusi *attention map* pada layer akhir encoder merespons variasi kepadatan prompt? Bagaimana stabilitas inferensi dapat dioptimalkan ketika menghadapi prompt yang mengandung noise geometris atau redundansi informasi? Jawaban atas pertanyaan-pertanyaan ini akan membutuhkan analisis mendalam terhadap mekanisme internal model. Pembahasan teknis ini akan kita lanjutkan pada slide berikutnya, di mana kita akan mengurai arsitektur tripartit SAM—mulai dari ekstraksi embedding citra, pengodean prompt, hingga dekoder mask—serta mengevaluasi keunggulan desainnya dalam mendukung interaksi real-time.

---

## Slide 022 - Arsitektur SAM: Ringkasan

### Narasi

Slide ini menyajikan ringkasan arsitektur tiga komponen utama SAM yang membentuk alur pemrosesan dari citra input hingga keluaran mask beserta skor kepercayaan. Diagram pada slide menggambarkan bagaimana data mengalir secara paralel sebelum digabungkan dalam tahap dekoding. Alur ini bukan sekadar pembagian modul, melainkan strategi desain yang sangat disengaja untuk mencapai efisiensi komputasi dan fleksibilitas promptable segmentation.

Komponen pertama adalah Image Encoder yang memproses citra masuk dan menghasilkan image embedding atau peta fitur berdimensi tinggi. Karena proses encoding ini cukup berat secara komputasi, SAM menerapkan mekanisme caching di mana embedding hanya dihitung satu kali per citra, terlepas dari berapa banyak prompt yang akan diberikan. Strategi ini menjadi kunci mengapa SAM mampu menangani interaksi multi-prompt tanpa perlu melakukan inferensi ulang pada bagian encoder.

Komponen kedua adalah Prompt Encoder yang bertugas mengonversi berbagai jenis masukan pengguna—seperti titik koordinat, bounding box, atau mask kasar—menjadi representasi vektor yang seragam. Prompt encoder ini dirancang ringan dan cepat, sehingga memungkinkan respons waktu nyata saat pengguna berinteraksi dengan antarmuka segmentasi.

Kedua representasi tersebut kemudian dilewatkan ke komponen ketiga, yaitu Mask Decoder. Decoder ini berfungsi sebagai jembatan yang menggabungkan konteks visual dari image embedding dengan instruksi spasial dari prompt encoder. Hasil akhirnya bukan hanya mask biner, tetapi juga prediksi skor IoU (Intersection over Union) yang memberikan estimasi kualitas segmentasi. Skor ini penting dalam konteks penelitian karena dapat digunakan sebagai confidence measure atau diintegrasikan ke dalam pipeline post-processing otomatis.

Keunggulan arsitektur ini terletak pada pemisahan beban komputasi antara ekstraksi fitur global dan pemrosesan prompt lokal. Dengan mendesain mask decoder yang relatif ringan dibandingkan encoder, SAM mempertahankan kecepatan inference yang tinggi sambil tetap menjaga akurasi zero-shot segmentation. Desain modular seperti ini juga memudahkan integrasi SAM sebagai backbone atau plug-in module dalam sistem computer vision yang lebih kompleks, yang selaras dengan visi foundation model untuk tugas segmentasi umum.

Pada slide berikutnya, kita akan menelusuri lebih dalam mengenai spesifikasi teknis Image Encoder, termasuk adaptasi Vision Transformer untuk resolusi tinggi dan pemanfaatan pretrained representation melalui pendekatan MAE yang berkaitan erat dengan konsep self-supervised learning yang telah kita diskusikan sebelumnya.

---

## Slide 023 - Image Encoder SAM

### Narasi

Pada slide ini, kita menguraikan komponen pertama dari arsitektur Segment Anything Model, yaitu Image Encoder. Komponen ini bertanggung jawab atas ekstraksi representasi visual yang menjadi fondasi bagi seluruh proses segmentasi downstream.

Secara spesifikasi teknis, Image Encoder pada SAM mengimplementasikan arsitektur Vision Transformer atau ViT. Model dasar ViT telah mengalami modifikasi struktural agar mampu menampung resolusi input tinggi, misalnya 1024x1024 piksel, tanpa mengalami degradasi informasi spasial. Selain itu, proses pretraining encoder ini mengadopsi paradigma Masked Autoencoder atau MAE. Sebagaimana telah dikaji pada Pertemuan 04 mengenai self-supervised learning, MAE melatih model untuk merekonstruksi patch citra yang disembunyikan secara acak. Pendekatan ini memungkinkan SAM mempelajari representasi visual yang sangat robust dan umum, tanpa bergantung pada anotasi segmentasi manual yang mahal dan terbatas.

Peran encoder ini dalam alur pemrosesan dapat diringkas sebagai berikut:
```text
Citra input -> Image Encoder -> image embedding (large feature map)
```
Proses ini menghasilkan large feature map yang mempertahankan informasi geometri dan semantik tingkat rendah hingga tinggi. Embedding yang dihasilkan bersifat stateless terhadap prompt, sehingga dapat digunakan kembali berulang kali untuk berbagai skenario interaksi.

Karena operasi transformer pada resolusi setinggi itu memiliki beban komputasi yang signifikan, strategi implementasi standar menempatkan image encoding sebagai langkah statis yang dieksekusi lebih dulu. Setelah embedding terbentuk, sistem dapat menerima berbagai kombinasi prompt dari pengguna tanpa perlu melakukan forward pass pada encoder lagi. Efisiensi ini menjadi kunci mengapa SAM mampu berinteraksi secara cepat meskipun menggunakan backbone yang kompleks.

Pembahasan mengenai Image Encoder ini melengkapi gambaran arsitektur SAM yang telah kita tinjau pada slide sebelumnya, khususnya terkait pembagian tugas antara ekstraksi fitur dan dekoding. Jika encoder menjawab pertanyaan tentang bagaimana model memahami konten visual secara mandiri, maka komponen selanjutnya akan fokus pada bagaimana user memberikan instruksi spesifik. Hal ini akan kita bedah lebih lanjut pada slide berikutnya mengenai Prompt Encoder, termasuk mekanisme koding untuk point, box, dan mask, serta implikasinya terhadap paradigma promptable AI.

---

## Slide 024 - Prompt Encoder SAM

### Narasi

Setelah image encoder pada slide sebelumnya memproses input visual menjadi large feature map yang kaya representasi, sistem memerlukan mekanisme untuk menerjemahkan interaksi pengguna menjadi representasi numerik yang kompatibel. Prompt encoder SAM berperan tepat pada celah ini, mengubah anotasi spasial atau preferensi manual menjadi embedding yang siap digabung dengan image embedding.

Model ini mendukung tiga kategori utama prompt, masing-masing dengan format representasi yang disesuaikan:
- **Point**: Direpresentasikan sebagai sparse embedding yang menyertakan koordinat piksel serta labelnya, baik positif maupun negatif.
- **Box**: Menggunakan sparse embedding untuk mengkodekan posisi batas kiri-atas dan kanan-bawah dari region yang dituju.
- **Mask**: Diubah menjadi dense embedding yang langsung disuntikkan ke dalam decoder sebagai sinyal tambahan untuk memandu proses refinemen segmentasi.

Secara komputasional, prompt encoder dirancang sangat ringan dibandingkan image encoder. Ringannya arsitektur ini memungkinkan penggabungan beberapa prompt secara simultan tanpa membongkar bottleneck latency. Setiap prompt dikonversi menjadi vektor atau peta fitur yang selaras dengan dimensi ruang embedding, sehingga memastikan konsistensi representasi sebelum masuk ke tahap fusi.

Dari sudut pandang riset tingkat doktoral, desain ini membawa implikasi metodologis yang signifikan. Berbeda dengan pendekatan vision-language seperti CLIP yang mengikat objek pada semantik teks, SAM sepenuhnya bersifat spatial-intent driven. Model tidak melakukan klasifikasi berbasis kelas, melainkan mengekstrak objek berdasarkan lokasi dan konfigurasi prompt yang diberikan. Konfigurasi prompt yang berbeda dapat menghasilkan mask yang bervariasi meskipun diterapkan pada himpunan piksel yang identik, sehingga membuka ruang eksplorasi untuk kontrol granularitas, segmentasi multi-skala, dan adaptasi terhadap domain-specific annotation schemes.

Setelah embedding prompt berhasil dihasilkan, pasangan image embedding dan prompt embedding akan diteruskan ke mask decoder. Di tahap berikutnya, mekanisme cross-attention dan transposed convolution akan menggabungkan kedua representasi tersebut untuk menghasilkan mask biner serta estimasi kualitas segmentasi secara bersamaan.

---

## Slide 025 - Mask Decoder SAM

### Narasi

Setelah pada slide sebelumnya kita membahas bagaimana berbagai jenis prompt—baik titik, kotak, maupun mask—dikonversi menjadi representasi vektor oleh Prompt Encoder, kini kita beralih ke komponen krusial berikutnya dalam arsitektur Segment Anything Model, yaitu Mask Decoder. Komponen ini berperan sebagai inti pemrosesan yang menerjemahkan gabungan informasi visual dari gambar dan instruksi spasial dari prompt menjadi output segmentasi yang konkret.

Secara arsitektural, Mask Decoder menerima dua aliran embedding utama: image embedding yang telah diekstrak oleh image encoder, serta prompt embedding dari encoder tadi. Kedua aliran ini digabungkan melalui mekanisme cross-attention, yang memungkinkan model secara dinamis memfokuskan perhatian pada region tertentu sesuai dengan lokasi atau bentuk yang ditunjuk pengguna. Setelah proses attention selesai, fitur yang telah dimodulasi dilewatkan melalui serangkaian lapisan transposed convolution untuk melakukan upsampling secara progresif hingga menghasilkan resolusi pixel penuh.

Keluaran dari decoder ini bersifat ganda dan sangat informatif bagi tahap evaluasi maupun interaksi lanjutan. Pertama, model menghasilkan mask biner yang merepresentasikan objek spesifik yang diminta. Kedua, decoder sekaligus memprediksi skor IoU (Intersection over Union) sebagai estimasi kepercayaan terhadap kualitas mask tersebut. Nilai IoU ini bukan sekadar metrik pasif, melainkan menjadi sinyal umpan balik yang aktif digunakan dalam pipeline inference.

Yang menarik dari desain Mask Decoder adalah kemampuannya untuk dijalankan secara iteratif. Mask yang dihasilkan pada langkah pertama dapat dikembalikan ke sistem sebagai prompt baru, sehingga decoder dapat dipanggil berulang kali untuk menyempurnakan batas objek atau memisahkan objek yang tumpang tindih. Pendekatan ini secara alami mendukung paradigma human-in-the-loop, di mana intervensi pengguna dapat disisipkan kapan saja untuk mengarahkan hasil segmentasi tanpa perlu fine-tuning ulang model.

Keterbukaan terhadap pemanggilan berulang ini juga membuka celah penelitian yang relevan dengan materi selanjutnya, khususnya terkait penanganan ambiguitas prompt. Ketika satu titik atau kotak merujuk pada beberapa interpretasi valid, decoder akan menghasilkan beberapa kandidat mask bersamaan, masing-masing dilengkapi dengan skor IoU-nya sendiri. Hal ini membawa kita langsung ke pembahasan mengenai strategi seleksi mask otomatis dan dampak ambiguitas terhadap evaluasi performa model.

---

## Slide 026 - Ambiguity dan Multi-Mask Output

### Narasi

Pada slide sebelumnya, kita telah membahas bahwa mask decoder dari Segment Anything Model dapat dipanggil secara berulang untuk mendukung interaksi manusia dalam loop. Namun, mekanisme ini langsung menghadapi tantangan fundamental ketika input yang diberikan bersifat ambigu. Dalam konteks segmentasi interaktif, satu titik koordinat saja tidak selalu cukup untuk mendefinisikan batas objek secara unik.

Sebagai contoh, jika kita menempatkan sebuah point prompt di tengah gambar sebatang pohon, sistem tidak memiliki konteks semantik yang cukup untuk membedakan apakah pengguna menginginkan seluruh batang pohon, tajuk daunnya, atau hanya satu cabang tertentu. Ambiguitas ini muncul karena representasi embedding citra yang dihasilkan image encoder bersifat global dan padat, sehingga kehilangan informasi granularitas spasial yang diperlukan untuk disambiguasi secara deterministik.

Untuk mengatasi hal tersebut, arsitektur SAM dirancang khusus untuk menghasilkan beberapa kandidat mask secara simultan. Mask decoder dilengkapi dengan head prediktif ganda yang tidak hanya mengeluarkan mask biner, tetapi juga memperkirakan skor Intersection over Union (IoU) untuk setiap kandidat. Skor ini merepresentasikan kepercayaan model terhadap kualitas pemisahan foreground dan background pada mask yang bersangkutan.

Ilustrasi pada slide menunjukkan bagaimana satu titik prompt dapat memicu keluaran berupa tiga mask berbeda. Mask A mungkin merepresentasikan keseluruhan objek, Mask B menangkap sub-bagian spesifik, sedangkan Mask C mencakup area lebih luas yang masih mengandung titik tersebut. Pengguna kemudian berperan sebagai arbiter untuk memilih mask yang paling sesuai dengan niat awal, yang menjadi inti dari filosofi promptable foundation model.

Dari perspektif penelitian tingkat doktoral, fenomena ini membuka dua pertanyaan metodologis yang krusial. Pertama, bagaimana merancang mekanisme seleksi otomatis yang mampu memprediksi mask optimal tanpa keterlibatan manusia, misalnya melalui thresholding dinamis pada skor IoU atau penggunaan model ranking berbasis reinforcement learning. Kedua, bagaimana ambiguitas intrinsik ini memengaruhi metrik evaluasi standar. Evaluasi IoU terhadap ground truth bisa menjadi bias jika referensi annotator sendiri tidak konsisten dengan pilihan model, sehingga memerlukan protokol evaluasi yang mempertimbangkan ketidakpastian anotasi.

Pemahaman tentang dinamika ambiguitas dan strategi penanganan multi-mask ini akan menjadi fondasi penting saat kita menelaah variasi jenis prompt pada slide berikutnya. Kita akan melihat bagaimana perubahan modalitas input—mulai dari titik, bounding box, hingga mask awal—secara signifikan mengubah sensitivitas decoder dan stabilitas keluaran, serta implikasinya terhadap desain eksperimen yang rigor.

---

## Slide 027 - Jenis Prompt pada SAM

### Narasi

Slide ini membahas keragaman jenis prompt yang dapat diberikan kepada Segment Anything Model (SAM) untuk mengarahkan proses segmentasi. Sebagai arsitektur *promptable*, kinerja SAM sangat bergantung pada bagaimana informasi spasial atau semantik diberikan oleh pengguna atau sistem otomatis. Fleksibilitas ini menjadi pembeda utama dibanding metode segmentasi tradisional yang kaku dan memerlukan fine-tuning ulang untuk setiap objek baru.

Berikut adalah klasifikasi tipe prompt beserta karakteristik sensitivitasnya:
- **Point Prompt**: Pemberian satu atau beberapa koordinat titik tepat pada objek. Memiliki sensitivitas posisi yang sangat tinggi; pergeseran pixel saja dapat menggeser boundary mask secara signifikan.
- **Box Prompt**: Kotak pembatas (*bounding box*) yang menyelimuti objek. Secara empiris lebih stabil daripada titik karena memberikan konteks wilayah yang lebih luas kepada image encoder.
- **Mask Prompt**: Mask awal yang berasal dari model segmentasi lain atau anotasi kasar. Kualitas output sangat bergantung pada akurasi dan kelengkapan mask referensi tersebut.
- **Kombinasi Prompt**: Penggabungan dua atau lebih tipe di atas. Umumnya meningkatkan robustness model karena memanfaatkan informasi lokal (titik), kontekstual (box), dan struktural (mask) secara simultan.

Perlu ditekankan bahwa SAM versi dasar tidak memproses prompt berbasis teks secara native. Integrasi bahasa alami ke dalam pipeline segmentasi memerlukan model *grounding* eksternal seperti Grounding DINO. Model ini berfungsi menerjemahkan deskripsi teks menjadi bounding box koordinat, yang kemudian disuplai sebagai prompt box ke SAM. Pendekatan ini telah menjadi standar dalam literatur *vision-language segmentation* dan foundation model adaptation.

Dari perspektif metodologi penelitian tingkat doktoral, desain eksperimen harus mengontrol variabel prompt secara ketat. Variasi posisi titik relatif terhadap pusat massa objek, rasio aspek box terhadap dimensi target, serta strategi fusi prompt perlu divariasikan secara sistematis. Penggunaan *random seed* yang konsisten dan protokol evaluasi yang terstandarisasi mutlak diperlukan agar perbandingan metrik IoU dan Dice bersifat adil, reproduktibel, dan bebas dari bias sampling. Hal ini juga merupakan respons langsung terhadap tantangan ambiguitas yang dibahas pada slide sebelumnya; pemilihan prompt yang presisi akan meminimalkan ketidakpastian dalam distribusi skor IoU multi-mask.

Alur interaksi berbasis prompt ini akan diimplementasikan secara teknis pada sesi praktikum berikutnya. Mahasiswa akan mengikuti siklus umpan balik iteratif antara operator dan model, di mana mask dapat dikoreksi melalui penambahan prompt tambahan hingga memenuhi kriteria penerimaan. Implementasi ini akan menjembatani konsep teoretis prompt engineering dengan evaluasi kuantitatif menggunakan IoU dan Dice terhadap data referensi, sekaligus menyiapkan fondasi untuk eksplorasi *human-in-the-loop segmentation* pada penelitian disertasi.

---

## Slide 028 - Workflow Promptable Segmentation

### Narasi

Setelah membahas variasi tipe prompt yang dapat diberikan kepada Segment Anything Model pada slide sebelumnya, kita kini masuk ke mekanisme operasionalnya. Workflow segmentasi berbasis prompt dirancang sebagai proses iteratif yang memungkinkan pengguna memandu model secara bertahap hingga menghasilkan region of interest yang presisi. Dalam konteks foundation model, pendekatan interaktif ini menjadi kunci utama karena mengatasi keterbatasan metode fully automatic yang cenderung kaku dan sulit beradaptasi dengan kompleksitas objek di luar distribusi pelatihan.

Alur kerja interaktif dimulai dengan pemrosesan awal citra melalui image encoder untuk menghasilkan image embedding. Embedding ini dihitung sekali saja dan disimpan, sehingga setiap kali pengguna memberikan prompt baru, model tidak perlu melakukan forward pass penuh kembali. Prompt yang diterima—berupa titik, kotak, atau mask kasar—langsung diproses oleh mask decoder bersama embedding tersebut untuk menghasilkan predik mask beserta skor IoU sebagai indikator kepercayaan. Jika hasil segmentasi belum memenuhi kriteria, pengguna dapat menambahkan prompt koreksi, dan siklus ini berulang hingga mask secara definitif diterima. Mekanisme umpan balik ini meniru alur anotasi manual, namun dipercepat secara drastis oleh efisiensi arsitektur transformer.

Representasi logika tersebut dapat dilihat dalam pseudocode sederhana berikut. Tahap pertama adalah komputasi `image_embedding = image_encoder(image)` yang bersifat statis. Selanjutnya, sebuah loop menunggu `user_input()` untuk mendapatkan prompt. Setiap iterasi memanggil `mask_decoder` dengan embedding dan prompt terkini, lalu mengembalikan mask beserta skornya. Loop hanya dihentikan ketika kondisi `user_accept(mask)` terpenuhi. Struktur ini menegaskan bahwa pemisahan antara encoding global dan decoding lokal adalah strategi desain yang menjaga responsivitas sistem meskipun terjadi beberapa kali iterasi revisi.

Dalam praktikum, mahasiswa akan menerapkan workflow ini pada subset data yang telah disiapkan. Evaluasi tidak hanya bergantung pada verifikasi visual, tetapi juga pada perhitungan metrik kuantitatif seperti Intersection over Union (IoU) dan Dice coefficient terhadap annotasi referensi. Penguasaan implementasi loop ini akan menjadi prasyarat kritis ketika kita membahas dataset SA-1B pada slide berikutnya, yang menyediakan miliaran mask untuk melatih dan mengukur robustness model sejenis. Pemahaman mendalam tentang dinamika interaksi ini akan membantu Anda merancang protokol eksperimen yang ketat serta mengidentifikasi potensi bias dalam pipeline segmentasi modern.

---

## Slide 029 - SA-1B: Dataset Segment Anything

### Narasi

Pada slide ini, kita membahas dataset SA-1B yang menjadi fondasi utama dalam pengembangan model Segment Anything. Dataset ini memiliki skala yang sangat masif, mencakup sekitar 11 juta citra dengan total lebih dari 1 miliar mask anotasi. Pembangunannya tidak dilakukan secara manual murni, melainkan melalui pipeline hibrida yang menggabungkan kemampuan model awal dengan koreksi manusia.

Proses pembuatan dataset ini dimulai dengan menjalankan model SAM versi awal pada sekumpulan citra untuk menghasilkan prediksi mask secara otomatis. Setelah itu, anotator manusia melakukan koreksi menggunakan mekanisme prompt interaktif yang telah kita bahas pada slide sebelumnya. Iterasi antara prediksi model dan revisi manusia inilah yang secara signifikan meningkatkan kualitas maupun kuantitas label yang dihasilkan.

Namun, sebagai peneliti tingkat doktoral, kita harus menyikapi dataset ini dengan perspektif kritis. Mask yang terdapat di SA-1B bukanlah ground truth yang sempurna. Sebagian besar mask merupakan hasil prediksi model yang telah disetujui atau dikoreksi oleh manusia, bukan anotasi piksel demi piksel yang diverifikasi ketat. Selain itu, distribusi objek dalam dataset ini tidak seimbang dan belum tentu mewakili seluruh domain aplikasi vision yang kompleks.

Implikasi risetnya cukup signifikan. Ketika Anda menggunakan SA-1B atau dataset turunannya untuk evaluasi model, perlu dilakukan validasi tambahan agar metrik yang diperoleh tidak bias terhadap karakteristik dataset tersebut. Mask yang dihasilkan oleh SAM pada data baru sebaiknya tidak langsung dianggap sebagai label emas tanpa proses verifikasi independen, terutama jika tujuan penelitian Anda adalah membangun benchmark yang rigor.

Penjelasan mengenai dinamika interaksi antara manusia dan model ini akan terus berlanjut pada slide berikutnya, di mana kita akan mendalami konsep Human-in-the-Loop untuk anotasi, termasuk keuntungan operasional serta tantangan teknis yang perlu diantisipasi dalam alur kerja penelitian modern.

---

## Slide 030 - Human-in-the-Loop untuk Anotasi

### Narasi

Pada slide sebelumnya kita telah membahas konstruksi dataset SA-1B yang mengandalkan kombinasi model dan anotasi manusia. Dari sinilah muncul paradigma baru dalam praktik anotasi skala besar, yaitu pendekatan *Human-in-the-Loop*. Konsep dasarnya menggeser beban kerja dari penggambaran mask piksel demi piksel menjadi pemberian *prompt* oleh manusia. Setelah model menghasilkan kandidat mask, anotator hanya perlu melakukan koreksi pada bagian yang tidak akurat. Interaksi iteratif ini secara fundamental mengubah efisiensi alur kerja anotasi.

Keuntungan utama dari mekanisme ini sangat signifikan:
- Waktu anotasi berkurang drastis dibandingkan metode tradisional.
- Memungkinkan eksplorasi objek atau kategori baru tanpa memerlukan proses pelatihan ulang (*retraining*) dari nol.
- Sangat strategis untuk membangun dataset domain spesifik yang membutuhkan akurasi tinggi dengan keterbatasan sumber daya anotasi.

Namun, di tingkat riset doktoral, kita harus tetap kritis terhadap tantangan yang melekat:
- Kualitas akhir sangat bergantung pada kejelian dan konsistensi manusia dalam memberikan koreksi.
- Kesalahan kecil pada iterasi awal berpotensi menyebar dan terakumulasi melalui proses umpan balik berulang.
- Anotasi yang dihasilkan oleh *foundation model* cenderung membawa bias sistemik yang inheren dari distribusi data latennya.

Pembahasan mengenai dinamika ini akan terus berkembang pada slide berikutnya, di mana kita akan mengupas lebih dalam peran SAM sebagai anotator otomatis. Kita akan menelaah peluang produktivitas yang ditawarkan serta risiko teknis seperti ketidakakuratan pada domain yang berbeda jauh dari data alami. Evaluasi kritis terhadap validasi subset data dan metrik keandalan seperti IoU atau Dice akan menjadi kunci dalam merancang protokol anotasi yang robust untuk penelitian Anda selanjutnya.

---

## Slide 031 - SAM sebagai Annotator: Peluang dan Risiko

### Narasi

Slide ini mengupas peran Segment Anything Model (SAM) sebagai annotator otomatis dalam alur kerja penelitian pengolahan citra digital. Mengacu pada pembahasan sebelumnya tentang human-in-the-loop, di mana manusia masih berperan aktif memberikan prompt dan melakukan koreksi iteratif, kini kita mengevaluasi bagaimana SAM dapat diposisikan sebagai komponen annotator yang lebih otonom. Pergeseran ini memiliki implikasi metodologis yang signifikan, terutama dalam konteks riset doktoral yang menuntut transparansi dan reproducibility.

Dari sisi peluang, integrasi SAM dalam pipeline anotasi menawarkan efisiensi yang tidak terabaikan. Model ini mampu mempercepat produksi dataset skala besar, khususnya untuk objek yang secara fisik atau logistik sulit dijangkau oleh anotator manusia, seperti struktur mikroskopis, citra satelit resolusi tinggi, atau kondisi lingkungan ekstrem. Selain itu, output mask dari SAM dapat langsung diintegrasikan ke tahap pelatihan segmentasi supervised, menciptakan siklus pengembangan model yang lebih cepat. Dalam praktik penelitian, hal ini memungkinkan eksplorasi domain spesifik yang sebelumnya terhambat oleh keterbatasan sumber daya anotasi manual.

Namun, penggunaan SAM sebagai annotator membawa risiko metodologis yang wajib dikaji secara kritis. Karena dilatih pada kumpulan data alamiah yang masif, SAM cenderung mematuhi kontur visual dan prior geometri yang dominan dalam distribusi training-nya. Ketika diterapkan pada domain yang sangat berbeda—seperti citra histopatologi, material science, atau drone inspection—akurasi mask dapat menurun secara substansial. Lebih krusial lagi, jika mask hasil SAM digunakan secara mentah sebagai ground truth tanpa validasi ketat, evaluasi metrik akan mengalami bias sistemik. Model yang dilatih kemudian berisiko hanya mempelajari artefak atau bias SAM, bukan pola intrinsik domain target, yang dapat melemahkan klaim kontribusi ilmiah.

Untuk mitigasi risiko tersebut, terdapat tiga rekomendasi praktis yang harus diadopsi dalam desain eksperimen tingkat doktor. Pertama, lakukan validasi ketat pada subset representatif sebelum menggunakan seluruh output SAM untuk pelatihan massal. Kedua, dokumentasikan secara transparan persentase mask yang memerlukan koreksi manual, sebagai indikator kualitas anotasi otomatis dan bahan analisis sensitivitas. Ketiga, gunakan metrik objektif seperti Intersection over Union (IoU) atau Dice coefficient terhadap anotasi manual referensi untuk mengukur reliabilitas mask SAM. Pendekatan ini memastikan fondasi dataset tetap solid dan hasil penelitian dapat dipertanggungjawabkan secara akademis.

Transisi menuju slide berikutnya akan menyoroti perbandingan fundamental antara segmentasi supervised konvensional dengan pendekatan promptable foundation model. Perbedaan utamanya terletak pada kebutuhan data, fleksibilitas penutupan kelas, tingkat interaktivitas, serta trade-off komputasional dan evaluatif. Pemahaman komparatif ini akan menjadi landasan bagi Anda dalam merancang strategi hibrida yang menggabungkan keunggulan kedua paradigma, sesuai dengan arah penelitian dan research question yang sedang Anda bangun.

---

## Slide 032 - Supervised vs Promptable Segmentation

### Narasi

Pada slide sebelumnya, kita telah menelaah bagaimana Segment Anything Model dapat berperan sebagai annotator otomatis untuk mempercepat produksi dataset, sekaligus mengidentifikasi risiko bias domain dan ketergantungan model pada kontur alami data pelatihan. Dari temuan tersebut, muncul pertanyaan metodologis mendasar mengenai posisi strategis antara segmentasi konvensional yang sepenuhnya diawasi dengan model fondasi yang responsif terhadap prompt. Slide ini menyajikan kerangka perbandingan sistematis untuk membantu peneliti menentukan paradigma yang paling relevan, atau merancang kombinasi optimal sesuai tujuan riset.

Berikut adalah penjabaran enam aspek perbandingan yang perlu dipertimbangkan dalam desain penelitian:
- **Data**: Segmentasi terawasi menuntut mask berlabel pixel-per-pixel yang memerlukan biaya anotasi tinggi, sedangkan model promptable hanya memerlukan input geometris atau tekstual sederhana dan dapat beroperasi tanpa label kelas eksplisit.
- **Kelas**: Pendekatan terawasi terkunci pada ruang kelas tertutup selama fase pelatihan, sementara model fondasi bersifat open-vocabulary dan cakupan objek ditentukan secara dinamis oleh pengguna.
- **Interaksi**: Segmentasi terawasi bersifat non-interaktif setelah deployment, berbeda dengan pendekatan promptable yang mendukung siklus iteratif hingga hasil mask memenuhi kriteria subjektif atau objektif peneliti.
- **Komputasi**: Beban utama bergeser dari biaya pelatihan model yang intensif pada metode terawasi menjadi biaya inferensi yang signifikan namun tanpa tahapan fine-tuning pada model fondasi.
- **Kualitas**: Akurasi segmentasi terawasi sangat stabil ketika domain uji konsisten dengan domain pelatihan, sedangkan performa model promptable cenderung fluktuatif pada domain baru yang menyimpang dari distribusi pretraining.
- **Evaluasi**: Metrik standar seperti mIoU, Dice, atau Panoptic Quality digunakan untuk mengukur konsistensi global pada dataset terawasi, sementara evaluasi model promptable lebih lazim dilakukan per-prompt menggunakan IoU atau Dice.

Dari sudut pandang riset tingkat doktoral, tabel ini mengarah pada strategi hibrida yang semakin dominan dalam literatur terkini. Segmentasi terawasi tetap menjadi baseline yang kuat ketika Anda bekerja pada domain spesifik dengan distribusi data stabil dan membutuhkan jaminan reproduktibilitas evaluasi. Sebaliknya, model promptable menawarkan kecepatan adaptasi tinggi untuk eksplorasi hipotesis awal, rapid prototyping, atau skenario di mana anotasi manual tidak feasible. Integrasi paling efektif terletak pada pemanfaatan prompt untuk menghasilkan pseudo-label atau draft mask, diikuti validasi subset manual, dan akhirnya pelatihan ulang model segmentasi terawasi pada label hibrida tersebut. Pendekatan ini meminimalkan bias anotasi sekaligus mempertahankan kontrol kualitas melalui supervision parsial.

Ketika merancang eksperimen dengan arsitektur hibrida atau murni berbasis prompt, stabilitas input menjadi variabel kritis yang harus dikontrol secara ketat. Hal ini membawa kita secara natural ke slide berikutnya, di mana kita akan membahas sensitivitas model terhadap variasi posisi dan tipe prompt. Memahami dinamika ini penting untuk menyusun protokol eksperimen yang reproducible, terutama apabila hasil segmentasi akan dijadikan ground truth, fitur masukan, atau dasar perbandingan kuantitatif pada tahap downstream seperti deteksi objek atau klasifikasi semantik.

---

## Slide 033 - Seberapa Sensitif terhadap Prompt?

### Narasi

Setelah membandingkan karakteristik segmentasi supervised versus model fondasional yang dapat diprompt pada slide sebelumnya, kini kita masuk ke dimensi evaluasi yang sering terlewat namun krusial dalam desain eksperimen tingkat doktor: sensitivitas model terhadap prompt. Pertanyaan mendasarnya adalah seberapa besar hasil segmentasi bergantung pada jenis, posisi, dan presisi input yang diberikan. Apakah arsitektur benar-benar menangkap representasi semantik objek secara utuh, atau sekadar melakukan interpolasi geometris mengikuti kontur lokal di sekitar prompt? Pergeseran koordinat yang hanya beberapa piksel pun dapat mengubah topologi mask secara signifikan, mengungkap potensi ketidakstabilan deterministik atau ambiguitas dalam ruang fitur model.

Untuk mengkuantifikasi perilaku tersebut, diperlukan protokol eksperimen yang terstruktur dan dapat direproduksi. Variasi parameter dapat dirancang sebagai berikut:
- **Posisi titik:** letakkan pada pusat massa objek, tepat di tepi batas, serta di area latar belakang untuk mengamati respons false positive.
- **Bentuk bounding box:** gunakan box yang tight mengikuti anotasi ground truth, box longgar yang mencakup konteks tambahan, dan box yang digeser sebagian untuk menguji toleransi offset.
- **Pengulangan stokastik:** jalankan inferensi berulang dengan seed acak berbeda guna mengukur variabilitas output dan konsistensi sampling.

Temuan empiris umumnya menunjukkan bahwa box prompt lebih stabil daripada point prompt, mengingat area konteks yang lebih luas membantu model melakukan disambiguasi semantik. Sebaliknya, penempatan titik di tepi objek rentan terhadap kebocoran mask atau pemotongan parsial karena kurangnya informasi struktural di sekitarnya. Model seperti SAM yang mendukung multi-mask output justru menyediakan mekanisme mitigasi ambiguitas yang elegan, memungkinkan peneliti melakukan seleksi berbasis metrik kualitas atau kriteria domain spesifik.

Implikasi metodologisnya menuntut disiplin dokumentasi yang ketat. Setiap protokol prompt harus mencatat koordinat absolut, skala resolusi citra, aturan fallback jika muncul multiple masks, dan nilai seed yang digunakan. Tanpa standarisasi ini, reproduktibilitas penelitian menjadi rapuh, dan klaim novelty atau perbandingan state-of-the-art kehilangan validitas statistik. Pemahaman mendalam tentang sensitivitas prompt bukan sekadar latihan teknis, melainkan fondasi rigor dalam merancang benchmark yang kredibel.

Pembahasan mengenai stabilitas prompt ini berfungsi sebagai jembatan konseptual menuju tantangan berikutnya: generalisasi lintas domain dan domain shift. Jika sensitivitas prompt menguji konsistensi model dalam satu distribusi data, maka domain shift menguji ketahanan model ketika menghadapi perubahan distribusi citra yang drastis, misalnya dari foto natural ke citra mikroskopis, satelit, atau dokumen historis. Kedua aspek ini saling melengkapi dalam membangun kerangka evaluasi robust untuk model fondasional vision, yang akan kita bedah lebih lanjut pada slide berikutnya.

---

## Slide 034 - Generalisasi Lintas Domain dan Domain Shift

### Narasi

Setelah menelaah sensitivitas model terhadap jenis dan posisi prompt pada slide sebelumnya, kita kini beralih ke aspek yang lebih fundamental dalam penerapan foundation model di lingkungan nyata, yaitu generalisasi lintas domain dan fenomena domain shift. Domain shift terjadi ketika distribusi statistik citra target secara signifikan menyimpang dari distribusi citra yang digunakan selama tahap pelatihan. Sebagai ilustrasi praktis, model segmentasi seperti SAM dilatih secara masif pada kumpulan citra alamiah, namun sering kali harus diimplementasikan pada domain spesifik seperti citra medis, penginderaan jauh, atau dokumen digital. Perbedaan karakteristik visual ini menuntut evaluasi kritis terhadap kemampuan transfer pengetahuan yang telah dipelajari model.

Dari perspektif penelitian doktoral, beberapa pertanyaan kunci perlu diuji secara empiris. Pertama, seberapa kuat kualitas mask yang dihasilkan oleh SAM bertahan ketika diaplikasikan pada domain di luar data latihnya. Kedua, apakah penurunan performa bersifat homogen untuk seluruh kelas dan ukuran objek, atau justru menunjukkan bias tertentu. Ketiga, faktor determinan apa yang paling dominan mempengaruhi degradasi kualitas tersebut: apakah tekstur permukaan objek, rasio kontras foreground-background, atau kompleksitas dan clutter pada latar belakang? Jawaban atas pertanyaan-pertanyaan ini akan membentuk landasan metodologis untuk studi robustness yang lebih lanjut.

Untuk menjawab pertanyaan riset tersebut, metode evaluasi yang direkomendasikan mengikuti alur terstruktur berikut:
- Siapkan subset data representatif dari minimal tiga domain berbeda.
- Terapkan protokol pemberian prompt yang identik secara ketat untuk memastikan fair comparison.
- Hitung metrik IoU dan Dice per domain, lalu analisis distribusinya menggunakan teknik statistik deskriptif atau uji non-parametrik.
Pendekatan ini memungkinkan kita mengidentifikasi pola kegagalan sistematis dan menentukan apakah penurunan akurasi disebabkan oleh karakteristik domain atau kelemahan arsitektural model.

Penting untuk dicatat bahwa pembahasan komprehensif mengenai robustness dan distribution shift akan dilanjutkan secara eksplisit pada Pertemuan 11. Namun, pemahaman awal tentang domain shift ini sudah cukup krusial karena langsung berdampak pada desain eksperimen kita. Ketika kita mulai membandingkan hasil segmentasi lintas domain, kita juga harus mempertimbangkan bahwa referensi ground truth yang menjadi acuan evaluasi mungkin saja mengandung noise atau inkonsistensi anotasi, yang akan menjadi fokus analisis pada slide berikutnya.

---

## Slide 035 - Anotasi Tidak Sempurna dan Evaluasi

### Narasi

Setelah membahas tantangan generalisasi lintas domain dan bagaimana distribusi citra target dapat menyimpang dari data pelatihan pada slide sebelumnya, kita kini menyoroti aspek evaluasi yang sama krusialnya, yaitu masalah anotasi tidak sempurna atau imperfect ground truth. Dalam praktik pengolahan citra digital tingkat lanjut, referensi mask yang dianggap sebagai standar emas jarang sekali bebas dari noise. Variasi antar-anotator manusia adalah fakta empiris, terutama ketika berhadapan dengan objek berukuran kecil, batas objek yang samar, atau region dengan kontras rendah. Ketidakonsistenan ini bukan sekadar kesalahan administratif, melainkan bias sistematis yang harus dipetakan dalam desain riset.

Dampaknya terhadap metrik evaluasi kuantitatif seperti Intersection over Union (IoU) dan Dice Coefficient cukup mendalam. Kedua metrik ini secara inheren mengukur kesamaan antara prediksi model dan referensi mask. Jika referensi tersebut mengandung error atau inkonsistensi, model yang sebenarnya menghasilkan segmentasi lebih akurat secara semantik justru akan menerima skor rendah. Selain itu, perbandingan antar metode penelitian menjadi tidak adil jika dataset acuan menggunakan protokol anotasi yang berbeda atau memiliki tingkat reliabilitas yang tidak seragam.

Untuk memitigasi dampak ini, terdapat tiga strategi evaluasi yang dapat diintegrasikan ke dalam pipeline riset. Pertama, libatkan multiple annotator pada subset data representatif, lalu hitung inter-annotator agreement sebagai baseline ketidakpastian anotasi. Kedua, lakukan analisis sensitivitas dengan melakukan perturbasi kecil pada mask referensi untuk mengamati stabilitas skor metrik terhadap variasi input. Ketiga, lengkapi evaluasi kuantitatif dengan review visual terstruktur, karena metrik skalar sering kali gagal menangkap kesalahan topologis atau kehilangan detail boundary yang kritis dalam aplikasi medis atau satelit.

Dari perspektif penelitian doktoral, dua pertanyaan metodologis perlu dijawab secara eksplisit. Apakah mask otomatis yang dihasilkan oleh foundation model seperti SAM dapat menggantikan peran ground truth dalam skenario tertentu, misalnya untuk self-training atau benchmarking internal? Bagaimana cara melaporkan ketidakpastian anotasi secara transparan dalam paper, sehingga kontribusi novelty dapat dinilai berdasarkan robustness terhadap noise label rather than overfitting pada referensi yang bias. Menjawab pertanyaan ini memerlukan framework evaluasi yang lebih toleran terhadap ambiguitas label.

Pembahasan konseptual ini menjadi prasyarat logis sebelum kita memasuki tahap implementasi pada slide berikutnya. Pada praktikum selanjutnya, Anda akan menjalankan SAM secara langsung di Google Colab, menguji respons model terhadap point, box, dan mask prompt, serta menghitung IoU dan Dice pada subset data nyata. Eksperimen tersebut akan memberikan konteks empiris langsung terhadap keterbatasan anotasi manusia dan strategi mitigasi yang baru saja kita diskusikan.

---

## Slide 036 - Praktikum: Eksperimen SAM di Google Colab

### Narasi

Pada slide ini, kita langsung beralih ke tahap implementasi praktis untuk menguji kemampuan *Promptable Foundation Models*, khususnya Segment Anything Model atau SAM. Setelah membahas secara teoretis bagaimana ketidaksempurnaan anotasi manusia dapat memengaruhi metrik evaluasi seperti IoU dan Dice pada slide sebelumnya, kini saatnya kita mengukur secara empiris seberapa robust model ini terhadap berbagai jenis input prompt.

Tujuan utama praktikum ini adalah menjalankan SAM dengan beberapa tipe prompt, membandingkan kualitas mask yang dihasilkan, serta menghitung metrik IoU dan Dice pada subset data tertentu. Alur eksperimen mengikuti tujuh langkah sistematis: mulai dari instalasi paket `segment-anything` beserta dependensinya, pengunduhan checkpoint model SAM berbasis arsitektur ViT-B, pemuatan gambar target, persiapan referensi mask, hingga eksekusi prediksi menggunakan *point*, *box*, dan *mask prompt*. Tahap akhir mencakup perhitungan metrik kuantitatif untuk setiap hasil mask.

Berikut adalah contoh kode ringkas yang akan kita gunakan selama sesi praktikum:

```python
from segment_anything import sam_model_registry, SamPredictor

sam = sam_model_registry["vit_b"](checkpoint="sam_vit_b_01ec64.pth")
predictor = SamPredictor(sam)
predictor.set_image(image)

mask, score, _ = predictor.predict(
    point_coords=np.array([[x, y]]),
    point_labels=np.array([1]),
    multimask_output=True
)
```

Mari kita bedah mekanisme kode tersebut. Baris pertama mengimpor registri model dan prediktor dari library resmi SAM. Inisialisasi model dilakukan melalui `sam_model_registry["vit_b"]` dengan memuat bobot pre-trained dari file `sam_vit_b_01ec64.pth`. Objek `SamPredictor` kemudian diikat ke model tersebut, dan metode `set_image` dipanggil untuk mengekstrak representasi fitur gambar sebelum proses inferensi. 

Inti dari eksperimen terletak pada pemanggilan `predictor.predict`. Parameter `point_coords` menerima array NumPy berdimensi dua yang merepresentasikan koordinat pixel `(x, y)` sebagai *point prompt*, sementara `point_labels` menentukan semantik titik tersebut dengan nilai `1` untuk foreground. Penetapan `multimask_output=True` merupakan kunci penting, karena instruksi ini memaksa model untuk menghasilkan tiga kandidat mask sekaligus beserta skor kepercayaan masing-masing. Output ini memungkinkan kita melakukan seleksi otomatis berdasarkan skor tertinggi atau menganalisis variasi geometris antar kandidat mask.

Hasil empiris dari alur dan kode di atas akan menjadi fondasi material untuk slide berikutnya, di mana kita akan menyusun tabel perbandingan performa berdasarkan strategi prompt yang diterapkan. Dengan data ini, kita dapat mengevaluasi secara kritis apakah pendekatan promptable foundation model memang layak menggantikan anotasi manual konvensional, sekaligus mengidentifikasi celah penelitian terkait stabilitas mask pada batas objek yang ambigu—sebuah isu sentral yang perlu ditindaklanjuti dalam perancangan metodologi riset tingkat doktor.

---

## Slide 037 - Evaluasi Prompt Strategy dan Rekomendasi Riset

### Narasi

Setelah kita menyelesaikan praktikum implementasi SAM di lingkungan Google Colab pada slide sebelumnya, langkah logis berikutnya adalah mengevaluasi bagaimana strategi pemilihan prompt memengaruhi kualitas segmentasi secara kuantitatif dan kualitatif. Tabel yang ditampilkan menyajikan perbandingan ilustratif antara lima konfigurasi prompt umum: titik di tengah objek, titik di tepi, bounding box ketat, bounding box longgar, serta penggunaan mask awal sebagai referensi.

Perhatikan bahwa nilai IoU dan Dice sangat bergantung pada presisi dan lokasi prompt. Prompt titik di tengah objek cenderung menghasilkan IoU 0,72 dengan Dice 0,83, namun sering kehilangan detail pada bagian tepi karena model mengasumsikan pusat sebagai representasi dominan. Sebaliknya, prompt di tepi objek menunjukkan penurunan stabilitas dengan IoU turun menjadi 0,55, mengingat SAM sangat sensitif terhadap noise atau ambiguitas geometris di batas objek. Bounding box ketat memberikan hasil paling konsisten dengan IoU 0,85 dan Dice 0,92, sementara box longgar masih mempertahankan performa baik (IoU 0,79) meski kadang menyertakan latar belakang yang tidak relevan. Penggunaan mask awal sebagai prompt justru memberikan perbaikan maksimal, mencerminkan kemampuan SAM dalam melakukan refinement berbasis konteks spasial yang sudah terdefinisi. Perlu ditekankan bahwa angka-angka dalam tabel ini bersifat ilustratif untuk keperluan pedagogis, bukan hasil eksperimen final dari dataset spesifik Anda.

Dari perspektif metodologi penelitian tingkat doktoral, dokumentasi protokol prompt harus dilakukan secara sistematis dan reproducible. Variasi jenis prompt perlu diuji secara eksplisit untuk mengukur sensitivitas model terhadap kondisi input yang berbeda-beda. Jika SAM dimanfaatkan sebagai alat bantu anotasi semi-otomatis, validasi silang terhadap anotasi manual pada subset data tetap menjadi keharusan guna memastikan reliabilitas ground truth sebelum digunakan untuk tahap training atau evaluasi model downstream.

Untuk persiapan seminar atau defense riset Anda, pertanyakan kembali relevansi metrik evaluasi sesuai dengan karakteristik domain aplikasi. Apakah IoU lebih diutamakan untuk tugas segmentasi semantik yang menuntut overlap pixel sempurna, atau apakah Dice coefficient lebih tepat untuk kasus kelas tidak seimbang seperti segmentasi medis beresolusi tinggi? Hasil evaluasi prompt strategy ini juga harus diposisikan secara kritis terhadap state-of-the-art saat ini. Bagaimana temuan Anda berkontribusi pada pemahaman tentang robustness foundation model terhadap variasi prompt, dan apakah ada celah riset yang bisa dikembangkan menjadi novelty metodologis atau arsitektural?

Dengan demikian, analisis evaluasi ini menutup rangkaian pembahasan mengenai segmentasi berbasis promptable foundation models. Pada pertemuan berikutnya, kita akan beralih ke topik yang semakin berkembang pesat, yaitu Generative Vision dengan Diffusion Models.

---

## Slide 038 - Penutup

### Narasi

Kita telah menyelesaikan pembahasan komprehensif mengenai segmentasi citra berbasis model fondasi yang responsif terhadap prompt, dengan penekanan utama pada arsitektur Segment Anything Model (SAM) serta dinamika pengaruh berbagai jenis input prompt terhadap hasil segmentasi. Pada slide evaluasi sebelumnya, kita telah melihat secara kuantitatif bagaimana variasi prompt—mulai dari titik koordinat, bounding box, hingga mask inisial—secara langsung membentuk nilai IoU dan Dice, sekaligus mengungkap potensi ketidakstabilan jika protokol tidak dikendalikan dengan ketat.

Eksperimen sensitivitas prompt ini harus dipahami sebagai bagian fundamental dari metodologi penelitian tingkat doktoral. Dokumentasi lengkap parameter prompt, strategi preprocessing gambar sebelum masuk ke encoder, serta prosedur validasi terhadap anotasi ground truth merupakan prasyarat agar temuan Anda dapat direproduksi dan dibandingkan secara adil dengan state-of-the-art lainnya. Jika SAM dimanfaatkan sebagai generator anotasi otomatis, pastikan dilakukan sampling stratified pada subset data kritis untuk mengukur bias sistematis yang mungkin tersembunyi di balik skor agregat yang tampak tinggi.

Dengan demikian, pertemuan kedelapan ini menutup siklus diskusi tentang pendekatan promptable foundation models sebagai solusi segmentasi semantik dan instan. Transisi alami berikutnya mengarah pada bagaimana representasi visual tidak hanya dibagi atau dipartisi, tetapi juga dihasilkan kembali secara kondisional melalui proses stokastik yang terstruktur. 

Pertemuan selanjutnya akan membahas Generative Vision dengan Diffusion Models. Kita akan menguraikan prinsip dasar forward-reverse diffusion process, teknik conditioning berbasis teks dan citra, serta strategi optimasi training yang relevan untuk tugas-tugas lanjutan seperti image restoration, inpainting, dan synthesis berbasis constraint. Terima kasih atas kontribusi dan diskusi intensif selama sesi hari ini.
