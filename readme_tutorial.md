# Propensity Score Matching for Assessing Academic Mobility Impact: A Step-by-Step Guide using R



## Learning Objectives

- Understand the concept of propensity score matching.
- Implement propensity score matching using R programming language.
- Interpret the results of propensity score matching.
- Apply the technique to assess the impact of academic mobility on key research metrics.

## Description

In this tutorial, we illustrate how propensity score matching using [this method](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/method/) can be used to estimate the effect of a treatment or intervention by accounting for the co-variates that predict the receiving treatment. 
It includes step-by-step instructions, example code, and explanations to facilitate understanding and implementation.

In this example, the propensity score method is employed to estimate the causal effect of academic mobility on researchers' outcomes in terms of: 
- Research productivity
- Received citations 
- Collaboration indicators
  
Academic mobility, represented by the treatment variable *"MOBILE"*, distinguishes between researchers who have experienced academic mobility to other countries *(MOBILE = 1)* as the treatment group and those who have not *(MOBILE = 0)* as a control group. The method aims to balance covariates such as *region, main field of study, international co-authorship, gender, age, and origin country GDP per capita* of the researchers between mobile and non-mobile researchers. By balancing covariates through 'propensity score matching', the method allows for a more accurate assessment of the impact of academic mobility on outcomes mentioned above via indicators:
- CPP (citations per paper)
- PPY (number of papers per year)
- COPP (co-authors per paper).

*Standardized Mean Difference (SMD)* assesses the covariance balance between treatment and control groups before and after matching.  *SMD* is commonly used in *propensity score matching*, with a lower SMD indicating better balance and greater comparability regarding covariates. SMD facilitates comparing effect sizes across different studies or analyses, especially when outcome variables have different scales. In the context of this method, SMD is calculated for both unmatched and matched data to evaluate the balance achieved after matching. The treatment group consists of researchers who have experienced academic mobility to other countries (MOBILE = 1), while the control group consists of researchers who have not experienced academic mobility (MOBILE = 0).

The *Mean Difference* for each variable of interest (CPP, PPY, COPP) quantifies the average difference in these research productivity, received citations, and collaboration indicators between mobile and non-mobile researchers. A positive Mean Difference indicates that mobile researchers, on average, have higher values of the respective metric than non-mobile researchers and vice versa. For example, 
- A *positive Mean Difference* in CPP suggests that mobile researchers receive more citations per paper than non-mobile researchers.
- A *negative Mean Difference* indicates that mobile researchers, on average, have lower values of the respective metric than non-mobile researchers.
Understanding the sign and magnitude of *Mean Differences* provides insights into the direction and magnitude of the impact of academic mobility on research productivity, received citations, and collaboration indicators.

## Target Audience

This tutorial is targeted towards social science researchers, particularly those interested in assessing the impact of academic mobility on research outcomes. 

## Prerequisites Required

Before starting this tutorial, you should have:
- Basic understanding of R programming language.
- Familiarity with statistical concepts such as regression analysis.
- Access to R environment with necessary packages mentioned below.

## Difficulty Level

Normal

## Duration

Approximately 30-45 minutes

## Social Science Relevance

**Social Science Usecase(s):**  
This tutorial addresses the technical challenge in social science research of assessing the impact of academic mobility, a topic of growing importance in the context of globalization and international collaboration in academia.

Setup

## Environment Setup (Installing dependencies) 
Ensure you have R (version 3.6.0 or higher) environment on your local machine.

Install the required R packages by running the following commands:

```R
install.packages("Matching")
install.packages("tableone")
```

[Include a flow diagram here if applicable]

