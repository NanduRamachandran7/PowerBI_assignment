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


# 📊 Global Operations & Workforce Analytics — Power BI Desktop Project

> **Sectional Assignment II** | **File:** `powr_bi_assignment_2.pbix`  
> **Dashboard Title:** Global Operations & Workforce Analytics  
> **Canvas Size:** Custom Executive Dimensions ($2700 \times 1860\text{ px}$)  

---

## 📌 Executive Overview

This repository contains the complete Power BI Desktop analysis, data model, calculated columns, DAX measures, and visual dashboards built for **Sectional Assignment II**.

The project integrates data from multiple domain datasets—customer orders, workforce layoffs, flight schedules, and product breakdowns—into a unified data model. The dashboard delivers operational insights across country order distributions, workforce layoff trends, order fulfillment time series, and data quality metrics.

---

## 🛠️ Data Model & Dataset Architecture

The data model operates in **Import Mode** across five core tables:

| Table Name | Rows | Columns | Description & Role in Project |
| :--- | :---: | :---: | :--- |
| **`ListOfOrders`** | 4,117 | 12 | Primary order table used for the **Clustered Column Chart** (Orders by Country). |
| **`Customer Data`** | 699 | 10 | Customer transaction details used for the **Line Chart** and **Calculated Column**. |
| **`Employee_Layoffs`** | 2,273 | 10 | Global layoff records used for the **Pie Chart** and **all 6 DAX Measures**. |
| **`OrderBreakdown`** | 8,047 | 8 | Line-item order breakdown loaded into the data model. |
| **`FlightData`** | 43 | 3 | Regional flight performance data loaded into the data model. |

---

## 📋 Completed Tasks & Assignment Deliverables

### 1. Clustered Column Chart: Orders by Country
* **Table:** `ListOfOrders`
* **X-Axis (Category):** `Country`
* **Y-Axis (Values):** `Count of Order ID`
* **Purpose:** Identifies top international market demand by sorting countries by total order placement volume (e.g., France, Germany, United Kingdom, Italy, Spain).

---

### 2. Line Chart: Order Volume Time Series
* **Table:** `Customer Data`
* **X-Axis (Category):** `Date` (Full Date Hierarchy: Year $\rightarrow$ Quarter $\rightarrow$ Month $\rightarrow$ Day)
* **Y-Axis (Values):** `Count of Order ID`
* **Purpose:** Plots order volume over time across 2013–2016, enabling multi-level drill-down from annual trends to daily order spikes.

---

### 3. Pie Chart: Layoffs by Location
* **Table:** `Employee_Layoffs`
* **Legend / Category:** `Location`
* **Values:** `Sum of Total_Laid_Off`
* **Purpose:** Visualizes proportional workforce reductions across global tech hubs and cities (e.g., SF Bay Area, Seattle, New York City, Bengaluru, London, Austin).

---

### 4. Calculated Column: Replicated Total Price
Added to the **`Customer Data`** table to verify transactional pricing consistency row by row.

```dax
New Total Price = 'Customer Data'[Quantity] * 'Customer Data'[Price Per Unit ]
