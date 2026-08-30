# Narasi TD Pengolahan Citra Digital - Pertemuan 04

## Self-Supervised Learning dan Foundation Vision Models

Sumber: markdown/pert04-self-supervised-learning-dan-foundation-vision-models.md

---

## Slide 000 - Cover

### Narasi

Slide pembuka ini menetapkan fokus utama pada *Self-Supervised Learning* dan *Foundation Vision Models*. Pada jenjang doktoral, paradigma pembelajaran tanpa label eksplisit dipahami sebagai pergeseran fundamental dalam cara model mempersepsi dan mengkodekan realitas visual. Alih-alih bergantung pada anotasi manusia yang terbatas dan rentan bias, metode ini memanfaatkan struktur statistik dan korelasi intrinsik dari data citra berskala besar untuk membentuk representasi yang kaya, invariant terhadap augmentasi, dan siap ditransfer.

Kombinasi antara arsitektur modern seperti Vision Transformer dengan mekanisme pembelajaran kontrasitif atau masking reconstruction telah melahirkan model yang mampu menghasilkan embedding universal. Representasi ini bersifat highly transferable, sehingga dapat diadaptasi ke berbagai tugas computer vision dengan efisiensi komputasi dan kebutuhan data yang jauh lebih rendah dibandingkan pendekatan supervised tradisional.

Materi ini berfungsi sebagai jembatan metodologis antara fondasi arsitektural yang telah dipelajari sebelumnya, dengan kebutuhan riset mutakhir akan representasi yang mandiri dan skalabel. Langkah selanjutnya adalah memetakan posisi strategis topik ini dalam roadmap pengembangan kurikulum, yang secara eksplisit menghubungkan pemahaman backbone modern dengan persiapan menuju integrasi multimodal.

---

## Slide 001 - Posisi Pertemuan 04 dalam RPS

### Narasi

Slide ini memetakan posisi Pertemuan 04 dalam struktur Rencana Pembelajaran Semester (RPS). Tabel yang disajikan menunjukkan kesinambungan logis antar pertemuan, di mana Pertemuan 04 berfungsi sebagai penghubung metodologis antara Pertemuan 03 dan Pertemuan 05.

Pada Pertemuan 03, kita telah membangun pemahaman mendalam mengenai representasi visual modern, termasuk arsitektur backbone CNN, mekanisme attention, dan Vision Transformer. Arsitektur tersebut memberikan kapabilitas ekstraksi fitur yang kuat. Namun, kekuatan arsitektur harus diimbangi dengan strategi pembelajaran yang efisien. Pertemuan 04 menjawab kebutuhan ini dengan fokus pada Self-Supervised Learning (SSL) dan Foundation Vision Models, yaitu pendekatan yang memungkinkan pelatihan representasi visual tanpa ketergantungan pada label eksplisit.

Transisi ini sangat krusial bagi penelitian tingkat doktor. Dataset domain-spesifik seringkali memiliki keterbatasan anotasi, sehingga metode self-supervised menawarkan solusi untuk mengekstrak pengetahuan umum dari data mentah. Representasi berkualitas tinggi yang dipelajari pada pertemuan ini akan langsung diaplikasikan pada Pertemuan 05, khususnya sebagai modalitas visual dalam model vision-language dan representasi multimodal.

Secara ringkas, alur materi ini dirancang untuk:
- Menghubungkan pemahaman arsitektur dari Pertemuan 03 dengan teknik pembelajaran representasi tanpa label.
- Menyiapkan fondasi representasi yang akan menjadi komponen inti dalam analisis model multimodal seperti CLIP pada Pertemuan 05.

Dengan peta perjalanan akademik yang jelas ini, kita dapat langsung beralih ke tujuan pembelajaran spesifik yang akan menjadi acuan evaluasi kompetensi kita sepanjang pertemuan ini.

---

## Slide 002 - Tujuan Pembelajaran

### Narasi

Slide ini menyajikan tujuan pembelajaran yang menjadi kompas akademis bagi seluruh diskusi dan eksperimen pada pertemuan keempat. Setelah mengikuti sesi ini, mahasiswa diharapkan mampu menjelaskan konsep fundamental *self-supervised learning* atau SSL serta secara kritis membedakannya dari paradigma *supervised learning*. Pembedaan ini sangat krusial mengingat SSL memanfaatkan struktur intrinsik data sebagai sinyal pelatihan, sehingga menghilangkan ketergantungan pada anotasi manual yang mahal, laborious, dan rentan terhadap bias manusia. Pendekatan ini sejalan dengan transisi yang dibahas pada slide sebelumnya, di mana arsitektur backbone modern yang telah dipahami pada pertemuan ketiga kini diarahkan untuk mengeksploitasi representasi tanpa label eksplisit.

Analisis mendalam akan difokuskan pada mekanisme-mekanisme inti yang mendominasi ekosistem SSL untuk visi komputer. Mahasiswa akan mempelajari bagaimana *contrastive learning* memaksa model untuk memisahkan representasi positif dan negatif melalui augmentasi data, sebagaimana diimplementasikan dalam kerangka kerja SimCLR dan MoCo. Selain itu, konsep *teacher-student learning* dengan *momentum encoder* akan dibahas sebagai strategi stabilisasi training yang memungkinkan konsistensi prediksi tanpa memerlukan memori buffer skala besar. Paralel dengan itu, *masked image modeling* atau MAE akan dianalisis sebagai paradigma alternatif yang merekonstruksi bagian citra yang disembunyikan untuk belajar representasi semantik yang kuat dan kontekstual.

Evaluasi kualitas representasi yang dipelajari oleh model-model tersebut tidak dapat dilakukan secara subjektif. Oleh karena itu, protokol evaluasi seperti *linear probing* dan visualisasi embedding akan menjadi fokus metodologis yang ketat. Mahasiswa diajak memahami bahwa performa linear classifier pada fitur beku (*frozen features*) merupakan indikator objektif atas kualitas abstraksi visual yang telah berhasil dipelajari oleh backbone model. Visualisasi embedding menggunakan teknik reduksi dimensi akan memberikan wawasan intuitif mengenai klasterisasi kelas dan pemisahan domain dalam ruang vektor berdimensi tinggi, sekaligus menjadi dasar untuk identifikasi *research gap* terkait kapasitas generalisasi model.

Pada tingkat penelitian doktoral, kemampuan merancang eksperimen transferabilitas menjadi kompetensi inti. Mahasiswa akan dilatih untuk mengevaluasi seberapa robust representasi DINO atau DINOv2 ketika diterapkan pada variasi domain, resolusi input, jumlah label terbatas, serta distribusi data yang berbeda. Eksperimen ini dirancang bukan sekadar benchmarking, melainkan upaya sistematis mengidentifikasi batas-batas generalisasi model foundation vision dan potensi *domain shift* yang sering menjadi kendala utama dalam penerapan industri maupun pengembangan metodologi lanjutan.

Sebagai penutup tujuan pembelajaran, mahasiswa diharapkan mampu memberikan rekomendasi strategis dalam pemilihan dan adaptasi *foundation vision model* sesuai dengan konteks penelitian masing-masing. Rekomendasi ini harus didasarkan pada pertimbangan matang mengenai trade-off antara kompleksitas arsitektur, kebutuhan komputasi, ketersediaan data, dan karakteristik tugas akhir. Pembahasan pada slide ini akan mengalir langsung ke agenda pertemuan berikutnya, yang mencakup rincian topik mulai dari perbandingan paradigma pembelajaran, implementasi kontrastif hingga masked modeling, hingga desain eksperimen transferabilitas, praktikum ekstraksi embedding, dan persiapan seminar paper.

---

## Slide 003 - Agenda Pertemuan

### Narasi

Agenda pertemuan ini disusun sebagai peta jalan sistematis untuk mencapai kompetensi penelitian tingkat doktoral yang telah ditetapkan pada slide tujuan pembelajaran. Fokus utama kita adalah menguasai fondasi teoretis, menganalisis mekanisme arsitektural, dan merancang protokol evaluasi yang rigor untuk model foundation vision berbasis self-supervised learning.

Kita akan memulai dengan memetakan evolusi paradigma pembelajaran, mulai dari supervised, unsupervised, hingga self-supervised. Dari kerangka ini, kita akan mengidentifikasi celah metodologis yang mendorong lahirnya pendekatan self-supervised khusus untuk visi komputer, di mana sinyal supervisi tidak lagi bergantung pada anotasi manual.

Pembahasan teknis akan masuk ke inti mekanisme contrastive learning, mencakup implementasi SimCLR dan MoCo, serta peran krusial momentum encoder dalam menjaga konsistensi representasi selama proses optimasi. Selanjutnya, kita akan membedah prinsip teacher-student learning yang menjadi tulang punggung DINO, di mana distilasi pengetahuan digunakan untuk memaksa model belajar fitur invarian tanpa label.

Selain pendekatan kontrastif, kita akan menelaah masked image modeling, khususnya arsitektur MAE, yang memanfaatkan prediksi patch tersembunyi untuk merekonstruksi konteks spasial dan semantik. Representasi berkualitas tinggi yang dihasilkan oleh DINOv2 akan diukur menggunakan protokol linear probing, sebuah standar evaluasi yang membuktikan efektivitas embedding tanpa memerlukan fine-tuning komputasi mahal.

Setelah fase teoritis, kita akan langsung menerapkannya dalam desain eksperimen transferability. Anda akan menguji ketahanan representasi terhadap variasi domain, resolusi input, kelangkaan label, dan pergeseran distribusi data. Praktikum akan menyelaraskan teori dengan kode melalui ekstraksi embedding, visualisasi manifold fitur, dan implementasi linear probing. Rangkaian ini ditutup dengan diskusi seminar paper dan penyusunan target keluaran penelitian.

Agenda ini secara langsung membuka pintu menuju pertanyaan fundamental yang akan kita gali lebih dalam pada slide berikutnya: mengapa self-supervised learning menjadi solusi strategis ketika label anotasi bersifat mahal, lambat, dan rentan inkonsistensi. Dengan memahami bagaimana pretext task membangun supervisi dari data itu sendiri, kita dapat menilai potensi kontribusi ilmiah baru dalam riset pengolahan citra digital.

---

## Slide 004 - Mengapa Self-Supervised Learning?

### Narasi

Salah satu tantangan utama dalam pengembangan model visi komputer adalah ketergantungan pada data berlabel. Proses anotasi manual memerlukan biaya tinggi, waktu yang lama, dan sangat rentan terhadap inkonsistensi antar annotator. Di sisi lain, kita justru memiliki akses ke kumpulan data citra tanpa label yang jumlahnya masif, mulai dari internet umum hingga domain spesifik seperti pencitraan medis, satelit, dan pemantauan industri. Model yang hanya dilatih dengan subset kecil data berlabel cenderung mengalami overfitting dan gagal menggeneralisasi ketika dihadapkan pada distribusi data atau domain baru yang berbeda.

Hal ini membawa kita pada pertanyaan fundamental dalam penelitian pengolahan citra digital saat ini: bagaimana sebuah model dapat mempelajari representasi visual yang bermakna dan robust tanpa memerlukan label eksplisit untuk setiap sampel data? Pertanyaan ini bukan sekadar masalah efisiensi, melainkan kunci untuk membuka skalabilitas pembelajaran mesin di era big data.

Self-supervised learning menjawab tantangan tersebut melalui mekanisme pretext task. Alih-alih mengandalkan manusia sebagai sumber supervisi, model secara otomatis menciptakan tugas pembelajaran dari struktur data itu sendiri. Supervisi tidak diberikan secara eksternal, melainkan diturunkan dari hubungan internal data, seperti memprediksi bagian gambar yang disembunyikan, membandingkan augmentasi yang berbeda dari objek yang sama, atau menyusun kembali patch-patch citra. Pendekatan ini memungkinkan model menangkap pola geometris, tekstur, dan semantik tingkat tinggi secara mandiri.

Pada slide sebelumnya, agenda pertemuan telah menyebutkan berbagai arsitektur dan protokol evaluasi yang akan kita bedah lebih lanjut. Setelah memahami motivasi dasar mengapa self-supervised learning menjadi paradigma yang krusial, kita akan melanjutkan ke perbandingan sistematis antara supervised, unsupervised, dan self-supervised learning pada slide berikutnya. Tabel komparasi tersebut akan membantu kita menempatkan posisi SSL secara konseptual, sekaligus mempersiapkan landasan untuk desain eksperimen transferability yang akan kita praktikkan nanti.

---

## Slide 005 - Perbandingan Paradigma Pembelajaran

### Narasi

Slide ini menyajikan pemetaan komparatif antara tiga paradigma pembelajaran mesin yang menjadi kerangka metodologis utama dalam pengembangan model computer vision terkini. Sebagai tindak lanjut dari motivasi mengapa self-supervised learning diperlukan, tabel ini secara eksplisit membedah sumber supervisi, contoh implementasi, keunggulan strategis, serta batasan inheren dari setiap pendekatan.

Pada spektrum tradisional, *Supervised Learning* bergantung penuh pada label manual yang telah diverifikasi manusia. Pendekatan ini memberikan akurasi tinggi pada tugas target yang spesifik, namun sangat rentan terhadap kelelahan anotasi, inkonsistensi penandaan, dan kesulitan ketika diterapkan pada distribusi data domain baru. Di ujung lainnya, *Unsupervised Learning* bekerja tanpa label eksternal, hanya mengandalkan struktur statistik atau geometri data melalui teknik seperti clustering atau dekomposisi matriks. Meskipun bebas anotasi, evaluasi metriknya sering kali subjektif dan outputnya belum siap secara langsung untuk tugas prediksi terarah.

Posisi *Self-Supervised Learning* menempati zona hibrida yang strategis. Model tidak menunggu instruksi label dari manusia, melainkan mengekstrak sinyal supervisi secara otomatis dari data mentah melalui konstruksi *pretext task*. Label yang digunakan bersifat sintetis, diturunkan dari augmentasi citra, masking wilayah, pencocokan fitur antar-viewport, atau prediksi konteks spasial. Skala pembelajaran menjadi tak terbatas, transferability representasi meningkat drastis, dan dependency pada anotasi hilang total. Namun, kompleksitas metodologisnya tetap tinggi: desain pretext task haruslah kritis, karena jika terlalu trivial atau hanya menangkap artefak permukaan, representasi yang dihasilkan akan miskin semantik. Selain itu, fase pra-pelatihan skala besar menuntut alokasi komputasi GPU/TPU yang substansial.

Dari kacamata riset doktoral, pemahaman perbandingan ini menjadi acuan fundamental saat Anda menyusun experimental design dan memposisikan novelty karya ilmiah. Pemilihan paradigma akan menentukan bagaimana Anda menetapkan baseline, memilih metrik evaluasi, dan merancang ablation study untuk validasi klaim kontribusi. Pembahasan selanjutnya akan menguraikan definisi formal self-supervised learning, alur end-to-end dari input tanpa label hingga ekstraksi embedding, serta kriteria ketat yang harus dipenuhi oleh sebuah pretext task agar benar-benar selaras dengan downstream task seperti deteksi objek, segmentasi semantik, atau restorasi citra medis dan satelit.

