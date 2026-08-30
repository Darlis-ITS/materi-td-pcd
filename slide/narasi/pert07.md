# Narasi TD Pengolahan Citra Digital - Pertemuan 07

## Object Detection Modern dengan YOLO dan Transformer

Sumber: markdown/pert07-object-detection-modern-dengan-yolo-dan-transformer.md

---

## Slide 000 - Cover

### Narasi

Slide ini menjadi pembuka resmi untuk Pertemuan 07 mata kuliah Topik Dalam Pengolahan Citra Digital. Fokus utama pertemuan kali ini adalah eksplorasi mendalam mengenai arsitektur deteksi objek modern, dengan penekanan khusus pada evolusi keluarga model YOLO serta integrasi mekanisme transformer dalam tugas instance understanding. Pada jenjang doktoral, pembahasan tidak hanya berhenti pada implementasi praktis, melainkan menuntut analisis kritis terhadap trade-off antara akurasi, kecepatan inferensi, efisiensi komputasi, dan skalabilitas pada dataset besar.

Perkembangan terkini menunjukkan pergeseran arsitektural yang signifikan. Dari dominasi CNN-based yang mengandalkan receptive field bertahap, komunitas riset kini beralih ke hybrid architectures yang menggabungkan lokalisasi lokal dengan pemodelan konteks global melalui self-attention mechanisms. Model YOLO generasi terbaru telah mengadopsi komponen vision transformer untuk memperkuat representasi fitur multi-skala, sementara pendekatan pure transformer mulai menantang dominasi YOLO dalam skenario dengan kompleksitas latar belakang tinggi, occlusion parah, atau kebutuhan generalisasi lintas domain yang ketat.

Untuk mendukung kajian literatur dan eksperimen komputasional pada sesi lanjutan, materi ini akan membimbing mahasiswa dalam mengevaluasi metodologi state-of-the-art, mengidentifikasi research gap terkait robustness dan interpretability, serta merancang benchmarking yang valid dan reproducible. Pembahasan teknis dan pemetaan capaian pembelajaran akan dilanjutkan pada slide berikutnya, yang secara eksplisit menghubungkan topik ini dengan kemampuan mengevaluasi klaim ilmiah, merancang eksperimen komputasional, serta pemanfaatan framework foundation model modern untuk penelitian tingkat lanjut.

---

## Slide 001 - Posisi Pertemuan 07 dalam RPS

### Narasi

Slide ini berfungsi sebagai penanda posisi pertemuan ketujuh dalam alur kurikulum mata kuliah Topik Dalam Pengolahan Citra Digital. Sebelumnya, pada pertemuan enam, kita telah membahas *image restoration* dan *computational imaging*, yang berfokus pada pemulihan kualitas citra dari berbagai bentuk degradasi fisik maupun matematis. Materi kali ini melanjutkan rantai tersebut dengan beralih ke deteksi objek modern menggunakan arsitektur YOLO dan model berbasis *transformer*. Setelah sesi ini, perkuliahan akan berlanjut ke segmentasi citra serta *promptable foundation models*, sehingga tercipta kesinambungan konseptual dari pemulihan citra, identifikasi objek, hingga pemetaan semantik tingkat tinggi.

Secara struktural, bahan kajian ini merujuk pada BK-07 tentang *Object Detection* dan *Instance Understanding*. Pada jenjang doktoral, penguasaan algoritma saja tidak memadai. Kita harus mampu melakukan evaluasi kritis terhadap klaim metodologis dalam literatur terkini, sebagaimana tertuang dalam CPMK-2. Mahasiswa dituntut untuk mengidentifikasi *research gap*, menilai validitas eksperimen, dan memposisikan karya ilmiah terhadap *state-of-the-art* yang sedang berkembang.

Selain aspek tinjauan literatur, pertemuan ini juga menyasar dua kompetensi inti lainnya:
- CPMK-3 menuntut kemampuan merancang eksperimen komputasional yang valid dan dapat direproduksi (*reproducible*). Ini mencakup seleksi dataset, protokol augmentasi, konfigurasi hiperparameter, serta pemilihan metrik evaluasi yang objektif dan konsisten.
- CPMK-4 mengarahkan pada pemanfaatan ekosistem komputasi modern seperti PyTorch, *timm*, Hugging Face, serta integrasi model fondasi seperti YOLO, CLIP, atau DINOv2 untuk membangun baseline eksperimen yang kuat dan siap dikembangkan menjadi kontribusi ilmiah baru.

Peta konseptual ini akan menjadi panduan utama dalam menyusun analisis kritis terhadap paper, merancang skema eksperimen, dan merumuskan pertanyaan penelitian yang tajam. Penjelasan lebih rinci mengenai tujuan pembelajaran spesifik dan agenda diskusi akan dijabarkan pada slide berikutnya, yang akan membedah pipeline deteksi, metrik evaluasi standar, serta strategi mitigasi kesalahan deteksi.

---

## Slide 002 - Tujuan Pembelajaran dan Agenda

### Narasi

Pada pertemuan ini, kita memasuki topik ketujuh yang berfokus pada object detection dan instance understanding. Sesuai dengan kerangka RPS, materi ini dirancang untuk memenuhi capaian pembelajaran tingkat doktor, khususnya dalam mengevaluasi metodologi deteksi mutakhir, merancang eksperimen komputasional yang valid, serta memanfaatkan framework modern untuk eksplorasi ilmiah.

Tujuan pembelajaran pada slide ini mencakup empat pilar utama. Pertama, mahasiswa harus memahami pipeline end-to-end object detection, mulai dari representasi citra masukan hingga ekstraksi fitur yang menghasilkan bounding box, label kelas, dan nilai confidence. Kedua, kita akan membedah perbedaan arsitektural dan filosofis antara detector one-stage seperti keluarga YOLO dengan pendekatan transformer-based yang mengandalkan mekanisme self-attention untuk menangkap konteks global. Ketiga, evaluasi kinerja detektor memerlukan metrik yang rigor, bukan sekadar akurasi konvensional, melainkan Intersection over Union (IoU), precision, recall, dan mean Average Precision (mAP) yang menjadi standar publikasi bereputasi. Keempat, analisis kesalahan deteksi harus dilakukan secara sistematis melalui confusion analysis, studi variasi ukuran objek, serta identifikasi domain shift yang sering menjadi akar masalah kegagalan generalisasi model.

Agenda pertemuan ini disusun secara progresif untuk menjembatani teori dan praktik penelitian. Kita akan memulai dengan konfirmasi posisi materi dalam kurikulum, dilanjutkan dengan penguatan konsep dasar deteksi dan interpretasi metrik evaluasi. Pembahasan teknis akan berlanjut pada arsitektur YOLO dan transformer-based detector, diikuti oleh diskusi kritis mengenai tantangan penelitian aktual seperti class imbalance, optimasi threshold, konsistensi anotasi, serta paradigma open-vocabulary detection yang menawarkan ruang kontribusi ilmiah baru. Sesi terakhir akan mengarah pada workflow eksperimen reproducible, panduan praktikum, dan metodologi analisis error yang siap diintegrasikan ke dalam rancangan penelitian disertasi.

Sebelum masuk ke detail arsitektur, perlu dijaga kesinambungan konseptual dengan pembahasan pertemuan sebelumnya mengenai image restoration dan computational imaging. Meskipun fokus bergeser dari pemulihan citra menuju pemahaman semantik dan spasial, prinsip evaluasi objektif tetap relevan. Kualitas citra memang berdampak pada performa detektor, namun pada tahap ini perhatian utama kita tertuju pada akurasi lokalisasi dan klasifikasi. Mindset baseline yang adil serta pemahaman mendalam tentang karakteristik data akan menjadi fondasi kritis ketika kita menguji model deteksi modern terhadap noise, degradasi, atau distribusi data yang tidak seimbang.

---

## Slide 003 - Recap Singkat Pertemuan 06

### Narasi

Mari kita mulai dengan rekonsilisi singkat terhadap materi Pertemuan 06 mengenai Image Restoration dan Computational Imaging. Pada sesi tersebut, kita telah mendefinisikan restorasi citra sebagai sebuah masalah inversi, di mana tujuan komputasinya adalah memperkirakan citra bersih dari observasi yang terdegradasi. Kita juga mengkritisi keterbatasan metrik kuantitatif konvensional seperti PSNR dan SSIM yang sering kali gagal menangkap kesenjangan dengan kualitas perseptual manusia. Seiring perkembangan terkini, pendekatan berbasis diffusion telah menjadi state-of-the-art dalam domain ini, namun implementasinya menuntut protokol evaluasi yang ketat agar optimisasi teknis tidak mengabaikan validitas semantik.

Kaitan fundamental antara restorasi dan deteksi objek perlu ditekankan kembali. Meskipun kualitas citra secara langsung memengaruhi robustness model deteksi, fokus utama pada tugas deteksi bergeser dari rekonstruksi piksel menuju penentuan koordinat spasial dan klasifikasi semantik. Pola pikir metodologis yang kita bangun selama studi restorasi tetap menjadi kerangka kerja yang valid dan dapat diterjemahkan secara langsung ke dalam konteks deteksi modern, meliputi:
- Penetapan baseline yang adil untuk membandingkan peningkatan performa
- Karakterisasi pola degradasi dan dampaknya terhadap ekstraksi fitur
- Analisis kegagalan sistematis yang tidak hanya melihat akurasi global, tetapi juga distribusi error berdasarkan kompleksitas scene

Dengan demikian, pertemuan ini menandai pergeseran orientasi analitis dari pemulihan citra menuju pemahaman isi citra secara lebih komprehensif.

Langkah ini merupakan konsekuensi logis dari agenda dan tujuan pembelajaran yang telah kita tetapkan pada awal perkuliahan. Setelah menyepakati roadmap evaluasi, eksplorasi arsitektur detektor mutakhir akan menjadi fokus utama kita. Penjelasan mengenai pergeseran paradigma ini juga menyiapkan landasan konseptual untuk Pertemuan 08. Konsep evaluasi area, ketidakseimbangan data, dan analisis kesalahan yang akan kita bedah hari ini akan direkayasa ulang untuk konteks segmentasi piksel-per-piksel, termasuk adaptasi metrik IoU dan Dice dalam framework seperti SAM dan U-Net. Deteksi pada tahap ini berfungsi sebagai benchmark kualitas representasi fitur, sedangkan segmentasi akan menuntut granularitas pemahaman visual yang jauh lebih halus.

---

## Slide 004 - Jembatan Menuju Pertemuan 08

### Narasi

Pada slide sebelumnya, kita telah meninjau kembali bagaimana image restoration diposisikan sebagai inverse problem dan mengapa metrik kuantitatif konvensional seperti PSNR maupun SSIM sering kali gagal menangkap degradasi perseptual yang sebenarnya. Fokus kini bergeser secara strategis dari pemulihan citra menuju pemahaman isi citra. Pergeseran ini bukan sekadar perpindahan topik, melainkan perluan metodologis untuk menguji seberapa robust representasi yang telah kita bangun selama pertemuan-pertemuan sebelumnya mampu mengekstrak struktur semantik yang lebih kompleks dan terstruktur.

Slide ini berfungsi sebagai jembatan konseptual menuju pertemuan berikutnya, di mana kita akan mendalami segmentasi citra secara mendalam. Deteksi objek pada tingkat dasarnya hanya menghasilkan bounding box, sedangkan segmentasi menuntut granularitas per piksel melalui mask biner atau probabilistik. Konsep evaluasi yang akan kita pelajari hari ini, termasuk Intersection over Union (IoU), analisis kesalahan deteksi, dan trade-off antara precision dan recall, akan menjadi fondasi langsung ketika kita membahas koefisien Dice, arsitektur U-Net, serta model segmentasi berbasis transformer seperti SAM dan pendekatan promptable segmentation. Evolusi metrik ini penting untuk dipahami karena mencerminkan pergeseran prioritas dari lokalisasi kasar menuju pemetaan wilayah semantik yang presisi.

Secara garis besar, benang merah perkuliahan menunjukkan bahwa pertemuan 03 hingga 06 telah membangun pondasi berupa representasi fitur, teknik computational imaging, dan penyelarasan multimodal. Object detection hadir di sini sebagai tugas turunan yang berfungsi sebagai benchmark kritis untuk mengukur kualitas representasi tersebut. Jika deteksi sudah mampu melokalisasi dan mengklasifikasi objek secara global, maka segmentasi akan menuntut pemahaman spasial yang jauh lebih halus, membuka ruang bagi eksplorasi model foundation dan interpretasi region-level yang lebih presisi. Dari perspektif penelitian tingkat doktoral, memahami batas antara deteksi dan segmentasi membantu kita mengidentifikasi research gap, misalnya pada kasus objek tumpang tindih atau skenario zero-shot prompting yang memerlukan representasi kontekstual lebih dalam daripada kotak pembatas.

Untuk memulai pembahasan teknis mengenai modern object detection dengan YOLO dan Transformer, mari kita terlebih dahulu menyamakan persepsi mengenai definisi formal dan format outputnya. Pada slide berikutnya, kita akan menguraikan struktur data deteksi, sistem koordinat piksel standar, serta makna statistik dari confidence score sebelum masuk ke arsitektur model mutakhir dan strategi training yang relevan untuk eksperimen tingkat lanjut.

---

## Slide 005 - Apa itu Object Detection?

### Narasi

Object detection merupakan tugas fundamental dalam computer vision yang menggabungkan dua proses sekaligus: pelokalan spasial objek dan identifikasi kategorinya. Berbeda dengan klasifikasi citra yang hanya menghasilkan satu label global untuk seluruh gambar, deteksi objek harus memetakan setiap entitas yang relevan ke dalam region spesifik. Output standar dari sistem deteksi modern berupa sekumpulan bounding box yang dilengkapi dengan label kelas dan skor keyakinan, sehingga model tidak hanya menjawab "apa", tetapi juga "di mana".

Representasi output yang paling umum digunakan adalah tuple lima elemen: `(x_min, y_min, x_max, y_max, class, score)`. Sistem koordinat piksel mengacu pada titik asal di pojok kiri atas citra, dengan sumbu x bergerak ke kanan dan sumbu y bergerak ke bawah. Nilai `score` merepresentasikan probabilitas atau confidence score yang dihasilkan oleh head klasifikasi model. Dalam praktik penelitian tingkat lanjut, thresholding pada nilai ini menjadi langkah kritis sebelum menerapkan non-maximum suppression (NMS) atau integrasi dengan loss berbasis IoU yang telah dibahas pada jembatan menuju pertemuan berikutnya.

