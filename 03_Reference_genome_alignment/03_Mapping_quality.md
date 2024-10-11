# Mapping quality of the reads to the reference genome

## Background

### Inputs

### Flags

## Running Samtools
1. Generating text file to run array script
  
In the command line:
```
 ls | awk -F'[_]' '{print $0, $1}' > BAM_filelist.txt
```

2. Running samtools as an array script  
  
array_samtools_mappingQ.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=128GB
#SBATCH --time=2-12:00
#SBATCH --account=NAME
#SBATCH --array=1-48
#SBATCH -o mapQ_%A_%a.out
#SBATCH -e mapQ_%A_%a.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

readinfo=`sed -n -e "$SLURM_ARRAY_TASK_ID p" $1`

IFS=' ' read -r -a readarray <<< "$readinfo"

inputbam=${readarray[0]}
output=${readarray[1]}

module load samtools

samtools view -f 0X2 -q 30 -b $inputbam -o $output.Q30.bam
```
Command line:
```
 sbatch ~/projects/def-leeyaw-ab/jbergman/scripts/array_samtools_mappingQ.sh BAM_filelist.txt
```

### Outputs

## Notes
