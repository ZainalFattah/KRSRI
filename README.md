# Struktur Direktori dan Deskripsi Project KRSRI

Dokumen ini memuat informasi mendetail mengenai struktur direktori dari project ini beserta penjelasan mendalam mengenai fungsi, tujuan, dan isi dari masing-masing folder dan file yang menyusun keseluruhan alur kerja *machine learning* untuk deteksi objek (menggunakan YOLO).

---

## Struktur Direktori

```text
KRSRI/
│
│
├── dataset/
│   ├── Dataset KRSRI 2024 -2-YOLO11/
│   ├── Dataset KRSRI 2024 -2-YOLO12/
│   ├── Dataset KRSRI 2024 -2-YOLO26/
│   ├── Dataset KRSRI 2024 -2-YOLO8/
│   ├── Dataset KRSRI 2024 -2-YOLO9/
│   └── ZIP/
├── Dokumentasi Gambar Output/
│   ├── YOLOv11n/
│   ├── YOLOv12n/
│   ├── YOLOv26n/
│   ├── YOLOv5nu/
│   ├── YOLOv8n/
│   └── YOLOv9s/
├── krsri/
│   ├── Include/
│   ├── Lib/
│   ├── Scripts/
│   ├── etc/
│   ├── share/
│   └── pyvenv.cfg
├── pretrained_models/
│   ├── yolo11n.pt
│   ├── yolo12n.pt
│   ├── yolo26n.pt
│   ├── yolov5nu.pt
│   ├── yolov8n.pt
│   └── yolov9s.pt
├── runs/
│   └── detect/
├── trained_models/
│   ├── yolo11n_best.pt
│   ├── yolo12n_best.pt
│   ├── yolo26n_best.pt
│   ├── yolov5nu_best.pt
│   ├── yolov8n_best.pt
│   └── yolov9s_best.pt
├── benchmark_results.csv
├── test.jpg
└── Train_and_benchmark.ipynb
```

---

## Deskripsi Lengkap Folder dan File

### 📂 Folder Utama & Sub-Foldernya

#### 1. `dataset/`
Ini adalah direktori inti untuk manajemen data *machine learning*. Folder ini memuat keseluruhan dataset gambar beserta label anotasi (*bounding box*) untuk kelas objek yang akan dideteksi.
- **`Dataset KRSRI 2024 -2-YOLO11/`**, **`YOLO12/`**, **`YOLO26/`**, **`YOLO8/`**, **`YOLO9/`**: Masing-masing sub-folder ini menyimpan dataset yang sudah dikonfigurasi dan dipartisi (biasanya terbagi ke dalam *train*, *valid*, *test*) secara khusus untuk versi model YOLO yang bersangkutan. Meskipun secara umum format label `.txt` (YOLO format) serupa, pemisahan folder ini seringkali digunakan untuk mengorganisasi versi dataset *export* yang berbeda, mencegah tumpang tindih data, dan memastikan stabilitas *pathing* direktori saat eksperimen berjalan secara sekuensial.
- **`ZIP/`**: Tempat penyimpanan file arsip mentah (*raw files*) dari dataset dalam format kompresi `.zip`. Berfungsi sebagai berkas cadangan (*backup master*) yang sewaktu-waktu bisa diekstrak ulang jika terjadi kesalahan manipulasi pada folder dataset utama.

#### 2. `Dokumentasi Gambar Output/`
Direktori ini dikhususkan sebagai media penyimpanan bukti visual berupa tangkapan layar (*screenshot*) dari proses dan hasil eksperimen. Gambar-gambar di dalamnya memuat dokumentasi log terminal saat proses *training*, ringkasan arsitektur model (*layer summary*), proses unduh *pretrained weights*, hingga hasil metrik evaluasi akhir (seperti nilai mAP50, mAP50-95, metrik *speed / fps*, dll) yang dicetak oleh Ultralytics.
- Sub-folder seperti **`YOLOv11n/`**, **`YOLOv12n/`**, **`YOLOv5nu/`**, dsb., berfungsi untuk mengategorikan secara rapi dokumentasi gambar milik setiap model. Sebagai contoh, di dalam sub-folder `YOLOv5nu/` terdapat gambar dokumentasi log pelatihan yang merekam informasi jumlah parameter (*2,509,049 parameters*), nilai evaluasi (contoh: mAP50: 0.991), serta laporan *Early Stopping*. Hal ini sangat berguna untuk keperluan pelaporan (*reporting*) atau melacak riwayat performa eksekusi model tanpa harus membongkar log teks mentahnya kembali.

#### 3. `krsri/`
Folder ini merupakan fondasi dari *Python Virtual Environment* (Lingkungan Virtual). Digunakan untuk mengisolasi instalasi *package* atau pustaka eksternal (seperti modul `ultralytics`, `torch`, `opencv`, `pandas`) yang dibutuhkan khusus oleh project ini. Hal ini memastikan versi pustaka tidak bentrok (*conflict*) dengan *environment* Python sistem secara global atau project lainnya.
- **`Include/`**, **`Lib/`** (terdapat *site-packages* di dalamnya), **`Scripts/`**: Direktori hierarki yang memuat *executable files* (seperti `python.exe` dan `pip.exe`) serta semua modul pihak ketiga yang diunduh.
- **`etc/`**, **`share/`**: Berisi *file* pendukung konfigurasi *tools*.
- **`pyvenv.cfg`**: Berkas teks yang memuat variabel konfigurasi dasar *virtual environment* tersebut.

