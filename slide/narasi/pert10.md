# Narasi TD Pengolahan Citra Digital - Pertemuan 10

## 3D Vision, Multi-View Geometry, dan Neural Rendering

Sumber: markdown/pert10-3d-vision-multi-view-geometry-dan-neural-rendering.md

---

## Slide 000 - Cover

### Narasi

Slide ini membuka bahasan pertemuan ke-10 dengan judul *3D Vision, Multi-View Geometry*, dan *Neural Rendering*. Pada tingkat doktoral, transisi dari representasi 2D menuju pemahaman ruang 3D bukan sekadar penambahan dimensi koordinat, melainkan perubahan paradigma dalam memodelkan hubungan antara kamera, objek, dan pencahayaan. Slide ini berfungsi sebagai penanda bahwa fokus analitis kini bergeser ke pemulihan struktur geometris dan sintesis visual berbasis neural.

Pembahasan akan dimulai dari prinsip dasar *Multi-View Geometry*, yang mencakup pemetaan proyektif, kalibrasi kamera, estimasi matriks esensial dan fundamental, serta teknik *Structure-from-Motion* untuk merekonstruksi titik 3D dari sekumpulan citra 2D. Aspek-aspek ini menjadi landasan matematis yang harus dipahami sebelum memasuki pendekatan modern yang lebih fleksibel namun tetap memerlukan grounding geometri yang kuat.

Selanjutnya, materi akan berlanjut ke *Neural Rendering*, khususnya representasi implisit seperti *Neural Radiance Fields (NeRF)* dan variannya. Di sini, kita akan mengeksplorasi bagaimana jaringan saraf dipelajari secara end-to-end untuk memetakan koordinat spasial dan vektor arah pandang ke warna serta kerapatan volume. Pendekatan diferensiabel ini memungkinkan sintesis pandangan baru yang fotorealistik sekaligus membuka peluang riset di bidang optimisasi memori, percepatan inferensi, dan integrasi dengan representasi 3D lain seperti Gaussian Splatting.

Pada slide berikutnya, kita akan memetakan posisi topik ini dalam alur keseluruhan perkuliahan, serta menelaah target capaian pembelajaran yang menuntut mahasiswa untuk menganalisis tantangan riset 3D vision, memanfaatkan tool eksperimental untuk rekonstruksi sederhana, dan merumuskan masalah penelitian yang memiliki novelty di persimpangan geometri klasik dan representasi neural.

---

## Slide 001 - Posisi Pertemuan 10 dalam Rangkaian Perkuliahan

### Narasi

Pada rangkaian perkuliahan ini, pertemuan satu hingga delapan telah membangun fondasi kuat mengenai representasi visual, deteksi objek, segmentasi semantik, serta pemahaman konteks pada domain dua dimensi. Pertemuan sembilan kemudian memperluas cakupan ke generative vision dengan pendekatan diffusion models, di mana model belajar mendistribusikan data visual untuk menghasilkan konten baru yang realistis. 

Pertemuan sepuluh menandai pergeseran paradigma yang signifikan: dari manipulasi dan generasi citra 2D menuju pemulihan informasi tiga dimensi dari observasi gambar datar. Fokus utama kali ini adalah memahami bagaimana geometri adegan dapat direkonstruksi secara eksplisit melalui pendekatan tradisional, maupun secara implisit menggunakan representasi neural seperti Neural Radiance Fields (NeRF) dan teknik terkait. Pemulihan kedalaman, estimasi pose kamera, dan konsistensi multiview menjadi inti dari diskusi ini, mengingat bahwa informasi 3D tidak pernah terekam langsung oleh sensor kamera konvensional, melainkan harus disimpulkan dari proyeksi perspektif dan parallax antar view.

Capaian pembelajaran pada sesi ini dirancang selaras dengan kompetensi tingkat doktor. Mahasiswa diharapkan mampu menganalisis paradigma 3D vision secara kritis, mengidentifikasi celah riset seperti masalah occlusion, sparse view, scale ambiguity, hingga dinamika adegan. Secara praktis, mahasiswa akan dibimbing memanfaatkan tools komputasional untuk mengeksplorasi estimasi kedalaman dan rekonstruksi sederhana. Lebih lanjut, sesi ini menuntut mahasiswa merumuskan masalah penelitian awal yang terstruktur, mencakup asumsi geometri, pemilihan baseline, mitigasi risiko teknis, serta potensi kontribusi ilmiah yang dapat dikembangkan lebih lanjut.

Pembahasan ini akan berlanjut secara rinci pada tujuan pembelajaran spesifik dan target keluaran yang harus dicapai, termasuk format concept note yang menjadi dasar evaluasi proyek akhir mata kuliah. Dengan memahami posisi strategis pertemuan ini dalam alur kurikulum, mahasiswa dapat mempersiapkan diri untuk melakukan transisi konseptual dari computer vision berbasis piksel menuju pemodelan ruang tiga dimensi yang konsisten dan dapat diinterpretasikan secara geometris maupun neural.

---

## Slide 002 - Tujuan Pembelajaran dan Target Keluaran

### Narasi

Pada slide ini, kita menetapkan arah pembelajaran dan ekspektasi keluaran untuk pertemuan ke-10. Setelah pada pertemuan sebelumnya kita menelusuri bagaimana model generatif mampu menghasilkan konten visual baru, fokus diskusi kini bergeser secara fundamental ke pemulihan struktur tiga dimensi dari data citra dua dimensi.

Terdapat empat tujuan pembelajaran yang menjadi landasan kognitif dan analitis bagi mahasiswa pascasarjana. Pertama, memahami sumber informasi tiga dimensi yang dapat diekstrak dari citra dua dimensi serta mengenali keterbatasan fisik dan matematis inherent dari proses tersebut. Kedua, menjelaskan fondasi geometris seperti model kamera, geometri epipolar, prinsip stereo correspondence, dan teknik estimasi kedalaman baik berbasis supervised maupun unsupervised. Ketiga, melakukan bedah komparatif antara representasi scene tradisional berupa point cloud dan mesh, dengan representasi kontinyu berbasis neural seperti NeRF dan Gaussian Splatting. Keempat, mengidentifikasi tantangan teknis yang kerap menghambat rekonstruksi lapangan nyata, meliputi occlusion, sparse view configuration, scale ambiguity, serta kompleksitas dynamic scene.

Dari perspektif target keluaran, mahasiswa dituntut untuk menyusun concept note awal yang berfokus pada masalah penelitian di bidang 3D vision. Dokumen ini harus bersifat strategis dan terstruktur, mencakup spesifikasi dataset dan asumsi geometri yang mendasarinya, pemilihan baseline metodologis yang relevan, pemetaan risiko teknis yang mungkin muncul selama eksperimen, serta formulasi potensi kontribusi ilmiah yang jelas terhadap perkembangan state-of-the-art.

Penjabaran tujuan dan target ini berfungsi sebagai kerangka kerja yang langsung menyambung ke slide berikutnya. Dengan pemahaman yang kuat mengenai batasan dan mekanisme setiap pendekatan, kita dapat merumuskan pertanyaan riset yang tajam, mulai dari menentukan batas fundamental antara metode monocular dan multi-view, mendesain protokol akuisisi berdasarkan jumlah dan posisi view, hingga mengevaluasi kapan representasi neural memberikan keunggulan signifikan dibandingkan pipeline tradisional dalam konteks aplikasi tertentu.

---

## Slide 003 - Pertanyaan Kunci Pertemuan Ini

### Narasi

Slide ini mengonkretkan arah diskusi pertemuan ke-10 melalui tiga pertanyaan kunci yang menjadi fondasi eksplorasi 3D vision dan neural rendering. Setiap pertanyaan dirancang untuk mengarahkan pemikiran kritis menuju perumusan masalah penelitian tingkat doktoral.

Pertanyaan pertama menanyakan informasi 3D apa yang dapat dipulihkan dari citra 2D. Secara matematis, pemetaan dari 3D ke 2D bersifat many-to-one, sehingga informasi kedalaman dan pose kamera tidak unik tanpa asumsi tambahan. Implikasi risetnya terletak pada penentuan batas fundamental antara pendekatan monocular dan multi-view. Mahasiswa dituntut untuk memahami kapan metode monokuler masih memberikan estimasi yang stabil, dan kapan transisi ke kerangka multi-view mutlak diperlukan untuk menyelesaikan ambigu skala dan struktur.

Pertanyaan kedua menyoroti pengaruh jumlah dan posisi view terhadap rekonstruksi. Distribusi kamera secara langsung memengaruhi kondisi well-posedness dari masalah inverse geometry. Dalam konteks penelitian, hal ini menuntut desain akuisisi data yang optimal, mempertimbangkan trade-off antara coverage area, densitas overlap, dan efisiensi komputasi. Pertanyaan ini juga membuka peluang eksplorasi adaptive camera placement, active vision, atau sampling strategy berbasis reinforcement learning untuk meningkatkan robustness rekonstruksi.

Pertanyaan ketiga mengajak evaluasi kritis terhadap representasi neural untuk scene 3D. Model seperti NeRF merepresentasikan scene sebagai implicit continuous function yang dipelajari via network, menawarkan kualitas rendering tinggi dan detail halus. Namun, representasi ini menghadapi tantangan besar dalam hal training time, memory footprint, generalization cross-scene, dan interpretability geometri. Implikasi risetnya adalah memilih atau merancang representasi hybrid yang selaras dengan constraint aplikasi, apakah itu inferensi real-time, skalabilitas dataset besar, atau interoperabilitas dengan downstream tasks.

Sebagai pendalaman, terdapat beberapa pertanyaan turunan yang harus dijawab secara eksplisit dalam proposal penelitian. Kalibrasi kamera dan asumsi geometri tetap menjadi prasyarat fundamental; error kecil pada intrinsik atau ektrinsik dapat merambat menjadi distorsi sistematis pada triangulasi dan optimization loop. Occlusion dan region yang tidak teramati memerlukan strategi regularization atau semantic-aware inpainting agar rekonstruksi tidak menghasilkan artefak topologis. Terakhir, perbandingan NeRF versus metode tradisional tidak boleh hanya didasarkan pada visual quality, tetapi harus dievaluasi menggunakan metrik geometri (misalnya ATE/RTE), konsistensi multi-view, dan kompatibilitas dengan pipeline computer vision modern.

Pembahasan ini secara langsung menjawab tujuan pembelajaran pada slide sebelumnya, khususnya terkait pemahaman sumber informasi 3D, pembedaan representasi tradisional versus neural, serta identifikasi tantangan teknis seperti occlusion dan sparse view. Selanjutnya, alur materi pada slide berikutnya akan menerjemahkan pertanyaan-pertanyaan ini ke dalam langkah pembelajaran sistematis, mulai dari motivasi, model kamera, multi-view geometry, hingga implementasi praktis dan eksplorasi research gap yang siap dikembangkan menjadi konsep disertasi.

---

## Slide 004 - Agendan dan Alur Materi

### Narasi

Slide ini menyajikan agendan dan alur pembelajaran yang akan kita tempuh. Materi disusun secara bertahap untuk membangun pemahaman geometri klasik terlebih dahulu, sebelum beralih ke representasi neural modern yang menjadi fokus riset tingkat doktor.

Alur dimulai dari motivasi 3D vision dan posisinya dalam computer vision. Pembahasan ini akan langsung menjawab pertanyaan kunci pada slide sebelumnya, khususnya mengenai batas fundamental antara pendekatan monocular versus multi-view, serta bagaimana jumlah dan posisi kamera memengaruhi akurasi rekonstruksi.

Langkah selanjutnya adalah pemodelan kamera dan proyeksi objek 3D ke bidang citra 2D. Transformasi koordinat ini menjadi prasyarat matematis sebelum masuk ke inti multi-view geometry, meliputi epipolar constraint, fundamental matrix, dan prosedur triangulasi. Setelah fondasi geometris terpenuhi, materi berlanjut ke stereo matching dan estimasi kedalaman, dilanjutkan dengan representasi point cloud serta teknik rekonstruksi tradisional berbasis multi-view.

Pemaparan tahapan klasik ini bertujuan untuk mengidentifikasi keterbatasan metode konvensional, terutama dalam menangani occlusion, region yang tidak teramati, dan beban komputasi yang tinggi. Dari celah metodologis inilah, kita beralih ke neural rendering dan NeRF, yang memanfaatkan representasi implicit dan optimisasi berbasis jaringan saraf untuk menghasilkan scene 3D yang kontinu dan fleksibel.

Bagian akhir alur mencakup tantangan penelitian aktual dan strategi perumusan masalah, yang langsung menyasar kompetensi tingkat S3: mengidentifikasi research gap, merumuskan hipotesis, dan mendesain eksperimen. Praktikum eksplorasi depth dan rekonstruksi sederhana akan menjadi wadah validasi konseptual melalui implementasi kode.

Secara kontekstual, alur ini mengintegrasikan capaian pertemuan sebelumnya. Kapabilitas diffusion models kini relevan diadaptasi untuk 3D generation dan novel view synthesis, sementara backbone CNN atau Vision Transformer yang telah kita pelajari sebelumnya akan berperan sebagai encoder utama dalam pipeline depth estimation maupun training NeRF.

Dengan peta materi yang terstruktur ini, kita siap menelusuri slide berikutnya untuk mengevaluasi urgensi 3D vision, baik dari perspektif aplikasi industri maupun peluang kontribusi ilmiah yang masih bersifat ill-posed dan membutuhkan pendekatan representasi baru.

---

## Slide 005 - Mengapa 3D Vision Penting?

### Narasi

Slide ini menyoroti urgensi 3D vision dalam konteks perkembangan computer vision mutakhir dan implikasinya terhadap riset tingkat doktoral. Pada tabel domain aplikasi, terlihat bahwa kebutuhan representasi 3D telah meluas jauh di luar pemrosesan citra konvensional. Di bidang robotika dan navigasi otonom, estimasi kedalaman serta obstacle avoidance menjadi fondasi bagi sistem SLAM yang harus beroperasi secara real-time di lingkungan dinamis. Ekosistem AR/VR menuntut kemampuan view synthesis dan relighting yang realistis agar interaksi pengguna dengan scene virtual terasa natural. Sektor medis mengandalkan rekonstruksi 3D dari data CT, MRI, atau USG untuk perencanaan bedah presisi, sementara industri otomotif mengintegrasikan LiDAR dan kamera untuk depth estimation yang robust. Digital twin dan scanning objek untuk e-commerce maupun pelestarian budaya juga membuka peluang standarisasi pipeline rekonstruksi skala besar.

Dari perspektif kontribusi ilmiah, 3D vision berperan sebagai jembatan kritis antara persepsi berbasis citra 2D dengan aksi atau representasi dunia nyata. Masalah mendasar yang dihadapi adalah sifat ill-posed dari inversi proyeksi kamera, di mana satu citra 2D dapat dihasilkan oleh tak terhingga konfigurasi scene 3D. Ambiguitas ini justru menjadi ruang riset yang sangat subur untuk pengembangan asumsi geometris, prior statistik, hingga representasi neural yang mampu membatasi solusi ke arah yang fisikal dan konsisten. Tantangan ini sejalan dengan fokus mata kuliah pada identifikasi research gap dan perumusan hipotesis yang terukur.

Merujuk pada alur pembelajaran sebelumnya, motivasi aplikasi dan tantangan matematis ini menjadi landasan mengapa metode tradisional kini berintegrasi erat dengan arsitektur deep learning modern. Representasi hierarkis dari CNN atau Vision Transformer sering diadaptasi sebagai backbone untuk mengekstrak fitur geometri dan semantik yang mendukung estimasi kedalaman maupun rendering neural. Hal ini juga membuka peluang eksplorasi penggunaan foundation models seperti CLIP atau DINOv2 untuk grounding geometri 3D melalui representasi multimodal, sekaligus menghubungkan diskusi dengan penerapan diffusion models untuk novel view synthesis yang dibahas pada pertemuan sebelumnya.