---

## Slide 006 - Definisi Self-Supervised Learning

### Narasi

Setelah menelaah perbandingan paradigma pembelajaran pada slide sebelumnya, kita kini mendefinisikan secara eksplisit fondasi konseptual Self-Supervised Learning (SSL). Pada jenjang doktoral, penting untuk memandang SSL bukan sekadar teknik penghematan anotasi, melainkan sebuah kerangka representation learning yang sistematis. Pendekatan ini mengonstruksi pretext task langsung dari struktur intrinsik data mentah, sehingga model dipaksa mempelajari dependensi spasial, geometris, atau semantik yang melekat pada data itu sendiri tanpa campur tangan manusia.

Diagram alur pada slide menggambarkan transformasi data secara bertahap. Input tanpa label diproses melalui mekanisme pretext task untuk mengekstrak fitur laten yang padat. Representasi atau embedding yang dihasilkan kemudian menjadi basis universal untuk berbagai downstream task, seperti klasifikasi, deteksi objek, atau segmentasi. Transfer pengetahuan ini umumnya dieksekusi melalui dua skema: linear probing untuk mengukur kualitas representasi secara cepat dan terisolasi, maupun fine-tuning penuh ketika domain target memerlukan penyesuaian bobot model yang lebih intensif.

Keberhasilan transfer representasi sangat bergantung pada rigor desain pretext task. Dalam konteks penelitian mutakhir, sebuah pretext task harus memenuhi tiga syarat fundamental agar layak dikembangkan menjadi foundation model:
- Harus menuntut pemahaman semantik yang mendalam, bukan sekadar pencocokan pola permukaan atau korelasi statistik rendah tingkat.
- Harus memiliki relevansi struktural yang kuat dengan downstream task, memastikan fitur yang dipelajari benar-benar translatable ke tugas prediksi akhir.
- Harus dapat dioptimalkan secara stabil dan efisien pada skala dataset masif, mengingat training foundation model menuntut keseimbangan antara kompleksitas objective function dan throughput infrastruktur komputasi.

Prinsip-prinsip desain inilah yang kemudian memetakan diversifikasi arsitektur dalam literatur computer vision terkini. Tergantung pada bagaimana sinyal supervisi buatan tersebut diformulasikan dan dioptimalkan, metode SSL untuk citra berkembang menjadi tiga taksonomi utama yang akan kita analisis secara kritis pada slide berikutnya. Kategori tersebut meliputi pendekatan kontrastif yang memanipulasi ruang embedding melalui pasangan positif-negatif, masked image modeling yang menekankan rekonstruksi bagian citra yang hilang, serta metode non-kontrastif yang menghindari sampel negatif sepenuhnya melalui konsistensi bootstrap atau regularisasi implisit.

---

## Slide 007 - Taksonomi Self-Supervised Learning untuk Citra

### Narasi

Pada slide sebelumnya, kita telah mendefinisikan self-supervised learning sebagai pendekatan pembelajaran representasi yang membangun pretext task dari data tanpa label. Representasi yang dihasilkan kemudian dapat diadaptasi ke tugas hilir melalui linear probing atau fine-tuning. Untuk memahami mekanisme pembentukan representasi tersebut secara sistematis, kita perlu menelaah taksonomi utama yang digunakan dalam domain citra.

Secara umum, taksonomi self-supervised learning untuk citra dibagi menjadi tiga paradigma besar, masing-masing dengan strategi objektif yang berbeda:

- **Contrastive Methods**: Pendekatan ini bekerja dengan menarik representasi positif agar saling berdekatan dan menjauhkan representasi negatif. Implementasi standarnya meliputi SimCLR dan MoCo. DINO juga masuk dalam kelompok ini, meskipun menggunakan arsitektur teacher-student yang menekankan konsistensi antar view.
- **Masked Image Modeling**: Metode ini tidak mengandalkan pasangan positif-negatif, melainkan menyembunyikan sebagian patch citra dan memaksa model untuk merekonstruksinya berdasarkan konteks sekitarnya. MAE dan SimMIM adalah contoh prominent yang membuktikan bahwa rekonstruksi lokal dapat menghasilkan embedding semantik yang sangat kaya.
- **Non-Contrastive Methods**: Kelompok ini menghilangkan kebutuhan akan negative samples, sehingga mengurangi beban komputasi dan kompleksitas training. Model hanya menggunakan augmented views positif, namun mencegah collapse representasi melalui regularisasi atau teknik bootstrap. BYOL dan DINOv2 berada dalam kategori ini, dengan DINOv2 yang menggabungkan beberapa objective untuk meningkatkan stabilitas embedding.

Pemahaman terhadap pembagian taksonomi ini bersifat fundamental karena setiap metode menawarkan trade-off yang berbeda dalam hal skalabilitas, kebutuhan memori, dan kualitas fitur yang dihasilkan. Bagi peneliti tingkat doktoral, pengetahuan ini menjadi landasan untuk mengkritisi arsitektur foundation model yang ada dan memilih strategi pretraining yang paling sesuai dengan batasan sumber daya serta karakteristik dataset penelitian.

Sebagaimana akan dibahas lebih lanjut pada slide berikutnya, hampir semua foundation vision model modern mengandalkan SSL sebagai tahap pretraining awal. Dengan menguasai karakteristik masing-masing kategori taksonomi, Anda dapat mengidentifikasi celah penelitian yang strategis, mulai dari transferability ke domain spesifik, efisiensi kebutuhan label, hingga robustness terhadap variasi distribusi data. Dari pemahaman inilah peluang novelty sering kali lahir, baik melalui desain pretext task baru maupun kombinasi objective yang belum tereksplorasi secara mendalam.

---

## Slide 008 - Mengapa SSL Penting untuk Riset Doktoral?

### Narasi

Pada slide sebelumnya, kita telah menguraikan taksonomi utama dalam self-supervised learning untuk citra, mencakup metode kontrastif, masked image modeling, serta pendekatan non-kontrastif. Klasifikasi tersebut bukan sekadar pemetaan arsitektural, melainkan cerminan dari evolusi strategi ekstraksi fitur yang kini menjadi standar industri dan akademis.

Hampir seluruh foundation vision model mutakhir mengandalkan self-supervised learning sebagai tahap pretraining sebelum dilakukan adaptasi ke tugas spesifik. Penguasaan terhadap prinsip ini memungkinkan peneliti doktoral melakukan evaluasi kritis terhadap bagaimana representasi visual dikonstruksi, distabilkan, dan dioptimalkan tanpa bergantung pada skema anotasi manual yang masif dan mahal.

Fokus riset tingkat doktor dapat diarahkan pada tiga dimensi strategis yang secara alami terbuka melalui ekosistem SSL:
- **Transferability**: menganalisis kapasitas representasi yang dipelajari secara mandiri untuk berpindah ke domain atau tugas baru dengan degradasi kinerja minimal.
- **Efisiensi Label**: mengidentifikasi ambang batas jumlah label terkecil yang masih mempertahankan kinerja kompetitif, sehingga mengurangi ketergantungan pada data terannotasi.
- **Robustness**: menguji ketahanan representasi terhadap variasi resolusi input, augmentasi ekstrem, maupun pergeseran distribusi data yang signifikan.

Selain itu, SSL menyediakan lahan subur untuk menghasilkan novelty penelitian melalui perancangan pretext task yang belum tereksplorasi atau penggabungan multi-objective yang lebih kompleks. Pada jenjang doktoral, kontribusi ilmiah bergeser dari peningkatan metrik marginal menuju perumusan kerangka pembelajaran representasi yang lebih efisien, generalizable, atau memiliki interpretabilitas struktural yang jelas.

Pemahaman mengenai urgensi SSL dalam konteks riset doktoral ini akan menjadi landasan analitis ketika kita masuk ke mekanisme inti dari salah satu paradigma paling dominan. Pada slide berikutnya, kita akan membedah intuisi dasar contrastive learning, termasuk bagaimana augmentasi citra membentuk pasangan positif-negatif dan bagaimana encoder memampatkan informasi tersebut ke dalam ruang embedding yang terstruktur secara geometris.

---

## Slide 009 - Contrastive Learning: Intuisi Dasar

### Narasi

Setelah pada slide sebelumnya kita membahas mengapa *Self-Supervised Learning* menjadi fondasi krusial bagi riset doktoral di bidang *Computer Vision*, kini kita akan menyoroti salah satu paradigma paling berpengaruh dalam SSL, yaitu *Contrastive Learning*. Pendekatan ini tidak memerlukan anotasi manual sama sekali, melainkan memanfaatkan struktur intrinsik dari data citra itu sendiri untuk membentuk representasi yang bermakna dan generalisasi tinggi.

Intuisi dasar dari *contrastive learning* sangat elegan namun powerful: dua augmentasi berbeda yang berasal dari citra yang sama harus menghasilkan representasi atau *embedding* yang sangat mirip. Proses ini dimulai dengan mengambil satu sampel citra asli, lalu menerapkan serangkaian transformasi acak seperti *random crop*, *horizontal flip*, *color jitter*, atau *rotasi*. Hasilnya adalah dua *view* atau perspektif berbeda yang secara semantik tetap merepresentasikan objek atau konteks visual yang sama.

Perhatikan alur yang disajikan pada diagram di atas. Citra asli $x$ diproses melalui dua jalur augmentasi independen, menghasilkan *view1* dan *view2*. Kedua *view* tersebut kemudian dilewatkan ke jaringan *encoder* yang memiliki bobot bersama (*shared weights*). Output dari *encoder* berupa vektor *embedding* yang kemudian dibandingkan jaraknya. Tujuan optimasinya adalah meminimalkan jarak atau memaksimalkan kesamaan antara *embedding1* dan *embedding2*, sehingga $d(\text{embedding1}, \text{embedding2})$ mendekati nilai terkecil.

Dalam kerangka kerja ini, definisi pasangan sangat menentukan arah pembelajaran model:
- Pasangan positif didefinisikan sebagai dua *view* yang berasal dari sumber citra yang identik.
- Pasangan negatif merujuk pada perbandingan antara *view* dari satu citra dengan *view* dari citra lain yang berbeda secara semantik.
Mekanisme penarikan (*pull*) terhadap pasangan positif dan pendorongan (*push*) terhadap pasangan negatif inilah yang memaksa model untuk belajar fitur tingkat tinggi yang *invariant* terhadap variasi augmentasi, sekaligus sensitif terhadap perbedaan konten visual.

Konsep penarikan dan pendorongan ini tidak hanya bersifat konseptual, melainkan harus dijabarkan ke dalam formulasi matematis yang dapat dioptimalkan menggunakan *gradient descent*. Pada slide berikutnya, kita akan mengupas bagaimana mekanisme ini diimplementasikan secara formal melalui fungsi kerugian *InfoNCE*, lengkap dengan peran parameter suhu (*temperature*) yang mengatur ketajaman distribusi probabilitas dalam ruang *embedding*.

---

## Slide 010 - Contrastive Loss: InfoNCE

### Narasi

Pada slide sebelumnya, kita telah membahas intuisi dasar dari contrastive learning, yaitu prinsip bahwa dua augmentasi berbeda dari citra yang sama harus menghasilkan representasi yang mirip, sementara citra yang berbeda harus dijauhkan dalam ruang embedding. Langkah logis berikutnya adalah menerjemahkan intuisi tersebut ke dalam formulasi matematika yang dapat dioptimalkan secara gradien selama pelatihan. Fungsi loss yang menjadi standar de facto untuk tujuan ini adalah InfoNCE.

InfoNCE atau *Info Noise-Contrastive Estimation* menghitung probabilitas bahwa pasangan embedding positif benar-benar cocok, dibandingkan dengan seluruh kandidat embedding lain dalam satu batch. Rumus yang tertera pada slide ini dapat diuraikan sebagai berikut:

- `z_i` dan `z_j` adalah embedding dari dua view positif yang berasal dari satu citra asli.
- `z_k` mewakili embedding dari view lain dalam batch, yang secara otomatis berperan sebagai sampel negatif.
- `sim` adalah fungsi kesamaan, umumnya *cosine similarity*, yang mengukur sudut antara dua vektor embedding.
- `tau` adalah parameter suhu (*temperature*) yang mengontrol ketajaman distribusi probabilitas. Penurunan nilai tau akan memperbesar margin pemisahan antar kelas, sedangkan peningkatan nilai tau memberikan efek regularisasi yang lebih halus.

Secara struktural, loss ini bekerja dengan mendorong pembilang—eksponensiasi kesamaan pasangan positif dibagi tau—untuk mendominasi penyebut yang menjumlahkan semua eksponensiasi kesamaan terhadap sampel dalam batch. Hasilnya, optimasi secara otomatis akan menarik pasangan positif saling mendekat dan menolak semua sampel negatif dalam satu langkah komputasi yang efisien. Keunggulan utama pendekatan ini adalah tidak memerlukan sampling negatif yang rumit, karena seluruh batch secara alami menyediakan referensi negatif yang beragam dan dinamis.

Dalam praktiknya, performa InfoNCE sangat bergantung pada ukuran batch. Batch size yang besar diperlukan untuk memastikan ketersediaan negatif samples yang cukup, yang secara langsung berkorelasi dengan kualitas representasi akhir. Selain itu, normalisasi embedding sebelum perhitungan kesamaan sering kali diterapkan untuk menjaga stabilitas numerik selama backpropagation. 

Konsep loss ini tidak berdiri sendiri, melainkan menjadi inti dari berbagai framework modern. Pada slide berikutnya, kita akan melihat bagaimana InfoNCE diimplementasikan secara utuh dalam pipeline SimCLR, termasuk peran encoder backbone, projection head, dan strategi augmentasi yang dirancang khusus untuk memaksimalkan efektivitas loss tersebut.

---

## Slide 011 - SimCLR: Framework Contrastive Sederhana

### Narasi

Pada slide sebelumnya kita telah menguraikan fungsi loss InfoNCE yang menjadi fondasi matematis dari pembelajaran kontrastif. Loss ini mendorong model untuk memaksimalkan kesamaan antara pasangan positif sambil menekan kesamaan dengan pasangan negatif dalam satu batch. Kini, kita akan melihat bagaimana prinsip tersebut diintegrasikan ke dalam arsitektur end-to-end melalui framework SimCLR yang diperkenalkan oleh Chen dkk. pada tahun 2020.

