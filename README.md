#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`

```
[tltyner@nova assignments]$ cd UNIX_Assignment/
[tltyner@nova UNIX_Assignment]$ ls
fang_et_al_genotypes.txt  transpose.awk        UNIX_Assignment_Template.md
README.md                 UNIX_Assignment.md   UNIX_Assignment_Template.pdf
snp_position.txt          UNIX_Assignment.pdf
[tltyner@nova UNIX_Assignment]$ wc fang_et_al_genotypes.txt
    2783  2744038 11051939 fang_et_al_genotypes.txt
[tltyner@nova UNIX_Assignment]$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
986
[tltyner@nova UNIX_Assignment]$ du -h fang_et_al_genotypes.txt
6.1M    fang_et_al_genotypes.txt

```

By inspecting this file I learned that:

1. 2783 lines, 2744038 words, 11051939 bytes
2. 6.1M for file size
3. 986 columns


###Attributes of `snp_position.txt`

```
[tltyner@nova UNIX_Assignment]$ wc snp_position.txt
  984 13198 82763 snp_position.txt
[tltyner@nova UNIX_Assignment]$ awk -F "\t" '{print NF; exit}' snp_position.txt
15
[tltyner@nova UNIX_Assignment]$ du -h snp_position.txt
38K     snp_position.txt

```

By inspecting this file I learned that:

1. 984 lines, 13198 words, 82763 bytes
2. 38K file size
3. 15 columns


##Data Processing

###Maize Data

```
[tltyner@nova UNIX_Assignment]$ grep -E "(Sample_ID|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
#separated those specific groups from the rest of the file
[tltyner@nova UNIX_Assignment]$ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt
#transposed columns to rows in order to match snp_position lines for join
[tltyner@nova UNIX_Assignment]$ sort -k1,1 transposed_maize_genotypes.txt > transposed_maize_genotypes_sorted.txt
#sorted by common column for join
[tltyner@nova UNIX_Assignment]$ sort -k1,1 snp_position.txt > snp_position_sorted.txt
#sorted by common column for join
[tltyner@nova UNIX_Assignment]$ join -1 1 -2 1 -t $'\t' -a 1 -a 2 snp_position_sorted.txt transposed_maize_genotypes_sorted.txt > combined_snp_genotype_maize.txt
#joined two files based on column 1, keeping data from each file that didn't have a matching line in the other (so no data was lost)
[tltyner@nova UNIX_Assignment]$ cat combined_snp_genotype_maize_test.txt
#check to make sure it worked
[tltyner@nova UNIX_Assignment]$ grep "Chromosome" combined_snp_genotype_maize.txt
#searched for which column Chromosome was in to be sure (headers got sorted with rest of columns)
[tltyner@nova UNIX_Assignment]$ sort -k3,3n combined_snp_genotype_maize.txt
#sorted by Chromosome
[tltyner@nova UNIX_Assignment]$ awk '$3 ~ /^1$/' combined_snp_genotype_maize.txt > snp_genotype_maize_chr1.txt
#extracted only data where column three matched the value of 1 (e.g. chromosome 1), placed into renamed file
[tltyner@nova UNIX_Assignment]$ sort -k4,4n snp_genotype_maize_chr1.txt > snp_genotype_maize_chr1_sorted.txt
#sorted data for chromosome 1 by increasing position values, placed into renamed file
[tltyner@nova UNIX_Assignment]$ cat snp_genotype_maize_chr1_sorted.txt
#check to make sure it worked
[tltyner@nova UNIX_Assignment]$ sort -k4,4nr snp_genotype_maize_chr1.txt > snp_genotype_maize_chr1_sorted_reverse.txt
#sorted data for chromosome 1 by decreasing position values, placed into renamed file
[tltyner@nova UNIX_Assignment]$ sed 's/?/-/g' snp_genotype_maize_chr1_sorted_reverse.txt > snp_genotype_maize_chr1_sorted_reverse_-.txt
#replaced '?' (e.g. missing data) with '-' and placed into renamed file
[tltyner@nova UNIX_Assignment]$ cat snp_genotype_maize_chr1_sorted_reverse_-.txt
#check to make sure it worked
#repeated lines 69-80 for chromosomes 2-10




###Teosinte Data

```
[tltyner@nova UNIX_Assignment]$ head -n 1 fang_et_al_genotypes.txt
[tltyner@nova UNIX_Assignment]$ head -n 1 transposed_genotypes.txt
[tltyner@nova UNIX_Assignment]$ head -n 1 snp_position.txt
[tltyner@nova UNIX_Assignment]$ grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt
[tltyner@nova UNIX_Assignment]$ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

```

Here is my brief description of what this code does
