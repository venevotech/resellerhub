# ResellerHub AI

# MySQL Schema Design Document

## Overview

This document defines the database schema for the ResellerHub AI – AI-Powered Reseller & Dropshipping Management Platform.

The schema supports:

- User Authentication & Authorization
- Reseller Management
- Product & Inventory Management
- Order Processing
- Commission Management
- Sales Reporting
- AI Predictions & Recommendations
- Business Analytics

---

# 1. User

```sql
User(
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('Admin','Reseller') NOT NULL,
    status ENUM('Active','Inactive','Suspended') DEFAULT 'Active',
    created_at DATETIME,
    updated_at DATETIME
)
```

### Description
Stores authentication and authorization information.

---

# 2. Reseller

```sql
Reseller(
    reseller_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT UNIQUE,
    company_name VARCHAR(150),
    phone VARCHAR(30),
    address TEXT,
    commission_rate DECIMAL(5,2),
    performance_score DECIMAL(5,2),
    approval_status ENUM('Pending','Approved','Rejected'),
    created_at DATETIME,
    FOREIGN KEY (user_id)
    REFERENCES User(user_id)
)
```

### Description
Stores reseller profile and performance information.

---

# 3. Category

```sql
Category(
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(100) UNIQUE NOT NULL,
    description TEXT
)
```

### Description
Stores product categories.

---

# 4. Product

```sql
Product(
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    category_id INT,
    product_name VARCHAR(200) NOT NULL,
    description TEXT,
    price DECIMAL(10,2),
    stock_quantity INT,
    image_url VARCHAR(255),
    status ENUM('Active','Inactive'),
    created_at DATETIME,
    FOREIGN KEY (category_id)
    REFERENCES Category(category_id)
)
```

### Description
Stores product information and inventory.

---

# 5. Customer_Order

```sql
Customer_Order(
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    reseller_id INT,
    total_amount DECIMAL(12,2),
    order_status ENUM(
        'Pending',
        'Processing',
        'Delivered',
        'Cancelled'
    ),
    order_date DATETIME,
    FOREIGN KEY (reseller_id)
    REFERENCES Reseller(reseller_id)
)
```
---

# 6. Order_Item

```sql
Order_Item(
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10,2),
    subtotal DECIMAL(10,2),
    FOREIGN KEY (order_id)
    REFERENCES Customer_Order(order_id),
    FOREIGN KEY (product_id)
    REFERENCES Product(product_id)
)
```

---

# 7. Commission

```sql
Commission(
    commission_id INT PRIMARY KEY AUTO_INCREMENT,
    reseller_id INT,
    order_id INT,
    commission_amount DECIMAL(12,2),
    commission_percentage DECIMAL(5,2),
    payment_status ENUM('Pending','Paid'),
    created_at DATETIME,
    FOREIGN KEY (reseller_id)
    REFERENCES Reseller(reseller_id),
    FOREIGN KEY (order_id)
    REFERENCES Customer_Order(order_id)
)
```

---

# 8. Sales_Report

```sql
Sales_Report(
    report_id INT PRIMARY KEY AUTO_INCREMENT,
    report_date DATE,
    total_orders INT,
    total_sales DECIMAL(12,2),
    total_profit DECIMAL(12,2)
)
```

---

# 9. AI_Prediction

```sql
AI_Prediction(
    prediction_id INT PRIMARY KEY AUTO_INCREMENT,
    prediction_type ENUM(
        'SalesForecast',
        'DemandForecast',
        'ChurnPrediction'
    ),
    prediction_value TEXT,
    generated_at DATETIME
)
```

---

# 10. Product_Recommendation

```sql
Product_Recommendation(
    recommendation_id INT PRIMARY KEY AUTO_INCREMENT,
    reseller_id INT,
    product_id INT,
    recommendation_score DECIMAL(8,4),
    generated_at DATETIME,
    FOREIGN KEY (reseller_id)
    REFERENCES Reseller(reseller_id),
    FOREIGN KEY (product_id)
    REFERENCES Product(product_id)
)
```

---

# 11. Reseller_Score

```sql
Reseller_Score(
    score_id INT PRIMARY KEY AUTO_INCREMENT,
    reseller_id INT,
    total_sales DECIMAL(12,2),
    conversion_rate DECIMAL(5,2),
    customer_retention DECIMAL(5,2),
    revenue_contribution DECIMAL(12,2),
    final_score DECIMAL(5,2),
    calculated_at DATETIME,
    FOREIGN KEY (reseller_id)
    REFERENCES Reseller(reseller_id)
)
```

---

# 12. AI_Assistant_Log

```sql
AI_Assistant_Log(
    log_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    query_text TEXT,
    ai_response TEXT,
    created_at DATETIME,
    FOREIGN KEY (user_id)
    REFERENCES User(user_id)
)
```

---

# Relationships Summary

* User → Reseller (1:1)
* Category → Product (1:M)
* Reseller → Customer_Order (1:M)
* Customer_Order → Order_Item (1:M)
* Product → Order_Item (1:M)
* Reseller → Commission (1:M)
* Customer_Order → Commission (1:1)
* Reseller → Product_Recommendation (1:M)
* Product → Product_Recommendation (1:M)
* Reseller → Reseller_Score (1:M)
* User → AI_Assistant_Log (1:M)

---

# Future Features

* Payment Gateway Integration
* Courier API Integration
* WhatsApp Notifications
* Multi-Vendor Marketplace
* AI Fraud Detection
* Real-Time Analytics
* Mobile Application
* Advanced Business Intelligence Dashboard

---

Prepared For:
Software Engineering, Database Systems & Final Year Project

Project:
ResellerHub AI – AI-Powered Reseller & Dropshipping Management Platform
