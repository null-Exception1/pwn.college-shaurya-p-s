# File Globbing

## Challenge 1: Matching with *

### Problem Statement:
```
The first glob we'll learn is *. When it encounters a * character in any argument, the shell will treat it as a "wildcard" and try to replace that argument with any files that match the pattern. It's easier to show you than explain:

hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_*
Look: file_a file_b file_c
Of course, though in this case, the glob resulted in multiple arguments, it can just as simply match only one. For example:

hacker@dojo:~$ touch file_a
hacker@dojo:~$ ls
file_a
hacker@dojo:~$ echo Look: file_*
Look: file_a
When zero files are matched, by default, the shell leaves the glob unchanged:

hacker@dojo:~$ touch file_a
hacker@dojo:~$ ls
file_a
hacker@dojo:~$ echo Look: nope_*
Look: nope_*
The * matches any part of the filename except for / or a leading . character. For example:

hacker@dojo:~$ echo ONE: /ho*/*ck*
ONE: /home/hacker
hacker@dojo:~$ echo TWO: /*/hacker
TWO: /home/hacker
hacker@dojo:~$ echo THREE: ../*
THREE: ../hacker
Now, practice this yourself! Starting from your home directory, change your directory to /challenge, but use globbing to keep the argument you pass to cd to at most four characters! Once you're there, run /challenge/run for the flag!
```

### Key points:
- `*` matches zero or more characters in a filename
- the shell expands wildcards before executing the target binary
- we need to be in `/challenge` to run it properly without breaking the criteria

```bash
shaurya_pratap@cheeese:~$ ssh -i key hacker@dojo.pwn.college
Connected!
hacker@globbing~matching-with-:~$ cd /ch*
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
pwn.college{8700NSRQFWsxSgUa1SWrOai45Tz.QXxIDO0wSO2EzNwIzW}
```

## Challenge 2: Matching with ?

### Problem Statement:
```
Next, let's learn about ?. When it encounters a ? character in any argument, the shell will treat it as a single-character wildcard. This works like *, but only matches one character. For example:

hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_cc
hacker@dojo:~$ ls
file_a	file_b	file_cc
hacker@dojo:~$ echo Look: file_?
Look: file_a file_b
hacker@dojo:~$ echo Look: file_??
Look: file_cc
Now, practice this yourself! Starting from your home directory, change your directory to /challenge, but use the ? character instead of c and l in the argument to cd! Once you're there, run /challenge/run for the flag!
```

### Key points:
- `?` acts as a wildcard for exactly one single character
- useful when you know the character length but not the specific characters
- can combine multiple `?` characters to match specific lengths

```bash
hacker@globbing~matching-with-:~$ cd /?ha??enge
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
pwn.college{kqUq5hWLSn7kxFFCuRYLm8b8FJM.QXyIDO0wSO2EzNwIzW}
```

## Challenge 3: Matching with Brackets []

### Problem Statement:
```
Next, we will cover []. The square brackets are, essentially, a limited form of ?, in that instead of matching any character, [] is a wildcard for some subset of potential characters, specified within the brackets. For example, [pwn] will match the character p, w, or n. For example:

hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: file_[ab]
Look: file_a file_b
Try it here! We've placed a bunch of files in /challenge/files. Change your working directory to /challenge/files and run /challenge/run with a single argument that bracket-globs into file_b, file_a, file_s, and file_h!
```

### Key points:
- `[]` defines a character class or a custom list of allowed characters at that position
- ranges like `[a-d]` simplify multiple alternatives
- case-sensitivity applies within character classes unless configured otherwise

```bash
hacker@globbing~matching-with-:~$ cd /challenge/files
hacker@globbing~matching-with-:/challenge/files$ /challenge/run file_[absh]
You got it! Here is your flag!
pwn.college{AdLSAItw7J5e7teFmVuw6tKuG3G.QXzIDO0wSO2EzNwIzW}
```

## Challenge 4: Matching paths with []

### Problem Statement:
```
Globbing happens on a path basis, so you can expand entire paths with your globbed arguments. For example:

hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a	file_b	file_c
hacker@dojo:~$ echo Look: /home/hacker/file_[ab]
Look: /home/hacker/file_a /home/hacker/file_b
Now it's your turn. Once more, we've placed a bunch of files in /challenge/files. Starting from your home directory, run /challenge/run with a single argument that bracket-globs into the absolute paths to the file_b, file_a, file_s, and file_h files!
```

