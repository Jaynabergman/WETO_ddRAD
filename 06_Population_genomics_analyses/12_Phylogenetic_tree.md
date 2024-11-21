# Phylogenetic tree

## Background

## vcf to fasta 
https://github.com/edgardomortiz/vcf2phylip/blob/master/README.md

## IQTree

```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=1-00:00:00
#SBATCH -o iqtree_%A.out
#SBATCH -e iqtree_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

inputfile=$1

module load iqtree

iqtree -s $inputfile -m MFP -mtree
```
