---
title: My first bash script
weight: 4
chapter: false
pre: "<b>3.4 </b>"
---
### Building our first script
The native language of Linux for its terminal is `bash` and we can write  scripts and create a small program that does all the above for us with  single line in bash. First, we need to create a file with `nano` which  is a text editor that is also installed in Linux by default. There are  other text editors like `vim`. Using `nano` we can create a `.txt` like 
so: 

First let's move to our `afternoon` folder and create a folder called `scripts` and `cd` to `scripts`

```bash
$ cd /scratch/asecas86/afternoon
$ mkdir scripts
$ cd scripts
```

Then let's type the following: 
You already learned how to use `nano`. Let's practice. 


```bash
$ nano myfirstscript.txt 
```

	[asecas86@n252 scripts]$  nano myfirstscript.txt
	  GNU nano 2.0.9                                     File: myfirstscript.txt                                                                        Modified

	Andres Espindola













																			 [ New File ]
	^G Get Help               ^O WriteOut               ^R Read File              ^Y Prev Page              ^K Cut Text               ^C Cur Pos
	^X Exit                   ^J Justify                ^W Where Is               ^V Next Page              ^U UnCut Text             ^T To Spell

Hit `Ctrl+X` then hit `Y`


### Important tips about using `nano`
- Your mouse will NOT work at all in the terminal, so please do not try  to click in different parts of the `nano editor` to edit the document.
 
 {{< figure src="https://media.giphy.com/media/pFwRzOLfuGHok/giphy.gif" 
title="" >}}

 - Use your keyboard arrows to move around the document  instead.
 
 {{< figure src="https://media.giphy.com/media/FdUILv11pQ15S/giphy.gif" 
title="" >}}
 
Now that you remembered how to use `nano`. We can create your first script.
 
First let's remove that file that you recently created. 

```bash 
$ rm myfirstscript.txt 
```

Any script in Linux should start with [**shebang:"#!"**](https://en.wikipedia.org/wiki/Shebang_(Unix)). Start another `nano` document: 


```bash 
$ nano myfirstscript.sh 
``` 
Type the following in the editor:

	#!/bin/bash
	# declare STRING variable
	STRING="Pistol Pete"
	#print variable on a screen
	echo $STRING

	
Everything of what you are writing in your first bash script has been already covered during the [morning session](../../morning). Save the file and now we need to make it executable.

```bash 
$ chmod +x myfirstscript.sh
```

Now you can execute your first script 


```bash
$ ./myfirstscript.sh 
```

Do you want to try something more difficult: YASSSS 

### Building a script that contains `arguments`

Many scripts are created to facilitate the user typing the same code over and over again. Most of the scripts have inputs and outputs. In our previous script we did not have any of those. 

The easiest way to create a `bash` script with `input` `output` feature is by adding `argument` options to the script. 

For example, one of the scripts that we previously used to linearize `fasta` contained one `argument`.

```bash
$ bash linearize.sh genome.fasta
```
The `genome.fasta` was the argument at that moment. 

Let's modify the previous script in order to take `arguments`. 

```bash
$ nano myfirstscript.sh
```

	  GNU nano 2.0.9            File: myfirstscript.sh                    Modified

	#!/bin/bash

	# declare STRING variable
	STRING=$1
	#print variable on a screen
	echo $STRING













	Save modified buffer (ANSWERING "No" WILL DESTROY CHANGES) ?
	 Y Yes
	 N No           ^C Cancel


Hit `Y` to save and then let's run the program with arguments


> #### TASK 6
> Create a Script that takes your `first name` and your `last name` as `arguments` and print them in the reverse order. 


Ideally, you can use all the information given in this small workshop to use different manipulation techniques and put them in a `script` that you can run on an everyday basis. 


### Using the `TORQUE` scheduler to submit jobs in pistol pete HPC

Although we have been using the `login` nodes throughout this workshop. It is very very important to understand that computing intensive jobs **must** be submitted through the HPC scheduler. Now I will teach you how to do it. 

All this information and guidelines are found at the [HPC online tutorial](https://hpcc.okstate.edu/content/new-user-tutorial).  However, this is a very brief explanation how to use and submit a very basic job. 


- First, to submit your job you need to have a `submission script` which contains all the arguments for submission. Let's create a basic one:

```bash
nano my_first_pbs_script.pbs
```

Then we want to type the following inside that script. (Please copy and paste this part inside your nano prompt)


	#!/bin/bash
	#PBS -q express
	#   specify the queue batch, express or bigmem
	#PBS -l nodes=1:ppn=1
	#   request 1 processor on 1 node
	#PBS -l walltime=10:00
	#   choose a walltime slightly longer than your job will take
	#PBS -j oe
	cd $PBS_O_WORKDIR
	module load python/3.5.0
	 
	myfirstscript.sh
	

Once you have finished modifying you save it and submit the job as follows:

```bash
qsub my_first_pbs_script.pbs
```

You can check your position in que queue by typing

```bash
qstat -n | grep "yourusername"
```

Your STDOUT output will be written into a file that is created by the `TORQUE` scheduler. 


And that's all FOLKS.






