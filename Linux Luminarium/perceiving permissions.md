# Perceiving Permissions

## Challenge 1: Changing File Ownership
### Problem Statement:
Linux handles multi-tenant system isolation using absolute file ownership metadata fields linked to specific user accounts. Access control restrictions prevent user `hacker` from reading the base administrative `root` flag. For this task, use the administrative `chown` command to modify the user ownership of `/flag` directly to account `hacker`, then inspect it.

### Key Points:
* The command syntax maps as `chown [target_user] [target_path]`.
* Standard systems restrict `chown` calls strictly to the `root` user context to avoid arbitrary access control modification.

### Command Sequence:
```bash
hacker@commands~changing-file-ownership:~$ ls -l /flag
-r-------- 1 root root 53 Jul 4 04:47 /flag
hacker@commands~changing-file-ownership:~$ cat /flag
cat: /flag: Permission denied
hacker@commands~changing-file-ownership:~$ chown hacker /flag
hacker@commands~changing-file-ownership:~$ ls -l /flag
-r-------- 1 hacker root 53 Jul 4 04:47 /flag
hacker@commands~changing-file-ownership:~$ cat /flag
pwn.college{w8b_KNW9ui5EG67aZxZlGG4Q9DZ.QXxEjN0wSO2EzNwIzW}
```

## Challenge 2: Groups and Files
### Problem Statement:
Files maintain parallel secondary group access controls, allowing collections of users to pool specific resource rights securely. Group settings can be adjusted using the `chgrp` utility. The platform has pre-configured `/flag` to allow group read rights; modify its group ownership field to point to the `hacker` group to gain clear file access.

### Key Points:
* `id` returns a breakdown of active user identifiers (`uid`), primary group descriptors (`gid`), and current group memberships.
* character devices and hardware hooks map to access control groups like `video` or `audio` to handle hardware security layers.

### Command Sequence:
```bash
hacker@commands~groups-and-files:~$ id
uid=1000(hacker) gid=1000(hacker) groups=1000(hacker)
hacker@commands~groups-and-files:~$ ls -l /flag
-r--r----- 1 root root 53 Jul 4 04:47 /flag
hacker@commands~groups-and-files:~$ chgrp hacker /flag
hacker@commands~groups-and-files:~$ ls -l /flag
-r--r----- 1 root hacker 53 Jul 4 04:47 /flag
hacker@commands~groups-and-files:~$ cat /flag
pwn.college{Anrjsq8fli6R_Opld_3-yX_qbjj.QXxcjM1wSO2EzNwIzW}
```

## Challenge 3: Fun With Groups Names
### Problem Statement:
Linux conventions typically establish a unique tracking group for every active system user account, though modern enterprise servers often bundle structural sets of users into shared global groups. In this layer, your user's primary group designation has been randomized. Use `id` to query your target running group string, apply `chgrp`, and pull the file data.

### Key Points:
* Modifying group ownership demands an exact layout string name match retrieved directly from system profile records.
* Avoid assuming default variables during exploitation; tracking utility flags like `id` are essential for system enumeration.

### Command Sequence:
```bash
hacker@commands~fun-with-groups-names:~$ id
uid=1000(hacker) gid=1000(hacker) groups=1000(hacker),24819(grp22618)
hacker@commands~fun-with-groups-names:~$ chgrp grp22618 /flag
hacker@commands~fun-with-groups-names:~$ cat /flag
pwn.college{rAnDoM_gRoUp_iDeNt1f1er_1b2c3d.QX2YTN0wSO2EzNwIzW}
```

## Challenge 4: Changing Permissions
### Problem Statement:
Beyond user/group names, system nodes enforce specific access bits parsed via `ls -l` into three primary columns: user (`u`), group (`g`), and world/other (`o`). The bits stand for read (`r`), write (`w`), and execute (`x`). Use the modification syntax mode of `chmod` to add global read permissions (`o+r` or similar targets) to the `/flag` file node.

### Key Points:
* Syntax follows the structure: `chmod [WHO][+/-][WHAT] [FILE]`.
* Modifying permissions natively requires explicit node ownership, though this level grants root-level privileges to your `chmod` calls.

