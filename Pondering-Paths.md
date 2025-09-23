
# The Root

Alright, so the filesystem starts at /. Under that, there are a whole mess of other directories, configuration files, programs, and, most importantly, flags. In this level, we've added a program right in /, called pwn, that will give you the flag. All you need to do for this level is to invoke this program!

You can invoke a program by providing its path on the command line. In this case, you'll be giving the exact path, starting from /, so the path would be /pwn. This style of path, one that starts with the root directory, is referred to as an "absolute path".

Start the challenge, launch a terminal, invoke the pwn program using its absolute path, and Capture that Flag! Good luck!

## Solution:

- Since the filesystem starts at `/`, typing `/pwn` gives the flag.

## Flag: 

```sh
pwn.college{w0DD2UcdxDKWCqzu5HY_6Y9nM37.QX4cTO0wCO0AzNzEzW}
```

### References:

- None

### Notes:

- A filesystem starts at `/`
- Other directories, configuration files, programs and files are under it.
- An absolute path is the exact path of a file which starts from the root directory (i.e. `/pwn`).
- An absolute path always starts with `/`
- `pwn` refers to a program named pwn that is in the root directory.



# Program and Absolute Paths

Let's explore a slightly more complicated path! Except for in the previous level, challenges in pwn.college are in the challenge directory and the challenge directory is, in turn, right in the root directory (/). The path to the challenge directory is, thus, /challenge. The name of the challenge program in this level is run, and it lives in the /challenge directory. Thus, the path to the run challenge program is /challenge/run.

This challenge again requires you to execute it by invoking its absolute path. You'll want to execute the run file that is in the challenge directory that is, in turn, in the / directory. If you invoke the challenge correctly, it will give you the flag. Good luck!

## Solution:

- The absolute path to a program is constructed by `/` followed by the directory and then `/` and the program name.
- Thus typing `/challenge/run` returns the flag.

## Flag: 

```sh
pwn.college{k2nP4Brg3wHqGi56MPR3CH0Igb1.QX1QTN0wCO0AzNzEzW}
```

### References:

- none
  
### Notes:

- The full absolute path consists of `/` with directory name and `/` with program name in that order (i.e. `/directory/program`).
- This command invokes the absolute path by executing the program run in the directory challenge.




# Position Thy Self

The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:
```sh
hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$
```
This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

This challenge will require you to execute the /challenge/run program from a specific path (which it will tell you). You'll need to cd to that directory before rerunning the challenge program. Good luck!

## Solution:

- First `/challenge/run` to get the directory (specific path) to which the directory is to be changed to.
- In this case, that directory is `/etc/apt/sources.list.d` so we use `cd /etc/apt/sources.list.d`.
- Once the directory is obtained, we use `/challenge/run` with it to get the flag - `/etc/apt/sources.list.d$ /challenge/run`.

## Flag: 

```sh
pwn.college{INeciiUhBxrEAr-bDco9NeWkCf7.QX2QTN0wCO0AzNzEzW}
```

### References:

- none
  
### Notes:

- Linux filesystem has lots of directories with lots of files.
- `cd` (change directory) command changes the shell's current working directory.
- Its syntax is `cd <directory_name>`.
- Current working directory is the directory in which a particular process is hanging out in at that instant.
- `~` command shows the current directory a shell is located in (`/home/hacker`).




# Position Elsewhere

The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:

hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$
This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

This challenge will require you to execute the /challenge/run program from a specific path (which it will tell you). You'll need to cd to that directory before rerunning the challenge program. Good luck!

## Solution:

- First `/challenge/run` to get the directory (specific path) to which the directory is to be changed to.
- In this case, that directory is `/usr/bin` so we use `cd /usr/bin`.
- Once the directory is obtained, we use `/challenge/run` with it to get the flag - `/usr/bin$ /challenge/run`.

## Flag: 

```sh
pwn.college{gM0Ie1oBFhZdEKkEDoIfLptyIkT.QX3QTN0wCO0AzNzEzW}
```


### References:

- none
  
### Notes:

- Linux filesystem has lots of directories with lots of files.
- `cd` (change directory) command changes the shell's current working directory.
- Its syntax is `cd <directory_name>`.
- Current working directory is the directory in which a particular process is hanging out in at that instant.
- `~` command shows the current directory a shell is located in (`/home/hacker`).




