# **Calculating Unit Economics for Streamline Pro**

**Background:** Streamline Pro is a comprehensive project management and collaboration tool designed to help businesses manage projects, track progress, and collaborate efficiently. Understanding the unit economics of Streamline Pro is crucial for evaluating its financial health and sustainability. This involves analysing key metrics such as Customer Acquisition Cost (CAC), Average Revenue Per User (ARPU), Cost of Goods Sold (COGS), Gross Margin, Customer Lifetime Value (LTV), and the LTV/CAC ratio.

**Objective:** Calculate the unit economics for Streamline Pro for the month of March 2023. This will help us assess the profitability and efficiency of our customer acquisition strategies and operational expenses.

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
