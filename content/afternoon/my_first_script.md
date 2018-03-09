---
title: My first bash script
weight: 3
chapter: false
pre: "<b>3.3 </b>"
---
### Building our first script
The native language of Linux for its terminal is `bash` and we can write 
scripts and create a small program that does all the above for us with 
single line in bash. First, we need to create a file with `nano` which 
is a text editor that is also installed in Linux by default. There are 
other text editors like `vim`. Using `nano` we can create a `.txt` like 
so: ```bash nano myfirstscript.txt ```
	  GNU nano 2.0.9 File: myfirstscript.txt Modified
	Andres Espindola
									  [ New File ]
	^G Get Help ^O WriteOut ^R Read File ^Y Prev Page ^K Cut Text ^C 
Cur Pos
	^X Exit ^J Justify ^W Where Is ^V Next Page ^U UnCut Text^T To 
Spell Once finished typing your name you must save the file as by 
hitting `Ctrl+X` The program will ask you if you want to save the 
current modification and you have to push `Y` for Yes and `N` for No. 
**Hit Y please**
	GNU nano 2.0.9 File: myfirstscript.txt Modified
	Andres Espindola
	Save modified buffer (ANSWERING "No" WILL DESTROY CHANGES) ?
	 Y Yes
	 N No ^C Cancel Once you have typed yes, the program will ask 
you if you want to keep that filename `myfirstscript.txt`. Hit `Enter` 
to keep it PLEASE.
	GNU nano 2.0.9 File: myfirstscript.txt Modified
		
	Andres Espindola
	File Name to Write: myfirstscript.txt
	^G Get Help ^T To Files M-M Mac Format M-P Prepend
	^C Cancel M-D DOS Format M-A Append M-B Backup File
### Important tips about using `nano`
- Your mouse will NOT work at all in the terminal, so please do not try 
to click in different parts of the `nano editor` to edit the document.
 {{< figure src="https://media.giphy.com/media/pFwRzOLfuGHok/giphy.gif" 
title="" >}} - Use your keyboard arrows to move around the document 
instead.
 {{< figure src="https://media.giphy.com/media/FdUILv11pQ15S/giphy.gif" 
title="" >}}
 
Now that you know how to use `nano`. We can create your first script. 
First let's remove that file that you recently created. ```bash $ rm 
myfirstscript.txt ``` Any script in Linux should start with 
[**shebang:"#!"**](https://en.wikipedia.org/wiki/Shebang_(Unix)). Start 
another `nano` document: ```bash $ nano myfirstscript.sh ``` Type the 
following in the editor:
	#!/bin/bash
	# declare STRING variable
	STRING="Pistol Pete"
	#print variable on a screen
	echo $STRING
	
Everything of what you are writing in your first bash script has been 
already covered during the [morning session](../../morning). Save the 
file and now we need to make it executable. ```bash $ chmod +x 
myfirstscript.sh ``` Now you can execute your first script ```bash $ 
./myfirstscript.sh ```
https://linuxconfig.org/bash-scripting-tutorial
