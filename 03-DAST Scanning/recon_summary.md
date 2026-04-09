# Recon & Scanning Summary — OJS VPS 10.34.100.178

## Target Information
| Field | Value |
|---|---|
| Target IP | 10.34.100.178 |
| Target Port | 80 |
| Application | Open Journal Systems 3.3.0-8 |
| OS | Ubuntu (Apache/2.4.58) |
| Scan Date | 2026-04-06 |

---

## Nikto Findings

| # | Temuan | Severity | Keterangan |
|---|---|---|---|
| 1 | X-Frame-Options header tidak ada | Medium | Rentan Clickjacking |
| 2 | Cookie OJSSID tanpa httponly flag | Medium | Cookie bisa dicuri via XSS |
| 3 | Server leaks inodes via ETags (/robots.txt) | Low | Info disclosure |
| 4 | /cache/ Directory indexing found (OSVDB-3268) | Medium | File cache bisa diakses publik |
| 5 | /cache/ listed in robots.txt, non-forbidden | Low | Path terekspos |
| 6 | robots.txt berisi 1 entry sensitif | Info | Perlu dicek manual |
| 7 | Apache/2.4.58 version disclosure | Low | Server version terekspos |
| 8 | Root page redirects ke /index.php/JPK | Info | Redirect behaviour |

---

## SSRF Testing — CVE-2021-27188

### Vector 1: Open Redirect via `source` parameter
| Field | Value |
|---|---|
| URL | http://10.34.100.178/index.php/index/login?source=http://127.0.0.1:3306 |
| Method | GET |
| Response | HTTP 200 — Redirect ke halaman Login |
| Result | **Not Vulnerable** via vector ini |
| Evidence | ssrf_redirect_test.txt |

### Vector 2: Stylesheet Upload via Management Settings
| Field | Value |
|---|---|
| URL | http://10.34.100.178/index.php/index/management/settings/website |
| Method | POST |
| Payload | styleSheet[uploadedFile]=http://127.0.0.1:3306/evil.css |
| Response | HTTP 302 → authorizationDenied (noContext) |
| Result | **Authorization barrier present** — No journal context configured |
| Evidence | ssrf_stylesheet_test.txt |

### Kesimpulan SSRF
OJS 3.3.0-8 diinstall tanpa journal aktif sehingga endpoint management
tidak dapat diakses. Vector SSRF CVE-2021-27188 membutuhkan journal
context dan session Editor/Admin yang valid. Authorization control
berfungsi mencegah akses tanpa konteks jurnal.

---

## Gobuster Findings
| #  | Temuan                                                       | Severity | Keterangan                                              |
| -- | ------------------------------------------------------------ | -------- | ------------------------------------------------------- |
| 1  | `/admin` redirect ke halaman login                           | Medium   | Endpoint admin terdeteksi, butuh autentikasi            |
| 2  | `/user` redirect ke profile                                  | Info     | Endpoint user tersedia                                  |
| 3  | `/login` ditemukan (200 OK)                                  | Medium   | Halaman login terbuka untuk akses                       |
| 4  | `/management` redirect ke login                              | Medium   | Panel management terdeteksi                             |
| 5  | `/payments` redirect ke login                                | Medium   | Endpoint pembayaran ada, kemungkinan sensitif           |
| 6  | `/stats` redirect ke login                                   | Medium   | Statistik sistem tersedia (terproteksi)                 |
| 7  | `/submissions` redirect ke login                             | Medium   | Endpoint submission terdeteksi                          |
| 8  | `/search` accessible (200 OK)                                | Info     | Fitur pencarian terbuka                                 |
| 9  | `/issue` accessible (200 OK)                                 | Info     | Endpoint issue dapat diakses                            |
| 10 | `/about` accessible (200 OK)                                 | Info     | Halaman informasi publik                                |
| 11 | `/help` redirect ke dokumentasi                              | Info     | Dokumentasi sistem tersedia                             |
| 12 | `/gateway` redirect ke index                                 | Low      | Endpoint gateway ada, perlu analisis lebih lanjut       |
| 13 | `/install` masih tersedia (redirect)                         | High     | Potensi misconfiguration jika installer belum diamankan |
| 14 | `/information` redirect ke index                             | Low      | Endpoint informasi sistem                               |
| 15 | `/index` accessible (200 OK)                                 | Info     | Entry point aplikasi                                    |
| 16 | `/sitemap` accessible (200 OK)                               | Low      | Struktur situs terekspos                                |
| 17 | Root `/` accessible (200 OK)                                 | Info     | Halaman utama aktif                                     |
| 18 | Multiple endpoints menggunakan parameter `source` pada login | Medium   | Potensi open redirect / parameter tampering             |
| 19 | Cookie `OJSSID` digunakan untuk akses                        | Medium   | Session handling terlihat, perlu cek keamanan cookie    |
| 20 | Banyak endpoint sensitif hanya dilindungi redirect (302)     | Medium   | Bisa diuji bypass authentication                        |

