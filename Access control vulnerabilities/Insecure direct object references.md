This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.

inspect the request 
```python
GET /download-transcript/1.txt HTTP/2
Host: 0af600f5032c78c481c070fd00f300e1.web-security-academy.net
Cookie: session=4y10Gr3Aq6uuvmhDV3R3qZu5oJZSZCYs
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:145.0) Gecko/20100101 Firefox/145.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Origin: https://0af600f5032c78c481c070fd00f300e1.web-security-academy.net
Referer: https://0af600f5032c78c481c070fd00f300e1.web-security-academy.net/download-transcript/1.txt
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
```

response 
```python
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
Content-Disposition: attachment; filename="1.txt"
X-Frame-Options: SAMEORIGIN
Content-Length: 520

CONNECTED: -- Now chatting with Hal Pline --
You: Hi Hal, I think I've forgotten my password and need confirmation that I've got the right one
Hal Pline: Sure, no problem, you seem like a nice guy. Just tell me your password and I'll confirm whether it's correct or not.
You: Wow you're so nice, thanks. I've heard from other people that you can be a right ****
Hal Pline: Takes one to know one
You: Ok so my password is ww6eoq8ulwt8ag8yadem. Is that right?
Hal Pline: Yes it is!
You: Ok thanks, bye!
Hal Pline: Do one!
```

