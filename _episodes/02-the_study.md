---
title: "The case-study"
teaching: 10
exercises: 0
questions:
- "What is the biological question?"
- "What is the experimental design?"
- "What is the data?"
- "RNA-seq analysis"


objectives:
- "Discuss how to address a biological question using RNA-Seq"
keypoints:
- XXX
---


#### The study
- Today we will work with the dataset generated for a knockout mouse model to study Williams-Beuren Syndrome (WBS) obtained from a study published in 2016 by <I>Corley et al</I>. 

  <p align="center">
  <img src="{{ page.root }}/fig/the_study.png" style="margin:10px;height:200px"/>
  </p>

- Williams-Beuren Syndrome (WBS) is a rare disease found in people.
- WBS is a genetic disorder associated with multisystemic abnormalities such as: 
   - Facial dysmorphology
   - Intellectual disability
   - Cardiovascular abnormalities
- WBS is caused by a hemizygous microdeletion involving up to 28 genes in chromosome 7q11.23. 
- Two evolutionary-related transcription factors, GTF2I and GTF2IRD1, a transcription factor gene are implicated as prime candidates for the cause of the facial dysmorphology.

#### The experimental design
[Corley et al. 2016](https://pubmed.ncbi.nlm.nih.gov/27295951/) created a Gtf2ird1 knockout mouse model of this disease with an aim to improve the understanding of the WBS disease.

<p align="center">
  <img src="{{ page.root }}/fig/experimental_design.png" style="margin:10px;height:150px"/>
</p>

- The experimental design consists of a 3 X 3 design.
   - Wildtype healthy mice (WT) : 3 replicates
   - Knock out (KO) mice in which the Gtf2ird1 gene was knocked out : 3 replicates
 
#### The biological question
- Which genes are upregulated or downregulated between the two treatment groups (WT and knockout mice)?
- Do the regulated genes relate to the disease phenotype?
 
#### The sequencing data 
- RNA sequencing was performed on the Illumina HiSeq2000 platform to generate 100 base pair reads.
- The raw sequence files are provided in the [FASTQ](https://www.drive5.com/usearch/manual7/fastq_files.html) format.
- A reduced sequencing data (sub-setted to chr 18 region) will be used to ensure that the jobs complete in an appropriate time on a training virtual manchine (VM).
- The full dataset will be used to repeat the the analysis to identify functional enrichments from the differentially expressed (DE) genes.

#### Analysis 
**Part-I:**  RNA-Seq reads to counts
- Here we will ask the following questions:
   - How to convert RNA-seq reads into counts?
   - How to perform quality control (QC) of RNA-seq reads?
   - How to do this analysis efficiently using a nextflow pipeline?

**Part-II:** RNA-seq counts to genes  
- In part II we ask the following questions:
      - What are the differentially expressed genes across the wild type and KO mice?
      - How to analyze RNA count data using the R-library DeSeq2?
      - How to identify functionally enriched gene groups from the list of differentially expressed genes?


