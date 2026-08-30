# Narasi TD Pengolahan Citra Digital - Pertemuan 09

## Generative Vision dengan Diffusion Models

Sumber: markdown/pert09-generative-vision-dengan-diffusion-models.md

---

## Slide 000 - Cover

### Narasi

Slide ini berfungsi sebagai pembuka formal untuk fokus utama pertemuan ini, yaitu Generative Vision dengan Diffusion Models. Pada jenjang doktoral, pembahasan tidak berhenti pada penggunaan API atau library generatif, melainkan menuntut pemahaman komprehensif terhadap formulasi probabilistik di balik proses difusi, mulai dari Markov chain forward noising hingga arsitektur U-Net atau transformer-based reverse denoiser yang beroperasi di latent space.

Kajian akan diarahkan pada dekonstruksi mekanisme conditioning yang memungkinkan kontrol presisi atas output generatif, seperti classifier-free guidance, timestep conditioning, serta integrasi cross-attention untuk multimodal prompting. Mahasiswa diharapkan mampu menganalisis trade-off antara fidelity, diversity, dan computational efficiency, serta mengkritisi fenomena mode collapse, posterior collapse, dan bias distribusi yang sering muncul pada model skala besar.

Sebagai langkah awal sebelum masuk ke teknis implementasi dan review paper, slide berikutnya akan memetakan posisi topik ini dalam alur perkembangan kompetensi selama satu semester. Pemetaan ini diperlukan untuk menunjukkan kesinambungan konseptual antara fondasi yang telah dipelajari, aplikasi synthetic data, serta eksplorasi geometri 3D yang akan dibahas pada sesi selanjutnya. Mari lanjut ke slide berikutnya untuk melihat kerangka keterkaitan kurikulum secara terstruktur.

---

## Slide 001 - Posisi Pertemuan 09 dalam Rangkaian Perkuliahan

### Narasi

Pada slide ini, kita memetakan Pertemuan 09 dalam alur perkembangan materi mata kuliah secara menyeluruh. Sebelumnya, pada Pertemuan 08, kita telah mengeksplorasi segmentasi citra dan *promptable foundation models* seperti SAM, dengan penekanan pada kualitas mask, strategi prompt, serta kemampuan model dalam menangkap representasi visual semantik. Pertemuan kali ini mengalihkan fokus ke *generative vision* menggunakan *diffusion models*. Kita akan mendalami mekanisme generasi berbasis *noise schedule*, peran *conditioning* dalam berbagai skenario seperti *text-to-image*, *image-to-image*, dan *inpainting*, serta eksplorasi penggunaan *synthetic data* beserta risiko teknis dan etisnya. Setelah pembahasan ini, Pertemuan 10 akan berlanjut ke ranah 3D Vision, Multi-View Geometry, dan Neural Rendering, yang membahas representasi geometri tiga dimensi dan rekonstruksi scene.

Keterkaitan antar pertemuan dalam Rencana Pembelajaran Semester (RPS) dirancang untuk menunjukkan kesinambungan metodologis dan potensi aplikasi riset:
- **Pertemuan 06 (Image Restoration):** Pendekatan berbasis diffusion menjadi jembatan konseptual menuju generative vision, karena teknik restorasi dan generasi berbagi prinsip *denoising* yang fundamental.
- **Pertemuan 07 (Object Detection):** *Synthetic data* yang dihasilkan oleh diffusion model dapat dimanfaatkan sebagai augmentasi dataset untuk meningkatkan robustness dan generalisasi detektor objek.
- **Pertemuan 08 (Segmentasi dan Foundation Model):** Model segmentasi dan generatif berbagi fondasi representasi visual yang kuat, khususnya dalam pemahaman struktur, tekstur, dan konteks semantik.
- **Pertemuan 10 (3D Vision dan Neural Rendering):** Integrasi diffusion model dengan representasi 3D membuka peluang besar untuk *novel view synthesis* dan rekonstruksi 3D probabilistik yang lebih realistis.

Penempatan materi ini tidak hanya bersifat kronologis, melainkan strategis untuk membangun pemahaman bertahap dari analisis citra pasif menuju sintesis citra aktif. Dengan memahami posisi pertemuan ini, mahasiswa diharapkan mampu melihat diffusion model sebagai komponen kunci yang menghubungkan berbagai paradigma pengolahan citra modern. Hal ini akan menjadi landasan kritis sebelum kita menelaah tujuan pembelajaran dan capaian kompetensi yang harus dicapai pada slide berikutnya.

---

## Slide 002 - Tujuan Pembelajaran dan Capaian Terkait

### Narasi

Slide ini menetapkan peta jalan pembelajaran yang secara langsung melanjutkan konteks dari slide sebelumnya. Setelah pertemuan delapan membahas segmentasi berbasis prompt dan foundation model seperti SAM, fokus kita kini bergeser ke ranah generatif. Pergeseran ini bukan sekadar perubahan arsitektur, melainkan transformasi paradigma dari pemetaan diskriminatif menuju pemodelan distribusi data visual secara utuh, yang menjadi prasyarat penting sebelum memasuki topik 3D vision dan neural rendering pada pertemuan sepuluh.

Empat tujuan pembelajaran utama akan menjadi panduan kritis selama sesi berlangsung:
- Memahami prinsip generasi berbasis noise schedule, forward diffusion, dan reverse denoising sebagai fondasi matematis dan probabilistik dari proses generasi.
- Menjelaskan peran conditioning dalam berbagai skenario aplikasi seperti text-to-image, image-to-image, dan inpainting, serta bagaimana representasi semantik diintegrasikan ke dalam ruang laten.
- Membandingkan GAN dengan diffusion model secara komparatif dari sisi stabilitas latihan, cakupan mode (mode coverage), dan konsistensi kualitas sampel.
- Mengevaluasi potensi dan risiko synthetic data secara mendalam, mencakup isu bias sistemik, memorisasi dataset, fenomena hallucination, serta tantangan provenance dan traceability.

Capaian pembelajaran mata kuliah yang selaras dengan materi ini diturunkan menjadi tiga kompetensi inti yang harus dicapai oleh mahasiswa tingkat doktoral:
- CPMK-1: Menganalisis paradigma dan tantangan riset generative vision secara kritis, termasuk identifikasi research gap dan batasan metodologis pada paper terkini.
- CPMK-3: Merancang eksperimen komputasional yang rigor untuk evaluasi kualitas dan keragaman generasi citra, menggunakan metrik standar maupun custom metrics sesuai kebutuhan penelitian.
- CPMK-4: Memanfaatkan pipeline Diffusers secara praktis untuk membangun prototipe eksperimen awal, menguji hyperparameter, dan melakukan ablation study pada komponen conditioning.

Rangkaian tujuan dan capaian ini akan menjadi kerangka kerja sebelum kita menelusuri agenda teknis pertemuan ini. Sesuai dengan alur kuliah yang akan dipaparkan pada slide berikutnya, pembahasan akan dimulai dari transisi konseptual dari model diskriminatif ke generatif, dilanjutkan dengan landasan teori likelihood dan latent variable, eksplorasi arsitektur U-Net, mekanisme classifier-free guidance, hingga implementasi praktis dan evaluasi etika-saintifik. Seluruh alur tersebut dirancang untuk membekali Anda menyusun desain eksperimen yang siap dikembangkan menjadi proposal penelitian atau publikasi internasional di bidang generative vision.

---

## Slide 003 - Agenda Pertemuan

### Narasi

Slide ini menyajikan agenda pertemuan yang disusun secara terstruktur untuk memandu pemahaman dari fondasi teoretis hingga implementasi kritis generative vision. Alur kuliah dirancang agar mahasiswa dapat melacak evolusi metodologi, memahami mekanisme inti diffusion models, dan mengevaluasi potensi risetnya secara mendalam sesuai standar jenjang doktoral.

Kita akan memulai dengan pergeseran paradigma dari pendekatan discriminative menuju generative vision. Materi kemudian mengupas konsep dasar generative model, meliputi estimasi likelihood, pemodelan latent variable, dan upaya memetakan distribusi data visual yang kompleks. Sebagai konteks historis, arsitektur GAN akan dibahas singkat untuk menyoroti kelebihan dan keterbatasannya dibandingkan pendekatan modern.

Inti pembahasan akan difokuskan pada mekanisme diffusi, mulai dari forward noising hingga reverse denoising. Untuk mengatasi beban komputasi, kita akan mendalami latent diffusion dan peran sentral arsitektur U-Net dalam proses denoising bertahap. Aspek conditioning akan dijelaskan lebih lanjut, mencakup integrasi teks, citra referensi, dan mask sebagai pengarah generasi. Teknik classifier-free guidance juga akan diuraikan sebagai standar de facto untuk meningkatkan stabilitas dan kontrol output.

Dari perspektif aplikasi, agenda mencakup text-to-image, image-to-image, dan inpainting, dilanjutkan dengan eksplorasi synthetic data serta augmentation berbasis diffusion. Pada tingkat riset lanjutan, analisis kritis terhadap risiko seperti bias, memorization, hallucination, dan provenance menjadi wajib. Evaluasi kualitas dan keragaman hasil generasi akan menjadi acuan metodologis untuk desain eksperimen yang valid.

Bagian praktikum memberikan pengalaman hands-on menggunakan pipeline Diffusers dengan variasi prompt, yang secara langsung mendukung CPMK-4 pada slide sebelumnya tentang pemanfaatan tools generatif. Eksperimen awal ini akan menjadi basis empiris untuk identifikasi research gap dan validasi hipotesis.

Akhirnya, seluruh materi akan disintesis dalam sesi critical review dan diskusi penelitian. Agenda ini secara eksplisit menjawab pertanyaan kunci pada slide berikutnya, termasuk dampak synthetic data terhadap generalisasi dan bias, validitas metrik evaluasi, dokumentasi provenance, perbandingan mode coverage antara GAN dan diffusion, serta fenomena memorization. Dengan demikian, setiap poin agenda tidak hanya bersifat prosedural, tetapi diposisikan sebagai landasan perancangan proposal disertasi dan publikasi internasional.

---

## Slide 004 - Pertanyaan Kunci Pertemuan Ini

### Narasi

Pada agenda pertemuan sebelumnya, kita telah menyusun peta jalan perkuliahan mulai dari transisi paradigma diskriminatif ke generatif, konsep likelihood dan latent variable, hingga mekanisme forward noising dan reverse denoising pada diffusion model. Slide ini berfungsi sebagai kompas analitis yang mengarahkan seluruh diskusi dan eksperimen praktikum pada pertemuan ini. Lima pertanyaan kunci yang disajikan bukan sekadar daftar tinjauan literatur, melainkan fondasi metodologis yang harus Anda uji secara kritis saat merancang penelitian doktoral di bidang generative vision.

Berikut adalah rincian pertanyaan kunci beserta relevansinya terhadap desain penelitian:
- Apakah citra sintetis meningkatkan generalisasi model atau justru memperkuat bias? Pertanyaan ini menuntut Anda membandingkan performa classifier yang dilatih dengan data asli versus data augmentasi berbasis diffusion, serta menganalisis propagasi bias melalui confusion matrix dan subgroup evaluation.
- Bagaimana kualitas dan keragaman hasil generasi diukur secara valid? Evaluasi tidak boleh bergantung pada satu metrik tunggal. Anda perlu menggabungkan FID/KID untuk distribusi, CLIP-Score untuk alignment semantik, serta analisis varians latent space untuk mengukur keragaman dan stabilitas sampling.
- Bagaimana provenance data sintetis didokumentasikan? Reproducibility riset doktoral mensyaratkan pencatatan lengkap: versi backbone, scheduler, jumlah step inferensi, seed deterministik, dan metadata preprocessing. Tanpa dokumentasi ini, klaim kontribusi ilmiah sulit diverifikasi.
- Apa perbedaan mendasar GAN dengan diffusion model dalam hal mode coverage? GAN rentan terhadap mode collapse akibat optimasi min-max yang tidak stabil, sedangkan diffusion model menjamin coverage distribusi yang lebih luas melalui proses Markov reversibel, meski dengan biaya komputasi iteratif yang lebih tinggi.
- Apakah model diffusion menghafal data latih? Analisis memorization melibatkan pengujian proximity reconstruction, membership inference attack simulation, dan evaluasi privacy leakage, terutama ketika model dilatih pada dataset terbatas atau mengandung informasi sensitif.

Target keluaran yang diharapkan adalah penyusunan katalog eksperimen generatif yang terstruktur. Katalog ini tidak hanya berisi galeri visual, tetapi juga mencakup tabel perbandingan konsistensi, keragaman, bottleneck komputasi, serta rekomendasi penggunaan berdasarkan karakteristik dataset dan tujuan penelitian Anda. Pendekatan ini memastikan bahwa setiap implementasi menggunakan Diffusers, PyTorch, atau timm tidak bersifat trial-and-error, melainkan memiliki justifikasi eksperimental yang ketat.

Pergeseran perspektif ini akan membawa kita langsung ke slide berikutnya, yaitu transformasi fundamental dalam rumusan masalah penelitian dari pendekatan diskriminatif yang berfokus pada pemetaan x→y dan batas keputusan, menuju pendekatan generatif yang mempelajari distribusi p(x) dan kondisi bersyarat p(x|c). Dengan memahami lima pertanyaan kunci ini, Anda akan memiliki kerangka evaluasi yang diperlukan untuk membedah arsitektur, strategi conditioning, serta implikasi etis dan metodologis dari model generatif mutakhir dalam konteks riset doktoral.

---

## Slide 005 - Dari Diskriminatif ke Generatif: Perubahan Pertanyaan Penelitian

### Narasi

Pada pertemuan sebelumnya, kita telah merumuskan lima pertanyaan kunci yang menjadi landasan kritis dalam mengeksplorasi citra sintetis dan model generatif. Pertanyaan-pertanyaan tersebut menyentuh isu fundamental seperti trade-off antara generalisasi dan bias, validitas metrik kualitas versus keragaman, standar dokumentasi provenance data, perbedaan mendasar GAN dan diffusion model dalam cakupan mode, serta risiko memorisasi data latih. Slide ini akan menjembatani pemahaman kita dengan mengontraskan dua paradigma komputasi visual yang berbeda, sekaligus menunjukkan bagaimana formulasi pertanyaan penelitian harus berevolusi seiring pergeseran dari pendekatan diskriminatif ke generatif.

Model diskriminatif, yang telah mendominasi pembahasan pada pertemuan 1 hingga 8, berfokus pada pembelajaran batas keputusan atau pemetaan deterministik dari representasi citra ke label semantik. Tugas-tugas seperti klasifikasi gambar, deteksi objek, dan segmentasi semantik merupakan contoh klasik dari paradigma ini. Pertanyaan inti yang dijawab oleh model diskriminatif bersifat analitis: apa label, struktur, atau properti yang terkandung dalam citra input? Keunggulan utamanya terletak pada performa tinggi dalam tugas tertutup yang memiliki ground truth terstruktur dan label yang jelas.

Model generatif mengubah orientasi penelitian dengan mempelajari distribusi probabilitas data asli, baik secara marginal p(x) maupun bersyarat p(x|c). Keluarga model seperti GAN, diffusion models, hingga text-to-image systems dirancang untuk menangkap kompleksitas variabilitas visual dan menghasilkan sampel baru yang koheren. Pertanyaan penelitian bergeser secara radikal menjadi bagaimana cara mensintesis citra baru yang konsisten dengan karakteristik distribusi data atau memenuhi kondisi eksternal tertentu. Pergeseran ini menuntut peneliti untuk tidak hanya menilai akurasi prediksi, tetapi juga memahami mekanisme sampling, stabilitas konvergensi, dan integritas distribusi yang dihasilkan.

Implikasi bagi penelitian doktoral sangat strategis. Meskipun model diskriminatif tetap menjadi tulang punggung untuk evaluasi berbasis label, model generatif membuka ruang eksplorasi data sintetik, augmentasi domain, dan simulasi skenario langka yang sulit dikoleksi secara manual. Namun, kebebasan sintesis ini datang dengan beban metodologis yang lebih ketat. Evaluasi hasil generasi memerlukan protokol yang hati-hati, mencakup analisis konsistensi struktural, keragaman semantik, deteksi mode collapse, serta mitigasi bias yang mungkin terinternalisasi selama proses pembelajaran distribusi.