## Input Data (DBD datasets)
This method can work with any dataset containing variables of interest, a treatment indicator, and covariates. For example:
- [The 'Call me sexist but' Dataset (CMSB)](https://search.gesis.org/research_data/SDN-10.7802-2251?doi=10.7802/2251) to Assessing the Impact of Gender Bias in Social Media Posts.


## Step-wise Guide

To utilize the propensity score matching technique for assessing the impact of academic mobility on research productivity, received citations, and collaboration indicators, follow these steps:

### 1. Download Files:
   - Download the following files into a single folder:
     - ["mydata_sample.csv"](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/tutorial/mydata_sample.csv): Input dataset containing the relevant variables for analysis.
     - ["propensity_matching_functions.R"](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/tutorial/propensity_matching_functions.R): R script containing functions for propensity score matching.
     - ["main_script.R"](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/tutorial/main_script.R): R script for executing the analysis.

### 2. Run Commands in "main_script.R":
   - Open "main_script.R" in your R environment.
   - Run the commands sequentially to execute the analysis.

### 3. Load Input Dataset:
   - Execute the following lines to load the input dataset into R as `data_sample.`:
     ```R
      # Get the directory path of the main_script.R
      script_dir <- dirname(rstudioapi::getActiveDocumentContext()$path)
      
      # Example usage:
      data_sample <- read.table(file.path(script_dir, "mydata_sample.csv"), header = TRUE, sep = ",", quote = "\r", dec = ".")
      ```
       
   **Sample Input Data**
   Sample input data can be provided in CSV format with columns representing variables of interest (PPY, COPP, CPP), a treatment indicator (MOBILE), and covariates (REGION, MAIN_FIELD, INTERNATIONAL_COAUTHOR, GENDER, GDP_PC_ORIGIN, AGE). (mydata_sample.csv).
Here is a screenshot of sample input data:
   ![Image Alt Text](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/tutorial/sample_data.PNG)


### 4. Define Functions:
   - Execute this line to define the necessary functions from "propensity_matching_functions.R":
     ```R
     source(file.path(script_dir, "propensity_matching_functions.R"))
     ```

### 5. Define Treatment Variable and Covariates:
   - Define the treatment variable (`treatment_var`) as "MOBILE" and covariates (`covariates`) as "REGION", "MAIN_FIELD", "INTERNATIONAL_COAUTHOR", "GENDER", "GDP_PC_ORIGIN", and "AGE":
     ```R
        # Define variables
         treatment_var <- "MOBILE"  # Specify your treatment variable
         covariates <- c("REGION", "MAIN_FIELD", "INTERNATIONAL_COAUTHOR", "GENDER", "GDP_PC_ORIGIN", "AGE")
     ```

### 6. Perform Propensity Score Matching:
   - Call the `perform_propensity_matching` function with parameters `data_sample`, `treatment_var`, and `covariates` to conduct propensity score matching:
     ```R
        # Perform propensity score matching
         matching_results <- perform_propensity_matching(data = data_sample,
                                                   treatment_var = treatment_var,
                                                   covariates = covariates)
     ```
   - The output includes standardized mean differences (SMDs) of unmatched/matched data (`unmatched_smd` and `matched_smd`):
     ```R
      # Access SMDs of unmatched and matched data
      unmatched_smd <- matching_results$unmatched_smd
      matched_smd <- matching_results$matched_smd
     ```
      and the matched data:
      ```R
     # Matched data
      matched_data <- matching_results$matched_data
      ```

### 7. Define Variables of Interest:
   - Define the variables of interest (`vars_of_interest`) as "PPY", "CPP", and "COPP":
     ```R
      vars_of_interest <- c("PPY", "CPP", "COPP")  # Specify variables for mean difference calculation
     ```
     
### 8. Calculate Mean Differences:
   - Call the `calculate_mean_diff` function in line 33 of "main_script.R" with parameters `matched_data`, `treatment_var`, and `vars_of_interest` to calculate mean differences for the variables of interest.
     ```R
     # Calculate mean differences
      mean_diff <- calculate_mean_diff(data = matched_data, treatment_var = treatment_var, vars_of_interest = vars_of_interest)
     ```
   - The output provides the mean differences of `vars_of_interest`.
     ```R
      # Print mean differences
      print(mean_diff)
     ```
     **Sample Output**
      Sample output includes standardized mean differences (SMD) for unmatched and matched data, as well as mean differences, t-values, and standard errors for variables of interest. 
      
      **Standard Mean Deviation:**
      By examining the SMD for unmatched and matched data under different covariances, we assess the effectiveness of the matching process in achieving balance between the treatment and control groups. A lower SMD indicates a smaller       difference between the two groups. For instance, in this example, the SMD for the variable "AGE" is 0.34 for unmatched data and 0.03 for matched data. This suggests that the treatment group in the matched data is more similar to the control group compared to the unmatched data.
      ![Image Alt Text](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/tutorial/output_SMD.PNG)
      
      
      **Mean difference:**
      Example for the interpretation: A mean difference of 3.04 for the variable CPP indicates that, on average, the treatment group's CPP value is 3.04 units higher than that of the control group.
      ![Image Alt Text](https://github.com/momenifi/methodHub/blob/main/academic_mobility_propensity_score/tutorial/output_mainDiff.PNG)
      
      
      
 By following these steps, you can successfully conduct propensity score matching analysis to assess the impact of academic mobility on research productivity and collaboration indicators, focusing on the variables of interest "PPY", "CPP", and "COPP".
      


## Conclusion

In conclusion, this tutorial provides a detailed overview of propensity score matching techniques and their application in assessing the impact of academic mobility on research productivity, received citations, and collaboration indicators. By following the step-by-step guide and sample code provided, learners gain a comprehensive understanding of how to implement propensity score matching in R and interpret the results effectively.

**How the Learning Goal is Achieved**

The learning goal of understanding and implementing propensity score matching techniques to assess the impact of academic mobility has been achieved through:

- Introduction to the concept of propensity score matching.
- Step-by-step instructions on data preparation, propensity score estimation, matching, and outcome analysis.
- Hands-on experience with example code and explanations.
- Interpretation of results and assessment of balance after matching.

**Skills Acquired with this Tutorial**

Upon completing this tutorial, learners have acquired the following skills:

- Understanding of propensity score matching and its relevance in social science research.
- Proficiency in using R programming language for propensity score matching analysis.
- Ability to interpret standardized mean differences (SMDs) and mean differences in matched data.
- Competence in assessing the impact of academic mobility on research productivity, received citations, and collaboration indicators using propensity score matching.

**Concluding Remarks**

Propensity score matching is a powerful technique for estimating causal effects in observational studies, particularly in the context of social science research. By mastering this technique, researchers can overcome challenges associated with selection bias and confounding variables, leading to more robust and reliable research findings. We encourage learners to further explore advanced topics in propensity score matching and apply these skills to their own research endeavors.

## References
- [The many facets of academic mobility and its impact on scholars' career](https://doi.org/10.1016/j.joi.2022.101280)
