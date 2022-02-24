---
layout: single
title: Starting Point Fawn Write-up
date: 2022-02-24
categories: [Hack The Box, CTF]
tags: [Linux, Account Misconfiguration, FTP]
excerpt: "Hack The Box Starting point machine tier 0 Fawn my step-by-step solution"
toc: true
toc_sticky: true
sidebar:
    nav: "starting_point"
---

![logo](/assets/img/htb/logo-htb.svg){: width="80%"}![meow-logo](/assets/img/htb/starting-point/fawn/b64f85071e626e4cc2272d54332e4131.png){: width="20%"}

Today I will share with you my personal solution for the Fawn machine. This machine belongs to Starting Point track of [Hack the box](https://app.hackthebox.com/).

## Scanning

To begin with, I'm going to run [Nmap](https://nmap.org/) searching for open ports using the following command. In my case, the IP address of the target is `10.129.234.46`.

```bash
$ sudo nmap -Pn 10.129.234.46
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2022-02-24 10:46 EST
Nmap scan report for 10.129.234.46
Host is up (0.29s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
21/tcp open  ftp

Nmap done: 1 IP address (1 host up) scanned in 2.81 seconds
```

Now we know that this target has an FTP service, let’s get more information, for this, I will use two flags that allow  Nmap to get information about the version and run some scripts searching for weaknesses on the target port.

```bash
$ sudo nmap -p21 -sC -sV 10.129.234.46
Starting Nmap 7.91 ( https://nmap.org ) at 2022-02-24 10:50 EST
Nmap scan report for 10.129.234.46
Host is up (0.13s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.193
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
Service Info: OS: Unix
```

**Flags:**\\
`-p` Specify the port number that will be scanned in this case 23\\
`-sC` Will run the default scripts used by Nmap to detect possible weaknesses\\
`-sV` Will scan in order to get information about versions of the services
{: .notice }

With this output, we get important information. Now we know that the target allows the Anonymous login without a password and the server stores a file named `flag.txt` and we have permission to read it.

## Connecting to the server

So, the next step would be to connect to the FTP server. For this, I used [FileZilla](https://filezilla-project.org/). I just specified the Host as the IP of my target, the username as Anonymous, as we saw in the scanning phase, and I left empty the password field.

![flag](/assets/img/htb/starting-point/fawn/flag.png){: .align-center}

**Anonymous** is the default user of an FTP server and with default settings, this user doesn't have a password. This is considered Account Misconfiguration.
{: .notice}

Once connected with the server you only need to move the `flag.txt` file to your system directory and read it.

![pwned](/assets/img/htb/starting-point/fawn/pwned.jpeg){: .align-center}

Thanks for the read, I hope it was useful. I'm starting my journal on cybersecurity and [Hack the box](https://app.hackthebox.com/) looks like a great start.
