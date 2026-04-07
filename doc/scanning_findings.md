# Tabel Temuan Raw Scanning (SAST & DAST)

Daftar temuan hasil pemindaian menggunakan OWASP ZAP, Nikto, SQLMap, dan Semgrep pada target `10.34.100.178`.

| No | Tool | Temuan (Vulnerability) | Kategori | Lokasi / Parameter |
|:---|:---|:---|:---|:---|
| 1 | SQLMap | SQL Injection (Time-based) | A03: Injection | `/index.php/index/login` (username) |
| 2 | ZAP | Reflected XSS | A03: Injection | `/index.php/JPK/search` (query) |
| 3 | Nikto | Outdated OJS Version (3.3.0-8) | A06: Outdated | Header `X-Powered-By` / Fingerprinting |
| 4 | Semgrep | Insecure File Upload | A08: Integrity | `classes/file/FileManager.inc.php` |
| 5 | ZAP | Missing Anti-clickjacking Header | A05: Misconfig | Seluruh halaman (X-Frame-Options) |
| 6 | Nikto | Exposed `/server-status` | A05: Misconfig | `http://10.34.100.178/server-status` |
| 7 | Semgrep | Hardcoded Credentials | A07: Auth | `config.inc.php` (DB Password) |
| 8 | ZAP | Cookie No HttpOnly Flag | A05: Misconfig | Cookie: `OJSSID` |
| 9 | Nikto | PHP 7.4.33 End-of-Life | A06: Outdated | Server Banner |
| 10 | Semgrep | Unsafe Use of `eval()` | A03: Injection | `lib/pkp/classes/core/PKPComponentRouter.inc.php` |
