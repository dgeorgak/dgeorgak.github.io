---
title: find cheatsheet
date: 2025-02-05
categories: [Tools, find]
tags: [find, pentest, cheatsheet, ctf]     # TAG names should always be lowercase
---

"`find`".  
An ever present, and very essential tool in the cli.  
With `find` one can... well... find files and directories in the system they are in.
Sometimes the command seems a bit intimidating in its use though.  So rather than learning its ins and outs by heart, why not make a cheatsheet for it?

So here are 
## 5 basic and useful `find` commands
for your everyday basic cli needs.
#### 1. Find a file by name:
Want to find a file but don’t remember where you left it? This command will hunt it down.
`find /this/is/the/path/I/want/to/search/in -name "filename"`
#### 2. Find a directory by its name:
Looking for a specific directory? The command is very similar to the one above, but you need to specify the `-type d` option.
`find /this/is/the/path/I/want/to/search/in -type d -name "dirname"`
#### 3. Find files by extension:
Need all `.txt` files in a directory? This will grab them all for you.
`find /this/is/the/path/I/want/to/search/in -type f -name "*.txt"`
#### 4. Find files modified in the last *x* days:
Are you looking for a file that you are sure you modified during the last week?
`find /this/is/the/path/I/want/to/search/in -type f -mtime -7`
#### 5. Find and delete specific files:
Need to clean up all `.log` files in a directory? No need to list them one by one.
`find /this/is/the/path/I/want/to/search/in -type f -name "*.log" -delete`

How about we spice it up a bit now, with
## 10 advanced `find` commands
that can prove useful for the everyday pentester or CTF aficionado?
#### 1. Find world writable files:
Finds files that anyone can write to.  If an attacker can modify these, they might be able to escalate privileges.
`find / -type f -perm -o+w 2>/dev/null`
#### 2. Find files owned by a specific user
Need to see what files a specific user owns? This will list them all.
`find / -type f -user targetuser 2>/dev/null`
#### 3. Find SUID binaries
SUID binaries run as their owner.  And sometimes the owner can be root.  If misconfigured, they could be exploited for privilege escalation.
`find / -perm -4000 -type f 2>/dev/null`
#### 4. Find files with a specific group ownership
Curious about what files a specific group has access to? You can list them all with the following command.
`find / -type f -group targetgroup 2>/dev/null`
#### 5. Find writeable directories
Checks for directories that anyone can write to.  This can lead to unearthing potential places for attackers to drop malicious scripts.
`find / -type d -perm -o+w 2>/dev/null`
#### 6. Find all SSH private keys
Misplaced SSH keys are always an interesting finding.  This command looks for private keys lying around the system.
`find / -type f -name "id_rsa" 2>/dev/null`
#### 7. Find readable sensitive files
The possibility of the average user leaving files that might contain secrets, credentials, API keys, or internal configurations lying around is quite high.
`find / -type f \( -name "*.conf" -o -name "*.ini" -o -name "*.yaml" \) -readable 2>/dev/null`
#### 8. Find files modified in the last hour
Because having access to another user's history file is not always possible.
`find / -type f -mmin -60 2>/dev/null`
#### 9. Find credentials inside files
Search `.log` files for the word `password`.  Modify extension and required string accordingly.
`find / -type f -name "*.log" -exec grep -i "password" {} + 2>/dev/null`
#### 10. Find backup and db files
Backup and database files often contain sensitive data.
`find / -type f \( -name "*.bak" -o -name "*.sql" -o -name "*.db" \) 2>/dev/null`

So there you have it.
A cheatsheet for you to use in order to find files, directories, and useful data for your attack path.
Because the `locate` command might seem easier, but is not as tweakable (more about that on another post?)
