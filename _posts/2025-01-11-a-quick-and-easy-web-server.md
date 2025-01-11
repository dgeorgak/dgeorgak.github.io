---
title: A quick and easy web server
date: 2025-01-11
categories: [Tools, webserver]
tags: [ctf, pentest, intro, web-server]     # TAG names should always be lowercase
---

There are many reasons why a pentester would want to quickly fire up a web server.  
Imagine having gained access to a machine, and now you need to upload a payload or a script.  You could try messing around with file transfer protocols.  But it would be much easier to just use a one-liner and spin up a quick HTTP server.  
The reverse scenario is also true.  Sometimes, you’ve got to exfiltrate files.  Maybe it’s sensitive data or just something that proves you’ve achieved your objective.  Hosting a server makes this super easy.  You just grab what you need, pull it over HTTP, and you’re done.  Simple and clean.  
Another scenario can be a staging environment.  Ever needed to have a fake site up and running for phishing purposes?  A quick server makes that painless.  You can set it up right there, test how things look, and tweak it if needed.  It’s way faster than setting up a full-blown environment.  
Or maybe it’s just about testing stuff.  Having your own web server running lets you try things out without needing to rely on anything external.  Plus, most of these servers log requests for you, so you can see exactly what’s happening.

So let your imagination free and set up an easy web server for your own purposes!  Fire up a python `http.server` and you're good to...wait, what?...no python!?
Fret not!  There are many ways to have a web server running depending on the programming languages available on your local (or remote) host.

Here's a few of them:
- Python:  `python -m http.server 8000`
- PHP:`php -S 127.0.0.1:8000`
- Node.js:  `npx http-server ./ --port 8000`
- Rust:  `cargo install miniserve && miniserve -p 8000 .`
- Ruby:  `ruby -run -e httpd ./ -p 8000`
- BusyBox:  `busybox httpd -f -p 8000`
- Caddy:  `caddy file-server`
- R:  `Rscript -e 'servr::httd()' -p 8000`
