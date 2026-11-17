---
tags:
  - tool
www: http://github.com/ropnop/kerbrute
htb:
  - https://academy.hackthebox.com/app/module/147/section/1326#enumerating-valid-usernames-with-kerbrute
---
Can be used for brute-forcing, password spraying and username enumeration.

```bash
./kerbrute_linux_amd64 userenum --dc 10.129.201.57 --domain inlanefreight.local names.txt
```

[[Kerberos]]