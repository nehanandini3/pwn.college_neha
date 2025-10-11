# The PATH Variable 

It turns out that the answer to "How does the shell find `ls`?" is fairly simple. There is a special shell variable, called `PATH`, that stores a bunch of directory paths in which the shell will search for programs corresponding to commands. If you blank out the variable, things go badly:
```sh
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ PATH=""
hacker@dojo:~$ ls
bash: ls: No such file or directory
hacker@dojo:~$
```
Without a PATH, bash cannot find the `ls` command.

In this level, you will disrupt the operation of the `/challenge/run` program. This program will DELETE the flag file using the `rm` command. However, if it can't find the `rm` command, the flag will not be deleted, and the challenge will give it to you! Thus, you must make it so that `/challenge/run` also can't find the `rm` command!

Keep in mind: `/challenge/run` will be a child process of your shell, so you must apply the concepts you learned in `Shell Variables` to mess with its `PATH` variable! If you don't succeed, and the flag gets deleted, you will need to restart the challenge to try again!

## Solution:

- This challenge requires to ensure the program can't find the `rm` command by using `PATH=""`.
- Run `/challenge/run` to get the flag.
  
## Flag:

```
pwn.college{II1sKZTMjX_b2GDMEWppB8vxbEl.QX2cDM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `PATH` is a shell variable that stores a bunch of directory paths in which the shell searches for programs.



# Setting PATH

Okay, so things break when you blank out `PATH`. But what about doing something useful with `PATH`?

Let's explore how we would, for example, add a new directory of programs to our command repertoire. Recall that `PATH` stores a list of directories to find commands in and, for commands in nonstandard places, we must typically execute them via their path:
```sh
hacker@dojo:~$ ls /home/hacker/scripts
goodscript	badscript	okayscript
hacker@dojo:~$ goodscript
bash: goodscript: command not found
hacker@dojo:~$ /home/hacker/scripts/goodscript
YEAH! This is the best script!
hacker@dojo:~$
```
If you maintain useful scripts that you want to be able to launch by bare name, this is annoying. However, by adding directories to or replacing directories in this list, you can expose these programs to be launched using their bare name! For example:
```sh
hacker@dojo:~$ PATH=/home/hacker/scripts
hacker@dojo:~$ goodscript
YEAH! This is the best script!
hacker@dojo:~$
```
Let's practice. This level's `/challenge/run` will run the `win` command via its bare name, but this command exists in the `/challenge/more_commands/` directory, which is not initially in the PATH. The win command is the only thing that `/challenge/run` needs, so you can just overwrite `PATH` with that one directory. Good luck!

## Solution:

- The challenge requires to overite `PATH` using `PATH=/challenge/more_commands`.
- Run `/challenge/run` to get the flag. 

## Flag:

```
pwn.college{Mr4weZUvl7waLftDdCarW1EkAiz.QX1cjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `PATH` can be used to add directories to the list so a program can be launched without having to mention its whole path every time.



# Finding Commands 

When you type the name of a command, something inside one of the many directories listed in your `$PATH` variable is what actually gets executed (of course, unless the command is a builtin!). But which file, precisely? You can find out with the aptly-named `which` command:
```sh
hacker@dojo:~$ which cat
/bin/cat
hacker@dojo:~$ /bin/cat /flag
YEAH
hacker@dojo:~$
```
Mirroring what the shell does when searching for commands, `which` looks at each directory in `$PATH` in order and prints the first file it finds whose name matches the argument you passed.

In this challenge, we added a `win` command somewhere in your `$PATH`, but it won't give you the flag. Instead, it's in the same directory as a `flag` file that we made readable by you! You must find `win` (with the which command), and `cat` the flag out of that directory!

## Solution:

- This challenge requires to find the directory in which the win command is situated by using ` which win`.
- The found directory will be the same directory as the flag.
- Use `cat /challenge/paths/159/flag` to get the flag, where `/challenge/paths/159/` is the directory found previously. 

## Flag:

```
pwn.college{cFEKtns09CIc2o6htEs-xCDtMjs.01NzEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `which` looks at each directory in $PATH in order and prints the first file it finds whose name matches the argument.



# Adding Commands 

Recall our example from the previous level:
```sh
hacker@dojo:~$ ls /home/hacker/scripts
goodscript	badscript	okayscript
hacker@dojo:~$ PATH=/home/hacker/scripts
hacker@dojo:~$ goodscript
YEAH! This is the best script!
hacker@dojo:~$
```
What we see here, of course, is the `hacker` making the shell more useful for themselves by bringing their own commands to the party. Over time, you might amass your own elegant tools. Let's start with `win`!

Previously, the `win` command that `/challenge/run` executed was stored in `/challenge/more_commands`. This time, `win` does not exist! Recall the final level of `Chaining Commands`, and make a shell script called `win`, add its location to the `PATH`, and enable `/challenge/run` to find it!

Hint: `/challenge/run` runs as `root` and will call `win`. Thus, `win` can simply cat the flag file. Again, the `win` command is the only thing that `/challenge/run` needs, so you can just overwrite `PATH` with that one directory. But remember, if you do that, your `win` command won't be able to find `cat`.

You have three options to avoid that:

1. Figure out where the `cat` program is on the filesystem. It must be in a directory that lives in the `PATH` variable, so you can print the variable out (refer to `Shell Variables` to remember how!), and go through the directories in it (recall that the different entries are separated by `:`), find which one has `cat` in it, and invoke `cat` by its absolute path.
2. Set a `PATH` that has the old directories plus a new entry for wherever you create `win`.
3. Use `read` (again, refer to `Shell Variables`) to read `/flag`. Since `read` is a builtin functionality of `bash`, it is unaffected by `PATH` shenanigans.
Now, go and `win`!

## Solution:

- `mkdir /tmp/pwn` to create a dictionary.
- `cd /tmp/pwn` navigates to the created dictionary.
- `echo "/bin/cat /flag" > win` creates a script that uses absolute path to `cat` to read the flag file.
- `chmod +x win` gives the script execute permissions so it can be run as a command.
- `PATH="/tmp/pwn:$PATH" /challenge/run` runs the program with modified changes.
- The flag is printed.

## Flag:

```sh
pwn.college{wn2LW-a64HK3rzDNxUfWYidbQg9.QX2cjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- A new directory can be added to a path rather than wiping and redefining it every time by using `PATH="/new/directory:$PATH"`.



# Hijacking Commands 

Armed with your knowledge, you can now carry out some shenanigans. This challenge is almost the same as the first challenge in this module. Again, this challenge will delete the flag using the `rm` command. But unlike before, it will not print anything out for you.

How can you solve this? You know that `rm` is searched for in the directories listed in the `PATH` variable. You have experience creating the `win` command when the previous challenge needed it. What else can you create?

## Solution:

- `mkdir /tmp/pwn` to create a dictionary.
- `echo "/bin/cat /flag" > /tmp/pwn/rm` creates a fake `rm` that actually reads the flag.
- `chmod +x /tmp/pwn/rm` to make it executable.
- `PATH="/tmp/pwn:$PATH" /challenge/run` is run with the fake `rm` in PATH.
- The flag is printed.

## Flag:

```sh
pwn.college{stQNWfSUYgNu8jM3jdISsCDpaT4.QX3cjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Decoy commands can be placed to trick the shell search into finding the wrong commands.
