---
layout: default
title: Using bedtools
parent: Lesson 4 - Genome wide methylation distribution analysis
nav_order: 2
description: A comprehensive guide to understanding epigenetics.
published: true
---

We will now repeat the same analysis as done previously, but in this case the windows and raw data to be imported in `R` for the final graph will be obtained using `bedtools`.

Bedtools is a software package for the manipulation of genomic datasets. It is designed to be fast, flexible, and relatively easy to use. It is particularly useful for the analysis of large genomic datasets. It is written in C++ and is available for Linux, MacOS and Windows. It is free and open source. It is available at [http://bedtools.readthedocs.io/en/latest/](http://bedtools.readthedocs.io/en/latest/)

We will use the same raw table obtained from Bismark:



# 1. Filtering of the dataset 
We need to filter the file in order to remove positions without coverage and by selecting the methylation contexts (`CG`) for the chromosome of interest.

```bash
awk 'OFS="\t" {if($1=="Chr1" && ($4+$5)>0 && $6=="CG") {meth=100*($4)/($4+$5); print $0,meth}}' file > ..
```

# 2. Create the windows of a fixed size using bedtools 
We will use `bedtools makewindows` to create the windows. It requires the size of the window and the chromosome length. We will use the same size of the window as previously.

The **chromosome size** is obtained using `bedtools getfasta` with the `-fo` option to get the length of the chromosome. The output is a tab separated file with the following columns:
1. chromosome
2. length

We will set the **windows size** to 100,000bp.

In order to generate the windows, we will use the following command:
```bash
bedtools makewindows -g chromosome_size.txt -w 100000 > windows.bed
```

In order to assign the single sites to the windows we will use `bedtools intersect` with the `-wa` option. We need to create a file with the single sites in bed format (or bedgraph format). We need to format the columns of the input file by creating a bed-like structure:

```bash
awk 'OFS="\t" {print $1,$2-1,$2,$8}' .. > methylome.bed 
```

Now we can **intersect** the Cs sites with the previously created windows:

```bash
bedtools intersect -a methylome.bed -b windows.bed -wa -wb > methylome_windows.bed
```

Now we have the individuals methylation values assigned to the windows. 
We can obtain a mean methylation value for each window by using `bedtools groupby`:

```bash
bedtools groupby -i methylome_windows.bed -g 1-3 -c 7 -o mean > methylome_windows_mean.bed
```

```bash

```

need to prepare slides for 
file formats
fasta
fastq
sam/bam
bed
bedgraph
wiggle
gff


slides for 