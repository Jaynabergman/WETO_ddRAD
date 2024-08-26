# WETO ddRAD pipeline 2024
## Project and data overview
### Project
- **Taxon:** Western toads (*Anaxyrus boreas*)
- **Spatial scale:** Broadscale population analysis
- **Goal:** Assess the genetic structure of western toads (*Anaxyrus boreas*) in the Canadian portion of the range.
- **Background:** The western toads in Canada are currently recognized as two populations (calling population and non-calling population: Pauly, 2008) which are delineated as separate Designatable Units (DUs: COSEWIC, 2012). This designation was determined by morphological and behavioural differences among the populations (i.e. the calling population possesses a vocal sac and a pronounced breeding calling, and the lack thereof in the non-calling population). Genetic differences have yet to be assessed between these two DUs.
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

- **Reference genome:** Trumbo et al., 2023 (DOI: 10.1111/mec.17175)
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

## Filtering overview
### Decisions
| Stage | Filter | Program | Setting | Explaination |
| --- | --- | --- | --- | --- |
| Pre-VCF file | Read quality | Fastp? | Phred score? | EX |
| Pre-VCF file | Mapping quality | BWA? | ?? | EX |
| Pre-VCF file | Read depth | STACKS (populations)? <br> multiple steps? | ?? | EX |
| Post-VCF file | MAC | vcftools | a) 3 <br> b) 5 | EX |
| Post-VCF file | Missing data | vcftools | a) 50% <br> b) 80% | EX |
| Post-VCF file | HWE | PLINK | a) with <br> b) without | EX |

### Reporting (Pre-VCF file)
| Filtering step | Number of Individuals | Number of reads |
| --- | --- | --- |

### Datasets (Post VCF-file)
| Filter | Dataset 1 | Dataset 2 | Dataset 3 | Dataset 4 |
| --- | --- | --- | --- | --- |

