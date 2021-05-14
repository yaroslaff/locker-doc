# HTTP Interface

HTTP Interface to locker

## Get file info (stat)
```
curl -I -H 'X-API-KEY: Zb..KY' http://test.u1.locker.lan/app/etc/keys.json
HTTP/1.1 200 OK
Server: nginx/1.14.2
Date: Mon, 26 Apr 2021 17:13:56 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 0
Connection: keep-alive
X-FileType: FILE
X-FileMTime: 1619449763.69608
X-FileSize: 203
Set-Cookie: session=a0e6df4d-34f5-4497-bd62-ac8c11f25e7c; Expires=Tue, 27-Apr-2021 17:13:56 GMT; Secure; HttpOnly; Path=/
```

## Get file
```
curl -H 'X-API-KEY: Zb...KY' http://test.u1.locker.lan/app/etc/keys.json
```
