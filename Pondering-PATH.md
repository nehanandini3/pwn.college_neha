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

- This challenge requires to ensure the program can't find the `rm` command by using `PATH=""`
- Run `/challenge/run` to get the flag
  
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

- The challenge requires to overite `PATH` using `PATH=/challenge/more_commands`
- Run `/challenge/run` to get the flag 

## Flag:

```
pwn.college{Mr4weZUvl7waLftDdCarW1EkAiz.QX1cjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `PATH` can be used to add directories to the list so a program can be launched without having to mention its whole path every time

# Finding Commands 

## Solution:

## Flag:

### References:

- none

### Notes:

# Adding Commands 

## Solution:

## Flag:

### References:

- none

### Notes:

# Hijacking Commands 

## Solution:

## Flag:

### References:

- none

### Notes:
