---
layout: single
title: Starting Point Meow Write-up
date: 2022-02-23
categories: [Hack The Box, CTF] 
tags: [Linux, Account Misconfiguration, SSH]
excerpt: "Hack The Box Starting point machine tier 0 Meow my step-by-step solution"
toc: true
toc_sticky: true
sidebar:
    nav: "starting_point"
---

![logo](/assets/img/htb/logo-htb.svg){: width="80%"}![meow-logo](/assets/img/htb/starting-point/meow/61b5837dfdfe1fb1ca3750cf2712da44.png){: width="20%"}

## Scanning

The first thing we need to check is our connection with the target. To do this we can use `ping`. To use this command we also need the IP address of the target. [Hack The Box](https://app.hackthebox.com/) already provides the IP. In my case, the IP of my target is `10.129.50.2`. So the command and the output should look like follows:

```bash
$ ping -c 2 10.129.50.2            
PING 10.129.50.2 (10.129.50.2) 56(84) bytes of data.
64 bytes from 10.129.50.2: icmp_seq=1 ttl=63 time=76.0 ms
64 bytes from 10.129.50.2: icmp_seq=2 ttl=63 time=77.4 ms

--- 10.129.50.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 75.977/76.670/77.363/0.693 ms
```

The flag `-c` is used to fix the number of ECHO_REQUESTS
{: .notice }

Now we can start with the port scanning. To do this, I will use [Nmap](https://nmap.org/). First, I'm going to explore the service that our target has open.

```bash
$ sudo nmap -Pn 10.129.50.2
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2022-02-23 16:40 EST
Nmap scan report for 10.129.50.2
Host is up (0.46s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 3.52 seconds
```

In this case, I disabled the host discovery using the `-Pn` flag and we can see that the port 23/TCP is open with the telnet service. If we want to get more information about the service version and the host, we can try to get it using the following command.

```bash
$ sudo nmap -p23 -sC -sV 10.129.50.2
Starting Nmap 7.91 ( https://nmap.org ) at 2022-02-23 16:57 EST
Nmap scan report for 10.129.50.2
Host is up (0.085s latency).

PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.71 seconds
```

**Flags:**\\
`-p` Specify the port number that will be scanned in this case 23\\
`-sC` Will run the default scripts used by Nmap to detect possible weaknesses\\
`-sV` Will scan in order to get information about versions of the services
{: .notice }

## Get Access

Now we can try to connect with the telnet server of the target.

![login](/assets/img/htb/starting-point/meow/login.png){: .align-center}

As we saw in the scanning step we have a Linux machine, so what happens if we try login as root?

![root](/assets/img/htb/starting-point/meow/flag.png){: .align-center}

We don't need a password this target has an Account Misconfiguration and we use it to get the flag.

![pwned](/assets/img/htb/starting-point/meow/pwned.jpeg){: .align-center}

Thanks for the read, I hope it was be usefull. I'm starting my journal on cybersecurity and [Hack the box](https://app.hackthebox.com/) looks like a great start.