Sebagai kelanjutan logis, slide berikutnya akan mengurai secara teknis bagaimana informasi 3D diekstraksi dari berbagai sumber, meliputi monocular cues, binocular stereo, multi-view reconstruction, hingga sensor aktif seperti RGB-D dan LiDAR. Penjelasan ini akan menekankan bahwa meskipun multi-view mengurangi ambiguitas, tantangan seperti occlusion dan area tak teramati tetap memerlukan desain eksperimen yang ketat serta pemilihan representasi data yang tepat sebelum memasuki tahap neural rendering.

---

## Slide 006 - Dari Citra 2D ke Informasi 3D

### Narasi

Slide ini melanjutkan diskusi mengenai urgensi persepsi tiga dimensi dengan menguraikan mekanisme konversi data visual 2D menjadi representasi spasial 3D. Sebagaimana ditegaskan pada slide sebelumnya, 3D vision berfungsi sebagai penghubung kritis antara interpretasi citra datar dan aksi fisik di lingkungan nyata. Namun, proses transformasi ini secara inheren menghadapi kendala matematis berupa masalah *ill-posed*, di mana solusi unik jarang dapat diperoleh tanpa asumsi tambahan atau regularisasi yang kuat.

Informasi geometri 3D dapat dipulihkan dari berbagai sumber akuisisi data, masing-masing dengan trade-off tersendiri:
- **Monocular cues**: Memanfaatkan isyarat visual seperti bayangan, gradien tekstur, perspektif linier, dan ukuran relatif objek untuk memperkirakan kedalaman dari citra tunggal. Metode ini sangat bergantung pada *prior* alamiah manusia atau model pembelajaran mesin.
- **Binocular stereo**: Menggunakan sepasang kamera dengan baseline terkalibrasi untuk menghitung disparitas pixel. Akurasinya tinggi pada area berstruktur, namun rentan gagal pada permukaan homogen atau reflektif.
- **Multi-view**: Mengumpulkan banyak observasi dari sudut berbeda, menjadi fondasi bagi *Structure-from-Motion*, *Multi-View Stereo*, hingga teknik rendering neural modern. Konsistensi epipolar menjadi kunci validasi geometri.
- **Sensor aktif**: LiDAR, Time-of-Flight, atau structured light memberikan pengukuran jarak langsung. Data mentah biasanya berbentuk *point cloud* atau peta kedalaman, meminimalkan ambiguitas inferensi tetapi menambah beban kalibrasi dan biaya perangkat keras.

Poin krusial yang perlu ditekankan adalah bahwa estimasi kedalaman dari citra tunggal bersifat ambigu secara fundamental. Tak terhingga variasi scene 3D dapat memproyeksikan hasil rasterisasi yang identik pada bidang gambar 2D. Penggunaan multi-view memang menekan ambiguitas melalui konsistensi silang antar frame, namun tantangan praktis seperti *occlusion*, pencahayaan dinamis, area tak terekam (*unobserved regions*), serta kesulitan *feature correspondence* pada tekstur repetitif tetap menjadi hambatan signifikan. Pada tingkat penelitian doktoral, strategi penanganan sering kali melibatkan integrasi *physics-based lighting models*, regularisasi topologi, atau paradigma *self-supervised learning* yang mengeksploitasi konsistensi temporal dan geometri prospektif.

Transisi logis dari ekstraksi informasi ke penyimpanan dan komputasi akan dibahas pada slide berikutnya. Setelah memahami sumber data, langkah selanjutnya adalah menentukan format representasi yang optimal. Dari struktur diskret konvensional seperti *point cloud*, *mesh*, dan *voxel* yang menghadapi batasan resolusi memori atau diskontinuitas permukaan, tren riset terkini mengarah pada *continuous neural representations* seperti *neural radiance fields*. Pendekatan ini memungkinkan interpolasi geometri dan radiance yang mulus, sekaligus membuka peluang arsitektur hibrida yang menggabungkan efisiensi komputasi struktur diskret dengan kapasitas ekspresif model neural.

---

## Slide 007 - Representasi Scene 3D

### Narasi

Setelah membahas berbagai sumber informasi tiga dimensi pada slide sebelumnya, kini kita beralih ke bagaimana data spasial tersebut diorganisasikan secara komputasional. Representasi scene 3D bukan sekadar format penyimpanan, melainkan fondasi yang menentukan bagaimana algoritma computer vision memproses geometri, tekstur, dan hubungan topologi dalam ruang kontinu maupun diskrit. Berikut adalah karakteristik utama dari masing-masing representasi yang sering muncul dalam literatur terkini:

- **Point cloud**: Struktur paling sederhana yang menyimpan koordinat XYZ beserta fitur tambahan seperti intensitas atau warna. Kelemahan utamanya terletak pada ketiadaan informasi konektivitas permukaan, sehingga sulit digunakan untuk operasi berbasis mesh atau simulasi fisika.
- **Mesh**: Mendefinisikan permukaan melalui kumpulan wajah poligonal yang sangat efisien untuk rendering real-time dan simulasi. Namun, rekonstruksi permukaan yang akurat dari data mentah masih memerlukan algoritma kompleks seperti Poisson Surface Reconstruction atau Delaunay triangulation.
- **Voxel**: Merepresentasikan ruang sebagai grid tiga dimensi yang kompatibel langsung dengan arsitektur CNN 3D standar. Tantangan utamanya adalah *curse of dimensionality*, di mana kebutuhan memori tumbuh secara kubik seiring peningkatan resolusi.
- **Depth map**: Pendekatan pragmatis berupa peta kedalaman dua setengah dimensi yang mudah diestimasi dari model monocular atau sensor aktif. Representasi ini terbatas pada satu sudut pandang dan cenderung kehilangan informasi geometri penuh pada area occlusion.
- **Neural field (NeRF)**: Memodelkan scene sebagai fungsi kontinu yang dipelajari oleh neural network, mampu menghasilkan render fotorealistik dari sudut pandang baru. Meskipun powerful, training konvensional masih membutuhkan komputasi intensif dan jumlah view yang masif.

Arah riset saat ini menunjukkan pergeseran paradigma dari representasi diskret menuju *continuous neural representation*. Untuk mengatasi keterbatasan komputasi dan memori, pendekatan hybrid mulai banyak diadopsi, misalnya dengan menggabungkan point cloud sparse sebagai kerangka awal lalu memperkaya setiap titik dengan fitur neural yang diproyeksikan ke berbagai view. Teknik seperti 3D Gaussian Splatting juga sedang berkembang sebagai alternatif yang menggabungkan efisiensi rendering tradisional dengan fleksibilitas representasi berbasis neural.

Pemahaman mendalam tentang representasi ini menjadi prasyarat mutlak sebelum memasuki tahap pemetaan geometri kamera. Pada slide berikutnya, kita akan menguraikan model proyeksi pinhole yang menjadi dasar matematis dalam menghubungkan titik dunia 3D dengan observasi citra 2D, termasuk dekomposisi matriks intrinsik dan ekstrinsik yang krusial untuk multi-view geometry dan pipeline neural rendering.

---

## Slide 008 - Model Kamera: Proyeksi Pinhole

### Narasi

Slide ini memperkenalkan fondasi geometri proyeksi yang menjadi prasyarat mutlak dalam pipeline 3D vision dan multi-view geometry. Setelah membahas berbagai bentuk representasi scene 3D pada slide sebelumnya, langkah selanjutnya adalah memahami secara matematis bagaimana objek tiga dimensi direkam oleh sensor kamera menjadi citra dua dimensi. Model pinhole dipilih sebagai basis teoritis karena menawarkan penyederhanaan optika kompleks menjadi transformasi perspektif yang dapat dimodelkan secara aljabar linear, sekaligus menjadi acuan standar dalam algoritma Structure-from-Motion, dense reconstruction, hingga neural rendering.

Pada bagian proyeksi, slide menampilkan pemetaan dari titik dunia $X = (X, Y, Z)$ ke koordinat piksel $x = (u, v)$. Persamaan yang ditunjukkan menggambarkan proyeksi perspektif di mana komponen kedalaman $Z$ berfungsi sebagai pembagi, menciptakan efek konvergensi optik yang realistis. Parameter $f_x$ dan $f_y$ merepresentasikan focal length dalam satuan piksel, yang mencerminkan skala optik setelah disesuaikan dengan resolusi sensor. Sementara itu, $(c_x, c_y)$ menandai principal point atau pusat proyeksi optik pada bidang gambar. Pemahaman numerik terhadap parameter-parameter ini sangat krusial karena kesalahan estimasi langsung berimbas pada akurasi rekonstruksi 3D.

Dekomposisi matriks kamera memisahkan karakteristik internal perangkat dari pose spasialnya. Matriks intrinsik $K$ mengkompensasi sifat fisik kamera seperti focal length, principal point, dan skew antar sumbu piksel. Di sisi lain, matriks ekstrinsik $[R|t]$ mendeskripsikan orientasi dan posisi kamera dalam ruang dunia melalui operasi rotasi $R$ dan translasi $t$. Kombinasi keduanya menghasilkan persamaan proyeksi lengkap $x = K [R|t] X$, yang menjadi inti dari epipolar geometry dan triangulasi multi-view. Dalam konteks riset tingkat doktoral, formulasi ini sering kali didiferensialkan atau digabungkan dengan jaringan saraf untuk memungkinkan end-to-end learning pada tugas seperti neural radiance fields atau differentiable rendering.

Perlu ditekankan bahwa model pinhole mengasumsikan lensa ideal tanpa aberrasi optik. Padahal, pada implementasi nyata, distorsi radial dan tangensial selalu hadir akibat konstruksi lensa fisikal. Ketidakakuratan dalam memodelkan proyeksi ini akan menyebabkan propagasi error yang signifikan pada estimasi struktur 3D dan konsistensi view synthesis. Oleh karena itu, pemahaman mendalam tentang dekomposisi matriks kamera ini menjadi landasan wajib sebelum kita membahas teknik kalibrasi dan koreksi distorsi lensa pada slide berikutnya, yang secara langsung menentukan validitas empiris eksperimen computer vision Anda.

---

## Slide 009 - Distorsi dan Kalibrasi Kamera

### Narasi

Setelah memahami proyeksi ideal pada model pinhole, kita sekarang memasuki realitas optik yang lebih kompleks: distorsi lensa dan kalibrasi kamera. Dalam praktik akuisisi citra, elemen optik fisik tidak mematuhi proyeksi linear sempurna. Distorsi utama yang dominan adalah distorsi radial, yang termanifestasi sebagai efek barrel atau pincushion tergantung pada desain lensa. Selain itu, distorsi tangensial sering muncul akibat ketidaksejajaran mekanis antara modul lensa dan sensor gambar. Secara matematis, koreksi ini dimodelkan dengan ekspansi polinomial, misalnya `x_distorted = x(1 + k1 r^2 + k2 r^4 + ...)`, di mana `k1` dan `k2` merepresentasikan koefisien distorsi radial yang harus diestimasi secara presisi.

Proses untuk memperoleh parameter intrinsik `K` beserta koefisien distorsi tersebut disebut kalibrasi kamera. Metode yang paling standar dan banyak diadopsi dalam literatur computer vision adalah chessboard calibration, yang telah tersedia secara efisien dalam pustaka OpenCV. Algoritma ini bekerja dengan mendeteksi grid checkerboard pada serangkaian citra acak, lalu meminimalkan reprojection error untuk menemukan set parameter kamera yang paling konsisten dengan data observasi.

Dari perspektif riset tingkat doktor, kalibrasi bukanlah sekadar langkah preprocessing rutin, melainkan fondasi validitas eksperimental. Rekonstruksi tiga dimensi dan analisis multi-view sangat sensitif terhadap kesalahan intrinsik. Deviasi kecil pada `K` atau koefisien distorsi akan berlipat ganda saat menghitung garis epipolar dan melakukan triangulasi titik-titik correspondence antar view. Oleh karena itu, dokumentasi protokol kalibrasi, termasuk jumlah frame, kondisi pencahayaan, dan metrik reprojection error, wajib dilaporkan secara transparan dalam setiap publikasi ilmiah.

Meskipun model distorsi polinomial telah menjadi standar de facto, pendekatan ini memiliki batas inherent yang akan kita diskusikan pada slide berikutnya terkait asumsi dan keterbatasan model kamera. Pemahaman mendalam tentang bagaimana kesalahan kalibrasi merambat ke pipeline downstream akan membantu Anda merancang eksperimen yang lebih robust dan mengidentifikasi celah penelitian, seperti pengembangan camera models yang adaptif terhadap kondisi pencahayaan ekstrem atau lensa wide-angle khusus.

---

## Slide 010 - Asumsi dan Keterbatasan Model Kamera

### Narasi

Pada slide sebelumnya, kita telah membahas mekanisme distorsi lensa serta prosedur kalibrasi kamera untuk memperoleh matriks intrinsik dan koefisien koreksi. Namun, sebelum memasuki ranah geometri multi-view, kita perlu mengkritisi validitas model yang menjadi fondasi perhitungan tersebut. Model kamera standar, seperti pinhole atau model lensa sederhana, beroperasi di atas sejumlah asumsi idealisasi yang jarang terpenuhi sepenuhnya pada kondisi lapangan atau eksperimen kompleks.

Secara umum, asumsi standar ini mencakup tiga pilar utama. Pertama, sistem optik dianggap mematuhi model pinhole atau model lensa parsial yang dapat dikoreksi secara analitik. Kedua, pencahayaan diasumsikan merata, tanpa interferensi refraksi kompleks atau pantulan internal antar elemen optik. Ketiga, karakteristik sensor dan alignment mekanik lensa dianggap statis selama seluruh proses akuisisi data. Dalam lingkungan terkontrol, asumsi ini sering kali memberikan residual error yang dapat ditoleransi.

Namun, pada praktik riset tingkat lanjut, keterbatasan model ini menjadi faktor kritis yang harus diidentifikasi. Penggunaan lensa wide-angle ekstrem menghasilkan distorsi non-linear yang tinggi, sehingga pendekatan polinomial orde rendah tidak lagi memadai. Sistem rolling shutter memperkenalkan artefak geometris berupa distorsi temporal pada scene bergerak, yang melanggar asumsi eksposur simultan. Permukaan dengan refleksi kuat atau sifat transparan akan melanggar asumsi Lambertian, menyebabkan kegagalan estimasi kedalaman berbasis intensitas. Selain itu, area dengan tekstur seragam menimbulkan ambiguities kedalaman, sehingga pipeline stereo matching konvensional cenderung gagal konvergen atau menghasilkan noise struktural.

Implikasi langsung dari keterbatasan ini bagi peneliti adalah kebutuhan strategis dalam pemilihan model kamera yang selaras dengan domain aplikasi. Jika penelitian Anda melibatkan dinamika kecepatan tinggi, material reflektif, atau struktur semi-transparan, model pinhole standar perlu diperluas dengan parameter dinamis, pemodelan BRDF, atau pendekatan berbasis neural implicit representations. Lebih penting lagi, dokumentasikan setiap asumsi kalibrasi secara eksplisit dalam concept note dan bagian metodologi. Transparansi ini menjadi dasar justifikasi novelty, memperkuat validitas eksperimen, dan memudahkan replikasi oleh komunitas ilmiah.

Dengan memahami batasan model kamera saat ini, kita siap membangun kerangka kerja geometris yang lebih robust. Slide berikutnya akan membahas epipolar geometry, di mana kita akan menurunkan constraint matematis yang menghubungkan dua pandangan kamera melalui fundamental matrix, sekaligus menunjukkan bagaimana prinsip ini mereduksi kompleksitas stereo matching dari pencarian 2D menjadi pencarian 1D sepanjang epipolar line.

---

## Slide 011 - Epipolar Geometry: Dua Kamera dan Satu Titik 3D

### Narasi

