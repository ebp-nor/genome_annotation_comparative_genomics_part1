## Masking repeats
When annotating a genome assembly, the first step is usually to mask the repeats. This can be thought of as a two-step process. First find the repeats, and then mask them. 'Repeats' in this context means all different kinds of sequences that are repeated across a genome. This might be interspered repeats such as transposable elements which spread themselves across a genome, or tandem repeats, such as short tandem repeats which consists of a motif repeated in tandem (for instance 'AC' repeated six times: ACACACACACAC). There might be multiple reasons for masking repeats, but the two main ones would be to avoid creating a lot of alignments due to the repetitive sequence itself, and to avoid annotating genes found in transposable elements as the genes of the host. Arguably, it could be interesting to actually annotate the genes in transposable elements properly, so to better characterize them, but this is usually outside the scope of a genome annotation project. 

The actual masking itself can be done either hard or soft. Hard masking would entail replacing the found repeats with either 'X' or 'N', thereby hiding the sequence itself and making it unavailable for alignments. The other is soft masking. Nucleotides in fasta files are usually in capital letters (ACTG) and with soft masking the nucleotides in repeats would be replaces with the same, but in lower case (actg). This makes it possible for different tools to avoid creating alignments in these repeats, but alignments can be extended into the repeats. Some tools, such as [miniprot](https://github.com/lh3/miniprot), ignores soft masking (that is, it has no effect on the program). 

People often use [RepeatMasker](https://www.repeatmasker.org/) when masking repeats. This is done by first creating a repeat library with  [RepeatModeler](http://www.repeatmasker.org/RepeatModeler/) ([Flynn et al (2020)](https://doi.org/10.1073/pnas.1921046117), resulting in a file with different repeats charaterised ((into different classes of transposable elements for instance). 

If you do go that route, we would recommend using a [container](https://github.com/Dfam-consortium/TETools). RepeatMasker can use quite a lot of resources when running. It will try to characterise the repeats it finds (into different classes of transposable elements for instance), but this is not needed for annotating the genes of a species. It is, however, essential if you are interested in different transposable elements and where they are. Here, we settle for a program that uses a lot less resources, namely Red ([Giris 2015](https://doi.org/10.1186/s12859-015-0654-5)). Red and an associated Python script [redmask.py](https://github.com/nextgenusfs/redmask) as been set up on Fox using conda and downloading the redmask GitHub repository.  

To facilitate error management and to have a system we would like you to create a folder at 
```
/projects/ec146/work
```
with you username for instance. You can do this by 
```
mkdir -p /projects/ec146/work/$USER
```

Please navigate to this location (`cd /projects/ec146/work/$USER`) and create a subfolder called 'annotation' and enter that. This will be our working area for now. 

Create a subfolder called 'softmask', and enter it. 

To run Red, you can use a script like this:
```
#!/bin/bash
#SBATCH --job-name=red
#SBATCH --account=ec146
#SBATCH --time=1:0:0
#SBATCH --mem-per-cpu=10G
#SBATCH --ntasks-per-node=1

eval "$(/fp/projects01/ec146/miniconda3/bin/conda shell.bash hook)"

conda activate anno_pipeline

python /projects/ec146/opt/redmask/redmask.py -i ${1} -o ${2}
```
We have set this script up for you, so you can create a `run.sh` in you current folder (`/projects/ec146/work/$USER/annotation/softmask` presumably) with this content (with `nano` for instance):
```
ln -s /projects/ec146/data/gzUmbRama1.contigs.fasta .
sbatch /projects/ec146/scripts/annotation/run_red.sh gzUmbRama1.contigs.fasta gzUmbRama1
```

If you then type `sh run.sh` you will submit this job to the cluster.

|[Previous](https://github.com/ebp-nor/genome_annotation_comparative_genomics_part1/blob/main/00_introduction.md)|[Next](https://github.com/ebp-nor/genome_annotation_comparative_genomics_part1/blob/main/00_introduction.md)|
|---|---|
