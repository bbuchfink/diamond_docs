# Introduction

DIAMOND is a sequence aligner for protein and translated DNA searches, designed for high performance analysis of big sequence data. The key features are:

- Pairwise alignment of proteins and translated DNA at 500x-20,000x speed of BLAST.
- Frameshift alignments for long read analysis.
- Low resource requirements and suitable for running on standard desktops or laptops.
- Various output formats, including BLAST pairwise, tabular and XML, as well as taxonomic classification.

DIAMOND was published in Nature Methods in 2015 . Please cite this paper when using the program in published research.

# Quick start guide

We assume to have a protein database file in FASTA format named `nr.faa`
and a file of DNA reads that we want to align named `reads.fna`. In
order to set up a reference database for DIAMOND, the makedb command
needs to be executed with the following command line:

> `diamond makedb –in nr.faa -d nr`

This will create a binary DIAMOND database file with the specified name
(`nr.dmnd`). The alignment task may then be initiated using the `blastx`
command like this:

> `diamond blastx -d nr -q reads.fna -o matches.m8`

The output file here is specified with the `-o` option and named
`matches.m8`. By default, it is generated in BLAST tabular format.

Note:

  - The program may use quite a lot of memory and also temporary disk
    space. Should the program fail due to running out of either one, you
    need to set a lower value for the block size parameter `-b` (see
    [3.7](#subsec:memory)).

  - Use the options `–sensitive` or `–more-sensitive` for searches with
    higher sensitivity.

  - The runtime of the program is not linear in the size of the query
    file and it is more efficient for large query files (\(> 1\) million
    sequences) than for smaller ones.

  - Low complexity masking is applied to the query and reference
    sequences by default. Masked residues appear in the output as X.

  - The default e-value cutoff of DIAMOND is 0.001 while that of BLAST
    is 10, so by default the program will search a lot more stringently
    than BLAST and not report weak hits.
