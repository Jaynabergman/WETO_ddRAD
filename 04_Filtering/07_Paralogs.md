# Removing Paralogs 

## Background
Paralogs are two copies of the same region of the genome that occur as a result of genome duplication. These events can duplicate small or large sequences in the genome. It is important to identify and remove reads that are likely mapping to a paralogous region in the genome. The main program that we will use is **ngsParalog** which finds reads based on coverage and heterozygosity that are positioned in likely paralogous regions of the genome. By running this program we will remove reads that are mapping to a paralogous region that is not included in the reference genome.  

However, there are multiple steps that are required before and after running **ngsParalog** inorder to identify and then remove these reads (see overview of steps below).

### Overview of steps
1. **Generating SNPs position file (STACKS)** 
2. **Running ngsParalog:** 
3. **Determine sites to include** 
4. **Running populations (STACKS)** 

## 1. Generating SNPs position file (STACKS)
To optimize the running of **ngsParalog** we need to generate a file that has the SNP positions. This will be done in **STACKS** using the *ref_map.pl*. The SNPs position file is not necessary, but it will speed up the computation time.

### Inputs

### Flags

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
