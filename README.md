# Customer Shopping Behavior Analysis

## Overview

This project is an end-to-end **data analytics project** focused on analyzing customer shopping behavior and identifying patterns in purchasing, customer segments, product performance, discounts, subscriptions, shipping, and revenue.

The project follows a practical analytics workflow:

**Raw Dataset → Python Data Preparation → SQL Analysis → Power BI Dashboard → Business Insights**

Python was used for data inspection, cleaning, and transformation. The cleaned dataset was then prepared for analysis using SQL Server, where business questions were answered using SQL queries. The results were visualized through an interactive Power BI dashboard.

---

## Dataset

The dataset contains **3,900 customer shopping records and 18 original columns**. The attributes cover customer demographics, products, purchases, reviews, subscriptions, shipping, discounts, previous purchases, payment methods, and purchase frequency.

### Key Attributes

* Customer ID
* Age
* Gender
* Item Purchased
* Category
* Purchase Amount
* Location
* Size
* Color
* Season
* Review Rating
* Subscription Status
* Shipping Type
* Discount Applied
* Previous Purchases
* Payment Method
* Frequency of Purchases

---

## Tools & Technologies

| Tool                                    | Purpose                                                          |
| --------------------------------------- | ---------------------------------------------------------------- |
| **Python**                              | Data loading, cleaning, transformation, and analysis preparation |
| **Pandas**                              | Data manipulation and preprocessing                              |
| **Jupyter Notebook**                    | Python development environment                                   |
| **SQL Server**                          | Data storage and SQL analysis                                    |
| **SQL Server Management Studio (SSMS)** | Executing SQL queries                                            |
| **Power BI**                            | Interactive dashboard and data visualization                     |
| **Gamma**                               | Project presentation                                             |

---

# Project Workflow

## 1. Data Loading

The dataset was loaded into a Pandas DataFrame using `pd.read_csv()`.

```python
df = pd.read_csv("customer_shopping_behavior.csv")
```

Initial inspection was performed using:

* `df.head()`
* `df.info()`
* `df.describe(include='all')`
* `df.isnull().sum()`

The initial inspection showed **3,900 records**, with **37 missing values in the Review Rating column**.

---

## 2. Data Cleaning & Preprocessing

### Handling Missing Values

The missing values in `Review Rating` were handled using **median imputation within each product category**.

```python
df['Review Rating'] = df.groupby('Category')['Review Rating'].transform(
    lambda x: x.fillna(x.median())
)
```

This approach preserves differences in review-rating distributions across categories rather than replacing all missing values with one overall median.

After imputation, the dataset contained no missing values.

---

### Standardizing Column Names

The column names were converted to lowercase and spaces were replaced with underscores to improve readability and make them easier to work with in Python and SQL.

The `Purchase Amount (USD)` column was also renamed to `purchase_amount`.

Example:

```text
Customer ID → customer_id
Purchase Amount (USD) → purchase_amount
Review Rating → review_rating
```

---

### Creating Age Groups

A new `age_group` column was created using `pd.qcut()`.

Customers were divided into four groups:

* Young Adult
* Adult
* Middle-Aged
* Senior

```python
labels = ['Young Adult', 'Adult', 'Middle-Aged', 'Senior']

df['age_group'] = pd.qcut(
    df['age'],
    q=4,
    labels=labels
)
```

This creates age segments based on quartiles of the age distribution.

---

### Converting Purchase Frequency

The categorical `frequency_of_purchases` column was converted into a numerical `purchase_frequency_days` column.

For example:

| Purchase Frequency | Days |
| ------------------ | ---: |
| Weekly             |    7 |
| Fortnightly        |   14 |
| Monthly            |   30 |
| Quarterly          |   90 |
| Annually           |  365 |

This transformation makes purchase frequency easier to analyze numerically.

---

### Removing Redundant Data

The `discount_applied` and `promo_code_used` columns were compared.

The comparison showed that both columns contained the same information, so `promo_code_used` was removed as a redundant column.

```python
(df['discount_applied'] == df['promo_code_used']).all()

df = df.drop('promo_code_used', axis=1)
```

---

## 3. Final Dataset Export

After cleaning and transformation, the processed DataFrame was exported as a CSV file:

```python
df.to_csv("customer_shopping_behavior_final.csv", index=False)
```

The `index=False` parameter prevents the Pandas index from being saved as an additional column.

The resulting dataset contains the cleaned original attributes along with the newly created:

* `age_group`
* `purchase_frequency_days`

and excludes the redundant `promo_code_used` column.

---

# 4. SQL Analysis

The cleaned dataset was loaded into the **CUSTOMER_SHOPPING_BEHAVIOUR** database in SQL Server.

SQL was then used to answer business-oriented questions about customer behavior and purchasing patterns.

### Business Questions

The analysis addressed the following questions:

1. **Revenue by Gender**
   What is the total revenue generated by male vs. female customers?

2. **Discounted Purchases**
   Which customers used a discount but still spent more than the average purchase amount?

