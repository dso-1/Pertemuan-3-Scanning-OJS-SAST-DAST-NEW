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

## Checklist Scanning

### DAST Checklist
- [x] Nikto dijalankan dan output tersimpan
- [ ] ZAP Spider dijalankan
- [ ] ZAP Active Scan dijalankan (unauthenticated)
- [ ] ZAP Active Scan dijalankan (authenticated)
- [ ] SQLMap dijalankan
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

