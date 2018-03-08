---
title: Unix overview
weight: 3
chapter: false
pre: "<b>2.3 </b>"
---

## The Cowboy is a command-line interface

> Forget about your mouse from now on. In the shell, only your keyboard will take you where you wanna be...

This is your shell prompt, and it looks like this whenever it's ready to accept your commands (input):

```bash
your_username_here $
``` 

When you have your prompt, you *communicate* with the computer by typing a command and hitting `enter` to run it.

In computer words, 'typing a command' is accepting input your keyboard (from you). This is called **standard input** (stdin).

When you run a command, the computer prints the output of that command to the screen. This is called **standard output** (stdout).  


### Standard Input 

Some commands are stand-alone, e.g. `date`, `ls`, and `cal`, which means that you don't necessarily have to input anything, because they have a default input.

```bash
$ date
```
	Wed Mar  7 09:39:26 CST 2018

> Try running `cal` and `ls`. What's the output?

The majority of the commands we'll use, e.g. `cat` and `less`, require some input. You can simply do this by redirecting stdin. To redirect stdin, you can use `<`. 

```bash
$ cat < mgrisea_mat1_aa.txt
```
	>lcl|AB080670.2_prot_BAC65087.1_1_[gene=MAT1-1-1]_alpha_box
	MIASLSPDDIARLIPQETLTSLLRANDEKERLRELPVSPRAVAAASKNKKKVNGFMAFRSYYAGIFQDRPQKERSPFITLLWQKETLKSRWTLMANVFSRIRDFAGTTRGRMAMSGFLRVACPLLGITKPCDYLRRYNWELEFVADASAPYDAAMKYEISQSQIPHIVDEFEVPTTEIELLRACVQGGFPFENSAQLLRDMEDSSVTVMTRTAPIMAPSHASQASHGQHNHHFINTLINDPDAAISALLPQDEDIGSLMVDMNIIHSLETDSSTTSSARNSVSPLEQHLFFHEDVSIDPSTMVSFPGEGHGHPETQYSYPNPTLGLW

or, you can simply omit `>`

```bash
$ cat mgrisea_mat1_aa.txt
```
        >lcl|AB080670.2_prot_BAC65087.1_1_[gene=MAT1-1-1]_alpha_box
        MIASLSPDDIARLIPQETLTSLLRANDEKERLRELPVSPRAVAAASKNKKKVNGFMAFRSYYAGIFQDRPQKERSPFITLLWQKETLKSRWTLMANVFSRIRDFAGTTRGRMAMSGFLRVACPLLGITKPCDYLRRYNWELEFVDASAPYDAAMKYEISQSQIPHIVDEFEVPTTEIELLRACVQGGFPFENSAQLLRDMEDSSVTVMTRTAPIMAPSHASQASHGQHNHHFINTLINDPDAAISALLPQDEDIGSLMVDMNIIHSLETDSSTTSSARNSVSPLEQHLFFHEDVSIDPSTMVSFPGEGHGHPETQYSYPNPTLGLW

> We'll use `less` in just a moment...

### Standard output

The results or output of these commands are printed on the screen (stdout).

The stdout on the screen means that:

1. Output is not being stored anywhere in any form  
2. The command is not altering the original data  


{{% notice tip %}}
Hit the `tab` for autocompletion of file names and paths.  
Bash keeps history of your commands. Hit the up-arrow key to scroll through.
{{% /notice %}}

### Basic commands

+ Who are you? `whoami` prints your username.  
```bash
$ whoaim
```
		nathalia

+ Where am I? `pwd` stands for print current working directory. It prints your current *location*, aka `path`.

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

`ls` is a command that has *options*, which modify the default behavior of a command. In the case of `ls`, options consist of a dash and one or more characters such as `ls -l`

```bash
$ ls -l
```

> Did you notice a different in listing display?

*You: Natty, I love options. I wanna see all options for ls!
Natty: You can check out the manual page for all the options available!*

For any command, `man` opens up the manual page of any command as a kind of pop-up window.

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


Hit `q` to exit and return to the command-line.

>*You: But, Nathy, what's your favorite flag?*  
>*Natty: I am so glad you asked!*

```bash
$ ls -lhtr
```

> Test it out. Search the `man` pages, and figure out what each options does.


+ Creating a directory. `mkdir` stands for make a directory, and it requires input directory name. It should be a new name, never duplicate names. 
Let's create a directory for our workshop.

```bash
$ mkdir workshop_mar17
```

> Hit `ls`. What do you see?

We created the directory, however we are not inside yet.

+ Change directory. `cd` is the command to change directories. This commands requires input directory name that should be immediately *where you are* (aka, your working directory). 