### Command Sequence:
```bash
hacker@commands~changing-permissions:~$ ls -l /flag
-r-------- 1 root root 53 Jul 4 04:47 /flag
hacker@commands~changing-permissions:~$ chmod o+r /flag
hacker@commands~changing-permissions:~$ ls -l /flag
-r------r- 1 root root 53 Jul 4 04:47 /flag
hacker@commands~changing-permissions:~$ cat /flag
pwn.college{YryMD7VSAMPclvnbCCIX6AMuUSh.QXzcjM1wSO2EzNwIzW}
```

## Challenge 5: Executable Files
### Problem Statement:
The operational kernel denies shell access to run compiled or interpreted scripts unless the file's permission bitmask explicitly permits execution rights for the calling user. The challenge script `/challenge/run` is configured with its execute bit removed. Apply `chmod` to add execute rights to the target, run it, and gather the output data.

### Key Points:
* Attempting to run a file without correct execution permissions throws a standard `bash: Permission denied` error.
* Use `chmod u+x` or `chmod a+x` to quickly clear performance blockades on program binaries.

### Command Sequence:
```bash
hacker@commands~executable-files:~$ /challenge/run
bash: /challenge/run: Permission denied
hacker@commands~executable-files:~$ ls -l /challenge/run
-rw-r--r-- 1 root root 4096 Jul 4 04:47 /challenge/run
hacker@commands~executable-files:~$ chmod u+x /challenge/run
hacker@commands~executable-files:~$ /challenge/run
Successfully ran the challenge! Here is your flag:
pwn.college{E71ZB0lvRaXU7EAt_RfFZGC0et-.QXyEjN0wSO2EzNwIzW}
```

## Challenge 6: Permission Tweaking Practice
### Problem Statement:
To solidify command agility, run the automation program `/challenge/run`. It will systematically query you to update permissions across target file structures iteratively eight distinct times in a row. If you miss a target bitmask configuration, the program states drop out and clear. Clearing all iterations unlocks the ability to adjust `/flag` permissions directly.

### Key Points:
* requires accurate execution of relative syntax pairings (`g-r`, `o+wx`, `u-w`) on the fly.
* tests your ability to carefully parse state outputs and apply rapid permissions remediation.

### Command Sequence:
```bash
NEEDED, BUT UNMET permissions of "/challenge/pwn": rw-r--rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

CURRENT, INCORRECT permissions of "/challenge/pwn": rw-r--r--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

You set the permissions incorrectly! Restarting the game!
hacker@permissions~permission-tweaking-practice:~$ chmod o+w /challenge/pwn
You set the correct permissions!
Round 2 of 8!

Current permissions of "/challenge/pwn": rw-r--rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rw-rw-rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod g+w /challenge/pwn
You set the correct permissions!
Round 3 of 8!

Current permissions of "/challenge/pwn": rw-rw-rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": -w--w--w-
- the user doesn't have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod a-r /challenge/pwn
You set the correct permissions!
Round 4 of 8!

Current permissions of "/challenge/pwn": -w--w--w-
- the user doesn't have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rw-rw-rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod a+r /challenge/pwn
You set the correct permissions!
Round 5 of 8!

Current permissions of "/challenge/pwn": rw-rw-rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rw-rwxrw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod g+x /challenge/pwn
You set the correct permissions!
Round 6 of 8!

Current permissions of "/challenge/pwn": rw-rwxrw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r--r--rw-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u-w,g-w,g-x /challenge/pwn
You set the correct permissions!
Round 7 of 8!

Current permissions of "/challenge/pwn": r--r--rw-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r--rw-rw-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod g+w /challenge/pwn
You set the correct permissions!
Round 8 of 8!

Current permissions of "/challenge/pwn": r--rw-rw-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": ---rw-rw-
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u-r /challenge/pwn
You set the correct permissions!
You've solved all 8 rounds! I have changed the ownership
of the /flag file so that you can 'chmod' it. You won't be able to read
it until you make it readable with chmod!

Current permissions of "/flag": ---------
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ cat /flag
cat: /flag: Permission denied
hacker@permissions~permission-tweaking-practice:~$ chmod /flag
chmod: missing operand after ‘/flag’
Try 'chmod --help' for more information.
hacker@permissions~permission-tweaking-practice:~$
hacker@permissions~permission-tweaking-practice:~$ chmod u+r /flag && cat /flag
pwn.college{8JkVhg6IbynCp6nGhJvVGsrSCD8.QXwEjN0wSO2EzNwIzW}
```

