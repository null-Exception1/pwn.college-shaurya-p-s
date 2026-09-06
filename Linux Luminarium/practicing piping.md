# Practicing Piping

## Challenge 1: Redirecting output

### Problem Statement:
```
First, let's look at redirecting stdout to files. You can accomplish this with the > character, as so:

hacker@dojo:~$ echo hi > asdf

This will redirect the output of echo hi (which will be hi ) to the file asdf . You can then use a program such as cat to output this file:

hacker@dojo:~$ cat asdf hi

In this challenge, you must use this output redirection to write the word PWN (all uppercase) to the filename COLLEGE (all uppercase).
```

### Key points:
- `>` lets you redirect stdout of a process straight into a file
- if the file is already there it truncates everything inside first
- i need to echo the string PWN into a file named COLLEGE

```bash
hacker@piping~redirecting-output:~$ echo PWN > COLLEGE
pwn.college{QPjd3YurXECI7svnnpp7rHMIM0E.QX0YTN0wSO2EzNwIzW}
```

## Challenge 2: Redirecting more output

### Problem Statement:
```
Aside from redirecting the output of echo , you can, of course, redirect the output of any command. In this level, /challenge/run will once more give you a flag, but *only* if you redirect its output to the file myflag . Your flag will, of course, end up in the myflag file!
```

### Key points:
- redirection works on any command execution block not just echo
- programs can print text to terminal via stderr even if stdout is redirected
- i need to redirect the run binary output into a file called myflag

```bash
hacker@piping~redirecting-more-output:~$ /challenge/run > myflag
[INFO] WELCOME! This challenge makes the following asks of you:
...
hacker@piping~redirecting-more-output:~$ cat myflag
pwn.college{0KN_TBXt9KEf2jgkq084OTwlFWx.QX1YTN0wSO2EzNwIzW}
```

## Challenge 3: Appending output

### Problem Statement:
```
You can redirect input in append mode using >> instead of > , as so:

hacker@dojo:~$ echo pwn > outfile hacker@dojo:~$ echo college >> outfile
```

### Key points:
- `>` overwrites files but `>>` appends incoming stream text to the bottom of the file
- helps aggregate output without destroying old context history
- i need to run the challenge binary using append mode targeting the-flag path

```bash
hacker@piping~appending-output:~$ /challenge/run >> /home/hacker/the-flag
hacker@piping~appending-output:~$ cat /home/hacker/the-flag
pwn.college{w_nO-w717bF4XFVHaivmUW3MKfn.QX3ATO0wSO2EzNwIzW}
```

## Challenge 4: Redirecting errors

### Problem Statement:
```
A File Descriptor (FD) is a number that describes a communication channel in Linux.
- FD 0: Standard Input
- FD 1: Standard Output
- FD 2: Standard Error
```

### Key points:
- file descriptors explicitly label process channels where 0 is stdin, 1 is stdout, 2 is stderr
- you can catch errors by putting the descriptor number right before the arrow like 2>
- i just need to redirect stdout to myflag and stderr to instructions at the same time

```bash
hacker@piping~redirecting-errors:~$ /challenge/run > myflag 2> instructions
hacker@piping~redirecting-errors:~$ cat myflag
pwn.college{cWWL-KhV42zKjoCovW9f8lkhPZ0.QX3YTN0wSO2EzNwIzW}
```

## Challenge 5: Redirecting input

### Problem Statement:
```
Just like you can redirect output from programs, you can redirect input to programs! This is done using <
```

### Key points:
- `<` operator passes file content straight into the stdin channel of a program
- easy way to feed inputs without typing manually inside the application terminal
- i need to echo COLLEGE into a file called PWN and then pass it as input to the runner

```bash
hacker@piping~redirecting-input:~$ echo COLLEGE > PWN
hacker@piping~redirecting-input:~$ /challenge/run < PWN
pwn.college{4Aq-cyOPevHlhd2SvyPhmpNV7sz.QXwcTN0wSO2EzNwIzW}
```

## Challenge 6: Grepping stored results

### Problem Statement:
```
1. Redirect the output of /challenge/run to /tmp/data.txt .
2. This will result in a hundred thousand lines of text, with one of them being the flag, in /tmp/data.txt .
3. grep that for the flag!
```

### Key points:
- saving output data logs to a tmp file helps look through huge bulks of text strings later
- piping straight to grep fails because the challenge validates if stdout is a real file path descriptor
- so first i redirect the run output to data.txt then grep it out using the pwn keyword

```bash
hacker@piping~grepping-stored-results:~$ /challenge/run > /tmp/data.txt
hacker@piping~grepping-stored-results:~$ grep pwn.college /tmp/data.txt
pwn.college{M2cS9lmIxlNVObfjlesQU9zsdZZ.QX4EDO0wSO2EzNwIzW}
```

## Challenge 7: Grepping live output

### Problem Statement:
```
You can do this by using the | (pipe) operator. Standard output from the command to the left of the pipe will be connected to ( piped into) the standard input of the command to the right of the pipe.
```

### Key points:
- `|` links the stdout channel of one process to the stdin channel of another process directly
- removes the annoying step of creating temporary scratch files on disk space
- all i need to do is pipe the binary stream directly into grep

```bash
hacker@piping~grepping-live-output:~$ /challenge/run | grep pwn.college
pwn.college{QWnPCxb68gXCqvYlpY6dpcB-9_E.QX5EDO0wSO2EzNwIzW}
```

## Challenge 8: Grepping errors

