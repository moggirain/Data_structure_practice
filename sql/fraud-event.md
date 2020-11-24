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
# Are previous identified fraud account still open?
# IF all previous identified fruad accounts are closed
# output: date | account_first_fraud
# Step1: Find all the fraud account that is labelled today 
# Step1: Find all the fraud account that is not labelled today 
# Step3: Join two tables and identify the accounts that is only labelled today

SELECT COUNT(*) as first_time_account 
FROM (
SELECT DISTINCT account_id 
FROM ad_account 
WHERE status = 'fraud'
AND date = CURDATE()) t1
JOIN 
(SELECT DISTINCT account_id
FROM ad_account 
WHERE status = 'fraud' 
AND DATEDIFF(CURDATE(), date) >= 1) t2 
USING (account_id)
```

### Q3. What would be the financial impact of letting fraud accounts become active \(how would you approach this question\)

```sql
# financial impact -- revenue 
# If accurately labeled -- True Positive 
#     - reduce the continuous loss 
# If not accurately labelled -- False Positive 
#      - reduce customers and potential revenue
# soft action 
# monitor the spend of the accounts, suspicious spend 
# more than X days spend more than 75% accounts spend

SELECT account_id 
FROM 
SELECT 
       100 * SUM(spend) OVER(PARTITION BY date, account_id)/ SUM(spend) OVER(PARTITON BY date) as daily_spend
FROM ad_account 
WHERE status = 'open'; 

SELECT 
       100 * SUM(spend) OVER(PARTITION BY date, account_id)/ SUM(spend) OVER(PARTITON BY date) as daily_spend
FROM ad_account 
WHERE status = 'fraud'; 
```



