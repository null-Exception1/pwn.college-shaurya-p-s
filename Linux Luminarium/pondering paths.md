# Pondering Paths Module

## Challenge 1: The Root
> learnt how to run programs in linux

### Solve:
- a general case windows equivalent of an executable is called a binary in linux
- at first i thought the command should be ./pwn, but then they suggested i do /pwn instead


```bash
hacker@paths~the-root:~$ ./pwn
bash: ./pwn: No such file or directory
hacker@paths~the-root:~$ ls
hacker@paths~the-root:~$ pwn
It looks like you invoked 'pwn' instead of the absolute path ('/pwn'). You'll 
learn more about how this is different later, but suffice it to say: you ran 
the wrong thing. Use the absolute path, instead!
hacker@paths~the-root:~$ /pwn
BOOM!!!
Here is your flag:
pwn.college{sST__00WpmQRFGZIVZWIKlTaVCG.QX4cTO0wSO2EzNwIzW}
```

### Flag

pwn.college{sST__00WpmQRFGZIVZWIKlTaVCG.QX4cTO0wSO2EzNwIzW}

## Challenge 2: Programs and Absolute Paths
> learnt what exactly absolute paths are

### Solve:
- always use absolute paths when you're trying to find in reference to root

```bash
hacker@paths~program-and-absolute-paths:~$ /challenge/run
Correct!!!
/challenge/run is an absolute path! Here is your flag:
pwn.college{ccsSrDf1OqRTtugramLC7P-fEjF.QX1QTN0wSO2EzNwIzW}
```

### Flag

pwn.college{ccsSrDf1OqRTtugramLC7P-fEjF.QX1QTN0wSO2EzNwIzW}

## Challenge 3: Position thyself
> learnt what exactly relative paths are and how to enter and leave directories by doing `cd`

### Solve:
- you need to enter a certain directory to run the program from to get the flag
- u do so by doing cd

```bash
hacker@paths~position-thy-self:~$ cd challenge
bash: cd: challenge: No such file or directory
hacker@paths~position-thy-self:~$ cd /challenge
hacker@paths~position-thy-self:/challenge$ run
bash: run: command not found
hacker@paths~position-thy-self:/challenge$ cd run
bash: cd: run: Not a directory
hacker@paths~position-thy-self:/challenge$ ls
Dockerfile  run
hacker@paths~position-thy-self:/challenge$ run
bash: run: command not found
hacker@paths~position-thy-self:/challenge$ ./run
Incorrect...
You are not currently in the / directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-thy-self:/challenge$ cd ..
hacker@paths~position-thy-self:/$ ./challenge/run
Incorrect...
You did not call this challenge using an absolute path!
An absolute path is anchored at the root of the filesystem, so it starts with /
hacker@paths~position-thy-self:/$ cd ..
hacker@paths~position-thy-self:/$ ./challenge/run
Incorrect...
You did not call this challenge using an absolute path!
An absolute path is anchored at the root of the filesystem, so it starts with /
hacker@paths~position-thy-self:/$ pwd
/
hacker@paths~position-thy-self:/$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{oHBGhAHlZFiUukWJjV8SkFAfjsY.QX2QTN0wSO2EzNwIzW}
hacker@paths~position-thy-self:/$ 
```

### Flag

pwn.college{oHBGhAHlZFiUukWJjV8SkFAfjsY.QX2QTN0wSO2EzNwIzW}


## Challenge 4: Position elsehwere
> same thing 5 times
### Solve:

```bash
hacker@paths~position-elsewhere:~$ /challenge/run
Starting level 1.
Incorrect...
You are not currently in the /var directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-elsewhere:~$ cd /var
hacker@paths~position-elsewhere:/var$ /challenge/run
Starting level 1.
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Moving on to level 2
Please use the `cd` utility to change directory to /usr/include
hacker@paths~position-elsewhere:/var$ cd /user/include
bash: cd: /user/include: No such file or directory
hacker@paths~position-elsewhere:/var$ cd /usr/include
hacker@paths~position-elsewhere:/usr/include$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Moving on to level 3
Please use the `cd` utility to change directory to /tmp
hacker@paths~position-elsewhere:/usr/include$ cd /tmp
hacker@paths~position-elsewhere:/tmp$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Moving on to level 4
Please use the `cd` utility to change directory to /sys/kernel
hacker@paths~position-elsewhere:/tmp$ ^C
hacker@paths~position-elsewhere:/tmp$ cd /sys/kernel
hacker@paths~position-elsewhere:/sys/kernel$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Moving on to level 5
Please use the `cd` utility to change directory to /etc
hacker@paths~position-elsewhere:/sys/kernel$ cd /etc
hacker@paths~position-elsewhere:/etc$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{w91Xa0suLwOqS-JvybsuoxiO2u0.QX3QTN0wSO2EzNwIzW}
hacker@paths~position-elsewhere:/etc$ ^C
hacker@paths~position-elsewhere:/etc$ 
```

### Flag

pwn.college{w91Xa0suLwOqS-JvybsuoxiO2u0.QX3QTN0wSO2EzNwIzW}


# Challenge 5: implicit relative paths from /

