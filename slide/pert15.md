# Slide 00 - Cover

EF256129 - TD PCD
Pertemuan 15
# Seminar Proposal Awal dan Evaluasi State-of-the-Art

Dr. Darlis Herumurti
Departemen Teknik Informatika - ITS

---

# Slide 01 - Kedudukan Pertemuan dan Tujuan Pembelajaran

## Posisi dalam Rangkaian Perkuliahan

- Pertemuan 1–11: membangun pemahaman teknis tentang state-of-the-art Pengolahan Citra Digital.
- Pertemuan 12–14: membangun keterampilan eksperimen, perumusan masalah, dan metodologi disertasi.
- Pertemuan 15: menguji kemampuan mengomunikasikan proposal dan mengevaluasi posisi terhadap state-of-the-art secara terbuka.
- Pertemuan 16: mengonsolidasikan masukan menjadi proposal akhir dan rencana publikasi.

## Tujuan Pembelajaran

Setelah pertemuan ini, mahasiswa diharapkan mampu:

- menyusun presentasi proposal awal yang ringkas, defensible, dan berbasis bukti;
- mengevaluasi state-of-the-art secara kritis sebagai landasan positioning;
- menginterpretasikan hasil eksperimen awal secara jujur dan terkendali;
- menerima serta mencatat masukan penguji untuk revisi prioritas.

---

# Slide 02 - Agenda Seminar dan Luaran yang Diharapkan

## Agenda

- Presentasi proposal individual, sekitar 15–20 menit.
- Demonstrasi hasil eksperimen awal atau notebook.
- Tanya jawab doktoral oleh penguji dan audiens.
- Peer review formal menggunakan rubrik.

## Tiga Luaran pada Akhir Pertemuan

| Luaran | Bentuk | Fungsi |
|---|---|---|
| Presentasi proposal awal | Slide dan demo | Mengomunikasikan ide dan bukti awal |
| Naskah ringkas proposal | 2–4 halaman | Mendokumentasikan argumen secara utuh |
| Daftar revisi prioritas | Tabel atau catatan | Menjadi masukan untuk pertemuan 16 |

---

# Slide 03 - Dari Rancangan Eksperimen ke Seminar Proposal

## Yang Sudah Dikerjakan pada Pertemuan 14

- Model konseptual dan experimental matrix.
- Pemilihan dataset, baseline, metrik, dan rencana mitigasi risiko.
- Hasil eksperimen pendahuluan awal.

## Yang Berubah pada Pertemuan 15

- Fokus bergeser dari merancang menjadi mempertanggungjawabkan.
- Audiens tidak hanya melihat kebenaran teknis, tetapi juga konsistensi seluruh alur: gap, RQ, kontribusi, bukti, keterbatasan.
- Seminar adalah ujian terhadap kemampuan berpikir doktoral, bukan sekadar laporan kemajuan.

> Prinsip utama: **setiap klaim yang disajikan harus dapat ditelusuri ke bukti atau argumen yang eksplisit.**

---

# Slide 04 - Tiga Pertanyaan Kunci yang Akan Diuji

## Pertanyaan dari Perspektif Penguji

| Pertanyaan | Makna bagi Proposal |
|---|---|
| Apakah kontribusi dapat dibedakan dari paper pembanding? | Novelty dan positioning harus jelas, bukan sekadar mengganti dataset atau menambah modul kecil. |
| Apakah eksperimen mampu mendukung klaim? | Desain eksperimen, baseline, dan metrik harus dapat memverifikasi hipotesis. |
| Apakah ruang lingkup proposal realistis untuk disertasi? | Cakupan tidak terlalu sempit atau terlalu luas, dan dapat diselesaikan dalam waktu studi. |

## Implikasi untuk Presentasi

- Setiap slide harus menjawab salah satu dari ketiga pertanyaan ini.
- Jika tidak, slide tersebut sebaiknya dihapus atau digabungkan.

---

# Slide 05 - Struktur Presentasi Proposal Awal

## Struktur yang Disarankan

1. Judul, identitas, dan satu kalimat ringkasan proposal.
2. Research gap dan posisi terhadap state-of-the-art.
3. Research question dan hipotesis.
4. Kontribusi ilmiah yang dijanjikan.
5. Metodologi dan rancangan eksperimen secara ringkas.
6. Hasil eksperimen awal dan interpretasi.
7. Keterbatasan serta rencana penelitian selanjutnya.

