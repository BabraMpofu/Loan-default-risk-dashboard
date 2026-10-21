### Loan Default Risk Dashboard (Power BI)

An interactive Power BI dashboard that analyzes 148,670 loan records to show where default risk is concentrated, across regions, age groups, loan types, loan purposes, and credit score bands.

<img width="525" height="294" alt="image" src="https://github.com/user-attachments/assets/9e94dd2d-35c6-4b01-9e1d-e6ea47c9e9da" />


### Project goal

Lenders lose money when borrowers default. This dashboard turns a large loan dataset into a few clear visuals so a reader can quickly see which groups of loans default more often, and which show little difference.

### Dataset

- Source: [Loan Default Dataset on Kaggle](https://www.kaggle.com/datasets/yasserh/loan-default-dataset?select=Loan_Default.csv)
- Size: 148,670 loans and 34 columns (all from 2019)
- Target column: Status (1 = defaulted, 0 = did not default)
- Overall default rate: 24.6% (36,639 defaulted loans)
- The dataset contains no names or contact details. The original country and lender are not stated in the data, so no claims are made about them.

### Tools

- Power BI Desktop (Power Query for cleaning, DAX for measures)

### Data cleaning (Power Query)

| Step | What I did | Why |
|---|---|---|
| Removed columns | Dropped ID, year, rate_of_interest, Interest_rate_spread, Upfront_charges | ID is unique per row and year is 2019 for every row. The three interest and fee columns are blank for almost every defaulted loan, so any analysis using them would be misleading |
| Fixed data types | Set numbers, text, and whole numbers correctly | Wrong types break sums, averages, and sorting |
| Standardized labels | Capitalized Region values (for example "south" to "South") | Consistent labels in charts and slicers |
| Filled text blanks | Replaced blanks in text columns with "Not Recorded" | Keeps those loans visible as their own group instead of an unnamed blank |
| Left numeric blanks | Did not fill income, LTV, dtir1, or property_value | Filling with averages would invent data and distort results |
| Kept all rows | 148,670 rows before and after cleaning | Removing rows would change the default rate |

New columns created:

- Default Status: "Defaulted" or "Not Defaulted" (readable version of Status)
- Credit Score Band: Poor (500-599), Fair (600-699), Good (700-799), Excellent (800+)
- Credit Band Order: sort column so bands appear Poor to Excellent
- Income Bands: Low, Medium, High, plus "No Income Recorded" and "Not Recorded"
- Income Valid: flags the loans with zero or missing income
- Age Order: sort column so age groups appear in natural order

### DAX measures

Total Loans = COUNTROWS(Loan_Default)

Defaulted Loans = CALCULATE(COUNTROWS(Loan_Default), Loan_Default[Status] = 1)

Default Rate = DIVIDE([Defaulted Loans], [Total Loans])

### Dashboard contents

- Cards: Total Loans, Defaulted Loans, Default Rate
- Donut chart: share of defaulted vs. not defaulted loans
- Bar charts: default rate by Region, Age Group, Loan Type, Loan Purpose, and Credit Score Band
- Slicers: Region, Loan Type, Age (all charts and cards update when a slicer is used)

### Key insights

1. Region matters. North-East has the highest default rate (30.4%), followed by Central (27.5%) and South (26.6%). North is lowest (22.5%).
2. Loan type 2 is the riskiest. Type 2 loans default at 34.5%, compared with 25.1% for type 3 and 22.8% for type 1.
3. Loan purpose p2 stands out. It has a 33.1% default rate, against 23.0% to 25.9% for the other purposes.
4. Credit score band makes almost no difference. Default rates are about 24% to 25% in every band (Poor 24.6%, Fair 24.5%, Good 24.3%, Excellent 25.2%), so credit score alone does not separate defaulters from non-defaulters in this dataset.
5. Younger and older borrowers default more. The oldest group (over 74) is highest at 30.0% and the under-25 group follows at 28.9%, while ages 25 to 44 are lowest at about 22%.

### Limitations

- This is a descriptive dashboard. It shows patterns in the data, not causes.
- Some groups are small. For example, North-East has only 1,235 loans, so its rate is less reliable than the others.
- "Not Recorded" groups (a few hundred loans each for some columns) were left out of the age and loan purpose charts because of their small size.
- Many columns in the dataset were not visualized. Other fields, such as credit_type and submission_of_application, also show clear differences in default rate and could be added in a future version.

### How to explore it

Download the .pbix file and open it in Power BI Desktop (Windows). The dataset itself is not included here; download it from the Kaggle link above if you want to rebuild the report.
