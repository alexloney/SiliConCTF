# new login who dis?

> Our hacker team was able to perform a man-in-the-middle attack on one of the empire generals. The attack generated this pcap file. Can you look through it to see if there is anything interesting?

For this challenge, we receive a PCAP dump of some network traffic. After sifting through the file, I quickly found the following HTTP request:

```
GET /for100/secret HTTP/1.1
Host: 13.64.74.211
Connection: keep-alive
Cache-Control: max-age=0
Authorization: Basic bW9mZjpHIWQzMG4=
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86\_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.72 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,\*/\*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9

HTTP/1.1 200 OK
Date: Tue, 27 Apr 2021 18:58:13 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 46
Connection: keep-alive

flag{this_is_not_the_flag_you_are_looking_for}
```

True to the name above, entering that flag will not work.

I tried to access the same host (`13.64.74.211/for100/secret`) both with and without the basic auth (which is `moff:G!d30n`). But was unsuccessful in accessing it.

It took quite a bit of time (and a lot of scouering through the rest of the network traffic) to realize that we need to replace the IP with `http://challenges.silicon-ctf.party/for100/secret`

```bash
$ curl -u 'moff:G!d30n' https://challenges.silicon-ctf.party/for100/secret
silicon{Base64I$n73ncrypti0n}
```

Giving the flag:

```
silicon{Base64I$n73ncrypti0n}
```