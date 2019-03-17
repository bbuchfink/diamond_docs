# Optimizing performance

You should take the time to configure the program to your needs instead
of just running it with default settings, as this will substantially
optimize the use of system resources. Some points to consider:

  - Set the `-e` parameter (maximum expected value) as low as possible.

  - Set the `-k` parameter (number of target sequences to report
    alignments for) as low as possible. This will improve performance
    and reduce the use of temporary disk space and the size of the
    output files.

  - Consider using the `--top` parameter instead of `-k`. For example,
    setting `--top 5` will report all alignments whose score is at most
    5% lower than the top alignment score for a query.

  - Configure the tabular output format to only report the output fields
    that are actually required (see [output options](index.php?pages/command_line_options/#output-options)).

  - Use the BLAST tabular format for machine-based processing of the
    output. Use the XML and SAM format only if required by your
    downstream tools or if you prefer it as a human-readable format.

  - Use the option `--compress 1` to automatically generate
    gzip-compressed output files.
