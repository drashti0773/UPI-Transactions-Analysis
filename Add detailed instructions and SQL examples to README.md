# UPI-Transactions-Analysis
Power BI dashboard and SQL insights for UPI transaction analysis.
# UPI-Transactions-Analysis

This project performs end-to-end analysis of UPI (Unified Payments Interface) transactions using SQL and Power BI. It helps analyze transaction trends, customer demographics, payment methods, and merchant shares to uncover business insights.

---

## 🔧 How It Works

1. A SQL database table `UPI_Transactions` is created:

    ```sql
    CREATE TABLE UPI_Transactions (
        TransactionID VARCHAR(20) PRIMARY KEY,
        TransactionDate DATE,
        Amount DECIMAL(10,2),
        BankNameSent VARCHAR(100),
        BankNameReceived VARCHAR(100),
        RemainingBalance DECIMAL(10,2),
        City VARCHAR(20),
        Gender VARCHAR(10),
        TransactionType VARCHAR(50),
        Status VARCHAR(20),
        TransactionTime TIME,
        DeviceType VARCHAR(50),
        PaymentMethod VARCHAR(50),
        MerchantName VARCHAR(100),
        Purpose VARCHAR(100),
        CustomerAge INT,
        PaymentMode VARCHAR(50),
        Currency VARCHAR(10)
    );
    ```

2. Additional columns are created using SQL:

    ```sql
    ALTER TABLE upi_transactions ADD COLUMN CityTier VARCHAR(10);
    UPDATE upi_transactions
    SET CityTier = CASE
        WHEN City IN ('Delhi', 'Mumbai', 'Bangalore') THEN 'Tier 1'
        WHEN City IN ('Indore', 'Rewa', 'Lucknow') THEN 'Tier 2'
        ELSE 'Tier 3'
    END;
    ```

    ```sql
    ALTER TABLE upi_transactions ADD COLUMN AgeCategory VARCHAR(20);
    UPDATE upi_transactions
    SET AgeCategory = CASE
        WHEN CustomerAge < 25 THEN 'Youth'
        WHEN CustomerAge BETWEEN 25 AND 40 THEN 'Adult'
        ELSE 'Senior'
    END;
    ```

    ```sql
    ALTER TABLE upi_transactions ADD COLUMN IsSuccess BOOLEAN;
    UPDATE upi_transactions
    SET IsSuccess = CASE
        WHEN Status = 'Success' THEN 1
        ELSE 0
    END;
    ```

    *(… similar SQL code for BankMatch, CurrencyType, DeviceGroup, TimeOfDay, etc.)*

3. A summary view is created for Power BI:

    ```sql
    CREATE OR REPLACE VIEW upi_transaction_summary AS
    SELECT
        TransactionID,
        TransactionDate,
        TransactionMonth,
        WeekdayName,
        HourOfDay,
        TimeOfDay,
        Amount,
        AmountCategory,
        BankMatch,
        City,
        CityTier,
        Gender,
        AgeCategory,
        TransactionType,
        TransactionTypeGroup,
        IsSuccess,
        DeviceType,
        DeviceGroup,
        PaymentMethod,
        MerchantName,
        Purpose,
        PaymentMode,
        Currency
    FROM upi_transactions;
    ```

---

## 📊 Power BI Dashboard

The transformed data is visualized in Power BI. The dashboard shows:

- Transaction volumes by month
- Merchant shares (e.g. Zomato, Flipkart)
- Age and gender distributions
- Payment methods (Phone Number, QR Code, UPI ID)
- Time-of-day transaction patterns
- Average remaining balances

### Dashboard Preview

![UPI Analysis_Dashboard](Screenshot 2025-07-02 123949.png)

---

## 🚀 How to Run This Project

- Clone the repository:

    ```bash
    git clone https://github.com/drashti0773/UPI-Transactions-Analysis.git
    ```

- Load your UPI data into the database.
- Run the SQL scripts to generate new columns and views.
- Connect the summary view to Power BI to create visuals.

---

**Created by Drashti Bamharoliya for learning and analytics purposes.**




















UPI-Transactions-Analysis is an end-to-end data analytics project that combines SQL data modeling and Power BI to analyze UPI transactions in India. The project creates a database table UPI_Transactions, enriches it with new derived columns like CityTier, AgeCategory, BankMatch, CurrencyType, DeviceGroup, and time-based features, and builds a summary view for reporting. SQL scripts transform raw data to enable insights into transaction trends by merchant, payment methods, customer demographics, and transaction timing. A Power BI dashboard visualizes key metrics such as transaction amounts, merchant shares, gender and age distributions, and time-based transaction patterns. To use this project, clone the repository using git clone https://github.com/drashti0773/UPI-Transactions-Analysis.git, load your UPI transaction data into a database table matching the provided schema, run the SQL scripts to generate new columns and views, and connect the summary view to Power BI for visualization. The repository also includes a dashboard screenshot for quick reference. Created by Drashti Bamharoliya for learning and analytics purposes.
