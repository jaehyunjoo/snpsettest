
<!-- README.md is generated from README.Rmd. Please edit that file -->

# snpsettest

<!-- badges: start -->

[![R-CMD-check](https://github.com/HimesGroup/snpsettest/workflows/R-CMD-check/badge.svg)](https://github.com/HimesGroup/snpsettest/actions)
<!-- badges: end -->

The goal of **snpsettest** is to provide simple tools that perform a
set-based association test (e.g., gene-based association test) using
GWAS summary statistics. **This package is currently under
development.**

A set-based association test in the **snpsettest** package is based on
the statistical test described in
[VEGAS](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2896770/)
(**ve**rsatile **g**ene-based **a**ssociation **s**tudy), which combines
the effects of a set of SNPs within a set accounting for linkage
disequilibrium between markers. This package uses a more efficient
method than the original implementation for computing a set-level
p-value, significantly reducing computation time.

## Installation

To install this package,

``` r
devtools::install_github("HimesGroup/snpsettest")
```

## Getting started

This is a basic example which shows you how to perform gene-based
association tests using GWAS summary statistics:

### GWAS summary file

``` r
library(snpsettest)

# Load an example of GWAS summary file
# snpsettest requires id, chr, pos, A1, A2, and p columns for GWAS summary file
data(exGWAS)
```

### Reference data

To infer the relationships among SNPs, the **snpsettest**\* package
requires a reference data set. The GWAS genotype data itself can be used
as the reference data. Otherwise, you could use publicly available data,
such as the 1000 Genome. This package accepts PLINK 1 binary files
(.bed, .bim, .fam) as an input.

``` r
# Path to .bed file
bfile <- system.file("extdata", "example.bed", package = "snpsettest")

# Read a .bed file using bed.matrix-class in gaston package
# Genotypes are retrieved on demand
x <- read_reference_bed(bfile)
```

### Harmonize GWAS summary to the reference data

Pre-processing of GWAS summary data is required because the sets of
variants available in a particular GWAS might be poorly matched to the
variants in reference data. SNP matching can be performed either 1) by
SNP ID or 2) by chromosome code, base-pair position, and allele codes,
while taking into account possible strand flips and reverse reference
alleles.

``` r
# Harmonize by SNP IDs
hsumstats1 <- harmonize_sumstats(exGWAS, x)

# Harmonize by genomic position and allele codes
# Reference allele swap will be taken into account (e.g., A/C match C/A)
hsumstats2 <- harmonize_sumstats(exGWAS, x, match_by_id = FALSE)

# Check matching entries by flipping allele codes (e.g., A/C match T/G)
# Ambiguous SNPs will be excluded from harmonization
hsumstats3 <- harmonize_sumstats(exGWAS, x, match_by_id = FALSE, 
                                 check_strand_flip = TRUE)
```

### Map SNPs to genes

To perform gene-based association tests, it is necessary to annotate
SNPs onto their neighboring genes.

``` r
# Load gene information
data(gene.curated.GRCh37) # extracted from gencode release 19

# Map SNPs to genes
snp_sets <- map_snp_to_gene(hsumstats1, gene.curated.GRCh37)

# Allows a certain kb window before/after the gene to be included for SNP mapping
snp_sets_50kb <- map_snp_to_gene(
  hsumstats, gene.curated.GRCh37, 
  extend_start = 50, extend_end = 50 # default is 20 (20kb)
)
```

### Gene-based association tests

``` r
# Perform gene-based association tests
res <- snpset_test(hsumstats1, x, snp_sets$sets)
```
