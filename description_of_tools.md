---
title: "Detailed Description of the Bioinformatic Tools"
author: 'Stephen Kelly'
date: 'November 3, 2019'
---

Various software packages are used during the targeted exome sequencing and variant calling analysis used by the 580 gene panel. The bulk of these tools are scripted in the [`NGS580-nf`](https://github.com/NYU-Molecular-Pathology/NGS580-nf) pipeline (https://github.com/NYU-Molecular-Pathology/NGS580-nf).

# Trimmomatic Version 0.36

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4103590/

http://www.usadellab.org/cms/?page=trimmomatic

A flexible read trimming tool for Illumina NGS data. Trimmomatic performs a variety of useful trimming tasks for illumina paired-end and single ended data.

## Settings Used

- `PE`: Paired End

- `ILLUMINACLIP:/ref/contaminants/trimmomatic.fa:2:30:10:1:true`: Cut adapter and other illumina-specific sequences from the read.

- `TRAILING:5`: TRAILING: Cut bases off the end of a read, if below a threshold quality

- `SLIDINGWINDOW:4:15`: Scan the read with a 4-base wide sliding window, cutting when the average quality per base drops below 15

- `MINLEN:35`: Drop the read if it is below a specified length

# BWA Version: 0.7.17

https://github.com/lh3/bwa

http://bio-bwa.sourceforge.net/bwa.shtml

BWA is a software package for mapping DNA sequences against a large reference genome, such as the human genome.

## Settings Used

- `bwa mem`: Align 70bp-1Mbp query sequences with the BWA-MEM algorithm.

- `-M`: Mark shorter split hits as secondary

- `-v 1`: Control the verbose level of the output. 1 for outputting errors only


# Sambamba v0.6.8

https://github.com/lomereiter/sambamba

http://lomereiter.github.io/sambamba/

http://lomereiter.github.io/sambamba/docs/sambamba-view.html

Tools for working with SAM/BAM data.

Sambamba is a high performance modern robust and fast tool (and library), written in the D programming language, for working with SAM and BAM files. Current functionality is an important subset of samtools functionality, including view, index, sort, markdup, and depth.

## Settings Used

- `--filter='mapping_quality>=10'`: Set custom filter for alignments.

# GATK version 3.8

https://software.broadinstitute.org/gatk/

https://software.broadinstitute.org/gatk/documentation/

https://software.broadinstitute.org/gatk/documentation/tooldocs/3.8-0/

Genome Analysis Toolkit for variant discovery in high-throughput sequencing data. Developed in the Data Sciences Platform at the Broad Institute, the toolkit offers a wide variety of tools with a primary focus on variant discovery and genotyping.

## Settings Used

### RealignerTargetCreator

- `-dt NONE`: Type of read downsampling to employ at a given locus

- `--interval_padding 10`: Amount of padding (in bp) to add to each interval

### IndelRealigner

- `-dt NONE`: Type of read downsampling to employ at a given locus

- `--maxReadsForRealignment 50000`: Max reads allowed at an interval for realignment

### BaseRecalibrator

- `-rf BadCigar`: Filters to apply to reads before analysis

- `--interval_padding 10`: Amount of padding (in bp) to add to each interval

- `-BQSR`: Base quality score recalibration

### DepthOfCoverage

- `-dt NONE`: Type of read downsampling to employ at a given locus

- `-rf BadCigar`: Filters to apply to reads before analysis

- `--omitIntervalStatistics`: Do not calculate per-interval statistics

- `--omitLocusTable`: Do not calculate per-sample per-depth counts of loci

- `--omitDepthOutputAtEachBase`: Do not output depth of coverage at each base

- `-ct [10, 50, 100, 200, 300, 400, 500]`: Coverage threshold (in percent) for summarizing statistics

- `-mbq 20`: Minimum quality of bases to count towards depth

- `-mmq 20`: Minimum mapping quality of reads to count towards depth

### MuTect2

- `-dt NONE`: Type of read downsampling to employ at a given locus

- `--standard_min_confidence_threshold_for_calling 30`: The minimum phred-scaled confidence threshold at which variants should be called

- `--max_alt_alleles_in_normal_count 10`: Threshold for maximum alternate allele counts in normal

- `--max_alt_allele_in_normal_fraction 0.05`: Threshold for maximum alternate allele fraction in normal

- `--max_alt_alleles_in_normal_qscore_sum 40`: Threshold for maximum alternate allele quality score sum in normal

- `--interval_padding 10`: Amount of padding (in bp) to add to each interval

# LoFreq version 2.1.2

http://csb5.github.io/lofreq/

https://github.com/CSB5/lofreq

https://www.ncbi.nlm.nih.gov/pubmed/23066108

LoFreq is a fast and sensitive variant-caller for inferring SNVs and indels from next-generation sequencing data. It makes full use of base-call qualities and other sources of errors inherent in sequencing (e.g. mapping or base/indel alignment uncertainty), which are usually ignored by other methods or only used for filtering.

## Settings Used

- `--call-indels`: include indels in variant call output

# bcftools version 1.3

https://samtools.github.io/bcftools/

https://samtools.github.io/bcftools/bcftools.html

http://www.htslib.org/download/

BCFtools is a set of utilities that manipulate variant calls in the Variant Call Format (VCF) and its binary counterpart BCF. All commands work transparently with both VCFs and BCFs, both uncompressed and BGZF-compressed.

## Settings Used

- `index`: index VCF/BCF

- `norm`: normalize indels

- `view`: subset, filter and convert VCF and BCF files

- `--multiallelics`: split multiallelic sites into biallelic records (-) or join biallelic sites into multiallelic records (+)

- `-both`: abbreviation of "-c indels  -c snps"

- `-c snps`: any SNP records are compatible, regardless of whether the ALT alleles match or not. For duplicate positions, only the first SNP record will be considered and appear on output.

- `-c indels`: all indel records are compatible, regardless of whether the REF and ALT alleles match or not. For duplicate positions, only the first indel record will be considered and appear on output.

- `--output-type v`: Output compressed BCF (b), uncompressed BCF (u), compressed VCF (z), uncompressed VCF (v). Use the -Ou option when piping between bcftools subcommands to speed up performance by removing unnecessary compression/decompression and VCF←→BCF conversion.

- `--fasta-ref`: reference sequence in fasta format

# ANNOVAR revision 150617

http://annovar.openbioinformatics.org/en/latest/

ANNOVAR is an efficient software tool to utilize update-to-date information to functionally annotate genetic variants detected from diverse genomes (including human genome hg18, hg19, hg38, as well as mouse, worm, fly, yeast and many others)

## Settings Used


- `--buildver hg19`: genome build version

- `--protocol refGene,clinvar_20170905,cosmic70,1000g2015aug_all,avsnp150,exac03,snp138`: comma-delimited string specifying database protocol

- `--operation g,f,f,f,f,f,f`: comma-delimited string specifying type of operation
