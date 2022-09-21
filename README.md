# Overview

Presto performs a fast Wilcoxon rank sum test and auROC analysis. Latest benchmark ran 1 million observations, 1K features, and 10 groups in 16 seconds (sparse input) and 85 seconds (dense input). 

the logfc was calculated same as seurat now. 


# Installation

We are working on getting presto into CRAN. For now, install Presto from github directly:

```{r}
library(devtools)
install_github('microbiomeCat/presto')
```

# Usage

Run presto on a matrix, Seurat, or SingleCellExperiment input object. 

```
wilcoxauc(X, y)
wilcoxauc(seurat_object, 'group_name')
wilcoxauc(sce_object, 'group_name')
```

For examples, see `?wilcoxauc` and the [vignette](http://htmlpreview.github.io/?https://github.com/immunogenomics/presto/blob/master/docs/getting-started.html)

# get seurat results

```

markers <- wilcoxauc(seurat_object, 'seurat_clusters',seurat_assay="RNA",assay="data") %>% 
  filter(logFC >= 0.25) %>% filter(pct_in >= 10) %>% filter(pval <= 0.01)  

markers$padj <- p.adjust(markers$pval, method='bonferroni', n=nrow(seurat_object@assays$RNA@data))

markers <- markers[,c("pval","logFC","pct_in","pct_out","padj","group","feature")]
colnames(markers) <- c("p_val","avg_log2FC","pct.1","pct.2","p_val_adj","cluster","gene")
markers$pct.1 <- round(markers$pct.1/100,3)
markers$pct.2 <- round(markers$pct.2/100,3)
```
