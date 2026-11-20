# scRNA-seq-with-Scanpy

# GSE150728: Single-Cell RNA-seq Analysis Using Scanpy and DESeq2

## Overview

This repository provides an end-to-end Python workflow for single-cell RNA sequencing (scRNA-seq) analysis using an existing AnnData (`.h5ad`) dataset from **GSE150728**, investigating the peripheral immune response associated with COVID-19.

The pipeline uses Scanpy, AnnData, Python and DESeq2 to perform quality control, data preprocessing, dimensionality reduction, cell clustering, marker gene identification and sample-level pseudobulk differential gene expression analysis.

The workflow is designed to run in Google Colab without requiring FASTQ files or a separate alignment workflow.

## Dataset Information

| Parameter               | Description                                      |
| ----------------------- | ------------------------------------------------ |
| GEO Accession           | GSE150728                                        |
| Study                   | Single-cell analysis of COVID-19 immune response |
| Organism                | Homo sapiens                                     |
| Data format             | AnnData (.h5ad)                                  |
| Analysis framework      | Scanpy                                           |
| Differential expression | DESeq2                                           |
| Programming language    | Python and R                                     |
| Environment             | Google Colab                                     |

### Dataset Download

**CELLxGENE processed AnnData dataset:**

[https://datasets.cellxgene.cziscience.com/8d7fad46-2f37-4aa0-ac9d-babbc36efb6f.h5ad](https://datasets.cellxgene.cziscience.com/8d7fad46-2f37-4aa0-ac9d-babbc36efb6f.h5ad)

**NCBI GEO record:**

[https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE150728](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE150728)

Note: Verify the dataset accession, sample annotations and file provenance before interpreting results. The CELLxGENE file is processed data and may not contain all original raw-count information.

## Workflow

```
GSE150728 AnnData Dataset (.h5ad)
             |
             v
     Data Loading and Inspection
             |
             v
       Quality Control (QC)
             |
             v
     Cell and Gene Filtering
             |
             v
     Normalization and log1p
             |
             v
   Highly Variable Gene Selection
             |
             v
       PCA Dimensionality
             |
             v
       UMAP Visualization
             |
             v
       Leiden Clustering
             |
             v
      Marker Gene Analysis
             |
             v
       Cell-Type Annotation
             |
             v
       Pseudobulk Aggregation
             |
             v
       DESeq2 Differential
        Expression Analysis
             |
             v
        DEG CSV Export
```

## Technologies Used

- Python
- Scanpy
- AnnData
- NumPy
- Pandas
- SciPy
- Matplotlib
- Leiden
- R
- DESeq2
- Google Colab

## Repository Structure

```
GSE150728-scRNAseq-Analysis/
│
├── README.md
│
├── notebooks/
│   └── GSE150728_scRNAseq_Analysis.ipynb
│
├── scripts/
│   ├── 01_data_loading.py
│   ├── 02_quality_control.py
│   ├── 03_normalization.py
│   ├── 04_HVG_selection.py
│   ├── 05_PCA_UMAP.py
│   ├── 06_Leiden_clustering.py
│   ├── 07_marker_gene_analysis.py
│   ├── 08_pseudobulk.py
│   └── 09_DESeq2_DEG.R
│
├── data/
│   └── README.md
│
├── results/
│   ├── QC/
│   ├── UMAP/
│   ├── Clustering/
│   ├── Marker_Genes/
│   └── DEG/
│
└── requirements.txt
```

## Installation

Install the required Python packages:

```
pip install scanpy anndata leidenalg igraph
pip install numpy pandas scipy matplotlib
```

For differential expression analysis, install R and DESeq2:

```
if (!requireNamespace("BiocManager", quietly=TRUE)) {
    install.packages("BiocManager")
}

BiocManager::install("DESeq2")
```

## Running the Pipeline

### Option 1: Google Colab

1. Open Google Colab.
2. Install the required packages.
3. Download the GSE150728 `.h5ad` dataset.
4. Load the AnnData file using Scanpy.
5. Inspect the expression matrix and metadata.
6. Run preprocessing and clustering.
7. Identify marker genes.
8. Aggregate raw counts by biological sample and cell type.
9. Run DESeq2.
10. Export DEG results as CSV files.

### Option 2: Local Python Environment

Clone the repository:

```
git clone https://github.com/YOUR_USERNAME/GSE150728-scRNAseq-Analysis.git

cd GSE150728-scRNAseq-Analysis


## Major Analysis Modules

### 1. Data Loading and Inspection

- Import AnnData `.h5ad` files.
- Inspect expression matrices.
- Identify cell metadata and gene annotations.
- Determine raw-count availability.

### 2. Quality Control

- Genes detected per cell.
- Total counts per cell.
- Mitochondrial gene percentage.
- Low-quality cell filtering.

### 3. Normalization and Feature Selection

- Library-size normalization.
- Log1p transformation.
- Highly variable gene selection.

Raw counts must be preserved for pseudobulk differential expression.

### 4. Dimensionality Reduction

- Principal Component Analysis (PCA).
- Nearest-neighbour graph construction.
- Uniform Manifold Approximation and Projection (UMAP).

### 5. Cell Clustering

- Leiden clustering.
- Cluster visualization.
- Cluster-specific marker identification.

### 6. Cell-Type Annotation

Representative marker genes:

| Cell Type       | Marker Genes       |
| --------------- | ------------------ |
| T cells         | CD3D, CD3E, CD3G   |
| B cells         | CD79A, MS4A1, CD19 |
| Monocytes       | LST1, LYZ, S100A8  |
| NK cells        | NKG7, GNLY, KLRD1  |
| Dendritic cells | FCER1A, CD1C       |
| Plasma cells    | MZB1, JCHAIN, SDC1 |

### 7. Differential Gene Expression

Pseudobulk count aggregation is performed at the biological sample and cell-type level.

DESeq2 is used to identify differentially expressed genes between COVID-19 and control samples.

Criteria for significant DEGs:

- Adjusted p-value < 0.05
- Absolute log2 fold change ≥ 1

Comparisons require sufficient independent biological replicates. Matched samples require an appropriate paired statistical design.

## Expected Results

| Output File                    | Description                    |
| ------------------------------ | ------------------------------ |
| QC_Metrics.csv                 | Quality-control measurements   |
| QC_Violin.png                  | QC visualization               |
| Highly_Variable_Genes.csv      | Selected variable genes        |
| PCA_Variance.png               | PCA variance                   |
| UMAP.png                       | UMAP visualization             |
| UMAP_Clusters.png              | Leiden clusters                |
| Cluster_Marker_Genes.csv       | Cluster-specific markers       |
| Pseudobulk_Counts.csv          | Aggregated gene counts         |
| Pseudobulk_Sample_Metadata.csv | Sample-level metadata          |
| DEG\_\*.csv                    | Cell-type-specific DEG results |
| Final_Analysis.h5ad            | Processed AnnData dataset      |

## Important Considerations

- The original raw-count matrix must be identified before count-based analysis.
- Do not normalize already normalized or log-transformed expression data again.
- Use biological samples as independent replicates for disease comparisons.
- Cell-level Wilcoxon testing is exploratory and should not replace sample-level pseudobulk analysis.
- Cell-type annotations should be validated using multiple marker genes.
- Batch effects and clinical covariates should be considered when integrating samples.
- Do not interpret missing DEG results as evidence of no biological differences.

## Applications

This workflow can be adapted for:

- COVID-19 immune response research.
- Single-cell biomarker discovery.
- Immune-cell population characterization.
- Differential gene expression analysis.
- Cell-type-specific transcriptomic signatures.
- Comparative disease and control studies.

## Future Enhancements

- Harmony-based multi-dataset integration.
- Automated cell-type annotation.
- Gene Ontology and KEGG enrichment.
- Cell-cell communication analysis.
- Pseudotime trajectory analysis.
- Machine learning-based biomarker identification.
- Interactive UMAP visualization.

## References

1. GSE150728 – NCBI Gene Expression Omnibus: [https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE150728](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE150728)
2. Scanpy documentation: [https://scanpy.readthedocs.io/](https://scanpy.readthedocs.io/)
3. AnnData documentation: [https://anndata.readthedocs.io/](https://anndata.readthedocs.io/)
4. DESeq2 documentation: [https://bioconductor.org/packages/DESeq2/](https://bioconductor.org/packages/DESeq2/)

## License

This repository is intended for academic and research purposes. Check the original dataset's usage terms and applicable licensing before redistribution.

## Author

Developed as a computational bioinformatics workflow for single-cell transcriptomic analysis using Python, Scanpy and DESeq2.


📺 Future Omics — Bioinformatics Made Easy https://www.youtube.com/@Bioinformatics_Made_Easy?sub_confirmation=1

The channel focuses on simplified bioinformatics and omics tutorials for students, researchers, and life-science learners.

<img width="1536" height="1024" alt="Single-Cell RNA-seq Analysis Infographic" src="https://github.com/user-attachments/assets/01e50001-7414-4140-a899-14f451824372" />

