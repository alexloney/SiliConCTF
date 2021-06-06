# Putting the Pieces Together

> Your hacker team was deep inside the empire network and found a highly important encrypted file and a screenshot of the private key used to decrypt the file. How lucky! Unfortunately, when exfiltrating the private key, your team was discovered and had their access cut off. We only got a portion of the key.
> 
> Is there anything we can do to decrypt the file?

This problem wasn't that hard, more tedious. Once the input file was crafted correctly, the rest just flowed together.

Basically, we're given a screenshot of a private SSH key and a file that was encrypted with ssh-vault (which is a program that can encrypt using a private SSH key). The image of the private key has some sections blacked out, which turns out to only be the public modulus that's blacked out.

Once you pain-stakingly type the private key into a file, you're just left with determining the public modulus and then using the ssh-vault program to decrypt the vault.

So to start with, we have to get the key into text to work with instead of an image. Additionally, we have a blacked out spot, but we can count and see exactly how many rows/columns there are, and replace those with AAAAA... values (which will evaluate to NULL when base64 decoding). So the constructed file is:

```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFmBWCwR4VgsEeAAAAB3NzaC1yc2
EAAAGBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMBAAEAAAGAS7YT/eG8rFlG0A3ecucUeOqj5Z
WfLSmL07zZz6DKdm/wgWbDs3upSxxJ6p8UX4U5EH7aHGI2EKORFt3euPxvQmtdon5EymVS
UW8BbD8bcyngCWOIwYrspvPT2vdfhkdglH3wueg2pSH9jV5E+c5PHHKh/fq1gI+vaMTsST
ThDzRjW+f173bu9BS0+QH9h6/o8DT92FTuAtdnOSubV/RIxKLorIgJXx/Z+3lDwc8ukodv
wEm5vS795yWyXi/gd2EoGgMSxjBTKdt37uZIqYVLY7J1XT6+YBprLZ7UC4/cy5mn3tT56J
0goBI2h/9BWdi2tM9aKiVT4c+e/JH22pnU4mrPkTLEwUdRlmyIKZQQ74361gwKYkG1TzDC
JgGhdmLcQAmb7kzuL8BYGReLkk9F3Vi5JnbOi6rzsTxk7tsGpuhevw8bq4FZGJxHEnbEfh
ECAXLx5XPHWRLMtpM6KvPSizokv5qshYChf4gGVszW854xO6etDacSCuugLWjxMHphAAAA
wQCJ2z9Pb1JMGVNwKsTRWAf46LGMSMXg3iSKfJJ9EaQGdkGKQj+dvkgJRg0vNDTe+6FzCL
AVZSdHOjwxKutBtpN4YfiryaUYQ93pbReASP/7JDtUmxthY0rTqYLjx+jgd199/RABymHb
MJtB40QQj2KKqJQHcIOJERHcn3S+hgYKLRElt/sndcSQThruHISTErwVHVl8vJvUfPpBXa
IT/zg845yDc5FVMJmY08kFSEIX2pc2p2ZLsxQxgq3CZUmmXiYAAADBAOI75l6d0XqvkHqt
r20D7Vljfxt5ppXeq88Ox9GM6DctnrtMUehmPuqAE0Av+w+XRW+97/EnU6I/T2WEAE0FZA
kWxpAgnq6VQCHuRtoQOUPX+uG0HdPLQHi/yOe0iiieD7m1LBE6RJ4SYTe8z67Wnvz7hzZl
0OB/YwGzEGqXefvxzpu2abVF0iTGXUnt6pil/9rCfCPfijKHDKipM8NX4tDgTlVUQVT2mG
A3z7Say1EBTBwPx7larjAkUEAsOhqhTQAAAMEAwwmJC+FnqlRyfIG1MfwMncid2hfrYSFY
ki6AQK8My6u+ut3UZUwTdf9tjdvdMr3C4yeaJpP+Eoynxe2y/phA7cK6EB4MChPkSfObA0
vh89DQCgU5aYAbm7H6fuOoJlTESBowFhFH8RNHRkWVtGG982JVKVONqj+AnYcEUFg6WdOf
wiE2zjPcNsOhAw+/QlL0whzriJZeDW+PbSauGZ6MvuKgeE3f2uCUDTsDb/JjmEW61oHobm
Ujy4SzjvOCqa1rAAAAHXJvb3RAbG9jYWxob3N0LmxvY2FsAQIDBAU=
-----END OPENSSH PRIVATE KEY-----
```

