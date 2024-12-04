# Phylogenetic tree - BEAST2 (using snapper)

## Background

## Creating xml file
1. vcf file to nexus
2. nexus to xml in BEAUti GUI on desk top



## Running snapper in BEAST2

### Script
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=64GB
#SBATCH --time=6-12:00:00
#SBATCH --account=NAME
#SBATCH -o beast_%A.out
#SBATCH -e beast_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL


module load StdEnv/2020
module load beast/2.7.5


inputfile=$1


beast $inputfile

```
