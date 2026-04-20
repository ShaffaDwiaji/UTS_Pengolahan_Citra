# Sistem Pendeteksi Tingkat Keparahan Terhadap Kerusakan Jalanan di Indonesia

## Deskripsi
Penelitian ini mengembangkan sistem cerdas untuk mendeteksi dan menganalisis tingkat keparahan kerusakan permukaan jalan raya, khususnya jalan retak (*cracking*) dan berlubang (*potholes*). Sistem ini menggunakan pendekatan **Hybrid Computer Vision**, yang menggabungkan keandalan *Deep Learning* (YOLO) untuk lokalisasi objek dengan presisi Pengolahan Citra Digital (*Edge Detection*) untuk ekstraksi kontur geometris.

Tantangan utama dalam deteksi ini berasal dari kondisi visual di lapangan yang sangat kompleks, seperti adanya aspal berkerikil, genangan air, atau bayangan yang dapat memunculkan *noise* visual. Penggabungan metode ini bertujuan untuk menciptakan sistem deteksi yang akurat, kebal terhadap *noise* lingkungan, dan efisien secara komputasi.

---

## Tujuan
* Membangun purwarupa (*prototype*) sistem deteksi kerusakan jalan otomatis menggunakan kombinasi *Deep Learning* dan Pengolahan Citra Digital.
* Mengimplementasikan algoritma **YOLO** untuk mengidentifikasi keberadaan dan lokasi spesifik (*Bounding Box*) kerusakan jalan secara cepat.
* Menerapkan dan membandingkan algoritma **Canny Edge Detection** dan **Sobel Edge Detection** secara eksklusif pada area *Bounding Box* untuk mengekstrak kontur anomali aspal dengan akurat.
* Mengukur dan mengklasifikasikan tingkat keparahan kerusakan jalan berdasarkan luasan area dan pola garis tepi yang berhasil diekstrak.
* Menyediakan landasan analitis bagi pihak berwenang untuk menentukan prioritas perbaikan infrastruktur secara objektif.

---

## Dataset
Dataset citra yang digunakan dalam penelitian ini bersumber dari Kaggle:
🔗 **[Potholes-Detection-YOLOv8](https://www.kaggle.com/datasets/anggadwisunarto/potholes-detection-yolov8)**

Dataset ini difokuskan untuk melatih model deteksi objek serta menguji algoritma deteksi tepi. Dataset ini berisi kumpulan citra permukaan aspal dengan berbagai kondisi, seperti:
* **Jalan berlubang (*potholes*)**
* **Jalan retak (*cracking*)**
* **Kondisi aspal normal** dengan berbagai variasi pencahayaan.

Penggunaan dataset ini sangat krusial untuk menguji ketahanan model terhadap gangguan visual di lapangan.

---

## Metodologi (The Hybrid Pipeline)

Sistem ini berjalan melalui tahapan berurutan (*pipeline*) sebagai berikut:

### 1. Deteksi Objek (Lokalisasi dengan YOLO)
Citra mentah dimasukkan ke dalam model YOLO untuk memindai dan mengenali area yang terindikasi sebagai kerusakan. Output dari tahap ini adalah koordinat *Bounding Box* atau *Region of Interest* (ROI).

### 2. Preprocessing ROI
Sistem melakukan pemotongan (*cropping*) gambar hanya pada area ROI. Tahap ini meliputi konversi ke *grayscale* dan pengaplikasian *Gaussian Blur* untuk meredam *noise* dari kerikil aspal agar tidak terdeteksi sebagai tepi semu.

### 3. Ekstraksi Kontur (Edge Detection)
Menerapkan dua metode komparasi secara terpisah di dalam ROI:
* **Metode 1:** Canny Edge Detection
* **Metode 2:** Sobel Edge Detection
Kedua algoritma ini bertugas memisahkan area kerusakan dari latar belakang aspal berdasarkan perbedaan intensitas piksel.

### 4. Analisis Tingkat Keparahan
Menghitung luasan piksel di dalam kontur untuk mengklasifikasikan tingkat keparahan kerusakan (Ringan, Sedang, atau Parah).

---

## Metode Utama yang Digunakan

### 🔹 YOLO (You Only Look Once)
Berperan sebagai pendeteksi utama yang melokalisasi posisi kerusakan di tengah lingkungan yang kompleks. YOLO memastikan bahwa proses pengolahan citra selanjutnya (seperti filter dan deteksi tepi) hanya fokus pada area yang benar-benar rusak, sehingga sangat menghemat komputasi.

### 🔹 Canny Edge Detection
Berperan dalam mendefinisikan bentuk pasti dan detail kontur kerusakan. Algoritma ini unggul dalam menghasilkan garis tepi yang halus, kontinu, dan akurat berdasarkan perbedaan tingkat *grayscale* yang tajam setelah melalui filter *Gaussian*.

### 🔹 Sobel Edge Detection
Berperan sebagai operator konvolusi 3x3 (horizontal dan vertikal) yang sangat cepat dan efektif untuk mengekstrak bentuk dasar objek. Metode ini digunakan sebagai pembanding untuk mendeteksi gradien kontras yang ekstrem antara lubang yang dalam dan permukaan aspal normal.

---

## Hasil & Evaluasi (Draf)
*(Catatan: Bagian ini dapat diperbarui dengan metrik akurasi nyata seperti mAP YOLO dan presisi kontur setelah fase pengujian selesai)*

| Tahapan | Metode | Fungsi Utama | Karakteristik Performa |
|---|---|---|---|
| **Lokalisasi** | YOLO | Mencari koordinat *Bounding Box* | Tahan terhadap *noise* lingkungan (bayangan, genangan air). |
| **Ekstraksi Tepi** | Canny | Analisis tekstur presisi tinggi | Bentuk geometris jelas, sangat bergantung pada *thresholding*. |
| **Ekstraksi Tepi** | Sobel | Analisis kontras tinggi | Sangat cepat, namun rentan pada aspal berkerikil (*noise* tinggi). |

---

## Pengembangan Selanjutnya
* Mengoptimalkan model pemrosesan agar sistem dapat berjalan secara *real-time* menggunakan kamera *dashboard* mobil.
* Menyempurnakan parameter adaptif pada proses *preprocessing* agar otomatis menyesuaikan pencahayaan gambar (siang vs. malam).
* Membangun antarmuka pengguna (UI/UX) atau *dashboard* analitik untuk memvisualisasikan pemetaan jalan rusak bagi instansi terkait.

---

## Tools & Teknologi
* **Bahasa Pemrograman:** Python
* **Deep Learning Framework:** Ultralytics (YOLOv8)
* **Computer Vision:** OpenCV (`cv2`)
* **Komputasi & Visualisasi:** NumPy, Matplotlib
* **Platform:** Google Colab / Jupyter Notebook

---

## Catatan
Proyek ini diajukan sebagai Proposal Ujian Tengah Semester untuk mata kuliah Pengolahan Citra di Program Studi Rekayasa Perangkat Lunak, Jurusan Teknik, Politeknik Negeri Madiun (2026). 

**Disusun Oleh:**
* **Dionesyus Divo Crista Marvino** (NPM. 234311012)
* **Shaffa Dwiaji Feryansyah Putra** (NPM. 234311028)