# Position Yet Elsewhere

The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:

hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$
This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

This challenge will require you to execute the /challenge/run program from a specific path (which it will tell you). You'll need to cd to that directory before rerunning the challenge program. Good luck!

## Solution:

- First `/challenge/run` to get the directory (specific path) to which the directory is to be changed to.
- In this case, that directory is `/etc` so we use `cd /etc`.
- Once the directory is obtained, we use `/challenge/run` with it to get the flag - `/etc$ /challenge/run`.

## Flag: 

```
pwn.college{sIdU-hql1xagikCcVhxNtw1XfGB.QX4QTN0wCO0AzNzEzW}
```

### References:

- none
 
### Notes:

- Linux filesystem has lots of directories with lots of files.
- `cd` (change directory) command changes the shell's current working directory.
- Its syntax is `cd <directory_name>`.
- Current working directory is the directory in which a particular process is hanging out in at that instant.
- `~` command shows the current directory a shell is located in (`/home/hacker`).




# Implicit Relative Paths, from /

Now you're familiar with the concept of referring to absolute paths and changing directories. If you put in absolute paths everywhere, then it really doesn't matter what directory you are in, as you likely found out in the previous three challenges.

However, the current working directory does matter for relative paths.

A relative path is any path that does not start at root (i.e., it does not start with /).
A relative path is interpreted relative to your current working directory (cwd).
Your cwd is the directory that your prompt is currently located at.
This means how you specify a particular file, depends on where the terminal prompt is located.

Imagine we want to access some file located at /tmp/a/b/my_file.

If my cwd is /, then a relative path to the file is tmp/a/b/my_file.
If my cwd is /tmp, then a relative path to the file is a/b/my_file.
If my cwd is /tmp/a/b/c, then a relative path to the file is ../my_file. The .. refers to the parent directory.
Let's try it here! You'll need to run /challenge/run using a relative path while having a current working directory of /. For this level, I'll give you a hint. Your relative path starts with the letter c 😊

## Solution:

- As its given that the relative path starts with c, the cwd (current working directory) is `/`.
- Use `cd /` to change the directory.
- `challenge/run` command to get the flag.

## Flag: 

```
pwn.college{csWYLTvs2yDGNv7OMS3eqKG-Xg1.QX5QTN0wCO0AzNzEzW}
```


### References:

- none
  
### Notes:

- For absolute paths, it doesn't matter what the current working directory is.
- The current working directory (cwd) does matter for relative paths.
- A relative path is any path that does not start at root (i.e., it does not start with `/`).
- A relative path is interpreted relative to the cwd.
- The cwd is the directory that the prompt is currently located at.
- Specifying a particular file depends on where the terminal prompt is located.
- The `..` refers to the parent directory.




# Explicit Relative Paths, from /

Previously, your relative path was "naked": it directly specified the directory to descend into from the current directory. In this level, we're going to explore more explicit relative paths.

In most operating systems, including Linux, every directory has two implicit entries that you can reference in paths: `.` and `..`. The first, `.`, refers right to the same directory, so the following absolute paths are all identical to each other:

-`/challenge`
-`/challenge/.`
-`/challenge/./././././././././`
-`/./././challenge/././`
The following relative paths are also all identical to each other:

-`challenge`
-`./challenge`
-`./././challenge`
-`challenge/.`
Of course, if your current working directory is `/`, the above relative paths are equivalent to the above absolute paths.

This challenge will get you using `.` in your relative paths. Get ready!

## Solution:

- `cd /` to changw the current working directory to `/`.
- `./challnge/run`, a command that uses relative path, to get the flag.

## Flag: 

```
pwn.college{AJSSyNHHgkX7thVvKmHvkFuU_C_.QXwUTN0wCO0AzNzEzW}
```

### References:

- none
  
### Notes:

- In most operating systems, including Linux, every directory has two implicit entries that can be referenced in paths: `.` and `..`.
- The first, `.` , refers to the current directory.
- These 4 absolute paths - `/challenge`, `/challenge/.`, `/challenge/./././././././././`, `/./././challenge/././` are all identical.
- These 4 relative paths - `challenge`, `./challenge`, `./././challenge`, `challenge/.` are also all identical.



