# Align reads to reference genome (and sort)

## Background
After the reference genome is indexed, we will use the program **BWA** to align the reads to the reference genome. **BWA mem** is the algorithm to use when you have paired-end Illumina data. 
 
When using **BWA mem** the read alignments come out in the order that they are listed in the Fastq file. Therefore we want to sort the mapped alignments. We will use the program **Samtools** to sort the SAM files into a *coordinate-ordered* format which means that the alignments will be sorted by how they apppear in the reference genome. **Samtools** will also convert the SAM files to BAM format (BAM is the compressed binary form of SAM). 

This script is written so the outputs from **BWA mem** are directly piped into **samtools** using `|`. This means that the SAM file from **BWA mem** dosen't have to be saved which saves memory space.  

Finally, this script is written as an array job. So we can submit each individual job at one time. 

### Inputs
1) Path to indexed reference genome
2) Path to read 1 
3) Path to read 2
<br>

**NOTE:** Because this script is written as an array job, we will genetate a text document that has tab separated columns which will indicate the paths to read1, read2, and the reference genome. This text document will also provide the sample ID and the reference information.

### Flags
**BWA:**  
`-t` Number of threads to use  
`-M` Mark shorter split hits as secondary (used for Picard compatibility)
`-R` Indicates the complete read group header line (read group information). @RG is the header tag, ID is the individual, SM is the sample (These two will be identicical if the individual is only sampled once), PL is the sequencing info.  
<br>
**Samtools:**  
`-@` Number of threads to use  
`-o` output file name

## Running BWA and Samtools
array_map_sort.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=128GB
#SBATCH --time=2-12:00
#SBATCH --account=NAME
#SBATCH -o map_rmdup_44_%A_%a.out
#SBATCH -e map_rmdup_44_%A_%a.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

readinfo=`sed -n -e "$SLURM_ARRAY_TASK_ID p" $1`

IFS=' ' read -r -a readarray <<< "$readinfo"

read_1=${readarray[0]}
read_2=${readarray[1]}
ref=${readarray[2]}
rg_info=${readarray[3]}
ref_info=${readarray[4]}

module load StdEnv/2020

module load bwa
module load samtools

bwa mem -t 4 -M -R $(echo "@RG\tID:$rg_info\tSM:$rg_info\tPL:Illumina") \
    $ref $read_1 $read_2 | samtools sort -@ 4 -o $rg_info.$ref_info.sort.bam 
```
command line
```
sbatch
```

### Outputs
BAM file for each individual sample.

## Notes
Normally at this step we would want to index the aligned reads (using **samtools**). However, we cannot index the alignments because the scaffolds in the western toad genome are too large to be built in the BAM format.  
