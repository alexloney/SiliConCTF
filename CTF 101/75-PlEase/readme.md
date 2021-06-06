# PlEase

> We found this dropper malware in our enviroment. We are trying to determine if this is a new attack or an older one. Can you help us determine when this malware was compiled? PlEase help us!
> 
> Submit the flag like: silicon{epoch_time}

This one had me stumped for a while! I obtained the time pretty quickly/easily using `objdump`, however what caught me was that the time was actually displayed in my local time instead of UTC and the flag should be submitted in UTC time (NOTE: The problem didn't state it needed to be in UTC or what zone, so that was annoying).

The way I solved this one was to actually view the binary in a hex editor then view the PE header definition, locate the date, and copy that value. However, I believe that this would be solvable by using epochconverter.com and selecting "Local time".

```
silicon{233456412}
```
