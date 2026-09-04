# Comprehending commands

## Challenge 1: cat: not the pet but the command!

### Problem Statement:
```
One of the most critical Linux commands is cat. cat is most often used for reading out files, like so:

hacker@dojo:~$ cat /path/to/file
Hello Hackers!
cat will concatenate (hence the name) multiple files if provided multiple arguments. For example:
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

Finally, if you give no arguments at all, cat will read from the terminal input and output it. We'll explore that in later challenges...

In this challenge, I will copy the flag to the flag file in your home directory (where your shell starts). Go read it with cat!
```

### Key points:
- cat reads buffers, not just file text, this is proven by what they meant by the functionality of cat when no arguments are given
- cat works with absolute, relative, implicit and explicit paths
- cat works with multiple files
- essential command for working with files and looking through files


```bash
shaurya_pratap@cheeese:~$ ssh -i key hacker@dojo.pwn.college
Connected!
hacker@commands~cat-not-the-pet-but-the-command:~$ cat flag
pwn.college{A4EDen4wGDRL7xHCi3QAIHdsCRX.QXxcTN0wSO2EzNwIzW}
```



## Challenge 2: catting absolute paths

### Problem Statement:
```
In the last level, you did cat flag to read the flag out of your home directory! You can, of course, specify cat's arguments as absolute paths:

hacker@dojo:~$ cat /path/to/file
Hello Hackers!
In this challenge, I will not copy it to your home directory, but I will make it readable. You can read it with cat at its absolute path: /flag.

FUN FACT: /flag is where the flag always lives in pwn.college, but unlike in this challenge, you typically can't access that file directly.
```

### Key points:
- so cat works with absolute paths
- so all i need to do is do an absolute path starting from / to get the flag in this instance


```bash
Connected!
hacker@commands~catting-absolute-paths:~$ cat /flag
pwn.college{gLtmmeFw2wlco_IgZ9TV2YwCDB2.QX5ETO0wSO2EzNwIzW}
```

## Challenge 3: more catting practice

### Problem Statement:
```
You can specify all sorts of paths as arguments to commands, and we'll practice some more with cat. In this level, I'll put the flag in some crazy directory, and I will not allow you to change directories with cd, so no cat flag for you. You must retrieve the flag by absolute path, wherever it is.
```

### Key points:
- no cd allowed, they're trying to make me use absolute paths for practice
- i expect changing to different absolute paths before i find the final path
- nevermind

```bash
shaurya_pratap@cheeese:~$ ssh -i key hacker@dojo.pwn.college
Connected!
hacker@commands~catting-absolute-paths:~$
Connected!
You cannot use the 'cd' command in this level, and must retrieve the flag by
absolute path. Plus, I hid the flag in a different directory! You can find it
in the file /usr/local/flag. Go cat it out without using cd!
hacker@commands~more-catting-practice:~$ cat /flag
cat: /flag: No such file or directory
hacker@commands~more-catting-practice:~$ cat /user/local/flag
cat: /user/local/flag: No such file or directory
hacker@commands~more-catting-practice:~$ cat /usr/local/flag
pwn.college{YSyNN7-qVXjPn7UZ4yy94ID7XB3.QXwITO0wSO2EzNwIzW}
```

## Challenge 4: grepping for a needle in a haystack

### Problem Statement:
```
Sometimes, the files that you might cat out are too big. Luckily, we have the grep command to search for the contents we need! We'll learn it in this challenge.

There are many ways to grep, and we'll learn one way here:

hacker@dojo:~$ grep SEARCH_STRING /path/to/file
Invoked like this, grep will search the file for lines of text containing SEARCH_STRING and print them to the console.

In this challenge, I've put a hundred thousand lines of text into the /challenge/data.txt file. grep it for the flag!

HINT: The flag always starts with the text pwn.college.
```
### Key points:
- grep is generally used to find strings in big files that are unreadable, this allows us to skim files for more information
- grep is ALSO used for buffers that are given from other commands, for example getting grep of a proc name from a proc list containing names of all the processes
- in this case out of curiosity i tried to cat /challenge/data.txt, it was filled with 1000 lines of random words
- so i used grep after that

