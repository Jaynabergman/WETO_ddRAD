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

### Population genomic analysis
9. Stats: Fst, Heterozygosity
10. PCA
11. STRUCTURE
12. Isolation-by-distance
13. Unrooted phylogenetic tree

## Filtering decisions
### Decisions
| Stage | Filter | Program | Setting | Explaination |
| --- | --- | --- | --- | --- |
| Pre-VCF file | Read quality | Fastp? | Phred score? | EX |
| Pre-VCF file | Mapping quality | BWA? | ?? | EX |
| Pre-VCF file | Read depth | STACKS (populations)? <br> multiple steps? | ?? | EX |
| Post-VCF file | MAC | vcftools | 3 & 5 | EX |
| Post-VCF file | Missing data | vcftools | 50% & 80% | EX |
| Post-VCF file | HWE | PLINK | with & without | EX |

### Reporting (Pre-VCF file)
| Filtering step | Number of Individuals | Number of reads |
| --- | --- | --- |

### Datasets (Post VCF-file)
| Filter | Dataset 1 | Dataset 2 | Dataset 3 | Dataset 4 |
| --- | --- | --- | --- | --- |

