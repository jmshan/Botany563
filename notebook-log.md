# Botany 563 project - Repeated evolution of plant prickles
Prickles are sharp epidermal or cortical outgrowths that have evolved at least 28 times across tracheophytes. The development of prickles in Solanum, as well as other plant genera, is dependent on the co-option of LONELY GUY (LOG) family plant hormone biosynthetic genes, which encode enzymes that catalyze the final step of cytokinin (CK) biosynthesis, a key regulator of cell proliferation. This project aims to reconstruct the phylogenetic tree of LOG protein family and test whether specific subclade of LOG family associated with prickle development have undergone positive selection.
## Description of dataset
 I will use LOG protein sequences from at least 28 representative genus with instance of independent prickle evolution. I will obtain my sequences from published genome assembly papers and solpangenomics database (https://solpangenomics.com/dist/pages/downloads/index.php). LOG orthologs will be identified using OrthoFinder. 

| Clade  | Family            | Species  | Prickle state | Genome source |
| :----- | :----- | :---- | :---- | :---- |
| Asterids | Solanaceae | *Solanum prinophyllum* | Present | [Satterlee et al., 2024](https://www.science.org/doi/10.1126/science.ado1663)
| Asterids | Solanaceae | *Solanum lycopersicum* | Absence | [Satterlee et al., 2024](https://www.science.org/doi/10.1126/science.ado1663)
| Rosids | Vitaceae   | *Vitis romanetii*      | Present | In-house genome assembly
| Rosids | Vitaceae   | *Vitis vinifera* var. Muscat | Absence | In-house genome assembly
| Rosids | Brassicaceae | *Brassica nigra* Ni100 | Present |ONT assembly [Perumal et al. 2020]([text](https://doi.org/10.1038/s41477-020-0735-y))
| Rosids | Brassicaceae | *Arabidopsis thaliana* | Absence | Araport11 [Cheng et al., 2017](https://doi.org/10.1111/tpj.13415)
| Rosids | Rosaceae | *Rosa chinense* | Present | [Hibrand Saint-Oyant et al., 2018](https://doi.org/10.1038/s41477-018-0166-1)
| Rosids | Rosaceae | *Fragaria vesca* | Absence | [Shulaev et al., 2011]([text](https://doi.org/10.1038/ng.740))
|   | Rutaceae | *Zanthoxylum piperitum* | Present | 
| ANA-grade | Nymphaeaceae | *Victoria cruziana* | Present | [Wen et al., 2025]([text](https://doi.org/10.1016/j.xplc.2025.101342))
| ANA-grade | Nymphaeaceae | *Nymphaea colorata*  | Absence | [Zhang et al., 2020]([text](https://doi.org/10.1038/s41586-019-1852-5))
| Ferns | Cyatheaceae | *Alsophila spinulosa* | Present | [Huang et al., 2022](https://doi.org/10.1038/s41477-022-01146-6)	
|   |   |      | Absence | 


## OrthoFinder


## Multiple Sequence Alignment (MSA)
### MUSCLE

MUSCLE is a multiple sequence alignment algorithm using progressive alignment with iterative refinement. There could be bias towards the guide tree and we assume that the bias can be neglected. 

```
cd /Users/morven/Desktop/Botany563/data
muscle -align LOGs_aa.fa -output LOGs_aa_aligned_muscle.fa

muscle 5.3.osxarm64 []  8.6Gb RAM, 8 cores
Built Jul 31 2025 00:34:33
(C) Copyright 2004-2021 Robert C. Edgar.
https://drive5.com

[align LOGs_aa.fa]
Input: 29 seqs, avg length 215, max 306, min 77

00:00 4.2Mb   100.0% Derep 29 uniques, 0 dupes
00:00 4.2Mb  CPU has 8 cores, running 8 threads
00:00 190Mb   100.0% Calc posteriors
00:00 190Mb   100.0% UPGMA5         
00:00 193Mb   100.0% Consistency (1/2)
00:00 193Mb   100.0% Consistency (2/2)
00:00 193Mb   100.0% Refining         
```
### MAFFT

MAFFT is a multiple sequence alignment algorithm using progressive alignment with iterative refinement. It is based on fast Fourier transform. It is sensitive to guide tree errors.

```
mafft --auto LOGs_aa.fa > LOGs_aa_aligned_mafft.fa

outputhat23=16
treein = 0
compacttree = 0
stacksize: 8176 kb
rescale = 1
All-to-all alignment.
tbfast-pair (aa) Version 7.526
alg=L, model=BLOSUM62, 2.00, -0.10, +0.10, noshift, amax=0.0
0 thread(s)

outputhat23=16
Loading 'hat3.seed' ... 
done.
Writing hat3 for iterative refinement
rescale = 1
Gap Penalty = -1.53, +0.00, +0.00
tbutree = 1, compacttree = 0
Constructing a UPGMA tree ... 
   20 / 29
done.

Progressive alignment ... 
STEP    27 /28 
Reallocating..done. *alloclen = 1618
STEP    28 /28 
done.
tbfast (aa) Version 7.526
alg=A, model=BLOSUM62, 1.53, -0.00, -0.00, noshift, amax=0.0
1 thread(s)

minimumweight = 0.000010
autosubalignment = 0.000000
nthread = 0
randomseed = 0
blosum 62 / kimura 200
poffset = 0
niter = 16
sueff_global = 0.100000
nadd = 16
Loading 'hat3' ... done.
rescale = 1

   20 / 29
Segment   1/  1    1- 395
STEP 004-009-0  identical.   
Oscillating.

done
dvtditr (aa) Version 7.526
alg=A, model=BLOSUM62, 1.53, -0.00, -0.00, noshift, amax=0.0
0 thread(s)


Strategy:
 L-INS-i (Probably most accurate, very slow)
 Iterative refinement method (<16) with LOCAL pairwise alignment information

If unsure which option to use, try 'mafft --auto input > output'.
For more information, see 'mafft --help', 'mafft --man' and the mafft page.

The default gap scoring scheme has been changed in version 7.110 (2013 Oct).
It tends to insert more gaps into gap-rich regions than previous versions.
To disable this change, add the --leavegappyregion option.
```
## Distance-based methods
There are algorithms that produce the optimum tree without having to search the space of trees. Thus, the distance-based method is the fastest. However, it could be sensitive to unequal rates of evolution.

The following commands are run inside R.
1. Installing necessary packages 
```
install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)
```
2. Load installed packages
```
library(ape)
library(adegenet)
library(phangorn)
```
3. Loading the sample data
```
dna <- fasta2DNAbin(file="LOGs_cds_aligned_muscle.fa")
```
4. Computing the genetic distances. 
```
D <- dist.dna(dna, model="GTR")
```
5. Get the NJ tree
```
tre <- nj(D)
```
6. To get the ladderized effect when plotting the tree
```
tre <- ladderize(tre)
```
7. Plot the tree
```
plot(tre, cex=.6)
title("A simple NJ tree")
```
## Parsimony method
Parsimony-based method wants to find a tree with the minimum changes to explain the data we see at the tips of the tree. It assumes that all characters evolve independently. Parsimony-based method could be susceptible to long-branch attraction. 

The following commands are run inside R.
1. Installing necessary packages 
```
install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)
```
2. Load installed packages
```
library(ape)
library(adegenet)
library(phangorn)
```
3. Loading the sample data and read as phangorn object
```
dna <- fasta2DNAbin(file="LOGs_cds_aligned_muscle.fa")
dna2 <- as.phyDat(dna)
```
4. Generating a starting tree for the search on tree space and compute the parsimony of this tree
```
tre.ini <- nj(dist.dna(dna,model="raw"))
parsimony(tre.ini, dna2)
```
5. Search for the tree with maximum parsimony
```
tre.pars <- optim.parsimony(tre.ini, dna2)
```
6. Plot tree
```
plot(tre.pars, cex=0.6)
```