Setelah menelaah asumsi standar dan batasan fisik model kamera pada slide sebelumnya, kita kini memasuki fondasi geometri multi-view yang menjadi tulang punggung estimasi struktur 3D dan registrasi pandangan. Ketika satu titik ruang tiga dimensi diamati oleh dua kamera berbeda, hubungan spasial antar proyeksinya tidak acak, melainkan terikat oleh aturan geometri yang ketat. Prinsip inilah yang dikenal sebagai *epipolar geometry*, dan pemahaman mendalam tentangnya wajib dimiliki peneliti tingkat doktoral sebelum menyentuh metode rekonstruksi modern atau neural rendering berbasis multi-view.

Secara konseptual, misalkan terdapat titik 3D `X` yang terekam di dua posisi kamera dengan pusat optik `C1` dan `C2`. Proyeksi titik tersebut pada bidang sensor masing-masing kamera adalah `x1` dan `x2`. Secara geometris, titik 3D `X`, pusat kamera `C1`, dan pusat kamera `C2` selalu terletak pada satu bidang datar tunggal yang disebut sebagai **epipolar plane**. Bidang ini bersifat dinamis; ia berputar mengikuti gerakan relatif kamera atau objek, namun selalu mempertahankan sifat planar yang konsisten.

Karena semua titik korespondensi harus berada pada bidang yang sama, lokasi proyeksi `x1` pada kamera pertama secara otomatis membatasi posisi pasangan `x2` pada kamera kedua. Batasan ini muncul dalam bentuk garis lurus pada bidang gambar kamera kedua, yang dinamakan **epipolar line**. Implikasi komputasinya sangat besar: pencarian pasangan titik (*stereo matching*) yang semula merupakan masalah optimasi dua dimensi, direduksi menjadi pencarian satu dimensi sepanjang epipolar line tersebut. Reduksi dimensi ini secara drastis menurunkan beban komputasi, mempercepat konvergensi, dan meminimalkan false positives akibat area dengan tekstur seragam.

Secara aljabar linear, hubungan ini dikodekan dalam persamaan kuadratik berikut:
```
[ u2 v2 1 ] F [ u1 v1 1 ]^T = 0
```
Persamaan ini menyatakan bahwa perkalian silang koordinat homogen dari pasangan titik korespondensi, yang dimediasi oleh suatu matriks koefisien, menghasilkan nilai nol. Matriks `F` yang berperan di sini adalah **fundamental matrix**, yang beroperasi langsung pada domain piksel tanpa memerlukan pengetahuan mengenai parameter intrinsik kamera. Jika koordinat piksel terlebih dahulu dinormalisasi menggunakan matriks intrinsik `K`, maka `F` berubah menjadi **essential matrix** (`E`). Perbedaan domain representasi dan kebutuhan kalibrasi inilah yang menentukan kapan masing-masing matriks digunakan dalam pipeline riset, yang akan kita bedah secara formal pada slide berikutnya.

Dalam konteks penelitian tingkat S3, epipolar constraint bukan sekadar alat verifikasi geometri klasik, melainkan komponen kritis untuk filtering correspondences yang robust, validasi konsistensi multi-view, dan regularisasi spasial pada arsitektur deep learning berbasis graf atau transformer. Memahami bagaimana constraint ini menjembatani observasi 2D dengan inferensi 3D akan memudahkan Anda dalam menganalisis kegagalan model, merancang loss function yang aware geometri, atau mengevaluasi foundation models untuk tugas 3D vision. Pada slide selanjutnya, kita akan menguraikan perbedaan teknis antara fundamental dan essential matrix, serta mekanisme dekomposisi untuk mengekstrak rotasi dan translasi relatif antar kamera.

---

## Slide 012 - Fundamental Matrix dan Essential Matrix

### Narasi

Pada slide sebelumnya, kita telah membahas konsep geometri epipolar dan bagaimana proyeksi titik tiga dimensi dari dua view membentuk persamaan kendala epipolar. Persamaan tersebut memperkenalkan matriks `F`, namun dalam praktik pengolahan citra digital tingkat lanjut, pemilihan representasi matriks bergantung pada ketersediaan informasi kalibrasi kamera. Berikut adalah perbedaan mendasar antara Fundamental Matrix dan Essential Matrix yang perlu dipahami sebelum masuk ke tahap estimasi numerik:

- **Fundamental Matrix (`F`)**: Bekerja langsung pada koordinat piksel mentah. Tidak memerlukan pengetahuan parameter intrinsik kamera, sehingga cocok untuk skenario uncalibrated atau ketika kalibrasi belum tersedia.
- **Essential Matrix (`E`)**: Beroperasi pada koordinat ternormalisasi. Memerlukan inversi matriks intrinsik kamera (`K`) untuk mengubah piksel ke ruang metric, sehingga memberikan representasi geometri yang lebih akurat.

Hubungan di antara kedua matriks ini bersifat linear dan dapat ditransformasikan melalui parameter intrinsik masing-masing view. Secara matematis, hubungannya dinyatakan sebagai `E = K2^T F K1`. Kelebihan utama penggunaan `E` terletak pada kemampuannya mengodekan pose relatif kamera. Matriks ini menyimpan informasi rotasi `R` dan translasi `t` antara dua posisi kamera. Ketika dilakukan dekomposisi, persamaan akan menghasilkan empat pasangan solusi `(R, t)`. Hanya satu solusi yang konsisten secara geometris, yaitu ketika titik triangulasi berada di depan bidang lensa kedua kamera. Validasi positif depth menjadi langkah wajib untuk memilih solusi yang benar.

Dalam konteks penelitian doktoral, penguasaan terhadap `F` dan `E` bukan hanya materi klasik, melainkan fondasi metodologis yang masih aktif dikembangkan. Matriks ini sering diintegrasikan ke dalam pipeline modern untuk validasi konsistensi geometri, penyaringan korespondensi fitur yang robust terhadap noise, serta menjadi blok inti dalam Structure-from-Motion (SfM) dan Visual Odometry. Banyak paper terbaru menggabungkan kendala geometris ini dengan representasi neural atau self-supervised learning untuk meningkatkan akurasi estimasi struktur 3D tanpa dependensi penuh pada ground truth.

Untuk menerjemahkan konsep teoritis ini ke dalam implementasi komputasional, kita perlu memahami algoritma numerik yang stabil. Slide berikutnya akan membahas estimasi Fundamental Matrix melalui 8-Point Algorithm, mencakup prosedur normalisasi koordinat, pembentukan design matrix, penyelesaian least squares via SVD, serta penerapan RANSAC untuk menangani outlier. Pendekatan ini akan menjadi dasar eksperimen Anda saat menguji pipeline multi-view geometry pada dataset nyata.

---

## Slide 013 - Estimasi Fundamental Matrix: 8-Point Algorithm

### Narasi

Setelah pada slide sebelumnya kita membahas definisi formal serta hubungan aljabar antara Fundamental Matrix dan Essential Matrix, kini kita beralih ke aspek komputasionalnya. Bagaimana cara memperoleh nilai numerik matriks F dari sekumpulan pasangan titik korespondensi yang diamati? Pendekatan standar yang menjadi fondasi banyak pipeline multi-view geometry adalah 8-Point Algorithm, sebuah metode estimasi berbasis persamaan linear yang memerlukan ketelitian numerik tinggi.

Ide dasarnya sederhana namun menuntut pemahaman mendalam tentang stabilitas komputasi. Setiap pasangan titik korespondensi yang valid menghasilkan tepat satu persamaan linear dalam sembilan komponen matriks F. Dengan mengumpulkan delapan titik atau lebih, sistem persamaan menjadi overdetermined dan dapat diselesaikan melalui linear least squares. Poin kritis yang menentukan keberhasilan estimasi adalah penerapan normalisasi koordinat sesuai prosedur Hartley. Tanpa normalisasi ini, matriks desain yang terbentuk cenderung ill-conditioned, sehingga solusi yang dihasilkan sangat rentan terhadap propagasi noise dan kehilangan presisi floating-point.

Berikut adalah alur pseudocode yang merepresentasikan proses estimasi secara komputasional:
```python
import numpy as np

### 6. Denormalize F

```
Mari kita uraikan setiap langkah implementasinya:
1. **Normalisasi titik**: Koordinat piksel ditransformasi agar pusat massanya berada di origin dan rata-rata kuadrat jarak ke origin bernilai dua. Transformasi affine ini menstabilkan kondisi numerik matriks desain.
2. **Pembangunan matriks desain A**: Berdasarkan persamaan epipolar $x_2^T F x_1 = 0$, setiap pasangan titik mengisi satu baris pada matriks A berukuran $(N, 9)$.
3. **Dekomposisi SVD**: Matriks A didekomposisi menjadi $U \Sigma V^T$. Solusi least squares berada di null space A, yang direpresentasikan oleh baris terakhir dari $V^T$.
4. **Penegangan constraint rank-2**: Matriks sementara direshape menjadi $3 \times 3$ dan dilakukan SVD ulang. Singular value terkecil disetel ke nol untuk memaksa rank matriks menjadi tepat dua, sesuai sifat geometrik fundamental matrix.
5. **De-normalisasi**: Matriks F yang telah terkoreksi dikalikan dengan transformasi invers normalisasi dari kedua view, mengembalikan estimasi ke domain koordinat piksel asli.

Dalam konteks riset dan eksperimen tingkat lanjut, data pengamatan hampir selalu mengandung outlier akibat kesalahan deteksi fitur, korespondensi ambigu, atau distorsi lensa. Estimator linear murni tidak mampu menangani hal ini secara robust. Oleh karena itu, pipeline standar wajib mengintegrasikan RANSAC untuk melakukan sampling iteratif, menghitung reprojection error, dan memisahkan inlier dari outlier secara adaptif. Untuk akselerasi prototipe, OpenCV menyediakan fungsi `cv2.findFundamentalMat` yang mengemas seluruh mekanisme ini, termasuk dukungan estimator alternatif seperti LMEDS atau RHO, sehingga peneliti dapat langsung berfokus pada validasi geometri dan analisis residual.

Setelah fundamental matrix berhasil diestimasi dan divalidasi, langkah struktural berikutnya adalah memulihkan geometri tiga dimensi dari proyeksi dua view. Pada slide selanjutnya, kita akan membahas prinsip triangulasi, yaitu metode rekonsiliasi dua sinar epipolar untuk menemukan posisi titik 3D yang meminimalkan error geometrik, beserta faktor-faktor yang secara langsung memengaruhi akurasi rekonstruksi depth.

---

## Slide 014 - Triangulasi: Menghitung Titik 3D dari Dua Ray

### Narasi

Setelah pada slide sebelumnya kita membahas estimasi matriks fundamental melalui algoritma 8-point, langkah logis berikutnya adalah mengubah korespondensi titik 2D tersebut menjadi koordinat ruang tiga dimensi. Proses inilah yang disebut sebagai triangulasi, yaitu metode untuk merekonstruksi posisi titik 3D berdasarkan dua sinar proyeksi dari kamera yang berbeda.

Secara prinsip, jika kita telah mengetahui posisi titik korespondensi `x1` dan `x2` serta matriks proyeksi kamera `P1` dan `P2`, maka titik 3D `X` harus memenuhi persamaan proyeksi `x1 = P1 X` dan `x2 = P2 X`. Kedua persamaan ini dapat digabungkan menjadi sistem linear homogen berbentuk `A X = 0`. Solusi dari sistem ini memberikan perkiraan awal lokasi titik di ruang 3D.

Terdapat beberapa pendekatan komputasi untuk menyelesaikan masalah triangulasi ini. Metode pertama adalah **triangulasi linear** atau *Direct Linear Transform* (DLT), yang secara langsung menyelesaikan sistem `AX=0` menggunakan dekomposisi nilai singular (SVD). Pendekatan kedua adalah **metode midpoint**, yang mencari titik tengah pada segmen garis terpendek yang menghubungkan kedua sinar proyeksi, sehingga meminimalkan jarak geometris secara sederhana. Untuk tingkat akurasi tertinggi, khususnya dalam konteks penelitian S3, kita beralih ke **triangulasi optimal**. Metode ini memformulasikan ulang masalah sebagai minimisasi error geometrik, biasanya menggunakan *Sampson distance* atau *reprojection error*, yang memerlukan optimasi non-linear seperti Gauss-Newton atau Levenberg-Marquardt.

Akurasi hasil triangulasi sangat bergantung pada beberapa faktor geometri dan instrumental. Berikut adalah rincian pengaruh masing-masing faktor terhadap ketidakpastian rekonstruksi:
- **Baseline sempit**: Jarak antar kamera yang terlalu dekat menyebabkan sudut parallax kecil, sehingga ketidakpastian estimasi depth meningkat secara eksponensial.
- **Sudut pandang ekstrem**: Perbedaan orientasi kamera yang terlalu besar dapat memicu degenerasi numerik dan penurunan kualitas korespondensi fitur.
- **Noise pada korespondensi**: Kesalahan sub-pixel pada deteksi titik 2D akan merambat (*propagate*) langsung ke domain 3D, terutama jika kondisi geometri tidak ideal.
- **Kalibrasi tidak akurat**: Parameter intrinsik dan ekstrinsik yang bias akan menghasilkan rekonstruksi dengan distorsi sistematis yang sulit dikoreksi hanya melalui optimasi.

Penting untuk dicatat bahwa triangulasi standar bersifat lokal dan tidak memperhitungkan konsistensi multi-view secara global. Oleh karena itu, dalam pipeline *Structure-from-Motion* yang akan kita bahas pada slide berikutnya, hasil triangulasi awal ini berfungsi sebagai titik awal (*initialization*). Posisi titik 3D dan pose kamera yang diperoleh kemudian akan dioptimalkan bersama-sama melalui *bundle adjustment*, sebelum akhirnya dikembangkan menjadi *dense point cloud* melalui teknik *Multi-View Stereo*.

---

## Slide 015 - Structure-from-Motion dan Multi-View Reconstruction

### Narasi

Setelah membahas prinsip dan metode triangulasi pada slide sebelumnya, kita kini memasuki kerangka kerja yang lebih komprehensif, yaitu Structure-from-Motion (SfM). SfM bukan sekadar algoritma tunggal, melainkan sebuah alur pemrosesan sistematis yang mengintegrasikan geometri multi-view untuk merekonstruksi struktur 3D sekaligus memperkirakan pose kamera secara simultan.

Alur SfM yang tercantum dalam slide ini dapat diuraikan menjadi lima tahap utama:
1. Deteksi fitur lokal menggunakan descriptor klasik seperti SIFT atau ORB, maupun arsitektur modern berbasis deep learning seperti SuperPoint.
2. Pencocokan fitur antar gambar dengan verifikasi geometris untuk menyaring outlier.
3. Estimasi matriks fundamental (`F`) atau essential (`E`), yang kemudian didekomposisi untuk memperoleh rotasi (`R`) dan translasi (`t`) relatif antar view.
4. Triangulasi titik 3D berdasarkan sinar proyeksi dari masing-masing kamera, sesuai dengan mekanisme yang telah dibahas pada slide sebelumnya.
5. Bundle adjustment untuk optimasi bersama posisi kamera dan koordinat titik 3D.

Output akhir dari pipeline SfM standar adalah sparse point cloud beserta pose kamera yang telah dioptimasi. Dalam konteks riset tingkat doktoral, sparse reconstruction ini umumnya menjadi langkah awal sebelum dilanjutkan ke Multi-View Stereo (MVS). MVS memanfaatkan informasi depth dari beberapa view untuk menghasilkan dense point cloud atau mesh permukaan, yang kemudian dapat digunakan untuk aplikasi downstream seperti novel view synthesis atau training neural radiance fields.

Perlu dicatat bahwa keberhasilan SfM sangat bergantung pada kualitas korespondensi fitur, overlap antar gambar, serta ketahanan terhadap degenerasi geometrik. Untuk sistem yang skalabel dan robust, implementasi open-source seperti COLMAP atau OpenMVG menjadi standar de facto dalam riset computer vision terkini, termasuk sebagai preprocessing wajib sebelum melatih model neural rendering.

Tahap kelima dalam alur SfM, yaitu bundle adjustment, merupakan komponen kritis yang menentukan konsistensi global hasil rekonstruksi. Pembahasan mengenai bagaimana reprojection error diminimalkan, mengapa optimasi ini menghilangkan drift akumulatif, serta kaitannya dengan akurasi pose untuk NeRF, akan kita bedah secara mendalam pada slide berikutnya.

