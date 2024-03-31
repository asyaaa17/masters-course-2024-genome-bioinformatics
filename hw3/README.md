# Genome Bioinformatics: Homework 3. Gene Annotation in Genomes

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
I used AUGUSTUS to predict genes in the Drosophila melanogaster genome. Given the significant size of the genome and computational constraints, I performed the analysis using the fly species model.

3. **Running AUGUSTUS for Gene Prediction:**  
    ```bash
    time augustus --species=fly GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna > augustus_output.gff3
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

6. **Run GeneMark**:
     ```bash
     perl ~/hw3/gmes_linux_64_4/gmes_petap.pl --sequence GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna --ES --cores 8
     ```
This procedure takes ~150 minutes.
#### Deliverables

- **Summary of Predicted Gene Numbers**:
The AUGUSTUS tool was used for gene prediction in the *Drosophila melanogaster* genome using the `fly` species model. The process resulted in a file named `augustus_output.gff3`, which contains the predicted gene structures.
    ```bash
     grep -c -P "\tgene\t" augustus_output.gff3
     ```
- **Total predicted genes (AUGUSTUS):** 6310
  
After running GeneMark-ES, the number of predicted genes will be detailed in the output file `genemark.gtf`. 
    ```bash
    grep -c 'gene' genemark.gtf
    ```   
- **Total predicted genes (GeneMark-ES):** 185915

The significant difference in gene counts between genemark.gtf and augustus_output.gff3 could be attributed to the inherent differences in prediction methodologies, parameters used, and sensitivity of the GeneMark and AUGUSTUS gene prediction tools. GeneMark might predict a larger number of genes due to its modeling approach or settings, which could result in a higher sensitivity to potential gene sequences. Conversely, AUGUSTUS, depending on its configuration and training, might provide a more conservative estimate, focusing on genes with higher confidence levels. These variations highlight the importance of understanding the characteristics and underlying algorithms of each tool for genomic annotation.

