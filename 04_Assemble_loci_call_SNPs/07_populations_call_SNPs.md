# Call SNPs

## Background

We then call SNPs using the module *populations*. This module is able to execute filtering options and computes population level statistics. Additionally, *populations* is able to generate a number of output files that are required for different programs used in the downstream anlayses. 


### Inputs

### Flags
*populations no filters*  
`P` Input directory that contains the BAM files (from gstacks)  
`M` The population map with the list of samples in the BAM files   
`O` The output directory  
`--vcf` Determines that the output will be SNPs in variant call format (vcf).  

*populations filters*  
`--max-obs-het`  
`--min-samples-overall`  
`--min-mac`  
`--min-gt-depth`  
`--write-single-snp`  
`--fstats`  

## Running populations
1) First run populations without any filters to have a baseline comparions:

populations_nofilters_vcf.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=64GB
#SBATCH --time=1-12:00
#SBATCH --account=NAME
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

infolder=$1
popmap=$2
outfolder=$3

mkdir -p $outfolder

~/local/bin/populations -P $infolder -M $popmap -O $outfolder --vcf

```
Command line:
```
sbatch ~scripts/populations_nofilters_vcf.sh gstacks_plate2_Q20/ popmap.txt populations_nofi
lters
```
### Outputs

  
2. Run populations with the filters defined:

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
--max-obs-het $maxHO --min-samples-overall X --min-mac 3 --min-gt-depth X \
--write-single-snp --fstats -O $outfolder --vcf

```



 
