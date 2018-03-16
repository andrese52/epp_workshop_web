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
awk '/pattern/{ print $0 }' file
```

Let's move to one of our `afternoon` directories. 

```bash
cd /scratch/asecas86/afternoon
```



## Basic awk

[[back to top](#contents)]


We will be using basic `awk` with our `fna` and `gff` files. Lets start with `fna` files.

Let's move to `genomes`

```bash 
cd genomes
```

Now select the following file: `GCA_002443195.1_AflaGuard_genomic.fna` and run the following command:

```bash
awk '/NNNNN/{ print $0 }' GCA_002443195.1_AflaGuard_genomic.fna
```

Pretty nasty output. We must linearize first the fasta files.  There is this very helpful script that linearizes any fasta file. Let's download it first. 

```bash
wget --no-check-certificate https://raw.githubusercontent.com/andrese52/Training_datasets_scripting/master/scripts/linearize.sh
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
cat linearized_GCA_002443195.1_AflaGuard_genomic.fna  | awk 'NR%2==0' | awk '{print length($1)}'
```

- What is happening above is that `NR%2==0` is requesting that every even line in the file is printed, then these lines are piped into another awk that gives the length of the sequence.

- We can also redirect the output to a new file.

```bash
cat linearized_GCA_002443195.1_AflaGuard_genomic.fna  | awk 'NR%2==0' | awk '{print length($1)}' > read_dist_GCA_002443195.1_AflaGuard_genomic.fna
```

Now let's check the read distribution file

```bash
head -n 5 read_dist_GCA_002443195.1_AflaGuard_genomic.fna
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
sort -n read_dist_GCA_002443195.1_AflaGuard_genomic.fna | head -n 1
sort -n read_dist_GCA_000006275.2_JCVI-afl1-v2.0_genomic.fna | head -n 1
```

> #### Task 4
> Obtain the largest contig length on both genomes 


Very helpful command to separate columns. Similar to excel.

Extract fields 2, 4, and 5 from file.txt:

    awk '{print $2,$4,$5}' input.txt


Print each line where the 5th field is equal to ‘abc123’:

    awk '$5 == "abc123"' file.txt


Print each line where the 5th field is *not* equal to ‘abc123’:

    awk '$5 != "abc123"' file.txt


Print each line whose 7th field matches the regular expression:

    awk '$7  ~ /^[a-f]/' file.txt


Print each line whose 7th field *does not* match the regular expression:

    awk '$7 !~ /^[a-f]/' file.txt


Get unique entries in file.txt based on column 2 (takes only the first instance):

    awk '!arr[$2]++' file.txt


Print rows where column 3 is larger than column 5 in file.txt:

    awk '$3>$5' file.txt


Sum column 1 of file.txt:

    awk '{sum+=$1} END {print sum}' file.txt


Compute the mean of column 2:

    awk '{x+=$2}END{print x/NR}' file.txt

	
	
Keep only top bit scores in blast hits (best bit score only):

    awk '{ if(!x[$1]++) {print $0; bitscore=($14-1)} else { if($14>bitscore) print $0} }' blastout.txt


Keep only top bit scores in blast hits (5 less than the top):

    awk '{ if(!x[$1]++) {print $0; bitscore=($14-6)} else { if($14>bitscore) print $0} }' blastout.txt


Split a multi-FASTA file into individual FASTA files:

    awk '/^>/{s=++d".fa"} {print > s}' multi.fa

Output sequence name and its length for every sequence within a fasta file:

    cat file.fa | awk '$0 ~ ">" {print c; c=0;printf substr($0,2,100) "\t"; } $0 !~ ">" {c+=length($0);} END { print c; }'
	
	

Print everything except the first line

    awk 'NR>1' input.txt

Print rows 20-80:

    awk 'NR>=20&&NR<=80' input.txt

Calculate the sum of column 2 and 3 and put it at the end of a row:

    awk '{print $0,$2+$3}' input.txt

Calculate the mean length of reads in a fastq file:

    awk 'NR%4==2{sum+=length($0)}END{print sum/(NR/4)}' input.fastq	
	
And this is the best of the best. For the end: Changing the headers of multiple fasta files based on their naming prefix<sup>[ref.](https://www.biostars.org/p/237163/#237167)</sup>. I use this in a regular basis after I have done multiple modifications to fasta files. 

```bash
 ls *probes.fasta | cut -d "-" -f 1 | while read PREFIX; do awk -v P=${PREFIX} '/^>/{print ">" P "_" ++i; next}{print}' ${PREFIX}-probes.fasta > ${PREFIX}-probes-new.fasta ; done
 ```

## Basic SED

[[back to top](#contents)]


	
Replace all occurances of `foo` with `bar` in file.txt:

    sed 's/foo/bar/g' file.txt


Trim leading whitespaces and tabulations in file.txt:

    sed 's/^[ \t]*//' file.txt


Trim trailing whitespaces and tabulations in file.txt:

    sed 's/[ \t]*$//' file.txt


Trim leading and trailing whitespaces and tabulations in file.txt:

    sed 's/^[ \t]*//;s/[ \t]*$//' file.txt


Delete blank lines in file.txt:

    sed '/^$/d' file.txt


Delete everything after and including a line containing `EndOfUsefulData`:

    sed -n '/EndOfUsefulData/,$!p' file.txt

Convert a FASTQ file to FASTA:

    sed -n '1~4s/^@/>/p;2~4p' file.fq > file.fa

Extract every 4th line starting at the second line (extract the sequence from FASTQ file):

    sed -n '2~4p' file.fq
	
Remove spaces in fasta headers: this one-liner removes spaces and re-writes the file

	sed 's, ,_,g' -i FILENAME.FASTA
	
Now, let's try by eliminating the `-i`

```bash
sed 's, ,_,g' FILENAME.FASTA
```


## sort, uniq, cut, etc.

[[back to top](#contents)]

Number each line in file.txt:

    cat -n file.txt

Count the number of unique lines in file.txt

    cat file.txt | sort -u | wc -l


Find lines shared by 2 files (assumes lines within file1 and file2 are unique; pipe to `wd -l` to count the _number_ of lines shared):

    sort file1 file2 | uniq -d

    # Safer
    sort -u file1 > a
    sort -u file2 > b
    sort a b | uniq -d

    # Use comm
    comm -12 file1 file2


Sort numerically (with logs) (g) by column (k) 9:

    sort -gk9 file.txt


Find the most common strings in column 2:

    cut -f2 file.txt | sort | uniq -c | sort -k1nr | head


Pick 10 random lines from a file:

    shuf file.txt | head -n 10


Print all possible 3mer DNA sequence combinations:

    echo {A,C,T,G}{A,C,T,G}{A,C,T,G}

	
## GFF3 Annotations

[[back to top](#contents)]


Print all sequences annotated in a GFF3 file.

    cut -s -f 1,9 yourannots.gff3 | grep $'\t' | cut -f 1 | sort | uniq


Determine all feature types annotated in a GFF3 file.

    grep -v '^#' yourannots.gff3 | cut -s -f 3 | sort | uniq


Determine the number of genes annotated in a GFF3 file.

    grep -c $'\tgene\t' yourannots.gff3


Extract all gene IDs from a GFF3 file.

    grep $'\tgene\t' yourannots.gff3 | perl -ne '/ID=([^;]+)/ and printf("%s\n", $1)'