---

## Slide 016 - Bundle Adjustment

### Narasi

Pada slide ini, kita membahas inti dari tahap optimasi dalam pipeline Structure-from-Motion, yaitu Bundle Adjustment. Setelah proses triangulasi menghasilkan titik-titik 3D awal dan estimasi pose kamera kasar, langkah selanjutnya adalah menyempurnakan seluruh parameter secara simultan. Yang dioptimalkan pada tahap ini mencakup dua komponen utama: parameter kamera, baik intrinsik maupun ekstrinsik, serta koordinat spasial dari setiap titik 3D yang telah direkonstruksi.

Tujuan utamanya adalah meminimalkan reprojection error, yang dirumuskan sebagai berikut:
```
min Σ_i Σ_j || x_ij - P_j X_i ||^2
```
Dalam persamaan ini, $x_{ij}$ merepresentasikan posisi piksel teramati pada gambar ke-$j$ untuk titik 3D ke-$i$, sedangkan $P_j$ adalah matriks proyeksi kamera ke-$j$. Nilai $X_i$ adalah koordinat titik 3D yang dicari. Proses minimisasi non-linear ini memastikan bahwa proyeksi balik titik 3D ke bidang citra paling sesuai dengan observasi aktual dari semua sudut pandang secara bersamaan.

Pentingnya Bundle Adjustment terletak pada kemampuannya menghilangkan drift dan error akumulatif yang sering muncul dari estimasi pose berurutan. Dengan mengoptimalkan pose kamera dan struktur 3D secara global, hasil akhir menjadi konsisten secara geometris. Inilah sebabnya mengapa algoritma ini menjadi komponen inti dalam berbagai perangkat lunak standar seperti COLMAP, OpenMVG, serta sistem SLAM modern.

Jika kita menelaah koneksi dengan slide sebelumnya, Bundle Adjustment merupakan kelanjutan langsung dari tahap trianguasi dalam pipeline SfM. Tanpa optimasi ini, sparse point cloud dan pose kamera yang dihasilkan akan memiliki noise tinggi dan inkonsistensi geometri. Di sisi lain, kaitan materi ini dengan Neural Rendering sangat krusial. Model NeRF membutuhkan pose kamera yang sangat akurat sebagai input awal. Oleh karena itu, COLMAP yang menjalankan Bundle Adjustment sering dijadikan preprocessing wajib sebelum melatih NeRF. Ketepatan pose kamera secara langsung menentukan kualitas novel view synthesis; kesalahan kecil pada pose dapat menyebabkan artefak geometri atau blur pada render sintesis.

Sebagai transisi ke materi berikutnya, setelah pose dan struktur 3D berhasil distabilkan melalui Bundle Adjustment, kita dapat mengeksplorasi pendekatan berbasis pasangan kamera atau estimasi kedalaman. Slide berikutnya akan membahas bagaimana prinsip stereo matching dan perhitungan depth map memanfaatkan baseline dan focal length untuk menghasilkan representasi kedalaman yang lebih padat, yang dapat melengkapi atau menjadi alternatif efisien terhadap pipeline multi-view geometry lengkap.

---

## Slide 017 - Stereo Matching dan Depth Estimation

### Narasi

Setelah pada slide sebelumnya membahas bagaimana Bundle Adjustment menyempurnakan pose kamera dan struktur 3D secara global melalui minimisasi reprojection error, kita kini beralih ke salah satu pendekatan paling mendasar dalam rekonstruksi tiga dimensi: Stereo Matching dan Estimasi Kedalaman. Meskipun metode multi-view geometry seperti SfM dan MVS sangat powerful, stereo matching tetap menjadi fondasi penting, terutama untuk aplikasi real-time, robotika, dan sistem yang memerlukan estimasi depth langsung dari pasangan citra binokular tanpa memerlukan rekonsiliasi view yang kompleks.

Konsep dasar stereo memanfaatkan prinsip triangulasi geometris. Ketika dua kamera sejajar mengalami proses rectifikasi, garis epipolar akan menjadi horizontal dan sejajar dengan sumbu x. Hal ini menyederhanakan pencarian korespondensi karena piksel pada citra kiri hanya perlu dicari di sepanjang baris yang sama pada citra kanan. Jarak horizontal antara titik koresponden tersebut disebut sebagai disparity, yang dilambangkan dengan `d = u_left - u_right`. Disparity memiliki hubungan terbalik dengan kedalaman absolut objek, yang dapat dimodelkan melalui persamaan fundamental stereo:

```
Z = (f * B) / d
```

Dalam rumus ini, `Z` merepresentasikan jarak atau depth objek dari bidang kamera, `f` adalah focal length lensa, dan `B` adalah baseline atau jarak fisik antara pusat optik kedua kamera. Persamaan ini menunjukkan bahwa semakin besar disparity, semakin dekat objek tersebut, dan sebaliknya. Akurasi estimasi depth sangat bergantung pada ketepatan pengukuran disparity dan kalibrasi intrinsik serta ekstrinsik kamera.

Alur kerja stereo matching umumnya mengikuti empat tahapan utama yang sistematis. Pertama, rectifikasi dilakukan untuk menyelaraskan kedua citra sehingga epipolar line menjadi horizontal, menghilangkan distorsi geometris antar view. Kedua, tahap matching mencari korespondensi piksel berdasarkan kesamaan intensitas atau fitur. Ketiga, hasil pencocokan dikompilasi menjadi disparity map, yaitu peta dua dimensi yang menyimpan nilai disparity untuk setiap piksel citra referensi. Terakhir, disparity map dikonversi menjadi depth map menggunakan rumus triangulasi di atas, menghasilkan representasi kedalaman siap pakai untuk rendering, navigasi, atau analisis scene.

Memahami konsep dan alur ini menjadi prasyarat kritis sebelum mengevaluasi evolusi algoritma yang mendominasi bidang ini. Pada slide berikutnya, kita akan membedah bagaimana metode stereo matching berevolusi dari teknik klasik berbasis optimasi lokal dan global, menuju arsitektur deep learning modern yang mampu menangkap konteks semantik dan tekstur kompleks secara end-to-end.

---

## Slide 018 - Metode Stereo Matching: Dari Klasik ke Modern

### Narasi

Pada slide ini, kita akan mengurai evolusi algoritma *stereo matching* dari pendekatan konvensional menuju arsitektur berbasis *deep learning*. Sebagaimana telah dibahas pada slide sebelumnya mengenai alur kerja stereo matching dan hubungan fundamental antara *disparity* dengan kedalaman, inti permasalahan terletak pada akurasi pencarian korespondensi piksel, khususnya pada region dengan tekstur minim, pola repetitif, atau permukaan specular. Pemahaman terhadap perkembangan metodologi ini krusial untuk mengevaluasi trade-off antara kompleksitas komputasi, ketahanan terhadap noise, dan kemampuan generalisasi ke domain nyata.

Mari kita tinjau terlebih dahulu metode klasik yang menjadi landasan historis bidang ini. *Block matching* dengan metrik seperti Sum of Absolute Differences (SAD) beroperasi secara lokal dengan membandingkan jendela piksel antar gambar kiri dan kanan. Keunggulannya terletak pada kemudahan implementasi dan efisiensi memori, namun metode ini sangat sensitif terhadap ambiguitas karena mengabaikan konteks spasial di luar jendela. Untuk mengatasi kelemahan tersebut, *Semi-Global Matching (SGM)* memperkenalkan formulasi optimasi energi sepanjang jalur epipolar dengan menambahkan *smoothness penalty*, sehingga menghasilkan peta *disparity* yang lebih koheren tanpa beban komputasi penuh seperti optimasi global. Di sisi lain, pendekatan berbasis *Graph Cut* dan *Belief Propagation* memodelkan masalah sebagai *Markov Random Field (MRF)* untuk mencari konfigurasi label optimal secara global, meskipun sering kali menghadapi tantangan skalabilitas pada resolusi tinggi dan kebutuhan parameter tuning yang intensif.

Pergeseran paradigma terjadi ketika jaringan saraf tiruan mulai diintegrasikan untuk ekstraksi fitur dan estimasi *disparity* secara end-to-end. Arsitektur seperti DispNet memanfaatkan kerangka *encoder-decoder* untuk memetakan pasangan citra stereo langsung ke peta kedalaman. PSMNet kemudian meningkatkan representasi spasial dengan membangun *cost volume* berlapis melalui *pyramid stereo matching network*, memungkinkan integrasi informasi multi-skala yang lebih kaya. Terobosan terkini diwakili oleh RAFT-Stereo, yang mengadopsi mekanisme *recurrent update* untuk memperbaiki estimasi *disparity* secara iteratif, mirip dengan prinsip *optical flow* modern. Secara empiris, model-model ini umumnya dilatih secara *supervised* menggunakan dataset sintetis berkualitas tinggi seperti Scene Flow yang menyediakan *ground truth* kedalaman presisi sub-piksel. Namun, untuk deployment pada aplikasi dunia nyata, diperlukan strategi adaptasi domain, augmentasi realistis, atau *fine-tuning* agar model tetap robust terhadap distorsi lensa, variasi iluminasi, dan artefak sensor yang tidak tereksploitasi dalam data sintetis.

Eksplorasi dari metode statistik-kombinatorial menuju pembelajaran representasi mendalam ini tidak hanya meningkatkan akurasi numerik, tetapi juga mengubah cara kita merumuskan masalah geometri 3D. Ketika kita melangkah ke slide berikutnya, kita akan menyoroti bagaimana batasan kebutuhan pasangan kamera dan kalibrasi ketat dapat didekati melalui *Monocular Depth Estimation*, yang memanfaatkan priors struktural dari citra tunggal maupun konsistensi temporal pada urutan video untuk merekonstruksi kedalaman secara implisit.

---

## Slide 019 - Monocular Depth Estimation

### Narasi

Setelah membahas metode stereo matching pada slide sebelumnya yang mengandalkan pasangan citra dan kalibrasi kamera untuk menghitung disparitas, kini kita beralih ke pendekatan yang lebih menantang secara geometris dan komputasional: estimasi kedalaman dari satu citra tunggal atau *monocular depth estimation*. Tantangan fundamentalnya terletak pada sifat ambigu dari proyeksi perspektif. Satu citra 2D dapat direkonstruksi menjadi tak terhingga variasi scene 3D, sehingga informasi kedalaman absolut tidak dapat dipulihkan tanpa asumsi tambahan. Selain itu, skala kedalaman bersifat ambigu; tanpa parameter intrinsik kamera atau referensi objek berukuran diketahui, model hanya mampu menghasilkan kedalaman relatif.

Untuk mengatasi keterbatasan ini, literatur terkini mengembangkan tiga paradigma utama. Pertama, pendekatan *supervised* yang memerlukan data *ground truth* kedalaman dari sensor aktif seperti LiDAR atau stereo rig. Meskipun memberikan akurasi tinggi, ketergantungan pada anotasi manual membatasi skalabilitas dan keberagaman domain. Kedua, pendekatan *self-supervised* yang memanfaatkan urutan video monokular dengan prinsip konsistensi fotometri dan temporal, seperti yang diimplementasikan pada arsitektur Monodepth2. Pendekatan ini menghindari kebutuhan label eksternal dengan meminimalkan kesalahan rekonstruksi antar frame berturut-turut. Ketiga, integrasi *foundation model* seperti DINOv2 sebagai backbone ekstraksi fitur, yang kemudian dihubungkan dengan kepala prediktif khusus (*depth head*). Model ini memanfaatkan representasi semantik dan struktural yang telah dipelajari melalui pembelajaran mandiri, sehingga mampu menghasilkan estimasi kedalaman yang lebih robust terhadap variasi iluminasi dan tekstur.

Pada tingkat penelitian doktoral, arah pengembangan saat ini bergerak menuju beberapa isu kritis yang belum sepenuhnya terpecahkan. Konsistensi skala pada estimasi kedalaman video menjadi fokus penting agar transisi antar frame tetap koheren secara geometris dan temporal. Estimasi kedalaman dan pose kamera juga semakin sering diformulasikan sebagai masalah bersama (*joint estimation*) untuk saling memperkuat akurasi masing-masing komponen melalui umpan balik silang. Selain itu, kesenjangan domain antara data sintetis yang melimpah dan distribusi data dunia nyata masih memerlukan teknik adaptasi domain atau *domain generalization* yang robust, terutama ketika diterapkan pada skenario real-time atau perangkat edge.

Pemahaman mendalam mengenai estimasi kedalaman monokular ini menjadi fondasi penting sebelum kita mengeksplorasi representasi spasial tiga dimensi secara eksplisit. Hasil prediksi kedalaman tersebut, ketika dikombinasikan dengan parameter kamera dan transformasi geometri, akan membentuk kumpulan titik ruang tiga dimensi yang dikenal sebagai *point cloud*. Pada slide berikutnya, kita akan membahas karakteristik representasi *point cloud*, sumber datanya, serta ekosistem alat pemrosesan berbasis Python yang relevan untuk penelitian lanjutan.

---

## Slide 020 - Point Cloud: Representasi dan Visualisasi

### Narasi

Setelah membahas tantangan dan pendekatan pada estimasi kedalaman monokular di slide sebelumnya, kita kini beralih ke representasi geometri tiga dimensi yang lebih eksplisit, yaitu *point cloud*. Berbeda dengan peta kedalaman (*depth map*) yang bersifat kontinu dalam domain 2D, *point cloud* merepresentasikan scene sebagai kumpulan diskrit titik-titik spasial. Setiap titik umumnya menyimpan koordinat Cartesian `(x, y, z)` dan dapat diperkaya dengan fitur tambahan seperti intensitas atau nilai warna `(r, g, b)` untuk mendukung tugas segmentasi, klasifikasi, atau rekonstruksi permukaan.

Kualitas dan karakteristik *point cloud* sangat bergantung pada sumber akuisisi datanya:
- **LiDAR**: Menghasilkan data yang sangat akurat secara metrik, namun bersifat *sparse* dan memerlukan perangkat keras mahal. Umumnya digunakan dalam pemetaan topografi dan sistem navigasi otonom.
- **Depth Camera**: Memberikan data yang lebih padat (*dense*) secara real-time, namun rentan terhadap noise pada tepi objek, permukaan gelap, atau material reflektif.
- **SfM / MVS**: Memanfaatkan banyak citra 2D untuk merekonstruksi geometri 3D. Hasilnya sangat padat jika cakupan sudut pandang memadai, namun mengalami degradasi signifikan pada area dengan tekstur rendah atau pencahayaan tidak merata.
- **Estimasi Depth Monokular**: Menawarkan skalabilitas tinggi tanpa sensor khusus, namun hanya menghasilkan depth relatif tanpa skala absolut, sehingga memerlukan tahap penskalaan atau kalibrasi tambahan sebelum dikonversi menjadi koordinat 3D yang konsisten.

Dalam ekosistem komputasi Python, manipulasi dan analisis *point cloud* ditopang oleh beberapa pustaka yang saling melengkapi. `open3d` menjadi pilihan utama untuk operasi geometri 3D lanjutan, termasuk registrasi titik via algoritma ICP (*Iterative Closest Point*), downsampling, clustering, dan rendering interaktif. Untuk kebutuhan plotting cepat atau integrasi dengan pipeline analisis numerik, `matplotlib` dan `pyvista` menyediakan antarmuka yang ringan, sementara `numpy` tetap menjadi fondasi esensial untuk manipulasi array koordinat secara vektorisasi. Pada slide berikutnya, kita akan langsung mengimplementasikan konstruksi *point cloud* sintetis menggunakan kode Python, serta membahas praktik terbaik dalam penyimpanan format `.ply`/`.xyz` dan interaksi rotasi menggunakan `open3d` selama sesi praktikum.

---

## Slide 021 - Visualisasi Point Cloud dengan Python

### Narasi

