# Listing Processes

First, we will learn to list running processes using the `ps` command. Depending on whom you ask, `ps` either stands for "process snapshot" or "process status", and it lists processes. By default, `ps` just lists the processes running in your terminal, which honestly isn't very useful:

```sh
hacker@dojo:~$ ps
    PID TTY          TIME CMD
    329 pts/0    00:00:00 bash
    349 pts/0    00:00:00 ps
hacker@dojo:~$
```
In the above example, we have the shell (`bash`) and the `ps` process itself, and that's all that's running on that specific terminal. We also see that each process has a numerical identifier (the Process ID, or PID), which is a number that uniquely identifies every running process in a Linux environment. We also see the terminal on which the commands are running (in this case, the designation `pts/0`), and the total amount of cpu time that the process has eaten up so far (since these processes are very undemanding, they have yet to eat up even 1 second!).

In the majority of cases, this is all that you'll see with a default `ps`. To make it useful, we need to pass a few arguments.

As `ps` is a very old utility, its usage is a bit of a mess. There are two ways to specify arguments.

"Standard" Syntax: in this syntax, you can use `-e` to list "every" process and `-f` for a "full format" output, including arguments. These can be combined into a single argument `-ef`.

"BSD" Syntax: in this syntax, you can use `a` to list processes for all users, `x` to list processes that aren't running in a terminal, and `u` for a "user-readable" output. These can be combined into a single argument `aux`.

These two methods, `ps -ef` and `ps aux`, result in slightly different, but cross-recognizable output.

Let's try it in the dojo:

```sh
hacker@dojo:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
hacker         1       0  0 05:34 ?        00:00:00 /sbin/docker-init -- /bin/sleep 6h
hacker         7       1  0 05:34 ?        00:00:00 /bin/sleep 6h
hacker       102       1  1 05:34 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server --auth=none -
hacker       138     102 11 05:34 ?        00:00:07 /usr/lib/code-server/lib/node /usr/lib/code-server/out/node/entr
hacker       287     138  0 05:34 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server/lib/vscode/ou
hacker       318     138  6 05:34 ?        00:00:03 /usr/lib/code-server/lib/node --dns-result-order=ipv4first /usr/
hacker       554     138  3 05:35 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server/lib/vscode/ou
hacker       571     554  0 05:35 pts/0    00:00:00 /usr/bin/bash --init-file /usr/lib/code-server/lib/vscode/out/vs
hacker       695     571  0 05:35 pts/0    00:00:00 ps -ef
hacker@dojo:~$
```
You can see here that there are processes running for the initialization of the challenge environment (`docker-init`), a timeout before the challenge is automatically terminated to preserve computing resources (`sleep 6h` to timeout after 6 hours), the VSCode environment (several `code-server` helper processes), the shell (`bash`), and my `ps -ef` command. It's basically the same thing with `ps aux`:

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
There are many commonalities between `ps -ef` and `ps aux`: both display the user (`USER` column), the PID, the TTY, the start time of the process (`STIME/START`), the total utilized CPU time (`TIME`), and the command (`CMD/COMMAND`). `ps -ef` additionally outputs the Parent Process ID (`PPID`), which is the PID of the process that launched the one in question, while `ps aux` outputs the percentage of total system CPU and Memory that the process is utilizing. Plus, there's a bunch of other stuff we won't get into right now.

Anyways! Let's practice. In this level, I have once again renamed `/challenge/run` to a random filename, and this time made it so that you cannot `ls` the `/challenge` directory! But I also launched it, so you can find it in the running process list, figure out the filename, and relaunch it directly for the flag! Good luck!

