## Drosophila melanogaster

### Task 1: Searching and Downloading Genomes
### Description of the organism
*Drosophila melanogaster*, often called the fruit fly, is a pivotal model organism in biological research, renowned for its manageable care, rapid lifecycle, and straightforward genetics. Originating from the Drosophilidae family, it thrives in environments with rotting fruit and fermenting beverages, making it common in both natural and urban settings. Its fully sequenced genome, one of the first among multicellular organisms, has vastly advanced our knowledge of genetics, development, and neurobiology, cementing its status in genetic studies.

I chose this organism because it is a model organism in genetic studies, and the availability of genomic resources - well studied and annotated.

### Genome Data Sources

#### NCBI
- **Request**: Drosophila melanogaster
- **Filters applied**: RefSeq annotation, Chromosome
- **Full file path**: https://www.ncbi.nlm.nih.gov/assembly/GCF_000001215.4/
- **File name**: GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna.gz

#### Ensembl
- **Request**: Drosophila melanogaster
- **Full file path**: https://ftp.ensembl.org/pub/release-104/fasta/drosophila_melanogaster/dna/
- **File name**: Drosophila_melanogaster.BDGP6.46.dna.toplevel.fa.gz

#### UCSC Genome Browser
- **Request**: Drosophila melanogaster dm6
- **Full file path**: http://genome.ucsc.edu/cgi-bin/hgTracks?db=dm6
- **Genome found**: dm6.fa.gz

### Comparison

| Parameter                | NCBI (GCF_000001215.4)                          | Ensembl (BDGP6.46)                                      | UCSC (dm6)                    |
|--------------------------|-------------------------------------------------|---------------------------------------------------------|-------------------------------|
| File Size                | 139M                                            | 140M                                                    | 140M                          |
| Number of Lines          | 1,799,366                                       | 2,398,219                                               | 2,877,329                     |
| Number of Headers        | 1870                                            | 1870                                                    | 1870                          |
| Examples of Headers      | >NC_004354.4 Drosophila melanogaster chromosome X | >2L dna:primary_assembly primary_assembly:BDGP6.46:2L:1:23513712:1 REF | >chr2L      |

### Analysis:
**File Size:** The file sizes are similar across all three databases, suggesting that each contains complete genome assemblies of *Drosophila melanogaster*. The minimal differences in file size might be attributed to the varying annotation methods employed by each database and the inclusion of additional non-coding sequences in some assemblies.

**Number of Lines:** The UCSC database's file exhibits the highest number of lines among the three, which could indicate the presence of more small fragments or a more detailed level of annotation. This suggests that the UCSC version of the genome might provide a more granular view of the *Drosophila melanogaster* genome.

**Number of Headers:** Across all three databases, the number of headers remains consistent, pointing to a uniform representation of chromosomes and major genomic elements.

**Header Format:** A notable difference between the databases is the format of the headers in the genome files:
  - **NCBI**: Headers include direct identifiers and names of chromosomes, offering straightforward reference points for genomic locations.
  - **Ensembl**: Headers are more detailed, encompassing DNA type and assembly information, which could be particularly useful for studies requiring in-depth genomic context.
  - **UCSC**: Uses a simplified notation with short chromosome designations, potentially making it easier for quick reference or for researchers familiar with chromosome shorthand notations.

### Implications for Research
- The choice of database influence the ease of use and the type of analysis possible, given the differences in header formatting and the level of detail in annotations. Might prefer one database over another based on their specific needs, whether they require detailed annotations (favoring Ensembl), straightforward chromosome identifiers (favoring NCBI), or a balance of simplicity and detail (potentially found in UCSC).
- The presence of more detailed annotations or additional non-coding sequences in some databases might enrich certain types of genetic analyses, such as those focusing on regulatory elements or comparative genomics.


### Task 2: Running QUAST

### Installation and Running QUAST