```python
def xyxy_to_cxcywh(box):
    x1, y1, x2, y2 = box
    cx = (x1 + x2) / 2
    cy = (y1 + y2) / 2
    w = x2 - x1
    h = y2 - y1
    return [cx, cy, w, h]
```

Dalam ekosistem framework deteksi, representasi koordinat dapat bervariasi tergantung arsitektur backbone dan head yang digunakan. Fungsi konversi di atas mengubah format `xyxy` menjadi `cxcywh`, yang banyak dipakai dalam loss functions seperti GIoU, DIoU, atau CIoU karena sifatnya yang invariant terhadap translasi. Perhitungan `cx` dan `cy` menggunakan rata-rata batas minimum dan maksimum, sementara lebar dan tinggi diperoleh dari selisih koordinat ekstrem. Memahami konversi ini sangat penting ketika melakukan custom dataset parsing, debugging gradient propagation, atau mengintegrasikan modul deteksi baru ke dalam pipeline multi-task learning.

Konsep representasi bounding box ini akan terus berkembang pada slide berikutnya, di mana kita akan membahas tiga format utama (`xyxy`, `xywh`, `cxcywh`) beserta implikasi numeriknya. Pemahaman mendalam tentang format koordinat bukan sekadar urusan sintaks library, melainkan fondasi desain loss function, normalisasi target, dan evaluasi metrik yang akan menentukan stabilitas training serta generalisasi model pada data distribusi-shifted.

---

## Slide 006 - Representasi Bounding Box

### Narasi

Pada slide sebelumnya, kita telah menyinggung bahwa output standar sistem object detection mencakup koordinat lokasi, label kelas, dan confidence score. Koordinat tersebut secara default biasanya ditulis dalam format `xyxy`. Namun, dalam ekosistem computer vision yang kompleks, representasi geometri ini tidak bersifat universal. Setiap framework, backbone network, atau protokol benchmark sering kali memiliki preferensi format sendiri-sendiri. Ketidaktepatan dalam memahami dan mengelola representasi ini dapat menyebabkan propagasi error yang fatal pada tahap training maupun inference.

Secara akademis dan praktis, terdapat tiga format bounding box yang wajib dikuasai:
- `xyxy`: `(x_min, y_min, x_max, y_max)`. Format berbasis koordinat sudut ini paling mudah divisualisasikan dan sering menjadi standar awal annotation tool.
- `xywh`: `(x_min, y_min, width, height)`. Lebih efisien untuk komputasi area dan banyak diadopsi pada head detektor yang memprediksi offset relatif terhadap anchor.
- `cxcywh`: `(center_x, center_y, width, height)`. Representasi berbasis pusat ini memberikan stabilitas numerik yang lebih baik selama backpropagation, sehingga menjadi pilihan dominan pada arsitektur modern seperti YOLO series dan DETR variants.

Konversi antar format bukanlah operasi sekunder, melainkan bagian integral dari data pipeline. Perhatikan implementasi fungsi berikut:
```python
def xyxy_to_cxcywh(box):
    x1, y1, x2, y2 = box
    cx = (x1 + x2) / 2
    cy = (y1 + y2) / 2
    w = x2 - x1
    h = y2 - y1
    return [cx, cy, w, h]
```
Fungsi ini mengekstrak keempat koordinat ekstrem, menghitung titik pusat melalui rata-rata aritmatika, lalu menurunkan dimensi lebar dan tinggi melalui pengurangan koordinat. Dalam konteks penelitian tingkat doktoral, Anda akan kerap menghadapi annotasi dari sumber heterogen. Selalu validasi schema metadata dataset sebelum melakukan transformasi tensor, karena ketidaksesuaian format dapat mengacaukan loss calculation dan menurunkan performa evaluasi secara drastis.

Penguasaan representasi geometri ini menjadi fondasi kritis sebelum kita mengkuantifikasi akurasi prediksi. Ketika model menghasilkan bounding box, kita perlu ukuran objektif untuk menilai seberapa baik prediksi tersebut menyatu dengan ground truth. Pada slide berikutnya, kita akan membahas Intersection over Union (IoU), metrik fundamental yang menjadi standar industri dalam menentukan true positive, false positive, serta protokol evaluasi rigor seperti yang diterapkan pada COCO benchmark.

---

## Slide 007 - Intersection over Union (IoU)

### Narasi

Setelah membahas representasi koordinat bounding box pada slide sebelumnya, langkah logis berikutnya dalam pipeline object detection adalah mengevaluasi akurasi spasial prediksi model terhadap ground truth. Evaluasi ini tidak lagi bergantung pada format koordinat, melainkan pada seberapa besar area prediksi tumpang tindih dengan anotasi sebenarnya. Konsep yang menjadi standar de facto untuk tujuan ini adalah Intersection over Union atau IoU.

Secara matematis, IoU mengukur proporsi area irisan terhadap area gabungan dari dua bounding box, dinyatakan sebagai IoU = |A ∩ B| / |A ∪ B|. Diagram pada slide ini secara visual memisahkan area unik A, area unik B, dan area irisan keduanya. Pembilang merepresentasikan konsistensi lokalisasi model, sedangkan penyebut menormalisasi nilai sehingga hasilnya selalu berada dalam rentang [0, 1]. Nilai 1 menandakan kesempurnaan tumpang tindih, sementara nilai mendekati 0 menunjukkan ketidakcocokan lokasi yang signifikan.

Implementasi fungsionalnya dapat dilihat pada kode berikut:
```python
def iou(box1, box2):
    x1 = max(box1[0], box2[0])
    y1 = max(box1[1], box2[1])
    x2 = min(box1[2], box2[2])
    y2 = min(box1[3], box2[3])
    inter = max(0, x2 - x1) * max(0, y2 - y1)
    area1 = (box1[2]-box1[0]) * (box1[3]-box1[1])
    area2 = (box2[2]-box2[0]) * (box2[3]-box2[1])
    union = area1 + area2 - inter
    return inter / union if union > 0 else 0
```
Fungsi ini menerjemahkan prinsip geometri secara eksplisit. Batas kiri atas irisan ditentukan oleh maksimum koordinat awal, sedangkan batas kanan bawah oleh minimum koordinat akhir. Operasi `max(0, ...)` pada perhitungan lebar dan tinggi irisan berfungsi sebagai guard clause untuk menangani kasus non-overlapping yang secara alami menghasilkan dimensi negatif. Luas gabungan dihitung dengan prinsip inklusi-eksklusi (`area1 + area2 - inter`) untuk mencegah penghitungan ganda. Kondisi `union > 0` memastikan stabilitas numerik saat kedua kotak tidak bersentuhan sama sekali.

Dalam praktik evaluasi state-of-the-art, IoU berperan sebagai mekanisme thresholding biner. Sebuah prediksi dianggap valid (true positive) hanya jika nilai IoU-nya melebihi ambang batas tertentu; jika tidak, prediksi tersebut diklasifikasikan sebagai false positive. Protokol evaluasi benchmark seperti COCO tidak puas dengan single-threshold evaluation. Mereka menerapkan multi-threshold protocol dengan menghitung mAP pada rentang IoU 0,50 hingga 0,95 dengan langkah 0,05. Pendekatan ini memaksa peneliti untuk tidak hanya mengoptimalkan deteksi kasar, tetapi juga meningkatkan presisi boundary localization, yang sangat relevan untuk tugas-tugas downstream seperti instance segmentation atau medical imaging.

Nilai IoU yang telah dikomputasi untuk setiap prediksi akan menjadi fondasi klasifikasi label pada tahap evaluasi. Dengan menetapkan threshold IoU yang konsisten, kita dapat memetakan setiap prediksi ke dalam kategori TP, FP, atau FN. Klasifikasi inilah yang kemudian memungkinkan perhitungan metrik agregat seperti Precision dan Recall, yang akan kita bahas secara mendalam pada slide berikutnya untuk menilai trade-off antara ketepatan prediksi dan kelengkapan deteksi.

---

## Slide 008 - Precision dan Recall

### Narasi

Setelah pada slide sebelumnya kita membahas Intersection over Union sebagai fondasi pengukuran tumpang tindih antara bounding box prediksi dan ground truth, kini kita beralih ke bagaimana hasil pengukuran tersebut diklasifikasikan menjadi tiga kategori dasar evaluasi deteksi:

- **True Positive (TP)**: prediksi yang memenuhi ambang batas IoU dan memiliki kesesuaian kelas dengan label aktual.
- **False Positive (FP)**: prediksi yang gagal memenuhi kriteria IoU atau mengalami mismatch kelas, sehingga dianggap sebagai kesalahan deteksi.
- **False Negative (FN)**: objek nyata dalam citra yang sama sekali tidak berhasil ditangkap oleh model.

Berdasarkan pengelompokan ini, dua metrik fundamental dihitung melalui formulasi berikut:
- **Precision** = TP / (TP + FP)
- **Recall** = TP / (TP + FN)

Precision menjawab tingkat akurasi lokal model terhadap setiap prediksi yang diluncurkannya, sedangkan Recall mengukur cakupan global model dalam mengidentifikasi seluruh objek target. Dalam implementasinya, kedua metrik ini bersifat inversely proportional. Menaikkan jumlah prediksi atau menurunkan ambang batas confidence score akan mendorong Recall naik karena objek yang terlewat semakin sedikit, namun hal ini otomatis meningkatkan False Positive sehingga Precision turun. Pada riset tingkat doktoral, manajemen trade-off ini sering kali ditangani melalui teknik confidence calibration, non-maximum suppression yang lebih ketat, atau penyesuaian mekanisme attention pada arsitektur transformer-based detector.

Konsep Precision dan Recall ini merupakan blok pembangun yang belum lengkap tanpa konteks variasi ambang batas. Pada slide berikutnya, kita akan menggabungkan pasangan nilai ini sepanjang rentang confidence threshold untuk membentuk Precision-Recall Curve. Kurva tersebut kemudian diintegralkan secara numerik menjadi Average Precision (AP), yang menjadi standar emas dalam evaluasi benchmark modern seperti COCO dan menjadi titik tolak utama dalam melakukan critical comparison antar-state-of-the-art object detectors.

---

## Slide 009 - Precision-Recall Curve dan Average Precision

### Narasi

Setelah memahami definisi dasar dari precision dan recall pada slide sebelumnya, langkah selanjutnya adalah melihat bagaimana kedua metrik ini berinteraksi secara dinamis ketika model menghasilkan banyak kandidat bounding box. Pada object detection, output model bukanlah satu label biner, melainkan sekumpulan prediksi dengan confidence score yang bervariasi. Untuk membangun kurva Precision-Recall, seluruh prediksi harus diurutkan dari confidence tertinggi ke terendah.

Evaluasi dilakukan secara kumulatif sepanjang daftar yang telah diurutkan tersebut. Setiap kali kita melintasi satu prediksi, sistem akan memeriksa apakah IoU memenuhi ambang batas dan apakah kelas cocok. Jika ya, prediksi ditandai sebagai True Positive; jika tidak, sebagai False Positive. Dengan menghitung precision dan recall pada setiap titik peringkat, kita memperoleh serangkaian pasangan koordinat yang membentuk kurva bertingkat. Kurva ini secara visual memetakan trade-off fundamental: upaya meningkatkan recall dengan menurunkan threshold kepercayaan hampir selalu berdampak pada penurunan precision.

Average Precision atau AP kemudian berfungsi sebagai ringkasan numerik yang padat dari kurva tersebut. Secara matematis, AP merepresentasikan luas area di bawah kurva Precision-Recall. Nilai AP yang mendekati satu mengindikasikan bahwa model berhasil mempertahankan precision tinggi bahkan saat recall ditingkatkan, yang menjadi tolok ukur utama untuk menilai kualitas detektor sebelum masuk ke tahap optimisasi hyperparameter atau arsitektur.

Implementasi logika ini tercermin jelas dalam pseudo-code berikut:
```text
predictions sorted by confidence descending
for each prediction:
    if IoU >= threshold and class matches:
        mark TP
    else:
        mark FP
    compute precision and recall at each rank
AP = area under the precision-recall curve
```
Algoritma ini menegaskan bahwa pengurutan berdasarkan confidence adalah prasyarat mutlak. Iterasi sekuensial menandai status TP/FP secara kumulatif, lalu memperbarui metrik precision dan recall pada setiap langkah. Setelah seluruh prediksi diproses, teknik integrasi atau interpolasi diterapkan pada kurva yang terbentuk untuk mengekstrak nilai AP tunggal per kelas.

Konsep AP ini menjadi landasan analitis yang krusial sebelum kita beralih ke metrik agregasi tingkat lanjut. Pada slide berikutnya, kita akan membahas bagaimana AP individual dirata-ratakan menjadi mean Average Precision (mAP), serta varian standar seperti AP@IoU=0.5, AP@IoU=0.75, hingga AP@IoU=0.5:0.95 yang menjadi acuan benchmark COCO. Penguasaan mekanisme konstruksi kurva dan perhitungan AP ini diperlukan untuk melakukan validasi eksperimental yang rigor dan identifikasi kelemahan model secara presisi.

---

## Slide 010 - mAP dan Variannya

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana Average Precision (AP) dihitung berdasarkan kurva Precision-Recall untuk satu kelas objek tertentu. Namun, dalam skenario deteksi objek nyata, dataset hampir selalu mengandung banyak kategori. Di sinilah Mean Average Precision atau mAP diperkenalkan sebagai ekstensi logis dari konsep AP. Secara matematis, mAP dihitung dengan mengambil rata-rata aritmatik dari nilai AP yang diperoleh pada setiap kelas yang ada dalam dataset.

Sebagai metrik agregat, mAP menjadi standar utama dalam evaluasi benchmark modern, khususnya COCO Detection Challenge. Nilai ini memberikan gambaran holistik tentang performa model sekaligus dalam hal klasifikasi dan lokalisasi. Meskipun ringkas, interpretasi mAP memerlukan pemahaman mendalam terhadap strategi penghitungan yang digunakan oleh benchmark tersebut.

