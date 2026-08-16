# 🛍️ Consumer Shopping Behavior Analysis

> **An end-to-end data analytics project using Python, Microsoft SQL Server, and Power BI to uncover customer purchasing patterns, segment customers, and deliver business-ready insights through an interactive dashboard.**

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-Data%20Processing-013243?logo=numpy)](https://numpy.org/)
[![SQL Server](https://img.shields.io/badge/Microsoft%20SQL%20Server-T--SQL-CC2927?logo=microsoftsqlserver)](https://www.microsoft.com/sql-server)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi)](https://powerbi.microsoft.com/)

---

# 📌 Project Overview

Understanding **who buys, what they buy, how frequently they purchase, and what influences their spending** is critical for making informed retail decisions.

This project analyzes **3,900 customer transactions across 18 attributes** to identify:

* Customer purchasing patterns
* Revenue contribution across customer segments
* Discount and promotional behavior
* Product and category performance
* Seasonal purchasing trends
* Subscription behavior
* Customer loyalty patterns
* Spending differences across demographic groups

The project follows a complete analytics pipeline:

```text
Raw CSV Dataset
       ↓
Python Data Cleaning
       ↓
Feature Engineering
       ↓
MSSQL Server
       ↓
T-SQL Business Analysis
       ↓
Power BI Data Model
       ↓
Interactive Dashboard
       ↓
Business Insights
```

### 🎯 Business Objective

> **Transform raw customer transaction data into actionable insights that can help stakeholders understand customer behavior, identify valuable segments, and improve retention, promotions, and revenue strategies.**

---

# 📂 Dataset

**Dataset:** `customer_shopping_behavior.csv`

| Attribute      | Details                    |
| -------------- | -------------------------- |
| Records        | 3,900                      |
| Columns        | 18                         |
| Domain         | Retail / Consumer Shopping |
| Primary Unit   | Customer transaction       |
| Revenue Metric | Purchase Amount (USD)      |

### Key Fields

* `Customer ID`
* `Age`
* `Gender`
* `Item Purchased`
* `Category`
* `Purchase Amount (USD)`
* `Location`
* `Season`
* `Review Rating`
* `Subscription Status`
* `Discount Applied`
* `Promo Code Used`
* `Previous Purchases`
* `Payment Method`
* `Frequency of Purchases`
* `Shipping Type`

---

# 🧠 Analytical Approach

The project was designed around three major analytical layers:

### Layer 1 — Python

Used for:

* Data inspection
* Data cleaning
* Missing-value treatment
* Data standardization
* Feature engineering

### Layer 2 — MSSQL Server

Used for:

* Structured data storage
* T-SQL analysis
* Customer segmentation
* Revenue analysis
* Product analysis
* Loyalty analysis

### Layer 3 — Power BI

Used for:

* KPI reporting
* Customer segmentation
* Revenue analysis
* Trend analysis
* Interactive stakeholder reporting

---

# 🧹 1. Data Cleaning

The raw dataset was first inspected using Pandas.

### Initial Data Inspection

Used:

```python
df.info()
df.describe(include="all")
```

to understand:

* Data types
* Missing values
* Numerical distributions
* Categorical variables
* Potential data-quality issues

---

## Missing Value Treatment

The dataset contained **37 missing values in `Review Rating`**.

Instead of replacing all missing ratings with a single global median, category-level imputation was used.

### Approach

```text
Product Category
       ↓
Calculate category median rating
       ↓
Fill missing ratings
       ↓
Preserve category-specific patterns
```

This approach is preferable because customer ratings can have different distributions across product categories.

---

# 🧹 Column Standardization

Original column names contained spaces, special characters, and inconsistent naming conventions.

For example:

```text
Purchase Amount (USD)
```

was transformed into:

```text
purchase_amount
```

Other columns were similarly converted to **snake_case**.

### Benefits

* Cleaner Python code
* Easier SQL querying
* Consistent naming conventions
* Better Power BI field usability
* Reduced risk of column-reference errors

---

# 🔍 Redundant Feature Detection

During data-quality analysis, it was identified that:

```text
discount_applied
```

and

```text
promo_code_used
```

contained identical information across all records.

Since both columns represented the same information, the redundant:

```text
promo_code_used
```

column was removed.

### Analytical Principle

> **More columns do not necessarily mean more information.**

Removing redundant variables reduces noise and keeps the analytical dataset easier to maintain.

---

# ⚙️ 2. Feature Engineering

Two major analytical features were created.

---

## 👥 Age Group

Customers were divided into four age segments using `pd.qcut()`:

```text
Young Adult
Adult
Middle-aged
Senior
```

Using quartile-based segmentation creates approximately balanced groups based on the distribution of customer ages.

This enables comparison of:

* Revenue
* Purchase behavior
* Product preferences
* Subscription behavior

across customer age segments.

---

## 📅 Purchase Frequency Days

The original `frequency_of_purchases` column contained categorical values such as:

```text
Weekly
Bi-Weekly
Monthly
Quarterly
Annually
```

These categories were mapped to numerical day values.

Example:

```text
Weekly      → 7
Bi-Weekly   → 14
Monthly     → 30
Quarterly   → 90
Annually    → 365
```

### Why?

Converting categorical purchase frequency into numerical values allows the dataset to support additional calculations such as:

* Expected annual purchase frequency
* Customer lifetime value estimation
* Purchase cadence analysis
* Customer activity scoring

---

# 🗄️ 3. MSSQL Server Integration

After cleaning and feature engineering, the processed dataset was loaded into **Microsoft SQL Server**.

### Technology

```text
Python
   ↓
SQLAlchemy
   ↓
pyodbc
   ↓
Microsoft SQL Server
```

The cleaned DataFrame was loaded into:

```text
Database: customer_behavior
Table: customer
```

using:

```python
df.to_sql()
```

This created a centralized SQL layer where business questions could be answered using **T-SQL**.

---

# 🧮 4. SQL Business Analysis

A total of **10 business-focused T-SQL queries** were developed.

The goal was not simply to practice SQL syntax, but to answer realistic business questions.

|  # | Business Question                                                                 |
| -: | --------------------------------------------------------------------------------- |
|  1 | How much revenue is generated by male vs. female customers?                       |
|  2 | Which customers used discounts but still spent above the average purchase amount? |
|  3 | Which 5 products have the highest average review ratings?                         |
|  4 | How does average purchase amount differ between Standard and Express shipping?    |
|  5 | How do spending and revenue compare between subscribers and non-subscribers?      |
|  6 | Which 5 products have the highest percentage of discounted purchases?             |
|  7 | How do New, Returning, and Loyal customers compare?                               |
|  8 | What are the top 3 most purchased products within each category?                  |
|  9 | How likely are repeat buyers to subscribe?                                        |
| 10 | How much revenue does each age group contribute?                                  |

---

# 👥 Customer Segmentation

Customers were classified into three behavioral segments based on previous purchases:

```text
New Customer
     ↓
Returning Customer
     ↓
Loyal Customer
```

This segmentation makes it possible to compare:

* Spending
* Revenue contribution
* Subscription status
* Purchase frequency
* Customer behavior

across different levels of customer loyalty.

---

# 📊 5. Power BI Dashboard

An interactive Power BI dashboard was created using the processed customer data stored in MSSQL Server.

### Dashboard Focus Areas

#### 💰 Revenue KPIs

* Total Revenue
* Average Order Value
* Discount Usage Rate

#### 👥 Customer Segmentation

* New Customers
* Returning Customers
* Loyal Customers

#### 🛒 Product Analysis

* Revenue by Category
* Product purchasing patterns
* Top-performing products

#### 👤 Customer Demographics

* Revenue by Gender
* Revenue by Age Group

#### 📅 Behavioral Trends

* Seasonal purchasing trends
* Shipping type comparison
* Subscription behavior
* Discount usage

---

# 📈 Dashboard Architecture

```text
                    MSSQL Server
                         │
                         ▼
                  customer table
                         │
                         ▼
                    Power BI
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Revenue       Customers       Products
          │              │              │
          ▼              ▼              ▼
       KPIs          Segments        Categories
```

---

# 💡 Key Insights

## 1. Category-Aware Missing Value Imputation

Missing review ratings were not treated as a simple global-median problem.

Using category-level medians preserved differences in rating behavior between product categories.

### Takeaway

> Data-cleaning decisions can directly affect the reliability of downstream analysis.

---

## 2. Redundant Variables Can Add Noise

`discount_applied` and `promo_code_used` contained identical information.

Removing the redundant field resulted in a cleaner analytical dataset without sacrificing information.

### Takeaway

> Feature selection is an important part of data preparation, even when building descriptive dashboards.

---

## 3. Purchase Frequency Can Become an Analytical Metric

Converting purchase-frequency categories into numerical day values creates opportunities for deeper analysis.

For example:

```text
Frequency
    ↓
Days Between Purchases
    ↓
Estimated Purchases / Year
    ↓
Customer Value Analysis
```

This creates a foundation for future **customer lifetime value (CLV)** and retention analysis.

---

## 4. Customer Loyalty Enables More Targeted Analysis

Separating customers into:

```text
New
Returning
Loyal
```

allows businesses to compare customer value and engagement across lifecycle stages.

This can support differentiated strategies such as:

* New customer onboarding
* Returning customer incentives
* Loyalty rewards
* Subscription campaigns

---

# 🎯 Business Recommendations

Based on the analytical framework, the business could:

### 1. Develop Customer Lifecycle Strategies

Create different marketing strategies for:

```text
New → Returning → Loyal
```

rather than treating all customers identically.

### 2. Optimize Discount Strategies

Identify customers who:

* Frequently use discounts
* Still generate high purchase values
* Respond positively to promotions

This can help improve promotional targeting.

### 3. Strengthen Subscription Conversion

Analyze repeat buyers and identify customers with high purchase frequency who are not yet subscribed.

These customers may represent strong candidates for subscription campaigns.

### 4. Focus on High-Value Customer Segments

Use revenue and purchase behavior to identify customers who contribute disproportionately to overall revenue.

These customers can be targeted with:

* Loyalty programs
* Personalized offers
* Early product access
* Premium services

### 5. Use Seasonal Trends for Campaign Planning

Seasonal purchasing behavior can help businesses plan:

* Inventory
* Marketing campaigns
* Promotions
* Product launches

---

# 🛠️ Technology Stack

## Programming & Data Processing

* Python
* Pandas
* NumPy

## Database

* Microsoft SQL Server
* T-SQL
* SQLAlchemy
* pyodbc

## Business Intelligence

* Microsoft Power BI

## Analytical Techniques

* Data cleaning
* Missing-value imputation
* Feature engineering
* Customer segmentation
* Aggregation analysis
* Revenue analysis
* Product analysis
* Behavioral analysis
* KPI development
* Business intelligence reporting

---

# 📁 Project Structure

```text
consumer-shopping-behavior/
│
├── 📓 Customer_Shopping_Behavior_Analysis.ipynb
│   └── Data cleaning + feature engineering
│
├── 🐍 mssql_load.py
│   └── Loads processed data into MSSQL Server
│
├── 📄 customer_shopping_behavior.csv
│   └── Raw dataset
│
├── 🗄️ sql_queries_mssql.sql
│   └── Business analysis queries
│
├── 📊 dashboard.pbix
│   └── Power BI dashboard
│
└── 📖 README.md
```

---

# 🚀 How to Run

## 1. Clone the Repository

```bash
git clone <your-repository-url>
cd consumer-shopping-behavior
```

## 2. Install Python Dependencies

```bash
pip install pandas numpy sqlalchemy pyodbc
```

## 3. Prepare SQL Server

Make sure you have:

* Microsoft SQL Server
* SQL Server Management Studio (SSMS) or Azure Data Studio
* ODBC Driver 17 or 18 for SQL Server

Create the database:

```sql
CREATE DATABASE customer_behavior;
```

---

## 4. Add the Dataset

Place:

```text
customer_shopping_behavior.csv
```

inside the project directory.

---

## 5. Run the Python Analysis

Open:

```text
Customer_Shopping_Behavior_Analysis.ipynb
```

and execute the notebook.

The notebook performs:

```text
Data Inspection
      ↓
Data Cleaning
      ↓
Missing Value Treatment
      ↓
Column Standardization
      ↓
Feature Engineering
```

---

## 6. Load Data Into MSSQL

Update the SQL Server connection configuration inside:

```text
mssql_load.py
```

Then run:

```bash
python mssql_load.py
```

The cleaned data will be loaded into:

```text
customer_behavior
      └── customer
```

---

## 7. Run SQL Analysis

Open:

```text
sql_queries_mssql.sql
```

in SSMS and execute the queries against the `customer` table.

---

## 8. Open the Power BI Dashboard

Open:

```text
dashboard.pbix
```

in Power BI Desktop.

Connect the dashboard to the MSSQL:

```text
Database: customer_behavior
Table: customer
```

Refresh the dataset to load the latest data.

---

# 🔄 End-to-End Data Pipeline

The complete project workflow can be summarized as:

```text
┌──────────────────────────┐
│  Raw Customer Dataset    │
│        CSV File          │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│        Python            │
│ Pandas + NumPy           │
│                          │
│ • Data Cleaning          │
│ • Missing Values         │
│ • Feature Engineering    │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│    Microsoft SQL Server  │
│                          │
│ • Data Storage           │
│ • T-SQL Analysis         │
│ • Segmentation           │
│ • Business Questions     │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│        Power BI          │
│                          │
│ • KPIs                   │
│ • Customer Segments      │
│ • Revenue Analysis       │
│ • Trends                 │
│ • Interactive Dashboard  │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│    Business Insights     │
│                          │
│ • Customer Strategy      │
│ • Promotion Strategy     │
│ • Loyalty Strategy       │
│ • Revenue Opportunities  │
└──────────────────────────┘
```

---

# ⚠️ Data & Project Considerations

* The dataset is primarily transactional and does not represent a complete customer lifecycle.
* Customer lifetime value is not directly calculated.
* Marketing campaign costs are not included.
* Revenue analysis is based on purchase amounts in the available dataset.
* Observed relationships should not automatically be interpreted as causal relationships.
* The Power BI dashboard is designed for descriptive and diagnostic analysis rather than predictive modeling.

---

# 🔮 Future Improvements

This project can be extended into a more advanced customer analytics solution.

### 🤖 1. Customer Lifetime Value

Develop an estimated CLV metric using:

```text
Purchase Frequency
×
Average Purchase Value
×
Expected Customer Lifetime
```

### 📊 2. Customer RFM Analysis

Introduce:

* **Recency**
* **Frequency**
* **Monetary Value**

to identify high-value and at-risk customers.

### 🎯 3. Customer Churn Prediction

Build a machine-learning model using:

* Logistic Regression
* Random Forest
* Gradient Boosting
* XGBoost

to predict customer churn or inactivity.

### 🧠 4. Customer Propensity Modeling

Predict the likelihood that a customer will:

* Subscribe
* Purchase again
* Respond to discounts
* Become a loyal customer

### 📈 5. Advanced Power BI Dashboard

Add:

* Drill-through pages
* Customer-level analysis
* Dynamic segmentation
* What-if parameters
* Tooltip pages
* Time-based comparisons
* Advanced DAX measures

---

# 🎓 Skills Demonstrated

This project demonstrates practical experience across the complete **Data Analyst workflow**:

### Python

* Pandas
* NumPy
* Data cleaning
* Missing-value treatment
* Feature engineering
* Exploratory analysis

### SQL

* Microsoft SQL Server
* T-SQL
* Aggregations
* GROUP BY
* CASE statements
* Subqueries
* Customer segmentation
* Business-oriented querying

### Power BI

* KPI development
* Dashboard design
* Customer segmentation
* Revenue analysis
* Interactive reporting
* Stakeholder-oriented visualization

### Business Analytics

* Translating business questions into SQL queries
* Identifying customer segments
* Revenue analysis
* Promotional analysis
* Customer behavior analysis
* Converting data findings into recommendations

---

# 👨‍💻 Author

**Jagapathi Tankala**

Aspiring **Data Analyst**

**Skills:** Python • SQL • Power BI • Excel • Data Analytics

---

## ⭐ Project Highlights

```text
3,900+
Customer Records

18
Original Attributes

10
Business Questions

3
Analytics Technologies

1
End-to-End Data Pipeline
```

> **Python → SQL Server → Power BI → Business Insights**
