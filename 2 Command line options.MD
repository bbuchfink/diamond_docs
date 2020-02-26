# Command line options

## Commands

Commands are issued as the first parameter on the command line and set
the task to be run by the program.

  - `makedb`  
      
    Create a DIAMOND formatted reference database from a FASTA input
    file.

  - `blastp`  
      
    Align protein query sequences against a protein reference database.

  - `blastx`  
      
    Align translated DNA query sequences against a protein reference
    database.

  - `view`  
      
    Generate formatted output from DAA files.

  - `version`  
      
    Print version information.

  - `dbinfo`  
      
    Print information about a database file.

  - `help`  
      
    Print help message.

## Makedb options

  - `--in <file>`  
      
    Path to the input protein reference database file in FASTA format
    (may be gzip compressed). If this parameter is omitted, the input
    will be read from `stdin`.

  - `--db/-d <file>`  
      
    Path to the output DIAMOND database file.

  - `--taxonmap <file>`  
      
    Path to mapping file that maps NCBI protein accession numbers to
    taxon ids (gzip compressed). This parameter is optional and needs to
    be supplied in order to provide taxonomy features. The file can be
    downloaded from NCBI: <ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/accession2taxid/prot.accession2taxid.gz>.

  - `--taxonnodes <file>`  
      
    Path to the `nodes.dmp` file from the NCBI taxonomy. This parameter
    is optional and needs to be supplied in order to provide taxonomy
    features. The file is contained within this archive downloadable at
    NCBI: <ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/taxdmp.zip>.
    
  - `--taxonnames <file>`
    
    Path to the `names.dmp` file from the NCBI taxonomy. This parameter
    is optional and needs to be supplied in order to provide taxonomy
    features. The file is contained within this archive downloadable at
    NCBI: <ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/taxdmp.zip>.

## General options

  - `--threads/-p #`  
      
    Number of CPU threads. By default, the program will auto-detect and
    use all available virtual cores on the machine.

