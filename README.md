# LRA - Data analysis and integration

This repository provides an insight into the bioinformatics framework and platform created for standardizing data collection, analysis, and storage to ensure compatibility and reproducibility in our approach across the program. The repo is divided into SQL database side and the analysis side. The following document outlines details of installation, usage of bioinformatics analysis pipeline and results generated from it.

## Overview of Project 5 (Data analysis and integration)

More information about the sql database and the shiny app.

## Prerequisites
**For MacOS**

**For Windows**

## Availability of code and installation

## Example analysis

Mapping and QC of RNAseq data:
```
lgRscript RNAseq.map.R -l map.log -m username@email.com
```

Coverage check:
```
lgRscript RNAseq.coverage.R -l cov.log -m username@email.com
```

Differential analysis:
```
lgRscript diff.glm.RNAseq.R -l diff.log -m username@email.com
```

## Input requirements

### For Shiny App
The first file is the manifest file. This is usually named something like **RNAseq.sample.manifest.txt**. It is a tab delimited file that stores metadata about the experiment including quality scores for all samples.

Every manifest file must have:

A group column to designate experimental groups
A sample column to uniquely name each sample
An include column, to denote samples that should be excluded from analysis
A unique.reads column, as a measure of overall sequencing depth for the experiment
Each column name must be unique and consist solely of alphanumerics, the period and the underscore

The second file is a diff file. This is usually named something like **diff.glm.RNAseq.analysisName.txt**, where analysisName is a very short description of what comparisons were performed. It is also a tab delimited file that stores all the information relevant to running the differential tests. This includes:

The normalized count for each gene or called peak for each sample ending in *.RPKM for RNAseq, or .RPPM for ATACseq
A SYMBOL column for RNAseq projects that contains the the gene names
If this is an ATACseq project, it must have a respective Gene.Name column AND a peak column
A collection of sets of 5 columns describing each group comparison performed, each with a *.sig column
A sigAny column that denotes whether a feature was significantly different in any comparison test
After you upload both of them in their respective field in the top-left of the sidebar, you may click the load button and explore each tab and all of the options available to that tab to see what your data looks like.

## Output description

### Shiny App plots
**BB Plot**
This tab displays a bee swarm plot. This is very similar to a boxplot diagram. When your samples are divided into several groups, you may select any gene or peak (feature) and see what the read depth for each sample within each group looks like.
<img src="https://github.com/MGupta313/LRA/Analysis/Images/bbplot.png" alt="Folders" width="200"/>


**Volcano Plot**
This is a standard plot for bulk sequencing projects like this. It allows you to see the overall “effect size” between the two conditions you are comparing. To start, select a comparison to look at. The x-axis represents the fold change between the two conditions for each feature, and the y-axis represents the significance value of the difference test for each feature. A significant difference here is one where the absolute log2FC is greater than 1 and the adjusted p-value is less than 0.05.

<img src="https://github.com/MGupta313/LRA/Analysis/Images/volcano.png" alt="Folders" width="200"/>


**PCA Plot**
This is a very common machine learning technique to explore multidimensional data. This scatterplot takes either all or a user selected set of features, and compresses all 10k+ genes/peaks into ~10 dimensions, with the dimensions being ordered by “importance” (percent variance). Looking at the first and second principal components is generally enough to see whether different sets of your data segregate or not, whether there might be an outlier to explore further, or whether your data appears to be composed of heterogenous subsets that you were unaware of.

<img src="https://github.com/MGupta313/LRA/Analysis/Images/pca.png" alt="Folders" width="200"/>

What do the ellipses represent?
It is computed using the car::dataEllipse() function. This function superimposes a normal distribution over the scatterplot. In this app, the ellipse drawn is a contour encompassing 90% of that normal distribution.

**Heat Map**
This plot takes all relevant features and all relevant samples and uses hierarchical clustering to visually represent apparent differences between sets of samples.

<img src="https://github.com/MGupta313/LRA/Analysis/Images/heatmap.png" alt="Folders" width="200"/>
