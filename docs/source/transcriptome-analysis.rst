.. image:: /Images/NC_State.gif
   :target: http://www.ncsu.edu


.. role:: bash(code)
   :language: bash


Transcriptome Analysis Annotation and Differential Gene Expression
==================================================================



Global Overview
***************

Transcriptome analysis is a very broad topic that covers a spectrum from initial characterization of expressed genes in a non-model species with no other genomic information available, to detailed analysis of alternative splicing and gene expression among organs, tissues, or even individual cells of a model organism for which a well-annotated reference genome sequence is known. As previously noted, if the objective of a sequencing experiment is simply discovery of the sequence itself, then experimental design considerations may be less critical, but if the objective is to use the sequencing experiment as a quantitative measure of some biological process (such as gene expression or alternative splicing differences among individuals, developmental stages, or treatments), then an appropriate experimental design is essential.


Objective
*********

The objective of this session is to introduce participants to the additional complexity of analyzing transcriptomes by deep sequencing, above the already complex process of analyzing genome structure. RNA transcripts vary both in abundance, in primary sequence, and in the outcome of splicing processes. All these types of variation can have important biological effects, and may be of interest in a "transcriptomics" experiment. 



Description
***********

RNA-seq experiments are growing in popularity as a means of characterizing the transcriptome, detecting alternative splicing events, and measuring differences in gene expression between samples of different types. The importance of good experimental design in conducting RNA-seq experiments is emphasized in the first paper in the recommended reading, by Auer and Doerge. Any experiment in which differences between samples are to be interpreted in a biological context should take seriously the need for good experimental design. The most reliable conclusions will result from a well-replicated design in which the experimental treatments are orthogonal to nuisance factors. `Slides <https://drive.google.com/open?id=1NB2ICMSgcGO10v0i5ZhwQZuNhsMmR4C9>`_ with an overview of RNA-seq workflows and some discussion of experimental design and analysis strategies are available. Engström et al (`2013 <http://www.nature.com/nmeth/journal/v10/n12/full/nmeth.2722.html>`_) compared the performance of several different programs designed to align RNA-seq reads to reference genome sequences, and provide a thorough discussion of the advantages and disadvantages of the programs tested. More recently, Conesa et al (`2016 <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4728800/>`_) surveyed best practices for RNA-seq experiments, and noted that no single workflow is optimal for every experiment. 

\

The exercise in RNA-seq data analysis will follow the description in the `EdgeR user's guide <https://www.bioconductor.org/packages/3.4/bioc/vignettes/edgeR/inst/doc/edgeRUsersGuide.pdf>`_ or the `DESeq2 vignette <https://bioconductor.org/packages/3.4/bioc/vignettes/DESeq2/inst/doc/DESeq2.pdf>`_ on your own or in class. The exercise is based on an experiment reported by Cumbie et al. (`2011 <http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0025279>`_), and involves comparison of gene expression levels in Arabidopsis plants inoculated with a bacterial pathogen or mock-inoculated with sterile solution. The complete data from the experiment are downloaded from `NCBI SRA <http://www.ncbi.nlm.nih.gov/sra/?term=SRP004047>`_ during the exercise using the `At_RNAseq.sh <https://drive.google.com/open?id=18NJkXMWjOLUzgiiez4Q-t_z6alM40h7Z>`_ script saved in the `AtRNAseq <https://drive.google.com/open?id=1_-cX7Scvp_e8zlN4glcD3-i2eJg5Tv71>`_ archive; R scripts to run differential gene expression analysis with **DESeq2** or **edgeR** packages are in the same directory. The same directory also contains smaller datasets consisting primarily of reads that map to chromosome 5 of the Arabidopsis genome; these smaller datasets allow the exercise to be completed more quickly and with less RAM if you want to carry out the analysis on a laptop with fewer resources.


Key Facts
*********

RNA-Seq library construction strategies may be different for different experimental objectives:

+ Differential gene expression: one sequenced “tag region” per gene is enough to estimate relative levels of gene expression if a reference genome sequence is available to allow mapping tags uniquely to genes, but "tag sequencing" does not give information on alternate splicing.

\

+ Gene discovery: comprehensive coverage of transcripts is an asset to obtain complete sequences of expressed genes in species for which a reference genome sequence is not available. Normalization methods that reduce the difference in abundance among transcripts can work well to obtain more complete coverage of all transcripts, but may be a problem if accurate estimates of the relative abundance of transcripts is an experimental objective.

\

+ Alternative splicing analysis: complete coverage of exons is essential, and estimates of relative abundance are important also.

