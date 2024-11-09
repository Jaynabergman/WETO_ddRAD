# Call SNPs

## Background

We then call SNPs using the module *populations*. This module is able to execute filtering options and computes population level statistics. Additionally, *populations* is able to generate a number of output files that are required for different programs used in the downstream anlayses. 


### Inputs

### Flags
*populations no filters*  
`P` Input directory that contains the BAM files (from gstacks)  
`M` The population map with the list of samples in the BAM files   
`--max-obs-het` Specifies the maximum observed heterozygosity required to process a nucleotide site at a locus  
`O` The output directory  
`--vcf` Determines that the output will be SNPs in variant call format (vcf).  

## Running populations


  
populations_filters.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=10:00:00
#SBATCH --account=NAME
#SBATCH -o pops_%A.out
#SBATCH -e pops_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

infolder=$1
popmap=$2
outfolder=$3
maxHO=$4

mkdir $outfolder

~/local/bin/populations -P $infolder -M $popmap  \
 --max-obs-het $maxHO -O $outfolder --vcf

```
Command line:
```
sbatch ~scripts/populations_nofilters_vcf.sh gstacks_plate2_Q20/ popmap.txt populations_HO-0.6 0.6
```
### Outputs
Folder called populations_HO-0.6 (one for each observed Het setting) (important files):  
1) populations.log - Tells you how many varient sites (SNPs) were removed (56,377 for a setting of 0.6)
2) populations.snp.vcf - vcf file that has the genotypes per locus

## NOTES

We ran populations while testing different values of max heterozygosity (0.6, 0.7, 0.8). Regardless of the max observed heterozygosity setting, we got the same clustering patterns of the individuals (see sup map for PCA plots).
