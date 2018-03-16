---
title: Regular expressions
weight: 3
chapter: false
pre: "<b>3.3 </b>"
---


## Contents

- [Basic awk](#basic-awk)
- [Basic sed](#basic-sed)
- [sort, uniq, cut, etc.](#sort-uniq-cut-etc)
- [GFF3 Annotations](#gff3-annotations)



## Intro
A regular expression, regex or regexp (sometimes called a rational expression) is, in theoretical computer science and formal language theory, a sequence of characters that define a search pattern. Usually this pattern is then used by string searching algorithms for "find" or "find and replace" operations on strings.

Multiple tools in LINUX can be utilized to find regular expressions. The most common one is `grep` which you already used during the morning. 

Now there are a couple of more powerful tools that work with regular expressions, these are `awk` and `sed`. 


For example: the most basic `awk` command looks like this:

```bash
$ awk '/pattern/{ print $0 }' file
```

Let's move to one of our `afternoon` directories. 

```bash
$ cd /scratch/asecas86/afternoon
```



## Basic awk

[[back to top](#contents)]


We will be using basic `awk` with our `fna` and `gff` files. Lets start with `fna` files.

Let's move to `genomes`

```bash 
$ cd genomes
```

Now select the following file: `GCA_002443195.1_AflaGuard_genomic.fna` and run the following command:

```bash
$ awk '/NNNNN/{ print $0 }' GCA_002443195.1_AflaGuard_genomic.fna
```

Pretty nasty output. We must linearize first the fasta files.  There is this very helpful script that linearizes any fasta file. Let's download it first. 

```bash
$ wget --no-check-certificate https://raw.githubusercontent.com/andrese52/Training_datasets_scripting/master/scripts/linearize.sh
```

Let's check how many lines `GCA_002443195.1_AflaGuard_genomic.fna` has prior linearizing it.

```bash
$ wc -l GCA_002443195.1_AflaGuard_genomic.fna
```

	453747 GCA_002443195.1_AflaGuard_genomic.fna


Run the script as follows:

```bash
$ bash linearize.sh GCA_002443195.1_AflaGuard_genomic.fna
```

This script manipulates the fasta file and just reformats it to have the following format:

	>seq1
	ATCCCCCCC
	>seq2
	ATCTCTCT



Once the file is linearized, count the lines again like so: 

```bash
$ wc -l linearized_GCA_002443195.1_AflaGuard_genomic.fna
```

	196 linearized_GCA_002443195.1_AflaGuard_genomic.fna

BOOM!! You have linearized a .fasta file.  This is very helpful to manipulate the file and get metrics and regular expressions. 

Let's make sure the file was correctly linearized by counting the number of fasta headers in the original file. The count should give 196/2 = 98. Let's check!!

```bash
$ grep -c ">" GCA_002443195.1_AflaGuard_genomic.fna
```
	98


GREAT, you just linearized a `fasta` file. Now let's get some metrics. 


### Identifying patterns in `fasta` files with `awk`

An important pattern to find are those nucleotides that have not been identified yet due to low quality sequencing. We might want to eliminate `contigs` or `scaffolds` containing these `NNNN` patterns since they are not useful for downstream analyses (this varies depending on the researcher's needs). Let's check:


```bash
$ awk '/NNNNN/{ print $0 }' linearized_GCA_002443195.1_AflaGuard_genomic.fna | wc -l
```
	32
	
So, 32 out of 98 contigs have some sort of `NNNNN` patterns of at least 5 Ns. 



Let's get the sequence lengths of the current scaffolds

```bash
$ cat linearized_GCA_002443195.1_AflaGuard_genomic.fna  | awk 'NR%2==0' | awk '{print length($1)}'
```

- What is happening above is that `NR%2==0` is requesting that every even line in the file is printed, then these lines are piped into another awk that gives the length of the sequence.

- We can also redirect the output to a new file.

```bash
$ cat linearized_GCA_002443195.1_AflaGuard_genomic.fna  | awk 'NR%2==0' | awk '{print length($1)}' > read_dist_GCA_002443195.1_AflaGuard_genomic.fna
```

Now let's check the read distribution file

```bash
$ head -n 5 read_dist_GCA_002443195.1_AflaGuard_genomic.fna
```

It appears like some scaffolds have 4 million nucleotides 

	4471384
	4150194
	2714413
	2659415
	2557007
	
> #### Task 3
> Linearize the fasta file `GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna` and count the number of nucleotides on each scaffold.



```bash
$ bash linearize.sh GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna
$ cat linearized_GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna  | awk 'NR%2==0' | awk '{print length($1)}' > read_dist_GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna
```


Once you have both files let's get the shortest contig lenghts: 

```bash
$ sort -n read_dist_GCA_002443195.1_AflaGuard_genomic.fna | head -n 1
$ sort -n read_dist_GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna | head -n 1
```


> #### Task 4
> Obtain the largest contig length on both genomes 

## Annotation files and `awk`

[[back to top](#contents)]


Annotation files of these genomes contain information about the genes that the nucleotides encode. We will be doing some basic filtering with `awk` to find genes of interest.


First let's move to the `annotations` directory


```bash
$ cd ../annotations
```

**We will be working on the annotations of the same two genomes that we used for counting the length of each scaffold.**

Important information about `gff` files can be found online [here](https://uswest.ensembl.org/info/website/upload/gff.html)

What is most important about these files is that they usually have 9 columns.


Position index	| Position name	| Description
--------|------|------
1	| sequence	| The name of the sequence where the feature is located.
2	|source	|Keyword identifying the source of the feature, like a program (e.g. Augustus or RepeatMasker) or an organization (like TAIR).
3	|feature	|The feature type name, like "gene" or "exon". In a well structured GFF file, all the children features always follow their parents in a single block (so all exons of a transcript are put after their parent "transcript" feature line and before any other parent transcript line). In GFF3, all features and their relationships should be compatible with the standards released by the Sequence Ontology Project.
4	|start	|Genomic start of the feature, with a 1-base offset. This is in contrast with other 0-offset half-open sequence formats, like BED files.
5	|end|	Genomic end of the feature, with a 1-base offset. This is the same end coordinate as it is in 0-offset half-open sequence formats, like BED files.[citation needed]
6	|score|	Numeric value that generally indicates the confidence of the source on the annotated feature. A value of "." (a dot) is used to define a null value.
7	|strand|	Single character that indicates the Sense (molecular biology) strand of the feature; it can assume the values of "+" (positive, or 5'->3'), "-", (negative, or 3'->5'), "." (undetermined).
8	|frame| (GTF, GFF2) or phase (GFF3)	Frame or phase of CDS features; it can be either one of 0, 1, 2 (for CDS features) or "." (for everything else). Frame and Phase are not the same, See following subsection.
9	|Attributes|	All the other information pertaining to this feature. The format, structure and content of this field is the one which varies the most between the three competing file formats


It would be good if we first explore the header:

```bash
$ head -n 5 GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff
```

The header contains a lot of information about generalities of the annotation and the genome itself. We must skip these first lines. They are usually not tab-delimited.

The best way to do it is just by targetting lines of interest, for example I am interested in aflatoxin genes. Therefore I will `grep` the file for the pattern `aflatoxin`.

```bash
$ grep -i "aflatoxin" GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff
```

Since I am interested only on those genes I will redirect the output. 


```bash
$ grep -i "aflatoxin" GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff > aflatoxin_genes.gff
```


Now let's analyze by column. Usually the word `aflatoxin` should be found in the `Attributes` column which is column 9. So I can also get those same lines by doing this: 



Print each line where the 9th field is similar to `aflatoxin`:
```bash
$ awk -F "\t" '$9 ~ "aflatoxin"' GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff
```

Now let's check the difference between `grep` and `awk`


```bash
$ grep -i "aflatoxin" GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff | wc -l
$ awk -F "\t" '$9 ~ "aflatoxin"' GCA_000006275.2_JCVI-afl1-v2.0_genomic.gff | wc -l
```

We must get the same result for both which is 107 entries. 

We have already redirected the aflatoxin annotations to a separate file. Now let's play with that file. 


We can get all the positive sense `CDS` and count them

```bash
$ awk -F "\t" '$7 == "+"' aflatoxin_genes.gff | wc -l
```

	45
	
> #### Task 5
> Count the number of `CDS` in the aflatoxin annotation file



## Extras with `gff` files

Print all sequences annotated in a GFF3 file.

    cut -s -f 1,9 yourannots.gff3 | grep $'\t' | cut -f 1 | sort | uniq


Determine all feature types annotated in a GFF3 file.

    grep -v '^#' yourannots.gff3 | cut -s -f 3 | sort | uniq


Determine the number of genes annotated in a GFF3 file.

    grep -c $'\tgene\t' yourannots.gff3




