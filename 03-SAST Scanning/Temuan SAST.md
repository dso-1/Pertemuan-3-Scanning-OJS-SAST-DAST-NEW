###Temuan #1
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

#Temuan #2
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

#Temuan #3
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

#Temuan #4
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

#Temuan #5
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

#Temuan #6
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

#Temuan #7
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

#Temuan #8
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

#Temuan #9
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

#Temuan #10
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

#Temuan deduplikasi 
