# FastP

## Background

### Inputs

### Flags

## Running Fastp
array_fastp.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=32GB
#SBATCH --time=0-8:00
#SBATCH --account=NAME
#SBATCH -o arr_fastp_%A_%a.out
#SBATCH -e arr_fastp_%A_%a.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

module load StdEnv/2020
module load fastp/0.23.4

readinfo=`sed -n -e "$SLURM_ARRAY_TASK_ID p" $1`

IFS=' ' read -a namearr <<< $readinfo

fastp -f 5 -F 5 --dedup --dup_calc_accuracy 6 -l 50 -p -P 1 --trim_poly_g \
      --cut_right --thread 4 -i ${namearr[0]} \
      -I ${namearr[1]} -o ${namearr[0]%.fastq.gz}_T.fastq.gz \
      -O ${namearr[1]%.fastq.gz}_T.fastq.gz -h ${namearr[2]}.html \
	  -j ${namearr[2]}.fastp.json
```
command line
```
sbatch
```
### Outputs
