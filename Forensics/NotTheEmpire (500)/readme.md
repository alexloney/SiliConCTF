# NotTheEmpire


> We need your help Rebel. We have found some anomalous network traffic in our enviroment. We think that it is some sort of Command and Control Traffic associated with Empire malware.
> 
> The network traffic we are seeing is going out to Here.
> 
> We think there is something hidden on this site that the malware has been reaching out to.
> 
> We also recovered some samples of what we think to be early development versions of the malware. They are stored here


This one ended up being a bit simpler than I had expected. Basically there's a "malware" that will do "something" and you have to figure out what. I eventually created a limited user on my Kali VM and just executed it.

Since I knew I was looking for network calls specifically, I used `strace` to locate just network calls

```
$ strace -f -e trace=network -s 10000 ./stager 2>trace2.txt
```

In the output file, I noticed an attempted connection to localhost:8235

```
...
[pid 31573] getpeername(6, {sa_family=AF_INET, sin_port=htons(8235), sin_addr=inet_addr("127.0.0.1")}, [112->16]) = 0
...
```

I then created a Python server to catch and proxy this to the website that it was supposed to go to, but that never really ended up being what was needed. So instead, here's a netcat output of what the request is:

```
$ nc -lnvp 8235
listening on [any] 8235 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 37280
GET /c2 HTTP/1.1
Host: localhost:8235
User-Agent: Go-http-client/1.1
Auth_signature: e257bd8ad74764d6163c5ffa914f232a
Auth_timestamp: 1622937316
Accept-Encoding: gzip
```

Given that we know (from the problem) that the request is meant to go to https://challenges.silicon-ctf.party/for500/index.html:

```
$ curl -H 'User-Agent: Go-http-client/1.1' -H 'Auth_signature: e90070b7dcd794caaa768512b13f2f70' -H 'Auth_timestamp: 1622777888' -H 'Accept-Encoding: gzip' https://challenges.silicon-ctf.party/for500/c2
<!DOCTYPE html>
<!--
Template Name: Wavefire
Author: <a href="https://www.os-templates.com/">OS Templates</a>
Author URI: https://www.os-templates.com/
Copyright: OS-Templates.com
Licence: Free to use under our free template licence terms
Licence URI: https://www.os-templates.com/template-terms
-->
<html lang="en">

<head>
<title>Totally Normal Website</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

</head>
<body style="background-image: url('static/images/404.png'); background-size: auto; background-repeat: no-repeat; background-position: center fixed;">

</body>
</html>
```

Well, that didn't seem to work.

After a bit of thought and playing around, the `Auth_signature` and `Auth_timestamp` looked strange, specifically the underscore. So changing them to dashes worked instantly!

```
$ curl -H 'User-Agent: Go-http-client/1.1' -H 'Auth-signature: e90070b7dcd794caaa768512b13f2f70' -H 'Auth-timestamp: 1622777888' -H 'Accept-Encoding: gzip' https://challenges.silicon-ctf.party/for500/c2
silicon{H3r3_7s_UR_P@yl0ad}
```

Giving the flag:

```
silicon{H3r3_7s_UR_P@yl0ad}
```