## Alur Logis yang Harus Terlihat

```text
Mengapa penting? → Apa yang belum terjawab? → Apa yang akan dikerjakan?
→ Bagaimana membuktikannya? → Apa bukti awalnya? → Apa keterbatasannya?
```

---

# Slide 06 - Pembagian Waktu Presentasi yang Efektif

## Contoh Alokasi Waktu Presentasi 20 Menit

| Bagian | Durasi | Poin Kunci |
|---|---|---|
| Pembukaan dan gap | 3 menit | Masalah, pentingnya, celah |
| RQ dan kontribusi | 3 menit | Fokus dan janji ilmiah |
| Metodologi | 4 menit | Alur singkat, bukan detail kode |
| Hasil awal | 6 menit | Bukti, visualisasi, interpretasi |
| Keterbatasan dan rencana | 3 menit | Kejujuran dan langkah berikut |
| Penutup | 1 menit | Ringkasan dan undangan bertanya |

## Prinsip

- Jangan menghabiskan waktu pada detail yang sudah diketahui audiens.
- Prioritaskan bukti yang paling mendukung klaim utama.
- Sisakan waktu untuk tanya jawab.

---

# Slide 07 - Membuka dengan Research Gap dan Position Statement

## Elemen Pembukaan yang Kuat

- Satu kalimat tentang domain dan urgensi masalah.
- Satu kalimat tentang keterbatasan metode yang ada.
- Satu kalimat tentang posisi proposal terhadap kesenjangan tersebut.

## Contoh Pola Kalimat

> "Model segmentasi berbasis SAM terbukti kuat pada citra natural, tetapi akurasinya menurun pada citra medis dengan anotasi tidak lengkap. Proposal ini mengisi celah tersebut dengan strategi prompt adaptif."

## Hindari

- Membuka dengan sejarah panjang yang tidak perlu.
- Menampilkan terlalu banyak istilah sebelum masalah dinyatakan.
- Mengklaim belum ada yang meneliti tanpa bukti.

---

# Slide 08 - Menyajikan Research Question dan Hipotesis secara Ringkas

## Format RQ yang Dapat Dievaluasi

- RQ harus spesifik, terukur, dan berkaitan langsung dengan gap.
- Gunakan satu atau dua RQ utama, bukan daftar panjang.

## Contoh Tampilan Slide

```text
RQ:
  Pada domain citra medis dengan anotasi parsial,
  apakah strategi prompt adaptif berbasis prioritas
  dapat meningkatkan kualitas segmentasi SAM?

Hipotesis:
  Prompt adaptif berbasis confidence map meningkatkan
  rerata IoU dibandingkan point prompt tunggal.
```

## Perhatikan

- Hipotesis harus dapat diuji dengan eksperimen yang direncanakan.
- Jika RQ dan hipotesis tidak sejalan, penguji akan menyerang konsistensi.

---

# Slide 09 - Menyatakan Kontribusi Ilmiah dengan Jelas

## Kontribusi yang Baik Bersifat

- Spesifik: menunjukkan bagian metode atau pengetahuan yang baru.
- Dapat diuji: memiliki indikator atau metrik yang jelas.
- Dibedakan dari state-of-the-art: menjelaskan perbedaan dengan paper pembanding.

## Contoh Pernyataan Kontribusi

> "Kami mengusulkan mekanisme prompt adaptif berbasis confidence map untuk model segmentasi yang sudah ada. Berbeda dari metode yang menambahkan decoder baru, pendekatan kami tidak mengubah arsitektur inti sehingga dapat diterapkan pada berbagai foundation model."

## Latihan Saat Menyusun Slide

- Tulis kontribusi dalam satu kalimat.
- Kemudian tulis kalimat "kontribusi ini berbeda dari ... karena ...".
- Jika tidak dapat menyelesaikan kalimat kedua, kontribusi belum siap diseminarkan.

---

# Slide 10 - Taksonomi Kontribusi pada Riset Pengolahan Citra Digital

## Jenis Kontribusi yang Umum dalam Computer Vision

