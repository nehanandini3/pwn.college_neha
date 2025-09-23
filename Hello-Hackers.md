# Intro to Commands

In this challenge, you will invoke your first command! When you type a command and hit enter, the command will be invoked, as so:

```sh
hacker@dojo:~$ whoami
hacker
hacker@dojo:~$
```
Here, the user executed the whoami command, which simply prints the username (hacker) to the terminal. When the command terminates, the shell once again displays the prompt, ready for the next command.

In this level, invoke the hello command to get the flag! Keep in mind: commands in Linux are case sensitive: hello is different from HELLO.

## Solution:

- Invoke the ```hello``` command to print the flag.
```sh
$ hello
```

## Flag: 

```
pwn.college{0paRzY0Bms8w38dz0-z7mx-iV6b.QX3YjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- commands in Linux are case-sensitive
- when a command is entered by the user, it gets invoked 
- once the command terminates, the prompt reappers in the shell, allowing for the next command to be entered



# Intro to Arguments

Let's try something more complicated: a command with arguments, which is what we call additional data passed to the command. When you type a line of text and hit enter, the shell actually parses your input into a command and its arguments. The first word is the command, and the subsequent words are arguments. Observe:

```sh
hacker@dojo:~$ echo Hello
Hello
hacker@dojo:~$
```
In this case, the command was echo, and the argument was Hello. echo is a simple command that "echoes" all of its arguments back out onto the terminal, like you see in the session above.

Let's look at echo with multiple arguments:

```sh
hacker@dojo:~$ echo Hello Hackers!
Hello Hackers!
hacker@dojo:~$
```
In this case, the command was echo, and Hello and Hackers! were the two arguments to echo. Simple!

In this challenge, to get the flag, you must run the hello command (NOT the echo command) with a single argument of hackers. Try it now!

## Solution:

- Invoke the ```hello``` command with ```hackers``` command to print the flag.
```sh
$ hello hackers
```

## Flag: 

```
pwn.college{Ec0HqCfPJTPKNB8bhYKCAvGxksO.QX4YjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- When you type a line of text and hit enter, the shell actually parses your input into a command and its arguments.
- The first word is the command, and the subsequent words are arguments.
- ```echo``` is a simple command that "echoes" all of its arguments back out onto the terminal



# Command History

You're going to type a lot of commands, and typing everything from scratch can be annoying. Luckily, the shell saves a history of every command you invoke.

You can scroll through those saved commands with the up/down arrow keys, and we'll practice that in this challenge. This challenge will inject the flag into your history. Bring up a terminal, hit the up arrow, and grab it! In other challenges, the history will contain the log of the commands you've run, so if you need to run a similar command again, you can use the arrow keys to scroll through and find it!

## Solution:

- Shell has history of every command so flag is already in it. 
- ```up``` arrow to get last command entered

## Flag: 

```
pwn.college{ER0rMWO3IlCwuRifpLiQdl1Enan.0lNzEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Every command invoked by the user is saved in the shell
- Scrolling through those saved commands with the up/down arrow keys can be done