```bash
clothespins
repatriation
interject
departmental
ratio's
tamed
aviatrix's
matriculated
platformed
hacker@commands~grepping-for-a-needle-in-a-haystack:~$ grep pwn /challenge/data.txt
pwns
pwn.college{YMnpklVsDFOKqTAZELRgLkq2Jz2.QX3EDO0wSO2EzNwIzW}
```


## Challenge 5: comparing files

### Problem Statement:
```
When looking for changes between similar files, eyeballing them might not be the most efficient approach! This is where the diff command becomes invaluable.

diff compares two files line by line and shows you exactly what's different between them. For example:

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
The output tells us that line 2 changed (2c2), with world in the first file (<) being replaced by universe in the second file (>).

Sometimes, when new lines are added, you'll see something like:

hacker@dojo:~$ cat old
pwn
hacker@dojo:~$ cat new
pwn
college
hacker@dojo:~$ diff old new
1a2
> college
This tells us that after line 1 in the first file, the second file has an additional line (1a2 means "after line 1 of file1, add line 2 of file2").

Now for your challenge! There are two files in /challenge:

/challenge/decoys_only.txt contains 100 fake flags
/challenge/decoys_and_real.txt contains all 100 fake flags plus the one real flag
Use diff to find what's different between these files and get your flag!
```
### Key points:
- diff is to compare 2 files, this is actually similar to a command called `git diff`
- i assume git diff borrowed it from the diff command
- `>` this means additional line
- very simplistic and cant go wrong with it
- in this particular challenge i've been given decoys `/challenge/decoys_only.txt` and real with decoys`/challenge/decoys_and_real.txt`
- so the idea will be to instead of checking them with `cat`, using `diff` to be efficient with our time

```bash
hacker@commands~comparing-files:~$ diff /challenge/decoys_only.txt /challenge/decoys_and_real.txt
11a12
> pwn.college{EjaDqIH-NIuvy1S1V6fIANkT6ex.01MwMDOxwSO2EzNwIzW}
```

## Challenge 6: listing files


### Problem Statement:

```
So far, we've told you which files to interact with. But directories can have lots of files (and other directories) inside them, and we won't always be here to tell you their names. You'll need to learn to list their contents using the ls command!

ls will list files in all the directories provided to it as arguments, and in the current directory if no arguments are provided. Observe:

hacker@dojo:~$ ls /challenge
run
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ ls /home/hacker
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$
In this challenge, we've named /challenge/run with some random name! List the files in /challenge to find it. Then invoke the discovered absolute path to get the flag.
```
### Key points:
- ls is used to either find files in the cwd
- or you can do  `ls absolute/path` to list files in that path (directory)
- much useful for guiding yourself through the entire filesystem instead of being in the dark

```bash
hacker@commands~listing-files:~$ ls /challenge
26270-renamed-run-25225  Dockerfile
hacker@commands~listing-files:~$ /challenge/26270-renamed-run-25225
Yahaha, you found me! Here is your flag:
pwn.college{AHvDIUG1b8m1tEnZdkcj-ZzmaB1.QX4IDO0wSO2EzNwIzW}
```

## Challenge 7: touching files (ayo)

## Problem Statement
```
Of course, you can also create files! There are several ways to do this, but we'll look at a simple command here. You can create a new, blank file by touching it with the touch command:

hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ touch pwnfile
hacker@dojo:/tmp$ ls
pwnfile
hacker@dojo:/tmp$
It's that simple! In this level, please create two files: /tmp/pwn and /tmp/college, and run /challenge/run to get your flag!```
```
### Key points:
- touch files to create files on the spot without doing cat > file
- this is pretty good way to create files that you don't want to write into just yet
- challenge tells us to create /tmp/pwn, /tmp/college
- run /challenge/run to get flag since it'll check our created files

```bash
hacker@commands~listing-files:~$
Connected!
hacker@commands~touching-files:~$ touch /tmp/pwn /tmp/college
hacker@commands~touching-files:~$ /challenge/run
Success! Here is your flag:
pwn.college{YPM79qcKNAhYpnpF6ZACHGGzSp9.QXwMDO0wSO2EzNwIzW}
```

## Challenge 8: removing files

## Problem Statement
```
Files are all around you. Like candy wrappers, there'll eventually be too many of them. In this level, we'll learn to clean up!

