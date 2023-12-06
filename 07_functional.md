## Functional annotation

After filtering the EvidenceModeler genes we have a set of genes we (hopefully) trust, and which are (hopefully) correct. However, we don't actually know what each gene does, nor what it is named. Not all genes have a known function, nor name, but we have some tools that can help on this situation. Proteins usually contain domains that can be informative with regards to their function. [InterProScan](https://github.com/ebi-pf-team/interproscan) ([Jones et al (2014)](https://academic.oup.com/bioinformatics/article/30/9/1236/237988)) is a tool that annotates known domains in proteins by comparisons to several different databases, such as PFAM and Panther among many others. 

In addition to annotating domains, it is useful to attach gene names to the proteins. This can be done by a simple comparison using [DIAMOND](https://github.com/bbuchfink/diamond), similar to what we did for [filtering](06_filtering.md). 

Comparisons against UniProtKB/Swiss-prot can be done with this script:
```
#!/bin/bash
#SBATCH --job-name=uniprot
#SBATCH --account=ec146
#SBATCH --time=1:0:0
#SBATCH --mem-per-cpu=1G
#SBATCH --ntasks-per-node=5

eval "$(/fp/projects01/ec146/miniconda3/bin/conda shell.bash hook)" 

conda activate anno_pipeline

mkdir -p uniprot

diamond blastp \
--query $1 \
--db /projects/ec146/data/funannotate_db/uniprot_sprot.fasta  \
--outfmt 6 \
--sensitive \
--max-target-seqs 1 \
--evalue 1e-25 \
--threads 5 \
> uniprot/diamond.blastp.out
```
You can run this by 
```
sbatch /projects/ec146/scripts/annotation/run_uniprot.sh ../filter/filtered.proteins.fa
```
Please put that command into a `run.sh` script as usual to keep track of what has been done.


