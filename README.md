## Introduction to Statistical Learning 
### Final Project
### Fall 2020
### Greg Procario & Dominic DiSanto

# SepticShockPrediction
Using MIMIC-III data, can we predict septic shock events among patients with a positive diagnosis of sepsis or septicemia? A final project for Introduction to Statistical Learning

More information related to MIMIC-III data can be found at the [data set's website](https://mimic.physionet.org/) and [accompanying publication](https://www.nature.com/articles/sdata201635)  
  
  
*The use of the word "codes" in this document corresponds to ICD-9 diagnoses codes*

The files in this respository (as of 9/21/2020) are:

**`cohort_train.csv`** - Eighty percent of our analytic cohort, isolated as our training set on which we will perform 10-fold cross validation to tune our SVM cost parameter (using a linear basis function to begin) 

**`cohort_test.csv`** - A hold-out set of 20% of our analytic cohort on which we will assess test error following model fit on our training data

**`FinalProj_DataCleaning_EDA.RMD`** - An RMarkdown document including data cleaning and (very) brief exploratory data analysis. This code includes sparing annotations related to the data cleaning as well, including decisions made in subsetting our cohort to those with a sepsis/septicemia diagnosis and isolating stays of interest from patients with multiple stays


> Within these data frames, the variables `SUBJECT_ID` and `HADM_ID` correspond to patient and hospital stay identifiers respectively. Our cohort has been isolated to specific patient-stay combinations, such that either of these identifier variables uniquely identifies an observation. All other columns correspond to covariates/predictors of interest. Columns are described below with details of how conditions/variables were derived included in the data cleaning RMarkdown file. 

Definitions for ICD-9 codes most commonly accessed via [ICD-9 Data](https://www.icd9data.com)


|Variable|Description|
|---|---|
|`SUBJECT_ID` | Patient identifier (int)|
|`HADM_ID` | Stay identifier (int)|
|`GENDER` | 1=Male; 0=Female|
|`TraumaDiag` | 1=Admitted for trauma (injury, range of ICD codes from 800.00-959.99)|
|`CVD` | 1 if present (codes beginning with 410, 414, 416; specific codes 412, 429.2), otherwise 0 for absent |
|`DMII` | 1 if present (code of 250.00), otherwise 0 for absent|
|`HT` | 1 if present (codes of 401.1, 401.9), otherwise 0 for absent|
|`Sub_Abuse` | 1 if present (`Alc` or `Drug` present/equal to 1, see below table for definitions of `Alc` and `Drug`), otherwise 0 for absent|
|`RespDx` | Corresponds to COPD & diagnoses referred to as "allied conditions to COPD" 1 if present (Codes between 490.00 and 496.99 [inclusive]), otherwise 0 for absent|
|`Age` | Integer, value was floored (such that a person of 21 years, 51 weeks was considereed 21 years old)*|
|`SepticShock` | 1 if present (code of 785.52), otherwise 0 for absent|
|`ImmunoCompr` | 1 if present (present if any of the following were present: `ImmuneDisorder`, ,`HIV_AIDS`, `MalignantNP`, `CF`), otherwise 0 for absent|  

**Patients born prior to 1900 have age set to 300 by default in MIMIC data. These patients were set to age '90' during data cleaning, which is 1 greater than the maximum (true) age of MIMIC patients*

**Additional variables included only in Unedited CSV Files**
|Variable|Description|
|---|---|
|`Alc` | 1 if present (code between 303.00 to 304.99 or 291 to 291.99), otherwise 0 for absent|
|`Drug` | 1 if present (codes from 304-305.99 or 292-292.99), otherwise 0 for absent|
|`ImmuneDisorder` | Describes general but unspecificed immunocompromisation. 1 if present (codes from 279.00 to 279.99), otherwise 0 for absent|
|`HIV_AIDS` | 1 if present (codes of 042, V08, 07953, 795.71), otherwise 0 for absent|
|`CF` | 1 if present (code from 277.00 to 277.1), otherwise 0 for absent|
|`MalignantNP` | 1 if present (codes from 140-209.99 ), otherwise 0 for absent|
|`Sepsis` | 1 if present (codes of 955.91, 955.92), otherwise 0 for absent|
|`Septicemia` | 1 if present (codes from 038.00 to 038.99), otherwise 0 for absent|
|`InclusionDiag`	 | Tag for if patients were included in analytic cohort. 1=Yes (if Sepsis or Septicemia were present/==1); 0=No (Both Sepsis & Septicemia absent/==0|