Benchmark seperti COCO tidak hanya melaporkan satu nilai mAP tunggal, melainkan menggunakan beberapa variasi berdasarkan threshold Intersection over Union (IoU):
- AP@IoU=0.5: Merepresentasikan kriteria longgar yang mirip dengan gaya evaluasi PASCAL VOC, di mana bounding box dianggap benar jika tumpang tindih minimal 50 persen.
- AP@IoU=0.75: Menerapkan kriteria ketat yang menuntut akurasi lokalisasi tinggi, sehingga lebih sensitif terhadap pergeseran kecil pada koordinat kotak prediksi.
- AP@IoU=0.5:0.95: Merupakan default resmi COCO, yang menghitung rata-rata AP pada sepuluh threshold IoU bertahap mulai dari 0,50 hingga 0,95. Pendekatan ini memastikan model tidak hanya unggul pada lokalisasi kasar, tetapi juga konsisten pada penempatan kotak yang presisi.

Penting untuk dicatat bahwa mAP, meskipun sangat berguna, hanyalah sebuah angka agregat yang menggabungkan dua aspek berbeda: kebenaran klasifikasi dan ketepatan lokalisasi. Satu nilai mAP tidak pernah mengungkap di mana model gagal, apakah karena salah mengidentifikasi kategori, bounding box yang terlalu longgar, atau kegagalan mendeteksi objek tersembunyi. Oleh karena itu, dalam praktik penelitian tingkat lanjut, mAP harus selalu dilengkapi dengan analisis kesalahan yang lebih granular. Pembahasan mengenai keterbatasan mAP serta pentingnya error analysis akan kita kaji secara mendalam pada slide berikutnya, di mana kita akan membedah distribusi false positive, false negative, dan pengaruh ukuran objek terhadap skor akhir model.

---

## Slide 011 - Keterbatasan mAP dan Pentingnya Error Analysis

### Narasi

Setelah membahas metrik evaluasi utama seperti mAP pada slide sebelumnya, kita perlu menyadari bahwa angka tunggal ini memiliki keterbatasan mendasar dalam menilai performa detektor secara komprehensif. Dua model dapat menghasilkan nilai mAP yang identik, namun pola kesalahan mereka bisa sangat berbeda. Hal ini sering terjadi karena peningkatan mAP cenderung didominasi oleh kelas mayoritas atau objek berukuran besar, sehingga mengaburkan kinerja model pada kasus-kasus yang lebih sulit. Selain itu, perbaikan halus pada akurasi lokalisasi tidak selalu tercermin secara signifikan dalam perubahan nilai mAP.

Oleh karena itu, analisis kesalahan atau *error analysis* menjadi langkah kritis yang wajib dilakukan, terutama dalam konteks penelitian doktoral. Kita tidak boleh hanya berhenti pada angka agregat, melainkan harus membedah distribusi kesalahan secara sistematis:
- **Distribusi *false positive***: mengidentifikasi apakah kesalahan berasal dari ketidakakuratan batas kotak (*bounding box*), klasifikasi kategori yang salah, atau deteksi latar belakang yang keliru dianggap sebagai objek.
- **Distribusi *false negative***: menyoroti kegagalan mendeteksi objek berukuran kecil, objek yang tertutup sebagian (*occluded*), atau sampel dari domain yang secara inheren sulit diprediksi.
- **Evaluasi per subset ukuran objek**: memisahkan analisis menjadi kelompok kecil, sedang, dan besar untuk mengungkap bias arsitektural yang tersembunyi di balik rata-rata global.

Pendekatan evaluasi yang granular ini memungkinkan peneliti mengidentifikasi *bottleneck* spesifik. Misalnya, penurunan kinerja yang konsisten pada objek kecil mungkin mengindikasikan kebutuhan akan mekanisme *multi-scale feature fusion* yang lebih kuat, sementara dominasi kesalahan pada satu kategori tertentu dapat mengarah pada revisi strategi *sampling loss* atau augmentasi data berbasis domain. Hasil dari *error analysis* inilah yang biasanya menjadi dasar perumusan hipotesis penelitian dan penentuan arah modifikasi arsitektur.

Pemahaman mendalam tentang pola kesalahan ini akan menjadi fondasi penting sebelum kita masuk ke klasifikasi arsitektur detektor modern. Pada slide berikutnya, kita akan mengelompokkan berbagai pendekatan deteksi objek menjadi tiga kategori besar: *two-stage*, *one-stage*, dan *transformer-based*. Dari taksonomi tersebut, kita dapat melihat bagaimana masing-masing keluarga metode berusaha mengatasi tantangan lokalisasi dan klasifikasi yang telah kita bedah melalui analisis kesalahan tadi.

---

## Slide 012 - Taksonomi Detektor

### Narasi

Setelah membahas keterbatasan metrik mAP dan urgensi analisis kesalahan pada slide sebelumnya, langkah logis berikutnya adalah memahami kerangka taksonomi dari berbagai arsitektur deteksi objek. Klasifikasi ini bukan sekadar pengelompokan historis, melainkan fondasi strategis untuk memilih paradigma yang paling sesuai dengan karakteristik data, constraint komputasi, dan tujuan penelitian tingkat doktor Anda.

Pada tabel kategori besar, kita dapat membedakan tiga pendekatan utama yang mendominasi literatur terkini:
- **Two-stage**, diwakili oleh Faster R-CNN. Model ini bekerja secara bertahap: menghasilkan proposal wilayah (*region proposals*) terlebih dahulu, kemudian melakukan klasifikasi dan regresi bounding box pada wilayah tersebut. Pendekatan ini umumnya memberikan akurasi lokalisasi tinggi, namun beban komputasinya lebih besar karena melibatkan dua tahap terpisah dan pipeline yang kompleks.
- **One-stage**, diwakili oleh YOLO dan SSD. Model ini memprediksi bounding box dan kelas secara langsung dari peta fitur gambar tanpa melalui mekanisme proposal wilayah. Pendekatan ini mengutamakan kecepatan inferensi dan throughput, menjadikannya pilihan ideal untuk aplikasi real-time atau eksperimen yang memerlukan iterasi cepat pada dataset besar.
- **Transformer-based**, diwakili oleh DETR. Model ini mengubah formulasi deteksi objek menjadi masalah *set prediction*. Dengan memanfaatkan mekanisme *self-attention*, arsitektur ini mengevaluasi seluruh patch citra secara global sekaligus, sehingga menghilangkan ketergantungan pada komponen heuristik tradisional seperti Non-Maximum Suppression (NMS) dan manual anchor tuning.

Peta konsep yang ditampilkan dalam bentuk teks merepresentasikan hierarki arsitektur ini secara ringkas. Akar utamanya adalah "Detektor", yang bercabang ke tiga kelompok besar tersebut. Perlu dicatat bahwa di dalam setiap cabang terdapat evolusi metodologis yang signifikan. Misalnya, RetinaNet tetap diklasifikasikan sebagai *one-stage* meskipun memperkenalkan Focal Loss untuk mengatasi masalah ketidakseimbangan kelas ekstrem. Di sisi lain, Deformable DETR merepresentasikan perbaikan kritis atas DETR klasik dengan meningkatkan efisiensi komputasi dan mempercepat konvergensi pelatihan melalui deformable attention mechanism yang membatasi sampling titik pada area relevan saja.

Untuk pertemuan ini, fokus eksplorasi kita akan tertuju pada dua representatif utama dari dua paradigma berbeda: YOLO sebagai arsitektur *one-stage* yang dominan, dan DETR sebagai pionir serta standar baru untuk detektor berbasis *transformer*. Perbandingan ini penting karena keduanya menawarkan filosofi desain yang berlawanan namun saling melengkapi. YOLO mengandalkan ekstraksi fitur konvolusional lokal dan prediksi langsung, sedangkan DETR mengandalkan perhatian global dan pemetaan himpunan kandidat objek. Pemahaman mendalam terhadap perbedaan filosofis ini akan menentukan bagaimana Anda merancang abstraksi model, memilih komponen loss function, dan menafsirkan kegagalan model dalam penelitian disertasi Anda.

Memahami taksonomi ini juga menyiapkan landasan teoretis untuk diskusi teknis pada slide berikutnya mengenai pilihan arsitektural krusial: penggunaan **anchor** versus pendekatan **anchor-free**. Baik YOLO generasi terbaru maupun arsitektur transformer terkini banyak yang telah beralih sepenuhnya ke strategi *anchor-free*. Implikasi penelitian dari pergeseran ini sangat signifikan terhadap desain eksperimen, pemilihan hyperparameter, hingga interpretasi hasil evaluasi, yang akan kita bedah lebih lanjut setelah ini.

---

## Slide 013 - Anchor dan Anchor-Free

### Narasi

Pada slide sebelumnya, kita telah menguraikan taksonomi dasar detektor objek yang mencakup kategori two-stage, one-stage, hingga transformer-based. Peta konsep tersebut menunjukkan bahwa YOLO mewakili pendekatan one-stage, sementara DETR menandai pergeseran arsitektural menuju prediksi berbasis set attention. Untuk memahami alasan di balik evolusi arsitektur modern, kita perlu mendalami perbedaan mendasar antara mekanisme anchor-based dan anchor-free yang menjadi tulang punggung kedua kelompok tersebut.

Pendekatan anchor-based bekerja dengan menyiapkan sekumpulan kotak referensi yang telah ditentukan sebelumnya, mencakup berbagai skala dan rasio aspek. Model kemudian dilatih untuk memprediksi offset atau penyesuaian koordinat dari setiap anchor agar selaras dengan objek aktual dalam citra. Meskipun terbukti efektif pada era awal deep learning, metode ini membawa beban komputasi dan kompleksitas desain yang tinggi. Jumlah hyperparameter yang harus dikalibrasi sangat banyak, mulai dari densitas anchor per pixel, distribusi skala, hingga threshold IoU. Akibatnya, model cenderung kaku dan sulit beradaptasi ketika menghadapi objek dengan proporsi ekstrem atau bentuk yang tidak tercakup oleh himpunan anchor yang telah ditetapkan.

Sebagai alternatif, pendekatan anchor-free menghilangkan ketergantungan pada kotak referensi statis. Model dipandu untuk memrediksi parameter geometri objek secara langsung, seperti titik pusat, lebar-tinggi bounding box, atau sekumpulan keypoint representatif. Pendekatan ini menyederhanakan pipeline inference, mengurangi bias bawaan dari heuristik anchor, dan meningkatkan generalisasi terhadap variasi bentuk objek. Saat ini, hampir seluruh varian YOLO generasi terbaru serta sejumlah detektor transformer telah beralih sepenuhnya ke skema anchor-free karena konsistensi akurasi dan efisiensi training yang lebih baik.

Dari sudut pandang penelitian doktoral, keputusan memilih antara anchor-based dan anchor-free bukan sekadar konfigurasi teknis, melainkan fondasi desain eksperimen. Pilihan ini akan berdampak langsung pada formulasi loss function, strategi label assignment, mekanisme nms atau soft-nms, serta interpretasi metrik evaluasi seperti AP50, AP75, dan latency. Mahasiswa diharapkan mampu mengevaluasi kapan pendekatan anchor-free memberikan gain marginal yang signifikan, serta bagaimana mengontrol variabel confounding akibat perbedaan baseline deteksi saat membandingkan arsitektur baru dengan state-of-the-art.

Pembahasan mengenai pergeseran ke anchor-free ini akan menjadi konteks langsung untuk slide berikutnya, yang akan mengurai filosofi one-stage detection pada YOLO. Kita akan melihat bagaimana pembagian citra ke dalam grid, prediksi serentak dalam satu forward pass, dan integrasi dengan mekanisme anchor-free membentuk siklus evolusi arsitektur YOLO dari versi awal yang cepat namun kurang akurat, hingga mencapai keseimbangan optimal antara kecepatan real-time dan presisi deteksi objek kecil maupun tumpang tindih.

---

## Slide 014 - YOLO: Filosofi One-Stage Detection

### Narasi

Slide ini membahas filosofi fundamental dari arsitektur deteksi objek modern yang diwakili oleh keluarga YOLO, khususnya konsep *One-Stage Detection*. Berbeda dengan pendekatan dua tahap (*two-stage*) yang memisahkan proposal region dan klasifikasi, YOLO mengadopsi paradigma *You Only Look Once*. Implementasinya dapat dirinci sebagai berikut:
- Citra dibagi menjadi grid $S \times S$ secara deterministik.
- Setiap sel grid bertanggung jawab langsung untuk memprediksi bounding box, nilai kepercayaan (*confidence score*), dan distribusi probabilitas kelas.
- Seluruh prediksi dihasilkan melalui satu kali *forward pass*, menghilangkan kebutuhan tahap seleksi region terpisah.

Keunggulan utama dari filosofi ini terletak pada kecepatan inferensi yang sangat tinggi, menjadikannya standar de facto untuk aplikasi *real-time* seperti pemrosesan video, otonomi kendaraan, dan sistem monitoring cerdas. Namun, pada generasi awal, model ini menghadapi tantangan akurasi yang cukup signifikan. Keterbatasan kapasitas representasi dan mekanisme *label assignment* yang kaku menyebabkan performa menurun drastis saat menangani objek berukuran kecil atau scene dengan kepadatan tinggi. Objek yang saling tumpang tindih sering kali gagal dideteksi akibat konflik prediktif antar sel grid.

Dari perspektif penelitian tingkat doktoral, titik lemah ini justru menjadi pemicu utama inovasi metodologis. Evolusi YOLO merupakan respons sistematis terhadap batasan tersebut, yang termanifestasi dalam empat lini perbaikan utama:
1. Modifikasi arsitektur backbone dan neck untuk ekstraksi fitur multi-skala yang lebih robust.
2. Desain fungsi *loss* baru yang lebih sensitif terhadap ketidakseimbangan kelas dan kesulitan lokalisasi.
3. Penerapan teknik augmentasi data agresif, seperti mosaik, untuk meningkatkan generalisasi model.
4. Pergeseran mekanik deteksi dari pendekatan berbasis *anchor* menuju *anchor-free*, sesuai dengan diskusi pada slide sebelumnya.

Perubahan arsitektural dan strategi pelatihan ini secara bertahap menutup kesenjangan antara latensi dan akurasi, sekaligus mentransformasi YOLO dari prototipe akademis menjadi baseline industri yang matang. Untuk melacak bagaimana masing-masing inovasi tersebut diimplementasikan dan dievaluasi secara empiris, kita akan meninjau linimasa perkembangan versi YOLO pada slide berikutnya, lengkap dengan catatan kritis mengenai validitas perbandingan antar generasi dan pentingnya memeriksa konfigurasi pelatihan sebelum menarik kesimpulan riset.

---

## Slide 015 - Evolusi YOLO

### Narasi

