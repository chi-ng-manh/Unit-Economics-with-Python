# **Calculating Unit Economics for Streamline Pro**

**Background:** Streamline Pro is a comprehensive project management and collaboration tool designed to help businesses manage projects, track progress, and collaborate efficiently. Understanding the unit economics of Streamline Pro is crucial for evaluating its financial health and sustainability. This involves analysing key metrics such as Customer Acquisition Cost (CAC), Average Revenue Per User (ARPU), Cost of Goods Sold (COGS), Gross Margin, Customer Lifetime Value (LTV), and the LTV/CAC ratio.

**Objective:** Calculate the unit economics for Streamline Pro for the month of March 2023. This will help us assess the profitability and efficiency of our customer acquisition strategies and operational expenses.

**Conclusion**
Comparing the LTV (2096 USD) and CAC (1214 USD) ratio, it shows that the cost of new customers is good and worth investing.

The LTV/CAC Ratio is above >1, which shows that the company is profitable in its business operations.

The company should maintain its operations at the current company and monitor this ratio regularly to maintain long-term profitability for the business.

##**How i solving this project**
** Read the Data (Monthly Expenses, Payroll, Customer Lifespan Data, Daily marketing spending, Receipts history)**

    monthly_expences_GS = '10OGbaywwMIqKgnPGy8VDvpBVtjyqln47iYa2lFhI9Mw'
    monthly_expences = pd.read_excel('https://docs.google.com/spreadsheets/d/' + monthly_expences_GS + '/export?format=xlsx', sheet_name = 'Sheet1')
    
    customer_lifespan_data_GS = '1by8tPHwOnq3uKYK2E7sA9VBUYoPM4p1Rnrm_Ss9cyHI'
    customer_lifespan_data = pd.read_excel('https://docs.google.com/spreadsheets/d/' + customer_lifespan_data_GS + '/export?format=xlsx', sheet_name = 'Sheet1')
    
    daily_marketing_spendings_GS = '1AZOIThOV4P-0eYDge53ZwumVkfkHoYPWxst3k3Bv87c'
    daily_marketing_spendings = pd.read_excel('https://docs.google.com/spreadsheets/d/' + daily_marketing_spendings_GS + '/export?format=xlsx', sheet_name = 'Sheet1')
    
    Payroll_GS = '1c_WihqTZCQvNgxzmd-OwhR9i5diwtfxXVLyMn8R-Lp4'
    Payroll = pd.read_excel('https://docs.google.com/spreadsheets/d/' + Payroll_GS + '/export?format=xlsx', sheet_name = 'Sheet1')
    
    receipts_history_GS = '1qayqML1zCKdmtzutkcy9LWvE6xFRm6TGBEVkHHJKIuE'
    receipts_history = pd.read_excel('https://docs.google.com/spreadsheets/d/' + receipts_history_GS + '/export?format=xlsx', sheet_name = 'Sheet1')
## ****1:** Calculation Customer Acquisition Cost (CAC)**
**CAC = Total Expences of Sales,Marketing / Total New Customer**

**1.1 Filtering the last month expences for Sale Solfware**
    
    Last_month = monthly_expences['month'].max().month
    Last_month
    
    cond = monthly_expences['month'].dt.month == Last_month
    last_month_expences = monthly_expences[cond]
    last_month_expences
 
    cond = last_month_expences['item'] == 'Salesforce'
    crm_cost = last_month_expences[cond]['amount'].values[0]
    print(crm_cost)
    
    last_month_expences[cond]['amount']
**1.2 Calculate Salaries of Sale,Marketing Employees**
    
    Last_month_payroll = Payroll['month'].max().month
    Last_month_payroll
    
    cond = Payroll['month'].dt.month == Last_month_payroll
    last_payroll = Payroll[cond]
    last_payroll
    
    cond = last_payroll['department'].isin(["Marketing", "Sales"])
    Sales_marketing_payroll = last_payroll[cond]['paid']
    print(Sales_marketing_payroll)
    
    Sales_Marketing_paid = sum(Sales_marketing_payroll)
    
**1.3 Marketing Costs**

    last_month_mkt_expences = daily_marketing_spendings['date'].max().month
    last_month_mkt_expences
    
    cond = daily_marketing_spendings['date'].dt.month == last_month_mkt_expences
    last_month_mkt = daily_marketing_spendings[cond]['spending']
    last_month_mkt
    
    last_month_mkt_expences = daily_marketing_spendings[cond]['spending'].sum()
    print(last_month_mkt_expences)
    
