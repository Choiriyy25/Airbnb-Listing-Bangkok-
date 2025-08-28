# Latar Belakang
Bangkok merupakan salah satu kota dengan jumlah listing Airbnb terbanyak di Asia Tenggara. Persaingan antar host tinggi, sehingga penentuan harga sewa menjadi faktor penting. Dengan data Airbnb (misalnya dari InsideAirbnb.com), dapat dilakukan analisis untuk mengetahui faktor apa saja yang mempengaruhi harga serta membangun model prediksi harga.

Kelebihan Airbnb:

* Harga lebih fleksibel dan kompetitif
* Pilihan properti sangat beragam dan unik
* Cocok untuk long stay maupun short stay
* Fleksibilitas dalam hal fasilitas dan durasi menginap
* Bisa merasakan kesempatan tinggal "seperti lokal"

Kekurangannya:

* Pada umumnya, Airbnb tidak menyediakan fasilitas seperti yang biasa ditemukan di hotel, seperti room service atau resepsionis.
* Kualitas bisa sangat bervariasi antar host
* Check-in kadang tidak sepraktis hotel jika host tidak responsif

  
# Dataset
Data dihimpun dari listing airbnb Bangkok yang dapat diakses melalui link berikut :
https://drive.google.com/drive/folders/1A_KBMRFTS5Mthpp46nulso679ML4ZwTF

# Pernyataan Masalah
Bangkok merupakan salah satu destinasi wisata terbesar di Asia Tenggara dan memiliki ribuan listing Airbnb yang tersebar di berbagai distrik. Tingginya jumlah penawaran membuat harga sewa sangat bervariasi, dipengaruhi oleh lokasi, tipe kamar, jumlah ulasan, rating, serta fasilitas yang ditawarkan. Kondisi ini menimbulkan beberapa permasalahan:

1) Host sering mengalami kesulitan dalam menentukan harga sewa yang kompetitif, karena tidak ada acuan jelas tentang faktor apa yang paling berpengaruh terhadap harga.
2) Calon tamu juga kesulitan memahami perbedaan harga antar listing yang menawarkan fasilitas serupa.
3) Belum tersedia model prediksi harga yang dapat membantu host menyesuaikan harga sewa sesuai kondisi pasar Airbnb di Bangkok.

Berdasarkan kondisi tersebut, maka permasalahan utama yang akan diteliti adalah:
* Faktor apa saja yang memengaruhi harga sewa Airbnb di Bangkok?
* Bagaimana membangun model prediksi harga sewa yang akurat untuk membantu host dalam strategi penetapan harga?

Business problem: 
Berdasarkan hal tersebut perlu diketahui karakteristik dari data penginapan yang telah dikumpulkan (listing) untuk selanjutnya bisa merekomendasikan beberapa hal yang berpotensi meningkatkan revenue dan efisiensi pada program dan target marketing.

# Data
Berdasarkan dari dictionary dataset, ini berisi informasi mengenai karakteristik properti host, lokasi, jumlah review, ketersediaan, dan sebagainya. Terdapat 15 kolom di dalam dataset yang akan digunakan, antara lain:
* id: kode unik untuk setiap listing yang terdaftar di Airbnb
* name: nama dari listing
* host_id: kode unik untuk host
* host_name: nama host (biasanya nama pertama/nama depan)
* neighborhood: lokasi listing (sesuai informasi latitude dan longitude)
* latitude: informasi lokasi di peta (Horzinta)
* longitude: informasi lokasi di peta (Vertical)
* room_type: tipe tempat tinggal yang ditawarkan (Private Room, Entire home/apt, Hotel room, Shared Room)
* price: harga dari listing menggunakan dengan nilai mata uang lokal.
* minimum_nights: jumlah hari minimal untuk menyewa listing
* number_of_reviews: jumlah review yang dimiliki listing
* last_review: tanggal dari review terbaru yang masuk
* calculated_host_listings_count: jumlah listing yang dimiliki host di suatu lokasi pada aplikasi Mybnb
* availability_365: Ketersediaan dari listing x hari kedepan
* number_of_reviews_ltm: Jumlah review yang dimiliki listing dalam 12 bulan terakhir

