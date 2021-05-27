# Prosper Loan Data Exploration
## by Erik Sorensen

## Overview

The purpose of this exploration is to determine which variables from the [Prosper](https://www.prosper.com) loan dataset influence the annual percentage rate (APR) for a loan.

## Dataset

The dataset is a collection of loan listings from [Prosper](https://www.prosper.com), a peer-to-peer lending marketplace.

The dataset contains one entry per loan. There is information on its status, on the parameters of the loan (e.g. term, amount, interest rate, APR), on credit-related variables for the borrower (e.g. employment, credit rating, credit accounts and balances, income, debt), on Prosper's own risk score and loan rating, and on investments in the loan. We are primarily interested in the loan parameters, risk scores and borrower credit-related variables.

## Summary of Findings

#### Univariate Exploration

- The dataset appears to be fairly clean, without blatantly obvious erroneous or missing entries.
- The exception was `EmploymentStatus` whose categories were confusing/overlapping in a way that didn't seem resolveable. This variable was not analyzed.
- The dataset also appears tidy, without multivariable columns and with each row containing an observation. The table structure is appropriately
- APR is nearly normally distributed, but has peaks near 0.30 and 0.35.
- Prosper's own [risk score](https://www.prosper.com/help/topics/general-prosper_score.aspx) and [loan rating] (https://www.prosper.com/invest/how-to-invest/prosper-ratings/?mod=article_inline) were also nearly normally distributed, except for plateaus in the middle values. The latter contains the former, so their distributions should be similar.
- Because these risk scores are expected to heavily influence APR, the dataset was limited to entries where they're given (loans after July 2009)
- The constituent variables of the Prosper risk score were not analyzed individually.
- Most borrower credit-related variables were found to be right-skewed. This includes credit score, reported income, debt:income ratio, credit accounts, balances, and delinquencies.
- For heavily right-skewed variables, extreme outliers were removed.
- Variables that were extremely skewed toward a single value (e.g. Delinquences and Recommendations) were not further analyzed as there was deemed to be insufficient data to see separation among categories.

#### Bivariate Exploration

##### Relationship of loan- and borrower-related variables to APR

- The strongest influence on APR appears to be the Prosper loan rating. This is a composite of the Prosper risk score and credit score, and appears to be the primary metric Prosper uses for determining loan risk. Since more creditworthy buyers receive the lowest APRs, this makes sense.
- The Prosper risk score and credit score were also correlated to APR, but the relationship didn't appear as strong, with wider standard deviations and less consistent slopes.
-The next most influential was loan amount, with larger amounts receiving lower APRs.
- Of the credit report-related variables that are __not__ part of the Prosper score, none except Debt:Income ratio showed a relationship to APR. This likely means that Prosper selected out all the important variables in creating their risk score.
- Homeowner status & prior Prosper loans were also associated very slightly with lower APR, although this appears to be due to their relationship to a slightly higher loan rating.

##### Other interesting relationships

- Loan amount was lower for the lowest-rated loans. This may be a result of investors wanting to limit the amount of their exposure to higher-risk loans.
- The two constituents of the loan rating variable, Prosper score and credit score, both appeared related to loan rating in a stepwise-increasing fashion. This could indicate these variables are split into categorical bins when calculating the loan rating.
- Debt:income ratio, one of the few credit report variables showing an association with APR, was also slightly associated with loan score (higher loan score with lower ratio). This could indicate the influence of debt:income ratio is indirect via its association with loan rating.
- Loan rating also showed a slight increase with income, although income did not appear to be related to APR.
- Debt:income ratio seemed to have a hyperbolic relationship to income, with lower ratios at higher incomes.

#### Multivariate Exploration

##### Relationship of loan- and borrower-related variables to APR

- The only variable that seemed to have a significant influence on APR in a mulitvariate context was loan rating
- Note that its constituents, credit score and Prosper score, were not examined separately as they should be correlated to loan rating
- The minor effects of debt:income ratio, homeowner status, and number of previous Prosper loans on APR appear insignificant when stratified by loan rating.
- There does not appear to be a significant influence of loan amount on APR when stratified by loan rating. However, it does appear that loans >\$25k are only given to borrowers with a loan rating >= 5. It also appears generally that these higher-dollar loans have lower APRs.
- Examining this further: compared to all loans <=\$25k, loans for >\$25k enjoy lower APRs. However, these loans are only given to borrowers with a loan rating >= 5. When compared to loans <=\$25k given to borrowers with loan ratings >=5, the APRs are not different.

##### Other interesting relationships

- In terms of the constitutents of the loan rating, there appears to be an interaction between credit score and Prosper score.
- At lower Prosper scores, the slope of credit score vs. loan rating is less steep than at higher scores. At higher Prosper scores, the relationship also seems more stepwise than linear, as was shown in the bivariate plots.
- Finally, the relationships do not appear to be completely monotonic, especially at lower scores. This may indicate that the formula is non-linear, or that there are additional factors included that aren't disclosed by Prosper.

## Key Insights for Presentation

- The most, and really only, variable to obviously influence APR is the Prosper loan rating. This will be shown with a pointplot of mean APR by loan rating.
- The credit-related variables did not influence APR. This isn't easy to show in compact fashion since there are so many variables. To give examples, I will show a matrix of heatmaps.
- The next most influential variable, loan amount, does not have an influence on APR except at amounts >\$25k. However, when stratified by loan rating, the difference disappears. Will use the box plots and stratified point plot to show this.
- Since loan rating is so influential, it is of interest to try to determine what effect its constituents have on it. This is a bit messy, but will show the multivariate point plot of loan rating by credit score and Prosper score. 
- All plots will need less abbreviated axis labels and titles. Will also want to assess for appropriate tick marks.
