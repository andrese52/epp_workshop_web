---
title: Unix essentials
weight: 4
chapter: false
pre: "<b>2.4 </b>"
---

Let's jump right into it!

First of all, get your things organized!

```bash
$ mkdir workshop_mar17
$ cd workshop_mar17
$ mkdir morning_session
$ cd morning_session
```

I believe you have worked with (or heard of) a fasta file. Fasta files might have `.fasta` or `.fa` extensions. These are basically text files that contain nucleotide or protein sequences. A fasta file can contain gene sequences, or even an entire genome. 

Let's download an *Aspergillus* genome that is available on the web. This is the website to the genome:

```
http://www.aspergillusgenome.org/download/sequence/A_nidulans_FGSC_A4/current/A_nidulans_FGSC_A4_current_chromosomes.fasta.gz
```

We can download this file directly to Cowboy using `curl -O`:

```bash
$ curl -O http://www.aspergillusgenome.org/download/sequence/A_nidulans_FGSC_A4/current/A_nidulans_FGSC_A4_current_chromosomes.fasta.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 9155k  100 9155k    0     0  5685k      0  0:00:01  0:00:01 --:--:-- 5682k
```

The file we download has `.gz` extension, which means this is a size-compressed file. To uncompress it, use `gunzip`:

```bash
$ gunzip A_nidulans_FGSC_A4_current_chromosomes.fasta.gz
```

**IMPORTANT**: never work with original files. Always make sure to save an original copy that you'll never modify.

```bash
$ mkdir originals
$ cp A_nidulans_FGSC_A4_current_chromosomes.fasta originals/
```

>What are the components of a fasta file?

When you download or receive a file from a collaborator, you must firstly investigate the file using `less`, `head`, and `tail`.

+ `less`

```bash
$ less A_nidulans_FGSC_A4_current_chromosomes.fasta
```

Hit `Q` to exit and return to the command-line.

+ `head` prints the first lines of a file. By default, it prints the first 10 lines.

```bash
$ head A_nidulans_FGSC_A4_current_chromosomes.fasta
>ChrIII_A_nidulans_FGSC_A4 (3470897 nucleotides)
CAACCTTGTTGTACTGAAATGGCTACCTCTGCCCCCCAGACCCTGATTTGCCGCTGCTCA
ACTGACCTTACAAGCCCCCTGAGCCCCCCCAGCCTGACACCAACCATCATCAATGCAAAG
TCAAAGAGGAGATACTACCACAATCAGCAATGGGAGCAGGAGTGTGTGCTGCAGGCCAGC
CTGATAACAGCTCTGCCTCAGTACCCTGCTCCCTTGGACAGCATGGATGAGCTTGTGATC
GCAAAGCCCGCAGCAGACACCAACAACTCCCACAACAACAACAACAACTCCCACAACAAC
AACAATAACAACAACAACACCAACACCAACAGCAACAACAGCCAGGACGCGGACAGCAAC
GCGGACAGCGACCACAACAGCAACAGTGACCAGAATCAGCAGAAGCCTATTATTGAGACT
ACTGATGATCACAGTGTAGATGACCAGCACATCGACACCGTCAGTGACAGCGATGAACCC
AGCTCCCCAATCATGGGCATTGGCCTCCGCCCCATCAGCAAGCCTGTGTCTGTTGTTGTC
```

You can add an *count option* such as `head -20 A_nidulans_FGSC_A4_current_chromosomes.fasta`, and now `head` prints the first 20 lines of the file.

+ `tail` prints the last lines of a file, and it works just like `head`.