```bash
$ cd workshop_mar17 
```

> Hit `pwd`. What do you see?

Let's say we wanted to go back to our `/home`. There are multiple ways to do that:

Option 1:
```bash
$ cd ~
```

Option 2:
```bash
$ cd 
```

Option 3:
```bash
$ cd /home/*your_username_here*
```

With `cd` we can move across different levels of directory hierarchy if you input a `path`.

The figure below is a representation of a file system. Let's pause for 2 minutes to check this out.

>INSERT FILE SYSTEM FIGURE

Your prompt (aka, where we currently are) is in `/home`. Now, try to answer these questions quickly without coding:  
     - What's the command to change directory to `aa_sequences/`?  
     - What's the command to change directory to `cowboy_scripts/`?  
     - What's the command to change directory to `annotation/`?  

Now, your prompt is in `/annotation`. Try to answer these questions quickly without coding:  
     - What do you see inside this directory?  
     - What's the command to change directory to `workshop_mar17/`?  


Use these `cd` shortcuts:  
`./` (dot) represents the current directory.  
`../` (two dots, no spaces) represents the parent directory.  


Going back to where we were... `/home`

Let's go back inside `workshop_mar17/`. 

```bash
$ cd workshop_mar17
```

+ Something you'll use a lot is a **text editor** to create files and scripts. `nano` is a very basic text editor, and very easy to learn.

Let's create a fasta file named `example_sequence.fasta`:

{{% notice notice %}}
Notice the extension of the file: `.fasta`. Fasta file is just a text file that contains nucleotide or amino acid sequences. We could name this file `example_sequence.txt` that it would be the same. The computer doesn't know `.fasta` extension, it *thinks* this is just a regular text file. Fasta file and many other file extensions used in bioinformatics were created by humans to humans.  
Tell me what are the two most important components of a `.fasta` file?
{{% /notice %}}

```bash
$ nano example_sequence.fasta
```

	  GNU nano 2.0.6                              File: example_sequence.fasta











                                             [ New File ]
         ^G Get Help    ^O WriteOut    ^R Read File     ^Y Prev Page     ^K Cut Text        ^C Cur Pos
         ^X Exit        ^J Justify     ^W Where Is      ^V Next Page     ^U UnCut Text      ^T To Spell



When you hit `enter`, the text editor appears on your screen. Notice the file name on the top. And on the bottom of the screen, notice keyboard shortcuts... remember that your mouse isn't gonna work here.

Let's create hypothetical nucleotide sequences.

```bash
>hypothetical_nucleotide_1
ATCTGATCGATCGATCGATATCTTTTTTAGCTAGG
>hypothetical_nucleotide_2
ACTAGCTAGCTATTACGGGGGGCTAGCTAGCTAGCGGGATCGATTTA
>hypothetical_nucleotide_3
ATCGATCGATCGAAAAAATCGATTTTCGATCGATCGATCGA
```

I just finish typing the last nucleotide sequence... I don't really like these file names anymore, I wanna change them.*

Use your arrow-keys to change the name of the files:

```bash
>nucleotide_sequence_1
ATCTGATCGATCGATCGATATCTTTTTTAGCTAGG
>nucleotide_sequence_2
ACTAGCTAGCTATTACGGGGGGCTAGCTAGCTAGCGGGATCGATTTA
>nucleotide_sequence_3
ATCGATCGATCGAAAAAATCGATTTTCGATCGATCGATCGA
```

Great! Looks much better.

To exit `nano` and save the file, hit `crtl+X`. Notice that a new message appears on the bottom of the screen:


   	       Save modified buffer (ANSWERING "No" WILL DESTROY CHANGES) ?
	       Y Yes
	       N No           ^C Cancel


Because we just created this file (made changes to it), `nano` is asking if we want to save what we just typed. If all looks good, hit `Y`. 

Another new message appears:

    	 File Name to Write: example_sequence.fasta                                                                                                   
	 ^G Get Help                        ^T To Files                        M-M Mac Format                     M-P Prepend
	 ^C Cancel                          M-D DOS Format                     M-A Append                         M-B Backup File


`nano` wants to make sure you want to save what you typed as the file name you provided. If all looks good, hit `enter` and you are back in the command-line.

+ To delete a file you don't want anymore use `rm`, which stands for remove.

{{% notice warning %}}
A word of caution here: when you remove a file from the command-line, the file is removed forever and there is no way to recover it.
With `rm` **THERE IS NO GOING BACK!** 
{{% /notice %}}

```bash
$ rm example_sequence.fasta
```

```bash
$ ls
```

Yup... it's gone!


## **How's that for your first exposure to Cowboy? Any questions?**