| Jenis | Deskripsi | Contoh |
|---|---|---|
| Metode atau arsitektur | Modul, fungsi, atau alur baru | Blok attention ringan |
| Strategi atau formulasi | Cara baru memandang masalah | Prompt adaptif |
| Dataset atau benchmark | Data atau protokol evaluasi baru | Dataset domain spesifik |
| Analisis atau insight | Pemahaman baru tentang perilaku model | Studi kegagalan state-of-the-art |
| Framework atau teori | Kerangka kerja atau formalisasi | Formulasi degradasi baru |

## Implikasi

- Satu proposal tidak harus memiliki semua jenis.
- Pilih satu kontribusi utama sebagai tulang punggung, lalu posisikan kontribusi pendukung sebagai pelengkap.
- Hindari menyebut semua detail teknis sebagai kontribusi.

---

# Slide 11 - Menghindari Klaim yang Melebihi Bukti

## Jenis Klaim Berlebih yang Sering Muncul

- Mengatakan lebih baik tanpa menyebut konfigurasi dan baseline.
- Mengatakan mengungguli semua metode padahal hanya diuji pada dua dataset.
- Mengatakan umum padahal hanya berlaku pada domain tertentu.
- Menyebut state-of-the-art tanpa membandingkan dengan karya terbaru.

## Cara Membuat Klaim yang Aman

- Sertakan konteks: dataset, resolusi, jumlah seed, dan domain.
- Gunakan kata-kata yang sepadan dengan bukti: pada dataset X, dalam pengaturan Y.
- Nyatakan batas metrik yang dicapai, bukan hanya selisih.
- Bedakan hasil yang signifikan secara statistik dari yang sekadar lebih tinggi.

---

# Slide 12 - Evaluasi State-of-the-Art: Fungsi dan Tujuannya

## Fungsi Evaluasi State-of-the-Art dalam Seminar

- Menunjukkan bahwa proposal memahami lanskap riset.
- Menjelaskan dasar pemilihan baseline dan pembanding.
- Menjadi dasar argumentasi bahwa gap benar-benar ada.
- Membantu penguji menilai apakah novelty cukup kuat.

## Kesalahan Umum

- Menampilkan daftar paper tanpa analisis perbedaan.
- Hanya menyalin tabel akurasi tanpa menjelaskan mengapa metode usulan berpotensi unggul.
- Mengabaikan karya terbaru yang lebih relevan daripada karya klasik.

## Evaluasi Bukan Sekadar Literatur Review

- Evaluasi harus diarahkan pada keputusan: pada posisi mana proposal berdiri?

---

# Slide 13 - Matriks Literatur sebagai Bahan Evaluasi State-of-the-Art

## Bentuk Matriks yang Umum

| Paper / Metode | Masalah | Data | Pendekatan | Metrik | Keterbatasan | Kode? |
|---|---|---|---|---|---|---|
| Metode A | ... | ... | ... | ... | ... | Ya |
| Metode B | ... | ... | ... | ... | ... | Tidak |

## Cara Menggunakannya

- Bandingkan berdasarkan dimensi yang relevan dengan RQ.
- Identifikasi kombinasi fitur yang belum dieksplorasi.
- Gunakan matriks untuk menjelaskan mengapa posisi proposal unik.

## Catatan

- Matriks lengkap tidak perlu ditampilkan seluruhnya dalam slide.
- Tampilkan hanya baris-baris kunci yang membangun argumen gap.
- Matriks penuh dapat dimuat dalam naskah ringkas atau lampiran.

---

# Slide 14 - Memilih Baseline dan Pembanding yang Adil

## Prinsip Pemilihan Baseline

- Gunakan metode yang paling kuat dan relevan, bukan yang paling mudah dikalahkan.
- Sertakan metode klasik, deep learning, dan foundation model bila sesuai.
- Jika memungkinkan, gunakan implementasi resmi atau hasil yang direproduksi sendiri.

## Contoh Protokol

```text
Baseline A: metode yang menjadi fondasi domain.
Baseline B: state-of-the-art terbaru dengan kode publik.
Baseline C: varian tanpa komponen utama usulan (ablation dasar).
Metode usulan: metode dengan kontribusi yang diusulkan.
```

## Hal yang Perlu Disebutkan

- Sumber hasil: paper, kode resmi, atau reproduksi ulang.
- Konfigurasi yang disamakan: dataset, preprocessing, resource.
- Jumlah ulangan dan variasi seed.

---

# Slide 15 - Menyusun Tabel Perbandingan State-of-the-Art

