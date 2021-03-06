# chromVARmotifs

The goal of chromVARmotifs is to make it easy to use several different motif collections in R, particularly for use with motifmatchr and chromVAR packages.  Motifs are stored as PWMatrixList objects (object from TFBSTools package).  

## Installation

Installation is easiest using the devtools package. The function `install_github` will install the package.

``` r
devtools::install_github("GreenleafLab/chromVARmotifs")
```

The TFBSTools package from Bioconductor is the only direct dependency; the PWMatrixList object from that package is used to store the motifs.

## Motif collections included:

``` r
data("human_pwms_v1") #curated collection of human motifs from cisBP database
data("mouse_pwms_v1") #curated collection of mouse motifs from cisBP database
data("human_pwms_v2") #filtered collection of human motifs from cisBP database
data("mouse_pwms_v2") #filtered collection of mouse motifs from cisBP database
data("homer_pwms") #motifs from HOMER
data("encode_pwms") #motifs from ENCODE
```

## Version 2

The script used to make the "version 2" motifs (curated from the large master list for 
less redundancy) is shown below:


```r
library(chromVARmotifs)
library(dplyr)
library(tidyr)
library(data.table)
data("human_pwms_v1")
data("mouse_pwms_v1")

sout <- sapply(strsplit(names(human_pwms_v1), split = "_"), function(s) c(s[3]))
human_pwms_v2 <- human_pwms_v1[match(unique(sout), sout)]

sout <- sapply(strsplit(names(mouse_pwms_v1), split = "_"), function(s) c(s[3]))
mouse_pwms_v2 <- mouse_pwms_v1[match(unique(sout), sout)]

save(human_pwms_v2, file = 'human_pwms_v2.rda')
save(mouse_pwms_v2,  file = 'mouse_pwms_v2.rda')
```

## Conversion to PWM

Motifs originally in position frequency matrix form were converted to PWMs as follows:

1) Except for `homer_pwms`, a 0.008 pseudocount was added. Frequencies were then re-normalized to sum to 1.
2) Frequencies were divided by 0.25
3) Computed the log of the relative frequencies

## Curation of cisBP motifs

We curated the motifs from the cisBP database to ensure each TF regulator was represented by at least one motif while reducing redundancy for the motifs linked to each TF>  

## Sources

`encode_pwms` are based on http://compbio.mit.edu/encode-motifs/
`homer_pwms` are based on http://homer.ucsd.edu/homer/custom.motifs
`human_pwms_v1` and `mouse_pwms_v1` are based on http://cisbp.ccbr.utoronto.ca/


<br><br>


