---
title: Version with with git
weight: 5
chapter: false
pre: "<b>3.5 </b>"
---
Probably you have heard about  [Github](https://github.com/) and how bioinformaticians are using it too keep their developments up to date. It is an extraordinary tool for keep track of all your script changes and versions. It just requires a little bit of training to **SUCCEED**.

 {{< figure src="https://thumbs.gfycat.com/GargantuanSimilarCoypu-size_restricted.gif" title="" >}}

First let's create a folder inside scripts: 


```bash
mkdir my_git_script
cd my_git_script
mv ../myfirstscript.sh .
```
Create a new repository

```bash
git init
```

First you have to configure your global `git` parameters

```bash
git config --global user.name "username"
```

You have initiated a new repository

a hidden folder called `.git` has been created

```bash
ls -ltha
```

	total 256K
	drwxr-xr-x 7 asecas86 clusterusers 4.0K Mar 16 01:11 .git
	drwxr-xr-x 3 asecas86 clusterusers 4.0K Mar 16 01:11 .
	drwxr-xr-x 3 asecas86 clusterusers 4.0K Mar 16 01:11 ..
	-rwxr-xr-x 1 asecas86 clusterusers  101 Mar 16 01:10 myfirstscript.sh

```bash
git add *
```

Adds all the changes in your directory to the local git cache

```bash
git status
```

Shows all the changes you have done in your directory

	[asecas86@n252 my_git_script]$ git status
	# On branch master
	#
	# Initial commit
	#
	# Changes to be committed:
	#   (use "git rm --cached <file>..." to unstage)
	#
	#       new file:   myfirstscript.sh

```bash
git commit -m "My first commit"
```



