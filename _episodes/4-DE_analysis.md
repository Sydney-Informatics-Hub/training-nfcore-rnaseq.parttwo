---
title: "Differential expression analysis using DeSeq2"
teaching: 10
exercises: 0
questions:
- "How to use the DESeq2 module to perform differential expression (DE) analysis?"
- "How to visualise the results from DeSeq2 analysis?"



objectives:
- "Generate a list of differentially expressed genes."
- "Use visualisation techniques to analyse the results."

keypoints:
- We have used DeSeq2 to identify DE genes.
- 

---



#### Differential expression using the DESeqDataSet object
- A DESeqDataSet object must have an associated design formula. 
  - The design formula expresses the variables which will be used in modeling. 
  - The formula should be a tilde (~) followed by the variables with plus signs between them.
  - The design can be changed later, however then all differential analysis steps should be repeated, as the design formula is used to estimate the dispersions and to estimate the log2 fold changes of the model.
- There are multiple ways of constructing a DESeqDataSet, depending on what pipeline was used upstream of DESeq2 to generated counts or estimated counts
  - Here we have a matrix (as read in a dataframe above) of read counts prepared from our previous analysis using nfcore-rnaseq pipeline.
  - So we use have used the function - DESeqDataSetFromMatrix


```{r}
dds <- DESeqDataSetFromMatrix(countData = counttable,
colData = meta,
design = ~ condition)
colData = meta
```
In the above formula we have a : 
- A count matrix (countData) called counttable 
- A table of sample information called meta. 
- The design which defines the sample types as per the condition ; here `WT` and `KO`
- If we are controlling for batch differences then the design can be defined as design= ~ batch + condition.
- The two factor variables batch and condition should be columns of coldata.


### Pre-filtering of lowly expressed genes
- Our count matrix may contain many rows with only zeros, and additionally many rows with only a few fragments in total
- Pre-filtering of such genes with very low counts is useful because: 
- Genes with very low counts across all samples provide little evidence for differential expression and they interfere with some of the statistical approximations that are used later in the pipeline.
- They also add to the multiple testing burden when estimating false discovery rates, reducing power to detect differentially expressed genes. 


```{r}
# How many gene rows before filtering?
nrow(dds)
keep <- rowSums(cpm(counttable)>1) >=4
dds <- dds[keep,]

# How many gene rows after filtering?
nrow(dds)
```

**Note**: 
- Only 67% of the genes remain after performing this filtering step.
- This demonstrates the degree to which our performance will be improved by discarding the non-informative entries.
```
[1] 19859
[1] 13412
```

#### Explicitly set the factors levels 
- By default, R will choose a reference level for factors based on alphabetical order.
- So if you never tell the DESeq2 functions which level you want to compare against (e.g. which level represents the control group), the comparisons will be based on the alphabetical order of the levels.
- Setting the factor levels can be done in two ways:


```{r}
# Using factor
#dds$condition <- factor(dds$condition, levels = c("Wild","KO"))
#OR

# using relevel, just specifying the reference level:
dds$condition ~ relevel(dds$condition, ref="Wild")
```


### Differential expression analysis

#### The DESeq function
```{r}
dds <- DESeq(dds)
res <- results(dds)

# padj 0.05
res_padj0.05<-results(dds,alpha=0.05)
summary(res_padj0.05)
resSig005_subset<-subset(res_padj0.05, padj < 0.05)
write.table(resSig005_subset, "res_DeSeq2_FDR0.05_comparison_Wild_vs_KO_FUllMatrix.tab", sep="\t", col.names=NA, quote=F)

resSig005_subset <- data.frame(genes = row.names(resSig005_subset), resSig005_subset)
colnames(resSig005_subset)

# Writing normalized counts
normalised_counts<-counts(dds,normalized=TRUE)
write.table(normalised_counts, "normalised_all_samples_DeSeq2_FUllMatrix.tab", sep="\t", col.names=NA, quote=F)

```
> ## Results
> out of 13412 with nonzero total read count
> adjusted p-value < 0.05
> LFC > 0 (up)       : 1092, 8.1%
> LFC < 0 (down)     : 1216, 9.1%
> outliers [1]       : 4, 0.03%
> low counts [2]     : 0, 0%
> (mean count < 19)
> [1] see 'cooksCutoff' argument of ?results
> [2] see 'independentFiltering' argument of ?results