## Format Tabel yang Informatif

| Metode | Dataset A (IoU) | Dataset B (Dice) | Parameter | Waktu | Catatan |
|---|---|---|---|---|---|
| Metode A | 72,4 | 0,811 | 45M | 12 ms | Kode tersedia |
| Metode B | 74,1 | 0,823 | 89M | 38 ms | Hasil paper |
| Metode C | 75,0 | 0,830 | 91M | 40 ms | Reproduksi kami |
| **Usulan** | **76,8** | **0,842** | **92M** | **42 ms** | Hasil awal |

## Prinsip Penyajian

- Beri tanda pada baris yang menjadi fokus.
- Nyatakan bahwa hasil masih awal jika memang demikian.
- Tambahkan kolom biaya agar tidak hanya berbicara akurasi.
- Jangan menyembunyikan sel kosong; tulis tidak dilaporkan.

---

# Slide 16 - Diagram Posisi Penelitian terhadap State-of-the-Art

## Gagasan Diagram Dua Dimensi

```text
Kemampuan adaptasi domain
        ^
        |                 Usulan
        |              x
        |        A
        |   B
        |              C
        |
        +----------------------> Efisiensi komputasi
```

- Sumbu dipilih berdasarkan dimensi yang paling membedakan proposal.
- Setiap titik diberi label nama metode.
- Area kosong menunjukkan ruang yang diisi oleh proposal.

## Catatan

- Diagram hanya alat bantu; tetap perlu penjelasan naratif.
- Pastikan posisi titik dapat dipertanggungjawabkan dari data.
- Hindari diagram yang terlalu rumit untuk dibaca dalam dua menit.

---

# Slide 17 - Menunjukkan Hasil Eksperimen Awal

## Jenis Bukti yang Dapat Ditampilkan

- Tabel metrik kuantitatif dari eksperimen pendahuluan.
- Visualisasi output model: mask, bounding box, heatmap, dan lain-lain.
- Kurva pelatihan atau konvergensi.
- Perbandingan kualitatif dengan baseline.

## Prinsip

- Hasil awal tidak harus sempurna; harus konsisten dengan arah klaim.
- Jika hasil awal belum mendukung, sampaikan secara jujur dan jelaskan perbaikan yang direncanakan.
- Setiap metrik harus disertai konfigurasi eksperimen yang singkat namun cukup.

## Hindari

- Menampilkan hanya hasil terbaik dari sekian percobaan.
- Menggunakan visualisasi tanpa skala atau tanpa label.
- Menyembunyikan eksperimen yang gagal padahal informatif.

---

# Slide 18 - Interpretasi Hasil: Bukan Sekadar Angka

## Tiga Lapis Interpretasi

1. Deskripsi: apa yang terjadi? Contoh: IoU naik 2 poin.
2. Penjelasan: mengapa hal itu terjadi secara teknis?
3. Implikasi: apa artinya bagi hipotesis dan kontribusi?

## Contoh Interpretasi yang Baik

> "Peningkatan IoU terutama terjadi pada objek kecil. Hal ini konsisten dengan hipotesis bahwa confidence map membantu memperbaiki prompt pada wilayah ambigu."

## Contoh Interpretasi yang Lemah

> "Metode kami lebih baik karena arsitekturnya lebih canggih."

## Gunakan Analisis Statistik Sederhana Bila Memungkinkan

```python
## Ilustrasi perbandingan rerata beberapa seed
import numpy as np

hasil_usulan = np.array([76.1, 76.8, 77.0])
hasil_baseline = np.array([74.0, 74.5, 75.2])

print("delta rerata:", hasil_usulan.mean() - hasil_baseline.mean())
```

- Nyatakan variasi antar seed, bukan hanya rerata.

---

# Slide 19 - Analisis Error dan Failure Case

## Mengapa Perlu Ditampilkan

- Menunjukkan kedewasaan ilmiah dan pemahaman terhadap metode.
- Membantu penguji menilai batas keandalan hasil.
- Memberi arah untuk penelitian lanjutan.

## Format Analisis Error

- Kategorikan kegagalan: objek kecil, okulasi, domain shift, anotasi salah, dan lain-lain.
- Tampilkan dua atau tiga contoh visual dengan label input, prediksi, ground truth.
- Jelaskan pola kesalahan dan hipotesis penyebabnya.

