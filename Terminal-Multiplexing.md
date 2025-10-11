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

Time for some screen detective work!

If you become an avid screen user, you will inevitably end up with multiple sessions running. How do you find the right one to reattach to?

Well, we can list them:
```sh
hacker@dojo:~$ screen -ls
There are screens on:
        23847.mysession   (Detached)
        23851.goodwork    (Detached)
        23855.morework    (Detached)
3 Sockets in /run/screen/S-hacker.
```
The identifiers of the sessions are the PID of each respective screen process, a dot, and the name of the screen session. To attach to a specific one, you use its name or its PID by giving it as an argument to `screen -r`.
```sh
hacker@dojo:~$ screen -r goodwork
```
In this challenge, we've created three screen sessions for you. One of them contains the flag. The other two are decoys!

You'll need to check each one until you find it. Don't forget to detach (Ctrl-A d) before trying the next session!

## Solution:

- The chalenge requires to search three different screens for the flag
- Using `screen -ls`, all the sessions that are running are printed with their PIDs
- Using `screen -r 147`, the screen is opened and the flag can be retrieved 

## Flag:
```
pwn.college{EeNcCF3ekIhLDIoshvM9FqmQQWD.01N4IDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `screen -ls` can be used to see the multiple sessions that are running
- `screen -r` can be used to attach a specific screen by specifying its PID or name as an argument

# Switching Windows 

Okay, so far, `screen` is just a weird sort of terminal-with-a-terminal. But it can be much more!

Inside a single screen session, you can have multiple windows, like your browser has multiple tabs. This can be super handy for organizing different tasks!

These windows are handled with different keyboard shortcuts, all starting with `Ctrl-A`:

- `Ctrl-A c` - Create a new window
- `Ctrl-A n` - Next window
- `Ctrl-A p` - Previous window
- `Ctrl-A 0` through `Ctrl-A 9` - Jump directly to window 0-9
- `Ctrl-A "` - bring up a selection menu of all of the windows
For this challenge, we've set up a screen session with two windows:

Window 0 has... well, you'll have to switch there to find out!
Window 1 has a welcome message
Attach to the session with `screen -r`, then use one of the key combinations above to switch to Window 1. Go get that flag!

## Solution:

- The challenge requires to attach the session using `screen -r`
- Then use `Ctrl-A 0` to jump directly to the first window to get the flag 

## Flag:

```
pwn.college{8Zt4BBhD7vOlLEIejOMCVAvh71B.0FO4IDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

The following keyboard shortcuts can be used on the screen:
- `Ctrl-A c` - Create a new window
- `Ctrl-A n` - Next window
- `Ctrl-A p` - Previous window
- `Ctrl-A 0` through `Ctrl-A 9` - Jump directly to window 0-9
- `Ctrl-A "` - bring up a selection menu of all of the windows

# Detaching and Attaching [tmux]

Let's try the same thing with `tmux`!

`tmux` (terminal multiplexer) is screen's younger, more modern cousin. It does all the same things but with some different key bindings. The biggest difference? Instead of `Ctrl-A`, tmux uses `Ctrl-B` as its command prefix.

So to detach from tmux, you press `Ctrl-B` followed by `d`.
```sh
hacker@dojo:~$ tmux
[doing some work...]
[Press Ctrl-B, then d]
[detached (from session 0)]
hacker@dojo:~$
```
The commands also differ:

- `tmux ls` - List sessions
- `tmux attach` or `tmux a` - Reattach to session
For this challenge:

1. Launch tmux
2. Detach from it.
3. Run `/challenge/run` (this will send the flag to your detached session!)
4. Reattach to see your prize

## Solution:

- The challenge requires to launch `tmux`
- Detach using `Ctrl-B` followed by `d`
- Run `/challenge/run` to send the flag to the detached session
- Use `tmux a` to reattach it and get the flag 

## Flag:
```
pwn.college{kOl7z6JKNjpQuIMr9D6wnPnjei1.0VO4IDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Instead of `Ctrl-A` (for `screen`), `tmux` uses `Ctrl-B` as its command prefix.
-  `tmux ls` - List sessions
-  `tmux attach` or `tmux a` - Reattach to session
-  `tmux` command is used to launch it 


# Switching Windows [tmux] 

## Solution:

## Flag:

### References:

- none

### Notes:
