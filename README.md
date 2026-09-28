# Scraper Tempat Makan dan Wisata Viral di Indonesia

Project ini mengumpulkan artikel berita berbahasa Indonesia dari Google News RSS untuk mencari tempat makan dan destinasi wisata yang sedang viral dalam periode tertentu. Isi artikel kemudian diambil dan diproses untuk mengekstrak nama tempat, lokasi, perkiraan waktu mulai viral, alasan viral, serta koordinat geografis.

Hasil akhirnya dapat digunakan sebagai bahan riset atau analisis awal. Karena ekstraksi dilakukan dengan heuristik berbasis regex, hasil tetap perlu diperiksa dan dibersihkan secara manual sebelum digunakan untuk kebutuhan resmi.

## Alur Pemrosesan

1. Mencari artikel melalui Google News RSS berdasarkan beberapa kata kunci, seperti `tempat makan viral`, `kuliner viral`, `cafe viral`, `restoran viral`, dan `wisata viral`.
2. Menghapus artikel duplikat berdasarkan URL berita dan menyaring artikel berdasarkan tanggal publikasi.
3. Membuka URL Google News dan mengambil URL sumber berita menggunakan `googlenewsdecoder`.
4. Mengambil isi artikel dari metadata `articleBody`, paragraf HTML, atau deskripsi artikel.
5. Menyimpan artikel dengan isi yang berhasil diambil dan panjang minimal 100 karakter.
6. Mengekstrak nama tempat, lokasi, waktu mulai viral, dan alasan viral menggunakan pola regex.
7. Melakukan geocoding lokasi menggunakan Nominatim dari OpenStreetMap.

## Struktur File

| File | Keterangan |
| --- | --- |
| `scrap_tempat_viral.ipynb` | Notebook utama untuk mengambil berita dan menghasilkan dataset tempat viral. |
| `scrap_tempat_viral_extend.ipynb` | Notebook lanjutan untuk memproses dataset berita dalam beberapa batch. |
| `df_news.csv` | Dataset berita yang URL dan isi artikelnya berhasil diambil. |
| `df_news_extended_1_249.csv` | Batch pertama dataset berita yang diperluas. |
| `df_news_extended_250_503.csv` | Batch kedua dataset berita yang diperluas. |
| `df_news_extended_503_757.csv` | Batch ketiga dataset berita yang diperluas. |
| `daftar_tempat_viral.csv` | Dataset hasil kurasi tempat makan dan wisata viral. |

## Persyaratan

- Python 3.10 atau lebih baru
- Jupyter Notebook atau Visual Studio Code dengan ekstensi Jupyter
- Koneksi internet untuk Google News, situs sumber berita, dan layanan geocoding

Library Python yang digunakan:

```bash
pip install feedparser requests beautifulsoup4 lxml geopy pandas python-dateutil tqdm openpyxl pytrends googlenewsdecoder
```

## Cara Menjalankan

1. Buka folder project ini di Visual Studio Code atau Jupyter Notebook.
2. Pilih interpreter Python yang sudah memiliki library pada bagian [Persyaratan](#persyaratan).
3. Buka `scrap_tempat_viral.ipynb`.
4. Jalankan cell secara berurutan dari atas ke bawah.
5. Sesuaikan `KEYWORDS` dan `MONTHS_BACK` jika ingin mengubah kata kunci atau periode pencarian.
6. Periksa file `df_news.csv` dan hasil ekstraksi tempat setelah notebook selesai.

Untuk data dalam jumlah lebih besar, gunakan `scrap_tempat_viral_extend.ipynb`. Notebook ini membaca hasil batch sebelumnya, mengambil isi artikel secara bertahap, lalu menyimpan batch lanjutan seperti `df_news_extended_250_503.csv` dan `df_news_extended_503_757.csv`.

## Format Data

`df_news.csv` berisi antara lain:

- `keyword`: kata kunci pencarian
- `title`: judul berita
- `link`: URL Google News
- `published`: waktu publikasi
- `source`: nama sumber berita
- `final_url`: URL artikel sumber setelah di-resolve
- `article_text`: isi artikel yang berhasil diekstrak
- `article_text_length`: panjang isi artikel

`daftar_tempat_viral.csv` berisi antara lain:

- `nama_tempat`: nama tempat
- `kategori`: kategori tempat
- `jenis_tempat`: jenis atau konsep tempat
- `lokasi`: lokasi yang ditemukan pada artikel
- `keterangan`: menu, daya tarik, atau informasi pendukung
- `status_viral`: indikasi viral dari artikel
- `jumlah_artikel`: jumlah artikel sumber
- `keyword_pencarian`: kata kunci yang menemukan tempat tersebut
- `judul_artikel_sumber`: judul artikel rujukan
- `link_sumber`: tautan artikel rujukan

## Catatan dan Batasan

- Nama tempat, lokasi, dan alasan viral tidak diekstrak menggunakan model NLP/NER penuh. Format artikel yang beragam dapat menyebabkan hasil tidak lengkap atau kurang tepat.
- Viralitas pada dataset merupakan indikasi berdasarkan pemberitaan, bukan pengukuran langsung jumlah tayangan atau interaksi media sosial.
- Nominatim memiliki kebijakan penggunaan dan batas permintaan. Notebook menggunakan jeda antar-request dan `user_agent`; jangan menjalankan scraping geocoding dalam jumlah besar tanpa memperhatikan kebijakan layanan.
- Ketersediaan, struktur, dan isi situs berita dapat berubah sehingga hasil eksekusi ulang mungkin berbeda.
- Patuhi Terms of Service Google News, situs sumber berita, dan OpenStreetMap/Nominatim. Gunakan data terutama untuk riset dan analisis internal.
- Dataset CSV yang sudah tersedia merupakan hasil eksekusi sebelumnya dan tidak selalu merepresentasikan kondisi terkini.