Evolusi YOLO menunjukkan transformasi bertahap dari konsep deteksi berbasis grid awal menuju arsitektur yang sangat dioptimalkan untuk kecepatan dan akurasi. Sebagai peneliti, memahami kronologi ini membantu kita mengevaluasi mengapa setiap iterasi dipilih dalam konteks penelitian tertentu.

Berikut adalah ringkasan kontribusi kunci setiap versi:
- **YOLO v1**: Memperkenalkan deteksi one-stage berbasis grid yang memprediksi bounding box dan kelas dalam satu forward pass.
- **YOLO v2/v3**: Memperkenalkan anchor boxes, multi-scale prediction, dan feature pyramid untuk mengatasi kelemahan pada objek kecil dan tumpang tindih.
- **YOLO v4/v5**: Mengadopsi CSP backbone untuk efisiensi komputasi, augmentasi mosaik untuk diversifikasi data, serta berbagai training trick seperti focal loss.
- **YOLO v8**: Beralih ke arsitektur anchor-free, menyederhanakan pipeline inferensi, dan menyediakan ekosistem Ultralytics yang standar.
- **YOLO v9/v10**: Fokus pada efisiensi ekstrem, generalisasi perhatian, informasi gradien terprogram, serta pendekatan NMS-free untuk percepatan inference.

Dalam konteks penelitian tingkat doktoral, perbandingan kinerja antar versi ini memerlukan pendekatan metodologis yang ketat. Klaim peningkatan akurasi sering kali dipengaruhi oleh perbedaan konfigurasi pelatihan, hyperparameter, augmentasi, dan bahkan kode sumber yang tidak terbuka sepenuhnya. Oleh karena itu, validasi eksperimental harus dilakukan dengan baseline yang setara, lingkungan kontrol yang konsisten, dan metrik evaluasi yang transparan. Selalu merujuk pada dokumentasi resmi Ultralytics atau repository akademik terkait untuk memastikan reproduktibilitas hasil dan pemahaman mendalam terhadap implikasi teknis setiap perubahan arsitektural.

Pergeseran filosofis dari deteksi grid sederhana pada slide sebelumnya kini telah matang menjadi sistem modular yang terstruktur. Struktur pipeline tiga tahap ini akan kita uraikan lebih lanjut pada slide berikutnya, di mana kita akan melihat bagaimana backbone, neck, dan head berkolaborasi secara sinergis untuk menghasilkan prediksi deteksi yang presisi.

---

## Slide 016 - Pipeline YOLO Modern

### Narasi

Setelah membahas linimasa perkembangan arsitektur YOLO pada slide sebelumnya, kini kita akan membedah struktur internal yang menjadi fondasi performa deteksi modern. Pipeline YOLO saat ini mengikuti pola tiga komponen utama yang bekerja secara berurutan namun terintegrasi erat.

Komponen pertama adalah backbone, yang bertugas mengekstrak representasi visual dari gambar input. Pada implementasi modern seperti seri YOLOv8 hingga v10, backbone umumnya menggunakan arsitektur CSPDarknet. Desain ini mengoptimalkan aliran gradien dan mengurangi redundansi komputasi, sehingga ekstraksi fitur berjalan lebih efisien tanpa mengorbankan kapasitas model.

Hasil ekstraksi dari backbone kemudian diteruskan ke neck. Fungsi neck adalah menggabungkan fitur multi-skala melalui mekanisme seperti PAN-FPN. Di sinilah terjadi fusi informasi antara lapisan dalam yang kaya konteks semantik dan lapisan luar yang mempertahankan detail spasial. Output dari neck kemudian masuk ke head, yang bertanggung jawab langsung atas prediksi akhir. Head memisahkan tugas menjadi tiga jalur paralel: regresi koordinat bounding box, klasifikasi objek, dan estimasi confidence score.

Secara alur pemrosesan, data mengalir dari input menuju backbone, dilanjutkan ke neck untuk penyatuan skala fitur, dan akhirnya diurai oleh head menjadi tiga keluaran prediktif tersebut. Mekanisme multi-scale feature ini sangat krusial dalam konteks penelitian computer vision tingkat lanjut. Fitur resolusi tinggi menjaga akurasi pada objek berukuran kecil atau jauh, sementara fitur resolusi rendah memberikan konteks global yang diperlukan untuk mendeteksi objek besar atau memahami hubungan antarobjek dalam scene.

Pemahaman mendalam tentang pipeline ini menjadi prasyarat sebelum kita memasuki tahap praktis. Pada slide berikutnya, kita akan melihat bagaimana arsitektur ini diimplementasikan secara konkret melalui framework Ultralytics, mulai dari penyiapan struktur dataset, format anotasi, hingga eksekusi fine-tuning menggunakan bobot pre-trained COCO sebagai titik awal transfer learning.

---

## Slide 017 - Contoh Fine-tuning YOLO dengan Ultralytics

### Narasi

Merujuk pada pembahasan arsitektur pipeline YOLO pada slide sebelumnya yang mencakup backbone, neck, dan head, kini kita transisi ke aspek implementasi praktis. Slide ini menyajikan panduan teknis melakukan fine-tuning model YOLO menggunakan ekosistem Ultralytics, yang merupakan framework paling dominan dalam riset deteksi objek terkini.

Penyiapan dataset merupakan fondasi awal yang harus mengikuti struktur hierarkis ketat. Direktori dataset dibagi menjadi dua komponen utama: `images` untuk menyimpan file raster, dan `labels` untuk anotasi bounding box. Keduanya kembali dipisah menjadi subset `train` dan `val`. Pemisahan ini memungkinkan dataloader secara otomatis memisahkan data untuk optimisasi bobot dan monitoring overfitting selama siklus pelatihan.

Untuk anotasi, YOLO mengadopsi format normalisasi berbasis koordinat relatif. Setiap baris dalam file `.txt` berisi lima elemen: `class cx cy w h`. Nilai `cx` dan `cy` merepresentasikan pusat bounding box, sementara `w` dan `h` adalah dimensi lebarnya. Semua nilai diskalakan ke interval 0 hingga 1 berdasarkan resolusi citra asli. Skema ini meminimalkan bias geometri akibat perbedaan resolusi input dan mempercepat stabilitas numerik pada fungsi loss regression.

Berikut adalah implementasi training yang ditampilkan pada slide:

```python
from ultralytics import YOLO

model = YOLO("yolov8n.pt")
model.train(
    data="dataset.yaml",
    epochs=50,
    imgsz=640,
    batch=16,
    project="exp_deteksi"
)
```

Kode di atas mengilustrasikan kesederhanaan API Ultralytics. Baris pertama mengimpor modul inti, diikuti oleh inisialisasi model `YOLO("yolov8n.pt")` yang memuat arsitektur Nano YOLOv8 beserta bobot pralatih dari dataset COCO. Method `.train()` kemudian menerima serangkaian hiperparameter strategis: `data` mengarah ke file YAML yang memetakan path dataset dan definisi kelas; `epochs=50` menetapkan durasi pelatihan; `imgsz=640` menstandarisasi dimensi input; `batch=16` mengatur ukuran mini-batch untuk keseimbangan memori GPU; dan `project` menentukan direktori output untuk menyimpan checkpoint serta log eksperimen.

Penggunaan bobot `yolov8n.pt` secara eksplisit mengimplementasikan transfer learning. Alih-alih melatih dari inisialisasi acak, model memanfaatkan representasi fitur tingkat rendah dan menengah yang sudah matang dari miliaran parameter COCO. Fine-tuning hanya memerlukan penyesuaian ringan pada layer deteksi akhir dan adaptasi terhadap distribusi domain spesifik penelitian Anda. Strategi ini sangat krusial dalam konteks riset doktoral di mana efisiensi komputasi, reproducibility, dan generalisasi menjadi prioritas utama.

Setelah proses fine-tuning selesai dan model tersimpan sebagai `best.pt`, langkah kritis berikutnya adalah validasi rigor. Pada slide berikutnya, kita akan membahas prosedur perhitungan metrik evaluasi standar seperti mAP@0.5:0.95, serta teknik visualisasi prediksi untuk memastikan bahwa performa numerik konsisten dengan observasi empiris di lapangan.

---

## Slide 018 - Evaluasi dan Visualisasi Prediksi

### Narasi

Setelah proses fine-tuning YOLO selesai pada slide sebelumnya, langkah kritis berikutnya adalah mengevaluasi performa model secara kuantitatif dan kualitatif. Pada tahap ini, kita tidak hanya mengandalkan angka akurasi tunggal, melainkan metrik standar dalam object detection modern seperti mean Average Precision (mAP).

Untuk menghitung metrik evaluasi, kita memuat kembali model terbaik yang dihasilkan selama pelatihan, yaitu `best.pt`. Kemudian, kita memanggil fungsi validasi dengan mengarahkannya ke file konfigurasi dataset. Output dari fungsi ini merupakan objek komprehensif yang berisi berbagai statistik performa. Kita dapat mengakses `metrics.box.map` untuk melihat mAP pada IoU threshold 0.5 hingga 0.95, yang menjadi standar utama dalam benchmark seperti COCO. Selain itu, `metrics.box.map50` dan `metrics.box.map75` memberikan gambaran spesifik mengenai toleransi model terhadap ketidakakuratan lokalization pada threshold ketat maupun longgar.

```python
model = YOLO("best.pt")
metrics = model.val(data="dataset.yaml")
print(metrics.box.map)        # mAP@0.5:0.95
print(metrics.box.map50)      # mAP@0.5
print(metrics.box.map75)      # mAP@0.75
```

Namun, metrik numerik saja tidak cukup untuk menjamin robustness model di lapangan. Oleh karena itu, visualisasi prediksi menjadi komponen wajib. Dengan menggunakan `model.predict`, kita dapat menguji model pada sampel citra individual. Parameter `conf=0.25` mengatur ambang batas kepercayaan minimal agar hanya deteksi yang signifikan yang ditampilkan. Opsi `save=True` dan `save_txt=True` akan menyimpan hasil overlay bounding box beserta file label terformat, memudahkan audit manual.

```python
model.predict(
    source="gambar.jpg",
    conf=0.25,
    save=True,
    save_txt=True
)
```

Hasil eksekusi kode tersebut menghasilkan citra anotasi dan file teks yang mencatat koordinat serta kelas setiap objek terdeteksi. Penting untuk ditekankan bahwa inspeksi visual tetap menjadi praktik esensial dalam penelitian tingkat doktoral. Angka mAP yang tinggi bisa menutupi kegagalan sistematis pada kasus tertentu, seperti false positive pada latar belakang kompleks atau missed detection pada objek kecil. Analisis kesalahan melalui visualisasi inilah yang sering kali mengungkap research gap dan menjadi dasar perbaikan arsitektur atau strategi augmentasi data.

Evaluasi mendalam pada YOLO juga membuka perspektif baru mengenai keterbatasan pendekatan berbasis proposal dan non-maximum suppression (NMS) tradisional. Ketika kita menyoroti kebutuhan untuk memahami konteks global tanpa bergantung pada heuristik post-processing, transisi menuju arsitektur deteksi berbasis transformer menjadi sangat relevan. Hal ini secara natural mengarahkan kita pada pembahasan selanjutnya mengenai DETR, yang mereformulasi deteksi objek sebagai masalah prediksi himpunan langsung.

---

## Slide 019 - Transformer Detector: DETR

### Narasi

Pada slide sebelumnya, kita telah membahas evaluasi metrik dan visualisasi prediksi menggunakan arsitektur deteksi modern berbasis CNN seperti YOLO. Meskipun metode tersebut menawarkan latency rendah dan akurasi yang kompetitif, pendekatan tradisional ini masih memerlukan komponen post-processing eksplisit seperti anchor boxes, region proposal networks, dan Non-Maximum Suppression (NMS) untuk merapikan output. Pergeseran arsitektural signifikan terjadi ketika komunitas penelitian mulai mengadopsi mekanisme attention untuk tugas deteksi, dengan DETR menjadi fondasi utamanya.

DETR, atau Detection Transformer, mereformulasi deteksi objek sebagai masalah set prediction. Model ini tidak lagi menghasilkan ribuan kandidat bounding box yang harus disaring secara iteratif, melainkan memprediksi sekumpulan bounded box beserta kelasnya secara simultan dalam satu forward pass. Konsekuensi langsung dari formulasi ini adalah penghapusan total terhadap anchor, proposal generation, dan NMS, sehingga pipeline deteksi menjadi sepenuhnya end-to-end trainable dan lebih elegan secara desain sistem.

Arsitektur inti DETR tersusun atas empat blok fungsional yang saling terintegrasi. CNN backbone berfungsi sebagai ekstraktor fitur spasial awal. Transformer encoder kemudian memproses tensor fitur tersebut untuk membangun konteks global antar region, mengatasi batasan receptive field lokal yang inheren pada arsitektur CNN murni. Di sisi decoder, serangkaian vektor yang disebut object queries berinteraksi dengan representasi kontekstual dari encoder untuk menghasilkan prediksi akhir. Terakhir, perhitungan loss tidak dilakukan secara element-wise, melainkan melalui mekanisme bipartite matching yang mencocokkan prediksi model dengan ground truth secara optimal sebelum menghitung residual error.

Pendekatan set prediction ini menuntut pemahaman mendalam tentang bagaimana model menyetem korespondensi antara prediksi dan anotasi sebenarnya. Object queries bertindak sebagai slot prediktif yang dipelajari selama training, tanpa korespondensi statis terhadap lokasi atau skala objek di dalam citra. Sementara itu, bipartite matching memastikan bahwa setiap prediksi hanya bertanggung jawab atas satu ground truth, mencegah duplikasi deteksi secara matematis. Meskipun elegan, mekanisme ini dikenal memiliki laju konvergensi yang lebih lambat dibanding detector berbasis anchor, khususnya pada distribusi objek berukuran kecil. Pembahasan teknis mengenai karakteristik object queries, implementasi algoritma pencocokan optimal, serta strategi mitigasi bottleneck konvergensi akan kita bedah secara kritis pada slide berikutnya.

---

## Slide 020 - Object Query dan Bipartite Matching

### Narasi

Pada slide sebelumnya, kita telah menguraikan arsitektur dasar DETR yang mereformulasi deteksi objek sebagai masalah *set prediction*. Pendekatan ini secara fundamental menghilangkan ketergantungan pada komponen pipeline konvensional seperti *anchor boxes*, *region proposal networks*, hingga *Non-Maximum Suppression* (NMS). Fondasi mekanismenya bertumpu pada dua pilar utama yang akan kita bedah secara mendalam di slide ini: *Object Query* dan *Bipartite Matching*.

