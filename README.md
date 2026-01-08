# Differential-Gene-Expression-Analysis
Differential Gene Expression Analysis of Bulk and Single Cell RNA Data Using R and Python

- Analysed and identified cytokine-driven distinct Th0 vs Th2 gene expression in Naive CD4+ T cells and Memory CD4+ T cells in a paper published by Cano-Gamez et al, 2020.
- Used DESeq2 in R for bulk RNA-seq data analysis and annData and ScyPy in Python for single cell RNA Seq data analysis.

## Bulk RNA-Seq data analysis using R
![pheno_matrix](images/pheno_mat_all.png)
![pheno_matrix_subset](images/pheno_mat_subset.png)
![ge_matrix_subset](images/ge_mat_subset.png)
![pca sequencing batch](images/pca_sequencing_batch.png)
![scree plot](images/scree_plot.png)
![result table](images/result_table.png)
![vocano_plot](images/vocano_plot.png)
![heatmap](images/heatmap.png)

the count matrix (ge_matrix) originally has 58051 rows (genes) and 94 columns (samples/ cells)
the metadata (pheno_matrix) has 94 rows (corresponding to the cells) and 10 columns (information about the sample)

we have 9 sample/ cell that satisfy our condition of
- simulation time = 5 days
- cytokine condition = Th0 or Th2
- cell tupe = CD4 Memory cell
therefore our ge_matrix is now 58051 x 9 and our pheno_matrix is now 9 x 10
that is, we are taking all the genes, but only those cells that meet our criteria

we remove genes who have < 10 reads across all samples. 
after removing these genes, we have 26656 genes. 
that is, our ge_matrix is now 26656 x 9 and our pheno_matrix is 9 x 10 (same)

For Memory cells, we see that, when plotting sample groups by treatment, we dont see a clear distinction (visible for Naive CD4 cells)
This could be due to the following reasons:
- Cytokine effect is subtle: PCA is affected by some other stronger factor
- Cytokine affects a limited gene set: out of 26k genes, only a few genes respond
- Cytokine effect lies beyond PC1–PC2

the outcome of the analysis is that we have 25 differentially expressed genes in CD4 Memory cells.

## Single cell RNA-Seq data analysis using Python