```python
import numpy as np
import matplotlib.pyplot as plt

### Generate synthetic point cloud (hemisphere)

phi = np.random.uniform(0, 2*np.pi, 5000)
theta = np.random.uniform(0, np.pi/2, 5000)
r = 1.0
x = r * np.sin(theta) * np.cos(phi)
y = r * np.sin(theta) * np.sin(phi)
z = r * np.cos(theta)
points = np.stack([x, y, z], axis=1)

### color by height

colors = plt.cm.viridis(z / z.max())

fig = plt.figure(figsize=(6,6))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(points[:,0], points[:,1], points[:,2],
           c=colors, s=1)
ax.set_title('Synthetic Point Cloud')
plt.show()
```

Kode pada slide ini mengilustrasikan implementasi dasar pembuatan dan render awan titik sintetis menggunakan ekosistem Python. Tahap pertama memanfaatkan NumPy untuk membangkitkan distribusi seragam pada sudut azimuthal (`phi`) dan polar (`theta`). Koordinat bola tersebut kemudian ditransformasi ke sistem kartesian melalui persamaan standar, menghasilkan vektor posisi titik-titik yang membentuk permukaan hemisfer. Array hasil konversi ditumpuk menjadi matriks berukuran `(5000, 3)` agar siap diproses oleh fungsi plotting.

Pada bagian visualisasi, penentuan warna tiap titik didasarkan pada elevasi sumbu z. Normalisasi dilakukan dengan membagi seluruh nilai z terhadap maksimumnya, lalu dipetakan ke colormap `viridis` dari Matplotlib. Rendering tiga dimensi diinisialisasi dengan `projection='3d'`, dan fungsi `scatter` digunakan untuk memetakan koordinat beserta warna yang telah dihitung. Parameter `s=1` diberikan untuk menjaga kepadatan visual tanpa menyebabkan overplotting yang mengganggu interpretasi geometri.

Meskipun Matplotlib efektif untuk demonstrasi algoritma, lingkungan riset computer vision tingkat lanjut umumnya mengadopsi Open3D sebagai standar de facto. Library ini menyediakan primitif interaktif untuk rotasi kamera, pengukuran jarak antar titik, serta operasi filtrasi dan downsampling yang krusial dalam preprocessing data geometri. Selain itu, hasil simulasi atau rekonstruksi point cloud sebaiknya diekspor ke format industri seperti `.ply` atau `.xyz` agar kompatibel dengan pipeline downstream seperti registrasi, meshing, atau analisis kurvatur.

Penguasaan manipulasi koordinat dan teknik visualisasi titik ini menjadi fondasi analitis sebelum memasuki tahap rekonstruksi otomatis dari domain citra. Pada slide berikutnya, kita akan membahas bagaimana kumpulan titik serupa dihasilkan secara sistematis dari input gambar melalui pipeline tradisional seperti COLMAP, yang mengintegrasikan ekstraksi fitur, estimasi pose kamera, hingga dense reconstruction berbasis multi-view stereo.

---

## Slide 022 - Multi-View Reconstruction Tradisional

### Narasi

Setelah kita mengeksplorasi representasi dan teknik visualisasi point cloud pada slide sebelumnya, langkah logis berikutnya dalam alur kerja 3D vision adalah memahami bagaimana geometri tiga dimensi tersebut dibangun kembali dari sekumpulan citra dua dimensi. Slide ini memperkenalkan pendekatan tradisional untuk multi-view reconstruction, yang secara praktis sering diimplementasikan melalui pipeline seperti COLMAP. Alur kerjanya terdiri dari empat tahap utama: ekstraksi fitur lokal dan matching antar gambar, diikuti oleh Structure from Motion (SfM) untuk mengestimasi pose kamera dan menghasilkan titik 3D yang sparse. Tahap terakhir adalah Multi-View Stereo (MVS), yang memanfaatkan konsistensi intensitas pixel dari berbagai view untuk melakukan dense reconstruction, sehingga menghasilkan point cloud padat atau mesh permukaan yang siap digunakan.

Pendekatan berbasis SfM dan MVS ini menawarkan beberapa keunggulan fundamental yang menjadikannya fondasi kuat dalam computer vision klasik. Pertama, ekosistem tool seperti COLMAP bersifat open-source, matang, dan sangat banyak digunakan baik di akademisi maupun industri. Kedua, pipeline ini secara eksplisit menangani kalibrasi intrinsik dan ektrinsik kamera, serta menerapkan bundle adjustment untuk meminimalkan reprojection error secara global, sehingga akurasi geometri dapat dioptimalkan secara matematis. Ketiga, ketika jumlah view cukup banyak dan distribusinya merata, kualitas rekonstruksi yang dihasilkan sangat tinggi dengan detail permukaan yang terjaga.

Namun, sebagai peneliti tingkat doktoral, kita harus mengidentifikasi batasan kritis dari metode ini. Sistem tradisional sangat bergantung pada ketersediaan view dengan overlap yang memadai dan area yang kaya tekstur. Region dengan permukaan reflektif, transparan, atau low-texture sering kali gagal dideteksi oleh matcher konvensional, menyebabkan hole rekonstruksi yang signifikan. Selain itu, kompleksitas komputasi SfM dan MVS tumbuh secara drastis seiring bertambahnya jumlah gambar dan resolusi, membuat pipeline ini lambat dan kurang scalable untuk scene skala besar tanpa strategi chunking atau optimasi paralelisasi.

Keterbatasan ini secara alami mengarah pada pertanyaan empiris tentang hubungan antara konfigurasi pengambilan gambar dan kualitas output. Seperti yang akan kita bedah pada slide berikutnya, jumlah view bukanlah satu-satunya penentu keberhasilan; distribusi sudut dan baseline antar kamera justru lebih krusial untuk stabilitas triangulasi. Oleh karena itu, dalam perancangan eksperimen riset, disarankan untuk melakukan ablation study terhadap jumlah dan pose view, mendokumentasikan distribusi rotasi-translasi kamera secara eksplisit, serta memahami trade-off antara coverage spasial dan konsistensi geometri sebelum mengevaluasi metode neural rendering atau learning-based reconstruction yang lebih mutakhir.

---

## Slide 023 - Kualitas Rekonstruksi vs Jumlah View

### Narasi

Merujuk pada pembahasan sebelumnya mengenai alur tradisional seperti COLMAP, kita kini beralih ke aspek krusial yang sering menjadi bottleneck dalam pipeline multi-view geometry: hubungan antara jumlah view dengan kualitas rekonstruksi tiga dimensi. Pada level penelitian doktoral, memahami dinamika ini bukan sekadar hal teknis, melainkan fondasi metodologis sebelum memutuskan apakah pendekatan tradisional masih relevan atau perlu digantikan oleh representasi modern.

Secara empiris, jumlah view memberikan pengaruh langsung terhadap kepadatan geometri dan stabilitas hasil akhir. Dengan dua hingga tiga view, rekonstruksi cenderung sangat sparse, dipenuhi hole, dan sangat rentan terhadap noise serta kesalahan feature matching. Ketika jumlah view mencapai lima hingga lima belas, representasi mulai memadat dan cukup memadai untuk skenario object-centric, selama distribusi sudutnya terdistribusi merata. Untuk scene statis tanpa oklusi berat, dua puluh view atau lebih umumnya menghasilkan struktur yang stabil dan detail tinggi.

Namun, kuantitas view bukanlah jaminan tunggal. Distribusi sudut pandang justru lebih deterministik daripada sekadar menghitung frame input. View dengan baseline yang terlalu sempit menghasilkan parallax minimal sehingga hampir tidak menambah informasi kedalaman baru. Di sisi lain, view dengan sudut ekstrem atau pencahayaan yang sangat kontras dapat memicu kegagalan triangulasi dan destabilisasi estimasi pose kamera. Fenomena ini menjelaskan mengapa metode tradisional sering gagal pada area reflektif, transparan, atau berstruktur rendah, sebagaimana disinggung pada slide sebelumnya.

Untuk implementasi dalam riset, Anda harus mendesain eksperimen dengan ablation study yang ketat terhadap variabel jumlah view. Pastikan untuk mendokumentasikan metrik berikut secara sistematis:
- Kepadatan dan sebaran pose kamera di ruang 3D.
- Rasio baseline terhadap jarak objek utama.
- Coverage sudut dan redundansi informasi antar-frame.
Catatan distribusi ini akan menjadi landasan objektif ketika Anda mengevaluasi ketahanan model terhadap view sparsity, sekaligus membuka jalan menuju diskusi tentang representasi neural yang mampu memodelkan scene secara kontinu meskipun data input terbatas atau tidak seragam. Pembahasan mengenai bagaimana neural field mengatasi keterbatasan diskrit ini akan kita lanjutkan pada slide berikutnya.

---

## Slide 024 - Representasi Neural untuk Scene 3D

### Narasi

Pada slide sebelumnya, kita telah membahas batasan empiris dari pendekatan multi-view geometry klasik, khususnya bagaimana kualitas rekonstruksi sangat bergantung pada jumlah dan distribusi sudut pandang kamera. Penambahan jumlah view memang meningkatkan kepadatan titik, namun metode tradisional tetap menghadapi masalah fundamental berupa diskritisasi permukaan, sensitivitas terhadap noise, dan ketidakmampuan menangkap efek pencahayaan yang bergantung pada arah pandang. Keterbatasan inilah yang menjadi motivasi utama peralihan ke representasi berbasis neural.

Representasi konvensional seperti point cloud maupun mesh bersifat diskret dan sangat bergantung pada kualitas tahap surface reconstruction awal. Sebaliknya, neural field memodelkan seluruh scene sebagai fungsi kontinu yang dipelajari secara end-to-end. Fungsi ini dituliskan sebagai:
```
F : (x, y, z, view direction) -> (RGB, density)
```
Dalam formulasi ini, setiap koordinat ruang tiga dimensi `(x, y, z)` bersama vektor arah pandang kamera tidak lagi dipetakan ke grid atau poligon tetap, melainkan diumpankan ke jaringan saraf yang langsung menghasilkan nilai warna `(RGB)` dan kerapatan optik (`density`). Pendekatan ini memungkinkan interpolasi yang mulus antar titik observasi, mengakomodasi medium semi-transparan, dan menghilangkan kebutuhan akan segmentasi atau ekstraksi permukaan eksplisit yang rentan terhadap error kumulatif.

Dalam perkembangan terkini, terdapat empat varian utama representasi neural yang perlu Anda pahami sebagai landasan pemilihan metodologi riset:
- **NeRF**: Menggunakan konsep radiance field yang digabungkan dengan volume rendering diferensial untuk mensimulasikan propagasi cahaya melalui medium volumetrik.
- **SDF-based (NeuS, VolSDF)**: Memanfaatkan Signed Distance Function untuk mendefinisikan permukaan implicit, kemudian mengintegrasikannya dengan pipeline rendering guna meningkatkan stabilitas topologi dan detail tepi.
- **3D Gaussian Splatting**: Menggantikan MLP dengan sekumpulan kernel Gaussian tiga dimensi yang dioptimalkan secara langsung untuk rasterisasi cepat, mengatasi bottleneck inference pada NeRF konvensional.
- **Tri-plane / Feature Grid**: Representasi hibrida yang mengompresi fitur laten ke dalam struktur grid berdimensi rendah, memberikan akselerasi training dan inferensi tanpa mengorbankan fidelitas visual secara signifikan.

Pemilihan representasi ini akan secara langsung mempengaruhi desain eksperimen, loss function, dan metrik evaluasi yang Anda bangun dalam penelitian tingkat doktoral. Untuk memahami mekanisme inti dari paradigma ini, kita akan lanjutkan pembahasan pada slide berikutnya dengan menelaah konsep dasar NeRF, mulai dari definisi radiance field, turunan persamaan volume rendering, hingga peran strategis positional encoding dalam memperkuat kapasitas frekuensi tinggi dari MLP.

---

## Slide 025 - NeRF: Konsep Dasar

### Narasi

Pada slide sebelumnya, kita telah membahas pergeseran paradigma dari representasi geometri diskret menuju fungsi kontinu yang dimodelkan oleh jaringan neural. NeRF mengimplementasikan konsep ini melalui radiance field, di mana setiap titik dalam ruang tiga dimensi tidak lagi sekadar menyimpan warna statis, melainkan membawa dua atribut simultan: warna `c = (r, g, b)` dan kepadatan material `σ`. Keunggulan fundamental NeRF terletak pada dependensi warna terhadap arah pandang kamera `d`. Mekanisme view-dependent ini memungkinkan model merekonstruksi fenomena optik realistis seperti highlight specular, glossy reflection, dan perubahan intensitas cahaya yang bervariasi sesuai sudut observasi.

Untuk menerjemahkan representasi volumetrik kontinu tersebut menjadi citra raster dua dimensi, NeRF memanfaatkan prinsip volume rendering. Warna akhir pada setiap piksel diperoleh melalui integrasi fisik sepanjang sinar kamera yang menembus scene:
```
C(r) = ∫ T(t) σ(r(t)) c(r(t), d) dt
```
Persamaan ini mengaproksimasi transport radiasi cahaya secara diferensial. `σ(r(t))` menentukan koefisien ekstinksi (penyerapan dan hamburan) pada posisi tertentu, sedangkan `c(r(t), d)` memberikan warna lokal yang dipengaruhi oleh orientasi pengamatan. Faktor `T(t)` merepresentasikan transmittance, yaitu probabilitas kumulatif bahwa sinar masih dapat merambat bebas tanpa terblokir oleh material padat di depannya. Dalam praktiknya, integral ini dievaluasi secara numerik melalui stratified sampling titik-titik diskrit sepanjang sinar, menjembatani domain kontinu neural field dengan output piksel yang teramati.

Proses pemetaan koordinat spasial dan sudut pandang ke atribut radiance field dijalankan oleh Multilayer Perceptron (MLP). Input jaringan terdiri dari koordinat 3D `(x, y, z)` serta parameter sudut `(θ, φ)` yang mendefinisikan arah pandang. Karena MLP konvensional cenderung kesulitan mempelajari sinyal berfrekuensi tinggi yang dominan pada tekstur halus atau tepi geometris tajam, NeRF menerapkan positional encoding sebelum data memasuki lapisan pertama. Teknik ini memproyeksikan input berdimensi rendah ke basis fungsi trigonometri bertingkat, sehingga meningkatkan kapasitas representasional jaringan dalam menangkap variasi gradien cepat tanpa memerlukan arsitektur yang terlalu dalam.

Konsep fundamental ini membentuk landasan teoritis yang kokoh sebelum kita menelusuri implementasi teknisnya. Pada slide berikutnya, kita akan menguraikan arsitektur spesifik jaringan, alur forward pass selama training, strategi minimasi loss MSE, serta persyaratan eksperimental kritis seperti ketersediaan pose kamera akurat, jumlah citra multi-view, dan asumsi scene statis dengan pencahayaan konsisten. Pembahasan ini akan melengkapi pemahaman kita tentang bagaimana prinsip radiance field dan volume rendering dioperasionalkan menjadi pipeline pembelajaran mesin yang fungsional.

---

## Slide 026 - Arsitektur dan Training NeRF

### Narasi

Setelah pada slide sebelumnya kita menguraikan konsep radiance field dan formulasi integral volume rendering, slide ini membawa kita ke implementasi komputasionalnya. Di sini, kita membahas bagaimana representasi kontinu tersebut diwujudkan melalui arsitektur jaringan dan prosedur pelatihan yang menjadi standar dalam literatur NeRF awal.

Arsitektur dasar yang diusulkan mengikuti alur pemetaan bertahap sebagai berikut:
- Input berupa koordinat spasial `(x, y, z)` dikombinasikan dengan vektor arah pandang kamera.
- Kedua komponen tersebut diproses melalui *positional encoding*, teknik transformasi sinusoidal yang wajib ada agar MLP mampu menangkap frekuensi tinggi dari detail geometri dan tekstur halus.
- Hasil encoding dilewatkan ke MLP inti berukuran 8 lapisan dengan 256 unit per lapisan. Lapisan ini memuat dua jalur keluaran: nilai opasitas `sigma` dan vektor fitur laten.
- Vektor fitur laten kemudian digabungkan kembali dengan informasi arah pandang pada lapisan tambahan, yang akhirnya menghasilkan prediksi warna RGB.

