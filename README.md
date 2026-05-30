# PRT564-Assessment-4-Sydney-Group-8
The repository contains the Python script for Assessment 4 PRT564

**Note:** An error has been observed with the code files. If the error persists, refer to the GitHub Link file for the backup link.

## Project Overview

This code package supports the PRT564 Assessment 4 group project. The project applies classification analysis to Australian labour-market, education and migrant income data using Australian Bureau of Statistics (ABS) datasets.

The analysis is based on two classification research questions:

**RQ1:** Can an individual’s highest non-school qualification relevance to their current job be predicted using demographic, educational, employment, and occupation-income characteristics?

**RQ2:** Can migrant employee subgroups be classified into above- or below-sample-median employee income groups using demographic, migration, and visa-related characteristics?

The workflow is organised into three main stages:

1. **Part 1 – Data Enrichment and Feature Engineering** Minh Nguyet Tran 
2. **Part 2 – Exploratory Data Analysis and Visualisation** Minh Nguyet Tran 
3. **Part 3 – Classification Modelling and Evaluation** Seema
4. **Part 4 – Model Evaluation and Discussion** Md Muhibul Azim

---

## Required Software and Libraries

The code was written in Python and requires the following packages:

```bash
pip install pandas numpy matplotlib scipy scikit-learn openpyxl jupyter
```

Recommended environment:

- Python 3.10 or later
- Jupyter Notebook for Part 3, if using the `.ipynb` file
- Excel-compatible files saved locally on the user’s computer

---

## Input Data Files

The project uses ABS Excel data cubes. The file paths in the code are written for a Windows computer and may need to be changed if the files are stored in a different folder.

Expected input files:

```text
C:\Users\ASUS\Downloads\QAW2223DC (2).xlsx
C:\Users\ASUS\Downloads\Table 4 - Employment income, earners and summary statistics by industry and occupation of main job, 2021-22 to 2022-23.xlsx
C:\Users\ASUS\Downloads\Table 12 - Migrants, Employee income by arrival group, 2018-19 to 2022-23.xlsx
```

The first file is used for ABS Qualifications and Work data. The second file is used to add occupation-income features. The third file is used to create the migrant income classification dataset for RQ2.

---

## Part 1 – Data Enrichment and Feature Engineering

### Main purpose

Part 1 prepares the cleaned datasets required for later EDA and classification modelling.

### Main processing steps

For **RQ1**, the code:

- Loads QAW Table 10 as the base table for qualification relevance.
- Uses the ABS `Relevant` and `Not relevant` categories as the target variable.
- Expands ABS estimate values into representative records because the public ABS data is aggregated, not individual-level microdata.
- Adds extra predictors from QAW Table 6 and QAW Table 11 using class-based weighted sampling.
- Merges occupation-income variables from ABS Personal Income Table 4.2.
- Creates derived occupation-income features such as income level and income rank.

For **RQ2**, the code:

- Loads ABS Personal Income Tables 12.1, 12.2 and 12.3.
- Combines migrant income records by arrival group.
- Keeps relevant predictors such as visa group, age range, applicant status, sex and arrival group.
- Creates an `income_target` variable based on whether each subgroup is above or below the sample median employee income.
- Removes income columns used to create the target to avoid data leakage.

### Main outputs

The final cleaned datasets are saved as CSV files:

```text
outputs_prt564_part1_v2/cleaned_data/rq1_qualification_relevance_part1_dataset_v2.csv
outputs_prt564_part1_v2/cleaned_data/rq2_migrant_income_part1_dataset_v2.csv
```

These files should be used as the input files for Part 2 and Part 3.

---

## Part 2 – Exploratory Data Analysis and Visualisation

### Main purpose

Part 2 explores the cleaned datasets and checks whether meaningful patterns exist between predictors and target variables before modelling.

### Main analyses performed

The EDA code produces:

- Target distribution charts for RQ1 and RQ2.
- Stacked percentage charts for categorical predictors.
- Cramer’s V association ranking for categorical variables.
- Chi-square test results for categorical relationships.
- Boxplots for selected numerical predictors.
- Mann-Whitney U test results for numerical group differences.

### Key findings supported by the EDA

For **RQ1**, the most important patterns are linked to:

- Current job skill level
- Occupation
- Occupation-income level
- Employment status

For **RQ2**, the strongest patterns are linked to:

- Age range
- Visa group
- Arrival group
- Sex

