---
title: "DE genes to functional enrichment in R"
teaching: 10
exercises: 10
questions:
- "How to perform enrichment analysis in R (RStudio)?"
- "How to visualise functionally enriched Gene ontologies (GO) / pathways as networks?"

objectives:
- "Perform Functional enrichment analysis of the DE genes in 'R'"

keypoints:
- XXX
---

### clusterProfiler for Gene Ontology (GO) over-representation analysis 
- We will be using clusterProfiler to perform over-representation analysis on GO terms associated with our list of significant genes. 
- The tool takes as input a significant gene list and a background gene list and performs statistical enrichment analysis using hypergeometric testing.
- The basic arguments allow the user to select the appropriate organism and GO ontology (BP, CC, MF) to test.

> ## Get the lists of UP and DOWN -regulated genes 
> ```
> # P adj < 0.05 
> sig <- res_tidy.DE[res_tidy.DE$p.adjusted < 0.05, ]
> # Upregulated: LFC > 1, remove NAs
> sig.up <- sig[sig$estimate > 1, ]
> sig.up <- na.omit(sig.up)
> sig.up.LFC <- sig.up$estimate
> names(sig.up.LFC) <- sig.up$gene
> # Sort by LFC, decreasing
> sig.up.LFC <- sort(sig.up.LFC, decreasing = TRUE)
> # Downregulated: LFC < 1, remove NAs
> sig.dn <- sig[sig$estimate < 1, ]
> sig.dn <- na.omit(sig.dn)
> sig.dn.LFC <- sig.dn$estimate
> names(sig.dn.LFC) <- sig.dn$gene
> # Sort by LFC, decreasing
> sig.dn.LFC <- sort(sig.dn.LFC, decreasing = TRUE)
> ```
> {: .language-bash}
{: .solution}

### Genes Down-regulated in WT
- The clusterProfiler package implements enrichGO() for gene ontology over-representation test.
> ## Run enrichGO
> ~~~
> ego.down <- enrichGO(gene = names(sig.up.LFC),
>                       OrgDb = org.Mm.eg.db, 
>                       keyType = 'SYMBOL',
>                       readable = FALSE,
>                       ont = "ALL",
>                       pAdjustMethod = "BH",
>                       pvalueCutoff = 0.05, 
>                       qvalueCutoff = 0.2)
> ~~~
> {: .language-bash}
{: .solution}

- Explanation 
- Observation 1
- Observation 2

> ## Bar plot
> Bar plot is the most widely used method to visualize enriched terms. 
> It depicts the enrichment scores (e.g. p values) and gene count or ratio as bar height and color.
> ```{r, fig.height=7, fig.width=6}
> barplot(ego.down, showCategory=20)
> ```
> {: .language-bash}
{: .solution}

- Observation 1
- Observation 2


> ## cnetplot
- Both the barplot and dotplot only displayed most significant enriched terms, while users may want to know which genes are involved in these significant terms. 
> - The cnetplot depicts the linkages of genes and biological concepts (e.g. GO terms or KEGG pathways) as a network.
> - The cnetplot depicts the linkages of genes and biological concepts (e.g. GO terms or KEGG pathways) as a network.
> ```{r}
> cnetplot(ego.down, 
>          categorySize="pvalue", 
>          foldChange=sig.up.LFC,
>         cex_label_gene = 1,
>          showCategory = 5,cex_label_category=1.2,shadowtext='category')
> ```
> {: .language-bash}
{: .solution}

- Explanation 
- Observation 1
- Observation 2

> ## Heatmap-like functional classification
> - The heatplot is similar to cnetplot, while displaying the relationships as a heatmap. 
> - The gene-concept network may become too complicated if user want to show a large number significant terms. 
> - The heatplot can simplify the result and more easy to identify expression patterns.
> ```{r}
> heatplot(ego.down)
> ```
> {: .language-bash}
{: .solution}

> ## UpSet Plot
> - The upsetplot is an alternative to cnetplot for visualizing the complex association between genes and gene sets. 
> - It emphasizes the gene overlapping among different gene sets.
> ```{r}
> #Not working as of now!!! Check if this adds value?
> #library("DOSE")
> #upsetplot.library()
> #upsetplot(ego.up,n=10)
> ```
> {: .language-bash}
{: .solution}

