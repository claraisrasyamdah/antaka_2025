# Antaka - Sistem Rekomendasi Wisata Interaktif

## Deskripsi Proyek

**Antaka** adalah sistem rekomendasi wisata berbasis AI yang dirancang untuk memberikan saran destinasi wisata di Indonesia berdasarkan preferensi pengguna, seperti kategori wisata, kota, harga tiket, dan rating minimal. Sistem ini menggunakan model BERT untuk ekstraksi entitas (Named Entity Recognition) dan mengintegrasikan Google Maps API untuk menghitung jarak serta waktu tempuh antar lokasi. Antaka mendukung dua mode antarmuka di Google Colab:

- **Mode Teks**: Input pengguna melalui prompt teks sederhana.
- **Mode Visual**: Antarmuka interaktif dengan kolom input, tombol, dan peta visual menggunakan `ipywidgets` dan `folium`.

Proyek ini memanfaatkan data wisata dari file JSON yang di-host di GitHub dan model NLP dari Hugging Face untuk memproses input pengguna dalam bahasa Indonesia.

## Fitur Utama

- **Ekstraksi Entitas**: Menggunakan model BERT (`claraisra/antaka-v2`) untuk mengidentifikasi kategori, kota, harga, dan rating dari input pengguna.
- **Rekomendasi Wisata**: Mencocokkan preferensi pengguna dengan database wisata dan mengembalikan hingga 5 rekomendasi terbaik.
- **Perhitungan Rute**: Menghitung jarak dan waktu tempuh antar lokasi menggunakan Google Maps Directions API.
- **Visualisasi Peta**: Menampilkan peta interaktif dengan marker untuk lokasi wisata dan rute menggunakan `folium` (khusus mode visual).
- **Antarmuka Interaktif**: Mode visual menyediakan kolom input dan tombol submit untuk pengalaman pengguna yang lebih baik.

## Prasyarat

Untuk menjalankan Antaka di Google Colab, pastikan Anda memiliki:

- **Google Colab**: Lingkungan runtime dengan akses internet.
- **Google Maps API Key**: Aktifkan Directions API di Google Cloud Console dan masukkan kunci API di variabel `google_api_key`.
- **OpenAI API Key**: Untuk ekstraksi nama tempat (opsional, tergantung penggunaan `get_place_name`).
- **Koneksi Internet**: Untuk mengunduh data JSON dan model dari Hugging Face.

### Dependensi

Instal library berikut di Google Colab:

```bash
!pip install transformers torch requests ipywidgets folium
```

## Struktur Kode

Kode Antaka terdiri dari beberapa bagian utama:

1. **Library**: Mengimpor library seperti `transformers`, `torch`, `requests`, `ipywidgets`, dan `folium`.
2. **Data**: Mengunduh data wisata, pemetaan kota, kategori, dan alias tempat dari URL GitHub.
3. **Model**: Memuat model BERT (`claraisra/antaka-v2`) untuk ekstraksi entitas.
4. **Fungsi Pendukung**:
   - `predict_bio`: Memprediksi label entitas dari input pengguna menggunakan model BERT.
   - `get_place_name`: Mengekstrak nama tempat menggunakan OpenAI API (opsional).
   - `find_specific_place`: Mencari tempat spesifik berdasarkan alias.
   - `find_matching_wisata`: Mencocokkan preferensi pengguna dengan data wisata.
   - `extract_entities`: Mengekstrak entitas dari hasil prediksi BERT.
   - `normalize_entities`: Menormalkan entitas ke format standar (misalnya, harga ke angka, kota ke nama standar).
   - `generate_pertanyaan`: Menghasilkan pertanyaan untuk meminta input pengguna yang belum terkonfirmasi.
   - `semua_terkonfirmasi`: Memeriksa apakah semua entitas sudah terkonfirmasi.
5. **Mode Teks**: Implementasi berbasis prompt teks menggunakan `input()`.
6. **Mode Visual**: Implementasi dengan antarmuka interaktif menggunakan `ipywidgets` dan peta `folium`.

## Cara Menjalankan

1. **Siapkan API Key**:

   - Ganti `google_api_key` di kode dengan kunci Google Maps API Anda:

     ```python
     google_api_key = "YOUR_GOOGLE_API_KEY"
     ```
   - (Opsional) Ganti `openai_api_key` dengan kunci OpenAI API Anda jika menggunakan fungsi `get_place_name`:

     ```python
     openai_api_key = "YOUR_OPENAI_API_KEY"
     ```

