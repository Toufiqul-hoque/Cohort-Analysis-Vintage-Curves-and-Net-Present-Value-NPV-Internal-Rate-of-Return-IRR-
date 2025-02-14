# Cohort-Analysis-Vintage-Curves-and-Net-Present-Value-NPV-Internal-Rate-of-Return-IRR-
predict how different cohorts (or vintages) of credit accounts perform over time, estimating return and loss trends.
Initial Data Analysis & Interpretation
import pandas as pd

# Load the uploaded dataset
file_path = "C:/Users/sazid/Desktop/documents/Tech prep/C1B/New folder/default+of+credit+card+clients/default of credit card clients.csv"
data = pd.read_csv(file_path)

# Display basic information about the dataset
data.info(), data.head()
# Clean column names by renaming them properly
data.columns = [
    "ID", "LIMIT_BAL", "SEX", "EDUCATION", "MARRIAGE", "AGE", 
    "PAY_1", "PAY_2", "PAY_3", "PAY_4", "PAY_5", "PAY_6", 
    "BILL_AMT1", "BILL_AMT2", "BILL_AMT3", "BILL_AMT4", "BILL_AMT5", "BILL_AMT6", 
    "PAY_AMT1", "PAY_AMT2", "PAY_AMT3", "PAY_AMT4", "PAY_AMT5", "PAY_AMT6", 
    "DEFAULT"
]

# Convert numeric columns to appropriate types
numeric_columns = data.columns[1:]
data[numeric_columns] = data[numeric_columns].apply(pd.to_numeric, errors='coerce')

# Display summary statistics
data.describe()
import matplotlib.pyplot as plt
import seaborn as sns

# Create age groups (cohorts)
bins = [20, 30, 40, 50, 60, 70, 80]
labels = ['20-29', '30-39', '40-49', '50-59', '60-69', '70+']
data['AgeGroup'] = pd.cut(data['AGE'], bins=bins, labels=labels, right=False)

# Calculate default rate per age group
cohort_default_rate = data.groupby('AgeGroup')['DEFAULT'].mean().reset_index()

# Plot default rate per cohort
plt.figure(figsize=(10, 6))
sns.barplot(data=cohort_default_rate, x='AgeGroup', y='DEFAULT', palette="Blues_r")
plt.title('Default Rate by Age Cohort')
plt.xlabel('Age Group')
plt.ylabel('Default Rate')
plt.ylim(0, 0.3)
plt.show()

# Display the default rates per age group
cohort_default_rate


# Assume the monthly interest rate and discount rate
interest_rate = 0.02  # 2% monthly
discount_rate = 0.01  # 1% monthly

# Estimate cohort-level cash flows
cohort_data = data.groupby("AgeGroup").agg(
    AverageCreditLimit=("LIMIT_BAL", "mean"),
    DefaultRate=("DEFAULT", "mean"),
    CustomerCount=("DEFAULT", "count")
).reset_index()

# Calculate monthly cash flows per cohort
cohort_data["MonthlyCashFlow"] = cohort_data["AverageCreditLimit"] * (1 - cohort_data["DefaultRate"]) * interest_rate




import numpy as np

# Define NPV function
def npv(rate, cashflows):
    return sum(cf / (1 + rate) ** i for i, cf in enumerate(cashflows))

# Define IRR function using Newton's method
def irr(cashflows, max_iterations=1000, tol=1e-6):
    """Compute IRR using Newton's method."""
    rate = 0.1  # Initial guess
    for _ in range(max_iterations):
        npv_value = sum(cf / (1 + rate) ** i for i, cf in enumerate(cashflows))
        npv_derivative = sum(-i * cf / (1 + rate) ** (i + 1) for i, cf in enumerate(cashflows))
        
        if abs(npv_value) < tol:  # Convergence condition
            return rate
        rate -= npv_value / npv_derivative  # Newton-Raphson update
        
    return None  # No solution found

# Recalculate NPV and IRR using the custom functions
cohort_data["NPV"] = cohort_data["MonthlyCashFlow"].apply(lambda x: npv(discount_rate, [x] * 12))
cohort_data["IRR"] = cohort_data["MonthlyCashFlow"].apply(lambda x: irr([-cohort_data["AverageCreditLimit"].mean()] + [x] * 12))


cohort_data
