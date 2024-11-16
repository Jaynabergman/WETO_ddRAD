# STRUCTURE

## Background


### Files & inputs 
1) create_strauto_slurm_scripts.py
2) input.py
3) strauto_1.py
4) harvesterCore.py
5) structureHarvester.py
6) filename.str

### Steps
1. Edit structure file by removing the header line (first line in the file) and change the file extension to .str
  
2. Load the following modules in the command line:
- module load StdEnv/2020
- module load gcc/9.3.0
- module load structure/2.3.4
- module load python/2.7.18
  
3. Type in the command line: chmod u+x create_strauto_slurm_scripts.py

4. Edit the input.py file:
- change burnin (i.e. 300,000)
- Change mcmc (i.e. 1,000,000)
- number of individuals
- number of loci
- max number of populations
- number of kruns

5. Type in the command line: ./strauto_1.py (follow instructions)

6. Type in the command line: ./create_strauto_slurm_scripts.py

7. Type in the command line: bash submit_strauto_jobs.sh

8. Once all jobs have completed - type in the command line: bash post_strauto.sh
