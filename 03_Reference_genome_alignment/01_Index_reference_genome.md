# Index the reference genome

## Background

In order to efficiently use a reference genome we first need to create an index for it. Indexing a genome can be equated to indexing a chapter book where page numbers indicate the start of each chapter. It is much more efficient and faster to use an index to narrow down the origin of a sequence within a genome than having the aligner go through the entire genome. Indexing a genome can take a while computationally, but you only need to do this once for each reference genome. We will use the program **Burrows-Wheeler Alignment Tool** (i.e. **BWA**) to index the reference genome.  

For this pipeline we are using a western toad reference genome that was previously published by Trumbo et al., 2023. This reference genome is ~5GB and is in scaffolds.  

### Inputs
1) Reference genome in Fasta format

### Flags
`-p` prefix of the outputs
`a` set as **bwtsw** because... 

## Running BWA
index_referencegenome.sh

```
#!/bin/bash
#SBATCH -c 1
#SBATCH --mem=64GB
#SBATCH --account=NAME
#SBATCH --time=2-12:00
#SBATCH -o proc_radt_%A.out
#SBATCH -e proc_radt_%A.err
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=EMAIL

prefix=$1
ref=$2

module load bwa

bwa index -p $prefix -a bwtsw $ref
```
Command line:
```

```
### Outputs
The outputs will be five files with the output prefix you specify and the following file formats: **prefix.fa.{amb, ann, bwt, pac, sa}** 