```bash 
$ tail A_nidulans_FGSC_A4_current_chromosomes.fasta 
AGAAATTTTGTAGCTAAAAAATCACCAATAGCTCATAAATATATGAATCACGGTACATTA
ATAGAGTTAATTTGAACAATAACACCAGCATTTATTTTAATACTAATAGCATTCCCTTCT
TTCAAATTATTATATTTAATGGATGAAGTAATGGATCCTTCTTTAGTTGTTTATGCAGAA
GGTCACCAATGATATTGAAGTTACCAATATCCTGATTTTACAAATGAAGATAATGAGTTT
ATAGAATTTGATTCATATATAGTACCAGAAAGTGATTTAGAAGAAGGTCAATTTAGAATG
TTAGAGGTTGATAATAGAGTAATTATTCCAGAATTAACTCACACAAGATTTGTAATTTCT
GCAGCAGATGTTATACATTCATATGCTTGTCCATCTTTAGGTATAAAAGCGGATGCATAC
CCTGGTAGATTAAATCAAGCATCAGTTTATATAAATCGTCCTGGAACTTTCTTCGGACAA
TGTTCTGAAATATGTGGTATATTACATAGCTCAATGCCTATAGCTATACAATCAGTATCA
ATAAAAGATTTCTTATTATGATTAAGAGAACAAATGGAAGGATAAGT
```


Did you notice the **Chr** in the fasta header? *A. nidulans* has a complete genome, and these sequences are to the chromosome level. 

Knowing that, how many chromosomes does this assembly has? Use `grep` to search for pattern in a file.

```bash
$ grep 'Chr' A_nidulans_FGSC_A4_current_chromosomes.fasta 
>ChrIII_A_nidulans_FGSC_A4 (3470897 nucleotides)
>ChrII_A_nidulans_FGSC_A4 (4070060 nucleotides)
>ChrIV_A_nidulans_FGSC_A4 (2887738 nucleotides)
>ChrI_A_nidulans_FGSC_A4 (3759208 nucleotides)
>ChrVIII_A_nidulans_FGSC_A4 (4934093 nucleotides)
>ChrVII_A_nidulans_FGSC_A4 (4550218 nucleotides)
>ChrVI_A_nidulans_FGSC_A4 (3407944 nucleotides)
>ChrV_A_nidulans_FGSC_A4 (3403833 nucleotides)
```

But, is that all the sequences in this file? We can answer this question by searching for a mandatory component of a fasta file, the `>`.

```bash
$ grep '>' A_nidulans_FGSC_A4_current_chromosomes.fasta
>ChrIII_A_nidulans_FGSC_A4 (3470897 nucleotides)
>ChrII_A_nidulans_FGSC_A4 (4070060 nucleotides)
>ChrIV_A_nidulans_FGSC_A4 (2887738 nucleotides)
>ChrI_A_nidulans_FGSC_A4 (3759208 nucleotides)
>ChrVIII_A_nidulans_FGSC_A4 (4934093 nucleotides)
>ChrVII_A_nidulans_FGSC_A4 (4550218 nucleotides)
>ChrVI_A_nidulans_FGSC_A4 (3407944 nucleotides)
>ChrV_A_nidulans_FGSC_A4 (3403833 nucleotides)
>mito_A_nidulans_FGSC_A4 (33227 nucleotides)
```

`grep` prints the lines in which the pattern is found. Use `grep -c` to count pattern matches.

```bash
$ grep -c 'Chr' A_nidulans_FGSC_A4_current_chromosomes.fasta 
8

$ grep -c '>' A_nidulans_FGSC_A4_current_chromosomes.fasta
9
```

Remember that the output of these programs get printed on the screen, aka **stdout**.  

The stdout on the screen means that:  
+It is not being stored anywhere in any form	
+It is not altering the original data    

But, if we are interested in keeping this information, we can **REDIRECT** stdout to a text file to store information.  

To redirect stdout we use `>`.

```bash
$ grep 'Chr' A_nidulans_FGSC_A4_current_chromosomes.fasta > output_A_nidulans_fasta_headers.txt
```

