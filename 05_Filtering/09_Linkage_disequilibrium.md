# Linkage Disequilibrium

## Background

## Step 1: Plink

### Inputs

### Flags
`--vcf`  
`--double-id`  
`--allow-extra-chr`  
`--make-bed`  
`--indep-pairwise`  

### Script

1. Run plink to get prune in file

```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=01:00:00
#SBATCH -o plink_%A.out
#SBATCH -e plink_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL


module load StdEnv/2020
module load plink/1.9b_6.21-x86_64


infile=$1
outfile=$2


plink  --vcf $infile --double-id --allow-extra-chr --make-bed --out Temp_data

plink --bfile Temp_data --allow-extra-chr --set-missing-var-ids @:# --make-bed --out sorted_data

plink --bfile sorted_data --allow-extra-chr --pca --out $outfile
```
### Outputs

## Step 2: Vcftools

### Inputs

### Flags
`--gzvcf`  
`--snps`  
`--recode`  
`--recode-INFO-all`  
`--out`  

### Script

2. Run vcftools to get new vcf file with only prune in SNPs
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=01:00:00
#SBATCH -o vcf_%A.out
#SBATCH -e vcf_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL


infile=$1
list=$2
outfile=$3

module load vcftools

vcftools --gzvcf $infile --snps $list --recode --recode-INFO-all --out $outfile

```
### Outputs