# Data Understanding
Berdasarkan dari dictionary dataset, ini berisi informasi mengenai karakteristik properti host, lokasi, jumlah review, ketersediaan, dan sebagainya. Terdapat 15 kolom di dalam dataset yang akan digunakan, antara lain:
* id: kode unik untuk setiap listing yang terdaftar di Airbnb
* name: nama dari listing
* host_id: kode unik untuk host
* host_name: nama host (biasanya nama pertama/nama depan)
* neighborhood: lokasi listing (sesuai informasi latitude dan longitude)
* latitude: informasi lokasi di peta (Horizonta)
* longitude: informasi lokasi di peta (Vertical)
* room_type: tipe tempat tinggal yang ditawarkan (Private Room, Entire home/apt, Hotel room, Shared Room)
* price: harga dari listing menggunakan dengan nilai mata uang lokal.
* minimum_nights: jumlah hari minimal untuk menyewa listing
* number_of_reviews: jumlah review yang dimiliki listing
* last_review: tanggal dari review terbaru yang masuk
* calculated_host_listings_count: jumlah listing yang dimiliki host di suatu lokasi pada aplikasi Mybnb
* availability_365: Ketersediaan dari listing x hari kedepan
* number_of_reviews_ltm: Jumlah review yang dimiliki listing dalam 12 bulan terakhir

  # Data Cleaning
Apa aja yang udah saya laukan di tahap data celaning
1. Drop Kolom Tidak Relevan

    * `Unnamed: 0` dihapus karena hanya index hasil ekspor.
2. Validasi Format

    * `last_review` dikonversi ke format datetime
3. Penanganan Missing Value

    * `reviews_per_month`: Missing → diisi 0
    * `last_review`: Missing → dibiarkan NaT karena penting untuk deteksi listing tidak aktif.
    * `host_name`: Missing → diisi 'Unknown'
    * `name`: Missing (sangat sedikit) → baris dihapus
4. Hapus Nilai Tidak Masuk Akal

    * Listing dengan `price` = 0 dihapus (1 baris)
    * `minimum_nights > 365` → dipertahankan, digunakan untuk segmentasi long-stay
5.  Normalisasi Data Kategorikal

    * `neighbourhood`: kapitalisasi + perbaikan typo
    * Contoh: `'Yan Na Wa'` → `'Yan Nawa'`, `'Parthum Wan'` → `'Pathum Wan'`
6.  Cek & Validasi Duplikasi

    * Tidak ditemukan baris duplikat
    * `host_id` boleh duplikat (karena 1 host bisa punya banyak listing)
    * Kombinasi `host_id` & `host_name` juga konsisten
7.  Deteksi Outlier (IQR & Boxplot)

    * `price`: 1.402 outlier → dipertahankan untuk segmen premium
    * `minimum_nights`: 3.168 outlier → long-stay, tidak dihapus
    * `number_of_reviews` : 2.240 outlier → justru jadi indikator popularitas

# Data Analysis

Kita telah melakukan Data Understanding and Cleaning. Berikutnya data ini akan digunakan untuk mencoba menjawab business problem dengan melihat gambaran dan karakteristik yang berpengaruh dan berpotensi untuk meningkatkan transaksi dan revenue. Beberapa pendekatan yaitu dengan mencoba fokus pada `jenis kamar` yang paling banyak diminati berdasarkan review dan membandingkan fitur-fitur lain yang tersedia agar bisa melihat gambaran dan karakteristik dari behaviour untuk berikutnya bisa merekomendasikan strategi target marketing untuk meningkatkan revenue. 

# Jumlah Listing Airbnb di Setiap distrik di Bangkok
  