```bash
$ cat output_A_nidulans_fasta_headers.txt
>ChrIII_A_nidulans_FGSC_A4 (3470897 nucleotides)
>ChrII_A_nidulans_FGSC_A4 (4070060 nucleotides)
>ChrIV_A_nidulans_FGSC_A4 (2887738 nucleotides)
>ChrI_A_nidulans_FGSC_A4 (3759208 nucleotides)
>ChrVIII_A_nidulans_FGSC_A4 (4934093 nucleotides)
>ChrVII_A_nidulans_FGSC_A4 (4550218 nucleotides)
>ChrVI_A_nidulans_FGSC_A4 (3407944 nucleotides)
>ChrV_A_nidulans_FGSC_A4 (3403833 nucleotides)
```

Let's download a different file from *A. nidulans*:

```
http://www.aspergillusgenome.org/download/chromosomal_feature_files/A_nidulans_FGSC_A4/A_nidulans_FGSC_A4_current_chromosomal_feature.tab
```

```bash
$ curl -O http://www.aspergillusgenome.org/download/chromosomal_feature_files/A_nidulans_FGSC_A4/A_nidulans_FGSC_A4_current_chromosomal_feature.tab
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 2724k  100 2724k    0     0  2568k      0  0:00:01  0:00:01 --:--:-- 2570k
```

This file isn't compressed.  

What kind of file is this?! This is a chromosomal feature file. You can find the explanation to the components of this file in this link:

```
http://www.aspergillusgenome.org/download/chromosomal_feature_files/A_nidulans_FGSC_A4/
```

What do you do when you first download/receive a file?

```bash
$ cp A_nidulans_FGSC_A4_current_chromosomal_feature.tab originals/
```

Now, what do you do? Investigate with `less`, of course.

```bash
$ less A_nidulans_FGSC_A4_current_chromosomal_feature.tab
```

While in the `less` *window*, hit `-S` to wrap lines.

There is a lot of information in this file! It is difficult to inspect the contents.

Something very common is to build up commands to filter data using **pipes**.

**Pipes** are built with `|` in between commands.

Let's focus on columns 1, 4 and 5.

+ column 1 is gene identifier
+ column 4 is gene feature
+ column 5 is chromosome


Let's build a pipe to filter out these columns.

We can start a pipe with `cat`, then use `cut` to slice the columns we want, and finish with `head -20`:

```bash
$ cat A_nidulans_FGSC_A4_current_chromosomal_feature.tab | cut -f 1,4,5 | head -20
! File name: A_nidulans_FGSC_A4_version_s10-m04-r06_chromosomal_feature.tab
! Organism: Aspergillus nidulans FGSC A4
! Genome version: s10-m04-r06
! Date created: Thu Nov 30 13:46:18 2017
! Created by: The Aspergillus Genome Database (http://www.aspergillusgenome.org/)
! Contact Email: aspergillus-curator AT lists DOT stanford DOT edu
! Funding: NIAID at US NIH, grant number R01-AI077599-01
!
AN0004	pseudogene	ChrVIII_A_nidulans_FGSC_A4
AN0005	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
AN0006	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
AN0007	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
AN0008	pseudogene		ChrVIII_A_nidulans_FGSC_A4
AN0009	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
AN0010	pseudogene		ChrVIII_A_nidulans_FGSC_A4
AN0011	pseudogene		ChrVIII_A_nidulans_FGSC_A4
AN0012	ORF|Verified		ChrVIII_A_nidulans_FGSC_A4
AN0013	pseudogene		ChrVIII_A_nidulans_FGSC_A4
AN0014	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
AN0015	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
```

There is more to inspect about this file. Redirect output and work with a less cluttered file.

```bash
$ cat A_nidulans_FGSC_A4_current_chromosomal_feature.tab | cut -f 1,4,5 > output_A_nidulans_filtered_features.txt
```

What do you see in the second column (gene features) of this output file?

> pseudogene, ORF|Uncharacterized, ORF|Verified, etc.

Let's find all unique features. Let's also delete the first eight lines, which are comments and information about the group that produced this genome.

