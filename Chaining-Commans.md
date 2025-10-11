# Chaining with Semicolons

The easiest way to chain commands is `;`. In most contexts, `;` separates commands in a similar way to how Enter separates lines. So, this:

```sh
hacker@dojo:~$ echo COLLEGE > pwn
hacker@dojo:~$ cat pwn
COLLEGE
hacker@dojo:~$
```
Is roughly the same as this:

```sh
hacker@dojo:~$ echo COLLEGE > pwn; cat pwn
COLLEGE
hacker@dojo:~$
```
Basically, when you hit Enter, your shell executes your typed command and, after that command terminates, gives you the prompt to input another command. The semicolon is analogous, just without the prompt and with you entering both commands before anything is executed.

Give it a try now! In this level, you must run `/challenge/pwn` and then `/challenge/college`, chaining them with a semicolon.

## Solution:

- `/challenge/pwn ; /challenge/college` is run so that `/challenge/pwn` is executed first and then `/challenge/college`.
- The flag is printed.

## Flag: 

```
pwn.college{UqQ-K0r8ndHYBUb-eMmOV5oBb7M.QX1UDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Commands separated by `;` operator run one after another.
- First command completes fully before second command starts so the order of the commands matter.



# Building on Success

You learned about exit codes in the `Processes` module. Now let's use them to chain commands together!

The `&&` operator allows you to run a second command only if the first command succeeds (in Linux convention, this means it exited with code 0). This is called the "AND" operator because both conditions must be true: the first command must succeed AND then the second command will run. That's super useful for complex commandline workflows where certain actions depend on the success of other actions.

Here's the syntax:

```sh
hacker@dojo:~$ command1 && command2
```
This means: "Run command1, and IF it succeeds, then run command2."

Some examples:

```sh
hacker@dojo:~$ touch /home/hacker/file && echo "this will run"
success
this will run
hacker@dojo:~$ touch /file && echo "this will NOT run"
touch: cannot touch '/file': Permission denied
hacker@dojo:~$
```
That second invocation of `touch` failed because the hacker user does not have write access to `/file`, so the `echo` did not run.

In this challenge, you need to chain the programs `/challenge/first-success` and `/challenge/second` using the `&&` operator. Try running each command separately first to see what happens (which is that you will not get the flag). But if you chain them with `&&`, the flag will appear!

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



