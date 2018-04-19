# Redirect http to https, with a health check

This ~5MB image redirects all requests to `https`, except for the `/health` path. This returns a `200` and the string `healthy`.

The presence of the health-check allows, for example, AWS _Elastic Load Balancers_ to check the status of the service.

## Start the container

```
$ docker run -d -p 8080:80 riktytgat/https-redirect-with-healthcheck
Unable to find image 'riktytgat/https-redirect-with-healthcheck:latest' locally
latest: Pulling from riktytgat/https-redirect-with-healthcheck
714d2b8f0606: Already exists
965b9eb96db4: Already exists
70e1fcc37bdd: Already exists
Digest: sha256:7b29482790e57b9022e14930e93200c937ab0d9bfb2320f85de8316499bae4f5
Status: Downloaded newer image for riktytgat/https-redirect-with-healthcheck:latest
646f39ef42a96f779f0e7e6aeed739a6c5004b6af623fe01afe962bd24931c32
```

## Test

### The redirect

```
$ curl  -i 'http://localhost:8080/he'
HTTP/1.1 301 Moved Permanently
Server: nginx/1.12.2
Date: Thu, 19 Apr 2018 07:18:07 GMT
Content-Type: text/html
Content-Length: 185
Connection: keep-alive
Location: https://localhost/he

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.12.2</center>
</body>
</html>
```

### The `health` endpoint

```
$ curl  -i 'http://localhost:8080/health'
HTTP/1.1 200 OK
Server: nginx/1.12.2
Date: Thu, 19 Apr 2018 07:18:00 GMT
Content-Type: text/plain
Content-Length: 8
Connection: keep-alive

healthy
```
