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
cd /scratch/asecas86/afternoon
mkdir scripts
cd scripts
```

Then let's type the following: 
You already learned how to use `nano`. Let's practice. 


```bash
 nano myfirstscript.txt 
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

Do you want to try something more difficult: 


