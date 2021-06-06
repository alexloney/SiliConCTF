# Stuffed

> This guy looks stuffed could there be more to this photo?

This was a segenography challenge. Using `stegosuite` and opening the `Stuffed.jpg` file and selecting "Extract" there is a "WhatsThis.xxd" file. The file is then a hex dump of a file and it has multiple compressions on it. It's just a matter of running `file` to determine the type, then unzipping with the appropriate program until you finally receive a base64 encoded text string containing the flag:

```
Silicon{FlagYouHaveFound}
```