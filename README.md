# WETO ddRAD pipeline 2024
## Project and data overview
<br>

## Pipeline overview
### Data transfer
<br>

### Prepocessing
1. Demultiplexing in **STACKS**
2. Fastq quality filtering in **Fastp**
3. Summarizing fastp results in **Multiqc**

### Reference Genome Alignment
4. Index reference genome in **BWA**
5. Map reads to reference genome in **BWA**

### Filtering 
6. Filter for mapping quality in **samtools**
7. Filter for paralogs in **ngsParalog** and remove regions in **STACKS**
8. Additional filtering of vcf files in **vcftools** and **PLINK**

## Filtering decisions
| Stage | Filter | Explaination | Setting |
| --- | --- | --- | --- |
| Pre-VCF file |  Read quality | Assessed in fastp step | Phred > 20 |
