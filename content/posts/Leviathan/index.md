---
title: "overthewire leviathan war games"
date: 2025-07-26
description: This is a writeup on leviathan from overthewire wargames.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "overthewire wargames leviathan series" # Here you can write a small summary of the post if needed
tags: [leviathan, linux, binary exploitation]
categories: [Overthewire]
---

## Leviathan 0-1

As described on the page, this challenges do not require programming skills and the difficulty level is 1/10.

```jsx
Summary:
Difficulty:     1/10
Levels:         8
Platform:   Linux/x86

Author:
Anders Tonfeldt

Special Thanks:
We would like to thank AstroMonk for coming up with a replacement idea for the last level,
deadfood for finding a leveljump and Coi for finding a non-planned vulnerability.

Description:
This wargame doesn't require any knowledge about programming - just a bit of common
sense and some knowledge about basic *nix commands. We had no idea that it'd be this
hard to make an interesting wargame that wouldn't require programming abilities from 
the players. Hopefully we made an interesting challenge for the new ones.

Leviathan’s levels are called leviathan0, leviathan1, … etc. and
can be accessed on leviathan.labs.overthewire.org through SSH on port 2223.

To login to the first level use:

Username: leviathan0
Password: leviathan0

Data for the levels can be found in the homedirectories. You can look
at /etc/leviathan_pass for the various level passwords.
```

With that, we can log in to our first level and look for what is of interest to us in the home directory.

```jsx
leviathan0@leviathan:~$ ls -a
.  ..  .backup  .bash_logout  .bashrc  .profile
leviathan0@leviathan:~$ 

```

As seen above we have a folder named `.backup` we can look into it and see what is contained in there.

```jsx
leviathan0@leviathan:~$ cd .backup/
leviathan0@leviathan:~/.backup$ ls
bookmarks.html
```

We have an html file called `bookmarks.html` we shall go through it and see if we will find the password for the next level using `grep` as shown below..

```jsx
leviathan0@leviathan:~/.backup$ grep "leviathan1" bookmarks.html 
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is [REDACTED]" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
leviathan0@leviathan:~/.backup$ 
```

We now have the password for leviathan1.

## Leviathan 1-2

For this level, we have a binary, `check`, that we need to look into closely as seen below.

```jsx
leviathan1@leviathan:~$ ls -la
total 36
drwxr-xr-x   2 root       root        4096 Jul 28 19:05 .
drwxr-xr-x 150 root       root        4096 Jul 28 19:06 ..
-rw-r--r--   1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root       root        3851 Jul 28 18:47 .bashrc
-r-sr-x---   1 leviathan2 leviathan1 15084 Jul 28 19:05 check
-rw-r--r--   1 root       root         807 Mar 31  2024 .profile
leviathan1@leviathan:~$ 
```

When we try to execute it, we are required to provide a password to execute the file as shown below.

```jsx
leviathan1@leviathan:~$ ./check 
password: 

```

When we input the wrong password, we get the following output.

```jsx
leviathan1@leviathan:~$ ./check 
password: 124242      
Wrong password, Good Bye ...
leviathan1@leviathan:~$ 

```

