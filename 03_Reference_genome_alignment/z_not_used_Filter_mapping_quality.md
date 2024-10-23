# Determine mapping quality of the reads to the reference genome

## Background
We can use the program **samtools** to look at BAM files, get summary statistics, and filter for mapping quality. In this case we are using **samtools** to filter for the reads that aligned to the reference genome with a specific quality score (i.e. filtering for mapping quality). Reads that have poor mapping quality may suggest that they are aligning to multiple places of the reference genome.  
  
This step should remove all reads from the negative control because we do not expect to have DNA from our species of interest (WETO) in the negative controls. It is a good idea to ensure that the negative controls have zero reads after running this step. The negative controls can then be removed.

### Inputs
1. The BAM files that you want to filter - in this case we are providing a list of BAM files to filter. 

### Flags
`-f` Indicates to include / keep reads that agree with the flag statement that follows  \
`0X2` (which follows the `-f` flag) indicates **PROPER_PAIR** so each sagment is properly aigned according to the aligner  \
`-q` Skips alignments that are mapped with quality scores (Phred scores) less than the value that is indicated  \
`-b` Indicates that the output will be in the BAM format  \
`-o` Determines the output file name

## Running Samtools
1. Generating text file to run array script. This can be done in the command line in the folder where the BAM files are. 
  
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

samtools view -f 0X2 -q 30 -b -o $output.Q30.bam $inputbam
```
Command line:
```
 sbatch ~/projects/def-leeyaw-ab/jbergman/scripts/array_samtools_mappingQ.sh BAM_filelist.txt
```

### Outputs
1. BAM file for each individual that contains only the reads that were mapped with equal to or greater than the quality score specified (e.x. AS-3-DNA80.WETO.Q30.bam)

## Read Counts
We need to determine how many reads were kept after filtering for the mapping qualtiy. We will use the same script as previously used to count the reads that were aligned to the reference genome.  

bam_file_counts.sh
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=8GB
#SBATCH --time=05:00:00
#SBATCH --account=NAME
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL


module load samtools

input_files="/home/jbergman/projects/def-leeyaw-ab/jbergman/plate2/BAM_WETO_plate2/ref_aligned/Q30/"

output_file="read_count.txt"

for bam_file in "$input_files"/*.bam; do

        filename=$(basename "$bam_file" .bam)

        read_count=$(samtools view -c "$bam_file")

        echo "$filename $read_count" >> "$output_file"

done
```
### Outputs
A text file with the sample names and the number of reads that were kept after filtering for the mapping quality (done for both Q=20 and Q=30).  

**Total number of reads kept (Q20) = 310,998,339**  
**Total number of reads kept (Q30) = 294,489,690**

## Notes
- Mapping quality was done twice: once to filter for Q>=20 and once for Q>=30.
- Negative controls have zero reads


