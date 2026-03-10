# Loan Default Risk Analysis Dashboard
### Dashboard Link : https://app.powerbi.com/links/xPJRelJLfn?ctid=898975c9-56c3-411b-a9a1-ce4e2a6ce02d&pbi_source=linkShare&bookmarkGuid=d0ac403f-4126-42ea-8800-35251530b4f3

### Problem Statement

This dashboard helps financial institutions understand borrower behavior, loan distribution, and default risk patterns. It allows banks to identify which borrower segments are more likely to default and which groups generate the most loan demand.

Through analysis of loan purpose, credit score, employment type, income bracket, age group, and marital status, the dashboard highlights areas where financial risk is higher. This helps lending institutions make data-driven decisions while approving loans.

The dashboard also tracks yearly loan growth and default trends, helping organizations detect whether defaults are rising due to internal policy issues or external economic factors.

### Steps followed:

- Step 1 : Before working on the dataset, the business requirements and test environment structure were reviewed.

#### Key objectives identified:

1. Analyze loan distribution

2. Identify default risk patterns

3. Understand borrower demographics

4. Track yearly loan growth trends

A test environment was used first to validate queries before moving to production.

- Step 2 : The raw loan dataset (CSV file) was first imported into MS SQL Server to perform data cleaning and transformation before connecting it to Power BI.

A new database was created and the dataset was imported using Import Wizard.

- Step 3: Data quality checks were performed to ensure accurate reporting.

#### Cleaning tasks included:

"Identifying NULL values"

"Removing duplicate records"

"Standardizing categorical values"

"Verifying data types"

- Step 4: Multiple tables were joined to create a unified dataset for reporting. A LEFT JOIN was used to combine borrower data with loan transaction details.

- Step 5: The cleaned dataset stored in MS SQL Server was connected directly to Power BI Service, through server name and database name.

- Step 6: After loading the dataset, Power Query Editor enabled "column distribution", "column profiling" and "column distribution"  under view table for data profiling and validation. 
By default profiling is based on first 1000 rows, so the option “Column profiling based on entire dataset” was selected to analyze the complete dataset.

- Step 7: Since the data contains various calculations, a measure table was added to use DAXK (Data Analysis Expressions).
1. Total loan amount 
2. Average loan amount 
3. Default rate 
      
       Default Rate % =DIVIDE(COUNT(Loan_Data[DefaultStatus]),COUNT(Loan_Data[LoanID])) * 100


- Step 8: New Age Group column was created in Power Query Editor using a Conditional Column.
This helps segment borrowers into different age categories for further analysis and DAX calculations.
        
        Age Group = if [Age] <= 18 then "Teen",
        else if [Age] <= 45 then "Adults",
        else if [Age] <= 60 then "Middle Age Adults",
        else "Senior Citizens"

- Step 9 :Area chart was added to the dashboard showcasing median loan amount by credit score category for which credit score category was created in power query using conditional column. DAX was used for calculating median loan amount.

       Median by Credit score bins = MEDIANX('Loan_default 2','Loan_default 2'[LoanAmount]) 

snap of the Area Chart 
![Image](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560169644-1069e8ac-2ee3-4905-aabf-79d53cbab5f7.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T090905Z&X-Amz-Expires=300&X-Amz-Signature=7ac0161ad24c995f3832c16081d4a5af53deb49b465bd4456d4fddce62cb853f&X-Amz-SignedHeaders=host)

- Step 10 : Three slicers were added at the top of the dashboard to allow users to filter the entire report dynamically.
Slicers Used:
1. LoanID
2. Age_group
3. LoanPurpose

![Snap_2](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560195022-b4de3e93-51c2-40f1-bbee-b3a36926eafc.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T095217Z&X-Amz-Expires=300&X-Amz-Signature=8dd4ab89573f5d482a07a0f1647e9c1411f609bdf279e2ebfd3b2bdf5739645f&X-Amz-SignedHeaders=host)

- Step 11 : DAX was used to create line chart for the Education Analysis. 

#### DAX measure: 
     Total Loans = COUNT(loan[LoanID])

From the chart it was observed that mid-level education groups borrow more frequently and Bachelor's degree holders have the highest number of loans (~64K). 


![Snap_2](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560153646-4ee2bc05-beaf-42d2-9343-019f46aef7f5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T091134Z&X-Amz-Expires=300&X-Amz-Signature=ef4efd367812469b58a4e59cdf69e12f6ca6844b5dc2b378a3351ad070633518&X-Amz-SignedHeaders=host)



