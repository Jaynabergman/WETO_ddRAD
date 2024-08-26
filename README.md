# WETO ddRAD pipeline 2024
## Project and data overview
### Project
- **Taxon:** Western toads (*Anaxyrus boreas*)
- **Spatial scale:** Broadscale phylogenetic analysis
- **Goal:** Assess the genetic structure of western toads (*Anaxyrus boreas*) in the Canadian portion of the range.
- **Background on the species:** The western toads in Canada are currently recognized as two populations (calling and non-calling populations) assigned as separate Designatable Units (DUs: COSEWIC, 2012). This designation was determined by morpholigical and behavioural differences among the populations (i.e. calling population possesses a vocal sac and a pronouced breeding calling, and the lack thereof in the non-calling population). Genetic differences have yet to be determined between these two DUs.
### Data
- **Sequencing Type:** Double digest restriction-site associated DNA sequencing (ddRADseq)
- **Enzymes:** SbfI / MspI (plate 1)
- **Library prep:** Sent to Plateforme d’Analyses Génomiques of the Institut de Biologie Intégrative et des Systèmes (IBIS, Universite ́Laval, Quebec, Canada)
- **Sequencing Info:** Paired-end sequencing on an Illumina NovaSeq 6000 at Centre d’expertise et de service Génome Québec at McGill University in Montreal, QC
<br>

- **Number of individuals sequenced:** 48 (inclusive of two positive controls - i.e. replicates, and one negative control)
- **Number of sites:** 37 breeding sites
- **Range of individuals per site:** 1 to 5 
<br>

## Pipeline overview
### Data transfer
Downloaded raw reads from nanuq and performed md5sum check.
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

