# Secret Note

> One of our spies passed us this note about the Death Star. However, it seems they are using an older code... it still checks out.. but we are not able to decrypt it. Can you give it a try?
> 
> We know the first few words are: The Death Star has one critical flaw.


My main problem with this one was that the flag wasn't being correctly accepted when I submitted it. After asking a question, they resolved the issue.

We're given a note with the following:

```
frt wtyfr jfyg ryj dht egpfpeyk okyn. frtgt pj yh tcrymjf sdgf fryf pj hd kygutg fryh y ndls gyf. po nt eyh jrddf phfd frpj nrdkt, frt nrdkt wtyfr jfygf npkk plskdwt. jpkpedh{jmqjfpfmfpdh_epsrtgj_wdhf_kpat_ahdnh_skyphftcf}
```

And we know that the first few characters decode to

```
The Death Star has one critical flaw.
```

So all that had to be done was solve the substitution cipher with our known values

```
THE DEATH STAR HAS ONE CRITICAL FLAW. THERE IS AN EcHAmST sORT THAT IS NO LARuER THAN A WOls RAT. IF WE CAN SHOOT INTO THIS WHOLE, THE WHOLE DEATH START WILL IlsLODE. SILICON{SmqSTITmTION_CIsHERS_DONT_LIaE_aNOWN_sLAINTEcT}
```

Which hasn't completely solved it, but it gave us enough to work with, using the last few remaining characters we can get the final solution

```
THE DEATH STAR HAS ONE CRITICAL FLAW. THERE IS AN EXHAUST PORT THAT IS NO LARGER THAN A WOMP RAT. IF WE CAN SHOOT INTO THIS WHOLE, THE WHOLE DEATH START WILL IMPLODE. SILICON{SUBSTITUTION_CIPHERS_DONT_LIKE_KNOWN_PLAINTEXT}
```

With the flag

```
SILICON{SUBSTITUTION_CIPHERS_DONT_LIKE_KNOWN_PLAINTEXT}
```