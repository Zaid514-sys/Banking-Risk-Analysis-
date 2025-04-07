# Banking-Risk-Analysis-
Effectively combined SQL, Python, Excel, and Power BI to deliver a comprehensive view of banking risk. Enabled stronger insights into customer risk exposure and behaviour  through systematic data modelling and visual storytelling. Demonstrates a high-impact application of analytics in financial risk management and business intelligence.
      
      CREATE DATABASE Banking_Analysis;
      
      Show tables;
      
      Select * from customers;

# Installed Connections  
    
        pip install mysql-connector-python  
    
        pip install mysql-connector-python[telemetry]
    
        import mysql.connector
    
    
# Connected to server
        cnx = mysql.connector.connect(
            host="127.0.0.1",
            port=3306,
            user="root",
            password="12345")

# Imported Librabries 

    import pandas as pd 
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt 
    %matplotlib inline 
    import seaborn as sns


# Data Ready for Analysis / accessible

    query = "select * from Banking_Analysis.customers"
    df = pd.read_sql(query , cnx)
    cnx.close()
    print(df)
    df.head(5)
    df.describe()
    df.info()
    df.shape

# Null Values 

    df.isnull().sum()

# Created Income bands And categorised in 3 Parts 

    bins = [0,100000,300000,float('inf')]
    labels = ['low','Med','High']
    
    df['Income Band'] = pd.cut(df['Estimated Income'],bins=bins,labels=labels,right=False)
    df['Income Band'].value_counts()

# Bar Plots -

      df['Income Band'].value_counts().plot(kind='bar')

  ![image](https://github.com/user-attachments/assets/d8c15c94-9289-49be-a81c-66500984365f)


#  Examine the distribution of unique cataegories in categorical columns
    
    categorical_cols = df[["BRId","GenderId","IAId","Amount of Credit Cards","Nationality","Occupation","Fee Structure","Loyalty Classification","Properties Owned","Risk         Weighting","Income Band"]].columns
    
    for col in categorical_cols:
        print(f"Value Counts for '{col}':")
        display(df[col].value_counts()) 

# Univarite Analysis

    for i, predictor  in enumerate(df[["BRId","GenderId","IAId","Amount of Credit Cards","Nationality","Occupation","Fee Structure","Loyalty Classification","Properties Owned","Risk Weighting","Income Band"]].columns):
        plt.figure(i)
        sns.countplot(data=df,x = predictor)

![image](https://github.com/user-attachments/assets/3d93f051-b73e-4777-b690-a97c8d4ed76d)
![image](https://github.com/user-attachments/assets/3ccc05b5-68e2-4f81-80fd-abf7d633b1ba)
![image](https://github.com/user-attachments/assets/41b640d9-4b36-440c-ad58-a865cf0038d9)
![image](https://github.com/user-attachments/assets/b2eee920-7318-4f18-bfa1-5bc1052621c3)
![image](https://github.com/user-attachments/assets/10556548-8534-48c8-9e60-cbd3589070df)
![image](https://github.com/user-attachments/assets/f46ee774-d5fc-47c1-a1aa-83d5bbb597c3)
![image](https://github.com/user-attachments/assets/80c38759-d29a-4fdc-8ec5-5699d85b3cfa)
![image](https://github.com/user-attachments/assets/8db9f960-8a27-49c1-96c9-ef2982031869)
![image](https://github.com/user-attachments/assets/1dd52a9a-cf57-4b00-ad8e-565fd01150fd)


# Bivariate Analysis 

    for i, predictor  in enumerate(df[["BRId","GenderId","IAId","Amount of Credit Cards","Nationality","Occupation","Fee Structure","Loyalty Classification","Properties Owned","Risk Weighting","Income Band"]].columns):
        plt.figure(i)
        sns.countplot(data=df,x = predictor,hue='GenderId')

![image](https://github.com/user-attachments/assets/4fa460ad-c842-4ea5-a906-c0f3a30ddaf6)
![image](https://github.com/user-attachments/assets/d1c56d0c-02a2-4a01-b597-68b61b4f9e9f)
![image](https://github.com/user-attachments/assets/57c26fde-ef5c-4316-84f0-50a41aa605ad)
![image](https://github.com/user-attachments/assets/bc9668e0-f754-4d15-910c-a8a43fe5e1bf)
![image](https://github.com/user-attachments/assets/002780a6-a9bd-422c-9dec-8a9bd7683049)
![image](https://github.com/user-attachments/assets/ad61cfe0-26c9-4555-a819-2cb1e8393457)

# histplots if values counts for different occupation 
    
    for col in categorical_cols:
        if col == "Occupation":
            continue
    plt.figure(figsize=(8,4))
    sns.histplot(df[col]),
    plt.title('Hostogram Occupation count'),
    plt.xlabel(col),
    plt.ylabel("count"),
    plt.show()

![image](https://github.com/user-attachments/assets/a0a7661b-f306-4aa3-9ff8-c61ee404b095)


# Numerical Analysis 
    
    numerical_cols = ['Estimated Income','Superannuation Savings','Credit Card Balance','Bank Loans','Bank Deposits','Checking Accounts','Saving Accounts',	'Foreign Currency Account','Business Lending']
    
    
    plt.figure(figsize=(20,20))
    for i,col in enumerate(numerical_cols):
        plt.subplot(4,3,i+1)
        sns.histplot(df[col],kde=True)
        plt.title(col)
    plt.show()

![image](https://github.com/user-attachments/assets/78ed98d8-3e0a-4412-bdff-19391b3d5933)


# Heat Map

    numerical_cols = ['Estimated Income','Superannuation Savings','Credit Card Balance','Bank Loans','Bank Deposits','Checking Accounts','Saving Accounts',	'Foreign Currency Account','Business Lending']
    
    Correlation_matrix = df[numerical_cols].corr()
    
    plt.figure(figsize=(10,10))
    sns.heatmap(Correlation_matrix,annot=True,cmap='crest',fmt=".2f")
    plt.title('Correlation_Matrix')
    plt.show()

![image](https://github.com/user-attachments/assets/fe5506dc-1ebe-40be-a624-fd13b54c4cc2)

# Insights from EDA ?
    1. The Strongest positive Correlation occur amoung "Bank Deposits" with "Checking Accounts","Saving Accounts" and "Foreign Currency Account" 
    indicating that Customers Who maintain High Balances in one account often hold substancial amount/funds across other accounts as well.


# Power Query : -

      Total_deposite = sum(Banking[Bank Deposits])+sum(Banking[Saving Accounts])+sum(Banking[Checking Accounts])+sum(Banking[Foreign Currency Account])

      Total_Loan = SUM(Banking[Bank Loans])+SUM(Banking[Business Lending])+SUM(Banking[Credit Card Balance])

      Joined_Bank = YEAR(Banking[Joined Bank])



# Dash-Boards Power-BI 

<img width="806" alt="image" src="https://github.com/user-attachments/assets/e1fa78cb-ac87-4911-849c-7aedcf347f7c" />

<img width="810" alt="image" src="https://github.com/user-attachments/assets/f4d7923b-7066-4498-925b-8c0c110d8f48" />

<img width="810" alt="image" src="https://github.com/user-attachments/assets/d192d269-86a8-4db9-8d04-2eec3ada3de8" />

















      





    
 
            

        
