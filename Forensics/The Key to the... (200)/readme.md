# The Key to the...?

> Digging through a scrap pile on Tatooine, one of our scavengars found a discarded Imperial hard drive.
> 
> We were only able to recover part of the drive's contents. There appears to be a key in here but it's not clear how to make use of it.
> 
> Mind taking a look and seeing what you can find?
> 
> Decompress with tar xzf drive.tar.gz

For this one we were given the `drive.tar.gz` file which contained an SSH private key and a file pointing to github. After a bit of searching I discovered that there's a way to locate which repository uses a key on github: https://docs.github.com/en/github/authenticating-to-github/troubleshooting-ssh/error-key-already-in-use#finding-where-the-key-has-been-used

Which tells me that the key is in use here:
```bash
$ ssh -T -ai ./.ssh/id_rsa git@github.com
Warning: Permanently added the RSA host key for IP address '192.30.255.112' to the list of known hosts.
Hi tc-trooper17254/private_files! You've successfully authenticated, but GitHub does not provide shell access.
```

From there, it was just a matter of cloning the repository mentioned in the previous command

```bash
$ git clone git@github.com:tc-trooper17254/private_files
Cloning into 'private_files'...
remote: Enumerating objects: 26, done.
remote: Counting objects: 100% (26/26), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 26 (delta 2), reused 20 (delta 2), pack-reused 0
Receiving objects: 100% (26/26), 2.17 MiB | 1.58 MiB/s, done.
Resolving deltas: 100% (2/2), done.
```

And viewing the files to find the flag:

```
silicon{PR0t3ct_y0Ur_G1tHUb_k3y5}
```

