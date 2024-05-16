# Capstone-Module-2-Antonius-Andriyanto-JCDS-2304
## New York City TLC Taxi Trip

### Latar Belakang :
Komisi NYC TLC (Taxi & Limousine Commission) merupakan lembaga pemerintah Kota New York yang bertanggung jawab untuk mengatur seluruh yang berhubungan dengan kendaraan taksi. Tanggung jawab dari komisi NYC TLC termasuk dalam melindungi keselamatan publik, menerbitkan dan mengatur izin serta menetapkan dan membatasi tarif taksi. Di mana Komisi NYC TLC ingin data analyst untuk menganalisa pengoperasian perjalanan taksi selama di bulan Januari 2023. Langkah ini diambil setelah komisi NYC TLC memutuskan untuk menaikkan biaya perjalanan (fares) dari USD 2.50 menjadi USD 3.00. Hasil analisa tersebut tentunya dapat membantu Komisi NYC TLC untuk mendapatkan rekomendasi untuk meningkatkan profit sehingga para pengemudi taksi juga mendapatkan pendapatan yang lebih baik di tengah terjadinya pengunduran diri besar-besaran yang terjadi di Amerika setelah pandemi dan lonjakan inflasi.

### Pernyataan Masalah :
Stakeholder : Business Team NYC TLC
Dalam memberikan rekomendasi untuk meningkatkan profit bagi NYC TLC serta para pengemudi taksi juga mendapatkan pendapatan yang lebih baik maka seorang Data Analyst akan menjawab beberapa pertanyaan :
1. Di wilayah mana pendapatan taksi terbesar dan pendapatan taksi terbesar terjadi di hari apa ? 
2. Di wilayah mana jumlah pickup terbesar dan kapan jumlah pickup terbesar terjadi (weekdays/weekend) ?
3. Pukul berapa penumpang umumnya memerlukan layanan taksi untuk melakukan perjalanan?
4. Perjalanan pickup-dropoff mana yang paling sering dilakukan saat trip ?

### Goals :
Untuk meningkatkan profit bagi NYC TLC dan meningkatkan pendapatan pengemudi taksi maka dalam analisa ini terdiri atas :
1. Mengidentifikasi wilayah dan dispesifikasikan ke dalam zona di setiap wilayah serta waktu (hari) yang memiliki potensi untuk mendapatkan income yang besar.
2. Mengidentifikasi wilayah serta waktu (weekdays / weekend) yang memiliki jumlah permintaan penumpang terhadap layanan taksi.
3. Mengidentifikasi waktu (jam) yang menjadi peak dalam permintaan untuk menggunakan layanan taksi di setiap wilayah.
4. Mengidentifikasi perjalanan yang banyak sering dilakukan oleh penumpang.

### Cleaning and Understanding Data:
- Convert datatype lpep_pickup_datetime dan lpep_dropoff_datetime dari object menjadi datetime.
- Menambah dan menggabungkan data eksternal untuk mendapatkan lokasi serta zona dari data taxi+_zone_lookup.csv
- Menghapus kolom location ID, service zone yang tidak digunakan.
- Menghapus kolom ehail_fee karena memiliki missing value sebesar 100% dan kolom ini hanya dapat diisi jika mengetahui harganya berdasarkan aplikasi.
- Mengisi Null Value di mana PUZone dan DOZone yang isinya Unknown menjadi Unknown (wilayah di luar New York City).
- Menghapus kolom store_and_fwd_flag, RatecodeID, passenger_count, payment_type, trip_type, congestion_surcharge yang terdapat missing value karena tidak bisa dihubungkan dengan kolom lainnya dikarenakan memerlukan data dari pengemudi.
- Mengubah nilai negatif menjadi positif tetapi nilainya menjadi duplikat dengan data yang lainnya sehingga dihapus data tersebut.
- Memperbaiki nilai maksimum pada kolom trip_distance = 1571.97 miles dikarenakan tidak wajar jika dibandingkan dengan data yang identik kode pickup dan kode dropoffnya. Oleh karena itu nilai tersebut diganti dengan nilai mediannya data yang identik kode pickup dan kode dropoffnya dikarenakan dicek pada kolom trip_distance datanya tidak terdistribusi normal.
- Menghapus data dengan kolom trip_distance = 0 dikarenakan taksi itu dianggap tidak berjalan.
- Menghapus data yang total_amount dan fare_amount nya bernilai 0 dikarenakan harga awal ketika membuka pintu taksi adalah USD 3 kecuali apabila payment_typenya = 3 artinya tidak ada biaya sehingga total_amountnya dan fare_amountnya = 0.
- Menambahkan kolom lpep_trip_datetime (dalam jam) dan duration (dalam menit) untuk mengetahui lama perjalanan penumpang menggunakan taksi. Pada data ini diambil data yang kurang dari 4 jam dikarenakan penggunaan taksi melebihi 3 jam dianggap tidak wajar karena terlalu lama dan data yang durationnya 0 menit akan dihapus juga dikarenakan dianggap tidak berjalan.
- Menambahkan kolom PUHour untuk memperlihatkan jam pickup dan kolom DOHour untuk memperlihatkan jam dropoff.
- Menambahkan kolom total_payment yang merupakan akumulasi dari harga perjalanan taksi untuk mengecek kolom total_amount apakah sudah sesuai atau belum dan ketika dicek terdapat data yang tidak sama sehingga untuk proses selanjutnya akan digunakan kolom total_payment.
- Menambahkan kolom PUday yang isinya hari pick up dan akan dimapping menjadi weekdays / weekend di kolom day_mapping.
Setelah dibersihkan serta memfilter data yang lebih relevan dengan realita maka sekarang kita memiliki 60291 baris dari 68211 baris.

