---
layout: post
title:  "Linux Admin"
date: 2018-01-01 23:57:23 -0400
categories: articles
---

# Good system administrators write scripts. 

```bash
which 
whereis #it searches a broader range of system directories and is independent of your shell’s search path.
locate 
```

Cheak all environment variables:
```hash
printenv
```

```bash
wget
```
very process has at least three communication channels available to it: “standard input” (STDIN), “standard output” (STDOUT), and “standard error” (STDERR). STDIN normally reads from the keyboard and both STDOUT and STDERR write their output to the screen. Most commands accept their input from STDIN and write their output to STDOUT. They write error messages to STDERR. This convention lets you string commands together like building blocks to create composite pipelines.

A < symbol connects the command’s STDIN to the contents of an existing file. The > and >> symbols redirect STD- OUT; > replaces the file’s existing contents, and >> appends to them.

Example:
```sh
echo "This is a test message." > /tmp/mymessage
```
Use the | symbol, commonly known as a pipe. Some examples:
```sh
ps -ef | grep httpd
cut -d: -f7 < /etc/passwd | sort -u
```

To execute a second command only if its precursor completes successfully, you can separate the commands with an && symbol. For example,
```sh
lpr /tmp/t2 && rm /tmp/t2
```

Conversely, the || symbol executes the following command only if the preceding command fails (produces a nonzero exit status).
In a script, you can use a backslash to break a command onto multiple lines, help- ing to distinguish the error-handling code from the rest of the command pipeline:
cp --preserve --recursive /etc/* /spare/backup \ || echo "Did NOT make backup"

**Do not put spaces around the = symbol or the shell will mistake your variable
name for a command name.**

```sh
$ etcdir='/etc' 
$ echo $etcdir
```

When referencing a variable, you can surround its name with curly braces to clar- ify to the parser and to human readers where the variable name stops and other text begins; for example, ${etcdir} instead of just $etcdir. The braces are not nor- mally required, but they can be useful when you want to expand variables inside double-quoted strings. Often, you’ll want the contents of a variable to be followed by literal letters or punctuation. For example,
```sh
$ echo "Saved ${rev}th version of mdadm.conf." Saved 8th version of mdadm.conf.
```

first executes boot code that is stored in ROM.
