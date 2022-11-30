# ShortTracks
Useful length- and strand-based coverage files (bigwig) from small RNA-seq alignments (BAM)

# Author
Michael J. Axtell, The Pennsylvania State University, mja18@psu.edu

# Synopsis
Flexible conversion of BAM-formatted alignment into one or more bigwig coverage files, based on query lengths or readgroups and strandedness. Especially useful when viewed with the JBrowse2 <https://jbrowse.org/jb2/> multi-wiggle track feature.

# Install
## Install with conda
*operational soon*
Install `conda` with the bioconda <https://bioconda.github.io> channel properly configured, then:

## Linux, Intel-based Macs
```
conda install shorttracks
```

## Silicon-based Macs

Not all dependencies are available for Silicon-based Macs at this time, so the following work around is required
```
conda create --name shorttracks
conda activate shorttracks
conda config --env --set subdir osx-64
conda install shortstracks
```

## Manual install
First install dependencies / create an environment with these in the path
- `python` (>=3.10)
- `samtools` (>= 1.10) <https://www.htslib.org>
- `wigToBigWig` <https://genome.ucsc.edu/goldenPath/help/bigWig.html>

Then, copy the `ShortTracks` script somewhere to your $PATH or environment.

# Usage
`ShortTracks [-h] [--version] [--mode {simple,readgroup,readlength}] [--stranded] --bamfile BAMFILE`

## Options

- `-h` : Display help message then quit
- `--version` : Display version information then quit
- `--mode` : Set the mode. Either `simple`, `readgroup`, or `readlength`. Defaults to `simple`
- `--stranded` : If set, create a plus and minus-strand track for each trim.
- `--bamfile` (required) : Path to the bamfile being processed

## Modes
### simple
The simple mode (the default) processes the entire bam file (i.e. no splitting by read group or by read length.
### readgroup
If separate files for each readgroup are produced. The denominator for the read-per-million calculation is based on the total reads in each read group (so the resulting values are comparable between readgroups).
### readlength
Four separate files, based on read lengths, are produced: 21, 22, 23-24, and all others. These size separations mirror functionally distinct types of small RNAs from plants.

# How it works
1. Raw depths by chromosomal position are first calculated using `samtools` and written to disk.
2. Depths are converted to reads per million and then re-formatted into wiggle files.
3. The wiggle files are converted to bigwig files.