```bash
$ cat output_A_nidulans_filtered_features.txt | sed '1,8d' | cut -f 2 | sort | uniq
ORF|Merged/Split|Uncharacterized
ORF|Merged/Split|Verified
ORF|Uncharacterized
ORF|Uncharacterized|Merged/Split
ORF|Uncharacterized|transposable element gene
ORF|Verified
multigene locus
ncRNA|Uncharacterized
ncRNA|Verified
pseudogene
pseudogene|Verified
pseudogene|transposable element gene
rRNA|Uncharacterized
tRNA|Uncharacterized
tRNA|Verified
uORF|Uncharacterized
uORF|Verified
```

`sed 1,8d` deletes the first eight lines.

`sort` and `uniq` must be used together and in this order to get unique features.

Repeat the pipe and redirect output.

```bash
$ cat output_A_nidulans_filtered_features.txt | sed '1,8d' output_A_nidulans_filtered_features.txt | cut -f 2 | sort | uniq > output_A_nidulans_filtered_annotations.txt
```

### Challenge!!

1. How many times `ORF|Uncharacterized` appears on the 2nd column?
2. How many times `pseudogene` appears on the 2nd column?
3. Does the 1st column list each gene name (those named 'AN###') only once?
4. Which word appears 199 times on the 2nd column?

### Dip your toes into some advanced stuff...

+ **Wildcard**  

The wildcard `*` is going to match that a pattern before running a command.  

```bash
$ ls *.txt
output_A_nidulans_fasta_headers.txt
output_A_nidulans_filtered_features.txt

$ ls A_nidulans*
A_nidulans_FGSC_A4_current_chromosomal_feature.tab
A_nidulans_FGSC_A4_current_chromosomes.fasta
A_nidulans_FGSC_A4_current_chromosomes.fasta.gz

$ ls *FGSC_A4*
A_nidulans_FGSC_A4_current_chromosomal_feature.tab
A_nidulans_FGSC_A4_current_chromosomes.fasta
A_nidulans_FGSC_A4_current_chromosomes.fasta.gz
```

+ **For loop**  

A loop is an iteration statement that will be repeatedly executed. 

This is a basic for loop syntax:

```bash  
$ for VARIABLE in SOMEWHERE;  
> do command1;   
> command2;  
> commandN;  
> done   
```

or,

```bash
$ for VARIABLE in SOMEWHERE; do command1; command2; commandN; done  
```

`VARIABLE` is an arbitrary name that you choose.   
`SOMEWHERE` can be a file or a directory. 

**IMPORTANT:** What is a 'VARIABLE'?  
A variable is simply a *box*, which you create, to *place* values into it. A more technical definition is: a character string that you assign a value. The value could be text, number, filename, path, etc. You can assing more than one type of value to a variable.  

Don't use `!`, `*` or `-` in variable names because these characters have special meaning for unix...  

You call a variable you defined by using `$` in front of the variable name.  

This is how you define a variable:

```bash
$ variable_name=variable_value
```

```bash
$ words="one two three"
$ echo $words
one two three
$ words="flower sun moon and me"
$ echo $words
flower sun moon and me
```

The syntax of a `for loop` in plain English:  

```bash
$ for variable in collection; do things with variable; done
```

What does a for loop do?

```bash
$ for character in $words; do echo $character; done
flower
sun
moon
and
me
$ for char in $words; do echo $char; done
flower
sun
moon
and
me
$ for bananas in $words; do echo $bananas; done
flower
sun
moon
and
me
```

```bash
$ for i in {1..5}; do echo "Hello $i times"; done
Hello 1 times
Hello 2 times
Hello 3 times
Hello 4 times
Hello 5 times
```

Now, the English translation:

```bash
$ for i in {1..5}; do echo "It's gonna print i $i times"; done
It's gonna print i 1 times
It's gonna print i 2 times
It's gonna print i 3 times
It's gonna print i 4 times
It's gonna print i 5 times
```

Let's execute commands in files that begin with `A_nidulans*`.