Desain modular ini memisahkan estimasi kerapatan geometri dari pemodelan efek pencahayaan bergantung sudut pandang (*view-dependent shading*), sehingga model dapat mensimulasikan fenomena optik seperti kilau permukaan (*specular highlights*) secara implisit.

Proses pelatihan berjalan dengan paradigma *pixel-wise supervised learning*. Alurnya dapat dirangkum sebagai berikut:
- Untuk setiap piksel pada gambar referensi, algoritma melakukan *sampling* titik-titik diskrit sepanjang sinar kamera (*camera ray*).
- Warna sintetik dihitung menggunakan persamaan volume rendering yang telah kita definisikan sebelumnya, yaitu integrasi tertimbang antara transmittance, opasitas, dan warna pada setiap titik sampel.
- Fungsi kerugian yang dioptimalkan umumnya adalah *Mean Squared Error* (MSE) antara warna hasil render dan warna ground truth dari dataset.
- Gradien balik (*backpropagation*) digunakan untuk memperbarui bobot seluruh lapisan MLP hingga konvergensi tercapai.

Keberhasilan optimisasi ini sangat bergantung pada kepatuhan terhadap persyaratan data dan kondisi aquisisi yang ketat:
- Ketersediaan minimal 20 hingga lebih dari 100 gambar dari berbagai sudut pandang untuk menutupi ruang 3D secara memadai.
- Pose kamera yang presisi, biasanya diekstrak melalui pipeline Structure-from-Motion seperti COLMAP. Kesalahan pose akan menyebabkan artefak geometri yang signifikan.
- Asumsi scene statis dan pencahayaan konsisten selama pengambilan data. Variasi iluminasi atau dinamika objek akan melanggar asumsi radiance field stasioner yang mendasari model.

Dari perspektif penelitian tingkat doktoral, pemahaman terhadap arsitektur dan batasan data ini menjadi landasan kritis untuk mengidentifikasi *research gap*. Ketatnya kebutuhan pose akurat, scene statis, dan komputasi intensif melahirkan motivasi kuat bagi pengembangan varian NeRF yang lebih efisien dan adaptif. Sebagaimana akan kita telaah pada slide berikutnya, trade-off antara kualitas visual fotorealistik dan keterbatasan komputasional—seperti waktu rendering yang lambat, durasi training yang panjang, serta sifat *per-scene* yang menghambat generalisasi—menjadi titik tolak utama dalam merumuskan novelty metodologis. Evaluasi empiris terhadap faktor-faktor ini akan menentukan seberapa jauh suatu pendekatan neural rendering dapat dideploy pada skenario dunia nyata yang kompleks.

---

## Slide 027 - Kelebihan dan Keterbatasan NeRF

### Narasi

Setelah membahas arsitektur MLP berbasis positional encoding dan mekanisme training volume rendering pada slide sebelumnya, kita kini melakukan evaluasi kritis terhadap performa praktis Neural Radiance Fields. Pemahaman mendalam mengenai kekuatan dan kelemahan fundamental NeRF menjadi prasyarat penting sebelum mengeksplorasi varian-modifikasi terkini.

Dari perspektif kualitas visual, NeRF mampu menghasilkan novel view synthesis yang sangat fotorealistik. Keunggulan ini bersumber dari representasi fungsionalnya yang bersifat kontinu, sehingga resolusi output tidak lagi terkunci oleh batas grid diskrit seperti pada metode volumetrik konvensional. Fleksibilitas representasi ini juga memudahkan integrasi dengan berbagai kondisi eksternal, termasuk editing geometri lokal, manipulasi material, maupun relighting berbasis arah cahaya baru.

Namun, keunggulan tersebut dibayar dengan sejumlah keterbatasan struktural yang signifikan. Pertama, NeRF menuntut jumlah view input yang besar, umumnya puluhan hingga ratusan citra, sehingga performanya menurun drastis pada skenario sparse-view. Kedua, proses rendering masih komputasional mahal karena setiap piksel memerlukan banyak sampel titik sepanjang sinar untuk mengintegrasikan densitas dan warna. Ketiga, asumsi scene statis dan pencahayaan konsisten membatasi adaptabilitas model pada dynamic scene atau perubahan iluminasi dinamis. Keempat, waktu training dapat mencapai jam hingga berhari-hari per scene akibat optimasi unik yang dilakukan untuk setiap lokalisasi. Terakhir, NeRF bersifat per-scene, bukan model general yang mampu langsung digeneralisasi ke lingkungan baru tanpa proses fine-tuning ulang.

Evaluasi kritis ini menunjukkan bahwa NeRF bukanlah solusi akhir, melainkan sebuah kerangka kerja representasi 3D yang masih terbuka untuk inovasi. Batasan-batasan di atas justru menjadi pemicu utama lahirnya berbagai pendekatan mutakhir. Pada slide berikutnya, kita akan menelaah bagaimana komunitas riset mengklasifikasikan perkembangan NeRF ke dalam kategori akselerasi, generalisasi, penanganan dinamika, ekspansi ke 3D generation, hingga alternatif representasi seperti 3D Gaussian Splatting.

---

## Slide 028 - Varian dan Perkembangan NeRF

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi bahwa NeRF menawarkan novel view synthesis yang fotorealistik serta representasi scene yang kontinu, namun dibatasi oleh kebutuhan banyak view, rendering yang lambat, dan ketidakmampuan menangani dynamic scene atau generalisasi lintas scene. Slide ini melanjutkan diskusi dengan menunjukkan bagaimana komunitas riset merespons keterbatasan tersebut melalui diversifikasi varian NeRF yang dikategorikan berdasarkan tujuan optimasinya.

Untuk mengatasi bottleneck komputasi, muncul pendekatan akselerasi seperti Instant-NGP yang memanfaatkan multi-resolution hash encoding untuk mempercepat lookup, serta Plenoxels yang mengganti MLP non-parametrik dengan representasi eksplisit berbasis voxel. Jika fokusnya adalah generalisasi tanpa fine-tuning per scene, metode seperti PixelNeRF dan IBRNet mengadopsi encoder berbasis CNN atau Transformer untuk memetakan sekumpulan citra input langsung ke representasi implicit field. 

Kasus dynamic scene ditangani melalui dekomposisi latent space. D-NeRF memisahkan komponen geometri statis dari deformasi temporal, sedangkan HyperNeRF menggunakan hypernetwork untuk memprediksi bobot MLP secara adaptif sepanjang waktu. Ketika data capture bersifat sparse view, teknik regularisasi kuat dan constraint geometris seperti pada DietNeRF, RegNeRF, dan FreeNeRF diterapkan untuk menekan overfitting dan mempertahankan konsistensi struktur 3D.

Di luar representasi volumetrik tradisional, NeRF berevolusi menjadi fondasi untuk generative 3D. DreamFusion menerapkan Score Distillation Sampling, di mana model diffusion text-to-image berfungsi sebagai surrogate loss untuk mengoptimalkan representasi 3D hanya dari prompt teks. Untuk manipulasi pencahayaan dan editing fisik, Ref-NeRF dan NeRF in the Wild menambahkan estimasi BRDF serta environment lighting agar scene dapat direlight secara fisika-correct. Menariknya, kategori representasi alternatif kini mulai digeser oleh 3D Gaussian Splatting, yang akan kita bedah lebih mendalam pada slide berikutnya.

Implikasi strategis dari evolusi ini bagi penelitian tingkat doktoral adalah pergeseran perspektif: NeRF bukan lagi algoritma tunggal, melainkan sebuah framework scene representation yang modular. Kontribusi ilmiah Anda tidak harus selalu memperbaiki arsitektur MLP dasar. Ruang inovasi terbuka pada tiga dimensi utama: (1) data pipeline, termasuk strategi sampling view, augmentasi multi-modal, atau handling occlusion; (2) representasi, seperti hybrid implicit-explicit structures atau differentiable rasterizers; dan (3) efisiensi serta robustness, mencakup kompresi latent, anti-aliasing, atau adaptasi ke kondisi capture real-world yang noisy. Pemahaman ini memungkinkan Anda merumuskan research question yang tajam, mendesain experimental setup yang terukur, dan melakukan positioning yang jelas terhadap state-of-the-art sebelum melangkah ke implementasi eksperimental.

---

## Slide 029 - 3D Gaussian Splatting

### Narasi

Setelah pembahasan mengenai berbagai varian NeRF yang telah memperluas kerangka kerja representasi scene, kita kini beralih ke salah satu terobosan paling signifikan dalam bidang neural rendering: 3D Gaussian Splatting. Berbeda dengan pendekatan implicit neural field yang memodelkan warna dan densitas melalui jaringan saraf, metode ini merepresentasikan scene secara eksplisit sebagai kumpulan primitif Gaussian tiga dimensi. Setiap primitif Gaussian dikarakterisasi oleh parameter posisi, skala, rotasi, opacity, serta koefisien spherical harmonics untuk menangkap variasi warna berdasarkan sudut pandang.

Proses rendering pada 3D Gaussian Splatting menggeser paradigma dari volume rendering berbasis ray marching menuju rasterization langsung. Primitif Gaussian di-"splat" atau diproyeksikan ke bidang gambar menggunakan transformasi affine, kemudian di-blend secara transparansi berurutan berdasarkan kedalaman. Pendekatan ini menghilangkan kebutuhan evaluasi MLP pada setiap titik sepanjang sinar, sehingga komputasi menjadi jauh lebih ringan dan kompatibel dengan pipeline grafis modern.

Metode ini menawarkan sejumlah keunggulan yang menjadikannya alternatif kuat dibandingkan NeRF klasik:
- Rendering jauh lebih cepat, mampu mencapai performa real-time bahkan pada resolusi tinggi.
- Training relatif efisien, umumnya selesai dalam rentang menit hingga puluhan menit.
- Kualitas visual tetap tinggi, menghasilkan rekonstruksi fotorealistik yang kompetitif.

Di sisi lain, implementasi skala besar masih menghadapi beberapa tantangan teknis yang perlu diantisipasi dalam desain eksperimen:
- Konsumsi memori meningkat signifikan pada scene berukuran besar atau detail halus.
- Stabilitas rendering belum optimal untuk kasus pencahayaan kompleks dan bayangan dinamis.
- Area riset terbuka meliputi kompresi representasi, anti-aliasing, pengembangan dynamic 3DGS, serta peningkatan robustness pada kondisi sparse view.

Karakteristik unik dari 3D Gaussian Splatting ini akan menjadi bahan analisis kritis ketika kita mengevaluasi pilihan representasi neural secara lebih sistematis. Pada slide berikutnya, kita akan membandingkan secara mendalam antara NeRF, 3D Gaussian Splatting, dan pendekatan berbasis SDF, serta membahas bagaimana memilih representasi yang paling sesuai dengan tujuan penelitian dan batasan komputasi yang Anda hadapi.

---

## Slide 030 - Perbandingan Representasi Neural

### Narasi

Pada slide ini, kita melakukan evaluasi komparatif sistematis terhadap tiga paradigma representasi neural yang mendominasi literatur terkini: Neural Radiance Fields (NeRF), 3D Gaussian Splatting (3DGS), serta metode berbasis Signed Distance Function (SDF). Tabel di atas merangkum perbedaan fundamental dari sisi efisiensi komputasi, kualitas geometri, kebutuhan infrastruktur, serta kematangan ekosistem perangkat lunak. Pemahaman komparatif ini krusial untuk menentukan arah eksperimental dan positioning riset Anda pada tingkat doktoral.

Dari perspektif kecepatan training, NeRF masih memerlukan waktu pelatihan yang cukup lama karena sifatnya yang mengoptimalkan jaringan MLP secara end-to-end sepanjang sinar. 3DGS menawarkan proses training yang lebih efisien berkat pendekatan primitif Gaussian yang dapat dioptimalkan secara langsung melalui rasterization. Metode SDF berada di posisi menengah, dengan kompleksitas komputasi yang sebanding namun lebih terstruktur dalam mengekspresikan permukaan implisit.

Untuk kecepatan rendering, 3DGS unggul secara signifikan karena mekanisme splatting yang memungkinkan rendering real-time bahkan pada hardware konsumen standar. NeRF tetap mengandalkan ray marching yang secara inheren lambat, sementara SDF membutuhkan evaluasi grid atau marching cubes yang menghasilkan kecepatan sedang. Terkait kualitas permukaan, SDF memberikan representasi eksplisit yang halus dan sangat cocok untuk simulasi fisika atau animasi karakter. 3DGS bersifat point-based sehingga kurang ideal untuk ekstraksi mesh tanpa post-processing tambahan, sedangkan NeRF tidak menghasilkan permukaan eksplisit sama sekali.

Aspek hardware requirement dan kematangan tooling juga perlu dipertimbangkan dalam desain eksperimen. Ketiganya menuntut GPU berkapasitas tinggi, namun ekosistem NeRF sudah sangat matang dengan berbagai library open-source yang stabil. 3DGS berkembang sangat pesat dalam beberapa tahun terakhir, meskipun optimasi memori dan anti-aliasing masih menjadi area aktif penelitian. SDF memiliki tooling yang solid terutama di ranah geometry processing, namun integrasinya dengan pipeline deep learning modern masih terus disempurnakan.

Pemilihan representasi harus selalu didasari oleh tujuan riset spesifik. Jika fokus Anda adalah rekonstruksi geometri presisi tinggi untuk simulasi atau animasi, pendekatan SDF-based adalah pilihan utama. Apabila target aplikasi Anda menekankan pada rendering interaktif atau real-time visualization, 3DGS memberikan trade-off terbaik antara kecepatan dan kualitas visual. Sementara itu, NeRF tetap relevan sebagai baseline klasik untuk validasi metrik fotorealistik atau ketika Anda membutuhkan referensi komparatif yang telah banyak dikutip dalam literatur.

Transisi ke slide berikutnya akan membahas tantangan ilmiah mendasar yang masih menghambat adopsi penuh ketiga representasi ini di dunia nyata, mulai dari masalah occlusion, sparse view, hingga generalisasi cross-scene. Analisis komparatif hari ini akan menjadi fondasi metodologis saat kita mengidentifikasi research gap dan merumuskan kontribusi teknis yang inovatif pada tingkat penelitian doktoral.

---

## Slide 031 - Tantangan Utama 3D Vision untuk Riset

### Narasi

Setelah kita mengurai kelebihan dan keterbatasan representasi neural seperti NeRF, 3D Gaussian Splatting, dan SDF-based pada slide sebelumnya, kini kita beralih ke aspek yang paling menentukan arah penelitian doktoral: tantangan ilmiah mendasar dalam 3D Vision. Memahami batasan teknis ini bukan sekadar teori, melainkan fondasi untuk merumuskan *research question* dan *novelty* yang relevan.

Mari kita tinjau enam tantangan utama yang terstruktur dalam tabel ini. Pertama, *occlusion*. Dalam konfigurasi multi-view, sebagian besar permukaan objek atau latar belakang akan tertutup oleh elemen lain di beberapa sudut kamera. Penanganan occlusion memerlukan pendekatan resampling geometri yang robust, fusi multimodal untuk melengkapi informasi yang hilang, serta integrasi prior semantik guna menebak struktur yang tersembunyi.

Kedua, *sparse view reconstruction*. Merekonstruksi scene 3D yang koheren hanya dari dua hingga lima citra merupakan masalah *ill-posed* yang masih aktif diteliti. Solusi modern mengandalkan regularisasi topologi yang ketat, strategi pretraining pada dataset skala besar, dan pemanfaatan prior generatif untuk mengisi wilayah scene yang tidak terobservasi.

