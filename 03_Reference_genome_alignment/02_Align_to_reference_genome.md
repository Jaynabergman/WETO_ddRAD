# Align reads to reference genome

## Background

### Inputs

### Flags

## Running alignment
array_map.sh
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
#module load picard/2.26.3
module load samtools

bwa mem -t 4 -M -R $(echo "@RG\tID:$rg_info\tSM:$rg_info\tPL:Illumina") \
    $ref $read_1 $read_2 | samtools sort -@ 4 -o $rg_info.$ref_info.sort.bam -
```

### Outputs

## Notes
Normally at this step we would want to index the aligned reads (using **samtools**). However, in this case for the western toad genome we cannot index the alignments because the scaffolds are too large to be build in the BAM format.  
