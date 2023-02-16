# UNIX Assignment

## Data Inspection

### Attributes of `fang_et_al_genotypes`

```
$ cd UNIX_Assignment/
$ ls
fang_et_al_genotypes.txt  transpose.awk        UNIX_Assignment_Template.md
README.md                 UNIX_Assignment.md   UNIX_Assignment_Template.pdf
snp_position.txt          UNIX_Assignment.pdf
$ wc fang_et_al_genotypes.txt
    2783  2744038 11051939 fang_et_al_genotypes.txt
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
986
$ du -h fang_et_al_genotypes.txt
6.1M    fang_et_al_genotypes.txt

```

By inspecting this file I learned that:

1. 2783 lines, 2744038 words, 11051939 bytes
2. 6.1M for file size
3. 986 columns


### Attributes of `snp_position.txt`

```
$ wc snp_position.txt
  984 13198 82763 snp_position.txt
$ awk -F "\t" '{print NF; exit}' snp_position.txt
15
$ du -h snp_position.txt
38K     snp_position.txt

```

By inspecting this file I learned that:

1. 984 lines, 13198 words, 82763 bytes
2. 38K file size
3. 15 columns


## Data Processing

### Maize Data

```
$ grep -E "(Sample_ID|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
#separated those specific groups from the rest of the file
$ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt
#transposed columns to rows in order to match snp_position lines for join
$ sort -k1,1 transposed_maize_genotypes.txt > transposed_maize_genotypes_sorted.txt
#sorted by common column for join
$ sort -k1,1 snp_position.txt > snp_position_sorted.txt
#sorted by common column for join
$ join -1 1 -2 1 -t $'\t' -a 1 -a 2 snp_position_sorted.txt transposed_maize_genotypes_sorted.txt > combined_snp_genotype_maize.txt
#joined two files based on column 1, keeping data from each file that didn't have a matching line in the other (so no data was lost)
$ cat combined_snp_genotype_maize_test.txt
#check to make sure it worked
$ grep "Chromosome" combined_snp_genotype_maize.txt
#searched for which column Chromosome was in to be sure (headers got sorted with rest of columns)
$ sort -k3,3n combined_snp_genotype_maize.txt
#sorted by Chromosome
$ awk '$3 ~ /^1$/' combined_snp_genotype_maize.txt > snp_genotype_maize_chr1.txt
#extracted only data where column three matched the value of 1 (e.g. chromosome 1), placed into renamed file
$ sort -k4,4n snp_genotype_maize_chr1.txt > snp_genotype_maize_chr1_sorted.txt
#sorted data for chromosome 1 by increasing position values, placed into renamed file
$ cat snp_genotype_maize_chr1_sorted.txt
#check to make sure it worked
$ sort -k4,4nr snp_genotype_maize_chr1.txt > snp_genotype_maize_chr1_sorted_reverse.txt
#sorted data for chromosome 1 by decreasing position values, placed into renamed file
$ sed 's/?/-/g' snp_genotype_maize_chr1_sorted_reverse.txt > snp_genotype_maize_chr1_sorted_reverse_-.txt
#replaced '?' (e.g. missing data) with '-' and placed into renamed file
$ cat snp_genotype_maize_chr1_sorted_reverse_-.txt
#check to make sure it worked
#repeated lines 69-80 for chromosomes 2-10

$ awk '$4 ~ /^unknown$/' combined_snp_genotype_maize.txt > snp_position_unknown_maize.txt
#extracted SNPs with unknown positions into new file
$ cat snp_position_unknown_maize.txt
#check to see if it worked
$ awk '$4 ~ /^multiple$/' combined_snp_genotype_maize.txt > snp_position_multiple_maize.txt
#extracted SNPs with multiple positions into new file
$ cat snp_position_unknown_maize.txt 
#check to see if it worked
```

### Teosinte Data

```
$ grep -E "(Sample_ID|ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt 
#extracted groups
$ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt 
#transposed genotype data to match snp data
$ cat transposed_teosinte_genotypes.txt 
#checked to see if it worked
$ sort -k1,1 transposed_teosinte_genotypes.txt > transposed_teosinte_genotypes_sorted.txt 
#sort common column
$ join -1 1 -2 1 -t $'\t' -a 1 -a 2 snp_position_sorted.txt transposed_teosinte_genotypes_sorted.txt > combined_snp_genotype_teosinte.txt
#combine teosinte genotype and snp data
$ cat combined_snp_genotype_teosinte.txt 
#check to see if it worked
$ grep "Chromosome" combined_snp_genotype_teosinte.txt 
#double checking that chromosome didn't shift
$ awk '$3 ~ /^1$/' combined_snp_genotype_teosinte.txt > snp_genotype_teosinte_chr1.txt  
#extract chromosome 1 (repeat 2-10)
$ cat snp_genotype_teosinte_chr10.txt  
#check to see if it worked
$ sort -k4,4n snp_genotype_teosinte_chr1.txt > snp_genotype_teosinte_chr1_sorted.txt
#sort by increasing position values
$ sort -k4,4nr snp_genotype_teosinte_chr1.txt > snp_genotype_teosinte_chr1_reverse.txt
#sort by decreasing position values
$ sed 's/?/-/g' snp_genotype_teosinte_chr1_reverse.txt > snp_genotype_teosinte_chr1_reverse_-.txt
#replace '?' with '-' for missing data
$ cat snp_genotype_teosinte_chr10_reverse_-.txt 
#check to see if it worked

$ awk '$4 ~ /^unknown$/' combined_snp_genotype_teosinte.txt > snp_position_unknown_teosinte.txt
#extracted SNPs with unknown position into new file
$ cat snp_position_unknown_teosinte.txt
#check to see if it worked
$ awk '$4 ~ /^multiple$/' combined_snp_genotype_teosinte.txt > snp_position_multiple_teosinte.txt
#extracted SNPs with multiple positions into new file
$ cat snp_position_multiple_teosinte.txt 
#check to see if it worked
```

### Finishing Commands
```
$ mkdir and $ mv
#used to organize files
$ git commit -a -m "updated files"
#commit changes to files in Nova
$ git push
#push changes to this repository
```

### Notes
when joining my files, column 2 was not chromosome, instead it was cdv_marker. I didn't want to cut data that might be important and I couldn't find a code in help channels that easily rearranged columns.
