# PlaySynteny: a genome evolution practical

Connect via ssh to the hydra cluster of ULB (hydra.ulb.ac.be)

Create a folder for this practical; cd into it.

If you don't have it yet: create a folder in your $PATH in which you can put the programs that you want to be able to run from anywhere in your computer.

For instance:
`cd` 

`mkdir bin`

`export PATH=~/bin:$PATH`

If you want this to become permanent (i.e. everytime you log into your account you get ~/bin added to your $PATH then you should add the line `export PATH=~/bin:$PATH` to the .bashrc or .profile file at the root of your account (if no such file exists yet, create one).

Download the genome and proteome of Saccharomyces cerevisiae (Sce) from https://www.yeastgenome.org/ and Candida glabrata (Cgl) from http://www.candidagenome.org.

`wget https://downloads.yeastgenome.org/sequence/S288C_reference/genome_releases/S288C_reference_genome_Current_Release.tgz`

`tar xvf S288C_reference_genome_Current_Release.tgz`

`wget http://www.candidagenome.org/download/sequence/C_glabrata_CBS138/current/C_glabrata_CBS138_current_chromosomes.fasta.gz`

`gunzip C_glabrata_CBS138_current_chromosomes.fasta.gz`
`mv C_glabrata_CBS138_current_chromosomes.fasta Cgl.fasta
mv S288C_reference_sequence_R64-2-1_20150113.fsa Sce.fasta`

Install miniasm and minimap2 and add them to your $PATH.
`git clone https://github.com/lh3/minimap2.git
make`




Compare the two genomes using minimap2 and draw a synteny plot using minidot (included into the miniasm package).

`minimap2 -xasm10 Sce.fasta Cgl.fasta > Sce_Cgl_asm10.paf`

Install the the package MCScanX and add it to your $PATH.

Perform a MCScanX synteny analysis comparing the two genomes, and an analysis of the different types of gene duplicates found in each genome.

