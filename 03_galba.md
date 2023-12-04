## GALBA
[Miniprot](02_miniprot) will only output alignments based on the proteins it aligns. It cannot provide any gene models not supported by the input proteins. To cover our bases (nucleotides) we also want to run an _ab initio_ gene predictor. As you might have gotten the impression from the miniprot paper if you read that, not that much has happened in the genome annotation world the last 10 years or so. There have been (continued) development of some _ab initio_ gene predictors, mainly [AUGUSTUS](https://github.com/Gaius-Augustus/Augustus) and [GeneMark](http://exon.gatech.edu/GeneMark/), but no new programs as far as we know. GeneMark has a lisence which might make it harder to use in many cases, but AUGUSTUS is free and open. 

The developers of AUGUSTUS have through the years developed different programs that wrap and train AUGUSTUS using different datasets (BRAKER1 with RNA-seq data, BRAKER2 with protein data, both available from https://github.com/Gaius-Augustus/BRAKER). The latest wrapper is called [GALBA](https://github.com/Gaius-Augustus/GALBA), and aligns proteins using miniprot and is trained on those aligned proteins. 

Here, we will use GALBA on the proteins from a related fungus to generate a set up _ab initio_ predicted genes.

You can run GALBA with a script similar to this: 
```
#!/bin/bash
#SBATCH --job-name=galba
#SBATCH --account=ec146
#SBATCH --time=1:0:0
#SBATCH --mem-per-cpu=1G
#SBATCH --ntasks-per-node=10

eval "$(/fp/projects01/ec146/miniconda3/bin/conda shell.bash hook)"

conda activate anno_pipeline

singularity exec -B $PWD:/data /projects/ec146/opt/galba/galba.sif cp -rf /usr/share/augustus/config /data/
singularity exec -B $PWD:/data /projects/ec146/opt/galba/galba.sif galba.pl --version > galba.version
singularity exec -B $PWD:/data /projects/ec146/opt/galba/galba.sif galba.pl --species=$2 --threads=10 \
--genome=/data/$1 \
--verbosity=4 --prot_seq=/data/$3  --workingdir=/data \
--augustus_args="--stopCodonExcludedFromCDS=False" --gff3 \
--AUGUSTUS_CONFIG_PATH=/data/config \
1> galba_"`date +\%y\%m\%d_\%H\%M\%S`".out 2> galba_"`date +\%y\%m\%d_\%H\%M\%S`".err

agat_sp_extract_sequences.pl --gff galba.gtf -f $1  -t cds -p -o galba.proteins.fa
```
Here we use a [Singularity container](https://docs.sylabs.io/guides/3.5/user-guide/introduction.html), because sometimes it can be a hassle to set up everything properly even with Conda. 

Why do we copy both the genome assembly and the protein sequences to the current folder?

When we ran this, it took about 50 minutes.