SimCLR menawarkan pipeline yang elegan dan mudah direproduksi. Alur pemrosesan data dapat kita baca sebagai berikut:
```text
Citra x -> Augmentasi T -> x1, x2 -> Encoder f -> h1, h2 -> Projection g -> z1, z2 -> InfoNCE
```
Proses dimulai dengan pengambilan satu citra asli $x$, yang kemudian mengalami dua transformasi augmentasi independen menggunakan operator $T$, menghasilkan $x_1$ dan $x_2$. Kedua view ini diinjak oleh encoder $f$, yang umumnya merupakan backbone CNN atau Vision Transformer, untuk mengekstrak embedding $h_1$ dan $h_2$. Embedding tersebut selanjutnya dilewatkan ke projection head $g$, sebuah MLP dangkal yang memetakan fitur ke ruang kontras berdimensi tetap. Hasil akhirnya adalah $z_1$ dan $z_2$ yang langsung dievaluasi menggunakan loss InfoNCE.

Setelah fase pretraining selesai, projection head $g$ sengaja dihentikan penggunaannya. Encoder $f$ yang tersisa kemudian di-fine-tune atau digunakan langsung sebagai feature extractor untuk tugas downstream. Desain ini didasarkan pada temuan empiris bahwa projection head berfungsi sebagai regularizer yang mencegah representasi runtuh (representation collapse) selama training, namun tidak membawa manfaat signifikan untuk generalisasi semantik pada tahap inference.

Keberhasilan SimCLR sangat bergantung pada tiga pilar utama. Pertama, strategi augmentasi harus kuat dan mencakup variasi geometris serta fotometrik yang luas, sehingga model belajar invariansi terhadap noise non-semantis. Kedua, ukuran batch harus dibuat sangat besar untuk menyediakan distribusi negatif yang padat; hal ini meminimalkan bias estimasi dalam perhitungan softmax pada InfoNCE. Ketiga, L2-normalisasi pada embedding sebelum perhitungan cosine similarity wajib diterapkan untuk menstabilkan gradien dan mencegah eksplosinya nilai loss.

Namun, kebutuhan akan batch size raksasa ini menciptakan bottleneck komputasi dan memori yang serius, terutama ketika bekerja dengan arsitektur skala besar atau dataset beresolusi tinggi. Keterbatasan ini membuka jalan bagi optimasi lebih lanjut, yang akan kita bahas pada slide berikutnya melalui mekanisme momentum encoder dan pemanfaatan queue untuk memperluas himpunan negative samples secara efisien tanpa meningkatkan beban GPU secara linear.

---

## Slide 012 - Momentum Encoder dan Negative Samples

### Narasi

Pada pembahasan slide sebelumnya mengenai SimCLR, telah ditegaskan bahwa pembelajaran kontras membutuhkan ukuran *batch* yang sangat besar untuk menyediakan cukup sampel negatif guna mencegah runtuhnya representasi. Namun, memperbesar *batch size* secara langsung akan membebani komputasi dan memori GPU secara signifikan, sehingga tidak skalabel untuk model skala besar. Slide ini membahas solusi arsitektural yang mengatasi batasan tersebut melalui integrasi *momentum encoder* dan mekanisme antrian (*queue*).

Secara fundamental, pembelajaran kontras sangat bergantung pada ketersediaan sampel negatif yang beragam dan stabil. Jika hanya mengandalkan *batch* saat ini, jumlah negatif terbatas oleh dimensi *batch*. Solusi naifnya adalah memperbesar *batch*, tetapi pendekatan ini mahal secara komputasi. Oleh karena itu, diperlukan mekanisme untuk menyimpan dan memanfaatkan sampel dari iterasi sebelumnya sebagai negatif tambahan tanpa meningkatkan beban memori per langkah optimisasi.

Mekanisme yang diterapkan dapat dipahami melalui alur berikut:
```text
queue (sampel lama) -> momentum encoder -> negative keys
                      |
                      |  (diupdate perlahan)
                      v
sampel saat ini -> online encoder -> query  -> contrastive loss dengan negative keys dari queue
```
Sampel-sampel lama yang tersimpan dalam *queue* diproses oleh *momentum encoder* untuk menghasilkan representasi yang berfungsi sebagai *negative keys*. Sementara itu, sampel pada *batch* saat ini diproses oleh *online encoder* untuk menghasilkan *query*. *Contrastive loss* kemudian dihitung antara *query* dan kumpulan *negative keys* yang diambil dari *queue*. Pendekatan ini memungkinkan jumlah negatif yang jauh melampaui ukuran *batch* aktual.

Kunci stabilitas pendekatan ini terletak pada pembaruan parameter *momentum encoder*. *Momentum encoder* merupakan salinan terstruktur dari *online encoder*, namun parameter-parameternya tidak diperbarui sepenuhnya setiap langkah optimisasi. Sebaliknya, pembaruan dilakukan secara bertahap menggunakan rata-rata bergerak eksponensial (*exponential moving average*) terhadap bobot *online encoder*. Hal ini memastikan bahwa representasi negatif yang dihasilkan tetap konsisten dan tidak berfluktuasi drastis antar-*step*, sehingga proses kontrastif menjadi lebih stabil dan andal.

Dengan memisahkan kebutuhan akan jumlah sampel negatif yang besar dari keterbatasan ukuran *batch*, metode ini secara efektif menghemat sumber daya komputasi sambil mempertahankan kualitas representasi. Konsep dasar ini kemudian dikembangkan lebih lanjut menjadi kerangka kerja lengkap yang dinamakan MoCo, yang akan kita bedah lebih mendalam pada slide berikutnya.

---

## Slide 013 - MoCo: Momentum Contrast

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi mengapa kontrastive learning memerlukan volume negatif sampel yang besar agar representasi tidak mengalami collapse, serta bagaimana momentum encoder dapat digunakan untuk mengelola sampel yang disimpan dalam antrian. Langkah metodologis selanjutnya adalah melihat implementasi konkret dari konsep tersebut dalam arsitektur yang menjadi standar de facto di bidang self-supervised learning, yaitu MoCo atau Momentum Contrast.

MoCo, yang diperkenalkan oleh He dkk. pada tahun 2020, menggabungkan dua inovasi struktural sekaligus: penggunaan momentum encoder dan penerapan dynamic dictionary. Desain ini secara eksplisit menjawab batasan komputasi dalam pembelajaran kontras tanpa mengandalkan pembesaran batch size yang tidak realistis. Komponen inti dalam MoCo dapat diuraikan sebagai berikut:

- **Query encoder**: memproses view positif dari satu sampel input saat ini.
- **Key encoder**: memproses seluruh sampel lain yang tersimpan dalam dictionary.
- **Dictionary queue**: berfungsi sebagai buffer memori yang menyimpan embedding dari mini-batch sebelumnya.
- **Momentum update**: parameter key encoder mengikuti query encoder secara perlahan melalui eksponensial moving average.

Keunggulan utama dari arsitektur ini terletak pada efisiensi memori dan stabilitas optimisasi. Dengan dictionary queue, sistem mampu mengakses ribuan hingga jutaan negatif sampel berkualitas tinggi meskipun batch size fisik tetap kecil. Konsistensi representasi pada dictionary terjaga karena momentum encoder hanya berubah secara bertahap, sehingga mencegah fluktuasi gradien yang tajam dan menjaga distribusi latent space tetap stabil selama pelatihan.

Dari perspektif riset tingkat doktor, penting untuk menekankan bahwa MoCo bukan sekadar rekayasa teknis, melainkan fondasi metodologis yang mengubah cara kita merancang loss function dan pipeline ekstraksi fitur. Prinsip query-key-queue-momentum ini kemudian menjadi blueprint bagi banyak varian SSL berbasis kontras berikutnya, termasuk pengembangan arsitektur Vision Transformer yang dioptimalkan untuk pembelajaran tanpa label, serta integrasinya ke dalam framework seperti timm dan PyTorch untuk eksperimen skala besar.

Menghubungkan dengan slide berikutnya, mekanisme konsistensi representasi dan pembaruan parameter bertahap yang dibangun MoCo akan berevolusi ke dalam paradigma teacher-student learning. Di sana, prinsip distilasi pengetahuan tanpa label akan memanfaatkan stop-gradient dan momentum secara lebih eksplisit, di mana model student secara aktif berusaha menyamai output stabil dari model teacher yang terus mengakumulasi pengetahuan secara perlahan.

---

## Slide 014 - Teacher-Student Learning dalam SSL

### Narasi

Slide ini melanjutkan konsep momentum encoder yang diperkenalkan pada MoCo sebelumnya, dengan menggeser fokus ke kerangka kerja *teacher-student* yang menjadi tulang punggung banyak metode *self-supervised learning* modern. Pada tingkat doktoral, penting untuk memahami bahwa mekanisme ini bukan sekadar duplikasi arsitektur, melainkan strategi regulasi representasi yang menjaga stabilitas target tanpa bergantung pada sampel negatif dalam jumlah masif.

Prinsip dasar *teacher-student learning* dalam konteks SSL merujuk pada *knowledge distillation*, di mana model *student* dilatih untuk meniru distribusi probabilitas atau embedding dari model *teacher*. Kedua jaringan dapat menggunakan arsitektur backbone dan head yang identik, namun parameter mereka diperbarui secara independen. Keunikan pendekatan ini terletak pada ketiadaan label eksternal; *teacher* tidak menerima supervisi langsung, melainkan memperoleh konsistensi melalui pembaruan *momentum* atau operasi *stop-gradient* terhadap gradien dari *student*.

Perhatikan skema alur data pada slide:

```text
Input -> student (backbone + head) -> prediksi
  |
  +----> teacher (momentum backbone + head) -> prediksi target
  student berusaha menyamai target teacher
```

Diagram tersebut menggambarkan proses paralel di mana satu input citra mengalami augmentasi berbeda sebelum diproses oleh kedua jaringan. *Teacher* menghasilkan target representasi yang bersifat stabil karena parameternya diperbarui secara eksponensial bergerak (*exponential moving average*) atau dibekukan gradiennya. Sementara itu, *student* melakukan optimisasi aktif untuk meminimalkan selisih antara prediksinya sendiri dengan target yang dihasilkan *teacher*. Fungsi kerugian (*loss function*), biasanya berbasis KL-divergence atau MSE, dihitung secara langsung antara keluaran kedua modul ini.

Dinamika pembaruan parameter menciptakan asimetri kontrol yang krusial. Karena *teacher* hanya mengumpulkan pengetahuan secara perlahan dari *student*, ia berfungsi sebagai *moving average* dari representasi historis jaringan. Hal ini memastikan bahwa target yang diberikan kepada *student* tetap koheren sepanjang pelatihan, sehingga *student* belajar mengekstrak fitur yang invarian terhadap augmentasi tanpa terjebak dalam solusi trivial. Mekanisme ini secara fundamental mengubah paradigma pembelajaran tanpa label menjadi proses konsistensi representasi jangka panjang.

Konsep *teacher-student* yang dijelaskan pada slide ini merupakan fondasi arsitektural yang akan dikembangkan lebih lanjut pada materi berikutnya mengenai DINO. Dengan menggabungkan prinsip distilasi ini bersama objektif kontrastif yang menghilangkan kebutuhan sampel negatif, serta teknik *centering* dan *sharpening* pada softmax, DINO berhasil mencapai performa state-of-the-art dalam ekstraksi fitur semantik. Pemahaman mendalam tentang dinamika pembaruan parameter dan stabilitas target pada slide saat ini akan sangat relevan ketika kita membedah komponen teknis dan analisis kritis paper DINO pada slide selanjutnya.

---

## Slide 015 - DINO: Self-Distillation with No Labels

### Narasi

Slide ini memperkenalkan DINO atau *DIStillation with NO labels*, sebuah kerangka kerja self-supervised learning yang diusulkan oleh Caron dkk. pada tahun 2021. DINO mengambil prinsip teacher-student learning yang telah kita diskusikan pada slide sebelumnya, namun menyempurnakannya dengan menghilangkan kebutuhan akan pasangan negatif (*negative samples*) yang umum digunakan dalam metode kontrastif tradisional. Nama DINO secara harfiah merujuk pada mekanisme distilasi pengetahuan yang berjalan sepenuhnya tanpa supervisi label eksternal.

Berikut adalah alur arsitektur yang menjadi inti dari DINO:

```text
x -> view1 (local crop) -> student -> softmax center -> cross-entropy loss
x -> view2 (global crop) -> teacher -> softmax center -> (target)
student berusaha menyamai target teacher
```

Pada skema di atas, satu citra input $x$ diproses menjadi dua augmentasi berbeda. Augmentasi lokal (*local crop*) dialirkan ke arsitektur student yang dilengkapi dengan head proyeksi dan fungsi softmax centered. Sebaliknya, augmentasi global (*global crop*) diproses oleh arsitektur teacher yang juga menggunakan softmax centered. Objective pelatihan diformulasikan sebagai *cross-entropy loss* antara distribusi probabilitas output student dan output teacher. Dengan demikian, model dituntut untuk menghasilkan representasi yang invariant terhadap variasi augmentasi, sekaligus mempertahankan konsistensi semantik antar view.

Keunggulan operasional DINO terletak pada mekanisme pembaruan parameter teacher. Alih-alih menghitung gradien langsung ke teacher, parameter teacher diperbarui secara eksponensial melalui momentum dari parameter student. Pendekatan ini menciptakan target pembelajaran yang stabil dan bergerak perlahan, sehingga student dapat mempelajari fitur hierarkis yang lebih robust tanpa mengalami fluktuasi gradien yang drastis.

Namun, tanpa regulasi khusus, mekanisme distilasi murni rentan mengalami representasi collapse di mana seluruh sampel terkompresi ke satu cluster tunggal. Untuk mencegah hal tersebut, DINO menerapkan dua komponen kritis: centering dan sharpening. Centering mengurangi rata-rata output teacher agar distribusi tidak menyimpang ekstrem, sementara sharpening meningkatkan ketajaman distribusi probabilitas dengan menurunkan suhu pada fungsi softmax. Kombinasi ini menjaga keseimbangan antara invariansi spasial dan pemisahan semantik antar objek. Pembahasan teknis mengenai implementasi centering dan sharpening serta dampaknya terhadap stabilitas pelatihan akan kita uraikan secara rinci pada slide berikutnya.

---

## Slide 016 - Komponen DINO: Centering dan Sharpening

### Narasi

Pada slide sebelumnya, kita telah menguraikan kerangka kerja DINO yang menggabungkan self-distillation antara student dan teacher tanpa bergantung pada label maupun sampel negatif. Meskipun pendekatan ini elegan, pelatihan self-supervised murni sangat rentan terhadap fenomena mode collapse, di mana seluruh representasi model berkontraksi menuju satu titik atau cluster tunggal. Untuk menjamin stabilitas konvergensi dan menjaga keragaman fitur, DINO menerapkan dua mekanisme regulasi yang menjadi pilar utama: centering dan sharpening.

Slide ini menyajikan formulasi eksplisit dari komponen centering:
```text
center_t = mean(output_teacher)  # diperbarui eksponensial
prediksi_teacher = softmax(output_teacher - center_t)
```
Rumus ini menunjukkan bahwa nilai pusat $c_t$ dihitung sebagai rata-rata bergerak eksponensial dari seluruh logits teacher sepanjang epoch. Nilai tersebut kemudian dikurangkan dari output teacher sebelum melewati fungsi softmax. Secara matematis, operasi ini menetralkan bias drift pada distribusi logit, sehingga mencegah neuron tertentu mendominasi gradien. Dengan kata lain, centering memastikan bahwa ruang representasi tetap tersebar merata dan tidak terjebak dalam mode collapse.

