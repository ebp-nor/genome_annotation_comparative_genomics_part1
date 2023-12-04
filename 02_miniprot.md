## Miniprot
[Miniprot](https://github.com/lh3/miniprot) might be the most exciting that has happened in the genome annotation world for many years. Miniprot ([Li 2023](https://academic.oup.com/bioinformatics/article/39/1/btad014/6989621), which is a nice paper on its own, worth a read) maps proteins to a (nucleotide) genome, and basically creates gene models based on the mapped proteins. Since it is so fast, you can quickly map some proteins to a genome and get an impression of the genic content. For closely related species you might not need anything else than mapping the predicted proteins from one (well-assembly and well-annotated) species to another. However, since it is so fast, we like to map different datasets to the genome and merge the resulting aligments with [EvidenceModeler](04_evidencemodeler). We usally use proteins from a well-annotated and well-assembled relatively closely related species, for instance human for mammals. In addition, [UniProtKB/Swiss-Prot](https://academic.oup.com/nar/article/51/D1/D523/6835362) is a database with curated protein models which might contain protein evidence that might be lacking from the first dataset. Lastly, we often align the relevant part of [OrthoDB v11](https://academic.oup.com/nar/article/51/D1/D445/6814468). This is a large dataset, and might take substantial time and computational power, so we skip aligning OrthoDB proteins here.

Here, we will align UniProtKB/Swiss-Prot in additon to proteins from a related fungus. 
  
We suggest that you create a subfolder called `miniprot` at `/projects/ec146/work/$USER/annotation`. Enter the new folder. We have created two scripts to run miniprot, one for UniProtKB/Swiss-Prot and one for the related fungus. 

This is how the first one looks: 
```
#!/bin/bash
#SBATCH --job-name=miniprot
#SBATCH --account=ec146
#SBATCH --time=1:0:0
#SBATCH --mem-per-cpu=1G
#SBATCH --ntasks-per-node=5

eval "$(/fp/projects01/ec146/miniconda3/bin/conda shell.bash hook)"

conda activate anno_pipeline

miniprot -ut5 --gff $1 /projects/ec146/data/funannotate_db/uniprot_sprot.fasta > mp_uniprot_sprot.gff 2> miniprot_"`date +\%y\%m\%d_\%H\%M\%S`".err

agat_sp_extract_sequences.pl --gff mp_uniprot_sprot.gff -f $1 -t cds -p -o uniprot_sprot.proteins.fa \
1> agat_proteins_"`date +\%y\%m\%d_\%H\%M\%S`".out 2> agat_proteins_"`date +\%y\%m\%d_\%H\%M\%S`".err

/projects/ec146/scripts/annotation/miniprot_GFF_2_EVM_GFF3.py mp_uniprot_sprot_proteins.gff > mp_uniprot_sprot_evm.gff
```

You can run this by using this `run.sh` script:
```
ln -s /projects/ec146/data/gzUmbRama1.contigs.fasta .
ln -s /projects/ec146/data/GCF_025201355.1_Halrad1_protein.faa  .
sbatch /projects/ec146/scripts/annotation/run_miniprot_swissprot.sh gzUmbRama1.contigs.fasta
sbatch /projects/ec146/scripts/annotation/run_miniprot_model.sh gzUmbRama1.contigs.fasta
```
This `run.sh` script also starts the other miniprot job. If you look at the script itself, you'll see in addition to miniprot two other programs, `agat_sp_extract_sequences.pl` and `miniprot_GFF_2_EVM_GFF3.py`.  `agat_sp_extract_sequences.pl` is from [AGAT](https://github.com/NBISweden/AGAT) (short for Another Gtf/Gff Analysis Toolkit). It is a really useful set of tools if you need to convert between different formats, create the predicted proteins from the annotation/alignments (as used here) or for adding functional annotation information to the annotation (shown later). `miniprot_GFF_2_EVM_GFF3.py` is an utility that is provided together with EvidenceModeler to convert the output of miniprot into something EvindenceModeler understands (but not found in the current release yet).   

You should investigate what the different options to the different programs mean. Usually you will get this information by running the program without any options, or by the option -h. Some options might not be relevant for your proposes, or we might have gotten something wrong, so it is always good to check.

The alignment of UniProtKB/Swiss-Prot proteins took around 50 minutes when we ran it, but alignment of the related fungus proteins only took a couple of minutes. These jobs can be started independently of the softmasking.
