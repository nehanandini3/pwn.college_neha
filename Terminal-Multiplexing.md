# Launching Screen

Let's dive right in!

`screen` is a program that creates virtual terminals inside your terminal. It's somewhat like having multiple browser tabs, but for your command line!

Starting screen is super simple:
```sh
hacker@dojo:~$ screen
```
That's it! You're now inside a screen session. It looks exactly like a terminal, but there are new capabilities there, waiting to be discovered.

For this challenge, we've hooked things up so that just launching screen will get you the flag. Easy!

NOTE: When you're done with your command line, type `exit` or press `Ctrl-D` to leave the screen session. Then screen will terminate and return you to your original shell.

## Solution:

- The challenge requires to launch a screen in order to get the flag
- `screen` gives the flag 

## Flag:

```
pwn.college{c4v7NEH4W5iSfbKItdZZQi82HvJ.0VN4IDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `screen` is a program that creates virtual terminals inside the terminal
- type `exit` or press `Ctrl-D` to leave the screen session

# Detaching and Attaching 

Now we'll start digging in with the magic of detaching!

Imagine you're working on something important over a remote connection, and your connection drops. With a normal terminal (outside of this awesome dojo environment), everything's gone. With screen, your work keeps running, and you can reattach later!

You can also detach on purpose, which we'll do in this challenge. You detach by pressing `Ctrl-A`, followed by `d` (for detach). This leaves your session running in the background while you return to your normal terminal.
```sh
hacker@dojo:~$ screen
[doing some work...]
[Press Ctrl-A, then d]
[detached from 12345.pts-0.hostname]
hacker@dojo:~$
```
To reattach, you can use the `-r` argument to `screen`:
```sh
hacker@dojo:~$ screen -r
```
For this challenge, you'll need to:

1. Launch screen
2. Detach from it.
3. Run `/challenge/run` (this will secretly send the flag to your detached session!)
4. Reattach to see your prize
FUN FACT: `Ctrl-A` is `screen`'s activation key for all of its shortcuts in its default configuration. All `screen` functionality is activated by some command combination starting with `Ctrl-A`.

HINT: Remember: Hold Ctrl and press A, then release both and press d.

HINT: If you see `[detached from...]`, you did it right!

## Solution:

- This challenge requires to launch `screen`
- By pressing `Ctrl-A`, followed by `d` detach the screen
- Run `/challenge/run` to send the flag to the detached session
- Reattach the session using `screen -r` to get the flag 

## Flag:

```
 pwn.college{QW5m3_1gCYE3qJWk0upMZDuoM3i.0lN4IDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Press `Ctrl-A`, followed by `d` to detach the screen
- `screen -r` is used to reattach the session 

# Finding Sessions

## Solution:

## Flag:

### References:

- none

### Notes:

# Switching Windows 

## Solution:

## Flag:

### References:

- none

### Notes:

# Detaching and Attaching [tmux]

## Solution:

## Flag:

### References:

- none

### Notes:

# Switching Windows [tmux] 

## Solution:

## Flag:

### References:

- none

### Notes:
