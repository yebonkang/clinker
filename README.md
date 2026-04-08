# Clinker v2
Modified scripts from https://github.com/gamcil/clinker
Original scripts are saved in a `main` branch, whereas the modified ver. is saved in a `V2` branch. 

## Install
```bash
git clone https://github.com/yebonkang/clinker/tree/V2/clinker
cd clinker
pip install .
```

## Basic usage 
Move to clinker directory first. 
Then: 
```bash
cd clinker
python -m clinker.main *.gbk -st [protein/nucleotide] -p test.html -o test.tsv
```

## Details
The original clinker uses **blastp** to align CDS from genbank. It doesn't support **blastn**
It works similarly as an original script, but it allows nucleotide alignment. You can enable this using `-st nucleotide`

I used blastn web default settings which is:
-match score: 2.0
-mismatch score: -3
-open gap score: -5
-extend gap score: -2 

One caveat here is that using nucleotide sequence would take much longer time, since it's X3 longer than protein sequence. 

### Examples
#### Protein sequence
<img width="981" height="365" alt="image" src="https://github.com/user-attachments/assets/e385e6b1-6064-4f46-b7d9-2649d1dfd147" />

#### Nucleotide
<img width="957" height="320" alt="image" src="https://github.com/user-attachments/assets/8aff128c-0627-4942-b7b6-1eec603b7108" />
It generally matches to each other, but nucleotide may show connections of distantly related CDS, since it uses different algorithm. For highly conserved CDS, using blastp option generally gives higher identity values. 
(**blastp** uses BLOSUM64 algorithm, which gives penalties based on different amino acide groups, whereas **blastn** simply uses match/mismatch) 

## Command options
```bash
usage: clinker [-h] [--version] [-r RANGES [RANGES ...]] [-gf GENE_FUNCTIONS]
               [-cm COLOUR_MAP] [-dso] [-asc] [-na] [-i IDENTITY]
               [-st {protein,nucleotide}] [-j JOBS] [-s SESSION]
               [-ji JSON_INDENT] [-f] [-o OUTPUT] [-p [PLOT]] [-dl DELIMITER]
               [-dc DECIMALS] [-hl] [-ha] [-mo MATRIX_OUT] [-ufo]
               [files [files ...]]

clinker: Automatic creation of publication-ready gene cluster comparison figures.

clinker generates gene cluster comparison figures from GenBank files. It performs pairwise local or global alignments between every sequence in every unique pair of clusters and generates interactive, to-scale comparison figures using the clustermap.js library.

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit

Input options:
  files                 Gene cluster GenBank files
  -r RANGES [RANGES ...], --ranges RANGES [RANGES ...]
                        Scaffold extraction ranges. If a range is specified,
                        only features within the range will be extracted from
                        the scaffold. Ranges should be formatted like:
                        scaffold:start-end (e.g. scaffold_1:15000-40000)
  -gf GENE_FUNCTIONS, --gene_functions GENE_FUNCTIONS
                        2-column CSV file containing gene functions, used to
                        build gene groups from same function instead of
                        sequence similarity (e.g. GENE_001,PKS-NRPS).
  -cm COLOUR_MAP, --colour_map COLOUR_MAP
                        2-column CSV file containing gene functions and
                        colours (e.g. GENE_001,#FF0000).
  -dso, --dont_set_origin
                        Don't fix features which cross the origin in circular
                        sequences (GenBank format only)
  -asc, --as_separate_clusters
                        Records will be parsed into separate clusters. Enable
                        this option when the GenBank file you downloaded from
                        NCBI contains multiple sequences.

Alignment options:
  -na, --no_align       Do not align clusters
  -i IDENTITY, --identity IDENTITY
                        Minimum alignment sequence identity [default: 0.3]
  -st {protein,nucleotide}, --sequence_type {protein,nucleotide}
                        Sequence type used for pairwise alignment [default:
                        protein]
  -j JOBS, --jobs JOBS  Number of alignments to run in parallel (0 to use the
                        number of CPUs) [default: 0]

Output options:
  -s SESSION, --session SESSION
                        Path to clinker session
  -ji JSON_INDENT, --json_indent JSON_INDENT
                        Number of spaces to indent JSON [default: none]
  -f, --force           Overwrite previous output file
  -o OUTPUT, --output OUTPUT
                        Save alignments to file
  -p [PLOT], --plot [PLOT]
                        Plot cluster alignments using clustermap.js. If a path
                        is given, clinker will generate a portable HTML file
                        at that path. Otherwise, the plot will be served
                        dynamically using Python's HTTP server.
  -dl DELIMITER, --delimiter DELIMITER
                        Character to delimit output by [default: human
                        readable]
  -dc DECIMALS, --decimals DECIMALS
                        Number of decimal places in output [default: 2]
  -hl, --hide_link_headers
                        Hide alignment column headers
  -ha, --hide_aln_headers
                        Hide alignment cluster name headers
  -mo MATRIX_OUT, --matrix_out MATRIX_OUT
                        Save cluster similarity matrix to file

Visualisation options:
  -ufo, --use_file_order
                        Display clusters in order of input files

Example usage
-------------
Align clusters, plot results and print scores to screen:
  $ clinker files/*.gbk

Only save gene-gene links when identity is over 50%:
  $ clinker files/*.gbk -i 0.5

Save an alignment session for later:
  $ clinker files/*.gbk -s session.json

Save alignments to file, in comma-delimited format, with 4 decimal places:
  $ clinker files/*.gbk -o alignments.csv -dl "," -dc 4

Generate visualisation:
  $ clinker files/*.gbk -p

Save visualisation as a static HTML document:
  $ clinker files/*.gbk -p plot.html
```