Gambaran umum penginapan di bangkok:
- Airbnb bangkok sudah ada penginapan (listing) diseluruh distrik Bangkok.
- Sebaran jumlah penginapan terbanyak cenderung berada di daerah pusat Bangkok. 
Berikutnya kita akan coba melihat lebih jauh top 10 lokasi bedasarkan review terbanyak.

- Dapat kita lihat dari sebaran penginapan airbnb Bangkok di top 10 lokasi berdasarkan review juga cenderung berada daerah pusat Bangkok.
- Artinya dari sini kita bisa kita bisa melihat bahwa jumlah penginapan berbanding lurus dengan jumlah review.


# Jenis Kamar Top 10 Lokasi Berdasarkan Review 12 Bulan Terakhir 
Dari barplot tersebut, kita bisa melihat lokasi Airbnb yang paling populer di Bangkok. Melalui riset di google, dapat diketahui hal-hal berikut:
1. Khlong Toei: Merupakan district yang terkenal dengan marketnya (Khlong Toei Market) yang murah dan menjual berbagai jenis bahan makanan dan makanan jadi.
2. Vadhana: Juga dapat disebut sebagai Watthana, merupakan district yang terkenal dengan pusat perbelanjaan dan kuliner yang modern.
3. Sathon: Merupakan district yang tergolong mewah dan memiliki banyak pilihan kuliner gourmet sampai high-end.
4. Ratchathewi: Merupakan district yang dekat dengan sarana transportasi (merupakan transport hub).
5. Huai Khwang: Merupakan district yang kaya akan wisata budaya (cultural landmark) seperti Siam Niramit Theatre.
6. Bang Rak: Merupakan district yang terkenal dengan pusat belanja dan hiburannya.
7. Phaya Thai: Merupakan district tempat pemukiman affluent yang memiliki banyak pilihan kafe outdoor dan coffee shop.
8. Parthum Wan: Lebih dikenal dengan Pathum Wan, merupakan district yang terkenal dengan pusat perbelanjaannya, yaitu mega-mall Siam.
9. Chatu Chak: Merupakan district yang terkenal dengan marketnya yang hanya buka di akhir pekan (Chatuchak Market) yang menjual berbagai macam barang mulai dari makanan, hiasan patung buddha, keramik, oleh-oleh, dsb.
10. Phra Nakhon: Merupakan district yang terkenal dengan bangunan bersejarahnya seperti Wat Pho, Grand Palace, dsb.
11. Din Daeng (12 bulan terkahir): Merupakan district yang terkenal dengan marketnya.

# Proporsi Tipe Penginapan Airbanb Bangkok
- Dapat dilihat juga data review selama 12 bulan terakhir tetap konsisten didominasi oleh jenis kamar `Entire home/apt`.
- Dapat dilihat dari proporsi tipe penginapan airbnb bangkok didominasi jenis kamar `Entire home/apt` yang mencakup **56.2%**.
Dapat disimpulkan dari pendekatan lokasi bahwa jenis kamar `Entire home/apt` merupakan jenis kamar yang memiliki transaksi terbanyak dibandingkan jenis kamar lainnya.

# Pendekatan Harga
Beberapa hal yang dapat di highlight antara lain:
* Dapat dilihat bahwa korelasi variabel `price` dengan variabel `minimum_nights` tidak memilki korelasi yang postif dan cenderung negatif walaupun tidak signifikan
* Dapat dilihat bahwa korelasi variabel `price` dengan variabel `number_of_reviews` tidak memilki korelasi yang kuat dan cenderung negatif
walaupun tidak signifikan
* Dapat dilihat bahwa korelasi variabel `price` dengan variabel `availability_365` tidak memilki korelasi yang kuat dan cenderung negatif
walaupun tidak signifikan
* Dapat dilihat bahwa korelasi variabel `price` dengan variabel `number_of_reviews_ltm` tidak memilki korelasi yang kuat dan cenderung positif walaupun tidak signifikan

# Scatterplot Ketersediaan VS Banyak Review Dalam 12 Bulan Terakhir
Dapat disimpulkan berdasarkan pendekatan ketersediaan jenis kamar `entire home/apt` memiliki ketersediaan hari yang yang paling rendah menandakan bahwa jenis kamar ini paling banyak diminati dan didukung juga oleh data dari top 10 lokasi berdasarkan yang paling banyak di review.