Often researchers are interested in all aspects of transcriptome analysis – discovering new transcripts or alternate splicing events of annotated genes, plus measuring relative abundance and detecting genetic variation – so many RNA-Seq experiments use non-normalized libraries of cDNA primed with random oligos, to give relatively uniform coverage across entire transcripts. Accurate reconstruction of alternatively-spliced transcripts from individual genes is an important part of RNA-seq data analysis if the experimental objectives include testing for significant differences in levels of alternatively-spliced transcripts among individuals (genetic variation in splicing) or among treatment groups (which may include developmental stages as well as environmental conditions or chemical exposures). Software designed to test for association between genetic variants and levels of alternatively-spliced transcripts is available (Monlong et al, `2014 <http://www.nature.com/ncomms/2014/140820/ncomms5698/full/ncomms5698.html>`_). Pipeline tools that combine multiple software packages into an integrated analytical approach have been described by multiple groups (Cumbie et al, `2011 <http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0025279>`_ and Varet et al., `2015 <http://biorxiv.org/content/early/2015/09/26/021741>`_ are examples); these may be worth setting up if you have a lot of data to analyze and want the added functionality of an integrated pipeline.




Exercises
*********

You can use the commands found in the `Transcriptome_data.txt <https://drive.google.com/open?id=1jSNUzeBRg1dExWJhI2ylxRfggHYh4s1->`_ to download the required archives directly from the command line. 

1. A subset of the experimental data from `Cumbie et al 2011 <http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0025279>`_ is saved in the `AtRNAseq <https://drive.google.com/open?id=1_-cX7Scvp_e8zlN4glcD3-i2eJg5Tv71>`_ archive. The files `c1.fq.gz <https://drive.google.com/open?id=1A1ePOEEQxgY5-WbtH99_-wfpivYpLRyT>`_, `c2.fq.gz <https://drive.google.com/open?id=1OIwpkuNJIAhfDoXFsfAiEbCho6EXt412>`_, and `c3.fq.gz <https://drive.google.com/open?id=1DhVkPmszlpvH8dIKXef2iiSO-cF_cj-v>`_ contain sequence reads from three biological replicates of control samples, and files `t1.fq.gz <https://drive.google.com/open?id=13xP7gcbNCT8BwbGh1_bLg6LF_AWfruhn>`_, `t2.fq.gz <https://drive.google.com/open?id=1_gPRcV7zzs8HixgK7dwNRb-h8MPXjMpc>`_ and `t3.fq.gz <https://drive.google.com/open?id=1wr0qCiomXFSiB2T9zdrzYRSB7FcW67Cy>`_ contain sequence reads from three biological replicates of test samples. The file `Atchromo5.fasta.gz <https://drive.google.com/open?id=1i5p9JlQZh_xvhGN_d9JvLVaOxqF8Hp0_>`_ contains the sequence of Arabidopsis chromosome 5.

\
 
2. A script to download the complete data from the Sequence Read Archive at NCBI is `At_RNAseq.sh <https://drive.google.com/open?id=18NJkXMWjOLUzgiiez4Q-t_z6alM40h7Z>`_, and scripts to analyze the resulting data with `kallisto <https://drive.google.com/open?id=1EbVcHki5CeE2CGYGc682XFl4lQjKBbsB>`_ and either `edgeR <https://drive.google.com/open?id=1T_Am4Aj_RnYw-kFWpJFetNXo-DXNS_h1>`_ and `DESeq2 <https://drive.google.com/open?id=1fXbjVEqA-YRb_Vwd3C2MH17aBct6Tp5N>`_ are also available - these scripts are saved in the AtRNAseq archive from the google team drive, and links are provided here for those who want to try the analysis on other machines. A script to load output from `RSEM into R <https://drive.google.com/open?id=18q0rowXeDdbJC1D6agIg9cIptB9VDHsT>`_ for analysis  is also available. 

\
 
3. A fairly comprehensive discussion of RNA-seq workflow options (including different approaches to producing tables of read counts from BAM alignment files) is available in a `Bioconductor tutorial on gene-level exploratory data analysis <http://www.bioconductor.org/help/workflows/rnaseqGene/>`_; a description of using biomaRt, GO, and KEGG for annotation is given in `this tutorial <https://cran.r-project.org/web/packages/biomartr/vignettes/Functional_Annotation.html>`_. 

\
 
4. BAM alignment files are not the only way to estimate the number of transcripts from each gene detected in an RNA-seq dataset; an alternative approach is to create a k-mer hash table of the transcripts that might be detected, then use that table to analyze the filtered and trimmed reads themselves to estimate the count of reads from each transcript, and therefore the counts for each transcript detected. Software tools to carry out this type of transcript-count estimation include `Sailfish <http://www.cs.cmu.edu/~ckingsf/software/sailfish/>`_,  `Salmon <https://combine-lab.github.io/salmon/>`_, `Kallisto <https://pachterlab.github.io/kallisto/about>`_, and `HTSeq <http://www-huber.embl.de/HTSeq/doc/overview.html>`_. The AtRNAseq archive from the google team drive contains a file called `TAIR10.cDNA.fa.gz <https://drive.google.com/open?id=13n6Iu-Aht4ikGH2SyX0yTwKVfx3ply3R>`_ that contains 41,671 sequences of putative transcripts predicted from gene models in the TAIR 10 version of the Arabdopsis genome assembly. This file was downloaded from www.arabidopsis.org, from the Download/Sequences/TAIR10blastsets directory as a file called TAIR10_cdna_20101214_updated. 