## Contoh Tabel Ringkas

| Kategori Kesalahan | Frekuensi | Dugaan Penyebab |
|---|---|---|
| Objek kecil | Tinggi | Resolusi fitur tidak memadai |
| Batas tidak tegas | Sedang | Ambigu pada anotasi |
| Okulasi | Rendah | Konteks belum dimanfaatkan |

---

# Slide 20 - Menghubungkan Hasil Awal dengan Hipotesis

## Logika yang Harus Tampak

```text
Hipotesis nol: prompt adaptif tidak mengubah kualitas segmentasi.
Hipotesis alternatif: prompt adaptif meningkatkan rerata IoU.

Hasil: delta IoU +2,1 poin pada 3 seed dengan p < 0,05.
Kesimpulan awal: bukti awal mendukung hipotesis alternatif.
```

## Perhatikan

- Bedakan mendukung dari membuktikan.
- Hasil awal pada dataset kecil hanya memberikan indikasi.
- Jelaskan eksperimen lanjutan yang akan memperkuat kesimpulan.

---

# Slide 21 - Jujur terhadap Keterbatasan Eksperimen

## Hal yang Perlu Diakui

- Ukuran dataset atau jumlah kelas yang terbatas.
- Keterbatasan komputasi sehingga grid search belum menyeluruh.
- Ketergantungan pada model pretrained yang mungkin bias.
- Belum dilakukan evaluasi pada distribusi data yang berbeda.

## Cara Menyajikan Keterbatasan

- Pisahkan keterbatasan desain dan keterbatasan pelaksanaan.
- Nyatakan dampak keterbatasan terhadap interpretasi hasil.
- Sertakan rencana mitigasi atau eksperimen lanjutan.

> Contoh: "Hasil ini hanya diuji pada 200 citra dari satu institusi. Oleh karena itu, generalisasi lintas institusi belum dapat disimpulkan."

---

# Slide 22 - Demonstrasi Notebook atau Prototipe

## Tujuan Demo

- Memperlihatkan bahwa metode dapat berjalan, bukan hanya konsep.
- Memberi bukti nyata berupa input, proses, dan output.
- Membuka ruang tanya jawab yang lebih konkret.

## Pilihan Bentuk Demo

- Jupyter Notebook atau Google Colab dengan sel yang sudah dijalankan.
- Aplikasi kecil berbasis Gradio atau Streamlit.
- Visualisasi interaktif menggunakan Matplotlib atau napari.
- Video singkat untuk proses yang lambat.

## Contoh Alur Demo

```python
## Contoh alur demo di notebook
config = load_config("configs/segmen.yaml")
model = SegmenModel.from_pretrained(config.checkpoint)
img = load_image("samples/case_01.png")
mask = model.segment(img, prompt=prompt)
show_comparison(img, mask, baseline_mask)
```

## Prinsip

- Demo harus siap dalam mode fallback jika jaringan atau runtime gagal.
- Tidak perlu menunjukkan seluruh kode; fokus pada bagian yang mendukung klaim.
- Siapkan beberapa input uji yang beragam, termasuk kasus yang menantang.

---

# Slide 23 - Checklist Kesiapan Demo Teknis

## Sebelum Presentasi

- [ ] Data dan model tersedia pada environment yang sama.
- [ ] GPU pada Colab atau server sudah diuji dengan seed yang sama.
- [ ] Waktu eksekusi per sel dikendalikan agar tidak terlalu lama.
- [ ] Fallback video atau tangkapan layar siap.
- [ ] Font dan ukuran visualisasi cukup terbaca.

## Saat Presentasi

- Jalankan satu alur utama dari awal sampai akhir.
- Tunjukkan hasil perantara bila membantu.
- Jika terjadi error, akui, jelaskan, lalu lanjutkan ke fallback.

## Setelah Presentasi

- Catat pertanyaan yang muncul dari demo.
- Perbaiki dokumentasi kode agar siap direview pada pertemuan 16.

---

# Slide 24 - Etika Demonstrasi dan Dokumentasi

## Kejujuran dalam Demo

- Jangan menampilkan hasil prediksi dari dataset test tanpa menyebutnya.
- Jangan menghapus sel yang gagal agar terlihat mulus.
- Jangan menyembunyikan penggunaan model yang tidak sesuai lisensi.

