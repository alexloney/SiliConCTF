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

What I had to do to solve this one was create a self-signed x509 certificate and curl the ports.
