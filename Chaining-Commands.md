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

- `/challenge/first-success && /challenge/second` is run so that `/challenge/second` is run if `/challenge/first-success` succeeds in running.
- The flag is printed. 

## Flag: 

```
pwn.college{YNn2LjJOh0M7Ae2UFEJ7N0KZvWI.0lM0MDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `&&` operator runs the second command (only if the first command succeeds i.e. exited with code 0).
- This is called the "AND" operator because both conditions must be true: the first command must succeed AND then the second command will run.
- Syntax: `command1 && command2`
- This means "Run command1, and IF it succeeds, then run command2".



# Handling Failure 

You just learned about the `&&` operator, which runs the second command only if the first succeeds. Now let's learn about its opposite: the `||` operator allows you to run a second command only if the first command fails (exits with a non-zero code). This is called the "OR" operator because either the first command succeeds OR the second command will run.

Here's the syntax:
```sh
hacker@dojo:~$ command1 || command2
```
This means: "Run command1, and IF it fails, then run command2."

Some examples:
```sh
hacker@dojo:~$ touch /file || echo "touch failed, so this runs"
touch: cannot touch '/file': Permission denied
touch failed, so this runs
hacker@dojo:~$ touch /home/hacker/file || echo "this will NOT run"
hacker@dojo:~$
```
The `||` operator is super useful for providing fallback commands or error handling!

In this challenge, you need to chain `/challenge/first-failure` and `/challenge/second` using the `||` operator. Go for it!

## Solution:

- `/challenge/first-failure || /challenge/second` is run so that `/challenge/second` is executed if and `/challenge/first-failure` cannot be executed.
- The flag is printed.

## Flag: 

```
pwn.college{0vu-N-2wlfPH4GrExprY-V7S6wY.01M0MDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `||` operator is used to run a second command (only if the first command fails i.e. exits with a non-zero code).
- This is called the "OR" operator because either the first command succeeds OR the second command will run.
- Syntax: `command1 || command2`
- This means "Run command1, and IF it fails, then run command2".


  
# Your First Shell Script 

As you combine more and more commands to achieve complex effects, the length of the combined prompt quickly gets really annoying to deal with. When this happens, you can put these commands in a file, called a shell script, and run them by executing the file! For example, consider our semicolon technique:
```sh
hacker@dojo:~$ echo COLLEGE > pwn; cat pwn
COLLEGE
hacker@dojo:~$
```
We can create a shell script called `pwn.sh` (by convention, shell scripts are frequently named with a `sh` suffix):
```sh
echo COLLEGE > pwn
cat pwn
```
And then we can execute by passing it as an argument to a new instance of our shell (`bash`)! When a shell is invoked like this, rather than taking commands from the user, it reads commands from the file.
```sh
hacker@dojo:~$ ls
hacker@dojo:~$ bash pwn.sh
COLLEGE
hacker@dojo:~$ ls
pwn
hacker@dojo:~$
```
You can see that the shell script executed both commands, creating and printing the `pwn` file.

Now, it's your turn! Same as last level, run `/challenge/pwn` and then `/challenge/college`, but this time in a shell script called `x.sh`, then run it with `bash`!

NOTE: We haven't yet talked about Linux's amazing array of competent command line file editors. For now, feel free to use the `Text Editor` application in Desktop mode (`Applications->Accessories->Text Editor`) or the default editor in the VSCode Workspace!

## Solution:

- `echo -e "/challenge/pwn\n/challenge/college" > x.sh && bash x.sh` creates the shell script and runs the script with `bash`.
- The flag is printed.

## Flag: 

```
pwn.college{w5Auh2yWpoCZ7OmOlxT4s1eqcjL.QXxcDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `Shell scripts` turn complex command chains into reusable one-liners.
- Entire workflows can be packaged into single files.
- `shell scripts` are frequently named with a `sh` suffix.
- `bash` is used to run a script.



# Redirecting Script Output 

Let's try something a bit trickier! You've piped output between programs with `|`, but so far, this has just been between one command's output and a different command's input. But what if you wanted to send the output of several programs to one command? There are a few ways to do this, and we'll explore a simple one here: redirecting output from your script!

As far as the shell is concerned, your script is just another command. That means you can redirect its input and output just like you did for commands in the `Piping` module! For example, you can write it to a file:
```sh
hacker@dojo:~$ cat script.sh
echo PWN
echo COLLEGE
hacker@dojo:~$ bash script.sh > output
hacker@dojo:~$ cat output
PWN
COLLEGE
hacker@dojo:~$
```
All of the various redirection methods work: `>` for stdout, `2>` for stderr, `<` for stdin, `>>` and `2>>` for append-mode redirection, `>&` for redirecting to other file descriptors, and `|` for piping to another command.

In this level, we will practice piping (`|`) from your script to another program. Like before, you need to create a script that calls the `/challenge/pwn` command followed by the `/challenge/college` command, and pipe the output of the script into a single invocation of the `/challenge/solve` command!

## Solution:

- `echo -e "/challenge/pwn\n/challenge/college" > x.sh && bash x.sh | /challenge/solve` creates the script and pipes the script output to solve the command.
- The flag is printed.

## Flag: 

```
pwn.college{sRTsKa5wF5E1qBmDtTPRkVaQjvs.QX4ETO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- As far as the shell is concerned, a script is just another command.
- All of the various redirection methods work: `>` for stdout, `2>` for stderr, `<` for stdin, `>>` and `2>>` for append-mode redirection, `>&` for redirecting to other file descriptors, and `|` for piping to another command.
- Scripts can be used with pipes just like regular commands.
- Piping from scripts combines multiple command outputs into one stream



# Executable Shell Scripts

You have written your first shell script, but calling it via `bash script.sh` is a pain. Why do you need that `bash`?

When you invoke `bash script.sh`, you are, of course launching the `bash` command with the `script.sh` argument. This tells bash to read its commands from `script.sh` instead of standard input, and thus your shell script is executed.

It turns out that you can avoid the need to manually invoke `bash`. If your shell script file is executable (recall `File Permissions`), you can simply invoke it via its relative or absolute path! For example, if you create `script.sh` in your home directory and make it executable, you can invoke it via `/home/hacker/script.sh` or `~/script.sh` or (if your working directory is `/home/hacker`) `./script.sh`.

Try that here! Make a shellscript that will invoke `/challenge/solve`, make it executable, and run it without explicitly invoking `bash`!

## Solution:

- `echo "/challenge/solve" > x.sh && chmod +x x.sh && ./x.sh` creates the script, makes it executable and runs it directly.

## Flag: 

```
pwn.college{gDsM4NMAmvKRj9WoV-vmPlT4U5M.QX0cjM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- A shell file can be invoked by using its relative or absolute path.



# Understanding Shebangs

You're well on your way to your new life as a shell scripter! However, so far, your shellscripts can only be launched from the shell. Things worked great in the previous level (because you were invoking your script from the `bash` shell), but they won't work if your script was being invoked by, say, a program written in Python (or any other language).

When a program is invoked in Linux, the Linux kernel first inspects the file to determine how it should be run. This does NOT use the extension (which is why you don't have to name your shell scripts with a `.sh` extension, or your Python scripts with a `.py` extension, or so on). Rather, Linux looks at the first few bytes of the file for this information.

There are a bunch of different types of programs, but if the program file starts with the characters `#!` (often termed a "`shebang`"), Linux treats the file as an interpreted program, and the contents of the rest of the line as the path to the interpreter. It then invokes the interpreter with the path to the program file as its only argument.

Consider this shell script:

```sh
#!/bin/bash

echo "Hello Hackers!"
```
This can be executed as:

```sh
hacker@dojo:~$ chmod a+x script.sh
hacker@dojo:~$ ./script.sh
Hello Hackers!
hacker@dojo:~$
```
When `./script.sh` was executed, Linux opened the file, read the first line, extracted `/bin/bash` as the interpreter, and executed `/bin/bash ./script.sh` to launch the script!

Note, the shebang line must be the VERY FIRST line of the file - no blank lines or spaces before it!

For this challenge, create a script at `/home/hacker/solve.sh` that has a proper shebang and outputs "hack the planet". Remember to make it executable, then run `/challenge/run` to verify your script works correctly!

FUN FACT: Common shebangs you might see:

- `#!/bin/bash` for bash scripts
- `#!/usr/bin/python3` for Python scripts
- `#!/bin/sh` for POSIX shell scripts --- this is a more primitive predecessor to `bash` with fewer features, but more compatibility to non-Linux systems!

## Solution:

- `echo -e '#!/bin/bash\necho "hack the planet"' > /home/hacker/solve.sh` to create a script.
- `chmod +x /home/hacker/solve.sh` makes it executable.
- `/challenge/run` prints the flag.

## Flag: 

```
pwn.college{QeyTp2_H6MGqQVBX9jlp1cEqZCv.0VOzMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Shebang (`#!`) tells Linux which interpreter to use.
- If the program file starts with the characters `#!` (often termed a "shebang"), Linux treats the file as an interpreted program.



# Scripting with Arguments

You've learned how to make shell scripts, but so far they've just been lists of commands. Scripts become much more powerful when they can accept arguments! This might look like:

```sh
hacker@dojo:~$ bash myscript.sh hello world
```
The script can access these arguments using special variables:

- `$1` contains the first argument ("hello")
- `$2` contains the second argument ("world")
- `$3` would contain the third argument (if there had been one)
...and so on
Here's a simple example:

```sh
hacker@dojo:~$ cat myscript.sh
#!/bin/bash
echo "First argument: $1"
echo "Second argument: $2"
hacker@dojo:~$ bash myscript.sh hello world
First argument: hello
Second argument: world
hacker@dojo:~$
```
For this challenge, you need to write a script at `/home/hacker/solve.sh` that:

- Takes two arguments
- Outputs them in REVERSE order (second argument first, then the first argument)
For example:

```sh
hacker@dojo:~$ bash /home/hacker/solve.sh pwn college
college pwn
hacker@dojo:~$
```
Once your script works correctly, run `/challenge/run` to get your flag!

## Solution:

- `echo -e '#!/bin/bash\necho "$2 $1"' > /home/hacker/solve.sh` creates the script with argument handling.
- `chmod +x /home/hacker/solve.sh` makes it executable.
- `bash /home/hacker/solve.sh pwn college` tests the script.
- `/challenge/run` prints the flag.

## Flag: 

```
pwn.college{4a6L_kic3LV2983LJvzyaoQEmGU.0VNzMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Scripts can accept command-line arguments.
- `$1` contains the first argument.
- `$2` contains the second argument.
- `$3`, `$4`, etc. contain subsequent arguments.
- Arguments are separated by spaces on command line.



# Scripting with Conditionals

Now that you can use arguments in scripts, let's make them smarter with conditional logic!

In bash, you can use `if` statements to make decisions:

```sh
if [ "$1" == "ping" ]
then
    echo "pong"
fi
```
The above, in English, is `if the first argument is "ping", print out "pong"`. The syntax is somewhat unforgiving for a few reasons. First, you must have spaces after `if` (if you're used to a language like C, this is different), after `[`, and before `]`. Second, `if`, `then`, and `fi` must all be on different lines (or separated by semicolons); you can't lump them into the same statement. It's also a bit weird: instead of `endif` or `end` or something like that, the terminator of the `if` statement is `fi` (`if` backwards). Just something you have to remember.

For this challenge, write a script at `/home/hacker/solve.sh` that:

- Takes one argument
- If the argument is "pwn", output "college"
- For any other input, output nothing
Example:

```sh
hacker@dojo:~$ bash /home/hacker/solve.sh pwn
college
hacker@dojo:~$ bash /home/hacker/solve.sh foo
hacker@dojo:~$
```
Once your script works correctly, run `/challenge/run` to get your flag!

NOTE: Interested in what else you can check in a condition, other than string equality? Read all about it with `help test`!

## Solution:

- `echo -e '#!/bin/bash\nif [ "$1" == "pwn" ]\nthen\n    echo "college"\nfi' > /home/hacker/solve.sh` create the script with conditional logic.
- `chmod +x /home/hacker/solve.sh` makes it executable.
- `/challenge/run` prints the flag.

## Flag: 

```
pwn.college{QxlxaURpVMg7s4sUINFFfWKLX_t.0lNzMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `if` statements add decision-making to scripts.
- Syntax: `if [ condition ]`
- `==` for equality checking.
- `if`, `then`, `fi` must be on separate lines.



# Scripting with Default Cases

Your `if` statements so far have handled specific cases, but what about everything else? That's where `else` comes in!

The `else` clause executes when the `if` condition is false:

```sh
if [ "$1" == "hello" ]
then
    echo "Hi there!"
else
    echo "I don't understand"
fi
```
Note that the `else` doesn't have a condition --- it catches everything that didn't match previously. It also doesn't have a `then` statement. Finally, the `fi` goes after the `else` block to denote the end of the whole complex statement! It is also optional: you didn't have it in the previous level, and you only need it if the logic you're trying to achieve demands it.

Here's a practical example:

```sh
if [ "$1" == "start" ]
then
    echo "Starting the service..."
else
    echo "Unknown command. Use 'start' to begin."
fi
```
For this challenge, write a script at `/home/hacker/solve.sh` that:

- Takes one argument
- If the argument is "pwn", output "college"
- For any other input, output "nope"
Example:

```sh
hacker@dojo:~$ bash /home/hacker/solve.sh pwn
college
hacker@dojo:~$ bash /home/hacker/solve.sh hack
nope
hacker@dojo:~$ bash /home/hacker/solve.sh anything
nope
hacker@dojo:~$
```
Once your script works correctly, run `/challenge/run` to get your flag!

## Solution:

- `echo -e '#!/bin/bash\nif [ "$1" == "pwn" ]\nthen\n    echo "college"\nelse\n    echo "nope"\nfi' > /home/hacker/solve.sh` creates script with if-else logic.
- `chmod +x /home/hacker/solve.sh` makes it executable.
- `/challenge/run` prints the flag.

## Flag: 

```
pwn.college{cZ63sxarLSluV26Rgl1FAdYeR1h.01NzMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `else` clause handles all cases where `if` condition fails.
- No condition needed for `else` as it's the default case.
- It also doesn't have a `then` statement.



# Scripting with Multiple Conditions

You've learned how to use a single `if` statement to check a condition. But what if you need to check multiple conditions? You can use `elif` (short for `else if`):

```sh
if [ "$1" == "one" ]
then
    echo "1"
elif [ "$1" == "two" ]
then
    echo "2"
elif [ "$1" == "three" ]
then
    echo "3"
else
    echo "unknown"
fi
```
Note that you do need a `then` after the elif, just like the `if`. As before the `else` at the end catches everything that didn't match.

For this challenge, write a script at `/home/hacker/solve.sh` that:

- Takes one argument
- If the argument is "hack", output "the planet"
- If the argument is "pwn", output "college"
- If the argument is "learn", output "linux"
- For any other input, output "unknown"
Example:

```sh
hacker@dojo:~$ bash /home/hacker/solve.sh hack
the planet
hacker@dojo:~$ bash /home/hacker/solve.sh pwn
college
hacker@dojo:~$ bash /home/hacker/solve.sh learn
linux
hacker@dojo:~$ bash /home/hacker/solve.sh foo
unknown
hacker@dojo:~$
```
Once your script works correctly, run `/challenge/run` to get your flag!

NOTE: As you're creating your script, make sure to follow the spacing closely in the examples. Unlike many other languages, bash requires the `[` and the `]` to be separated from other characters by spaces, otherwise it cannot parse the condition.

## Solution:

- `echo -e '#!/bin/bash\nif [ "$1" == "hack" ]\nthen\n    echo "the planet"\nelif [ "$1" == "pwn" ]\nthen\n    echo "college"\nelif [ "$1" == "learn" ]\nthen\n    echo "linux"\nelse\n    echo "unknown"\nfi' > /home/hacker/solve.sh` creates script with multiple conditions using `elif`.
- `chmod +x /home/hacker/solve.sh` makes it executable.
- `/challenge/run` prints the flag.

## Flag: 

```
pwn.college{seTUXVPKdKkX-K86Qh9OrfHn-yL.0FOzMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `elif` is basically "else if" for multiple conditions.
- Conditions are checked top to bottom.
- Each `elif` needs its own `then`, unlike `else`.



# Reading Shell Scripts

You're not the only one who writes shell scripts! They are very handy for doing simple "system-level" tasks, and are a common tool that developers and sysadmins reach for. In fact, a surprising fraction of the programs on a typical Linux machine are shell scripts.

In this level, we will learn to read shell scripts. `/challenge/run` is a shell script that requires you to put in a secret password, but that password is hardcoded into the script iself! Read the script (using, say, `cat`), figure out the password, and get the flag!

NOTE: Feel free to try to read the code of other challenges as well! Reading code is a critical strategy in learning new skills, because you can see how certain functionality was implemented and reuse those strategies in your own scripts. But watch out: some program files are machine code, and will not be readable to humans. You can use the `file` command to differentiate, but almost all the challenges in the Linux Luminarium are implemented as shell scripts and are safe to `cat` out.

## Solution:

- `cat /challenge/run` to read the challenge script.
- `/challenge/run` runs the challenge.
- Enter the password which is `hack the PLANET` as shown from the first command.
- The flag is printed.

## Flag: 

```
pwn.college{cwhLcHqHA6sYt13JHuEmAEXh411.0lMwgDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `cat` can be used to read the script of a shell.





