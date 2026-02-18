# Botany 563 project - Repeated evolution of plant prickles
Prickles are sharp epidermal or cortical outgrowths that have evolved at least 28 times across tracheophytes. The development of prickles in Solanum, as well as other plant genera, is dependent on the co-option of LONELY GUY (LOG) family plant hormone biosynthetic genes, which encode enzymes that catalyze the final step of cytokinin (CK) biosynthesis, a key regulator of cell proliferation. This project aims to reconstruct the phylogenetic tree of LOG protein family and test whether specific subclade of LOG family associated with prickle development have undergone positive selection.
## Description of dataset
 I will use LOG protein sequences from at least 28 representative genus with instance of independent prickle evolution. I will obtain my sequences from published genome assembly papers and solpangenomics database (https://solpangenomics.com/dist/pages/downloads/index.php). LOG orthologs will be identified using OrthoFinder. 

Representative species list: 
- Solanaceae Solanum prinophyllum 
- Araliaceae 	Aralia spinosa DOI: 10.3389/fpls.2022.822942
- Campanulaceae	Cyanea solanaceae	
- Solanaceae	Datura stramonium	
- Caprifoliaceae	Dipsacus fullonum	
- Asteraceae	Latuca serriola	
- Cleomaceae	Tarenaya hassleriana	
- Convolvulaceae	Ipomoea setosa	
- Brassicaceae	Brassica nigra	
- Cyatheaceae	Alsophila spinulosa [Huang et al., 2022](https://doi.org/10.1038/s41477-022-01146-6)	
- Zamiaceae	Zamia furfuracea	
- Smilacaceae	Smilax rotundifolia	
- Poaceae	Oyrza sativa	
- Pandanaceae	Pandanus utilis	
- Arecaceae	Socratea exorrhiza	
- Arecaceae	Astrocaryum alatum	
- Dioscoreaceae	Dioscorea basiclavicaulis	
- Nymphaeaceae	Victoria amazonica	
- Rosaceae	Rosa chinense [Hibrand Saint-Oyant et al., 2018](https://doi.org/10.1038/s41477-018-0166-1)
- Fabaceae	Prosopis cineraria	
- Rhamnaceae	Ziziphus jujubae	
- Rutaceae	Zanthoxylum piperitum	
- Euphorbiaceae	Euphorbia milii	
- Malvaceae	Hibiscus cannabinus	
- Euphorbiaceae	Hura crepitans	
- Cucurbitaceae	Cucumis melo	
- Bixaceae	Bixa orellana	
- Grossulariaceae	Ribes lacustre	
- Vitaceae Vitis romanetii
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
