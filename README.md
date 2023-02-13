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
[tltyner@nova UNIX_Assignment]$ head -n 1 fang_et_al_genotypes.txt
[tltyner@nova UNIX_Assignment]$ head -n 1 transposed_genotypes.txt
[tltyner@nova UNIX_Assignment]$ head -n 1 snp_position.txt
[tltyner@nova UNIX_Assignment]$ grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
[tltyner@nova UNIX_Assignment]$ awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt
[tltyner@nova UNIX_Assignment]$ sort -k1,1 transposed_maize_genotypes.txt > transposed_maize_genotypes_sorted.txt
[tltyner@nova UNIX_Assignment]$ sort -k1,1 snp_position.txt > snp_position_sorted.txt
[tltyner@nova UNIX_Assignment]$ join -1 1 -2 1 -t $'\t' -a 1 snp_position_sorted.txt transposed_maize_genotypes_sorted.txt > combined_snp_genotype_maize.txt
[tltyner@nova UNIX_Assignment]$ cat combined_snp_genotype_maize.txt
[tltyner@nova UNIX_Assignment]$ sort -k3,3n combined_snp_genotype_maize.txt
[tltyner@nova UNIX_Assignment]$ awk -v OFS='\t' '{print $1, $3, $4, $2, $5, $6, $7, $8, $9, $10, $11, $12, $13, $14, $15}' combined_snp_genotype_maize.txt > combined_snp_genotype_maize_adjusted.txt
[tltyner@nova UNIX_Assignment]$ awk '$2 ~ /^1$/' combined_snp_genotype_maize_adjusted.txt >  snp_genotype_maize_chr1.txt

...
[tltyner@nova UNIX_Assignment]$ awk '$2 ~ /^10$/' combined_snp_genotype_maize.txt >  snp_genotype_maize_chr10.txt
[tltyner@nova UNIX_Assignment]$ sort -k3,3 snp_genotype_maize_chr1.txt > snp_genotype_maize_chr1_sorted.txt




ssh

```

transpose fang_et_al_genotypes.txt (columns to rows)
search the headers of fang_fang_et_al_genotypes.txt, transposed_genotypes.txt, and snp_position.txt to search for a common column/common values (Sample_ID and SNP_ID)


###Teosinte Data

```
[tltyner@nova UNIX_Assignment]$ head -n 1 fang_et_al_genotypes.txt
[tltyner@nova UNIX_Assignment]$ head -n 1 transposed_genotypes.txt
[tltyner@nova UNIX_Assignment]$ head -n 1 snp_position.txt
[tltyner@nova UNIX_Assignment]$ grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt
[tltyner@nova UNIX_Assignment]$ awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

```

Here is my brief description of what this code does