\

5. The "Tuxedo" package of programs (`Bowtie2 <http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.3.0/bowtie2-2.3.0-linux-x86_64.zip>`_, `Tophat <http://ccb.jhu.edu/software/tophat/downloads/tophat-2.1.1.Linux_x86_64.tar.gz>`_, `Cufflinks <http://cole-trapnell-lab.github.io/cufflinks/assets/downloads/cufflinks-2.2.1.Linux_x86_64.tar.gz>`_) provide splice-aware read alignment, transcript reconstruction, and estimation of transcript abundance. The latest versions of Bowtie2, Tophat, and Cufflinks are available as compiled executables, and those version can read and write gzipped files. Simply download and unpack the archives for each program, then create a symbolic link between the program and the /usr/local/bin directory

\
 
6. A complete tutorial for analysis of RNA-seq data using Tophat and Cufflinks is available in `Trapnell et al (2012) <http://www.nature.com/nprot/journal/v7/n3/full/nprot.2012.016.html>`_; this can be used as a guide to carry out analysis of the control and test datasets used for the RNA-seq exercise described above.



Additional Resources
********************

+ `Statistical design and analysis of RNA sequencing data <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2881125>`_. Auer & Doerge, Genetics 185(2):405-416, 2010.

\

+ `Systematic and integrative analysis of large gene lists using DAVID bioinformatics resources. <https://www.nature.com/nprot/journal/v4/n1/pdf/nprot.2008.211.pdf>`_ Huang et al, Nature Protocols 4: 44-57, 2009

\

+ `Identification of genetic variants associated with alternative splicing using sQTLseekeR. <http://www.nature.com/ncomms/2014/140820/ncomms5698/full/ncomms5698.html>`_ Monlong et al, Nature Comm 5:4698, 2014 

\

+ `Scotty: a web tool for designing RNA-Seq experiments to measure differential gene expression. <http://bioinformatics.oxfordjournals.org/content/29/5/656>`_ Busby et al, Bioinformatics 29:656–657, 2013 

\

+ `Systematic evaluation of spliced alignment programs for RNA-seq data. <http://www.nature.com/nmeth/journal/v10/n12/full/nmeth.2722.html>`_ Engström et al, Nature Methods 10:1185-1191, 2013. *This paper reports results of comparisons of several different splice-aware alignment programs, and concludes that none of the programs tested is optimal by all criteria. The STAR alignment program (Dobin et al, 2013; see next reference) ranks highly by most measures, though, and is recommended for use by the Broad Institute as part of their* `Best Practices <https://www.broadinstitute.org/gatk/guide/best-practices?bpm=RNAseq>`_ *pipeline for variant discovery in RNA-Seq experiments.*

\

+ `STAR: ultrafast universal RNA-seq aligner. <http://bioinformatics.oxfordjournals.org/content/29/1/15>`_ Dobin et al, Bioinformatics 29:15-21, 2013

\

+ `A survey of best practices for RNA-seq data analysis. <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4728800/>`_ Conesa et al, Genome Biology 17:13, 2016 

\

+ `GENE-counter: a computational pipeline for the analysis of RNA-seq data for gene expression differences. <http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0025279>`_ Cumbie et al, PLoS ONE 6(10): e25279, 2011.

\

+ `Molecular indexing enables quantitative targeted RNA sequencing and reveals poor efficiencies in standard library preparations. <http://www.pnas.org/content/111/5/1891>`_ Fu et al, PNAS 111:1891–1896, 2014

\

+ `Robust adjustment of sequence tag abundance. <http://www.ncbi.nlm.nih.gov/pubmed/24108185>`_ Baumann & Doerge, Bioinformatics 30(5):601-605, 2014

\

+ `Differential analysis of gene regulation at transcript resolution with RNA-seq. <http://www.nature.com/nbt/journal/v31/n1/full/nbt.2450.html>`_ Trapnell et al, Nat Biotechnol 31:46-53, 2013

\

+ `Improving RNA-Seq expression estimates by correcting for fragment bias. <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3129672/>`_ Roberts et al, Genome Biol 12:R22, 2011



Last modified 24 February 2019.
Edits by `Ross Whetten <https://github.com/rwhetten>`_, `Will Kohlway <https://github.com/wkohlway>`_, & `Maria Adonay <https://github.com/amalgamaria>`_.
