# The Droid

> We've heard the empire is working on a new weapon and intelligence suggests the location is being held on a convoy that we're approaching.
> 
> This is a terminal challenge. Use the link below and open it in a new tab. Give the droid some time to start up. It's pretty old.
> 
> Misc100

After a bit of googling on what kafkacat is, I found that I can list values with the following

```
$ kafkacat -b localhost:9092 -L
Metadata for all topics (from broker 0: localhost:9092/0):
 1 brokers:
  broker 0 at localhost:9092 (controller)
 1 topics:
  topic "empire_intelligence" with 1 partitions:
    partition 0, leader 0, replicas: 0, isrs: 0
```

Now that I can list values, I can then save the values to a file from the listed values as such. The resulting values were a bunch of base64 strings, so a little bash manipulate can allow us to base64 decode each of them. Then a grep to see if any contained a flag.

```
$ kafkacat -b localhost:9092 -t empire_intelligence > intel.txt
$ cat intel.txt | sed 's/\(.*\)/echo "$(echo \1 | base64 -d)" >> out.txt/g' > intel.sh
$ chmod +x intel.sh
$ ./intel.sh
$ grep -i 'silicon' out.txt
```

And we get the flag:

```
Silicon{WEaPoN_i$_0n_tA7oOiN3}
```