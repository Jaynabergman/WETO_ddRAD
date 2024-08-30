# FastP

## Background
After the reads are demultiplexed, we have Fastq files for each individual. These Fastq files need to go through preprocessing steps to ensure quality control before they can be used for any downstream analysis. These steps typically include 1) trimming adapters, and 2) prunning data that does not meet set quality thresholds. To process the fastq files in a single step we will use the program **fastp**.  \
  \
The following script is written as an array job which allows many jobs to be submitted at once through a single script.     
  
### Inputs
1. The demultiplexed fastq files for each individual.
2. A text file that has a list of the individual files. This is needed to submit the script as an array job.
### Flags
`-i` Read1 input file name  \
`-I` Read2 input file name  \
`-o` Read1 output file name  \
`-O` Read2 output file name  \
`-j` Saves the output as a json format and sets the file name.  \
  \
`f` This value indicates how many bases to trim at the front end of read1. This is needed when there is restriction site contamination on the reads. To know which value to set this to, manually look at the demultiplexed read files and remove the number of bases that are identical at the beginning of all the reads. This value was set to **5**.  \
  \
`F` This value indicates how many bases to trim at the front end of read2 (similar to `f`). This value was set to **5**. **Should this always be identical to -f?**  **Why is tail trimming not needed?**  \
  \
`--dedup` This flag enables deduplication so duplicated reads/pairs of reads are dropped.  \
  \
`--dup_calc_accuracy` This sets the accuracy level used to calculate duplication. The value can range between 1 to 6, with 6 being the highest level of accuracy (which also uses more memory and will take more time to run).  \
  \
`-l` This is the minimum length that reads are required to be. Reads are discarded if they are short than this. **How do you decided on this value?**  \
  \
`-p` Enables overrepresented sequence analysis  \
  \
`-P` **I dont understand this**  \
  \
`--trim_poly_g` This enforces that polyG tail trimming is turned on. This is important to enable for Illumina NovaSeq data (turned on by default for Illumina data), because read tails may have access Gs since G means no signal in the Illumina two-color systems. The default length is 10 to detect a polyG tail.   \
  \
`--cut_right` Moves the sliding window from the front of the read to the tail when assessing read quality. If the window reaches quality that is below one of the given thresholds, than the bases in the window and to the right of the window will be dropped. 

## Running Fastp
Create a text file with the list of demultiplexed files:
```
#!/bin/bash

ls plate1_demultiplex_out/*.fq.gz | grep -v "rem\." > samples_files_list.txt

exec 3< samples_files_list.txt
exec 4> fastp_files_list.txt

# Read each line from the input file
while IFS= read -r file1_path <&3 && IFS= read -r file2_path <&3; do

    # Extract the base filenames
    file1=$(basename "$file1_path")
    file2=$(basename "$file2_path")

    # Extract the common part from the filenames
    common=$(echo "$file1" | sed 's/_.*//')

    # Output the formatted line with filenames on the same line
    echo "$file1_path $file2_path $common" >&4

done

exec 3<&-
exec 4>&-
```

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
For every individual there will be:
1) sample_name.json
