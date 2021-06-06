# Deathstar Trench Run

> We've built a simulation of planned attack on the Empire's new battlestation. Use this to practice before the assault beings.
> 
> One thing: The developer forgot to tell us what the coordinates are to hit the vent, and now they're off planet on a mission.
> 
> I wonder what happens if the right input is given.
> 
> Enter the Simulation
> 
> Note: A local copy of the executable has been attached for testing purposes. Please note this executable will only run on Linux systems.

At first I thought this one was going to require using a debugger (and I did try `edb`), but it turns out there's a much easier way to do this using `ltrace`.

I started by running this in `ltrace`. When you do that, you will eventually locate the following inside the generated output file (I searched for a string I knew would be very close, "You missed!")

```
...
sprintf("42.79,66.98", "%s%c", "42.79,66.98", '8')
...
```

Which gave me the solution of

```
42.79,66.98
```

Then it was just a matter of playing the game through the link that was provided to obtain the flag

```
silicon{n1c3_j0b_u531ng_th3_f0rC3_c4d3t}
```

![[Pasted image 20210605183235.png]]