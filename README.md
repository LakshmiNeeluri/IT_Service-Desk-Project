# IT Service Desk Analytics Dashboard

## 📌 Project Overview

The **IT Service Desk Analytics Dashboard** is a data analytics project designed to analyze IT support tickets, resolution performance, SLA compliance, ticket priorities, categories, departments, and technician performance.

The project uses **Python for data preparation**, **MySQL for data storage and SQL analysis**, and **Power BI for interactive dashboard development**.

The objective is to transform raw IT service desk ticket data into meaningful business insights that can help organizations monitor service performance and identify areas for improvement.

---

## 🎯 Objectives

* Analyze IT service desk ticket volumes and trends.
* Monitor ticket status and resolution performance.
* Analyze SLA compliance.
* Identify high-priority and frequently occurring ticket categories.
* Analyze technician/agent performance.
* Compare ticket resolution times across departments and priorities.
* Build an interactive Power BI dashboard for decision-making.

---

## 🛠️ Technologies Used

| Technology   | Purpose                                     |
| ------------ | ------------------------------------------- |
| **Python**   | Data generation, cleaning and preprocessing |
| **Pandas**   | Data manipulation and preparation           |
| **NumPy**    | Numerical operations                        |
| **MySQL**    | Data storage and SQL analysis               |
| **SQL**      | Querying and aggregating ticket data        |
| **Power BI** | Interactive dashboard and visualization     |
| **DAX**      | Measures and KPI calculations               |

---

## 📊 Dataset

The project uses an IT service desk ticket dataset containing approximately **50,000 records**.

### Main Columns

* `Ticket_ID`
* `Created_Date`
* `Resolved_Date`
* `Priority`
* `Category`
* `Subcategory`
* `Department`
* `Assigned_Agent`
* `Channel`
* `Status`
* `SLA_Target_Hours`
* `Resolution_Time_Hours`

---

## 🔄 Project Workflow

```text
Raw Ticket Data
       ↓
Python
Data Generation / Cleaning
       ↓
MySQL
Data Storage
       ↓
SQL Analysis
       ↓
Power BI
Data Connection
       ↓
DAX Measures
       ↓
Interactive Dashboard
       ↓
Business Insights
```

---

# 📁 Project Structure

```text
IT-Service-Desk-Analytics/
│
├── data/
│   └── service_tickets.csv
│
├── python/
│   └── data_preparation.py
│
├── sql/
│   ├── create_database.sql
│   ├── create_table.sql
│   └── analysis_queries.sql
│
├── powerbi/
│   └── IT_Service_Desk_Dashboard.pbix
│
├── screenshots/
│   ├── overview.png
│   ├── sla_analysis.png
│   └── technician_performance.png
│
└── README.md
```

---

# 🐍 Python Data Preparation

Python and Pandas were used to prepare the service desk dataset before loading it into MySQL.

Major activities include:

* Creating/loading ticket data
* Handling missing values
* Formatting date columns
* Validating ticket records
* Calculating resolution-related fields
* Preparing the dataset for database analysis

Example:

```python
import pandas as pd

df = pd.read_csv("service_tickets.csv")

df["Created_Date"] = pd.to_datetime(df["Created_Date"])
df["Resolved_Date"] = pd.to_datetime(df["Resolved_Date"])

df["Resolution_Time_Hours"] = (
    df["Resolved_Date"] - df["Created_Date"]
).dt.total_seconds() / 3600

print(df.head())
print(df.shape)
```

---

# 🗄️ MySQL Database

The cleaned dataset is stored in MySQL.

### Database

```sql
CREATE DATABASE IT_ServiceDesk;
```

### Main Table

```text
service_tickets
```

The table contains ticket information required for service desk performance analysis.

---

# 🔎 SQL Analysis

SQL queries are used to analyze ticket volume, priorities, resolution times, and SLA performance.

### Total Tickets

```sql
SELECT COUNT(*) AS Total_Tickets
FROM service_tickets;
```

### Tickets by Priority

```sql
SELECT
    Priority,
    COUNT(*) AS Total_Tickets
FROM service_tickets
GROUP BY Priority
ORDER BY Total_Tickets DESC;
```

### Average Resolution Time by Priority

```sql
SELECT
    Priority,
    ROUND(AVG(Resolution_Time_Hours), 2)
        AS Avg_Resolution_Hours
FROM service_tickets
WHERE Resolved_Date IS NOT NULL
GROUP BY Priority
ORDER BY Avg_Resolution_Hours DESC;
```

### Tickets by Category

```sql
SELECT
    Category,
    COUNT(*) AS Total_Tickets
FROM service_tickets
GROUP BY Category
ORDER BY Total_Tickets DESC;
```

---

# 📊 Power BI Dashboard

The final dashboard is developed using **Microsoft Power BI Desktop**.

The dashboard is divided into multiple analytical pages.

## Page 1 — IT Service Desk Overview

Key KPIs:

* Total Tickets
* Resolved Tickets
* Open Tickets
* Average Resolution Time
* SLA Compliance

Suggested visuals:

* Ticket trend over time
* Tickets by Priority
* Tickets by Category
* Tickets by Status
* Tickets by Department

---

## Page 2 — Ticket & SLA Analysis

This page focuses on service-level performance.

Analysis includes:

* SLA compliance
* SLA breaches
* Average resolution time
* Resolution time by priority
* Resolution time by category
* Ticket volume trends

Suggested visuals:

* SLA Compliance KPI
* SLA Breach Count
* Average Resolution Hours
* Priority vs Resolution Time
* Category vs SLA Performance

---

## Page 3 — Technician Performance

This page analyzes assigned agents/technicians.

Analysis includes:

* Tickets handled by each agent
* Resolved tickets
* Average resolution time
* SLA performance
* Workload distribution

Suggested visuals:

* Tickets by Agent
* Average Resolution Time by Agent
* Resolved Tickets by Agent
* Agent workload comparison

---

# 📐 Power BI DAX Measures

### Total Tickets

```DAX
Total Tickets =
COUNTROWS(service_tickets)
```

### Resolved Tickets

```DAX
Resolved Tickets =
CALCULATE(
    COUNTROWS(service_tickets),
    service_tickets[Status] = "Resolved"
)
```

### Open Tickets

```DAX
Open Tickets
```