```bash
$ for file in A_nidulans*; do echo $file; done
A_nidulans_FGSC_A4_current_chromosomal_feature.tab
A_nidulans_FGSC_A4_current_chromosomes.fasta
A_nidulans_FGSC_A4_current_chromosomes.fasta.gz
```

Remember that `$` reflect to the given variable. If you omit `$`:

```bash
$ for file in A_nidulans*; do echo file; done
file
file
file
```

```bash
$ for file in output_*; do echo $file; echo ; head -10 $file; echo; done
output_A_nidulans_fasta_headers.txt

>ChrIII_A_nidulans_FGSC_A4 (3470897 nucleotides)
>ChrII_A_nidulans_FGSC_A4 (4070060 nucleotides)
>ChrIV_A_nidulans_FGSC_A4 (2887738 nucleotides)
>ChrI_A_nidulans_FGSC_A4 (3759208 nucleotides)
>ChrVIII_A_nidulans_FGSC_A4 (4934093 nucleotides)
>ChrVII_A_nidulans_FGSC_A4 (4550218 nucleotides)
>ChrVI_A_nidulans_FGSC_A4 (3407944 nucleotides)
>ChrV_A_nidulans_FGSC_A4 (3403833 nucleotides)

output_A_nidulans_filtered_features.txt

! File name: A_nidulans_FGSC_A4_version_s10-m04-r06_chromosomal_feature.tab
! Organism: Aspergillus nidulans FGSC A4
! Genome version: s10-m04-r06
! Date created: Thu Nov 30 13:46:18 2017
! Created by: The Aspergillus Genome Database (http://www.aspergillusgenome.org/)
! Contact Email: aspergillus-curator AT lists DOT stanford DOT edu
! Funding: NIAID at US NIH, grant number R01-AI077599-01
!
AN0004	pseudogene	ChrVIII_A_nidulans_FGSC_A4
AN0005	ORF|Uncharacterized	ChrVIII_A_nidulans_FGSC_A4
```

The more you practice on your own, and the more you struggle, the more you'll learn. So let's practice struggling! 

### Challenge!!

Now, you are on your own. Talk to your neighbor and ask your best friend *Google* whenever you have a burning question.

Download *A. flavus* genome:

```
http://www.aspergillusgenome.org/download/sequence/A_flavus_NRRL_3357/current/A_flavus_NRRL_3357_chromosomes.fasta.gz
```

Then, ultimately I want you to create a for loop that will execute the following on both *Asperillus* genomes:  
1. Print the file name.  
2. Print the number of fasta sequences followed by the word 'sequences'.  
3. Print all the fasta headers.  

My heart is soft... soft like a fungal mycelia, or room-temperature butter... Here goes the answer:

```
A_flavus_NRRL_3357_chromosomes.fasta
126 sequences

>1041045516887_A_flavus_NRRL_3357 (3491 nucleotides)
>1041045516889_A_flavus_NRRL_3357 (4170 nucleotides)
>1041045516890_A_flavus_NRRL_3357 (20049 nucleotides)
>1041045516891_A_flavus_NRRL_3357 (2076547 nucleotides)
(...)

A_nidulans_FGSC_A4_current_chromosomes.fasta
9 sequences

>ChrIII_A_nidulans_FGSC_A4 (3470897 nucleotides)
>ChrII_A_nidulans_FGSC_A4 (4070060 nucleotides)
>ChrIV_A_nidulans_FGSC_A4 (2887738 nucleotides)
>ChrI_A_nidulans_FGSC_A4 (3759208 nucleotides)
>ChrVIII_A_nidulans_FGSC_A4 (4934093 nucleotides)
>ChrVII_A_nidulans_FGSC_A4 (4550218 nucleotides)
>ChrVI_A_nidulans_FGSC_A4 (3407944 nucleotides)
>ChrV_A_nidulans_FGSC_A4 (3403833 nucleotides)
>mito_A_nidulans_FGSC_A4 (33227 nucleotides)
```

This is how the answer looks like. Now, this is a piece of cake! I made it super easy for you!