Mari kita fokus terlebih dahulu pada konsep *Object Query*. Model dilengkapi dengan sekumpulan vektor learnable yang jumlahnya tetap, umumnya ditetapkan antara 100 hingga 300 tergantung kompleksitas dataset. Setiap vektor query bertindak sebagai placeholder laten yang bertugas memprediksi satu objek spesifik. Yang perlu ditekankan adalah tidak adanya korespondensi statis antara indeks query tertentu dengan posisi spasial objek di dalam citra. Query-query ini bersifat global dan saling berinteraksi melalui mekanisme *cross-attention* pada decoder transformer, memungkinkan setiap query untuk secara dinamis mengagregasi konteks semantik dan geometris dari seluruh region fitur yang dihasilkan oleh backbone CNN.

Setelah decoder menghasilkan prediksi kelas dan koordinat bounding box untuk setiap query, proses perhitungan *loss* memerlukan strategi penugasan yang rigor. Di sinilah *Bipartite Matching* beroperasi. Diagram pada slide menggambarkan logika dasarnya: sejumlah prediksi dipetakan secara optimal ke *ground truth* tanpa duplikasi. Implementasinya menggunakan algoritma *Hungarian* untuk meminimalkan total biaya (*cost function*) yang merupakan kombinasi tertimbang dari entropi silang untuk klasifikasi dan jarak IoU/L1 untuk regresi kotak. Dengan mencocokkan prediksi ke target secara matematis optimal, mekanisme ini secara alami menyelesaikan masalah over-detection, sehingga NMS tidak lagi diperlukan.

Meskipun elegan secara teoretis, pendekatan ini membawa implikasi komputasi yang signifikan. Konvergensi model DETR terbukti lebih lambat dibandingkan detektor berbasis *anchor* generasi sebelumnya. Tantangan ini paling akut saat menangani objek berukuran kecil atau adegan dengan kepadatan objek tinggi. Decoder harus mengandalkan informasi awal dari *object queries* saja untuk mengkonvergensikan prediksi ke lokalisasi yang presisi, yang sering kali menuntut lebih banyak epoch pelatihan, scheduler learning rate yang lebih hati-hati, dan augmentasi data yang agresif untuk menstabilkan gradien.

Keterbatasan laju konvergensi dan sensitivitas terhadap skala objek inilah yang menjadi katalis utama bagi evolusi arsitektur DETR. Sebagaimana akan kita diskusikan di slide berikutnya, riset lanjutan merespons celah ini dengan memperkenalkan *deformable attention* yang membatasi sampling titik referensi, pemisahan eksplisit antara query posisi dan konten, serta teknik *contrastive denoising training* dan *look-forward scheme* pada detektor DINO. Penguasaan mendalam terhadap dinamika query dan matching bukan hanya penting untuk memahami literatur terkini, tetapi juga memberikan landasan analitis bagi mahasiswa doktoral dalam merumuskan hipotesis perbaikan efisiensi inference dan robustness pada skenario *real-world deployment*.

---

## Slide 021 - Perkembangan Deformable DETR dan Detector DINO

### Narasi

Setelah memahami konsep object query dan mekanisme bipartite matching pada slide sebelumnya, kita kini membahas bagaimana arsitektur DETR awal disempurnakan untuk mengatasi keterbatasan konvergensi yang lambat dan performa yang masih lemah pada objek berukuran kecil. Tabel pada slide ini merangkum tiga evolusi metodologis yang menjadi fondasi detector berbasis transformer modern.

Berikut adalah inti perbaikan dari masing-masing metode:
1. **Deformable DETR** memperkenalkan mekanisme attention yang selektif. Alih-alih menghitung attention terhadap seluruh feature map resolusi penuh, model hanya melakukan sampling titik di sekitar referensi region. Pendekatan ini secara drastis mengurangi kompleksitas komputasi dan mempercepat proses pembelajaran tanpa mengorbankan cakupan konteks global.
2. **Conditional DETR** memisahkan representasi query menjadi dua komponen independen: posisi dan konten. Pemisahan ini memungkinkan decoder untuk lebih fokus pada prediksi koordinat bounding box yang presisi sambil menjaga konsistensi semantik dari setiap object query.
3. **DINO (detector)** mengintegrasikan tiga inovasi sekaligus: *contrastive denoising training* untuk menstabilkan fase awal pelatihan, *query selection mechanism* yang menyaring query paling relevan, serta *look-forward scheme* yang memanfaatkan estimasi dari iterasi sebelumnya guna meningkatkan akurasi bounding box secara bertahap.

Perlu dicatat secara eksplisit bahwa DINO yang dibahas dalam konteks object detection ini berbeda total dengan DINO versi self-supervised learning yang telah kita telaah pada pertemuan keempat. Keduanya berbagi nama karena inspirasi arsitektural encoder yang serupa, namun head deteksi, fungsi loss, dan protokol trainingnya tidak dapat disamakan. Dalam penulisan paper atau diskusi riset tingkat doktor, selalu nyatakan varian dan konfigurasi spesifik saat melakukan perbandingan metodologi untuk menghindari ambiguitas teknis dan kesalahan replikasi eksperimen.

Evolusi ini membuka jalan bagi diskusi lebih lanjut mengenai trade-off praktis antara pendekatan one-stage seperti YOLO dan family transformer-based. Pada slide berikutnya, kita akan membandingkan kedua paradigma tersebut secara sistematis berdasarkan aspek kecepatan inference, akurasi pada objek kecil, serta kebutuhan komponen manual seperti FPN atau NMS. Perbandingan ini akan membantu Anda menentukan strategi pemilihan model yang tepat sesuai arah penelitian, batasan komputasi, dan target publikasi yang dituju.

---

## Slide 022 - Perbandingan One-Stage dan Transformer Detector

### Narasi

Slide ini menyajikan evaluasi komparatif sistematis antara dua arus utama dalam object detection modern: pendekatan one-stage berbasis grid atau anchor-free seperti seri YOLO, versus detektor berbasis transformer seperti keluarga DETR. Perbedaan fundamental terletak pada formulasi masalah dan mekanisme prediksi. YOLO memetakan fitur spasial ke grid atau menggunakan query bebas anchor, lalu melakukan regresi langsung untuk koordinat bounding box dan distribusi kelas. DETR, sebaliknya, merumuskan deteksi sebagai masalah set prediction, di mana decoder transformer secara iteratif menghasilkan sekumpulan query yang kemudian dipadankan dengan ground truth melalui bipartite matching.

Dari perspektif infrastruktur arsitektural, YOLO masih mengandalkan modul manual seperti Feature Pyramid Network (FPN) dan Non-Maximum Suppression (NMS) pada sebagian besar versinya. DETR dirancang lebih minimalis karena mekanisme matching satu-satu menghilangkan kebutuhan akan NMS. Konsekuensi langsungnya terlihat pada karakteristik inferensi: YOLO mempertahankan keunggulan latency dan throughput, menjadikannya standar industri untuk sistem real-time. DETR secara inheren lebih lambat, meskipun arsitektur turunan seperti Deformable DETR telah berupaya mengurangi overhead komputasi melalui sampling points adaptif.

Performa pada objek kecil dan stabilitas pelatihan menjadi pembeda krusial lainnya. Setelah evolusi ke YOLO v8 dan varian selanjutnya, akurasi pada skala kecil meningkat signifikan berkat desain neck dan head yang lebih matang. DETR klasik memang rentan terhadap false negative pada objek kecil akibat resolusi feature map yang terbatas dan ketergantungan pada global attention. Namun, perbaikan seperti Deformable DETR telah menutup celah ini secara substansial. Terkait konvergensi, YOLO menunjukkan kurva loss yang relatif stabil dengan hyperparameter standar. DETR memerlukan strategi pelatihan khusus, termasuk auxiliary losses, denoising training, dan warm-up schedule yang lebih panjang, agar matcher bipartite dapat beroperasi optimal sejak awal epoch.

Implikasi praktis dari perbandingan ini sangat relevan untuk konteks penelitian tingkat doktoral:
- Pilihan arsitektur harus dikaitkan secara eksplisit dengan constraint aplikasi dan tujuan riset. Fokus pada deployment edge atau low-latency system mengarahkan ke YOLO, sedangkan eksplorasi representasi kontekstual jangka panjang atau integrasi multimodal lebih cocok dieksplorasi lewat DETR.
- Setiap klaim kinerja harus divalidasi dengan kontrol ketat terhadap jumlah parameter, FLOPs, dan protokol pelatihan. Tanpa normalisasi metrik komputasi, perbandingan menjadi bias dan sulit direproduksi.
- Posisi riset Anda terhadap state-of-the-art ditentukan oleh bagaimana Anda memanfaatkan kelebihan masing-masing paradigma, misalnya menggabungkan efisiensi YOLO dengan mekanisme attention transformer, atau mengintegrasikan backbone self-supervised ke dalam pipeline DETR.

Pembahasan ini menjadi fondasi logis menuju materi berikutnya tentang transfer learning dan fine-tuning. Mengingat biaya anotasi bounding box yang sangat tinggi, pemanfaatan bobot pretrained—baik dari dataset masif seperti COCO maupun representasi visual dari backbone CNN, ViT, hingga model self-supervised seperti DINOv2—merupakan praktik wajib. Slide selanjutnya akan membahas strategi pembekuan layer, penyesuaian learning rate, augmentasi data, serta dokumentasi skema fine-tuning yang memastikan detektor modern dapat diadaptasi secara efisien ke domain target tanpa kehilangan kapasitas generalisasi awal.

---

## Slide 023 - Transfer Learning dan Fine-tuning Detector

### Narasi

Setelah membahas perbandingan paradigma deteksi satu tahap versus berbasis transformer pada slide sebelumnya, kita kini beralih ke aspek implementasi yang menentukan keberhasilan adaptasi model pada masalah nyata: transfer learning dan fine-tuning detector. Pada jenjang penelitian doktoral, melatih model deteksi dari awal hampir selalu tidak efisien mengingat kompleksitas loss landscape dan kebutuhan komputasi yang masif.

Alasan fundamental penerapan transfer learning terletak pada mahalnya proses anotasi bounding box. Anotasi yang akurat memerlukan domain expertise dan konsistensi inter-annotator yang sulit dipertahankan pada skala besar. Bobot pretrained pada dataset seperti COCO menyediakan representasi visual awal yang sudah matang, mampu mengekstrak fitur hierarkis mulai dari tepi geometris hingga struktur semantik objek. Fine-tuning kemudian berfungsi sebagai mekanisme penyesuaian representasi tersebut agar selaras dengan distribusi data dan prior domain target yang lebih spesifik.

Strategi fine-tuning harus dirancang secara sistematis, terutama ketika ketersediaan data terbatas. Pada epoch awal, membekukan lapisan backbone mencegah degradasi fitur dasar dan mengurangi risiko overfitting terhadap noise dataset kecil. Pendekatan ini sebaiknya dikombinasikan dengan augmentasi data yang mereplikasi variasi kondisi capture target, serta penggunaan learning rate yang konservatif untuk memastikan stabilitas konvergensi. Dokumentasi transparan mengenai asal bobot pretrained, protokol freezing, schedule optimizer, dan konfigurasi hardware wajib dicatat demi reproduktibilitas dan auditabilitas hasil riset.

Keterkaitan materi ini merujuk kembali pada pembahasan pertemuan tiga dan empat mengenai konstruksi backbone. Arsitektur deteksi modern tidak lagi bergantung exclusively pada CNN klasik atau ViT vanilla. Representasi yang dihasilkan oleh framework self-supervised learning seperti DINOv2, maupun model vision-language seperti CLIP, dapat diintegrasikan sebagai encoder dalam pipeline deteksi. Integrasi ini memungkinkan pemanfaatan knowledge transfer lintas tugas dan meningkatkan robustness model pada domain dengan label sparse atau distribusi kelas imbalanced.

Penerapan strategi fine-tuning ini secara langsung bergantung pada karakteristik data yang digunakan. Oleh karena itu, pada slide berikutnya kita akan mengulas benchmark standar serta dataset domain-spesifik, termasuk bagaimana heterogenitas jumlah kelas, rasio aspek, dan skala objek memengaruhi pemilihan metrik evaluasi serta desain eksperimental dalam konteks pengembangan metodologi deteksi tingkat lanjut.

---

## Slide 024 - Dataset dan Benchmark Deteksi

### Narasi

Melanjutkan pembahasan strategi transfer learning dan fine-tuning detector pada slide sebelumnya, pemilihan dataset yang tepat menjadi fondasi kritis dalam merancang eksperimen deteksi objek yang valid dan bermakna. Pada jenjang doktoral, pemahaman mendalam terhadap ekosistem benchmark tidak hanya bersifat teknis, tetapi juga strategis untuk memposisikan novelty riset terhadap state-of-the-art dan menghindari evaluasinya yang bias.

Benchmark standar saat ini didominasi oleh beberapa dataset yang telah menjadi acuan komunitas penelitian:
- **COCO**: Standar de facto dengan 80 kelas objek, menonjol karena variasi ukuran, aspek rasio, dan tingkat oklusi yang tinggi. Sangat cocok untuk menguji generalisasi model pada kondisi kompleks dan dinamis.
- **PASCAL VOC**: Benchmark historis dengan 20 kelas, sering dipertahankan dalam studi longitudinal untuk melacak evolusi performa arsitektur dari tahun ke tahun.
- **Open Images**: Menawarkan cakupan label yang masif dan anotasi dalam skala besar, sangat relevan untuk mengeksplorasi long-tail distribution, multi-label detection, atau fine-grained classification.

Di luar benchmark umum, penelitian terapan dan akademis sering kali memerlukan dataset domain-spesifik. Bidang seperti pencitraan medis, analisis citra satelit, sistem kendaraan otonom, dan inspeksi industri memiliki karakteristik data yang sangat unik. Distribusi ukuran objek, densitas anotasi, dan hierarki kelas pada dataset-domain ini umumnya menyimpang jauh dari pola COCO. Akibatnya, metrik evaluasi konvensional seperti mAP atau IoU perlu dikalibrasi atau dilengkapi dengan metrik tambahan agar mampu menangkap nuansa performa yang sesungguhnya sesuai konteks domain.

Karakteristik dataset yang telah diidentifikasi ini akan langsung menjadi input utama dalam menyusun protokol eksperimen fine-tuning yang akan kita bahas pada slide berikutnya. Ketika struktur data, pembagian kelas, dan tantangan domain telah jelas, langkah selanjutnya adalah mendefinisikan train/val/test split, skema augmentasi, konfigurasi optimizer, serta baseline yang fair. Transparansi dalam dokumentasi seed, versi library, dan environment akan memastikan bahwa setiap hasil eksperimen dapat direproduksi, diverifikasi, dan dijadikan landasan metodologis yang kuat untuk pengembangan proposal disertasi.