Di sisi lain, sharpening bekerja melalui manipulasi parameter suhu (temperature) pada fungsi softmax. Ketika temperature diturunkan, softmax akan menekan probabilitas kecil dan memperkuat probabilitas tinggi, menghasilkan distribusi yang lebih tajam dan mendekati one-hot encoding. Efek ini memaksa student untuk mempelajari batas keputusan yang lebih diskrit dan terdefinisi jelas, sekaligus memberikan sinyal error yang lebih informatif dibandingkan distribusi probabilitas yang terlalu halus. Kombinasi keduanya menciptakan keseimbangan optimal antara invariansi terhadap augmentasi spasial dan pemisahan klaster semantik yang tajam.

Jika kita merujuk pada tabel ringkasan pada slide, keempat elemen inti DINO beroperasi secara sinergis. Centering memangkas risiko mode collapse, sharpening mendorong prediksi yang tegas, multi-crop memperkaya konteks lokal dan global untuk memperkuat robustness, sedangkan momentum update pada teacher menstabilkan target yang diberikan kepada student. Dari perspektif penelitian tingkat doktoral, kombinasi ini membuktikan bahwa desain objective function dan strategi regularisasi dapat sepenuhnya menggantikan peran supervision eksternal dalam membentuk representasi visual yang bermakna.

Stabilitas pelatihan yang dijaga oleh komponen-komponen ini bukan hanya pencapaian teknis, melainkan fondasi yang memungkinkan model mengeksploitasi struktur internal arsitektur secara maksimal. Ketika representasi sudah terkondisi dengan baik melalui centering dan sharpening, DINO mulai menampilkan sifat-sifat emergent yang tidak pernah secara eksplisit dimandatkan dalam loss function-nya. Fenomena inilah yang akan kita analisis pada slide berikutnya, khususnya mengenai kemunculan self-attention map yang mampu melakukan segmentasi implisit, karakteristik part-based, serta implikasinya terhadap transfer learning dan evaluasi state-of-the-art dalam computer vision modern.

---

## Slide 017 - Emergent Properties pada DINO

### Narasi

Pada pembahasan sebelumnya, kita telah menguraikan bagaimana mekanisme centering dan sharpening bekerja bersama dengan momentum teacher untuk menstabilkan proses pembelajaran self-supervised. Tujuan utamanya adalah menjaga keseimbangan antara invariansi spasial dan pemisahan klaster semantik agar model tidak jatuh ke dalam mode collapse. Ketika mekanisme ini diterapkan pada arsitektur Vision Transformer, model tidak hanya memenuhi objective kontrastif, tetapi juga mengembangkan sifat-sifat emergen yang tidak dirancang secara eksplisit.

Berdasarkan observasi empiris, DINO menghasilkan empat karakteristik menonjol yang relevan untuk kajian tingkat doktoral:
- Peta self-attention pada lapisan akhir transformer secara implisit melakukan segmentasi objek terhadap latar belakang tanpa memerlukan anotasi bounding box atau mask.
- Representasi fitur yang dihasilkan mampu mendukung tugas segmentasi tanpa label (unsupervised segmentation) dengan akurasi yang kompetitif dibandingkan metode tradisional.
- Fitur yang dipelajari bersifat part-based, di mana fokus perhatian model secara konsisten menyoroti bagian-bagian spesifik objek seperti kepala, kaki, atau badan, mencerminkan pemahaman struktural hierarkis.
- Kinerja transfer ke downstream task menunjukkan peningkatan yang signifikan, terutama ketika backbone berbasis Vision Transformer digunakan, membuktikan bahwa ruang embedding yang terbentuk sangat kaya informasi semantik.

Fenomena ini memicu pertanyaan kritis dalam komunitas riset: apakah properti tersebut muncul karena struktur arsitektur ViT, atau murni akibat desain objective DINO? Hasil eksperimen ablation dalam literatur DINO menegaskan bahwa keduanya berinteraksi secara sinergis. ViT menyediakan kapasitas pemodelan dependensi jarak jauh yang diperlukan untuk memahami konteks global, sedangkan objective DINO memaksa model untuk mengorganisir ruang embedding berdasarkan kesamaan makna. Tanpa kombinasi ini, representasi yang terbentuk cenderung dangkal, terlalu terfragmentasi, atau kehilangan invariansi yang dibutuhkan untuk generalisasi.

Sifat-sifat emergen ini menjadi landasan penting untuk memahami bagaimana model belajar konsep visual secara mandiri. Ketika representasi DINO diekstrak, distribusi embeddingnya secara alami membentuk kelompok-kelompok yang bermakna secara semantik, bahkan tanpa supervisi manusia. Pembahasan mengenai bagaimana klasterisasi otomatis ini terbentuk, bagaimana pseudo-label yang dihasilkan dapat divalidasi, dan implikasinya terhadap desain eksperimen penelitian akan kita lanjutkan pada slide berikutnya.

---

## Slide 018 - Semantic Clustering Otomatis

### Narasi

Pada slide sebelumnya, kita telah menguraikan bagaimana DINO menghasilkan properti emergent yang tidak diminta secara eksplisit, seperti pemetaan self-attention yang secara implisit memisahkan bagian-bagian objek dan menunjukkan karakter part-based. Sifat representasi ini tidak hanya bersifat geometris, tetapi juga membawa struktur semantik yang tertanam rapat di dalam ruang embedding. Ketika representasi tersebut diekstrak dari kumpulan gambar tanpa melibatkan anotasi manual, kita dapat mengamati fenomena *semantic clustering* yang muncul secara alami akibat optimasi objective DINO.

Proses ekstraksi dan pengelompokan ini dapat disederhanakan menjadi alur komputasi berikut:
```text
Embedding DINO -> k-means -> pseudo-label -> evaluasi dengan label sebenarnya
```
Alur ini menggambarkan bahwa setelah model DINO menghasilkan embedding berdimensi tinggi untuk setiap patch atau gambar utuh, kita langsung menerapkan algoritma k-means sederhana pada ruang fitur tersebut. Hasilnya adalah kelompok-kelompok atau *pseudo-classes* yang secara mengejutkan koheren secara semantik. Meskipun akurasi pseudo-label ini tidak setara dengan anotasi ahli, nilai utamanya terletak pada kemampuannya mengungkap struktur intrinsik dan hierarki kategori yang tersimpan dalam data.

Pseudo-label yang dihasilkan memiliki beberapa aplikasi strategis dalam pipeline penelitian dan eksperimen:
- **Inisialisasi clustering:** Memberikan titik awal yang lebih informatif daripada inisialisasi acak pada tugas pengelompokan downstream.
- **Self-training:** Berfungsi sebagai sinyal supervisi iteratif, di mana model dilatih ulang menggunakan prediksi berkepercayaan tinggi (*high-confidence predictions*) untuk memperluas cakupan data yang terlabelisasi.
- **Analisis struktur data:** Memetakan keragaman, redundansi, dan kesenjangan kelas dalam dataset, yang sangat berguna untuk merancang strategi sampling, kurasi data, atau augmentasi yang lebih presisi.

Dari perspektif riset tingkat doktoral, pengamatan ini menjadi landasan empiris untuk menjawab pertanyaan fundamental: apakah model vision transformer benar-benar belajar konsep objek yang abstrak, atau sekadar menghafal korelasi statistik permukaan? Kita dapat merancang eksperimen kontrol untuk menguji ketahanan (*robustness*) cluster tersebut ketika domain data mengalami pergeseran, misalnya dari gambar natural ke domain medis, satelit, atau mikroskopis. Jika struktur semantik tetap stabil meskipun terjadi *domain shift*, hal ini mengindikasikan bahwa representasi yang dipelajari bersifat umum dan invariant terhadap variasi tekstur maupun pencahayaan, sehingga layak dijadikan fondasi untuk arsitektur *foundation model*.

Pemahaman mengenai clustering semantik ini melengkapi ekosistem pembelajaran tanpa pengawasan dan membuka jalan menuju paradigma SSL berikutnya yang lebih eksplisit dalam memaksa model memahami ketergantungan spasial dan kontekstual. Pada slide selanjutnya, kita akan beralih ke pendekatan *Masked Image Modeling* atau MIM, di mana sebagian patch citra sengaja disembunyikan dan model ditantang untuk merekonstruksinya. Pendekatan ini mengisi celah yang belum sepenuhnya tercover oleh metode kontrastive, dengan menambahkan komponen rekonsruksi yang memperkuat pemahaman struktural model sebelum diaplikasikan pada tugas deteksi, segmentasi, atau generasi citra.

---

## Slide 019 - Masked Image Modeling (MIM)

### Narasi

Setelah membahas bagaimana embedding dari model seperti DINO mampu membentuk klaster semantik yang koheren tanpa memerlukan label manual, kita kini beralih ke salah satu pilar utama self-supervised learning modern: Masked Image Modeling atau MIM. Berbeda dengan pendekatan kontrastif yang mengandalkan pencocokan representasi antar augmented views, MIM memanfaatkan struktur intrinsik citra dengan menyembunyikan sebagian input dan menuntut model untuk memulihkan bagian yang hilang. Paradigma ini memaksa jaringan untuk belajar prior geometri, tekstur, dan hubungan kontekstual antar wilayah citra, bukan sekadar fitur permukaan yang rentan terhadap augmentasi artifisial.

Mekanisme ini dapat dianalogikan dengan pelatihan BERT pada pemrosesan bahasa alami, di mana token kalimat tertentu ditutup dan model dilatih untuk menebaknya berdasarkan konteks sekitarnya. Pada domain visi komputer, analogi ini diterjemahkan ke dalam grid patch. Sebagian patch di-mask secara acak, dan arsitektur model harus merekonstruksi nilai pixel asli atau representasi latent dari patch tersebut. Aliran komputasinya dapat diringkas sebagai berikut:

```text
Citra asli -> [x1 x2 x3 x4] -> mask x2,x4 -> model -> prediksi x2,x4 -> loss vs asli
```

Beberapa metode utama yang mengadopsi prinsip MIM menunjukkan variasi signifikan dalam target prediksi dan desain decoder. Berikut adalah perbandingan mendasar yang perlu diperhatikan dari perspektif metodologi riset:
- **MAE**: Langsung memprediksi nilai pixel kontinu pada patch yang di-mask menggunakan decoder ringan.
- **SimMIM**: Mengoptimalkan distilasi pengetahuan dengan decoder yang lebih efisien untuk memetakan representasi encoder ke pixel target, mengurangi beban komputasi decoder.
- **BEiT**: Tidak bekerja langsung pada ruang pixel, melainkan melakukan diskritisasi citra terlebih dahulu menggunakan discrete VAE (dVAE), lalu melatih model untuk memprediksi token visual kategorikal yang hilang.

Dari sudut pandang penelitian tingkat doktoral, MIM menawarkan trade-off yang menarik antara kompleksitas tugas dan kualitas representasi. Rasio masking yang tinggi mengubah rekonstruksi dari tugas lokal sederhana menjadi masalah inferensi global, sehingga mendorong model untuk menangkap dependensi semantik jarak jauh. Namun, keberhasilan pendekatan ini sangat bergantung pada stabilitas training, desain fungsi loss, dan kemampuan decoder untuk menghindari collapse ke mean statistik dataset.

Pembahasan ini menjadi fondasi langsung untuk slide berikutnya yang akan membedah arsitektur spesifik MAE (Masked Autoencoders). Kita akan menganalisis mengapa penggunaan rasio masking hingga 75% justru meningkatkan efisiensi komputasi encoder sambil memperkuat kapasitas generalisasi model, serta mengevaluasi secara kritis perbedaan karakteristik antara evaluasi melalui linear probing versus fine-tuning ketika dibandingkan dengan metode self-supervised kontrastif tradisional.

---

## Slide 020 - MAE: Masked Autoencoders

### Narasi

Slide ini mengupas arsitektur Masked Autoencoder atau MAE yang diperkenalkan oleh He dan tim pada tahun 2022. Metode ini merupakan realisasi paling matang dari konsep Masked Image Modeling yang telah kita paparkan pada slide sebelumnya. Berbeda dengan paradigma kontrastif yang mengandalkan pencocokan pasangan augmentasi, MAE memanfaatkan masking dalam skala besar untuk memaksa model membangun pemahaman struktural dan semantik yang mendalam.

Alur pemrosesan data dalam MAE dapat diikuti melalui diagram berikut:
```text
Citra -> tokenize patch -> mask 75% patch -> encoder (hanya visible patch)
      -> representasi visible -> decoder -> rekonstruksi semua patch -> MSE loss
```
Proses dimulai dengan partisi citra menjadi patch-patch kecil yang kemudian ditokenisasi. Sebanyak tujuh puluh lima persen patch disembunyikan secara acak. Encoder hanya menerima patch yang terlihat, sehingga menghasilkan representasi laten yang ringkas. Representasi tersebut diteruskan ke decoder berparameter ringan yang bertugas merekonstruksi seluruh patch, termasuk yang di-mask. Fungsi kerugian yang dioptimalkan adalah Mean Squared Error antara patch asli dan patch hasil dekoding.

Rasio masking yang tinggi ini merupakan inti dari efektivitas MAE. Dengan menutup sebagian besar informasi visual, model tidak dapat lagi mengandalkan pola lokal sederhana dan terpaksa harus menginferensi konteks global serta hubungan spasial antar wilayah citra. Di sisi lain, efisiensi komputasi tetap terjaga karena encoder melewatkan perhitungan untuk patch yang di-mask, mempercepat iterasi pre-training secara signifikan dibandingkan metode yang memproses seluruh patch secara penuh.

Karakteristik representasi yang dihasilkan MAE menunjukkan profil kinerja yang spesifik. Untuk skenario fine-tuning pada downstream task seperti klasifikasi halus atau deteksi objek, MAE konsisten memberikan hasil yang sangat kuat dan sering kali melampaui baseline kontrastif. Namun, evaluasi melalui linear probing mengungkapkan performa yang lebih moderat. Hal ini mengindikasikan bahwa representasi MAE lebih bersifat adaptif dan memerlukan penyesuaian parameter menyeluruh untuk mengeksploitasi potensi penuhnya, berbeda dengan metode kontrastif yang cenderung menghasilkan fitur statis yang langsung siap pakai.

Pembahasan mengenai trade-off antara efisiensi, kesulitan tugas, dan karakteristik representasi ini menjadi fondasi kritis untuk slide berikutnya. Kita akan melakukan analisis komparatif sistematis antara pendekatan kontrastif dan Masked Image Modeling, mencakup aspek objective, jenis informasi yang ditangkap, beban komputasi, serta implikasi strategisnya dalam perancangan riset computer vision tingkat lanjut.