## Input options

  - `--db/-d <file>`  
      
    Path to the DIAMOND database file.

  - `--query/-q <file>`  
      
    Path to the query input file in FASTA or FASTQ format (may be gzip
    compressed). If this parameter is omitted, the input will be read
    from `stdin`.

  - `--taxonlist <list>`  
      
    Comma-separated list of NCBI taxonomic IDs to filter the database
    by. Any taxonomic rank can be used, and only reference sequences
    matching one of the specified taxon ids will be searched against.
    Using this option requires setting the `--taxonmap` and `--taxonnodes`
    parameters for `makedb`.

  - `--query-gencode #`  
      
    Genetic code used for translation of query in BLASTX mode. A list of
    possible values can be found at the [NCBI
    website](https://www.ncbi.nlm.nih.gov/Taxonomy/Utils/wprintgc.cgi).
    By default, the Standard Code is used. Note: changing the genetic
    code is currently not fully supported for the DAA format.

  - `--strand {both, plus, minus}`  
      
    Set strand of query to align for translated searches. By default
    both strands are searched.

  - `--min-orf/-l #`  
      
    Ignore translated sequences that do not contain an open reading
    frame of at least this length. By default this feature is disabled
    for sequences of length below 30, set to 20 for sequences of length
    below 100, and set to 40 otherwise. Setting this option to `1` will
    disable this feature.

## Alignment options

  - `--sensitive`
  
    This mode provides a lot higher sensitivity than the default (fast) mode.
    
  - `--more-sensitive`  
      
    This mode provides some additional sensitivity compared to the
    sensitive mode.

  - `--frameshift/-F #`  
      
    Penalty for frameshifts in DNA-vs-protein alignments. Values around
    15 are reasonable for this parameter. Enabling this feature will
    have the aligner tolerate missing bases in DNA sequences and is most
    recommended for long, error-prone sequences like MinION reads.  
    In the pairwise output format, frameshifts will be indicated by `\`
    and `/` for a shift by +1 and -1 nucleotide in the direction of
    translation respectively.  
    Note that this feature is disabled by default.

  - `--gapopen #`  
      
    Gap open penalty.

  - `--gapextend #`  
      
    Gap extension penalty.

  - `--matrix <matrix name>`  
      
    Scoring matrix. The following matrices are supported, with the
    default being
    BLOSUM62.  
      
    
    | Matrix   | Supported values for (gap open)/(gap extend) | Default gap penalties |
    | :------- | :------------------------------------------- | :-------------------- |
    | BLOSUM45 | (10-13)/3; (12-16)/2; (16-19)/1              | 14/2                  |
    | BLOSUM50 | (9-13)/3; (12-16)/2; (15-19)/1               | 13/2                  |
    | BLOSUM62 | (6-11)/2; (9-13)/1                           | 11/1                  |
    | BLOSUM80 | (6-9)/2; 13/2; 25/2; (9-11)/1                | 10/1                  |
    | BLOSUM90 | (6-9)/2; (9-11)/1                            | 10/1                  |
    | PAM250   | (11-15)/3; (13-17)/2; (17-21)/1              | 14/2                  |
    | PAM70    | (6-8)/2; (9-11)/1                            | 10/1                  |
    | PAM30    | (5-7)/2; (8-10)/1                            | 9/1                   |
    

  - `--comp-based-stats (0,1)`  
      
    Compositional bias correction of alignment scores. 0 means no score
    correction, 1 means compositional bias correction as described in 
    [1]. Compositionally biased sequences often cause false
    positive matches, which are effectively filtered by this algorithm
    in a way similar to the composition based statistics used by BLAST .

  - `--algo (0,1)`  
      
    Algorithm for seed search. 0 means double-indexed and 1 means
    query-indexed. The double-indexed algorithm is the program’s main
    algorithm, but it is inefficient for very small query files, where
    the query-indexed algorithm should be used instead.  
    By default, the program will automatically choose one of the
    algorithms based on the size of the query and database files. The
    algorithm used will be displayed at program startup.  
    Note that while the two algorithms are configured to provide roughly
    the same sensitivity for the respective modes, results will not be
    exactly identical to each other.

## Output options

  - `--out/-o <file>`  
      
    Path to the output file. If this parameter is omitted, the results
    will be written to the standard output and all other program output
    will be suppressed.

  - `--outfmt/-f #`  
      
    Format of the output file. The following values are accepted:
    
      - `0`  
        BLAST pairwise format.
    
      - `5`  
        BLAST XML format.
    
      - `6`  
        BLAST tabular format (default). This format can be customized,
        the `6` may be followed by a space-separated list of the
        following keywords, each specifying a field of the output.
        
          - `qseqid`
            Query Seq - id
        
          - `qlen`
            Query sequence length
        
          - `sseqid`
            Subject Seq - id
        
          - `sallseqid`
            All subject Seq - id(s), separated by a ’;’
        
          - `slen`
            Subject sequence length
        
          - `qstart`
            Start of alignment in query
        
          - `qend`
            End of alignment in query
        
          - `sstart`
            Start of alignment in subject
        
          - `send`
            End of alignment in subject
        
          - `qseq`
            Aligned part of query sequence
            
          - `full_qseq`
            Full query sequence
        
          - `sseq`
            Aligned part of subject sequence
        
          - `full_sseq`
            Full subject sequence
        
          - `evalue`
            Expect value
        
          - `bitscore`
            Bit score
        
          - `score`
            Raw score
        
          - `length`
            Alignment length
        
          - `pident`
            Percentage of identical matches
        
          - `nident`
            Number of identical matches
        
          - `mismatch`
            Number of mismatches
        
          - `positive`
            Number of positive - scoring matches
        
          - `gapopen`
            Number of gap openings
        
          - `gaps`
            Total number of gaps
        
          - `ppos`
            Percentage of positive - scoring matches
        
          - `qframe`
            Query frame
        
          - `btop`
            Blast traceback operations(BTOP)
            
          - `cigar`
            CIGAR string
        
          - `staxids`
            Unique Subject Taxonomy ID(s), separated by a ’;’ (in
            numerical order). This field requires setting the
            `--taxonmap` parameter for `makedb`.
            
          - `sscinames`
            Unique Subject Scientific Name(s), separated by a ';'. This
            field requires setting the `--taxonmap` and `--taxonnames`
            parameters for `makedb`.
          
          - `sskingdoms`
            Unique Subject Super Kingdom(s), separated by a ';'. This
            field requires setting the `--taxonmap`, `--taxonnodes`
            and `--taxonnames` parameters for `makedb`.
            
          - `skingdoms`
            Unique Subject Kingdom(s), separated by a ';'. This
            field requires setting the `--taxonmap`, `--taxonnodes`
            and `--taxonnames` parameters for `makedb`.
            
          - `sphylums`
            Unique Subject Phylums(s), separated by a ';'. This
            field requires setting the `--taxonmap`, `--taxonnodes`
            and `--taxonnames` parameters for `makedb`.
        
          - `stitle`
            Subject Title
        
          - `salltitles`
            All Subject Title(s), separated by a ’\<\>’
        
          - `qcovhsp`
            Query Coverage Per HSP
            
          - `scovhsp`
            Subject Coverage Per HSP
        
          - `qtitle`
            Query title
            
          - `qqual`
            Query quality values for the aligned part of the query
            
          - `full_qqual`
            Query quality values
            
          - `qstrand`
            Query strand
        
        By default, there are 12 preconfigured fields: `qseqid sseqid
        pident length mismatch gapopen qstart qend sstart send evalue
        bitscore`.
    
      - `100`  
        DIAMOND alignment archive (DAA). The DAA format is a proprietary
        binary format that can subsequently be used to generate other
        output formats using the `view` command. It is also supported by
        MEGAN and allows a quick import of results.
    
      - `101`  
        SAM format.
    
      - `102`  
        Taxonomic classification. This format will not print alignments
        but only a taxonomic classification for each query using the LCA
        algorithm. The output lines consist of 3 tab-delimited fields:
        
        1.  Query ID
        
        2.  NCBI taxonomy ID (0 if unclassified)
        
        3.  E-value of the best alignment with a known taxonomic ID
            found for the query (0 if unclassified)
        
        The score range for the LCA algorithm is set by the `--top`
        parameter. The default value is 10 which means that all
        alignments whose score is at most 10% lower than the best score
        are considered for the LCA computation.
        
        Using this format requires setting the `--taxonmap` and
        `--taxonnodes` parameters for `makedb`.
    
      - `103`  
        [PAF format](https://github.com/lh3/miniasm/blob/master/PAF.md).
        The custom fields in the format are AS (bit score), ZR (raw
        score) and ZE (e-value).

  - `--salltitles`  
      
    Include full length subject titles into the DAA format. By default,
    DAA files contain only the shortened sequence id (up to the first
    blank character).

  - `--sallseqid`  
      
    Include all subject ids into the DAA file. By default only the first
    id of each subject is included. As the subject ids are much shorter
    than the full titles this option will save space compared to the
    `--salltitles` option.

  - `--compress (0,1)`  
      
    Enable compression of the output file. 0 (default) means no
    compression, 1 means gzip compression.

  - `--max-target-seqs/-k #`  
      
    The maximum number of target sequences per query to report
    alignments for (default=25). Setting this to 0 will report all
    alignments that were found.

  - `--top #`  
      
    Report alignments within the given percentage range of the top
    alignment score for a query (overrides `--max-target-seqs` option).
    For example, setting this to 10 will report all alignments whose
    score is at most 10% lower than the best alignment score for a
    query.

  - `--range-culling`  
      
    Restrict hit culling to overlapping query ranges. This feature is
    designed for long query DNA sequences that may span several genes.
    In these cases, the default of reporting the 25 best overall hits
    could cause hits to a lower scoring gene to be overshadowed. But
    just increasing the number of alignments reported will bloat the
    output size and reduce performance.  
    Using this feature along with `-k 25` (default), a hit will only be
    deleted if at least 50% of its query range is spanned by at least 25
    higher or equal scoring hits.  
    Using this feature along with `--top 10`, a hit will only be deleted
    if its score is more than 10% lower than that of a higher scoring
    hit over at least 50% of its query range.  
    The percentage is configurable using `--range-cover`. Note that this
    feature is currently only available in frameshift alignment mode.

  - `--evalue/-e #`  
      
    Maximum expected value to report an alignment (default=0.001).

  - `--min-score #`  
      
    Minimum bit score to report an alignment. Setting this option will
    override the `--evalue` parameter.

  - `--id #`  
      
    Report only alignments above the given percentage of sequence
    identity.

  - `--query-cover #`  
      
    Report only alignments above the given percentage of query cover.

  - `--subject-cover #`  
      
    Report only alignments above the given percentage of subject cover.

  - `--max-hsps #`  
      
    The maximum number of HSPs per subject sequence to report for each
    query. Note that in general any number of HSPs may exist for a
    single query/subject pair. The program’s default policy is to report
    any HSP if its query and subject ranges are not enveloped by a
    higher scoring HSP and if it meets the e-value treshold.  
    This option may be used to limit the number of HSPs reported.
    Setting this option to 1 will cause only the single best HSP for
    each query/subject pair to be reported.

  - `--unal (0,1)`  
      
    Report unaligned queries (0=no, 1=yes). By default, unaligned
    queries are reported for the BLAST pairwise, BLAST XML and SAM
    format.

  - `--no-self-hits`  
      
    Suppress reporting of identical self-hits between sequences.

## Memory & performance options

  - `--block-size/-b #`  
      
    Block size in billions of sequence letters to be processed at a
    time. This is the main parameter for controlling the program’s
    memory usage. Bigger numbers will increase the use of memory and
    temporary disk space, but also improve performance. The program can
    be expected to use roughly six times this number of memory (in GB).
    So for the default value of `-b2.0`, the memory usage will be about
    12 GB.

  - `--tmpdir/-t <directory>`  
      
    Directory to be used for temporary storage. This is set to the
    output directory by default. The amount of disk space that will be
    used depends on the program’s settings and your data. As a general
    rule you should ensure that 100 GB of disk space are available here.
    If you run the program in a cluster environment, and disk space is
    only available over a slow network based file system, you may want
    to set the `--tmpdir` option to `/dev/shm`. This will keep temporary
    information in memory and thus increase the program’s memory usage
    substantially.

  - `--index-chunks/-c #`  
      
    The number of chunks for processing the seed index (default=4). This
    option can be additionally used to tune the performance. It is
    recommended to set this to 1 on a high memory server, which will
    increase performance and memory usage, but not the usage of
    temporary disk space.

## View options

  - `--daa/-a <file>`  
      
    Path to input file in DAA format.

  - `--out/-o <file>`  
      
    Path to output file. If this parameter is omitted, the results will
    be written to the standard output and all other program output will
    be suppressed.

These aligner parameters apply to the view command as well and work in
the same way: `--outfmt`, `--compress`, `--max-target-seqs`, `--top`. Note
that taxonomy features are currently not available for the DAA format.

## Advanced options

  - `--freq-sd #`  
      
    During the seed search, seeds with very high frequency in the
    queries or the database are ignored. This option sets the number of
    standard deviations above the average frequency for ignoring a seed.
    The default values are 50 in default mode, 10 in sensitive mode, and
    200 in more-sensitive mode.

  - `--band #`  
      
    Dynamic programming band for seed extension. Initial seed hits are
    scanned across a band of this length in both directions.

  - `--xml-blord-format`  
      
    Use gnlBL\_ORD\_ID style format for hit IDs in XML output.

  - `--range-cover #`  
      
    The percentage of a hit’s query range that needs to be spanned by
    higher scoring hits for a hit to be deleted in range culling mode
    (default=50.0).
    
---

[1] Maria Hauser, Martin Steinegger, Johannes Söding; MMseqs software suite
for fast and deep clustering and searching of large protein sequence sets,
Bioinformatics, Volume 32, Issue 9, 1 May 2016, Pages 1323–1330
