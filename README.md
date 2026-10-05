# Power BI Assignment 1: E-Commerce Sales Analysis

An end-to-end Power BI data transformation, data modeling, and reporting project focused on e-commerce sales performance, profitability analysis, and target tracking.

---

## 📌 Project Overview

This project demonstrates the complete workflow for processing e-commerce transactional data using **Power BI Desktop** and **Power Query Editor**. The project transforms raw transactional data, handles data quality issues, creates custom calculated fields, aggregates performance metrics, and establishes a relational star schema data model.

---

## 📁 Source Datasets

The analysis is based on three core CSV files:

* **`List of Orders.csv`**: Contains order metadata including `Order ID`, `Order Date`, `CustomerName`, `State`, and `City`.
* **`Order Details.csv`**: Contains line-item details including `Order ID`, `Amount`, `Profit`, `Quantity`, `Category`, and `Sub-Category`.
* **`Sales target.csv`**: Contains target benchmarks structured by `Month of Order Date` and `Category`.

---

## 🛠️ Data Transformation Workflow (Power Query)

### 1. Data Cleaning & Standardization
* **Row Restriction**: Filtered `List of Orders` to retain strictly the first **500 rows**.
* **Data Type Conversion**:
  * Set `Order Date` $\rightarrow$ **`Date`**
  * Set `Amount` and `Target` $\rightarrow$ **`Fixed Decimal Number`** (`Currency`)
* **Text Formatting**: Formatted `CustomerName` to **Proper Case** (`Capitalize Each Word`) for consistent capitalization.
* **Geographic Consolidation**: Merged `City` and `State` into a single column named **`Location`** in the format `City, State`.

### 2. Calculated & Conditional Fields
* **`Profit Margin`** *(Custom Column)*: 
  $$\text{Profit Margin} = \frac{\text{Profit}}{\text{Amount}}$$
  *Formatted as Percentage (`%`).*
* **`Profit Status`** *(Conditional Column)*: Categorized profitability based on net profit:
  * $\text{Profit} < 0 \rightarrow$ **`Loss`**
  * $\text{Profit} = 0 \rightarrow$ **`Break-Even`**
  * $\text{Profit} > 0 \rightarrow$ **`Profit`**

### 3. Data Integration & Quality Assurance
* **Table Join**: Merged `List of Orders` and `Order Details` on `Order ID` into a unified table named **`Orders Data`**.
* **Deduplication**: Executed duplicate checks on key identifiers (`Order ID`) to prevent data inflation.
* **Null Handling**: Filtered non-conforming null keys and defaulted missing numeric metrics to `0`.

---

## 📊 Aggregations & Analytical Views

* **Recent Trends Analysis**: Sorted `Orders Data` by `Order Date` in **Descending Order**.
* **Regional Analysis**: Filtered data by specific geographic areas (e.g., **Tamil Nadu**) for localized performance evaluation.
* **Order Performance Summaries**:
  * Total Order Count: `COUNT(Order ID)`
  * Average Profitability: `AVERAGE(Profit)` grouped by `Category`
  * Revenue Performance: `SUM(Amount)` grouped by `Sub-Category`
* **Target Aggregations**: Calculated aggregated target benchmarks by `Month of Order Date` in a dedicated summary model.

---

## 🔗 Data Modeling & Relationships

Relationships were created in the **Model View** to enable proper filter context across reports:

| Primary Table | Target Table | Linking Key | Cardinality | State |
| :--- | :--- | :--- | :--- | :--- |
| **`List of Orders`** | **`Order Details`** | `Order ID` | One-to-Many ($1:*$) | **Active** |
| **`Order Details`** | **`Sales Target`** | `Category` | Many-to-One ($*:1$) | **Active** |

---

## 🚀 Getting Started

1. Clone or download this repository:
   ```bash
   git clone https://github.com/your-username/powerbi-ecommerce-analysis.git
   ```
2. Open **`PwerBI_Assignment_1.pbix`** in **Power BI Desktop**.
3. Explore the Power Query transformations by clicking **Transform Data**.
4. Check the schema setup in the **Model View**.

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.