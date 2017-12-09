# PlaySynteny: a genome evolution practical

## Preparing your work environment

If you don't have it yet: create a folder in your $PATH in which you can put the programs that you want to be able to run from anywhere in your computer.

For instance:
`cd` 

`mkdir bin`

`export PATH=~/bin:$PATH`

If you want this to become permanent (i.e. everytime you log into your account you get ~/bin added to your $PATH then you should add the line `export PATH=~/bin:$PATH` to the .bashrc or .profile file at the root of your account; if no such file exists yet, create one).


## Installing the required programs

We will be using two ways of comparing genomes and looking into genome duplications/rearrangements: the traditional (but cumbersome) way, which compares the inferred proteome of each genome and searches for series of protein-coding genes with homologues in the same order in several places in the annotated genome sequence; and the more direct (but less sensitive) way of comparing directly unannotated genome sequences with each other.

For this we will be using two programs: MCScanX (https://academic.oup.com/nar/article/40/7/e49/1202057) by Yupeng Wang and minimap2 (https://arxiv.org/abs/1708.01492) by Heng Li.

Installing minimap2 is relatively easy:

`git clone https://github.com/lh3/minimap2.git`

`make`

`cp minimap2 ~/bin/` (or into any other folder of your $PATH)

To display the alignment produced by minimap2 in a graphical fashion ("Oxford plot") we will need the minidot program from another package by the same author:

`git clone https://github.com/lh3/miniasm.git`

`make`

`cp minidot ~/bin/`



Create a folder for this practical; cd into it.

Download the genome and proteome of Saccharomyces cerevisiae (Sce) from https://www.yeastgenome.org/ and Candida glabrata (Cgl) from http://www.candidagenome.org.

`wget https://downloads.yeastgenome.org/sequence/S288C_reference/genome_releases/S288C_reference_genome_Current_Release.tgz`

`tar xvf S288C_reference_genome_Current_Release.tgz`

`wget http://www.candidagenome.org/download/sequence/C_glabrata_CBS138/current/C_glabrata_CBS138_current_chromosomes.fasta.gz`

`gunzip C_glabrata_CBS138_current_chromosomes.fasta.gz`
`mv C_glabrata_CBS138_current_chromosomes.fasta Cgl.fasta
mv S288C_reference_sequence_R64-2-1_20150113.fsa Sce.fasta`


Compare the two genomes using minimap2 and draw a synteny plot using minidot (included into the miniasm package).
`minimap2 -xasm10 Sce.fasta Cgl.fasta > Sce_Cgl_asm10.paf`

For more information about the meaning of the columns in the PAF output file of minimap2, see https://github.com/lh3/miniasm/blob/master/PAF.md.

Use minidot to generate a dot plot:
`minidot -m 10000 Sce_Cgl_asm10.paf > Sce_Cgl_asm10.eps`

You can visualize the resulting EPS file using your favorite graphical program (e.g. Preview on OSX).

Install the the package MCScanX and add it to your $PATH.

Perform a MCScanX synteny analysis comparing the two genomes, and an analysis of the different types of gene duplicates found in each genome.

Download the proteome of C. glabrate: `wget http://www.candidagenome.org/download/sequence/C_glabrata_CBS138/current/C_glabrata_CBS138_current_orf_trans_all.fasta.gz`
gunzip it
rename it as e.g. Cgl.prot
rename the proteome of S. cerevisiae as Sce.prot

As per the manual of MCScanX:
`formatdb -p T -i Cgl.prot`
`blastall -i Sce.prot -d Cgl.prot -p blastp -e 1e-10 -b 5 -v 5 -m 8 -o Sce_Clg_blastp`
(this is using the older version of blast (the one suggested in the MCScanX manual); if your version is more recent the commandline will be slightly different).

To run MCScan X you also need a gff file telling the position of each gene along each scaffold: you can get the one for C. glabrata from http://www.candidagenome.org/download/gff/C_glabrata_CBS138/C_glabrata_CBS138_current_features.gff
and the one for S. cerevisiae from 



