---
title: nmap intro
date: 2024-05-26
categories: [Tools, nmap]
tags: [nmap, scanning, intro, reconnaissance, enumeration]     # TAG names should always be lowercase
---

So you have a target IP and want to scan for open ports.
If you have no knowledge of nmap then I would suggest to go through its man pages (as you should for any cli tool), but in the meantime you can use this specific command to -hopefully- get some quick results.
`nmap target-IP`

This is the most basic of scans that nmap can do.
So what does this do?
It scans the target IP's top 1000 ports to see if any of them are responsive.  If they are, then they will be marked as open (a service is actively listening on this port), or filtered (the port is blocked, eg by a firewall).  The rest will be marked as closed.
An indicative output would be such as the one below, listing the number of each port, its status and its respective service:
```terminal
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-14 12:00 UTC
Nmap scan report for 10.10.145.33
Host is up (0.00012s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE    SERVICE
22/tcp  open     ssh
80/tcp  filtered http
443/tcp open     https
21/tcp  open     ftp

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds
```

---
All this is fine, but what if you want more details?
What if you want to know the version of each service?  The OS running on the target?  Or even potentially start collecting security misconfigurations?
Then add the following flags in your command: 
`nmap -T4 -A -p- target-IP`

- `-T4` defines the speed at which nmap runs the scan.  You can pick any option between `-T0` (slowest) to `-T5` "fastest".  But be aware about the fact that the faster the scan runs, the easier it gets for your scan to be detected.  
- `-A`, also known as "Aggressive" scan, is used to detect service versions, the underlying OS, runs scripts to gather additional info and performs a traceroute.  This option makes your activities very "loud", which in turn makes it easy for intrusion detection systems to notice your scanning the system.
- `-p-` means that you ask nmap to scan all (65535) ports rather than the top 1000 which are scanned by default.
The output that these flags can generate can become very complex, depending on the findings.  As mentioned earlier this is not only presenting the ports found, but also their versions and any other information that could be retrieved, the operating system of the target, and the results of the tests run by the nmap scripts.  

The example below could be the result of scanning a target:
```terminal  
Starting Nmap 7.91 ( https://nmap.org ) at 2023-10-04 13:22 UTC
Nmap scan report for 10.10.228.174 (10.10.228.174)
Host is up (0.076s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17  2020 note.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.14.13.255
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 726.05 seconds  
```

Before I close this I want to point out one more thing that is important to keep in mind.
Every time nmap is called to gather information about a host (or a set of), before it performs the port scan it starts a process of gathering information.  One of theses steps is pinging the target hosts to assess whether they are reachable in the network.   This is done by default.  
There is a caveat here.  If the network or the target's defences are set up so as not to respond to ICMP requests, then nmap sees no target to scan.  You see how this can be a problem, right?
Thankfully, there is a way to force nmap to treat every target as a live host:
`nmap -Pn target-IP`
- `-Pn` is used to tell nmap to skip the host discovery phase and assume that the target host is online.  Even if ICMP requests are blocked, nmap will still run its scans on the target

I will be making another post elaborating on other available nmap options that might come in handy in your enumeration processes, so come back for more.