> [1] "genes"          "baseMean"       "log2FoldChange" "lfcSE"          "stat"           "pvalue"         "padj"  

> {: .language-bash}
{: .solution}


### Volcano plot
- A volcano plot can help us summarise the differential expression of gene for our analysis.
- The y-axis shows -log10(adjusted-p-value) while the X-axis shows logFC of all genes.
- The default colour coding highlights the statistically significant genes and further those with a specific logFC cutoff.


```{r}
## Merge with normalized count data
resdata <- merge(as.data.frame(res), as.data.frame(counts(dds, normalized=TRUE)), by="row.names", sort=FALSE)
names(resdata)[1] <- "Gene"
head(resdata)

## Volcano plot with "significant" genes labeled
volcanoplot <- function (res, lfcthresh=2, sigthresh=0.05, main="Volcano Plot", legendpos="bottomright", labelsig=TRUE, textcx=1, ...) {
with(res, plot(log2FoldChange, -log10(pvalue), pch=20, main=main, ...))
with(subset(res, padj<sigthresh ), points(log2FoldChange, -log10(pvalue), pch=20, col="red", ...))
with(subset(res, abs(log2FoldChange)>lfcthresh), points(log2FoldChange, -log10(pvalue), pch=20, col="orange", ...))
with(subset(res, padj<sigthresh & abs(log2FoldChange)>lfcthresh), points(log2FoldChange, -log10(pvalue), pch=20, col="green", ...))
if (labelsig) {
require(calibrate)
with(subset(res, padj<sigthresh & abs(log2FoldChange)>lfcthresh), points(log2FoldChange, -log10(pvalue), labs=Gene, cex=textcx, ...))
}
legend(legendpos, xjust=1, yjust=1, legend=c(paste("FDR<",sigthresh,sep=""), paste("|LogFC|>",lfcthresh,sep=""), "both"), pch=20, col=c("red","orange","green"))
}

volcanoplot(resdata, lfcthresh=1, sigthresh=0.05, textcx=.8, xlim=c(-2.3, 2),ylim=c(0, 50))


```
  <p align="center">
  <img src="{{ page.root }}/fig/volcano_plot_alternate.png" style="margin:10px;height:350px"/>
  </p>

#### Visualise a few differentially expressed (DE) genes
```{r}
dds_orig <- DESeqDataSetFromMatrix(countData = counttable, colData = colData,design = ~condition)

# Sp110
p_site_Sp110<-plotCounts(dds_orig, gene="Sp110", intgroup = "condition", returnData = TRUE) %>%
ggplot() + aes(dds_orig$condition, count) + geom_boxplot(aes(fill=dds_orig$condition)) + geom_jitter(color="black", size=0.6, alpha=0.9) + scale_y_log10() + theme_bw()+ggtitle("Sp110")+ theme(legend.position = "none")
p_site_Sp110
```
<p align="center">
  <img src="{{ page.root }}/fig/Sp110.png" style="margin:10px;height:350px"/>
</p>

```{r}
# Krt2
p_site_Krt2<-plotCounts(dds, gene="Krt2", intgroup = "condition", returnData = TRUE) %>%
ggplot() + aes(dds$condition, count) + geom_boxplot(aes(fill=dds$condition)) + geom_jitter(color="black", size=0.6, alpha=0.9) + scale_y_log10() + theme_bw()+ggtitle("Krt2")+ theme(legend.position = "none")
p_site_Krt2
```

<p align="center">
  <img src="{{ page.root }}/fig/Krt2.png" style="margin:10px;height:350px"/>
</p>



