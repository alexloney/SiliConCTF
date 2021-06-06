# Landing Dock Patrol

> To sneak Rebels on to the Death Star, we need to know when the dock is going to be unguarded. A successful phishing campagin got us some credentials to the Patrol Scheduling website. Unfortunately, these creds only get us one schedule. We need adminstrative access.
> 
> Your our only hope...
> 
> Username: stormtrooper2
> 
> Password: StormTrooperRulez
> 
> Link to Site

This one had a simple login page, and when logging in it specifies an `id=2` value. Changing that to `id=1` in burp gave the flag:

```
silicon{StromTroopersGottaEatToo}
```