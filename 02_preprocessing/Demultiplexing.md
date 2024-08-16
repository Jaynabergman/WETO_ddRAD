#Demultiplexing
<br>
##Background
<br>
The fastq files that we have received from Genome Qubec represent a single multiplexed plate of sequences (see _Understanding.the.data_ for more details). The first thing we have to do to process the raw reads is demultiplex the reads in STACKS using the *process_radtags* program. This program will sort the raw reads using the unique barcodes to recover the individual samples from the library. 
<br>
### Inputs
1) The foward (R1) raw fastq.gz file
<br>
2) The reverse (R2) raw fastq.gz file
<br>
3) Barcode file: The program needs to be told which barcodes to expect. The barcodes will be specific for the enzyme pair that was used during library prep (this is where you will get the barcode list). The barcode file will be a text file (.txt) with one to two columns, separated by a tab. The first column is the barcodes and the second columm (optional) is for if you want to rename the output files. 
<br>
### Flags  
1) -1  R1 input file (fastq.gz)
2) -2  R2 input file (fastq.gz)
3)
