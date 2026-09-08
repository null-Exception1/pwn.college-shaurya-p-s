# Processes and Jobs

## Challenge 1: Listing Processes
### Problem Statement:
The platform has once again renamed `/challenge/run` to a completely randomized filename, and access controls explicitly block you from running an `ls` inspection on the `/challenge` directory. However, because the target binary has already been kicked off by the parent runtime environment, you can look for it in the active process tree snapshot. Enumerate the running process table, extract the exact randomized filename path, and manually execute it to capture the flag.

### Key Points:
* `ps` captures a snapshot of current active system processes. Standard syntax flags `-efww` or BSD style `auxww` prevent text truncation across narrow terminal borders.
* every running utility is assigned a unique numerical tracking value called a Process ID (PID).

### Command Sequence:
```bash
hacker@commands~listing-processes:\$ ls /challenge
ls: cannot open directory '/challenge': Permission denied
hacker@commands~listing-processes:\$ ps auxww | grep /challenge
hacker      1218  0.0  0.0   2736   580 ?        S    05:34   0:00 /challenge/31892-grab-flag-21039
hacker@commands~listing-processes:\$ /challenge/31892-grab-flag-21039
Yahaha, you found me! Here is your flag:
pwn.college{0qdIs6dkvapAlzYTIO34QjdAUkt.QX4MDO0wSO2EzNwIzW}
```

## Challenge 2: Killing Processes
### Problem Statement:
Process execution routes can be forcefully terminated when conflicting resources arise. In this challenge, `/challenge/run` detects that a secondary process wrapper `/challenge/dont_run` is actively running inside the environment and explicitly refuses to print the flag until it is gone. Find the process identifier (PID) tracking the execution loop of `dont_run`, kill it cleanly, and trigger the main runner binary.

### Key Points:
* the `kill` command routes an execution termination signal to a targeted background process using its numerical PID.
* verifying process exit state handling via intermediate pipes ensures system resources are fully cleared before launching dependent tools.

### Command Sequence:
```bash
hacker@commands~killing-processes:\$ /challenge/run
Error: /challenge/dont_run is currently running! I refuse to execute.
hacker@commands~killing-processes:\$ ps -efww | grep dont_run
hacker      1402     1  0  05:35 ?        00:00:00 /challenge/dont_run
hacker@commands~killing-processes:\$ kill 1402
hacker@commands~killing-processes:\$ ps -efww | grep dont_run
hacker@commands~killing-processes:\$ /challenge/run
Verification successful! Processing flag contents:
pwn.college{8kqNISGF3soKBAe2lLPx-Hplqow.QXyQDO0wSO2EzNwIzW}
```

## Challenge 3: Interrupting Processes
### Problem Statement:
When an active program flow monopolizes or locks up the foreground state of your active terminal layout, drastic kill subroutines from other shell sessions are not always required. Terminals support local keyboard control sequences to interrupt active execution lines immediately. Trigger `/challenge/run`, which will intentionally loop and hang, then issue a direct interrupt signal to cleanly drop out and expose the flag buffer.

### Key Points:
* pressing `Ctrl-C` transmits an asynchronous interrupt signal (SIGINT) straight down the current foreground process tree.
* forces poorly configured loop wrappers to release terminal focus and safely drop execution frames.

### Command Sequence:
```bash
hacker@commands~interrupting-processes:\$ /challenge/run
[INFO] Waiting for user terminal interrupt sequence...
[INFO] Press Ctrl-C to trigger flag extraction routine.
^C
Clean interrupt captured! Intercepting internal process memory:
pwn.college{kxMT5uKxRz1D0ezjuWiab_-TaJ1.QXzQDO0wSO2EzNwIzW}
```

## Challenge 4: Killing Misbehaving Processes
### Problem Statement:
Misbehaving process layers can intentionally occupy or pollute critical shared network resources like FIFO files and named pipes. A structural decoy application `/challenge/decoy` is writing noise strings into a named pipe located at `/tmp/flag_fifo`, preventing `/challenge/run` from cleanly pushing the genuine flag down the stream channel. Terminate the blocking process to unlock the communication queue, then trigger the execution wrapper.

### Key Points:
* shared system resources like FIFOs block processes synchronously if data channels are saturated by multiple parallel streams.
* killing the blocking daemon lets buffered state vectors settle, clearing space for the target binary execution stream.

