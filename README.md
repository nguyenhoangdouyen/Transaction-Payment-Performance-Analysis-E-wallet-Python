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

### 💡 **Summary**

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

