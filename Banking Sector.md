# Financial Database SQL Queries

This repository contains a collection of SQL queries for various financial operations and analysis tasks.

---

## 1. Retrieve All Transactions for a Specific Customer

**Description**: Query to retrieve all transactions for a customer with ID `C001`, ordered by the transaction date in descending order.

```sql
-- Retrieve all transactions for customer C001
SELECT transaction_id, customer_id, transaction_date, amount, transaction_type
FROM transactions
WHERE customer_id = 'C001'
ORDER BY transaction_date DESC;
```

---

## 2. Calculate the Total Balance for All Accounts

**Description**: Calculate the total balance across all accounts.

```sql
-- Calculate total balance from all accounts
SELECT SUM(balance) AS total_balance
FROM accounts;
```

---

## 3. Find Customers with a Loan Amount Greater Than $50,000

**Description**: Identify customers who have loans greater than $50,000.

```sql
-- Find customers with loan amounts exceeding $50,000
SELECT customer_id, loan_id, loan_amount, loan_date
FROM loans
WHERE loan_amount > 50000;
```

---

## 4. Identify Duplicate Accounts

**Description**: Find duplicate accounts in the system based on account numbers.

```sql
-- Detect duplicate account numbers
SELECT account_number, COUNT(*)
FROM accounts
GROUP BY account_number
HAVING COUNT(*) > 1;
```

---

## 5. Find All Transactions That Occurred in the Last 30 Days

**Description**: Retrieve all transactions that occurred in the last 30 days.

```sql
-- Get transactions within the last 30 days
SELECT transaction_id, customer_id, amount, transaction_date
FROM transactions
WHERE transaction_date >= DATEADD(DAY, -30, GETDATE());
```

---

## 6. Calculate the Average Loan Amount by Loan Type

**Description**: Calculate the average loan amount for each loan type.

```sql
-- Compute average loan amount grouped by loan type
SELECT loan_type, AVG(loan_amount) AS avg_loan_amount
FROM loans
GROUP BY loan_type;
```

---

## 7. Find the Top 5 Customers with the Highest Account Balances

**Description**: Retrieve the top 5 customers with the highest account balances.

```sql
-- Retrieve top 5 account balances
SELECT customer_id, account_number, balance
FROM accounts
ORDER BY balance DESC
LIMIT 5;
```

---

## 8. Retrieve the Loan Repayment History for a Customer

**Description**: Query the repayment history for customer `C001`, ordered by payment date in descending order.

```sql
-- Get loan repayment history for customer C001
SELECT repayment_id, loan_id, customer_id, payment_date, payment_amount
FROM repayments
WHERE customer_id = 'C001'
ORDER BY payment_date DESC;
```

---

## 9. Calculate the Total Number of Accounts by Account Type

**Description**: Count the total number of accounts for each account type.

```sql
-- Count accounts grouped by account type
SELECT account_type, COUNT(*) AS total_accounts
FROM accounts
GROUP BY account_type;
```

---

## 10. Detect Suspicious Transactions

**Description**: Identify transactions marked as suspicious, where the amount is greater than $10,000 and the transaction type is `Withdrawal`.

```sql
-- Identify suspicious withdrawal transactions over $10,000
SELECT transaction_id, customer_id, amount, transaction_date
FROM transactions
WHERE amount > 10000 AND transaction_type = 'Withdrawal';
```

---

## 11. Find Accounts That Are Overdrawn

**Description**: Retrieve accounts with a negative balance.

```sql
-- List accounts with negative balances
SELECT account_number, customer_id, balance
FROM accounts
WHERE balance < 0;
```

---

## 12. Identify Customers with Multiple Loans

**Description**: Find customers who have more than one loan.

```sql
-- Identify customers with more than one loan
SELECT customer_id, COUNT(*) AS loan_count
FROM loans
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

---

## 13. Calculate the Total Interest Earned from All Loans

**Description**: Compute the total interest earned from all loans based on their interest rates.

```sql
-- Calculate total interest earned from all loans
SELECT SUM(loan_amount * interest_rate / 100) AS total_interest_earned
FROM loans;
```

---

## 14. Retrieve All Customers Who Have Never Taken a Loan

**Description**: Identify customers who have never taken a loan.

```sql
-- Find customers without loans using LEFT JOIN
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN loans l ON c.customer_id = l.customer_id
WHERE l.loan_id IS NULL;
```

---

## 15. Calculate the Average Time to Deliver a Credit Card

**Description**: Calculate the average time taken to deliver a credit card from the application date.

```sql
-- Compute average credit card delivery time
SELECT AVG(DATEDIFF(day, application_date, delivery_date)) AS avg_delivery_time
FROM credit_cards;
```

---

## 16. Retrieve the Top 3 Most Popular Products for Loans

**Description**: Find the top 3 loan types with the highest number of loans issued.

```sql
-- Get the top 3 most popular loan products
SELECT loan_type, COUNT(*) AS total_loans
FROM loans
GROUP BY loan_type
ORDER BY total_loans DESC
LIMIT 3;
```

---

## 17. Calculate Monthly Total Transactions

**Description**: Calculate the total transaction amount for each month, grouped by year and month.

```sql
-- Summarize total transactions by month and year
SELECT YEAR(transaction_date) AS year, MONTH(transaction_date) AS month, SUM(amount) AS total_transactions
FROM transactions
GROUP BY YEAR(transaction_date), MONTH(transaction_date)
ORDER BY year, month;
```

---

## 18. Identify Inactive Customers

**Description**: Identify customers who have not made any transactions in the past year.

```sql
-- Find customers with no transactions in the last year
SELECT customer_id, name
FROM customers
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id
    FROM transactions
    WHERE transaction_date >= DATEADD(YEAR, -1, GETDATE())
);
```

---

## 19. Check Loan-to-Value (LTV) Ratio for Home Loans

**Description**: Calculate the loan-to-value (LTV) ratio for home loans.

```sql
-- Compute LTV ratio for home loans
SELECT loan_id, property_value, loan_amount, 
       (loan_amount / property_value) * 100 AS ltv_ratio
FROM loans
WHERE loan_type = 'Home Loan';
```

---

## 20. Generate a Report of All Pending Loans

**Description**: Retrieve details of loans that are approved but not yet disbursed.

```sql
-- List approved but pending loans
SELECT loan_id, customer_id, loan_amount, loan_status
FROM loans
WHERE loan_status = 'Approved' AND disbursed_date IS NULL;
```

---


