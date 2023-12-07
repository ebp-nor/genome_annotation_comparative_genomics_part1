## The study organism
For the genome annotation exercise we will use [_Umbelopsis ramanniana_](https://en.wikipedia.org/wiki/Umbelopsis_ramanniana), a fungi common in soil. It is one of the species we are working on in the EBP-Nor project, and has a small genome which is really nice for our purposes here (everything is quicker). We will use this and other related species for comparative genomics later. 

## How to use this material
We have tried to write the first part of this workshop so you can work through it independently of other resources. The explanations hopefully succinct, but contains links to other resources where you can learn more. This is to both papers and websites. You do not have to read through these, but they are meant as resources, something you can refer back to if you want more information and to learn more. Please read both the text and the scripts carefully, and make sure to try to run the programs without any parameters to look at the information the programs output regarding different parameters. Please don't hesitate to ask questions. 

## Package management

Administrating the different programs that are needed in project can be a hassle. We like conda, especially [miniconda](https://docs.conda.io/en/latest/miniconda.html), and have set up different environments we will use for the different analyses. [Bioconda](https://bioconda.github.io) contain a lot of different packages that are relevant for us, and genomics and bioinformatics in general.

To load conda, do this:
```
eval "$(/fp/projects01/ec146/miniconda3/bin/conda shell.bash hook)" 
```

There are some of the different programs that are not available through conda. For most of these we use [Singularity containers](https://docs.sylabs.io/guides/3.5/user-guide/introduction.html). 

We mostly set up scripts and arranged data so it is ready to run, but ask you to modify them in some cases. We have backups of everything, but please be careful so you don't delete something you shouldn't.

## Infrastructure

For the different analyses we are doing, we will use [Educloud](https://www.uio.no/english/services/it/research/platforms/edu-research/). To use it, you need an account which you can get here: [https://research.educloud.no/register](https://research.educloud.no/register). The project we are using in this course is ec146, so please ask for access to that one, and we will let you in. 

We will do the work in this course at `/projects/ec146/work` on [Fox](https://www.uio.no/english/services/it/research/platforms/edu-research/help/fox/) which is the HPC part of Educloud. After creating an account, you can log in using `ssh <educloud-username>@fox.educloud.no`. You will be prompted for a One-Time Code for a 2-factor authenticator app (Microsoft Authenticator) and your Fox/Educloud password.

On Fox we will submit jobs/analyses as job scripts. This is for a system called SLURM. Basically, this is instructions to the system for what kind of analysis we are running, or more concretely, how much memory and computing power we need. 

A generic job script might look like this (copied from [Job Scripts on Fox](https://www.uio.no/english/services/it/research/platforms/edu-research/help/fox/jobs/job-scripts.md)):
```
#!/bin/bash

# Job name:
#SBATCH --job-name=YourJobname
#
# Project:
#SBATCH --account=ecXXX
#
# Wall time limit:
#SBATCH --time=DD-HH:MM:SS
#
# Other parameters:
#SBATCH ...

## Set up job environment:
set -o errexit  # Exit the script on any error
set -o nounset  # Treat any unset variables as an error

module --quiet purge  # Reset the modules to the system default
module load SomeProgram/SomeVersion
module list

## Do some work:
YourCommands
```

|[Next](https://github.com/ebp-nor/genome_annotation_comparative_genomics_part1/blob/main/01_repeatmasking.md)|
|---|