- Step 12 : Clustered column chart evaluates the relationship between existing financial obligations (mortgages and dependents) and total borrowing levels.

DAX measure: 

     Total Loan Amount = SUM(loan[LoanAmount])

Key Observation: Borrowers with mortgages and dependents tend to have slightly higher total loan values, suggesting that financial responsibilities influence borrowing behavior.

Snap of Clustered Chart:

![Snap_3](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560177150-3289d033-027a-423a-a8e4-9e2292b816c4.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T091758Z&X-Amz-Expires=300&X-Amz-Signature=69be98287b5ec41966efbc2c3bf2e6aa3b88da4aded3489e32db2f92c4f1c7b4&X-Amz-SignedHeaders=host)

Step 13: To demonstrate how loan amounts are distributed across credit score segments and borrower age groups flow/relationship chart was created. 

Key observation : The majority of loan value is concentrated among Adults and Middle-Age Adults, indicating that the working-age population represents the largest borrowing segment.

Snap of relationship chart:

![Snap_4](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560181795-f0010003-e177-475f-953d-d32b066f1487.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T092529Z&X-Amz-Expires=300&X-Amz-Signature=720614eafb409af6c5fc7cc5cd80b202d527aa0e77850c9494fb1f6da07ae4a5&X-Amz-SignedHeaders=host)

Step 14: Donut chart visualization was used to explore borrowing patterns among high-credit borrowers across different marital statuses and age groups.

Filter applied = High Credit 

Key observation: Loan amounts remain relatively consistent across demographic segments, with average loan values ranging between 126K and 128K.

![snap_5](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560184619-b27b4e2f-a73c-4696-9de8-240a4c46df35.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T093139Z&X-Amz-Expires=300&X-Amz-Signature=2e49db79633713719b4be85cac23f4669b5597f872a9d52df93bf1644956638f&X-Amz-SignedHeaders=host)

Step 15: Line chart showing Year-Over-Year change in loan defaults.
#### DAX measure:
       YOY Loan_Default Change = DIVIDE(
    CALCULATE(COUNTROWS(FILTER('Loan_default 2','Loan_default 2'[Default]=1)),'Loan_default 2'[loan_year]=YEAR(MAX
    ('Loan_default 2'[Loan Date (DD/MM/YYYY)]))) -   
    CALCULATE(COUNTROWS(FILTER('Loan_default 2','Loan_default 2'[Default]=1)),'Loan_default 2'[loan_year]=YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1)
    ,CALCULATE(COUNTROWS(FILTER('Loan_default 2','Loan_default 2'[Default]=1)),'Loan_default 2'[loan_year]=YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1),0)*100

Key observations: Financial institutions should closely monitor economic conditions or policy changes around 2015 and 2018 that may have influenced borrower default behavior.

Snap of line chart: 

![snap_6](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560397000-f4690ec6-40b0-4d78-bbfa-b8a37be1d971.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T155000Z&X-Amz-Expires=300&X-Amz-Signature=44256221671db10a1e3df4c22e27bdc514dde3eb7b8a77bbe8dfaad82b40c551&X-Amz-SignedHeaders=host)



Step 16: Line chart analyzing Year-Over-Year changes in loan amounts issued.
 #### DAX measure:
       yoy loan amount change = DIVIDE(
    CALCULATE(SUM('Loan_default 2'[LoanAmount]),'Loan_default 2'[loan_year]= YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)])))-CALCULATE(SUM('Loan_default 2'[LoanAmount]),'Loan_default 2'[loan_year]= YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1)
    , CALCULATE(SUM('Loan_default 2'[LoanAmount]),'Loan_default 2'[loan_year]= YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1),0)*100

Key Observation: Loan issuance declined in 2014 (-1.5) and 2017 (-1.1), indicating periods of tighter lending. It rebounded in 2015 (+1.3) and 2018 (+1.7), reflecting renewed credit expansion and higher loan demand. Overall, the pattern suggests cyclical lending behavior influenced by market conditions.

Snap of Line Chart: 

![Snap_7](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560401910-61e95788-5759-4c8e-96df-3040f9f1bbd5.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T155836Z&X-Amz-Expires=300&X-Amz-Signature=760e23bfa9457585bbbb480f57e5072703430e97e487a5512be6095cdeb00b98&X-Amz-SignedHeaders=host)

Step 17: Flow/Distribution chart showing loan distribution across credit score categories and marital status.

