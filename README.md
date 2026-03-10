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

snap of the Area Chart ![Area chart ](https://github.com/user-attachments/assets/874f3f51-f85c-45e1-8411-c8dd09649972)

- Step 10 : Three slicers were added at the top of the dashboard to allow users to filter the entire report dynamically.
Slicers Used:
1. LoanID
2. Age_group
3. LoanPurpose

<img width="1365" height="725" alt="image" src="https://github.com/user-attachments/assets/fd5f0dda-06dd-44a2-b004-ae4cee30757b" />

- Step 11 : DAX was used to create line chart for the Education Analysis. 

#### DAX measure: 
     Total Loans = COUNT(loan[LoanID])

From the chart it was observed that mid-level education groups borrow more frequently and Bachelor's degree holders have the highest number of loans (~64K). 


<img width="301" height="275" alt="image" src="https://github.com/user-attachments/assets/e877dfbf-b2f6-425d-a7d9-81eb49ec45cf" />

- Step 12 : Clustered column chart evaluates the relationship between existing financial obligations (mortgages and dependents) and total borrowing levels.

DAX measure: 

     Total Loan Amount = SUM(loan[LoanAmount])

Key Observation: Borrowers with mortgages and dependents tend to have slightly higher total loan values, suggesting that financial responsibilities influence borrowing behavior.

Snap of Clustered Chart:

<img width="464" height="256" alt="Screenshot 2026-03-09 at 2 47 18 PM" src="https://github.com/user-attachments/assets/ec4b92d3-fcbd-4370-a058-41f55a109acb" />


Step 13: To demonstrate how loan amounts are distributed across credit score segments and borrower age groups flow/relationship chart was created. 

Key observation : The majority of loan value is concentrated among Adults and Middle-Age Adults, indicating that the working-age population represents the largest borrowing segment.

Snap of relationship chart:

<img width="410" height="337" alt="Screenshot 2026-03-09 at 2 43 54 PM" src="https://github.com/user-attachments/assets/c545f5fc-45b7-4273-b741-6bcf83c697e2" />
Step 14: Donut chart visualization was used to explore borrowing patterns among high-credit borrowers across different marital statuses and age groups.

Filter applied = High Credit 

Key observation: Loan amounts remain relatively consistent across demographic segments, with average loan values ranging between 126K and 128K.

<img width="716" height="249" alt="Screenshot 2026-03-09 at 3 00 44 PM" src="https://github.com/user-attachments/assets/50f67d92-fdc7-4326-81ca-9253bf53e3bd" />

Step 15: Line chart showing Year-Over-Year change in loan defaults.
#### DAX measure:
       YOY Loan_Default Change = DIVIDE(
    CALCULATE(COUNTROWS(FILTER('Loan_default 2','Loan_default 2'[Default]=1)),'Loan_default 2'[loan_year]=YEAR(MAX
    ('Loan_default 2'[Loan Date (DD/MM/YYYY)]))) -   
    CALCULATE(COUNTROWS(FILTER('Loan_default 2','Loan_default 2'[Default]=1)),'Loan_default 2'[loan_year]=YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1)
    ,CALCULATE(COUNTROWS(FILTER('Loan_default 2','Loan_default 2'[Default]=1)),'Loan_default 2'[loan_year]=YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1),0)*100

Key observations: Financial institutions should closely monitor economic conditions or policy changes around 2015 and 2018 that may have influenced borrower default behavior.

Snap of line chart: 
<img width="608" height="257" alt="Screenshot 2026-03-09 at 9 18 37 PM" src="https://github.com/user-attachments/assets/119b04ba-fe0e-4083-94df-a4cb76be29ce" />

Step 16: Line chart analyzing Year-Over-Year changes in loan amounts issued.
 #### DAX measure:
       yoy loan amount change = DIVIDE(
    CALCULATE(SUM('Loan_default 2'[LoanAmount]),'Loan_default 2'[loan_year]= YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)])))-CALCULATE(SUM('Loan_default 2'[LoanAmount]),'Loan_default 2'[loan_year]= YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1)
    , CALCULATE(SUM('Loan_default 2'[LoanAmount]),'Loan_default 2'[loan_year]= YEAR(MAX('Loan_default 2'[Loan Date (DD/MM/YYYY)]))-1),0)*100

Key Observation: Loan issuance declined in 2014 (-1.5) and 2017 (-1.1), indicating periods of tighter lending. It rebounded in 2015 (+1.3) and 2018 (+1.7), reflecting renewed credit expansion and higher loan demand. Overall, the pattern suggests cyclical lending behavior influenced by market conditions.

Snap of Line Chart: 

<img width="582" height="262" alt="Screenshot 2026-03-09 at 9 27 30 PM" src="https://github.com/user-attachments/assets/77517065-fefc-481d-90ca-eedfd8c9db82" />


Step 17: Flow/Distribution chart showing loan distribution across credit score categories and marital status.

#### DAX measure: 

     YTD Loan Amounts = CALCULATE(SUM('Loan_default 2'[LoanAmount]), DATESYTD('Loan_default 2'[Loan Date (DD/MM/YYYY)].[Date]),ALLEXCEPT('Loan_default 2','Loan_default 2'[Credit score bin ],'Loan_default 2'[Age_group],'Loan_default 2'[EmploymentType],'Loan_default 2'[Education],'Loan_default 2'[MaritalStatus]))

Key Observation: Loan distribution is concentrated among high and median credit score borrowers (~0.65bn), while very low and low credit score groups receive significantly smaller loan amounts (~0.17bn–0.36bn). This indicates that banks prioritize lower-risk borrowers through risk-based lending strategies.

Snap of flow chart:
<img width="505" height="286" alt="Screenshot 2026-03-09 at 11 47 11 PM" src="https://github.com/user-attachments/assets/496027a6-b7b6-44d4-bd4e-18d06cee7365" />

Step 18 : Hierarchical flow diagram analyzing loan exposure across income groups and employment types.

    Total Loan Amount = 32.6 Billion
Key Observation: High-income borrowers account for the largest share of loan exposure (~66%), while medium and low-income groups receive significantly smaller loan amounts. Loan distribution across employment types remains relatively balanced, indicating lenders prioritize financially stable borrowers to minimize risk.

Snap of flow chart: 
<img width="709" height="520" alt="Screenshot 2026-03-10 at 12 03 00 AM" src="https://github.com/user-attachments/assets/5dd5efec-800b-4f44-b62f-fa0de1a46e07" />

Step 19 : The report was then stored  to my workspace in Power BI service.

Snap of stored Dashboard:
<img width="1432" height="829" alt="Screenshot 2026-03-10 at 12 41 29 PM" src="https://github.com/user-attachments/assets/a7dfdc1d-0d5a-4637-942a-757712e2405a" />

### Insights:

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

