# 📊 Project Title: Transaction Payment Performance Analysis For E-Wallet | Python

**Author:** Nguyen Hoang Do Uyen

**Date:** March 2025

**Tools Used:** Python

## 📑 Table of Contents

📌 Background & Overview

📂 Dataset Description & Data Structure

🔎 Final Conclusion & Recommendations

## 📖 Background & Overview

### Objective:
This project aims to analyze transaction and payment data within an e-wallet system to:

✔️ Understand payment and transaction trends.  
✔️ Detect anomalies in product ownership and team performance.  
✔️ Identify key contributors to refunds and transaction volume.  
✔️ Categorize transactions into meaningful types for further analysis.  

### 👤 Who is this project for?

✔️ Data analysts & business analysts  
✔️ Decision-makers & stakeholders in the fintech industry  
✔️ Product teams optimizing e-wallet services  

## 📂 Dataset Description & Data Structure

### 📌 Data Source
- **Source**: Internal company database (e-wallet transaction records)  
- **Size**:  
  - `product.csv`: 493 rows × 3 columns  
  - `payment_report.csv`: 920 rows × 5 columns  
  - `transactions.csv`: 1,324,002 rows × 9 columns  
- **Format**: `.csv`  

---

### 📊 Data Structure & Relationships

#### 1️⃣ Tables Used:
This project utilizes **three** datasets:

1. **product.csv** – Product details, including category and team ownership.  
2. **payment_report.csv** – Monthly payment volume and source tracking.  
3. **transactions.csv** – Comprehensive transaction records.  

**Key Relationships:**
- `product_id` links **product.csv** and **payment_report.csv**.  
- `transaction_id` is the unique key in **transactions.csv** for tracking payments.  
- `source_id` in **payment_report.csv** contributes to refund analysis.  

---

#### 2️⃣ Table Schema & Data Snapshot

##### Table 1: Products Table (`product.csv`) - *493 rows, 3 columns*

👉🏻 **Schema:**
| Column Name  | Data Type | Description |
|-------------|----------|-------------|
| product_id  | INT      | Unique identifier for each product |
| category    | TEXT     | Product category |
| team_own    | TEXT     | Team responsible for the product |

---

##### Table 2: Payment Report (`payment_report.csv`) - *920 rows, 5 columns*

👉🏻 **Schema:**
| Column Name    | Data Type | Description |
|---------------|----------|-------------|
| report_month  | DATE     | Month of the payment report |
| payment_group | TEXT     | Type of payment (e.g., refund, purchase) |
| product_id    | INT      | Associated product ID |
| source_id     | INT      | Source of the transaction |
| volume        | FLOAT    | Total payment volume |

---

##### Table 3: Transactions (`transactions.csv`) - *1,324,002 rows, 9 columns*

👉🏻 **Schema:**
| Column Name    | Data Type | Description |
|---------------|----------|-------------|
| transaction_id | INT      | Unique transaction identifier |
| merchant_id    | INT      | Merchant involved in the transaction |
| volume         | FLOAT    | Transaction amount |
| transType      | INT      | Type of transaction |
| transStatus    | TEXT     | Status of the transaction (e.g., completed, failed) |
| sender_id      | INT      | Sender of the transaction |
| receiver_id    | INT      | Receiver of the transaction |
| extra_info     | TEXT     | Additional details about the transaction |
| timeStamp      | TIMESTAMP | Time when the transaction occurred |

---

## ⚒️ Main Process

### 1️⃣ Data Cleaning & Preprocessing 

#### Step 1. Import library

```python
import numpy as np
import matlotblib as plt
import seaborn as sns
import pandas as pd
```

#### Step 2. Load dataset 

```python
#Load data to Colab
from google.colab import drive
drive.mount('/content/drive')

# Import file csv to Colab
import pandas as pd
payment = pd.read_csv('/content/drive/MyDrive/Python_Project2/payment_report.csv', encoding='utf-8')
product = pd.read_csv('/content/drive/MyDrive/Python_Project2/product.csv', encoding='utf-8')
transaction = pd.read_csv('/content/drive/MyDrive/Python_Project2/transactions.csv', encoding='utf-8')
```

#### Step 3. Display the first 5 rows of the product table

[In 1]:

```python
payment.head()
product.head()
transaction.head()
```

[Out 1]:

