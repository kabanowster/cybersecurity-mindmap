---
htb:
  - https://academy.hackthebox.com/app/module/134/section/1179
---
Insecure Direct Object References

IDOR is among the most common web vulnerabilities and can lead to accessing data that should not be accessible by attackers. What makes this attack very common is essentially the lack of a solid access control system on the back-end. As web applications store users' files and information, they may use sequential numbers or user IDs to identify each item. Suppose the web application lacks a robust access control mechanism and exposes direct references to files and resources. In that case, we may access other users' files and information by simply guessing or calculating their file IDs.

[[IDOR Information Disclosure Vulnerabilities]]
[[IDOR Insecure Function Calls]]
[[Object-Level Access Control]]