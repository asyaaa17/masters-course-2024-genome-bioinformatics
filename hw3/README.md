# Genome Bioinformatics: Homework 3. Gene Annotation in Genomes

## TASK 1
### AUGUSTUS Installation and Running Instructions

1. **Create a Conda environment for AUGUSTUS:**  
    ```bash
    conda create -n augustus_env
    conda activate augustus_env
    ```

2. **Install AUGUSTUS via Conda:**  
   ```bash
    conda install -c bioconda augustus -y
    augustus --species=help
    ```
I used AUGUSTUS to predict genes in the Saccharomyces cerevisiae S288C genome masked (RepeatMasker). Given the significant size of the genome and computational constraints, I performed the analysis using the *Saccharomyces_cerevisiae_S288C* species model.

3. **Running AUGUSTUS for masked genome:**  
    ```bash
    time augustus --species=saccharomyces_cerevisiae_S288C GCA_000146045.2_R64_genomic.masked.fna > GCA_000146045.2_R64_genomic.masked.augustus_output.gff3
    ```
The execution was running approximately 246 minutes.

### GeneMark-ES Installation and Running Instructions

1. **Download GeneMark**:
   - Visit the [GeneMark website](http://exon.gatech.edu/GeneMark/) and download the GeneMark-ES/ET/EP+ ver 4.72_lic suitable for kernel 3.10 - 5.

2. **Extract the Package**:
   - Unzip the downloaded package using `tar -xzvf gmes_linux_64_4.tar.gz`, which will create a directory named `gmes_linux_64_4` containing the program files.

3. **Install Perl Dependencies**:
   - GeneMark requires several Perl modules to run. Install these modules using CPAN or CPANM:
     ```
     cpan YAML
     cpan Hash::Merge
     ```
         
4. **License Key**:
   - Download a license key (`gm_key_64.gz`) from the GeneMark website.
   - Unzip the license key: `gunzip gm_key_64.gz`.
   - Move the key to your home directory: `mv gm_key_64 ~/.gm_key`.

5. **Set Environment Variables**:
   - Add the following lines to your `.bashrc` or equivalent shell configuration file to set the necessary environment variables:
     ```bash
     export PATH=$PATH:$HOME/gmes_linux_64_4/bin
     export GMS_DIR=$HOME/gmes_linux_64_4
     export PERL5LIB=$PERL5LIB:/home/nana/perl5/lib/perl5
     export GENEMARK_PATH=$HOME
     ```

6. **Runing GeneMark for masked genome**:
     ```bash
     perl ~/hw3/gmes_linux_64_4/gmes_petap.pl --sequence GCA_000146045.2_R64_genomic.masked.fna --ES --cores 8
     ```
This procedure takes ~150 minutes.
#### Deliverables

- **Summary of Predicted Gene Numbers**:
The AUGUSTUS tool was used for gene prediction in the *saccharomyces_cerevisiae_S288C*. The process resulted in a file named `augustus_output.gff3`, which contains the predicted gene structures.
    ```bash
     grep -c -P "\tgene\t"  GCA_000146045.2_R64_genomic.masked.augustus_output.gff3
     ```
- **Total predicted genes (AUGUSTUS):** 6310
  
After running GeneMark-ES, the number of predicted genes will be detailed in `genemark.gtf` in the output file. 

    ```
     grep -c 'gene' genemark.gtf
     ```
- **Total predicted genes (GeneMark-ES):** 185915

The significant difference in gene counts between genemark.gtf and GCA_000146045.2_R64_genomic.masked.augustus_output.gff3 could be attributed to the inherent differences in prediction methodologies, parameters used, and sensitivity of the GeneMark and AUGUSTUS gene prediction tools. GeneMark might predict a larger number of genes due to its modeling approach or settings, which could result in a higher sensitivity to potential gene sequences. Conversely, AUGUSTUS, depending on its configuration and training, might provide a more conservative estimate, focusing on genes with higher confidence levels. These variations highlight the importance of understanding the characteristics and underlying algorithms of each tool for genomic annotation.

## TASK 2
### Homology-Based Annotation
### Exonerate with Proteins: Installation and Running Instructions

1. **Install Exonerate via Conda:**  
    ```bash
    conda install -c bioconda exonerate
    ```

2. **Download and unzip protein sequence from NCBI Ref-seq:**  
   ```bash
    wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/146/045/GCA_000146045.2_R64/GCA_000146045.2_R64_protein.faa.gz
    gzip -d GCA_000146045.2_R64_protein.faa.gz  
    ```

3. **Run Exonerate for masked genome:**  
    ```bash
    exonerate --model protein2genome --showtargetgff true GCA_000146045.2_R64_protein.faa GCA_000146045.2_R64_genomic.masked.fna > exonerate_output.gff3
    ```
### MMSeqs2 with Proteins: Installation and Running Instructions

1. **Install MMSeqs2 via Conda:**  
    ```bash
    conda install -c bioconda mmseqs2 -y
    ```

2. **Create a database for the protein sequences and the masked genome:**  
   ```bash
    mmseqs createdb GCA_000146045.2_R64_protein.faa proteinsDB
    mmseqs createdb GCA_000146045.2_R64_genomic.masked.fna genomeDB
    ```

3. **Run the search:**  
    ```bash
    time mmseqs search --start-sens 2 -s 7 --sens-steps 3 -a 1 --num-iterations 2 proteinsDB genomeDB resultDB tmp
    ```
 The execution was running approximately 124 minutes.  

Include a discussion on the significance of the protein alignments in gene annotation.*****

## TASK 3
### RNA-seq Mapping
### Exonerate with Proteins: Installation and Running Instructions
 
1. **Install HISAT2 via Conda:**  
    ```bash
    conda install -c bioconda hisat2
    ```
2. **Search SRA RNA-seq for Saccharomyces cerevisiae:**  
Information:
SRX9418308: GSM4876373: Input_rep1; Saccharomyces cerevisiae S288C; RIP-Seq
1 ILLUMINA (Illumina HiSeq 2500) run: 7.3M spots, 471M bases, 199Mb downloads

3. **Download and fastq-dump RNA-seq:**  
   ```bash
    wget  wget https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos3/sra-pub-zq-24/SRR012/12965/SRR12965715/SRR12965715.lite.1
    fastq-dump --split-files SRR12965715.lite.1
    ```
4. **Search and Download ref-seq genome for Saccharomyces cerevisiae**:
   ```bash
    wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/146/045/GCA_000146045.2_R64/GCA_000146045.2_R64_genomic.fna.gz
    gzip -d GCA_000146045.2_R64_genomic.fna.gz
    ```
5. **Build an index for your genome:**
    ```bash
    hisat2-build GCA_000146045.2_R64_genomic.fna index_yeast
    ```
6. **Map RNA-seq reads to the genome:**
   ```bash
    hisat2 -x index_yeast -U SRR12965715.lite.1_1.fastq -S mapped_reads.sam
    ```

   Discuss how RNA-seq evidence supports gene annotation.**********
    
























