---
title: Answers!
weight: 5
chapter: false
pre: "<b>2.5 </b>"
---

### The answers 

Here you'll find the answers to the challenges in [2.4 Unix essentials](./unix_essentials)


+ **Pipes:**


**How many times `ORF|Uncharacterized` appears on the 2nd column?**

```bash
$ cat output_A_nidulans_filtered_features.txt | sed '1,8d' | cut -f 2 | grep -c 'ORF|Uncharacterized'
9312
```

**How many times `pseudogene` appears on the 2nd column?**

```bash
$ cat output_A_nidulans_filtered_features.txt | sed '1,8d' | cut -f 2 | grep -c 'pseudogene'
58
```

**Does the 1st column list each gene name (those named 'AN###') only once?**

```bash
$ cat A_nidulans_FGSC_A4_current_chromosomal_feature.tab | sed '1,8d' | cut -f 1 | grep 'AN' -c
10779
$ cat A_nidulans_FGSC_A4_current_chromosomal_feature.tab | sed '1,8d' | cut -f -1 | sort | uniq | grep 'AN' -c
10779
```

>Why a single and straight 'wc -l ' won't work?   
>Did you tail? There are genes that do not start with 'AN'.


**Which word appears 199 times on the 2nd column?**

```bash
$ cat output_A_nidulans_filtered_features.txt | sed '1,8d' | cut -f 2 | sort | uniq -c
 189 ORF|Merged/Split|Uncharacterized
   4 ORF|Merged/Split|Verified
9235 ORF|Uncharacterized
   1 ORF|Uncharacterized|Merged/Split
  49 ORF|Uncharacterized|transposable element gene
1209 ORF|Verified
   1 multigene locus
   1 ncRNA|Uncharacterized
   1 ncRNA|Verified
  55 pseudogene
   2 pseudogene|Verified
   1 pseudogene|transposable element gene
   2 rRNA|Uncharacterized
 199 tRNA|Uncharacterized
   9 tRNA|Verified
  27 uORF|Uncharacterized
   4 uORF|Verified
```

It's tRNA|Uncharacterized.



+ **For loop**:


Download *A. flavus* genome, *and unzip it*.

```bash
$ curl -O http://www.aspergillusgenome.org/download/sequence/A_flavus_NRRL_3357/current/A_flavus_NRRL_3357_chromosomes.fasta.gz
A_flavus_NRRL_3357_chromosomes.fasta.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 10.9M  100 10.9M    0     0  6618k      0  0:00:01  0:00:01 --:--:-- 6616k
```

```bash
$ gunzip A_flavus_NRRL_3357_chromosomes.fasta.gz
$ ls *flavus*
A_flavus_NRRL_3357_chromosomes.fasta
```

Then, ultimately I want you to create a for loop that will execute the following on both *Asperillus* genomes:  

1. Print the file name.  
2. Print the number of fasta sequences followed by the word 'sequences'.  
3. Print all the fasta headers.  

```bash
$ for file in A_*.fasta; do echo $file; a=`grep -c '>' $file`; echo $a sequences; echo ; grep '>' $file; echo; done
A_flavus_NRRL_3357_chromosomes.fasta
126 sequences

>1041045516887_A_flavus_NRRL_3357 (3491 nucleotides)
>1041045516889_A_flavus_NRRL_3357 (4170 nucleotides)
>1041045516890_A_flavus_NRRL_3357 (20049 nucleotides)
>1041045516891_A_flavus_NRRL_3357 (2076547 nucleotides)
>1041045516892_A_flavus_NRRL_3357 (3256 nucleotides)
>1041045516893_A_flavus_NRRL_3357 (2183 nucleotides)
>1041045516894_A_flavus_NRRL_3357 (2614 nucleotides)
>1041045516895_A_flavus_NRRL_3357 (2135 nucleotides)
>1041045516896_A_flavus_NRRL_3357 (2529 nucleotides)
>1041045516897_A_flavus_NRRL_3357 (2052 nucleotides)
>1041045516898_A_flavus_NRRL_3357 (2187 nucleotides)
>1041045516899_A_flavus_NRRL_3357 (2177 nucleotides)
>1041045516900_A_flavus_NRRL_3357 (2275 nucleotides)
>1041045516901_A_flavus_NRRL_3357 (2401 nucleotides)
>1041045516902_A_flavus_NRRL_3357 (2719 nucleotides)
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

**BONUS**

Redirect output of a for loop:

```bash
$ for file in A_*.fasta; do echo $file ; a=`grep -c '>' $file`; echo $a sequences; echo ; grep '>' $file; echo ; done > output_genomes_fasta_headers.txt
$ ls output*.txt
output_A_nidulans_fasta_headers.txt
output_A_nidulans_filtered_features.txt
output_genomes_fasta_headers.txt
$ cat output_genomes_fasta_headers.txt
(...)
```