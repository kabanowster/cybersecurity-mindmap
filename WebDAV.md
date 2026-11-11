---
tags:
  - module
  - python
---
Alternative [[SMB]] share run over [[HTTP]] if there are restrictions on [[SMB]]/445
[RFC 4918](https://datatracker.ietf.org/doc/html/rfc4918)

```shellsession
sudo pip3 install wsgidav cheroot
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
```
```cmd
C:\htb> dir \\192.168.49.128\DavWWWRoot
```

`DavWWWRoot` is a special keyword recognized by the Windows Shell. No such folder exists on your WebDAV server. The DavWWWRoot keyword tells the Mini-Redirector driver, which handles WebDAV requests that you are connecting to the root of the WebDAV server.

You can avoid using this keyword if you specify a folder that exists on your server when connecting to the server. For example: \192.168.49.128\sharefolder

[[HTTP]]