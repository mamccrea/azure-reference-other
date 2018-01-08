---
title: "Fisher Linear Discriminant Analysis | Microsoft Docs"
ms.custom: ""
ms.date: 08/09/2016
ms.reviewer: ""
ms.service: "machine-learning"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: dcaab0b2-59ca-4bec-bb66-79fd23540080
caps.latest.revision: 19
author: "jeannt"
ms.author: "jeannt"
manager: "jhubbard"
---
# Fisher Linear Discriminant Analysis
*Identifies the linear combination of feature variables that can best group data into separate classes*  
  
 Category: [Feature Selection Modules](feature-selection-modules.md)  
  
##  <a name="Remarks"></a> Module Overview  
 You can use the **Fisher Linear Discriminant Analysis** module to create a new feature dataset that captures the combination of features that best separates two or more classes. You provide a label column and set of numerical feature columns as inputs, and the algorithm determines the optimal combination of the input columns that linearly separates each group of data while minimizing the distances within each group.  
  
 This method is often used for dimensionality reduction, because it projects a set of features onto a smaller feature space while preserving the information that discriminates between classes. This not only reduces computational costs for a given classification task, but can help prevent overfitting.  
  
 Linear discriminant analysis is similar to analysis of variance (ANOVA) in that it works by comparing the means of the variables. Like ANOVA, it relies on these assumptions:  
  
-   Predictors are independent  
  
-   The conditional probability density functions of each sample are normally distributed  
  
-   Variances among groups are similar  
  
 The module returns a dataset containing the compact, transformed features, along with a   transformation that you can save and apply to another dataset.  
  
## How to Use Linear Discriminant Analysis  
  
1.  Add your input dataset and check that the input data meets these requirements:  
  
    -   Your data should be as complete as possible. Rows with any missing values will be ignored.  
  
    -   Values are expected to have a normal distribution.  Before using **Fisher Linear Discriminant Analysis**,  review the data for outliers, or test the distribution.  
  
    -   You should have fewer predictors than there are samples.  
  
    -   Remove any non-numeric columns. The algorithm will examine **all** valid numeric columns included in the inputs, and return an error if invalid columns are included.  
  
         Therefore, if you want to exclude a numeric column, you must add a [Select Columns in Dataset](select-columns-in-dataset.md) module before **Fisher Linear Discriminant Analysis**, to create a view that contains only the columns you wish to analyze.  
  
        > [!TIP]
        >  You can rejoin the columns later using [Add Columns](add-columns.md). The original order of rows is preserved.  
  
2.  Connect the input data to the **Fisher Linear Discriminant Analysis** module.  
  
3.  For **Class labels column**, click **Launch column selector** and choose one label column.  
  
4.  For **Number of feature extractors**, type the number of columns that you want as a result.  
  
     For example, if your dataset contains eight numeric feature columns, you might type `3` to collapse them into a new, reduced feature space of only three columns.  
  
     it is important to understand that the output columns do not correspond exactly to the input columns, but rather represent a compact transformation of the values in the input columns.  
  
     If you use 0 as the value for **Number of feature extractors**, and n columns are used as input, n feature extractors are returned, containing new values representing the n-dimensional feature space.  
  
5.  Run the experiment.  
  
     The algorithm determines the combination of values in the input columns that linearly separates each group of data while minimizing the distances within each group, and creates two outputs:  
  
    -   **Transformed features**. A dataset containing the specified number of feature extractor columns, named **col1**, **col2**, **col3**, and so forth. The output also includes the class or label variable as well.  
  
         You can use this compact set of values for training a model.  
  
    -   **Fisher linear discriminant analysis transformation**. A transformation that you can save and then apply to a dataset that has the same schema. This is useful if you are analyzing many datasets of the same type and want to apply the same feature reduction to each. The dataset that you apply it to should have the same schema.  
  
## Examples  
 To see examples of how this feature selection method is applied, explore these sample experiments in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com):  
  
-   The [Fisher Linear Discriminant Analysis](https://gallery.cortanaintelligence.com/Experiment/Fis-her-Linear-Discriminant-Analysis-and-Apply-Transformation-2) sample demonstrates how to use this module for dimensionality reduction.  
  
##  <a name="Notes"></a> Technical Notes  
 Linear Discriminant Analysis is sometimes abbreviated to LDA, but this is easily confused with *Latent Dirichlet Allocation*. The techniques are completely different, so in this documentation, we will use the full names wherever possible.  
  
-   This method works only on continuous variables, not categorical or ordinal variables.  
  
-   Rows with missing values are ignored when computing the transformation matrix.  
  
-   If you save a transformation from an experiment, the transformations computed from the original experiment are reapplied to each new set of data, and are not recomputed. Therefore, if you want to compute a new feature set for each set of data, use a new instance of **Fisher Linear Discriminant Analysis** for each dataset.  
  
### How It Works  
 The dataset of features is transformed using *eigenvectors*. The eigenvectors for the input dataset are computed based on the provided feature columns, also called a *discrimination matrix*.  
  
 The transformation output by the module contains these eigenvectors, which can be applied to transform another dataset that has the same schema.  
  
 For more information about how the eigenvalues are calculated, see this paper (PDF): [Eigenvector-based Feature Extraction for Classification](http://www.aaai.org/Papers/FLAIRS/2002/FLAIRS02-070.pdf). Tymbal, Puuronen et al.  
  
##  <a name="ExpectedInputs"></a> Expected Inputs  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Dataset|[Data Table](data-table.md)|Input dataset|  
  
##  <a name="parameters"></a> Module Parameters  
  
###  
  
|Name|Type|Range|Optional|Default|Description|  
|----------|----------|-----------|--------------|-------------|-----------------|  
|Class labels column|ColumnSelection||Required|None|Select the column that contains the categorical class labels|  
|Number of feature extractors|Integer|>=0|Required|0|Number of feature extractors to use. If zero, then all feature extractors will be used|  
  
##  <a name="Outputs"></a> Outputs  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Transformed features|[Data Table](data-table.md)|Fisher linear discriminant analysis features transformed to eigen vector space|  
|Fisher linear discriminant analysis transformation|[ITransform interface](itransform-interface.md)|Transformation of Fisher linear discriminant analysis|  
  
##  <a name="exceptions"></a> Exceptions  
  
|Exception|Description|  
|---------------|-----------------|  
|[Error 0001](errors/error-0001.md)|Exception occurs if one or more specified columns of data set couldn't be found.|  
|[Error 0003](errors/error-0003.md)|Exception occurs if one or more of inputs are null or empty.|  
|[Error 0017](errors/error-0017.md)|Exception occurs if one or more specified columns have type unsupported by current module.|  
  
## See Also  
 [Feature Selection](feature-selection-modules.md)   
 [Filter Based Feature Selection](filter-based-feature-selection.md)   
 [Principal Component Analysis](principal-component-analysis.md)