---

## Slide 021 - Perbandingan Contrastive Learning vs Masked Image Modeling

### Narasi

Setelah membahas mekanisme Masked Autoencoder pada slide sebelumnya, kita kini mengevaluasi perbandingan fundamental antara dua paradigma dominan dalam self-supervised learning untuk computer vision: contrastive learning dan masked image modeling. Tabel ini menyajikan pemetaan aspek-aspek kritis yang perlu dipertimbangkan secara eksplisit ketika merancang eksperimen atau memilih baseline untuk penelitian tingkat doktor.

Perbedaan paling mendasar terletak pada objective training. Metode kontrastif seperti SimCLR, MoCo, atau DINO dirancang untuk menyamakan representasi antar view yang mengalami augmentasi berbeda. Sebaliknya, masked image modeling seperti MAE menuntut model untuk merekonstruksi patch yang di-mask. Konsekuensinya, informasi yang ditangkap oleh masing-masing pendekatan juga berbeda:
- Contrastive learning menghasilkan invariansi terhadap augmentasi dan menangkap semantik global.
- Masked image modeling memaksa pembelajaran struktur spasial, tekstur lokal, dan dependensi kontekstual antar patch.

Evaluasi representasi menunjukkan pola yang konsisten dengan karakteristik objective tersebut. Untuk linear probing, metode kontrastif umumnya memberikan performa yang lebih stabil dan kuat. Namun, masked image modeling sering kali mendominasi pada fase fine-tuning, berkat representasi yang lebih kaya secara detail. Dari sisi efisiensi, biaya komputasi contrastive learning sangat bergantung pada ukuran batch dan jumlah sampel negatif, sedangkan MIM lebih ringan karena encoder hanya memproses subset patch yang visible.

Implikasi riset yang perlu ditekankan adalah bahwa keunggulan suatu metode bersifat kontekstual terhadap downstream task. Jika target aplikasi Anda membutuhkan pemahaman spasial presisi seperti segmentasi semantik atau deteksi objek, foundation berbasis MIM cenderung lebih adaptif. Sebaliknya, untuk klasifikasi gambar, retrieval, atau clustering berbasis makna, pendekatan kontrastif tetap menjadi standar industri. Tren metodologis terkini justru mengarah pada sinergi: menggabungkan kedua paradigma dapat menutupi kelemahan masing-masing dan menghasilkan representasi yang lebih robust.

Perspektif ini menjadi landasan langsung untuk slide berikutnya, yaitu DINOv2. Model tersebut secara eksplisit mengimplementasikan filosofi integratif dengan menggabungkan objective kontrastif, masked reconstruction, clustering loss, serta koordinat patch, semuanya dilatih pada dataset kurasi skala besar untuk menghasilkan fitur visual universal tanpa label.

---

## Slide 022 - DINOv2: Robust Visual Features without Supervision

### Narasi

Slide ini membahas DINOv2, yang merupakan evolusi arsitektural dari DINO asli untuk menghasilkan representasi visual universal tanpa ketergantungan pada label supervisi. Mengacu pada diskusi slide sebelumnya mengenai trade-off antara contrastive learning dan masked image modeling, DINOv2 tidak lagi memandang kedua pendekatan tersebut sebagai pilihan yang saling eksklusif. Sebaliknya, model ini mengadopsi filosofi hybrid yang mengintegrasikan kekuatan masing-masing metode sekaligus menambahkan mekanisme penguatan representasi lainnya.

Secara komputasional, DINOv2 mengoptimalkan empat komponen loss secara bersamaan. Objective contrastif standar dari DINO dipertahankan untuk menjaga invariansi terhadap augmentasi. Masked image modeling diimplementasikan melalui iBOT loss yang memaksa jaringan merekonstruksi patch yang disembunyikan, sehingga memperkuat pemahaman struktur lokal. SwAV loss ditambahkan untuk melakukan clustering representasi secara online, yang menstabilkan pembelajaran tanpa supervision. Terakhir, informasi koordinat patch disisipkan ke dalam proses forward pass agar model dapat mempelajari posisi relatif spasial, yang secara langsung meningkatkan kemampuan lokalisasi objek.

Rangkuman integrasi multi-objective tersebut dapat dilihat pada persamaan berikut:
```text
DINOv2 = DINO loss + iBOT loss (masked) + SwAV loss + Koordinat patch
```
Kombinasi ini menciptakan ruang fitur yang sangat kaya, di mana aspek semantik global dan detail tekstural-spatial saling melengkapi. Hasilnya adalah representasi yang terbukti robust dan generalisasi tinggi untuk berbagai downstream task, baik dalam skenario linear probing maupun full fine-tuning.

Untuk mencapai kapasitas representasi tersebut, DINOv2 dilatih pada dataset berskala masif yang dikurasi secara mandiri, yaitu LVD-142M. Keberhasilan model ini tidak hanya bergantung pada desain arsitektur, tetapi juga pada kualitas distribusi visual yang terkandung dalam dataset. Pada slide berikutnya, kita akan mengurai pipeline otomatis yang digunakan untuk membangun LVD-142M, mulai dari web crawling, deduplikasi, filtering berbasis kualitas, hingga retrieval menggunakan embedding. Diskusi ini akan menegaskan prinsip kunci dalam riset foundation models: strategi kurasi data memiliki bobot strategis yang setara dengan inovasi arsitektur jaringan saraf.

---

## Slide 023 - DINOv2: Strategi Pengumpulan Data

### Narasi

Setelah membahas komponen loss function dan arsitektur DINOv2 pada slide sebelumnya, kita kini menyoroti fondasi metodologis yang memungkinkan model mencapai representasi universal: strategi pengumpulan dan kurasi data berskala masif. DINOv2 tidak bergantung semata-mata pada inovasi arsitektural, melainkan pada pipeline otomatis yang dirancang untuk membangun corpus LVD-142M dari sumber web terbuka.

```text
Web crawl -> deduplikasi -> filtering -> retrieval berbasis embedding -> kurasi -> LVD-142M
```

Pipeline ini beroperasi melalui serangkaian tahap yang saling bertumpuk secara kritis. Tahap **web crawl** mengumpulkan jutaan citra mentah secara paralel dari berbagai sumber daring. Selanjutnya, **deduplikasi** menerapkan teknik seperti perceptual hashing atau cosine similarity pada embedding awal untuk mengidentifikasi dan menghapus citra identik atau hampir identik, sehingga mengurangi redundansi dan noise komputasional. Tahap **filtering** menggunakan heuristik serta classifier ringan untuk menyaring gambar berkualitas rendah, mengandung watermark berlebihan, atau konten yang keluar dari domain visual umum. Kemudian, **retrieval berbasis embedding** memanfaatkan model proxy untuk memilih subset citra yang paling mirip dengan distribusi target, memastikan cakupan visual yang beragam dan seimbang. Terakhir, **kurasi** dilakukan sebagai validasi akhir sebelum seluruh subset digabungkan menjadi dataset LVD-142M.

Tujuan strategis dari pipeline ini adalah merepresentasikan distribusi visual dunia secara holistik, bukan sekadar mengoptimalkan performa pada benchmark tertutup. Dari perspektif riset tingkat doktoral, terdapat dua implikasi metodologis yang harus menjadi perhatian utama. Pertama, kualitas kurasi data memiliki bobot yang setara dengan desain arsitektur model. Tanpa pipeline pembersihan data yang rigor, bahkan model paling mutakhir akan rentan terhadap overfitting terhadap artefak web atau degradasi generalisasi. Kedua, bias yang tersisa atau tidak terdeteksi selama proses pengumpulan akan terekam secara permanen dalam ruang embedding. Hal ini menuntut peneliti untuk secara eksplisit memetakan bias dataset, melakukan analisis fairness, dan mendokumentasikan batas-batas transferability model sebelum mengklaim kontribusi ilmiahnya.

Ketika representasi telah terbentuk melalui pipeline data yang ketat, langkah selanjutnya adalah mengukur kualitas embedding tersebut tanpa mengubah bobot backbone. Pada slide berikutnya, kita akan membahas protokol evaluasi standar yang disebut linear probing, yang berfungsi sebagai indikator objektif seberapa baik fitur yang dipelajari oleh DINOv2 dapat memisahkan kelas secara langsung dan murni mencerminkan kualitas representasi yang dihasilkan.

---

## Slide 024 - Evaluasi Representasi: Linear Probing

### Narasi

Setelah membahas strategi pengumpulan dan kurasi data berskala besar pada DINOv2 yang menghasilkan dataset LVD-142M, langkah metodologis berikutnya adalah mengukur seberapa bermakna representasi yang dipelajari oleh model dari data tersebut. Di sinilah protokol evaluasi standar bernama *linear probing* berperan krusial. Metode ini dirancang khusus untuk menilai kualitas fitur tanpa melakukan modifikasi terhadap bobot backbone model, sehingga kita dapat mengisolasi kinerja murni dari representasi yang dihasilkan oleh proses self-supervised learning.

Prosedur linear probing berjalan dengan mekanisme yang terstruktur dan efisien. Pertama, seluruh data pelatihan dilewatkan melalui backbone model yang telah dibekukan (*frozen*) untuk mengekstrak embedding atau vektor fitur. Kedua, sebuah klasifier linear sederhana, seperti regresi logistik atau lapisan fully connected tunggal, dilatih secara eksklusif pada embedding tersebut. Ketiga, akurasi prediksi diukur pada data uji. Alur komputasinya dapat direpresentasikan sebagai berikut:
```text
Embedding backbone (frozen) -> Linear classifier -> Prediksi label
```
Karena tidak ada pembaruan gradien pada backbone selama fase pelatihan klasifier, hasil akhir secara langsung mencerminkan seberapa baik kelas-kelas target sudah terpisah secara linear di ruang fitur. Tidak ada bobot backbone yang dimodifikasi, sehingga metrik yang diperoleh murni mencerminkan kualitas embedding itu sendiri.

Interpretasi hasil akurasi menjadi kunci dalam analisis kritis pada tingkat doktoral. Akurasi tinggi menunjukkan bahwa representasi sudah memisahkan kelas secara linear, menandakan adanya pemetaan semantik yang kuat antar kategori visual. Sebaliknya, akurasi rendah mengindikasikan bahwa informasi kelas mungkin masih tersimpan dalam hubungan non-linear yang kompleks, atau fitur yang dihasilkan cenderung bersifat tekstural daripada konseptual. Pendekatan ini memungkinkan peneliti membedakan antara kapasitas adaptasi model versus kualitas intrinsik representasinya, yang merupakan fondasi penting dalam merancang eksperimen computer vision yang robust.

Pembahasan mengenai signifikansi metrik ini akan terus dikembangkan pada slide berikutnya, khususnya dalam konteks mengapa linear probing menjadi tolok ukur esensial untuk memisahkan kualitas representasi dari kemampuan fine-tuning. Ketika model foundation mengalami fine-tuning bebas, ia selalu mampu menyesuaikan diri, sehingga sulit menilai apakah representasi dasarnya benar-benar bermakna. Linear probing memberikan jawaban objektif mengenai kelayakan fitur untuk downstream tasks, sekaligus membuka peluang identifikasi *research gap* terkait optimalisasi ekstraksi fitur semantik dalam arsitektur vision modern.

---

## Slide 025 - Mengapa Linear Probing Penting?

### Narasi

Setelah pada slide sebelumnya kita membahas prosedur standar linear probing sebagai protokol evaluasi representasi tanpa melakukan fine-tuning, kini kita perlu memahami alasan mendasar mengapa metode ini menjadi sangat krusial dalam konteks model-model skala besar. Linear probing secara eksplisit memisahkan dua aspek yang sering tertukar: kualitas representasi intrinsik dari model versus kemampuannya untuk beradaptasi melalui penyesuaian bobot.

Ketika kita mengizinkan fine-tuning berjalan bebas pada model arsitektur yang sangat besar, model tersebut hampir selalu dapat menyesuaikan diri dengan tugas target. Hal ini justru menyulitkan peneliti untuk menilai apakah representasi yang dipelajari oleh backbone memang sudah bermakna, atau sekadar hasil dari kapasitas optimisasi yang masif. Dengan membekukan seluruh bobot backbone dan hanya melatih klasifier linear di atasnya, kita mendapatkan ukuran murni terhadap kualitas embedding yang dihasilkan.

Protokol ini mengungkap tiga hal fundamental yang tidak bisa diukur hanya dengan melihat performa akhir setelah fine-tuning:
- Apakah fitur yang diekstraksi sudah bersifat linearly separable antar kelas.
- Apakah informasi semantik tentang kelas telah tersimpan secara eksplisit dalam ruang embedding.
- Apakah representasi tersebut benar-benar menangkap pola semantik tingkat tinggi, atau hanya mengandalkan tekstur dan pola lokal yang dangkal.

Perhatikan interpretasi dari blok teks pada slide ini. Jika akurasi linear probing tinggi, hal itu mengindikasikan bahwa model telah mempelajari fitur semantik yang kuat dan langsung dapat dimanfaatkan untuk tugas klasifikasi downstream. Sebaliknya, akurasi linear probing yang rendah menandakan bahwa informasi kelas masih memerlukan transformasi non-linear yang lebih kompleks untuk dipisahkan.

Sebagai contoh konkret, perhatikan perilaku Model MAE (Masked Autoencoder). MAE cenderung menunjukkan akurasi linear probing yang relatif rendah, namun performanya melonjak signifikan saat dilakukan fine-tuning penuh. Fenomena ini mengonfirmasi bahwa representasi yang dipelajari MAE sangat kaya secara spasial dan kontekstual, namun belum terstruktur secara linearly separable. Oleh karena itu, ia membutuhkan mekanisme adaptasi non-linear dari fine-tuning untuk mengeksploitasi kekayaan fitur tersebut secara optimal.

Pemahaman ini menjadi jembatan penting menuju penerapan praktis pada foundation vision models. Seperti yang akan kita bahas pada slide berikutnya, desain foundation model seperti DINOv2 memang dioptimalkan agar representasi frozen-nya sudah cukup kuat untuk langsung dipakai dengan linear probing. Namun, ketika terjadi pergeseran domain yang ekstrem, misalnya pada data medis atau citra satelit, strategi fine-tuning tetap diperlukan untuk menjembatani kesenjangan tersebut.

---

## Slide 026 - Transfer Learning dengan Foundation Vision Models

### Narasi

Setelah slide sebelumnya membahas bagaimana linear probing berfungsi sebagai indikator murni untuk menguji kualitas representasi tanpa bias dari kemampuan adaptasi model, kita kini menyoroti implementasinya dalam kerangka transfer learning berbasis foundation vision models. Model-model ini telah melalui proses pretraining berskala masif, sehingga menghasilkan ruang fitur yang umum dan siap diwariskan ke berbagai tugas downstream.

