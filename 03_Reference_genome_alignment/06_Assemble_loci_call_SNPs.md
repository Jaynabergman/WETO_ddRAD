# Assemble loci and call SNPs

## Background
We will use **STACKS** to assemble the loci and generate output files for additional filtering steps.  
  
**gstacks** is the first module used in the pipeline when there is alignments to a reference genome. This module is used to assemble the loci after the reads are aligned to the reference genome (done in previous step). *gstacks* will take the paired-end reads and identify SNPs for each locus and then genotype each individual at the identified SNPs.  

We then call SNPs using the module *populations*. This module is able to execute filtering options and computes population level statistics. Additionally, *populations* is able to generate a number of output files that are required for different programs used in the downstream anlayses. 

### Inputs
1. The aligned and sorted BAM file for each individual
   
2. Populations map (text file that indicates which population the individuals are in. Keep it as a single population unless there is prior evidence to indicate more than one population. In this case we have all the individuals in one population.)

### Flags
*gstacks*  
`I` Input directory that contains the BAM files  
`M` The population map with the list of samples in the BAM files    
`O` The output directory   
`--min-mapq` This is a flag for *gstacks* that determines the minimum mapping quality for a read to be considered.  

*populations*  
`P` Input directory that contains the BAM files (from gstacks)  
`M` The population map with the list of samples in the BAM files   
`O` The output directory  
`--vcf` Determines that the output will be SNPs in variant call format (vcf).

## Running scripts
First, to make the population map, in the folder with the bam files type in the command line: 
```
ls *.bam | sed 's/\.bam$//' | awk '{print $0 "\t1"}' > popmap.txt
```
### gstacks

gstacks.sh
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

~/local/bin/gstacks -I $infolder -M $popmap -O $outfolder --min-mapq 20

```
Command line:
```
sbatch ~/scripts/gstacks.sh BAM_WETO_plate2/ref_aligned/unfiltered_BAMs/ popmap.txt gstacks_plate2
```
### Outputs

### populations

populations_vcf.sh
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