## Dokumentasi

- Tuliskan versi pustaka: PyTorch, timm, Albumentations, dan lain-lain.
- Simpan random seed dan konfigurasi di satu tempat.
- Gunakan Git untuk melacak perubahan.

## Lisensi dan Data

- Pastikan dataset memiliki izin penggunaan.
- Jika memakai model pretrained, periksa ketentuan lisensi.
- Jika memublikasikan demo, sertakan kredit yang sesuai.

---

# Slide 25 - Teknik Menjawab Pertanyaan Doktoral

## Prinsip Dasar

- Dengarkan pertanyaan sampai selesai sebelum menjawab.
- Jelaskan asumsi yang digunakan jika perlu.
- Jika tidak tahu, katakan tidak tahu, lalu tawarkan cara menemukan jawabannya.

## Struktur Jawaban yang Baik

1. Klarifikasi: "Pertanyaan Anda tentang ... bukan?"
2. Jawaban langsung: berikan inti jawaban dalam dua atau tiga kalimat.
3. Bukti: kaitkan dengan hasil, literatur, atau logika.
4. Keterbukaan: akui batas dan nyatakan langkah lanjutan.

## Hindari

- Menghindar dengan jargon yang tidak menjawab.
- Berdebat tanpa bukti.
- Memberi jawaban panjang ketika pertanyaan singkat.

---

# Slide 26 - Pertanyaan yang Sering Diajukan Penguji

## Kelompok Pertanyaan

| Aspek | Contoh Pertanyaan |
|---|---|
| Novelty | Apa bedanya dengan paper X yang sudah melakukan hal serupa? |
| Metodologi | Mengapa memilih baseline ini? |
| Hasil | Apakah perbedaan ini signifikan secara statistik? |
| Data | Bagaimana kualitas anotasi data Anda? |
| Risiko | Apa yang terjadi jika hasil utama tidak tercapai? |
| Publikasi | Apa target venue dan kontribusi manuskrip? |

## Persiapan

- Tuliskan jawaban singkat untuk setiap pertanyaan di atas.
- Latih bersama rekan sejawat.
- Jangan menghafal jawaban secara kaku; pahami logika di baliknya.

---

# Slide 27 - Strategi Menghadapi Pertanyaan Sulit

## Jika Pertanyaan Menyerang Kelemahan

- Akui kelemahan secara jujur.
- Jelaskan mengapa kelemahan tersebut tidak mengubah argumen utama.
- Sebutkan rencana perbaikan.

## Jika Pertanyaan Menunjukkan Kesalahpahaman

- Koreksi dengan sopan dan berikan bukti.
- Gunakan kalimat: "Mungkin saya perlu memperjelas bahwa ..."

## Jika Pertanyaan di Luar Lingkup

- Nyatakan bahwa hal itu menarik tetapi di luar lingkup proposal.
- Jelaskan kaitannya bila ada, atau tawarkan diskusi di luar seminar.

## Sikap Umum

- Tetap tenang dan berpikir sebelum menjawab.
- Anggap pertanyaan sebagai peluang memperkuat proposal.
- Catat pertanyaan yang belum bisa dijawab untuk tindak lanjut.

---

# Slide 28 - Peer Review Formal: Peran dan Tanggung Jawab

## Tujuan Peer Review

- Memberikan umpan balik yang konstruktif, bukan menghakimi.
- Membantu presenter melihat kelemahan yang tidak disadarinya.
- Melatih kemampuan reviewer seperti pada proses publikasi.

## Peran Reviewer

- Membaca naskah ringkas sebelum seminar.
- Mengisi rubrik evaluasi.
- Memberikan komentar per bagian dan satu ringkasan.

## Etika Reviewer

- Fokus pada karya, bukan pribadi.
- Berikan bukti atau contoh untuk setiap kritik.
- Sebutkan kekuatan di samping kelemahan.
- Jaga kerahasiaan naskah jika diminta.

---

# Slide 29 - Rubrik Evaluasi Seminar Proposal

## Contoh Rubrik

| Aspek | Skala | Indikator |
|---|---|---|
| Kejelasan gap | 1–4 | Gap dinyatakan dengan bukti literatur |
| Kualitas RQ | 1–4 | RQ spesifik dan dapat diuji |
| Novelty | 1–4 | Kontribusi dibedakan dari state-of-the-art |
| Metodologi | 1–4 | Rancangan eksperimen adil dan logis |
| Hasil awal | 1–4 | Bukti mendukung klaim |
| Penyampaian | 1–4 | Presentasi jelas dan tepat waktu |