Dalam praktik deployment, transfer learning dengan foundation models umumnya dijalankan melalui dua skema utama:
- **Frozen backbone plus linear probing**: Arsitektur inti model dikunci, dan hanya head klasifikasi yang dilatih ulang. Skema ini sangat efisien secara komputasi dan direkomendasikan ketika jumlah data berlabel terbatas atau infrastruktur GPU terbatas.
- **Fine-tuning**: Seluruh atau sebagian layer backbone diperbarui selama pelatihan. Pendekatan ini lebih tepat ketika dataset target berukuran memadai dan terdapat kesenjangan distribusi yang signifikan antara data pretraining dan domain tugas baru.

Arsitektur seperti DINOv2 secara eksplisit dioptimalkan agar representasi dari frozen backbone sudah cukup kuat dan semantik, sehingga sering kali linear probing saja sudah mencapai performa yang kompetitif. Namun, kekuatan ini tidak bersifat universal lintas domain. Ketika target aplikasi berada pada ranah yang sangat spesifik atau jauh dari distribusi data pretraining—seperti pencitraan medis, satelit, atau mikroskop elektron—adaptasi lebih lanjut melalui fine-tuning tetap menjadi langkah yang diperlukan untuk menangkap karakteristik visual yang unik.

Kapan tepatnya fine-tuning harus diterapkan versus mengandalkan linear probing? Keputusan ini tidak bersifat mutlak dan sangat bergantung pada pertimbangan empiris, mulai dari volume data, jarak domain, resolusi input, hingga kendala memori dan risiko overfitting. Pada slide berikutnya, kita akan menguraikan faktor-faktor penentu tersebut secara sistematis, serta merancang protokol eksperimen untuk membandingkan kinerja kedua pendekatan tersebut berdasarkan variasi subset label.

---

## Slide 027 - Kapan Fine-tuning Diperlukan?

### Narasi

Pada slide sebelumnya, kita telah membahas dua mode transfer learning dengan foundation vision models: frozen backbone dengan linear probing, serta fine-tuning sebagian atau seluruh backbone. DINOv2 memang dirancang agar representasi frozennya sudah cukup kuat untuk berbagai tugas umum. Namun, dalam konteks penelitian tingkat lanjut, keputusan untuk melakukan fine-tuning tidak bersifat mutlak. Slide ini akan menguraikan faktor-faktor empiris yang menentukan kapan adaptasi lebih lanjut benar-benar diperlukan.

Keputusan fine-tuning bergantung pada beberapa variabel kritis berikut:
- **Jumlah label**: Dataset dengan ribuan hingga puluhan ribu kelas lebih aman dan efektif jika menggunakan fine-tuning dibanding linear probing.
- **Jarak domain**: Semakin jauh distribusi data target dari data pretraining (misalnya citra medis, satelit, atau mikroskopis), semakin tinggi kebutuhan adaptasi model.
- **Resolusi input**: Perbedaan resolusi yang signifikan dapat mengganggu positional embedding, sehingga memerlukan penyesuaian bobot melalui fine-tuning.
- **Ketersediaan komputasi**: Fine-tuning model besar menuntut memori GPU yang memadai dan alokasi waktu pelatihan yang realistis.
- **Risiko overfitting**: Penerapan fine-tuning pada dataset kecil tanpa regularisasi ketat berisiko tinggi menyebabkan memorisasi, bukan generalisasi.

Untuk memvalidasi keputusan ini secara ilmiah, mahasiswa disarankan merancang eksperimen komparatif terstruktur. Bandingkan kinerja linear probing versus fine-tuning pada berbagai subset label, misalnya 1%, 10%, dan 100%. Catat dan plot kurva akurasi terhadap proporsi label yang digunakan. Analisis tren ini akan menunjukkan titik break-even di mana biaya komputasi fine-tuning mulai memberikan return yang signifikan, sekaligus mengidentifikasi batas bawah dataset yang layak untuk pendekatan fine-tuning.

Temuan dari eksperimen kuantitatif ini akan menjadi jembatan menuju diskusi konseptual pada slide berikutnya. Setelah mengetahui kapan fine-tuning diperlukan, langkah selanjutnya adalah memahami apa sebenarnya yang sedang dipelajari model selama proses adaptasi tersebut. Apakah peningkatan performa didorong oleh penguatan representasi semantik objek, atau sekadar penyesuaian terhadap bias tekstural? Pertanyaan fundamental mengenai semantik versus tekstur pada DINOv2 akan kita bedah bersama untuk memperkuat justifikasi metodologis dalam penelitian disertasi Anda.

---

## Slide 028 - Pertanyaan Kunci: Semantik vs Tekstural

### Narasi

Setelah sebelumnya membahas faktor-faktor yang memengaruhi keputusan untuk melakukan fine-tuning, kini kita perlu menguji kualitas representasi itu sendiri sebelum memutuskan strategi adaptasi. Keputusan fine-tuning tidak hanya bergantung pada jumlah data atau jarak domain, tetapi juga pada apa yang sebenarnya dipelajari oleh model foundation selama fase self-supervised learning. Pertanyaan mendasar yang harus dijawab adalah apakah representasi yang dihasilkan benar-benar menangkap konsep semantik objek, atau sekadar menghafal pola tekstural dari data pretraining.

Penelitian terkini menunjukkan bahwa arsitektur CNN tradisional cenderung sangat bergantung pada tekstur permukaan daripada bentuk geometris objek. Hal ini menimbulkan kekhawatiran validitas ketika model tersebut diaplikasikan pada tugas downstream yang memerlukan pemahaman struktural. Untuk DINO dan DINOv2, klaim bahwa mereka belajar representasi semantik yang lebih kuat perlu dibuktikan secara empiris melalui pengujian yang terstruktur.

Kita dapat menguji perilaku kedua model tersebut menggunakan tiga pendekatan eksperimental berikut:
1. Menggunakan dataset dengan manipulasi tekstur, di mana objek tetap sama namun tekstur permukaannya diubah secara signifikan.
2. Melakukan perturbasi frekuensi dengan menghilangkan komponen frekuensi tinggi dari citra, sehingga hanya struktur global yang tersisa.
3. Menerapkan linear probing berbasis atribut untuk mengukur apakah embedding clustering mengikuti kategori kelas objek atau kategori kelas tekstur.

Pertanyaan riset inti yang bisa dikembangkan adalah apakah DINOv2 dalam domain tertentu lebih mengandalkan bentuk objek atau tekstur permukaan. Jawaban atas pertanyaan ini akan menentukan arah strategi fine-tuning. Jika representasi bersifat semantik, transfer learning dapat dilakukan dengan adaptasi minimal. Sebaliknya, jika model masih terjebak pada bias tekstural, fine-tuning menyeluruh atau mekanisme regularisasi khusus menjadi wajib untuk mencegah degradasi kinerja.

Pemahaman mengenai dikotomi semantik versus tekstural ini menjadi fondasi penting sebelum mengevaluasi kemampuan generalisasi model secara lebih luas. Pada slide berikutnya, kita akan beralih ke aspek transferability DINO dan DINOv2, di mana representasi tersebut akan diukur ketahanannya terhadap perubahan domain, resolusi, kelimpahan label, dan distribusi data yang belum pernah dilihat selama pretraining. Evaluasi ini akan menghasilkan rekomendasi konkret untuk pemilihan backbone dan desain eksperimen penelitian mahasiswa.

---

## Slide 029 - Fokus Riset: Transferability DINO dan DINOv2

### Narasi

Setelah pada slide sebelumnya kita menguji apakah representasi model benar-benar menangkap semantik objek atau hanya mengandalkan pola tekstural, langkah logis berikutnya adalah mengevaluasi seberapa kuat representasi tersebut bertahan ketika dihadapkan pada kondisi yang berbeda. Dalam konteks *Self-Supervised Learning* dan *Foundation Vision Models*, kemampuan ini disebut sebagai *transferability*. Transferability bukan sekadar akurasi tinggi pada data referensi, melainkan ketahanan representasi untuk tetap bermakna dan dapat dieksploitasi oleh kepala klasifikasi sederhana saat diterapkan pada distribusi data baru.

Untuk mengukur *transferability* secara sistematis dan dapat direplikasi, kita perlu membedahnya ke dalam empat dimensi kritis yang relevan dengan standar penelitian tingkat doktoral:
- **Domain**: Apakah embedding yang dihasilkan dari pretraining pada citra natural masih efektif ketika diadaptasi ke domain spesifik seperti medis, penginderaan jauh, atau inspeksi industri? Pergeseran domain sering kali mengungkap batas generalisasi dan bias bawaan model.
- **Resolusi**: Perubahan ukuran input dapat memengaruhi struktur patch pada *Vision Transformer*. Kita perlu mengamati apakah penurunan atau peningkatan resolusi merusak koherensi embedding atau justru mempertahankan informasi semantik kunci.
- **Jumlah Label**: Efisiensi pembelajaran sedikit sampel (*few-shot*) menjadi indikator kekuatan fitur bawaan. Pertanyaannya adalah berapa minimum anotasi yang diperlukan agar *linear probe* mencapai kinerja yang layak, tanpa memerlukan fine-tuning berat yang rentan overfitting.
- **Distribusi Data Baru**: Bagaimana model merespons data yang sama sekali tidak terwakili selama fase pretraining? Kemampuan ini menguji robustness representasi terhadap *out-of-distribution* shifts dan kesiapannya untuk deployment nyata.

Keluaran utama dari fokus riset ini adalah analisis komparatif yang ketat antara arsitektur DINO dan DINOv2, khususnya perbedaan performa antara varian backbone ringan seperti ViT-S dengan varian lebih besar seperti ViT-B. Hasil analisis ini akan diterjemahkan menjadi rekomendasi praktis mengenai kapan dan bagaimana mahasiswa sebaiknya memanfaatkan model fondasi ini dalam rancangan penelitian disertasi mereka. Pembahasan teoretis dan dimensi evaluasi ini akan langsung dioperasionalkan menjadi protokol eksperimen konkret pada slide berikutnya, yang merinci alur ekstraksi embedding, desain *linear probe*, serta kontrol metodologis yang wajib dipenuhi untuk memastikan validitas statistik dan reproduktibilitas hasil penelitian.

---

## Slide 030 - Desain Eksperimen Transferability

### Narasi

Slide ini menerjemahkan pertanyaan riset mengenai transferability dari slide sebelumnya menjadi kerangka eksperimen yang terstruktur, rigor, dan siap direplikasi. Diagram alur menunjukkan bahwa input domain dan resolusi tertentu dilewatkan ke dalam model fondasi DINO atau DINOv2 yang statusnya dibekukan (*frozen*). Pembekuan bobot ini menjamin bahwa evaluasi hanya mengukur kapasitas representasi bawaan model, tanpa bias dari fine-tuning penuh. Ekstrak embedding yang dihasilkan kemudian dialirkan ke dua jalur paralel: jalur pertama digunakan untuk melatih *linear probe* pada subset data berlabel guna menghasilkan metrik akurasi, sedangkan jalur kedua mengevaluasi embedding pada data uji untuk analisis distribusi fitur dan ketahanan model.

Protokol eksperimen dirancang secara bertahap untuk menguji setiap dimensi transferability yang telah diidentifikasi sebelumnya:
1. Pilih dataset target yang selaras dengan fokus penelitian, apakah citra medis, satelit, infrastruktur, atau domain industri lainnya.
2. Ekstrak embedding menggunakan resolusi asli dataset serta variasi resolusi lain untuk mengamati sensitivitas representasi terhadap perubahan skala spasial.
3. Latih *linear probe* pada empat skenario kelimpahan label: 1%, 10%, 50%, dan 100%. Pendekatan ini menguji efisiensi model dalam skenario *extreme few-shot* hingga *full-supervised*.
4. Ukur kinerja menggunakan akurasi atau F1-score, serta tambahkan metrik spesifik domain jika relevan dengan masalah penelitian.
5. Bandingkan hasil dengan baseline berbasis fitur terawasi, misalnya ResNet-50 yang dilatih pada ImageNet, untuk memposisikan kontribusi metodologi Anda relatif terhadap *state-of-the-art*.

Validitas ilmiah eksperimen ini sangat bergantung pada kontrol prosedur yang ketat. Catat secara eksplisit pengaturan *random seed*, strategi pembagian data latih-uji-validasi, serta konfigurasi *optimizer* dan *learning rate*. Laksanakan minimal tiga kali ulangan percobaan untuk setiap kombinasi kondisi eksperimen guna menghitung varians dan memastikan bahwa perbedaan kinerja bukan berasal dari fluktuasi stokastik. Dokumentasi parameter yang transparan ini merupakan standar wajib dalam penulisan jurnal bereputasi dan memperkuat posisi penelitian Anda dalam tinjauan kritis literatur.

Kerangka desain ini akan langsung diimplementasikan secara teknis pada slide berikutnya, di mana kita akan membahas skrip Python lengkap untuk memuat backbone DINOv2 melalui `torch.hub`, menyusun pipeline transformasi, dan mengekstrak embedding global dari token `[CLS]`. Dengan memadukan protokol eksperimen yang sistematis pada slide ini dengan implementasi kode pada slide berikutnya, mahasiswa dilengkapi dengan alur kerja end-to-end yang siap dikonversi menjadi bab metodologi proposal disertasi atau naskah publikasi internasional.

---

## Slide 031 - Praktikum: Ekstraksi Embedding DINO atau DINOv2

### Narasi

```python
import torch
from torchvision import transforms
from PIL import Image

### Muat backbone DINOv2 dari torch.hub

model = torch.hub.load('facebookresearch/dinov2', 'dinov2_vits14')
model.eval()

transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],
                         std=[0.229, 0.224, 0.225])
])

def extract_embedding(path):
    img = Image.open(path).convert('RGB')
    x = transform(img).unsqueeze(0)
    with torch.no_grad():
        feat = model(x)  # [1, 384] untuk ViT-S
    return feat.squeeze().numpy()
```

Pada slide sebelumnya, kita telah merumuskan protokol eksperimen transferability yang mengandalkan model fondasi dalam keadaan *frozen*. Langkah implementatif berikutnya adalah mengekstrak representasi vektor dari dataset target menggunakan arsitektur DINO atau DINOv2, sebagaimana diilustrasikan dalam kode di atas.

Baris awal mengimpor modul inti untuk manipulasi tensor dan pra-pemrosesan citra. Pemanggilan `torch.hub.load('facebookresearch/dinov2', 'dinov2_vits14')` mengambil arsitektur Vision Transformer kecil dengan ukuran patch 14 piksel secara otomatis dari repository resmi Facebook Research. Penetapan `model.eval()` mutlak diperlukan untuk menonaktifkan lapisan stokastik seperti dropout dan menyesuaikan statistik normalisasi batch, sehingga menjamin output embedding bersifat deterministik selama fase inference.

