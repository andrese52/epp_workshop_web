---
title: Data Acquisition
weight: 2
chapter: false
pre: "<b>3.2 </b>"
---

## Contents

- [Introduction](#Intro)
- [Back to the terminal](#back-to-the-terminal)
- [Retrieving my first ftp file](#retrieving-my-first-ftp-file)
- [Filtering Data](#filtering-data)
- [Loops and variables in  Linux](#loops-and-variables-in-linux)
- [Task 1](#task-1)
- [Task 2](#task-2)


## Intro
[[back to top](#contents)]


{{% notice note %}} While using coding and bioinformatics, we want to 
avoid using graphical user interphases (GUIs) as much as we can. Can 
somebody answer why? {{% /notice %}}
 
Visualizing large datasets (as the ones that we are going to download) 
could be practically impossible.  Opening the whole document will likely 
**crash your computer**.
 
 {{< figure 
src="https://media.giphy.com/media/l3q2Mdm0cRPLPcuGI/giphy.gif" title="" 
>}}
 
 However, there are exceptions, where we already know the data structure 
of a file and we can start running codes on it.
 
 [NCBI](https://www.ncbi.nlm.nih.gov/) which stands for the The National 
Center for Biotechnology Information helps bioinformaticians by 
providing access to biomedical and genomic information. This is a GUI 
which helps people search and process data. This is given the fact that 
the data has been previously curated by others.
 
 They also have access to an [FTP site](ftp://ftp.ncbi.nlm.nih.gov), 
where we can download files in bulk. Usually, exploring these folders 
become difficult, therefore, they create summary files that we can play 
with.
 
 Let's start by downloading the `assembly_summary_genbank.txt` from the 
NCBI FTP site at ftp://ftp.ncbi.nlm.nih.gov/genomes/ASSEMBLY_REPORTS/. 
If you click on this site link the directory content should look 
something like this:
 
	.
	├── [ 30] README_assembly_summary.txt -> ../README_assembly_summary.txt
	├── [ 27] README_change_notice.txt -> ../README_change_notice.txt
	├── [ 22K] species_genome_size.txt.gz
	├── [ 31M] ANI_report_bacteria.txt
	├── [2.2M] assembly_summary_genbank_historical.txt
	├── [ 46M] assembly_summary_genbank.txt
	├── [2.4M] assembly_summary_refseq_historical.txt
	└── [ 34M] assembly_summary_refseq.txt
	
## Back to the terminal
[[back to top](#contents)]


Create a new folder in your `/scratch/username` directory named 
`afternoon` and move inside the `afternoon` folder: 

```bash 
$ mkdir afternoon 
$ cd afternoon 
```

Let's make sure that you are in the correct directory by typing `pwd` 
```bash 
$ pwd 
```
you should get something like this: 

	/scratch/asecas86/afternoon

> My username is ***asecas86*** (you already learned this)
	
Now, let us retrieve the `assembly_summary_genbank.txt`

## Retrieving my first ftp file
[[back to top](#contents)]


```bash 
$ wget ftp://ftp.ncbi.nlm.nih.gov/genomes/ASSEMBLY_REPORTS/assembly_summary_genbank.txt 
```

	--2018-03-04 22:46:48-- 
	ftp://ftp.ncbi.nlm.nih.gov/genomes/ASSEMBLY_REPORTS/assembly_summary_genbank.txt
					   => “assembly_summary_genbank.txt”
			Resolving ftp.ncbi.nlm.nih.gov... 165.112.9.229, 
	2607:f220:41e:250::7
			Connecting to ftp.ncbi.nlm.nih.gov|165.112.9.229|:21... 
	connected.
			Logging in as anonymous ... Logged in!
			==> SYST ... done.  ==> PWD ... done.
			==> TYPE I ... done.  ==> CWD (1) 
	/genomes/ASSEMBLY_REPORTS ... done.
			==> SIZE assembly_summary_genbank.txt ... 48113668
			==> PASV ... done.  ==> RETR 
	assembly_summary_genbank.txt ... done.
			Length: 48113668 (46M) (unauthoritative)
			
	100%[=====================================================================================================================================>] 
	48,113,668 11.1M/s in 8.4s 

Let's verify that that the file is present 

```bash 
$ ls -lth 
```

	total 52M
	-rw------- 1 asecas86 clusterusers 46M Mar 4 22:46 assembly_summary_genbank.txt 

Let's explore the head of the file

```bash 
$ head -n 2 assembly_summary_genbank.txt 
```

	#   See ftp://ftp.ncbi.nlm.nih.gov/genomes/README_assembly_summary.txt for a description of the columns in this file.
	# assembly_accession    bioproject      biosample       wgs_master      refseq_category taxid   species_taxid   organism_name   infraspecific_name      isolate       version_status  assembly_level  release_type    genome_rep      seq_rel_date    asm_name        submitter       gbrs_paired_asm paired_asm_comp ftp_path      excluded_from_refseq    relation_to_type_material
	GCA_000001215.4 PRJNA13812      SAMN02803731            reference genome        7227    7227    Drosophila melanogaster                 latest  Chromosome   Major    Full    2014/08/01      Release 6 plus ISO1 MT  The FlyBase Consortium/Berkeley Drosophila Genome Project/Celera Genomics       GCF_000001215.4 identical     ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/215/GCA_000001215.4_Release_6_plus_ISO1_MT


	
Can we make sense of the data from here?  Really difficult to tell 
right??? That's why it is important to **ALWAYS CREATE A README FILE**.  


Let's take a peek at their `README_assembly_summary.txt`.


```bash 
$ wget ftp://ftp.ncbi.nlm.nih.gov/genomes/ASSEMBLY_REPORTS/README_assembly_summary.txt 
``` 


`nano` is utilized to edit files. However, we just want to read the file so let's use `less`


```bash 
$ less README_assembly_summary.txt
```

 Keep pushing `enter` until you reach the end of the document and tell me what information about their ftp archives can be recovered and are useful. Take notes of what information we can get on each column. 
 
 {{% notice warning %}} Push `Q` to quit watching the `readme` file. {{% /notice %}}
 

 

## Filtering Data
[[back to top](#contents)]


Let's focus on our organism of interest `Aspergillus flavus`. Let's try searching for our pathogen in the database: 
```bash 
$ grep "aspergillus flavus" assembly_summary_genbank.txt
``` 

***What did you get as output from the previous code?*** **NOTHING? HOW COME??**

Try this 
```bash 
$ grep -i "aspergillus flavus" assembly_summary_genbank.txt
``` 

{{% notice note %}} Take 1 minute to understand why this happened {{% /notice %}} 

You are right, this is because you added the `-i` argument which asks for `case insensitive`


- Let's count how many entries for `Aspergillus flavus` we have and at the same time let's redirect our entries to a file named `aspergillus_assemblies.txt` 

```bash 
$ grep -ic "aspergillus flavus" assembly_summary_genbank.txt
$ grep -i "aspergillus flavus" assembly_summary_genbank.txt > aspergillus_assemblies.txt 
```
	17
	
We do have 17 assemblies of `Aspergillus flavus` available in this FTP site. Guess what, not all of them are of importance to us. I want only assemblies at the chromosome level. Remember that there can be different types of assembly levels [See above](../../afternoon/terminology/#sequence-assembly). We want to sort by column to select those Chromosomal assemblies (if any). Now that we have retrieved data we will learn how to filter by column in the next session. 

Well, let me introduce you to the beautiful `awk` (We will be seeing more of `awk` under [Regular Expressions](../regex))

```bash 
$ awk -F'\t' '$12 == "Chromosome"' aspergillus_assemblies.txt | wc -l 
```
		0
		
**Can somebody tell me what are we doing with the above code?**



We are piping the output of `awk` to a `wc` command that is the famous `word count`. You can count how many lines are found in a file with `wc -l`.

Unfortunately, we have `0` crhomosomal assemblies. Now let's try Scaffolds.


### Loops and variables in  Linux
[[back to top](#contents)]

Assigning values to variables is really easy, just type the following:

```bash
a="Andres"
```
Now if you want to retrieve your variable and print it to the prompt do the following.


```bash
echo $a
```


For the next task you need to know how loops work in linux.

Let's try some, but first let's get some variables assigned


```bash
a="like"
b="this"
c="workshop"
```

```bash
for i in $a $b $c
do
echo $i
done
```




> #### TASK 1:

> [[back to top](#contents)]


> Complete the following tab-delimited table with information about how many assemblies are at the chromosome, scaffold and contig level. The table must look something like this:

>Assembly level | Number of assemblies
--------|------
Chromosome | 0
Scaffold | --
Contig | --

>{{% notice tip %}}
**HINT:** You can create a for loop

{{% /notice %}}


#### Solution 1
First we need to know which list will be used in our for loop. In this case I want a list of `Chromosome, Scaffold and Contig`. Then you create the `loop` like so:

```bash
for i in Chromosome Scaffold Contig
do
assemblies=`awk -v var="$i" -F'\t' '$12 == Chromosome' aspergillus_assemblies.txt | wc -l`
echo -e $i'\t'$assemblies >> table_assemblies.txt
echo -e $i'\t'$assemblies
done
```

Did you really try to only copy and paste the loop and got an error? Try harder..... Focus on the `var`


The result should look something like this:

	[asecas86@n252 afternoon]$ for i in Chromosome Scaffold Contig
	> do
	> assemblies=`awk -v var="$i" -F'\t' '$12 == var' aspergillus_assemblies.txt | wc -l`
	> echo -e $i ' \t ' $assemblies >> table_assemblies.txt
	> echo -e $i ' \t ' $assemblies
	> done
	Chromosome        0
	Scaffold          5
	Contig            12


Let us retrieve only the best assemblies, in this case lets select `Scaffolds`


```bash 
$ awk -F'\t' '$12 == "Scaffold"' aspergillus_assemblies.txt > chr_aspergillus_assemblies.txt
``` 

Let's check those 5 entries by using `head` 

```bash
$ head chr_aspergillus_assemblies.txt
```

I want to get the ftp address to download those files.

```bash
$ awk -F'\t' '{print $20}' chr_aspergillus_assemblies.txt > ftp-ids.txt
```

and finally check if you got the `five` ftp addresses. 

```bash
$ less ftp-ids.txt
```

We want to compare these five genomes.  Therefore, we want to get the genome files and the annotation files (fasta and gff or gtf)

You can open one of the files to explore the directory, for example, let's explore this:  ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/002/443/195/GCA_002443195.1_AflaGuard

 {{< figure 
src="../../images/aflaguard.png" title="" 
>}}

There are multiple files inside this directory, we are interested in `.fna` and `gff` or `gtf` files. Let's target those files with a `wildcard` and a `while` loop.

First let's make a dir for the genomes and annotations that we will retrieve:

```bash
$ mkdir genomes
```

Now finally let us retrieve those files with the following loop:

**Very important note:** Make sure that the space between `wget -P genome/` and `$p/*.fna.gz` is present, otherwise `wget` will not work. Can you guess why??

```bash
while read p; do
  echo $p
wget -P genomes/ $p/*.fna.gz
done <ftp-ids.txt
```

Now check what files you downloaded:

```bash
$ ls -lth genomes/
```

	total 77M
	-r--r--r-- 1 asecas86 clusterusers  12M Jan  5 22:21 GCA_002864195.1_ASM286419v1_genomic.fna.gz
	-r--r--r-- 1 asecas86 clusterusers  11M Dec 14 01:09 GCA_002456175.1_AF36_genomic.fna.gz
	-r--r--r-- 1 asecas86 clusterusers  12M Nov 17 11:31 GCA_002443215.1_K49_genomic.fna.gz
	-r--r--r-- 1 asecas86 clusterusers  11M Nov 17 11:29 GCA_002443195.1_AflaGuard_genomic.fna.gz
	-r--r--r-- 1 asecas86 clusterusers 5.9M Nov 13 05:26 GCA_000006275.2_JCVI-afl1-v2.0_cds_from_genomic.fna.gz
	-r--r--r-- 1 asecas86 clusterusers 5.9M Nov 13 05:26 GCA_000006275.2_JCVI-afl1-v2.0_rna_from_genomic.fna.gz
	-r--r--r-- 1 asecas86 clusterusers  12M Jun 16  2016 GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna.gz

you should have 5 files, why do we have 7?

This is because these folders contain all information about annotations and sometimes they include `CDS` which stand for coding regions or `rna` which are regions that were mapped to the genome using its transcripts. It seems like the only one that has this issue is `JCVI` assembly. 

### File compression

Most files in bioinformatics are compressed to allow fast sharing, otherwise they can weight many Gigabytes. The most common compression that you will find online is `.gz`.  The best way to uncompress this type of files is by typing `gunzip *.gz`

We can try that with a loop

```bash
$ cd genomes
```

```bash
for i in *.gz
do
echo "Unzipping $i"
gunzip $i
done 
```

DONE, now you have `fasta` files to analyze. And this is the way you retrieve data in bulk from ftp sites. 
Notice that the directory size increased to 247 MB.

	total 247M
	-r--r--r-- 1 asecas86 clusterusers 37M Jan  5 22:21 GCA_002864195.1_ASM286419v1_genomic.fna
	-r--r--r-- 1 asecas86 clusterusers 36M Dec 14 01:09 GCA_002456175.1_AF36_genomic.fna
	-r--r--r-- 1 asecas86 clusterusers 36M Nov 17 11:31 GCA_002443215.1_K49_genomic.fna
	-r--r--r-- 1 asecas86 clusterusers 36M Nov 17 11:29 GCA_002443195.1_AflaGuard_genomic.fna
	-r--r--r-- 1 asecas86 clusterusers 20M Nov 13 05:26 GCA_000006275.2_JCVI-afl1-v2.0_cds_from_genomic.fna
	-r--r--r-- 1 asecas86 clusterusers 20M Nov 13 05:26 GCA_000006275.2_JCVI-afl1-v2.0_rna_from_genomic.fna
	-r--r--r-- 1 asecas86 clusterusers 36M Jun 16  2016 GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna

> #### TASK 2

> [[back to top](#contents)]


> Download all the `gff` or `gtf` files associated to these five genomes and put them in a folder called `annotations`
{{% notice tip %}}
**HINT:** use `mkdir annotations` in your `afternoon` directory 
{{% /notice %}}

#### Solution 2

```bash
$ mkdir ../annotations
```
What would you change in the following code to get gff files

**Very important note:** Make sure that the space between `wget -P annotations/` and `$p/*.gff.gz` is present, otherwise `wget` will not work. Can you guess why??

```bash
while read p; do
	echo $p
	wget -P annotations/ $p/*.fna.gz
done <ftp-ids.txt
```

You will not get a single cup of coffee if you do not solve this loop as expected. 
 {{< figure 
src="https://media.giphy.com/media/7qV3yswT0K8hi/giphy.gif" title="" 
>}}

What about this Solution?? (Think twice before copying and pasting this one): 
```bash
p="u r 2 Lazy"
for i in {1..100}
do
	echo $p $i "Times"
done
```
Ok try the loop with `gff` but first:

```bash
$ cd ..
```
 Then:
**Very important note:** Make sure that the space between `wget -P annotations/` and `$p/*.gff.gz` is present, otherwise `wget` will not work. Can you guess why??

```bash
while read p; do
	echo $p
	wget -P annotations/ $p/*.gff.gz
done <ftp-ids.txt
```

Let's see what we got:

```bash
$ ls -lth annotations/
```

	total 3.0M
	-r--r--r-- 1 asecas86 clusterusers 4.2K Jan  5 22:21 GCA_002864195.1_ASM286419v1_genomic.gff.gz
	-r--r--r-- 1 asecas86 clusterusers 3.3K Dec 14 01:09 GCA_002456175.1_AF36_genomic.gff.gz
	-r--r--r-- 1 asecas86 clusterusers 3.3K Nov 17 11:31 GCA_002443215.1_K49_genomic.gff.gz
	-r--r--r-- 1 asecas86 clusterusers 3.1K Nov 17 11:29 GCA_002443195.1_AflaGuard_genomic.gff.gz
	-r--r--r-- 1 asecas86 clusterusers 2.3M Nov 13 05:26 GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff.gz

Uncompress:
```bash
$ cd annotations
```

and finally

```bash
for i in *.gz
do
	echo "Unzipping $i"
	gunzip $i
done 
```

Check what you got: 

```bash
$ ls -lth
```

You should have 5 files. If you have more or less, then you did something wrong!

	total 29M
	-r--r--r-- 1 asecas86 clusterusers 47K Jan  5 22:21 GCA_002864195.1_ASM286419v1_genomic.gff
	-r--r--r-- 1 asecas86 clusterusers 31K Dec 14 01:09 GCA_002456175.1_AF36_genomic.gff
	-r--r--r-- 1 asecas86 clusterusers 32K Nov 17 11:31 GCA_002443215.1_K49_genomic.gff
	-r--r--r-- 1 asecas86 clusterusers 29K Nov 17 11:29 GCA_002443195.1_AflaGuard_genomic.gff
	-r--r--r-- 1 asecas86 clusterusers 26M Nov 13 05:26 GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff

 
In the upcoming modules you will learn how to iterate through the files that we just downloaded to compare basic metrics. Now, please get some fresh air. 

 [[back to top](#contents)]