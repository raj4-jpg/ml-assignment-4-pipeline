--- 1. Data Dictionary ---
               Column                  Meaning            Type        Unit            Expected Range
          customer_id        Unique identifier  Categorical/ID        None               CUST-10000+
                  age             Customer age   Numeric (Int)       Years                 18 to 100
         annual_spend        Gross expenditure Numeric (Float)     USD ($)                0 to 20000
delivery_distance_raw       Warehouse distance    Text (Mixed)  km / miles               0.5 to 100 km
        orders_placed    Total lifetime orders Numeric (Float)       Count                   1 to 50
      membership_tier           Loyalty status     Categorical    Category   Gold/Silver/Bronze/None
        service_calls  Support inquiries count   Numeric (Int)       Count                   0 to 20
          days_active       Tenure on platform   Numeric (Int)        Days                 1 to 2000
      promo_code_used    Joined via promotion    Binary (Int)        Flag                    0 or 1
                churn   Target: customer churn   Binary (Int)        Flag                    0 or 1

--- 2. Raw Quality Report ---
               Issue Type  Count                                                    Details
           Missing Values    500                    annual_spend (320), orders_placed (180)
  Duplicate Records (Key)    150                            Duplicated customer_id records
        Impossible Values     25                                        Negative ages (< 18)
       Inconsistent Units    906                      Distances logged in miles instead of km
      Inconsistent Labels   6000                     Mixed casing & whitespace in categories

--- 5. Final Output Shapes ---
X_train: (4680, 15)
X_test:  (1170, 15)
y_train: (4680,)
y_test:  (1170,)

--- 6. Assertions ---
All assertions passed successfully (0 NaNs, equal columns, non-zero variance).

--- 7. Before vs After Summary ---
                               Stage  Row Count  Column Count  Missing Values  Duplicate Count
                  Raw Stage (df_raw)       6000            10             500              150
Final Clean Stage (X_train + X_test)       5850            15               0                0

--- 8. Persistence ---
Saved 'cleaned_data.csv' and 'pipeline.joblib'.

--- 9. Decision Log ---
- Dropped duplicates on 'customer_id': 150 rows (2.50%) dropped to maintain entity uniqueness.
- Handled impossible 'age': 25 values (< 18) replaced with NaN for median imputation.
- Standardized 'delivery_distance_raw': 906 rows (15.10%) converted from miles to km (x 1.60934).
- Cleaned 'membership_tier': 6,000 rows stripped of whitespace and normalized to title case.
- Imputed 'annual_spend': 320 missing values (5.33%) replaced via median strategy.
- Imputed 'orders_placed': 180 missing values (3.00%) replaced via median strategy.
- Feature scaling: RobustScaler selected due to heavy right skew in expenditure distribution.

--- 10. Unresolvable Flaw ---
What remains unfixed is unobserved temporal and survivorship bias: 'days_active' combines dormant churned users with actively purchasing customers without recording inactivity timestamps. Additionally, missing route incident logs make it impossible to determine if high delivery distances stemmed from carrier rerouting rather than physical customer proximity.
