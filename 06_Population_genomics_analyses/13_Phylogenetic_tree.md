# Phylogenetic tree

## Background

## Filter out variant sites 
In order to run IQTree with the ASC flag, which tells the models that there are only invariant sites, we need to remove any sites that resulted as variant sites during the filtering process (i.e. could have occurred due to removing individuals).  

We will do this in the R package in SNPfiltR using the function `min.mac(mac=1)`. 

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
#SBATCH --mail-user=jberg031@uottawa.ca


inputfile=$1


module load intel/2020.1.217
module load StdEnv/2020
module load iq-tree



iqtree2 -s $inputfile -m MFP -mtree
```
