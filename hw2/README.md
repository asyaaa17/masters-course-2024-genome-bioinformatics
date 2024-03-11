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
