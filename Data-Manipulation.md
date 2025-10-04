# Translating characters

One of the purposes of piping data is to modify it. Many Linux commands will help you modify data in really cool ways. One of these is `tr`, which translates characters it receives over standard input and prints them to standard output.

In its most basic usage, `tr` translates the character provided in its first argument to the character provided in its second argument:
```sh
hacker@dojo:~$ echo OWN | tr O P
PWN
hacker@dojo:~$
```
It can also handle multiple characters, with the characters in different positions of the first argument replaced with associated characters in the second argument.
```sh
hacker@dojo:~$ echo PWM.COLLAGE | tr MA NE
PWN.COLLEGE
hacker@dojo:~$
```
Now, you try it! In this level, `/challenge/run` will print the flag but will swap the casing of all characters (e.g., `A` will become `a` and vice-versa). Can you undo it with `tr` and get the flag?

## Solution:

- Its mentioned that `tr` command can be used to translate characters from the first character mentioned to the second character.
- To translate lower case characters to upper case and vice-versa in the file `/challenge/run`, use the command `/challenge/run | tr '[A-Z][a-z]' '[a-z][A-Z]'` to get the flag. 

## Flag: 

```
pwn.college{o9Oqi8msR0UpgFcpcuBV8MJ0F5B.01MxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `tr` command is used to translate characters from one character to another.
- The first character on the left of `|` is the character to be translated into the character to the right of it.

# Deleting characters

`tr` can also translate characters to nothing (i.e., delete them). This is done via a `-d` flag and an argument of what characters to delete:
```sh
hacker@dojo:~$ echo PAWN | tr -d A
PWN
hacker@dojo:~$
```
Pretty simple! Now you give it a try. I'll intersperse some decoy characters (specifically: `^` and `%`) among the flag characters. Use `tr -d` to remove them!

## Solution:

- It is mentioned that the `tr` command can be used to delete a certain character by using the argument `-d`.
- To delete the characters `^` and `%`, the command `/challenge/run | tr -d ^%`.

## Flag: 

```
pwn.college{g1yS7NRKLMA3BGng6XilwldvslW.0FNxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `tr` command can be used to remove specific characters by using the argumnet `-d` with it.



# Deleting Newlines

A common class of characters to remove is a line separator. This happens when you have a stream of data that you want to turn into a single line for further processing. You can specify newlines almost like any other character, by escaping them:

```sh
hacker@dojo:~$ echo "hello_world!" | tr _ "\n"
hello
world!
hacker@dojo:~$
```
Here, the backslash (`\`) signifies that the character that follows it is a standin for a character that's hard to input into the shell normally. The newline, of course, is hard to input because when you typically hit `Enter`, you'll run the command itself. `\n` is a standin for this newline, and it must be in quotes to prevent the shell interpreter itself from trying to interpret it and pass it to `tr` instead.

Now, let's combine this with deletion. In this challenge, we'll inject a bunch of newlines into the flag. Delete them with `tr`'s `-d` flag and the escaped newline specification!

Fun fact! Want to actually replace a backslash (`\`) character? Because `\` is the escape character, you gotta escape it! `\\` will be treated as a backslash by `tr`. This isn't relevant to this challenge, but is a fun fact nonetheless!

## Solution:

- `/challenge/run | tr -d "\n"` gives the flag.
- `tr -d` deletes all occurrences of the specified characters which in this case is `\n` (newline character).

## Flag: 

```
pwn.college{kxKJrkmZn0PXjclGvrx8G7i2dRD.0VNxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- The backslash (`\`) signifies that the character that follows it is a standin for a character that's hard to input into the shell normally.
- `\n` is a standin for a newline, and it must be in quotes to prevent the shell interpreter itself from trying to interpret it and pass it to `tr` instead.
- `tr _ "\n"`, to add a new line or `tr -d "\n"`, to remove a new line.
- If a backslash (`\`) character is to replaced, it has to be escaped as `/` is the escape character, `\\` will be treated as a backslash by tr. 



# Extracting the First Lines with Head

In your Linux journey, you'll experience situations where you need to grab just the early output of very verbose programs. For this, you'll reach for `head`! The `head` command is used to display the first few lines of its input:

```sh
hacker@dojo:~$ cat /something/very/long | head
this
is
just
the
first
ten
lines
of
the
file
hacker@dojo:~$
```
By default, it shows the first 10 lines, but you can control this with the `-n` option:

```sh
hacker@dojo:~$ cat /something/very/long | head -n 2
this
is
hacker@dojo:~$
```
This challenge's `/challenge/pwn` outputs a bunch of data, and you'll need to pipe it through head to grab just the first 7 lines and then pipe them onwards to `/challenge/college`, which will give you the flag if you do this right! Your solution will be a long composite command with two pipes connecting three commands. Good luck!

## Solution:

- `/challenge/pwn | head -n 7 | /challenge/college` gives the flag.
- `/challenge/pwn` generates the total output.
- `head -n 7` extracts only the required first 7 lines from the output.
- `/challenge/college` processes the filtered output to return the flag.

## Flag: 

```
pwn.college{IUycE1Oy4X7AehVVldzEl-Ko1hu.0lNxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `head` command is used to display the first few lines of its input.
- By default, it usually shows the first 10 lines.
- The number of line to be displayed can be controlled using `head -n` and then specify the number of lines to be shown.



# Extracting Specified Sections of Text

Sometimes, you want to grab specific columns of data, such as the first column, the third column, or the 42nd column. For this, there's the `cut` command.

For example, imagine that you have the following data file:

```sh
hacker@dojo:~$ cat scores.txt
hacker 78 99 67
root 92 43 89
hacker@dojo:~$
```
You could use `cut` to extract specific columns:

```sh
hacker@dojo:~$ cut -d " " -f 1 scores.txt
hacker
root
hacker@dojo:~$ cut -d " " -f 2 scores.txt
78
92
hacker@dojo:~$ cut -d " " -f 3 scores.txt
99
43
hacker@dojo:~$
```
The `-d` argument specifies the column delimiter (how columns are separated). In this case, it's a space character. Of course, it has to be in quotes here so that the shell knows that the space is an argument rather than a space separating other arguments! The `-f` argument specifies the field number (which column to extract).

In this challenge, the `/challenge/run` program will give you a bunch of lines with random numbers and single characters (characters of the flag) as columns. Use `cut` to extract the flag characters, then pipe them to `tr -d "\n"` (like the previous level!) to join them together into a single line. Your solution will look something like `/challenge/run | cut ??? | tr ???`, with the `???` filled out.

## Solution:

- `/challenge/run | cut -d' ' -f 2 | tr -d '\n'` prints the flag.
- This command retreives the second column of `/challenge/run` using `cut` and then remove the new lines using `tr`,

## Flag: 

```
pwn.college{Udy-0ojVeTBLDaKf2HFQ6vDd9nk.01NxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `cut` command is essential for excracting column from a specified data.
- The `-d` argument specifies the column delimiter (how columns are separated). It has to be in quotes so that the shell knows that the space is an argument rather than a space separating other arguments.
- The `-f` argument specifies the field number (which column to extract).



# Sorting Data

Files (or output lines of commands) aren't always in the order you need them! The `sort` command helps you organize data. It reads lines from input (or files) and outputs them in sorted order:

```sh
hacker@dojo:~$ cat names.txt
  hack
  the
  planet
  with
  pwn
  college
hacker@dojo:~$ sort names.txt
  college
  hack
  planet
  pwn
  the
  with
hacker@dojo:~$
```
By default, `sort` orders lines alphabetically. Arguments can change this:

```sh
- `-r`: reverse order (Z to A)
- `-n`: numeric sort (for numbers)
- `-u`: unique lines only (remove duplicates)
- `-R`: random order!
```
In this challenge, there's a file at `/challenge/flags.txt` containing 100 fake flags, with the real flag mixed among them. When sorted alphabetically, the real flag will be at the end (we made sure of this when generating fake flags). Go get it!

## Solution:

- `sort /challenge/flags.txt` gives the flag.
- `sort` arranges all flags in alphabetical order (A-Z)
- Its mentioned that the real flag will be the end alphabetically so it is in the last line.

## Flag: 

```
pwn.college{wCjlYpzhmOqdk2yzwFr1Qu-d-ZN.0FM0MDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `sort` command helps to organize data.
- It reads lines from input (or files) and outputs them in sorted order.
- By default, `sort` orders lines alphabetically.
- Arguments can change this:
  - `-r`: reverse order (Z to A)
  - `-n`: numeric sort (for numbers)
  - `-u`: unique lines only (remove duplicates)
  - `-R`: random order







