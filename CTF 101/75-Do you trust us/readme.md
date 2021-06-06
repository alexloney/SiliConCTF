# Do you trust us?

> Do you trust things the Empire is hosting for download?
> No
> 
> But how do you get the flag? ¯\(ツ)/¯
> 
> Link to Files

This has a pcap file, opening the pcap file, I eventually found a request to download a .zip file which was stored in it. Downloading the "archive.zip" file from it (I used WireShark hex view, then copied/pasted the hex, but there's probably an easier way).

I may not have gotten the full archive, as when I attempted to extract it via `unzip`, it would fail. However, opening it with the graphical ZIP tool worked to extract the `Iamnotdangerous` file.

A quick check on the file:
```
$ strings Iamnotdangerous | grep -i 'sili'
```

Obtains the flag:

```
Silicon{ItsDangerousToRunM3!}
```
