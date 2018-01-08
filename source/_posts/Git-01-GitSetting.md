---
title: Git-01-GitSetting
date: 2017-12-19 00:29:47
categories: Git
tags:
- Git
---

# Git Installation and Setting

## Git installation

For Windows:

1. Download Git-scm from https://git-scm.com/downloads
2. Install
3. Open `Git bash` from Windows start menu

If a command line window pops up, the installation is success.

## Set user name & email

```
$ git config --global user.name "Your Name"
$ git config --global user.email "Your Email@example.com"
```

## Add SSH key to your server

1. Open Git Bash

2. Check if existing SSH keys are present:

   ```
   # Lists the files in your .ssh directory, if they exist
   cd ~/.ssh
   ls
   # or use ls ~/.ssh
   ```

   If public and private key pair like `id_rsa  id_rsa.pub` exist, 

   you already have a SSH key.

   `id_rsa` is your private key, and `id_rsa.pub` is your public key. If you are using Mac OS, you can find them at `home/.ssh/`. If you are using Windows, they are at `C:\Documents and Settings\your-username.ssh\` or  `C:\Users\your-username.ssh`.

3. Generate SSH key

   If SSH keys do not exist, generate keys by:

   ```
   # -t: key type
   # -C: Your Email
   ssh-keygen -t rsa -C "Your Email@gmail.com"
   ```

   If you don't want to set password, press SPACE for 3 times.

4. Add your SSH key to the ssh-agent

   - show your public key:

     ```
     cat ~/.ssh/id_rsa.pub
     ```

     You will get the contents of your public key:

     > ssh-rsa AAAB3nZaC1aycAAEU+/ZdulUJoeuchOUU02/j18L7fo+ltQ0f322+Au/9yy9oaABBRCrHN/yo88BC0AB3nZaC1aycAAEU+/ZdulUJoeuchOUU02/j18L7fo+ltQ0f322AB3nZaC1aycAAEU+/ZdulUJoeuchOUU02/j18L7fo+ltQ0f322AB3nZaC1aycAAEU+/ZdulUJoeuchOUU02/j18L7fo+ltQ0f322AB3nZaC1aycAAEU+/ZdulUJoeuchOUU02/j18L7fo+ltQ0f322klCi0/aEBBc02N+JJP john@example.com

   - give your public key to Server Administrator or upload it to your Github Server.

     To copy public key quickly:

     ```
     # on Mac
     pbcopy < ~/.ssh/id_rsa.pub
     # on Windows
     clip < ~/.ssh/id_rsa.pub
     ```

     Open your Github, and go to `Settings -> SSH and GPG keys -> SSH keys -> New SSH key`

     Select a `Title`, and paste your public key to `Key` area.

   - Verify your setting:

     ```
     # Attempts to ssh to Github
     ssh -T git@github.com
     ```

     If you get:

     > ```
     > Hi username! You've successfully authenticated, but GitHub does not # provide shell access.
     > ```

     it means your setting is OK.

## Change communication protocol

To change protocol from `https` to `ssh`:

1. show present setting

   ```
   git remote -v
   ```

   If you get:

   > origin https://github.com/someaccount/someproject.git (fetch)
   >
   > origin https://github.com/someaccount/someproject.git (push)

   you are using `hhttps`.

2. sign in to your Github account to `ssh` url.

   > git@github.com:someaccount/someproject.git

   Then use:

   ```
   git remote set-url origin git@github.com:someaccount/someproject.git
   ```

   to change the setting.

   At last use`git remote -v` to confirm your new setting.