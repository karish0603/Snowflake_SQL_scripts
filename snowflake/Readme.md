[README.md](https://github.com/user-attachments/files/21690655/README.md)
# E-commerce Data Warehouse Setup in Snowflake

This project sets up a **sample e-commerce data warehouse** in Snowflake with multiple databases, schemas, and tables to manage orders, returns, payments, and deliveries. It demonstrates how to create and insert sample data into transient and permanent tables for analytics and reporting.

---

## 📌 Features

- **Warehouse Creation** with auto-suspend and auto-resume
- **Multiple Databases** (`amazon`, `flipcart`)
- **Schemas** for different business domains
- **Transient Tables** for temporary data
- **Sample Data Insertion**
- **Simple Queries** to verify data

---

## 🗂 Project Structure

```
ecommerce_dw/
│
├── amazon/
│   ├── orders/
│   │   └── order_summary (transient table)
│   └── returns/
│       └── return_summary (transient table)
│
├── flipcart/
│   ├── payment/
│   │   └── payment_summary (permanent table)
│   └── delivery/
│       └── delivery_status (permanent table)
│
└── README.md
```

---

## ⚙️ Steps in the Script

### 1. Create Snowflake Warehouse
```sql
CREATE WAREHOUSE ecommerce
WITH warehouse_size = 'medium'
AUTO_SUSPEND = 100
AUTO_RESUME = TRUE
INITIALLY_SUSPENDED = TRUE;
```

---

### 2. Amazon Database
```sql
CREATE OR REPLACE DATABASE amazon;
USE DATABASE amazon;

-- Orders Schema
CREATE OR REPLACE SCHEMA orders;
USE SCHEMA orders;
CREATE OR REPLACE TRANSIENT TABLE order_summary (
    order_id INT,
    customer_name STRING,
    product_name STRING,
    order_date DATE
);
INSERT INTO order_summary VALUES
(10,'Sonakshi','Airpod','2025-08-01'),
(20,'Prakshi','Laptop','2025-07-01'),
(40,'Priya','Headphone','2025-07-06');

-- Returns Schema
CREATE OR REPLACE SCHEMA returns;
USE SCHEMA returns;
CREATE OR REPLACE TRANSIENT TABLE return_summary (
    return_id INT,
    order_id INT,
    return_date DATE,
    return_reason STRING
);
INSERT INTO return_summary VALUES
(1,10,'2025-08-03','Not working'),
(2,20,'2025-08-06','Battery Not working');
```

---

### 3. Flipcart Database
```sql
CREATE OR REPLACE DATABASE flipcart;
USE DATABASE flipcart;

-- Payment Schema
CREATE OR REPLACE SCHEMA payment;
USE SCHEMA payment;
CREATE OR REPLACE TABLE payment_summary (
    order_id INT,
    payment_id INT,
    payment_method STRING,
    payment_amount INT,
    payment_date DATE
);
INSERT INTO payment_summary VALUES
(1,0001,'UPI',1000,'2025-05-01'),
(2,0002,'Credit Card',2000,'2025-05-02'),
(3,0003,'Cash',4000,'2025-05-04');

-- Delivery Schema
CREATE OR REPLACE SCHEMA delivery;
USE SCHEMA delivery;
CREATE OR REPLACE TABLE delivery_status (
    order_id INT,
    delivery_date DATE,
    status STRING
);
INSERT INTO delivery_status VALUES
(1,'2025-03-01','In-transit'),
(2,'2025-03-02','Out for delivery'),
(3,'2025-03-02','Pending');
```

---

## 📊 Sample Output

### Orders
| order_id | customer_name | product_name | order_date |
|----------|---------------|--------------|------------|
| 10       | Sonakshi      | Airpod       | 2025-08-01 |
| 20       | Prakshi       | Laptop       | 2025-07-01 |
| 40       | Priya         | Headphone    | 2025-07-06 |

### Returns
| return_id | order_id | return_date | return_reason      |
|-----------|----------|-------------|--------------------|
| 1         | 10       | 2025-08-03  | Not working        |
| 2         | 20       | 2025-08-06  | Battery Not working|

---

## 🚀 How to Run
1. Log in to your Snowflake account.
2. Open a **Worksheet**.
3. Copy and paste the SQL script from `ecommerce_dw.sql`.
4. Run the commands in sequence.
5. Verify tables with:
```sql
SELECT * FROM <table_name>;
```

---

## 📌 Notes
- **Transient tables** are used for `amazon` database to save storage costs for temporary data.
- **Permanent tables** are used for `flipcart` database for long-term data storage.
- Auto-suspend helps reduce cost by suspending the warehouse when idle.
