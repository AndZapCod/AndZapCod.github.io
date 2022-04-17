---
layout: single
title: Starting Point Appointment Write-up
date: 2022-04-17
categories: [Hack The Box, CTF] 
tags: [Linux, SQL injection, ]
excerpt: "Hack The Box Starting point machine tier 1 Appointment my step-by-step solution"
toc: true
toc_sticky: true
sidebar:
    nav: "starting_point"
---

![logo](/assets/img/htb/logo-htb.svg){: width="80%"}![appointment-logo](/assets/img/htb/starting-point/appointment/a9ddcda8d2f6eb388c6717de2caff896.png){: width="20%"}

Hello everyone, in this post, I am going to share with you the step-by-step of how I solved the Appointment machine of the Starting point track of [Hack the Box](https://app.hackthebox.com/).

## Scanning

The first thing I did was to scan the target with [Nmap](https://nmap.org) looking for open services. In this case, we have a machine with an HTTP server open on port 80/TCP.

```bash
$ sudo nmap -Pn 10.129.226.41
Nmap scan report for 10.129.226.41
Host is up (0.38s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http
```

The next thing I tried was to get more information about the service on port 80/TCP. So I ran the following command.

```bash
$ sudo nmap -p80 -sV -sC 10.129.226.41
Nmap scan report for 10.129.226.41
Host is up (0.17s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Login
|_http-server-header: Apache/2.4.38 (Debian)
```

Now we know that the service has a login on port 80, the next thing we can do is to try to crack the login.

**Flags:**\\
`-p` Specify the port number that will be scanned in this case 80\\
`-sC` Will run the default scripts used by Nmap to detect possible weaknesses\\
`-sV` Will scan in order to get information about versions of the services
{: .notice }

## SQL injection on login

If we put the IP address of the victim in a browser we get the following:

![login](/assets/img/htb/starting-point/appointment/login.png){: .align-center}

The first thing I tried to do was a usual SQL injection `admin' OR 1=1;--` assuming that the admin user exists. But, it didn't work, so following [Hack the Box](https://app.hackthebox.com/)'s hints I changed the SQL comment to a `#`. Then I entered `admin' OR 1=1;#` in the user field and filled in anything else in the password field. The server responded with the flag.

![pwned](/assets/img/htb/starting-point/appointment/pwned.png){: .align-center}

Thanks for reading, I hope it was useful.