3. **Product Ratings**
   Which five products have the highest average review rating?

4. **Shipping Analysis**
   How does the average purchase amount compare between Standard and Express shipping?

5. **Subscription Analysis**
   Do subscribed customers spend more? The analysis compares average purchase amount, total revenue, and customer count.

6. **Discount Rate by Product**
   Which five products have the highest percentage of purchases with discounts applied?

7. **Customer Segmentation**
   Customers were segmented into:

   * New
   * Returning
   * Loyal

   based on their number of previous purchases.

8. **Top Products by Category**
   What are the top three most-purchased products within each category?

9. **Repeat Buyers and Subscriptions**
   Are customers with more than five previous purchases also more likely to subscribe?

10. **Revenue by Age Group**
    What is the revenue contribution of each age group?

---

## SQL Concepts Used

The project demonstrates several SQL concepts, including:

* `SELECT`
* `WHERE`
* `GROUP BY`
* `ORDER BY`
* `TOP`
* `SUM()`
* `AVG()`
* `COUNT()`
* `ROUND()`
* `CASE`
* Subqueries
* Common Table Expressions (CTEs)
* `ROW_NUMBER()`
* `PARTITION BY`

For example, `ROW_NUMBER()` with `PARTITION BY` was used to rank products **within each category** and identify the top three products in every category.

---

# 5. Power BI Dashboard

The cleaned and SQL-ready data was used to build an interactive **Power BI dashboard**.

The dashboard focuses on customer shopping behavior and provides visual analysis of areas such as:

* Sales by Category
* Revenue by Age Group
* Customer subscription behavior
* Product performance
* Discount behavior
* Customer segmentation
* Purchase patterns

Interactive slicers allow users to filter the dashboard and explore the data from different perspectives.

---

# 6. Key Business Insights

The project is designed to help answer questions such as:

* Which customer groups contribute the most revenue?
* Which product categories perform best?
* Which products receive the strongest customer ratings?
* How do subscribed and non-subscribed customers differ?
* How frequently do customers make purchases?
* Which products are most frequently purchased?
* How heavily are discounts used?
* Are repeat customers more likely to subscribe?
* Which age groups contribute the most revenue?

The SQL analysis and Power BI dashboard together provide a business-oriented view of customer purchasing behavior.

---

# 7. Project Deliverables

The project consists of:

* **Python Notebook** – Data loading, inspection, cleaning, and transformation
* **Cleaned CSV Dataset** – Final processed dataset
* **SQL Query File** – Business analysis using SQL Server
* **Power BI Dashboard** – Interactive visualization and reporting
* **Project Report** – Documentation of the methodology and findings
* **Gamma Presentation** – Project presentation

---

# 8. Project Structure

```text
Customer-Shopping-Behavior/
│
├── data/
│   ├── customer_shopping_behavior.csv
│   └── customer_shopping_behavior_final.csv
│
├── python/
│   └── Customer_Trends_DataAnalysis_Py.ipynb
│
├── sql/
│   └── Customer_Shopping_Behavior_query.sql
│
├── powerbi/
│   └── Customer_Shopping_Behavior.pbix
│
├── report/
│   └── Project_Report.pdf
│
├── presentation/
│   └── Project_Presentation.pdf
│
└── README.md
```

---

# 9. How to Run

### Step 1 — Python

Open the Jupyter Notebook:

`Customer_Trends_DataAnalysis_Py.ipynb`

Run the notebook to:

1. Load the raw dataset.
2. Inspect the data.
3. Identify missing values.
4. Impute missing review ratings.
5. Standardize column names.
6. Create age groups.
7. Convert purchase frequency into days.
8. Remove redundant columns.
9. Export the final dataset.

The final file will be generated as:

`customer_shopping_behavior_final.csv`

---

### Step 2 — SQL Server

Open **SQL Server Management Studio (SSMS)**.

1. Create/open the `CUSTOMER_SHOPPING_BEHAVIOUR` database.
2. Load the cleaned dataset into the `CUSTOMER_SHOPPING_BEHAVIOR` table.
3. Execute the SQL queries in:

`Customer_Shopping_Behavior_query.sql`

The queries generate the business analysis described above.

---

### Step 3 — Power BI

Open the Power BI `.pbix` file.

Connect the report to the SQL Server database if required and refresh the data.

Use the dashboard visuals and slicers to explore customer shopping behavior.

---

# 10. Conclusion

This project demonstrates an end-to-end data analytics workflow using **Python, SQL Server, and Power BI**.

Python was used to prepare and transform the raw customer shopping data. SQL Server was then used to perform structured business analysis, while Power BI was used to convert the results into an interactive dashboard.

The project demonstrates practical skills in:

* Data cleaning
* Data transformation
* Exploratory data analysis
* SQL querying
* Customer segmentation
* Data visualization
* Business-oriented analysis
* Dashboard development

The overall objective is to transform raw customer shopping data into meaningful insights that can support better understanding of customer behavior and purchasing patterns.
