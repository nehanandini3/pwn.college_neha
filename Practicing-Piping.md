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