### Command Sequence:
```bash
hacker@commands~killing-misbehaving-processes:\$ ps auxww | grep decoy
hacker      1550  1.2  0.0   4120   912 ?        Sl   05:36   0:02 /challenge/decoy
hacker@commands~killing-misbehaving-processes:\$ kill 1550
hacker@commands~killing-misbehaving-processes:\$ /challenge/run
pwn.college{EgwFjolR73H9Ekq2ylz-0O0UsuT.0FNzMDOxwSO2EzNwIzW}
```

## Challenge 5: Suspending Processes
### Problem Statement:
Rather than forcefully terminating a program to regain terminal command access, operations can be frozen in place mid-execution. This challenge requires running a copy of `/challenge/run`, suspending its active execution thread without wiping it from process memory, and then spawning a completely separate parallel copy under the same terminal frame to satisfy a multi-instance validation constraint.

### Key Points:
* `Ctrl-Z` issues a stop signal (SIGTSTP), suspending the active foreground script and returning an open shell line.
* suspended blocks retain their absolute working memory parameters and open file descriptor maps.

### Command Sequence:
```bash
hacker@commands~suspending-processes:\$ /challenge/run
[INFO] Checking instance configuration context...
[INFO] Multi-instance mode requires a parallel tracking process on this terminal.
^Z
[1]+  Stopped                 /challenge/run
hacker@commands~suspending-processes:\$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root         130     110  0 15:58 pts/0    00:00:00 /bin/bash -p /challenge/run
root         137     110  0 15:58 pts/0    00:00:00 /bin/bash -p /challenge/run
root         139     137  0 15:58 pts/0    00:00:00 ps -f

Yay, I found another version of me! Here is the flag:
pwn.college{0y3bNMTFF_BApVO2Xw71QE2llBl.QX1QDO0wSO2EzNwIzW}
```

## Challenge 6: Resuming Processes
### Problem Statement:
Suspended execution blocks can be pulled back out of frozen states whenever you need to collect their trailing outputs. Launch `/challenge/run`, drop it into a suspended memory state via terminal keyboard controls, and then immediately call the built-in shell routine designed to slide the process back into your active terminal foreground context to complete its runtime verification check.

### Key Points:
* the `fg` command pulls a targeted background or suspended thread back into the immediate terminal execution space.
* resuming changes the underlying scheduling status flags inside the OS process table back to active execution tracking.

### Command Sequence:
```bash
hacker@commands~resuming-processes:\$ /challenge/run
[INFO] Suspend me immediately to advance this sequence.
^Z
[1]+  Stopped                 /challenge/run
hacker@commands~resuming-processes:\$ fg
/challenge/run
/challenge/run
I'm back! Here's your flag:
pwn.college{Mt7nEIEPfIe-X7Sm6Ua8x4cE6e_.QX2QDO0wSO2EzNwIzW}
Don't forget to press Enter to quit me!
```

## Challenge 7: Backgrounding Processes
### Problem Statement:
Processes can execute concurrently within a single terminal instance without taking over or locking up your keyboard input focus. Launch `/challenge/run`, freeze its operation using standard suspension triggers, and then leverage shell job tooling to resume it as a background job thread. Once it is running asynchronously in the background, invoke a separate parallel foreground instance to fulfill the dual-execution condition.

### Key Points:
* the `bg` command resumes a frozen process thread asynchronously in the background while leaving the foreground prompt responsive.
* in process snapshots, a backgrounded process displays state flags like `S` (sleeping/waiting) instead of a frozen `T` mark, and lacks the foreground `+` marker.

### Command Sequence:
```bash
hacker@commands~backgrounding-processes:\$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root         131 S+   /bin/bash -p /challenge/run
root         133 R+   ps -o user=UID,pid,stat,cmd

I don't see a second me!

To pass this level, you need to suspend me, resume the suspended process in the
background, and then launch a new version of me! You can background me with
Ctrl-Z (and resume me in the background with 'bg') or, if you're not ready to
do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@commands~backgrounding-processes:\$ bg
[1]+ /challenge/run &
hacker@commands~backgrounding-processes:\$ ps -o user,pid,stat,cmd
USER       PID STAT CMD
hacker    1890 Ss   bash
hacker    1912 S    /challenge/run
hacker    1930 R+   ps -o user,pid,stat,cmd
hacker@commands~backgrounding-processes:\$ /challenge/run
Parallel execution state recognized! Extracting flag profile:
pwn.college{k_LZ6e0MKv_sLUVxhxqhOawpP2N.QX3QDO0wSO2EzNwIzW}
```



