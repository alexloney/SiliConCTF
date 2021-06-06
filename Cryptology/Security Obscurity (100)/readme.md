# Security Obscurity


> We received 85 messages from an operative we have in the field. We don't think it's encrypted, but we can't figure out the encoding... can you help us?
> 
> Here is one message:
> 
> F(oH)@rH73<+n(.1K#oKBJX[NH#PQ\DalO#@rGmlDJ+@


It turns out that this one is not encrypted, but instead it is just encoded. So it's just a matter of figuring out what the encoding it. It contains more than base64, so maybe something wih a larger space?

Using CyberChef, it turns out that it's encoded with Base85

```
silicon{Th3r3R0th3rtyp3soF3ncoding}> 
```
