# DarkSide

> Our servers were hit with the DarkSide ransomware! (same ransomware that hit the colonial pipeline - that software really held up over time).
> 
> Our elite group of hackers were able to get a private key from the attackers. Can you help to figure out how to decrypt our files?
> 
> Link to File
> 
> Link to fixed Files
> 
> Link to fixed Files 2
> 
> The third link contains the fixed files for this challenge. The files in the first and second link can be used to solve this challenge. But the third link has updated files. Please note - the zip files contain files generated with different keys

What made this one difficult was that the input files provided were somewhat incorrect. However I ended up solving it by using the incorrect files and not the final fixed files, but it took a while.

The description is that it is encrypted with the DarkSide ransomware, and upon researching the DarkSide ransomware, it turns out that it uses a `Salsa20` encryption with a randomly generated key that is then encrypted using `RSA`. The first two sets of files they gave had a 32-byte key and I wasted a lot of time trying to figure out how to "decrypt" this data. It turns out that the key they gave is actually the `Salsa20` key.

The problem though is that `Salsa20` also calls for a nonce.

It turns out that this is a "mixture of WannaCry and DarkSide" and that the RSA private key "may not be needed". With that information I ended up trying to decrypt using a nonce of `\x00\x00\x00\x00` (since WannaCry uses a NULL IV), and that worked!

Here's the full code for how I decrypted the data

```python
from Crypto.Cipher import Salsa20

with open('encrypted_Darkside_flag.jpeg', 'rb') as f:
    data = f.read()

key = data[4:36]
msg = data[41:]
nonce = b'\x00'*8

cipher = Salsa20.new(key=key, nonce=nonce)

plaintext = cipher.decrypt(msg)

with open('salsa-out.bin', 'wb') as f:
	f.write(plaintext)
```

Which successfully decrypted the files!

```
silicon{the_Empire_Needs_Better_Key_Management}
```
