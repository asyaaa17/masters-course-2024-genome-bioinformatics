# Drosophila melanogaster

## Description of the organism
*Drosophila melanogaster*, often called the fruit fly, is a pivotal model organism in biological research, renowned for its manageable care, rapid lifecycle, and straightforward genetics. Originating from the Drosophilidae family, it thrives in environments with rotting fruit and fermenting beverages, making it common in both natural and urban settings. Its fully sequenced genome, one of the first among multicellular organisms, has vastly advanced our knowledge of genetics, development, and neurobiology, cementing its status in genetic studies.

I chose this organism because it is a model organism in genetic studies, and the availability of genomic resources - well studied and annotated.

## Genome Data Sources

### NCBI
- **Request**: Drosophila melanogaster
- **Filters applied**: RefSeq annotation, Chromosome
- **Full file path**: https://www.ncbi.nlm.nih.gov/assembly/GCF_000001215.4/
- **File name**: GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna.gz

### Ensembl
- **Request**: Drosophila melanogaster
- **Full file path**: https://ftp.ensembl.org/pub/release-104/fasta/drosophila_melanogaster/dna/
- **File name**: Drosophila_melanogaster.BDGP6.46.dna.toplevel.fa.gz

### UCSC Genome Browser
- **Request**: Drosophila melanogaster dm6
- **Full file path**: http://genome.ucsc.edu/cgi-bin/hgTracks?db=dm6
- **Genome found**: dm6.fa.gz

## Comparative Analysis

| Parameter                | NCBI (GCF_000001215.4)                          | Ensembl (BDGP6.46)                                      | UCSC (dm6)                    |
|--------------------------|-------------------------------------------------|---------------------------------------------------------|-------------------------------|
| File Size                | 139M                                            | 140M                                                    | 140M                          |
| Number of Lines          | 1,799,366                                       | 2,398,219                                               | 2,877,329                     |
| Number of Headers        | 1870                                            | 1870                                                    | 1870                          |
| Examples of Headers      | >NC_004354.4 Drosophila melanogaster chromosome X | >2L dna:primary_assembly primary_assembly:BDGP6.46:2L:1:23513712:1 REF | >chr2L      |

### Comparative Analysis

The comparison across the NCBI, Ensembl, and UCSC databases reveals several key insights into the *Drosophila melanogaster* genome assemblies they contain. Here's an analysis of the findings:

**File Size** The file sizes are remarkably similar across all three databases, suggesting that each contains complete genome assemblies of *Drosophila melanogaster*. The minimal differences in file size might be attributed to the varying annotation methods employed by each database and the inclusion of additional non-coding sequences in some assemblies.

**Number of Lines** The UCSC database's file exhibits the highest number of lines among the three, which could indicate the presence of more small fragments or a more detailed level of annotation. This suggests that the UCSC version of the genome might provide a more granular view of the *Drosophila melanogaster* genome, potentially offering richer insights for certain types of genomic analyses.

**Number of Headers** Across all three databases, the number of headers remains consistent, pointing to a uniform representation of chromosomes and major genomic elements. This consistency is crucial for researchers relying on comprehensive and comparable genomic data across different sources.

**Header Format** A notable difference between the databases is the format of the headers in the genome files:
  - **NCBI**: Headers include direct identifiers and names of chromosomes, offering straightforward reference points for genomic locations.
  - **Ensembl**: Headers are more detailed, encompassing DNA type and assembly information, which could be particularly useful for studies requiring in-depth genomic context.
  - **UCSC**: Uses a simplified notation with short chromosome designations, potentially making it easier for quick reference or for researchers familiar with chromosome shorthand notations.

#### Implications for Research
- The choice of database may significantly influence the ease of use and the type of analysis possible, given the differences in header formatting and the level of detail in annotations. Researchers might prefer one database over another based on their specific needs, whether they require detailed annotations (favoring Ensembl), straightforward chromosome identifiers (favoring NCBI), or a balance of simplicity and detail (potentially found in UCSC).
- The presence of more detailed annotations or additional non-coding sequences in some databases might enrich certain types of genetic analyses, such as those focusing on regulatory elements or comparative genomics.

In summary, while the core genomic data for *Drosophila melanogaster* remains consistent across NCBI, Ensembl, and UCSC, the nuances in file size, annotation detail, and header formatting underscore the importance of choosing the right database to match the specific needs of a study.


## QUAST Analysis

### Installation and Running QUAST

```bash
pip install quast
wget -O drosophila_melanogaster_assembly.zip "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000001215.4/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT"
unzip drosophila_melanogaster_assembly.zip
quast.py -o quast_report ncbi_dataset/data/GCF_000001215.4/GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna
```

### To analyze large eukaryotic genomes with QUAST

It is recommended to use the `--large` option. This option is necessary to improve performance and adapt to genome complexity. The `--large` option reduces analysis time and memory consumption by optimizing sequence comparison and computation algorithms. The `--large` option adapts the analysis process to better account for eukaryotic genome features.

By default, QUAST excludes very small containers from analysis. The `--large` option allows to adapt this threshold, which can be useful to prevent results from being skewed by a large number of small fragments. And also the calculation of assembly quality metrics is adapted to account for the specificity of large genomes.

**Options and parameters to avoid for large genomes:**
- Detailed analysis of missassemblies (`--misassemblies`)
- Using all available CPU threads (`-t` or `--threads`)
- Redundant visualizations

**Summary of Key Metrics:**
- **N50 (25286936 bp)**: This metric indicates that 50% of the entire genome assembly is contained in contigs that are at least 25,286,936 bp long. It is a measure of assembly contiguity, with higher values suggesting a more contiguous assembly.
- **L50 (3)**: It provides an idea of how many contigs contribute significantly to the genome's total length.
- **Total Length (143,726,002 bp)**: This is the sum of the lengths of all contigs in the assembly, providing the assembly's total genomic content.
- **Largest Contig (32,079,331 bp)**: The size of the largest contig in the assembly. Larger contigs generally indicate a higher-quality assembly as they suggest fewer breaks in the genomic sequence.
- **GC Content (42.01%)**: This indicates the percentage of guanine and cytosine bases in the genome.
- **N90 (23,513,712 bp)**: Indicates that 90% of the total genome length is contained in contigs at least this long.
- **Number of N's per 100 kbp (802.21)**: This indicates the frequency of undetermined bases (N) in the assembly. A lower number suggests a more complete and less gapped assembly.

The provided metrics suggest a high-quality genome assembly for *Drosophila melanogaster*. The N50 and N90 values are particularly high, indicating that the assembly consists of long, continuous stretches of sequence, which is desirable for comprehensive genomic analyses. The total length of the assembly is consistent with the expected size of the *Drosophila melanogaster* genome, suggesting completeness. The relatively low number of N's per 100 kbp indicates that there are few gaps in the assembly.

The high N50 values and low number of N's per 100 kbp indicate a high degree of assembly integrity and continuity, providing a more accurate and complete understanding of genomic structure and function. This in turn contributes to improved gene annotation, variant identification and comparative genomic studies, improving the overall reliability and accuracy of biological inferences.

