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

#### 🧰 Find the top 3 products with the highest revenue. 

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