### Main outputs

Part 2 creates PNG visualisations in the output folder. The final combined figures used in the report are:

```text
figure_2_1_target_distributions.png
figure_2_2_rq1_main_categorical_predictors.png
figure_2_3_rq1_association_numeric_evidence.png
figure_2_4_rq2_main_categorical_predictors.png
figure_2_5_rq2_association_age_midpoint.png
```

These figures are used to support the Part 2 EDA discussion in the report.

---

## Part 3 – Classification Modelling and Evaluation

### Main purpose

Part 3 builds classification models for the two research questions and compares their predictive performance.

### Models used

For **RQ1**, the models are:

- Random Forest
- Support Vector Machine with RBF kernel

For **RQ2**, the models are:

- Decision Tree
- Gaussian Naive Bayes

### Main modelling steps

The modelling code includes:

- Loading the cleaned Part 1 CSV files.
- Separating features and target variables.
- Splitting data into training and testing sets using stratified sampling.
- Applying preprocessing through pipelines.
- Using one-hot encoding for categorical predictors.
- Applying scaling where needed, especially for SVM.
- Using GridSearchCV for hyperparameter tuning.
- Evaluating model performance using accuracy, balanced accuracy, precision, recall, F1-score, macro F1-score, confusion matrices and cross-validation.

### Important interpretation notes

For **RQ1**, overall accuracy is high, but the dataset is imbalanced. Therefore, balanced accuracy, macro F1-score and minority-class recall should be considered alongside accuracy.

For **RQ2**, the target classes are balanced. Decision Tree achieved the strongest held-out test accuracy, while Gaussian Naive Bayes also produced competitive results.

---

## Running the Code

### Step 1: Check file paths

Before running the code, make sure the ABS Excel files are saved in the correct folder. If the files are not in `C:\Users\ASUS\Downloads`, update the file paths in the script.

### Step 2: Run Part 1

Run the Part 1 Python script first to generate the cleaned CSV datasets.

Example command:

```bash
python "C:\Users\ASUS\Downloads\prt564_part1_data_enrichment_feature_engineering_only.py"
```

### Step 3: Run Part 2

Run the Part 2 EDA script after the Part 1 CSV files have been created.

Example command:

```bash
python "C:\Users\ASUS\Downloads\part2_eda_visualisation.py"
```

### Step 4: Run Part 3

Open the Part 3 notebook in Jupyter Notebook and run the cells from top to bottom.

Example command:

```bash
jupyter notebook
```

Then open the Part 3 `.ipynb` file and run all cells.

---

## Output Folders

Recommended folder structure:

```text
outputs_prt564_part1_v2/
│
└── cleaned_data/
    ├── rq1_qualification_relevance_part1_dataset_v2.csv
    └── rq2_migrant_income_part1_dataset_v2.csv

outputs_prt564_part2_eda/
│
└── plots/
    ├── figure_2_1_target_distributions.png
    ├── figure_2_2_rq1_main_categorical_predictors.png
    ├── figure_2_3_rq1_association_numeric_evidence.png
    ├── figure_2_4_rq2_main_categorical_predictors.png
    └── figure_2_5_rq2_association_age_midpoint.png
```

---

## Data and Method Notes

- The RQ1 dataset is based on representative synthetic records created from ABS aggregated estimates.
- The synthetic RQ1 records are not actual individuals.
- Weighted sampling is used only to approximate ABS published distributions where full row-level combinations are unavailable.
- The RQ2 target variable is created using the sample median of selected migrant subgroup median employee incomes.
- Income columns used to create the RQ2 target are removed before modelling to reduce data leakage risk.
- Encoding and scaling are applied during modelling rather than in the Part 1 output files.

---

## Troubleshooting

### FileNotFoundError

Check that the ABS Excel files are saved in the expected folder and that the file names match exactly.

### Length mismatch when renaming columns

This usually means the number of selected columns does not match the number of column names assigned. Check the column index list used in `read_excel` or `iloc`.

### Python shell error when running a script

If the terminal shows `>>>`, you are inside the Python shell. Type:

```python
exit()
```

Then run the script again from PowerShell or Command Prompt.

### ModuleNotFoundError

Install the missing package using `pip install package_name`.

---

## Author and Assessment Context

This README supports the code files prepared for PRT564 Assessment 4. The code is intended to support data preparation, exploratory analysis and classification modelling for the final group project report.