Jika dilihat dari diagram scatterplot secara umum hubungan jumlah review dengan ketersediaan tidak berkorelasi positif atau tidak berpengaruh signifikan.

Dari scatterplot dan uji korelasi yang dilakukan, dapat diketahui bahwa ketersediaan memiliki korelasi lemah negatif terhadap popularitas. Semakin lama ketersediaan suatu listing x hari, semakin sedikit jumlah review yang didapatkan.

Hal tersebut diduga terjadi karena guest cenderung memilih listing yang sudah terkenal atau berada di daerah yang populer. Listing yang populer atau berada di daerah yang populer cenderung akan ramai dengan guest, sehingga mengakibatkan ketersediaan listing semakin sedikit (karena penuh dibooking guest). Sehingga, terbentuklah korelasi negatif antara ketersediaan dengan jumlah review.

# Pendekatan Minimum Nights
Dapat dilihat bahwa tipe listing entire home memiliki rata-rata lama sewa minimum tertinggi dibanding tipe listing yang lain. Hal tersebut tentunya masuk akal, karena penyewaan satu rumah penuh jarang (hampir tidak pernah) hanya digunakan untuk 1 hari saja. Mengingat tipe listing entire home/apt adalah yang paling populer, lama minimum sewa yang rendah kurang menentukan popularitas listing.

* Dapat dilihat jenis kamar Entire home/apt memiliki lama sewa minimum tertinggi dari keseluruhan jenis kamar di top 10 lokasi.
* Dapat disimpulkan bahwa pendekatan minimum nights atau lama sewa minimum kurang berpengaruh signifikan terhadap review secara keseluruhan. Menandakan bahwa minimum nights bukanlah variabel potensial untuk strategi meningkatkan transaksi/revenue.

  # Kesimpulan

Berdasarkan analisis yang berfokus pada jenis kamar serta hubungannya dengan jumlah review untuk memahami gambaran dan karakteristik Airbnb di Bangkok, diperoleh beberapa insight penting sebagai berikut:
* Listing Airbnb tersebar di seluruh distrik Bangkok.
* Lokasi merupakan faktor penting dalam menentukan popularitas listing.
* Host dengan banyak listing cenderung memiliki jumlah review yang lebih besar, meskipun bukan menjadi satu-satunya faktor.
* Jenis kamar berperan besar dalam memengaruhi tingkat popularitas listing.
* Secara umum, harga tidak memiliki pengaruh signifikan terhadap popularitas, namun tetap berpengaruh terhadap preferensi tamu.
* Median harga menunjukkan bahwa tipe kamar Entire home/apt lebih dominan dibandingkan tipe lain.
* Tingkat ketersediaan memiliki hubungan negatif dengan popularitas, meskipun tidak terlalu signifikan.
* Ketersediaan Entire home/apt cenderung rendah, namun justru menandakan tingginya minat tamu terhadap tipe kamar ini.
* Lama sewa minimum tidak berhubungan erat dengan popularitas, namun tetap memengaruhi preferensi tamu karena fleksibilitas menjadi faktor penting.

Karakteristik listing yang populer di kalangan tamu antara lain:

* Berlokasi di area dengan destinasi wisata populer, seperti pasar, pusat perbelanjaan, bangunan bersejarah, dan pusat kota Bangkok.
* Jenis kamar Entire home/apt menjadi yang paling diminati, terutama di 10 lokasi dengan review terbanyak yang umumnya berada di pusat kota.
* Harga yang ditawarkan cenderung lebih rendah dari rata-rata, meskipun bukan faktor utama tamu.
* Lama sewa minimum yang fleksibel lebih menarik bagi tamu, terutama wisatawan yang sering berpindah lokasi.
* Entire home/apt mendominasi dari sisi ketersediaan dibandingkan tipe kamar lain.

