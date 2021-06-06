# Sarlacc

> We recovered this file with one of our drive by spear-phishing attacks on the Emprie. We beleive it contains important information on the Sarlacc creature of Tatooine. Could this mean that Jabba and the Empire have struck some sort of deal?

This one had me stumped for a while, and I eventually used a hint on it. It turns out that an encrypted ZIP file with a known plaintext value inside of it can be cracked pretty easily. Since we have the picture file which is also encrypted into the ZIP file, we can use that to crack it.

To start with, we have to compress the `almighty_sarlacc.jpeg` file with the same compression algorithm used in the encrypted zip, then we can use `pkcrack` to crack it.

```
$ zip clear.zip almighty_sarlacc.jpeg
$ ./pkcrack/bin/pkcrack -C deal_details.zip -c 'deal_details/almighty_sarlacc.jpeg' -P clear.zip -p almighty_sarlacc.jpeg -d out -a
```

After not very long of a runtime (maybe a minute), it resulted in a successfully decrypted "out.zip" file, which contained the following flag:

```
silicon{JH_gave_DV_Boba_Fett}
```
