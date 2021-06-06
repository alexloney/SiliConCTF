# Bounty

> There's this bounty hunter we hired to catch a person of interest. But we haven't heard from him in a while.
> 
> Can you go see if he needs help?
> 
> Bounty

This was the most difficult of the "101" problems. The largest issue for me was referencing the correct site. So even though we're on the host machine and that host machine is in the Star Wars universe, we're still referencing the domain "silicon-ctf.party". Knowing that, we just had to issue a zone transfer to ourselves for that domain (not the hostname of the machine or the ...no-internet domain that the machine has).

When first visiting the page, we receive:

```

           |~
           |.---.
          .'_____`. /\
          |~xxxxx~| ||
          |_  #  _| ||
     .------`-#-'-----.
    (___|\_________/|_.`.
     /  | _________ | | |
    /   |/   _|_   \| | |
   /   /X|  __|__  |/ `.|
  (  --< \\/    _\//|_ |`.
  `.    ~----.-~=====,:=======
    ~-._____/___:__(``/| |
      |    |      XX|~ | |
       \__/======| /|  `.|
       |_\|\    /|/_|    )
       |_   \__/   _| .-'
       | \ .'||`. / |(_|
    LS |  ||.'`.||  |   )
       |  `'|  |`'  |  /
       |    |  |    |\/

=======================================================================

I was hired to collect a bounty for a wanted fugitive and stumbled
on an old imperial base. I checked the computer systems and one of
them still has a nameserver running. Strange, but it looks like it's
listening on UDP and TCP. Maybe you can extract information from it?

=======================================================================
```

Then, when checking the network ports, we see:

```
$ netstat -anp
netstat: can't scan /proc - are you root?
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.11:41229        0.0.0.0:*               LISTEN      -
tcp        0      0 10.1.1.2:53             0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      -
udp        0      0 10.1.1.2:53             0.0.0.0:*                           -
udp        0      0 127.0.0.1:53            0.0.0.0:*                           -
udp        0      0 127.0.0.11:33876        0.0.0.0:*                           -
Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node PID/Program name    Path
unix  2      [ ]         DGRAM                    22585417 - 
```

So, we're listening on port 53, also we're listening as 10.1.1.2 as well. The below zone transfer will work using `127.0.0.1` or `10.1.1.2`

```
$ dig axfr silicon-ctf.party @10.1.1.2

