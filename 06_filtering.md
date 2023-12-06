## Filtering based on protein lenght and similarity to repeat proteins

After EvidenceModeler we have a set of genes that is the consensus of the input. However, while we ran [GALBA](03_galba.md) on the softmasked genome assembly, we mapped all of [UniProtKB/Swiss-prot](02_miniprot.md) against the unmasked genome assembly. UniProtKB/Swiss-prot contain all kinds of known proteins, also proteins that are found in transposable elements which we don't want to annotate here. And while softmasking with Red would find all (or most at least) repetitive parts of the genome, there might be transposable elements that only occur once and which might therefore not be masked. To address this, we compare all predicted proteins against a set of repeat proteins that are available through Funannotate. Further, we might have several short proteins that might not be real, so we will apply a filter on length also.

All this can be done with this script:
´´´
#!/bin/bash
#SBATCH --job-name=filter
#SBATCH --account=ec146
#SBATCH --time=1:0:0
#SBATCH --mem-per-cpu=1G
#SBATCH --ntasks-per-node=5

eval "$(/fp/projects01/ec146/miniconda3/bin/conda shell.bash hook)" 

conda activate anno_pipeline

MIN_AA_SIZE=50

diamond blastp --sensitive --query $1 --threads 5 --out repeats.tsv --db  /fp/projects01/ec146/data/funannotate_db/repeats.dmnd --evalue 1e-10 --max-target-seqs 1 --outfmt 6

cut -f 1 repeats.tsv |sort -u > kill.list

rm -rf removed_repeats*
agat_sp_filter_feature_from_kill_list.pl --gff $2 --kill_list kill.list -o removed_repeats.gff

rm -rf filtered_genes*
agat_sp_filter_by_ORF_size.pl --gff removed_repeats.gff -s $MIN_AA_SIZE -o filtered_genes.gff

agat_sp_extract_sequences.pl --gff filtered_genes_sup${MIN_AA_SIZE}.gff -f $3 -t cds -p -o filtered.proteins.fa 1> agat_proteins_"`date +\%y\%m\%d_\%H\%M\%S`".out 2> agat_proteins_"`date +\%y\%m\%d_\%H\%M\%S`".err
´´´

You can run this script by this `run.sh` script:
```
sbatch /projects/ec146/scripts/annotation/run_filter.sh \
../evm_gene/evm.proteins.fa \
../evm_gene/evm.gff3 \
../softmask/gzUmbRama1.softmasked.fa
```

This should only take a minute or so.