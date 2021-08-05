# seaConnect--compGenomics

Comparative seascape genomics of two commercially important fish species: the striped red mullet _Mullus surmuletus_ and the white seabream _Diplodus sargus._
Codes for the paper "Climate differently influences genomic patterns of two sympatric marine fish species" by Boulanger E., Benestan L, Guerin P. E., Dalongeville A., Mouillot D. and Manel S. (in revision).

## Data
GBS SNP calling was performed by [P. Guerrin using a dDocent-based pipeline](https://github.com/Grelot/seaConnect--dDocent)    
SNP filtering, outlier detection and format conversions was performed by [E. Boulanger in the project seaConnect--dataPrep](https://github.com/eboulanger/seaConnect--dataPrep)      


## Dependencies
You will need to install the following software:  
- [PGDSpider](http://www.cmpg.unibe.ch/software/PGDSpider/) 
- [ANVAGE](https://github.com/Grelot/anvage)

And you will need to have the following R libraries:  
adegenet, vegan, ggplot2, reshape2, pophelper, maps, mapdata, dplyr, plotrix, ...

## 00-Data
- compute dbMEM following the [tutorial](https://github.com/laurabenestan/Moran-Eigenvector-Maps-MEMs) of L. Benestan.
- compute AEM

## 01-genDiv
- compute genetic diversity maps of spatially replicated individuals
- calculate and compare individual heterozygosity

## 02-DAPC

Infer population structure with DAPC from [adegenet](http://adegenet.r-forge.r-project.org/files/tutorial-dapc.pdf) using outlier (& putatively adaptive) SNPs.

Interactive R-scripts `dip_dapc.R` and `mul_dapc.R`
This script runs two types of DAPC:  
- with the Mediterranean ecoregions set as prior groups
- with the detection of best-fit number of populations  

For both a scatterplot is created. 
For the latter, a barplot of the posterior membership probabilities with individuals ordered by longitude within Mediterranean Sea Ecoregions.
A pie map with the posterior membership probabilities is also created.

## 03-dbRDA

Constrained ordination to infer if genetic structure is driven by certain 
environmental and spatial  variables & look for signals of local adaptation.

Environmental variables considered:  
- Sea Surface Salinity  
- Sea Surface Temperature : separated winter and summer averages
- Chlorophyll a levels as proxies for productivity
- geographic distance: dbMEMs (x for _D. sargus_, x for _M. surmuletus_)  
- larval connectivity: AEMs   (x for _D. sargus_, x for _M. surmuletus_)  

We have too many potential explanatory variables so we use ordi2step to perform variable selection.
- `run_rda_neutral|adaptive.R` : run RDA with previously selected dbMEMs and AEMs, as environmental variables

## 04-annotationVariant

- Seek positions of variants onto genome and check if they are on an annotated region e.g.  coding region. 
- keep only non-synonymous variants in coding regions
- retrieve the flanking DNA sequences centered on the variant positions 
- align sequences against UNIPROT-swissprot sequences database using `blastx` and retrieve UNIPROT id