Transisi konseptual ini akan segera dioperasionalkan pada slide berikutnya melalui peta konsep generative vision. Kita akan menelusuri alur forward diffusion yang secara bertahap menambahkan noise hingga mencapai keadaan stokastik, dilanjutkan dengan reverse denoising yang belajar merekonstruksi sinyal asli. Mekanisme conditioning melalui teks, mask, atau citra referensi akan mengarahkan proses generasi, sementara parameter teknis seperti noise schedule, guidance strength, dan strategi evaluasi akan menjadi variabel kritis yang harus Anda kuasai dan rancang dalam eksperimen disertasi.

---

## Slide 006 - Peta Konsep Generative Vision

### Narasi

Pada slide sebelumnya, kita telah mengidentifikasi pergeseran mendasar dalam formulasi pertanyaan penelitian: dari pendekatan diskriminatif yang berfokus pada pemetaan deterministik ke label, beralih ke pendekatan generatif yang bertujuan memahami dan mensimulasikan distribusi data itu sendiri. Slide ini menyajikan peta konsep yang memetakan implementasi teknis dari paradigma tersebut dalam kerangka *generative vision*.

Diagram alur pada slide ini menggambarkan siklus inti dari arsitektur difusi. Proses dimulai dari data latih asli $x_0$, yang kemudian dialirkan melalui tahapan *forward diffusion*. Pada fase ini, noise Gaussian ditambahkan secara bertahap hingga mencapai representasi stokastik murni pada langkah $T$. Setelah itu, model dilatih untuk melakukan *reverse denoising*, yaitu mempelajari pola invers untuk menghilangkan noise secara iteratif dari $x_T$ kembali menuju rekonstruksi $x_0\_hat$. Tahapan selanjutnya adalah *conditioning*, di mana proses generasi dikendalikan oleh sinyal eksternal seperti teks, *mask* region, atau citra referensi. Output akhir berupa sintesis citra baru, area yang di-*inpainted*, atau gambar yang telah diedit sesuai kondisi input.

Untuk mendalami mekanisme di balik alur tersebut, cakupan materi pada bagian ini akan menyoroti lima pilar teknis yang wajib dikuasai:
- **Representasi laten**: kompresi fitur visual ke ruang berdimensi lebih rendah untuk efisiensi komputasi dan stabilitas training.
- **Noise schedule**: pengaturan laju penambahan dan pengurangan noise agar proses Markov tetap stabil dan konvergen.
- **Denoising process**: jantung pelatihan model yang memprediksi residual noise atau velocity pada setiap timesteps.
- **Guidance**: mekanisme kontrol (seperti classifier-free guidance) untuk menyelaraskan keluaran dengan prompt atau kondisi spesifik.
- **Evaluasi**: protokol metrik (FID, KID, Precision-Recall) untuk mengukur fidelitas, diversitas, dan kualitas sampel generatif secara rigor.

Pemahaman terhadap peta konsep ini menjadi fondasi teoretis sebelum kita memasuki definisi formal dan tujuan probabilistik dari sebuah *generative model*. Pada slide berikutnya, kita akan membedah gagasan dasar tentang bagaimana model generatif memodelkan distribusi probabilitas $p(x)$ maupun distribusi bersyarat $p(x|c)$, serta tiga tujuan pembelajaran utamanya: *sampling* realistis, estimasi *likelihood*, dan ekstraksi struktur laten. Penjelasan ini akan membantu Anda merumuskan kerangka metodologis yang kuat untuk eksplorasi riset doktoral di bidang *generative vision*.

---

## Slide 007 - Definisi Generative Model

### Narasi

Pada slide ini, kita membahas definisi fundamental dari *generative model*. Secara esensial, model generatif bertugas mempelajari distribusi probabilitas dari data observasi. Dalam konteks pengolahan citra digital, distribusi ini dapat direpresentasikan dalam dua bentuk matematis: $p(\mathbf{x})$ untuk distribusi marginal tanpa kondisi, dan $p(\mathbf{x}|\mathbf{c})$ untuk distribusi bersyarat di mana $\mathbf{c}$ merepresentasikan sinyal kondisional seperti teks, mask segmentasi, atau citra referensi. Penguasaan terhadap kedua formulasi distribusi ini menjadi prasyarat analitis sebelum mendalami mekanisme aproksimasi likelihood yang digunakan oleh arsitektur modern.

Tujuan komputasional dalam kerangka model generatif umumnya diklasifikasikan ke dalam tiga ranah utama:
- **Sampling**: kemampuan menghasilkan sampel baru yang secara statistik realistis dan konsisten dengan distribusi pelatihan, menjadi metrik keberhasilan paling intuitif untuk aplikasi kreatif dan augmentasi data.
- **Likelihood estimation**: fungsi untuk mengukur seberapa mungkin suatu data tertentu muncul berdasarkan model, sangat vital untuk evaluasi probabilistik, pembandingan model, dan deteksi anomali.
- **Representation learning**: proses di mana model dipaksa mengekstrak struktur laten dan variasi semantik yang mendasari data, memungkinkan interpolasi terkontrol, dekomposisi faktor, dan interpretasi representasi tingkat tinggi.

Ketika diaplikasikan pada domain citra, distribusi $p(\mathbf{x})$ tidak tersebar seragam di ruang piksel berdimensi tinggi, melainkan terkonsentrasi pada manifold berdimensi lebih rendah yang mematuhi hukum fisika dan semantik visual. Pada citra wajah, konsentrasi probabilitas membentuk manifold anatomi manusia yang koheren. Pada citra medis, pola konsentrasi bergeser mengikuti struktur jaringan, tekstur histologis, atau kontras agen pencitraan. Sementara itu, pada citra satelit, distribusi mencerminkan konfigurasi geografis, vegetasi, dan infrastruktur. Konsep manifold ini menjelaskan mengapa model generatif tidak boleh bekerja sebagai interpolator piksel naif, melainkan harus menangkap ketergantungan struktural jangka panjang.

Pembahasan mengenai distribusi probabilitas dan tujuan model generatif ini merupakan landasan teoretis langsung dari peta konsep pada slide sebelumnya, di mana proses *forward diffusion* dan *reverse denoising* sebenarnya merupakan mekanisme Markovian untuk mengaproksimasi distribusi target secara bertahap melalui penambahan dan penghilangan noise. Selanjutnya, pada slide berikutnya, kita akan mengonfrontasi definisi ini dengan taksonomi praktis dengan mengklasifikasikan berbagai pendekatan ke dalam keluarga model generatif, menganalisis trade-off antara kualitas sampling, estimasi likelihood, stabilitas pelatihan, serta posisi strategis arsitektur diffusion dalam ekosistem computer vision terkini.

---

## Slide 008 - Klasifikasi Keluarga Generative Model

### Narasi

Pada slide ini, kita akan membahas taksonomi atau klasifikasi keluarga dari model generatif. Pemetaan ini bukan sekadar pengelompokan teoritis, melainkan fondasi kritis untuk memahami trade-off mendasar dalam desain arsitektur, efisiensi komputasi, dan potensi kontribusi penelitian tingkat doktoral.

Secara umum, literatur terkini mengelompokkan model generatif menjadi empat keluarga utama berdasarkan mekanisme sampling dan pendekatan pemodelan distribusinya:

- **Autoregressive**, seperti PixelCNN atau Transformer berbasis patch. Model ini memodelkan data sebagai urutan dependen yang diprediksi satu per satu. Keunggulan utamanya adalah kemampuan menghitung likelihood eksplisit secara akurat dan stabil. Namun, proses sampling yang bersifat sekuensial membuatnya lambat, terutama untuk data berdimensi tinggi seperti citra resolusi penuh, serta rentan terhadap degradasi korelasi jangka panjang.
- **Variational**, contohnya VAE dan turunannya. Pendekatan ini mengandalkan kerangka probabilistik dengan encoder-decoder yang beroperasi di ruang laten kontinu. Kelebihan signifikan terletak pada representasi laten yang terstruktur dan smooth, memudahkan interpolasi, inpainting, dan kontrol kondisional. Keterbatasan klasik adalah optimisasi variational lower bound yang sering mengorbankan ketajaman frekuensi tinggi, menghasilkan sampel yang cenderung halus atau blur.
- **Implicit**, dengan GAN sebagai perwakilan dominan. Model ini tidak mendefinisikan density estimator eksplisit, melainkan mengandalkan kompetisi adversarial antara generator dan discriminator. Hasil sampling sangat tajam dan realistis, namun menghadapi tantangan fundamental seperti mode collapse, vanishing gradient, dan ketidakstabilan konvergensi. Evaluasi kualitas juga menjadi rumit karena tidak tersedia likelihood yang dapat dihitung secara langsung.
- **Diffusion**, mencakup DDPM, Score-Based Generative Modeling, hingga arsitektur skala besar seperti Stable Diffusion. Keluarga ini memodelkan proses markovian bertahap: forward process menambahkan noise secara gradual, sedangkan reverse process mempelajarinya secara iteratif. Kekuatan utamanya adalah coverage mode yang luas, stabilitas latihan yang konsisten, dan kompatibilitas tinggi dengan conditioning multimodal. Tantangan utamanya tetap pada beban komputasi akibat jumlah langkah denoising yang banyak, meskipun teknik distillation dan sampling accelerator terus berkembang.

Posisi diffusion dalam taksonomi ini strategis karena berhasil menjembatani celah antara likelihood yang dapat ditelusuri dan kualitas sampel yang kompetitif. Berbeda dengan GAN yang bergantung pada discriminative boundary, diffusion memanfaatkan score matching atau noise prediction yang lebih mudah dioptimalkan secara global. Hal ini membuka peluang riset yang signifikan di bidang kontrollable generation, efficiency optimization, dan integrasi dengan foundation models.

Pembahasan mengenai kelemahan inherent pada implicit model, khususnya dinamika adversarial dan kegagalan konvergensi, akan kita telaah lebih lanjut pada slide berikutnya melalui recap singkat komponen GAN. Sementara itu, pemahaman tentang tujuan dasar generative model dari slide sebelumnya—sampling, likelihood, dan representasi laten—menjadi lensa analitis untuk menilai mengapa setiap keluarga ini memilih jalur matematis yang berbeda dalam mendekati $p(x)$ maupun $p(x|c)$.

---

## Slide 009 - Recap Singkat GAN

### Narasi

Setelah pada slide sebelumnya kita mengklasifikasikan taksonomi keluarga model generatif, kini kita fokus meninjau kembali salah satu pilar utamanya, yaitu *Generative Adversarial Networks* (GAN). Memahami arsitektur dan dinamika latihan GAN secara mendalam merupakan prasyarat analitis penting sebelum mengevaluasi mengapa paradigma *diffusion* mampu mengatasi banyak keterbatasan historisnya dalam konteks *generative vision*.

Komponen dasar GAN dapat diringkas sebagai berikut:
- **Generator $G(z)$**: Jaringan yang memetakan vektor laten acak $z$ ke dalam ruang dimensi tinggi untuk mensintesis citra.
- **Discriminator $D(x)$**: Jaringan pengklasifikasi yang bertugas membedakan input antara data observasi asli dan hasil sintesis generator.

Proses pelatihan keduanya dijalankan dalam skema permainan kompetitif di mana generator berusaha meminimalkan kemampuan discriminator untuk mendeteksi kepalsuan, sementara discriminator berusaha memaksimalkan akurasinya. Secara teoretis, sistem ini diharapkan mencapai keseimbangan Nash di mana distribusi sampel generator identik sempurna dengan distribusi data nyata.

Meskipun berhasil menghasilkan sampel yang tajam, implementasi GAN dalam penelitian tingkat lanjut menghadapi tiga masalah fundamental yang sering menghambat reproduktibilitas dan skalabilitas:
- **Mode collapse**: Generator cenderung terjebak pada subset moda tertentu dari data training, sehingga gagal menangkap keragaman distribusi asli.
- **Ketidakstabilan latihan**: Interaksi minimax yang sensitif terhadap inisialisasi dan arsitektur membuat konvergensi sulit dicapai tanpa teknik regularisasi tambahan yang kompleks.
- **Evaluasi tanpa likelihood eksplisit**: Tidak adanya fungsi kepadatan probabilitas yang dapat dihitung langsung membuat penilaian kualitas generasi bergantung pada metrik proxy seperti FID atau Inception Score, yang tidak selalu berkorelasi dengan fidelitas struktural maupun semantik.

Keterbatasan ini menjelaskan mengapa riset mutakhir mulai menggeser fokus dari kompetisi adversarial menuju proses probabilistik yang lebih terstruktur dan terukur. Stabilitas optimasi, cakupan moda yang lebih lengkap, serta ketersediaan batas bawah variasional (*variational lower bound*) untuk evaluasi likelihood menjadikan *diffusion models* sebagai kandidat dominan yang melengkapi atau menggantikan GAN. Pada slide berikutnya, kita akan membedah perbedaan konseptual dan implikasi penelitian antara kedua pendekatan ini secara komprehensif.

---

## Slide 010 - Mengapa Diffusion Models? Perbedaan dengan GAN

### Narasi

Pada slide sebelumnya, kita telah meninjau kembali arsitektur dan dinamika latihan Generative Adversarial Networks. Generator dan discriminator saling berkompetisi dalam permainan minimax yang secara teoretis mengarah pada keseimbangan Nash. Namun, dalam implementasinya, mekanisme adversarial ini kerap mengalami mode collapse, ketidakstabilan konvergensi, serta kesulitan dalam evaluasi likelihood yang eksplisit. Keterbatasan fundamental inilah yang menjadi pemicu pergeseran paradigma menuju model generasi berbasis denoising bertahap.

Slide ini menyajikan perbandingan konseptual antara GAN dan Diffusion Model untuk menjustifikasi mengapa pendekatan difusi kini menjadi standar baru dalam riset generative vision. Perbedaan utamanya dapat diuraikan sebagai berikut:
- Mekanisme pelatihan GAN bersifat kompetitif-adversarial, sedangkan Diffusion Model mengandalkan proses denoising bertahap yang lebih deterministik dan terstruktur.
- Stabilitas latihan pada Diffusion Model jauh lebih tinggi karena menghindari masalah mode collapse yang kronis pada GAN.
- Cakupan mode atau keragaman data yang dihasilkan oleh Diffusion Model cenderung lebih lengkap dan representatif terhadap distribusi data asli.
- Kualitas sampel Diffusion Model kini mampu mencapai ketajaman visual yang setara dengan GAN state-of-the-art, namun dengan robustness yang lebih baik.
- Trade-off utamanya terletak pada kecepatan sampling yang lebih lambat karena sifatnya yang iteratif dan memerlukan banyak langkah denoising.
- Secara probabilistik, Diffusion Model menyediakan variational lower bound untuk menghitung likelihood, berbeda dengan GAN yang tidak memiliki estimasi likelihood eksplisit.
- Conditioning pada Diffusion Model dapat diintegrasikan secara natural melalui guidance mechanism, tanpa perlu rekayasa arsitektur tambahan yang kompleks.

Konsekuensi langsung dari perbandingan ini bagi agenda riset tingkat doktoral adalah adanya ruang untuk eksplorasi hibridisasi. Diffusion Model unggul dalam konsistensi dan keragaman generasi, sementara GAN tetap relevan ketika latensi inference dan efisiensi komputasi menjadi constraint kritis. Tren penelitian terkini banyak memanfaatkan teknik distillation, classifier-free guidance, atau arsitektur campuran yang meminimalkan kelemahan masing-masing pendekatan.

Memahami keunggulan konseptual ini membawa kita ke inti mekanismenya. Pada slide berikutnya, kita akan membedah forward diffusion process, yaitu bagaimana citra bersih ditransformasi secara bertahap ke arah distribusi Gaussian murni melalui penambahan noise Gaussian kecil pada setiap timestep. Proses ini bersifat markovian dan memiliki solusi bentuk tertutup yang memungkinkan perhitungan langsung dari state awal, yang menjadi pondasi matematis untuk reverse process dan sampling di tahap selanjutnya.