Ketiga, *scale ambiguity*. Masalah ini sangat menonjol ketika bekerja dengan estimasi kedalaman monocular atau sistem tanpa kalibrasi intrinsik/ekstrinsik yang presisi. Riset terkini mengarah pada metode *calibration-free*, estimasi *metric depth* yang konsisten antar frame, serta fusi data multi-sensor untuk mengembalikan satuan fisik yang akurat.

Keempat, *dynamic scenes*. Lingkungan nyata jarang bersifat statis. Adanya pergerakan objek, deformasi non-rigid, dan variasi pencahayaan dinamis sering merusak asumsi rigidity pada pipeline 3D konvensional. Arah pengembangan saat ini berfokus pada pemodelan temporal eksplisit, registrasi non-rigid, serta representasi yang mampu memisahkan komponen latar tetap dari entitas bergerak.

Kelima, ketersediaan *dataset 3D* dengan ground truth yang andal. Pengumpulan data 3D dunia nyata secara manual sangat terbatas dan mahal. Komunitas riset kini banyak memanfaatkan sintesis data melalui engine simulasi fisika, rendering prosedural, serta teknik *weak supervision* yang mengeksploitasi label parsial atau noisy sebagai sinyal pembelajaran.

Terakhir, isu *generalisasi*. Sebagian besar arsitektur 3D vision saat ini masih bergantung pada optimasi per-scene (*per-scene optimization*), yang membuatnya tidak efisien untuk aplikasi skala besar. Transisi menuju *cross-scene learning* dan pembangunan *foundation model 3D* menjadi kunci agar model dapat beradaptasi cepat ke domain baru tanpa proses training ulang yang intensif.

Tantangan-tantangan ini sebenarnya bukan jalan buntu, melainkan celah strategis untuk integrasi teknologi mutakhir. Sebagaimana akan kita bahas pada slide berikutnya, banyak solusi untuk masalah-masalah di atas justru lahir dari sinergi antara constraint geometri 3D dengan kekuatan prior dari diffusion models, serta kemampuan ekstraksi fitur kontekstual dari foundation models seperti DINOv2, CLIP, dan SAM. Kombinasi inilah yang sedang mendefinisikan batas terdepan dalam riset 3D Vision saat ini.

---

## Slide 032 - Menghubungkan dengan Diffusion Models dan Foundation Models

### Narasi

Setelah menguraikan tantangan ilmiah inti pada slide sebelumnya—mulai dari occlusion, sparse view, scale ambiguity, hingga kesulitan memperoleh dataset 3D ground truth—kita perlu melihat bagaimana komunitas riset kontemporer merespons keterbatasan tersebut. Jawaban yang paling dominan saat ini bukanlah membangun pipeline 3D dari nol, melainkan mengintegrasikan representasi 3D dengan model-model foundation dan generatif yang telah matang di ranah 2D. Slide ini menyoroti persimpangan strategis tersebut sebagai fondasi metodologis untuk riset tingkat doktor.

Berikut adalah peta peran lima teknologi kunci yang kini mendominasi arsitektur 3D vision modern:
- **Diffusion models**: Berfungsi sebagai prior generatif yang sangat kuat untuk mengisi celah pada sparse view dan menghasilkan representasi 3D langsung dari input 2D atau teks, seperti yang diimplementasikan dalam framework DreamFusion dan Score Distillation Sampling.
- **DINOv2**: Menyediakan fitur semantik yang robust dan invariant terhadap perubahan iluminasi, sehingga banyak dipakai untuk dense matching, penentuan correspondences antar view, hingga estimasi depth semi-supervised.
- **CLIP**: Membuka jalur text-guided 3D generation dan editing, memungkinkan manipulasi scene atau material berdasarkan prompt bahasa alami tanpa memerlukan annotation pixel-level.
- **SAM**: Memberikan kemampuan segmentasi objek tingkat tinggi yang presisi, sangat krusial untuk masking akurat dan dekomposisi scene sebelum tahap rekonstruksi atau rendering.
- **ViT**: Bertindak sebagai backbone esensial dalam berbagai pipeline, mulai dari estimasi depth monocular, multi-view stereo, hingga pelatihan Neural Radiance Fields karena kemampuannya menangkap konteks global secara efisien.

Implikasi dari integrasi ini cukup mendasar bagi orientasi penelitian jenjang S3. Riset 3D vision tidak lagi berdiri sendiri sebagai disiplin yang terisolasi; justru kekuatan dan novelty-nya sering kali terletak pada sinergi cerdas antara constraint geometris klasik dengan representasi neural yang dipandu oleh prior semantik atau generatif. Kombinasi geometric constraint dan neural representation ini memang menjadi arah yang sangat menjanjikan, namun menuntut evaluasi metodologis yang ketat. Pada slide berikutnya, kita akan membedah kerangka kajian paper yang tepat untuk menilai apakah sebuah proposal riset benar-benar menawarkan kontribusi baru atau sekadar komposisi ulang, serta daftar pertanyaan kritis yang harus diajukan saat mengevaluasi validitas klaim eksperimen di bidang ini.

---

## Slide 033 - Kajian Paper: Apa yang Harus Diperhatikan?

### Narasi

Setelah membahas bagaimana teknik seperti Diffusion Models, DINOv2, dan CLIP diintegrasikan ke dalam pipeline 3D vision pada slide sebelumnya, kini kita beralih ke aspek metodologis yang krusial: cara mengevaluasi literatur secara kritis. Pada jenjang doktoral, kemampuan membedah paper tidak lagi sekadar memahami alur kerja, melainkan menilai validitas klaim, reproducible, dan posisi kontribusi terhadap state-of-the-art.

Struktur kajian paper untuk topik 3D vision dapat diuraikan melalui enam elemen fundamental:
1. **Problem**: Identifikasi jelas tujuan penelitian, apakah fokus pada rekonstruksi 3D, novel view synthesis, estimasi kedalaman, atau task turunan lainnya.
2. **Data**: Tinjau dataset yang digunakan, seperti DTU, NeRF Synthetic, ScanNet, KITTI, atau RealEstate10K. Konsistensi dan relevansi data sangat menentukan generalisasi hasil.
3. **Metode**: Bedah representasi yang dipakai, mekanisme optimasi, serta dependency terhadap pose kamera. Apakah pipeline bersifat supervised, weakly-supervised, atau sepenuhnya unsupervised?
4. **Baseline**: Pastikan perbandingan dilakukan dengan metode terkini yang relevan. Evaluasi keadilan komparasi dari sisi komputasi, preprocessing, dan konfigurasi hyperparameter.
5. **Metrik**: Perhatikan jenis metrik yang dilaporkan, mulai dari fidelity piksel, akurasi geometri, hingga similarity perseptual.
6. **Keterbatasan**: Telaah batasan yang diakui penulis, lalu cek apakah klaim utama masih kuat meskipun menghadapi skenario tersebut.

Selain struktur formal, pertanyaan kritis menjadi penentu kualitas analisis Anda:
- Apakah peningkatan nilai metrik benar-benar tercermin dalam perbaikan visual, atau hanya noise numerik?
- Apakah skenario eksperimen merepresentasikan distribusi data dunia nyata, atau terbatas pada kondisi laboratorium yang terlalu ideal?
- Apakah metode menuntut sumber daya komputasi (GPU, memori, waktu training) yang tidak realistis untuk replikasi atau adopsi praktis?

Kerangka evaluasi ini akan menjadi landasan langsung ketika kita masuk ke detail teknis pengukuran performa pada slide berikutnya. Kita akan membedah secara spesifik interpretasi metrik seperti Abs Rel, RMSE, δ1–δ3, Chamfer Distance, F-score, hingga PSNR, SSIM, dan LPIPS dalam konteks depth estimation, rekonstruksi point cloud/mesh, dan novel view synthesis berbasis neural representation.

---

## Slide 034 - Metrik Evaluasi untuk 3D Vision

### Narasi

Setelah membahas kerangka kajian paper pada slide sebelumnya, kita kini turun ke level teknis yang menentukan validitas klaim penelitian: pemilihan metrik evaluasi. Di bidang 3D Vision, metrik bukan sekadar angka pelengkap, melainkan fondasi kuantitatif untuk membandingkan metode, mengidentifikasi research gap, dan memposisikan kontribusi ilmiah terhadap state-of-the-art. Pemilihan metrik harus selaras dengan tujuan tugas, apakah fokus pada akurasi geometri, kesetiaan fotorealistik, atau keseimbangan keduanya.

Untuk **Depth Estimation**, evaluasi berfokus pada regresi piksel demi piksel:
- **Abs Rel (Absolute Relative Error)**: Menormalkan selisih antara prediksi dan ground truth berdasarkan skala kedalaman, sehingga robust terhadap variasi jarak dalam scene.
- **RMSE (Root Mean Square Error)**: Memberikan bobot lebih besar pada deviasi ekstrem, berguna ketika outlier secara signifikan mengganggu aplikasi downstream.
- **δ1, δ2, δ3**: Mengukur persentase piksel di mana rasio error berada di bawah threshold 1.25, 1.25², dan 1.25³. Metrik berbasis threshold ini sangat relevan untuk skenario dunia nyata di mana ketepatan struktural lebih krusial daripada presisi milimeter.

Pada **Rekonstruksi 3D**, ruang evaluasi bergeser dari domain gambar ke ruang geometri tiga dimensi:
- **Chamfer Distance**: Menghitung rata-rata jarak tetangga terdekat antar dua point cloud, memberikan ukuran simetris atas kesetiaan bentuk.
- **F-score**: Menggabungkan precision dan recall pada threshold jarak tertentu, efektif menyeimbangkan false positive dan false negative dalam pemulihan permukaan.
- **Accuracy**: Mengukur jarak langsung dari setiap titik prediksi ke titik ground truth terdekat, memberikan indikator sensitif terhadap alignment geometris, meski rentan terhadap noise lokal.

Untuk **Novel View Synthesis** dan framework neural rendering seperti NeRF, fokus utama adalah kualitas perseptual dan fotorealistik:
- **PSNR**: Standar industri untuk kesetiaan piksel, namun memiliki korelasi lemah dengan penilaian manusia.
- **SSIM**: Memperbaiki kelemahan PSNR dengan mempertimbangkan luminance, kontras, dan informasi struktural, sehingga lebih sejalan dengan kualitas visual.
- **LPIPS**: Menggunakan fitur deep network pretrained untuk mengukur kemiripan perseptual. LPIPS konsisten menjadi metrik terkuat dalam menyelaraskan peringkat metode dengan preferensi subjektif manusia, menjadikannya wajib dilaporkan dalam publikasi tingkat S3.

Pemahaman mendalam terhadap ketiga kelompok metrik ini menjadi prasyarat sebelum merancang eksperimen. Pada slide berikutnya, kita akan membahas bagaimana mengoperasionalkan metrik-metrik tersebut ke dalam desain eksperimen yang ketat, mulai dari pemilihan dataset, strategi split training-testing, variasi jumlah view, hingga pemilihan baseline yang adil. Kita juga akan mengidentifikasi risiko metodologis seperti overfitting scene-specific, ketergantungan pada kamera pose yang tidak akurat, dan bias inherent pada dataset sintetis, agar proposal penelitian Anda memiliki fondasi eksperimental yang solid dan reproducible.

---

## Slide 035 - Desain Eksperimen 3D: Data dan Baseline

### Narasi

Setelah membahas metrik evaluasi pada slide sebelumnya, kita kini beralih ke aspek fundamental yang menentukan validitas dan reproduktibilitas hasil penelitian: desain eksperimen tiga dimensi. Pada jenjang doktoral, setiap keputusan teknis harus tertaut langsung pada formulasi masalah penelitian dan positioned terhadap state-of-the-art yang ada.

Mari kita bedah elemen-elemen kunci dalam tabel desain eksperimen ini. Pertama, pemilihan dataset harus dipertimbangkan berdasarkan karakteristik scene. Apakah fokus penelitian pada scene statis atau dinamis? Indoor atau outdoor? Objek tunggal atau lingkungan kompleks? Karakteristik ini akan sangat memengaruhi generalisasi model dan relevansi kontribusi ilmiahnya. Kedua, strategi splitting data wajib menghindari kontaminasi informasi. Scene untuk training dan testing tidak boleh overlap, karena tumpang tindih scene akan menghasilkan metrik yang terlalu optimistis dan tidak merepresentasikan performa di kondisi unseen.

Ketiga, analisis sensitivitas terhadap jumlah view merupakan langkah metodologis yang krusial. Dengan memvariasikan jumlah input kamera seperti 2, 4, 8, 16, hingga 32 view, Anda dapat mengidentifikasi titik jenuk (knee point) di mana penambahan view tidak lagi memberikan peningkatan signifikan pada rekonstruksi atau estimasi kedalaman. Keempat, pemilihan baseline harus komprehensif dan fair. Bandingkan pendekatan klasik seperti pipeline SfM/MVS berbasis COLMAP dengan metode neural modern seperti NeRF, 3D Gaussian Splatting, atau bahkan pendekatan monocular. Kombinasikan metrik geometris dan perseptual untuk menangkap trade-off antara akurasi struktural dan kualitas visual secara seimbang.

Selain itu, sebagai peneliti tingkat lanjut, Anda harus secara eksplisit mendokumentasikan risiko metodologis yang mungkin muncul. Overfitting pada scene tertentu sering terjadi ketika dataset terbatas atau memiliki distribusi tekstur yang homogen. Ketergantungan berlebihan pada pose kamera yang tidak akurat dapat merusak konsistensi geometri, terutama jika proses feature matching atau SfM awal gagal. Terakhir, bias pada dataset sintetis yang terlalu bersih perlu diwaspadai; model yang dilatih hanya pada data render sempurna cenderung mengalami domain gap yang parah saat diujicobakan pada citra dunia nyata.

Rancangan eksperimen yang ketat ini akan menjadi fondasi langsung untuk praktikum pada slide berikutnya. Di sana, Anda akan menerapkan alur kerja mulai dari pengambilan image sequence, estimasi pose menggunakan COLMAP, dense reconstruction, hingga visualisasi point cloud dengan Open3D, serta membandingkannya dengan estimasi kedalaman monocular menggunakan model pretrained seperti MiDaS atau DPT. Persiapan desain yang matang hari ini akan memastikan eksekusi praktikum berjalan terarah, menghasilkan temuan yang robust, dan siap dikaji secara kritis untuk pengembangan proposal disertasi Anda.

---

## Slide 036 - Praktikum: Eksplorasi Depth dan Multi-View Reconstruction

### Narasi

Slide ini menggeser fokus dari perencanaan metodologis ke eksekusi praktis melalui serangkaian eksperimen yang dirancang untuk menguji prinsip geometri multi-view dan estimasi kedalaman. Sesuai dengan kerangka desain eksperimen pada slide sebelumnya, praktikum ini memiliki tiga tujuan utama: memberikan pengalaman langsung dalam pipeline depth estimation atau rekonstruksi 3D, memvisualisasikan output berbentuk point cloud, serta menganalisis sensitivitas kualitas rekonstruksi terhadap variasi jumlah input view. Pendekatan ini memungkinkan mahasiswa mengidentifikasi trade-off antara akurasi geometris, densitas representasi, dan beban komputasi secara empiris.

Alur kerja yang terstruktur dalam tabel ini mencerminkan pipeline standar dalam computer vision modern, yang dapat dijabarkan sebagai berikut:
1. Siapkan image sequence dari dataset publik atau rekaman mandiri, pastikan pencahayaan dan tekstur objek cukup bervariasi untuk mendukung feature matching.
2. Estimasi pose kamera menggunakan SfM (Structure-from-Motion) via COLMAP, yang mengandalkan deteksi dan pencocokan fitur lokal untuk merekonstruksi posisi dan orientasi kamera secara relatif.
3. Lakukan dense reconstruction melalui pipeline MVS (Multi-View Stereo) pada COLMAP atau gunakan metode penyederhanaan sebagai baseline komparatif.
4. Visualisasikan hasil rekonstruksi sebagai point cloud menggunakan Open3D atau Matplotlib untuk verifikasi struktural awal.
5. Bandingkan dengan estimasi kedalaman monocular menggunakan model pretrained terkini seperti MiDaS, DPT, atau Depth Anything, guna mengevaluasi perbedaan pendekatan berbasis geometri murni versus pendekatan data-driven.

