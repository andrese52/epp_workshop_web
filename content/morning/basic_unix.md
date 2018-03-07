---
title: Morning Session
weight: 10
chapter: false
pre: "<b>2.3 </b>"
---

## Command-line interface

Forget about your mouse from now on. In the shell, only your keyboard will take you where you wanna be...

Use your arrow keys to move your cursor  :arrow_left: :arrow_up: :arrow_down: :arrow_right:

This is your shell prompt, and it looks like this whenever it's ready to accept your commands (input):

``bash
*your_username_here* $

When you have your prompt, you *communicate* with the computer by typing a command and hitting `enter` to run it.

In computer words, 'typing a command' is accepting input your keyboard (from you). This is called **standard input** (stdin).

When you run a command, the computer prints the output of that command to the screen. This is called ** standard output** (stdout).  


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
	\>lcl|AB080670.2_prot_BAC65087.1_1_[gene=MAT1-1-1]_alpha_box
	 MIASLSPDDIARLIPQETLTSLLRANDEKERLRELPVSPRAVAAASKNKKKVNGFMAFRSYYAGIFQDRPQKERSPFITLLWQKETLKSRWTLMANVFSRIRDFAGTTRGRMAMSGFLRVACPLLGITKPCDYLRRYNWELEFVADASAPYDAAMKYEISQSQIPHIVDEFEVPTTEIELLRACVQGGFPFENSAQLLRDMEDSSVTVMTRTAPIMAPSHASQASHGQHNHHFINTLINDPDAAISALLPQDEDIGSLMVDMNIIHSLETDSSTTSSARNSVSPLEQHLFFHEDVSIDPSTMVSFPGEGHGHPETQYSYPNPTLGLW


### Standard output

The results or output of these commands are  printed on the screen (stdout).

The stdout on the screen means that:

+ Output is not being stored anywhere in any form  
+ The command is not altering the original data  


Hit the `tab` for autocompletion of file names and paths.






## Basic commands

+ Who are you?

`whoami` prints your username?
```bash
$ whoaim
```
	nathalia

+ Where am I? 
`pwd` stands for print working directory. It prints where is your current location, aka `path`?

```bash
$ pwd
```
	/home/nathalia 

**What kind of path is this?**



## **Any questions?**