NOTE: Both `ps -ef` and `ps aux` truncate the command listing to the width of your terminal (which is why the examples above line up so nicely on the right side of the screen. If you can't read the whole path to the process, you might need to enlarge your terminal (or redirect the output somewhere to avoid this truncating behavior)! Alternatively, you can pass the `w` option twice (e.g., `ps -efww` or `ps auxww`) to disable the truncation.

## Solution:

- `ps auxww` is run to list all processes with full command lines (without truncation).
- `/challenge/20949-run-2492` is run as its shown in the output given by the previous command. This gives the flag.

## Flag: 

```
pwn.college{cZhfdRcGX4RTftXbqw5lArtJIHG.QX4MDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `ps` either stands for "process snapshot" or "process status", and it lists processes.
- By default, `ps` just lists the processes running in the terminal.
- A process has a numerical identifier that uniquely identifies each process in Linux.
- Standard syntax: `-e` lists every process. `f` shows full format output (parent, user, command arguments). The two can be used together as `ef`.
- BSD syntax: `a` - lists processed for all users. `x` - lists processes that aren't running in the terminal. `u` - lists user-readable output. `aux` - the above three can be used together.
- There are many commonalities between `ps -ef` and `ps aux`.
- Both display the user (`USER` column), the PID, the TTY, the start time of the process (`STIME/START`), the total utilized CPU time (`TIME`), and the command (`CMD/COMMAND`). `ps -ef` additionally outputs the Parent Process ID (`PPID`), which is the PID of the process that launched the one in question, while `ps aux` outputs the percentage of total system CPU and Memory that the process is utilizing. 



# Killing Processes

You've launched processes, you've viewed processes, now you will learn to terminate processes! In Linux, this is done using the aggressively-named `kill` command. With default options (which is all we'll cover in this level), `kill` will terminate a process in a way that gives it a chance to get its affairs in order before ceasing to exist.

Let's say you had a pesky `sleep` process (`sleep` is a program that simply hangs out for the number of seconds specified on the commandline, in this case, 1337 seconds) that you launched in another terminal, like so:

hacker@dojo:~$ sleep 1337
How do we get rid of it? You use `kill` to terminate it by passing the process identifier (the `PID` from `ps`) as an argument, like so:

```sh
hacker@dojo:~$ ps -e | grep sleep
 342 pts/0    00:00:00 sleep
hacker@dojo:~$ kill 342
hacker@dojo:~$ ps -e | grep sleep
hacker@dojo:~$
```
Now, it's time to terminate your first process! In this challenge, `/challenge/run` will refuse to run while `/challenge/dont_run` is running! You must find the `dont_run` process and `kill` it. If you fail, `pwn.college` will disavow all knowledge of your mission. Good luck.

## Solution:

- `ps aux` first to list all processes and search for `dont_run`.
- Using the shown PID 136, run `kill 136` to terminate the process.
- `/challenge/run` gives the flag

## Flag: 

```
pwn.college{Euu-Ll3zlREzzZXR40rVoEzqXS9.QXyQDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `kill` command is used to terminate processes.
- It's syntax is `kill <PID>`.
- With default options, `kill` will terminate a process in a way that gives it a chance to get its affairs in order before ceasing to exist.
- `kill` can also be used to terminate a `sleep` process.
- `sleep` is a program that simply hangs out for the number of seconds specified on the commandline.
- Synatax for `sleep` is `sleep <no. of second>`.



# Interrupting Processes

You've learned how to kill other processes with the `kill` command, but sometimes you just want to get rid of the process that's clogging up your terminal! Luckily, terminals have a hotkey for this: `Ctrl-C` (e.g., holding down the `Ctrl` key and pressing `C`) sends an "interrupt" to whatever application is waiting on input from the terminal and, typically, this causes the application to cleanly exit.

Try it here! `/challenge/run` will refuse to give you the flag until you interrupt it. Good luck!

For the very interested, check out this `article about terminals and "control codes"` (such as `Ctrl-C`).

## Solution:

- `/challenge/run` to run the program.
- It returns the following:
  ```sh
  I could give you the flag... but I won't, until this process exits. Remember, 
  you can force me to exit with Ctrl-C. Try it now!
  ```
- Press `Ctrl-C` to send an interrupt signal and get the flag.  

## Flag: 

```
pwn.college{kOjDWWBYnidQ4IiH3cuLkUyeMVX.QXzQDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `Ctrl-C`sends an "interrupt" to whatever application is waiting on input from the terminal and, typically, this causes the application to cleanly exit.



# Killing Misbehaving Processes

Sometimes, misbehaving processes can interfere with your work. These processes might need to be killed...

In this challenge, there's a decoy process that's hogging a critical resource - a named pipe (FIFO) at `/tmp/flag_fifo` into which (like in the `Practicing Piping` FIFO challenge) `/challenge/run` wants to write your flag. You need to `kill` this process.

Your general workflow should be:

- Check what processes are running.
- Find `/challenge/decoy` in the list and figure out its process ID.
- `kill` it.
- Run `/challenge/run` to get the flag without being overwhelmed by decoys (you don't need to redirect its output; it'll write to the FIFO on its own).
Good luck!

NOTE: You might see a few decoy flags show up even after killing the decoy process. This happens because Linux pipes are buffered: conceptually, they have a sort of length through which data flows, and you might kill the decoy process while data is in the pipe. That data, having already entered the pipe, will proceed to the other end (your `cat`). If you wait a second, you'll see the decoys stop, and then you can `/challenge/run` and win!

## Solution:

- `ps aux | grep decoy` to list all the processes and find the deocy.
- `kill 143` as 143 is the PID given in the last command's output.
- Run `/challenge/run & cat /tmp/flag_fifo` to run the challenge, the flag is sent to `/tmp/flag_fifo` and then it is printed.

## Flag: 

```
pwn.college{gi4Zi5M_MdVXOEr8qLwQaaUW-R6.0FNzMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `kill` terminares processes.



# Suspending Processes 

You have learned to interrupt processes with `Ctrl-C`, but there are less drastic measures you can use to get your terminal back! You can suspend processes to the background with `Ctrl-Z`. In this level, we'll explore how this works and, in the next level, we'll figure out how to resume those suspended processes!

This level's `run` wants to see another copy of itself running and using the same terminal. How? Use the terminal to launch it, then suspend it, then launch another copy while the first is suspended!

## Solution:

- `/challenge/run` command is the first instance of the challenge which is run.
- While the first instance is running, press `Ctrl-Z` to suspend the process.
- Run the second instance `/challenge/run` to get the flag.

## Flag: 

```
pwn.college{AGDtjNPqW2whGNhxI7D99W4vZX6.QX1QDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `Ctrl-Z` suspends a process while `Ctrl-C` terminates it.



# Resuming Processes

Usually, when you suspend processes, you'll want to resume them at some point. Otherwise, why not just terminate them? To resume processes, your shell provides the `fg` command, a builtin that takes the suspended process, resumes it, and puts it back in the foreground of your terminal.

Go try it out! This challenge's `run` needs you to suspend it, then resume it. Good luck!

## Solution:

- `/challenge/run` is run first.
- Press `Ctrl-Z` to suspend it.
- `fg` command to resume the suspended process and thus the flag is printed.

## Flag: 

```
pwn.college{8fPMBnL6igyBixxar9572f4hVHt.QX2QDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `fg` command resumes the most recent suspended process and brings it to the foreground.



# Backgrounding Processes

You've resumed processes in the foreground with the `fg` command. You can also resume processes in the background with the `bg` command! This will allow the process to keep running, while giving you your shell back to invoke more commands in the meantime.

This level's `run` wants to see another copy of itself running, not suspended, and using the same terminal. How? Use the terminal to launch it, then suspend it, then background it with `bg` and launch another copy while the first is running in the background!

ARCANUM: If you're interested in some deeper details, check out how to view the differences between suspended and backgrounded properties! Allow me to demonstrate. First, let's suspend a `sleep`:

```sh
hacker@dojo:~$ sleep 1337
^Z
[1]+  Stopped                 sleep 1337
hacker@dojo:~$
```
The `sleep` process is now suspended in the background. We can see this with `ps` by enabling the `stat` column output with the `-o` option:

```sh
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 T    sleep 1337
hacker       782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```
See that `T`? That means that the process is suspended due to our `Ctrl-Z`. The `S` in `bash`'s `STAT` column means that `bash` is sleeping while waiting for input. The `R` in `ps`'s column means that it's actively running, and the `+` means that it's in the foreground!

Watch what happens when we resume `sleep` in the background:

```sh
hacker@dojo:~$ bg
[1]+ sleep 1337 &
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 S    sleep 1337
hacker      1224 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```
Boom! The `sleep` now has an `S`. It's sleeping while, well, sleeping, but it's not suspended! It's also in the background and thus doesn't have the `+`.

## Solution:

- `/challenge/run` to run the first instance of the challenge.
- `CTRL-Z` to suspend the process.
- `bg` to resume the process in the background.
- While the first process is running in the background, run the challenge again.
- `/challenge/run` is run as it is the second instance of the challenge and the flag is shown.

## Flag: 

```
pwn.college{cDnfJhssUcYU5_V6Gs5OaD20W21.QX3QDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `bg` command allows to resume processes in the background by giving the user their shell back to invoke more commands while allowing the process to keep running as well.
- T: process is suspended due to Ctrl-Z
- S: sleeping while waiting for input
- R: it's actively running
- +: it's in the foreground



# Foregrounding Processes

Imagine that you have a backgrounded process, and you want to mess with it some more. What do you do? Well, you can foreground a backgrounded process with `fg` just like you foreground a suspended process! This level will walk you through that!

## Solution:

- `/challenge/run` to run the challenge.
- `CTRL-Z` to suspend the process.
- `bg` to resume the process in the background.
- `fg` to bring the backgrounded process back to foreground.
- ```sh
  /challenge/run
  YES! Great job! I'm now running in the foreground. Hit Enter for your flag!
  ```
  The above is the output.
- Pressing `enter` gives the flag.

## Flag: 

```
pwn.college{kWgKx7K8LZHA6I2aeFIMnq0fQFa.QX4QDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `fg` can foreground a backgrounded process just like how it foreground a suspended process.



# Starting Backgrounded Processes

Of course, you don't have to suspend processes to background them: you can start them backgrounded right off the bat! It's easy; all you have to do is append a `&` to the command, like so:

```sh
hacker@dojo:~$ sleep 1337 &
[1] 1771
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker      1709 Ss   bash
hacker      1771 S    sleep 1337
hacker      1782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```
Here, `sleep` is actively running in the background, not suspended. Now it's your turn to practice! Launch `/challenge/run` backgrounded for the flag!

## Solution:

- `/challenge/run &` to start the challenge in the background.
- The flag is printed.

## Flag: 

```
pwn.college{QjIG1vsSLa6iNVLWXI2KHGXSTJU.QX5QDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `&` operator starts processes directly in the background without needing `Ctrl-Z` and `bg`.



# Process Exit Codes

Every shell command, including every program and every builtin, exits with an exit code when it finishes running and terminates. This can be used by the shell, or the user of the shell (that's you!) to check if the process succeeded in its functionality (this determination, of course, depends on what the process is supposed to do in the first place).

You can access the exit code of the most recently-terminated command using the special `?` variable (don't forget to prepend it with `$` to read its value!):

```sh
hacker@dojo:~$ touch test-file
hacker@dojo:~$ echo $?
0
hacker@dojo:~$ touch /test-file
touch: cannot touch '/test-file': Permission denied
hacker@dojo:~$ echo $?
1
hacker@dojo:~$
```
As you can see, commands that succeed typically return `0` and commands that fail typically return a non-zero value, most commonly `1` but sometimes an error code that identifies a specific failure mode.

In this challenge, you must retrieve the exit code returned by `/challenge/get-code` and then run `/challenge/submit-code` with that error code as an argument. Good luck!

## Solution:

- `/challenge/get-code` to run the challenge.
- `/challenge/submit-code $?` to immediately capture the exit code after the `get-code` finishes, before any other command runs.
- The flag is printed

## Flag: 

```
pwn.college{EMJRTkRsRuxWkkmAoNeBbcAkSOU.QX5YDO1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Every shell command, including every program and every builtin, exits with an exit code when it finishes running and terminates.
- This can be used by the shell, or the user of the shell to check if the process succeeded in its functionality.
- `$?` contains the exit status of the last executed foreground command.
- The exit code of the most recently-terminated command can be accessed using the special `?` variable prepended with `$` to read its value.
- Commands that succeed typically return `0`.
- commands that fail typically return a non-zero value, most commonly `1` but sometimes an error code that identifies a specific failure mode.














