# Data package for Rat liver RNA-seq data from SEQC Toxicogenomics project

## Sources

  * Experimental data were generated in SEQC Toxicogenomics project. Original citation: Gong B, Wang C, Su Z, Hong H et al. Transcriptomic profiling of rat liver samples in a comprehensive study design by RNA-Seq. _Sci Data_ 2014;1:140021. PMID: [25977778](https://www.ncbi.nlm.nih.gov/pubmed/25977778)
  * Processing:
    * Sequencing reads were downloaded from SRA, at [PRJNA239561](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA239561)
    * Quantification was done by Kallisto 0.45.0, using a kallisto index built from Ensembl Rat genome Rnor_6.0.99 and 92 ERCC sequences
  
## Usage

Install the package, import the library and load the data set

```R
devtools::install_github('ttdtrang/data-rnaseq-Rnor')
library(data.rnaseq.Rnor)
data(rnor.rnaseq.gene)
dim(rnor.rnaseq.gene@assayData$exprs)
```

The package includes 2 data sets, one for transcript-level counts/TPM and another for gene-level counts/TPM. Transcript-level counts are direct output from Kallisto while gene-level counts are the total counts of all transcripts belonging to the same gene.

## Steps to re-produce data curation

1. `cd data-raw`
2. Download all necessary raw data files.
3. Set the environment variable `DBDIR` to point to the path containing said files
4. Run the R notebook `make-data-package.Rmd` to assemble parts into `ExpressionSet` objects.

You may need to change some code chunk setting from `eval=FALSE` to `eval=TRUE` to make sure all chunks would be run. These chunks are disabled by default to avoid overwriting existing data files in the folder.
