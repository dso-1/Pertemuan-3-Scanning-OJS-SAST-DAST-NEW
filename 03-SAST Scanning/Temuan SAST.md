# 10 TEMUAN
## Temuan #1
| Field                      | Nilai                                                               |
| -------------------------- | ------------------------------------------------------------------- |
| **Nama Kerentanan**        | Insecure Deserialization (unserialize)                              |
| **Tool Penemu**            | SAST                                                                |
| **Tool Spesifik**          | Semgrep                                                             |
| **URL / File**             | ojs-src/classes/migration/upgrade/OJSv3_3_0UpgradeMigration.inc.php |
| **Parameter / Baris Kode** | Line 123                                                            |
| **Method**                 | N/A                                                                 |
| **Payload**                | N/A                                                                 |
| **Response / Bukti**       | Penggunaan `unserialize($serializedValue)`                          |
| **OWASP Category**         | A08: Software and Data Integrity Failures                           |
| **Severity (Raw)**         | Medium                                                              |

## Temuan #2
| Field                      | Nilai                                     |
| -------------------------- | ----------------------------------------- |
| **Nama Kerentanan**        | Insecure Deserialization                  |
| **Tool Penemu**            | SAST                                      |
| **Tool Spesifik**          | Semgrep                                   |
| **URL / File**             | OJSv3_3_0UpgradeMigration.inc.php         |
| **Parameter / Baris Kode** | Line 124                                  |
| **Method**                 | N/A                                       |
| **Payload**                | N/A                                       |
| **Response / Bukti**       | `unserialize($serializedValue)`           |
| **OWASP Category**         | A08: Software and Data Integrity Failures |
| **Severity (Raw)**         | Medium                                    |

## Temuan #3
| Field                      | Nilai                                     |
| -------------------------- | ----------------------------------------- |
| **Nama Kerentanan**        | Insecure Deserialization                  |
| **Tool Penemu**            | SAST                                      |
| **Tool Spesifik**          | Semgrep                                   |
| **URL / File**             | OJSv3_3_0UpgradeMigration.inc.php         |
| **Parameter / Baris Kode** | Line 148                                  |
| **Method**                 | N/A                                       |
| **Payload**                | N/A                                       |
| **Response / Bukti**       | `unserialize($serializedOldValue)`        |
| **OWASP Category**         | A08: Software and Data Integrity Failures |
| **Severity (Raw)**         | Medium                                    |

## Temuan #4
| Field                      | Nilai                                     |
| -------------------------- | ----------------------------------------- |
| **Nama Kerentanan**        | Insecure Deserialization                  |
| **Tool Penemu**            | SAST                                      |
| **Tool Spesifik**          | Semgrep                                   |
| **URL / File**             | OJSv3_3_0UpgradeMigration.inc.php         |
| **Parameter / Baris Kode** | Line 149                                  |
| **Method**                 | N/A                                       |
| **Payload**                | N/A                                       |
| **Response / Bukti**       | `unserialize($serializedOldValue)`        |
| **OWASP Category**         | A08: Software and Data Integrity Failures |
| **Severity (Raw)**         | Medium                                    |

## Temuan #5
| Field                      | Nilai                                          |
| -------------------------- | ---------------------------------------------- |
| **Nama Kerentanan**        | Insecure Deserialization                       |
| **Tool Penemu**            | SAST                                           |
| **Tool Spesifik**          | Semgrep                                        |
| **URL / File**             | ojs-src/lib/pkp/classes/cache/APCCache.inc.php |
| **Parameter / Baris Kode** | Line 51                                        |
| **Method**                 | N/A                                            |
| **Payload**                | N/A                                            |
| **Response / Bukti**       | `unserialize(apc_fetch($key))`                 |
| **OWASP Category**         | A08: Software and Data Integrity Failures      |
| **Severity (Raw)**         | Medium                                         |

## Temuan #6
| Field                      | Nilai                          |
| -------------------------- | ------------------------------ |
| **Nama Kerentanan**        | Unsafe File Deletion           |
| **Tool Penemu**            | SAST                           |
| **Tool Spesifik**          | Semgrep                        |
| **URL / File**             | CacheManager.inc.php           |
| **Parameter / Baris Kode** | Line 135                       |
| **Method**                 | N/A                            |
| **Payload**                | N/A                            |
| **Response / Bukti**       | `unlink($file)`                |
| **OWASP Category**         | A05: Security Misconfiguration |
| **Severity (Raw)**         | Medium                         |

