---
title: "Introduction to Nextflow"
teaching: 10
exercises: 0
questions:
- "Why use bioinformatics workflow managers?"
- "What is Nextflow?"
- "What is nfcore?"

objectives:
- "Introduction to Nextflow and nfcore"
keypoints:
- Nextflow is a workflow management tool.
- nfcore is a community effort of curated nextflow pipelines.
---

### What are workflow managers?
- Transforming data into information involves running a large number of tools, optimizing parameters, and integrating dynamically changing reference data.
- [Workflow managers](https://www.nature.com/articles/s41592-021-01254-9)  simplify pipeline development, optimize resource usage, handle software installation and versions, and run on different compute platforms, enabling workflow portability and sharing. 
- More than 250 of such workflow managers are now available, including the more popular options such as [Snakemake](https://snakemake.readthedocs.io/en/stable/), [Nextflow](https://www.nextflow.io/) and [Galaxy](https://galaxyproject.org/learn/advanced-workflow/)


### Nextflow 
- [Nextflow](https://www.nextflow.io/) is one such workflow management tool which runs tasks across multiple compute infrastructures in a very portable manner. 
- Nextflow enables scalable and reproducible scientific workflows using software containers. 
- It uses [Docker](https://www.docker.com/)/[Singularity](https://docs.sylabs.io/guides/3.0/user-guide/index.html) containers making installation trivial and results highly reproducible.
- It allows the adaptation of pipelines written in the most common scripting languages.

### nfcore
- [nfcore](https://nf-co.re/) is a community effort to collect a curated set of analysis pipelines built using Nextflow.
- It is a diverse project spread across many groups.
- Currently [65 pipelines](https://nf-co.re/pipelines) are available as a part of nfcore.
- Some of the most commonly used nfcore pipelines include [nfcore-rnaseq](https://nf-co.re/rnaseq), [nfcore-sarek](https://nf-co.re/sarek) and [nfcore-ampliseq](https://nf-co.re/ampliseq) amongst otheres.
- A set of nfcore [companion tools](https://nf-co.re/tools/) are made available to assist with common tasks associated with nfcore pipeline usage. 

