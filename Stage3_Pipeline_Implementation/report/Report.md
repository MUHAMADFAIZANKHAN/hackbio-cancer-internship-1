# Stage 03 Task: HackBio Internship - Cancers

## Title
**Identifying Potential Biomarkers in Breast Cancer Using Differential Expression and Machine Learning Approaches**

### Authors
- Favour Igwezeke<sup>1</sup>
- Jessica Ovabor<sup>2</sup>
- Anarghya Hegde<sup>3</sup>
- Oluwatobiloba Johnson Osedimilehin<sup>4</sup>
- Ogochukwu Nwaigwe<sup>5</sup>
- Muhammad Faizan Khan<sup>6</sup>
- Rokaya Yasser<sup>7</sup>
- Muhammad Faheem Raziq<sup>8</sup>

---

## Contributors Information
| Name | Email | Slack ID |
|---|---|---|
| Favour Igwezeke | beingfave@gmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07KE59TWEP) |
| Jessica Ovabor | ovaborjessica85@gmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07JVPSU917) |
| Anarghya Hegde | anarghyahegde@outlook.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07JM2UDL7R) |
| Oluwatobiloba Johnson Osedimilehin | tobijohnson01@gmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07JP06QDB4) |
| Ogochukwu Nwaigwe | nwaigweogochukwu756@gmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07KP1D2F24) |
| Muhammad Faizan Khan | faizanjeee@hotmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07JJHPBFBL) |
| Rokaya Yasser | rokayayasser727@gmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07KUECLR40) |
| Muhammad Faheem Raziq | faheemraziq1999@gmail.com | [Slack](https://hackbiointern-leo4437.slack.com/team/U07JPMN7EDD) |

---

# Abstract
The study investigates the gene expression profiles of breast cancer using the TCGA-BRCA RNA-Seq dataset, with a focus on upregulated and downregulated genes. The data was processed through normalization and filtering steps to enhance robustness. Differential expression analysis identified 3462 significant genes, which were subjected to enrichment analysis using bioinformatics tools such as `TCGAbiolinks`, `biomaRt`, and `ggplot2`. The analysis identified key molecular pathways involved in breast cancer progression. Furthermore, the study highlights potential biomarkers, categorizing them into upregulated and downregulated genes based on fold change and p-values. Machine learning models, particularly Random Forest, were implemented to predict tumor classification with 100% accuracy. The findings provide insight into the molecular mechanisms of breast cancer and potential diagnostic and therapeutic targets.

## 2. Biomarker Dataset Extraction and Preprocessing

Using RStudio, code was written to query and retrieve TCGA-BRCA RNA-Seq data for breast cancer, with a focus on 20 samples each of “Primary Tumor” and “Solid Tissue Normal”. The chosen samples were retrieved along with the gene expression count matrix which was then stored as a CSV file.

### 2.1 Data Cleaning and Normalization

The data underwent normalization with the `TCGAanalyze_normalization()` function. This normalization was done to account for the differences in the length of each gene. The data was then filtered with the `TCGAanalyze_Filtering()` function. A quantile cut threshold of 0.25 was chosen to remove the genes whose expression levels were under the 25th percentile, as these genes could potentially contribute to noise. This filtration step was to ensure more robust downstream analysis.

**Figures**:
- **Figure 1**: Heatmap of filtered dataset (Green: Low, Black: Intermediate, Red: High expression)
- **Figure 2**: Heatmap clustered by row
- **Figure 3**: Heatmap clustered by column


## 3. Differential Expression Analysis
EdgeR was the pipeline of choice for analysis of the differential expression of genes between the solid tissue normal (STN) and primary tumor (PT) samples in the filtered breast cancer (BRCA) gene expression dataset. The analysis was done with the `TCGAanalyze_DEA()` function. An FDR cut-off of 0.01 and a log2 fold change cut off 2 were chosen. The result of the analysis was a group of 3462 genes with `abs(log2Foldchange >= 2)` and `FDR <= 0.01`. 98 overregulated and 129 underregulated genes sets were then selected based on stringent log2Foldchange thresholds of `>= 6` and `<= -6` for the former and latter respectively.

**Figure 4**: Volcano plot showing downregulated and upregulated genes.

## 4.Potential Biomarkers and Enrichment Analysis

This section performs a comprehensive analysis of gene expression data, focusing on identifying key upregulated and downregulated genes and performing enrichment analysis. The process begins by reading two CSV files, one containing upregulated genes and the other downregulated genes. The Ensembl IDs in these files are then converted to gene symbols using the `biomaRt` package. Afterward, the script identifies the top 5 genes for both upregulated and downregulated categories, saving these results into CSV files. Subsequently, the enrichment analysis is conducted using the `TCGAanalyze_EAcomplete` function. The resulting data is further processed to prepare for visualization by separating the enrichment results into distinct columns for GO terms, FDR values, and gene counts. The top 5 pathways for both upregulated and downregulated genes are selected based on their FDR values. To visualize the results, a lollipop plot is created for each set, where the circle size represents the number of genes involved, and the color intensity corresponds to the FDR. These plots are saved as PNG files, offering a clear graphical summary of the most biologically relevant pathways for both upregulated and downregulated genes.

### 4.1 Top Upregulated Biomarkers

| Gene  | Fold Change | P-Value     | Significance                                                                                             |
|-------|-------------|-------------|---------------------------------------------------------------------------------------------------------|
| CST5  | 10.15       | 4.99E-71    | Member of the cystatin peptide superfamily. Proteases play a role in tumor development, especially cysteine cathepsin.(4) |
| CGA   | 9.84        | 9.27E-67    | Increased in breast cancer tissue and associated with poor prognosis. A candidate for targeted therapy.(5) |
| HTN   | 9.66        | 1.65E-60    | Significantly higher in tumors than normal tissues.(6)                                                   |
| CLEC3A| 9.39        | 2.40E-69    | Correlates with metastatic potential and poor prognosis in breast cancer.(7)                              |

### 4.2 Top Downregulated Biomarkers

| Gene  | Fold Change | P-Value     | Significance                                                                                                      |
|-------|-------------|-------------|------------------------------------------------------------------------------------------------------------------|
| MYL   | -14.13      | 3.17E-105   | Functions as a prognostic marker in breast cancer, associated with immune infiltration.(8)                        |
| XIRP2 | -13.24      | 3.57E-100   | Encodes an actin-binding protein, significantly mutated in breast cancer metastasis.(9)                           |
| MYH   | -13.16      | 3.26E-102   | Part of the base repair pathway, detecting and protecting against oxidative DNA damage. Low expression leads to mutation and cancer.(10) |

## Figures

- **Figure 5**: Upregulated genes enriched pathways.
- **Figure 6**: Downregulated genes enriched pathways.

## 5. Machine Learning Analysis
### 5.1 Dataset Preparation
The dataset was split into 70% training and 30% test data. Labels were assigned as "Normal" or "Tumor" for machine learning classification. 

### 5.2 Feature Selection and Dimensionality Reduction
Using Principal Component Analysis (PCA), features were selected based on the most relevant biomarkers identified during enrichment analysis to reduce dimensionality.

## 6. Model Selection and Training
Random Forest was chosen for its ability to handle high-dimensional data. The model was trained with 500 trees and achieved high performance.

## 7. Model Performance
### 7.1 Confusion Matrix
| Prediction/Reference | Normal | Tumor |
|----------------------|--------|-------|
| **Normal**           | 6      | 0     |
| **Tumor**            | 0      | 6     |

The model achieved:
- **Accuracy**: 100%
- **95% CI**: (0.7354, 1)
- **Kappa**: 1
- **Sensitivity & Specificity**: Both 1

## 8. Feature Importance Analysis
Random Forest feature importance shows which features significantly impact the model's predictions.