### Kesimpulan :

1a. Wilayah dengan pendapatan terbesar berada di wilayah Manhattan. Dapat dilihat dari heatmap juga bahwa pendapatan yang tinggi di Manhattan itu didapatkan pada hari weekday (Senin-Jumat). Di mana seperti diketahui bahwa Manhattan merupakan pusat kota bisnis sehingga tentunya pada hari weekdays merupakan peluang yang besar untuk pengemudi mencari penumpang di sekitaran Manhattan sehingga mendapatkan pendapatan yang maksimal. Jika dilihat saat weekend (Sabtu-Minggu) terlihat bahwa wilayah Brooklyn selain Manhattan juga memiliki prospek untuk meraup keuntungan di mana terlihat pada heatmapnya keuntungan yang didapatkan besar jika dibandingkan saat weekdays.

1b. Apabila dispesifikan ke dalam suatu zona dari setiap wilayah, berikut ini merupakan zona yang memiliki prospek untuk mendapatkan keuntungan yang besar di wilayah tersebut dilihat berdasarkan pendapatannya selama Januari 2023:
- Manhattan     : East Harlem North dan East Harlem South
- Brooklyn     : Fort Greene dan Downtown Brooklyn/Metrotech
- Queens        : Forest Hills dan Elmhurst
- Bronx         : East Concourse/Concourse Village, West Concourse, Mott Haven/Port Morris dan Melrose South
- Staten Island : Saint George/New Brighton dan Bloomfield/Emerson Hill

2. Wilayah dengan permintaan pickup terbanyak juga terdapat pada wilayah Manhattan dan setiap wilayah menunjukkan permintaan tinggi terjadi saat weekdays sedangkan di weekend masih tergolong rendah untuk permintaan pickupnya.

3a. Pada grafik di atas dapat dilihat bahwa permintaan terhadap layanan taksi New York high demand pada saat :
Weekday : mulai pukul 07:00 dan peaknya berada saat pukul 15:00 - 18:00. 
Weekend : mulai pukul 10:00 dan peaknya berada saat pukul 14:00 - 15:00

3b. Jika dispesifikkan ke dalam sebuah wilayah pickup di New York maka high demand pada :
- Manhattan     : 
Weekday : mulai pukul 07:00 dan peaknya pukul 15:00 - 18:00
Weekend : mulai pukul 10:00 dan peaknya pukul 14:00 - 16:00

- Brooklyn      :
Weekday : mulai pukul 12:00 dan peaknya pukul 17:00 - 20:00
Weekend : mulai pukul 11:00 dan peaknya pukul 15:00 - 17:00

- Queens        :
Weekday : mulai pukul 13:00 dan peaknya pukul 17:00 - 20:00
Weekend : mulai pukul 12:00 dan peaknya pukul 16:00 - 18:00

- Bronx         :
Weekday : mulai pukul 09:00 dan peaknya pukul 09:00-10:00, 13:00-14:00 dan 15:00-16:00
Weekend : mulai pukul 10:00 dan peaknya pukul 14:00 - 15:00 dan 17:00-18:00

- Staten Island :
Weekday : mulai pukul 09:00 dan peaknya pukul 09:00-14:00 dan 16:00-17:00

4. Dari data perjalanan pickup-dropoff yang sering dilakukan kita dapat melihat bahwa pickup yang paling banyak terjadi di zona East Harlem North, East Harlem South dan Forest Hills sehingga menandakan ketiga zona ini menjadi titik yang paling difokuskan untuk ditingkatkan distribusi taksi pada area ini sehingga memungkinkan pengemudi mendapatkan profit yang tinggi.

### Recommendation:

1. Fokus pada wilayah Manhattan di zona East Harlem North dan East Harlem South selama weekdays. Hal ini dapat dilakukan dengan cara meningkatkan armada taksi di zona tersebut sedangkan saat weekend mencoba untuk membagi distribusi peningkatan armada taksi antara Manhattan dan Brooklyn dikarenakan dilihat dari data menunjukkan bahwa Brooklyn ketika weekend memiliki potensi untuk meningkatkan penghasilan dikarenakan saat bulan Januari 2023, penghasilan di wilayah Brooklyn saat weekend lebih tinggi jika dibandingkan dengan weekdays.

2. Untuk mempertahankan banyaknya permintaan pickup dari penumpang maka dapat dicoba untuk memberikan penawaran khusus bagi penumpang yang sering naik taksi. Hal ini dapat dilakukan dengan cara membuat program loyalty card dengan teknik mengumpulkan poin berdasarkan miles perjalanan taksi yang ditumpangi sehingga nantinya bisa memperoleh potongan harga untuk perjalanan selanjutnya.

3. Komisi NYC TLC dapat memberikan insentif yang dapat diatur berdasarkan miles perjalanan kepada pengemudi taksi ketika melakukan perjalanan di jam peak hour sehingga pengemudi taksi berlomba lomba untuk mendapatkan penumpang pada peak hour sehingga penumpang juga tidak kesulitan untuk mencari pengemudi taksi saat peak hour.

4. Menyediakan stasiun taksi khusus agar penumpang mudah untuk mencari taksi dan pengemudi juga mudah untuk mencari penumpang di 2 pickup zone yang menjadi tempat pickup terbanyak : di antara East Harlem North dan East Harlem South serta Forest Hills.