---

## Slide 011 - Forward Diffusion: Menambahkan Noise Secara Bertahap

### Narasi

Setelah pada slide sebelumnya kita menguraikan keunggulan konseptual model difusi dibandingkan GAN, khususnya terkait stabilitas latihan dan cakupan mode yang lebih lengkap, kini kita beralih ke mekanisme operasionalnya. Forward diffusion adalah jantung dari arsitektur generatif berbasis difusi, dan memahami alurnya secara intuitif maupun matematis merupakan prasyarat utama sebelum memasuki tahap denoising.

Gagasan dasarnya dimulai dari sebuah citra bersih yang kita sebut sebagai `x0`. Pada setiap langkah waktu `t`, kita menambahkan noise Gaussian kecil secara bertahap ke citra tersebut. Prosesnya bersifat rekursif: hasil transformasi pada langkah sebelumnya menjadi input untuk langkah berikutnya. Secara ringkas, langkah pertama dapat digambarkan sebagai kombinasi linear antara citra asli dan noise acak, yang masing-masing ditimbang oleh koefisien tertentu. Proses ini terus berulang hingga mencapai langkah akhir `T`, di mana citra telah terdegradasi sepenuhnya menjadi distribusi Gaussian murni yang tidak lagi mempertahankan pola visual dari objek asli.

Kunci pengendalian proses ini terletak pada noise schedule, atau jadwal varians noise. Parameter `beta_t` merepresentasikan varians noise yang disuntikkan pada langkah ke-`t`. Schedule ini berfungsi sebagai pengatur laju transisi informasi. Jika nilai `beta_t` dipilih terlalu besar, citra akan kehilangan struktur geometrisnya secara drastis dalam beberapa iterasi awal. Sebaliknya, jika nilainya terlalu kecil, proses degradasi berjalan lambat dan memerlukan jumlah langkah `T` yang sangat besar tanpa meningkatkan kualitas representasi latent. Pemilihan schedule yang optimal biasanya mengikuti kurva linear atau cosine-shaped, tergantung pada kompleksitas dataset dan tujuan penelitian.

Ada dua sifat kritis dari forward diffusion yang harus diperhatikan. Pertama, proses ini bersifat markovian, artinya distribusi keadaan pada langkah `t` hanya bergantung pada keadaan pada langkah `t-1`, tanpa memerlukan memori historis lengkap. Kedua, meskipun deskripsinya iteratif, proses ini memiliki solusi dalam bentuk tertutup (closed-form). Artinya, kita tidak perlu menjalankan simulasi bertahap dari `x0` hingga `xT` untuk memperoleh state intermediate. Variabel `xt` dapat dihitung secara eksplisit langsung dari `x0` dengan mengakumulasi seluruh koefisien noise schedule. Sifat closed-form inilah yang memungkinkan komputasi batch yang efisien selama fase training model.

Penjelasan konseptual ini akan segera kita formalisasikan secara rigor pada slide berikutnya. Di sana kita akan menurunkan definisi distribusi kondisional `q(xt | xt-1)` dan menunjukkan bagaimana parameter `alpha_t` serta produk kumulatifnya `alpha_bar_t` membentuk hubungan analitik langsung antara citra awal dan citra yang telah terdistrusi, yang menjadi fondasi utama dalam penulisan loss function diffusion model.

---

## Slide 012 - Matematika Dasar Forward Diffusion

### Narasi

Pada slide sebelumnya, kita telah membahas gagasan dasar forward diffusion di mana citra bersih ditambahkan noise secara bertahap hingga mendekati distribusi Gaussian murni. Proses ini bersifat Markovian dan dapat dihitung secara langsung tanpa harus mensimulasikan setiap langkah satu per satu. Untuk memahami mekanisme tersebut secara rigor, kita perlu meninjau formulasi matematika yang menjadi tulang punggung proses penambahan noise ini.

Secara formal, transisi dari langkah waktu $t-1$ ke $t$ dimodelkan sebagai distribusi normal multivariat dengan notasi $q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1-\beta_t} x_{t-1}, \beta_t I)$. Di sini, $\beta_t$ merepresentasikan varians atau jadwal noise pada langkah ke-$t$, sedangkan $I$ adalah matriks identitas yang menjamin noise ditambahkan secara independen pada setiap dimensi piksel. Faktor $\sqrt{1-\beta_t}$ berperan sebagai skala pelestari sinyal, memastikan bahwa informasi visual awal tidak hilang secara instan sebelum diganggu oleh komponen stokastik.

Salah satu keunggulan fundamental dari kerangka kerja ini adalah kemampuan untuk menuliskan ulang rantai transisi bertahap menjadi bentuk tertutup terhadap citra awal $x_0$. Dengan mendefinisikan $\alpha_t = 1 - \beta_t$ dan $\bar{\alpha}_t$ sebagai produk kumulatif dari $\alpha_1$ hingga $\alpha_t$, kita dapat menurunkan distribusi langsung $q(x_t | x_0) = \mathcal{N}(x_t; \sqrt{\bar{\alpha}_t} x_0, (1 - \bar{\alpha}_t) I)$. Bentuk tertutup ini sangat krusial dalam implementasi komputasional karena memungkinkan sampling noise secara simultan tanpa iterasi rekursif, sehingga mempercepat training dan stabilisasi gradien.

Dari persamaan bentuk tertutup tersebut, dinamika proses dapat diinterpretasikan berdasarkan posisi $t$. Ketika $t$ masih kecil, koefisien $\sqrt{\bar{\alpha}_t}$ bernilai mendekati satu sehingga $x_t$ tetap mempertahankan struktur dan tekstur $x_0$. Sebaliknya, seiring $t$ membesar menuju $T$, varians $(1 - \bar{\alpha}_t)$ mendominasi dan sinyal gambar perlahan teredam, meninggalkan hanya noise Gaussian putih. Laju transisi ini sepenuhnya dikendalikan oleh noise schedule yang telah dikonfigurasi. Pemahaman matematis ini menjadi fondasi analitis yang diperlukan untuk melangkah ke langkah berikutnya, yaitu merancang proses reverse denoising yang akan mempelajari cara membalikkan alur difusi tersebut secara probabilistik.

---

## Slide 013 - Reverse Denoising: Belajar Menghilangkan Noise

### Narasi

Merujuk pada pembahasan slide sebelumnya mengenai matematika dasar forward diffusion, kita telah melihat bagaimana citra bersih secara sistematis dihancurkan menjadi noise murni melalui serangkaian transisi Gaussian. Langkah logis berikutnya adalah memahami mekanisme pembalikannya, atau reverse denoising, yang menjadi inti dari generasi gambar berbasis difusi.

Gagasan teoretisnya cukup elegan: jika distribusi kondisional $q(x_{t-1} | x_t, x_0)$ diketahui, maka secara matematis kita dapat melakukan perjalanan mundur dari $x_T$ menuju $x_0$. Namun, dalam implementasinya muncul masalah fundamental. Distribusi reverse sebenarnya $q(x_{t-1} | x_t)$ tidak dapat diakses secara analitik karena informasi awal $x_0$ telah hilang selama proses forward.

Untuk mengatasi hal ini, kita mengganti distribusi yang tidak diketahui tersebut dengan model parametrik yang dapat dipelajari. Aproksimasi reverse didefinisikan sebagai:
- $p_\theta(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; \mu_\theta(x_t, t), \Sigma_\theta(x_t, t))$

Parameter $\mu_\theta$ dan $\Sigma_\theta$ bukanlah konstanta, melainkan keluaran dinamis dari jaringan saraf yang menerima input citra noisy pada langkah $t$ serta embedding waktu $t$. Pendekatan ini memungkinkan model menyesuaikan estimasinya terhadap tingkat degradasi noise pada setiap tahap.

Secara intuisi, fokus pelatihan bergeser dari rekonstruksi piksel langsung ke prediksi residual noise. Model tidak diminta menebak seluruh konten gambar sekaligus, melainkan hanya memperkirakan vektor noise yang harus dikurangi. Proses ini diulang secara iteratif sebanyak $T$ kali. Setiap siklus menghilangkan sebagian gangguan, hingga pada akhir rantai reverse, sinyal yang tersisa membentuk struktur visual yang koheren dan realistis.

Konsep reverse denoising ini menjadi fondasi kritis bagi perkembangan Denoising Diffusion Probabilistic Models (DDPM) yang akan kita bahas pada slide berikutnya. DDPM mengonversi teori aproksimasi di atas menjadi kerangka pelatihan yang jauh lebih efisien dengan membuktikan bahwa memprediksi noise epsilon secara langsung menghasilkan stabilitas optimasi yang superior, sebuah terobosan metodologis yang akan kita bedah lebih lanjut.

---

## Slide 014 - Denoising Diffusion Probabilistic Models (DDPM)

### Narasi

Pada slide ini, kita membahas fondasi metodologis dari generasi citra modern, yaitu *Denoising Diffusion Probabilistic Models* atau DDPM. Karya seminal dari Ho dkk. pada tahun 2020 ini berhasil menyederhanakan kerangka kerja difusi yang sebelumnya sangat kompleks secara probabilistik. Terobosan utamanya terletak pada reformulasi proses pelatihan menjadi tugas prediksi noise yang jauh lebih stabil dan mudah dioptimalkan.

Alih-alih mencoba memodelkan distribusi data secara langsung atau menghitung langkah mundur yang melibatkan turunan log-probabilitas yang rumit, DDPM mengalihkan fokus ke prediksi komponen noise Gaussian. Pendekatan ini mengubah masalah generasi yang non-linear menjadi regresi sederhana namun sangat representatif.

Formulasi fungsi kerugian (*objective training*) dalam DDPM dapat dituliskan sebagai berikut:
- `L = E_{t, x0, epsilon} [ || epsilon - epsilon_theta(x_t, t) ||^2 ]`

Dalam persamaan ini, `epsilon_theta` merepresentasikan jaringan neural parameter theta yang bertugas menebak noise aktual. Variabel `t` menandakan langkah waktu saat noise disuntikkan, `x0` adalah citra bersih awal, dan `x_t` adalah citra yang sudah terdegradasi. Ekspektasi dihitung melintasi distribusi langkah waktu, sampel citra awal, dan vektor noise standar.

Interpretasi dari objective tersebut sangat intuitif bagi peneliti computer vision. Model dilatih untuk meminimalkan error kuadrat antara noise yang benar-benar ditambahkan selama proses *forward diffusion* dan estimasi yang dihasilkan oleh jaringan. Semakin rendah nilai loss ini, semakin presisi model dalam menguraikan struktur noise pada setiap skala waktu, yang secara langsung meningkatkan fidelitas hasil denoising saat proses *reverse* dieksekusi.

Untuk mengimplementasikan prediksi noise ini secara komputasional, DDPM mengadopsi arsitektur U-Net sebagai backbone utama. Pilihan arsitektur ini didasarkan pada kemampuannya menangani resolusi spasial yang bervariasi sambil mempertahankan informasi kontekstual. U-Net tidak hanya berfungsi sebagai ekstraktor fitur, tetapi juga sebagai mesin estimasi noise yang peka terhadap dinamika temporal proses difusi.

Dengan demikian, penjelasan di atas menjembatani konsep aproksimasi distribusi balik dari slide sebelumnya dengan implementasi arsitektural yang akan kita bedah lebih lanjut. Pada slide berikutnya, kita akan mengurai secara spesifik mengapa U-Net menjadi standar industri, bagaimana mekanisme *skip connection* menjaga preservasi detail tekstur, serta bagaimana *time embedding* disuntikkan ke dalam blok-blok jaringan untuk menandai posisi setiap langkah denoising.

---

## Slide 015 - U-Net sebagai Backbone Denoising

### Narasi

Melanjutkan pembahasan objective training pada DDPM yang berfokus pada prediksi noise epsilon, kita kini beralih ke arsitektur yang menjadi tulang punggung proses denoising tersebut, yaitu U-Net. Pilihan arsitektur ini didasarkan pada kemampuannya mengintegrasikan struktur encoder-decoder dengan skip connection, sehingga dapat menangkap detail lokal sekaligus konteks global secara simultan. Sebagaimana telah dibuktikan pada materi segmentasi citra di pertemuan sebelumnya, mekanisme skip connection ini sangat efektif dalam menjaga integritas spasial selama proses kompresi dan rekonstruksi fitur, menjadikannya standar de facto untuk tugas vision yang memerlukan preservasi struktur.

Dalam implementasinya pada diffusion models, U-Net beroperasi melalui empat komponen utama yang saling terintegrasi:
- **Encoder**: Mengecilkan resolusi spasial sambil memperluas dimensi channel untuk ekstraksi fitur hierarkis.
- **Bottleneck**: Menampung representasi paling padat dan kontekstual dari seluruh input citra.
- **Decoder**: Mengembalikan resolusi citra ke ukuran awal melalui serangkaian operasi upsampling.
- **Skip Connection**: Menyalin fitur resolusi tinggi dari encoder langsung ke decoder, mencegah hilangnya informasi detail halus seperti tepi objek atau tekstur kompleks selama proses denoising.

Di samping struktur spasial, model diffusion juga memerlukan informasi temporal berupa langkah waktu $t$. Setiap langkah denoising memiliki karakteristik noise yang berbeda, sehingga model harus menyesuaikan strategi pemulihannya secara dinamis. Informasi $t$ diolah menjadi vektor embedding dan disuntikkan ke setiap blok arsitektur U-Net. Mekanisme conditioning ini menjamin bahwa jaringan saraf merespons tingkat kesulitan pada setiap iterasi dengan tepat. Pembahasan lebih lanjut mengenai teknik embedding sinusoidal dan implikasi strategisnya terhadap stabilitas training serta kualitas hasil denoising akan diuraikan secara mendetail pada slide berikutnya.

---

## Slide 016 - Peran Time Embedding

### Narasi

Merujuk pada penjelasan sebelumnya mengenai U-Net sebagai backbone denoising, slide ini akan mengupas lebih dalam salah satu komponen paling kritis dalam arsitektur tersebut, yaitu time embedding. Tanpa mekanisme ini, model tidak memiliki referensi temporal untuk menyesuaikan intensitas transformasi pada setiap langkah iteratif.

Keberadaan parameter $t$ memberikan konteks penting kepada model tentang tingkat noise yang masih melekat pada citra saat ini:
- Saat $t$ bernilai besar, dominasi noise sangat tinggi sehingga model harus melakukan penyesuaian struktural yang lebih agresif untuk membentuk siluet dasar objek.
- Saat $t$ mendekati nol, noise sudah sangat minim dan fokus model bergeser ke penajaman detail halus seperti tekstur, kontras, dan batas objek.
Informasi ini memungkinkan model mengatur perilaku kondisionalnya secara dinamis sesuai dengan fase denoising yang sedang berjalan.

Implementasi teknisnya mengubah nilai skalar $t$ menjadi representasi vektor berdimensi tinggi melalui sinusoidal embedding. Vektor hasil embedding ini kemudian diinjeksikan atau ditambahkan pada feature map di setiap blok encoder maupun decoder. Pendekatan ini menjamin bahwa setiap lapisan jaringan tetap aware terhadap posisi langkah difusi, sehingga pola aktivasi neuron dapat menyesuaikan diri secara presisi tanpa mengganggu hierarki ekstraksi fitur spasial.

Dari sisi implikasi arsitektural, kualitas conditioning via time embedding secara langsung menentukan stabilitas training dan fidelitas output akhir. Integrasi yang tidak optimal dapat menyebabkan ketidakseimbangan antara respons global dan lokal, yang pada akhirnya memicu artefak visual atau kegagalan konvergensi pada tahap akhir proses. Pemahaman mendalam terhadap mekanisme ini menjadi fondasi sebelum kita mengevaluasi skalabilitas arsitektur berbasis ruang piksel.

