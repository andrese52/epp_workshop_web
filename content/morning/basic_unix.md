---
title: Unix overview
weight: 3
chapter: false
pre: "<b>2.3 </b>"
---

## Command-line interface

Forget about your mouse from now on. In the shell, only your keyboard will take you where you wanna be...

Use your arrow keys to move your cursor  :arrow_left: :arrow_up: :arrow_down: :arrow_right:

This is your shell prompt, and it looks like this whenever it's ready to accept your commands (input):

```bash
your_username_here $
``` 

When you have your prompt, you *communicate* with the computer by typing a command and hitting `enter` to run it.

In computer words, 'typing a command' is accepting input your keyboard (from you). This is called **standard input** (stdin).

When you run a command, the computer prints the output of that command to the screen. This is called **standard output** (stdout).  


### Standard Input 

Some commands are stand-alone, e.g. `date`, `ls`, and `cal`. This means that you don't have to necessarily input, because they have a default input.

```bash
$ date
```
	Wed Mar  7 09:39:26 CST 2018

> Try running `cal` and `ls`. What's the output?

The majority of the commands we'll use, e.g. `cat` and `less`, require that you input data. You can simply do this by redirecting stdin. To redirect stdin, you can use `<`. 

```bash
$ cat mgrisea_mat1_aa.txt
```
	>lcl|AB080670.2_prot_BAC65087.1_1_[gene=MAT1-1-1]_alpha_box
	MIASLSPDDIARLIPQETLTSLLRANDEKERLRELPVSPRAVAAASKNKKKVNGFMAFRSYYAGIFQDRPQKERSPFITLLWQKETLKSRWTLMANVFSRIRDFAGTTRGRMAMSGFLRVACPLLGITKPCDYLRRYNWELEFVADASAPYDAAMKYEISQSQIPHIVDEFEVPTTEIELLRACVQGGFPFENSAQLLRDMEDSSVTVMTRTAPIMAPSHASQASHGQHNHHFINTLINDPDAAISALLPQDEDIGSLMVDMNIIHSLETDSSTTSSARNSVSPLEQHLFFHEDVSIDPSTMVSFPGEGHGHPETQYSYPNPTLGLW

> We'll use `less` in just a moment...


### Standard output

The results or output of these commands are printed on the screen (stdout).

The stdout on the screen means that:

1. Output is not being stored anywhere in any form  
2. The command is not altering the original data  

{{% notice tip %}}
Hit the `tab` for autocompletion of file names and paths.  
Bash keeps history of your commands. Hit the :arrow_up: to scroll through.
{{% /notice %}}

{{% notice tip %}}
A tip disclaimer
{{% /notice %}}

### Basic commands

+ Who are you?

`whoami` prints your username?
```bash
$ whoaim
```
	nathalia

+ Where am I? 
`pwd` stands for print current working directory. It prints your current *location*, aka `path`.

```bash
$ pwd
```
	/home/nathalia 

> What kind of path is this?


+ Directory listing. `ls` lists files and directories in your current directory

```bash
$ ls
```

> This prints the listing to the screen. If empty, won't print anything.

`ls` is a command that has *options*. *Options* modify the default behavior of a command. In the case of `ls`, use options consisting of a dash and one or more characters such as `ls -l`


```bash
$ ls -l
```

> Did you notice a different in listing display?

* You: Natty, I love options. I wanna see all options! *

You can check out the manual page for all the options available for `ls` and any other command.
`man` opens up the manual page of any command as a kind of pop-up window.

```bash
$ man ls
```

	 LS(1)                            User Commands                           LS(1)

        NAME
        ls - list directory contents

        SYNOPSIS
        ls [OPTION]... [FILE]...

        DESCRIPTION
        List  information  about the FILEs (the current directory by default).  Sort entries alphabetically if none of
        -cftuvSUX nor --sort.

        Mandatory arguments to long options are mandatory for short options too.

	-a, --all
              do not ignore entries starting with .

        -A, --almost-all
              do not list implied . and ..

        --author
              with -l, print the author of each file

        -b, --escape
              print octal escapes for nongraphic characters

        --block-size=SIZE
              use SIZE-byte blocks.  See SIZE format below

	-B, --ignore-backups
              do not list implied entries ending with ~

        -c     with -lt: sort by, and show, ctime (time of last modification of file status information) with -l: show
              ctime and sort by name otherwise: sort by ctime
        (...)


* You: But, Nathy, what's your favorite flag?
Natty: I am so glad you asked!*

```bash
$ ls -lhtr'
```

> Test it out. Search the `man` pages, and figure out what each options does.



## **Any questions?**
