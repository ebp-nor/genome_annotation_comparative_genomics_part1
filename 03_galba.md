## GALBA
[Miniprot](02_miniprot) will only output alignments based on the proteins it aligns. It cannot provide any gene models not supported by the input proteins. To cover our bases (nucleotides) we also want to run an _ab initio_ gene predictor. As you might have gotten the impression from the miniprot paper if you read that, not that much has happened in the genome annotation world the last 10 years or so. There have been (continued) development of some _ab initio_ gene predictors, mainly [AUGUSTUS](https://github.com/Gaius-Augustus/Augustus) and [GeneMark](http://exon.gatech.edu/GeneMark/), but no new programs as far as we know. GeneMark has a lisence which might make it harder to use in many cases, but AUGUSTUS is free and open. 

The developers of AUGUSTUS have through the years developed different programs that wrap and train AUGUSTUS using different datasets (BRAKER1 with RNA-seq data, BRAKER2 with protein data, both available from https://github.com/Gaius-Augustus/BRAKER). The latest wrapper is called [GALBA](https://github.com/Gaius-Augustus/GALBA), and aligns proteins using miniprot and is trained on those aligned proteins. 

Here, we will use GALBA on the proteins from a related fungus to generate a set up _ab initio_ predicted genes.




Why do we copy both the genome assembly and the protein sequences to the current folder?

