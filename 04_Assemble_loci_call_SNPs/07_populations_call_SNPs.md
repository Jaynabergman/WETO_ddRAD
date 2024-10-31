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

## Running populations
### First run populations without any filters to have a baseline comparions:

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
Folder called populations_nofilers (important files):  
1) populations.log - Tells you how many loci were removed (none in this case because no filters were used)
2) populations.snp.vcf - vcf file that has the genotypes per locus


## Get summary statistics using **vcftools**

### Flags
`--depth` Generates a file containing the mean depth per individual (.idepth)  
`--site-depth` Generates a file containing the depth per site summed across all individuals (.ldepth)  
`--site-mean-depth` Generates a file containing the mean depth per site average across all individuals (.ldepth.mean)  
`--geno-depth` Generates a file containing depth for each genotype in the vcf file (missing data is given -1) (.gdepth)  
`--het` Calculates a measure of heterozygosity on a per-individual basis (the inbreeding coefficient F is estimated for each individual) (.het)  
`--missing-indv` Generates a file reporting the missingness per individual (.imiss)  
`--missing-site` Generates a file reporting the missingness per site (.lmiss)

### Running the script
vcftools_ouputs.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=01:00:00
#SBATCH -o vcf_%A.out
#SBATCH -e vcf_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=jberg031@uottawa.ca



infile=$1
outfile=$2


module load vcftools


vcftools --vcf $infile --depth --out $outfile

vcftools --vcf $infile --site-mean-depth --out $outfile

vcftools --vcf $infile --site-depth --out $outfile

vcftools --vcf $infile --geno-depth --out $outfile

vcftools --vcf $infile --het --out $outfile

vcftools --vcf $infile --site-quality --out $outfile

vcftools --vcf $infile --missing-indv --out $outfile

vcftools --vcf $infile --missing-site --out $outfile
```

### Outputs
Separate files for each line. They can be downloaded to your desktop and brought into R.  

**See RMD** 


 
