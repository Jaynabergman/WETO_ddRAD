# Principal Component Analysis

## Background


pca_plink.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=01:00:00
#SBATCH -o plink_%A.out
#SBATCH -e plink_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=jberg031@uottawa.ca


module load StdEnv/2020
module load plink/1.9b_6.21-x86_64


infile=$1
outfile=$2


plink  --vcf $infile --double-id --allow-extra-chr --make-bed --out Temp_data

plink --bfile Temp_data --allow-extra-chr --set-missing-var-ids @:# --make-bed --out sorted_data

plink --bfile sorted_data --allow-extra-chr --pca --out $outfile
```
