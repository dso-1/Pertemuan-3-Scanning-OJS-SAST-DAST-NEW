# Hasil Diskusi Pertemuan 3

---

### 1. Mengapa DAST lebih cocok untuk menemukan *Stored XSS* dibanding SAST?

**Jawaban:**
SAST (Static Application Security Testing) menganalisis *source code* secara pasif tanpa menjalankannya. Untuk kasus *Stored XSS*, input berbahaya biasanya disimpan terlebih dahulu di database dan baru dirender/dieksekusi di halaman lain atau pada waktu yang berbeda. SAST sering kesulitan melacak aliran data kompleks (Data Flow Analysis) dari database ke *template engine* (sink).

Sebaliknya, DAST (Dynamic Application Security Testing) berinteraksi langsung dengan aplikasi yang sedang berjalan. DAST menyuntikkan *payload* (seperti `<script>alert(1)</script>`) ke form input, lalu melakukan *crawling* ke halaman lain untuk melihat apakah *payload* tersebut berhasil tereksekusi di browser. Oleh karena itu, DAST lebih akurat memvalidasi kerentanan *Stored XSS*.

---

### 2. Apa kekurangan utama Semgrep dalam mendeteksi kerentanan *business logic*?

**Jawaban:**
Kekurangan utama Semgrep (dan tools SAST pada umumnya) dalam mendeteksi kerentanan logika bisnis (*business logic flaws*) adalah mereka bekerja berbasis **pencocokan pola (Pattern Matching)** seperti yang ada di `custom_rules.yaml`. 

Kerentanan logika bisnis (seperti *Privilege Escalation*, IDOR, atau bypass alur otorisasi) tidak memiliki "sintaks kode yang salah" secara universal. Kode yang ditulis mungkin sempurna dan aman secara sintaksis (tidak ada injeksi eval atau SQL), namun alur logikanya melanggar aturan bisnis aplikasi itu sendiri. Semgrep tidak memiliki konteks bisnis untuk mengetahui apakah sebuah *user* Boleh atau Tidak Boleh mengakses sebuah fungsi.

---

### 3. Jelaskan mengapa *false positive* menjadi masalah serius dalam SAST, terutama dalam *pipeline* CI/CD!

**Jawaban:**
*False positive* terjadi ketika tool SAST menandai kode aman sebagai kerentanan. Dalam ekosistem CI/CD (Continuous Integration/Continuous Deployment), proses ini menjadi masalah yang sangat serius karena:

1. **Memblokir Deployment (Build Failure):** Pipeline CI/CD biasanya disetel untuk menggagalkan *build* (gagal otomatis) jika ditemukan kerentanan dengan tingkat *High* atau *Critical*. *False positive* akan menghentikan rilis perangkat lunak yang sebenarnya aman.
2. **Alert Fatigue (Kelelahan Peringatan):** Developer harus menghabiskan waktu berjam-jam untuk meninjau peringatan palsu. Hal ini membuat mereka menjadi abai dan berpotensi melewatkan *True Positive* (kerentanan asli) yang terselip di antara ribuan temuan palsu.

---

### 4. Pada kasus SSRF CVE-2021-27188, data apa yang dapat diakses *attacker* jika SSRF berhasil ke `http://127.0.0.1/`?

**Jawaban:**
Alamat `127.0.0.1` (*localhost*) merepresentasikan sistem server itu sendiri. Jika kerentanan SSRF (Server-Side Request Forgery) berhasil diarahkan ke `127.0.0.1`, server aplikasi akan dipaksa membuat *request* HTTP internal yang akan melewati perlindungan *firewall* eksternal. 

Data/layanan yang bisa diakses oleh *attacker* meliputi:
1. **Layanan Internal (Internal Services):** Mengakses *port* yang tidak diekspos ke publik, seperti panel admin lokal (misalnya phpMyAdmin di port 8080/lokal), Redis, Memcached, atau database internal (MySQL di port 3306).
2. **File Sistem:** Jika skema URL lokal (seperti `file:///etc/passwd`) didukung, *attacker* bisa membaca file sensitif di dalam OS server.
3. **Cloud Metadata (Jika menggunakan VPS AWS/GCP):** Meskipun IP-nya berbeda (biasanya `169.254.169.254`), SSRF sering digunakan untuk mencuri token akses dan rahasia infrastruktur server *cloud*.
