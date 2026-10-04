# ml-assignment-4-pipeline
An end-to-end data pipeline converting raw logistics and churn records into a leak-free, model-ready matrix. Handles 500 missing entries, drops 150 duplicate keys, standardizes units (miles to km), engineer 4 interaction ratios, and executes median imputation, RobustScaler, and OneHotEncoder through an asserted, serialized ColumnTransformer
