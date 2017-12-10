# PlaySynteny: a genome evolution practical

## Preparing your work environment

If you don't have it yet: create a folder in your $PATH in which you can put the programs that you want to be able to run from anywhere in your computer.

For instance:
`cd` (to go back to your home directory)

`mkdir bin`

`export PATH=~/bin:$PATH`

If you want this to become permanent (i.e. everytime you log into your account you get ~/bin added to your $PATH then you should add the line `export PATH=~/bin:$PATH` to the .bashrc or .profile file at the root of your account; if no such file exists yet, create one).


## Installing the required programs

We will comparing genomes and looking into genome duplications/rearrangements. Instead of using the traditional (but cumbersome) way, which compares the inferred proteome of each genome and searches for series of protein-coding genes with homologues in the same order in several places in the annotated genome sequence, we will choose the more direct (but less sensitive) way of comparing directly unannotated genome sequences with each other. As this approach is less sensitive, it only allows detecting duplicates that are relatively recent.

For this we will be using minimap2 (https://arxiv.org/abs/1708.01492), a recent program by Heng Li.

Installing minimap2 is relatively easy:

`git clone https://github.com/lh3/minimap2.git`

`make`

`cp minimap2 ~/bin/` (or into any other folder of your $PATH)

To display the alignment produced by minimap2 in a graphical fashion ("Oxford plot") we will need the minidot program from another package by the same author:

`git clone https://github.com/lh3/miniasm.git`

`make`

`cp minidot ~/bin/`


## Basic commands

Align two genomes with minimap2 (authorizing up to 10% difference):
`minimap2 -xasm10 genome1.fasta genome2.fasta > genome1_vs_genome2.paf`
(for more info regaring the PAF alignment format, see https://github.com/lh3/miniasm/blob/master/PAF.md)

Display a PAF alignment in a graphical way:
`minidot -f 5 -d genome1_vs_genome2.paf > genome1_vs_genome2.eps`

If you are comparing a genome with itself, you should add the -X option "skip self and dual mappings":
`minimap2 -xasm10 genome1.fasta genome1.fasta -X > genome1_vs_genome1.paf`. Then if you want to display also the self-hits on the same plot you can add `minimap2 -xasm10 genome1.fasta genome1.fasta >> genome1_vs_genome1.paf` (`>>` will append the self-hits at the bottome of the file instead of overwriting it).

You can visualize the resulting EPS plot file using your favorite graphical program (e.g. Preview on OSX). If you have been working on a cluster and need to copy the EPS file into your local computer to visualize it, you can get it using the command `scp`, e.g. `scp [user@]host:file .`.


## Downloading and preparing the sequence data

Download the assembled chromosomes from two strains of the baker's yeast *Saccharomyces cerevisiae*: S288C (the standard reference strain isolated years ago from a rotten fig) and Kyokai7 (a sake strain from Japan, https://doi.org/10.1093/dnares/dsr029). 

For S288C, the genome is available from https://www.yeastgenome.org/ (go to "Sequence", "Reference Genome", "Download Genome" or use the direct link
`wget https://downloads.yeastgenome.org/sequence/S288C_reference/genome_releases/S288C_reference_genome_Current_Release.tgz` (to uncompress such gzipped tar archive, use the commande `tar xvf`).

For Kyokai7, go to https://www.ncbi.nlm.nih.gov/nuccore/?term=DG000037:DG000052[accn], choose "Send to", "Complete Record", "Choose Destination: File", "Format: FASTA" and "Sort by: Organism name" (so that the chromosome sequences are arranged in the proper order).

Prior to further analyses, it is advisable to simplify the headers of each FASTA file by using e.g. the command `sed`. To visualize the headers of each fasta, you can use `grep '>'`. Ideally the final headers should be something like "S288C_I", "S288C_II", S288C_III" etc. (or "Kyokai7_I", "Kyokai7_II" etc. for the other strain).

Download the genome sequence from the bdelloid rotifer *Adineta vaga* (http://dx.doi.org/10.1038/nature12326) using the link http://www.genoscope.cns.fr/adineta/data/Adineta_vaga_v2.0.scaffolds.fa.gz. Use the command `gunzip` to unzip the file. As this genome is quite fragmented, we will only keep the 50 largest scaffolds: first figure out the line number of the 51st scaffold using `grep -n`, then keep only the lines till that point using the command `head`. You can also use the `sed` command to shorten the names of the scaffolds (by removing "scaffold_" and the size information).

## Practical questions

Compare the genomes of S288C and Kyokai7 using minimap2, and display the result. How many structural differences of which type do you detect between the two genomes, and on which chromosome(s)?

Compare the genomes of S288C and Kyokai7 with themselves using minimap2. What do you see? How do you interpret it?

Now compare the genome of the bdelloid rotifer *Adineta vaga* with itself using minimap. What do you see? How do you interpret it?

Hint: the genomes of S288C and Kyokai7 are haploid (as is the case for most published genome assemblies), whereas in the case of *Adineta vaga* a diploid assembly aimed at separating haplotypes was performed. Since *Adineta vaga* is asexual and reproduces mitotically, haplotypes do not recombine via meiosis and can be considered as duplicated genome regions.