Pembahasan selanjutnya akan menyoroti tantangan komputasi yang muncul ketika proses difusi dijalankan langsung pada resolusi gambar asli. Beban memori dan latency yang meningkat secara eksponensial seiring jumlah langkah $T$ menjadi alasan utama munculnya motivasi untuk beralih ke Latent Diffusion Models, di mana proses stokastik dipindahkan ke ruang laten berdimensi lebih rendah demi efisiensi tanpa mengorbankan kualitas semantik.

---

## Slide 017 - Latent Diffusion Models: Motivasi

### Narasi

Pada slide sebelumnya, kita telah membahas mengapa informasi waktu atau $t$ sangat krusial dalam proses denoising. Model perlu mengetahui langkah waktu saat ini agar dapat menyesuaikan tingkat agresivitas penghilangan noise secara halus. Implementasi time embedding menggunakan fungsi sinusoidal memungkinkan U-Net memahami konteks iterasi difusi tersebut. Namun, meskipun mekanisme conditioning sudah mapan, menjalankan proses difusi langsung pada ruang piksel citra resolusi tinggi tetap menghadapi tantangan komputasi yang signifikan.

Ketika denoising dilakukan di ruang piksel, setiap iterasi menuntut U-Net memproses jutaan titik warna secara simultan. Hal ini menyebabkan beban memori yang besar dan waktu inferensi yang panjang, terutama karena proses difusi harus diulang sebanyak $T$ kali. Untuk skala penelitian doktoral maupun aplikasi industri, pendekatan pixel-level ini menjadi tidak efisien dan sulit diskalakan.

Gagasan Latent Diffusion Models hadir sebagai respons terhadap bottleneck tersebut. Alih-alih bekerja langsung pada domain piksel, model pertama-tama mengompresi representasi citra ke dalam ruang laten berdimensi lebih rendah. Proses difusi kemudian dijalankan pada ruang kompak ini. Setelah noise berhasil diprediksi dan dihilangkan, hasil representasi laten didekode kembali menjadi citra akhir.

Keuntungan dari pendekatan ini bersifat ganda dan strategis:
- Efisiensi komputasi meningkat drastis karena dimensi data yang diproses oleh jaringan saraf jauh lebih kecil.
- Ruang laten secara alami menangkap struktur semantik dan fitur tingkat tinggi, sehingga model dapat fokus pada generasi konten yang koheren tanpa terbebani detail piksel redundan.

Motivasi efisiensi dan fokus semantik inilah yang menjadi landasan transisi dari Pixel Diffusion ke Latent Diffusion. Dengan fondasi konseptual yang jelas, kita siap untuk membedah implementasinya. Pada slide berikutnya, kita akan menguraikan arsitektur lengkap Latent Diffusion Model, mulai dari peran Encoder-Decoder, integrasi U-Net di ruang laten, hingga mekanisme kondisioning melalui cross-attention.

---

## Slide 018 - Arsitektur Latent Diffusion Model (LDM)

### Narasi

Slide ini menguraikan arsitektur inti dari Latent Diffusion Model atau LDM. Setelah pada slide sebelumnya kita membahas motivasi untuk meninggalkan komputasi di ruang piksel murni, kini kita lihat bagaimana komponen-komponen tersebut diorganisir secara sistematis.

Arsitektur LDM dibangun atas empat blok fungsional yang saling terintegrasi. Pertama, Encoder $E$ memproyeksikan citra input $x$ ke dalam manifold representasi laten $z$. Kedua, Decoder $D$ melakukan inversi, memetakan kembali laten $z$ ke domain citra. Ketiga, Diffusion U-Net beroperasi eksklusif pada ruang laten untuk menjalankan proses denoising bertahap. Keempat, mekanisme kondisioning eksternal, seperti prompt teks, diinjeksikan ke dalam jaringan melalui cross-attention layer agar output visual tetap koheren dengan instruksi semantik.

Alur pemrosesan datanya dapat direpresentasikan secara ringkas sebagai berikut:
```
x -> E(z) -> [diffusion in latent] -> z_hat -> D(x_hat)
```
Citra awal $x$ dikompresi oleh encoder menjadi laten $z$. Selama fase training atau inference, U-Net memandu proses stokastik di ruang laten hingga mencapai laten bersih $z\_hat$. Tahap terakhir dilakukan decoding oleh $D$ untuk menghasilkan citra sintetik $x\_hat$.

Implementasi paling matang dari desain ini adalah Stable Diffusion. Model tersebut mengandalkan Variational Autoencoder (VAE) sebagai pasangan encoder-decoder. Keunggulan utamanya terletak pada reduksi dimensi spasial yang signifikan; U-Net umumnya beroperasi pada tensor laten berukuran 64×64 atau resolusi setara lainnya. Kompresi ini menurunkan kompleksitas komputasi secara eksponensial, memungkinkan penggunaan U-Net berskala besar dengan ribuan langkah denoising tanpa melanggar batasan memori GPU.

Struktur arsitektural ini menunjukkan bahwa efisiensi LDM tidak hanya berasal dari pengurangan dimensi, tetapi juga dari dekomposisi tugas antara kompresi informasi dan generasi dinamis. Namun, performa akhir model sangat bergantung pada kapasitas encoder dan decoder dalam mempertahankan informasi semantik selama proses kompresi-dekompresi. Pembahasan mengenai bagaimana VAE berfungsi sebagai kompresor laten, serta dampaknya terhadap batas kualitas dan keragaman sampel, akan kita bahas secara mendalam pada slide berikutnya.

---

## Slide 019 - Peran VAE dalam Latent Diffusion

### Narasi

Pada slide sebelumnya, kita telah membahas arsitektur umum Latent Diffusion Model (LDM), di mana proses difusi tidak lagi dilakukan langsung pada ruang piksel berdimensi tinggi, melainkan dipindahkan ke dalam representasi laten yang lebih kompak. Slide ini akan menguraikan secara mendalam mengapa dan bagaimana Variational Autoencoder (VAE) menjadi komponen krusial dalam paradigma tersebut.

VAE berfungsi sebagai kompresor non-linear yang mempelajari distribusi probabilitas dari data citra asli. Encoder VAE memetakan setiap citra input $x$ ke dalam variabel laten $z$, yang merepresentasikan fitur-fitur esensial dengan dimensi jauh lebih rendah dibandingkan resolusi piksel asli. Sebaliknya, decoder bertugas merekonstruksi kembali citra dari variabel laten tersebut. Proses pembelajaran ini mendorong model untuk menangkap struktur semantik dan tekstur penting, sekaligus membuang noise atau redundansi yang tidak relevan bagi generasi visual.

Penting untuk dipahami bahwa LDM melakukan langkah forward dan reverse diffusion tepat di ruang laten yang dihasilkan oleh VAE, bukan di domain piksel. Hal ini memberikan dua keuntungan utama: pertama, mengurangi beban komputasi dan kebutuhan memori secara drastis karena ukuran tensor laten jauh lebih kecil; kedua, menciptakan ruang representasi yang lebih halus dan terdistribusi mendekati normal, sehingga proses stokastik difusi berjalan lebih stabil dan konvergen lebih cepat.

Namun, keberadaan VAE juga membawa implikasi teknis yang perlu diperhatikan dalam penelitian tingkat lanjut. Kualitas dan keragaman sampel yang dihasilkan oleh model generatif sangat bergantung pada kemampuan decoder VAE dalam merekonstruksi detail halus. Jika decoder mengalami underfitting atau bottleneck yang terlalu ketat, batasan atas kualitas output akan terbentuk, terlepas dari seberapa canggih jaringan U-Net difusinya. Oleh karena itu, fine-tuning VAE khusus pada domain target atau penggunaan arsitektur decoder yang lebih ekspresif sering kali menjadi strategi efektif untuk meningkatkan fidelitas visual tanpa mengubah arsitektur inti diffusion model.

Dengan pemahaman tentang peran fundamental VAE sebagai fondasi ruang laten ini, kita dapat melihat bagaimana informasi eksternal kemudian disuntikkan ke dalam proses generasi. Slide berikutnya akan membahas mekanisme conditioning, mulai dari bentuk-bentuk kondisi seperti teks, gambar, hingga mask, serta teknik injeksi melalui concatenation, cross-attention, atau adaptive normalization yang memungkinkan kontrol presisi terhadap hasil difusi.

---

## Slide 020 - Conditioning: Membawa Informasi Eksternal

### Narasi

Setelah pada slide sebelumnya kita membahas bagaimana Variational Autoencoder (VAE) berperan sebagai kompresor yang memampatkan citra ke dalam ruang laten berdimensi lebih rendah, langkah selanjutnya dalam arsitektur Latent Diffusion Model adalah menentukan bagaimana proses denoising tersebut diarahkan. Di sinilah konsep conditioning hadir sebagai mekanisme fundamental untuk membawa informasi eksternal ke dalam lintasan stokastik difusi. Tanpa conditioning, model hanya akan melakukan sampling dari distribusi prior, menghasilkan gambar yang secara statistik valid namun tidak terkontrol semantik maupun strukturnya.

Kondisi dapat direpresentasikan dalam berbagai bentuk tergantung pada tujuan aplikasi. Pada generasi berbasis deskripsi, kondisi berupa teks yang mengkodekan makna semantik. Untuk tugas referensi visual atau style transfer, kondisi diambil dari citra lain. Pada inpainting dan editing selektif, kondisi berbentuk mask biner yang menandai region spesifik untuk diregenerasi. Selain itu, label kategori atau anotasi spasial juga dapat berfungsi sebagai constraint struktural yang memandu arsitektur jaringan selama proses iteratif.

Mekanisme injeksi kondisi ke dalam model umumnya mengikuti tiga pendekatan arsitektural utama. Pertama, concatenation menggabungkan vektor atau tensor kondisi langsung ke kanal input atau feature map laten. Pendekatan ini sederhana tetapi bersifat statis dan kurang mampu menangkap interaksi dinamis antara konten kondisi dan fitur spasial yang berkembang. Kedua, cross-attention telah menjadi standar de facto pada model modern. Kondisi diproyeksikan menjadi matrix key dan value, sementara representasi spasial dari U-Net bertindak sebagai query. Mekanisme ini memungkinkan alignment kontekstual yang adaptif di setiap blok attention. Ketiga, modul adaptif seperti AdaIN atau FiLM memanipulasi statistik normalisasi atau transformasi affine pada layer tengah, sehingga kondisi mengatur skala dan pergeseran distribusi fitur tanpa mengubah topologi inti jaringan.

Pemilihan strategi conditioning secara langsung menentukan tingkat kendali pengguna atas hasil sintesis. Pada level riset doktoral, memahami trade-off antara kompleksitas komputasi cross-attention versus efisiensi adaptive modulation menjadi kunci dalam merancang arsitektur yang scalable. Sebagaimana akan kita bedah pada slide berikutnya, integrasi kondisi teks melalui cross-attention dalam pipeline end-to-end memanfaatkan encoder bahasa yang telah diselaraskan dengan ruang visual, sehingga menjembatani gap semantik antara prompt linguistik dan struktur piksel yang dihasilkan.

---

## Slide 021 - Text-to-Image dengan Cross-Attention

### Narasi

Pada slide ini, kita mengupas implementasi spesifik dari mekanisme *cross-attention* sebagai inti dari generasi *text-to-image* dalam arsitektur diffusion model. Sebagaimana dibahas pada slide 20 mengenai berbagai strategi *conditioning*, injeksi kondisi melalui *cross-attention* terbukti paling efektif untuk menangani data multimodal yang heterogen, khususnya teks yang bersifat sekuensial dan abstrak.

Proses dimulai dengan pemrosesan deskripsi linguistik menggunakan *text encoder*, umumnya CLIP atau T5. Encoder ini memetakan rangkaian token kata ke dalam ruang vektor berdimensi tinggi yang merepresentasikan makna semantik secara padat. Vektor hasil enkoding inilah yang berfungsi sebagai kondisi $c$ selama seluruh siklus denoising berjalan.

Integrasi kondisi teks ke dalam U-Net dilakukan pada setiap blok residual maupun tahap down/up-sampling. Lapisan *cross-attention* bekerja dengan mengambil representasi spasial citra laten sebagai *query*, sementara embedding teks berperan sebagai *key* dan *value*. Mekanisme ini memungkinkan setiap region dalam ruang laten untuk secara dinamis menarik informasi relevan dari deskripsi teks, sehingga tercipta sinkronisasi antara struktur geometris citra dan interpretasi semantik instruksi.

Ilustrasi alur yang tercantum pada slide merangkum pipeline end-to-end yang lazim diimplementasikan dalam ekosistem Diffusers:

```
Text -> text encoder -> embedding teks
                            |
Citra laten -> U-Net -> cross-attention -> denoised latent
                            |
                         VAE decoder -> citra
```

Pipeline ini menekankan efisiensi komputasi dengan menjalankan proses stokastik denoising di ruang laten yang berdimensi jauh lebih kecil dibandingkan ruang piksel asli. Setelah iterasi denoising selesai, latent yang telah bersih diteruskan ke VAE decoder untuk direkonstruksi kembali ke domain visual yang dapat diamati manusia.

Pemilihan CLIP sebagai *text encoder* standar didasari oleh sifatnya yang telah diselaraskan secara kontrastif pada skala besar. Representasi joint space yang dihasilkan memastikan bahwa deskripsi teks dan fitur visual berada dalam manifold yang koheren, sehingga model mampu menangkap nuansa kompleks seperti pencahayaan, gaya artistik, atau relasi spasial tanpa memerlukan penyetelan ulang yang masif.

Dengan fondasi *text-conditioned generation* yang mapan, logika kondisioning dapat digeneralisasi ke modalitas lain. Slide berikutnya akan menyoroti bagaimana prinsip yang sama diterapkan pada tugas *image-to-image* dan *inpainting*, di mana referensi visual dan mask spasial menggantikan teks sebagai pengendali utama, serta prosedur teknis penanganan region yang harus dipertahankan versus region yang boleh dimodifikasi.

---

## Slide 022 - Image-to-Image dan Inpainting

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana representasi teks dari text encoder diintegrasikan ke dalam arsitektur U-Net melalui mekanisme cross-attention. Kondisi berbasis bahasa memungkinkan model menafsirkan deskripsi semantik dan menghasilkan citra baru dari distribusi noise. Namun, dalam banyak skenario penelitian dan aplikasi industri, kondisi yang diperlukan bukan berupa kalimat, melainkan citra sumber itu sendiri. Pergeseran paradigma ini membawa kita ke dua variasi penting dalam framework diffusion: Image-to-Image dan Inpainting.

Image-to-Image berfungsi sebagai pemetaan kondisional antar domain visual. Berbeda dengan text-to-image yang mengandalkan embedding linguistik, pendekatan ini menggunakan citra awal sebagai anchor struktural. Model belajar transformasi non-linear yang mempertahankan geometri atau layout dasar, sementara menyesuaikan fitur tingkat tinggi sesuai instruksi atau referensi gaya. Contoh implementasinya meliputi konversi sketsa menjadi fotorealistik, style transfer domain-specific, serta restorasi citra terdegradasi yang memerlukan preservasi komposisi asli.

Inpainting merupakan spesialisasi dari image-to-image yang beroperasi secara selektif. Sebuah mask, biasanya direpresentasikan sebagai tensor biner atau probabilistik, menandai region mana yang diperbolehkan dimodifikasi selama proses denoising. Jaringan hanya mengalokasikan kapasitas komputasinya untuk mengisi area yang tertutup mask, sambil menjaga kontinuitas tekstur, pencahayaan, dan konteks global di luar region tersebut. Aplikasi kritisnya mencakup penghapunan objek tak diinginkan, restorasi artefak kompresi, serta augmentasi data sintetis untuk melatih detector pada kasus rare events.

Implementasi prosedur ini pada ekosistem Diffusers mengikuti alur yang terstruktur dan dapat direproduksi:
1. Konversi citra input dan mask ke ruang laten menggunakan VAE encoder, mengurangi dimensi spasial sekaligus mengisolasi fitur semantik.
2. Gabungkan tensor citra laten dan mask sesuai skema masking yang didukung arsitektur, misalnya dengan zero-padding pada region yang dilindungi.
3. Jalankan loop denoising iteratif, namun batasi perhitungan gradien dan prediksi noise hanya pada koordinat yang aktif dalam mask.
4. Pertahankan nilai laten pada region non-mask tanpa modifikasi, sehingga informasi asli tetap utuh dan terhindar dari drift distribusi.

