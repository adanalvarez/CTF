# The Vault
## Introduction
Has it been days? Weeks? You can't remember how long you've been standing at the door to the vault.
You can't remember the last time you slept or ate, or had a drop of water, for that matter.
But all of that is insignificant, in the presence of the untold fortunes that must lie just beyond the threshold.

But the door. It won't budge. It says it will answer only to the DUNGEON_MASTER.
Have you not shown your worth?
But more than that,
It demands to know your secrets.

Nothing you've tried has worked.
You've pled, begged, cursed, but the door holds steadfast, harshly judging your failed requests.

But with each failed attempt you start to notice more and morethat there's something peculiar about the way the door responds to you.

Maybe the door knows more than it's letting on.
...Or perhaps it's letting on more than it knows?

http://chal1.swampctf.com:2694

## Solution

When accessing the URL, the following screen appeared:

![N|Solid](https://donttouchmy.net/wp-content/uploads/2018/03/web-300x242.png)

We entered random data in the fields to test the form, we received the following message:

![N|Solid](https://donttouchmy.net/wp-content/uploads/2018/03/invalid.png)

We made the same request but using burp proxy to capture the response, the result was the following:

![N|Solid](https://donttouchmy.net/wp-content/uploads/2018/03/erroruser.png)

The error is clear, it says that the user does not exist. Therefore, the first thing we need is a valid user. We tried with the user DUNGEON_MASTER, since according to the web it is the only one with access: "only DUNGEON_MASTER may enter the vault". We tested this and effectively the answer was different:

![N|Solid](https://donttouchmy.net/wp-content/uploads/2018/03/hashsha256web-768x335.png)

The answer indicates that the test_hash XXX does not match real_hash XXX. The first hash is the SHA256 hash of our password and the second has to be the SHA256 hash of the password we need.

Knowing the SHA256 hash of the password we need, we tried to perform a brute force attack using hashcat.

We decided to use the rockyou dictionary:
```sh
$ hashcat64.exe -m 1400 -a 0 hash.txt rockyou.txt -r rules/best63.rules
```
We launched hashcat with the following options:

**-m 1400** indicate that it is a hash type SHA256, in the hashcat wiki you can see examples of each hash and its code in hashcat

**-a 0** Straight mode to use a dictionary and rules

**hash.txt** file where our hash will be stored

**rockyou.txt** dictionary to use

**rules\best63.rules** file with hashcat rules to create new passwords from the dictionary 

After launching the command, in a few seconds we obtained the password:

![N|Solid](https://donttouchmy.net/wp-content/uploads/2018/03/hash.png)

With these data we were able to field the form with user DUNGEON_MASTER and password smaug123 to get the flag:

![N|Solid](https://donttouchmy.net/wp-content/uploads/2018/03/fflag.png)