2. **Salin Kode**:

   - Salin kode lengkap ke sel baru di Google Colab.

3. **Jalankan Dependensi**:

   - Pastikan sel yang berisi `!pip install ipywidgets folium` dijalankan untuk menginstal library.

4. **Pilih Mode**:

   - **Mode Teks**: Jalankan fungsi `main()` di bagian `#UJI COBA ##TRY TEXT BASE`. Anda akan diminta memasukkan input melalui prompt teks.
   - **Mode Visual**: Jalankan kode di bagian `#UJI COBA ##TRY VISUAL`. Antarmuka dengan kolom teks dan tombol akan muncul, diikuti oleh peta interaktif setelah semua input diberikan.

5. **Interaksi**:

   - Masukkan preferensi wisata (kategori, kota, harga, rating) sesuai pertanyaan yang muncul.
   - Gunakan kata kunci seperti "bebas" atau "terserah" untuk melewati kriteria tertentu.
   - Setelah semua kriteria terpenuhi, sistem akan menampilkan:
     - Daftar rekomendasi wisata (maksimal 5 tempat).
     - Jarak total dan durasi perjalanan.
     - Link Google Maps untuk setiap lokasi dan rute lengkap.
     - (Mode Visual) Peta interaktif dengan marker dan rute.

## Contoh Penggunaan

### Mode Teks

1. Jalankan kode `main()`.
2. Contoh interaksi:

   ```
   ['Jenis wisata apa yang kamu mau? (Misal: Religi, Taman, Budaya...)']
   Masukkan jawaban Anda: Religi
   ['Kota tujuannya di mana? (Misal: Yogyakarta, Bandung...)']
   Masukkan jawaban Anda: Bandung
   ['Maksimal harga tiketnya berapa? (Contoh: 50000, 50rb)']
   Masukkan jawaban Anda: 50000
   ['Minimal rating tempatnya? (Contoh: 4, 3.0, 3)']
   Masukkan jawaban Anda: 4
   === Rekomendasi Wisata ===
   1. Masjid Raya Bandung (Tempat Ibadah) - Bandung | Rp0 | Rating: 4.8
      Link: https://www.google.com/maps/search/?api=1&query=-6.921851,107.606148
   ...
   Total Jarak: 10.5 km
   Total Durasi: 25.3 menit
   Link Rute Google Maps: https://www.google.com/maps/dir/?api=1&origin=-6.973330651584524,107.63032454232832&destination=...
   ```

### Mode Visual

1. Jalankan kode di bagian `#TRY VISUAL`.
2. Masukkan input di kolom teks dan klik tombol "Submit".
3. Setelah semua input diberikan, hasil akan ditampilkan dengan peta interaktif, marker untuk lokasi, dan link Google Maps.

## Catatan Penting

- **Data Wisata**: Data diambil dari `data_wisata.json` di GitHub. Pastikan URL valid dan data tersedia.
- **Model BERT**: Model `claraisra/antaka-v2` diunduh dari Hugging Face. Pastikan koneksi internet stabil.
- **Google Maps API**: Perhitungan jarak dan waktu memerlukan API key yang valid. Jika kuota API habis, Anda mungkin melihat error.
- **Fungsi** `get_place_name`: Saat ini di-comment di kode (`find_specific_place`). Aktifkan jika Anda memiliki OpenAI API key dan ingin mendukung pencarian tempat spesifik.
- **Keterbatasan Peta**: Peta `folium` menampilkan rute sebagai garis lurus antar koordinat. Untuk rute jalan akurat, gunakan link Google Maps yang dihasilkan.

## Struktur File Pendukung

- **data_wisata.json**: Dataset tempat wisata dengan atribut seperti `Place_Id`, `Place_Name`, `Category`, `City`, `Price`, `Rating`, `Lat`, `Long`.
- **category_mapping.json**: Pemetaan kategori wisata (misalnya, "religi" ke "Tempat Ibadah").
- **city_mapping.json**: Pemetaan nama kota ke format standar.
- **mapping_place_alias.json**: Pemetaan alias nama tempat ke nama kanonis.

## Kontribusi

Proyek ini dikembangkan untuk memenuhi Tugas Akhir saya di Telkom University
