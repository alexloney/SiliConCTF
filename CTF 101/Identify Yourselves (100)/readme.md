# Identify Yourselves

> We need you to join a crew that's headed after an imperial ship. They're a bit inexperienced and the ship has sophisticated defense mechanisms. Please give them a hand.
> 
> Identify Yourselves

Upon first logging into the terminal, you're greated with the following:

```

                                    _,--._
                                _,-::::::::-._
                             ,-'|::::::::::::|`-.
                           ,'   |::::::::::::|   `.
                         ,'o    |::::::::::::|8oo  `.
                        /   8o  |::;;'  `::::|       \
                       /    8   |;'        `:| 8 o8   \
                     ,|       ,'      __      `88      |.
                    ,|       /     ,-'  `-.     \       |.
                   ,|     _,/    ,:........:.    \._     |.
 |`-._        _,-'||   .-:::::. ,|          |. .::::`-.o88||`-._        _,-'|
 |    |______|    ||   |::::::| |            | |::::::|  8||    |______|    |
 | ============== |----|::::::| |            | |::::::|----| ============== |
,| ============== | nnn|::::::| |            | |::::::|nnn | ============== |.
[| ====,-.________|__.-|::::::| |-.--------.-| |::::::|-.__|________,-.==== |]
'|   ||`-.___________| |::::::| | |        | | |::::::| |___________,-'||   |'
 |   HH    ``::::::--|_|::::::| | |        | | |::::::|_|8|::::::''    HH   |
  \  ||        ``::|   |::::::) `||        ||' (::::::|o88|::''        ||  /
   `._      .-.-. ||   `:::::'   `:        ;'   `:::::'8  || .-.-.      _,'
      `-.___|-|-|_||     `::'      `-.__,-'      `::'o88  ||_|-|-|___,-'
                   `|       |                    |888    |'
                    |     o8|                    | 8     |
                    `| 8o   |       ......       | 8ooo |'
                     | 88   |       ::::::       | 8    |
                     `|     `|      ::::::      |'     |'
                      |      |      ::::::      |oo8oo |
                      `|     |      ::::::      |     |'
                       |     |      ::::::      |8Oo  |
                       `|    `|     ::::::     |'    |'
                        `|    |   ::':::::::   |  o |'
                          \o  |   :: :::::::   | 8 /
                           \8 |   :: :::::::   |8 /
                            `.`| :::::::::::  |'8'
                              `|::::::::::::  |'
                               |::::::::::::  |
                               |::::::::::'   |
                               `|:::         |'
                              __|-::       ,-|__
                             (  |  \      /  |  )
                             (  |   )    (   |  )
                              \_|  /      \  '_/
                               H \'        `/ H
                               H `.        ,' H
                               H   `-.__,-'   H

=================================================================================

Listen, we're trying to infiltrate an enemy ship but it's detected us and demanding
that we identify ourselves. The authentication mechanism is an HTTPS server and we
think there may be a weakness we can take advantage of. Some intelligence gathered
helped us build a couple pieces you might need.  Can you make something from them
and craft an authentication response so we can get close enough to board their ship?

=================================================================================
```

Running netstat to find listening ports:

```
$ netstat -anp
netstat: can't scan /proc - are you root?
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.11:40245        0.0.0.0:*               LISTEN      -
tcp        0      0 :::443                  :::*                    LISTEN      -
udp        0      0 127.0.0.11:51566        0.0.0.0:*                           -
Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node PID/Program name    Path
```

If you then curl https://localhost, it will inform you that you must provide a certificate.
Then if you figure out how to do that, it will inform you that it expected a specific
signer for the certificate.

```
$ openssl genrsa -out root.key
$ openssl req -x509 -new -nodes -key root.key -sha256 -days 365 -out cert.pem
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:California
Locality Name (eg, city) []:Folsom
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Silicon CTF
Organizational Unit Name (eg, section) []:Auth CA
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:
$ curl -k --key root.key --cert cert.pem https://localhost
Welcome, OU=Auth CA,O=Silicon CTF,L=Folsom,ST=California,C=US.  Your flag: silicon{D0_0r_DO_NoT_thER3_I5_No_TRY}f827
```

And we have the flag

```
silicon{D0_0r_DO_NoT_thER3_I5_No_TRY}
```
