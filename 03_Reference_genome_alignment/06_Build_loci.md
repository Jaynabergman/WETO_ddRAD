# Assemble loci and call SNPs

## Background
We will use the *ref_map.pl* in **STACKS** to assemble the loci and generate output files for additional filtering steps.  
  
The first module in the pipeline is *gstacks* which is used to assemble the loci after the reads are aligned to the reference genome (done in previous step). *gstacks* will take the paired-end reads and identify SNPs for each locus and then genotype each individual at the identified SNPs.  

Using the *ref_map.pl* the *gstacks* outputs are directly piped into the next module in **STACKS** which is *populations*. This module is able to execute filtering options and computes population level statistics. Additionally, *populations* is able to generate a number of output files that are required for different programs used in the downstream anlayses. 

### Inputs
1. The aligned and sorted BAM file for each individual
   
2. Populations map (text file that indicates which population the individuals are in. Keep it as a single population unless there is prior evidence to indicate more than one population. In this case we have all the individuals in one population.)

### Flags
`--min-mapq` This is a flag for *gstacks* that determines the minimum mapping quality for a read to be considered.  

`-X "populations: --vcf"` Defines the options from the populations portion of the pipeline. In this case the `--vcf` means that the output will be SNPs in variant call format (vcf).

## Running ref_map.pl
First, to make the population map, in the folder with the bam files type in the command line: 
```
ls *.bam | sed 's/\.bam$//' | awk '{print $0 "\t1"}' > popmap.txt
```
STACKS_ref_map_pl.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=64GB
#SBATCH --time=2-12:00
#SBATCH --acount=NAME
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL


outfolder=$1
popmap=$2
infolder=$3

mkdir -p $outfolder

~/local/bin/ref_map.pl --min-mapq 20 -o $outfolder --popmap $popmap --samples $infolder  \
 -X "populations: --vcf"
```
Command line:
```

```
### Outputs

