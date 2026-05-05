# Botany 563 project - Repeated evolution of plant prickles
Prickles are sharp epidermal or cortical outgrowths that have evolved at least 28 times across tracheophytes. The development of prickles in *Solanum*, as well as other plant genera, is dependent on the co-option of *LONELY GUY* (*LOG*) family plant hormone biosynthetic genes, which encode enzymes that catalyze the final step of cytokinin (CK) biosynthesis, a key regulator of cell proliferation. This project aims to reconstruct the phylogenetic tree of LOG protein family and test whether specific subclade of LOG family associated with prickle development have undergone positive selection.

## Description of dataset
 I use LOG protein sequences from 4 representative families/orders with instance of independent prickle evolution. I obtain my sequences from published genome assembly papers, [solpangenomics database](https://solpangenomics.com/dist/pages/downloads/index.php), and [MarpolBase](https://marchantia.info). LOG orthologs will be identified using OrthoFinder. 

| Clade  | Family            | Species  | Prickle state | Genome source |
| :----- | :----- | :---- | :---- | :---- |
| Asterids | Solanaceae | *Solanum prinophyllum* | Present | [Satterlee et al., 2024](https://www.science.org/doi/10.1126/science.ado1663)
| Asterids | Solanaceae | *Solanum lycopersicum* | Absence | [Satterlee et al., 2024](https://www.science.org/doi/10.1126/science.ado1663)
| Rosids | Cleomaceae (Brassicales) | *Cleome houtteana* | Present | [Cheng et al. 2013](https://doi.org/10.1105/tpc.113.113480)
| Rosids | Brassicaceae (Brassicales) | *Arabidopsis thaliana* | Absence | Araport11 [Cheng et al., 2017](https://doi.org/10.1111/tpj.13415)
| Rosids | Rosaceae | *Rosa chinensis* | Present | [Hibrand Saint-Oyant et al., 2018](https://doi.org/10.1038/s41477-018-0166-1)
| Rosids | Rosaceae | *Fragaria vesca* | Absence | [Shulaev et al., 2011](https://doi.org/10.1038/ng.740)
| ANA-grade | Nymphaeaceae | *Victoria cruziana* | Present | [Wen et al., 2025](https://doi.org/10.1016/j.xplc.2025.101342)
| ANA-grade | Nymphaeaceae | *Nymphaea colorata*  | Absence | [Zhang et al., 2020](https://doi.org/10.1038/s41586-019-1852-5)
| Angiosperms | Amborellaceae | *Amborella trichopoda* cv. SantaCruz75 HAP1| Absence | [Carey et al., 2024](https://doi.org/10.1038/s41477-024-01858-x)
| Bryophytes | Marchantiaceae | *Marchantia polymorpha* subsp. *ruderalis* accessions Tak-1/2| Absence (outgroup) | Standard genome v7.1 [MarpolBase](https://marchantia.info)

## OrthoFinder
The OrthoFinder uses gene trees during ortholog inferences, which allows high-accuracy inferences and the examination of every orthologous relationship in the tree. The updated version of OrthoFinder further incorporates the species trees inferences using gene trees, followed by duplication-loss-coalescence analysis to detect gene duplication events. 

### Installation
```
conda create -n of3_env python=3.12
conda activate of3_env
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
# close the terminal and open a new one
conda install orthofinder
```
### Pre-process proteomes to get primary transcript
Download primary_transcript.py from https://github.com/OrthoFinder/OrthoFinder/blob/main/tools/primary_transcript.py.
Then place it in the proteomes folder that need to be processed.
Make sure all the proteome files end in .fa
```
cd /Users/morven/Desktop/Botany563/data/pre-process_proteomes
for f in *fa ; do python primary_transcript.py $f ; done
```
### Run OrthoFinder (v3.1.4) with user-specified rooted species tree
#### Construct rooted species tree file `species_tree_input.nwk`
```
(M_polymorpha,(A_trichopoda,((N_colorata,V_cruziana),(((C_houtteana,A_thaliana),(R_chinensis,F_vesca)),(S_prinophyllum,S_lycopersicum)))));
```
#### Execute OrthoFinder
```
conda activate of3_env

orthofinder -f /Users/morven/Desktop/Botany563/data/primary_proteomes -s /Users/morven/Desktop/Botany563/data/primary_proteomes/species_tree_input.nwk
```

## Multiple Sequence Alignment (MSA)
### MUSCLE

MUSCLE is a multiple sequence alignment algorithm using progressive alignment with iterative refinement. There could be bias towards the guide tree and we assume that the bias can be neglected. 

```
cd /Users/morven/Desktop/Botany563/data/alignment

muscle -align OG0000396_LOGs-curated.fa -output OG0000396_LOGs-curated_aligned_muscle.fa

muscle 5.3.osxarm64 []  8.6Gb RAM, 8 cores
Built Jul 31 2025 00:34:33
(C) Copyright 2004-2021 Robert C. Edgar.
https://drive5.com

[align OG0000396_LOGs-curated.fa]
Input: 74 seqs, avg length 215, max 249, min 77

00:00 4.0Mb   100.0% Derep 73 uniques, 1 dupes
00:00 4.2Mb  CPU has 8 cores, running 8 threads
00:04 203Mb   100.0% Calc posteriors
00:04 203Mb   100.0% UPGMA5         
00:04 220Mb   100.0% Consistency (1/2)
00:04 213Mb   100.0% Consistency (2/2)
00:04 213Mb   100.0% Refining         
```
### MAFFT

MAFFT is a multiple sequence alignment algorithm using progressive alignment with iterative refinement. It is based on fast Fourier transform. It is sensitive to guide tree errors.

```
cd /Users/morven/Desktop/Botany563/data/alignment

mafft --auto OG0000396_LOGs-curated.fa > OG0000396_LOGs-curated_aligned_mafft.fa

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
   70 / 74
done.

Progressive alignment ... 
STEP    43 /73 
Reallocating..done. *alloclen = 1513
STEP    73 /73 
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

   70 / 74
Segment   1/  1    1- 457
STEP 003-054-0  identical.   
Converged.

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
### trimAl - Automated Alignment Trimming 
trimAl is a tool for automated alignment trimming in large-scale phylogenetic analyses.
The option *-automated1* uses a heuristic selection of the automatic method based on similarity statistics, which is optimized for Maximum Likelihood phylogenetic tree reconstruction.

```
cd /Users/morven/Desktop/Botany563/data/alignment

trimal -in OG0000396_LOGs-curated_aligned_muscle.fa -out OG0000396_LOGs-curated_aligned_muscle_trimmed.fa -automated1

trimal -in OG0000396_LOGs-curated_aligned_mafft.fa -out OG0000396_LOGs-curated_aligned_mafft_trimmed.fa -automated1
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
3. Set working directory
```
setwd("/Users/morven/Desktop/Botany563/data/distance-based")
```
4. Loading the alignment data
```
aa <- read.phyDat("OG0000396_LOGs-curated_aligned_muscle_trimmed.fa",
                  format = "fasta",
                  type = "AA")
```
5. Computing the pairwise distances
```
D <- dist.ml(aa, model="LG")
```
6. Get the NJ tree
```
tre <- nj(D)
```
7. To get the ladderized effect when plotting the tree
```
tre <- ladderize(tre)
```
8. Plot the tree
```
plot(tre, cex=.6)
```
9. Save the NJ tree
```
write.tree(tre, file="OG0000396_LOGs-curated_aligned_muscle_trimmed_nj.nwk")
```
10. The tree is manually rooted using FigTree v1.4.4 

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
3. Loading the alignment file and read as phangorn object
```
aa <- read.phyDat("OG0000396_LOGs-curated_aligned_muscle_trimmed.fa",
                  format = "fasta",
                  type = "AA")
```
4. Generating a starting tree for the search on tree space and compute the parsimony of this tree
```
tre.ini <- nj(dist.ml(aa, model="LG"))
parsimony(tre.ini, aa)
```
5. Search for the tree with maximum parsimony
```
tre.pars <- optim.parsimony(tre.ini, aa)
```
6. Plot tree
```
plot(tre.pars, cex=0.6)
```
7. Save the NJ tree
```
write.tree(tre, file="OG0000396_LOGs-curated_aligned_muscle_trimmed_pars.nwk")
```
10. The tree is manually rooted using FigTree v1.4.4 

## Maximum Likelihood
Both IQ-TREE and RAxML-NG are package softwares that infer maximum likelihood (ML) tree. Maximum likelihood method assumes that the mutation process is the same at every branch of the tree, and all sites evolve the same and independently. Both assumes that the input alignment is correctly aligned. Specifically, IQ-TREE is not really searching tree space; instead, it keeps a pool of candidate trees, and it constantly evaluates and removes trees from this pool with stochastic NNI, which make it more computationally efficiently than RAxML. 

### IQ-TREE 3
#### Installation
Download `iqtree-3.1.1-macOS` from [IQ-Tree Website](https://iqtree.github.io/)
```
sudo mv /Users/morven/Downloads/iqtree-3.1.1-macOS /Applications/
cd /Applications/iqtree-3.1.1-macOS/bin
sudo cp iqtree3 /usr/local/bin
```
#### Execution
```
cd /Users/morven/Desktop/Botany563/data/iqtree
iqtree3 -s OG0000396_LOGs-curated_aligned_muscle_trimmed.fa -b 100 -nt AUTO
```

### RAxML-NG v. 2.0.0
#### Installation

Click `Download 64-bit macOS binary1` to download `raxml-ng_v2.0.0_macos.zip` from [raxml-ng](https://github.com/amkozlov/raxml-ng) github.

```
sudo mv /Users/morven/Downloads/raxml-ng_v2.0.0_macos /Applications/
cd /Applications/raxml-ng_v2.0.0_macos
ls # check the raxml-ng executable is there
sudo cp raxml-ng /usr/local/bin
```
#### Execution
```
cd /Users/morven/Desktop/Botany563/data/raxml

raxml-ng --all --msa OG0000396_LOGs-curated_aligned_muscle_trimmed.fa --model AA --bs-trees 100
```
### Visualization and comparisons of two ML trees in R
```
# Load libraries
library(treeio)    # read.raxml(), read.iqtree(), root methods for treedata
library(ggtree)    # plotting and geom_*2 helpers
library(ape)       # read.tree, comparePhylo if desired
library(phytools)  # cophylo tanglegram plotting
library(phangorn)  # RF.dist (Robinson-Foulds) and other tree distances
library(ggplot2)

# Set working directory 
setwd("/Users/morven/Desktop/Botany563/data/maximum-likelihood/tree-visual")

# RAxML tree with bootstrap values
raxml_td <- read.iqtree("OG0000396_LOGs-curated_aligned_muscle_trimmed.fa.raxml.support")
raxml_phy <- as.phylo(raxml_td)  # convert to phylo object

# IQ-TREE tree with bootstrap
iq_td <- read.iqtree("OG0000396_LOGs-curated_aligned_muscle_trimmed.fa.treefile")
iq_phy <- as.phylo(iq_td)

raxml_rooted <- root(raxml_phy, outgroup = c("M_polymorpha_Mp1g00270.1"), resolve.root = TRUE)
iq_rooted    <- root(iq_phy, outgroup = c("M_polymorpha_Mp1g00270.1"), resolve.root = TRUE)

raxml_rooted <- ladderize(raxml_rooted, right = FALSE)
iq_rooted    <- ladderize(iq_rooted, right = FALSE)

extract_bs <- function(tree) {
  if(is.null(tree$node.label)) return(NULL)
  as.numeric(tree$node.label)
}

raxml_bs <- extract_bs(raxml_rooted)
iq_bs    <- extract_bs(iq_rooted)

# Create ggtree objects
p1 <- ggtree(raxml_rooted) +
  geom_tiplab(size = 3) +
  geom_point2(aes(subset = !isTip & !is.na(label),
                  size = as.numeric(label)), color = "#FDD0A2AA") +
  geom_text2(aes(subset = !isTip & !is.na(label),
                 label = label), hjust=0.5, vjust=0.5, size=3) +
  scale_size_continuous(range=c(2,4)) +  # adjust circle sizes
  ggtitle("RAxML Tree")+
  guides(size = guide_legend(title = "Bootstrap support")) +
  xlim_tree(1)

p2 <- ggtree(iq_rooted) +
  geom_tiplab(size = 3) +
  geom_point2(aes(subset = !isTip & !is.na(label),
                  size = as.numeric(label)), color = "#4DBBD5AA") +
  geom_text2(aes(subset = !isTip & !is.na(label),
                 label = label), hjust=0.5, vjust=0.5, size=3) +
  scale_size_continuous(range=c(2,4)) +  # adjust circle sizes
  ggtitle("IQ-TREE Tree")+
  guides(size = guide_legend(title = "Bootstrap support")) +
  xlim_tree(1)

# Side-by-side layout
p1 + p2 + plot_layout(ncol = 2)

final_plot <- p1 + p2 + plot_layout(ncol = 2)

png("RAxML_vs_IQTREE.png",
    width = 5000,
    height = 3000,
    res = 350)

print(final_plot)

dev.off()
```
Comparing tree topologies to highlight clades uniquely shown by each inference method.
```
splits1 <- prop.part(raxml_rooted)
splits2 <- prop.part(iq_rooted)

split_labels1 <- sapply(splits1, function(x) sort(raxml_rooted$tip.label[x]))
split_labels2 <- sapply(splits2, function(x) sort(iq_rooted$tip.label[x]))

unique_to_raxml_rooted <- setdiff(split_labels1, split_labels2)
unique_to_iq_rooted <- setdiff(split_labels2, split_labels1)

p1 <- ggtree(raxml_rooted) + geom_tiplab(size = 2)
p2 <- ggtree(iq_rooted) + geom_tiplab(size = 2)

get_conflict_nodes <- function(tree, splits_unique) {
  nodes <- c()
  for (clade in splits_unique) {
    tips <- unlist(clade)
    node <- getMRCA(tree, tips)
    nodes <- c(nodes, node)
  }
  unique(nodes)
}

nodes1 <- get_conflict_nodes(raxml_rooted, unique_to_raxml_rooted)
nodes2 <- get_conflict_nodes(iq_rooted, unique_to_iq_rooted)

p1 <- ggtree(raxml_rooted) +
  geom_tiplab(size = 2) +
  geom_hilight(node = nodes1, fill = "#FDD0A2AA", alpha = 0.3) +
  ggtitle("RAxML (conflicting clades highlighted)") +
  xlim_tree(1)

p2 <- ggtree(iq_rooted) +
  geom_tiplab(size = 2) +
  geom_hilight(node = nodes2, fill = "#4DBBD5AA", alpha = 0.3) +
  ggtitle("IQ-TREE (conflicting clades highlighted)") +
  xlim_tree(1)

p1 + p2 + plot_layout(ncol = 2)

ggsave("tree_compare.png",
       width = 10,
       height = 6,
       dpi = 300)
```

## Bayesian method
### MrBayes
MrBayes uses Bayesian phylogenetic inference, which can incorporate prior to build likelihood. Bayesians can overcome the phylogenetic terrace by using prior. It gives a posterior distribution. When posterior distribution needs to be approximated using MCMC, it could be a long process. 
#### Intallation
```
conda install -c bioconda mrbayes
```
#### File format convertion

Convert alignment file (.fasta) to the NEXUS multiple alignment format (.nex) using seqmagick

1. Installation
```
pip install seqmagick
```
2. Convert fasta file to nexus file
```
cd /Users/morven/Desktop/Botany563/data/bayesian

seqmagick convert --output-format nexus --alphabet protein OG0000396_LOGs-curated_aligned_muscle_trimmed.fa OG0000396_LOGs-curated_aligned_muscle_trimmed.nex
```

#### Create a mrbayes block in a separate text file called mbblock.txt
```
begin mrbayes;
 set autoclose=yes;
 prset aamodelpr=fixed(jones);
 lset rates=invgamma ngammacat=4;
 mcmcp ngen=1500000 samplefreq=100 printfreq=1000 nruns=2 nchains=4 savebrlens=yes;
 outgroup M_polymorpha_Mp1g00270.1;
 mcmc;
 sump;
 sumt;
end;
```
#### Append the MrBayes block to the end of the nexus file with the data OG0000396_LOGs-curated_aligned_muscle_trimmed.nex
```
cd /Users/morven/Desktop/Botany563/data/bayesian

cat OG0000396_LOGs-curated_aligned_muscle_trimmed.nex mbblock.txt > OG0000396_LOGs-curated_aligned_muscle_trimmed-mb.nex
```
#### Execute MrBayes

```
conda activate mrbayes

mb OG0000396_LOGs-curated_aligned_muscle_trimmed-mb.nex
```
***Note**: The resulting tracer plots appeared irregular despite testing multiple parameter settings. Thus, BEAST was used instead for Bayesian method.* 

## Coalescent
### ASTRAL

ASTRAL algorithm is a coalescent-based method for reconstructing species trees using a set of gene trees.
Given a set of gene trees, we compute the probability of observing these gene trees under a candidate species tree. We then explore the species tree space by proposing alternative species trees and evaluating their probability based on the collection of gene trees, ultimately identifying the species tree that maximizes this probability. The standard coalescent model assumes constant population. The branch lengths in the species tree are not the actual time; instead, they are measured in coalescent units (e.g., generations scaled by effective population size, g/N). To obtain estimates of the actual time, a separate calibration analysis is required.

The ASTRAL was run using sample dataset *song_mammals.424.gene.tre* from the ASTRAL github.

#### Installation
Installed ASTRAL by following this [instruction](https://github.com/smirarab/ASTRAL). Downloaded `Astral.5.7.8.zip`, extracted the content, and moved to the `/Application` folder.

#### Execution
```
cd /Applications/Astral

java -jar astral.5.7.8.jar -i test_data/song_mammals.424.gene.tre -o test_data/song-astral.tre
```
## Co-estimation

### BEAST
BEAST (Bayesian Evolutionary Analyses by Sampling Trees) is a package for performing Bayesian inference using MCMC. We input sequence alignment, and the program estimates the gene tree and species tree at the same time, which considers the estimation errors. The algorithm assumes that the input data is correct, every site is evolving independently, and mutation rate is linear proportion to time. However, BEAST cannot handle too many taxa, thus, not very scalable. 

#### Installation
Download BEAST 2 (version 2.7.8) software from [BEAST Website](https://www.beast2.org/). 

#### Creating the Analysis File (in XML format) with `BEAUti`
1. Begin by starting BEAUti2
2. Importing the alignment
3. Under `Site Model` tab:
   - Subsitution rate = 1 and check box for estimate
   - Gamma Category Count = 4
   - Shape = 1 and check box for estimate
   - Proportion Invariant = 0
   - Subst Model = JTT
4. Under `Priors` tab:
   - Tree.t = Yule Model
   - birthRate.t = Uniform[0.0,Infinity]; initial = [1.0][0.0,Infinity]
   - gammaShape.s = Exponential[1.0]; initial = [1.0][0.1,Infinity]
5. Under `MCMC` tab:
   - Set the Chain Length to 1000000
   - Expand the tracelog options
      - Set the Log Every parameter to 200
      - Leave the filename as is ($(filebase).log)
   - Expand the screenlog options
      - Leave the Log Every parameter at the default value of 1000
   - Expand the treelog.t options
      - Set the File Name to OG0000396_LOGs-curated_aligned_muscle_trimmed.trees
      - Leave the Log Every parameter at the default value of 1000
6. Generating the XML file using File > Save

#### Running the analysis with `BEAST2`
1. Select OG0000396_LOGs-curated_aligned_muscle_trimmed.xml file as input file
2. Set the Random number seed to 777 
3. Run BEAST2 by clicking the Run button

#### Analysing the results with `Tracer`
File > Import Trace File…, then locate and click on OG0000396_LOGs-curated_aligned_muscle_trimmed.log

#### Producing MCC tree with `TreeAnnotator`
1. Open TreeAnnotator
2. Set the Burnin percentage to 10 to discard the first 10% of trees in the log file
3. Leave the Posterior probability limit at the default value of 0
4. Leave the Target tree type at the default value of Maximum clade credibility tree
5. Select Mean heights in the Node heights drop-down menu
6. Click Choose File next to Input Tree File and choose OG0000396_LOGs-curated_aligned_muscle_trimmed.trees
7. Set the Output File to OG0000396_LOGs-curated_aligned_muscle_trimmed.MCC.tree.
8. Run the program

#### Visualising the MCC tree in `FigTree`
1. Check the Node Bars checkbox, expand the options and select height_95%_HPD from the Display drop-down menu.
2. Check the Node Labels checkbox, expand the options and select posterior from the Display drop-down menu.