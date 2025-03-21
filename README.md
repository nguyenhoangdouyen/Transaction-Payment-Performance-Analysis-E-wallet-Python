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
