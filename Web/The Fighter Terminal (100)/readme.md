# The Fighter Terminal

> We have gained access to a Tie Fighter terminal. We are having trouble derving any information from it. Check it out to see what you can find:
> 
> Link to Terminal

When first logging into the terminal you're greeted with a message, but a quick look at the source reveals that it's not really a real terminal. The source references a query_flag.php, and just visiting that URL

```
https://challenges.silicon-ctf.party/web100//query_flag.php?auth=1
```

Gives the flag

```
silicon{TheTieFighterMayBeFasterButXWingHasBetterFirePower}
```
