
# MySQL Statistical Functions Guide

This guide provides a comprehensive overview of **statistical functions** in MySQL, including descriptions, examples, and how to compute advanced operations such as correlation and regression.

---

## Table of Contents

1. [Aggregate Statistical Functions](#1-aggregate-statistical-functions)
2. [Variance and Standard Deviation](#2-variance-and-standard-deviation)
3. [Covariance Functions](#3-covariance-functions)
4. [Percentile and Rank Functions](#4-percentile-and-rank-functions)
5. [Other Useful Functions](#5-other-useful-functions)
6. [Manual Calculations for Advanced Statistics](#6-manual-calculations-for-advanced-statistics)
7. [Summary](#7-summary-of-supported-statistical-functions)

---

## 1. Aggregate Statistical Functions

These functions operate on a set of rows and return a single aggregated result.

| **Function**     | **Description**                                                                 | **Example**                         |
|------------------|-------------------------------------------------------------------------------|-----------------------------------|
| `AVG()`          | Returns the average (mean) value of a numeric column.                        | `SELECT AVG(column_name) FROM table_name;` |
| `SUM()`          | Returns the sum of a numeric column.                                          | `SELECT SUM(column_name) FROM table_name;` |
| `MIN()`          | Returns the minimum value of a column.                                        | `SELECT MIN(column_name) FROM table_name;` |
| `MAX()`          | Returns the maximum value of a column.                                        | `SELECT MAX(column_name) FROM table_name;` |
| `COUNT()`        | Returns the count of rows that match the condition or column.                 | `SELECT COUNT(column_name) FROM table_name;` |

---

## 2. Variance and Standard Deviation

These functions are used to measure variability and dispersion in a dataset.

| **Function**        | **Description**                                                                  | **Example**                                      |
|---------------------|----------------------------------------------------------------------------------|------------------------------------------------|
| `VAR_POP()`         | Returns the **population variance** (variance for the entire dataset).          | `SELECT VAR_POP(column_name) FROM table_name;`  |
| `VAR_SAMP()`        | Returns the **sample variance** (variance for a sample of the dataset).         | `SELECT VAR_SAMP(column_name) FROM table_name;` |
| `STD()`             | Returns the **sample standard deviation**.                                      | `SELECT STD(column_name) FROM table_name;`      |
| `STDDEV()`          | Same as `STD()`; returns the **sample standard deviation**.                     | `SELECT STDDEV(column_name) FROM table_name;`   |

---

## 3. Covariance Functions

Covariance measures the relationship between two columns.

| **Function**         | **Description**                                                                          | **Example**                                          |
|-----------------------|----------------------------------------------------------------------------------------|----------------------------------------------------|
| `COVAR_POP()`         | Returns the **population covariance** between two columns.                            | `SELECT COVAR_POP(column1, column2) FROM table_name;` |
| `COVAR_SAMP()`        | Returns the **sample covariance** between two columns.                                 | `SELECT COVAR_SAMP(column1, column2) FROM table_name;` |

---

## 4. Percentile and Rank Functions

These advanced distribution-related functions are supported in **MySQL 8.0 and later**.

| **Function**          | **Description**                                                                         | **Example**                                                 |
|-----------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `PERCENT_RANK()`      | Returns the relative rank of a row as a percentage.                                   | `SELECT column1, PERCENT_RANK() OVER (ORDER BY column1) FROM table_name;` |
| `RANK()`              | Assigns a rank to each row, with gaps for duplicate ranks.                            | `SELECT column1, RANK() OVER (ORDER BY column1) FROM table_name;`         |
| `DENSE_RANK()`        | Assigns ranks without gaps for duplicate ranks.                                       | `SELECT column1, DENSE_RANK() OVER (ORDER BY column1) FROM table_name;`   |
| `NTILE()`             | Divides the data into equal-sized buckets or groups.                                  | `SELECT column1, NTILE(4) OVER (ORDER BY column1) FROM table_name;`       |
| `PERCENTILE_CONT()`   | Calculates a value at a specified percentile within a group (e.g., for Median).       | `SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY column1) FROM table_name;` |
| `ROW_NUMBER()`        | Assigns a unique sequential number to rows.                                           | `SELECT column1, ROW_NUMBER() OVER (ORDER BY column1) FROM table_name;`   |

---

## 5. Other Useful Functions

These functions can assist in manual statistical calculations.

| **Function**        | **Description**                                                                              | **Example**                                      |
|---------------------|--------------------------------------------------------------------------------------------|------------------------------------------------|
| `POW()`             | Returns the value of a number raised to a specified power.                                 | `SELECT POW(column1, 2) FROM table_name;`       |
| `SQRT()`            | Returns the square root of a number (useful for standard deviation calculation).            | `SELECT SQRT(SUM(POW(column1, 2))) FROM table_name;` |
| `ROUND()`           | Rounds a numeric value to the specified number of decimal places.                           | `SELECT ROUND(AVG(column1), 2) FROM table_name;` |

---

## 6. Manual Calculations for Advanced Statistics

### Pearson Correlation Coefficient

```sql
SELECT 
    (COUNT(*) * SUM(column1 * column2) - SUM(column1) * SUM(column2)) /
    SQRT(
        (COUNT(*) * SUM(POW(column1, 2)) - POW(SUM(column1), 2)) *
        (COUNT(*) * SUM(POW(column2, 2)) - POW(SUM(column2), 2))
    ) AS correlation
FROM table_name;
```

### Linear Regression (Slope and Intercept)

#### Slope (`m`):
```sql
SELECT 
    (COUNT(*) * SUM(column1 * column2) - SUM(column1) * SUM(column2)) /
    (COUNT(*) * SUM(POW(column1, 2)) - POW(SUM(column1), 2)) AS slope
FROM table_name;
```

#### Intercept (`b`):
```sql
SELECT 
    (SUM(column2) - slope * SUM(column1)) / COUNT(*) AS intercept
FROM table_name;
```

---

## 7. Summary of Supported Statistical Functions

1. **Aggregate Functions**:
   - `AVG()`, `SUM()`, `MIN()`, `MAX()`, `COUNT()`

2. **Variance and Standard Deviation**:
   - `VAR_POP()`, `VAR_SAMP()`, `STD()`, `STDDEV()`

3. **Covariance**:
   - `COVAR_POP()`, `COVAR_SAMP()`

4. **Percentile and Rank** (MySQL 8.0+):
   - `PERCENT_RANK()`, `RANK()`, `DENSE_RANK()`, `NTILE()`, `PERCENTILE_CONT()`, `ROW_NUMBER()`

5. **Math Functions**:
   - `POW()`, `SQRT()`, `ROUND()`

6. **Manual Operations**:
   - Correlation, Regression (calculated manually using aggregate and math functions).

---

This guide allows you to perform comprehensive statistical analysis in MySQL.