; <<>> DiG 9.12.4-P2 <<>> axfr silicon-ctf.party @10.1.1.2
;; global options: +cmd
silicon-ctf.party.      3600    IN      SOA     ns1.silicon-ctf.party.silicon-ctf.party. contact.silicon-ctf.party. 2021052206 3600 900 86400 3600
silicon-ctf.party.      1800    IN      NS      ns1.silicon-ctf.party.
silicon-ctf.party.      1800    IN      NS      ns2.silicon-ctf.party.
00---SSS----00.silicon-ctf.party. 60 IN A       127.0.0.1
01--S-------01.silicon-ctf.party. 60 IN A       127.0.0.1
02---SSS----02.silicon-ctf.party. 60 IN A       127.0.0.1
03------S---03.silicon-ctf.party. 60 IN A       127.0.0.1
04--SSSS----04.silicon-ctf.party. 60 IN A       127.0.0.1
05----------05.silicon-ctf.party. 60 IN A       127.0.0.1
06----------06.silicon-ctf.party. 60 IN A       127.0.0.1
07--III-----07.silicon-ctf.party. 60 IN A       127.0.0.1
08---I------08.silicon-ctf.party. 60 IN A       127.0.0.1
09---I------09.silicon-ctf.party. 60 IN A       127.0.0.1
10---I------10.silicon-ctf.party. 60 IN A       127.0.0.1
11--III-----11.silicon-ctf.party. 60 IN A       127.0.0.1
12----------12.silicon-ctf.party. 60 IN A       127.0.0.1
13----------13.silicon-ctf.party. 60 IN A       127.0.0.1
14--L-------14.silicon-ctf.party. 60 IN A       127.0.0.1
15--L-------15.silicon-ctf.party. 60 IN A       127.0.0.1
16--L-------16.silicon-ctf.party. 60 IN A       127.0.0.1
17--L-------17.silicon-ctf.party. 60 IN A       127.0.0.1
18--LLLL----18.silicon-ctf.party. 60 IN A       127.0.0.1
19----------19.silicon-ctf.party. 60 IN A       127.0.0.1
20----------20.silicon-ctf.party. 60 IN A       127.0.0.1
21--III-----21.silicon-ctf.party. 60 IN A       127.0.0.1
22---I------22.silicon-ctf.party. 60 IN A       127.0.0.1
23---I------23.silicon-ctf.party. 60 IN A       127.0.0.1
24---I------24.silicon-ctf.party. 60 IN A       127.0.0.1
25--III-----25.silicon-ctf.party. 60 IN A       127.0.0.1
26----------26.silicon-ctf.party. 60 IN A       127.0.0.1
27----------27.silicon-ctf.party. 60 IN A       127.0.0.1
28---CCC----28.silicon-ctf.party. 60 IN A       127.0.0.1
29--C-------29.silicon-ctf.party. 60 IN A       127.0.0.1
30--C-------30.silicon-ctf.party. 60 IN A       127.0.0.1
31--C-------31.silicon-ctf.party. 60 IN A       127.0.0.1
32---CCC----32.silicon-ctf.party. 60 IN A       127.0.0.1
33----------33.silicon-ctf.party. 60 IN A       127.0.0.1
34----------34.silicon-ctf.party. 60 IN A       127.0.0.1
35---OOO----35.silicon-ctf.party. 60 IN A       127.0.0.1
36--O---O---36.silicon-ctf.party. 60 IN A       127.0.0.1
37--O---O---37.silicon-ctf.party. 60 IN A       127.0.0.1
38--O---O---38.silicon-ctf.party. 60 IN A       127.0.0.1
39---OOO----39.silicon-ctf.party. 60 IN A       127.0.0.1
40----------40.silicon-ctf.party. 60 IN A       127.0.0.1
41----------41.silicon-ctf.party. 60 IN A       127.0.0.1
42--N---N---42.silicon-ctf.party. 60 IN A       127.0.0.1
43--NN--N---43.silicon-ctf.party. 60 IN A       127.0.0.1
44--N-N-N---44.silicon-ctf.party. 60 IN A       127.0.0.1
45--N--NN---45.silicon-ctf.party. 60 IN A       127.0.0.1
46--N---N---46.silicon-ctf.party. 60 IN A       127.0.0.1
47----------47.silicon-ctf.party. 60 IN A       127.0.0.1
48----------48.silicon-ctf.party. 60 IN A       127.0.0.1
49----CU----49.silicon-ctf.party. 60 IN A       127.0.0.1
50---RL-----50.silicon-ctf.party. 60 IN A       127.0.0.1
51--YB------51.silicon-ctf.party. 60 IN A       127.0.0.1
52---RA-----52.silicon-ctf.party. 60 IN A       127.0.0.1
53----CE----53.silicon-ctf.party. 60 IN A       127.0.0.1
54----------54.silicon-ctf.party. 60 IN A       127.0.0.1
55----------55.silicon-ctf.party. 60 IN A       127.0.0.1
56--777777--56.silicon-ctf.party. 60 IN A       127.0.0.1
57-----77---57.silicon-ctf.party. 60 IN A       127.0.0.1
58----77----58.silicon-ctf.party. 60 IN A       127.0.0.1
59---77-----59.silicon-ctf.party. 60 IN A       127.0.0.1
60--77------60.silicon-ctf.party. 60 IN A       127.0.0.1
61----------61.silicon-ctf.party. 60 IN A       127.0.0.1
62----------62.silicon-ctf.party. 60 IN A       127.0.0.1
63--H---H---63.silicon-ctf.party. 60 IN A       127.0.0.1
64--H---H---64.silicon-ctf.party. 60 IN A       127.0.0.1
65--HHHHH---65.silicon-ctf.party. 60 IN A       127.0.0.1
66--H---H---66.silicon-ctf.party. 60 IN A       127.0.0.1
67--H---H---67.silicon-ctf.party. 60 IN A       127.0.0.1
68----------68.silicon-ctf.party. 60 IN A       127.0.0.1
69----------69.silicon-ctf.party. 60 IN A       127.0.0.1
70--III-----70.silicon-ctf.party. 60 IN A       127.0.0.1
71---I------71.silicon-ctf.party. 60 IN A       127.0.0.1
72---I------72.silicon-ctf.party. 60 IN A       127.0.0.1
73---I------73.silicon-ctf.party. 60 IN A       127.0.0.1
74--III-----74.silicon-ctf.party. 60 IN A       127.0.0.1
75----------75.silicon-ctf.party. 60 IN A       127.0.0.1
76----------76.silicon-ctf.party. 60 IN A       127.0.0.1
77---5555---77.silicon-ctf.party. 60 IN A       127.0.0.1
78--55------78.silicon-ctf.party. 60 IN A       127.0.0.1
79---5555---79.silicon-ctf.party. 60 IN A       127.0.0.1
80-----55---80.silicon-ctf.party. 60 IN A       127.0.0.1
81--5555----81.silicon-ctf.party. 60 IN A       127.0.0.1
82----------82.silicon-ctf.party. 60 IN A       127.0.0.1
83----------83.silicon-ctf.party. 60 IN A       127.0.0.1
84--III-----84.silicon-ctf.party. 60 IN A       127.0.0.1
85---I------85.silicon-ctf.party. 60 IN A       127.0.0.1
86---I------86.silicon-ctf.party. 60 IN A       127.0.0.1
87---I------87.silicon-ctf.party. 60 IN A       127.0.0.1
88--III-----88.silicon-ctf.party. 60 IN A       127.0.0.1
89----------89.silicon-ctf.party. 60 IN A       127.0.0.1
90----------90.silicon-ctf.party. 60 IN A       127.0.0.1
91---5555---91.silicon-ctf.party. 60 IN A       127.0.0.1
92--55------92.silicon-ctf.party. 60 IN A       127.0.0.1
93---5555---93.silicon-ctf.party. 60 IN A       127.0.0.1
94-----55---94.silicon-ctf.party. 60 IN A       127.0.0.1
95--5555----95.silicon-ctf.party. 60 IN A       127.0.0.1
96----------96.silicon-ctf.party. 60 IN A       127.0.0.1
97----------97.silicon-ctf.party. 60 IN A       127.0.0.1
98--TTTTTT--56.silicon-ctf.party. 60 IN A       127.0.0.1
99----TT----57.silicon-ctf.party. 60 IN A       127.0.0.1
A0----TT----A0.silicon-ctf.party. 60 IN A       127.0.0.1
A1----TT----A1.silicon-ctf.party. 60 IN A       127.0.0.1
A2----TT----A2.silicon-ctf.party. 60 IN A       127.0.0.1
A3----------A3.silicon-ctf.party. 60 IN A       127.0.0.1
A4----------A4.silicon-ctf.party. 60 IN A       127.0.0.1
A5--H---H---A5.silicon-ctf.party. 60 IN A       127.0.0.1
A6--H---H---A6.silicon-ctf.party. 60 IN A       127.0.0.1
A7--HHHHH---A7.silicon-ctf.party. 60 IN A       127.0.0.1
A8--H---H---A8.silicon-ctf.party. 60 IN A       127.0.0.1
A9--H---H---A9.silicon-ctf.party. 60 IN A       127.0.0.1
B0----------B0.silicon-ctf.party. 60 IN A       127.0.0.1
B1----------B1.silicon-ctf.party. 60 IN A       127.0.0.1
B2--33333---B2.silicon-ctf.party. 60 IN A       127.0.0.1
B3-----33---B3.silicon-ctf.party. 60 IN A       127.0.0.1
B4---33-----B4.silicon-ctf.party. 60 IN A       127.0.0.1
B5-----33---B5.silicon-ctf.party. 60 IN A       127.0.0.1
B6--33333---B6.silicon-ctf.party. 60 IN A       127.0.0.1
B7----------B7.silicon-ctf.party. 60 IN A       127.0.0.1
B8----------B8.silicon-ctf.party. 60 IN A       127.0.0.1
B9-W-----W--B9.silicon-ctf.party. 60 IN A       127.0.0.1
C0-W-----W--C0.silicon-ctf.party. 60 IN A       127.0.0.1
C1-W--W--W--C1.silicon-ctf.party. 60 IN A       127.0.0.1
C2--W-W-W---C2.silicon-ctf.party. 60 IN A       127.0.0.1
C3---W-W----C3.silicon-ctf.party. 60 IN A       127.0.0.1
C4----------C4.silicon-ctf.party. 60 IN A       127.0.0.1
C5----------C5.silicon-ctf.party. 60 IN A       127.0.0.1
C6---AA-----C6.silicon-ctf.party. 60 IN A       127.0.0.1
C7--A--A----C7.silicon-ctf.party. 60 IN A       127.0.0.1
C8--AAAA----C8.silicon-ctf.party. 60 IN A       127.0.0.1
C9--A--A----C9.silicon-ctf.party. 60 IN A       127.0.0.1
D0--A--A----D0.silicon-ctf.party. 60 IN A       127.0.0.1
D1----------D1.silicon-ctf.party. 60 IN A       127.0.0.1
D2----------D2.silicon-ctf.party. 60 IN A       127.0.0.1
D3--Y---Y---D3.silicon-ctf.party. 60 IN A       127.0.0.1
D4---Y-Y----D4.silicon-ctf.party. 60 IN A       127.0.0.1
D5----Y-----D5.silicon-ctf.party. 60 IN A       127.0.0.1
D6----Y-----D6.silicon-ctf.party. 60 IN A       127.0.0.1
D7----Y-----D7.silicon-ctf.party. 60 IN A       127.0.0.1
D8----------D8.silicon-ctf.party. 60 IN A       127.0.0.1
D9----------D9.silicon-ctf.party. 60 IN A       127.0.0.1
E0--CU------E0.silicon-ctf.party. 60 IN A       127.0.0.1
E1---RL-----E1.silicon-ctf.party. 60 IN A       127.0.0.1
E2----YB----E2.silicon-ctf.party. 60 IN A       127.0.0.1
E3---RA-----E3.silicon-ctf.party. 60 IN A       127.0.0.1
E4--CE------E4.silicon-ctf.party. 60 IN A       127.0.0.1
E5----------E5.silicon-ctf.party. 60 IN A       127.0.0.1
ns1.silicon-ctf.party.  1800    IN      A       127.0.0.1
ns2.silicon-ctf.party.  1800    IN      A       127.0.0.1
silicon-ctf.party.      3600    IN      SOA     ns1.silicon-ctf.party.silicon-ctf.party. contact.silicon-ctf.party. 2021052206 3600 900 86400 3600
;; Query time: 0 msec
;; SERVER: 10.1.1.2#53(10.1.1.2)
;; WHEN: Sun Jun 06 01:26:29 UTC 2021
;; XFR size: 152 records (messages 1, bytes 4787)
```

If we read the flag embedded into the host records, we receive:

```
silicon{7HI5I5TH3WAY}
```