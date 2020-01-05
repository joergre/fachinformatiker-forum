---
layout: page
title: LF6 Bash 
---

# Grundlagen zur Bash

Die Datei .bash_profile im Homeverzeichnis wird immer aufgerufen, wenn sich ein User neu anmeldet. Es gibt eine .bash_profile pro Benutzer. Bei einigen Systemen heißt sie auch einfach .profile.

```bash
ls -al
cat .bash_profile
# ~/.profile: executed by the command interpreter for login shells.
# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
# exists.
# see /usr/share/doc/bash/examples/startup-files for examples.
# the files are located in the bash-doc package.

# the default umask is set in /etc/profile; for setting the umask
# for ssh logins, install and configure the libpam-umask package.
#umask 022

# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
        . "$HOME/.bashrc"
    fi
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
```
In der ersten Zeile wird die Datei .bashrc aus dem Homeverzeichnis geladen. Im unteren Block wird ein privates bin-Verzeichnis für private Befehle der PATH-Variable hinzugefügt.

 
# .bashrc

Wir schauen uns die .bashrc-Datei an. Dann ersetzen wir den Inhalt durch einen neuen:

```bashrc
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi
```

Wir führen die Bash neu aus:

```bash
bash
```





