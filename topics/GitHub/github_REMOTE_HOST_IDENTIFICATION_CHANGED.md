---
parent: GitHub
grand_parent: Topics
layout: default
title: "github: REMOTE HOST IDENTIFICATION CHANGED"
description:  "The scary message and what to do about it"
indent: true
---

If you've used ssh to connect to GitHub in the past (specifically, before 05:00 UTC on March 24, 2023)
then the first time you connect to GitHub using ssh after 05:00 UTC on March 24, 2023 (e.g. doing a `git clone`, `git pull` or
`git push` operation), you'll get this scary message:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:uNiVztksCsDhcc0u9e8BujQXVUpKZIDTMczCvj3tD2s.
Please contact your system administrator.
Add correct host key in /Users/pconrad/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/pconrad/.ssh/known_hosts:4
Host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

The rest of this page explains why this is happening, and what to do to fix it.

# Why is this happening?

Basically, someone at GitHub goofed, and they had to update their 

# How to fix it

On the system where you are trying to use GitHub (be that CSIL, or your own machine), you need to locate a file called `known_hosts`.

The error message will tell you exactly where this file is.   For example:
Add correct host key in /Users/pconrad/.ssh/known_hosts to get rid of this message.

All you need to do is delete the line that starts with `github.com`, and leave the rest of the file alone.

That should fix it, though the first time you connect to github after this, you'll get this dialog, to which you need to answer `yes`:

```
The authenticity of host 'github.com (192.30.255.112)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

This is the dialog that recreates the correct line to put in the `known_hosts` file.

# A few more little security details

Should we have just said `yes` to that dialog?  Well, it depends.

There are a few circumstances when you might have said `no` or `fingerprint`:

* If you were truly paranoid
* If you had actual reason to suspect that you or GitHub were under cyberattack
* If you were working on an application with very high security needs (e.g. nuclear weapons code)

Under those circumstances, you'd get the fingerprint for the GitHub public key through some means other than an internet connection (e.g. secure courier) and you'd verify it against the number shown there 

Typically, however, we just trust that the connection is not under active attack, and we accept the number as valid.  

Then, if it later changes—as it did in this case—we verify that it was supposed to change (as we did, by reading the GitHub blog.)  If it wasn't supposed to change, then we might really believe that we were being subjected to a so-called "man-in-the-middle" attack.
