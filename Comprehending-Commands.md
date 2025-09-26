# cat: not The Pet but The Command

One of the most critical Linux commands is `cat`. `cat` is most often used for reading out files, like so:

```sh
hacker@dojo:~$ cat /challenge/DESCRIPTION.md
One of the most critical Linux commands is `cat`.
`cat` is most often used for reading out files, like so:
```
`cat` will concatenate (hence the name) multiple files if provided multiple arguments. For example:

```sh
hacker@dojo:~$ cat myfile
This is my file!
hacker@dojo:~$ cat yourfile
This is your file!
hacker@dojo:~$ cat myfile yourfile
This is my file!
This is your file!
hacker@dojo:~$ cat myfile yourfile myfile
This is my file!
This is your file!
This is my file!
```
Finally, if you give no arguments at all, `cat` will read from the terminal input and output it. We'll explore that in later challenges...

In this challenge, I will copy the flag to the `flag` file in your home directory (where your shell starts). Go read it with `cat`!

## Solution:

- Its mentioned that `cat` command is most often used to read files and the particular file to be read here is the `flag` file in home directory.
- Using `cat flag` will give the flag.

## Flag: 

```
pwn.college{EEXDqwij635FB96aytc_6UdiGIn.QXxcTN0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `cat` is primarly used to read files in Linux.
- `cat` will concatenate multiple files if provided multiple arguments
- `cat` is short for concatenate.
- If no arguments are given, `cat` will read from the terminal input and output it



# Catting Absolute Path

In the last level, you did `cat flag` to read the flag out of your home directory! You can, of course, specify `cat`'s arguments as absolute paths:

```sh
hacker@dojo:~$ cat /challenge/DESCRIPTION.md
In the last level, you did `cat flag` to read the flag out of your home directory!
You can, of course, specify `cat`'s arguments as absolute paths:
...
```
In this directory, I will not copy it to your home directory, but I will make it readable. You can read it with `cat` at its absolute path: `/flag`.

FUN FACT: `/flag` is where the flag always lives in pwn.college, but unlike in this challenge, you typically can't access that file directly.

## Solution:

- Given that `cat`'s arguments can be absolute paths.
- Command `cat /flag` gives the flag.

## Flag: 

```
pwn.college{s_nwG08boogcLwt0FZDD7TjL96j.QX5ETO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- A file can be read by using `cat` at its absolute path.
- `/flag` is where the flag always lives in pwn.college



# More Catting Practice

You can specify all sorts of paths as arguments to commands, and we'll practice some more with `cat`. In this level, I'll put the flag in some crazy directory, and I will not allow you to change directories with `cd`, so no `cat flag` for you. You must retrieve the flag by absolute path, wherever it is.

## Solution:

- Since `cd` cant be used here, absolute path is given as argument with `cat`.
- Its mentioned that `/usr/share/cmake/flag` is the file where the flag is in.
- The command to be used in this case for the flag is ` cat /usr/share/cmake/flag`.

## Flag: 

```
pwn.college{4dvA4NLKvdp2FHj3j0jBlsAedDe.QXwITO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- When `cd` (change directory) cant be used, the absolute path is taken with `cat`.



# Grepping for a Needle in a Haystack

Sometimes, the files that you might `cat` out are too big. Luckily, we have the `grep` command to search for the contents we need! We'll learn it in this challenge.

There are many ways to `grep`, and we'll learn one way here:

```sh
hacker@dojo:~$ grep SEARCH_STRING /path/to/file
```
Invoked like this, `grep` will search the file for lines of text containing `SEARCH_STRING` and print them to the console.

In this challenge, I've put a hundred thousand lines of text into the `/challenge/data.txt` file. `grep` it for the flag!

HINT: The flag always starts with the text `pwn.college`.

## Solution:

- Given that the flag always starts with the taxt `pwn.college`,
- The command will be `grep pwn.college /challenge/data.txt`.

## Flag: 

```
pwn.college{MHLHILKZqx5_dLY36Bc5u6x5fdW.QX3EDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- If a file that is to be `cat` is too big, `grep` is used to search through the file for lines and print them out to the console.
- The flag always starts with the text pwn.college.



# Comparing Files

When looking for changes between similar files, eyeballing them might not be the most efficient approach! This is where the `diff` command becomes invaluable.

`diff` compares two files line by line and shows you exactly what's different between them. For example:

```sh
hacker@dojo:~$ cat file1
hello
world
hacker@dojo:~$ cat file2
hello
universe
hacker@dojo:~$ diff file1 file2
2c2
< world
---
> universe
```
The output tells us that line 2 changed (`2c2`), with `world` in the first file (`<`) being replaced by `universe` in the second file (`>`).

Sometimes, when new lines are added, you'll see something like:

```sh
hacker@dojo:~$ cat old
pwn
hacker@dojo:~$ cat new
pwn
college
hacker@dojo:~$ diff old new
1a2
> college
```
This tells us that after line 1 in the first file, the second file has an additional line (`1a2` means "after line 1 of file1, add line 2 of file2").

Now for your challenge! There are two files in `/challenge`:

`/challenge/decoys_only.txt` contains 100 fake flags
`/challenge/decoys_and_real.txt` contains all 100 fake flags plus the one real flag
Use `diff` to find what's different between these files and get your flag!

## Solution:

- `diff /challenge/decoys_only.txt /challenge/decoys_and_real.txt` gives which flag is the real one as 1st file contains all 100 fake flags but 2nd flag contains 100 fake flags and 1 real flag.
- `46a47` is the real flag.

## Flag: 

```
pwn.college{gyBdEbwRk2lbn72lv8xgCe2T1Pt.01MwMDOxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `diff` is used for comparing two different files.
- It compares files line by line and gives whats exactly different between them.
- `2c2` means that the 2nd line in the 1st file was changed to become the 2nd line in the 2nd file.
- `<` gives the line that is being replaced.
- `>` gives the line that the previous one was replaced to.
- `1a2` means that after line 1 in the first file, a new line has been added at line 2 of the second file.



# Listing Files

So far, we've told you which files to interact with. But directories can have lots of files (and other directories) inside them, and we won't always be here to tell you their names. You'll need to learn to list their contents using the `ls` command!

`ls` will list files in all the directories provided to it as arguments, and in the current directory if no arguments are provided. Observe:

```sh
hacker@dojo:~$ ls /challenge
run
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ ls /home/hacker
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$
```
In this challenge, we've named `/challenge/run` with some random name! List the files in `/challenge` to find it.

## Solution:

- Type in `ls /challenge` first and then the output is `7211-renamed-run-28324  DESCRIPTION.md`.
- When `ls /challenge/7211-renamed-run-28324` is entered, it gives `/challenge/7211-renamed-run-28324` which means its a file (as ls lists all the files in the directories).
- Thus `/challenge/7211-renamed-run-28324` is directly executed to get the flag.

## Flag: 

```
pwn.college{4ZoWvV6PHwiogGcJl4CKKBCYnX6.QX4IDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `ls` is used to display all the files in a directory that is passed as its argument.
- If no argument passed, `ls` shows the files in its current directory.



# Touching Files

Of course, you can also create files! There are several ways to do this, but we'll look at a simple command here. You can create a new, blank file by touching it with the `touch` command:

```sh
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ touch pwnfile
hacker@dojo:/tmp$ ls
pwnfile
hacker@dojo:/tmp$
```
It's that simple! In this level, please create two files: `/tmp/pwn` and `/tmp/college`, and run `/challenge/run` to get your flag!

## Solution:

- Two files are to be created in the `tmp` directory.
- For that, first use `cd tmp` to go that directory.
- Next, use `touch pwn` and `touch college` to create 2 files named pwn and college.
- Use `ls` to verify if the files have been created. (this is an optional step)
- `/challenge/run` to get the flag.

## Flag: 

```
pwn.college{QDPKeqKLh7VIbOeSk_ROVLPD1uN.QXwMDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `touch` command is for creating a new, blank file.



# Removing Files

Files are all around you. Like candy wrappers, there'll eventually be too many of them. In this level, we'll learn to clean up!

In Linux, you remove files with the `rm` command, as so:

```sh
hacker@dojo:~$ touch PWN
hacker@dojo:~$ touch COLLEGE
hacker@dojo:~$ ls
COLLEGE     PWN
hacker@dojo:~$ rm PWN
hacker@dojo:~$ ls
COLLEGE
hacker@dojo:~$
```
Let's practice. This challenge will create a `delete_me` file in your home directory! Delete it, then run `/challenge/check`, which will make sure you've deleted it and then give you the flag!

## Solution:

- It is required to create a new file called delete_me in home dictionary.
- By default the terminal is in home dictionary.
- So `touch delete_me` is used.
- Use `ls` to verify if the file has been created.
- `rm delete_me` to delete the file.
- `/challenge/check` which makes sure the file is deleted and gives the flag.

## Flag: 

```
pwn.college{4utsRyKDbci39WkgD3bqPkrTteA.QX2kDM1wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `rm` is used to delete/remove files.



# Moving Files

You can also move files around with the `mv` command. The usage is simple:

```sh
hacker@dojo:~$ ls
my-file
hacker@dojo:~$ cat my-file
PWN!
hacker@dojo:~$ mv my-file your-file
hacker@dojo:~$ ls
your-file
hacker@dojo:~$ cat your-file
PWN!
hacker@dojo:~$
```
This challenge wants you to move the `/flag` file into `/tmp/hack-the-planet` (do it)! When you're done, run `/challenge/check`, which will check things out and give the flag to you.

## Solution:

- `mv /flag /tmp/hack-the-planet` to move the `/flag` into `/tmp/hack-the-planet`.
- `/challenge/check` which will check things out and prints the flag.

## Flag: 

```
pwn.college{8LcgRIrnSriVadKhRs8FWODxhZR.0VOxEzNxwCO0AzNzEzW}
```

### References:

- none

### Notes:

- `mv` is used to move files around.
- its syntax is `mv source destination`.



# Hidden Files

Interestingly, `ls` doesn't list all the files by default. Linux has a convention where files that start with a `.` don't show up by default in `ls` and in a few other contexts. To view them with `ls`, you need to invoke `ls` with the `-a` flag, as so:

```sh
hacker@dojo:~$ touch pwn
hacker@dojo:~$ touch .college
hacker@dojo:~$ ls
pwn
hacker@dojo:~$ ls -a
.college	pwn
hacker@dojo:~$
```
Now, it's your turn! Go find the flag, hidden as a dot-prepended file in `/`.

## Solution:

- `cd /` to go to `/` directory as its mentioned that the hidden file is within it.
- `ls` to see all files present under `/`.
- `ls -a` to see all the hidden files in `/` directory.
- Given that the hidden file required to find is a dot-prepended file - `.flag-11919273917272`.
- `cat .flag-11919273917272` to get the flag as cat reals the file's contents.

## Flag: 

```
pwn.college{Y51IbO2iJh2Sr8Zpsh2eBbOluOs.QXwUDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `ls` doesn't list all the files by default.
- Linux has a convention where files that start with a `.` don't show up by default in `ls` and in a few other contexts.
- To view them with `ls`, `ls -a` is used.



# An Epic Filesystem Quest

With your knowledge of `cd`, `ls`, and `cat`, we're ready to play a little game!

We'll start it out in `/`. Normally:

```sh
hacker@dojo:~$ cd /
hacker@dojo:/$ ls
bin   challenge  etc   home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  dev        flag  lib   lib64  media   opt  root  sbin  sys  usr
```
That's a lot of contents! One day, you will be quite familiar with them, but already, you might recognize the `flag` file and the challenge `directory`.

In this challenge, I have hidden the flag! Here, you will use `ls` and `cat` to follow my breadcrumbs and find it! Here's how it'll work:

Your first clue is in `/`. Head on over there.
Look around with `ls`. There'll be a file named HINT or CLUE or something along those lines!
`cat` that file to read the clue!
Depending on what the clue says, head on over to the next directory (or don't!).
Follow the clues to the flag!
Good luck!

## Solution:

- It is mentioned to go to `/` first so `cd /`.
- Use `ls` and then `cat SECRET` as secret is similar to hint/clue.
- `cd /usr/share/icons/Adwaita/32x32/devices` and `ls -a` as its mentioned the next clue is hidden.
- `cat .WHISPER` gives the next clue which is also hidden, so after `cd /usr/share/racket/pkgs/srfi-lib/srfi/%3a71`, enter `ls -a`.
- `cat .TRACE` yet again gives the next clue which is hidden, so use `cd /usr/share/javascript/mathjax/jax/output/SVG/fonts/STIX-Web/Shapes/Bold` and `ls -a`.
- `cat .ALERT` which leads to `cd /usr/lib/python3/dist-packages/docutils/utils/__pycache__` and then `ls -a`.
- `cat BLUEPRINT` as except for BLUEPRINT file, all other files were a .pyc file.
- This gives the next clue which is trapped and since it self-destruct if `cd` is used, `ls -a /usr/share/racket/pkgs/parser-tools-doc/parser-tools/compiled` leads to using `cat /usr/share/racket/pkgs/parser-tools-doc/parser-tools/compiled/DOSSIER-TRAPPED`.
- DOSSIER-TRAPPED is used as it does not have a file extension, unlike the others.
- This returns the next clue which is delayed, `cd` has to be used.
- `cd /usr/share/icons/hicolor/48x48/stock/code` and then `ls -a` which leads to `cat EVIDENCE`.
- The next clue is delayed.
- `cd /usr/share/javascript/mathjax/unpacked/jax/output/HTML-CSS/fonts/Asana-Math/Size4/Regular` and then `ls -a` that leads to `cat BRIEF`.
- The next clue is trapped.
- `ls -a /usr/share/racket/pkgs/drracket/drracket/private/syncheck` and then `cat /usr/share/racket/pkgs/drracket/drracket/private/syncheck/CLUE-TRAPPED` as CLUE-TRAPPED matches the given condition.
- Voila, the previous command prints the flag!

## Flag: 

```
pwn.college{szzRGdoGiIcbOqG-8HqMeMwaZAw.QX5IDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Pay close attention to files and directories while using ls.
- Pay close attention to each clue.



# Making Directories

We can create files. How about directories? You make directories using the `mkdir` command. Then you can stick files in there!

Watch:

```sh
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ mkdir my_directory
hacker@dojo:/tmp$ ls
my_directory
hacker@dojo:/tmp$ cd my_directory
hacker@dojo:/tmp/my_directory$ touch my_file
hacker@dojo:/tmp/my_directory$ ls
my_file
hacker@dojo:/tmp/my_directory$ ls /tmp/my_directory/my_file
/tmp/my_directory/my_file
hacker@dojo:/tmp/my_directory$
```
Now, go forth and create a `/tmp/pwn` directory and make a `college` file in it! Then run `/challenge/run`, which will check your solution and give you the flag!

## Solution:

- `cd /tmp` to go to it first.
- `mkdir pwn` to create a directory named pwn in tmp.
- `ls` to verify that pwn was created.
- `cd pwn` to go into /tmp/pwn.
- `touch college` to create the file called college.
- `ls` to verify the file was created.
- `/challenge/run` to check the solution and give the flag.

## Flag: 

```
pwn.college{4tITYb2ZD_fPiQUFKdrDuKKFMG9.QXxMDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- `mkdir` is used to create directories



# Finding Files

So now we know how to list, read, and create files. But how do we find them? We use the `find` command!

The `find` command takes optional arguments describing the search criteria and the search location. If you don't specify a search criteria, `find` matches every file. If you don't specify a search location, `find` uses the current working directory (`.`). For example:

```sh
hacker@dojo:~$ mkdir my_directory
hacker@dojo:~$ mkdir my_directory/my_subdirectory
hacker@dojo:~$ touch my_directory/my_file
hacker@dojo:~$ touch my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find
.
./my_directory
./my_directory/my_subdirectory
./my_directory/my_subdirectory/my_subfile
./my_directory/my_file
hacker@dojo:~$
```
And when specifying the search location:

```sh
hacker@dojo:~$ find my_directory/my_subdirectory
my_directory/my_subdirectory
my_directory/my_subdirectory/my_subfile
hacker@dojo:~$
```
And, of course, we can specify the criteria! For example, here, we filter by name:

```sh
hacker@dojo:~$ find -name my_subfile
./my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find -name my_subdirectory
./my_directory/my_subdirectory
hacker@dojo:~$
```
You can search the whole filesystem if you want!

```sh
hacker@dojo:~$ find / -name hacker
/home/hacker
hacker@dojo:~$
```
Now it's your turn. I've hidden the flag in a random directory on the filesystem. It's still called `flag`. Go find it!

Several notes. First, there are other files named `flag` on the filesystem. Don't panic if the first one you try doesn't have the actual flag in it. Second, there're plenty of places in the filesystem that are not accessible to a normal user. These will cause `find` to generate errors, but you can ignore those; we won't hide the flag there! Finally, `find` can take a while; be patient!

## Solution:

- `find / -name flag` to search for flag in home directory but all the directories' permission is denied.
- `find /tmp -name flag` and `find /challenge -name flag` also give permission denied. 
- `find /usr -name flag` gives 2 directories. (tmp,challenge and usr directories are checked as they were mentioned previously too)
- `cat /usr/local/lib/python3.8/dist-packages/pwnlib/flag` doesnt give the flag.
- `cat /usr/share/doc/libdrm-amdgpu1/flag` gives the flag.

## Flag: 

```
pwn.college{wxAA1AARM8VobUVnfT-LBNEx7QZ.QXyMDO0wCO0AzNzEzW}
```

### References:

- none

### Notes:

- Use `find` command to find the file.
- The find command takes optional arguments describing the search criteria and the search location.
- If a search criteria, find matches every file.
- If you don't specify a search location, find uses the current working directory (.)


















