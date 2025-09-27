# Learning From Documentation

The typical need you'll have for documentation is just to figure out how to use all these dang programs, and a specific case of that is figuring out what arguments to specify on the command line. This module will mostly dig into that concept, as a proxy for figuring out how to use the programs in general. Through the rest of the module, you'll go through various ways of asking the environment for help for the programs, but first, we'll dig into the concept of reading documentation.

The correct usage of programs depends, in a large part, on the proper specification of arguments to them. Recall the `-a` of `ls -a` in the `hidden files` challenge of the `Basic Commands` module: that `-a` was an argument that told `ls` to list out hidden files as well as non-hidden files. Because we wanted to list out hidden files, invoking `ls` with the `-a` argument was the correct way to use it in our scenario.

The program for this challenge is `/challenge/challenge`, and you'll need to invoke it properly in order for it to give you the flag. Let's pretend that this is its documentation:

```sh
Welcome to the documentation for `/challenge/challenge`! To properly run this program, you will need to pass it the argument of `--giveflag`. Good luck!
```

Given that knowledge, go get the flag!

## Solution:

- It is explicitly mentioned that `/challenge/challenge` needs to be invoked with the argument `--giveflag`.
- The command thus is `/challenge/challenge --giveflag`.

## Flag: 

```
pwn.college{Umbp6DQ0XEHhk5hGEKJUTjWS-f9.QX0ITO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- For documentation, it is important to figure out what arguments to specify on the command line.
- `-a` was an argument that told `ls` to list out hidden files as well as non-hidden files.



# Learning Complex Usage

While using most commands is straightforward, the usage of some commands can get quite complex. For example, the arguments to commands like `sed` and `awk`, which we're definitely not getting into right now, are entire programs in an esoteric programming language! Somewhere on the spectrum between `cd` and `awk` are commands that take arguments to their arguments...

This sounds crazy, but you've already encountered this with the `find` level in `Basic Commands`. `find` has a `-name` argument, and the `-name` argument itself takes an argument specifying the name to search for. Many other commands are analogous.

Here is this level's documentation for `/challenge/challenge`:

```sh
Welcome to the documentation for `/challenge/challenge`! This program allows you to print arbitrary files to the terminal, when given the `--printfile` argument. The argument to the `--printfile` argument is the path of the flag to read. For example, `/challenge/challenge --printfile /challenge/DESCRIPTION.md` will print out the description of the level!
```

Given that documentation, go get the flag!

## Solution:

- First tried `/challenge/challenge --printfile /challenge/flag` which showed that its the correct argument but the flag didnt exist which suggests that the flag is not in this location.
- `ls -la /challenge` to see files and only found .init, DESCRIPTION.md, and challenge that are related to the problem.
- `/challenge/challenge --printfile /challenge/DESCRIPTION.md` to check if the program is working correctly. The argument syntax is correct since it gave the problem description.
- Used ` ls -la` to see all the files that are available.
- Found a symbolic link `not-the-flag -> /flag`.
- Executed `/challenge/challenge --printfile /flag` which prints the flag.

## Flag: 

```
pwn.college{E8VWQewy045h56936hRu050fnn8.QX1ITO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Command arguments can be nested.
- ex: `find` as it has a `-name` argument, and the `-name` argument itself takes an argument specifying the name to search for.



# Reading Manuals

This level introduces the `man` command. `man` is short for `manual`, and will display (if available) the manual of the command you pass as an argument. For example, let's say we wanted to learn about the `yes` command (yes, this is a real command):

```sh
hacker@dojo:~$ man yes
```
This will display the manual page for `yes`, which will look something like this:

```sh
YES(1)                           User Commands                          YES(1)

NAME
       yes - output a string repeatedly until killed

SYNOPSIS
       yes [STRING]...
       yes OPTION

DESCRIPTION
       Repeatedly output a line with all specified STRING(s), or 'y'.

       --help display this help and exit

       --version
              output version information and exit

AUTHOR
       Written by David MacKenzie.

REPORTING BUGS
       GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
       Report any translation bugs to <https://translationproject.org/team/>

COPYRIGHT
       Copyright  ©  2020  Free Software Foundation, Inc.  License GPLv3+: GNU
       GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
       This is free software: you are free  to  change  and  redistribute  it.
       There is NO WARRANTY, to the extent permitted by law.

SEE ALSO
       Full documentation <https://www.gnu.org/software/coreutils/yes>
       or available locally via: info '(coreutils) yes invocation'

GNU coreutils 8.32               February 2022                          YES(1)
```
The important sections are:

```sh
NAME(1)                           CATEGORY                          NAME(1)

NAME
	This gives the name (and short description) of the command or
	concept discussed by the page.

SYNOPSIS
	This gives a short usage synopsis. These synopses have a standard
	format. Typically, each line is a different valid invocation of the
	command, and the lines can be read as:

	COMMAND [OPTIONAL_ARGUMENT] SINGLE_MANDATORY_ARGUMENT
	COMMAND [OPTIONAL_ARGUMENT] MULTIPLE_ARGUMENTS...

DESCRIPTION
	Details of the command or concept, with detailed descriptions
	of the various options.

SEE ALSO
	Other man pages or online resources that might be useful.

COLLECTION                        DATE                          NAME(1)
```
You can scroll around the manpage with your arrow keys and PgUp/PgDn. When you're done reading the manpage, you can hit `q` to quit.

Manpages are stored in a centralized database. If you're curious, this database lives in the `/usr/share/man` directory, but you never need to interact with it directly: you just query it using the `man` command. The arguments to the `man` command aren't file paths, but just the names of the entries themselves (e.g., you run `man yes` to look up the `yes` manpage, rather than `man /usr/bin/yes`, which would be the actual path to the `yes` program but would result in man displaying garbage).

The challenge in this level has a secret option that, when you use it, will cause the challenge to print the flag. You must learn this option through the man page for `challenge`!

## Solution:

- Access the manpage for challenge by using `man challenge`.
- Read through the manpage for a secret option.
- DESCRIPTION has three options: fortune: read a fortune, version: outputs version information and exit & jxzfvj NUM: prints the flag if NUM is 477.
- From the above, its clear `/challenge/challenge --jxzfvj 477` prints the flag.

## Flag: 

```
pwn.college{M47jx7zXAfvQJBjOQACSwRvzcj_.QX0EDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `man` is short for manual.
- It displays (if available) the manual of the command passed as an argument.
- A commaned named `yes` exists.
- The manpage consists of NAME, SYNOPSIS, DESCRIPTION, AUTHOR, REPORTING BUGS, COPYRIGHT and SEE ALSO.
- The important parts to pay attention to are NAME, SYNOPSIS, DESCRIPTION and SEE ALSO.
- Arrow keys and PgUp/PgDn to scroll through the manpage.
- To quit manpage, `q` is entered.
- Manpages are stored in a centralized database.
- This database lives in the `/usr/share/man directory`, but no need to interact with it directly, just query it with `man` command.
- The arguments to the man command aren't file paths, but just the names of the entries themselves, i.e. `man command_name` and  not `man /path/to/command`.



# Searching Manuals

You can scroll man pages with the arrow keys (and PgUp/PgDn) and search with `/`. After searching, you can hit `n` to go to the next result and `N` to go to the previous result. Instead of `/`, you can use `?` to search backwards!

Find the option that will give you the flag by reading the `challenge` man page.

## Solution:

- `man challenge` to go the manpage of challenge.
- Scroll through the manpage using arrow keys and PgUp/PgDn.
- Search for `flag` using `/` for forward search and `?` for backward search.
- `n` for going to the next result and `N` for previous result.
- --hprygmc This argument will give you the flag! - This option clearly states that flag will be printed when executed.
- ` /challenge/challenge --hprygmc` gives the flag.

## Flag: 

```
pwn.college{Af7s76eL3ddQ34D9K6f7aULKZan.QX1EDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Scrolling through the manpage can be done by using the arrow keys and PgUp/PgDn.
- `/` is used for forward search and `?` for backward search.
- `n` is entered to go to the next result and `N` for previous result.



# Searching for Manuals

This level is tricky: it hides the manpage for the challenge by randomizing its name. Luckily, all of the manpages are gathered in a searchable database, so you'll be able to search the man page database to find the hidden challenge man page! To figure out how to search for the right manpage, read the `man` page manpage by doing: `man man`!

HINT 1: `man man` teaches you advanced usage of the `man` command itself, and you must use this knowledge to figure out how to search for the hidden manpage that will tell you how to use `/challenge/challenge`

HINT 2: though the manpage is randomly named, you still actually use `/challenge/challenge` to get the flag!

## Solution:

- Use `man man` to search the man page database to find the hidden challenge man page.
- The below is found in manpage of `man` when searched for `man -k` as its mentioned in the synopsis/
  man -k printf
           Search  the  short  descriptions and manual page names for the keyword printf as regular expression.  Print out any matches.
- `man -k challenge` gives cfqzluwvwz (1)       - print the flag!
- `man cfqzluwvwz` shows the manpage that has 3 options: fortune- read a fortune, version- output version information and exit & cfqzlu NUM- print the flag if NUM is 292.
- It is given in problem that `/challenge/challenge` is used to get the flag.
- Obviously `/challenge/challenge --cfqzlu 292` prints the flag.

## Flag: 

```
pwn.college{cf-qzl2FuMJASGH92XwvKUELDRM.QX2EDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- All of the manpages are gathered in a searchable database.
- `man man` teaches advanced usage of the `man` command itself.



# Helpful Programs

Some programs don't have a man page, but might tell you how to run them if invoked with a special argument. Usually, this argument is `--help`, but it can often be `-h` or, in rare cases, `-?`, `help`, or other esoteric values like `/?` (though that latter is more frequently encountered on Windows).

In this level, you will practice reading a program's documentation with `--help`. Try it out!

## Solution:

- `/challenge/challenge --help` to see how to get the flag.
- these 2 are required:
  -g GIVE_THE_FLAG, --give-the-flag GIVE_THE_FLAG
                        get the flag, if given the correct value
  -p, --print-value     print the value that will cause the -g option to give you the flag
- `/challenge/challenge -p` which gives the correct value that is 423.
- Finally, `/challenge/challenge -g 423` prints the flag.

## Flag: 

```
pwn.college{kCG4IBMQZLAFapYMDV2PVEGkLYi.QX3IDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Some programs don't have a man page, but might tell how to run them if invoked with a special argument.
- Usually, this argument is `--help`, but it can often be `-h` or, in rare cases, `-?`, `help`, or other esoteric values like `/?`.
- `/?` more frequently encountered on Windows.



# Help for Builtins

Some commands, rather than being programs with man pages and help options, are built into the shell itself. These are called builtins. Builtins are invoked just like commands, but the shell handles them internally instead of launching other programs. You can get a list of shell builtins by running the builtin `help`, as so:

```sh
hacker@dojo:~$ help
```
You can get help on a specific one by passing it to the `help` builtin. Let's look at a builtin that we've already used earlier, `cd`!

```sh
hacker@dojo:~$ help cd
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
    
    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.
...
```
Some good information! In this challenge, we'll practice using `help` to look up help for builtins. This challenge's `challenge` command is a shell builtin, rather than a program. Like before, you need to lookup its help to figure out the secret value to pass to it!

## Solution:

- `help challenge` to figure out how to use the challenge shell builtin to get the flag as its given that here challenge is a shell builtin.
- --secret VALUE    prints the flag, if VALUE is correct
  and the secret value is "wCDjCl9Q".
- `challenge --secret wCDjCl9Q` prints the flag.

## Flag: 

```
pwn.college{wCDjCl9QdEU30q2WgTzoCx8dUyI.QX0ETO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Builtins are commands are built into the shell itself rather than being programs with man pages and help options.
- Builtins are invoked just like commands, but the shell handles them internally instead of launching other programs.
- A list of shell builtins is shown when the builtin `help` is run.
- Help on a specific one is also shown when its passed to the help builtin.