### Key points:
- paths expand directly using pattern fragments across specific directories
- can perform lookups across the filesystem absolute routes straight from home terminal context
- binary expects configuration context relative to standard working setups
- did [absh] as the wildcard here

```bash
hacker@globbing~matching-paths-with-:~$ cd /challenge/files
hacker@globbing~matching-paths-with-:/challenge/files$ /challenge/run /challenge/files/file_[absh]
Error: please run with a working directory of /home/hacker!
hacker@globbing~matching-paths-with-:/challenge/files$ cd /home/hacker
hacker@globbing~matching-paths-with-:~$ /challenge/run /challenge/files/file_[absh]
You got it! Here is your flag!
pwn.college{IAQUgsTaS4pPUk9p8pwlooP_nU-.QX0IDO0wSO2EzNwIzW}
```

## Challenge 5: Multiple globs

### Problem Statement:
```
So far, you've specified one glob at a time, but you can do more! Bash supports the expansion of multiple globs in a single word. For example:

hacker@dojo:~$ cat /*fl*
pwn.college{YEAH}
hacker@dojo:~$
What happens above is that the shell looks for all files in / that start with anything (including nothing), then have an f and an l, and end in anything (including ag, which makes flag).

Now you try it. We put a few happy, but diversely-named files in /challenge/files. Go cd there and run /challenge/run, providing a single argument: a short (3 characters or less) globbed word with two * globs in it that covers every word that contains the letter p.
```

### Key points:
- multiple wildcards can be placed inside a single argument string
- allows ultra-short expressions to encompass a wide selection of matching targets
- requires careful parsing to avoid matching unwanted file items
- i kept the 2 globs

```bash
hacker@globbing~multiple-globs:~$ cd /challenge/files
hacker@globbing~multiple-globs:/challenge/files$ /challenge/run *p*
You got it! Here is your flag!
pwn.college{c5_EzU8mxwt4bwBJUA7NAFL1k7U.0lM3kjNxwSO2EzNwIzW}
```

## Challenge 6: Mixing globs

### Problem Statement:
```
Now, let's put the previous levels together! We put a few happy, but diversely-named files in /challenge/files. Go cd there and, using the globbing you've learned, write a single, short (6 characters or less) glob that (when passed as an argument to /challenge/run) will match only the files "challenging", "educational", and "pwning"!

HINT: Make sure to look at the names of the files in /challenge/files. Do you see any patterns that could help you make your glob?
```

### Key points:
- mixing different globbing styles like character sets `[]` and wildcards `*` creates incredibly fine filters
- the constraint forces our final argument string to be strictly 6 characters or less
- by evaluating the prefixes (`challenging`, `educational`, `pwning`), they map directly to `c`, `e`, and `p` to isolate target objects away from decoys cleanly
- the common thing between all the files is they start with a wildcard which is always [cep] then put * glob
```bash
hacker@globbing~mixing-globs:~$ cd /challenge/files
hacker@globbing~mixing-globs:/challenge/files$ /challenge/run [cep]*
You got it! Here is your flag!
pwn.college{AIMRrT9wx2GF6fAjhMwg4eu7nu5.QX1IDO0wSO2EzNwIzW}
```

## Challenge 7: Exclusionary globbing

### Problem Statement:
```
Sometimes, you want to filter out files in a glob! Luckily, [] helps you do just this. If the first character in the brackets is a ! or (in newer versions of bash) a ^ , the glob inverts, and that bracket instance matches characters that aren't listed. For example:

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

Armed with this knowledge, go forth to /challenge/files and run /challenge/run with all files that don't start with p , w , or n !

NOTE: The ! character has a different special meaning in bash when it's not the first character of a [] glob, so keep that in mind if things stop making sense! ^ does not have this problem, but is also not compatible with older shells.
```

### Key points:
- adding a `!` or `^` immediately inside the opening bracket completely flips the matching selection rule
- matches any character at that explicit index positions except the ones explicitly listed

```bash
hacker@globbing~exclusionary-globbing:~$ cd /challenge/files
hacker@globbing~exclusionary-globbing:/challenge/files$ /challenge/run [!pwn]*
You got it! Here is your flag!
pwn.college{YdZJRCgVuhAFJUGpRQbSM2N3bQ2.QX2IDO0wSO2EzNwIzW}
```

## Challenge 8: Tab completion

