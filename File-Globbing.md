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



# Challenge 1 name

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
- 

## Flag: 

```
pwn.college{}
```

### References:

- none

### Notes:

Include things you learnt, alternate methods or mistakes you made while solving