We can check the library calls when we execute the binary using the command [`ltrace`](https://ltrace.org/) as sown below.

```jsx
leviathan1@leviathan:~$ ltrace ./check 
__libc_start_main(0x80490ed, 1, 0xffffd424, 0 <unfinished ...>
printf("password: ")                                                                                                 = 10
getchar(0, 0, 0x786573, 0x646f67password: 123141ds
)                                                                                    = 49
getchar(0, 49, 0x786573, 0x646f67)                                                                                   = 50
getchar(0, 0x3231, 0x786573, 0x646f67)                                                                               = 51
strcmp("123", "sex")                                                                                                 = -1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)                                                                                 = 29
+++ exited (status 0) +++
```

From above, the binary is using the `strcmp,` a C library function that compares two strings against one another. From the calls, the binary is comparing the password provided by the user to the word `sex`. We can now try to use the password sex and see if we can get a shell.

```jsx
leviathan1@leviathan:~$ ./check
password: sex
$ whoami
leviathan2
$ 

```

With the password `sex`, we now have a shell. Just like bandit, we can get the password for the next level at `/etc/leviathan_pass/<leviathan(level)>` 

```jsx
$ cat /etc/leviathan_pass/leviathan2
[REDACTED]
$ 

```

## Leviathan 2-3

Just like the previous level, this challenge has a binary too that we need to look into `printfile,` as shown below.

```jsx
leviathan2@leviathan:~$ ls -la
total 36
drwxr-xr-x   2 root       root        4096 Jul 28 19:05 .
drwxr-xr-x 150 root       root        4096 Jul 28 19:06 ..
-rw-r--r--   1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root       root        3851 Jul 28 18:47 .bashrc
-r-sr-x---   1 leviathan3 leviathan2 15072 Jul 28 19:05 printfile
-rw-r--r--   1 root       root         807 Mar 31  2024 .profile
leviathan2@leviathan:~$ 
```

WE can try and execute it to see how it behaves.

```jsx
leviathan2@leviathan:~$ ./printfile 
*** File Printer ***
Usage: ./printfile filename
leviathan2@leviathan:~$ 

```

Form the above output, it seems we are required to pass a file for the binary to execute. We can try and open the password file and see f we can get the password for the next level using the binary.

```jsx
leviathan2@leviathan:~$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...
leviathan2@leviathan:~$ 
```

We can not open the file using the binary. We can create a temporary directory where we can explore the binary further.

```jsx
leviathan2@leviathan:/tmp/new$ ls
printfile  test.txt
leviathan2@leviathan:/tmp/new$ 

```

From above, we can use the binary to open the test file we have created and look at the library calls being made by the binary using `ltrace.` 

```jsx
leviathan2@leviathan:/tmp/new$ ltrace ./printfile test.txt 
__libc_start_main(0x80490ed, 2, 0xffffd3f4, 0 <unfinished ...>
access("test.txt", 4)                                                                                                = 0
snprintf("/bin/cat test.txt", 511, "/bin/cat %s", "test.txt")                                                        = 17
geteuid()                                                                                                            = 12002
geteuid()                                                                                                            = 12002
setreuid(12002, 12002)                                                                                               = 0
system("/bin/cat test.txt" <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                               = 0
+++ exited (status 0) +++
leviathan2@leviathan:/tmp/new$ 
```

  `ltrace ./printfile test.txt`

You're using `ltrace` to trace **library calls** made by the `printfile` program.

---

  `__libc_start_main(0x80490ed, 2, 0xffffd3f4, 0 <unfinished ...>`

This is the standard entry point into a C program — it calls `main()`.

---

  `access("test.txt", 4) = 0`

This checks whether the file `test.txt` is **readable** (mode `4` = read permission). The return value `0` means the file is accessible.

---

  `snprintf("/bin/cat test.txt", 511, "/bin/cat %s", "test.txt") = 17`

The program is **constructing a shell command string** that ends up as:

```
/bin/cat test.txt
```

It's preparing to run this.

  `geteuid() = 12002`

  `geteuid() = 12002`

It checks the **effective user ID (EUID)** twice — value `12002`. This likely corresponds to the user running the binary or the binary's setuid owner.

---

  `setreuid(12002, 12002) = 0`

It sets both the **real** and **effective** user ID to `12002`. This is usually done to **drop privileges** or to ensure the binary runs as a specific user (possibly its setuid owner).

---

  `system("/bin/cat test.txt" <no return ...>`

This line shows the program is now running:

```
/bin/cat test.txt
```

via the `system()` call, which invokes a shell to execute the command.

---

  `-- SIGCHLD (Child exited) ---`

This signal is received when the child process (i.e., `/bin/cat`) **exits**.

---

  `<... system resumed> ) = 0`

The `system()` call completes successfully (return value `0`), meaning the command executed without error.

  `+++ exited (status 0) +++`

The `printfile` program exits **normally** with exit code `0`.

From the binary, we have a small security hole that is, using `system()` with user-supplied input (`test.txt`) and not sanitizing it is dangerous, it makes the binary **vulnerable to command injection**.

In short, the **real user ID (RUID)** represents the actual user who owns the process, essentially your true identity. The **effective user ID (EUID)**, on the other hand, is what the operating system uses to decide whether you have permission to perform certain actions — it's the ID that determines your access rights in most cases (though there are some exceptions).

With that we can try to have a file with spaces and see how the binary will execute it.

```jsx
leviathan2@leviathan:/tmp/new$ touch pass\ file.txt
leviathan2@leviathan:/tmp/new$ ./printfile "pass file.txt" 
/bin/cat: pass: No such file or directory
/bin/cat: file.txt: No such file or directory
leviathan2@leviathan:/tmp/new$ ltrace ./printfile "pass file.txt" 
__libc_start_main(0x80490ed, 2, 0xffffd3f4, 0 <unfinished ...>
access("pass file.txt", 4)                                                                                           = 0
snprintf("/bin/cat pass file.txt", 511, "/bin/cat %s", "pass file.txt")                                              = 22
geteuid()                                                                                                            = 12002
geteuid()                                                                                                            = 12002
setreuid(12002, 12002)                                                                                               = 0
system("/bin/cat pass file.txt"/bin/cat: pass: No such file or directory
/bin/cat: file.txt: No such file or directory
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                               = 256
+++ exited (status 0) +++
leviathan2@leviathan:/tmp/new$ 
```

As seen above, the binary is reading the file as two separate files. We can create a [symbolic link](https://en.wikipedia.org/wiki/Symbolic_link) for the pass part of the file and link it to `/etc/leviathan_pass/leviathan3` in order to get the password for the next level when the binary tries to open the file as shown below.

```jsx
leviathan2@leviathan:/tmp/new$ ln -s /etc/leviathan_pass/leviathan3 /tmp/new/pass
leviathan2@leviathan:/tmp/new$ ls -la
total 40
drwxrwxr-x   2 leviathan2 leviathan2  4096 Jul 29 07:15 .
drwxrwx-wt 455 root       root       20480 Jul 29 07:15 ..
lrwxrwxrwx   1 leviathan2 leviathan2    30 Jul 29 07:15 pass -> /etc/leviathan_pass/leviathan3
-rw-rw-r--   1 leviathan2 leviathan2     0 Jul 29 07:10 pass file.txt
-r-xr-x---   1 leviathan2 leviathan2 15072 Jul 29 06:48 printfile
leviathan2@leviathan:/tmp/new$ 
```

```jsx
ln -s /etc/leviathan_pass/leviathan3 /tmp/new/pass
```

Below is a breakdown of the command above.

- `ln`: the `link` command, used to create links between files.
- `s`: creates a **symbolic (soft) link**, not a hard link.
- `/etc/leviathan_pass/leviathan3`: the **target file** — it contains the password for the user `leviathan3`.
- `/tmp/new/pass`: the **name of the new symlink** that will point to the target.

With that, we now have the password for the next level.

```jsx
leviathan2@leviathan:/tmp/new$ ~/printfile "pass.txt file.txt" 
/bin/cat: pass.txt: No such file or directory
[REDACTED]
leviathan2@leviathan:/tmp/new$ 
```

## Leviathan 3-4

This challenge is similar to level 2 and 3, we have another binary to look into called `level3.` We shall start by executing it to see how it behaves.

```jsx
leviathan3@leviathan:~$ ./level3
Enter the password>  eowevmweg
bzzzzzzzzap. WRONG
leviathan3@leviathan:~$ 
```

We are required to input a password, for that we shall look at the library calls to see what libraries are being called.

```jsx
leviathan3@leviathan:~$ ltrace ./level3 
__libc_start_main(0x80490ed, 1, 0xffffd424, 0 <unfinished ...>
strcmp("h0no33", "kakaka")                                                                                           = -1
printf("Enter the password> ")                                                                                       = 20
fgets(Enter the password> mnnnwenw
"mnnnwenw\n", 256, 0xf7fab5c0)                                                                                 = 0xffffd1fc
strcmp("mnnnwenw\n", "snlprintf\n")                                                                                  = -1
puts("bzzzzzzzzap. WRONG"bzzzzzzzzap. WRONG
)                                                                                           = 19
+++ exited (status 0) +++
```

  `__libc_start_main(0x80490ed, 1, 0xffffd424, 0 <unfinished ...>)`

This is where execution begins. It's calling the program's `main()` function.

---

  `strcmp("h0no33", "kakaka") = -1`

This is likely a **decoy or debug comparison** that the program performs before asking for the actual password.

It compares `"h0no33"` and `"kakaka"` — since they're not equal, `strcmp()` returns `-1`.

---

  `printf("Enter the password> ") = 20`

The program prints the prompt:

```
Enter the password>
```

The return value `20` is the number of characters printed.

---

  `fgets("mnnnwenw\n", 256, 0xf7fab5c0) = 0xffffd1fc`

- This shows the program **reads your input** (`mnnnwenw` followed by a newline).
- It reads up to 256 bytes, and stores it in memory.
- The input includes the newline (`\n`) because `fgets()` does not strip it.

---

  `strcmp("mnnnwenw\n", "snlprintf\n") = -1`

Now the program compares what you typed with the **expected password** (`snlprintf\n`).

Since they're different, the return value is `-1` — meaning the password is **incorrect**.

---

  `puts("bzzzzzzzzap. WRONG")`

The program prints:

```
bzzzzzzzzap. WRONG
```

because your input did not match the expected password.

---

  `+++ exited (status 0) +++`

The program exits normally (`exit code 0`), even though the password was wrong.

we now have the password for the binary `snlprintf`

```jsx
leviathan3@leviathan:~$ ./level3 
Enter the password> snlprintf  
[You've got shell]!
$ whoami
leviathan4
$ cat /etc/leviathan_pass/leviathan4
[REDACTED]
$ 

```

## Leviathan 4-5

For this challenge, we have an interesting folder just like for level 0 as shown below.

```jsx
leviathan4@leviathan:~$ ls -la
total 24
drwxr-xr-x   3 root root       4096 Jul 28 19:05 .
drwxr-xr-x 150 root root       4096 Jul 28 19:06 ..
-rw-r--r--   1 root root        220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root root       3851 Jul 28 18:47 .bashrc
-rw-r--r--   1 root root        807 Mar 31  2024 .profile
dr-xr-x---   2 root leviathan4 4096 Jul 28 19:05 .trash
```

Looking into the directory, we find an interesting binary file as shown below that when executed, it outputs a binary code.

```jsx
leviathan4@leviathan:~$ cd .trash
leviathan4@leviathan:~/.trash$ ls
bin
leviathan4@leviathan:~/.trash$ ls -la
total 24
dr-xr-x--- 2 root       leviathan4  4096 Jul 28 19:05 .
drwxr-xr-x 3 root       root        4096 Jul 28 19:05 ..
-r-sr-x--- 1 leviathan5 leviathan4 14940 Jul 28 19:05 bin
leviathan4@leviathan:~/.trash$ ./bin
00110000 01100100 01111001 01111000 01010100 00110111 01000110 00110100 01010001 01000100 00001010 
```

We shall use an online decryption tool like [cyberchef](https://gchq.github.io/CyberChef/) to decode the message. Upon decryption, we get the password for the next level.

## Leviathan 5-6

Looking at the home directory, we yet have another binary file to explore. We shall start first by executing it to see what happens

```jsx
leviathan5@leviathan:~$ ls -a
.  ..  .bash_logout  .bashrc  leviathan5  .profile
leviathan5@leviathan:~$ ./leviathan5 
Cannot find /tmp/file.log
leviathan5@leviathan:~$ 
```

As seen above the binary file is looking for a file called `file.log` in the temporary directory, which seems cannot be found. We shall look at the library calls being made to see if we can find anything of interest to us.

```jsx
leviathan5@leviathan:~$ ltrace ./leviathan5 
__libc_start_main(0x804910d, 1, 0xffffd414, 0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                                          = 0
puts("Cannot find /tmp/file.log"Cannot find /tmp/file.log
)                                                                                    = 26
exit(-1 <no return ...>
+++ exited (status 255) +++
```

  `__libc_start_main(0x804910d, 1, 0xffffd414, 0 <unfinished ...>)`

This is the start of the program. It sets up the environment and then calls `main()` (located at `0x804910d`).

---

  `fopen("/tmp/file.log", "r") = 0`

The program tries to **open the file `/tmp/file.log` in read mode (`"r"`)**.

- Return value `0` means **failure** — `fopen()` returns `NULL` when it can't open a file.
- This could be because:
    - The file does not exist.
    - The user doesn't have read permissions.
    - It's a broken symlink, or other file system error.

---

  `puts("Cannot find /tmp/file.log") = 26`

Since the file couldn’t be opened, the program prints an error message:

```
Cannot find /tmp/file.log
```

`puts()` returns the number of characters printed — 26 in this case.

---

  `exit(-1 <no return ...>`

The program immediately **exits** with status `-1`.

---

  `+++ exited (status 255) +++`

Because exit codes are unsigned 8-bit integers, `exit(-1)` becomes **`255`** (i.e., `-1 & 0xFF`).

So the program exits with status 255, indicating an **error condition**.

From the above breakdown, we can create a symbolic link with the file and `/etc/leviathan_pass/leviathan6` such that, when we execute the binary, we get the password for level 6.

```jsx
leviathan5@leviathan:~$ ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
leviathan5@leviathan:~$ ./leviathan5 
[REDACTED]
leviathan5@leviathan:~$ 
```

We now have the password for the next level 🙂

## Leviathan 6-7

For this level, we have a binary that requires a four digit pin in order for us to get a shell as shown below.

```jsx
leviathan6@leviathan:~$ ls -la
total 36
drwxr-xr-x   2 root       root        4096 Jul 28 19:05 .
drwxr-xr-x 150 root       root        4096 Jul 28 19:06 ..
-rw-r--r--   1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root       root        3851 Jul 28 18:47 .bashrc
-r-sr-x---   1 leviathan7 leviathan6 15036 Jul 28 19:05 leviathan6
-rw-r--r--   1 root       root         807 Mar 31  2024 .profile
leviathan6@leviathan:~$ ./leviathan6 
usage: ./leviathan6 <4 digit code>
leviathan6@leviathan:~$ 
```

To solve this one, we shall use a script to automate the process and get our shell.

```jsx
#!/bin/bash

for a in {0000..9999}
do
~/leviathan6 $a
done
```

  🔹`#!/bin/bash`

This is the **shebang** — it tells the system to run this script using the Bash shell.

---

  🔹 `for a in {0000..9999}`

This is a **loop** that iterates over all numbers from `0000` to `9999`.

- `{0000..9999}` is Bash's **brace expansion**.
- It expands into all 4-digit numbers (with leading zeros).
- So `a` will take values: `0000`, `0001`, `0002`, ..., `9999`.

---

  🔹 `~/leviathan6 $a`

This runs the binary `leviathan6` in the user's home directory with the current number (`$a`) as an argument.

- Example commands:
    
    ```
    ~/leviathan6 0000
    ~/leviathan6 0001
    ...
    ~/leviathan6 9999
    ```
    

So it's attempting every 4-digit PIN/password.

After the script gets a pin, a shell will pop as seen below.

```jsx
Wrong
Wrong
Wrong
Wrong
Wrong
Wrong
Wrong
Wrong
Wrong
Wrong
Wrong
$ whoami
leviathan7
$ cat /etc/leviathan_pass/leviathan7
[REDACTED]
$ 
```

## Leviathan 7

With the password we got from level 6, we can use it to log in to level 7 and get to complete the challenge. After log in, we are provided with a congratulations note and we are done with the challenge.

```jsx
leviathan7@leviathan:~$ ls
CONGRATULATIONS
leviathan7@leviathan:~$ cat CONGRATULATIONS 
Well Done, you seem to have used a *nix system before, now try something more serious.
```

We are done, we shall move on to the next challenge.