### Problem Statement:
```
As tempting as it might be, using * to shorten what must be typed on the commandline can lead to mistakes. Your glob might expand to unintended files, and you might not spot it until the rm command is already running! No one is safe from this style of error.

A safer alternative when you are trying to specify a specific target is tab completion. If you hit tab in the shell, it'll try to figure out what you're going to type and automatically complete it. Auto-completion is super useful, and this challenge will explore its use in specifying files.

This challenge has copied the flag into /challenge/pwncollege , and you can freely cat that file. But you can't type the filename: we used some serious trickery to make sure that you must tab-complete it. Try it out!

hacker@dojo:~$ ls /challenge
Dockerfile  pwncollege
hacker@dojo:~$ cat /challenge/pwncollege
cat: /challenge/pwncollege: No such file or directory
hacker@dojo:~$ cat /challenge/pwn<TAB>
pwn.college{HECK YEAH}
hacker@dojo:~$

When you hit that tab key, the name will expand and you'll be able to read the file. Good luck!
```

### Key points:
- tab completion maps command parameters cleanly without accidental wildcard collisions
- trickery involves custom low-level filesystem or hook setups that block raw keyboard string submissions
- pressing `<TAB>` automatically resolves the true matching descriptor path to feed the data directly to cat

```bash
hacker@globbing~tab-completion:~$ cat /challenge/pwn
# pressed TAB here
hacker@globbing~tab-completion:~$ cat /challenge/pwncollege
You found the tab completion shortcut! Here is your flag:
pwn.college{YwyLV2fTfpdRJdyW6P-jcCEiN9U.0FN0EzNxwSO2EzNwIzW}
```

## Challenge 9: Multiple options for tab completion

### Problem Statement:
```
Consider the following situation:

hacker@dojo:~$ ls
flag  flamingo  flowers
hacker@dojo:~$ cat f<TAB>
There are multiple options! What happens?

What happens varies based on the specific shell and its options. By default bash will auto-expand until the first point when there are multiple options (in this case, fl ). When you hit tab a second time, it'll print out those options. Other shells and configurations, instead, will cycle through the options.

This challenge has a /challenge/files directory with a bunch of files starting with pwncollege . Tab-complete from /challenge/files/p or so, and make your way to the flag!
```

### Key points:
- completion halts at the exact index point where the structural names begin to branch apart
- striking `<TAB>` twice breaks the silence and reveals the complete alternative inventory
- systematically maps exact file matches across congested directories
- then i tabbed the shit out of all the files in the directory
- pwncollege-flag was the answer

```bash
hacker@globbing~multiple-options-for-tab-completion:~$ ls /challenge/files
No ls for you in this level! Use tab-completion instead!
hacker@globbing~multiple-options-for-tab-completion:~$ /challenge/files/pwncollege-
pwncollege-family      pwncollege-flamingo    pwncollege-flyswatter  pwncollege-hacking
hacker@globbing~multiple-options-for-tab-completion:~$ /challenge/files/pwncollege-hacking
/challenge/files/pwncollege-hacking: line 1: No: command not found
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-hacking
No flag in this file!
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-flyswatter
No flag in this file!
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-flamingo
No flag in this file!
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-family
No flag in this file!
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn
No flag in this file!
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn
pwn                    pwn-the-planet         pwncollege-flag        pwncollege-flyswatter
pwn-college            pwncollege-family      pwncollege-flamingo    pwncollege-hacking
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn
pwn                    pwn-the-planet         pwncollege-flag        pwncollege-flyswatter
pwn-college            pwncollege-family      pwncollege-flamingo    pwncollege-hacking
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn-the-plant
cat: /challenge/files/pwn-the-plant: No such file or directory
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn-the-planet
No flag in this file!
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-flag
pwn.college{AMGjH5hi0giURdBJp3-EdcYHFh8.0lN0EzNxwSO2EzNwIzW}
```

## Challenge 10: Tab completion on commands

### Problem Statement:
```
Tab completion is for more than files! You can also tab-complete commands. This level has a command that starts with pwncollege , and it'll give you the flag. Type pwncollege and hit the tab key to auto-complete it!

NOTE: You can auto-complete any command, but be careful: callous auto-completes without double-checking the result can wreak havoc in your shell if you accidentally run the wrong commands!
```

### Key points:
- speeds up system utility invocation and completely prevents spelling mistakes
- allows you to easily discover internal challenge binaries without manual directory digging
- so i did pwn then hit tab
```bash
Connected!
hacker@globbing~tab-completion-on-commands:~$ pwncollege-15337
Correct! Here is your flag:
pwn.college{AwmRV1eeHPLGQ2c4dRUN36PisY_.0VN0EzNxwSO2EzNwIzW}
```