## Penggunaan Rubrik

- Skor digunakan untuk umpan balik, bukan penilaian akhir.
- Komentar tertulis lebih penting daripada angka.
- Rubrik dapat disesuaikan dengan kebutuhan program studi.

---

# Slide 30 - Format Naskah Ringkas Proposal

## Tujuan Naskah Ringkas

- Mendokumentasikan argumen proposal dalam bentuk yang dapat direview.
- Menjadi bahan diskusi bagi penguji.
- Menjadi cikal bakal bab pendahuluan disertasi.

## Struktur Umum

1. Judul dan abstrak singkat.
2. Latar belakang dan research gap.
3. Research question dan hipotesis.
4. Kontribusi yang dijanjikan.
5. Metodologi dan experimental design secara ringkas.
6. Hasil awal dan keterbatasan.
7. Rencana publikasi dan timeline.

## Batasan

- Panjang 2–4 halaman, mengikuti template yang diberikan dosen.
- Gunakan referensi yang relevan dan dapat diverifikasi.
- Sertakan tautan ke repositori kode bila ada.

---

# Slide 31 - Contoh Kerangka Naskah Ringkas

## Kerangka Konten

```text
1. Judul
   Satu kalimat yang menggambarkan kontribusi utama.

2. Gap dan motivasi
   - Kondisi state-of-the-art saat ini.
   - Keterbatasan yang relevan dengan RQ.
   - Konsekuensi bila gap tidak diisi.

3. RQ, hipotesis, kontribusi
   - Daftar RQ, maksimal dua.
   - Hipotesis untuk setiap RQ.
   - Kontribusi utama dan pendukung.

4. Metodologi
   - Alur metode usulan.
   - Dataset dan protokol evaluasi.
   - Baseline dan analisis statistik.

5. Hasil awal
   - Tabel atau gambar utama.
   - Interpretasi dan keterbatasan.

6. Rencana publikasi
   - Target venue, isi manuskrip, dan jadwal.
```

---

# Slide 32 - Merekam dan Mengelola Masukan Penguji

## Cara Mencatat Masukan

- Gunakan tabel dengan kategori: pertanyaan, kritik, saran, tindak lanjut.
- Catat nama penguji jika memungkinkan.
- Bedakan masukan substantif dan masukan editorial.

## Contoh Format

| No | Sumber | Kategori | Masukan | Prioritas |
|---|---|---|---|---|
| 1 | Penguji 1 | Novelty | Jelaskan beda dengan paper X | Tinggi |
| 2 | Penguji 2 | Eksperimen | Tambah uji statistik | Tinggi |
| 3 | Audiens | Presentasi | Kurangi slide latar belakang | Sedang |

## Setelah Seminar

- Kategorikan masukan berdasarkan urgensi.
- Masukkan ke daftar revisi prioritas.
- Diskusikan dengan dosen pembimbing sebelum pertemuan 16.

---

# Slide 33 - Menyusun Daftar Revisi Prioritas

## Matriks Prioritas

| Prioritas | Karakteristik | Contoh |
|---|---|---|
| Tinggi | Mengancam validitas klaim | Baseline tidak adil |
| Sedang | Mengurangi daya dukung hasil | Perlu tambah dataset |
| Rendah | Memperbaiki penyampaian | Tata letak slide |

## Prinsip Penyusunan

- Fokus pada revisi yang mengubah argumen atau bukti.
- Jangan mencantumkan semua saran tanpa seleksi.
- Tentukan siapa yang bertanggung jawab dan kapan selesainya.

## Luaran Akhir

Daftar revisi prioritas menjadi bahan utama pada pertemuan 16, yaitu **Konsolidasi Proposal Disertasi dan Rencana Publikasi**.

---

# Slide 34 - Dari Hasil Awal ke Rencana Publikasi

## Hubungan Hasil Awal dengan Publikasi

- Hasil awal berfungsi sebagai proof of concept untuk manuskrip.
- Publikasi pertama dapat berisi kontribusi metode, hasil awal, dan eksperimen lanjutan.
- Publikasi berikutnya dapat memperluas evaluasi dan analisis.

