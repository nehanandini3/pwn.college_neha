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

- `/challenge/run 2>&1 | grep "pwn.college{"` gives the flag.
- `2>&1` redirects stderr to stdout.
- Both outputs are combined into stdout.
- The combined output is piped to grep.

## Flag: 

```
pwn.college{sc0I-vTwF0Xtng10EKzVkI9XtHe.QX1ATO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `>` operator redirects a given file descriptor to a file, and `2>` redirects to fd 2, which is standard error.
- The `|` operator redirects only standard output to another program, and there is no `2|` form of the operator.
- It can only redirect standard output (file descriptor 1).
- The shell has a >& operator, which redirects a file descriptor to another file descriptor.
- This means that there is a two-step process to `grep` through errors.
- First, the standard error is redirected to standard output (`2>& 1`) and then pipe the now-combined stderr and stdout as normal (`|`).



# Filtering with grep -v

The `grep` command has a very useful option: `-v` (invert match). While normal `grep` shows lines that MATCH a pattern, `grep -v` shows lines that do NOT match a pattern:

```sh
hacker@dojo:~$ cat data.txt
hello hackers!
hello world!
hacker@dojo:~$ cat data.txt | grep -v world
hello hackers!
hacker@dojo:~$
```
Sometimes, the only way to filter to just the data you want is to filter out the data you don't want. In this challenge, `/challenge/run` will output the flag to stdout, but it will also output over 1000 decoy flags (containing the word `DECOY` somewhere in the flag) mixed in with the real flag. You'll need to filter out the decoys while keeping the real flag!

Use `grep -v` to filter out all the lines containing "DECOY" and reveal the real flag!

## Solution:

- `/challenge/run | grep -v "DECOY"` prints the flag.
- `/challenge/run` generates all flags both real and decoys.
- Each line is passed to `grep -v "DECOY"`.
- Lines containing "DECOY" are filtered out.
- Only the line without "DECOY" (the real flag) passes through.

## Flag: 

```
pwn.college{QciRz-F4x3uDgNMZDAtV-nMdBfF.0FOxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `grep -v` excludes lines matching a pattern instead of including them.
- Sometimes, the only way to filter to get just the data required is to filter out the data not required.



# Duplicating Piped Data with tee

When you pipe data from one command to another, you of course no longer see it on your screen. This is not always desired: for example, you might want to see the data as it flows through between your commands to debug unintended outcomes (e.g., "why did that second command not work???").

Luckily, there is a solution! The `tee` command, named after a "T-splitter" from plumbing pipes, duplicates data flowing through your pipes to any number of files provided on the command line. For example:

```sh
hacker@dojo:~$ echo hi | tee pwn college
hi
hacker@dojo:~$ cat pwn
hi
hacker@dojo:~$ cat college
hi
hacker@dojo:~$
```
As you can see, by providing two files to `tee`, we ended up with three copies of the piped-in data: one to stdout, one to the `pwn` file, and one to the `college` file. You can imagine how you might use this to debug things going haywire:

```sh
hacker@dojo:~$ command_1 | command_2
Command 2 failed!
hacker@dojo:~$ command_1 | tee cmd1_output | command_2
Command 2 failed!
hacker@dojo:~$ cat cmd1_output
Command 1 failed: must pass --succeed!
hacker@dojo:~$ command_1 --succeed | command_2
Commands succeeded!
```
Now, you try it! This process' `/challenge/pwn` must be piped into `/challenge/college`, but you'll need to intercept the data to see what `pwn` needs from you!

## Solution:

- `/challenge/pwn | tee intercepted_data.txt | /challenge/college` duplicates pipe data to both stdout and files.
- `/challenge/pwn` generates output.
- Then `tee` receives this output and displays it on stdout.
- It then gets savesd it to `intercepted_data.txt`.
- Then it gets passed along to `/challenge/college`.
- It shows that a secret code is needed.
- `cat intercepted_data.txt` shows the secret code which is `AtF6yf_U`.
- `/challenge/pwn --secret AtF6yf_U | /challenge/college` then prints the flag.

## Flag: 

```
pwn.college{AtF6yf_UtV299Vb0LjNi_iSfqTo.QXxITO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `tee` command, named after a "T-splitter" from plumbing pipes, duplicates data flowing through pipes to any number of files provided on the command line. 



# Process Substitution for Input

Sometimes you need to compare the output of two commands rather than two files. You might think to save each output to a file first:

```sh
hacker@dojo:~$ command1 > file1
hacker@dojo:~$ command2 > file2
hacker@dojo:~$ diff file1 file2
```
But there's a more elegant way! Linux follows the philosophy that `"everything is a file"`. That is, the system strives to provide file-like access to most resources, including the input and output of running programs! The shell follows this philosophy, allowing you to, for example, use any utility that takes file arguments on the command line and hook it up to the output of programs, as you learned in the previous few levels.

Interestingly, we can go further, and hook input and output of programs to arguments of commands. This is done using `Process Substitution`. For reading from a command (input process substitution), use `<(command)`. When you write `<(command)`, bash will run the command and hook up its output to a temporary file that it will create. This isn't a real file, of course, it's what's called a named pipe, in that it has a file name:

```sh
hacker@dojo:~$ echo <(echo hi)
/dev/fd/63
hacker@dojo:~$
```
Where did `/dev/fd/63` come from? `bash` replaced `<(echo hi)` with the path of the named pipe file that's hooked up to the command's output! While the command is running, reading from this file will read data from the standard output of the command. Typically, this is done using commands that take input files as arguments:

```sh
hacker@dojo:~$ cat <(echo hi)
hi
hacker@dojo:~$
```
Of course, you can specify this multiple times:

```sh
hacker@dojo:~$ echo <(echo pwn) <(echo college)
/dev/fd/63 /dev/fd/64
hacker@dojo:~$ cat <(echo pwn) <(echo college)
pwn
college
hacker@dojo:~$
```
Now for your challenge! Recall what you learned in the `diff` challenge from `Comprehending Commands`. In that challenge, you diffed two files. Now, you'll diff two sets of command outputs: `/challenge/print_decoys`, which will print a bunch of decoy flags, and `/challenge/print_decoys_and_flag` which will print those same decoys plus the real flag.

Use process substitution with `diff` to compare the outputs of these two programs and find your flag!

## Solution:

- `diff <(/challenge/print_decoys) <(/challenge/print_decoys_and_flag)` prints the flag.
- `<(command)` creates temporary file descriptors for each command's output.
- `diff` sees these as regular files and compares them line by line.
- Since `/challenge/print_decoys_and_flag` has one extra line (the real flag), `diff` will show this addition.

## Flag: 

```
pwn.college{QMC0dl4MhFogvFk-zycJx_Qg-HE.0lNwMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Linux follows the philosophy that "everything is a file".
- That is, the system strives to provide file-like access to most resources, including the input and output of running programs.
- The shell follows this philosophy, allowing to, for example, use any utility that takes file arguments on the command line and hook it up to the output of programs.
- Hooking input and output of programs to arguments of commands can also be done.
- This is done using Process Substitution.
- For reading from a command (input process substitution), `<(command)` is used.
- When `<(command)` is executed, `bash` will run the command and hook up its output to a temporary file that it will create.
- This isn't a real file, of course, it's what's called a named pipe, in that it has a file name.
- Process substitution `<(command)` creates a temporary named pipe that makes command output appear as a file.
- This allows the use of commands that expect files as arguments with the output of other commands.



# Writing to Multiple Programs

Now you've learned that process substitution can make command output appear as files for reading with `<(command)`. But you can also use process substitution for writing to commands!

You can duplicate data to two files with `tee`:

```sh
hacker@dojo:~$ echo HACK | tee THE > PLANET
hacker@dojo:~$ cat THE
HACK
hacker@dojo:~$ cat PLANET
HACK
hacker@dojo:~$
```
And you've used `tee` to duplicate data to a file and a command:

```sh
hacker@dojo:~$ echo HACK | tee THE | cat
HACK
hacker@dojo:~$ cat THE
HACK
hacker@dojo:~$
```
But what about duplicating to two commands? As `tee` says in its manpage, it's designed to write to files and to standard output:

```sh
TEE(1)                           User Commands                          TEE(1)

NAME
       tee - read from standard input and write to standard output and files
```
But wait! You just learned that bash can make commands look like files using process substitution! For writing to a command (output process substitution), use `>(command)`. If you write an argument of `>(rev)`, bash will run the `rev` command (this command reads data from standard input, reverses its order, and writes it to standard output!), but hook up its input to a temporary named pipe file. When commands write to this file, the data goes to the standard input of the command:

```sh
hacker@dojo:~$ echo HACK | rev
KCAH
hacker@dojo:~$ echo HACK | tee >(rev)
HACK
KCAH
```
Above, the following sequence of events took place:

- `bash` started up the `rev` command, hooking a named pipe (presumably `/dev/fd/63`) to `rev`'s standard input.
- `bash` started up the `tee` command, hooking a pipe to its standard input, and replacing the first argument to `tee` with `/dev/fd/63`. tee never even saw the argument `>(rev)`; the shell substituted it before launching `tee`.
- `bash` used the `echo` builtin to print `HACK` into `tee`'s standard input.
- `tee` read `HACK`, wrote it to standard output, and then wrote it to `/dev/fd/63` (which is connected to `rev`'s stdin).
- `rev` read `HACK` from its standard input, reversed it, and wrote `KCAH` to standard output.
Now it's your turn! In this challenge, we have `/challenge/hack`, `/challenge/the`, and `/challenge/planet`. Run the `/challenge/hack` command, and duplicate its output as input to both the `/challenge/the` and the `/challenge/planet` commands! Scroll back through the previous challenges "Duplicating piped data with tee" and "Process substitution for input" if you need a refresher on this method.

Trivia!

The observant learner will realize that the following are equivalent:

```sh
hacker@dojo:~$ echo hi | rev
ih
hacker@dojo:~$ echo hi > >(rev)
ih
hacker@dojo:~$
```
More than one way to pipe data! Of course, the second route is way harder to read and also harder to expand. For example:

```sh
hacker@dojo:~$ echo hi | rev | rev
hi
hacker@dojo:~$ echo hi > >(rev | rev)
hi
hacker@dojo:~$
```
That's just silly! The lesson here is that, while Process Substitution is a powerful tool in your toolbox, it's a very specialized tool; don't use it for everything!

## Solution:

- `/challenge/hack | tee >(/challenge/the) >(/challenge/planet)` prints the flag.
- `/challenge/hack` generates output.
- `tee` receives the output and duplicates it to stdout (which displays in terminal), `>(/challenge/the)` creates a named pipe connected to `/challenge/the`'s stdin and `>(/challenge/planet)` creates a named pipe connected to `/challenge/planet`'s stdin.
- 

## Flag: 

```
pwn.college{k67k0MBGd0U2-v-CGMKwDkPTG34.QXwgDN1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- As `tee` says in its manpage, it's designed to write to files and to standard output.
- For writing to a command (output process substitution), use `>(command)`.
- In `>(rev)` command, `bash` will run the `rev` command (this command reads data from standard input, reverses its order, and writes it to standard output!), but hook up its input to a temporary named pipe file.
- When commands write to this file, the data goes to the standard input of the command.
- `>(command)` writes to command like it’s a file.
- Combined with `tee`, a command’s output is duplicared to multiple commands or files at once without creating temporary files.



# Split-piping stderr and stdout

Now, let's put your knowledge together. You must master the ultimate piping task: redirect stdout to one program and stderr to another.

The challenge here, of course, is that the `|` operator links the stdout of the left command with the stdin of the right command. Of course, you've used `2>&1` to redirect stderr into stdout and, thus, pipe stderr over, but this then mixes stderr and stdout. How to keep it unmixed?

You will need to combine your knowledge of `>()`, `2>`, and `|`. How to do it is a task I'll leave to you.

In this challenge, you have:

- `/challenge/hack`: this produces data on stdout and stderr
- `/challenge/the`: you must redirect `hack`'s stderr to this program
- `/challenge/planet`: you must redirect `hack`'s stdout to this program
Go get the flag!

## Solution:

- `/challenge/hack 2> >(/challenge/the) | /challenge/planet` prints the flag.
- `2> >(/challenge/the)` redirects stderr (FD 2) to the input of `/challenge/the`.
- `| /challenge/planet` pipes stdout (FD 1) to `/challenge/planet`
- Both commands receive their respective contents simultaneously.

## Flag: 

```
pwn.college{UpW2DClEyAW1skZQSfq2T1stTS8.QXxQDM2wCO0AzNzEzW}
```

### References:

- none

### Notes:

- stdout and stderr can be piped to different commands simultaneously.
- `>` is used to send stdout to a file.
- `2>` is used to send stderr to a file.



# Named Pipes

You've learned about pipes using `|`, and you've seen that process substitution creates temporary named pipes (like `/dev/fd/63`). You can also create your own persistent named pipes that stick around on the filesystem! These are called FIFOs, which stands for First (byte) In, First (byte) Out.

You create a FIFO using the `mkfifo` command:

```sh
hacker@dojo:~$ mkfifo my_pipe
hacker@dojo:~$ ls -l my_pipe
prw-r--r-- 1 hacker hacker 0 Jan 1 12:00 my_pipe
-rw-r--r-- 1 hacker hacker 0 Jan 1 12:00 some_file
hacker@dojo:~$
```
Notice the p at the beginning of the permissions - that indicates it's a pipe! That's markedly different than the - that's at the beginning of normal files, such as `some_file` in the above example.

Unlike the automatic named pipes from process substitution:

- You control where FIFOs are created
- They persist until you delete them
- Any process can write to them by path (e.g., `echo hi > my_pipe`)
- You can see them with `ls` and examine them like files
One problem with FIFOs is that they'll "block" any operations on them until both the read side of the pipe and the write side of the pipe are ready. For example, consider this:

```sh
hacker@dojo:~$ mkfifo myfifo
hacker@dojo:~$ echo pwn > myfifo
```
To service `echo pwn > myfifo`, bash will open the `myfifo` file in write mode. However, this operation will hang until something also opens the file in read mode (thus completing the pipe). That can be in a different console:

```sh
hacker@dojo:~$ cat myfifo
pwn
hacker@dojo:~$
```
What happened here? When we ran `cat myfifo`, the pipe had both sides of the connection all set, and unblocked, allowing `echo pwn > myfifo` to run, which sent `pwn` into the pipe, where it was read by `cat`.

Of course, this can somewhat be done by normal files: you've learned how to `echo` stuff into them and `cat` them out. Why use a FIFO instead? Here are key differences:

- No disk storage: FIFOs pass data directly between processes in memory - nothing is saved to disk
- Ephemeral data: Once data is read from a FIFO, it's gone (unlike files where data persists)
- Automatic synchronization: Writers block until the readers are ready, and vice-versa. This is actually useful! It provides automatic synchronization. Consider the example above: with a FIFO, it doesn't matter if `cat myfifo` or `echo pwn > myfifo` is executed first; each would just wait for the other. With files, you need to make sure to execute the writer before the reader.
- Complex data flows: FIFOs are useful for facilitating complex data flows, merging and splitting data in flexible ways, and so on. For example, FIFOs support multiple readers and writers.
This challenge will be a simple introduction to FIFOs. You'll need to create a `/tmp/flag_fifo` file and redirect the stdout of `/challenge/run` to it. If you're successful, `/challenge/run` will write the flag into the FIFO! Go do it!

HINT: The blocking behavior of FIFOs makes it hard to solve this challenge in a single terminal. You may want to use the Desktop or VSCode mode for this challenge so that you can launch two terminals.

## Solution:

- `mkfifo /tmp/flag_fifo` in the first terminal to creates a named pipe.
- `cat /tmp/flag_fifo` in the second terminal to start reading from the FIFO.
- `/challenge/run > /tmp/flag_fifo` in the first terminal print the flag.

## Flag: 

```
pwn.college{AJYnonP_wuqp7DXM0UVlogc7ESw.01MzMDOxwCN3kjNzEzW}
```

### References:

- none

### Notes:

- `mkfifo` creates named pipes that exist until deleted.
-  FIFOs block until both reader and writer are connected.
-  FIFO stands for First (byte) In, First (byte) Out.
-  `p` at the beginning of the permissions indicates it's a pipe.
-  A FIFO can be executed like a normal file.
-  FIFOs pass data directly between processes in memory therefore, nothing is saved to disk.























