---
title: Data Acquisition
weight: 3
chapter: false
pre: "<b>3.3 </b>"
---
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
	
### Back to the terminal
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
 

 

 
Let's focus on our organism of interest `Aspergillus flavus`. Let's try searching for our pathogen in the database: 
```bash 
$ grep "aspergillus" assembly_summary_genbank.txt
``` 

> ***What did you get as output from the previous code?*** **NOTHING? HOW COME??**

Try this 
```bash 
$ grep -i "aspergillus" assembly_summary_genbank.txt
``` 

{{% notice note %}} Take 1 minute to understand why this happened {{% /notice %}} 


Let's count how many entries for `Aspergillus flavus` we have and at the same time let's redirect our entries to a file named `aspergillus_assemblies.txt` 

```bash 
$ grep -ic "aspergillus" assembly_summary_genbank.txt
$ grep -i "aspergillus" assembly_summary_genbank.txt > aspergillus_assemblies.txt 
```
	110
	
We do have 110 assemblies of `Aspergillus flavus` available in this FTP site. Guess what, not all of them are of importance to us. I want only assemblies at the chromosome level. Remember that there can be different types of assembly levels [See above](../../afternoon/terminology/#sequence-assembly). We want to sort by column to select those Chromosomal assemblies (if any). Well, let me introduce you to the beautiful `awk`

```bash 
awk -F'\t' '$12 == "Chromosome"' aspergillus_assemblies.txt | wc -l 
```
		3
		
> **Can somebody tell me what are we doing with the above code?**

We are piping the output of `awk` to a `wc` command that is the famous `word count`. You can count how many lines are found in a file with `wc -l`.

Redirecting the output to a new file: 

```bash 
$ awk -F'\t' '$12 == "Chromosome"' aspergillus_assemblies.txt > chr_aspergillus_assemblies.txt
``` 

Let's check those 3 entries by using `head` 

```bash
head chr_aspergillus_assemblies.txt
```

I want to get the ftp address to download those files.

```bash
$ awk -F'\t' '{print $20}' chr_aspergillus_assemblies.txt > ftp-ids.txt
```

and finally check if you got the three ftp addresses. 

```bash
$ less ftp-ids.txt
```

We want to compare these three genomes and we want to get the genome files and the annotation files (gff or gtf)

You can open one of the files to explore the directory, for example, let's explore ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/002/655/GCA_000002655.1_ASM265v1

There are multiple files inside this directory, we are interested in `.fna` files. Let's target those files with a `wildcard` and a `while` loop.

First let's make a dir for the genomes

```bash
$ mkdir genomes
```

```bash
while read p; do
  echo $p
wget -P genomes/ $p/*.fna.gz
done <ftp-ids.txt
```

### File compression

Most files in bioinformatics are shared compressed, otherwise they can weight many Gigabytes. The most common compression that you will find online is `.gz`.  The best way to uncompress this type of files is by typing `gunzip *.gz`

We can try that with a loop

```bash
cd genomes
```

```bash
for i in *.gz
do
echo "Unzipping $i"
gunzip $i
done 
```

DONE, now you have `fasta` files to analyze. And this is the way you retrieve data in bulk from ftp sites. 



> ### FIVE Minute task #1
> Download those three genomes. I should retrieve the ftp address for each of them from `chr_aspergillus_assemblies.txt`. Use `awk` to write the output to a new `.txt` file. 

{{% notice tip %}} Use `awk` and select a different column number. [Sol.](https://gist.github.com/andrese52/cd4cbf0b9bf37d342e1eec0950a00508) 
{{% /notice %}}