In Linux, you remove files with the rm command, as so:

hacker@dojo:~$ touch PWN
hacker@dojo:~$ touch COLLEGE
hacker@dojo:~$ ls
COLLEGE     PWN
hacker@dojo:~$ rm PWN
hacker@dojo:~$ ls
COLLEGE
hacker@dojo:~$
Let's practice. This challenge will create a delete_me file in your home directory! Delete it, then run /challenge/check, which will make sure you've deleted it and then give you the flag!

```
### Key points:
- deleting files
- not much explanation needed, this one tells us to delete the `delete_me` file

```bash
hacker@commands~removing-files:~$ rm delete_me
hacker@commands~removing-files:~$ /challenge/run
bash: /challenge/run: No such file or directory
hacker@commands~removing-files:~$ /challenge/check
Excellent removal. Here is your reward:
pwn.college{EC7J4_KqhWxxObh1u8WbGwfCxxm.QX2kDM1wSO2EzNwIzW}
```


## Challenge 9: moving files

## Problem Statement
```
You can also move files around with the mv command. The usage is simple:

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
This challenge wants you to move the /flag file into /tmp/hack-the-planet (do it)! Note, you must use the mv command rather than other methods (such as renaming the file in VSCode). When you're done, run /challenge/check, which will check things out and give the flag to you.
```
### Key points:
- mv `src` `dest` - this will be the standard syntax for our command, file in the source destination gets moved to destination path directory
- this one asks us to move the /flag file to /tmp/hack-the-planet 
- then run /challenge/check to let it check

```bash
hacker@commands~moving-files:~$ mv /flag /tmp/hack-the-planet
Correct! Performing 'mv /flag /tmp/hack-the-planet'.
hacker@commands~moving-files:~$ /challenge/run
bash: /challenge/run: No such file or directory
hacker@commands~moving-files:~$ /challenge/check
Congrats! You successfully moved the flag to /tmp/hack-the-planet! Here it is:
pwn.college{0N44S1h6y1nhPWPhk1yv-zMnEDq.0VOxEzNxwSO2EzNwIzW}
```


## Challenge 10: copying files

## Problem Statement
```
But what if you want to keep the original file? You can do so with the cp command. The usage is the same as with mv, but it will keep the source file. The command is cp SOURCE DESTINATION: the first argument is the existing file, and the second is where the copy should be created.

This challenge wants you to copy the /flag file to /tmp/hack-the-planet (do it)! When you're done, run /challenge/check, which will check things out and give the flag to you.

NOTE: When a cp destination is a directory, cp places the copy inside it using the source file's name. For this challenge, /tmp/hack-the-planet should name the copied file itself, rather than an existing directory.
```
### Key points:
- cp `src` `dest` - this will be the standard syntax for our command, file in the source destination gets copied to destination path directory
- you can't copy directories, you need to do it recursively by copying all the files from that dir you need a subscript for that 
- they want us to copy /flag to /tmp/hack-the-plant, then run /challenge/check

```bash
hacker@commands~copying-files:~$ cp /flag /tmp/hack-the-world
ERROR: make sure your destination is /tmp/hack-the-planet!
hacker@commands~copying-files:~$ cp /flag /tmp/hack-the-planet
Correct! Performing 'cp /flag /tmp/hack-the-planet'.
hacker@commands~copying-files:~$ /challenge/check
Congrats! You successfully copied the flag to /tmp/hack-the-planet! Here it is:
pwn.college{AEIaPnge-5Y4-gw58jdXaRDDW0r.0lNxQTMywSO2EzNwIzW}
```

## Challenge 11: hidden files

## Problem Statement
```
Interestingly, ls doesn't list all the files by default. Linux has a convention where files that start with a . don't show up by default in ls and in a few other contexts. To view them with ls, you need to invoke ls with the -a flag, as so:

hacker@dojo:~$ touch pwn
hacker@dojo:~$ touch .college
hacker@dojo:~$ ls
pwn
hacker@dojo:~$ ls -a
.college	pwn
hacker@dojo:~$
Now, it's your turn! Go find the flag, hidden as a dot-prepended file in /.

