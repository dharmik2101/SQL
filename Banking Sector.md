# Financial Database SQL Queries

This repository contains a collection of SQL queries for various financial operations and analysis tasks, formatted for better readability and usability. Interactive diagrams and explanations have been added to enhance understanding.

---

## 1. Retrieve All Transactions for a Specific Customer

**Description**: Query to retrieve all transactions for a customer with ID `C001`, ordered by the transaction date in descending order.

```sql
SELECT transaction_id, customer_id, transaction_date, amount, transaction_type
FROM transactions
WHERE customer_id = 'C001'
ORDER BY transaction_date DESC;
```

**Visual Aid**:
![Customer Transactions](https://via.placeholder.com/800x400?text=Customer+Transactions+Chart)
---

## 2. Calculate the Total Balance for All Accounts

**Description**: Calculate the total balance across all accounts.

```sql
SELECT SUM(balance) AS total_balance
FROM accounts;
```

**Visual Aid**:
![Total Balance](https://via.placeholder.com/800x400?text=Total+Balance+Summary)
---

## 3. Find Customers with a Loan Amount Greater Than $50,000

**Description**: Identify customers who have loans greater than $50,000.

```sql
SELECT customer_id, loan_id, loan_amount, loan_date
FROM loans
WHERE loan_amount > 50000;
```

**Visual Aid**:
![High Loan Customers](https://via.placeholder.com/800x400?text=High+Loan+Customers)
---

## 4. Identify Duplicate Accounts

**Description**: Find duplicate accounts in the system based on account numbers.

```sql
SELECT account_number, COUNT(*)
FROM accounts
GROUP BY account_number
HAVING COUNT(*) > 1;
```

**Visual Aid**:
![Duplicate Accounts](https://via.placeholder.com/800x400?text=Duplicate+Accounts+Detection)
---

## 5. Find All Transactions That Occurred in the Last 30 Days

**Description**: Retrieve all transactions that occurred in the last 30 days.

```sql
SELECT transaction_id, customer_id, amount, transaction_date
FROM transactions
WHERE transaction_date >= DATEADD(DAY, -30, GETDATE());
```

**Visual Aid**:
![Recent Transactions](https://via.placeholder.com/800x400?text=Transactions+Last+30+Days)
---

## 6. Calculate the Average Loan Amount by Loan Type

**Description**: Calculate the average loan amount for each loan type.

```sql
SELECT loan_type, AVG(loan_amount) AS avg_loan_amount
FROM loans
GROUP BY loan_type;
```

**Visual Aid**:
![Average Loan Amount](https://via.placeholder.com/800x400?text=Average+Loan+Amount+by+Type)
---

## 7. Find the Top 5 Customers with the Highest Account Balances

**Description**: Retrieve the top 5 customers with the highest account balances.

```sql
SELECT customer_id, account_number, balance
FROM accounts
ORDER BY balance DESC
LIMIT 5;
```

**Visual Aid**:
![Top Customers](https://via.placeholder.com/800x400?text=Top+5+Customers+by+Balance)
---

## 8. Retrieve the Loan Repayment History for a Customer

**Description**: Query the repayment history for customer `C001`, ordered by payment date in descending order.

```sql
SELECT repayment_id, loan_id, customer_id, payment_date, payment_amount
FROM repayments
WHERE customer_id = 'C001'
ORDER BY payment_date DESC;
```

**Visual Aid**:
![Repayment History](https://via.placeholder.com/800x400?text=Customer+Repayment+History)
---

## 9. Calculate the Total Number of Accounts by Account Type

**Description**: Count the total number of accounts for each account type.

```sql
SELECT account_type, COUNT(*) AS total_accounts
FROM accounts
GROUP BY account_type;
```

**Visual Aid**:
![Accounts by Type](https://via.placeholder.com/800x400?text=Accounts+By+Type)
---

## 10. Detect Suspicious Transactions

**Description**: Identify transactions marked as suspicious, where the amount is greater than $10,000 and the transaction type is `Withdrawal`.

```sql
SELECT transaction_id, customer_id, amount, transaction_date
FROM transactions
WHERE amount > 10000 AND transaction_type = 'Withdrawal';
```

**Visual Aid**:
![Suspicious Transactions](https://via.placeholder.com/800x400?text=Suspicious+Transactions)
---

## 11. Find Accounts That Are Overdrawn

**Description**: Retrieve accounts with a negative balance.

```sql
SELECT account_number, customer_id, balance
FROM accounts
WHERE balance < 0;
```

**Visual Aid**:
![Overdrawn Accounts](https://via.placeholder.com/800x400?text=Overdrawn+Accounts)
---

## 12. Identify Customers with Multiple Loans

**Description**: Find customers who have more than one loan.

```sql
SELECT customer_id, COUNT(*) AS loan_count
FROM loans
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

**Visual Aid**:
![Multiple Loans](https://via.placeholder.com/800x400?text=Customers+with+Multiple+Loans)
---

## 13. Calculate the Total Interest Earned from All Loans

**Description**: Compute the total interest earned from all loans based on their interest rates.

```sql
SELECT SUM(loan_amount * interest_rate / 100) AS total_interest_earned
FROM loans;
```

**Visual Aid**:
![Total Interest Earned](https://via.placeholder.com/800x400?text=Total+Interest+Earned)
---

## 14. Retrieve All Customers Who Have Never Taken a Loan

**Description**: Identify customers who have never taken a loan.

```sql
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN loans l ON c.customer_id = l.customer_id
WHERE l.loan_id IS NULL;
```

**Visual Aid**:
![No Loan Customers](https://via.placeholder.com/800x400?text=Customers+Without+Loans)
---

## 15. Calculate the Average Time to Deliver a Credit Card

**Description**: Calculate the average time taken to deliver a credit card from the application date.

```sql
SELECT AVG(DATEDIFF(day, application_date, delivery_date)) AS avg_delivery_time
FROM credit_cards;
```

**Visual Aid**:
![Credit Card Delivery](https://via.placeholder.com/800x400?text=Average+Credit+Card+Delivery+Time)
---

## 16. Retrieve the Top 3 Most Popular Products for Loans

**Description**: Find the top 3 loan types with the highest number of loans issued.

```sql
SELECT loan_type, COUNT(*) AS total_loans
FROM loans
GROUP BY loan_type
ORDER BY total_loans DESC
LIMIT 3;
```

**Visual Aid**:
![Popular Loan Products](https://via.placeholder.com/800x400?text=Top+3+Loan+Products)
---

## 17. Calculate Monthly Total Transactions

**Description**: Calculate the total transaction amount for each month, grouped by year and month.

```sql
SELECT YEAR(transaction_date) AS year, MONTH(transaction_date) AS month, SUM(amount) AS total_transactions
FROM transactions
GROUP BY YEAR(transaction_date), MONTH(transaction_date)
ORDER BY year, month;
```

**Visual Aid**:
![Monthly Transactions](https://via.placeholder.com/800x400?text=Monthly+Transactions)
---

## 18. Identify Inactive Customers

**Description**: Identify customers who have not made any transactions in the past year.

```sql
SELECT customer_id, name
FROM customers
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id
    FROM transactions
    WHERE transaction_date >= DATEADD(YEAR, -1, GETDATE())
);
```

**Visual Aid**:
![Inactive Customers](https://via.placeholder.com/800x400?text=Inactive+Customers)
---

## 19. Check Loan-to-Value (LTV) Ratio for Home Loans

**Description**: Calculate the loan-to-value (LTV) ratio for home loans.

```sql
SELECT loan_id, property_value, loan_amount, 
       (loan_amount / property_value) * 100 AS ltv_ratio
FROM loans
WHERE loan_type = 'Home Loan';
```

**Visual Aid**:
![LTV Ratios](https://via.placeholder.com/800x400?text=Loan-to-Value+Ratios)
---

## 20. Generate a Report of All Pending Loans

**Description**: Retrieve details of loans that are approved but not yet disbursed.

```sql
SELECT loan_id, customer_id, loan_amount, loan_status
FROM loans
WHERE loan_status = 'Approved' AND disbursed_date IS NULL;
```

**Visual Aid**:
![Pending Loans](https://via.placeholder.com/800x400?text=Pending+Loans+Report)
