# Printing Variables 

Let's start with printing variables out. The ```/challenge/run``` program will not, and cannot, give you the flag, but that's okay, because the flag has been put into the variable called "FLAG"! Just have your shell print it out!

You can accomplish this using a number of ways, but we'll start with ```echo```. This command just prints stuff. For example:
```sh
hacker@dojo:~$ echo Hello Hackers!
Hello Hackers!
```
You can also print out variables with ```echo```, by prepending the variable name with a ```$```. For example, there is a variable, ```PWD```, that always holds the current working directory of the current shell. You print it out as so:
```sh
hacker@dojo:~$ echo $PWD
/home/hacker
```
Now it's your turn. Have your shell print out the ```FLAG``` variable and solve this challenge!

## Solution:

- It is mentioned that `echo` command can be used to print out vairbales when by prepending the variable with `$`. In this case, the variable to be printed is `FLAG`.
- Using `echo $FLAG` will give the flag.

## Flag: 

```
pwn.college{wMjZGSxwarn9ZlUiySyl37sA5pP.QX3UTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `echo` can be used to print variables.
- The variables can be printed by prepending it with `$`.

# Setting Variables 

Naturally, as well as reading values stored in variables, you can write values to variables. This is done, as with many other languages, using `=`. To set variable `VAR` to value `1337`, you would use:
```sh
hacker@dojo:~$ VAR=1337
```
Note that there are no spaces around the `=`! If you put spaces (e.g., `VAR = 1337`), the shell won't recognize a variable assignment and will, instead, try to run the `VAR` command (which does not exist).

Also note that this uses `VAR` and not `$VAR`: the `$` is only prepended to access variables. In shell terms, this prepending of `$` triggers what is called variable expansion, and is, surprisingly, the source of many potential vulnerabilities (if you're interested in that, check out the Art of the Shell dojo when you get comfortable with the command line!).

After setting variables, you can access them using the techniques you've learned previously, such as:
```sh
hacker@dojo:~$ echo $VAR
1337
```
To solve this level, you must set the `PWN` variable to the value `COLLEGE`. Be careful: both the names and values of variables are case-sensitive! `PWN` is not the same as `pwn` and `COLLEGE` is not the same as `College`.

## Solution:

- Given that a value can be assigned to a variable using `=`.
-  Command `PWN=COLLEGE` to set the value `COLLEGE` to the variable `PWN` to get the flag.

## Flag: 

```
pwn.college{IY2YlsFjFBv0MZfN0jZMF7TU6pP.QX5UTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- A value can be assigned to a variable using `=`.
- There should be no space between `=`, the variable and the value being assigned to the variable.


# Multi-word Variables

In this level, you will learn about quoting. Spaces have special significance in the shell, and there are places where you can't use them spuriously. Recall our variable setting:
```sh
hacker@dojo:~$ VAR=1337
```
That sets the `VAR` variable to `1337`, but what if you wanted to set it to `1337 SAUCE`? You might try the following:
```sh
hacker@dojo:~$ VAR=1337 SAUCE
```
This looks reasonable, but it does not work, for similar reasons to needing to have no spaces around the `=`. When the shell sees a space, it ends the variable assignment and interprets the next word (`SAUCE` in this case) as a command. To set `VAR` to `1337 SAUCE`, you need to quote it:
```sh
hacker@dojo:~$ VAR="1337 SAUCE"
```
Here, the shell reads `1337 SAUCE` as a single token, and happily sets that value to VAR. In this level, you'll need to set the variable `PWN` to `COLLEGE YEAH`. Good luck!

## Solution:

- It is mentioned that the value of a variable should be put in `" "` if there is a space to in the value to be stored. Here the value to be stored is `COLLEGE YEAH`.
- The value `COLLEGE YEAH` is assigned to the variable `PWN` by the command `PWN="COLLEGE YEAH"`.

## Flag: 

```
pwn.college{wtnpsB5BH_TtHQnKhVuODTM8D7y.QXwYTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- When the shell sees a space, it ends the variable assignement.
- To input a multiworded value to a variable, the value has to be inside `" "`.
  
# Exporting Variables 

By default, variables that you set in a shell session are local to that shell process. That is, other commands you run won't inherit them. You can experiment with this by simply invoking another shell process in your own shell, like so:
```sh
hacker@dojo:~$ VAR=1337
hacker@dojo:~$ echo "VAR is: $VAR"
VAR is: 1337
hacker@dojo:~$ sh
$ echo "VAR is: $VAR"
VAR is:
```
In the output above, the `$` prompt is the prompt of `sh`, a minimal shell implementation that is invoked as a child of the main shell process. And it does not receive the `VAR` variable!

This makes sense, of course. Your shell variables might have sensitive or weird data, and you don't want it leaking to other programs you run unless it explicitly should. How do you mark that it should? You export your variables. When you export your variables, they are passed into the environment variables of child processes. You'll encounter the concept of environment variables in other challenges, but you'll observe their effects here. Here is an example:
```sh
hacker@dojo:~$ VAR=1337
hacker@dojo:~$ export VAR
hacker@dojo:~$ sh
$ echo "VAR is: $VAR"
VAR is: 1337
```
Here, the child shell received the value of `VAR` and was able to print it out! You can also combine those first two lines.
```sh
hacker@dojo:~$ export VAR=1337
hacker@dojo:~$ sh
$ echo "VAR is: $VAR"
VAR is: 1337
```
In this challenge, you must invoke `/challenge/run` with the `PWN` variable exported and set to the value `COLLEGE`, and the `COLLEGE` variable set to the value `PWN` but not exported (e.g., not inherited by /challenge/run). Good luck!

## Solution:

- It is given that the value `COLLEGE` should be given to the variable `PWN` which then should be exprted.
- This is done using the command `export PWN=COLLEGE`.
- It is the mentioned that the value `PWN` should be given to the variable `COLLEGE` but this varibale does not hvae to be exported.
- This is done using the command `COLLEGE=PWN`.
- Then run `/challenge/run` to get the flag.

## Flag: 

```
pwn.college{0-AtpZyY04oNIA5fWsxQbySrI9O.QXyYTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:
- Variables in a shell are local to that shell only and other commands that are run won't inherit them automatically.
- By using the `export` command, a variable can be accessed from outside the shell it is initially written in. 

# Printing Exported Variables 

There are multiple ways to access variables in bash. `echo` was just one of them, and we'll now learn at least one more in this challenge.

Try the `env` command: it'll print out every exported variable set in your shell, and you can look through that output to find the `FLAG` variable!

## Solution:

- It is mentioned that the `env` command can be used to print all exported variables.
- Using this command, multiple exported variables are printed from which the flag can be found. 

## Flag: 

```
pwn.college{0JBLS62KFWqUyuGjhri5IHkfRzU.QX4UTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `env` command is used to print out exported variables.
  
# Storing Command Output

In the course of working with the shell, you will often want to store the output of some command into a variable. Luckily, the shell makes this quite easy using something called `Command Substitution`! Observe:
```sh
hacker@dojo:~$ FLAG=$(cat /flag)
hacker@dojo:~$ echo "$FLAG"
pwn.college{blahblahblah}
hacker@dojo:~$
```
Neat! Now, you practice. Read the output of the `/challenge/run` command directly into a variable called `PWN`, and it will contain the flag!

Trivia: You can also use backticks instead of `$(): FLAG=`cat /flag` instead of `FLAG=$(cat /flag)` in the example above. This is an older format, and has some disadvantages (for example, imagine if you wanted to nest command substitutions. How would you do `$(cat $(find / -name flag))` with backticks? The official stance of pwn.college is that you should use `$(blah)` instead of ``blah``.

## Solution:

- The output of the command `/challenge/run` is directly read into the variable `PWN` by using `PWN=$(/challenge/run)`.
- The `PWN` variable is the printed using `echo $PWN` to get the flag.

## Flag: 

```
pwn.college{QVZiesC2boIum9r2swi3wUoXrEf.QX1cDN1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The output of a command can be read into a variable using `$`.
- For exmaple, `FLAG=$(/flag)` stores the output of the command `/flag` into `FLAG`.

# Reading Input 

We'll start with reading input from the user (you). That's done using the aptly named `read` builtin, which reads input into a variable!

Here is an example using the `-p` argument, which lets you specify a prompt (otherwise, it would be hard for you, reading this now, to separate input from output in the example below):
```sh
hacker@dojo:~$ read -p "INPUT: " MY_VARIABLE
INPUT: Hello!
hacker@dojo:~$ echo "You entered: $MY_VARIABLE"
You entered: Hello!
```
Keep in mind, `read` reads data from your standard input! The first `Hello!`, above, was inputted rather than outputted. Let's try to be more explicit with that. Here, we annotated the beginning of each line with whether the line represents `INPUT` from the user or `OUTPUT` to the user:
```sh
 INPUT: hacker@dojo:~$ echo $MY_VARIABLE
OUTPUT:
 INPUT: hacker@dojo:~$ read MY_VARIABLE
 INPUT: Hello!
 INPUT: hacker@dojo:~$ echo "You entered: $MY_VARIABLE"
OUTPUT: You entered: Hello!
```
In this challenge, your job is to use `read` to set the `PWN` variable to the value `COLLEGE`. Good luck!

## Solution:

- It is menationed that the `read` command can be used to get input from the user and store it in a variable.
- Here, the user input is stored in the variable `PWN` by using the command `read PWN`.
- The user input `COLLEGE` must then be entered to get the flag. 

## Flag: 

```
pwn.college{sROzRzjic2T-m2NNnSy260GMtf3.QX4cTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `read` command is used to get input from the user and store it in a variable.
- `-p` argument, can be used to specify a prompt.

# Reading Files 

Often, when shell users want to read a file into an environment variable, they do something like:
```sh
hacker@dojo:~$ echo "test" > some_file
hacker@dojo:~$ VAR=$(cat some_file)
hacker@dojo:~$ echo $VAR
test
```
This works, but it represents what grouchy hackers call a `"Useless Use of Cat"`. That is, running a whole other program just to read the file is a waste. It turns out that you can just use the powers of the shell!

Previously, you `read` user input into a variable. You've also previously redirected files into command input! Put them together, and you can read files with the shell.
```sh
hacker@dojo:~$ echo "test" > some_file
hacker@dojo:~$ read VAR < some_file
hacker@dojo:~$ echo $VAR
test
```
What happened there? The example redirects `some_file` into the standard input of `read`, and so when `read` reads into `VAR`, it reads from the file! Now, use that to read `/challenge/read_me` into the `PWN` environment variable, and we'll give you the flag! The `/challenge/read_me` will keep changing, so you'll need to read it right into the `PWN` variable with one command!

## Solution:

- Using the `read` command, `/challenge/read_me` can be redirected into `PWN` by using the command `read PWN</challenge/read_me`.

## Flag: 

```
pwn.college{cqp1LiSFhCxlf1q4F6hVjdllV2p.QXwIDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- A file can be redireded into another file by using the `read` command.
