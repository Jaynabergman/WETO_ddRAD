# Demultiplexing  
  
## Background  
  
The fastq files that we have received from Genome Qubec represent a single multiplexed plate of sequences (see *Project and data overview* for more details). The first thing we have to do to process the raw reads is demultiplex the reads in STACKS using the **process_radtags** program. This program will sort the raw reads using the unique barcodes to recover the individual samples from the library.   
  
### Inputs   
1) The foward (R1) raw fastq.gz file from Genome Quebec
2) The reverse (R2) raw fastq.gz file from Genome Quebec
3) Barcode file: The program needs to be told which barcodes to expect. The barcodes will be specific for the enzyme pair that was used during library prep (Barcode list is provided by Laval). The barcode file will be a text file (.txt) with one or two columns that are separated by a tab. The first column is the individual barcodes and the second column (optional) is used to rename the output files.
  
### Flags  
`-o` path to output folder  
`-1` R1 input file (fastq.gz)  
`-2` R2 input file (fastq.gz)  
`-b` barcode file  
`--renz-1` first restriction enzyme used in library prep  
`--renz-2` second restiction enzyme used in library prep  
`--inline-null` indicates that the barcodes are only on the foward read and is inline with the sequence  
`-r` rescues barcodes and RAD-Tag cut sites (if there are sequencing errors they get corrected so they can be properly recognized or they are discareded)  
`-c` cleans data by removing any read that has an uncalled base  
`-q` discards reads with low quality scores (default threshold is a Phred score of 10)  
`-D` writes a file with the discarded reads so we don't lose this information (output files with *.rem*)
  
## Running process_radtags
process_radtags.sh

```
#!/bin/bash
#SBATCH -c 1
#SBATCH --mem=64GB
#SBATCH --account=def-leeyaw-ab
#SBATCH --time=2-12:00
#SBATCH -o proc_radt_%A.out
#SBATCH -e proc_radt_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=jberg031@uottawa.ca

r1file=$1
r2file=$2
outfolder=$3
barcodes=$4
renz1=$5
renz2=$6

mkdir -p $outfolder

~/local/bin/process_radtags --threads 1 -o $outfolder -1 $r1file -2 $r2file \
-b $barcodes --renz-1 $renz1 --renz-2 $renz2 --inline-null -r -c -q -D

```
command line
```
sbatch scripts/process_radtags.sh WETO_plate1_rawdata/NS.LH00487_0009.007.D701---B503.LeeYaw_WETO_plate1_R1.fastq.gz WETO_plate1_rawdata/NS.LH00487_0009.007.D701---B503.LeeYaw_WETO_plate1_R2.fastq.gz process_radtags WETO_plate1_rawdata/WETO_plate1_barcodes.txt SbfI MspI
```
### Outputs
**process_radtags.log**:
   This has important summary information like *total_raw_read_counts* and *per_barcode_raw_read_counts*. See below the *total_raw_read_counts*:
```
Total Sequences         235868790
Barcode Not Found       647230         0.3%
Low Quality             1804214        0.8%
RAD Cutsite Not Found   1567175        0.7%
Retained Reads          231850171     98.3%
Properly Paired         114362730     97.0%
```
Four files for each individual:
1) sample_name.1.fq (forward reads that will be kept)
2) sample_name.2.fq (reverse reads that will be kept)
3) sample_name.rem.1.fq (forward reads that were removed because they didn't meet the flag requirements)
4) sample_name.rem.2.fq (reverse reads that were removed because they didn't meet the flag requirements)