Variasi jumlah view yang diujikan—mulai dari subset kecil hingga konfigurasi padat—langsung menjawab pertanyaan eksperimental mengenai batas minimum view yang diperlukan agar rekonstruksi tetap stabil. Hal ini juga terkait erat dengan risiko yang telah diidentifikasi pada slide sebelumnya, seperti ketergantungan pada akurasi pose, potensi overfitting pada scene tertentu, dan bias yang mungkin muncul jika dataset sintetis terlalu ideal. Dengan merancang praktikum secara bertahap, mahasiswa dapat mengamati secara langsung bagaimana penurunan jumlah view mempengaruhi noise pada permukaan, kelengkapan geometris, serta konsistensi depth map.

Untuk menerjemahkan alur kerja abstrak ini menjadi implementasi nyata, slide berikutnya akan menyajikan pseudocode berbasis command-line yang mengurai setiap modul COLMAP secara berurutan. Perintah tersebut mencakup feature extraction, exhaustive matching, sparse reconstruction, hingga pipeline dense reconstruction yang berakhir pada fusi stereo menghasilkan file `.ply`. Eksekusi skrip ini akan menjadi fondasi bagi evaluasi metrik kuantitatif dan kualitatif, sekaligus mempersiapkan mahasiswa untuk melakukan perbandingan kritis dengan metode neural rendering atau pendekatan monocular mutakhir dalam konteks penelitian tingkat doktoral.

---

## Slide 037 - Pseudocode Praktikum dengan COLMAP

### Narasi

Pada slide sebelumnya, kita telah merangkum alur kerja umum untuk praktikum eksplorasi depth dan multi-view reconstruction, mulai dari persiapan image sequence hingga estimasi pose kamera menggunakan SfM dan MVS. Lanjutan kali ini akan mengupas implementasi teknis pipeline tersebut melalui command-line COLMAP, yang tetap menjadi baseline kuat untuk rekonstruksi geometri multi-view dalam literatur computer vision terkini.

Proses dimulai dengan `colmap feature_extractor`, yang bertugas memindai seluruh gambar di direktori `images/`, mengekstrak deskriptor lokal, dan menyimpan metadata serta fitur ke dalam `database.db`. Langkah selanjutnya, `colmap exhaustive_matcher`, melakukan pencocokan fitur secara komprehensif antar pasangan gambar. Untuk dataset skala besar, matcher ini dapat digantikan dengan `sequential_matcher` atau `vocab_tree_matcher` guna mengurangi beban komputasi tanpa mengorbankan konsistensi geometri jangka panjang.

Tahap ketiga dijalankan melalui `colmap mapper`, yang melaksanakan Structure-from-Motion. Di sini, algoritma bundle adjustment bekerja untuk memperbarui posisi kamera dan koordinat titik 3D awal secara simultan sehingga meminimalkan reprojection error. Output disimpan di folder `sparse/`, yang menjadi fondasi struktural sebelum beralih ke tahap dense reconstruction.

Pipeline kemudian masuk ke fase dense reconstruction menggunakan tiga perintah berurutan. `image_undistorter` membersihkan distorsi lensa berdasarkan parameter intrinsik yang telah dioptimalkan oleh mapper. `patch_match_stereo` kemudian menghitung peta kedalaman per gambar dengan pendekatan probabilistik berbasis patch matching. Terakhir, `stereo_fusion` menggabungkan seluruh peta kedalaman tersebut menjadi file `fused.ply`, yang merepresentasikan point cloud padat dan konsisten secara geometris.

Sebagai bagian dari desain eksperimen tingkat doktoral, perhatikan instruksi untuk menggunakan subset gambar sebanyak 2, 4, dan 8 view. Variasi ini dirancang khusus untuk menganalisis trade-off antara akurasi rekonstruksi, stabilitas geometri, dan efisiensi memori. Mahasiswa diharapkan tidak hanya melakukan visualisasi `fused.ply` menggunakan Open3D, tetapi juga mengukur metrik kuantitatif seperti Chamfer Distance atau F-Score terhadap ground truth. Hasil rekonstruksi geometris berbasis multi-view ini nantinya akan dibandingkan secara kritis dengan pendekatan monocular yang akan kita eksekusi pada slide berikutnya, di mana estimasi kedalaman dihasilkan murni dari representasi learned model pretrained seperti MiDaS atau DPT tanpa ketergantungan pada informasi geometri multi-view.

---

## Slide 038 - Pseudocode Depth Estimation Monocular

### Narasi

Setelah menyelesaikan pipeline rekonstruksi tiga dimensi berbasis geometri multi-pandangan menggunakan COLMAP pada slide sebelumnya, kita kini beralih ke pendekatan estimasi kedalaman dari satu citra tunggal atau *monocular depth estimation*. Pendekatan ini menjadi alternatif strategis ketika akuisisi data multi-view tidak feasible, biaya sensor depth/LiDAR terlalu tinggi, atau ketika skenario aplikasi menuntut operasi real-time pada perangkat edge dengan kamera tunggal.

Kode Python yang disajikan memanfaatkan model pratraining DPT Large dari MiDaS melalui TorchHub. Baris `model = torch.hub.load(...)` dan `model.eval()` menginisialisasi arsitektur encoder-decoder berbasis transformer yang telah dilatih secara self-supervised pada jutaan pasangan gambar-kedalaman. Mode evaluasi wajib diaktifkan untuk mematikan dropout dan normalisasi batch, memastikan keluaran deterministik selama inferensi. Citra dimuat dengan PIL, dikonversi ke RGB, dan siap masuk ke tahap pra-pemrosesan. Meskipun pseudocode menuliskan komentar `## preprocessing: resize, normalize`, dalam implementasi doktor-level Anda harus menerapkan transformasi torchvision yang presisi: penskalaan resolusi sesuai expectasi model, normalisasi channel, serta konversi ke tensor float32 dengan shape `(1, C, H, W)` dan penempatan device yang konsisten (CPU/GPU).

Proses inferensi dieksekusi melalui `depth = model(input_tensor)`, yang mengembalikan peta kedalaman relatif dalam bentuk tensor. Karena model monocular umumnya tidak merekonstruksi skala absolut tanpa informasi kamera atau prior tambahan, hasil tensor perlu di-scale dan shifted jika dibandingkan dengan ground truth metric. Visualisasi dilakukan dengan Matplotlib menggunakan palet `inferno` dan `plt.colorbar()` untuk memetakan nilai intensitas pixel ke jarak relatif, memudahkan inspeksi kualitas geometri secara intuitif.

Dari sudut pandang penelitian tingkat doktoral, evaluasi kuantitatif menjadi fondasi validasi metodologi. Metrik seperti *Absolute Relative Error* (Abs Rel) dan *Root Mean Square Error* (RMSE) memberikan gambaran agregat performa model terhadap data referensi LiDAR atau stereo. Namun, analisis tingkat lanjut harus menyoroti pola kegagalan sistematis: *depth bleeding* pada tepi objek tajam akibat smoothing loss, estimasi tidak stabil pada region ber tekstur rendah atau repetitif, serta distorsi akibat refleksi specular dan bayangan keras. Identifikasi bias ini bukan sekadar temuan eksperimental, melainkan pintu masuk untuk novelty riset, seperti desain loss function aware-tepi, integrasi prior semantic, atau mekanisme kalibrasi intrinsik adaptif.

Hasil observasi dan analisis error dari eksperimen monocular ini secara alami mengarah pada perumusan masalah penelitian yang terstruktur. Pada slide berikutnya, kita akan membahas template *Concept Note* khusus untuk topik 3D vision, mencakup definisi dataset, asumsi geometri, pemilihan baseline, kontribusi yang ditargetkan, serta mitigasi risiko teknis. Transisi dari implementasi kode menuju formulasi riset doctoral ini memastikan bahwa setiap pertanyaan penelitian memiliki dasar empiris yang kuat dan posisi yang jelas terhadap state-of-the-art.

---

## Slide 039 - Perumusan Masalah Penelitian 3D: Template Concept Note

### Narasi

Setelah mengeksplorasi implementasi praktis estimasi kedalaman monokular pada slide sebelumnya, langkah kritis selanjutnya dalam riset tingkat doktoral adalah merumuskan masalah penelitian secara terstruktur. Slide ini menyajikan kerangka kerja Concept Note yang dirancang khusus untuk memetakan ide penelitian di bidang 3D Vision agar memiliki fondasi metodologis yang kuat, jelas, dan siap diuji secara empiris.

Struktur Concept Note yang ditampilkan terdiri dari enam komponen esensial. Judul masalah harus menangkap celah pengetahuan atau keterbatasan metode eksisting secara ringkas namun presisi. Bagian data memerlukan spesifikasi objektif mengenai dataset, jumlah view kamera, karakteristik scene, serta resolusi input, karena konsistensi dan kualitas data menjadi penentu validitas eksperimen. Asumsi geometri seperti status kalibrasi kamera, panjang baseline, dan sifat statis atau dinamis scene akan menentukan pemilihan pipeline rekonstruksi dan kompleksitas komputasi. Baseline method perlu dipilih berdasarkan justifikasi teoretis dan kinerja state-of-the-art terkini. Kontribusi yang dibayangkan harus mengarah pada inovasi substantif, baik berupa arsitektur baru, representasi geometrik alternatif, maupun koleksi data yang belum tereksplorasi. Terakhir, identifikasi risiko teknis seperti konvergensi training yang tidak stabil, bottleneck komputasi GPU, atau kesulitan akuisisi data ground truth memungkinkan perencanaan mitigasi sejak fase awal perancangan penelitian.

Contoh pertanyaan riset yang disajikan mencerminkan arah perkembangan mutakhir dalam literatur computer vision. Pertanyaan pertama menyoroti peluang pemanfaatan prior berbasis diffusion model untuk mengatasi keterbatasan sparse view reconstruction. Pertanyaan kedua menguji ketangguhan 3D Gaussian Splatting dalam menangani dinamika scene yang umum terjadi pada aplikasi real-time. Sementara itu, integrasi semantic cues sebagai regularizer untuk mengurangi ambiguitas depth monocular menawarkan jalur penelitian yang menggabungkan pemahaman kontekstual dengan pemulihan geometri. Ketiga pertanyaan ini menekankan pentingnya menyeimbangkan novelty, feasibility, dan dampak ilmiah yang terukur sesuai standar publikasi internasional bereputasi.

Pemahaman terhadap template ini akan menjadi landasan ketika kita menyusun rangkuman komprehensif pada slide berikutnya. Setelah merangkum prinsip-prinsip dasar 3D Vision, multi-view geometry, neural rendering, dan tantangan utama seperti occlusion, sparse view, serta scale ambiguity, diskusi akan dialihkan ke aspek evaluasi dan interpretabilitas model. Pertemuan berikutnya akan membahas bagaimana memastikan bahwa rekonstruksi 3D dan prediksi depth tidak hanya akurat secara numerik, tetapi juga robust, dapat dipercaya, dan mudah dijelaskan dalam konteks explainable serta trustworthy computer vision.

---

## Slide 040 - Rangkuman dan Kaitan ke Pertemuan Berikutnya

### Narasi

Pada slide penutup pertemuan kesepuluh ini, kita merangkum seluruh pembahasan mengenai 3D Vision, Multi-View Geometry, dan Neural Rendering yang telah diuraikan sebelumnya. Inti dari bidang ini adalah upaya memulihkan struktur tiga dimensi dari representasi dua dimensi melalui pemodelan kamera, geometri epipolar, stereo matching, hingga rekonstruksi multi-view. Pemahaman terhadap prinsip-prinsip dasar ini menjadi fondasi matematis dan komputasional yang wajib dikuasai sebelum memasuki metode modern berbasis pembelajaran mendalam.

Perkembangan terkini sangat didominasi oleh pendekatan neural representation seperti NeRF dan 3D Gaussian Splatting. Kedua metode ini menawarkan fleksibilitas tinggi dalam merepresentasikan adegan kompleks secara fotorealistik, sekaligus mengatasi keterbatasan representasi tradisional seperti voxel grid berdensitas tinggi atau mesh eksplisit yang sulit dioptimalkan. Namun, implementasinya tetap menghadapi tantangan fundamental berupa oklusi parsial, kondisi sparse view, ambigu skala, dinamika adegan, serta ketersediaan dataset 3D berkualitas tinggi yang masih terbatas di domain tertentu.

Dari sisi praktikum, pengalaman langsung dalam melakukan estimasi kedalaman monokuler dan rekonstruksi point cloud memberikan validasi empiris terhadap teori yang telah dipelajari. Mahasiswa diharapkan dapat mengamati bagaimana asumsi kalibrasi kamera, baseline antar-view, dan kualitas tekstur input secara langsung mempengaruhi akurasi, kelengkapan, dan noise pada hasil rekonstruksi tiga dimensi.

Merujuk pada concept note yang telah disusun pada slide sebelumnya, hasil rekonstruksi 3D ini tidak boleh berhenti pada aspek visual semata. Model 3D yang dihasilkan harus dievaluasi lebih lanjut dari segi keandalan numerik, konsistensi geometris, dan interpretabilitas keputusan model. Hal ini menjadi jembatan alami menuju pertemuan berikutnya yang akan membahas Explainable, Robust, dan Trustworthy Computer Vision.

Sebagai bahan refleksi untuk riset lanjutan, pertimbangkan pertanyaan-pertanyaan berikut: Bagaimana mekanisme internal model depth menjelaskan ketidakpastian prediksi pada region yang mengalami oklusi atau textureless? Bagaimana strategi regularisasi, arsitektur, atau fusion multimodal dapat dirancang agar rekonstruksi tetap robust terhadap shift distribusi data di lingkungan nyata? Gunakan pertanyaan ini sebagai landasan perumusan research question dan positioning metodologi Anda pada proposal disertasi.

---

## Slide 041 - TERIMA KASIH

### Narasi

Kita telah menyelesaikan pembahasan mendalam mengenai 3D Vision, Multi-View Geometry, dan Neural Rendering pada pertemuan ini. Dari pemulihan struktur tiga dimensi melalui epipolar geometry hingga representasi neural modern seperti NeRF dan 3D Gaussian Splatting, kita telah menelusuri bagaimana pergeseran paradigma dari metode geometris klasik ke pendekatan berbasis pembelajaran mesin terus memperluas batas fotorealisme dan efisiensi rendering. Tantangan inti seperti occlusion, sparsity view, ambigu skala, serta handling dynamic scene tetap menjadi frontier penelitian yang menuntut desain arsitektur, fungsi loss, dan sampling strategy yang lebih inovatif.

Eksperimen praktis estimasi depth dan rekonstruksi point cloud yang telah dijalankan memberikan verifikasi empiris terhadap teori yang dibahas. Hasil pengujian menunjukkan bahwa robustnya rekonstruksi sangat bergantung pada kalibrasi intrinsik-ekstrinsik, densitas viewpoint, serta kemampuan model dalam menggeneralisasi struktur geometri di luar domain training. Pada jenjang doktoral, peluang riset terbuka meliputi integrasi structural priors dengan representasi implicit, percepatan inference via differentiable rasterization, atau perluasan framework ke domain video dan multi-modal.

Sebagai penutup sesi ini, kita beralih ke fase krusial berikutnya: evaluasi dan validasi model 3D yang dihasilkan. Pertemuan selanjutnya akan membahas Explainable, Robust, dan Trustworthy Computer Vision. Fokusnya adalah pada mekanisme interpretabilitas decision-making model depth, jaminan stabilitas rekonstruksi terhadap covariate shift dan adversarial perturbation, serta kerangka auditabilitas untuk sistem vision yang dapat dipercaya dalam aplikasi kritis. Silakan kumpulkan pertanyaan awal terkait metrik kepercayaan dan teknik attribution mapping untuk diskusi lanjutan. Terima kasih atas perhatian dan kontribusi aktif selama perkuliahan berlangsung.