---

## Slide 025 - Protokol Eksperimen Fine-tuning Detector

### Narasi

Setelah membahas karakteristik dataset dan benchmark standar pada slide sebelumnya, langkah kritis berikutnya adalah merancang protokol eksperimen yang ketat untuk melakukan fine-tuning pada detector modern. Pada jenjang doktoral, protokol ini bukan sekadar prosedur teknis, melainkan fondasi validitas ilmiah yang menentukan apakah temuan penelitian Anda dapat dipertanggungjawabkan secara akademis.

Protokol fine-tuning yang komprehensif harus mencakup empat elemen utama:
- **Pembagian dataset**: Split train/val/test harus menerapkan stratifikasi jika distribusi kelas tidak merata, guna mencegah bias sampling yang mendistorsi evaluasi.
- **Preprocessing dan augmentasi**: Teknik seperti resize, mosaic, horizontal flip, dan color jitter wajib dikonfigurasikan sesuai karakteristik domain untuk meningkatkan generalisasi model.
- **Konfigurasi pelatihan**: Optimizer, learning rate schedule, batch size, jumlah epoch, dan random seed harus ditetapkan secara eksplisit sebelum eksekusi.
- **Penetapan baseline**: Gunakan model pretrained resmi dari framework yang dipilih, serta siapkan metode pembanding yang fair dan relevan dengan state-of-the-art terkini.

Sebagai implementasi praktis dalam ekosistem YOLO atau framework deteksi berbasis YAML, struktur konfigurasi dataset biasanya dituliskan sebagai berikut:
```yaml
train: dataset/images/train
val: dataset/images/val
nc: 2
names: ["objek_a", "objek_b"]
```
Parameter `nc` menyatakan jumlah kelas target, sementara `names` memetakan indeks numerik ke label semantik yang konsisten. Struktur ini menjamin pipeline loading data berjalan deterministik dan memudahkan integrasi dengan script training otomatis.

Aspek paling vital dalam penelitian tingkat S3 adalah reproduksibilitas. Seluruh variabel acak, versi library, dependensi lingkungan, dan konfigurasi hyperparameter harus didokumentasikan secara lengkap. Integrasi dengan platform pencatatan eksperimen seperti Weights & Biases, MLflow, atau TensorBoard sangat disarankan untuk melacak setiap iterasi secara transparan. Dokumentasi ini tidak hanya mempermudah proses audit metodologi, tetapi juga memperkuat posisi Anda saat menyusun paper atau proposal disertasi.

Dengan protokol yang terstandarisasi dan terekam dengan sistematis, kita siap memasuki tahap interpretasi hasil. Slide berikutnya akan membahas confusion analysis untuk mengurai jenis-jenis kesalahan deteksi, sehingga evaluasi tidak lagi bergantung semata pada metrik agregat seperti mAP, melainkan mampu mengidentifikasi apakah masalah utama terletak pada lokalisasi, klasifikasi, false positive, atau false negative.

---

## Slide 026 - Confusion Analysis: Jenis Kesalahan Deteksi

### Narasi

Setelah menetapkan protokol eksperimen yang ketat pada tahap fine-tuning, langkah kritis berikutnya adalah memahami di mana dan mengapa model gagal melakukan prediksi. Evaluasi berbasis metrik agregat saja tidak cukup untuk mengarahkan perbaikan arsitektur atau strategi pelatihan secara efektif. Oleh karena itu, kita perlu melakukan analisis kebingungan atau *confusion analysis* untuk mengurai jenis kesalahan deteksi secara granular.

Secara umum, kesalahan dalam deteksi objek dapat dikategorikan menjadi empat tipe utama:
- **Kesalahan Lokalisasi**: Model berhasil mengidentifikasi kelas objek dengan benar, namun batas kotak (*bounding box*) yang diprediksi tidak tumpang tindih cukup baik dengan anotasi *ground truth*. Biasanya, nilai *Intersection over Union* (IoU) berada di bawah ambang batas yang ditetapkan.
- **Kesalahan Klasifikasi**: Posisi deteksi sudah tepat, tetapi model memberikan label kelas yang salah. Ini sering kali disebabkan oleh kemiripan visual antar-kelas atau kurangnya representasi fitur diskriminatif pada lapisan akhir.
- **False Positive Background**: Model memprediksi adanya objek di area latar belakang yang sebenarnya tidak mengandung objek target. Hal ini biasanya terkait dengan ambang batas skor kepercayaan (*confidence threshold*) yang terlalu longgar atau respons berlebihan terhadap pola latar yang menyesatkan.
- **False Negative**: Objek aktual dalam gambar tidak terdeteksi sama sekali. Kegagalan ini umumnya mengindikasikan masalah pada mekanisme *recall*, seperti fitur objek yang terlalu lemah, tertutup oklusi, atau hilang akibat augmentasi data yang agresif.

Tujuan utama dari penguraian ini adalah menentukan prioritas perbaikan secara strategis. Alih-alih hanya terpaku pada nilai mean Average Precision (mAP) yang tampak tinggi, analisis error memberikan penjelasan kausal mengapa performa belum optimal. Apakah kita perlu menyesuaikan desain *anchor*, memperdalam *feature pyramid network*, menyetel ulang fungsi loss, atau memperbaiki distribusi kelas dalam dataset? Jawaban atas pertanyaan ini akan membentuk dasar evaluasi empiris yang lebih mendalam dan berorientasi pada kontribusi metodologis.

Dengan pemahaman tentang kategori kesalahan ini, kita dapat melanjutkan ke dimensi analisis yang lebih spesifik, yaitu pengaruh ukuran objek terhadap kinerja detektor. Pada slide berikutnya, kita akan melihat bagaimana kesalahan tersebut terdistribusi berdasarkan skala kecil, sedang, dan besar, serta implikasinya terhadap desain riset, pemilihan teknik augmentasi multi-scale, dan potensi penyusunan hipotesis penelitian baru.

---

## Slide 027 - Analisis Kesalahan Berdasarkan Ukuran Objek

### Narasi

Setelah pada slide sebelumnya kita membedah confusion analysis untuk mengidentifikasi apakah kesalahan deteksi lebih dominan pada aspek lokalisasi atau klasifikasi, langkah analitis selanjutnya adalah menelaah bagaimana performa model bervariasi terhadap dimensi fisik objek dalam gambar. Fokus kita kini beralih pada analisis kesalahan berdasarkan ukuran objek, sebuah metrik evaluasi yang menjadi standar de facto dalam benchmark seperti COCO dan sangat relevan untuk menilai robustness arsitektur deteksi modern.

Dalam protokol evaluasi COCO, objek dikelompokkan menjadi tiga subset berdasarkan luas area bounding box dalam piksel. Objek kecil didefinisikan memiliki luas kurang dari 32x32 piksel. Objek menengah berada pada rentang 32x32 hingga kurang dari 96x96 piksel. Sementara itu, objek besar mencakup semua bounding box dengan luas 96x96 piksel atau lebih. Pembagian ini bukan sekadar kategorisasi administratif, melainkan cerminan langsung dari tantangan representasi fitur pada tahap feature extraction dan neck network.

Pola empiris yang konsisten dilaporkan dalam literatur terkini menunjukkan bahwa objek kecil cenderung memicu false negative yang signifikan. Hal ini disebabkan oleh proses downsampling berulang pada backbone, yang sering kali mendilusi sinyal spasial halus dari target berukuran kecil hingga hilang atau menyerupai noise tekstur latar belakang. Sebaliknya, model umumnya mempertahankan recall dan precision yang jauh lebih stabil untuk objek berukuran besar. Untuk memitigasi bias skala ini, strategi augmentasi multi-scale serta desain loss function yang memberikan penalti atau bobot lebih pada area kecil telah terbukti efektif meningkatkan sensitivitas model terhadap target minoritas secara geometris.

Dari sudut pandang penelitian tingkat doktoral, pertanyaan riset kritis yang harus Anda rumuskan adalah pada kombinasi kelas dan ukuran apa kegagalan deteksi paling terkonsentrasi. Apakah kegagalan tersebut bersifat sistematis pada kategori semantik tertentu, atau murni akibat keterbatasan resolusi input dan kapasitas receptive field? Jawaban atas pertanyaan ini akan menentukan arah optimasi arsitektur, pemilihan anchor strategy, maupun modifikasi modul attention mechanism dalam pipeline Anda.

Ketika peta kegagalan sudah terpetakan berdasarkan dimensi objek, analisis ini secara inheren beririsan dengan distribusi dataset. Hal ini membawa kita secara natural ke pembahasan ketidakseimbangan kelas pada slide berikutnya, di mana ketimpangan jumlah sampel dapat memperburuk performa model pada objek kecil maupun kelas langka. Evaluasi per-kelas dan teknik mitigasi seperti re-weighting loss, oversampling strategis, atau generasi data sintetis akan menjadi komponen wajib dalam experimental design untuk menjamin generalisasi yang adil dan reproducible.

---

## Slide 028 - Ketidakseimbangan Kelas

### Narasi

Setelah kita mengidentifikasi pola kegagalan deteksi berdasarkan rentang ukuran objek pada slide sebelumnya, fokus analitis kita beralih ke faktor struktural lain yang sering menggerus performa model: ketidakseimbangan kelas atau *class imbalance*. Pada dataset dunia nyata maupun benchmark standar, distribusi objek jarang bersifat homogen. Ketika satu kategori mendominasi jumlah anotasi, mekanisme optimisasi pada YOLO maupun backbone transformer cenderung memprioritaskan fitur mayoritas, sehingga representasi kelas minoritas tidak terkonvergensi secara optimal.

Dampaknya tercermin jelas pada metrik evaluasi. Nilai mAP agregat dapat menutupi degradasi signifikan pada kelas langka. Jika kita menetapkan satu *confidence threshold* seragam untuk tahap deployment, ambang batas tersebut secara inheren akan memberatkan kelas minoritas karena distribusi skor kepercayaannya memang lebih rendah. Dalam praktik penelitian tingkat doktoral, laporan evaluasi per kelas bukan opsional, melainkan syarat metodologis untuk memastikan validitas eksternal hasil eksperimen.

Perhatikan contoh matriks analisis ini sebagai ilustrasi empiris. Kelas kendaraan dengan lima puluh ribu sampel mencapai AP 0,72, mencerminkan konsistensi deteksi yang baik. Sebaliknya, helm turun ke 0,41 dengan tiga ribu sampel, sementara pejalan kaki malam anjlok ke 0,12 akibat kelangkaan data yang ekstrem. Angka-angka ini menegaskan korelasi langsung antara kepadatan training set dan kapasitas model dalam menangkap variasi intrinsik tiap kategori.

Untuk memitigasi bias ini, strategi intervensi dapat diintegrasikan ke dalam desain eksperimen. *Loss re-weighting* memungkinkan fungsi kerugian memberi bobot lebih tinggi pada kesalahan prediksi kelas minoritas selama backpropagation. Teknik *oversampling* juga efektif, terutama ketika dipadukan dengan augmentasi domain-spesifik yang menjaga integritas geometri dan tekstur. Pada ranah riset mutakhir, pemanfaatan data sintetis melalui model generatif atau teknik *mixup* antar-kelas menjadi pendekatan yang semakin kredibel untuk menyeimbangkan distribusi tanpa mengorbankan kualitas anotasi.

Penyelesaian masalah ketidakseimbangan kelas harus diselesaikan sebelum kita menentukan parameter inference akhir. Setelah distribusi pelatihan stabil, perhatian kita perlu dialihkan ke pengaturan *confidence threshold*, kalibrasi skor kepercayaan, serta pertukaran sistematis antara presisi dan rekall yang akan kita uraikan secara kuantitatif pada slide berikutnya.

---

## Slide 029 - Threshold, Kalibrasi Confidence, dan Trade-off

### Narasi

Pada slide ini, kita membahas aspek krusial dalam tahap deploy model deteksi objek modern, yaitu pemilihan *confidence threshold*, kalibrasi skor kepercayaan, serta *trade-off* yang muncul di antara metrik evaluasi. Setelah pada slide sebelumnya kita menyoroti dampak ketidakseimbangan kelas terhadap performa deteksi per kategori, langkah selanjutnya adalah menentukan ambang batas keputusan yang adil dan efektif secara operasional.

Pemilihan *confidence threshold* secara langsung memengaruhi keseimbangan antara presisi dan *recall*. Jika kita menurunkan ambang batas, model akan cenderung mendeteksi lebih banyak objek positif, sehingga *recall* meningkat. Namun, hal ini juga meningkatkan jumlah *false positive*, yang berakibat turunnya presisi. Sebaliknya, menaikkan ambang batas akan menyaring prediksi hanya pada skor tinggi, meningkatkan presisi, tetapi mengorbankan *recall* karena banyak objek aktual terlewat. Keputusan ini tidak bersifat mutlak; ia sangat bergantung pada biaya kesalahan (*cost of error*) dalam konteks aplikasi spesifik. Sistem yang menuntut akurasi tinggi biasanya memilih threshold lebih ketat, sedangkan sistem yang mengutamakan cakupan deteksi akan menerima presisi lebih rendah demi *recall* yang maksimal.

Ilustrasi pada tabel menunjukkan bagaimana perubahan nilai threshold mengubah distribusi presisi dan *recall*. Pada threshold 0,1, model mencapai *recall* sebesar 0,90 dengan presisi 0,55. Saat threshold dinaikkan menjadi 0,25, terjadi kompromi di mana kedua metrik berada di kisaran 0,70 hingga 0,78. Pada threshold 0,5, presisi melonjak menjadi 0,84, namun *recall* turun signifikan ke 0,61. Pola ini menegaskan bahwa tidak ada satu nilai universal yang optimal; penentuan threshold harus dilakukan melalui analisis kurva Precision-Recall, grid search, atau optimisasi F1-score, disesuaikan dengan prioritas sistem dan toleransi kesalahan.