Pendekatan masking ini memberikan kontrol granular yang sangat dibutuhkan dalam riset computer vision modern, terutama ketika mengevaluasi stabilitas model terhadap perturbasi lokal atau ketika membangun pipeline generatif yang harus memenuhi constraint spasial ketat. Namun, kekuatan kondisi visual ini masih memerlukan mekanisme regulasi agar hasil sampling tidak menyimpang dari intent pengguna. Pembahasan mengenai bagaimana mengatur intensitas pengaruh kondisi tersebut terhadap trajectory denoising akan dilanjutkan pada slide berikutnya, dengan fokus pada teknik guidance yang menjadi standar de facto dalam praktik state-of-the-art.

---

## Slide 023 - Guidance: Mengarahkan Proses Denoising

### Narasi

Pada pembahasan sebelumnya mengenai image-to-image dan inpainting, kita telah melihat bagaimana masking dapat diterapkan pada ruang laten untuk menjaga region tertentu tetap utuh selama proses denoising. Namun, ketika model berjalan tanpa mekanisme pengarah tambahan, keluaran yang dihasilkan cenderung bersifat stokastik dan hanya mengikuti distribusi pelatihan secara umum. Tanpa kendali yang tepat, hasil generasi sering kali menyimpang dari spesifikasi tugas atau instruksi yang diharapkan. Oleh karena itu, diperlukan strategi *guidance* untuk memaksa proses denoising bergerak ke arah yang lebih spesifik dan terkontrol.

Secara konseptual, terdapat dua pendekatan utama dalam menerapkan *guidance*:

- **Classifier Guidance**: Memanfaatkan classifier terpisah yang dilatih khusus untuk membedakan kelas atau kategori target. Gradien dari classifier tersebut dihitung dan ditambahkan ke update rule denoising, sehingga model secara bertahap diarahkan menuju manifold data yang sesuai. Kelemahan utamanya terletak pada kebutuhan infrastruktur ganda, yang menambah beban komputasi dan potensi ketidakstabilan numerik.
- **Classifier-Free Guidance**: Melatih model secara simultan dalam dua skenario, yaitu dengan kondisi (*conditional*) dan tanpa kondisi (*unconditional*). Saat tahap inferensi, prediksi noise dari kedua skenario tersebut diinterpolasi secara linear. Pendekatan ini menghapus ketergantungan pada classifier eksternal, menyederhanakan pipeline, dan secara empiris memberikan hasil yang lebih konsisten serta mudah dioptimalkan.

Detail mekanistik dari kombinasi prediksi noise tersebut, termasuk formulasi matematis penggabungannya dan interpretasi parameter skala, akan kita bedah pada slide berikutnya. Dalam perspektif penelitian tingkat doktoral, penguasaan terhadap konsep *guidance* ini menjadi fondasi kritis karena menentukan bagaimana kita merancang eksperimen untuk mengevaluasi trade-off antara kesesuaian semantik, keragaman visual, dan stabilitas generasi pada arsitektur diffusi mutakhir.

---

## Slide 024 - Classifier-Free Guidance: Mekanisme

### Narasi

Pada slide ini, kita akan mengupas lebih dalam mekanisme di balik Classifier-Free Guidance yang sebelumnya telah dibahas secara konseptual pada pembahasan mengenai pengarahan proses denoising. Gagasan intinya terletak pada cara pelatihan model diffusion untuk memprediksi noise dalam dua skenario sekaligus. Pertama, model dilatih dengan kondisi tertentu, misalnya deskripsi teks atau mask segmentasi, yang direpresentasikan sebagai $\epsilon_\theta(x_t, t, c)$. Kedua, model juga dilatih tanpa kondisi apa pun, atau sering disebut empty condition, yang ditulis sebagai $\epsilon_\theta(x_t, t, \text{empty})$. Dengan melatih kedua mode ini secara bersamaan dalam satu arsitektur, kita tidak lagi memerlukan classifier terpisah seperti pada pendekatan klasik yang membutuhkan komputasi tambahan dan infrastruktur training yang lebih berat.

Saat proses sampling atau denoising berlangsung, prediksi noise total dihitung melalui kombinasi linear dari kedua output tersebut. Rumus utamanya adalah $\epsilon_{\text{total}} = \epsilon_{\text{uncond}} + w \cdot (\epsilon_{\text{cond}} - \epsilon_{\text{uncond}})$. Secara intuitif, bagian $(\epsilon_{\text{cond}} - \epsilon_{\text{uncond}})$ merepresentasikan gradien arah yang mengarah ke kondisi spesifik, sementara $w$ berfungsi sebagai pengali atau skala penguatan yang menentukan seberapa kuat model harus mematuhi prompt input selama inferensi.

Nilai parameter $w$ ini memiliki interpretasi yang sangat krusial bagi hasil generasi:
- Jika $w = 0$, model sepenuhnya mengandalkan prediksi tanpa kondisi, sehingga hasilnya cenderung acak dan tidak terikat pada prompt.
- Ketika $w > 0$, model mulai mengikuti arahan kondisi secara bertahap, menghasilkan gambar yang semakin sesuai dengan input.
- Jika nilai $w$ terlalu besar, kualitas visual bisa menurun drastis karena over-conditioning, serta keragaman sampel menjadi sangat terbatas. Fenomena ini sering terlihat sebagai artefak visual atau citra yang terlalu rigid dan kehilangan nuansa natural.

Dari perspektif penelitian tingkat doktoral, parameter $w$ bukan sekadar hyperparameter teknis, melainkan alat eksplorasi trade-off fundamental antara kesesuaian konten terhadap kondisi dan keragaman distribusi hasil. Mahasiswa diharapkan mampu melakukan analisis sensitivitas terhadap $w$ dalam eksperimen generatif, serta merumuskan strategi penyetelannya berdasarkan tujuan aplikasi, apakah mengutamakan fidelitas tinggi atau variasi kreatif yang luas. Penyesuaian nilai ini juga perlu dikaitkan dengan arsitektur backbone dan kapasitas model yang digunakan.

Mekanisme ini menjadi fondasi penting sebelum kita membahas bagaimana jadwal penambahan noise dan algoritma sampling berinteraksi dengannya. Pada slide berikutnya, kita akan menelaah peran noise schedule dan berbagai metode denoising sampling dalam menentukan stabilitas training serta efisiensi inference, termasuk perbandingan antara pendekatan bertahap seperti DDPM dan akselerasi seperti DDIM atau DPM-Solver.

---

## Slide 025 - Noise Schedule dan Sampling

### Narasi

Setelah membahas mekanisme *Classifier-Free Guidance* pada slide sebelumnya, kita kini beralih ke komponen fundamental lain yang menentukan kualitas dan stabilitas generasi citra: *Noise Schedule* dan proses *Sampling*. Dalam arsitektur *Diffusion Models*, jadwal penambahan noise selama proses forward tidak bersifat arbitrer, karena struktur temporal ini secara langsung membentuk distribusi intermediate yang harus dipelajari model saat melakukan inversi atau proses reverse.

Secara empiris, terdapat tiga pola jadwal yang paling sering diimplementasikan, yaitu *linear schedule*, *cosine schedule*, dan *sqrt schedule*. Pemilihan pola ini memengaruhi kecepatan transisi menuju keadaan noise penuh. Jadwal linear memberikan kenaikan noise yang konsisten per-timestep, cocok untuk baseline sederhana. Jadwal kosinus umumnya menghasilkan distribusi noise yang lebih seimbang di sepanjang rentang waktu, sehingga meningkatkan stabilitas pelatihan dan mengurangi artefak pada tahap akhir denoising. Sementara itu, jadwal akar kuadrat (*sqrt schedule*) sering dipakai untuk menyeimbangkan bobot antara fase awal dan akhir proses difusi, terutama ketika target prediksi adalah varians atau skor gradien.

Pada sisi inferensi atau *denoising sampling*, metode standar DDPM memerlukan serangkaian langkah bertahap yang panjang untuk mencapai konvergensi visual yang mulus. Untuk mengatasi beban komputasi, pendekatan seperti DDIM diperkenalkan dengan memanfaatkan sifat *non-Markovian* agar jumlah langkah dapat dikurangi drastis tanpa mengorbankan kualitas secara signifikan. Di ekosistem penelitian dan produksi mutakhir, scheduler turunan seperti Euler dan DPM-Solver semakin dominan karena kemampuannya menghasilkan sampel berkualitas tinggi dalam puluhan bahkan hanya satu hingga dua langkah, yang sangat relevan untuk implementasi *real-time* atau skalabilitas tinggi.

Implikasi metodologisnya cukup krusial bagi penelitian tingkat doktoral. Scheduler bukan sekadar detail teknis sekunder, melainkan *hyperparameter* strategis yang menentukan trade-off antara fidelitas konten, keragaman distribusi, dan efisiensi komputasi. Setiap eksperimen yang dilaporkan harus secara eksplisit mencantumkan jenis scheduler, jumlah langkah sampling, serta konfigurasi seed reproduktibilitas. Perubahan kecil pada parameter ini dapat mengubah interpretasi performa model secara signifikan, sehingga transparansi pelaporannya menjadi syarat utama validitas ilmiah.

Pilihan jadwal dan algoritma sampling ini akan menjadi fondasi penting ketika kita mengaitkan kembali konsep generatif dengan aplikasi konkret. Pada slide berikutnya, kita akan menelaah bagaimana mekanisme reverse diffusion yang telah diatur dengan schedule yang tepat dapat diintegrasikan ke dalam tugas *image restoration* dan augmentasi data, sekaligus mengevaluasi risiko halusinasi detail serta dampaknya terhadap generalisasi model deteksi dan segmentasi.

---

## Slide 026 - Integrasi dengan Pertemuan Sebelumnya: Restoration dan Augmentasi

### Narasi

Pada slide sebelumnya, kita telah membahas bagaimana noise schedule dan strategi sampling seperti DDPM atau DDIM memengaruhi stabilitas training serta kualitas hasil denoising. Pemahaman teknis ini menjadi fondasi penting ketika kita menggeser fokus dari mekanisme generatif murni menuju aplikasi praktis dalam pengolahan citra. Slide ini menghubungkan proses reverse diffusion yang telah dipelajari dengan dua bidang yang sudah kita bahas di pertemuan-pertemuan terdahulu, yaitu image restoration dan data augmentation berbasis model generatif.

Untuk image restoration, pendekatan diffusion memungkinkan kita merumuskan tugas seperti denoising, deblurring, maupun super-resolution sebagai proses reverse diffusion yang dikondisikan pada citra terdegradasi. Berbeda dengan baseline tradisional yang sering terjebak pada optimisasi metrik pixel-wise seperti MSE, model diffusion cenderung menghasilkan output dengan kualitas perseptual yang jauh lebih baik karena mampu menangkap distribusi data alami. Sebagai peneliti tingkat doktoral, kita harus kritis terhadap risiko inherentnya: detail halusinasi yang tidak sesuai dengan realitas objek asli dapat muncul, terutama ketika sinyal kondisional lemah atau kondisi degradasi berada di luar distribusi training. Evaluasi rigor diperlukan untuk memisahkan antara restorasi yang akurat dan artifak generatif.

Di sisi lain, diffusion-based augmentation menawarkan peluang strategis untuk memperbanyak dataset latih secara sintetis. Integrasi ini sangat relevan dengan pembahasan deteksi dan segmentasi pada pertemuan tujuh dan delapan, di mana kelimpahan data berlabel sering menjadi bottleneck utama. Dengan memanfaatkan prior struktur yang telah dipelajari model, kita dapat mensintesis variasi pose, iluminasi, atau tekstur yang sulit atau mahal untuk diperoleh di dunia nyata. Pertanyaan krusial yang perlu dijawab melalui eksperimen terkontrol adalah apakah augmentasi ini benar-benar meningkatkan generalisasi model downstream, atau justru memperkenalkan bias distribusi yang malah menurunkan performa pada data uji.

Transisi ini membawa kita secara alami ke diskusi yang lebih luas mengenai pemanfaatan data sintetis. Pada slide berikutnya, kita akan mengkaji peluang dan tantangan fundamental dari ekosistem synthetic data, mulai dari mitigasi kelangkaan data hingga isu etika, provenance, dan kebocoran data. Analisis kritis terhadap trade-off antara peningkatan akurasi versus overfitting terhadap distribusi sintetis akan menjadi kunci dalam merancang metodologi penelitian yang robust, reproducible, dan siap dikembangkan menjadi kontribusi ilmiah baru.

---

## Slide 027 - Synthetic Data: Peluang dan Tantangan

### Narasi

Pada pertemuan sebelumnya, kita telah membahas bagaimana proses reverse diffusion dapat dimanfaatkan untuk augmentasi data sintetis. Langkah ini memang menawarkan potensi signifikan dalam memperkaya dataset latih, terutama ketika kita berhadapan dengan keterbatasan data asli. Namun, penggunaan data sintetis bukan tanpa konsekuensi, sehingga perlu ditelaah secara kritis dari sisi peluang maupun tantangannya.

Dari sisi peluang, generasi citra sintetis melalui diffusion model memberikan beberapa keunggulan strategis:
- Mengatasi kelangkaan data berlabel yang sering menjadi hambatan utama dalam domain spesifik atau aplikasi kritis.
- Memungkinkan penciptaan variasi visual yang tidak terekam dalam dataset alami, sehingga meningkatkan keragaman representasi fitur.
- Secara substansial mengurangi beban biaya dan waktu untuk anotasi manual yang biasanya sangat intensif.
- Membuka ruang pengujian pada domain langka atau kondisi ekstrem yang sulit diakses di dunia nyata.

Di sisi lain, implementasi data sintetis menghadapi sejumlah tantangan teknis dan metodologis yang harus diwaspadai:
- Bias yang melekat pada data latih awal cenderung akan tercermin dan bahkan diperkuat pada data yang dihasilkan.
- Risiko memorization tetap ada, di mana model berpotensi menyalin sampel latih secara langsung alih-alih belajar distribusi umum.
- Hallucination atau detail fiktif yang tidak sesuai realitas dapat muncul, mengganggu validitas semantik citra.
- Provenance atau asal-usul data sintetis seringkali sulit dilacak, menimbulkan pertanyaan mengenai transparansi dan akuntabilitas.

Kondisi ini mendorong lahirnya beberapa pertanyaan penelitian krusial bagi riset tingkat doktor. Pertama, apakah peningkatan performa metrik akurasi benar-benar berasal dari kemampuan generalisasi model, atau justru akibat kebocoran data antara set latih dan uji? Kedua, bagaimana merancang kerangka evaluasi yang memastikan bahwa synthetic data digunakan secara etis, sah, dan tidak melanggar prinsip privasi atau hak cipta? Untuk menjawab tantangan kedua tersebut, khususnya terkait propagasi bias dan dampaknya, kita akan melanjutkan pembahasan ke slide berikutnya yang secara khusus mengupas sumber bias, dampak sosial-hukum, serta langkah mitigasinya dalam konteks generative vision.

---

## Slide 028 - Bias pada Generative Vision

### Narasi

Pada slide ini, kita membahas isu kritis yang sering terabaikan dalam pengembangan *Generative Vision*, yaitu bias sistemik. Setelah sebelumnya kita mengulas peluang dan tantangan penggunaan *synthetic data* pada slide 27, kini fokusnya bergeser ke bagaimana bias yang melekat dalam proses generasi citra dapat memperburuk ketidakadilan algoritmik.

Sumber bias dalam model generatif umumnya berakar dari tiga aspek utama:
- Ketidakseimbangan dataset pelatihan di mana beberapa kelas atau karakteristik demografis didominasi secara signifikan.
- Bias yang tertanam dalam representasi teks, misalnya asosiasi kata tertentu yang lebih kuat dengan kelompok tertentu akibat pola historis dalam korpus teks.
- Metrik evaluasi yang selama ini hanya berfokus pada kesesuaian visual terhadap *prompt*, tanpa mempertimbangkan dimensi keadilan atau inklusivitas.

