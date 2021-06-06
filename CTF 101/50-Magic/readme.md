# Magic

> We had a destructive attack agaisnt our machine! We have an important png file that must be recovered. Can you help us bring it back from the dead?

It turns out that the flag is a PNG file, but the magic number from the beginning of the file is missing. To fix this, I simply created another PNG file and copied the header over into the original PNG file to give it the correct magic number and allow it to be detected again.

```
silicon{7il3_$ign@tur3s_t3ll_th3_0s_h0w_t0_R3@d_Fil3s}
```


