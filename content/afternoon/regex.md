---
title: Regular expressions
weight: 2
chapter: false
pre: "<b>3.2 </b>"
---


## Contents

- [Basic awk](#basic-awk-sed)
- [Basic sed](#basic-sed)
- [sort, uniq, cut, etc.](#sort-uniq-cut-etc)
- [GFF3 Annotations](#gff3-annotations)
- [Other generally useful aliases for your .bashrc](#other-generally-useful-aliases-for-your-bashrc)



## Basic awk

[[back to top](#contents)]

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
	
Remove spaces in fasta headers

	sed 's, ,_,g' -i FASTA_file

## Pipes with AWK

[[back to top](#contents)]

Returns all lines on Chr 1 between 1MB and 2MB in file.txt. (assumes) chromosome in column 1 and position in column 3 (this same concept can be used to return only variants that above specific allele frequencies):

    cat file.txt | awk '$1=="1"' | awk '$3>=1000000' | awk '$3<=2000000'


Basic sequence statistics. Print total number of reads, total number unique reads, percentage of unique reads, most abundant sequence, its frequency, and percentage of total in file.fq:

    cat myfile.fq | awk '((NR-2)%4==0){read=$1;total++;count[read]++}END{for(read in count){if(!max||count[read]>max) {max=count[read];maxRead=read};if(count[read]==1){unique++}};print total,unique,unique*100/total,maxRead,count[maxRead],count[maxRead]*100/total}'



Convert a VCF file to a BED file
sed -e 's/chr//' file.vcf | awk '{OFS="\t"; if (!/^#/){print $1,$2-1,$2,$4"/"$5,"+"}}'


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


Untangle an interleaved paired-end FASTQ file. If a FASTQ file has paired-end reads intermingled, and you want to separate them into separate /1 and /2 files, and assuming the /1 reads precede the /2 reads:

    cat interleaved.fq |paste - - - - - - - - | tee >(cut -f 1-4 | tr "\t" "\n" > deinterleaved_1.fq) | cut -f 5-8 | tr "\t" "\n" > deinterleaved_2.fq

Take a fasta file with a bunch of short scaffolds, e.g., labeled `>Scaffold12345`, remove them, and write a new fasta without them:

    samtools faidx genome.fa && grep -v Scaffold genome.fa.fai | cut -f1 | xargs -n1 samtools faidx genome.fa > genome.noscaffolds.fa

	
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


Print length of each gene in a GFF3 file.

    grep $'\tgene\t' yourannots.gff3 | cut -s -f 4,5 | perl -ne '@v = split(/\t/); printf("%d\n", $v[1] - $v[0] + 1)'


FASTA header lines to GFF format (assuming the length is in the header as an appended "\_length" as in [Velvet](http://www.ebi.ac.uk/~zerbino/velvet/) assembled transcripts):

    grep '>' file.fasta | awk -F "_" 'BEGIN{i=1; print "##gff-version 3"}{ print $0"\t BLAT\tEXON\t1\t"$10"\t95\t+\t.\tgene_id="$0";transcript_id=Transcript_"i;i++ }' > file.gff
