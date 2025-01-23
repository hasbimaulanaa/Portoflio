# portfolio

```{r}
SELECT 
    t.transaction_id,
    t.date,
    c.branch_id,
    c.branch_name,
    c.kota,
    c.provinsi,
    c.rating AS rating_cabang,
    t.customer_name,
    t.product_id,
    p.product_name,
    CAST(t.price AS DECIMAL(18, 2)) AS actual_price,  -- Pastikan actual_price bertipe DECIMAL
    CAST(t.discount_percentage AS DECIMAL(5, 2)) AS discount_percentage,  -- Pastikan discount_percentage bertipe DECIMAL
    -- Hitung persentase gross laba
    CASE
        WHEN t.price <= 50000 THEN 10
        WHEN t.price > 50000 AND t.price <= 100000 THEN 15
        WHEN t.price > 100000 AND t.price <= 300000 THEN 20
        WHEN t.price > 300000 AND t.price <= 500000 THEN 25
        WHEN t.price > 500000 THEN 30
    END AS persentase_gross_laba,
    -- Hitung net_sales
    CAST(t.price AS DECIMAL(18, 2)) - (CAST(t.price AS DECIMAL(18, 2)) * CAST(t.discount_percentage AS DECIMAL(5, 2)) / 100) AS net_sales,
    -- Hitung net_profit
    (CAST(t.price AS DECIMAL(18, 2)) - (CAST(t.price AS DECIMAL(18, 2)) * CAST(t.discount_percentage AS DECIMAL(5, 2)) / 100)) * 
    CASE
        WHEN t.price <= 50000 THEN 0.1
        WHEN t.price > 50000 AND t.price <= 100000 THEN 0.15
        WHEN t.price > 100000 AND t.price <= 300000 THEN 0.2
        WHEN t.price > 300000 AND t.price <= 500000 THEN 0.25
        WHEN t.price > 500000 THEN 0.3
    END AS net_profit,
    t.rating AS rating_transaksi
FROM 
    kf_final_transaction t
JOIN 
    kf_product p ON t.product_id = p.product_id
JOIN 
    kf_inventory i ON t.product_id = i.product_id AND t.branch_id = i.branch_id
JOIN 
    kf_kantor_cabang c ON t.branch_id = c.branch_id;

```
