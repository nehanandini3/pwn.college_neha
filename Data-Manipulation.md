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
- To translate lower case characters to upper case and vice-versa in the file `/challenge/run`, use the command `/challenge/run | tr '[A-Z][a-z]' '[a-z][A-Z]'' to get the flag. 

## Flag: 

```
pwn.college{o9Oqi8msR0UpgFcpcuBV8MJ0F5B.01MxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

-`tr` command is used to translate characters from one character to another.
- The first character on the left of `|` is the character to be translated into the character to the right of it.

# Deleting characters
# Deleting newlines
# Extracting the first lines with head 
# Extracting specific sections of text
# Sorting data
