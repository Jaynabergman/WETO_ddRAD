# Removing Paralogs 

## Background
Paralogs are two copies of the same region of the genome that occur as a result of genome duplication. These events can duplicate small or large sequences in the genome. It is important to identify and remove reads that are likely mapping to a paralogous region in the genome. The main program that we will use is **ngsParalog** which finds reads based on coverage and heterozygosity that are positioned in likely paralogous regions of the genome. By running this program we will remove reads that are mapping to a paralogous region that is not included in the reference genome.  

However, there are multiple steps that are required before and after running **ngsParalog** inorder to identify and then remove these reads (see overview of steps below).

### Overview of steps
1. **Generating SNPs position file (STACKS)** 
2. **Running ngsParalog** 
3. **Determine sites to include** 
4. **Running populations (STACKS)** 

## 1. Generating SNPs position file (STACKS)
To optimize the running of **ngsParalog** we need to generate a file that has the SNP positions. The SNPs position file is not necessary, but it will speed up the computation time. This will be done in **STACKS** using the *ref_map.pl*. The *ref_map.pl* is the **STACKS** pipeline that runs *gstacks* and then *populations*. This pipeline requires samples to be aligned to a reference genome.

### Inputs
1. Populations map (text file that indicates which population the individuals are in. Keep it as a single population unless there is prior evidence to indicate more than one population. In this case we have all the individuals in one population.)
  
2. The filtered BAM file for each individual

### Flags
`-X "populations: --vcf"` Defines the options from the populations portion of the pipeline. In this case the `--vcf` means that the output will be SNPs in variant call format (vcf).  
  
To make the population map, in the folder with the bam files type in the command line: 
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

~/local/bin/ref_map.pl -o $outfolder --popmap $popmap --samples $infolder  \
 -X "populations: --vcf"

```
Command line:
```
sbatch ~scripts/STACKS_ref_map_pl.sh ref_map_pl_output popmap.txt BAM_WETO_plate2/ref_aligned/Q30/
```
### Outputs



## 2. Running ngsParalog 
We are running **ngsParalog** using *calcLR* to calculate the likelihood ratio of mismapping reads covering each site. 

### Inputs

### Flags

### Outputs

## 3. Determine sites to include
We will use the R code provided by the authors of ngsParalog (CITE) to calculate p-values that indicate which SNPs should be included in the vcf file. This should leave us with SNPs that we are confident are not paralogs.

### Inputs

### Flags

### Outputs

## 4. Running populations (STACKS)
This is ran to generate a vcf file that only has SNPs that were determined in the previous steps (i.e. are not in paralogous regions of the genome).

### Inputs

### Flags

### Outputs


## Notes
