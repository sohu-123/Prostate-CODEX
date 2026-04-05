# Prostate CODEX Spatial Analysis Pipeline

## Overview
This repository contains the complete analysis pipeline for prostate cancer CODEX imaging data. The workflow encompasses cell detection and segmentation, cellular neighborhood analysis, phenotypic characterization, and spatial visualization.

## Initial Data Generation

### Cell Detection and Segmentation
Hematoxylin & Eosin (H&E) stained images were subjected to cell detection and segmentation using the **StarDist** algorithm within the **QuPath** platform. StarDist is a deep learning-based method designed for robust, instance-aware detection of nuclei and cells in microscopy images. The StarDist algorithm generates precise cell boundary masks and coordinates, which serve as the foundation for all downstream analyses.

---

## Part 2: Discovery Cohort - Cellular Neighborhood (CN) Definition

### Functionality Summary
Part 2 contains the Python-based computational pipeline for defining cellular neighborhoods (CNs) based on cell-type composition. The workflow performs unsupervised clustering on spatially-localized cell assemblies to identify recurrent patterns of cell-type co-occurrence. Raw cell-type classifications are processed to compute neighborhood profiles using a radius-based approach (r=50 micrometers). K-means clustering (k=40 clusters) groups neighborhoods by their cellular composition patterns, creating a set of 40 distinct cellular neighborhood archetypes. Each CN is annotated with functional marker combinations (e.g., CN1_HLA_DR, CN7_Fibro_HII, CN14_ImmMix_Tumor), providing biologically meaningful labels. The resulting CN assignments are exported to CSV format and serve as input for all subsequent phenotypic and tissue purity analyses.

---

## Part 3: Discovery Cohort - TP/SP/TPL Classification and Analysis

### Functionality Summary
Part 3 performs comprehensive phenotypic classification and tissue-level annotation of the discovery cohort. Three key phenotypes are defined: Tumor-rich Regions (TP), Stroma-rich Regions (SP), and Tumor Purity Levels (TPL). The workflow loads cellular neighborhood data from Part 2 and performs hierarchical clustering of CN composition profiles to stratify tissue regions into phenotypic categories. For tumor purity assessment, tissue regions are classified into three levels (High/Medium/Low) based on CN-derived immune infiltration metrics. The analysis integrates clinical metadata (Gleason scores, treatment information) with spatial phenotypes through merging operations. Multiple statistical tests (Fisher's exact test with FDR correction) assess associations between phenotypes and clinical variables. Results are systematically saved to CSV files including phenotype annotations, contingency tables, p-values, and odds ratios for publication-ready figures.

---

## Part 5: Additional Figures - Tissue Composition and Clinical Association Analysis

### Functionality Summary
Part 5 generates supplementary figures and performs statistical association analyses between tissue phenotypes and clinical variables. The workflow hierarchically clusters tissue samples based on cell-type composition using distance metrics and dendrograms, stratifying samples into treatment-response or disease-progression groups. Interactive visualizations are created using ggplot2 and ComplexHeatmap, displaying cell-type proportions across tissue regions with custom color schemes. Multiple cross-tabulation analyses test associations between Gleason grades, androgen deprivation therapy (ADT) treatment status, and cellular neighborhood composition using Fisher's exact tests with Benjamini-Hochberg FDR correction. Volcano plots and contingency table heatmaps visualize statistical significance and effect sizes. Clinical metadata are matched to tissue regions by sample identifiers, enabling comprehensive phenotype-to-clinical-outcome mapping for figures in the main paper and supplementary materials.

---

## Part 12: Spatial Voronoi Diagram Visualization

### Functionality Summary
Part 12 implements spatial Voronoi tessellation-based visualization of cellular neighborhoods in tissue regions. The Python pipeline loads cell coordinates and CN assignments for each tissue region, then constructs a Voronoi diagram partitioning the tissue space based on individual cell positions. Finite polygons are computed by intersecting infinite Voronoi regions with the tissue convex hull to ensure realistic spatial boundaries. Each Voronoi polygon is colored according to its corresponding CN type using a consistent color palette (16 distinct colors assigned to major CN archetypes). Cell-type labels (A-T codes) are positioned at polygon centroids for spatial reference. The visualization pipeline generates high-resolution TIF images for each tissue region, displaying CN distribution patterns in anatomically-accurate spatial arrangement. This enables visual assessment of CN clustering and segregation within tissue architecture, facilitating identification of recurring spatial organization patterns in the tumor microenvironment.

---

## Directory Structure

```
Prostate-CODEX/
├── Part_2_discovery_cohort_CN_definition/
│   └── calculate_cn.py                    # CN definition pipeline
├── Part 3 discovery cohort TP SP TPL/
│   ├── 01_SP_TP_definition.R              # TP/SP/TPL classification
│   ├── 02_TPL_tumor_purity_level.R        # Tumor purity level analysis
│   ├── 02_TPL_tumor_purity_level.ipynb    # Interactive TPL analysis
│   ├── 03_TP SP TPL related figures.R     # Visualization scripts
│   ├── SPTP.ipynb                         # TP/SP analysis notebook
│   └── *.csv                              # Phenotype annotations and results
├── Part 5 other figures in Fig1-4 Fig S1-6/
│   ├── other_figure.ipynb                 # Cell-type composition analysis
│   ├── 10-OtherFigures-2-RCPC.ipynb       # CRPC cohort figures
│   ├── 10-OtherFigres-2.ipynb             # Additional figures
│   ├── drawPicture.ipynb                  # Figure generation
│   ├── feature_phyno/                     # Feature phenotype data
│   └── *.txt, *.csv                       # Data files and labels
└── Part12-Voronoi/
    ├── code_SP.py                         # SP Voronoi visualization
    ├── code_TP.py                         # TP Voronoi visualization
    ├── code_celltype.py                   # Cell-type Voronoi
    └── Code_celltype.ipynb                # Interactive Voronoi
```

## Computational Environment

### R Environment (Part 3, Part 5)
- **R Version**: 3.6.3 / 4.x
- **Key Libraries**: tidyverse, Seurat, pheatmap, ggplot2, ComplexHeatmap, survival, dendextend

### Python Environment (Part 2, Part 12)
- **Python Version**: 3.9+
- **Key Libraries**: pandas, numpy, scipy, scikit-learn, shapely, matplotlib, seaborn

## Key Outputs

- **CN Assignments**: Cellular neighborhood classification for all cells
- **Phenotype Annotations**: TP/SP/TPL labels for tissue regions
- **Statistical Results**: Fisher's exact test results with p-values and FDR-corrected q-values
- **Visualization Files**: Heatmaps, volcano plots, Voronoi diagrams
- **Clinical Correlations**: Association tests between spatial phenotypes and clinical variables

## Usage Notes

1. Ensure all data paths in scripts point to correct input files
2. Run analyses sequentially: Part 2 → Part 3 → Part 5 / Part 12
3. Check output CSV files for intermediate results before proceeding to next step
4. Adjust clustering parameters (k values, distance metrics) as needed for your dataset
5. Customize color palettes in visualization scripts for publication-ready figures

---

**Last Updated**: April 2026  
**Analysis Framework**: Multi-modal spatial transcriptomics with CODEX imaging  
**Primary Applications**: Prostate cancer microenvironment characterization and treatment response prediction
