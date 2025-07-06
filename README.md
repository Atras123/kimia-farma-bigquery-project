CREATE OR REPLACE TABLE `kimia-farma-465107.Rakamin_KF_Analytics.tabel_analisa_kimia_farma` AS
SELECT
  tr.transaction_id,
  tr.date,
  kc.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  
  -- Persentase Gross Laba
  CASE 
    WHEN tr.price <= 50000 THEN 0.10
    WHEN tr.price > 50000 AND tr.price <= 100000 THEN 0.15
    WHEN tr.price > 100000 AND tr.price <= 300000 THEN 0.20
    WHEN tr.price > 300000 AND tr.price <= 500000 THEN 0.25
    WHEN tr.price > 500000 THEN 0.30
  END AS persentase_gross_laba,

  -- Nett Sales
  tr.price * (1 - tr.discount_percentage) AS nett_sales,

  -- Nett Profit
  tr.price * (1 - tr.discount_percentage) *
    CASE 
      WHEN tr.price <= 50000 THEN 0.10
      WHEN tr.price > 50000 AND tr.price <= 100000 THEN 0.15
      WHEN tr.price > 100000 AND tr.price <= 300000 THEN 0.20
      WHEN tr.price > 300000 AND tr.price <= 500000 THEN 0.25
      WHEN tr.price > 500000 THEN 0.30
    END AS nett_profit,

  tr.rating AS rating_transaksi

FROM 
  `kimia-farma-465107.Rakamin_KF_Analytics.kf_final_transaction` tr
JOIN 
  `kimia-farma-465107.Rakamin_KF_Analytics.kf_kantor_cabang` kc
  ON tr.branch_id = kc.branch_id
JOIN 
  `kimia-farma-465107.Rakamin_KF_Analytics.kf_product` pr
  ON tr.product_id = pr.product_id;