## OWASP Zap Findings

| #  | Temuan                                           | Severity | Keterangan                                                           |
| -- | ------------------------------------------------ | -------- | -------------------------------------------------------------------- |
| 1  | Vulnerable JS Library (High)                     | High     | Library rentan (ua-parser-js) dengan CVE (RCE / prototype pollution) |
| 2  | Application Error Disclosure (Medium)            | Medium   | Error aplikasi terekspos                                             |
| 3  | Content Security Policy (CSP) Header Not Set     | Medium   | Rentan XSS                                                           |
| 4  | Directory Browsing                               | Medium   | File/directory bisa diakses publik                                   |
| 5  | HTTP Only Site (No HTTPS)                        | Medium   | Rentan MITM                                                          |
| 6  | Missing Anti-clickjacking Header                 | Medium   | Rentan clickjacking                                                  |
| 7  | Subresource Integrity Attribute Missing          | Medium   | Risiko supply chain attack                                           |
| 8  | Vulnerable JS Library (Medium)                   | Medium   | Library outdated lainnya                                             |
| 9  | Application Error Disclosure (Low)               | Low      | Error disclosure level rendah                                        |
| 10 | Cookie No HttpOnly Flag                          | Low      | Cookie bisa dicuri via XSS                                           |
| 11 | Cookie without SameSite Attribute                | Low      | Rentan CSRF                                                          |
| 12 | In Page Banner Information Leak                  | Low      | Informasi banner bocor                                               |
| 13 | Information Disclosure - Debug Error Messages    | Low      | Debug info terekspos                                                 |
| 14 | Private IP Disclosure                            | Low      | IP internal bocor                                                    |
| 15 | Server Leaks Version Information                 | Low      | Versi server terekspos                                               |
| 16 | X-Content-Type-Options Header Missing            | Low      | Rentan MIME sniffing                                                 |
| 17 | Authentication Request Identified                | Info     | Endpoint login terdeteksi                                            |
| 18 | Content-Type Header Missing                      | Info     | Header response tidak lengkap                                        |
| 19 | GET for POST                                     | Info     | Penggunaan method tidak aman                                         |
| 20 | Information Disclosure - Suspicious Comments     | Info     | Komentar HTML berisi info                                            |
| 21 | Modern Web Application Detected                  | Info     | Info teknologi aplikasi                                              |
| 22 | Session Management Response Identified           | Info     | Session handling terdeteksi                                          |
| 23 | User Agent Fuzzer                                | Info     | Endpoint merespon variasi user-agent                                 |
| 24 | User Controllable HTML Attribute (Potential XSS) | Info     | Potensi XSS                                                          |

## SQLMap Findings

Tidak ditemukan adanya SQL Injection dengan SQLMap. Berikut beberapa table keterangan singkat dari hasil SQLMap.

