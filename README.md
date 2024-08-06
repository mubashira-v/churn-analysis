# churn-analysis
INTRODUCTION TO CHURN ANALYSIS

In today's competitive business environment, retaining customers is crucial for long-term success. Churn analysis is a key technique used to understand and reduce this customer attrition. It involves examining customer data to identify patterns and reasons behind customer departures. By using advanced data analytics and machine learning, businesses can predict which customers are at risk of leaving and understand the factors driving their decisions. This knowledge allows companies to take proactive steps to improve customer satisfaction and loyalty.

WHO IS THE TARGET AUDIENCE

Although this project focuses on churn analysis for a telecom firm, the techniques and insights are applicable across various industries. From retail and finance to healthcare and beyond, any business that values customer retention can benefit from churn analysis. We will explore the methods, tools, and best practices for reducing churn and improving customer loyalty, transforming data into actionable insights for sustained success.

PROJECT TARGET

Create an entire ETL process in a database & a Power BI dashboard to utilize the Customer Data and achieve below goals:

Visualize & Analyse Customer Data at below levels
Demographic
Geographic
Payment & Account Info
Services
Study Churner Profile & Identify Areas for Implementing Marketing Campaigns
Identify a Method to Predict Future Churners
 

Metrics Required

Total Customers
Total Churn & Churn Rate
New Joiners

STEP 1 - ETL Process in SQL Server

So the first step in churn analysis is to load the data from our source file. For this purpose we will be using Microsoft SQL server because it is a widely used solution across the industry and also because a full-fledged Database System is better at handling recurring data loads and maintaining data integrity compared to an excel file.

STEP 2 - Power BI Transform
 :-Add a new column in prod_Churn
1.       Churn Status = if [Customer_Status] = "Churned" then 1 else 0
2.       Change Churn Status data type to numbers
3.       Monthly Charge Range = if [Monthly_Charge] < 20 then "< 20" else if [Monthly_Charge] < 50 then "20-50" else if [Monthly_Charge] < 100 then "50-100" else "> 100"
 :-Create a New Table Reference for mapping_AgeGrp
Keep only Age column and remove duplicates
1.       Age Group = if [Age] < 20 then "< 20" else if [Age] < 36 then "20 - 35" else if [Age] < 51 then "36 - 50" else "> 50"
2.       AgeGrpSorting = if [Age Group] = "< 20" then 1 else if [Age Group] = "20 - 35" then 2 else if [Age Group] = "36 - 50" then 3 else 4
3.       Change data type of AgeGrpSorting to Numbers

 :-Create a new table reference for mapping_TenureGrp

Keep only Tenure_in_Months and remove duplicates

1.       Tenure Group = if [Tenure_in_Months] < 6 then "< 6 Months" else if [Tenure_in_Months] < 12 then "6-12 Months" else if [Tenure_in_Months] < 18 then "12-18 Months" else if [Tenure_in_Months] < 24 then "18-24 Months" else ">= 24 Months"

2.       TenureGrpSorting = if [Tenure_in_Months] = "< 6 Months" then 1 else if [Tenure_in_Months] =  "6-12 Months" then 2 else if [Tenure_in_Months] = "12-18 Months" then 3 else if [Tenure_in_Months] = "18-24 Months " then 4 else 5

 Change data type of TenureGrpSorting  to Numbers

:-  Create a new table reference for prod_Services

 Unpivot services columns
 Rename Column – Attribute >> Services & Value >> Status

STEP 3 - Power BI Measure

Total Customers = Count(prod_Churn[Customer_ID])
New Joiners = CALCULATE(COUNT(prod_Churn[Customer_ID]), prod_Churn[Customer_Status] = "Joined")
Total Churn = SUM(prod_Churn[Churn Status])
Churn Rate = [Total Churn] / [Total Customers]

STEP 4 - Power BI Visualization
Summary Page
1.  Top Card

a.       Total Customers

b.       New Joiners

c.       Total Churn

d.       Churn Rate%

2.  Demographic

a.       Gender – Churn Rate

b.       Age Group – Total Customer & Churn Rate

3.  Account Info

a.       Payment Method – Churn Rate

b.       Contract – Churn Rate

c.       Tenure Group - Total Customer & Churn Rate

4.  Geographic

a.       Top 5 State – Churn Rate

5.  Churn Distribution

a.       Churn Category – Total Churn

b.       Tooltip : Churn Reason – Total Churn

6.  Service Used

a.       Internet Type – Churn Rate

b.       prod_Service >> Services – Status – % RT Sum of Churn Status

 

Churn Reason Page (Tooltip)

1.  Churn Reason – Total Churn
