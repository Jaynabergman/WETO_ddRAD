# Removing Paralogs 

## Background
Paralogs are two copies of the same region of the genome that occur as a result of genome duplication. These events can duplicate small or large sequences in the genome. It is important to identify and remove reads that are likely mapping to a paralogous region in the genome. The main program that we will use is **ngsParalog** which finds reads based on coverage and heterozygosity that are positioned in likely paralogous regions of the genome. By running this program we will remove reads that are mapping to a paralogous region that is not included in the reference genome.  

However, there are multiple steps that are required before and after running **ngsParalog** inorder to identify and then remove these reads (see overview of steps below).

### Overview of steps
1. **Running ngsParalog** 
2. **Determine sites to include** 
3. **Running populations (STACKS)** 

## 1. Generating SNPs position file (STACKS)
To optimize the running of **ngsParalog** we need to generate a file that has the SNP positions. The SNPs position file is not necessary, but it will speed up the computation time. This will be done in **STACKS** using the *ref_map.pl*. The *ref_map.pl* is the **STACKS** pipeline that runs *gstacks* and then *populations*. This pipeline requires samples to be aligned to a reference genome.


### Modifying SNP vcf file for ngsParalog input
1. Extract locus and base pair position from the vcf file into a text file.  
  
In command line:
```
grep -v "^#" populations.snps.vcf | cut -f1,2 > snp_positions.txt
```
2. Create a text file with the list of input BAM files (will use the BAM files that were filtered for Q>=30)

In command line:
```
find BAM_WETO_plate2/ref_aligned/Q30 -type f > BAM_list_plate2.txt
```
## 2. Running ngsParalog 
We are running **ngsParalog** using *calcLR* to calculate the likelihood ratio of mismapping reads covering each site. This program requires the input files to be in the **samtools mpileup** format. 

### Inputs
1. Text file with the list of BAM files (one for each individual) and the path to each BAM file
2. SNP position text file (created in the previous step)

### Flags
samtools:  
`-b`
`-l`
`-q`
`-Q`
`--ff`

ngsParalog:  
`-infile`
`-outfile`
`minQ`
`-minind`
`-mincov`

### Scripts 
Create a text file with the locus **SHOULD THIS BE LOCUS OR SNP**  and base pair positions by typing in the command line:
```
grep -v "^#" populations.snps.vcf | cut -f1,2 > snp_positions.txt
```
run_ngsParalog.sh
```
#!/bin/bash
#SBATCH -c 1
#SBATCH --mem=128GB
#SBATCH --time=6-12:00
#SBATCH -o ngsParalog_%A.out
#SBATCH -e ngsParalog_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=jberg031@uottawa.ca

bamlist=$1
SNPpositions=$2
outfile=$3
minimumind=$4

module load samtools

samtools mpileup -b $bamlist -l $SNPpositions -q 0 -Q 0 --ff UNMAP,DUP \
        | ~/local/software/ngsParalog/ngsParalog calcLR -infile - \
        -outfile $outfile -minQ 20 -minind $minimumind -mincov 1
```
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
