---
layout: single
title: Starting Point Dancing Write-up
date: 2022-02-28
categories: [Hack The Box, CTF]
tags: [Windows]
excerpt: "Hack The Box Starting point machine tier 0 Dancing my step-by-step solution"
toc: true
toc_sticky: true
sidebar:
    nav: "starting_point"
---

![logo](/assets/img/htb/logo-htb.svg){: width="80%"}![dancing-logo](/assets/img/htb/starting-point/dancing/ce52eadd09ff5a28a1eea8c65d6683a9.png){: width="20%"}

Hello everyone, in this post, I am going to share with you the step-by-step of how I solved the Dancing machine of the Starting point track of [Hack the Box](https://app.hackthebox.com/).

## Scanning

The first thing I did was to scan the target with [Nmap](https://nmap.org) looking for open services. In this case, we have a machine with an SMB server open on port 445/TCP. The other ports I understand support the SMB server on Windows machines.

```bash
$ sudo nmap -Pn 10.129.38.138
Starting Nmap 7.91 ( https://nmap.org ) at 2022-02-28 13:37 EST
Nmap scan report for 10.129.38.138
Host is up (0.24s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

The next thing I tried was to get more information on the ports. For this, I first used [Nmap](https://nmap.org) but did not get much information. So I decided to look for some tools to help me scan this service on the target.

```bash
$ sudo nmap -p135,139,445 -sC -sV 10.129.38.138
Starting Nmap 7.91 ( https://nmap.org ) at 2022-02-28 14:10 EST
Nmap scan report for 10.129.38.138
Host is up (0.16s latency).

PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 3h59m59s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-02-28T23:10:20
|_  start_date: N/A
```

The two tools I used were [nmbscan](http://nmbscan.g76r.eu/) and [nbtscan](https://github.com/resurrecting-open-source-projects/nbtscan). I ran default scans with these two tools but got nothing.

```bash
$ nmbscan -h 10.129.38.138                     
nmbscan version 1.2.6 - fedora - Mon Feb 28 02:17:23 PM EST 2022
domain -
  server -
    ip-address 10.129.38.138
$ nbtscan 10.129.38.138 
Doing NBT name scan for addresses from 10.129.38.138

IP address       NetBIOS Name     Server    User             MAC address      
------------------------------------------------------------------------------
```

Without any results, I decided to list the content of the share as [Hack the Box](https://app.hackthebox.com/) suggested. Then, I got the following output without providing a password:

```bash
$ smbclient -L 10.129.38.138
Password for [SAMBA\andzap]:

    Sharename       Type      Comment
    ---------       ----      -------
    ADMIN$          Disk      Remote Admin
    C$              Disk      Default share
    IPC$            IPC       Remote IPC
    WorkShares      Disk      
SMB1 disabled -- no workgroup available
```

With this, we can now try connecting to the WorkShares directory on the server.

## Connecting to the server

We know we don't need a password to enter the WorkShares directory so let's connect using [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html).

![flag](/assets/img/htb/starting-point/dancing/flag.png){: .align-center}

Searching in the server directories I found the flag, what is left is to get it using the `get` command and we will have it locally.

![pwned](/assets/img/htb/starting-point/dancing/pwned.png){: .align-center}

Personally, I haven't worked much with Windows machines, I thought it was a good introduction from Hack the Box this machine. Thanks for reading, I hope it was useful.