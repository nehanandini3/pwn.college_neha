# Bashrc Backdoor

When your shell starts up, it looks for `.bashrc` file in your home directory and executes it as a startup script. You can customize your `/home/hacker/.bashrc` with useful things, such as setting environment variables, tweaking your shell configuration, and so on.

You can also use it for evil! An unwitting victim's `.bashrc` is a common target for shenanigans. Imagine sneaking onto your friend's computer and adding a `echo "Hackers were here!"` at the end of their `.bashrc`. That's funny, but the same capability can be used for much more nefarious purposes. Malicious software, for example, often targets startup scripts such as `.bashrc to maintain persistence into the future!

In this challenge, we'll pretend that you've broken into a victim user's machine! That user is named zardus, with a home directory of /home/zardus. You, as the hacker user, have write access to his .bashrc, and zardus has read-access to /flag. The victim is simulated by the script /challenge/victim, and you can launch this script at any time to observe the victim logging into the computer. Can you get the flag?

HINT: Like the scripts you explored in Chaining Commands, the .bashrc script is just a shell script. Adding a new line with a command on it (e.g., echo Hello Hackers) will get that command executed, so all you really need to think about is what command will get you the flag!

NOTE: The victim's /home/zardus/.bashrc will have a lot of stuff already in it: the shell's startup is a complex affair. Don't panic --- just add your payload to the end and hope for the best!

HINT: Need to poke around as zardus to debug your solution? In practice mode, you can use sudo su --login zardus to drop into a Zardus session!

## Solution:

Describe your thought process and solve, write as much as possible with steps:

## Flag: 

```
pwn.college{}
```

### References:

- none

### Notes:

Include things you learnt, alternate methods or mistakes you made while solving