Next, we need to determine what is missing from the file. There are a few good resources to locate this information, such as [this](https://dnaeon.github.io/openssh-private-key-binary-format/) link, which will give you the definitions. So given the definition, we can base64 decode this and get all of the components that we have. That includes the following:

P:
```
E23BE65E9DD17AAF907AADAF6D03ED59637F1B79A695DEABCF0EC7D18CE8372D9EBB4C51E8663EEA8013402FFB0F97456FBDEFF12753A23F4F6584004D05640916C690209EAE954021EE46DA103943D7FAE1B41DD3CB4078BFC8E7B48A289E0FB9B52C113A449E126137BCCFAED69EFCFB873665D0E07F6301B3106A9779FBF1CE9BB669B545D224C65D49EDEA98A5FFDAC27C23DF8A32870CA8A933C357E2D0E04E55544154F6986037CFB49ACB51014C1C0FC7B95AAE302450402C3A1AA14D
```

Q:
```
C309890BE167AA54727C81B531FC0C9DC89DDA17EB612158922E8040AF0CCBABBEBADDD4654C1375FF6D8DDBDD32BDC2E3279A2693FE128CA7C5EDB2FE9840EDC2BA101E0C0A13E449F39B034BE1F3D0D00A053969801B9BB1FA7EE3A82654C4481A30161147F11347464595B461BDF3625529538DAA3F809D870450583A59D39FC22136CE33DC36C3A1030FBF4252F4C21CEB88965E0D6F8F6D26AE199E8CBEE2A0784DDFDAE0940D3B036FF2639845BAD681E86E6523CB84B38EF382A9AD6B
```

For RSA, the modulus (which is the missing piece) is just P * Q, and Python can easily compute this.

```python
>>> p = int("E23BE65E9DD17AAF907AADAF6D03ED59637F1B79A695DEABCF0EC7D18CE8372D9EBB4C51E8663EEA8013402FFB0F97456FBDEFF12753A23F4F6584004D05640916C690209EAE954021EE46DA103943D7FAE1B41DD3CB4078BFC8E7B48A289E0FB9B52C113A449E126137BCCFAED69EFCFB873665D0E07F6301B3106A9779FBF1CE9BB669B545D224C65D49EDEA98A5FFDAC27C23DF8A32870CA8A933C357E2D0E04E55544154F6986037CFB49ACB51014C1C0FC7B95AAE302450402C3A1AA14D", 16)
>>> q = int("C309890BE167AA54727C81B531FC0C9DC89DDA17EB612158922E8040AF0CCBABBEBADDD4654C1375FF6D8DDBDD32BDC2E3279A2693FE128CA7C5EDB2FE9840EDC2BA101E0C0A13E449F39B034BE1F3D0D00A053969801B9BB1FA7EE3A82654C4481A30161147F11347464595B461BDF3625529538DAA3F809D870450583A59D39FC22136CE33DC36C3A1030FBF4252F4C21CEB88965E0D6F8F6D26AE199E8CBEE2A0784DDFDAE0940D3B036FF2639845BAD681E86E6523CB84B38EF382A9AD6B", 16)
>>> hex(p * q)
'0xac5c0db1b997e7702e777ca92df8741a07956ee2e902beceec9ec3ad495442654b87201822fede6004bd70298989c73c211467ae4c36a9545a4c9bfd7ff98743924d2b2e7beedfb55cbe65e708c87ead8a6ef0b0308cfc23b788549d74e6216d0ebfa0e22a30fe0c9da4679a0e5418b5d9ed02c7dd869bde7f2ac3d577db53e3482cc6ed092f5481a6a815d4bbc15f954a378b29bfac8cc3ba00aa59e1e7767e3c84511b55c0b2133cd192e5bd5831b5ad9dd6c1a2ab7a06817d5956513c72d2af6fe0aeed25d04f78e70533e34ffb77e1e31e82d735c9f1995a0986315c3b49164107b04272a0528231ae30ebcc0b1f47beefbb07e278529b41eede0e5589e5bcd0969725d4b0d9d8887b3a3354570eefbc83198b95e94d6018c238cac35f8dfea928384e2f835c38c747fa441f12f54346c1f816bf54fd8df0c10fe0c73d77cbb2bacdbf2be4de6a27b187742fe46e9afef338445d6403e4b25237ff971ed80bec4b15bc06bb3dedfa54379c236d136c0e12b54a0a4d49a5336954ddf7742f'
```

Taking that value for the modulus and placing it into the correct missing location (make sure that you preceed it with a NULL byte (0x00) because the first byte (0xac) has a value >= 0x80). I also ran into some issues with getting lengths of things correct, so I used the [Python OpenSSH Key Parser](https://pypi.org/project/openssh-key-parser/) library to validate the key structure as I made minor tweaks to get everyting correct.

Once it's all in place, you should have the following private key:

```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

Then extracting a public key from it using `ssh-keygen -f id_rsa -e > id_rsa.pub` and a very little bit of massaging of the output format, you get this public key:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCsXA2xuZfncC53fKkt+HQaB5Vu4ukCvs7snsOtSVRCZUuHIBgi/t5gBL1wKYmJxzwhFGeuTDapVFpMm/1/+YdDkk0rLnvu37VcvmXnCMh+rYpu8LAwjPwjt4hUnXTmIW0Ov6DiKjD+DJ2kZ5oOVBi12e0Cx92Gm95/KsPVd9tT40gsxu0JL1SBpqgV1LvBX5VKN4spv6yMw7oAqlnh53Z+PIRRG1XAshM80ZLlvVgxta2d1sGiq3oGgX1ZVlE8ctKvb+Cu7SXQT3jnBTPjT/t34eMegtc1yfGZWgmGMVw7SRZBB7BCcqBSgjGuMOvMCx9Hvu+7B+J4UptB7t4OVYnlvNCWlyXUsNnYiHs6M1RXDu+8gxmLlelNYBjCOMrDX43+qSg4Ti+DXDjHR/pEHxL1Q0bB+Ba/VP2N8MEP4Mc9d8uyus2/K+Teaiexh3Qv5G6a/vM4RF1kA+SyUjf/lx7YC+xLFbwGuz3t+lQ3nCNtE2wOErVKCk1JpTNpVN33dC8= root@localhost.local
```

And with that, all that's left is to decrypt the vault file, which to do that the [ssh-vault program itself](https://github.com/ssh-vault/ssh-vault) will work.:

```
$ ./ssh-vault view crypto500/super_important_deathstar_file.txt
           .          .
 .          .                  .          .              .
       +.           _____  .        .        + .                    .
   .        .   ,-~"     "~-.                                +
              ,^ ___         ^. +                  .    .       .
             / .^   ^.         \         .      _ .
            Y  l  o  !          Y  .         __CL\H--.
    .       l_ `.___.'        _,[           L__/_\H' \\--_-          +
            |^~"-----------""~ ^|       +    __L_(=): ]-_ _-- -
  +       . !                   !     .     T__\ /H. //---- -       .
         .   \                 /               ~^-H--'
              ^.             .^            .      "       +.
                "-.._____.,-" .                    .
         +           .                .   +                       .
  +          .             +                                  .
         .             .      .       
                                                        .

silicon{EvenPartOfAPrivateKeyRevealsData}
```

Giving the flag:

```
silicon{EvenPartOfAPrivateKeyRevealsData}
```
