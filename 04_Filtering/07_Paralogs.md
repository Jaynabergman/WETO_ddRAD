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

### Scripts

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
1. populations.snps.vcf - snps writen in vcf file format. 1,724,026 loci.
3. ref_map.log
```
gstacks output:
Read 294489690 BAM records:
  kept 272631482 primary alignments (93.3%), of which 137458482 reverse reads
  skipped 0 primary alignments with insufficient mapping qualities (0.0%)
  skipped 19593496 excessively soft-clipped primary alignments (6.7%)
  skipped 0 unmapped reads (0.0%)
  skipped some suboptimal (secondary/supplementary) alignment records

  Per-sample stats (details in 'gstacks.log.distribs'):
    read 6401949.8 records/sample (921290-13886986)
    kept 90.9%-94.0% of these

Built 1724026 loci comprising 135173000 forward reads and 58260792 matching paired-end reads; mean insert length was 204.9

Genotyped 1724026 loci:
  effective per-sample coverage: mean=5.0x, stdev=2.1x, min=1.8x, max=11.2x
  mean number of sites per locus: 175.6
  a consistent phasing was found for 2920488 of out 3102619 (94.1%) diploid loci needing phasing


populations output:
Removed 0 loci that did not pass sample/population constraints from 1724026 loci.
Kept 1724026 loci, composed of 307960945 sites; 0 of those sites were filtered, 3801185 variant sites remained.
    269566510 genomic sites, of which 30587842 were covered by multiple loci (11.3%).
Mean genotyped sites per locus: 175.61bp (stderr 0.05).

```
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
