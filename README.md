# Sistem Pendeteksi Tingkat Keparahan Terhadap Kerusakan Jalanan di Indonesia

## Deskripsi
Penelitian ini mengembangkan sistem cerdas untuk mendeteksi dan menganalisis tingkat keparahan kerusakan permukaan jalan raya, khususnya jalan retak (*cracking*) dan berlubang (*potholes*). Sistem ini menggunakan pendekatan **Hybrid Computer Vision**, yang menggabungkan keandalan *Deep Learning* (YOLO) untuk lokalisasi objek dengan presisi Pengolahan Citra Digital (*Edge Detection*) untuk ekstraksi kontur geometris.

Tantangan utama dalam deteksi ini berasal dari kondisi visual di lapangan yang kompleks, seperti adanya aspal berkerikil, genangan air, atau bayangan. Penggabungan kedua metode ini bertujuan untuk menciptakan sistem deteksi yang akurat, efisien secara komputasi, dan mampu meminimalisir *noise* lingkungan.

---

## Tujuan
* Membangun purwarupa (*prototype*) sistem deteksi kerusakan jalan otomatis menggunakan kombinasi *Deep Learning* dan Pengolahan Citra Digital.
* Mengimplementasikan algoritma **YOLO** untuk mengidentifikasi keberadaan dan lokasi spesifik (*Bounding Box*) kerusakan jalan secara cepat.
* Menerapkan algoritma **Canny dan Sobel Edge Detection** secara eksklusif pada area *Bounding Box* untuk mengekstrak kontur anomali aspal dengan akurat.
* Mengukur dan mengklasifikasikan tingkat keparahan kerusakan jalan berdasarkan luasan area dan pola garis tepi yang berhasil diekstrak.
* Menyediakan landasan analitis bagi pihak berwenang untuk menentukan prioritas perbaikan infrastruktur secara objektif.

---

## Dataset
Dataset citra yang digunakan dalam penelitian ini bersumber dari Kaggle:
🔗 **[Potholes-Detection-YOLOv8](https://www.kaggle.com/datasets/anggadwisunarto/potholes-detection-yolov8)**

Dataset ini difokuskan untuk melatih model deteksi objek serta menguji algoritma deteksi tepi, yang berisi kumpulan citra permukaan aspal dengan berbagai kondisi, seperti:
* **Jalan berlubang (*potholes*)**
* **Jalan retak (*cracking*)**
* **Kondisi aspal normal** dengan berbagai variasi pencahayaan.

Penggunaan dataset ini sangat krusial untuk menguji ketahanan model terhadap gangguan visual seperti bayangan dan tekstur aspal yang tidak rata.

---

## Metodologi (The Hybrid Pipeline)

Sistem ini berjalan melalui tahapan berurutan (*pipeline*) sebagai berikut:

### 1. Deteksi Objek (Lokalisasi dengan YOLO)
Citra mentah dimasukkan ke dalam model YOLO untuk memindai dan mengenali area yang terindikasi sebagai kerusakan. Output dari tahap ini adalah koordinat *Bounding Box* atau *Region of Interest* (ROI).

### 2. Preprocessing ROI
Sistem melakukan pemotongan (*cropping*) gambar hanya pada area ROI. Tahap ini meliputi konversi ke *grayscale* dan pengaplikasian *Gaussian Blur* untuk meredam *noise* dari kerikil aspal agar tidak terdeteksi sebagai tepi semu.

### 3. Ekstraksi Kontur (Edge Detection)
Menerapkan **Canny** atau **Sobel Edge Detection** secara eksklusif di dalam ROI. Algoritma ini memisahkan area kerusakan dari latar belakang aspal berdasarkan perbedaan intensitas piksel yang tajam.

### 4. Analisis Tingkat Keparahan
Menghitung luasan piksel di dalam kontur untuk mengklasifikasikan tingkat keparahan kerusakan (Ringan, Sedang, atau Parah).

---

## Metode Utama yang Digunakan

### 🔹 YOLO (You Only Look Once)
Berperan sebagai pendeteksi utama yang melokalisasi posisi kerusakan di tengah lingkungan yang kompleks. YOLO memastikan bahwa proses pengolahan citra selanjutnya hanya fokus pada area yang relevan.

### 🔹 Canny & Sobel Edge Detection
Berperan dalam mendefinisikan bentuk pasti dan detail kontur kerusakan. Canny digunakan untuk mendapatkan garis tepi yang halus dan tersambung, sementara Sobel efektif untuk mendeteksi gradien kontras yang ekstrem antara lubang dan aspal.

---

## Hasil & Evaluasi (Draf)
*(Bagian ini dapat diperbarui dengan nilai mAP, Precision, dan Recall setelah fase pengujian selesai)*

| Metode | Fungsi Utama | Kelebihan |
|---|---|---|
| **YOLO** | Lokalisasi Kerusakan | Sangat cepat dan tahan terhadap distraksi latar belakang. |
| **Canny Edge** | Analisis Kontur | Memberikan detail geometris yang presisi untuk klasifikasi tingkat keparahan. |

---

## Pengembangan Selanjutnya
* Optimasi model agar dapat diimplementasikan untuk deteksi secara *real-time* melalui kamera *dashboard*.
* Penambahan fitur pemetaan lokasi kerusakan berbasis GPS.
* Pengembangan antarmuka pengguna (UI/UX) untuk visualisasi data bagi instansi terkait.

---

## Tools & Teknologi
* **Bahasa Pemrograman:** Python
* **Libraries:** OpenCV (`cv2`), Ultralytics (YOLOv8), NumPy, Matplotlib.
* **Platform:** Google Colab / Jupyter Notebook.

---

## Catatan
Proyek ini disusun sebagai bagian dari tugas Ujian Tengah Semester (UTS) mata kuliah Pengolahan Citra Digital di Program Studi Rekayasa Perangkat Lunak, Politeknik Negeri Madiun (2026).

**Tim Pengembang:**
* **Dionesyus Divo Crista Marvino** (NPM. 234311012)
* **Shaffa Dwiaji Feryansyah Putra** (NPM. 234311028)
