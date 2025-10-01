# Matching with *

The first glob we'll learn is `*`. When it encounters a `*` character in any argument, the shell will treat it as a "wildcard" and try to replace that argument with any files that match the pattern. It's easier to show you than explain:

```sh
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_*
Look: file_a file_b file_c
```
Of course, though in this case, the glob resulted in multiple arguments, it can just as simply match only one. For example:

```sh
hacker@dojo:~$ touch file_a
hacker@dojo:~$ ls
file_a
hacker@dojo:~$ echo Look: file_*
Look: file_a
```
When zero files are matched, by default, the shell leaves the glob unchanged:

```sh
hacker@dojo:~$ touch file_a
hacker@dojo:~$ ls
file_a
hacker@dojo:~$ echo Look: nope_*
Look: nope_*
```
The `*` matches any part of the filename except for `/` or a leading `.` character. For example:

```sh
hacker@dojo:~$ echo ONE: /ho*/*ck*
ONE: /home/hacker
hacker@dojo:~$ echo TWO: /*/hacker
TWO: /home/hacker
hacker@dojo:~$ echo THREE: ../*
THREE: ../hacker
```
Now, practice this yourself! Starting from your home directory, change your directory to `/challenge`, but use globbing to keep the argument you pass to `cd` to at most four characters! Once you're there, `run /challenge/run` for the flag!

## Solution:

- The challenge requres to change the directory to `/challenge`, but use globbing to keep the argument passed to cd to at most four characters.
- As `/challenge` needs to be matched uniqely, `echo /c*` is entered to see all the directories that start with c and only `/challenge` is given as result.
- `cd /c*` is run, `/c*` is only 3 characters long. This changes the directory to `/challenge`.
- `/challenge/run` gives the flag.

## Flag: 

