# Cohort-Analysis-Vintage-Curves-and-Net-Present-Value-NPV-Internal-Rate-of-Return-IRR-
predict how different cohorts (or vintages) of credit accounts perform over time, estimating return and loss trends.
Initial Data Analysis & Interpretation
Dataset Overview
•	The dataset contains 30,000 credit card clients.
•	The main attributes include:
o	LIMIT_BAL: Credit limit for each user.
o	PAY_X (PAY_1 to PAY_6): Past monthly payment statuses (e.g., whether they paid on time, were late, or defaulted).
o	BILL_AMTX (BILL_AMT1 to BILL_AMT6): Bill statements over the past six months.
o	PAY_AMTX (PAY_AMT1 to PAY_AMT6): Payment amounts over the past six months.
o	DEFAULT: Binary indicator (1 = defaulted, 0 = did not default).

Key Observations
1.	Default Rate:
o	Mean DEFAULT rate = 22.12% → About 22% of the clients defaulted on their payments.
2.	Credit Limits:
o	Mean = $167,484, but the range is wide (min = $10,000, max = $1,000,000).
o	A high standard deviation (~$129,748) suggests a large variance in credit limits.
3.	Age Distribution:
o	Clients range from 21 to 79 years old.
o	Median age is 34, suggesting a younger demographic.
4.	Repayment Behavior (PAY_X Variables):
o	Negative values indicate early repayment or no delay.
o	Mean values are close to 0, suggesting that most clients pay on time.
o	However, the maximum values go up to 8, meaning some clients are over 8 months late on payments.
5.	Bill & Payment Amounts:
o	The mean bill amount is around $40,000 per month, with some clients having negative balances (overpayments or credits).
o	Payment amounts vary significantly, with some clients making huge payments (up to $1.68 million in a single month).
We now analyze how different cohorts of customers default over time. A cohort is defined based on the AGE of the client (as a proxy for when they likely opened the account).

Interpretation of Cohort Analysis (Default Rate by Age)
•	Young Borrowers (20-29 years old) have a default rate of ~22.8%, slightly above average.
•	Middle-aged Borrowers (30-39 years old) show the lowest default rate (~20.3%).
•	Older Borrowers (50-69 years old) have an increasing default rate (from ~24.8% to ~28.3%).
•	Senior Borrowers (70+ years old) show a high default rate (28%), possibly due to fixed incomes or retirement challenges.
Net Present Value (NPV) & Internal Rate of Return (IRR) Analysis
We'll now estimate:

NPV: The expected financial return from different customer cohorts.
IRR: The return percentage of a typical credit account.
We assume:

The interest rate is 2% per month.
Defaulted accounts stop paying and generate losses.
Cash flow per cohort is estimated based on their credit limit and repayment rates.

Updated NPV & IRR Analysis Interpretation
•	The Net Present Value (NPV) remains unchanged, confirming that the previous calculation was correct.
•	Internal Rate of Return (IRR) now computes for most cohorts, but:
o	The 30-39, 40-49, and 60-69 age groups have negative IRRs (-1.68, -1.67, -1.67 respectively), suggesting that the cash flows from these cohorts are not generating positive financial returns under the assumed conditions.
o	The 50-59 cohort has an IRR of approximately -0.21, which is still negative but slightly better than other groups.
o	The 20-29 cohort IRR remains NaN, meaning the Newton's method might not have found a solution for this group.
Key Takeaways
1.	All IRRs are negative, meaning the lending conditions might need improvement (higher interest rates, better risk assessment).
2.	The 30-39 age group is still the most profitable, as shown by the highest NPV.
3.	Younger and older cohorts seem riskier, with lower NPVs and worse IRRs, requiring stricter credit policies.