| #  | Temuan                                                                           | Severity | Keterangan                                                                      |
| -- | -------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------- |
| 1  | Tidak ditemukan SQL Injection pada parameter `username`                          | Info     | Parameter tidak dinamis dan tidak injectable                                    |
| 2  | Tidak ditemukan SQL Injection pada parameter `password`                          | Info     | Tidak ada indikasi injection dari berbagai teknik (boolean, error, time-based)  |
| 3  | Tidak ditemukan SQL Injection pada parameter `remember`                          | Info     | Parameter statis, tidak dapat dimanipulasi                                      |
| 4  | Tidak ditemukan SQL Injection pada endpoint login (POST)                         | Info     | Semua parameter POST tidak injectable                                           |
| 5  | Tidak ditemukan SQL Injection pada parameter `query` (search)                    | Info     | Parameter GET tidak dinamis dan tidak injectable                                |
| 6  | Tidak ditemukan SQL Injection pada header `User-Agent`                           | Info     | Header injection juga diuji, hasil negatif                                      |
| 7  | Tidak ditemukan SQL Injection pada header `Referer`                              | Info     | Tidak ada indikasi injection                                                    |
| 8  | Tidak ditemukan SQL Injection pada request berbasis ZAP (`request.txt`)          | Info     | Pengujian lebih realistis (full request) tetap tidak injectable                 |
| 9  | Parameter `csrfToken` terdeteksi sebagai anti-CSRF token                         | Info     | SQLMap mengabaikan parameter ini karena proteksi CSRF                           |
| 10 | Parameter `source` tidak injectable                                              | Info     | Parameter kosong & tidak dinamis                                                |
| 11 | Tidak ada teknik SQL Injection yang berhasil (boolean, error, time-based, union) | Info     | Semua teknik gagal di seluruh parameter                                         |
| 12 | Kemungkinan adanya proteksi aplikasi / input validation                          | Low      | Bisa berupa sanitasi input atau ORM                                             |
| 13 | Kemungkinan adanya mekanisme proteksi tambahan (WAF/logic filtering)             | Low      | SQLMap menyarankan penggunaan tamper jika dicurigai                             |
| 14 | Penggunaan cookie session (`OJSSID`) diperlukan untuk testing                    | Info     | Session-based access control terlihat                                           |
| 15 | Parameter tidak menunjukkan perilaku dinamis                                     | Info     | Mengurangi kemungkinan SQL Injection                                            |


## Checklist Scanning

### DAST Checklist
- [x] Nikto dijalankan dan output tersimpan
- [x] ZAP Spider dijalankan
- [ ] ZAP Active Scan dijalankan (unauthenticated)
- [x] ZAP Active Scan dijalankan (authenticated)
- [x] SQLMap dijalankan
- [x] SSRF test dilakukan pada CVE-2021-27188
- [ ] Manual XSS test

### Output Files
| File | Keterangan |
|---|---|
| nikto_output.txt | Nikto basic scan |
| nikto_output.html | Nikto scan dengan autentikasi |
| ssrf_redirect_test.txt | SSRF vector 1 — GET redirect |
| ssrf_stylesheet_test.txt | SSRF vector 2 — POST stylesheet |
| login_response.html | Login response OJS |
| cookies.txt | Session cookie untuk testing |
| Gobuster-1.png | Gobuster tanpa cookies |
| Gobuster-2.png | Gobuster dengan cookies non-admin |
| Context Properties1.png, Context Properties2.png, Context Properties3.png | Properties Context untk autentikasi |
| Spider Process.png | Proses Spider OWASP Zap |
| Active Acan Process.png | Proses Active Scan OWASP Zap |
| Alert Result.png | Alert hasil OWASP Zap |
| Report.html | Laporan hasil generate OWASP Zap |
| 1 - Identifikasi parameter yang rentan.txt | Hasil langkah 1 SQLMap |
| 2 - Test parameter GET (search).txt | Hasil langkah 2 SQLMap |
| 3 - Gunakan request file dari ZAP.txt | Hasil langkah 3 SQLMap |
| Request.txt | Request dari OWASP Zap untuk langkah 3 SQLMap |