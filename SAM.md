---
tags:
  - windows
htb:
  - https://academy.hackthebox.com/app/module/147/section/1315
---
Security Accounts Manager

hashed passwords of local (machine) users

`HKLM/SAM`
`%SystemRoot%\System32\config\SAM`

| Registry Hive   | Description                                                                                                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HKLM\SAM`      | Contains password hashes for local user accounts. These hashes can be extracted and cracked to reveal plaintext passwords.                                        |
| `HKLM\SYSTEM`   | Stores the system boot key, which is used to encrypt the SAM database. This key is required to decrypt the hashes.                                                |
| `HKLM\SECURITY` | Contains sensitive information used by the Local Security Authority (LSA), including cached domain credentials (DCC2), cleartext passwords, DPAPI keys, and more. |

[[LSASS]]