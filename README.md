# dissertation-supplementary-files
this is the supplementary file of my dissertation: Assessing the role of microbes in the evolution of the fruit fly Drosophila melanogaster



1.[MAGeCK](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0554-4) (original MAGeCK, or ‘MAGeCK-RRA’)
- four steps: read count normalization, mean-variance modeling, sgRNA ranking and gene ranking
- sgRNAs are ranked based on P-values calculated from the negative binomial model
- a modified robust ranking aggregation (RRA) algorithm named α-RRA to is used to identify positively or negatively selected genes.
- different from edgeR, deseq2: not only at sgRNAs but also at gene and pathway levels

2.[MAGeCK-VISPR](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-015-0843-6) (MAGeCK-mle + VISPR)
- extends MAGeCK-RRA by a maximum likelihood estimation method to call essential genes under multiple conditions (MAGeCK-RRA can only compare samples between two conditions) while considering sgRNA knockout efficiency
- provides a web-based visualization framework (VISPR)

3. MAGeCK flute
