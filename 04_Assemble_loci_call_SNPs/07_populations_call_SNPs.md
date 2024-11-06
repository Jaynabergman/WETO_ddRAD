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

We will run populations while testing different values of max heterozygosity (0.6, 0.7, 0.8)  
  
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
Folder called populations_HO-0.6 (one for each HE setting) (important files):  
1) populations.log - Tells you how many varient sites (SNPs) were removed (56,377 for a setting of 0.6)
2) populations.snp.vcf - vcf file that has the genotypes per locus


## NOT USED: Get summary statistics using **vcftools**

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