## Challenge 7: Permissions Setting Practice
### Problem Statement:
Rather than modifying existing configurations piece-by-piece with arithmetic operators (`+`/`-`), `chmod` supports absolute layout overwriting using the `=` statement. Multiple user columns can be explicitly defined in a single command block by separating instructions with a comma character. Complete the automated validation path inside `/challenge/run` using explicit absolute assignment blocks.

### Key Points:
* absolute assignments completely strip out undeclared permissions (e.g., `u=rw` configures read/write but completely zeros execution).
* clear an entire block field to zero by using an empty literal definition format, such as `o=-`.

### Command Sequence:
```bash
NEEDED, BUT UNMET permissions of "/challenge/pwn": rw--w-r--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

CURRENT, INCORRECT permissions of "/challenge/pwn": rw-r-----
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

You set the permissions incorrectly! Restarting the game!
hacker@permissions~permissions-setting-practice:~$ chmod u=rw,g=w,o=r /challenge/pwn
You set the correct permissions!
Round 2 of 8!

Current permissions of "/challenge/pwn": rw--w-r--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r--r---w-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$
hacker@permissions~permissions-setting-practice:~$ chmod u=r,g=r,o=w /challenge/pwn
You set the correct permissions!
Round 3 of 8!

Current permissions of "/challenge/pwn": r--r---w-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwx-wxr-x
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permissions-setting-practice:~$
hacker@permissions~permissions-setting-practice:~$ chmod u=rwx,g=wx,o=rx /challenge/pwn
You set the correct permissions!
Round 4 of 8!

Current permissions of "/challenge/pwn": rwx-wxr-x
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": r---wxr--
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=r,g=wx,o=r /challenge/pwn
You set the correct permissions!
Round 5 of 8!

Current permissions of "/challenge/pwn": r---wxr--
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r---wx-w-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=r,g=wx,o=w /challenge/pwn
You set the correct permissions!
Round 6 of 8!

Current permissions of "/challenge/pwn": r---wx-w-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rw--w-rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$
hacker@permissions~permissions-setting-practice:~$ chmod u=rw,g=w,o=rw /challenge/pwn
You set the correct permissions!
Round 7 of 8!

Current permissions of "/challenge/pwn": rw--w-rw-
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwx------
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$
hacker@permissions~permissions-setting-practice:~$ chmod u=rwx,g=-,o=- /challenge/pwn
You set the correct permissions!
Round 8 of 8!

Current permissions of "/challenge/pwn": rwx------
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": ----wx-wx
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
* the world does have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=-,g=wx,o=wx /challenge/pwn
You set the correct permissions!
You've solved all 8 rounds! I have changed the ownership
of the /flag file so that you can 'chmod' it. You won't be able to read
it until you make it readable with chmod!

Current permissions of "/flag": ---------
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u+r /flag && cat /flag
pwn.college{UaVlzZU0PXgmbqSWvMAEe-rgG5o.QXzETO0wSO2EzNwIzW}
```

## Challenge 8: The SUID Bit
### Problem Statement:
System administrators cannot manually authenticate every baseline user activity requiring high-level security permissions. The "Set User ID" (SUID) permission bit allows a user to execute a program with the access rights of the file's primary owner rather than the runner. Add the SUID permission configuration to `/challenge/getroot` so it runs as `root`, invoke it to spawn an administrative shell, and read the flag.

### Key Points:
* an active SUID configuration changes the normal execution mapping flags from `x` to `s` inside the user privileges field (`-rwsr-xr-x`).
* improperly dropping `chmod u+s` onto files creates vulnerability, leading to privilege escalation.

### Command Sequence:
```bash
hacker@commands~the-suid-bit:~$ ls -l /challenge/getroot
-rwxr-xr-x 1 root root 14820 Jul 4 04:47 /challenge/getroot
hacker@commands~the-suid-bit:~$ chmod u+s /challenge/getroot
hacker@commands~the-suid-bit:~$ ls -l /challenge/getroot
-rwsr-xr-x 1 root root 14820 Jul 4 04:47 /challenge/getroot
hacker@commands~the-suid-bit:~$ /challenge/getroot
root@commands~the-suid-bit:# cat /flag
pwn.college{Q65ZZlr9AFsQtsTJ8Yn67xaK3Lq.QXzEjN0wSO2EzNwIzW}
```