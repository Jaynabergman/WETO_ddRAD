# Removing Paralogs 

## Background
Paralogs are two copies of the same region of the genome that occurr as a result of genome duplication. These events can duplicate small or large sequences in the genome. It is important to identify and remove reads that are likely mapping to a paralogous region in the genome. The main program that we will use is **ngsParalog** which finds reads based on coverage and heterozygosity that are positioned in likely paralogous regions of the reference genome. By running this program we will remove reads that are mapping to a paralogous region that is not included in the reference genome.  

However, there are multiple steps that are required before and after running **ngsParalog** inorder to identify and then remove these reads (see overview of steps below).

### Overview of steps
1. **Generating SNPs position file (STACKS):** To optimize the running of **ngsParalog** we need to generate a file that has the SNP positions. This will be done in **STACKS** using the *ref_map.pl*. The SNPs position file is not necessary, but it will speed up the computation time.
  
2. **Running ngsParalog:** 
  
3. **Determine sites to include:** in R we will use the code provided by the authors of ngsParalog to calculate p-values to indicate which SNPs should be included in the 
  
4. **Running populations (STACKS):**

## 1. Generating SNPs position file (STACKS)

### Inputs

### Flags

### Outputs

## 2. Running ngsParalog 

### Inputs

### Flags

### Outputs

## 3. Determine sites to include

### Inputs

### Flags

### Outputs

## 4. Running populations (STACKS)

### Inputs

### Flags

### Outputs


## Notes
