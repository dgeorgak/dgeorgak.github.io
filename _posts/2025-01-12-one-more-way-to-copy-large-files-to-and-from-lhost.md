---
title: One more way to copy large files to and from lhost
date: 2025-01-12
categories: [Tips, CTF]
tags: [ctf, pentest, copy_file]     # TAG names should always be lowercase
---

Lets say that you found a large .bak file in the rhost and you want to copy it to your lhost for investigation purposes.  
Or you need to copy a script to the rhost and fire it up for your exploit to take effect.
And in both of these cases there are no tools present that might prove useful to do such a thing.
The solution provided in this post will work both directions, and it works by encoding said file(s) as base64.
For this scenario we will copy `/etc/passwd` of our target to our own lhost.
1. In the rhost run `base64 /etc/passwd > b64_passwd`
	This will encode `/etc/passwd` and pass the encoded text to the file `b64_passwd`
2. Open `b64_passwd` in a text editor (eg nano) and copy the whole text
3. On your lhost paste the copied text to a new file `b64_passwd2`
4. Then run `base64 -d b64_passwd2 > obtained_passwd`
	This command will decode the contents of the `b64_passwd2` file and pass the resulting text to `obtained_passwd`
5. Now you have the contents of rhost's `/etc/passwd` file on your own `obtained_passwd` file

For the next scenario we will copy `linpeas.sh` from our lhost to the rhost.  This could be done manually by following the instructions above, but this time encoding our local file, copying its contents over to the rhost, and encoding there.  
The problem in this case is that `linpeas.sh` will result in a very long `base64` text which will make it difficult and error prone to paste it piece by piece.  To solve this, we will automate the part of copying the encoded text to our clipboard, so that we can carry it over to the rhost by pasting only once.
For this reason we will use `xclip`.  If not already present, you can install it by running `sudo apt install xclip` (or the equivalent of your distro).
The steps for this case are the following:
1. Run `base64 /opt/linpeas/linpeas.sh | xclip -selection clipboard`
	This command will encode `linpeas.sh`, and pipe it to our clipboard
2. On rhost paste to a new file (eg `b64_lp`)
3. Now run `base64 -d b64_lp > linpeas.sh`
4. `linpeas.sh` is now available in rhost
