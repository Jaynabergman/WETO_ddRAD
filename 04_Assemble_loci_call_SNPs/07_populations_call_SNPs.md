# Call SNPs

## Background

We then call SNPs using the module *populations*. This module is able to execute filtering options and computes population level statistics. Additionally, *populations* is able to generate a number of output files that are required for different programs used in the downstream anlayses. 


### Inputs

### Flags
*populations*  
`P` Input directory that contains the BAM files (from gstacks)  
`M` The population map with the list of samples in the BAM files   
`O` The output directory  
`--vcf` Determines that the output will be SNPs in variant call format (vcf).

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

```
### Outputs
