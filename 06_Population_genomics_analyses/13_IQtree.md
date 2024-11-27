# Phylogenetic tree - IQTree

## Background
Phylogenetic trees can help to answer "deep phylogenetic" questions. We are using the program **IQTREE** to reconstruct a maximum likelihood phylogenetic tree. 

## vcf to fasta 
We need to convert the vcf file (from the step above) to a fasta file. We will use the pythong script and tutorial from the following website: https://github.com/edgardomortiz/vcf2phylip/blob/master/README.md.  

submit_python.sh
```
#!/bin/bash
#SBATCH -c 1
#SBATCH --mem=64GB
#SBATCH --account=NAME
#SBATCH --time=1-12:00
#SBATCH -o fasta_%A.out
#SBATCH -e fasta_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

module load python


inputfile=$1


python vcf2phylip.py --input $inputfile --phylip-disable --fasta
```
### Output
A fasta file with the same file name as the input vcf file.

## IQTree

### Input files
Fasta file generated from the filtered vcf file (steps above).

### Flags
`-s` Directory of input alignment files.  
`-m` This tells the program which models to assess for the model select. We use **MFP+ASC** which stands for **Model finder Plus and to apply ascertainment bias correction models** (ASC is needed when there is SNP data with only invariant sites).  
`-mtree` Indates for the program to perfrom full tree search for every model.  
`--seqtype` Indicates what type of data is in the input file. In this case we are specifying that we are using DNA data.  
`-B` Turns on ultrafast bootstap for the generated trees. This will generate confidence intervals for the branch support.  

### Running the program
```
#!/bin/bash
#SBATCH -c 4
#SBATCH --mem=28GB
#SBATCH --time=1-00:00:00
#SBATCH -o iqtree_%A.out
#SBATCH -e iqtree_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=jberg031@uottawa.ca


inputfile=$1

module load StdEnv/2020
module load gcc/9.3.0
module load iq-tree/2.2.2.7



iqtree2 -s $inputfile -m MFP+ASC -mtree --seqtype DNA -B 1000
```

### Output
- filename.iqtree (Full results of the run - this is the main report file)  
- filename.log (Log of the entire run)
- filename.treefile (Maximum likelihood tree in NEWICK format - need to use a treeview program (i.e. Figtree) to visualize the tree)