#### 4. `pretrained_models/`
Folder krusial untuk menyimpan bobot awal (*base weights*) atau model standar pra-latih (*pre-trained*) dari para peneliti Ultralytics. Contoh isinya adalah **`yolo11n.pt`**, **`yolov8n.pt`**, **`yolov9s.pt`**, dll.
Model ini sebelumnya telah mempelajari miliaran pola dan fitur dasar dari dataset gambar masif berukuran besar (seperti MS COCO). Menggunakan file `.pt` di folder ini sebagai titik awal (*transfer learning*) berarti model YOLO tidak perlu dilatih benar-benar dari nol pada dataset KRSRI, sehingga proses konvergensi (*training*) jauh lebih efisien, hemat waktu komputasi, dan cenderung menghasilkan tingkat akurasi yang lebih tinggi.

#### 5. `runs/`
Ini adalah direktori otomatis yang digenerasi oleh sistem *backend* Ultralytics YOLO saat skrip *training*, *validation*, maupun *prediction* dieksekusi.
- **`detect/`**: Segala bentuk eksperimen terkait deteksi (bukan segmentasi/klasifikasi) direkam di folder ini. Jika Anda membedahnya, Anda akan menemukan folder eksperimen terpisah (biasanya dinamakan `train`, `train2`, `val`, dsb.). Di dalamnya, Ultralytics menyimpan berbagai *log* metrik kinerja model (*Loss graph*, *Confusion Matrix*, *F1-Confidence curve*, Precision-Recall curve), file CSV riwayat per-*epoch*, gambar *batch* asli (*ground truth*) vs. prediksi (*predictions*), hingga salinan bobot model (di folder `weights/`).

#### 6. `trained_models/`
Direktori khusus yang secara sengaja diorganisasikan untuk menyimpan hasil final terbaik (Final Output) dari keseluruhan proses pelatihan di project ini.
- Berisi file bobot model `.pt` dengan nomenklatur akhiran `_best` (seperti **`yolov5nu_best.pt`**, **`yolo26n_best.pt`**). File ini merepresentasikan bobot jaringan saraf (Neural Network) dengan peforma akurasi tervalidasi tertinggi sepanjang siklus *training* pada dataset spesifik KRSRI. File model terbaik inilah yang siap untuk digunakan lebih lanjut untuk fase produksi (*deployment* ke hardware robot/aplikasi).

---

### 📄 File di Luar Folder (*Root File*)

#### 1. `benchmark_results.csv`
Sebuah file tabel dengan format *Comma-Separated Values* (CSV) yang sangat penting karena merangkum komparasi kuantitatif atau nilai rapor peforma dari masing-masing model yang dilatih. Di dalamnya biasanya memuat variabel metrik metrik performa (*benchmarking*) kunci, di antaranya: jenis/arsitektur YOLO, mAP50, mAP50-95 (mean Average Precision), waktu inferensi (kecepatan prediksi per frame), besaran *Loss*, maupun profil efisiensi parameter (*model size/FLOPs*). File ini di-*generate* untuk mempermudah analisis keputusan penentuan algoritma mana yang paling efisien dipakai dalam implementasi nyata.

#### 2. `test.jpg`
File media (gambar mandiri) yang bersifat coba-coba (*sample inference image*). File ini disediakan di direktori dasar (*root*) agar developer / pengguna bisa langsung mencoba (*testing*) fungsi prediktif pendeteksian objek model yang baru saja dilatih, tanpa harus menulis ulang jalur panjang untuk memanggil dataset utuh atau memuat program kamera web terlebih dahulu.

#### 3. `Train_and_benchmark.ipynb`
Ini adalah *Main Control Script* (Naskah Program Inti) berupa Jupyter Notebook interaktif yang menaungi seluruh alur kerja logis atau *pipeline workflow* project ini. Skrip yang ada di dalamnya mencakup:
1. **Instalasi dan Impor dependensi** (menyambung ke Ultralytics dan pustaka terkait).
2. **Definisi *path* Dataset dan Konfigurasi Hyperparameter**.
3. **Training *Loop***: Blok kode untuk memanggil, memuat, dan melatih model pre-trained (dari `pretrained_models`) menggunakan dataset per setiap iterasi algoritma YOLO.
4. **Validasi (*Testing*)**: Skrip untuk menguji kinerja model dan mem-validasi akurasi metrik evaluasi.
5. **Ekspor Hasil Benchmarking**: Kode penyusun data hasil pelatihan (mAP, speed, dll) lalu menuliskannya (*export*) dalam format rapi ke `benchmark_results.csv`.
6. **Eksperimen Inferensi**: Blok kode khusus untuk men-tes gambar `test.jpg` dan menyimpan hasilnya ke `Dokumentasi Gambar Output/`.

---

**Dibuat oleh:** Zainal Fattah
