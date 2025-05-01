# Tiny-SQLite-database-using-python
## 📊 Sales Data Analysis & Visualization  

## 🔹 Overview  
This project analyzes **sales data** using **SQLite & Python**, generating key insights through SQL queries and visualizations. The dataset includes various products, sales quantities, and revenue calculations.  

## 🔹 Features  
✔ **SQLite Database** for storing and querying sales data  
✔ **SQL Queries** for summary statistics (total revenue, top product, etc.)  
✔ **Matplotlib Visualizations** including bar charts and pie charts  
✔ **Data Export** (CSV file for further analysis)  

## 🔹 Technologies Used  
- **Python** (Pandas, Matplotlib, SQLite3)  
- **Jupyter Notebook**  
- **SQL** for database querying  

🔹 Dataset & Queries
📌 Creating the Sales Database
import sqlite3

# Connect to SQLite
conn = sqlite3.connect("sales_data.db")
cursor = conn.cursor()

# Create table (if it doesn't exist)
cursor.execute("""
CREATE TABLE IF NOT EXISTS sales (
    id INTEGER PRIMARY KEY,
    product TEXT,
    quantity INTEGER,
    price REAL
)
""")

# Insert sample data with 10+ unique products
sales_data = [
    ("Product A", 10, 5.0), ("Product B", 15, 7.5), ("Product C", 8, 12.0),
    ("Product D", 20, 10.0), ("Product E", 5, 4.0), ("Product F", 18, 15.0),
    ("Product G", 12, 6.5), ("Product H", 25, 8.0), ("Product I", 30, 9.5),
    ("Product J", 22, 11.0), ("Product A", 5, 5.0), ("Product B", 10, 7.5),
    ("Product C", 12, 12.0), ("Product D", 8, 10.0), ("Product E", 14, 4.0)
]

cursor.executemany("INSERT INTO sales (product, quantity, price) VALUES (?, ?, ?)", sales_data)

# Save changes and close connection
conn.commit()
conn.close()

print("✅ Database updated with 10+ products!")



# 📊 Query 1: Sales Summary
SELECT product, SUM(quantity) AS total_qty, SUM(quantity * price) AS revenue  
FROM sales  
GROUP BY product;


import sqlite3
import pandas as pd

conn = sqlite3.connect("sales_data.db")

query = """
SELECT product, SUM(quantity) AS total_qty, SUM(quantity * price) AS revenue
FROM sales
GROUP BY product
"""

df = pd.read_sql_query(query, conn)
print(df)

conn.close()



# 🏆 Query 2: Top Revenue-Generating Product
SELECT product, SUM(quantity * price) AS total_revenue  
FROM sales  
GROUP BY product  
ORDER BY total_revenue DESC  
LIMIT 1;


query_top_product = """
SELECT product, SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY product
ORDER BY total_revenue DESC
LIMIT 1
"""

df_top_product = pd.read_sql_query(query_top_product, conn)
print(df_top_product)

conn.close()



# 🎨 Visualizations
📈 Bar Chart - Total Revenue Per Product
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
df.plot(kind='bar', x='product', y='revenue', legend=False, color=['skyblue'])

for index, value in enumerate(df["revenue"]):
    plt.text(index, value + (max(df["revenue"]) * 0.02), f"{value:.2f}", ha='center', fontsize=10, color="black")

plt.xlabel("Product", fontsize=12, fontweight='bold')
plt.ylabel("Total Revenue ($)", fontsize=12, fontweight='bold')
plt.title("Total Revenue Per Product", fontsize=14, fontweight='bold')
plt.xticks(rotation=45, fontsize=10)
plt.grid(axis='y', linestyle='--', alpha=0.7)

plt.savefig("sales_chart_updated.png")
plt.show()



# 📊 Bar Chart - Total Quantity Sold
query_quantity = """
SELECT product, SUM(quantity) AS total_qty
FROM sales
GROUP BY product
"""

df_qty = pd.read_sql_query(query_quantity, conn)
conn.close()

import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
df_qty.plot(kind='bar', x='product', y='total_qty', legend=False, color='lightcoral')

for index, value in enumerate(df_qty["total_qty"]):
    plt.text(index, value + 1, f"{value}", ha='center', fontsize=10, color="black")

plt.xlabel("Product", fontsize=12, fontweight='bold')
plt.ylabel("Total Quantity Sold", fontsize=12, fontweight='bold')
plt.title("Total Quantity Sold Per Product", fontsize=14, fontweight='bold')
plt.xticks(rotation=45, fontsize=10)
plt.grid(axis='y', linestyle='--', alpha=0.7)

plt.savefig("sales_quantity_chart.png")
plt.show()



#  Pie Chart - Sales Quantity Distribution
import matplotlib.pyplot as plt

plt.figure(figsize=(8, 8))
plt.pie(df_qty["total_qty"], labels=df_qty["product"], autopct="%1.1f%%", colors=plt.cm.Paired.colors)

plt.title("Sales Quantity Distribution by Product", fontsize=14, fontweight='bold')
plt.tight_layout()
plt.savefig("sales_quantity_pie_chart.png")
plt.show()



# 🔹 Exporting Data to CSV
df.to_csv("updated_sales_summary.csv", index=False)
print("✅ CSV file saved: updated_sales_summary.csv")



# 🔹 Results & Insights
✔ Identified the highest revenue-generating product
✔ Visualized revenue & quantity trends per product
✔ Stored processed data for future analysis (updated_sales_summary.csv
