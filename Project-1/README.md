# Project 1: E-Commerce Data Cleaning

## Introduction

This project focuses on cleaning and preparing an e-commerce order dataset for reliable downstream analysis. The dataset contains 1,200 order records and 14 variables covering customer information, product details, transaction values, shipping information, payment methods, order status, and promotional activity.

The primary objective of this project was to assess the quality of the dataset, identify potential data-quality issues, apply appropriate cleaning techniques, and validate the resulting dataset.

---

## Dataset Overview

The dataset contains **1,200 records and 14 columns**:

* **OrderID** – Unique identifier for each order
* **Date** – Order date
* **CustomerID** – Customer identifier
* **Product** – Product associated with the order
* **Quantity** – Number of items ordered
* **UnitPrice** – Price per item
* **ShippingAddress** – Customer shipping address
* **PaymentMethod** – Payment method used
* **OrderStatus** – Current status of the order
* **TrackingNumber** – Shipment tracking identifier
* **ItemsInCart** – Number of items in the customer's cart
* **CouponCode** – Coupon applied to the order
* **ReferralSource** – Source through which the customer was referred
* **TotalPrice** – Total value of the order

---

## Data Quality Assessment

The dataset was assessed for:

* Missing values
* Duplicate records
* Data types
* Categorical inconsistencies
* Numerical validity
* Internal consistency between calculated and recorded values

### Initial Findings

| Check                       | Finding                                    |
| --------------------------- | ------------------------------------------ |
| Dataset size                | 1,200 rows × 14 columns                    |
| Missing values              | Found only in `CouponCode`                 |
| Duplicate rows              | None identified                            |
| Categorical inconsistencies | None identified                            |
| CustomerID completeness     | No missing values                          |
| Numerical validity          | No obvious invalid values identified       |
| Date format                 | Stored as `object` and required conversion |

---

## Key Findings & Cleaning Decisions

| Finding                                                   | Action                                     | Reason                                                                | Impact                                                                    |
| --------------------------------------------------------- | ------------------------------------------ | --------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `CouponCode` contained 309 missing values                 | Replaced missing values with `"No Coupon"` | Missing values represented orders where no coupon was used            | Preserved all records while making coupon status explicit                 |
| `Date` was stored as `object`                             | Converted `Date` to datetime               | Enables reliable date-based analysis                                  | Improved temporal analysis and filtering                                  |
| No duplicate rows were found                              | No records removed                         | Duplicate records could distort analysis, but none were present       | Dataset remained at 1,200 records                                         |
| Categorical values were consistent                        | No standardization required                | Prevents unnecessary changes to valid data                            | Preserved original categorical information                                |
| `CustomerID` had no missing values                        | No changes made                            | Customer identifiers were complete                                    | Customer-level information was preserved                                  |
| Numerical variables were within reasonable ranges         | No corrections made                        | No obvious invalid or impossible values were identified               | No numerical records were unnecessarily altered                           |
| `TotalPrice` was validated against `Quantity × UnitPrice` | Used `np.isclose()` for validation         | Floating-point precision can create very small comparison differences | Confirmed that recorded totals were consistent with the calculated totals |

---

## Data Cleaning

The following transformations were applied:

### 1. Handling Coupon Codes

Missing `CouponCode` values represented orders where no coupon was used. These were replaced with `"No Coupon"`.

```python
dataset['CouponCode'] = dataset['CouponCode'].fillna('No Coupon')
```

### 2. Converting Date

The `Date` column was converted from an object to a datetime format.

```python
dataset['Date'] = pd.to_datetime(dataset['Date'])
```

### 3. Data Validation

The cleaned dataset was validated by checking:

```python
dataset.isnull().sum()
dataset.duplicated().sum()
dataset.dtypes
```

The `TotalPrice` field was also validated against the expected calculation:

```python
calculated_total = dataset['Quantity'] * dataset['UnitPrice']

np.isclose(
    dataset['TotalPrice'],
    calculated_total
).sum()
```

---

## Final Outcome

After cleaning and validation:

* All **1,200 records** were retained.
* No duplicate records were removed.
* Coupon information was made explicit by replacing applicable missing values with `"No Coupon"`.
* `Date` was converted to the appropriate datetime format.
* No categorical corrections were required.
* No invalid numerical values requiring correction were identified.
* `TotalPrice` was confirmed to be consistent with `Quantity × UnitPrice`.

The resulting dataset is structured and validated for subsequent exploratory analysis.

---

## Technologies Used

* **Python** – Data cleaning and analysis
* **Pandas** – Data manipulation and quality checks
* **NumPy** – Numerical validation
* **Jupyter Notebook** – Interactive analysis and documentation
* **Git & GitHub** – Version control and project documentation

---

## Project Structure

```text
Project_1_Data_Cleaning/
│
├── README.md
├── data/
│   └── cleaned_dataset.csv
│
└── notebooks/
    └── data_cleaning.ipynb
```

## Project Status

**Completed — Data Cleaning & Validation**

