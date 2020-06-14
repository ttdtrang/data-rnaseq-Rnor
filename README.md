# Data package for Rat liver RNA-seq data from SEQC Toxicogenomics project

## Sources

  * Experimental data were generated in SEQC Toxicogenomics project. Original citation: Gong B, Wang C, Su Z, Hong H et al. Transcriptomic profiling of rat liver samples in a comprehensive study design by RNA-Seq. _Sci Data_ 2014;1:140021. PMID: [25977778](https://www.ncbi.nlm.nih.gov/pubmed/25977778)
  * Processing:
    * Sequencing reads were downloaded from SRA, at [PRJNA239561](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA239561)
    * Quantification was done by 2 alternative workflows:
      1. Using Kallisto 0.45.0 with an index built from Ensembl Rat genome (cdna) Rnor_6.0.99 and 92 ERCC sequences
      2. Using STAR 2.7.1a to align against Ensembl Rat genome Rnor_6.0.99 and 92 ERCC sequences, and RSEM to estimate abundance levels for genes/isoforms.
  
## Usage

Install the package, import the library and load the data set

```R
devtools::install_github('ttdtrang/data-rnaseq-Rnor')
library(data.rnaseq.Rnor)
data(rnor.rnaseq.gene.kallisto)
dim(rnor.rnaseq.gene@assayData$exprs)
```

The package includes 4 data sets.
```
rnor.rnaseq.gene.kallisto
rnor.rnaseq.transcript.kallisto
rnor.rnaseq.gene.star_rsem
rnor.rnaseq.transcript.star_rsem
```

For Kallisto workflow, transcript-level counts are direct output from Kallisto while gene-level counts are the total counts of all transcripts belonging to the same gene.

For STAR-RSEM workflow, transcript-level and gene-level counts are collected from RSEM output `rsem.genes.results` and `rsem.isoforms.results`, respectively.

## Steps to re-produce data curation

1. `cd data-raw`
2. Download all necessary raw data files.
3. Set the environment variable `DBDIR` to point to the path containing said files. It is assumed that files are organized into directories corresponding to workflow, e.g.
```bash
├── kallisto
│   ├── feature_attributes.tsv
│   ├── matrix.est_counts.RDS
│   ├── matrix.gene.est_counts.RDS
│   ├── matrix.gene.tpm.RDS
│   └── matrix.tpm.RDS
├── PRJNA239561_metadata_cleaned.tsv
├── star-rsem
│   ├── feature_attrs.rsem.transcripts.tsv
│   ├── matrix.gene.expected_count.RDS
│   ├── matrix.gene.tpm.RDS
│   ├── matrix.transcripts.expected_count.RDS
│   ├── matrix.transcripts.tpm.RDS
│   └── starLog.final.tsv
└── subread
    ├── feature_attrs.featureCounts.genes.tsv
    ├── featureCounts-summary.genes.tsv
    ├── featureCounts-summary.transcripts.tsv
    └── matrix.gene.featureCounts.RDS
```
4. Run the R notebook `make-data-package.Rmd` to assemble parts into `ExpressionSet` objects.

You may need to change some code chunk setting from `eval=FALSE` to `eval=TRUE` to make sure all chunks would be run. These chunks are disabled by default to avoid overwriting existing data files in the folder.
