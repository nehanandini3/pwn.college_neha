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
Note that there are no spaces around the =! If you put spaces (e.g., VAR = 1337), the shell won't recognize a variable assignment and will, instead, try to run the VAR command (which does not exist).

Also note that this uses VAR and not $VAR: the $ is only prepended to access variables. In shell terms, this prepending of $ triggers what is called variable expansion, and is, surprisingly, the source of many potential vulnerabilities (if you're interested in that, check out the Art of the Shell dojo when you get comfortable with the command line!).

After setting variables, you can access them using the techniques you've learned previously, such as:

hacker@dojo:~$ echo $VAR
1337
To solve this level, you must set the PWN variable to the value COLLEGE. Be careful: both the names and values of variables are case-sensitive! PWN is not the same as pwn and COLLEGE is not the same as College.

# Multi-word Variables
# Exporting Variables 
# Printing Exported Variables 
# Storing Command Output
# Reading Input 
# Reading Files 
