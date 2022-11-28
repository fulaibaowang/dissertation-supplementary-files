# dissertation-supplementary-files
this is the supplementary file of my dissertation: Assessing the role of microbes in the evolution of the fruit fly Drosophila melanogaster


# CS_main_pipe

`samples.tsv`

Syntax:
```
<label>;<paired_samples>;<list_of_control_samples>;<list_of_treated_samples>;<list_of_control_sgrnas>;<list_of_control_genes>
```

Example 1:
```
ToxA;paired;Control_R1_S14_L008_R1_001_x.fastq.gz,Control_R2_S15_L008_R1_001_x.fastq.gz;ToxA_R1_98_S2_L008_R1_001_x.fastq.gz,ToxA_R2_S2_L005_R1_001_x.fastq.gz;none;none
ToxB;paired;Control_R1_S14_L008_R1_001_x.fastq.gz,Control_R2_S15_L008_R1_001_x.fastq.gz;ToxB_R1_98_S4_L008_R1_001_x.fastq.gz,ToxB_R2_S4_L005_R1_001_x.fastq.gz;none;none
```

Example 2:
```
ToxA;;;ToxA_R1_98_S2_L008_R1_001_x.fastq.gz,ToxA_R2_S2_L005_R1_001_x.fastq.gz;none;none
ToxB;;;ToxB_R1_98_S4_L008_R1_001_x.fastq.gz,ToxB_R2_S4_L005_R1_001_x.fastq.gz;none;none
```

Current mageck workflow:
- fastqc for raw reads
- clean reads with cutadapt leaving only the sgRNA to be mapped
- fastqc for clean reads
- mapping approach:
    - bowtie2 indexing of reference sequences
    - alignment of cleaned reads to reference sequences with bowtie2
- mageck count (either from aligned reads or from cleaned fastq)
- mageck test with CNV normalization when available
- calculation of sgRNA efficiencies with SSC when possible
- mageck mle with CNV normalization and sgRNA efficiencies when available
- mageck pathway
- mageck plot

Added:

- bagel2
- drugz

# VISPR

Copy the whole VISPR folder to your laptp (eg. `${HOME}/Desktop/vispr`) and map this folder to `/vispr/` when starting the VISPR container.
```
$ docker run -it -v ${HOME}/Desktop/vispr/:/vispr/ -p 5000:5000 --privileged=true davidliwei/mageck-vispr /bin/bash
$ vispr server --host 0.0.0.0 /vispr/*.yaml
```
You can now access VISPR on Chrome using [http://0.0.0.0:5000/](http://0.0.0.0:5000/)

# Local dev

```
mkdir -p ${HOME}/crispr/jupyter/ ${HOME}/crispr/data/CS_main_pipe
cd ${HOME}/crispr/data/CS_main_pipe
git clone https://github.molgen.mpg.de/mpg-age-bioinformatics/CS_main_pipe.git code
wget -o PinAPL-py_demo_data.zip http://pinapl-py.ucsd.edu/example-data
unzip PinAPL-py_demo_data.zip
wget http://www.gsea-msigdb.org/gsea/msigdb/download_file.jsp?filePath=/msigdb/release/7.2/msigdb.v7.2.symbols.gmt
head -n 1 msigdb.v7.2.symbols.gmt > msigdb.v7.2.symbols.gmt_
wget https://data.broadinstitute.org/ccle_legacy_data/dna_copy_number/CCLE_copynumber_byGene_2013-12-03.txt.gz
unpigz CCLE_copynumber_byGene_2013-12-03.txt.gz 
docker pull mpgagebioinformatics/crispr:latest
docker run -v ${HOME}/crispr/jupyter/:/root/.jupyterhub/ -v ${HOME}/crispr/data/:/srv/jupyterhub/ -p 8181:8181 -it mpgagebioinformatics/crispr:latest /bin/bash
```

# paper blur

Following demultiplexing, raw NGS libraries were quality checked using FastQC (version 0.11.8).

Upstream sequences and sgRNA length were used to trim reads with CutAdapat (version 3.1).

Mageck (version 0.5.9.4) count was used to quantify the number of reads per sgRNA. Mageck test was use to test and rank sgRNAs and genes. Maximum-likelihood analysis of gene essentialities was done using Mageck mle. 

# different tools

## 1.MAGeCK (original MAGeCK, or ‘MAGeCK-RRA’)
- four steps: read count normalization, mean-variance modeling, sgRNA ranking and gene ranking
- sgRNAs are ranked based on P-values calculated from the negative binomial model
- a modified robust ranking aggregation (RRA) algorithm named α-RRA to is used to identify positively or negatively selected genes.
- different from edgeR, deseq2: not only at sgRNAs but also at gene and pathway levels

## 2.MAGeCK-VISPR (MAGeCK-mle + VISPR)
- extends MAGeCK-RRA by a maximum likelihood estimation method to call essential genes under multiple conditions (MAGeCK-RRA can only compare samples between two conditions) while considering sgRNA knockout efficiency
- provides a web-based visualization framework (VISPR)

