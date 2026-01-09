# Differential-Gene-Expression-Analysis
Differential Gene Expression Analysis of Bulk and Single Cell RNA Data Using R and Python

- Analysed and identified cytokine-driven distinct Th0 vs Th2 gene expression in Naive CD4+ T cells and Memory CD4+ T cells in a paper published by Cano-Gamez et al, 2020.
- Used DESeq2 in R for bulk RNA-seq data analysis and annData and ScyPy in Python for single cell RNA Seq data analysis.

## Bulk RNA-Seq data analysis using R

Bulk RNA Seq measures average gene expression across thousands of cells in a pooled sample (like tissue or cells) to identify genes that are significantly turned up (upregulated) or down (downregulated) between different conditions. Here, we want to see the genes which are differentially expressed in CD4 Naive cells and CD4 Memory cells. 

the count matrix (ge_matrix) originally has 58051 rows (genes) and 94 columns (samples/ cells)
the metadata (pheno_matrix) has 94 rows (corresponding to the cells) and 10 columns (information about the sample)

we have 9 sample/ cell that satisfy our condition of
- simulation time = 5 days
- cytokine condition = Th0 or Th2
- cell type = CD4 Memory cell
therefore our ge_matrix is now 58051 x 9 and our pheno_matrix is now 9 x 10
that is, we are taking all the genes, but only those cells that meet our criteria
the same follows for CD4 Naive cells.

we remove genes who have < 10 reads across all samples for both Memory and Naive CD4 cells.
after removing these genes, we have 26656 genes with 10 or more reads in Memory cells. For Naive cells, this count is 27516 genes. 

The count matrix subset and metadata subset after selecting samples that satisfy the above condition-
- count matrix
![ge_matrix_subset](images/ge_mat_subset.png)
- metadata 
![pheno_matrix_subset](images/pheno_mat_subset.png)

PCA scree plot
![scree plot](images/scree_plot.png)

We see that samples are not grouped by sequencing batch. This is good as it tells us that sequencing batch is not a technical confounder. 
![pca sequencing batch](images/pca_sequencing_batch.png)

- DE genes in CD4 Memory cells
![vocano_plot](images/vocano_plot.png)
![heatmap](images/heatmap.png)

- DE genes in CD4 Naive cells
![vocano_plot](images/vocano_plot_naive.png)
![heatmap](images/heatmap_naive.png)

For Memory cells, we see that, when plotting sample groups by treatment, we dont see a clear distinction (visible for Naive CD4 cells)

- CD4 Memory cells sample groups by treatment
![vocano_plot](images/pca_condition_memory.png)
- CD4 Naive cells sample groups by reatment
![vocano_plot](images/pca_condition_naive.png)

This could be due to the following reasons:
- Cytokine effect is subtle: PCA is affected by some other stronger factor
- Cytokine affects a limited gene set: out of 26k genes, only a few genes respond
- Cytokine effect lies beyond PC1–PC2

### Result
The outcome of the analysis is:
- We have 25 differentially expressed genes in CD4 Memory cells.
- We have 939 differentially expressed genes in CD4 Naive cells.

- Number of shared DEGs between memory and naive CD4+ cells: 21 ("ENSG00000158125" "ENSG00000115896" "ENSG00000091181" "ENSG00000168386" "ENSG00000112902" "ENSG00000197594"...)
- Number of DEGs specific to naive CD4+ cells: 918 ("ENSG00000187608" "ENSG00000157330" "ENSG00000171729" "ENSG00000142621" "ENSG00000237276"...)
- Number of DEGs specific to memory CD4+ cells: 4 ("ENSG00000169429" "ENSG00000107249" "ENSG00000138347" "ENSG00000196917")

The lack of clear distinction of samples by treatment could be due to the fact that only 25 genes are DE in Memory cells, whereas in Naive CD4 cells, that count is 939

## Single cell RNA-Seq data analysis using Python