```
pwn.college{8gQW-VPGxKDLU-rlnLpsSAygs9l.QXxIDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Globbing means filename is expanded using wildcards.
- When zero files are matched, by default, the shell leaves the glob unchanged
- `*` matches any sequence of characters except for `/` and leading `.` in path components.
- Expansion happens before the command executes.



# Matching with ?

Next, let's learn about `?`. When it encounters a `?` character in any argument, the shell will treat it as a single-character wildcard. This works like `*`, but only matches one character. For example:

```sh
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_cc
hacker@dojo:~$ ls
file_a	file_b	file_cc
hacker@dojo:~$ echo Look: file_?
Look: file_a file_b
hacker@dojo:~$ echo Look: file_??
Look: file_cc
```
Now, practice this yourself! Starting from your home directory, change your directory to `/challenge`, but use the `?` character instead of `c` and `l` in the argument to `cd`! Once you're there, run `/challenge/run` for the flag!

## Solution:

- The problem requires to use `?` to replace both `c` and `l` in `/challenge` in the argument of 'cd'.
- So `/challenge` becomes `/?ha???nge`.
- `cd /?ha??enge` is run to change the directory to '/challenge'.
- `/challenge/run` is executed to get the flag.

## Flag: 

```
pwn.college{sgC3mMG4FdWg7KG957wpNoL3Nq2.QXyIDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The shell treats `?` as a single-character wildcard.
- While `?` and `*` work exactly the same, `?` matches exactly one character whereas `*` matches zero or more characters.
- Each `?` occupies exactly one character position in the filename.
- Using consecutive `??` matches exactly two unknown characters.



# Matching with []

Next, we will cover `[]`. The square brackets are, essentially, a limited form of `?`, in that instead of matching any character, `[]` is a wildcard for some subset of potential characters, specified within the brackets. For example, `[pwn]` will match the character `p`, `w`, or `n`. For example:

```h
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_[ab]
Look: file_a file_b
```
Try it here! We've placed a bunch of files in `/challenge/files`. Change your working directory to `/challenge/files` and run `/challenge/run` with a single argument that bracket-globs into `file_b`, `file_a`, `file_s`, and `file_h`!

## Solution:

- This challenge requires to change the working directory to `/challenge/files` and then run `/challenge/run` with a single argument that uses bracket globbing to match the files - `file_b`, `file_a`, `file_s`, and `file_h`.
- First `cd /challenge/run` to change the directory to `/challenge`.
- `ls` to see what all files are in `/challenge`. This is not a necessary step.
- As `file_b`, `file_a`, `file_s`, and `file_h` are to be matched the argument will be `file_[absh]` as all the 4 files start with `file_`.
- `/challenge/run file_[absh]` is run to get the flag.

## Flag: 

```
pwn.college{cYaK7hPsd7PC5qOEr_ypAz8zCpJ.QXzIDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The square brackets are, essentially, a limited form of `?`, in that instead of matching any character, `[]` is a wildcard for some subset of potential characters, specified within the brackets.
- `[abcd]` is more precise than `?` as the latter only matches the specified characters.
- Individual characters are listed inside the brackets in any order, `/challenge/run file_[bash]` in the this problem would have given the flag too.



# Matching Paths with []

Globbing happens on a path basis, so you can expand entire paths with your globbed arguments. For example:

```sh
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: /home/hacker/file_[ab]
Look: /home/hacker/file_a /home/hacker/file_b
```
Now it's your turn. Once more, we've placed a bunch of files in `/challenge/files`. Starting from your home directory, run `/challenge/run` with a single argument that bracket-globs into the absolute paths to the `file_b`, `file_a`, `file_s`, and `file_h` files!

## Solution:

- This challenge requires to create a single glob pattern that matches these absolute paths `/challenge/files/file_b`, `/challenge/files/file_a`, `/challenge/files/file_s` and `/challenge/files/file_h`.
- These need to be run from `/home/hacker` without changing directories first.
- Looking at the 4 absolute paths, it is obvious that the common absolute path pattern is `/challenge/files/file_` + `a character`.
- So the abslute path would be `/challenge/files/file_[bash]`.
- `/challenge/run /challenge/files/file_[bash]` gives the flag.

## Flag: 

```
pwn.college{oQtceEhpBrGWcmdC59_mYPm29T8.QX0IDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Globs work with full paths not just local filenames.
- Globbing happens on a path basis, so an entire path can be expanded with the globbed arguments.
- A single glob pattern can expand to multiple file paths.



# Multiple Globs

So far, you've specified one glob at a time, but you can do more! Bash supports the expansion of multiple globs in a single word. For example:

```sh
hacker@dojo:~$ cat /*fl*
pwn.college{YEAH}
hacker@dojo:~$
```
What happens above is that the shell looks for all files in `/` that start with anything (including nothing), then have an `f` and an `l`, and end in anything (including `ag`, which makes `flag`).

Now you try it. We put a few happy, but diversely-named files in `/challenge/files`. Go `cd` there and run `/challenge/run`, providing a single argument: a short (3 characters or less) globbed word with two `*` globs in it that covers every word that contains the letter `p`.

## Solution:

- This problem requires to go to `/challenge/files` and run `/challenge/run` with a single argument that is 3 characters or less, contains two `*` wildcards, and matches every file containing the letter `p`.
- `cd /challenge/files` to change the directory to `/challenge`.
- `*p*` is the pattern as the first `*` matches any characters (or none) before p, `p` - literal letter p and the last `*` matches any characters (or none) after p.
- The pattern also satisfies the condition that it is 3 characters long, contains two `*` wildcards and matches any file containing `p`.
- `/challenge/run *p*` to get the flag.

## Flag: 

```
pwn.college{gOvWYJ-wfXM5ILTzEod47jk8Y6Z.0lM3kjNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Bash supports the expansion of multiple globs in a single word.
- Multiple `*` can be used in a single pattern.
- `/*ab*` looks for all files in `/` that start with anything (including nothing), then have an `a` and an `b`, and end in anything.



# Mixing Globs

Now, let's put the previous levels together! We put a few happy, but diversely-named files in `/challenge/files`. Go `cd` there and, using the globbing you've learned, write a single, short (6 characters or less) glob that (when passed as an argument to `/challenge/run`) will match the files "challenging", "educational", and "pwning"!

## Solution:

- This challenge requires to go to `/challenge/file`s and create a single glob pattern (6 characters or less) that matches the files "challenging", "educational", and "pwning".
- All the 3 filenames contain the letter "n" but `*n*` would match too many files.
- A character set for the first letter is used as there is no big common factor between the 3.
- `[cep]*` is used as it matches for the 3 files. `[cep]` matches "c", "e", or "p" and `*` matches rest of the filename 
- `cd /challenge/files` to change the directory.
- `/challenge/run [cep]*` gives the flag.

## Flag: 

```
pwn.college{Qe4pfQmCdduPlrfTLq72q24ETes.QX1IDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- The `*` command is used as a wildcard to replace an argument with a specified file.
- The `?` command acts like `*` but only matches one character.
- The `[]` command is similar to `?` but rather, gives a set of characters inside the brackets, to match the argument of the file.



# Exclusionary Globbing

Sometimes, you want to filter out files in a glob! Luckily, `[]` helps you do just this. If the first character in the brackets is a `!` or (in newer versions of bash) a `^`, the glob inverts, and that bracket instance matches characters that aren't listed. For example:

```sh
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_[!ab]
Look: file_c
hacker@dojo:~$ echo Look: file_[^ab]
Look: file_c
hacker@dojo:~$ echo Look: file_[ab]
Look: file_a file_b
```
Armed with this knowledge, go forth to `/challenge/files` and run `/challenge/run` with all files that don't start with `p`, `w`, or `n`!

NOTE: The `!` character has a different special meaning in bash when it's not the first character of a `[]` glob, so keep that in mind if things stop making sense! `^` does not have this problem, but is also not compatible with older shells.

## Solution:

- This challenge requires to go to `/challenge/files` and run `/challenge/run` with all files that don't start with the letters `p`, `w`, or `n`.
- The pattern `[!pwn]` matches any single character that is NOT `p`, `w`, or `n`.
- `cd /challenge/files` to change the directory.
- `/challenge/run [!pwn]*` prints the flag.

## Flag: 

```
pwn.college{M5H8Hs0xmFXLmoRVnsEC_i4bKj1.QX2IDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- If the first character in the brackets is a `!` or (in newer versions of bash) a `^`, the glob inverts, and that bracket instance matches characters that aren't listed.
- `[!abc]` or `[^abc]` matches any character EXCEPT those listed.
- The `!` character has a different special meaning in bash when it's not the first character of a `[]` glob.
- `^` does not have this problem and does the same thing as `!` (i.e. negation function) but its more of a modern syntax so it is not compatible with older shells.



# Tab Completion

As tempting as it might be, using `*` to shorten what must be typed on the commandline can lead to mistakes. Your glob might expand to unintended files, and you might not spot it until the `rm` command is already running! No one is safe from this style of error.

A safer alternative when you are trying to specify a specific target is tab completion. If you hit tab in the shell, it'll try to figure out what you're going to type and automatically complete it. Auto-completion is super useful, and this challenge will explore its use in specifying files.

This challenge has copied the flag into `/challenge/pwncollege`, and you can freely `cat` that file. But you can't type the filename: we used some serious trickery to make sure that you must tab-complete it. Try it out!

```sh
hacker@dojo:~$ ls /challenge
DESCRIPTION.md  pwncollege
hacker@dojo:~$ cat /challenge/pwncollege
cat: /challenge/pwncollege: No such file or directory
hacker@dojo:~$ cat /challenge/pwn<TAB>
pwn.college{HECK YEAH}
hacker@dojo:~$
```
When you hit that tab key, the name will expand and you'll be able to read the file. Good luck!

## Solution:

- `ls /challenge` to see all the files in it.
- `cat /challenge/p` or `cat /challenge/pwn` and then pressing on tab gives the flag.

## Flag: 

```
pwn.college{MyUAl2B3VmJb17wK264OxP-VKXR.0FN0EzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Using `*` to shorten what must be typed on the commandline can lead to mistakes.
- The glob might expand to unintended files, and it might not be spotted it until the `rm` command is already running
- Tab key can be used in the shell to autocomplete.
- Tab completion happens in the shell before the command runs.
- Shell uses filesystem knowledge to complete paths.



# Multiple Options for Tab Completion

Consider the following situation:

```sh
hacker@dojo:~$ ls
flag  flamingo  flowers
hacker@dojo:~$ cat f<TAB>
```
There are multiple options! What happens?

What happens varies based on the specific shell and its options. By default `bash` will auto-expand until the first point when there are multiple options (in this case, `fl`). When you hit tab a second time, it'll print out those options. Other shells and configurations, instead, will cycle through the options.

This challenge has a `/challenge/files` directory with a bunch of files starting with `pwncollege`. Tab-complete from `/challenge/files/p` or so, and make your way to the flag!

## Solution:

- `cd /challenge/files` to changbe the directory.
- `cat /challenge/files/p<TAB>` goes to `cat /challenge/files/pwn`.
- Press tab again which shows a list of all the files that start with `pwn`.
- Multiple matches including "pwncollege-flag" are among the options displayed.
- Continue typing to "pwncollege-f" and tab twice to narrow down to files starting with that prefix.
- Type further to pwncollege-fla and tab to see only two remaining options.
- Complete to pwncollege-flag using Tab and execute to get the flag.

## Flag: 

```
pwn.college{Es15YGIgu5IDy86-wcKLK4iRe2d.0lN0EzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- If multiple matches exist, what happens varies based on the specific shell and its options.
- If multiple matches exist, by default bash will auto-expand to the longest common prefix.
- When tab is presses a second time, it'll print out those options.
- Other shells and configurations, instead, will cycle through the options.



# Tab Completion on Command

Tab completion is for more than files! You can also tab-complete commands. This level has a command that starts with `pwncollege`, and it'll give you the flag. Type `pwncollege` and hit the tab key to auto-complete it!

NOTE: You can auto-complete any command, but be careful: callous auto-completes without double-checking the result can wreak havoc in your shell if you accidentally run the wrong commands!

## Solution:

- `pwncollege<tab>` autocompletes to `pwncollege-18828 ` which is then executed to reveal the flag.

## Flag: 

```
pwn.college{YviUbF4kJVLKnzFIwsmmBAK4Vma.0VN0EzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- Tab completes commands too.
- Tab works for executable commands in path, not just files and directories.
- Shell searches all directories in path for matching executables.
- Always verify the completed command before executing.