Dampak dari bias ini bersifat kumulatif dan berbahaya. Model cenderung menghasilkan citra yang memperkuat stereotip sosial yang sudah ada. Ketika *synthetic data* hasil generasi ini digunakan untuk melatih model *downstream*, bias tersebut akan ikut tersalin dan diperkuat, menciptakan siklus ketidakadilan yang sulit diputus. Secara praktis, hal ini membuka risiko hukum dan etika yang serius, terutama jika model diterapkan dalam konteks sensitif seperti rekrutmen, pengawasan, atau layanan publik.

Untuk mengatasi masalah ini, diperlukan langkah mitigasi yang sistematis dan dapat ditelusuri:
- Lakukan audit rutin terhadap distribusi output model pada berbagai kelompok atau kategori spesifik.
- Dokumentasikan secara transparan komposisi dataset pelatihan, termasuk sumber data dan proporsi setiap kelas.
- Integrasikan temuan terkait bias ke dalam *model card* atau laporan teknis, sehingga pengguna akhir memiliki informasi yang cukup untuk menilai risiko penggunaan model.

Pembahasan mengenai bias ini akan berlanjut secara alami ke slide berikutnya, di mana kita akan menyoroti fenomena lain yang juga berkaitan dengan integritas data, yaitu *memorization*. Ketika model terlalu menghafal sampel pelatihan, privasi individu dapat terancam, yang semakin menegaskan pentingnya pendekatan etis, transparan, dan bertanggung jawab dalam seluruh rantai pengembangan *Generative Vision*.

---

## Slide 029 - Memorization: Model Menghafal Data Latih

### Narasi

Setelah menguraikan bagaimana bias sistemik dapat terbentuk dan terinternalisasi dalam representasi model pada slide sebelumnya, kita kini beralih ke mekanisme memorisasi atau penghafalan data latih. Pada level penelitian doktoral, memorisasi dalam model generatif tidak boleh disamakan dengan overfitting tradisional. Memorisasi terjadi ketika kapasitas ekspresif arsitektur—baik diffusion model maupun arsitektur berbasis transformer—menyimpan representasi eksak atau hampir eksak dari sampel tertentu ke dalam parameter bobot atau ruang laten. Kondisi ini cenderung muncul ketika dataset mengandung outlier, data langka, atau informasi sensitif yang tidak mengalami diversifikasi cukup selama proses augmentasi dan regularisasi.

Untuk mendeteksi memorisasi secara empiris, diperlukan pipeline evaluasi yang menggabungkan analisis kesamaan perseptual dan pengujian targeted generation. Langkah pertama adalah melakukan pencocokan sampel hasil generasi terhadap subset data pelatihan menggunakan metrik seperti LPIPS (Learned Perceptual Image Patch Similarity) atau membandingkan distribusi fitur lokal dengan referensi FID. Selanjutnya, peneliti dapat menjalankan stress test menggunakan prompt yang secara spesifik menargetkan entitas unik, watermark, atau metadata struktural. Konsistensi model dalam mereproduksi patch tekstur, pola anatomi, atau artefak kompresi yang identik dengan data latih menjadi indikator kuat bahwa model telah melakukan memorisasi aktif.

Implikasi etis dari fenomena ini bersifat fundamental bagi deployment model generatif skala besar. Ketika model publik mampu merekonstruksi kembali data pribadi atau konten berhak cipta, terjadi pelanggaran privasi yang sulit dilacak tanpa protokol audit transparan. Sebagai respons, praktik terbaik penelitian menuntut implementasi mekanisme debiasing, sanitasi metadata, dan teknik regularisasi seperti dropout latent atau gradient clipping khusus untuk membatasi kapasitas memorisasi. Tantangan ini juga membuka jembatan konseptual menuju slide berikutnya, yaitu hallucination pada citra sintetis, di mana kegagalan model tidak lagi terletak pada penghafalan data, melainkan pada kesalahan rekonstruksi konteks akibat inkonsistensi cross-attention atau keterbatasan decoder dalam menyelaraskan prior visual dengan constraint semantik.

---

## Slide 030 - Hallucination pada Citra Sintetis

### Narasi

Setelah membahas fenomena memorisasi di mana model cenderung menyalin secara ketat sampel dari data latih, kita kini beralih ke sisi lain dari artefak generatif, yaitu *hallucination* atau halusinasi pada citra sintetis. Jika memorisasi berkaitan dengan ketidakmampuan model untuk memisahkan salinan dari kreasi, halusinasi justru muncul ketika model menghasilkan detail visual yang sama sekali tidak ada dalam realitas atau kondisi input yang diberikan.

Secara definisi, halusinasi manifestasinya sangat beragam namun konsisten menunjukkan inkonsistensi semantik dan struktural. Contoh klasik meliputi teks pada poster atau rambu yang menjadi tidak terbaca, jumlah jari tangan yang tidak anatomis, hingga distorsi wajah yang sulit dikenali sebagai entitas manusia. Fenomena ini bukan sekadar kesalahan rendering, melainkan cerminan langsung dari bagaimana model memproses dan merekonstruksi informasi visual melalui mekanisme arsitekturalnya.

Penyebab halusinasi dapat ditelusuri ke tiga sumber utama. Pertama, inkonsistensi representasi dalam data latih menyebabkan model mempelajari pola yang ambigu atau saling bertentangan. Kedua, error pada mekanisme *cross-attention*, terutama pada arsitektur transformer yang mendasari banyak Diffusion Models modern, sering menghasilkan misalignment antara prompt tekstual dan fitur spasial yang dihasilkan. Ketiga, keterbatasan arsitektur decoder atau komponen rekonstruksi akhir juga berkontribusi signifikan terhadap hilangnya detail kritis selama proses denoising berlangsung.

Dampak praktis dari halusinasi sangat krusial, khususnya pada domain yang menuntut akurasi tinggi seperti pencitraan medis, analisis forensik, atau inspeksi industri presisi. Citra sintetis yang mengandung halusinasi tidak dapat dipercaya tanpa verifikasi tambahan, karena berpotensi mengarah pada kesimpulan diagnostik atau investigatif yang keliru. Oleh karena itu, evaluasi faktual dan struktural harus menjadi bagian wajib dalam pipeline validasi model, melampaui penggunaan metrik kualitas visual konvensional seperti FID atau LPIPS.

Kaitannya dengan materi sebelumnya, memorisasi dan halusinasi bersama-sama membentuk spektrum risiko teknis dan etis dalam generasi citra. Sementara memorisasi mengancam privasi melalui kebocoran data pelatihan, halusinasi mengancam integritas informasi melalui fabrikasi realitas. Untuk mengelola kedua risiko ini, diperlukan transparansi penuh mengenai bagaimana sampel dihasilkan, yang akan menjadi fokus pembahasan pada slide berikutnya mengenai pentingnya provenance data dan dokumentasi eksperimental.

---

## Slide 031 - Provenance Data dan Dokumentasi

### Narasi

Mengacu pada pembahasan sebelumnya mengenai *hallucination* pada citra sintetis, ketidakakuratan struktural atau detail artifaktual yang dihasilkan model bukanlah bug yang dapat diabaikan begitu saja. Fenomena ini justru menyoroti urgensi adanya jejak digital yang jelas atas setiap sampel yang dihasilkan. Dalam konteks penelitian tingkat doktor, kemampuan melacak asal-usul data atau *provenance* menjadi prasyarat mutlak. Tanpa dokumentasi yang transparan, klaim empiris mengenai performa generator atau kualitas output akan kehilangan validitas ilmiah karena tidak dapat diverifikasi atau direplikasi oleh komunitas akademik.

Untuk menjamin reproduktibilitas dan akuntabilitas metodologis, seluruh parameter eksperimen harus dicatat secara sistematis. Berikut adalah komponen kunci yang wajib didokumentasikan selama proses generasi:
- **Identitas Model & Pipeline:** Sebutkan arsitektur dasar (misalnya Stable Diffusion v2.1) beserta library dan versinya (contoh: Diffusers 0.27).
- **Prompt Teks:** Catat string input secara verbatim, mengingat sensitivitas model terhadap penyesuaian kata kunci dan struktur kalimat.
- **Seed Acak:** Tetapkan dan catat nilai *seed* awal agar proses stokastik dapat diulang persis sama.
- **Konfigurasi Sampling:** Dokumentasikan jenis *scheduler* (seperti DDIM), jumlah langkah (*steps*), dan parameter pengarah lainnya.
- **Lingkungan Eksekusi:** Cantumkan tanggal percobaan, versi runtime (Python 3.11, CUDA 12.1), serta spesifikasi perangkat keras GPU yang digunakan.

Catatan ini berfungsi sebagai fondasi kontrol eksperimen yang memungkinkan isolasi variabel penyebab bias atau artefak. Dengan *provenance* yang terstandarisasi, peneliti tidak hanya mampu melakukan analisis kasus secara mendalam, tetapi juga menyiapkan infrastruktur data yang siap diuji menggunakan metrik kuantitatif. Transisi ini akan mengarah langsung pada evaluasi objektif melalui ukuran statistik seperti FID, CLIP Score, dan LPIPS, yang akan kita bahas pada slide berikutnya untuk melengkapi inspeksi visual dengan bukti numerik yang robust dan bebas dari subjektivitas.

---

## Slide 032 - Evaluasi Kualitas: Metrik Kuantitatif

### Narasi

Setelah kita menekankan pentingnya dokumentasi lengkap dan pelacakan asal-usul data pada slide sebelumnya, langkah logis berikutnya adalah memastikan bahwa evaluasi hasil generasi citra tidak hanya mengandalkan penilaian subjektif atau inspeksi visual semata. Pada tingkat penelitian doktoral, klaim kualitas model harus didukung oleh metrik yang objektif, terukur, dan dapat direproduksi secara penuh.

Berikut adalah metrik kuantitatif yang paling sering dikutip dalam literatur generatif vision terkini:
- **FID (Fréchet Inception Distance):** Mengukur jarak statistik antara distribusi fitur dari sampel sintetis dan data asli. Nilai yang lebih rendah menunjukkan kesamaan distribusi yang lebih baik.
- **IS (Inception Score):** Mengevaluasi kombinasi kualitas dan keragaman gambar berdasarkan prediksi kelas dari model Inception.
- **CLIP Score:** Menilai keselarasan semantik antara teks prompt dan gambar yang dihasilkan menggunakan representasi ruang vektor bersama dari model CLIP.
- **LPIPS (Learned Perceptual Image Patch Similarity):** Mengukur kesamaan persepsi antar citra dengan memanfaatkan aktivasi jaringan saraf dalam, sehingga lebih sesuai dengan penilaian manusia dibanding metrik piksel tradisional.
- **KID (Kernel Inception Distance):** Merupakan variasi non-parametrik dari FID yang cenderung lebih stabil dan andal ketika ukuran sampel evaluasi relatif kecil.

Sebagai peneliti, kita harus kritis terhadap keterbatasan inherent dari setiap metrik tersebut. Semua metrik ini sangat bergantung pada ekstraktor fitur yang mendasarinya, seperti arsitektur Inception untuk FID, IS, dan KID, atau model CLIP untuk CLIP Score. Konsekuensinya, nilai metrik yang optimal tidak selalu berkorelasi langsung dengan estetika atau utilitas visual. Sebuah model dapat menghasilkan FID yang sangat rendah namun tetap mengandung artefak struktural atau ketidakwajaran semantik yang terlihat jelas. Oleh karena itu, evaluasi kuantitatif wajib dipadukan dengan inspeksi visual sistematis, analisis kasus per contoh, serta pertimbangan konteks aplikasi spesifik.

Pembahasan mengenai metrik kualitas ini menjadi fondasi penting sebelum kita menyoroti aspek evaluasi yang lebih spesifik. Pada slide berikutnya, kita akan mengurai bagaimana keragaman output dapat diukur melalui variasi parameter acak, serta bagaimana konsistensi terhadap prompt dapat dievaluasi secara lebih terstruktur melalui desain eksperimen yang ketat.

---

## Slide 033 - Evaluasi Keragaman dan Konsistensi

### Narasi

Setelah pembahasan pada slide sebelumnya mengenai metrik kuantitatif seperti FID, Inception Score, dan CLIP Score, kita perlu mengakui bahwa angka-angka statistik saja tidak cukup menangkap kompleksitas performa model generatif. Evaluasi yang komprehensif wajib memperluas cakupan ke dua dimensi fundamental: keragaman dan konsistensi.

Aspek keragaman menjawab pertanyaan seberapa bervariasi output yang dihasilkan ketika parameter input diubah, khususnya melalui variasi seed acak. Jika generasi dari berbagai seed menghasilkan citra yang hampir identik, ini mengindikasikan adanya mode collapse atau perilaku deterministik yang membatasi eksplorasi manifold data. Untuk mengukur keragaman secara objektif, peneliti dapat menghitung jarak antar sampel di ruang fitur, misalnya menggunakan embedding dari backbone model reference. Distribusi jarak yang lebar menandakan keragaman tinggi, sedangkan clustering yang padat menunjukkan keterbatasan kapasitas generasi model.

Di sisi lain, konsistensi menilai apakah output benar-benar selaras dengan spesifikasi input. Pertanyaan kritisnya meliputi apakah gaya visual, keberadaan objek, serta komposisi spasial dalam citra hasil generasi sesuai dengan prompt teks yang diberikan. Secara komputasional, kesesuaian ini dapat diukur menggunakan CLIP Score yang mengevaluasi cosine similarity antara embedding teks dan gambar. Namun, mengingat kompleksitas penalaran semantik dan struktural pada diffusion models, human evaluation tetap diperlukan sebagai validasi pelengkap, terutama untuk tugas yang menuntut akurasi kontekstual tinggi.

Dari perspektif desain eksperimen tingkat doktor, reproduktibilitas dan estimasi varians menjadi pondasi metodologi yang ketat. Variasikan seed, prompt, hingga guidance scale secara sistematis untuk mengamati sensitivitas model terhadap setiap hyperparameter. Dokumentasikan setiap konfigurasi eksperimen secara rinci agar proses replikasi dapat dilakukan oleh peneliti lain. Gunakan pengulangan statistik untuk menghitung mean dan varians dari setiap pengukuran, sehingga klaim kinerja model didukung oleh analisis dispersi yang robust, bukan sekadar contoh terpilih secara subjektif.

Pendekatan evaluasi keragaman dan konsistensi ini akan menjadi landasan transisi menuju slide berikutnya, yaitu evaluasi kualitatif terstruktur. Pada tahap selanjutnya, kita akan menerapkan rubrik penilaian berbasis kriteria untuk mengidentifikasi pola kegagalan secara granular, mengaitkannya kembali dengan parameter eksperimen, dan memperkuat argumen ilmiah untuk publikasi riset tingkat internasional.

---

## Slide 034 - Evaluasi Kualitatif Terstruktur

### Narasi

Setelah membahas metrik kuantitatif seperti keragaman dan konsistensi pada slide sebelumnya, langkah selanjutnya dalam evaluasi model generatif adalah menerapkan evaluasi kualitatif yang terstruktur. Pendekatan ini krusial karena metrik numerik semata tidak mampu menangkap nuansa visual, koherensi semantik, maupun artefak halus yang kerap muncul pada output *diffusion models*.

Rubrik pada slide ini membagi penilaian menjadi lima kriteria utama. Pertama, relevansi, yang mengukur kesesuaian antara konten visual dan instruksi *prompt* yang diberikan. Kedua, realisme, menilai apakah tekstur, pencahayaan, dan komposisi tampak alami atau justru terasa artifisial. Ketiga, keragaman, memastikan bahwa perubahan *seed* menghasilkan variasi output yang signifikan, bukan replikasi deterministik. Keempat, detail, memeriksa konsistensi elemen halus seperti lipatan, permukaan, atau anatomi objek. Kelima, artefak, yang mendeteksi distorsi visual seperti geometri yang tidak masuk akal, teks yang terbaca acak, atau ketidakmurnian batas objek.

