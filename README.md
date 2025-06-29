# Pemrograman Jaringan - Tugas 1

Repositori ini berisi solusi untuk Tugas 1 dari mata kuliah Pemrograman Jaringan. Tugas ini mencakup serangkaian percobaan untuk memahami konsep dasar soket TCP, komunikasi client-server, dan analisis lalu lintas jaringan menggunakan Wireshark.

## Deskripsi Tugas

Tugas ini terdiri dari empat skenario percobaan yang dijalankan dalam lingkungan lab virtual menggunakan Docker. Tiga mesin virtual (mesin-1, mesin-2, mesin-3) disediakan untuk menyimulasikan interaksi jaringan.

Tujuan utama dari setiap percobaan adalah:
1.  Menjalankan skrip Python untuk melakukan operasi jaringan Hasil Analisis: Log server dan capture Wireshark secara definitiftertentu.
2.  Menangkap lalu lintas jaringan yang dihasilkan menggunakan Wireshark.
3.  Menganalisis hasil tangkapan untuk memahami protokol dan proses yang terjadi di "balik layar".

## Rangkuman Percobaan

### Soal 1: Analisis Resolusi Nama Domain (DNS)

-   **Tujuan:** Memahami bagaimana sebuah nama domain (contoh: `www.its.ac.id`) diubah menjadi alamat IP sebelum koneksi dapat dibuat.
-   **Proses:** Menjalankan skrip `socket_info.py` yang menggunakan `socket.getaddrinfo()`.
-   **Hasil Analisis:** Wireshark menunjukkan adanya transaksi **DNS (Domain Name System)**. Klien mengirimkan paket "Standard query" untuk menanyakan alamat IP, dan server DNS merespons dengan alamat IP yang sesuai. Ini membuktikan bahwa resolusi nama adalah langkah awal yang fundamental dalam komunikasi jaringan berbasis nama.

### Soal 2: Komunikasi Client-Server Dasar

-   **Tujuan:** Mensimulasikan siklus hidup lengkap dari komunikasi TCP antara satu client dan satu server.
-   **Proses:** Menjalankan `server.py` di Mesin-1 dan `client.py` di Mesin-2. `client.py` dimodifikasi untuk menunjuk ke alamat IP Mesin-1.
-   **Hasil Analisis:** Wireshark dengan jelas menampilkan tiga fase utama dari koneksi TCP:
    1.  **Three-Way Handshake (`SYN`, `SYN-ACK`, `ACK`)** untuk membangun koneksi.
    2.  **Transfer Data (`PSH, ACK`)** di mana client mengirim pesan dan server menggemakannya kembali (echo).
    3.  **Four-Way Handshake (`FIN, ACK`)** untuk menutup koneksi secara baik-baik.

### Soal 3: Komunikasi dengan Port Kustom dan Pengiriman File

-   **Tujuan:** Menunjukkan fleksibilitas TCP untuk beroperasi pada port non-standar dan kemampuannya untuk mentransfer data dari sebuah file.
-   **Proses:** Server dan client dimodifikasi untuk berkomunikasi pada port `32444`. Client juga diubah untuk membaca konten dari sebuah file teks dan mengirimkannya.
-   **Hasil Analisis:** Wireshark mengkonfirmasi bahwa seluruh komunikasi terjadi pada port `32444`. Lebih penting lagi, dengan memeriksa *payload* dari paket data, konten dari file teks terlihat jelas, membuktikan bahwa soket TCP berhasil mentransmisikan data file melalui jaringan.

### Soal 4: Uji Coba Server Iteratif dengan Multiple Client

-   **Tujuan:** Mengamati dan memahami perilaku server tipe **iteratif** saat dihadapkan dengan beberapa koneksi client secara bersamaan.
-   **Proses:** Satu server di Mesin-1 dijalankan, sementara dua client (dari Mesin-2 dan Mesin-3) dijalankan secara serentak.
-   **Hasil Analisis:** Log server dan capture Wireshark secara definitif menunjukkan bahwa server melayani client **satu per satu secara berurutan (sekuensial)**. Sesi TCP lengkap untuk client pertama harus selesai sebelum sesi untuk client kedua dimulai. Ini menyoroti batasan server iteratif dan kebutuhan akan model yang lebih canggih (seperti *threading*) untuk menangani koneksi konkuren.

## Lingkungan Lab
- **Repository**: Clone repository berikut https://github.com/rm77/progjar
-   **Teknologi:** Docker & Docker Compose
-   **Mesin:** 3 kontainer Docker (`mesin-1`, `mesin-2`, `mesin-3`)
-   **Jaringan Internal:**
    -   `mesin-1`: `172.16.16.101`
    -   `mesin-2`: `172.16.16.102`
    -   `mesin-3`: `172.16.16.103`
-   **Alat Analisis:** Wireshark (diakses melalui noVNC) atau `tcpdump`.

## Cara Menjalankan Ulang Percobaan

1.  Pastikan Docker dan Docker Compose sudah terinstall.
2.  Clone repositori lingkungan lab dan jalankan dengan `docker-compose up -d`.
3.  Akses terminal dari setiap mesin melalui Jupyter Lab.
4.  Clone repositori ini (`git clone ...`) di dalam direktori `work/` pada setiap mesin.
5.  Ikuti langkah-langkah yang dijelaskan untuk setiap soal, termasuk modifikasi kode dan penggunaan Wireshark pada interface yang sesuai (`eth0` untuk Soal 1, `eth1` untuk Soal 2, 3, dan 4).

**Nama:** Muhammad Rifqi Ma'ruf   
**NRP:** 5025221060