![Image](https://github.com/user-attachments/assets/0bec70d6-555d-44ce-ac1e-50014ac0d3c5)

![Image](https://github.com/user-attachments/assets/e22287f2-a0db-47e4-999b-3db02a27700b)

![Image](https://github.com/user-attachments/assets/1a09c80e-4e99-4cd7-8cb2-bba3085aad51)


#### Step 4. Checked Dataset Structure

1. **payment**: 919 rows, 5 columns  
   - `report_month`: Object, `payment_group`: Object, `product_id`: Int64, `source_id`: Int64, `volume`: Int64  

2. **product**: 492 rows, 3 columns  
   - `product_id`: Int64, `category`: Object, `team_own`: Object  

3. **transaction**: 1,324,002 rows, 9 columns  
   - `transaction_id`: Int64, `merchant_id`: Int64, `volume`: Int64, `transType`: Int64, `transStatus`: Int64, `sender_id`: Float64, `receiver_id`: Float64, `extra_info`: Object, `timeStamp`: Int64

📝 **Checked for Missing Values**

- Missing values were detected in the **transaction** table:
  - **sender_id**: 49,059 missing values
  - **receiver_id**: 164,795 missing values
  - **extra_info**: 1,317,907 missing values

No missing values were found in the **payment** or **product** tables.

📝 **Checked for Duplicates**

- **Payment table**: No duplicates were found.
- **Product table**: No duplicates were found.
- **Transaction table**: 28 duplicate rows were identified.

## 💡 **Summary**

#### Data Type Review:
1. **payment**:
   - Convert the `report_month` column from `object` to `datetime` if it contains date or month information.

2. **product**:
   - Convert the `category` and `team_own` columns from `object` to `category` to optimize memory usage and improve performance.

3. **transaction**:
   - Convert the `sender_id` and `receiver_id` columns from `float64` to `int64`, provided there are no missing values (NaN).
   - Convert the `timeStamp` column from `int64` to `datetime` if it represents a Unix timestamp.

#### Data Quality Observations:
- The **payment** and **product** tables are clean with no missing values or duplicates.
- The **transaction** table has missing values in the **sender_id**, **receiver_id**, and **extra_info** columns, which should be addressed prior to further analysis.
- There are **28 duplicate rows** in the **transaction** table, which should be removed to maintain data integrity.
- The outliers in the volume column of both the **transaction** and **payment tables** are considered valid and meaningful. Therefore, **no outlier removal will be performed** in this case, as these values align with the data’s expected behavior and are essential for analysis.

### 2️⃣ Exploratory Data Analysis (EDA)

📝 **Handle Missing Values**

[In 2]:

```python
# The sender_id and receiver_id columns in the Transaction table are not important, so missing values are replaced with 'Unknow' for consistency
transaction['sender_id'] = transaction['sender_id'].fillna(value='Unknow')
transaction['receiver_id'] = transaction['receiver_id'].fillna(value='Unknow')

# Drop the 'extra_info' column from the Transaction table
transaction.drop('extra_info', axis=1, inplace=True)
```

📝 **Handle Duplicate**

[In 3]:

```python
#Drop duplicate
transaction.drop_duplicates(subset=None, keep='first')
```

📝 **Convert Data type**

[In 4]:
```python
# 1. Convert the report_month column from object to datetime
payment['report_month'] = pd.to_datetime(payment['report_month'], errors='coerce')

# 2. product: Convert 'category' and 'team_own' columns from object to category
product_columns = ['category', 'team_own']
for col in product_columns:
    product[col] = product[col].astype('category')

# 3. transaction: Convert 'sender_id' and 'receiver_id' columns from float64 to int64, and 'timeStamp' from int64 to datetime
transaction_columns_int64 = ['sender_id', 'receiver_id']
for col in transaction_columns_int64:
    transaction[col] = transaction[col].fillna('Unknow').astype(str)

transaction['timeStamp'] = pd.to_datetime(transaction['timeStamp'], errors='coerce')
```

### 3️⃣ Python Analysis

🧰 **Merge payment and product DataFrames**

[In 5]:

```python
# Merge payment and product DataFrames
payment_product = payment.merge(product, how='left', on='product_id')
```

#### ✅ Find the top 3 products with the highest revenue

Find the top 3 products with the highest revenue to identify which products are generating the most income. This helps in focusing on the highest-performing products for sales and marketing strategies.

[In 6]:

```python
# Calculate volume by product_id
volume_by_product = payment_product.groupby('product_id')['volume'].agg('sum').reset_index()

# Sort volume in descending order
volume_by_product.sort_values(by='volume', ascending=False)

# Filter the top 3 product_ids with the highest volume
top_3_productid = volume_by_product.head(3)

# Print the results
print("Top 3 product_ids with the highest volume:")
print(top_3_productid)
```

[Out 6]:

![Image](https://github.com/user-attachments/assets/bc5a3949-cd1d-4abb-8bfd-5a71278da524)


#### ✅ Verify whether each product is managed by only one team

Check whether each product is managed by a single team to ensure clear ownership and avoid potential conflicts in responsibility.

[In 7]:

```python
# Group by product_id and count the number of unique team_own values
products_by_teamown = payment_product.groupby('product_id')['team_own'].nunique()

# Identify product_id entries managed by more than one team
abnormal_products = products_by_teamown[products_by_teamown > 1]

# Print the results
if abnormal_products.empty:
  print('No abnormal products')
else:
  print('There are abnormal products:')
  print(abnormal_products)
```

[Out 7]:

![Image](https://github.com/user-attachments/assets/07ebae37-3d17-44b2-a5b2-b19b85a579c2)

#### ✅ Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.

Identify the team with the lowest performance in terms of total transaction volume since Q2 2023. Once the lowest-performing team is found, determine the product category that contributes the least to that team's total volume. This analysis helps pinpoint underperforming teams and categories, providing insights for potential improvements or strategic decisions.

[In 8]:

```python
# 1. Find the team with the lowest performance (lowest volume) since Q2.2023

# Filter data from Q2.2023 onwards
payment_product_Q2 = payment_product[payment_product['report_month'] >= '2023-04-01']

# Calculate the total volume for each team in Q2.2023
volume_by_team = payment_product_Q2.groupby('team_own', observed=False)['volume'].agg('sum').reset_index()

# Sort teams by volume in ascending order
volume_by_team.sort_values(by='volume', ascending=True)

# Get the team with the lowest volume
lowest_team_name = volume_by_team.loc[volume_by_team['volume'] == volume_by_team['volume'].min(), 'team_own'].iloc[0]
lowest_volume = volume_by_team.loc[volume_by_team['volume'] == volume_by_team['volume'].min(), 'volume'].iloc[0]

# Print the result
print(f"The lowest performance team in Q2.2023 is {lowest_team_name} with volume = {lowest_volume}")

# 2. Find the category that contributes the least to the lowest-performing team

# Filter data for the lowest-performing team (APS) in Q2
volume_lowest_team_Q2 = payment_product_Q2[payment_product_Q2['team_own'] == 'APS']

# Calculate total volume by category for the lowest-performing team
vol_by_cat_APS = volume_lowest_team_Q2.groupby('category', observed=False)['volume'].agg('sum').reset_index()

# Sort categories by volume in ascending order
vol_by_cat_APS.sort_values(by='category', ascending=True)

# Get the category with the lowest volume contribution
lowest_cat = vol_by_cat_APS.loc[vol_by_cat_APS['volume'] == vol_by_cat_APS['volume'].min(), 'category'].iloc[0]
lowest_vol = vol_by_cat_APS.loc[vol_by_cat_APS['volume'] == vol_by_cat_APS['volume'].min(), 'volume'].iloc[0]

# Print the result
print(f"The lowest contributing category to the team APS is {lowest_cat} with volume = {lowest_vol}")
```

[Out 8]:

![Image](https://github.com/user-attachments/assets/13941955-b53e-44d7-991c-0f1571345689)

#### ✅ Determine the contribution of source_ids in refund transactions (where payment_group = 'refund') and identify the source_id with the highest contribution.

The goal is to analyze the distribution of refund transactions across different source_ids and identify which source_id has the highest contribution. This helps in understanding refund patterns and potential areas for improvement.

[In 9]:

```python
# Filter transactions that are refunds
refund = payment_product[payment_product['payment_group'] == 'refund']

# Calculate total volume by each source_id
volume_by_id = refund.groupby('source_id')['volume'].agg('sum').reset_index()

# Calculate the percentage contribution of each source_id
volume_by_id['%_contribute'] = volume_by_id['volume'] / volume_by_id['volume'].sum() * 100

# Print the result
print(volume_by_id)

# Sort contributions in descending order
volume_by_id.sort_values(by='%_contribute', ascending=False)

# Get the source_id with the highest contribution
source_id = volume_by_id.loc[volume_by_id['volume'] == volume_by_id['volume'].max(), 'source_id'].iloc[0]
contribute = volume_by_id.loc[volume_by_id['volume'] == volume_by_id['volume'].max(), '%_contribute'].iloc[0]

print(f"{source_id} is the highest contributor to refund transactions with {contribute}%")
```

[Out 9]:

![Image](https://github.com/user-attachments/assets/659ccf8a-4b2f-4910-89c5-a2e0ff2d9ede)

#### ✅ Define the transaction type for each row based on the given conditions:

- **For transType = 2 and merchant_id = 1205:** Bank Transfer Transaction
- **For transType = 2 and merchant_id = 2260:** Withdraw Money Transaction
- **For transType = 2 and merchant_id = 2270:** Top Up Money Transaction
- **For transType = 2 and other merchant_id:** Payment Transaction
- **For transType = 8 and merchant_id = 2250:** Transfer Money Transaction
- **For transType = 8 and other merchant_id:** Split Bill Transaction
- **All remaining cases** are considered **invalid transactions**.

The purpose of this task is to categorize each transaction into a specific transaction type based on the given conditions. By doing so, we can clearly identify the type of each transaction in the dataset, which helps in analyzing and understanding transaction patterns, processing specific transactions, and detecting any anomalies or invalid transactions. 

[In 10]:

```python
# Define conditions for transaction types
condition = [
    (transaction['transType'] == 2) & (transaction['merchant_id'] == 1205),  # Bank Transfer Transaction
    (transaction['transType'] == 2) & (transaction['merchant_id'] == 2260),  # Withdraw Money Transaction
    (transaction['transType'] == 2) & (transaction['merchant_id'] == 2270),  # Top Up Money Transaction
    (transaction['transType'] == 2) & (~transaction['merchant_id'].isin([2270, 2260, 1205])),  # Payment Transaction
    (transaction['transType'] == 8) & (transaction['merchant_id'] == 2250),  # Transfer Money Transaction
    (transaction['transType'] == 8) & (transaction['merchant_id'] != 2250),  # Split Bill Transaction
]

# Define the corresponding transaction types
type_of_transactions = [
    'Bank Transfer Transaction',
    'Withdraw Money Transaction',
    'Top Up Money Transaction',
    'Payment Transaction',
    'Transfer Money Transaction',
    'Split Bill Transaction'
]

# Assign transaction types based on conditions
transaction['transaction_type'] = np.select(condition, type_of_transactions, default='Invalid Transaction')
```

#### ✅ Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers

This helps in understanding the distribution of transactions across different types, as well as the engagement of unique senders and receivers for each type of transaction.

[In 11]:
```python
# Remove invalid transactions
valid_transaction = transaction[transaction['transaction_type'] != 'Invalid Transaction']

# Group by transaction type and aggregate the required metrics
final = valid_transaction.groupby('transaction_type').agg(
    num_trans = ('transaction_id', 'count'),  # Count of transactions
    num_volume = ('volume', 'sum'),  # Sum of volumes
    num_sender = ('sender_id', 'nunique'),  # Count of unique senders
    num_receiver = ('receiver_id', 'nunique')  # Count of unique receivers
).reset_index()

# Print the final result
print(final)
```

[Out 11]:

![Image](https://github.com/user-attachments/assets/1d5a65cb-a9e5-4cee-b06c-9f988e2ea357)

## 🔎 Final Conclusion & Recommendations

### Findings:

1. **Top 3 Products with Highest Volume**:
   - **Product ID 15**: 4,206,315,258
   - **Product ID 12**: 1,934,440,830
   - **Product ID 3**: 6,000

2. **Team Ownership of Products**:
   - No abnormal products (each product is handled by one team).

3. **Lowest Performance Team in Q2.2023**:
   - **Team APS** with volume = 51,141,753
   - **Category PXXXXXB** contributes 0 volume.

4. **Highest Contributor in Refund Transactions**:
   - **Source ID 38** contributes 59.11% of refund volume.

5. **Transaction Types**:
   - **Bank Transfer**: 37,879 transactions, volume = 50.6B
   - **Payment**: 398,677 transactions, volume = 71.85B
   - **Split Bill**: 1,376 transactions, volume = 4.9M
   - **Top Up**: 290,502 transactions, volume = 108.6B
   - **Transfer**: 341,177 transactions, volume = 37B
   - **Withdraw**: 33,725 transactions, volume = 23.42B

---

### Recommendations:

1. **Product Focus**: 
   - Prioritize **Product ID 15** as it drives the most volume.
   - Investigate ways to boost **Product ID 3**, which has low volume.

2. **Team APS**:
   - APS has the lowest performance in Q2.2023. Investigate and optimize their operations, especially in the **PXXXXXB category**, which shows no contribution.

3. **Refunds**:
   - **Source ID 38** is the biggest contributor to refunds. Investigate potential issues related to this source to reduce refund volume.

4. **Transaction Optimization**:
   - Focus on improving the **Payment** and **Top Up** transactions as they have the highest volume.
   - Consider increasing engagement for **Split Bill Transactions** to boost volume.

5. **Sender & Receiver Engagement**:
   - Pay attention to enhancing the experience for senders and receivers, especially for high-volume transactions like **Payment** and **Top Up**.
