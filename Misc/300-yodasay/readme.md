# yodasay

> Our scouts have discovered the location of an ancient Jedi Temple on the planet Ilum. However, it seems as if the Empire has arrived first. We managed to break into one of their systems, but we aren't able to view the contents of the files. Can you find a way?
> 
> Jedi Temple Terminal

This challenge took me a little bit, there's an executable `yodasay` that will echo what you send in then call `cat art.txt` and I was trying to overflow the buffer to cause it to instead `cat artifact.txt`, but try and I might, I couldn't get it to do that.

Eventually, I tried using symbolic links, which changed the execution directory and got the flag.

```
bash-5.0$ ln -s ../officer/yodasay yodasay
bash-5.0$ ln -s ../officer/artifact.txt art.txt
bash-5.0$ ./yodasay hi
hi, I say
silicon{M1nd_tH053_rE14t1v3_p4th5_i_say}
     \     _____      /
      \   /     \    /
         /   s   \ 
    \   |    i    |   /
     \  |    l    |  /
        |    i    |
    \   |    c    |   /
     \  |    o    |  /
        |    n    |
    \   |    {    |   /
     \  |    M    |  /
        |    1    |
    \   |    n    |   /
     \  |    d    |  /
        |    _    |
    \   |    t    |   /
     \  |    H    |  /
        |    0    |
    \   |    5    |   /
     \  |    3    |  /
        |    _    |
    \   |    r    |   /
     \  |    E    |  /
        |    1    |
    \   |    4    |   /
     \  |    t    |  /
        |    1    |
    \   |    v    |   /
     \  |    3    |  /
        |    _    |
    \   |    p    |   /
     \  |    4    |  /
        |    t    |
    \   |    h    |   /
     \  |    5    |  /
        |    _    |
    \   |    i    |   /
     \  |    _    |  /
        |    s    |
    \   |    a    |   /
     \  |    y    |  /
        |    }    |  
        |      .-~|
        |     /   |
       =============
        |         |
        |         |
        |         |
        \---------/
         )-------(
         (-------)
         )-------(
         (-------)
         )-------(
         (-------)
         )-------(--+
        /---------\ |
        | | | | | | |
        | | | | | | |
        | | | | | | |
        | | | | | |-+
        | | | | | |
        | | | | | |
        | | | | | |
        | | | | | |
        | | | | | |
        | | | | | |
        | | | | | |
        | | | | | |
        LAB_|_|_|_|

# https://asciiart.website/index.php?art=movies/star%20wars
```

Which contains the flag

```
silicon{M1nd_tH053_rE14t1v3_P4th5_i_say}
```
