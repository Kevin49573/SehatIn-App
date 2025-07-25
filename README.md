# SehatIn - AI Health Scanner
Aplikasi web untuk screening kesehatan menggunakan AI yang bisa menganalisis foto mata, kulit, kuku, dan lidah untuk deteksi dini masalah kesehatan.
## Deskripsi
SehatIn adalah aplikasi health scanner yang memanfaatkan teknologi computer vision untuk analisis kesehatan non-invasif. User bisa upload foto atau ambil gambar langsung untuk mendapatkan analisis AI tentang kondisi kesehatan mereka.
Fitur utama:
1. Analisis mata (deteksi anemia, kelelahan)
2. Pemeriksaan kulit (hidrasi, defisiensi vitamin)
3. Analisis kuku (kekurangan nutrisi)
4. Pemeriksaan lidah (kesehatan pencernaan)
## Teknologi yang Digunakan
**Frontend:**
1. HTML5
2. CSS3 dengan animasi custom
3. JavaScript ES6+
4. Tailwind CSS
**Library & API:**
1. Lucide Icons untuk ikon
2. File API untuk upload gambar
3. Camera API untuk foto langsung
**AI Engine:**
1. IBM Granite AI (simulasi)
2. Computer vision untuk analisis gambar
3. Mock data untuk demo hasil analisis
## Fitur
1. **4 Jenis Scan**: Mata, kulit, kuku, lidah
2. **Upload Gambar**: Drag & drop atau pilih file
3. **Hasil Real-time**: Analisis dengan confidence score
4. **History Tracking**: Riwayat scan dan trend kesehatan
5. **Generate Report**: Export hasil analisis ke PDF
6. **Responsive Design**: Work di desktop dan mobile
7. **Notifikasi**: Alert untuk berbagai aksi user
## Setup & Instalasi
1. Clone atau download project ini
2. Buka file `index.html` di browser modern
3. Atau jalankan local server:
   ```bash
   python -m http.server 8000
   # atau
   npx live-server
   ```
4. Akses di `http://localhost:8000`
**Requirements:**
1. Browser modern (Chrome, Firefox, Safari, Edge)
2. Izin akses kamera untuk foto langsung
3. Koneksi internet untuk load CDN
## AI Support
Aplikasi ini menggunakan simulasi IBM Granite AI untuk analisis kesehatan:
**Cara Kerja:**
1. User upload foto organ yang ingin diperiksa
2. AI memproses gambar dan detect pattern kesehatan
3. System generate confidence score dan risk level
4. Memberikan rekomendasi berdasarkan findings
**Jenis Analisis:**
1. **Eye Analysis**: Deteksi anemia dari warna kelopak mata, tanda kelelahan
2. **Skin Health**: Cek tingkat hidrasi, elastisitas kulit, tanda defisiensi vitamin
3. **Nail Analysis**: Analisis warna kuku, tekstur, tanda kekurangan nutrisi
4. **Tongue Exam**: Pemeriksaan coating lidah, warna, indikator kesehatan pencernaan
**Mock Data:**
Saat ini menggunakan data simulasi untuk demo. Dalam implementasi nyata akan terintegrasi dengan API IBM Granite untuk analisis real.

**Akurasi:**
1. Setiap hasil dilengkapi confidence score (0-100%)
2. Risk level: Low (0-33%), Medium (34-66%), High (67-100%)
3. Rekomendasi berbasis medical knowledge base
## Disclaimer
Hasil analisis SehatIn hanya untuk informasi dan tidak menggantikan konsultasi medis profesional. Selalu konsultasi dengan dokter untuk diagnosis dan treatment yang tepat.
## Contact
Kevin Nataniel Halim - kevinnatanielhalim@gmail.com