Pipeline transformasi dikonfigurasi melalui `Compose` untuk menstandarisasi input ke kondisi yang diharapkan oleh model. Resizing ke 224×224 piksel mengikuti resolusi *training window* default ViT. Konversi ke tensor dan normalisasi menggunakan mean serta standar deviasi ImageNet sangat krusial, karena model fondasi self-supervised telah mempelajari prior visual yang sensitif terhadap distribusi statistik tersebut.

Fungsi `extract_embedding(path)` mengemas proses inferensi menjadi unit yang modular dan reusable. Gambar dibuka dan dipastikan berada dalam ruang warna RGB. Operasi `.unsqueeze(0)` menambahkan dimensi batch menjadi `[1, 3, 224, 224]` agar kompatibel dengan layer transformer. Konteks `torch.no_grad()` mencegah akumulasi gradien, mengoptimalkan utilisasi memori GPU. Tensor hasil forward pass merepresentasikan embedding global yang diekstrak khusus dari token `[CLS]`, dengan dimensi akhir `[1, 384]` untuk varian ViT-Small. Array ini kemudian dikonversi ke format NumPy untuk kompatibilitas dengan skrip evaluasi atau visualisasi lanjutan.

Perlu dicatat bahwa jika penelitian Anda menargetkan arsitektur DINO generasi awal, parameter string pada `torch.hub.load` dapat disesuaikan menjadi `'dino_vits16'` atau `'dino_vitb16'`. Mekanisme ekstraksi tetap identik, hanya kapasitas representasi dan resolusi patch yang berbeda.

Setelah embedding berhasil dikumpulkan untuk seluruh sampel dataset, analisis kualitas ruang fitur menjadi langkah prioritas sebelum melatih classifier. Slide berikutnya akan memperkenalkan pipeline reduksi dimensi menggunakan PCA dan t-SNE untuk memproyeksikan embedding berdimensi tinggi ke bidang dua dimensi. Visualisasi ini berfungsi sebagai validasi eksploratif terhadap kohesi antar-kelas, yang akan menentukan strategi fine-tuning atau pemilihan metrik evaluasi pada tahap eksperimen transferability.

---

## Slide 032 - Praktikum: Visualisasi Embedding dengan Reduksi Dimensi

### Narasi

Setelah berhasil mengekstrak representasi vektor berdimensi tinggi dari backbone DINO atau DINOv2 pada slide sebelumnya, langkah logis berikutnya adalah memverifikasi kualitas dan struktur ruang fitur yang telah dipelajari secara visual. Ruang embedding yang dihasilkan oleh model foundation vision umumnya memiliki dimensi ratusan hingga ribuan, sehingga mustahil untuk divisualisasikan secara langsung ke dalam bidang dua dimensi. Kita memerlukan teknik reduksi dimensi yang mampu mempertahankan topologi lokal maupun global dari data asli agar pola semantik dapat diamati.

Kode praktikum pada slide ini mengimplementasikan alur reduksi dimensi bertingkat menggunakan `numpy`, `sklearn.decomposition.PCA`, `sklearn.manifold.TSNE`, dan `matplotlib.pyplot`. Baris pertama dalam skrip menginisialisasi `PCA(n_components=50, random_state=0)` yang kemudian diterapkan pada matriks embedding `X` melalui perintah `X_pca = pca.fit_transform(X)`. Catatan pada slide menekankan pentingnya menjalankan PCA terlebih dahulu. Hal ini merupakan kebutuhan teknis karena t-SNE memiliki kompleksitas komputasi yang tinggi dan sangat sensitif terhadap noise serta curse of dimensionality. Mereduksi dimensi ke 50 komponen utama terlebih dahulu membuat proses fitting t-SNE menjadi jauh lebih stabil, cepat, dan menghasilkan peta yang lebih interpretable.

Setelah mendapatkan matriks `X_pca`, kode melanjutkan ke tahap kedua dengan menginisialisasi `TSNE(n_components=2, random_state=0, perplexity=30)`. Fungsi `fit_transform(X_pca)` akhirnya memproyeksikan data ke ruang dua dimensi yang siap digambar. Parameter `perplexity=30` berperan sebagai estimasi jumlah tetangga efektif per titik data; nilai ini menyeimbangkan antara penekanan pada struktur mikro (cluster kecil) dan makro (pemisahan kelas). Plot scatter yang dihasilkan mewarnai setiap titik berdasarkan variabel `y` (label ground truth) menggunakan colormap `tab10`. Nilai alpha sebesar 0.7 diberikan untuk menangani overplotting jika ada sampel yang tumpang tindih koordinatnya.

Saat mengamati hasil visualisasi, fokuskan analisis pada pola pengelompokan (clustering). Apakah titik-titik dari kelas yang sama benar-benar berkumpul membentuk cluster yang kompak, atau justru tersebar acak? Jika embedding berhasil menangkap representasi invariant melalui mekanisme self-supervised learning, kita akan melihat pemisahan antar kelas yang tajam meskipun model tidak pernah melihat label `y` selama fase pretraining. Visualisasi ini berfungsi sebagai diagnostic tool untuk mendeteksi bias dataset, kegagalan konvergensi, atau dominasi kelas tertentu sebelum kita melakukan evaluasi kuantitatif.

Hasil pengelompokan yang teramati pada slide ini menjadi fondasi validasi untuk langkah eksperimen berikutnya. Setelah kita meyakinkan diri bahwa ruang embedding telah mengorganisir fitur secara bermakna, kita akan mengukur performa aktualnya dengan metode yang lebih rigor. Pada slide berikutnya, kita akan menerapkan Linear Probing menggunakan Logistic Regression untuk mengonversi observasi visual kualitatif ini menjadi metrik akurasi numerik, sekaligus membandingkannya dengan baseline supervised learning tradisional.

---

## Slide 033 - Praktikum: Linear Probing

### Narasi

Setelah sebelumnya kita memvisualisasikan distribusi embedding menggunakan reduksi dimensi PCA dan t-SNE, langkah selanjutnya adalah mengevaluasi kualitas semantik representasi tersebut secara kuantitatif. Slide ini memperkenalkan praktikum linear probing, sebuah protokol evaluasi standar untuk mengukur seberapa baik fitur yang diekstrak dari model *foundation* atau *self-supervised* dapat memisahkan kelas-kelas target hanya dengan classifier linier sederhana.

Kita mulai dengan menyiapkan matriks embedding `X` dan vektor label `y`. Kode berikut melakukan partisi data menjadi subset pelatihan dan pengujian dengan menjaga proporsi kelas melalui parameter `stratify`. Pembagian ini penting untuk memastikan evaluasi tidak bias terhadap dominansi某一 kelas tertentu.

```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

### X: embedding, y: label

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=0, stratify=y
)

clf = LogisticRegression(max_iter=1000, C=1.0)
clf.fit(X_train, y_train)

y_pred = clf.predict(X_test)
acc = accuracy_score(y_test, y_pred)
print(f'Akurasi linear probing: {acc:.4f}')
```

Pemilihan regresi logistik bukan kebetulan. Model ini memiliki kapasitas representasional yang minimal, sehingga akurasi yang dihasilkan murni mencerminkan keterpisahan linear dari manifold fitur yang dipelajari oleh backbone. Parameter `max_iter` dinaikkan untuk menghindari peringatan konvergensi dini, sedangkan regulasi `C=1.0` dapat dioptimalkan melalui grid search jika dataset memungkinkan.

Perhatikan dua catatan metodologis berikut yang wajib diterapkan dalam eksperimen tingkat doktoral:
- Pada subset label yang terbatas atau mengalami ketidakseimbangan signifikan, selalu tambahkan `class_weight='balanced'` pada inisialisasi `LogisticRegression`. Ini mencegah dominasi gradien oleh kelas mayoritas dan menghasilkan estimasi generalisasi yang lebih robust.
- Bandingkan skor akurasi linear probing ini dengan ekstraksi fitur dari lapisan terakhir model yang dilatih secara *supervised* pada dataset referensi. Perbedaan performa akan mengungkap efektivitas transfer knowledge dari paradigma *self-supervised* versus pendekatan tradisional berbasis label manual.

Hasil akurasi yang tercetak merupakan proxy langsung untuk menilai kematangan representasi. Akurasi tinggi menandakan bahwa proses pretraining berhasil menyusun ruang fitur yang terstruktur secara kategorikal, sehingga cukup efisien untuk downstream tasks tanpa memerlukan fine-tuning menyeluruh.

Eksperimen ini menjadi jembatan menuju analisis komparatif pada slide berikutnya. Di sana, kita akan membedah tabel perbandingan sistematis antara fitur berbasis supervisi ketat dan fitur yang lahir dari pembelajaran mandiri skala besar, termasuk implikasi mereka terhadap transferability, sensitivitas domain, serta strategi pelaporan metrik yang rigor untuk publikasi internasional.

---

## Slide 034 - Perbandingan Fitur Supervised vs Self-Supervised

### Narasi

Setelah kita menyelesaikan implementasi linear probing pada slide sebelumnya menggunakan embedding dari model yang telah dilatih, langkah selanjutnya adalah memahami karakteristik representasi fitur yang dihasilkan oleh dua paradigma pembelajaran yang berbeda. Slide ini menyajikan perbandingan sistematis antara fitur yang diperoleh dari pelatihan supervised konvensional di ImageNet dengan fitur dari model self-supervised seperti DINOv2.

Pada aspek sumber label, model supervised mengandalkan sekitar 1,2 juta anotasi manusia dari ImageNet, sedangkan DINOv2 memanfaatkan jutaan gambar tanpa label sama sekali melalui kumpulan data LVD-142M. Perbedaan mendasar ini mengubah cara model menangkap struktur visual. Meskipun keduanya menunjukkan performa baik dalam linear probing dan fine-tuning, fitur self-supervised umumnya lebih robust terhadap variasi domain alami karena tidak terikat secara eksplisit pada distribusi kelas ImageNet yang spesifik.

Sensitivitas domain menjadi poin krusial dalam desain penelitian tingkat doktoral. Model supervised cenderung memiliki batas transferabilitas yang ketat ketika diterapkan pada data yang menyimpang jauh dari domain asal. Sebaliknya, representasi self-supervised lebih adaptif terhadap pergeseran distribusi, meskipun tetap mengandung bias implisit dari data pretraining. Dari sisi komputasi, biaya pretraining untuk DINOv2 sebenarnya lebih besar karena memerlukan epoch yang lebih banyak dan optimasi khusus, namun investasi tersebut dibayar kembali melalui fleksibilitas transfernya.

Secara praktik penelitian, hindari asumsi bahwa salah satu pendekatan selalu menang atau kalah secara mutlak. Protokol evaluasi yang rigor menuntut kita menguji kedua jenis fitur pada dataset target yang identik, menggunakan split data dan augmentasi yang konsisten. Selalu laporkan interval kepercayaan atau standar deviasi hasil, bukan hanya angka akurasi tunggal, agar klaim empiris dapat dipertanggungjawabkan secara statistik dan reproduktif.

Hasil perbandingan ini akan langsung menjadi landasan untuk menganalisis tabel eksperimen pada slide berikutnya. Kita akan menelaah apakah selisih akurasi yang teramati signifikan secara statistik, mengidentifikasi kelas-kelas mana yang masih menjadi titik lemah model, serta mengevaluasi ketahanan representasi DINOv2 terhadap penurunan resolusi dan pergeseran domain. Pendekatan kritis inilah yang akan mengarahkan Anda dalam mengidentifikasi research gap dan merancang kontribusi ilmiah yang solid.

---

## Slide 035 - Analisis Hasil dan Interpretasi

### Narasi

Setelah tahap eksperimen selesai, langkah kritis berikutnya adalah menyusun dan menginterpretasikan hasil pengujian secara sistematis. Pada slide ini, kita akan membedah tabel akurasi yang membandingkan kinerja model DINO ViT-S dan DINOv2 ViT-S pada skenario *linear probing* dengan variasi persentase label pelatihan, yaitu 1% dan 10%. Kedua model dijalankan pada resolusi tetap 224×224 piksel untuk menjaga konsistensi variabel kontrol, sesuai dengan prinsip metodologi penelitian yang ketat.

Dari data yang disajikan, terlihat peningkatan akurasi yang konsisten seiring penambahan rasio label. Untuk DINO ViT-S, akurasi naik dari 72,1% pada 1% label menjadi 81,4% pada 10% label, dengan standar deviasi masing-masing sebesar 1,2 dan 0,8. Sementara itu, DINOv2 menunjukkan performa yang lebih unggul di kedua skenario, mencapai 76,8% pada 1% label dan 84,2% pada 10% label, disertai varians yang lebih kecil. Penurunan standar deviasi pada DINOv2 mengindikasikan stabilitas representasi yang lebih baik dan konsistensi yang lebih tinggi selama proses evaluasi berulang.

Interpretasi mendalam terhadap tabel ini memerlukan pendekatan statistik dan analisis kesalahan yang rigor, terutama mengingat konteks penelitian doktoral. Berikut adalah poin-poin krusial yang perlu dijawab melalui analisis lanjutan:
- Apakah selisih akurasi signifikan secara statistik? Uji hipotesis seperti paired t-test atau bootstrap confidence interval wajib dilakukan untuk memvalidasi klaim keunggulan DINOv2.
- Pada kelas mana model gagal? Analisis matriks kebingungan (*confusion matrix*) diperlukan untuk mengidentifikasi bias kategorikal atau ambiguitas visual yang belum tertangkap oleh representasi.
- Apakah error berubah ketika resolusi diturunkan? Eksperimen sensitivitas resolusi akan menguji seberapa kuat model mengandalkan informasi frekuensi tinggi versus struktur global.
- Apakah representasi DINOv2 lebih tahan terhadap perubahan domain? Ini menyentuh inti keunggulan self-supervised learning dalam menangkap prior semantik yang lebih umum dibandingkan pembelajaran berbasis supervisi ketat.

Temuan dari setiap pertanyaan interpretasi di atas tidak boleh bersifat subjektif, melainkan harus menjadi dasar objektif untuk keputusan teknis. Hal ini sejalan dengan catatan praktik pada slide sebelumnya yang menekankan pentingnya pengujian komparatif dan pelaporan interval kepercayaan. Selanjutnya, hasil analisis ini akan diterjemahkan langsung menjadi panduan strategis pada slide berikutnya, di mana kita akan merumuskan rekomendasi penggunaan foundation vision model yang disesuaikan dengan karakteristik domain target, kelimpahan data terlabel, dan kendala komputasi.

---

## Slide 036 - Rekomendasi Penggunaan Foundation Vision Model

### Narasi

Setelah kita menguraikan hasil eksperimen pada slide sebelumnya, langkah selanjutnya adalah menerjemahkan temuan empiris tersebut menjadi panduan praktis yang dapat langsung diterapkan dalam penelitian. Tabel perbandingan akurasi antara DINO dan DINOv2 pada berbagai persentase label menunjukkan pola yang konsisten: peningkatan jumlah data training memang memperbaiki performa, namun kesenjangan antar model juga mengindikasikan bahwa arsitektur dan mekanisme pretraining memainkan peran krusial. Dari sini, fokus kita bergeser dari sekadar pelaporan angka menjadi penyusunan rekomendasi berbasis bukti untuk penggunaan foundation vision model di berbagai skenario nyata.

