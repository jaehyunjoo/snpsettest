
<!-- README.md is generated from README.Rmd. Please edit that file -->

# snpsettest

<!-- badges: start -->

[![R-CMD-check](https://github.com/HimesGroup/snpsettest/workflows/R-CMD-check/badge.svg)](https://github.com/HimesGroup/snpsettest/actions)
<!-- badges: end -->

The goal of ‘snpsettest’ is to provide simple tools that perform
set-based association tests (e.g., gene-based association tests) using
GWAS (genome-wide association study) summary statistics. A set-based
association test in this package is based on the statistical model
described in
[VEGAS](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2896770/)
(**ve**rsatile **g**ene-based **a**ssociation **s**tudy), which combines
the effects of a set of SNPs accounting for linkage disequilibrium
between markers. This package uses a different approach from the
original VEGAS implementation to compute set-level p values more
efficiently, as described in
[here](https://github.com/HimesGroup/snpsettest/wiki/Statistical-test-in-snpsettest).

To install this package,

``` r
install.packages("snpsettest")
```

Here is a [wiki
page](https://github.com/HimesGroup/snpsettest/wiki/Getting-started) for
tutorial.
