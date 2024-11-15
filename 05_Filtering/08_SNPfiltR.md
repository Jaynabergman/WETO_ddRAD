# Filtering in the R package SNPfiltR

## Background

The majority of the SNP filtering will be done using the R package **SNPfiltR**. This R package contains functions that are helpful for visualizing metrics for determining quality and missing data. The only filter applied before the vcf file is brought into R is the *--max-obs-het* set in *populations* in **STACKS**. 

### Inputs
1. vcf file (generated with *popuations* in **STACKS**)

### Filters
1. **Genotype depth** (*hard_filter(depth)*): Determines the minimum depth for genotype calls
2. **Genotype quality** (*hard_filter(gq)*): This sets the minimum genotype quality for the genotype that are called
3. **Allele balance** (*filter_allele_balance*): This determines the ratio of reads showing the reference allele to all reads for heterozygous individuals
4. **Maximum depth** (*max_depth*): This sets the maximum mean depth per sample across all SNPs
5. **Minimum minor allele count** (*min_mac*): This sets the minimm number of gene copies carrying the minor allele at a locus
6. **Missing by SNP** (*missing_by_SNP): Determines the maximum proportion of missing data allowed in a SNP (cutoff = 0.9 indicates 10% missingness per SNP)
7. **Missing by sample** (*misssing_by_sample*): Determines the maximum proportion of missing data allowed in a sample (cutoff = 0.3 indicates 30% missingness per sample)

## Filtering settings

See R markdown (*01_VCF_filtering-HO0.6.Rmd*) for the code that was ran and for the graphs that were used to help infrom the settings.See supplementary material for tests done on filtering settings.   

**Input vcf file was first filtered to have a max observed heterozygosity of 0.6 in *populations* in STACKS**
  
| Step | Filter | Rational of filter | Setting | Reported values |
| --- | --- | --- | --- | --- |
| 1. | Genotype depth <br> *hard_filter(depth=5)* | Gives support for the confidence of a genotype call | 5 | |
| 2. | Geotype qaulity <br> *hard_filter(gq=20)* | Support to minize genotype erorrs | 20 | |
| 3. | Allele balance <br> *filter_allele_balance(min.ratio = 0.2, max.ratio = 0.8)* | Gets rid of SNPs outside of expected heterozygosity range | 0.2 - 0.8 | |
| 4. | Maximum depth per SNP <br> *max_depth(maxdepth=28)* | Removes excessively high depth which could indicate multilocus contigs | 28 | |
| 5. | Minor allele count <br> *min_mac(min.mac = 3)* | Removes potentially artifactual called SNPs | 3 | |

**Dataset with filtering scheme 1: Less missing data**  

| Step | Filter | Setting | Reported values | Notes | 
| --- | --- | --- | --- | --- |
| 6. | Missing by SNP <br> *missing_by_snp* | | | |
| 7. | Missing by sample <br> *missing_by_sample* | | |


**Dataset with filtering scheme 2: More missing data**  

| Step | Filter | Setting | Reported values | Notes | 
| --- | --- | --- | --- | --- |
| 6. | Missing by SNP <br> *missing_by_snp* | | | |
| 7. | Missing by sample <br> *missing_by_sample* | | |
