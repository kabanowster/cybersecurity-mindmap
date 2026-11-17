---
tags:
  - tool
htb:
  - https://academy.hackthebox.com/app/module/24/section/161#file-transfer-with-netcat-and-ncat
---
Current modern implementation: [Ncat](https://nmap.org/ncat/)
nc, ncat, netcat

Networking utility for reading from and writing to network connections using TCP or UDP

#### basic listening on all interfaces
```bash
nc -lnvp 4444
```

Can be used for [[File Transfers]]: [file-transfer-with-netcat-and-ncat](https://academy.hackthebox.com/app/module/24/section/161#file-transfer-with-netcat-and-ncat)