Perlu dicatat bahwa metrik standar seperti mAP dihitung secara *threshold-agnostic* dengan mengintegrasikan seluruh kurva PR. Namun, dalam implementasi nyata, sistem membutuhkan satu titik keputusan tunggal untuk menghasilkan bounding box final. Di sinilah kalibrasi confidence menjadi penting. Skor keluaran dari model deteksi modern sering kali tidak merepresentasikan probabilitas sebenarnya akibat masalah overconfidence atau underconfidence. Tanpa kalibrasi yang tepat, threshold yang dipilih bisa menyesatkan. Teknik seperti temperature scaling, Platt scaling, atau post-processing berbasis histogram dapat diterapkan untuk memastikan bahwa skor kepercayaan benar-benar mencerminkan kemungkinan keberadaan objek, sehingga keputusan threshold tetap valid dan interpretable saat dipindahkan ke lingkungan produksi.

Pembahasan mengenai threshold dan kalibrasi ini menjadi fondasi sebelum kita mengevaluasi ketahanan model terhadap variasi kondisi dunia nyata. Ketika model sudah distandarkan dengan threshold yang tepat, tantangan berikutnya adalah memastikan bahwa performa tersebut tetap stabil meskipun terjadi pergeseran distribusi data antar lingkungan. Hal ini akan kita bahas secara mendalam pada slide berikutnya terkait *domain shift* dan strategi membangun *robustness* dalam arsitektur deteksi objek terkini.

---

## Slide 030 - Domain Shift dan Robustness

### Narasi

Setelah membahas trade-off antara precision dan recall serta pentingnya kalibrasi confidence pada slide sebelumnya, kita kini beralih ke aspek fundamental lain yang menentukan keberhasilan deployment model deteksi objek di dunia nyata: domain shift dan robustness. Pada tingkat penelitian doktor, memahami bagaimana performa model berubah ketika distribusi data pelatihan dan pengujian berbeda bukanlah hal sekunder, melainkan inti dari evaluasi metodologi yang rigor.

Domain shift terjadi ketika model yang telah dilatih pada satu domain tertentu dievaluasi atau diterapkan pada domain yang memiliki karakteristik statistik atau visual berbeda. Contoh klasik meliputi pergeseran kondisi pencahayaan dari siang ke malam, perbedaan perspektif pencitraan seperti citra satelit versus drone, maupun kesenjangan antara data sintetis yang dikontrol ketat dengan data riil yang penuh noise. Pergeseran ini dapat disebabkan oleh perubahan sensor, lingkungan, gaya rendering, atau bahkan bias sistematis dalam protokol pengumpulan data.

Untuk mengukur robustness secara kuantitatif, desain eksperimen harus mencakup test set yang sengaja mewakili variasi domain tersebut. Langkah-langkah eksperimental yang disarankan meliputi:
- Memisahkan dan melaporkan penurunan mAP per domain, bukan hanya mengandalkan metrik agregat global.
- Melakukan analisis error case dengan memvisualisasikan prediksi yang gagal untuk mengidentifikasi akar masalah secara visual.
- Mengevaluasi stabilitas skor confidence lintas domain guna memastikan bahwa kalibrasi tetap valid meskipun terjadi pergeseran distribusi data.

Pertanyaan kunci yang perlu dijawab dalam setiap kajian literatur dan eksperimen adalah apakah detector benar-benar memahami konsep objek secara umum, atau sekadar menghafal pola latar belakang dan asosiasi kontekstual yang spesifik pada data pelatihan. Di level S3, respons terhadap pertanyaan ini sering kali mengarah pada identifikasi research gap, misalnya dengan mengeksplorasi teknik self-supervised learning seperti DINOv2 untuk ekstraksi fitur yang lebih invariant terhadap domain, atau merancang mekanisme attention yang mengurangi ketergantungan pada background bias.

Pembahasan mengenai domain shift ini juga membuka jalan menuju tantangan praktis berikutnya: kualitas anotasi. Seperti yang akan kita bahas pada slide selanjutnya, bahkan dengan model yang robust secara arsitektural, ketidaksempurnaan ground truth—mulai dari bounding box yang kurang presisi hingga label yang tidak konsisten—dapat secara signifikan mendistorsi metrik evaluasi dan menutupi potensi sebenarnya dari sebuah metode. Oleh karena itu, audit kualitas anotasi dan transparansi protokol labeling menjadi prasyarat mutlak sebelum mengklaim kontribusi ilmiah baru, sekaligus menjadi konteks penting sebelum kita memanfaatkan tools seperti SAM pada pertemuan berikutnya untuk otomatisasi anotasi mask.

---

## Slide 031 - Kualitas Anotasi

### Narasi

Kita beralih dari pembahasan domain shift ke aspek fundamental yang sering kali menjadi sumber variasi performa dalam deteksi objek modern, yaitu kualitas anotasi data. Meskipun arsitektur seperti YOLO atau transformer mampu menangkap representasi yang sangat kompleks, kinerja akhir model tetap sangat bergantung pada bagaimana data pelatihan dan evaluasi diannotasi. Ketidaksempurnaan anotasi bukan sekadar masalah teknis minor, melainkan bias sistematis yang dapat mengaburkan interpretasi hasil eksperimen, terutama ketika kita mengevaluasi robustness lintas domain seperti yang telah kita diskusikan sebelumnya.

Anotasi dalam dataset nyata jarang sekali sempurna. Bounding box sering kali kurang presisi terhadap batas objek sebenarnya, label dapat mengandung inkonsistensi atau kesalahan kategorisasi, dan objek penting terkadang terlewat sehingga muncul sebagai false positive saat model diuji. Dampaknya signifikan: noise pada ground truth memaksa model belajar pola yang tidak relevan, menurunkan generalisasi, dan membuat metrik seperti mAP sulit diinterpretasikan secara objektif. Pada tingkat penelitian doktoral, hal ini menuntut kita untuk tidak hanya melaporkan angka, tetapi juga mengaudit distribusi error yang bersumber dari data itu sendiri.

Untuk menjaga integritas eksperimen, praktik penelitian yang baik harus diterapkan secara disiplin:
- Lakukan audit kualitas anotasi secara eksplisit, termasuk pengukuran inter-annotator agreement, konsistensi label antar kelas, dan validasi area bounding box.
- Selalu pelajari dokumentasi protokol labeling sebelum menggunakan dataset publik, karena setiap koleksi data memiliki konvensi annotasi yang berbeda.
- Jangan mengubah ground truth secara sembarangan; jika koreksi diperlukan, pastikan setiap perubahan didokumentasikan dengan alasan metodologis yang jelas agar hasil penelitian tetap reproducible.

Penting juga untuk melihat hubungan antara kualitas anotasi dengan perkembangan alat anotasi otomatis. Seperti yang akan kita bahas lebih lanjut pada pertemuan 08, model segmentasi seperti SAM dapat mempercepat pembuatan anotasi berbasis mask, namun outputnya tetap memerlukan validasi manusia karena rentan terhadap artefak pada objek kompleks atau tekstur halus. Validasi ini justru menjadi jembatan menuju topik berikutnya, yaitu open-vocabulary detection, di mana model tidak lagi bergantung pada kelas tertutup, melainkan memanfaatkan representasi vision-language untuk mengenali kategori baru melalui deskripsi teks. Pemahaman mendalam tentang kualitas data awal akan menentukan seberapa kuat fondasi eksperimen Anda dalam menghadapi tantangan deteksi generatif dan multimodal di tahap selanjutnya.

---

## Slide 032 - Open-Vocabulary Detection

### Narasi

Pada slide ini, kita beralih dari pembahasan kualitas anotasi ke paradigma deteksi objek yang lebih fleksibel, yaitu **open-vocabulary detection**. Berbeda dengan detektor tradisional yang bersifat *closed-set* dan hanya mengenali kelas yang ada dalam set pelatihan, pendekatan ini memungkinkan model mendeteksi kategori yang sama sekali tidak terlihat selama fase *training*. Mekanisme ini sangat bergantung pada representasi *vision-language* seperti CLIP yang telah kita bahas pada pertemuan kelima, di mana pemetaan bersama antara ruang gambar dan teks menjadi jembatan untuk menggeneralisasi pengetahuan ke domain semantik baru.

Secara arsitektural, kerangka kerja ini umumnya mengikuti alur tiga tahap:
- Pertama, komponen deteksi standar menghasilkan *region proposal* yang kandidat lokasinya masih bersifat umum.
- Kedua, fitur visual dari setiap proposal di-*align* secara krusial dengan embedding teks dari deskripsi kelas target melalui mekanisme pencocokan kesamaan (*similarity matching*).
- Ketiga, kepala klasifikasi tidak lagi dibatasi oleh fungsi *softmax* statis terhadap jumlah kelas tetap, melainkan berubah menjadi lapisan dinamis yang dapat mengevaluasi kecocokan antara proposal gambar dan prompt teks apa pun yang diberikan.

Meskipun menjanjikan, implementasi *open-vocabulary detection* menghadapi sejumlah tantangan metodologis yang signifikan. Pertama, masalah *domain shift* sering muncul ketika model diuji pada distribusi data yang berbeda jauh dari data pra-pelatihan. Kedua, bias inheren pada data *caption* yang digunakan untuk melatih model vision-language dapat menyebabkan ketidakseimbangan representasi antar-kelas. Ketiga, evaluasi kinerja untuk kelas yang tidak terlihat memerlukan protokol yang adil dan transparan, karena metrik konvensional sering kali gagal menangkap nuansa generalisasi semantik.

Tantangan-tantangan ini secara langsung mengarah pada pentingnya evaluasi kritis dalam penelitian tingkat doktoral. Sebelum mengklaim peningkatan performa, peneliti harus mampu menjawab mengapa suatu metode unggul, bukan sekadar melaporkan angka. Hal ini akan kita bedah lebih lanjut pada slide berikutnya, di mana kita akan menyoroti pertanyaan kunci seputar interpretasi mAP, analisis kesalahan, serta desain eksperimen yang robust untuk menguji generalisasi model di berbagai kondisi domain.

---

## Slide 033 - Pertanyaan Kunci Penelitian

### Narasi

Pada slide ini, kita beralih dari pelaporan metrik performa menuju evaluasi kritis yang menjadi fondasi penelitian tingkat doktoral. Mengingat slide sebelumnya membahas deteksi open-vocabulary yang memanfaatkan representasi vision-language untuk mengenali kategori baru, kini saatnya kita menguji klaim kinerja model tersebut secara lebih mendalam dan skeptis.

Metrik mAP sering dijadikan patokan baku dalam publikasi computer vision, namun peningkatan nilai mAP tidak otomatis mencerminkan perbaikan pada deteksi objek yang benar-benar signifikan secara kontekstual. Kita perlu mempertanyakan apakah peningkatan tersebut didorong oleh klasifikasi kelas mayoritas, atau justru terjadi pada objek kecil, teroklusi, dan berada di latar belakang kompleks. Distribusi false positive dan false negative harus dipetakan per kategori, bukan hanya dihitung secara agregat. Selain itu, robustness model terhadap pergeseran domain—seperti variasi pencahayaan, resolusi sensor, atau gaya rendering—harus diuji secara eksplisit, karena kemampuan generalisasi lintas domain merupakan indikator kematangan arsitektur deteksi modern.

Untuk konteks penyusunan disertasi, setiap klaim bahwa suatu metode "lebih baik" wajib diperkuat dengan analisis kesalahan yang transparan dan evaluasi subset yang spesifik. Pergeseran fokus dari pertanyaan "berapa banyak" menjadi "mengapa" akan memperkuat validitas metodologis dan membuka ruang kontribusi ilmiah yang jelas. Pendekatan ini juga menjadi jembatan langsung ke pembahasan experimental design pada pertemuan ke-12, di mana desain uji yang rigor diperlukan untuk menjawab pertanyaan-pertanyaan kritis tersebut secara sistematis dan reproducible.

Sebagai tindak lanjut praktis, refleksi teoretis ini akan diterjemahkan ke dalam langkah operasional pada slide berikutnya. Kita akan membahas workflow analisis error yang terstruktur, mulai dari penghitungan metrik global dan per-kelas, pemetaan confusion matrix, pengelompokan kesalahan berdasarkan tipe lokalisasi atau okulasi, hingga visualisasi contoh prediksi yang gagal. Alur ini memastikan bahwa setiap temuan empiris dapat ditelusuri kembali ke akar masalahnya sebelum menarik kesimpulan metodologis.

---

## Slide 034 - Workflow Analisis Error

### Narasi

Setelah pada slide sebelumnya kita membahas pentingnya evaluasi kritis terhadap metrik mAP dan pertanyaan-pertanyaan fundamental yang harus dijawab dalam penelitian disertasi, langkah selanjutnya adalah menerjemahkan evaluasi tersebut ke dalam sebuah alur kerja analisis kesalahan yang sistematis. Metrik agregat seperti mAP global hanya memberikan gambaran permukaan; untuk memahami mengapa model berhasil atau gagal, kita memerlukan breakdown yang terstruktur.

Berikut adalah tahapan operasional yang harus diikuti:
1. Jalankan detektor pada seluruh subset data uji.
2. Hitung mAP secara global, lalu pecahkan menjadi mAP per kelas.
3. Buat confusion matrix antar kelas untuk memetakan bias klasifikasi.
4. Kelompokkan false positive berdasarkan jenis kesalahan: lokalisasi, klasifikasi, atau background.
5. Kelompokkan false negative berdasarkan ukuran objek dan tingkat okulasi.
6. Visualisasikan contoh prediksi yang gagal secara langsung pada gambar asli.

Pemecahan per kelas sangat krusial karena distribusi objek dalam dataset nyata hampir selalu tidak seimbang, dan performa rata-rata bisa menutupi kegagalan signifikan pada kategori minoritas. Pengelompokan false positive dan false negative ini menjawab pertanyaan "mengapa" yang ditekankan pada diskusi sebelumnya. Apakah model gagal mendeteksi karena bounding box tidak tight, karena mirip dengan objek lain, atau karena kondisi lingkungan seperti bayangan dan halangan?

Diagram alir yang disajikan pada slide ini merangkum proses tersebut secara linear namun iteratif: Prediksi → Evaluasi mAP → Per-kelas → FP/FN → Visualisasi → Kesimpulan. Proses ini mengubah angka statistik menjadi insight empiris yang dapat dipertanggungjawabkan secara ilmiah. Visualisasi kasus ekstrem menjadi bukti kualitatif yang melengkapi skor kuantitatif, sehingga klaim kinerja model tidak lagi bersifat spekulatif.

Kerangka analisis error ini merupakan fondasi metodologis yang akan langsung Anda terapkan pada slide berikutnya. Melalui praktikum fine-tuning YOLOv8, Anda akan menjalankan workflow ini secara nyata, menghitung metrik yang diminta, dan menyusun laporan benchmark yang didukung oleh visualisasi prediksi serta analisis kegagalan yang rigor.