Untuk melaksanakan evaluasi ini secara sistematis, ikuti empat langkah berikut:
1. Susunlah *grid* keluaran model di bawah berbagai kondisi eksperimen, misalnya dengan memvariasikan skala panduan (*guidance scale*) atau jumlah langkah inferensi.
2. Berikan skor pada setiap kriteria rubrik secara independen untuk meminimalkan bias subjektif evaluator.
3. Identifikasi pola kegagalan yang muncul berulang, seperti kesalahan proporsi tubuh manusia, distorsi latar belakang, atau inkonsistensi pencahayaan.
4. Hubungkan temuan tersebut kembali ke parameter eksperimen, sehingga Anda dapat melacak apakah masalah berasal dari arsitektur model, konfigurasi *scheduler*, atau keterbatasan ruang laten.

Evaluasi kualitatif terstruktur ini akan menjadi fondasi langsung untuk praktikum pada slide berikutnya. Di sana, Anda akan mengimplementasikan *pipeline* inference menggunakan pustaka Hugging Face Diffusers, menjalankan generasi gambar, memvariasikan *prompt* dan *seed*, serta menerapkan rubrik ini untuk mengevaluasi hasil secara empiris dan menyiapkan dasar bagi analisis kritis tingkat penelitian.

---

## Slide 035 - Praktikum: Pipeline Diffusers

### Narasi

Pada slide ini, kita beralih dari kerangka evaluasi kualitatif ke implementasi praktis menggunakan ekosistem Hugging Face Diffusers. Tujuan praktikum ini adalah menjalankan inferensi text-to-image secara langsung, kemudian melakukan variasi terkontrol pada prompt dan seed, serta menerapkan rubrik penilaian yang telah dibahas pada slide sebelumnya. Implementasi ini menjadi fondasi metodologis untuk menguji stabilitas model generatif sebelum masuk ke tahap analisis research gap atau optimasi arsitektur.

Berikut adalah kode pipeline dasar yang akan dijalankan:

```python
from diffusers import StableDiffusionPipeline
import torch

pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16
)
pipe = pipe.to("cuda")

prompt = "landscape photograph, mountain lake at sunrise"
image = pipe(
    prompt,
    num_inference_steps=30,
    guidance_scale=7.5,
    seed=42
).images[0]
image.save("output.png")
```

Kode ini mengimplementasikan alur standar ekstraksi latent space menuju pixel space melalui proses denoising iteratif. Beberapa poin teknis yang perlu diperhatikan dalam konteks penelitian tingkat doktoral meliputi:

- `torch_dtype=torch.float16`: Konversi mixed-precision mengurangi footprint memori GPU hingga ~50% tanpa degradasi perceptual yang signifikan, memungkinkan batch processing atau eksperimen berulang di lingkungan terbatas seperti Google Colab.
- `pipe.to("cuda")`: Memindahkan seluruh komponen U-Net, VAE, dan Text Encoder ke perangkat akselerasi hardware untuk mempercepat loop sampling.
- `num_inference_steps=30`: Menentukan jumlah iterasi denoising. Nilai ini merupakan trade-off antara konvergensi distribusi latens dan biaya komputasi.
- `guidance_scale=7.5`: Koefisien classifier-free guidance yang mengatur kekuatan penuntun prompt teks terhadap trajectory sampling. Nilai terlalu tinggi dapat menghasilkan artefak over-saturated, sedangkan nilai rendah cenderung menghasilkan output ambigu.
- `seed=42`: Penetapan state generator acak menjamin reproduktibilitas hasil, syarat mutlak untuk validasi eksperimental dan pelaporan metrik yang dapat direplikasi oleh reviewer jurnal internasional.

Hasil inferensi berupa tensor gambar dikonversi ke format PIL lalu disimpan sebagai `"output.png"`. Pipeline ini menyiapkan baseline yang konsisten. Sesuai protokol yang akan kita bahas pada slide berikutnya, kita akan melakukan sistematisme variasi terhadap tiga variabel independen: prompt deskriptif, seed awal, dan guidance scale. Setiap konfigurasi akan dicatat bersama timestamp inferensi dan skor kualitatif per kriteria, sehingga pola kegagalan model maupun sensitivitas hyperparameter dapat dipetakan secara empiris dan siap dikaitkan dengan mekanisme attention atau noise scheduling dalam arsitektur diffusion.

---

## Slide 036 - Praktikum: Variasi Prompt dan Seed

### Narasi

Pada slide ini, kita akan membahas protokol eksperimen sistematis untuk menguji stabilitas dan responsivitas model difusi terhadap perubahan parameter input. Setelah sebelumnya berhasil menjalankan pipeline dasar text-to-image menggunakan Hugging Face Diffusers, langkah selanjutnya adalah melakukan variasi yang terkontrol untuk memahami bagaimana model merespons perubahan kecil pada prompt maupun seed.

Tabel kondisi eksperimen di atas menyajikan lima skenario pengujian yang dirancang dengan prinsip kontrol variabel. Kondisi satu dan dua mempertahankan prompt yang sama tetapi mengubah seed dari 1 menjadi 2, sambil menjaga guidance scale tetap di 7.5. Ini bertujuan untuk mengamati variasi stokastik murni yang dihasilkan oleh noise initialization tanpa mengubah arsitektur atau penuntun semantik. 

Selanjutnya, kondisi tiga dan empat menguji sensitivitas model terhadap perubahan konteks prompt, yaitu pergantian latar belakang dari pegunungan ke kota, serta peningkatan guidance scale menjadi 9.0 pada kondisi empat. Peningkatan guidance scale ini akan memperkuat pengaruh prompt teks terhadap proses denoising, sehingga hasil visual cenderung lebih ketat mengikuti deskripsi teks namun berisiko kehilangan koherensi struktural jika nilainya terlalu tinggi. Kondisi lima memperkenalkan gaya visual spesifik, yaitu isometrik, dengan seed berbeda dan guidance scale diturunkan menjadi 5.0 untuk memberikan ruang lebih besar bagi interpretasi model.

Untuk setiap kondisi, pencatatan output harus dilakukan secara terstruktur agar dapat dianalisis secara kritis. Data yang wajib dicatat meliputi:
- Gambar hasil akhir beserta metadata lengkap.
- Parameter prompt dan seed yang digunakan.
- Durasi waktu inferensi untuk evaluasi efisiensi komputasi.
- Skor kualitatif per kriteria, seperti kesesuaian semantik, kualitas tekstur, dan konsistensi komposisi.
- Catatan kegagalan atau artifacts yang muncul, misalnya distorsi geometri, inkonsistensi pencahayaan, atau degradasi detail halus.

Dokumentasi kegagalan ini sangat penting pada jenjang doktoral, karena pola error yang konsisten sering kali mengungkap keterbatasan representasi latent space atau bias dalam training data. Hasil dari protokol ini akan menjadi baseline empiris sebelum Anda beralih ke manipulasi citra eksisting. Pada slide berikutnya, kita akan menerapkan konsep conditioning yang sama pada pipeline image-to-image dan inpainting, di mana pemahaman mendalam terhadap kontrol noise dan guidance scale menjadi kunci keberhasilan transformasi visual.

---

## Slide 037 - Praktikum: Image-to-Image dan Inpainting

### Narasi

Pada slide ini, kita beralih dari eksperimen variasi parameter teks ke teknik generasi kondisional yang lebih kompleks, yaitu Image-to-Image dan Inpainting. Setelah sebelumnya menguji pengaruh seed dan guidance scale pada generasi teks-ke-citra, langkah logis berikutnya adalah memberikan kontrol spasial atau struktural melalui citra referensi sebagai kondisi tambahan.

Untuk Image-to-Image, kita memanfaatkan `StableDiffusionImg2ImgPipeline` dari library Diffusers. Perhatikan implementasi kode berikut:
```python
from diffusers import StableDiffusionImg2ImgPipeline
import requests
from PIL import Image

pipe = StableDiffusionImg2ImgPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5"
).to("cuda")

img = Image.open("sketch.png").convert("RGB")
result = pipe(
    prompt="foto realistis dari sketsa",
    image=img,
    strength=0.6,
    guidance_scale=7.5
).images[0]
result.save("output_img2img.png")
```
Kode ini menginisialisasi pipeline dengan model Stable Diffusion v1.5 yang di-deploy ke GPU. Citra input, dalam contoh ini berupa sketsa (`sketch.png`), dikonversi ke mode RGB sebelum diproses. Parameter kunci di sini adalah `strength=0.6`, yang mengatur seberapa jauh proses denoising awal akan meninggalkan struktur citra referensi. Nilai 0.6 menunjukkan keseimbangan optimal antara mempertahankan garis dasar sketsa asli dan menerapkan transformasi tekstur realistis sesuai prompt. `Guidance_scale` tetap diset di 7.5 untuk menjaga koherensi semantik dengan deskripsi teks.

Selain Image-to-Image, Diffusers menyediakan dukungan penuh untuk Inpainting. Teknik ini sangat krusial dalam aplikasi restorasi citra, komposisi visual, atau modifikasi lokal yang presisi. Untuk menjalankannya, Anda perlu menggunakan `StableDiffusionInpaintPipeline`. Berbeda dengan pendekatan sebelumnya, pipeline ini memerlukan dua input utama: citra sumber lengkap dan mask biner yang menandai wilayah target yang ingin digenerate ulang. Penyesuaian parameter `strength` kembali menjadi faktor penentu; nilai rendah akan menghasilkan perubahan halus di area masked, sedangkan nilai tinggi memungkinkan rekonstruksi konten yang lebih radikal tanpa terikat pada konteks sekitarnya.

Implementasi praktis kedua metode ini melengkapi protokol eksperimen pada slide sebelumnya, di mana kita telah mencatat waktu inferensi dan evaluasi kualitatif per kriteria. Dengan menguasai img2img dan inpainting, mahasiswa tidak hanya memahami manipulasi parameter, tetapi juga bagaimana arsitektur diffusion model merespons kondisi tambahan selain teks, serta bagaimana trade-off antara fidelitas struktur dan kreativitas generatif dapat dikontrol secara eksperimental.

Transisi menuju analisis akademis akan segera dilakukan pada slide berikutnya. Setelah menyelesaikan praktikum teknis ini, kita akan mengevaluasi landasan teoretis dan metodologis dari paper-paper seminal yang mempopulerkan teknik-teknik tersebut, serta mengidentifikasi celah penelitian yang masih terbuka di tingkat doktoral, termasuk aspek evaluasi synthetic data dan risiko memorization pada model diffusion.

---

## Slide 038 - Critical Review Paper: Pertanyaan Penuntun

### Narasi

Setelah sebelumnya melakukan implementasi praktis image-to-image dan inpainting menggunakan pipeline Diffusers, kita kini beralih ke tahap analisis kritis yang menjadi ciri khas pembelajaran tingkat doktoral. Slide ini menyajikan kerangka terstruktur untuk membedah literatur ilmiah di bidang generative vision berbasis diffusion model, memastikan setiap review dilakukan secara sistematis dan mendalam.

Struktur critical review yang ditampilkan terdiri dari enam pertanyaan penuntun yang saling berkesinambangan. Pertama, identifikasi problem statement atau celah pengetahuan spesifik yang ingin diatasi penulis. Kedua, klarifikasi kontribusi utama, apakah berupa modifikasi arsitektur, strategi training, atau insight teoretis baru. Ketiga, bedakan secara eksplisit bagaimana pendekatan yang diusulkan berbeda dari metode baseline atau state-of-the-art sebelumnya. Keempat, verifikasi kecukupan eksperimen terhadap klaim yang diajukan, meliputi pemilihan metrik, kompleksitas dataset, dan konsistensi ablation study. Kelima, kaji keterbatasan inheren serta risiko potensial seperti ketidakstabilan sampling, bias distribusi, atau kegagalan generalisasi ke domain lain. Terakhir, rumuskan pertanyaan penelitian lanjutan yang membuka jalur eksplorasi empiris maupun teoretis lebih lanjut.

Untuk mengoperasionalkan kerangka ini, empat contoh kasus relevan dalam konteks diffusion model dapat dijadikan studi awal. Analisis terhadap Latent Diffusion Models (LDM) atau Stable Diffusion perlu menyoroti mekanisme latent space compression dan dampaknya terhadap fidelity visual. Perbandingan fundamental antara GAN dan diffusion model harus mengevaluasi trade-off antara mode collapse versus sampling latency serta stabilitas training. Evaluasi synthetic data yang dihasilkan diffusion model pada benchmark computer vision standar memerlukan pemeriksaan ketat terkait domain shift dan degradasi performa pada downstream tasks. Selain itu, risiko memorization pada model diffusion harus dikaji secara kritis, mengingat kemampuan model menghafal sampel pelatihan dapat mengaburkan batas antara interpolasi distribusi dan replikasi konten, yang berdampak langsung pada keandalan dan etika hasil sintesis.

Kerangka review ini akan menjadi landasan langsung ketika kita memasuki diskusi etika dan reproducibility pada slide berikutnya. Dengan menerapkan keenam pertanyaan penuntun secara disiplin, mahasiswa tidak hanya menguasai mekanika teknis model, tetapi juga terlatih mengidentifikasi research gap, merumuskan hipotesis yang tajam, dan mendesain eksperimen yang rigor untuk kontribusi disertasi yang memiliki novelty dan dampak ilmiah nyata.

---

## Slide 039 - Diskusi Etika dan Reproducibility

### Narasi

Setelah menyelesaikan struktur pertanyaan penuntun untuk tinjauan kritis paper pada slide sebelumnya, kini kita beralih ke dua pilar yang menentukan kredibilitas penelitian tingkat doktoral: etika dan reproduktibilitas. Keduanya bukan sekadar checklist administratif, melainkan komponen metodologis yang langsung memengaruhi validitas klaim ilmiah dan posisi kontribusi Anda terhadap state-of-the-art.

**Etika** dalam pengembangan model generatif saat ini menuntut perhatian serius pada empat dimensi utama:
- **Hak cipta data latih:** Dataset skala besar sering kali mengandung karya kreatif tanpa lisensi komersial atau persetujuan eksplisit, memicu debat berkelanjutan tentang fair use, transformasi data, dan tanggung jawab hukum pengembang.
- **Konten yang menyesatkan (deepfake):** Kemampuan sintesis gambar realistis berisiko disalahgunakan untuk misinformasi, manipulasi narasi publik, atau penipuan digital yang sulit dideteksi secara manual.
- **Penggunaan citra tokoh publik:** Ekstraksi dan modifikasi wajah figur publik tanpa izin melanggar prinsip otonomi dan berpotensi menimbulkan sengketa privasi serta reputasi.
- **Dampak sosial dari citra sintetis:** Distribusi masiv visual buatan dapat mengaburkan batas antara fakta dan fiksi, mengubah cara masyarakat mengonsumsi informasi, dan menggeser norma estetika maupun dokumentasi sejarah.

Paralel dengan isu etika, **reproduktibilitas** menjadi ukuran objektivitas eksperimen yang wajib diinternalisasi sejak fase desain penelitian. Standar yang harus diterapkan meliputi:
- **Simpan konfigurasi lengkap:** Dokumentasikan semua hyperparameter, arsitektur backbone, scheduler, loss function, dan pipeline augmentasi secara terstruktur menggunakan file YAML atau config manager.
- **Gunakan random seed:** Kunci nilai acak pada generator noise, shuffling data, dropout, dan inisialisasi bobot untuk menghilangkan variabilitas non-deterministik antar eksekusi.
- **Catat versi library dan hardware:** Tandai secara eksplisit versi PyTorch, Diffusers, torchvision, CUDA, driver GPU, serta spesifikasi mesin training agar lingkungan komputasi dapat direkonstruksi secara presisi.
- **Bagikan script dan hasil sebagai artefak:** Publikasikan kode bersih, notebook Jupyter/Colab, checkpoint model, log training, dan metadata evaluasi di repositori terbuka untuk memudahkan audit independen oleh reviewer atau peneliti lain.

