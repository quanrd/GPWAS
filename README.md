# GPWAS [![](https://img.shields.io/badge/Release-v1.0.1-blue.svg)](https://github.com/shanwai1234/GPWAS/commits/master) [![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
## Genome-Phenome Wide Association Study

### Authors:
> Zhikai Liang, Yumou Qiu and James C. Schnable.

### Contact:
> [zliang@huskers.unl.edu](Zhikai Liang)

---
# Installation and Package loading
```r
# install "devtools" package in your R environment
> devtools::install_github("shanwai1234/GPWAS")
> library(GPWAS)
```
# Data Preparation

All of input data are required to be organized in following format.

## Genotype Data

**Gene**: Keep the title of this item as "Gene" and do not change it. All of gene names were kept below it.

**SNP**: Keep the title of this item as "SNP" and do not change it. The format of SNP should be written as "S"+chromosome+"\_"+SNP position.

**Sample**: Individual sample name. 

**Note**: All items should be split by \tab

| Gene | SNP | Sample1 | Sample2 | Sample3 |
| :---: | :---: |:---: |:---: | :---: |
|Zm00001d000501|S3_27396852| 0 | 1 | 0 |
|Zm00001d000501|S3_27397388| 0 | 2 | 1 |
|Zm00001d000632|S3_156234383| 0 | 1 | 1 |
|Zm00001d000632|S3_156234434| 1 | 2 | 1 |
|Zm00001d000654|S3_178548901| 2 | 1 | 1 |

## Phenotype Data

**Pheno** Name of phenotype name.

**Sample** Individual sample name.

**Note**: All items should be split by space.

| Taxa | Pheno1 | Pheno2 | Pheno3 |
| :---: | :---: |:---: |:---: |
| Sample1 | 23 | 50 | 55 |
| Sample2 | 10 | 12 | 8 |
| Sample3 | 120 | 150 | 133 |

## PC covariates
Version 1.0.1 GPWAS package controls population structure using PC scores generated by PCA. For each individual gene, population structure is controlled by PC scores calculated by the rest of other chromosomes. But generating PC scores were not included in GPWAS package. You are suggested to calculate it using function like [prcomp](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/prcomp.html).

|  | "PC1" | "PC2" | "PC3" |
| :---: | :---: |:---: |:---: |
| "Sample1" | -11 | 8 | 31 |
| "Sample2" | -36 | 35 | 138 |
| "Sample3" | 15 | 10 | -7 |

You need to prepare separate PC covariate files excluding each individual chromosome. If you have 10 chromosomes, you need to prepare 10 separate PC covariate files.

**Note**: All items should be split by space. The order of samples should be identical to the order of samples in both genotype and phenotype file.

**Example**: When you want to exclude chromosome 1, you need to make the file name as "Population\_structure\_exclude\_chrom\_1.txt". Then store all of these files to a folder.

# Running the model
```r
gpwas(ingeno, inpheno, inpc, g, gp, gv, R = num)
```
 **ingeno** Input genotype file name/directory.
 **inpheno** Input phenotype file name/directory.
 **inpc** Input folder with PCA parsed population structure covariance. If n number of chromosomes, n number of separate files should be included, as SNPs on each chromosome is excluded for performing PCA once.
 **g** A list of specific gene that needs to analyze. By default the model will run for all of genes detected in the input genotype file.
 **gp** Output file name/directory for selected phenotypes with every gene as well as p value of each selected phenotypes.
 **R** Number of iteration for scanning all of input phenotypes with one specific gene. Too big number will be redundancy and computationally cost. Suggested ranging from 10-50.