---

## Slide 035 - Praktikum dan Tugas

### Narasi

Pada slide ini, kita beralih dari kerangka teoritis ke implementasi praktis melalui eksperimen fine-tuning detektor modern. Langkah pertama yang harus Anda siapkan adalah pemilihan dataset. Untuk menjaga fokus penelitian dan efisiensi komputasi, pilihlah subset data atau dataset kecil yang merepresentasikan domain masalah spesifik Anda. Pastikan subset tersebut mencakup variasi kondisi pencahayaan, okulasi, dan densitas objek yang relevan dengan skenario nyata.

Setelah struktur data ditetapkan, jalankan proses fine-tuning menggunakan framework Ultralytics untuk YOLOv8. Konfigurasikan hyperparameter secara eksplisit, mulai dari image size, batch size, epoch, hingga choice optimizer dan scheduler. Pantau kurva loss training dan validation secara berkala. Di tingkat doktoral, perhatian terhadap stabilitas konvergensi dan kriteria early stopping menjadi kunci untuk menghindari overfitting pada data terbatas.

Evaluasi kuantitatif harus dilakukan secara komprehensif. Anda wajib menghitung:
- mAP@0.5 untuk mengukur akurasi lokalisasi pada IoU threshold 0.5.
- mAP@0.5:0.95 untuk evaluasi yang lebih ketat terhadap rentang IoU.
- AP per kelas untuk mengidentifikasi bias model terhadap kategori tertentu.
- AP per ukuran objek (small, medium, large) guna memetakan kemampuan deteksi berdasarkan skala visual.
Pemecahan metrik ini esensial untuk merumuskan hipotesis penelitian mengenai keterbatasan arsitektur pada kondisi ekstrem.

Selanjutnya, visualisasikan prediksi pada subset validasi dan susun confusion analysis berbasis dua dimensi: ukuran objek dan identitas kelas. Kelompokkan false positive dan false negative secara sistematis. Apakah kesalahan dominan disebabkan oleh lokalisasi yang meleset, klasifikasi silang antar kelas semantik mirip, atau kegagalan mendeteksi objek kecil yang tertutup sebagian? Analisis kualitatif ini akan menjadi fondasi kuat untuk identifikasi research gap.

Sebagai bukti belajar, Anda harus menyerahkan tiga komponen utama:
1. Laporan benchmark deteksi yang menyajikan temuan empiris secara terstruktur dan kritis.
2. Konfigurasi eksperimen lengkap, mencakup script Python, file YAML dataset, dan log training, demi prinsip reproduktibilitas ilmiah.
3. Contoh prediksi beserta analisis kegagalan yang mengintegrasikan hasil kuantitatif dengan observasi visual.

Persiapan materi pada slide ini merupakan tindak lanjut langsung dari workflow analisis error yang telah kita diskusikan pada slide sebelumnya. Dengan menerapkan evaluasi sistematis, Anda akan menghasilkan dataset empiris yang siap diolah menjadi naskah akademik. Struktur penyusunan laporan ini akan kita bedah lebih lanjut pada slide berikutnya, yang menyediakan template baku untuk menyusun benchmark detection report sesuai standar publikasi internasional.

---

## Slide 036 - Template Laporan Benchmark Deteksi

### Narasi

Pada slide ini, kita akan membahas kerangka penulisan laporan benchmark deteksi yang menjadi kelanjutan langsung dari eksperimen praktikum pada pertemuan sebelumnya. Setelah Anda melakukan fine-tuning model YOLOv8, menghitung metrik evaluasi, serta memvisualisasikan prediksi pada data validasi, langkah selanjutnya adalah mendokumentasikan seluruh proses tersebut secara sistematis dalam format laporan akademik yang standar. Struktur laporan ini dirancang agar setiap komponen penelitian dapat ditelusuri, direproduksi, dan dievaluasi oleh reviewer maupun komunitas ilmiah.

Laporan benchmark harus mengikuti enam bagian utama yang telah terstruktur:
1. Pendahuluan dan tujuan, di mana Anda merumuskan masalah deteksi spesifik beserta justifikasi ilmiahnya.
2. Dataset dan protokol evaluasi, mencakup sumber data, skema pembagian subset, serta augmentasi yang diterapkan.
3. Konfigurasi model dan pelatihan, mendeskripsikan arsitektur, hyperparameter, scheduler, serta kondisi penghentian pelatihan.
4. Hasil kuantitatif, menyajikan metrik evaluasi secara transparan tanpa manipulasi data.
5. Analisis error, mengklasifikasikan pola kegagalan berdasarkan karakteristik objek dan konteks gambar.
6. Kesimpulan dan keterbatasan, menutup dengan interpretasi objektif serta pengakuan terhadap batasan metodologi dan data.

Sebagai acuan penyajian data, perhatikan contoh tabel perbandingan performa berikut. Kolom mAP@0.5 mengukur akurasi deteksi dengan IoU threshold 0,5, sedangkan mAP@0.5:0.95 memberikan gambaran lebih ketat terhadap presisi bounding box. Nilai AP small dan AP large mengungkap sensitivitas model terhadap skala objek, yang sangat relevan untuk studi kasus dengan variasi ukuran target yang ekstrem. Perbandingan antara pretrained baseline dan model yang telah di-fine-tuning menunjukkan peningkatan signifikan, terutama pada kategori kecil. Peningkatan ini tidak hanya mencerminkan adaptasi model terhadap distribusi data baru, tetapi juga validasi hipotesis bahwa transfer learning efektif mengatasi domain shift.

Selain tabel numerik, laporan wajib menyertakan visualisasi prediksi sebelum dan sesudah fine-tuning. Gambar-gambar ini berfungsi sebagai bukti empiris yang melengkapi angka statistik, sekaligus memudahkan identifikasi bias model terhadap kelas tertentu atau kondisi lingkungan spesifik. Visualisasi harus dipilih secara representatif, mencakup kasus sukses, false positive, false negative, dan ambiguitas anotasi. Pendekatan ini memastikan bahwa interpretasi hasil tidak bergantung semata-mata pada metrik agregat, melainkan didukung oleh observasi kontekstual yang kritis.

Dokumentasi yang rapi pada tahap ini akan menjadi fondasi kuat ketika Anda beralih ke evaluasi literatur pada slide berikutnya. Kemampuan menyusun laporan benchmark yang terstruktur dan analitis merupakan prasyarat untuk melakukan critical paper review terhadap publikasi detector terkini. Dengan demikian, setiap klaim kemajuan metodologis dalam paper dapat dibandingkan secara adil menggunakan framework evaluasi yang konsisten, sesuai dengan kompetensi membaca kritis yang telah dibangun sejak pertemuan awal.

---

## Slide 037 - Critical Review Paper Detector

### Narasi

Pada slide ini, kita beralih dari sekadar menyusun laporan benchmark ke tahap evaluasi kritis terhadap literatur deteksi objek modern. Setelah Anda memahami struktur pelaporan pada slide sebelumnya, langkah selanjutnya adalah menguji validitas klaim yang diajukan dalam paper-paper terbaru mengenai arsitektur berbasis YOLO maupun Vision Transformer. Evaluasi kritis menjadi fondasi utama dalam penelitian tingkat doktoral, karena novelty dan kontribusi ilmiah tidak dapat diukur hanya dari angka metrik semata.

Lima pertanyaan review yang disajikan berfungsi sebagai kerangka analisis sistematis:
- Apakah masalah yang dijawab benar-benar relevan dan menjawab celah metodologis yang belum terpecahkan?
- Apa kontribusi teknisnya secara mendalam, apakah berupa modifikasi arsitektur, mekanisme attention baru, atau strategi pelatihan?
- Apakah baseline dan konfigurasi eksperimen adil, atau terdapat bias setup yang menguntungkan metode baru?
- Apakah metrik evaluasi seperti mAP@0.5:0.95, AP small/large, serta analisis error memadai untuk skenario dunia nyata?
- Apakah setiap klaim performa didukung oleh data empiris yang transparan dan dapat direproduksi?

Untuk memfasilitasi analisis tersebut, gunakan matriks perbandingan paper sebagai alat sintesis literatur. Kolom metode, dataset, dan mAP membantu Anda memetakan perkembangan state-of-the-art secara visual. Kolom keterbatasan justru menjadi kunci untuk mengidentifikasi research gap yang potensial menjadi topik disertasi. Isi matriks ini secara berkala selama proses literature review, lalu kelompokkan paper berdasarkan pendekatan arsitektural atau domain aplikasi untuk menemukan pola tren penelitian terkini.

Kemampuan critical paper reading yang telah dilatih sejak Pertemuan 02 kini diterapkan langsung pada konteks detector modern. Hasil review ini akan menjadi dasar diskusi pada slide berikutnya mengenai fairness baseline. Tanpa protokol evaluasi yang adil dan ablation study yang ketat, klaim performa model baru cenderung bias dan sulit dipertanggungjawabkan secara akademis. Siapkan diri Anda untuk menelaah bagaimana baseline yang lemah dapat mendistorsi persepsi kemajuan teknologi, serta bagaimana standar reproducible benchmarking harus diintegrasikan dalam desain eksperimen penelitian Anda.

---

## Slide 038 - Diskusi Fairness Baseline

### Narasi

Pada slide ini, kita membahas aspek krusial dalam evaluasi deteksi objek modern, yaitu keadilan atau *fairness* baseline. Sering kali, peneliti membandingkan metode barunya dengan baseline yang di-setup secara tidak setara, baik dari segi ukuran model, arsitektur, maupun protokol pelatihan. Perbandingan seperti ini dapat menciptakan ilusi kinerja yang lebih tinggi, padahal peningkatan tersebut mungkin hanya berasal dari konfigurasi yang lebih menguntungkan, bukan dari inovasi inti yang ditawarkan.

Untuk memastikan validitas klaim ilmiah, setiap komponen baru yang diusulkan harus dibuktikan kontribusinya melalui *ablation study*. Tanpa analisis pengurangan komponen satu per satu, sulit untuk membedakan apakah peningkatan metrik berasal dari arsitektur, strategi optimasi, atau sekadar bias dalam penyetelan hiperparameter.

Sebagai aturan praktis dalam penelitian tingkat lanjut, berikut adalah standar yang harus diikuti:
- Gunakan *pretrained weights* resmi dengan protokol pelatihan dan evaluasi yang sama persis.
- Laporkan jumlah parameter, FLOPs, serta waktu inferensi secara transparan.
- Lakukan *ablation study* untuk setiap komponen yang diklaim berkontribusi terhadap peningkatan kinerja.

Pembahasan ini merupakan kelanjutan langsung dari pertanyaan kritis pada slide sebelumnya mengenai keadilan baseline dan konfigurasi. Jika Anda telah mengidentifikasi celah dalam matriks perbandingan paper, langkah selanjutnya adalah menerapkan standar pelaporan yang ketat agar eksperimen Anda dapat dipertanggungjawabkan secara akademis.

Konsep *fairness baseline* ini juga menjadi fondasi penting untuk *experimental design* dan *reproducible benchmarking* yang akan dibahas mendalam di Pertemuan 12. Ketika kita beralih dari deteksi ke segmentasi pada slide berikutnya, prinsip evaluasi yang ketat ini akan terus diterapkan. Metrik akan berevolusi dari IoU bounding box menjadi IoU/Dice pada mask, sementara teknik visualisasi prediksi dan error analysis yang dipelajari hari ini akan menjadi dasar untuk mengevaluasi *domain shift* dan responsivitas *promptable foundation model* seperti SAM.

---

## Slide 039 - Koneksi ke Pertemuan Berikutnya

### Narasi

Pada slide ini, kita akan menjembatani materi deteksi objek modern yang baru saja kita bahas dengan topik lanjutan pada pertemuan berikutnya. Setelah fokus pada bounding box regression dan klasifikasi kelas dalam object detection, langkah logis selanjutnya adalah memperluas ruang prediksi dari region diskrit menjadi pemetaan piksel per piksel. Pertemuan 08 akan menggeser perhatian kita ke semantic, instance, hingga panoptic segmentation. Di tingkat doktoral, pemahaman mendalam tentang perbedaan ketiganya bukan sekadar taksonomi, melainkan fondasi untuk merancang arsitektur yang sesuai dengan kompleksitas scene visual yang dituju.

Rigor metodologis yang kita tekankan pada diskusi fairness baseline—terutama terkait kesetaraan protokol evaluasi, transparansi pelaporan parameter, dan pentingnya ablation study—akan terus menjadi standar utama saat kita beralih ke evaluasi segmentasi. Metrik IoU dan Dice tidak hanya dihitung sebagai angka, tetapi akan dianalisis korelasinya dengan bias dataset, stabilitas training, dan konsistensi prediksi across different scales.

Kita juga akan melakukan komparasi kritis antara pendekatan segmentasi berbasis supervised tradisional dengan paradigma baru menggunakan promptable foundation model seperti SAM (Segment Anything Model). Perbandingan ini akan mencakup aspek efisiensi komputasi, kemampuan generalisasi lintas domain, serta fleksibilitas dalam interaksi manusia-mesin. Evaluasi kuantitatifnya pun akan diperkaya dengan metrik IoU dan Dice coefficient, sambil secara eksplisit menganalisis dampak domain shift terhadap kualitas mask yang dihasilkan.

Keterampilan analisis error dan teknik visualisasi prediksi yang telah kita latun hari ini akan langsung diadopsi dan diperdalam. Pemahaman Anda mengenai perhitungan IoU pada bounding box kini akan berevolusi menjadi evaluasi overlap pada pixel-level mask. Transisi konseptual ini sangat krusial karena kesalahan lokal pada boundary detection atau false positive pada background dapat berdampak signifikan terhadap downstream tasks seperti tracking, robotic manipulation, atau medical image analysis.

Sebagai persiapan sebelum pertemuan 08, disarankan untuk membaca paper asli Segment Anything secara seksama. Fokuskan pemahaman Anda pada mekanisme prompting yang mendukung tiga modalitas input utama: point, box, dan mask. Pahami bagaimana prompt tersebut diterjemahkan menjadi spatial attention dalam decoder, serta bagaimana desain arsitektur image encoder dan prompt encoder memungkinkan zero-shot transfer ke unseen objects. Persiapan ini akan memaksimalkan diskusi kritis kita mengenai trade-off antara akurasi, latency, dan skalabilitas foundation model dalam konteks penelitian computer vision terkini.
