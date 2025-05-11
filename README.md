###  Perbandingan Kinerja

| Aspek                     | Dengan Koherensi (MESI/MI)                     | Tanpa Koherensi                     |
|--------------------------|-----------------------------------------------|-------------------------------------|
| Koherensi Data           | Terjamin                                      | Tidak terjamin                      |
| Latency Akses            | Lebih tinggi (karena traffic koherensi)       | Lebih rendah                        |
| Cache Miss Rate          | Bisa lebih tinggi                             | Bisa lebih rendah                   |
| Traffic Pesan Koherensi  | Banyak                                        | Tidak ada                           |
| Risiko Data Race         | Minim (dengan locking)                        | Tinggi                              |
| Kinerja Program          | Sedang (tergantung workload)                  | Tinggi (tapi tidak aman)            |



##  Penjelasan Setiap Aspek Koherensi Cache

###  Koherensi Data
- **Dengan koherensi**: Data di cache antar-core selalu sinkron, sehingga aman untuk akses bersama antar thread.
- **Tanpa koherensi**: Data bisa berbeda di tiap cache (inkonsistensi), sehingga rawan kesalahan dalam eksekusi paralel.

###  Latency Akses
- **Dengan koherensi**: Membutuhkan sinkronisasi dan broadcast antar cache, yang menambah delay akses memori.
- **Tanpa koherensi**: Akses cache bersifat lokal dan lebih cepat, tetapi data bisa saja tidak akurat.

###  Cache Miss Rate
- **Dengan koherensi**: Invalidasi cache sering terjadi karena sinkronisasi, yang bisa meningkatkan cache miss.
- **Tanpa koherensi**: Cache tetap stabil (tidak terganggu invalidasi), sehingga kemungkinan miss bisa lebih rendah.

###  Traffic Pesan Koherensi
- **Dengan koherensi**: Protokol seperti MESI menghasilkan banyak lalu lintas pesan antar core (INV, REQ, ACK, dsb).
- **Tanpa koherensi**: Tidak ada traffic pesan koherensi karena tidak ada sinkronisasi antar cache.

###  Risiko Data Race
- **Dengan koherensi dan locking**: Risiko data race dapat dikendalikan dengan mekanisme sinkronisasi.
- **Tanpa koherensi**: Data race sangat mungkin terjadi karena tidak ada jaminan konsistensi antar core.

###  Kinerja Program
- **Dengan koherensi**: Performa bisa stabil namun menurun jika traffic koherensi tinggi, tergantung workload.
- **Tanpa koherensi**: Program bisa berjalan lebih cepat, tetapi hasilnya bisa salah jika terjadi inkonsistensi data.

##  Kesimpulan

Penggunaan **protokol koherensi cache** seperti *MESI* pada sistem multi-core sangat penting untuk menjamin **konsistensi data** antar thread. 

Meskipun menyebabkan peningkatan *latency* dan *traffic* pesan koherensi, protokol ini mampu **mengurangi risiko data race** dan menjaga **keandalan hasil program**.

Sebaliknya, sistem **tanpa koherensi cache** bisa memberikan **performa lebih tinggi**, tetapi **berisiko menghasilkan data yang tidak valid** akibat inkonsistensi antar cache.

