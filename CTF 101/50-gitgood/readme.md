# gitgood

> Check out this open server hosting a git repository. Can you find anything worthwile?
> 
> Git Server

When first logging into the machine, there's a `repo` directory immediately in your home directory, which is a git repository.

Checking the git log shows that there were two commits, an initial commit and the next commit that removed the flag:

```
commit c4ef0c87f9e0e7b2d031a78f97e4ac758438fd8c (HEAD -> master)
Author: ctf <ctf@local>
Date:   Mon May 31 23:44:28 2021 +0000

    Remove flag.txt

commit 3b5c27285d41968ee93a85f3020e305a914f86b2
Author: ctf <ctf@local>
Date:   Mon May 31 23:44:27 2021 +0000

    Initial commit
```

Changing git branches:

```
git checkout 3b5c27285d41968ee93a85f3020e305a914f86b2
```

And we now have a flag.txt file containing:

```
silicon{G1t_h15T0rY_4LW4y5_r3m3b3R5}
```