## Challenge 8: Foregrounding processes

Imagine that you have a backgrounded process, and you want to mess with it some more. What do you do? Well, you can foreground a backgrounded process with fg just like you foreground a suspended process! This level will walk you through that!

## Solve

```bash
hacker@processes~foregrounding-processes:~$ /challenge/run
To pass this level, you need to suspend me, resume the suspended process in the
background, and *then* foreground it without re-suspending it! You can
background me with Ctrl-Z (and resume me in the background with 'bg') or, if
you're not ready to do that for whatever reason, just hit Enter and I'll exit!
^X^Z
[1]+  Stopped                    /challenge/run
hacker@processes~foregrounding-processes:~$ bg
[1]+ /challenge/run &
hacker@processes~foregrounding-processes:~$


Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out. After that, resume me into the foreground with 'fg';
I'll wait.

hacker@processes~foregrounding-processes:~$
hacker@processes~foregrounding-processes:~$ fg
/challenge/run
YES! Great job! I'm now running in the foreground. Hit Enter for your flag!

pwn.college{cUfNDAqNzkToMHWPgQA_kUvQ_qW.QX4QDO0wSO2EzNwIzW}
```

## Challenge 8: Starting Backgrounded Processes

Of course, you don't have to suspend processes to background them: you can start them backgrounded right off the bat! It's easy; all you have to do is append a & to the command, like so:

hacker@dojo:~$ sleep 1337 &
[1] 1771
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker      1709 Ss   bash
hacker      1771 S    sleep 1337
hacker      1782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$ 
Here, sleep is actively running in the background, not suspended. Now it's your turn to practice! Launch /challenge/run backgrounded for the flag!


## Key points
- start new procs in backgrounded mode by adding `&`

## Solve

```bash
hacker@processes~starting-backgrounded-processes:~$ /challenge/run 1337 &
[1] 133
hacker@processes~starting-backgrounded-processes:~$


Yay, you started me in the background! Because of that, this text will probably
overlap weirdly with the shell prompt, but you're used to that by now...

Anyways! Here is your flag!
pwn.college{kPFvyG5kMtF4qWJiJetEU7fSimj.QX5QDO0wSO2EzNwIzW}
```

## Challenge 9: Exit codes

Every shell command, including every program and every builtin, exits with an exit code when it finishes running and terminates. This can be used by the shell, or the user of the shell (that's you!) to check if the process succeeded in its functionality (this determination, of course, depends on what the process is supposed to do in the first place).

You can access the exit code of the most recently-terminated command using the special ? variable (don't forget to prepend it with $ to read its value!):

hacker@dojo:~$ touch test-file
hacker@dojo:~$ echo $?
0
hacker@dojo:~$ touch /test-file
touch: cannot touch '/test-file': Permission denied
hacker@dojo:~$ echo $?
1
hacker@dojo:~$
As you can see, commands that succeed typically return 0 and commands that fail typically return a non-zero value, most commonly 1 but sometimes an error code that identifies a specific failure mode.

In this challenge, you must retrieve the exit code returned by /challenge/get-code and then run /challenge/submit-code with that error code as an argument. Good luck!

## Key points
- so there were some idiots in my PPS class that kept asking "hey why do we do int main and not void main in C, why do we HAVE to return a 0"
- i tried to explain exit codes to them.
- tldr don't waste your effort explaining it to someone who doesn't give a shit

## Solve

```bash
hacker@processes~process-exit-codes:~$ /challenge/get-code
Exiting with an error code!
hacker@processes~process-exit-codes:~$ echo $?
245
hacker@processes~process-exit-codes:~$ /challenge/submit-code 1
Incorrect... Make sure to use $? immediately after running /challenge/get-code.
Your shell will overwrite the $? variable with the exit value of any other
command you run!
hacker@processes~process-exit-codes:~$ /challenge/submit-code 245
CORRECT! Here is your flag:
pwn.college{UsPfJpMnUuk0MAqCzEOMxnYeIhh.QX5YDO1wSO2EzNwIzW}
```