### Problem Statement:
```
The shell has a >& operator, which redirects a file descriptor to another file descriptor.
```

### Key points:
- the standard pipe line operator only catches descriptor 1 by default
- using `2>&1` maps the stderr stream onto the stdout path so everything gets merged together
- so i merge stderr into stdout and then pipe the whole stream straight into grep

```bash
hacker@piping~grepping-errors:~$ /challenge/run 2>&1 | grep pwn.college
pwn.college{sOYkUhhYVWGNZnF4LexGBbrlGES.QX1ATO0wSO2EzNwIzW}
```

## Challenge 9: Filtering with grep -v

### Problem Statement:
```
The grep command has a very useful option: -v (invert match). While normal grep shows lines that MATCH a pattern, grep -v shows lines that do NOT match a pattern
```

### Key points:
- grep -v reverses matching rules to output lines that do not have the search term
- highly effective for filtering out spam log lines or decoy strings from clean outputs
- i just need to filter out all the decoy flags containing the string DECOY

```bash
hacker@piping~filtering-with-grep-v:~$ /challenge/run | grep -v DECOY
pwn.college{UUd-gSeBQRlhbMTv4vGSrN0vmDS.0FOxEzNxwSO2EzNwIzW}
```

## Challenge 10: Filtering with sed

### Problem Statement:
```
sed provides an easy way to substitute patterns in text with a different word. The syntax for matching and replacing is simple: sed "s/oldword/newword/g"
```

### Key points:
- sed is a stream editor that dynamically replaces text chunks inside pipeline buffers
- global flag updates every iteration instance across the whole target row context string
- i just use an empty string block to strip the FAKEFLAG noise out of the flag characters

```bash
hacker@piping~filtering-with-sed:~$ /challenge/run | sed "s/FAKEFLAG//g"
pwn.college{8Daai4yIrgWwKT5nWpzwy8Os7fa.01NxQTMywSO2EzNwIzW}
```

## Challenge 11: Duplicating piped data with tee

### Problem Statement:
```
The tee command, named after a "T-splitter" from plumbing pipes, duplicates data flowing through your pipes to any number of files provided on the command line.
```

### Key points:
- tee clones pipeline stream buffers into physical logs without breaking data flow to stdout
- helps intercept or spy on backend communications when a command sequence starts failing
- i use tee to dump the pwn command feedback to a file, read the secret argument code, and rerun it

```bash
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn | tee debug.txt | /challenge/college
hacker@piping~duplicating-piped-data-with-tee:~$ cat debug.txt
SECRET_ARG should be "wst-7w-r"
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn --secret wst-7w-r | /challenge/college
pwn.college{wst-7w-rJWIaCBOLasin3QcWKXw.QXxITO0wSO2EzNwIzW}
```

## Challenge 12: Process substitution for input

### Problem Statement:
```
For reading from a command (input process substitution), use <(command) . When you write <(command) , bash will run the command and hook up its output to a temporary file
```

### Key points:
- process substitution turns a live binary execution stream into a temporary named pipe file path
- allows tools that exclusively demand flat file arguments to pull straight from live process loops
- so i just need to compare the logs of the two print commands using diff side by side

```bash
hacker@piping~process-substitution-for-input:~$ diff <(/challenge/print_decoys) <(/challenge/print_decoys_and_flag)
pwn.college{A-XHfv0dl7y0WErEDVcc5e9vfJn.0lNwMDOxwSO2EzNwIzW}
```

## Challenge 13: Writing to multiple programs

### Problem Statement:
```
For writing to a command (output process substitution), use >(command) .
```

### Key points:
- output process substitution turns a binary hook into a destination file path you can write to
- combining it with tee lets you clone a single output feed to multiple independent processes at once
- i pass the output of hack straight into the process paths of the and planet using tee

```bash
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | tee >(/challenge/the) >(/challenge/planet)
pwn.college{soyY6K2sUVYsu3-k5cv91WtAnuo.QXwgDN1wSO2EzNwIzW}
```

## Challenge 14: Split-piping stderr and stdout

### Problem Statement:
```
You must master the ultimate piping task: redirect stdout to one program and stderr to another.
```

### Key points:
- standard pipe operators only map to descriptor 1 so handling descriptor 2 requires manual hooks
- we can handle both lines separately by passing a process substitution target explicitly into the error route
- i redirect stderr to the via process substitution and pipe stdout directly into planet

```bash
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack 2> >(/challenge/the) | /challenge/planet
pwn.college{8zQ8lHMXQcKVV6XDCtFG6gPcKsa.QXxQDM2wSO2EzNwIzW}
```

## Challenge 15: Named pipes

### Problem Statement:
```
You create a FIFO using the mkfifo command... One problem with FIFOs is that they'll "block" any operations on them until both the read side of the pipe and the write side of the pipe are ready.
```

### Key points:
- mkfifo makes persistent named pipes on the filesystem that hold data queues like real files
- operations block thread execution synchronously until both reader and writer threads connect to the target descriptor path
- i create the pipe, throw the challenge runner into the background with `&` to open the writer side, then read it via cat

```bash
hacker@piping~named-pipes:~$ mkfifo /tmp/flag_fifo
hacker@piping~named-pipes:~$ /challenge/run > /tmp/flag_fifo &
hacker@piping~named-pipes:~$ cat /tmp/flag_fifo
pwn.college{kod86K7rGqV2gKV4vjOYlE4ei2Q.01MzMDOxwSO2EzNwIzW}
```