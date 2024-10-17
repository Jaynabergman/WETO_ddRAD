# WETO ddRAD pipeline 2024
## Project and data overview
### Project
- **Taxon:** Western toads (*Anaxyrus boreas*)
- **Spatial scale:** Broadscale population analysis
- **Goal:** Assess the genetic structure of western toads (*Anaxyrus boreas*) in the Canadian portion of the range.
- **Background:** The western toads in Canada are currently recognized as two populations (calling population and non-calling population: Pauly, 2008) which are delineated as separate Designatable Units (DUs: COSEWIC, 2012). This designation was determined by morphological and behavioural differences among the populations (i.e. the calling population possesses a vocal sac and a pronounced breeding calling, and the lack thereof in the non-calling population). Genetic differences have yet to be assessed between these two populations.
### Data - Plate1
- **Sequencing Type:** Double digest restriction-site associated DNA sequencing (ddRADseq)
- **Enzymes:** SbfI / MspI 
- **Library prep:** Sent to Plateforme d’Analyses Génomiques of the Institut de Biologie Intégrative et des Systèmes (IBIS, Universite ́Laval, Quebec, Canada)
- **Sequencing Info:** Paired-end sequencing on an Illumina NovaSeq 6000 at Centre d’expertise et de service Génome Québec at McGill University in Montreal, QC
<br>

- **Number of individuals sequenced:** 48 (inclusive of two positive controls - i.e. replicates, and one negative control)
- **Number of sites:** 37 breeding sites
- **Range of individuals per site:** 1 to 5 

### Data - Plate2
- **Sequencing Type:** Double digest restriction-site associated DNA sequencing (ddRADseq)
- **Enzymes:** PstI / MspI
- **Library prep:** Sent to Plateforme d’Analyses Génomiques of the Institut de Biologie Intégrative et des Systèmes (IBIS, Universite ́Laval, Quebec, Canada)
- **Sequencing Info:** Paired-end sequencing on an Illumina NovaSeq 6000 at Centre d’expertise et de service Génome Québec at McGill University in Montreal, QC
<br>

- **Number of individuals sequenced:** 48 (inclusive of one positive control - i.e. replicates, and two negative control). 45 unique individuals.
- **Number of sites:** 39 breeding sites
- **Range of individuals per site:** 1 to 5 (one site with 3 individuals, one with 5 individuals)
<br>

- **Reference genome:** Trumbo et al., 2023 (DOI: 10.1111/mec.17175)
- **Genome size:** ~5GB
<br>

## Pipeline overview
### Raw data
Downloaded raw reads from nanuq and performed md5sum check (DATE).
<br>

### Prepocessing
1. Demultiplexing in **STACKS**
2. Fastq quality (read quality) filtering in **Fastp**
3. Summarizing fastp results in **Multiqc**

### Reference genome alignment
4. Index reference genome in **BWA**
5. Map reads to reference genome in **BWA**
6. Filter for mapping quality in **samtools**

### Filtering 
7. Filter for paralogs in **ngsParalog** and remove regions in **STACKS**
8. Additional filtering of vcf files in **vcftools** and **PLINK** (see *Filtering overview* below)

### Population genomic analysis
9. Stats: Fst, Heterozygosity
10. PCA
11. DAPC
12. STRUCTURE
13. Isolation-by-distance
14. Unrooted phylogenetic tree (Neighbour-joining)
15. Entropy (?)

## Filtering overview
### Decisions
| Stage | Filter | Program | Setting | Explaination |
| --- | --- | --- | --- | --- |
| Pre-VCF file | Read quality | Fastp | Q (Phred score) >= 30 | Determines bases with a quality below the given threshold |
| Pre-VCF file | Mapping quality | Samtools | Q >= 30 | Score that indicates the quality of the alignment (mapping) of a read to the reference genome |
| Pre-VCF file | Mapping quality | ngsParalog | pval < 0.05 | Identifies likely paralogous regions of the reference genome |
| Pre-VCF file | Read depth | STACKS (populations) | 15X (low est.) <br> 30X (ideal) | The number of reads that cover a given loci to indicate sequencing depth |
| Post-VCF file | Missing data (individual & locus) | vcftools | a) 50% <br> b) 80% | Missing percent of genotypes for each individual and locus |
| Post-VCF file | Minor allele count (MAC) (locus) | vcftools | a) 3 <br> b) 5 | Sets the minimum number of copies for the minor alleles to be found at a locus |
| Post-VCF file | Hardy-Weinberg Equilibrium (HWE) | Populations (STACKS) | a) Remove loci out of HWE (threshold < 0.001) <br> b) Leave all loci | Determines if the expected frequencies of the genotypes at a given locus are under HWE |
| Post-VCF file | Linkage Disequilibrium (LD) | PLINK | Remove loci | Physical linkage or non-independent assortment leading to the non-random association of alleles at different loci |  

### Reporting (Pre-VCF file)
Starting number of reads: 1,506,075,128
| Filtering step | Number of Individuals | Number of reads |
| --- | --- | --- |
| Read quality (Q>=30) | 48 | 1,413,577,674 |
| Mapped reads (after alignment) | 46 | 631,122,668 |
| Mapping quality (Q>=30) | 46 | 294,489,690 |
| Mapping quality (Q>=20) | 46 | 310,998,339 |
| Read depth | # | # |
| Remove paralogs | # | # |

### Datasets (Post-VCF file)
| Filter | Dataset 1 | Dataset 2 | Dataset 3 | Dataset 4 | Dataset 5 | Dataset 6 | Dataset 7 | Dataset 8|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Missing data (ind & locus) | 50% | 50% | 80% | 80% | 50% | 50% | 80% | 80% |
| MAC | 3 | 3 | 3 | 3 | 5 | 5 | 5 | 5 |
| Number of individuals | # | # | # | # | # | # | # | # |
| Number of SNPs | # | # | # | # | # | # | # | # |