## Pilihan Bentuk Publikasi

- Konferensi: cepat, umpan balik luas, cocok untuk hasil awal.
- Jurnal: lebih lengkap, sesuai untuk kajian mendalam.
- Preprint: mempercepat penyebaran dan memperoleh komentar.

## Syarat Etis

- Jangan memublikasikan hasil yang sama secara duplikat tanpa rujuk silang.
- Jelaskan status preprint pada saat submission.
- Dokumentasikan semua eksperimen agar reviewer dapat memeriksa.

---

# Slide 35 - Positioning terhadap Target Venue

## Menentukan Target Venue

- Gunakan peta literatur dari pertemuan 2 dan 12.
- Perhatikan topik, metode, dan format paper di venue tersebut.
- Sesuaikan positioning dengan audiens venue.

## Pertanyaan untuk Memilih Venue

- Apakah kontribusi utama sesuai dengan scope venue?
- Apakah hasil awal cukup menarik bagi pembaca venue?
- Apakah kode dan data siap untuk kebijakan reproducibility?

## Contoh Pemetaan

| Venue | Fokus | Kecocokan Proposal |
|---|---|---|
| Konferensi A | Metode segmentasi | Tinggi |
| Konferensi B | Aplikasi medis | Sedang |
| Jurnal C | Evaluasi komprehensif | Setelah eksperimen lanjut |

- Hindari menargetkan venue tanpa membaca panduan penulis.

---

# Slide 36 - Timeline Disertasi dan Milestone Publikasi

## Contoh Bentuk Timeline

```text
Semester 1-2: Pengumpulan data, protokol, eksperimen awal.
Semester 3: Eksperimen utama, analisis statistik.
Semester 4: Penulisan dan submission konferensi pertama.
Semester 5: Ekstensi eksperimen, submission jurnal.
Semester 6: Finalisasi disertasi.
```

## Dalam Presentasi Proposal

- Sertakan timeline yang realistis, bukan muluk.
- Tandai milestone yang sudah dicapai, misalnya hasil awal.
- Tandai risiko utama dan rencana mitigasi.
- Jelaskan ketergantungan dengan ketersediaan data atau GPU.

## Catatan

- Timeline disesuaikan dengan kebijakan program studi.
- Jangan menampilkan tanggal spesifik yang tidak dapat dipertanggungjawabkan.

---

# Slide 37 - Latihan Singkat: Menguji Posisi State-of-the-Art

## Instruksi untuk Mahasiswa

Pilih satu baris pada matriks literatur Anda, lalu jawab tiga hal:

1. Apa kontribusi utama paper tersebut?
2. Apa keterbatasan yang belum dijawab?
3. Apa minimal satu ide yang dapat diuji untuk mengatasi keterbatasan itu?

## Contoh Cepat

```text
Paper: model segmentasi dengan point prompt tunggal.
Keterbatasan: sensitif terhadap posisi titik prompt.
Ide: agregasi confidence map dari beberapa kandidat prompt.
```

## Manfaat

- Melatih kemampuan membaca state-of-the-art secara kritis.
- Menyiapkan argumen seminar.
- Menjadi bahan diskusi peer review.

---

# Slide 38 - Ringkasan: Konsistensi sebagai Inti Seminar

## Empat Pilar Seminar Proposal Awal

1. Gap dan positioning didukung oleh evaluasi state-of-the-art.
2. Kontribusi dinyatakan secara spesifik dan dapat dibedakan dari state-of-the-art.
3. Hasil awal diinterpretasikan sebagai bukti, bukan sekadar angka.
4. Keterbatasan dan revisi dikelola secara jujur dan sistematis.

## Pertanyaan Final untuk Menguji Kesiapan

- Apakah saya dapat menjelaskan proposal dalam 3 menit kepada orang di luar bidang?
- Apakah saya dapat menyebutkan perbedaan utama dengan paper pembanding?
- Apakah eksperimen yang direncanakan dapat menjawab RQ?

## Luaran yang Harus Dibawa

- Presentasi dan materi demo.
- Naskah ringkas proposal.
- Daftar revisi prioritas untuk pertemuan 16.

---

# Slide 39 - Penutup

TERIMA KASIH

Pertemuan berikutnya

**Konsolidasi Proposal Disertasi dan Rencana Publikasi**