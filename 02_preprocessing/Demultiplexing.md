# Demultiplexing  
  
## Background  
  
The fastq files that we have received from Genome Qubec represent a single multiplexed plate of sequences (see *Understanding_the_data* for more details). The first thing we have to do to process the raw reads is demultiplex the reads in STACKS using the **process_radtags** program. This program will sort the raw reads using the unique barcodes to recover the individual samples from the library.   
  
### Inputs   
1) The foward (R1) raw fastq.gz file
2) The reverse (R2) raw fastq.gz file
3) Barcode file: The program needs to be told which barcodes to expect. The barcodes will be specific for the enzyme pair that was used during library prep (this is where you will get the barcode list). The barcode file will be a text file (.txt) with one to two columns, separated by a tab. The first column is the barcodes and the second columm (optional) is for if you want to rename the output files.
  
### Flags  
1) -o
   - path to output folder
2) -1
   - R1 input file (fastq.gz)
5) -2  R2 input file (fastq.gz)
6) -b  barcode file
7) --renz-1  first restriction enzyme used in library prep
8) --renz-2  second restiction enzyme used in library prep
9) --inline-null  Indicates that the barcodes are only on the foward read and is inline with the sequence
10) -r  rescues barcodes and RAD-Tag cut sites (what does this mean?)
11) -c  cleans data by removing any read that has an uncalled base
12) -q  discards reads with low quality scores (Threshold is a Phred score of 10)
13) -D  writes a file with the discarded reads so we don't lose this information