```bash
# Install QUAST
pip install quast
# Download the Drosophila melanogaster genome assembly
wget -O drosophila_melanogaster_assembly.zip "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000001215.4/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT"
# Unzip the downloaded file
unzip drosophila_melanogaster_assembly.zip
# Run QUAST on the genome assembly
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
- **N50 (25286936 bp)**: This metric indicates that 50% of the entire genome assembly is contained in contigs that are at least 25,286,936 bp long. It is a measure of assembly contiguity.
- **L50 (3)**: It provides an idea of how many contigs contribute significantly to the genome's total length.
- **Total Length (143,726,002 bp)**: This is the sum of the lengths of all contigs in the assembly, providing the assembly's total genomic content.
- **Largest Contig (32,079,331 bp)**: The size of the largest contig in the assembly. Larger contigs generally indicate a higher-quality assembly as they suggest fewer breaks in the genomic sequence.
- **GC Content (42.01%)**: This indicates the percentage of guanine and cytosine bases in the genome.
- **N90 (23,513,712 bp)**: Indicates that 90% of the total genome length is contained in contigs at least this long.
- **Number of N's per 100 kbp (802.21)**: This indicates the frequency of undetermined bases (N) in the assembly. A lower number suggests a more complete and less gapped assembly.

The provided metrics suggest a high-quality genome assembly for *Drosophila melanogaster*. The N50 and N90 values are particularly high, indicating that the assembly consists of long, continuous stretches of sequence, which is desirable for comprehensive genomic analyses. The total length of the assembly is consistent with the expected size of the *Drosophila melanogaster* genome, suggesting completeness. The relatively low number of N's per 100 kbp indicates that there are few gaps in the assembly.

The high N50 values and low number of N's per 100 kbp indicate a high degree of assembly integrity and continuity, providing a more accurate and complete understanding of genomic structure and function. This in turn contributes to improved gene annotation, variant identification and comparative genomic studies.

### Task 3: Running BUSCO
#### Conda Installation
BUSCO was installed using the Conda package manager which simplifies software installation and manages dependencies effectively. The following commands were used to create and activate a separate Conda environment and install BUSCO:

```bash
# Create a new Conda environment for BUSCO
mamba create -n busco_env -c bioconda -c conda-forge busco=5.3.2 -y

# Activate the BUSCO environment
conda activate busco_env
```

#### Running BUSCO
```bash
# Run BUSCO analysis on the genome assembly
conda run -n busco_env busco -i GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna -o drosophila_analysis -l diptera_odb10 -m genome
```
**Choosing the Appropriate Lineage:** 
The lineage dataset for *Drosophila melanogaster* was selected as diptera_odb10 from the available datasets on BUSCO's dataset page [https://busco-data.ezlab.org/v5/data/lineages/]. This dataset was chosen because it is specifically curated for the order *Diptera*, to which *Drosophila melanogaster* belongs. Selecting the right set provides the most accurate and representative comparison because it looks for orthologs that are specific to taxon.

**Advanced Options:** 
The --offline option was utilized to run BUSCO using the local copy of the lineage dataset without attempting to download it. 

#### Summary of Key Metrics

The BUSCO analysis provides several key metrics to evaluate the quality and completeness of the genome assembly or annotation:

- **Complete BUSCOs (C)**: 3245 / 3285 (98.8%). This metric represents the proportion of the universal single-copy orthologs that are fully present in the analyzed genome. A high percentage of Complete BUSCOs indicates a high-quality assembly or annotation.

- **Complete and single-copy BUSCOs (S)**: 3236. These are the BUSCOs found in the genome in exactly one copy. This is ideal for single-copy orthologs, indicating accurate assembly and annotation without duplications that might suggest assembly errors or misannotations.

- **Complete and duplicated BUSCOs (D)**: 9. These BUSCOs are found in more than one copy. A small number of duplicated BUSCOs might reflect true biological duplications in the genome. 

- **Fragmented BUSCOs (F)**: 14 (0.4%). Fragmented BUSCOs are those for which only a part of the gene has been found. This partial presence might be due to genuine fragmentation in the genome, or it could reflect issues in the assembly or annotation processes, such as incomplete or misassembled regions.

- **Missing BUSCOs (M)**: 26 (0.8%). Missing BUSCOs are the conserved orthologs not found in the genome assembly or annotation. While a small number of missing BUSCOs is expected due to natural genetic variation or true gene loss, a high number of missing BUSCOs could indicate gaps or significant issues in the genome assembly or annotation.

#### Analysis

- The high percentage of Complete BUSCOs (98.8%) demonstrates a well-assembled and annotated genome.
- The minimal fraction of Fragmented (0.4%) and Missing BUSCOs (0.8%) suggests few gaps or errors.
- The presence of a small number of duplicated BUSCOs (0.3%) could indicate regions of duplication within the genome or potential assembly artifacts.

#### Contextualizing Results

The BUSCO completeness scores provide a robust framework for evaluating the *Drosophila melanogaster* genome's readiness for further analysis. High completeness suggests a reliable foundation for functional genomics, evolutionary studies, and comparative genomics. Evolutionary expectations guide us in predicting gene content, where a genome rich in conserved orthologs, as demonstrated, supports its evolutionary lineage's integrity. 
