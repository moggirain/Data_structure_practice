# Fraud-Event

```sql
Table: ad_account 
account_id (INT) | date (VARCHAR) | status (‘open’, ‘closed’, ‘fraud’) | spend (double)
```

### Q1. What percent of active accounts are fraud?

```sql
# How do you define active accounts? 
# What is the output?
# date | fraud_perc
# if active account = 'open' 
# Assume account_id is unique
SELECT date, 
       100 * SUM(CASE WHEN status = 'fraud' THEN 1 ELSE 0 END)
       / SUM(CASE WHEN status = 'open' THEN 1 ELSE 0 END) as fraud_perc
FROM ad_account 
GROUP BY 1

 # if active account = 'spend' > 0 
 SELECT date, 
 100 * SUM(CASE WHEN status = 'fraud' THEN 1 ELSE 0 END)
/ COUNT(*) as fraud_perc
FROM ad_account
WHERE spend > 0
GROUP BY 1; 
```

###  Q2. How many accounts became fraud today for the first time? 

```sql

```

