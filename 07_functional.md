## Functional annotation

After filtering the EvidenceModeler genes we have a set of genes we (hopefully) trust, and which are (hopefully) correct. However, we don't actually know what each gene does, nor what it is named. Not all genes have a known function, nor name, but we have some tools that can help on this situation. Proteins usually contain domains that can be informative with regards to their function. [InterProScan](https://github.com/ebi-pf-team/interproscan) ([Jones et al (2014)](https://academic.oup.com/bioinformatics/article/30/9/1236/237988)) is a tool that annotates known domains in proteins by comparisons to several different databases, such as PFAM and Panther among many others. 

In addition to annotating domains, it is useful to attach gene names to the proteins. This can be done by a simple comparison using [DIAMOND](https://github.com/bbuchfink/diamond)
