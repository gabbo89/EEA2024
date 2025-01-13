---
layout: default
title: Genome wide methylation distribution analysis with __R__
parent: Lesson 4 - Genome wide methylation distribution analysis
nav_order: 1
description: A comprehensive guide to understanding epigenetics.
published: true
---

# 1. Filtering of the dataset 
Before reading the file in `R` we need to filter the file in order to remove positions without coverage and by selecting the methylation contexts (`CG`) of interest.

awk command 

# 2. Upload the data in `R`


# 3. Creating genomic windows of a fixed size
The chromosome will be divided in windows of a fixed size (e.g. 100,000 bp). The size is totally arbitrary. The methylation values will be averaged in each window. The windows are not overlapping.

Next we need to assing each C position to a genomic window (for example the C in position 11,450 will be assigned to the window 1-100,000, while the C in position 153,000 will be assigned to the window 100,000-200,000).

<!--We will use the `cut` function in `R` to assign each C position to a genomic window-->
We can perform this operation in a vectorized way using the column with the genomic coordinates (from the CG data.frame) and a new vector of assigned intervals (will become a new column in the CG data.frame).

We need to get the start and end of each chromosome in order to divide the chromosome in windows. We can do this by using the `cut` function in `R` with the `breaks` argument. The `breaks` argument is used to specify the points at which to split the data. `breaks` assign the last value of the window???

For example breaks of a length 100, (from 1 to 100bp), divided in windows of 10bp, will be: 10, 20, 30, 40, 50, 60 , 70, 80, 90, 100.

```r
my_breaks=c(0,10,20,30,40,50,60,70,80,90,100)
```
(starting from 0)

The same vector can be obtained in an automatic way using the following code:

```r
my_breaks=seq(0,100,by=10)
```

The vector with intervals of the windows assigned to each position, of length 1 to 100 can be obtained using the `cut` function

```r
# Define the genomic positions
all_genomic_positions=1:100

# Use the cut function
cut(all_genomic_positions,breaks=my_breaks)
```

{: .success-title }
> STDOUT message
>
>  [1] (0,10]   (0,10]   (0,10]   (0,10]   (0,10]   (0,10]   (0,10]   (0,10]   (0,10]   (0,10]   (10,20]  (10,20]  (10,20]
> 
> [14] (10,20]  (10,20]  (10,20]  (10,20]  (10,20]  (10,20]  (10,20]  (20,30]  (20,30]  (20,30]  (20,30]  (20,30]  (20,30] 
> 
> [27] (20,30]  (20,30]  (20,30]  (20,30]  (30,40]  (30,40]  (30,40]  (30,40]  (30,40]  (30,40]  (30,40]  (30,40]  (30,40] 
> 
> [40] (30,40]  (40,50]  (40,50]  (40,50]  (40,50]  (40,50]  (40,50]  (40,50]  (40,50]  (40,50]  (40,50]  (50,60]  (50,60] 
> 
> [53] (50,60]  (50,60]  (50,60]  (50,60]  (50,60]  (50,60]  (50,60]  (50,60]  (60,70]  (60,70]  (60,70]  (60,70]  (60,70] 
> 
> [66] (60,70]  (60,70]  (60,70]  (60,70]  (60,70]  (70,80]  (70,80]  (70,80]  (70,80]  (70,80]  (70,80]  (70,80]  (70,80] 
> 
> [79] (70,80]  (70,80]  (80,90]  (80,90]  (80,90]  (80,90]  (80,90]  (80,90]  (80,90]  (80,90]  (80,90]  (80,90]  (90,100]
> 
> [92] (90,100] (90,100] (90,100] (90,100] (90,100] (90,100] (90,100] (90,100] (90,100]
> 
> 10 Levels: (0,10] (10,20] (20,30] (30,40] (40,50] (50,60] (60,70] (70,80] (80,90] (90,100]

For example , if we want to divide the chromosome in windows of 100,000 bp, we can use the following code:

We can replace the intervals (0,10] with increasing numbers that identify the windows (1,2,3,4,5..) or with numbers that represent the end coordinates of each windows (100000,200000,300000..). For example:

This can be obtained using labels of the cut function. labels is a vector with the same size of the breaks vectore (less one unit, because it has a number at the beginning).

```r
# define the breaks
my_breaks=c(0,10,20,30,40,50,60,70,80,90,100)

# define the labels 
my_labels=c(1,2,3,4,5,67,8,9,10)

# or
my_labels=c(10,20,30,40,50,60,70,80,90,100)
```

The vector my_labels and my_breaks can also be created in an automatic way (little bit more tricky):

```r
# my_labels
my_labels = c(1,2,3,4,5,6,7,8,9,10) 

#can be obtained 

my_lables=1:(length(seq(0,100,by=10))-1)
```

```r
my_breaks = seq(0,100,by=10) 

#can be obtained 

my_lables=my_breaks[-1]
```

So what we obtain will be:

```r
all_genomic_positions=1:100

my_breaks = seq(0,100,by=10)

my_labels = my_breaks[-1]

cut(all_genomic_positions,breaks=my_breaks,labels=my_labels)
```

{: .success-title }
> STDOUT message
>

If we want to apply the same to the CG data.frame

```r
maxpos=1000*ceiling(max(CG$pos)/10000)
my_breaks=seq(0,maxpos,by=1000)
my_labels=my_breaks[-1]
CG$window=cut(CG$pos,breaks=my_breaks,labels=my_labels)
```

--- 