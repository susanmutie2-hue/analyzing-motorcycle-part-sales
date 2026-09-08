# Analyzing Motorcycle Part Sales 🏍️

An exploratory SQL data analysis project investigating net revenue trends across product lines, months, and warehouse locations for wholesale orders. Originally completed on DataCamp DataLab.

---

## 📌 Project Overview
A motorcycle parts company operating three warehouses wanted to better understand wholesale revenue across product lines and payment methods. Because payment methods incur processing fees (credit card, cash, bank transfer), net revenue must factor in these fees to reflect true performance.

**Objective:** Calculate total net revenue grouped by `product_line`, `month` (June, July, August), and `warehouse`, filtered exclusively for wholesale clients.

---

## 📊 Dataset Structure
The database table `sales` contains wholesale and retail order records with the following key fields:

| Field Name | Description |
| :--- | :--- |
| `order_number` | Unique order identifier |
| `date` | Transaction date |
| `warehouse` | Warehouse location (`North`, `Central`, `South`) |
| `client_type` | Customer type (`Wholesale` or `Retail`) |
| `product_line` | Part category (e.g., `Frame & body`, `Engine`, `Braking system`) |
| `total` | Gross order revenue |
| `payment_fee` | Fee percentage associated with the payment method |

---

## 🛠️ Analysis & SQL Solution

```sql
SELECT 
    product_line,
    CASE 
        WHEN EXTRACT('month' FROM date) = 6 THEN 'June'
        WHEN EXTRACT('month' FROM date) = 7 THEN 'July'
        WHEN EXTRACT('month' FROM date) = 8 THEN 'August'
    END AS month,
    warehouse,
    ROUND(SUM(total * (1 - payment_fee))::numeric, 2) AS net_revenue
FROM sales
WHERE client_type = 'Wholesale'
GROUP BY product_line, month, warehouse
ORDER BY product_line ASC, month ASC, net_revenue DESC;
```

---

## 💡 Key Business Insights
* **Top Product Lines:** High-margin product categories like `Frame & body` and `Suspension & traction` generated the largest portion of wholesale revenue.
* **Payment Fees:** Discounting payment fees provides an accurate reflection of warehouse yield compared to evaluating raw revenue alone.
* **Seasonality:** June and July exhibited distinct purchasing spikes across central warehouses.

---

## 📁 Repository Structure
* `data/sales.csv`: Raw transactional sales dataset.
* `queries/aggregate_wholesale_revenue.sql`: Complete PostgreSQL query script.
* `README.md`: Project summary and documentation.