**1.4 Total Expences**

    Total_Expences = crm_cost + Sales_Marketing_paid + last_month_mkt_expences
    print(Total_Expences)
    
**1.5 Total New Customer**

    Last_month_New_Customer = receipts_history['date'].max().month
    Last_month_New_Customer
    
    cond = receipts_history['date'].dt.month == Last_month_New_Customer
    New_Customer = receipts_history[cond]
    New_Customer
    
    cond = New_Customer['new_customer'] == 1
    Last_month_New_Customer = New_Customer[cond]
    
    Numberof_new_Customer = len(Last_month_New_Customer)
    Numberof_new_Customer
    
**1.6 Final CAC Calculation**

    CAC = Total_Expences / Numberof_new_Customer
    print(CAC)

## 2. Average Revenue Per User (ARPU)

ARPU = Total Revenue / Number of User

**2.1 Total Revenue**

    Last_month_Revenue = receipts_history['date'].max().month
    Last_month_Revenue
    
    cond = receipts_history['date'].dt.month == Last_month_Revenue
    Last_month_Revenue = receipts_history[cond]
    Last_month_Revenue
    
    Total_Revenue = Last_month_Revenue['receipt_amount'].sum()
    Total_Revenue
    
**2.2 Total Customer**

    TotalCustomer_last_month = len(New_Customer['customer_id'].unique())
    TotalCustomer_last_month
    
**2.3 ARPU Calculation**

    ARPU = Total_Revenue / TotalCustomer_last_month
    print(ARPU)

## 3. Cost of Goods Sold (COGS)

COGS = Beginning Inventory + Purchase During Period - Ending Inventory

**3.1 Last Month Purchase During Period**

    last_month_expences
    
    production_rows = ['AWS Hosting', 'Google Cloud Storage', 'Atlassian Jira', 'Slack', 'Zoom']
    production_purchase = last_month_expences[last_month_expences['item'].isin(production_rows)]
    production_purchase
    
    solfware_n_server_expences = production_purchase['amount'].sum()
    print(solfware_n_server_expences)
    
**3.2 Last Month payroll (Engineering)**

    last_payroll
    
    production_salaries_df = last_payroll[last_payroll['department'] == "Engineering"]
    production_salaries_df
    
    production_salaries = production_salaries_df['paid'].sum()
    print(production_salaries)
    
**3.3 COGS Calculation**

    COGS = solfware_n_server_expences + production_salaries
    print(COGS)

## 4. GROSS Margin

GrossMargin = ((Revenue - COGS) / Revenue) * 100

**4.1 Last Month Revenue**

    Last_month_Revenue
    
    Revenue = Last_month_Revenue['receipt_amount'].sum()
    print(Revenue)
    
4.2 Gross Margin Calculation

    GrossMargin = (Revenue - COGS) / Revenue * 100
    print(GrossMargin)
    
## 5. Customer Life Time Value (LTV)

LTV = ARPU x CustomerLifeTimeSpan x GrossMargin

**5.1 Average Customer Lifespan**
 
    customer_lifespan_data.sample(10)
    
 #Handle cases when churn_date is None (Customer did not churn whithin the observed period)
    
    lifespan_data = customer_lifespan_data.dropna(subset=['churn_date'])
    
Calculate customer lifespan in days (in the new column "lifespan_days")

    lifespan_data['lifespan_days'] = (lifespan_data['churn_date'] - lifespan_data['start_date']).dt.days

Calculate the average lifespan in days

    avg_lifespan_days = lifespan_data['lifespan_days'].mean()

Convert average lifespan to months

    avg_lifespan_months = avg_lifespan_days / 30
    print(avg_lifespan_months)

**5.2 LTV Calculation**

    LTV = ARPU * avg_lifespan_months * GrossMargin/100
    print(LTV)
    
## 6. LTV / CAC Ratio

    LTV_CAC_Ratio = LTV / CAC
    print(LTV_CAC_Ratio)
    
## 7. Conclusion

Comparing the LTV (2096 USD) and CAC (1214 USD) ratio, it shows that the cost of new customers is good and worth investing.

The LTV/CAC Ratio is above >1, which shows that the company is profitable in its business operations.

The company should maintain its operations at the current company and monitor this ratio regularly to maintain long-term profitability for the business.
