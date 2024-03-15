# Homework 2: Repeats Masking in Genomes

## Objective

This assignment focuses on providing practical experience in identifying and masking repetitive sequences in genomes using a variety of bioinformatics tools. The tasks include the installation and execution of WindowMasker, DUST, Tandem Repeats Finder (TRF), RepeatModeler, and RepeatMasker, comparing the fraction of the genome masked by each tool, analyzing the differences in types and amounts of repetitive sequences identified, and understanding the importance of repeat masking in genome analysis.

## Task 1: Installing Repeats Masking Tools

### Objective

To install WindowMasker, DUST, TRF, RepeatModeler, and RepeatMasker using Conda, aiming to familiarize with software installation and environment management.


#### Conda Environment Setup
1. **Create a new Conda environment named `genome_tools` with Python 3.8:**

   ```bash
   conda create -n genome_tools python=3.8
   ```
2. **Activate the newly created environment:**
   
   ```bash
   conda activate genome_tools
   ```
3. **Installing Tools BLAST (including WindowMasker and DUST):**

   ```bash
   mamba install -c bioconda blast
   ```
   - Version: 2.14.1
   ###### Note: The installation of BLAST includes WindowMasker and DUST, part of the NCBI toolkit.
4. **Tandem Repeats Finder (TRF):**
   
   ```bash
   mamba install -c bioconda trf
   ```
   - Version: 4.09.1
5. **RepeatModeler and RepeatMasker (including dependencies like rmblast):**

   ```bash
   mamba install -c bioconda repeatmodeler repeatmasker rmblast
   ```
   - RepeatModeler Version: 1.0.8
   - RepeatMasker Version: 4.1.2.p1
   - RMBlast Version: 2.14.1

## Task 2: Launching Repeat Masking Tools

Commands used for repeat masking:

#### WindowMasker:

```bash
windowmasker -in GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna -mk_counts -out genome.counts
windowmasker -in GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna -ustat genome.counts -out windowmasker_results.txt -outfmt fasta
```
#### DUST (via seqtk for simplicity):

```bash
seqtk seq -A GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna | dustmasker -infmt fasta | seqtk seq -a > dust_results.txt
```
#### Tandem Repeats Finder (TRF):

```bash
trf GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna 2 7 7 80 10 50 500 -m -f -d -h > trf_results
```
#### RepeatModeler and RepeatMasker (Creating a custom library and masking):

```bash
BuildDatabase -name dm_genome_db -engine ncbi GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna
RepeatModeler -database dm_genome_db -pa 4
RepeatMasker -lib RM_264104.ThuMar141729172024/consensi.fa -pa 4 -s -xsmall -e ncbi GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna
```
#### Comparative Table of Masked Genome Portions

| Tool          | Percentage of Masked Genome |
|---------------|-----------------------------|
| WindowMasker  | 22.32%                      |
| DUST          | 5.46%                       |
| TRF           | 3.21%                       |
| RepeatMasker  | 19.62%                      |

The analysis of repeat masking across different tools - **WindowMasker**, **DUST**, **Tandem Repeats Finder (TRF)**, and **RepeatMasker** using a custom library from RepeatModeler - reveals distinct variations in their ability to identify and mask repetitive sequences in the genome.  
**WindowMasker** and **RepeatMasker**, showing higher percentages of genome masking at 22.32% and 19.62% respectively, demonstrate a broad capacity for detecting a wide array of repetitive elements.  
**DUST**, with a focus on low-complexity sequences, identified a smaller yet significant portion of the genome (5.46%) as repetitive.  
**TRF**, specializing in tandem repeats, accounted for 3.28% of the genome, indicating its specialized role in uncovering repetitive sequences that other tools might overlook.  
While WindowMasker and RepeatMasker offer a more generalized approach suitable for broad sweeps of the genome, DUST and TRF provide depth and specificity, particularly valuable for detailed analysis of certain repeat types.

## Task 3:  Interpreting Results

#### Importance of Repeats Masking in Genome Analysis
The process of repeats masking is fundamental to genome analysis, serving as a critical step in preparing genomic data for accurate interpretation and further study. By identifying and masking repetitive sequences, researchers can prevent the misalignment of sequencing reads, which is essential for accurate gene annotation, variant identification, and phylogenetic analysis. Unmasked repeats can lead to the overestimation of gene content, incorrect gene structure prediction, and challenges in pinpointing evolutionary conserved regions. Essentially, repeats masking facilitates the distinction between unique genomic regions and repetitive DNA, thereby enhancing the reliability of genome assembly, comparison, and functional analysis.

#### Tool Comparison

**WindowMasker:**
- **Strengths:** Identifies a wide range of repetitive elements, including simple repeats and complex repeats. Relatively fast and easy to use.  
- **Weaknesses:** Can be prone to false positives, especially for low-complexity repeats.

**DUST:**  
- **Strengths:** Efficient filtering of low-complexity repeats. Less prone to false positives for these types of repeats.  
- **Weaknesses:** May not detect complex repetitive elements as effectively.

**TRF:**  
- **Strengths:** Focuses on tandem repeats with high accuracy. Useful for identifying functional repeats within microsatellites and regulatory regions.  
- **Weaknesses:** May miss dispersed repeats or repeats with lower periodicity.

**RepeatMasker:**  
- **Strengths:** Identifies repetitive elements against a comprehensive repeat database. Provides detailed information about the types and families of repeat elements discovered.  
- **Weaknesses:** Can be computationally intensive depending on database and search settings.

#### Biological Implications

The differential identification of repetitive sequences by WindowMasker, DUST, TRF, and RepeatMasker sheds light on the complex nature of the genome's structure and evolution. The varying amounts and types of repeats detected by each tool highlight the genome's dynamic composition, reflecting mechanisms of genome evolution such as duplication, mutation, and selection. These insights are invaluable for understanding genomic architecture, evolutionary history, and functional genomics. For instance, the prevalence of certain repeat types may indicate genomic stability regions or hotspots for recombination, influencing gene expression and phenotypic diversity. This knowledge can propel further research into genome function, species adaptation, and the identification of targets for genetic improvement or disease research.


