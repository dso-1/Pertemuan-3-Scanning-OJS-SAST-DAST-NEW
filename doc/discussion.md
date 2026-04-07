# Jawaban Pertanyaan Diskusi — Pertemuan 3

**1. Mengapa DAST lebih cocok untuk menemukan Stored XSS dibanding SAST?**
DAST menguji aplikasi saat sedang berjalan (runtime). DAST dapat mensimulasikan penyerang yang memasukkan payload ke database, lalu memeriksa apakah payload tersebut "muncul kembali" dan dieksekusi di halaman lain. SAST seringkali gagal karena tidak bisa melacak aliran data dari database hingga ke tampilan (view) secara dinamis antar sesi atau halaman yang berbeda.

**2. Apa kekurangan utama Semgrep dalam mendeteksi kerentanan business logic?**
Semgrep adalah static analysis yang bekerja berdasarkan pola kode (pattern matching). Semgrep tidak memahami konteks alur kerja bisnis. Contohnya, Semgrep bisa mendeteksi fungsi `deleteAccount()`, tapi ia tidak tahu apakah fungsi tersebut memiliki pengecekan izin (authorization) yang benar untuk mencegah pengguna menghapus akun pengguna lain.

**3. Jelaskan mengapa false positive menjadi masalah serius dalam SAST, terutama dalam pipeline CI/CD!**
*False positive* (temuan yang salah) dalam CI/CD dapat menghentikan proses *build* atau *deployment* secara otomatis. Jika terlalu banyak temuan salah, tim pengembang (Developer) akan kehilangan kepercayaan pada alat tersebut, mengabaikan peringatan keamanan, dan menyebabkan tertundanya pengiriman fitur baru (deployment lag).

**4. Pada kasus SSRF CVE-2021-27188, data apa yang dapat diakses attacker jika SSRF berhasil ke http://127.0.0.1:80?**
Penyerang dapat mengakses layanan internal yang hanya mendengarkan (listening) di *localhost* dan tidak terbuka untuk publik. Data yang dapat diakses meliputi:
- Informasi status server (seperti `/server-status`).
- Metadata dari penyedia cloud (jika di AWS/GCP).
- Layanan database atau cache (seperti Redis/Memcached) yang tidak memerlukan kata sandi jika diakses secara lokal.
- Bypass firewall atau ACL yang hanya membatasi akses dari luar.
