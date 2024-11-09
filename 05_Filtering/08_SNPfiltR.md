# Filtering in the R package SNPfiltR

## Background

The majority of the SNP filtering will be done using the R package **SNPfiltR**. This R package contains functions that are helpful for visualizing metrics for determining quality and missing data. The only filter applied before the vcf file is brought into R is the *--max-obs-het* set in *populations* in **STACKS**. 

### Inputs
1. Vcf file (generated from *popuations* in **STACKS**)

## Filters
1. **Genotype depth** (*hard_filter(depth)*): Determines the minimum depth for genotype calls
2. **Genotype quality** (*hard_filter(gq)*): This sets the minimum genotype quality for the genotype that are called
3. **Allele balance** (*filter_allele_balance*): This determines the ratio of reads showing the reference allele to all reads for heterozygous individuals
4. **Maximum depth** (*max_depth*): This sets the maximum mean depth per sample across all SNPs
5. **Minimum minor allele count** (*min_mac*): This sets the minimm number of gene copies carrying the minor allele at a locus
6. **Missing by SNP** (*missing_by_SNP): Determines the maximum proportion of missing data allowed in a SNP (cutoff = 0.9 indicates 10% missingness per SNP)
7. **Missing by sample** (*misssing_by_sample*): Determines the maximum proportion of missing data allowed in a sample (cutoff = 0.3 indicates 30% missingness per sample)