Berdasarkan analisis tersebut, berikut adalah panduan strategis dalam memilih pendekatan implementasi:
- **Jika domain target dekat dengan data natural**: DINOv2 frozen + linear probe sudah cukup untuk banyak kasus. Pendekatan ini meminimalkan risiko overfitting, menjaga stabilitas representasi yang telah dipelajari secara masif, sekaligus menghemat sumber daya komputasi.
- **Jika domain target sangat jauh** (misalnya histopatologi, CT, atau SAR): Evaluasi apakah fine-tuning pada sebagian layer diperlukan. Uji juga apakah augmentasi khusus domain membantu model menangkap karakteristik spesifik yang tidak tercakup dalam data pretraining.
- **Jika data label sangat sedikit**: Prioritaskan linear probing dengan representasi DINOv2 daripada fine-tuning. Representasi self-supervised cenderung lebih robust dan generalizable, sehingga mampu mengekstrak fitur bermakna tanpa bergantung pada supervisi penuh.
- **Jika resolusi berbeda jauh**: Perhatikan interpolasi positional embedding dan uji beberapa resolusi. Perubahan dimensi spasial dapat mengganggu konsistensi patch-level attention, sehingga pencarian titik optimal antara presisi dan efisiensi memori menjadi penting.

Penting untuk ditekankan bahwa semua rekomendasi ini harus berbasis bukti eksperimen, bukan sekadar preferensi atau tren metodologis. Validasi silang, analisis error, dan pengukuran statistik tetap menjadi fondasi utama dalam pengambilan keputusan penelitian tingkat doktoral. Dengan landasan yang kuat ini, kita akan beralih ke tahap eksplorasi konseptual dan persiapan riset mandiri. Pada slide berikutnya, kita akan membahas aktivitas seminar paper serta diskusi mendalam mengenai desain pretext task. Mahasiswa akan diminta membandingkan kontribusi dan keterbatasan arsitektur DINO versus DINOv2, lalu merancang tugas pretext yang paling sesuai dengan domain penelitian masing-masing. Diskusi ini juga akan menyentuh pertanyaan kritis mengenai keselarasan objective pretraining dengan downstream task, potensi transfer lintas domain, serta metrik evaluasi representasi semantik tanpa label.

---

## Slide 037 - Aktivitas Seminar Paper dan Diskusi Pretext Task

### Narasi

Pada slide ini, kita beralih dari rekomendasi praktis ke aktivitas akademik yang lebih mendalam, yaitu seminar paper dan diskusi teknis mengenai desain pretext task. Kegiatan ini dirancang untuk mengasah kemampuan analisis kritis kalian terhadap literatur utama di bidang self-supervised learning, khususnya pada arsitektur DINO dan DINOv2.

Untuk bagian seminar paper, fokuskan pembahasan pada empat aspek kunci: kontribusi metodologis, mekanisme pelatihan, hasil eksperimen, serta klaim-klaim yang diajukan oleh penulis. Jangan hanya menerima temuan secara mentah. Identifikasi secara eksplisit keterbatasan atau celah penelitian yang tidak dibahas dalam paper tersebut. Hal ini sejalan dengan tujuan mata kuliah tingkat doktor, di mana kalian harus mampu menemukan research gap yang valid sebelum merumuskan hipotesis atau metodologi baru.

Setelah seminar, kelas akan bergeser ke diskusi desain pretext task yang relevan dengan dataset penelitian masing-masing mahasiswa. Pretext task bukan sekadar teknik augmentasi atau loss function tambahan; ia adalah fondasi yang menentukan bagaimana model mempelajari representasi visual tanpa supervisi eksternal. Tanyakan pada diri sendiri dan kelompok: tugas apa yang paling sesuai dengan karakteristik domain Anda? Apakah berbasis kontrastif, mask image modeling, atau pendekatan lain? Justifikasi jawaban Anda berdasarkan struktur data, noise, dan kompleksitas visual domain riset Anda.

Tiga pertanyaan diskusi berikut menjadi panduan untuk memperdalam pemahaman konseptual:
- Apakah objective pretraining harus selaras sempurna dengan downstream task? Secara teoretis, kesamaan domain membantu, namun kekuatan foundation model justru terletak pada kemampuannya menangkap struktur umum yang transferable.
- Dapatkah representasi yang dilatih pada satu domain digunakan lintas domain? Jawaban ya, asalkan dilakukan evaluasi ketat terhadap shift distribusi dan penyesuaian layer akhir, seperti yang telah kita bahas pada rekomendasi penggunaan model sebelumnya.
- Bagaimana mengukur kualitas representasi yang "semantik" tanpa label? Kita dapat mengandalkan metrik proxy seperti linear probing accuracy, clustering quality, atau alignment dengan space bahasa multimodal, meskipun tetap perlu diakui bahwa ground truth semantik absolut sulit diperoleh tanpa anotasi manusia.

Diskusi ini akan menjadi jembatan langsung menuju tugas praktikum pada slide berikutnya. Hasil identifikasi keterbatasan paper dan pemilihan pretext task yang tepat akan menjadi dasar bagi kalian saat mengekstrak embedding, memvisualisasikan ruang fitur, dan melakukan evaluasi linear probing. Pastikan catatan diskusi ini terdokumentasi rapi, karena akan menjadi bahan interpretasi dalam laporan akhir yang menuntut rekomendasi berbasis bukti untuk riset disertasi kalian.

---

## Slide 038 - Target Keluaran dan Tugas

### Narasi

Setelah diskusi sebelumnya menyoroti bagaimana desain *pretext task* menentukan kualitas representasi tanpa label, sekarang kita beralih ke implementasi empiris. Slide ini merumuskan target keluaran dan skema tugas praktikum yang dirancang khusus untuk menguji hipotesis transferabilitas model *foundation vision* pada domain penelitian kalian.

Pelaksanaan tugas praktikum terdiri dari empat tahap eksperimental yang harus dijalankan secara berurutan:
- Ekstraksi *embedding* menggunakan arsitektur DINO atau DINOv2 pada dataset pilihan.
- Visualisasi *embedding* multidimensi melalui teknik reduksi dimensi.
- Evaluasi *linear probing* dengan memanipulasi variasi jumlah label pelatihan.
- Perbandingan langsung antara representasi *self-supervised* dengan fitur yang dihasilkan oleh model *supervised*.

Keluaran akademik yang diharapkan tertuang dalam format laporan yang harus kalian serahkan. Laporan wajib memuat deskripsi dataset dan konteks domain, kode notebook yang sepenuhnya *reproducible*, serta tabel hasil komparatif lintas kondisi eksperimen. Bagian terpenting adalah interpretasi kritis mengenai apakah representasi DINOv2 bersifat *transferable* ke domain spesifik kalian, diikuti oleh rekomendasi strategis pemanfaatan *foundation model* untuk memperkuat arah riset disertasi.

Target keluaran inti dari pertemuan ini adalah tercapainya analisis transferabilitas yang berbasis bukti, lengkap dengan rekomendasi penggunaan *foundation vision model* yang relevan. Untuk memastikan setiap komponen eksperimen berjalan terstruktur dan hasilnya dapat divalidasi, detail teknis pelaksanaan akan diuraikan pada slide berikutnya melalui checklist praktikum yang harus dipenuhi sebelum presentasi di kelas.

---

## Slide 039 - Checklist Praktikum

### Narasi

Slide ini menyajikan daftar periksa atau checklist praktikum yang berfungsi sebagai panduan eksekusi teknis setelah target keluaran dan format laporan ditetapkan pada slide sebelumnya. Dalam konteks penelitian tingkat doktoral, checklist ini tidak hanya bersifat administratif, melainkan dirancang untuk menjamin ketatnya metodologi, reproduktibilitas kode, dan kedalaman analisis empiris sebelum hasil dipresentasikan di forum akademik.

Tahapan pertama berfokus pada manajemen data dan ekstraksi representasi. Pembagian dataset secara stratifikasi wajib dilakukan untuk mencegah bias distribusi kelas antar subset pelatihan, validasi, dan pengujian. Proses ekstraksi embedding menggunakan foundation model seperti DINOv2 dijalankan dalam mode batch untuk mengoptimalkan utilisasi memori GPU dan menjaga stabilitas inference. Pengujian pada beragam resolusi gambar juga menjadi syarat mutlak, karena hal ini mengungkap sifat invariansi skala yang melekat pada representasi self-supervised.

Fase evaluasi menuntut penerapan linear probing dengan variasi proporsi label terlabel: satu persen, sepuluh persen, lima puluh persen, dan seratus persen. Skema ini mengisolasi kemampuan transfer knowledge dari model tanpa melakukan fine-tuning arsitektur inti. Sebagai pembanding objektif, fitur dari baseline supervised seperti ResNet yang dilatih pada ImageNet harus diekstrak untuk memberikan garis dasar performa end-to-end. Seluruh metrik kemudian disusun dalam tabel komparatif lengkap dengan error analysis yang menyoroti pola kegagalan sistematis, bukan sekadar angka akurasi agregat.

Aspek reproduktibilitas menjadi pilar utama dokumentasi. Pencatatan eksplisit terhadap random seed, versi dependensi, konfigurasi lingkungan, dan spesifikasi hardware harus tercantum jelas. Interpretasi akhir harus dibangun atas dasar bukti empiris dari hasil eksperimen, mencakup penilaian kritis mengenai kelayakan adaptasi foundation vision model ke domain riset spesifik. Apabila seluruh poin checklist telah terpenuhi, Anda siap menyampaikan temuan pada diskusi kelas. Representasi visual yang telah tervalidasi ini akan langsung menjadi modalitas gambar yang diperlukan pada pertemuan berikutnya, ketika kita membahas penyelarasan multimodal melalui framework contrastive learning seperti CLIP.

---

## Slide 040 - Hubungan dengan Pertemuan Berikutnya

### Narasi

Setelah seluruh poin pada checklist praktikum di slide sebelumnya diselesaikan, hasil ekstraksi embedding, validasi linear probe, serta analisis visualisasi t-SNE atau UMAP kini siap dikonversi menjadi aset representasi yang bermakna. Output teknis tersebut bukan sekadar angka atau plot, melainkan fondasi modalitas visual yang akan diintegrasikan ke dalam kerangka kerja penelitian yang lebih kompleks.

Pada pertemuan berikutnya, fokus kajian akan bergeser secara eksplisit ke *Vision-Language Models* dan representasi multimodal. Model CLIP menjadi titik tolak utama karena mekanisme pembelajaran kontrastifnya merupakan adaptasi langsung dari prinsip *self-supervised learning* yang telah dipelajari saat ini. Jika SimCLR memaksimalkan kesamaan antar augmentasi dari satu citra, CLIP memperluas kerangka kontrastif tersebut dengan pasangan positif berupa pasangan citra dan teks yang memiliki keselarasan semantik. Pemahaman mendalam terhadap SSL, khususnya terkait desain fungsi loss, penanganan *hard negative*, dan stabilitas optimasi, akan menjadi kunci analitis bagi mahasiswa untuk membedah mengapa CLIP mampu menunjukkan performa *zero-shot* yang kuat tanpa memerlukan penalaan parameter tambahan.

Untuk mempersiapkan eksplorasi pada pertemuan kelima, berikut adalah arahan konkret yang perlu disiapkan:
- Rancang eksperimen awal untuk menguji keselarasan ruang fitur (*feature space alignment*) antara embedding visual dari DINOv2 dan embedding teks dari language model.
- Evaluasi apakah representasi yang dihasilkan tetap robust terhadap variasi domain dan resolusi input.
- Identifikasi keterbatasan mendasar dari pendekatan contrastive alignment dalam konteks multimodal.
- Pilih satu paper terkini yang membahas integrasi vision-language, lalu susun pertanyaan kritis mengenai metodologi evaluasi, bias data, skalabilitas, dan celah penelitian yang belum terjamah.

Persiapan ini akan menjadi bahan diskusi utama untuk menguji kesiapan Anda dalam merumuskan hipotesis dan mendesain eksperimen tingkat doktoral. Dengan memahami jembatan konseptual antara SSL dan multimodal representation, transisi ke topik *Vision-Language Models* di pertemuan berikutnya akan berjalan dengan landasan analitis yang kuat.

---

## Slide 041 - Penutup

### Narasi

Kita telah menyelesaikan pembahasan mengenai *Self-Supervised Learning* dan *Foundation Vision Models*. Fokus utama pertemuan ini adalah memahami bagaimana model seperti DINOv2 mampu mempelajari representasi visual yang kuat tanpa bergantung pada anotasi manual, serta mengapa arsitektur berbasis *Vision Transformer* menjadi standar de facto dalam pengembangan *foundation models* terkini. Mekanisme *self-distillation*, strategi augmentasi data, dan desain fungsi loss dalam konteks SSL menjadi elemen krusial yang harus dievaluasi secara kritis ketika membandingkan kinerja berbagai arsitektur pada level penelitian doktoral.

Representasi yang dihasilkan oleh model-model ini memiliki sifat transferabilitas tinggi, sehingga dapat diadaptasi secara efisien untuk berbagai tugas turunan seperti klasifikasi, deteksi objek, maupun segmentasi semantik. Kemampuan adaptasi ini menjadikan *foundation models* sebagai titik awal yang strategis sebelum dilakukan *fine-tuning* pada dataset spesifik atau domain aplikasi yang ditargetkan.

Sebagaimana dibahas pada slide sebelumnya, representasi visual yang dipelajari hari ini akan berfungsi sebagai modalitas inti bagi model multimodal. Pertemuan berikutnya akan mengarah pada *Vision-Language Models* dan *Multimodal Representation*, dengan studi kasus utama pada arsitektur CLIP. Prinsip *contrastive learning* yang mendasari SimCLR juga menjadi fondasi penyelarasan antara ruang citra dan teks pada CLIP. Pemahaman mendalam terhadap SSL hari ini akan memudahkan analisis mengapa model multimodal tersebut mampu melakukan inferensi *zero-shot* dengan performa yang kompetitif.

Untuk persiapan pertemuan berikutnya, silakan kerjakan langkah-langkah berikut:
- Uji kompatibilitas representasi visual dari DINOv2 dengan embedding teks menggunakan lingkungan Jupyter Notebook atau Google Colab.
- Pilih satu paper terbaru terkait *vision-language alignment* dan catat metodologi evaluasinya.
- Susun pertanyaan kritis mengenai *research gap*, keterbatasan arsitektur, dan peluang kontribusi ilmiah baru.

Diskusi pada pertemuan berikutnya akan berfokus pada perancangan eksperimen awal dan positioning karya ilmiah terhadap state-of-the-art. Terima kasih atas partisipasi aktif dalam sesi ini. Sampai jumpa pada pertemuan berikutnya.
