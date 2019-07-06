
### Practice for fq and fa files

How many reads in a fq.
```
sed -n '1~4 p' data/sr.chr1.2mb_1.fq | wc
```

Get sequences for all reads
```
sed -n '1~4 p' data/sr.chr1.2mb_1.fq | less
```
or with line number
```
sed -n '2~4 p' data/sr.chr1.2mb_1.fq | awk '{print NR" : "$0}' | less
```

Get information after '+' for each reads
```
cat data/sr.chr1.2mb_1.fq | paste - - - - | awk '{print $3"\t"$4}' | less
```


Check the number of reads whose sequence containing 'N'
```
sed -n '2~4 p' data/sr.chr1.2mb_1.fq | grep "N" | wc
```

Check the total number of bases in a fq.
```
sed -n '2~4 p' data/sr.chr1.2mb_1.fq | wc
```
The last number if the total number of bases.


Count the total number of 'N' in a fq.
```
cat data/sr.chr1.2mb_1.fq | paste - - - - | cut -f 2 | grep -o [N] | wc
```

Count the number of 'ATCGNatcg' at the first 100 reads.
```
cat data/sr.chr1.2mb_1.fq  | head -n 100 | paste - - - - | cut -f 2 | grep -o [ATCGNatcg] | sort | uniq -c
```

Convert fq to fa
```
cat data/sr.chr1.2mb_1.fq | paste - - - - | cut -f 1,2 | sed 's/^@/>/' | tr "\t" "\n" > sr.chr1.2mb_1.fa
```

Delete 'N' from sequences of reads
```
sed -e '/^[^>]/s/N//g' sr.chr1.2mb_1.fa | less
```

Only output reads whose sequence length larger than 65.
```
cat data/sr.chr1.2mb_1.fq | paste - - | awk '{if (length($2)>65) print $1"\n"$2}' | less
```


### Practice for bam/sam files

Get basic statistics from a bam
```
samtools stats data/chr1.2mb.mp2.bam | grep ^SN | cut -f 2-
```

Check reads which failed to align in a bam
```
samtools view data/chr1.2mb.mp2.bam | awk '{if (and($2,4)) print NR" : "$0}' | less
```

Find FLAG distribution in a bam
```
samtools view data/chr1.2mb.mp2.bam | cut -f 2 | sort | uniq -c
```
