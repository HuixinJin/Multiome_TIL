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
The scATAC analysis using ArchR package in R is in: [scATAC/scATAC_analysis_ArchR.Rmd](https://github.com/HuixinJin/Multiome_TIL/edit/main/scATAC/scATAC_analysis_ArchR.Rmd)
- Build ArchR project and filter doublets
- Choose parameters for _addIterativeLSI()_ function based on Silhouette score
- Cluster based on iterative LSI result and choose resolution using Silhouette score
- Add UMAP and extract marker features for each cluster
- Add imupated gene scores and visualize on UMAP
- Call peaks in dataset using _addReproduciblePeakSet()_ and get marker peaks for each cluster
- Add motif matrix and identify enriched motifs in each cluster
- Visualize interested motifs on UMAP
- Add co-accessiblity between peaks and _plotBrowserTrack()_ shows the connection

## Multiomic Integration
### SCENIC+
The SCENIC+ analysis integrated scRNA and scATAC data in Python is in: [multiomic/SCENIC+_pipeline.ipynb](https://github.com/HuixinJin/Multiome_TIL/edit/main/multiomic/SCENIC+_pipeline.ipynb)
After scRNA preprocessing, scATAC also need to be processed via pycisTopic package.
- Generate pseudobulk ATAC-seq profiles
- Call peaks and generate a consensus peak set
- Quality control and filter on ATAC data
- Build cisTopic object
- Topic modelling using the optimal topics number 
- Provide another dimension reduction view of scATAC based on topic model
- Calculate differential accessible regions (DARs) and infer candidate enhancer regions
- Use pycistarget to do the motif enrichment analysis
- Create summarized SCENIC+ object for further analysis
- Dot-heatmap to show the correlation between TF expression and either target region accessibility or target gene expression
  - Calculate the region specificity scores of TFs
  - Get top eRegulons in each cluster
  - Use modified dot-heatmap plot function to visualize
### ArchR
ArchR provides a way to integrate scRNA expression matrix with scATAC data and further multiomic analysis.
The ArchR integration process in R is in: [multiomic/ArchR1-integrate-scRNA-into-object.Rmd](https://github.com/HuixinJin/Multiome_TIL/edit/main/multiomic/ArchR1-integrate-scRNA-into-object.Rmd)
- Import ArchR project and scRNA matrix
- Add scRNA data into ArchR object via _addGeneExpressionMatrix()_
- Iterative LSI dimension reduction on scATAC and scRNA and the two combined
- Add UMAP based on different dimension reduction results
- Cluster with optimal parameters
- Additional further analysis:
  - Build peak to gene linkages
  - Identify deviant TF motifs and the correlation between motifs and TF expression
  - Identify positive TF regulators

## Trajectory analysis
### ArchR
The trajectory built on the low dimension space can be colored either by predicted gene score or gene expression value. The code in R is in: [multiomic/ArchR2-trajectory-analysis-and-subclone.Rmd](https://github.com/HuixinJin/Multiome_TIL/edit/main/multiomic/ArchR2-trajectory-analysis-and-subclone.Rmd)
- Import another interested UMAP as a new choice for low dimension space
- Set the interested trajectory 
- Add the trajectory via _addTrajectory()_ function
- Visualize the trajectory on chosen UMAP and colored by either Gene score predicted from region accessiblity or the integrated scRNA expression value
- Visualize the changes also in heatmap style by _plotTrajectoryHeatmap()_
- Extract cells in specific clones to generate the gene fluctuation along trajectory
### SCENIC+
The trajectory analysis by SCENIC+ in Python is in: [multiomic/eR_influence_along_trajectory.ipynb](https://github.com/HuixinJin/Multiome_TIL/edit/main/multiomic/eR_influence_along_trajectory.ipynb)
- Create a anndata object combining the eRegulon AUC values in SCENIC+ object and scRNA data
- Add eRegulon-based UMAP and eRegulon-based cell type annotation to the anndata object
- Use PAGA to infer the trajectory
- Calculate pseudotime using eRegulon enrichment matrix
  - Some comparison with pseudotime computed based on multi-sample scRNA data
- Set paths of interest and calculate path matrices
- Generate eRegulon enrichment level along trajectories and plot on UMAP and curve plot