# Implicit Relative Path

In this level, we'll practice referring to paths using `.` a bit more. This challenge will need you to run it from the `/challenge` directory. Here, things get slightly tricky.

Linux explicitly avoids automatically looking in the current directory when you provide a "naked" path. Consider the following:

```sh
hacker@dojo:~$ cd /challenge
hacker@dojo:/challenge$ run
```
This will not invoke /challenge/run. This is actually a safety measure: if Linux searched the current directory for programs every time you entered a naked path, you could accidentally execute programs in your current directory that happened to have the same names as core system utilities! As a result, the above commands will yield the following error:

```sh
bash: run: command not found
```
We'll explore the mechanisms behind this concept later, but in this challenge, we'll learn how to explicitly use relative paths to launch `run` in this scenario. The way to do this is to tell Linux that you explicitly want to execute a program in the current directory, using . like in the previous levels. Give it a try now!

## Solution:

- `cd /challenge` to go to the directory challenge.
- `./run` to explicitly tell Linux to run the program from the current working directory using the `.` symbol.

## Flag: 

```
pwn.college{kIzs2Iuq3M3M8S_YaFEyAG9_S4i.QXxUTN0wCO0AzNzEzW}
```


### References:

- none
  
### Notes:

- Linux explicitly avoids automatically looking in the current directory when a "naked" path is provided.
- This is actually a safety measure to prevent accidentally executing programs in the current directory that happened to have the same names as core system utilities.
- So a the error `bash: run: command not found` is shown.
- To run a program in the current directory, explicitly specify it to Linux using a path.



# Home Sweet Home

Every user has a home directory, typically under `/home` in the filesystem. In the dojo, you are the `hacker` user, and your home directory is `/home/hacker`. The home directory is typically where users store most of their personal files. As you make your way through pwn.college, this is where you'll store most of your solutions.

Typically, your shell session will start with your home directory as your current working directory. Consider the initial prompt:

```sh
hacker@dojo:~$
```
The `~` in this prompt is the current working directory, with `~` being shorthand for `/home/hacker`. Bash provides and uses this shorthand because, again, most of your time will be spent in your home directory. Thus, whenever bash sees `~` provided as the start of an argument in a way consistent with a path, it will expand it to your home directory. Consider:

```sh
hacker@dojo:~$ echo LOOK: ~
LOOK: /home/hacker
hacker@dojo:~$ cd /
hacker@dojo:/$ cd ~
hacker@dojo:~$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~
hacker@dojo:~$ cd /home/hacker/asdf
hacker@dojo:~/asdf$
```
Note that the expansion of `~` is an absolute path, and only the leading `~` is expanded. This means, for example, that `~/~` will be expanded to `/home/hacker/~` rather than `/home/hacker/home/hacker`.

Fun fact: `cd` will use your home directory as the default destination:

```sh
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ cd
hacker@dojo:~$
```
Now it's your turn to play! In this challenge, `/challenge/run` will write a copy of the flag to any file you specify as an argument on the commandline, with these constraints:

Your argument must be an absolute path.
The path must be inside your home directory.
Before expansion, your argument must be three characters or less.
Again, you must specify your path as an argument to `/challenge/run` as so:

```sh
hacker@dojo:~$ /challenge/run YOUR_PATH_HERE
```

## Solution:

- `/challenge/run ~/f` command to get the flag.
- It meets all the constraints given.
- It is an absolute path `/f` - starts with `/`.
- Path is inside home directory as `~/f` expans to `/home/hacker/f`.
- Before expressions, argument is exactly 3 letters - `~`, `/` and `a`.

## Flag: 

```
pwn.college{INkLeRLhScm2NSiq-q7e3SLvYTk.QXzMDO0wCO0AzNzEz}
```

### References:

- none

### Notes:

- Every user has a home directory, typically under `/home` in the filesystem.
- In the dojo, one is the hacker user, and hence the home directory is `/home/hacker`.
- The home directory is typically where users store most of their personal files.
- The `~` is shorthand for the home directory, `/home/hacker`.
- The expansion of `~` is an absolute path, and only the leading `~` is expanded, i.e. `~/~` will be expanded to `/home/hacker/~` rather than `/home/hacker/home/hacker`.
- `cd` uses the home directory as the default destination.
