#### DAX measure: 

     YTD Loan Amounts = CALCULATE(SUM('Loan_default 2'[LoanAmount]), DATESYTD('Loan_default 2'[Loan Date (DD/MM/YYYY)].[Date]),ALLEXCEPT('Loan_default 2','Loan_default 2'[Credit score bin ],'Loan_default 2'[Age_group],'Loan_default 2'[EmploymentType],'Loan_default 2'[Education],'Loan_default 2'[MaritalStatus]))

Key Observation: Loan distribution is concentrated among high and median credit score borrowers (~0.65bn), while very low and low credit score groups receive significantly smaller loan amounts (~0.17bn–0.36bn). This indicates that banks prioritize lower-risk borrowers through risk-based lending strategies.

Snap of flow chart:
![Snap_8](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560474533-f6928d3f-38a0-4778-b61c-da7fe9d910bf.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T181755Z&X-Amz-Expires=300&X-Amz-Signature=ab8db126514531bc3153a6881d7dbdc42b9f12c7ddd192a8e8d567db091d0103&X-Amz-SignedHeaders=host)

Step 18 : Hierarchical flow diagram analyzing loan exposure across income groups and employment types.

    Total Loan Amount = 32.6 Billion
Key Observation: High-income borrowers account for the largest share of loan exposure (~66%), while medium and low-income groups receive significantly smaller loan amounts. Loan distribution across employment types remains relatively balanced, indicating lenders prioritize financially stable borrowers to minimize risk.

Snap of flow chart: 
![Snap_9](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560485399-def2c785-a3ea-413a-afcf-9e54cfb4426c.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260309T183337Z&X-Amz-Expires=300&X-Amz-Signature=ccdca776a471da95cc3eea617a6322e7f6a4f90ce8d607ee8dc544988c0f7e23&X-Amz-SignedHeaders=host)

Step 19 : The report was then stored  to my workspace in Power BI service.

Snap of stored Dashboard:
![Snap_10](https://github-production-user-asset-6210df.s3.amazonaws.com/266256600/560760116-c5886394-2a1e-4668-b655-9c827d958f64.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20260310%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260310T071253Z&X-Amz-Expires=300&X-Amz-Signature=df116cad45eaf16bde31033fd911f461d649e8184703a6d84ed2acf660ea0780&X-Amz-SignedHeaders=host)



### Insights

### Loan Demand Purpose: 
Home loans account for the largest loan amount (~6.5B) followed by Business and Education loans, while Auto and Other loans show comparatively lower demand.
Housing loans dominate the portfolio, indicating that the institution's lending strategy is heavily real-estate driven.

"Diversifying lending toward education and auto loans could help balance risk exposure."

### Employment Type vs Default Risk
Default rates are highest among unemployed borrowers (~3.38%) and lowest among full-time employed customers (~2.35%).
Employment stability strongly influences repayment ability.
Loan approval models should assign higher risk weights to unemployed or unstable employment categories.

### Income vs Credit Risk
Average income differences across employment categories are relatively small, suggesting that income alone is not a strong indicator of credit risk.
Even borrowers with similar incomes may show different repayment behavior depending on employment stability.
Credit scoring models should combine employment stability + credit score + loan purpose rather than relying only on income.

### Age Group Borrowing Pattern
Adults represent the highest average loan size (~127.9K) followed by middle-aged borrowers, while teenagers receive the lowest average loans.
Banks can target working adults with customized loan products such as home loans and business financing.

### Credit Score vs Loan Amount
The institution follows a risk-based lending policy, allocating larger loans to financially reliable borrowers.

### Education level borrowing behavior 
Borrowers with Bachelor’s degrees have the highest number of loans (~64K), followed by high school graduates.
Financial institutions can design specialized loan products for educated professionals.

### Default Rate Trend Over Time
Default rates fluctuate between 11.5% – 11.7% across years, indicating cyclical variation rather than a steady increase.
Loan defaults are likely influenced by external economic factors rather than internal policy changes.

### Mortgage and Dependents Impact
Loan amounts are very similar regardless of mortgage or dependent status, indicating these factors have minimal influence on total lending volume.Mortgage ownership may not significantly affect borrowing capacity

### Credit Score Distribution Across Age Groups
Adults and middle-aged borrowers contribute the largest share of loan amounts across most credit score categories, while senior citizens and teenagers contribute smaller portions.
The lending portfolio is heavily concentrated among economically active age groups.

### Income Bracket Contribution
High-income borrowers contribute over 21B in loan amounts, significantly higher than medium and low-income segments.
High-income customers represent the most valuable segment for loan revenue.

