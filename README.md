# Multiomic analysis of CD8+ T cells in lung cancer

This is the code archive of master thesis _Gene Regulatory Network Analysis of CD8+ T Cells in Human Lung Cancer Using Multiome Single-cell RNA and ATAC Sequencing Data_.

## scRNA Part
### Seurat analysis
The scRNA analysis using Seurat package in R is in: [scRNA/scRNA_analysis.Rmd](https://github.com/HuixinJin/Multiome_TIL/edit/main/scRNA/scRNA_analysis.Rmd)
- Filter TCR and BCR genes
- Filter cells based on nFeature_RNA and percent.mt
- Use SCT normalization
- Run PCA and UMAP dimension reuctions
- Cluster (using [scRNA/chooseR.R](https://github.com/HuixinJin/Multiome_TIL/edit/main/scRNA/chooseR.R) to select an optimal resolution)
- Annotate cell types to each cluster based on marker genes
### Scanpy analysis
The scanpy analysis of scRNA is the prerequisite of SCENIC+ analysis and the code in Python is in: [scRNA/scRNA_scanpy_preprocess_for_scenicplus.ipynb](https://github.com/HuixinJin/Multiome_TIL/edit/main/scRNA/scRNA_scanpy_preprocess_for_scenicplus.ipynb)
- Filter TCR and BCR genes
- Remove doublets
- Filter genes expressing in less than 3 cells
- Filter cells expressing less than 200 genes
- Filter cells based on feature counts and percent.mt (the same as Seurat)
- Normalization and scale using scanpy functions
- Dimension reduction with PCA and UMAP
- Cluster and annotation based on marker genes

## scATAC Part
The scATAC analysis using ArchR package in R is in: 