```
### Key points:
- ls -a gets you hidden files, which normally start with a .
- this happens even in ntfs formats, where windows hides hidden files 
- in this one lets get the hidden files using ls -a

```bash
hacker@commands~copying-files:~$ ls -a
Connected!
hacker@commands~hidden-files:~$ ls -a
.  ..  .bash_history  .config  a
hacker@commands~hidden-files:~$ ./.
bash: ./.: Is a directory
hacker@commands~hidden-files:~$ ls / -a
.   .dockerenv             bin   challenge  etc   lib    media  nix  proc  run   srv  tmp  var
..  .flag-172302396311407  boot  dev        home  lib64  mnt    opt  root  sbin  sys  usr
hacker@commands~hidden-files:~$ cat /.flag-172302396311407
pwn.college{IKQ4sPP3bSJx41mGlC5yDY6OVQ_.QXwUDO0wSO2EzNwIzW}
```


## Challenge 12: An Epic Filesystem Quest

## Problem Statement
```
With your knowledge of cd, ls, and cat, we're ready to play a little game!

We'll start it out in /. Normally:

hacker@dojo:~$ cd /
hacker@dojo:/$ ls
bin   challenge  etc   home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  dev        flag  lib   lib64  media   opt  root  sbin  sys  usr
That's a lot of contents! One day, you will be quite familiar with them, but already, you might recognize the flag file and the challenge directory.

In this challenge, I have hidden the flag! Here, you will use ls and cat to follow my breadcrumbs and find it! Here's how it'll work:

Your first clue is in /. Head on over there.
Look around with ls. There'll be a file named HINT or CLUE or something along those lines!
cat that file to read the clue!
Depending on what the clue says, head on over to the next directory (or don't!).
Follow the clues to the flag!
Good luck!
```
### Key points:
- our first real challenge is here, so first i went to /
- from that i found HINT which at first i couldnt figure out whether it was a normal file or directory, they should really rename stuff with .txt extension
- HINT led to cron.weekly dir to find CLUE file
- CLUE let to `/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__`
- fun fact, ive seen `__pycache__` folder many times while i worked on python projects, so it was pretty nice to see it again
- usually pertains to compiled c version of python code, its why its called cpython
- anyways our next clue said the next folder's files are hidden so this tells us i should apply ls -a
- next folder was this /usr/lib/python3/dist-packages/pkg_resources/_vendor/
- we found our next clue which is .DISPATCH, i almost got confused thinking that `.` was the file name itself (which isnt possible)
- /usr/share/man/sr next folder, file was .TIP
- in this stage i kept doing ls -a regardless, it doesnt hurt to see hidden files anyway, i mean unless there are too many hidden files in which case ls would benefit you, theres no point not doing ls -a
- /usr/share/doc/hostname - its the place where the linux host keeps it's hostname maybe
- SECRET is the file
- /usr/lib/x86_64-linux-gnu/perl-base/auto/List - we have some GNU references here, nice.
- file was LEAD
- /usr/share/libc-bin - this is where c keeps it's libraries
- so it says i cant cd to that place since it'll self destruct - whatever that means, maybe terminate my session
- so i decided to `ls /usr/share/libc-bin` instead and get the files i needed from there
- SNIPPET-TRAPPED this was the file
- /usr/share/locale/gl this was the next folder
- CUE was the file
- /var/cache/man/pt/cat5
- BRIEF 
- BRIEF is the last one, after which it gives you the flag
- overall nice challenge, gets repetitive slightly but it was going through all the known "developer" paths within the linux filesystem, which was a nice thing to see

```bash
ls / -a
.   .dockerenv  bin   challenge  etc   home  lib64  mnt  opt   root  sbin  sys  usr
..  HINT        boot  dev        flag  lib   media  nix  proc  run   srv   tmp  var
hacker@commands~an-epic-filesystem-quest:~$ /HINT
bash: /HINT: Permission denied
hacker@commands~an-epic-filesystem-quest:~$ cd /
hacker@commands~an-epic-filesystem-quest:/$ HINT
bash: HINT: command not found
hacker@commands~an-epic-filesystem-quest:/$ ./HINT
bash: ./HINT: Permission denied
hacker@commands~an-epic-filesystem-quest:/$ cat HINT
Yahaha, you found me!
The next clue is in: /etc/cron.weekly

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/$ cd /etc/cron.weekly
hacker@commands~an-epic-filesystem-quest:/etc/cron.weekly$ ls -a
.  ..  CLUE  man-db
hacker@commands~an-epic-filesystem-quest:/etc/cron.weekly$ cat CLUE
Tubular find!
The next clue is in: /usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/etc/cron.weekly$ ls /usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__
__init__.cpython-312.pyc   _compat.cpython-312.pyc     abc.cpython-312.pyc
_adapters.cpython-312.pyc  _itertools.cpython-312.pyc  readers.cpython-312.pyc
_common.cpython-312.pyc    _legacy.cpython-312.pyc     simple.cpython-312.pyc
hacker@commands~an-epic-filesystem-quest:/etc/cron.weekly$ cd /usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ ls -a
.          __init__.cpython-312.pyc   _compat.cpython-312.pyc     abc.cpython-312.pyc
..         _adapters.cpython-312.pyc  _itertools.cpython-312.pyc  readers.cpython-312.pyc
.DISPATCH  _common.cpython-312.pyc    _legacy.cpython-312.pyc     simple.cpython-312.pyc
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cat .
cat: .: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cat ..
cat: ..: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cat ./.
cat: ./.: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ ./.
bash: ./.: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cat "."
cat: .: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cd ./.
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ ls
__init__.cpython-312.pyc   _compat.cpython-312.pyc     abc.cpython-312.pyc
_adapters.cpython-312.pyc  _itertools.cpython-312.pyc  readers.cpython-312.pyc
_common.cpython-312.pyc    _legacy.cpython-312.pyc     simple.cpython-312.pyc
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ ls -a
.          __init__.cpython-312.pyc   _compat.cpython-312.pyc     abc.cpython-312.pyc
..         _adapters.cpython-312.pyc  _itertools.cpython-312.pyc  readers.cpython-312.pyc
.DISPATCH  _common.cpython-312.pyc    _legacy.cpython-312.pyc     simple.cpython-312.pyc
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cd "."
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cat .DISPATCH
Congratulations, you found the clue!
The next clue is in: /usr/share/man/sr

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/pkg_resources/_vendor/importlib_resources/__pycache__$ cd /usr/share/man/sr
hacker@commands~an-epic-filesystem-quest:/usr/share/man/sr$ ls -a
.  ..  .TIP  man1  man5  man8
hacker@commands~an-epic-filesystem-quest:/usr/share/man/sr$ cat .TIP
Congratulations, you found the clue!
The next clue is in: /usr/share/doc/hostname
hacker@commands~an-epic-filesystem-quest:/usr/share/man/sr$ cd /usr/share/doc/hostname
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/hostname$ ls -a
.  ..  SECRET  changelog.gz  copyright
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/hostname$ ls SECRET
SECRET
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/hostname$ cat SECRET
Lucky listing!
The next clue is in: /usr/lib/x86_64-linux-gnu/perl-base/auto/List

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/hostname$ cd /usr/lib/x86_64-linux-gnu/perl-base/auto/List
hacker@commands~an-epic-filesystem-quest:/usr/lib/x86_64-linux-gnu/perl-base/auto/List$ ls -a
.  ..  LEAD  Util
hacker@commands~an-epic-filesystem-quest:/usr/lib/x86_64-linux-gnu/perl-base/auto/List$ cat LEAD
Tubular find!
The next clue is in: /usr/share/libc-bin

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/usr/lib/x86_64-linux-gnu/perl-base/auto/List$ cat /usr/share/libc-bin
cat: /usr/share/libc-bin: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/lib/x86_64-linux-gnu/perl-base/auto/List$ ls /usr/share/libc-bin
SNIPPET-TRAPPED  nsswitch.conf
hacker@commands~an-epic-filesystem-quest:/usr/lib/x86_64-linux-gnu/perl-base/auto/List$ cat /usr/share/libc-bin/SNIPPET-TRAPPED
Tubular find!
The next clue is in: /usr/share/locale/gl
hacker@commands~an-epic-filesystem-quest:/usr/lib/x86_64-linux-gnu/perl-base/auto/List$ cd /usr/share/locale/gl
hacker@commands~an-epic-filesystem-quest:/usr/share/locale/gl$ ls -a
.  ..  CUE  LC_MESSAGES  LC_TIME
hacker@commands~an-epic-filesystem-quest:/usr/share/locale/gl$ cat CUE
Yahaha, you found me!
The next clue is in: /var/cache/man/pt/cat5
hacker@commands~an-epic-filesystem-quest:/usr/share/locale/gl$ cd /var/cache/man/pt/cat5
hacker@commands~an-epic-filesystem-quest:/var/cache/man/pt/cat5$ ls -a
.  ..  BRIEF
hacker@commands~an-epic-filesystem-quest:/var/cache/man/pt/cat5$ cat BRIEF
CONGRATULATIONS! Your perserverence has paid off, and you have found the flag!
It is: pwn.college{4d3kybxM4dKzy4HMdlvwuq8gQtM.QX5IDO0wSO2EzNwIzW}
```


## Challenge 12: making directories

## Problem Statement
```
We can create files. How about directories? You make directories using the mkdir command. Then you can stick files in there!

Watch:

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
Now, go forth and create a /tmp/pwn directory and make a college file in it! Then run /challenge/run, which will check your solution and give you the flag!
```
### Key points:
- `mkdir directory_name` is the syntax to create a new directory

```bash
hacker@commands~making-directories:~$ mkdir /tmp/pwn
mkdir: cannot create directory ‘/tmp/pwn’: File exists
hacker@commands~making-directories:~$ touch /tmp/pwn/college
hacker@commands~making-directories:~$ /challenge/run
Uh oh! /tmp/pwn/college does not exist. Please use the 'touch' command to
create it!
hacker@commands~making-directories:~$ cd /tmp/pwn
hacker@commands~making-directories:/tmp/pwn$ ls -a
.  ..  college
hacker@commands~making-directories:/tmp/pwn$ /challenge/run
Uh oh! /tmp/pwn/college does not exist. Please use the 'touch' command to
create it!
```

- i think their emulator just bugged the fk out

```bash
hacker@commands~making-directories:~$ mkdir /tmp/pwn
mkdir: cannot create directory ‘/tmp/pwn’: File exists
hacker@commands~making-directories:~$ rm /tmp/pwn
rm: cannot remove '/tmp/pwn': Is a directory
hacker@commands~making-directories:~$ rmdir /tmp/pwn
rmdir: failed to remove '/tmp/pwn': Directory not empty
hacker@commands~making-directories:~$ rm /tmp/pwn/college
rm: cannot remove '/tmp/pwn/college': Is a directory
hacker@commands~making-directories:~$ rmdir /tmp/pwn/college
hacker@commands~making-directories:~$ touch /tmp/pwn/college
hacker@commands~making-directories:~$ /challenge/run
Success! Here is your flag:
pwn.college{s-rPkmPZtdSQ_ZYXBe9ikzivwfX.QXxMDO0wSO2EzNwIzW}
```
- nevermind i had just created the directory instead of the file



## Challenge 13: finding files

## Problem Statement
```
So now we know how to list, read, and create files. But how do we find them? We use the find command!

The find command takes optional arguments describing the search criteria and the search location. If you don't specify a search criteria, find matches every file. If you don't specify a search location, find uses the current working directory (.). For example:

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
And when specifying the search location:

hacker@dojo:~$ find my_directory/my_subdirectory
my_directory/my_subdirectory
my_directory/my_subdirectory/my_subfile
hacker@dojo:~$
And, of course, we can specify the criteria! For example, here, we filter by name:

hacker@dojo:~$ find -name my_subfile
./my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find -name my_subdirectory
./my_directory/my_subdirectory
hacker@dojo:~$
You can search the whole filesystem if you want!

hacker@dojo:~$ find / -name hacker
/home/hacker
hacker@dojo:~$
Now it's your turn. I've hidden the flag in a random directory on the filesystem. It's still called flag. Go find it!

Several notes. First, there are other files named flag on the filesystem. Don't panic if the first one you try doesn't have the actual flag in it. Second, there're plenty of places in the filesystem that are not accessible to a normal user. These will cause find to generate errors, but you can ignore those; we won't hide the flag there! Finally, find can take a while; be patient!
```
### Key points:
- `find <path>` is the syntax to find a file recursively
- theres multiple flags on the system
- need to try all of them
- sometimes throws errors because no permissions - thats a good thing, attackers can exploit find otherwise

```bash
hacker@commands~finding-files:~$ find / -name flag
find: ‘/etc/ssl/private’: Permission denied
/usr/lib/python3/dist-packages/pwnlib/flag
/usr/lib/python3/dist-packages/pwnlib/data/elf/ret2dlresolve/flag
find: ‘/var/cache/apt/archives/partial’: Permission denied
find: ‘/var/cache/ldconfig’: Permission denied
find: ‘/var/lib/apt/lists/partial’: Permission denied
find: ‘/root’: Permission denied

^C
hacker@commands~finding-files:~$ cat /usr/lib/python3/dist-packages/pwnlib/flag /usr/lib/python3/dist-packages/pwnlib/data/elf/ret2dlresolve/flag
cat: /usr/lib/python3/dist-packages/pwnlib/flag: Is a directory
pwn.college{MDh6u_CPcDIL_XD8XRFWcW0Y1JY.QXyMDO0wSO2EzNwIzW}
```


## Challenge 14: linking files

## Problem Statement
```
Links come in two flavors: hard and soft (also known as symbolic) links. We'll differentiate the two with an analogy:

A hard link is when you address your apartment using multiple addresses that all lead directly to the same place (e.g., Apt 2 vs Unit 2).
A soft link is when you move apartments and have the postal service automatically forward your mail from your old place to your new place.
In a filesystem, a file is, conceptually, an address at which the contents of that file live. A hard link is an alternate address that indexes that data --- accesses to the hard link and accesses to the original file are completely identical, in that they immediately yield the necessary data. A soft/symbolic link, instead, contains the original file name. When you access the symbolic link, Linux will realize that it is a symbolic link, read the original file name, and then (typically) automatically access that file. In most cases, both situations result in accessing the original data, but the mechanisms are different.

Hard links sound simpler to most people (case in point, I explained it in one sentence above, versus two for soft links), but they have various downsides and implementation gotchas that make soft/symbolic links, by far, the more popular alternative.

In this challenge, we will learn about symbolic links (also known as symlinks). Symbolic links are created with the ln command using the syntax ln -s TARGET LINK_NAME. TARGET is the path that the link will point to, and LINK_NAME is the new path you are creating. For example:

hacker@dojo:~$ cat /tmp/myfile
This is my file!
hacker@dojo:~$ ln -s /tmp/myfile /home/hacker/ourfile
hacker@dojo:~$ cat ~/ourfile
This is my file!
hacker@dojo:~$
You can see that accessing the symlink results in getting the original file contents! In this example, /tmp/myfile is the target and /home/hacker/ourfile is the link name.

A symlink can be identified as such with a few methods. For example, the file command, which takes a filename and tells you what type of file it is, will recognize symlinks:

hacker@dojo:~$ file /tmp/myfile
/tmp/myfile: ASCII text
hacker@dojo:~$ file ~/ourfile
/home/hacker/ourfile: symbolic link to /tmp/myfile
hacker@dojo:~$
Okay, now you try it! In this level the flag is, as always, in /flag, but /challenge/catflag will instead read out /home/hacker/not-the-flag. The path /home/hacker/not-the-flag is the link name in this challenge. Choose the target that will fool /challenge/catflag into giving you the flag!

```
### Key points:
- a hard link is basically indexed via the global file descriptor of your machine, it has a different mechanism but the memory that address points to is the memory of the original file
- a soft link is allowed to have its own file name sort of like a `pointer` in c, but at the end of the day it points to the original file
- use `ln -s file link_name` for creating a link to the file (symbolic)
- this is actually a really good trick in ctfs, letting the host script point towards a symlink allows you to run something that wasn't supposed to be run or something that can't be reached

```bash
hacker@commands~linking-files:~$ ln -s /flag /home/hacker/not-the-flag
hacker@commands~linking-files:~$ /challenge/catflag
About to read out the /home/hacker/not-the-flag file!
pwn.college{Ikfd-QOKaKOfnFFfFTzMS4hQcrj.QX5ETN1wSO2EzNwIzW}
```