>instead of using absolute paths you can remove the / from the beginning and find using ur relative path


### Solve:

```bash
hacker@paths~implicit-relative-paths-from-:~$ cwd
bash: cwd: command not found
hacker@paths~implicit-relative-paths-from-:~$ cd /
hacker@paths~implicit-relative-paths-from-:/$ /challenge/run
Incorrect...
You invoked this challenge with an absolute path. This challenge needs a relative path!
hacker@paths~implicit-relative-paths-from-:/$ ls
bin  boot  challenge  dev  etc  flag  home  lib  lib64  media  mnt  nix  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
hacker@paths~implicit-relative-paths-from-:/$ cd challenge
hacker@paths~implicit-relative-paths-from-:/challenge$ /challenge/run
Incorrect...
You are not currently in the / directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~implicit-relative-paths-from-:/challenge$ /run
bash: /run: Is a directory
hacker@paths~implicit-relative-paths-from-:/challenge$ ./run
Incorrect...
You are not currently in the / directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~implicit-relative-paths-from-:/challenge$ cd ..
hacker@paths~implicit-relative-paths-from-:/$ challenge/run
Correct!!!
challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{YJf0H3y_7DJxBW80rBA0l-ekdY0.QX5QTN0wSO2EzNwIzW}
```

### Flag

pwn.college{YJf0H3y_7DJxBW80rBA0l-ekdY0.QX5QTN0wSO2EzNwIzW}



# Challenge 6: explicit relative paths from /

>need to use . and or .. in paths

### Solve:

- . means cwd
- .. means pwd

```bash
hacker@paths~explicit-relative-paths-from-:~$ pwd
/home/hacker
hacker@paths~explicit-relative-paths-from-:~$ /challenge/run
Incorrect...
You are not currently in the / directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~explicit-relative-paths-from-:~$ cd /
hacker@paths~explicit-relative-paths-from-:/$ /challenge/run
Incorrect...
You invoked this challenge with an absolute path. This challenge needs a relative path!
hacker@paths~explicit-relative-paths-from-:/$ ./challenge/run
Correct!!!
./challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{MSrkk6P4b97WPz1d9yIiGdp-k-q.QXwUTN0wSO2EzNwIzW}
```

### Flag

pwn.college{MSrkk6P4b97WPz1d9yIiGdp-k-q.QXwUTN0wSO2EzNwIzW}



# Challenge 7: explicit relative paths from /

>need to use . and or .. in paths

### Solve:

- . means cwd
- .. means pwd

```bash
hacker@paths~explicit-relative-paths-from-:~$ pwd
/home/hacker
hacker@paths~explicit-relative-paths-from-:~$ /challenge/run
Incorrect...
You are not currently in the / directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~explicit-relative-paths-from-:~$ cd /
hacker@paths~explicit-relative-paths-from-:/$ /challenge/run
Incorrect...
You invoked this challenge with an absolute path. This challenge needs a relative path!
hacker@paths~explicit-relative-paths-from-:/$ ./challenge/run
Correct!!!
./challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{MSrkk6P4b97WPz1d9yIiGdp-k-q.QXwUTN0wSO2EzNwIzW}
```

### Flag

pwn.college{MSrkk6P4b97WPz1d9yIiGdp-k-q.QXwUTN0wSO2EzNwIzW}



# Challenge 8: implicit relative paths

>need to use . and or .. in cwd instead of /

### Solve:

- part 2 of implicit

```bash
hacker@paths~explicit-relative-paths-from-:/$
Connected!
hacker@paths~implicit-relative-path:~$ exit
logout
Connection to dojo.pwn.college closed.
shaurya_pratap@cheeese:~$ ssh -i key hacker@dojo.pwn.college
Connected!
hacker@paths~implicit-relative-path:~$ cd /
hacker@paths~implicit-relative-path:/$ cd challenge
hacker@paths~implicit-relative-path:/challenge$ ./run
Correct!!!
./run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{EhirSssL-qYu-0XaAN8GKKjInPq.QXxUTN0wSO2EzNwIzW}
```

### Flag

pwn.college{EhirSssL-qYu-0XaAN8GKKjInPq.QXxUTN0wSO2EzNwIzW}


# Challenge 9: home sweet home

>use a special command, ~ is a special absolute path for your home path


### Solve:

- Your argument must be an absolute path. - so basically should start with /
- The path must be inside your home directory. - got it so if you do it from anywhere else ur done
- Before expansion, your argument must be three characters or less. - single letter file lmao ~/ and then a single letter will suffice

```bash
hacker@paths~home-sweet-home:~$ /challenge/run ~/a
Writing the file to /home/hacker/a!
... and reading it back to you:
pwn.college{wxm7aCgp91WPXrEJh4dqv2zZaz5.QXzMDO0wSO2EzNwIzW}
```

### Flag

pwn.college{wxm7aCgp91WPXrEJh4dqv2zZaz5.QXzMDO0wSO2EzNwIzW}

## Concepts Learnt
- how to navigate between directories
- absolute vs relative paths
- home directory and its shortcut
- why u have to run programs in the cwd with ./ instead of doing straight program names

## References
- i would like to thank my parents for raising me