Target utamanya adalah memastikan setiap eksperimen dapat diulang secara identik oleh mahasiswa lain atau reviewer jurnal. Ketika reproduktibilitas terjamin, diskusi teknis tidak lagi terjebak pada spekulasi implementasi, melainkan fokus pada analisis kontributif, isolasi variabel, dan perbaikan metodologis. Landasan etika dan reproduktibilitas ini akan menjadi pondasi penting ketika kita melanjutkan pembahasan ke integrasi model difusi dalam konteks 3D vision, serta strategi perumusan pertanyaan riset disertasi yang konkret pada slide berikutnya.

---

## Slide 040 - Hubungan dengan Pertemuan 10 dan Penelitian Disertasi

### Narasi

Setelah membahas aspek etika dan reproducibility pada slide sebelumnya, kita sekarang menempatkan diffusion models dalam konteks perkembangan riset lanjutan dan kesiapan penyusunan disertasi. Fokus utama slide ini adalah menjembatani pemahaman teknis dengan strategi penelitian yang terstruktur, reproducible, dan memiliki kontribusi ilmiah yang jelas.

Diffusion models kini telah melampaui batas domain 2D tradisional. Teknik ini dapat diadaptasi untuk novel view synthesis, di mana kondisi input berupa pose kamera atau representasi geometri 3D digunakan sebagai conditioning signal. Pendekatan ini menjadi jembatan konseptual yang sangat relevan untuk pembahasan Neural Radiance Fields (NeRF) pada pertemuan 10. Dengan memahami bagaimana noise ditambahkan dan dikondisikan pada ruang 3D, Anda dapat melihat kesinambungan antara generative vision dan rekonstruksi scene berbasis fisika cahaya.

Dari perspektif peluang riset, bidang generative vision masih menyimpan beberapa celah yang belum tereksplorasi secara optimal:
- Penggunaan synthetic data untuk domain langka atau skenario dengan keterbatasan pengumpulan data nyata.
- Pengembangan mekanisme conditioning yang lebih presisi, modular, dan terkontrol secara semantik.
- Perancangan metrik evaluasi yang melampaui FID, mengingat metrik statistik tersebut sering kali gagal menangkap koherensi struktural, konsistensi temporal, dan kualitas semantik.
- Strategi mitigasi terhadap bias distribusi training dan fenomena memorization pada model skala besar.

Untuk persiapan disertasi, penting bagi Anda untuk merumuskan pertanyaan riset yang konkret dan dapat diuji secara empiris. Desain eksperimen harus mampu memisahkan kontribusi teknis murni dari pengaruh konfigurasi hyperparameter, arsitektur backbone, atau infrastruktur komputasi. Selain itu, dokumentasi setiap keputusan metodologis—mulai dari pipeline preprocessing, strategi sampling, hingga protokol validasi—harus dicatat secara sistematis agar hasil penelitian dapat diverifikasi, direplikasi, dan dikembangkan oleh komunitas akademik.

Refleksi ini akan memperkuat fondasi Anda sebelum masuk ke rangkuman konsep kunci pada slide berikutnya, yang akan menyoroti kembali alur proses difusi bertahap, peran VAE dalam LDM, serta tantangan evaluasi dan risiko etis yang telah kita bahas bersama sepanjang pertemuan ini.

---

## Slide 041 - Rangkuman Konsep Kunci

### Narasi

Setelah menelaah potensi riset di bidang generative vision serta kaitannya dengan topik 3D vision pada pertemuan berikutnya, kita kini merangkum fondasi konseptual yang telah dibahas. Slide ini berfungsi sebagai peta konsep untuk memastikan seluruh elemen kunci dari arsitektur diffusion model tersimpan secara terstruktur dalam kerangka berpikir penelitian Anda.

Secara fundamental, diffusion model beroperasi melalui dua fase utama: proses penambahan noise secara bertahap ke citra asli, diikuti oleh proses denoising terbalik yang dipelajari oleh jaringan saraf. Untuk mengoptimalkan biaya komputasi dan mempertahankan kualitas representasi, Latent Diffusion Models atau LDM memindahkan proses difusi ini ke ruang laten yang dikompresi oleh Variational Autoencoder. Output yang dihasilkan kemudian dikendalikan melalui mekanisme conditioning dan guidance, yang memungkinkan penyesuaian arah generasi sesuai spesifikasi teknis yang diinginkan.

Tiga aplikasi inti yang menjadi standar eksplorasi saat ini meliputi text-to-image generation, image-to-image translation, dan inpainting. Namun, validitas sebuah model generatif tidak dapat diukur hanya dari satu metrik numerik seperti FID atau Inception Score. Evaluasi harus dirancang secara multidimensi, mencakup konsistensi struktural, koherensi semantik, dan stabilitas distribusi. Peneliti juga wajib mengantisipasi risiko inheren, yaitu bias data pelatihan, memorization konten sensitif, fenomena hallucination visual, serta tantangan verifikasi provenance atau asal-usul konten sintetis.

Pada jenjang doktoral, synthetic data tidak boleh diasumsikan sebagai substitusi langsung data nyata tanpa proses audit ketat terhadap representasi dan bias sistemiknya. Setiap eksperimen yang Anda rancang harus memenuhi standar reproducibility, mulai dari dokumentasi hyperparameter hingga penggunaan seed deterministik. Persiapan tugas praktikum dan analisis pada slide berikutnya akan menuntut penerapan prinsip evaluasi yang hati-hati ini secara empiris, sehingga Anda mampu mengidentifikasi batas kemampuan model dan merumuskan kontribusi ilmiah yang solid untuk tahap penyusunan disertasi Anda.

---

## Slide 042 - Tugas dan Bukti Belajar

### Narasi

Setelah merangkum peta konsep difusi, mekanisme latent diffusion, peran conditioning dan guidance, serta risiko etika dan teknis pada slide sebelumnya, kini kita masuk ke fase implementasi yang terukur. Eksperimen pada tingkat doktoral tidak hanya menuntut keberhasilan teknis, tetapi juga disiplin dalam dokumentasi dan analisis kritis.

Untuk tugas praktikum, jalankan pipeline Diffusers pada satu model Stable Diffusion yang telah dipilih. Lakukan minimal lima variasi prompt dan tiga variasi seed berbeda. Setiap kombinasi harus menghasilkan gambar yang disimpan dalam katalog folder yang terstruktur secara logis, misalnya mengelompokkan hasil berdasarkan parameter sampling atau kategori tema prompt. Penataan file ini merupakan standar dasar untuk memastikan reproduktibilitas eksperimen di kemudian hari.

Pada bagian tugas analisis, lakukan evaluasi kualitatif terhadap setiap output menggunakan rubrik yang telah ditetapkan. Amati empat dimensi utama: konsistensi visual antar generasi, keragaman output meskipun terdapat kemiripan prompt, tingkat kesesuaian antara output visual dengan instruksi teks, serta kehadiran artefak seperti distorsi struktur, duplikasi objek, atau noise yang tidak konsisten. Dari temuan tersebut, susun kesimpulan objektif mengenai keterbatasan arsitektur model dalam menangani kompleksitas semantik atau detail halus.

Sebagai bukti belajar, kumpulkan seluruh artifact eksperimen secara utuh. Ini mencakup notebook Jupyter yang memuat kode, penjelasan langkah, dan visualisasi intermediate; katalog gambar hasil generasi; serta laporan evaluasi kualitatif. Pastikan Anda menyertakan tabel konfigurasi yang mendetail, meliputi nilai seed, scheduler, jumlah step, guidance scale, dan resolusi input. Transparansi parameter ini memungkinkan reviewer menelusuri sumber variasi hasil tanpa bergantung pada eksekusi ulang.

Kelengkapan catatan pada tahap ini akan menjadi fondasi langsung bagi checklist pengumpulan laporan pada slide berikutnya. Pastikan tidak ada prompt, seed, atau hyperparameter yang terlewat, karena ketidakhadiran informasi tersebut akan melemahkan validitas klaim analisis Anda. Laporan yang disusun dengan baik harus berdiri sendiri, artinya pembaca dapat memahami alur eksperimen, kriteria evaluasi, dan batasan model hanya melalui dokumen yang Anda serahkan.

---

## Slide 043 - Checklist Sebelum Mengumpulkan Laporan

### Narasi

Setelah menyelesaikan seluruh eksekusi pipeline dan analisis kualitatif pada tugas sebelumnya, langkah selanjutnya adalah memastikan konsistensi dan kelengkapan hasil kerja sebelum laporan diserahkan. Pada tingkat doktoral, reproduktibilitas eksperimen dan transparansi parameter bukan sekadar formalitas administratif, melainkan fondasi metodologis yang menentukan kredibilitas temuan ilmiah. Checklist ini berfungsi sebagai instrumen verifikasi mandiri untuk menjamin bahwa setiap aspek teknis, dokumentasi, dan evaluatif telah terpenuhi sesuai standar akademik yang ketat.

Mari kita tinjau enam poin kunci dalam tabel ini secara berurutan:
1. Konfirmasi bahwa pipeline berhasil dijalankan tanpa error laten atau warning kritis yang mengaburkan hasil.
2. Catat secara eksplisit semua kombinasi prompt dan nilai seed yang digunakan, karena kontrol terhadap variabilitas stokastik esensial untuk analisis reproduktibilitas.
3. Simpan seluruh output visual dalam format PNG atau JPG agar kompatibel dengan sistem pengumpulan dokumen dan mudah diverifikasi secara manual.
4. Isi rubrik evaluasi dengan kriteria yang terukur, hindari penilaian berbasis kesan subjektif semata.
5. Tuliskan pola kegagalan model yang teramati, misalnya artefak struktural, distorsi anatomi, atau ketidaksesuaian semantik, karena observasi kritis terhadap kegagalan sering kali menjadi titik awal formulasi research gap.
6. Susun refleksi tertulis mengenai keterbatasan arsitektur diffusion model yang Anda uji, mencakup aspek komputasi, bias distribusi training data, maupun batasan resolusi dan fidelitas teks-ke-citra.

Perhatikan juga catatan penutup pada slide ini. Pastikan tidak ada prompt, hyperparameter, atau konfigurasi lingkungan yang terlewat dari dokumentasi. Laporan yang Anda kumpulkan harus bersifat self-contained, artinya pembaca atau evaluator dapat memahami alur eksperimen, interpretasi hasil, dan kesimpulan yang Anda tarik tanpa perlu menjalankan ulang notebook secara langsung. Kemandirian naskah ini mencerminkan kematangan dalam menyusun laporan penelitian yang jelas, terstruktur, dan siap dipertanggungjawabkan secara akademis. Apabila selama proses verifikasi Anda menemukan anomali atau memerlukan pendalaman mekanisme perbaikan model, referensi dan bacaan lanjutan pada slide berikutnya akan menyediakan panduan literatur serta dokumentasi teknis yang relevan untuk memperkuat analisis dan diskusi Anda.

---

## Slide 044 - Referensi dan Bacaan Lanjutan

### Narasi

Pada slide ini, kita mengakhiri modul Generative Vision dengan Diffusion Models melalui kurasi referensi dan bacaan lanjutan yang dirancang khusus untuk mendukung penulisan proposal disertasi dan eksplorasi metodologis tingkat doktor.

Untuk fondasi teoretis, Anda wajib mendalami empat paper inti berikut:
- Ho et al., *Denoising Diffusion Probabilistic Models*: landasan matematis proses forward dan reverse diffusion serta formulasi variational lower bound.
- Rombach et al., *High-Resolution Image Synthesis with Latent Diffusion Models*: pengenalan ruang laten dan cross-attention mechanism yang menjadi arsitektur standar industri saat ini.
- Goodfellow et al., *Generative Adversarial Nets*: kerangka kerja komparatif untuk memahami evolusi paradigma generatif dari adversarial training menuju score matching.
- Song et al., *Score-Based Generative Modeling through Stochastic Differential Equations*: pendekatan opsional yang mengaitkan diffusion dengan teori gradien skor dan SDE, sangat relevan jika Anda meneliti stabilitas sampling atau kontrol dinamika noise.

Dari perspektif implementasi, dokumentasi resmi menjadi rujukan teknis utama:
- Hugging Face Diffusers: pelajari struktur pipeline, manajemen scheduler, dan cara melakukan custom hooking pada setiap timestep.
- Model cards dan dokumentasi dataset: analisis distribusi training data, metadata, serta batasan etis dan representasional yang sering terabaikan dalam reproduksi eksperimen.
- Panduan evaluasi FID dan CLIP Score: pahami secara kritis kelemahan masing-masing metrik terhadap artefak visual, mode collapse, dan konsistensi semantik, sehingga Anda dapat merancang metrik evaluasi hibrida yang lebih robust.

Sebagai arahan riset lanjutan, lakukan langkah-langkah berikut untuk membangun novelty:
- Baca kode sumber Diffusers secara langsung untuk memetakan hubungan antara loss weighting, noise schedule, dan inference step dalam PyTorch.
- Lakukan benchmarking komparatif antara DDPM dan DDIM, fokus pada trade-off antara kualitas sampel, waktu komputasi, dan deterministik sampling.
- Telusuri literatur terkini mengenai synthetic data generation dan bias algoritmik, karena isu fairness, keberagaman representasi, dan mitigasi bias dalam model foundation masih menjadi research gap yang sangat potensial untuk kontribusi ilmiah tingkat disertasi.

Dengan merujuk pada checklist pengumpulan laporan dari slide sebelumnya, Anda kini memiliki kerangka lengkap untuk mengevaluasi hasil eksperimen generatif secara sistematis. Setelah sesi ini ditutup, kita akan beralih ke pertemuan berikutnya yang membahas 3D Vision, Multi-View Geometry, dan Neural Rendering, yang akan memperluas cakupan pengolahan citra dari representasi 2D ke pemodelan spasial tiga dimensi berbasis geometri proyektif dan rendering diferensial.

---

## Slide 045 - TERIMA KASIH

### Narasi

Kita menutup pertemuan ini dengan sintesis atas seluruh bahasan mengenai *Generative Vision* berbasis *Diffusion Models*. Fokus utama telah tertuju pada mekanisme *reverse diffusion process*, optimasi *sampling trajectories*, serta efisiensi komputasi melalui pendekatan *latent space*. Pemahaman terhadap komponen-komponen ini menjadi prasyarat fundamental untuk melakukan kritik metodologis terhadap arsitektur generatif terkini dan mengidentifikasi celah penelitian yang masih terbuka.

Silakan tinjau kembali referensi inti yang telah diuraikan pada slide sebelumnya. Literatur dari Ho et al., Rombach et al., Goodfellow et al., serta Song et al. membentuk pilar teoretis yang harus Anda kuasai untuk membedakan kontribusi ilmiah masing-masing pendekatan. Implementasi praktis dapat dipelajari langsung melalui kode sumber *Hugging Face Diffusers*. Lakukan eksperimen komparatif antara *DDPM* dan *DDIM*, serta analisis dampak *synthetic data bias* terhadap kinerja model downstream. Evaluasi kuantitatif wajib mencakup metrik *FID* dan *CLIP Score* sebagai standar benchmark objektif dalam menilai kualitas dan diversitas sampel generatif.

Pertemuan berikutnya akan mengalihkan fokus kajian ke domain spasial dan rekonstruksi visual. Topik *3D Vision, Multi-View Geometry, dan Neural Rendering* akan membahas transformasi data citra multi-perspektif menjadi representasi volumetrik atau parametrik. Anda akan mendalami prinsip *epipolar geometry*, estimasi *camera pose*, serta revolusi rendering berbasis jaringan saraf seperti *NeRF* dan *3D Gaussian Splatting*. Persiapkan dasar matematika linear, kalkulus multivariat, dan pemahaman awal tentang *implicit neural representations* untuk mengikuti diskusi tingkat doktoral ini.

Terima kasih atas kontribusi intelektual dan diskusi kritis selama sesi berlangsung. Catatan eksperimen dan draf literatur review dapat Anda kumpulkan sebagai bahan refleksi awal sebelum memasuki fase perumusan masalah penelitian mandiri.
