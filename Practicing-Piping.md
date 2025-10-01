# Redirecting Ouput

First, let's look at redirecting stdout to files. You can accomplish this with the `>` character, as so:

```sh
hacker@dojo:~$ echo hi > asdf
```
This will redirect the output of `echo hi` (which will be `hi`) to the file `asdf`. You can then use a program such as `cat` to output this file:

```sh
hacker@dojo:~$ cat asdf
hi
```
In this challenge, you must use this output redirection to write the word `PWN` (all uppercase) to the filename `COLLEGE` (all uppercase).

## Solution:

- `echo PWN > COLLEGE` redirects the output to the file "COLLEGE".
- This command also prints the flag.

## Flag: 

```
pwn.college{4pAl-cb0ynNbYbPM-R6PKVBvblF.QX0YTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `>` redirects stdout to a file, overwriting existing content.
- Filenames in Linux are case-sensitive.



# Redirecting More Output

Aside from redirecting the output of `echo`, you can, of course, redirect the output of any command. In this level, `/challenge/run` will once more give you a flag, but only if you redirect its output to the file `myflag`. Your flag will, of course, end up in the `myflag` file!

You'll notice that `/challenge/run` will still happily print to your terminal, despite you redirecting stdout. That's because it communicates its instructions and feedback over standard error, and only prints the flag over standard out!

## Solution:

- `/challenge/run > myflag` redirects the flag to the myflag file.
- `cat myflag` prints its contents which contains the flag.

## Flag: 

```
pwn.college{wBAwX2SbcAc46RS8-380beze2in.QX1YTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Aside from redirecting the output of echo, the output of any command can also be redirected.
- Programs can output to both stdout and stderr.
- Redirecting stdout can make a program appear to do nothing when it's actually working



# Appending Output

A common use-case of output redirection is to save off some command results for later analysis. Often times, you want to do this in aggregate: run a bunch of commands, save their output, and `grep` through it later. In this case, you might want all that output to keep appending to the same file, but `>` will create a new output file every time, deleting the old contents.

You can redirect input in append mode using `>>` instead of `>`, as so:

```sh
hacker@dojo:~$ echo pwn > outfile
hacker@dojo:~$ echo college >> outfile
hacker@dojo:~$ cat outfile
pwn
college
hacker@dojo:$
```
To practice, run `/challenge/run` with an append-mode redirect of the output to the file `/home/hacker/the-flag`. The practice will write the first half of the flag to the file, and the second half to `stdout` if `stdout` is redirected to the file. If you properly redirect in append-mode, the second half will be appended to the first, but if you redirect in truncation mode (`>`), the second half will overwrite the first and you won't get the flag!

Go for it now!

## Solution:

- `/challenge/run >> /home/hacker/the-flag` causes the first half of the flag to be written into the file.
- `cat /home/hacker/the-flag` gets the second half and hence prints the whole flag.

## Flag: 

```
pwn.college{AQDRQMy1nn_fd2Rs4mKbbP6Gh2X.QX3ATO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `>>` preserves existing content, `>` overwrites it.
- Programs can write to files directly and via stdout.
- Append mode is necessary when output over multiple runs is to be saved together.



# Redirecting Errors

Just like standard output, you can also redirect the error channel of commands. Here, we'll learn about File Descriptor numbers. A File Descriptor (FD) is a number that describes a communication channel in Linux. You've already been using them, even though you didn't realize it. We're already familiar with three:

FD 0: Standard Input
FD 1: Standard Output
FD 2: Standard Error
When you redirect process communication, you do it by FD number, though some FD numbers are implicit. For example, a `>` without a number implies `1>`, which redirects FD 1 (Standard Output). Thus, the following two commands are equivalent:

```sh
hacker@dojo:~$ echo hi > asdf
hacker@dojo:~$ echo hi 1> asdf
```
Redirecting errors is pretty easy from this point. If you have a command that might produce data via standard error (such as `/challenge/run`), you can do:

```sh
hacker@dojo:~$ /challenge/run 2> errors.log
```
That will redirect standard error (FD 2) to the `errors.log` file. Furthermore, you can redirect multiple file descriptors at the same time! For example:

```sh
hacker@dojo:~$ some_command > output.log 2> errors.log
```
That command will redirect output to `output.log` and errors to `errors.log`.

Let's put this into practice! In this challenge, you will need to redirect the output of `/challenge/run`, like before, to `myflag`, and the "errors" (in our case, the instructions) to `instructions`. You'll notice that nothing will be printed to the terminal, because you have redirected everything! You can find the instructions/feedback in `instructions` and the flag in `myflag` when you successfully pull this off!

## Solution:

- `/challenge/run 1> myflag 2> instructions` redirects FD 1 (stdout) to `myflag` and redirects FD 2 (stderr) to `instructions`.
- `cat instructions` prints the instructions/feedback.
- `cat myflag` prints the flag.

## Flag: 

```
pwn.college{EGTdpTJ0-Qa9ff0nUyMSs3aiYhn.QX3YTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Just like standard output, the error channel of commands can also be redirected.
- A File Descriptor (FD) is a number that describes a communication channel in Linux. 
- FD 0: Standard Input, FD 1: Standard Output, and FD 2: Standard Error
- When process communication is redirected, it is done through a FD number, though some FD numbers are implicit.
- For example, a `>` without a number implies `1>`, which redirects FD 1 (Standard Output).
- Using `1>` for stdout and `2>` for stderr.
- Multiple streams in one command can be redirected.
- When both stdout and stderr are redirected, nothing appears in terminal.



# Redirecting Input 

Just like you can redirect output from programs, you can redirect input to programs! This is done using `<`, as so:

```sh
hacker@dojo:~$ echo yo > message
hacker@dojo:~$ cat message
yo
hacker@dojo:~$ rev < message
oy
```
You can do interesting things with a lot of different programs using input redirection! In this level, we will practice using `/challenge/run`, which will require you to redirect the `PWN` file to it and have the `PWN` file contain the value `COLLEGE`! To write that value to the `PWN` file, recall the prior challenge on output redirection from `echo`!

## Solution:

- `echo COLLEGE > PWN` creates a file named "PWN" containing the text "COLLEGE".
- `/challenge/run < PWN` reads "COLLEGE" from the file and prints the flag.

## Flag: 

```
pwn.college{}
```

### References:

- none

### Notes:

- Just like output from programs can be redirected, input to programs can also be redirected.
- The < operator redirects standard input (stdin) to come from a file instead of the keyboard.



# Grepping Stored Results

You know how to run commands, how to redirect their output (e.g., `>`), and how to search through the resulting file (e.g., `grep`). Let's put this together!

In preparation for more complex levels, we want you to:

- Redirect the output of `/challenge/run` to `/tmp/data.txt`.
- This will result in a hundred thousand lines of text, with one of them being the flag, in `/tmp/data.txt`.
- `grep` that for the flag!

## Solution:

- `/challenge/run > /tmp/data.txt` to redirect the output to `/tmp/data.txt`.
- `grep "pwn.college{" /tmp/data.txt` searches for "pwn.college{" in the file and thus prints the flag.

## Flag: 

```
pwn.college{4zq6WJaU2d6h1WnaokjQTI8eI1Y.QX4EDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `>` command is used to redirect output to programs.



# Grepping Live Output 

It turns out that you can "cut out the middleman" and avoid the need to store results to a file, like you did in the last level. You can do this by using the | (pipe) operator. Standard output from the command to the left of the pipe will be connected to (piped into) the standard input of the command to the right of the pipe. For example:

```sh
hacker@dojo:~$ echo no-no | grep yes
hacker@dojo:~$ echo yes-yes | grep yes
yes-yes
hacker@dojo:~$ echo yes-yes | grep no
hacker@dojo:~$ echo no-no | grep no
no-no
```
Now try it for yourself! `/challenge/run` will output a hundred thousand lines of text, including the flag. `grep` for the flag!

## Solution:

- `/challenge/run | grep "pwn.college{"` prints the flag.
- `/challenge/run` executes and starts generating output line by line.
- Each line is immediately passed through the pipe to grep.
- `grep` checks each line against the pattern "pwn.college{".
- When a matching line is found, `grep` prints it.

## Flag: 

```
pwn.college{s179fBczTOO-F7lQXpyxFZqUy6V.QX5EDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `|` operator connects the stdout of the left command to the stdin of the right command, allowing direct data flow between programs without intermediate file.
- `grep` is used to search in files.



# Grepping Errors

You know how to redirect errors to a file, and you know how to pipe output to another program, such as `grep`. But what if you wanted to `grep` through errors directly?

The `>` operator redirects a given file descriptor to a file, and you've used `2>` to redirect fd 2, which is standard error. The `|` operator redirects only standard output to another program, and there is no `2|` form of the operator! It can only redirect standard output (file descriptor 1).

Luckily, where there's a shell, there's a way!

The shell has a `>&` operator, which redirects a file descriptor to another file descriptor. This means that we can have a two-step process to `grep` through errors: first, we redirect standard error to standard output (`2>& 1`) and then pipe the now-combined stderr and stdout as normal (`|`)!

Try it now! Like the last level, this level will overwhelm you with output, but this time on standard error. `grep` through it to find the flag!

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