## Temuan #7
| Field                      | Nilai                          |
| -------------------------- | ------------------------------ |
| **Nama Kerentanan**        | Unsafe File Deletion           |
| **Tool Penemu**            | SAST                           |
| **Tool Spesifik**          | Semgrep                        |
| **URL / File**             | FileCache.inc.php              |
| **Parameter / Baris Kode** | Line 60                        |
| **Method**                 | N/A                            |
| **Payload**                | N/A                            |
| **Response / Bukti**       | `unlink($this->filename)`      |
| **OWASP Category**         | A05: Security Misconfiguration |
| **Severity (Raw)**         | Medium                         |

## Temuan #8
| Field                      | Nilai                                     |
| -------------------------- | ----------------------------------------- |
| **Nama Kerentanan**        | Insecure Deserialization                  |
| **Tool Penemu**            | SAST                                      |
| **Tool Spesifik**          | Semgrep                                   |
| **URL / File**             | XCacheCache.inc.php                       |
| **Parameter / Baris Kode** | Line 54                                   |
| **Method**                 | N/A                                       |
| **Payload**                | N/A                                       |
| **Response / Bukti**       | `unserialize(xcache_get($key))`           |
| **OWASP Category**         | A08: Software and Data Integrity Failures |
| **Severity (Raw)**         | Medium                                    |

## Temuan #9
| Field                      | Nilai                         |
| -------------------------- | ----------------------------- |
| **Nama Kerentanan**        | Command Injection (Backticks) |
| **Tool Penemu**            | SAST                          |
| **Tool Spesifik**          | Semgrep                       |
| **URL / File**             | InstallTool.inc.php           |
| **Parameter / Baris Kode** | Line 97                       |
| **Method**                 | N/A                           |
| **Payload**                | Backtick execution            |
| **Response / Bukti**       | `` `/bin/stty -echo` ``       |
| **OWASP Category**         | A03: Injection                |
| **Severity (Raw)**         | High                          |

## Temuan #10
| Field                      | Nilai                         |
| -------------------------- | ----------------------------- |
| **Nama Kerentanan**        | Command Injection (Backticks) |
| **Tool Penemu**            | SAST                          |
| **Tool Spesifik**          | Semgrep                       |
| **URL / File**             | InstallTool.inc.php           |
| **Parameter / Baris Kode** | Line 104                      |
| **Method**                 | N/A                           |
| **Payload**                | Backtick execution            |
| **Response / Bukti**       | `` `/bin/stty echo` ``        |
| **OWASP Category**         | A03: Injection                |
| **Severity (Raw)**         | High                          |

## Temuan Deduplikasi

| No | Kerentanan | Rule Semgrep | Jumlah Awal | Setelah Deduplikasi | Status | Keterangan |
|----|-----------|-------------|------------|---------------------|--------|------------|
| 1 | Insecure Deserialization | unserialize-use | 6+ | 1 | Potensi | Banyak muncul pada fungsi yang sama |
| 2 | Unsafe File Deletion | unlink-use | 2 | 1 | Potensi | Pola penghapusan file serupa |
| 3 | Command Injection | backticks-use | 2 | 1 | Valid | Eksekusi command langsung |

### Analisis Deduplikasi

Dari total 38 temuan yang dihasilkan oleh Semgrep, ditemukan beberapa temuan yang memiliki pola serupa (duplikasi), seperti penggunaan fungsi `unserialize()` yang muncul di beberapa file dan baris berbeda. Temuan tersebut dikelompokkan menjadi satu kategori kerentanan yang sama yaitu Insecure Deserialization.

Selain itu, beberapa temuan seperti penggunaan `unlink()` juga muncul lebih dari satu kali dengan pola yang sama sehingga dikategorikan sebagai satu jenis kerentanan.

Proses deduplikasi ini bertujuan untuk menghilangkan redundansi serta mengidentifikasi kerentanan unik yang lebih representatif terhadap kondisi keamanan sistem. Temuan dengan tingkat risiko tinggi seperti penggunaan backticks tetap dipertahankan sebagai kerentanan valid.

### Identifikasi False Positive

Beberapa temuan dikategorikan sebagai false positive karena tidak melibatkan input pengguna secara langsung, seperti penggunaan `unserialize()` terhadap data internal sistem. Oleh karena itu, meskipun terdeteksi oleh tool, temuan tersebut tidak seluruhnya dianggap sebagai kerentanan yang dapat dieksploitasi.
