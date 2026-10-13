# Shipment Data Validation & Semantic Modeling | Power BI

## Project Overview

Before analyzing shipment costs and delivery performance, it's important to understand the quality and structure of the underlying data.

In this project, I worked with a dataset of 2,000 shipment records to identify data quality issues and build a star schema in Power BI.

My focus was on validating shipment identifiers, checking missing and inconsistent values, and organizing the dataset into fact and dimension tables for reporting.

**Tools:** Power BI, Power Query, DAX  
**Dataset:** 2,000 shipment records

## 1. Understanding and Validating the Data

The dataset contains shipment details such as carrier, origin warehouse, destination, shipment and delivery dates, weight, cost, distance, and shipment status.

I started by confirming the data grain: one row represents one shipment.

### Shipment ID Validation

I used Power Query to group records by `Shipment_ID` and check for duplicates.

| Check | Result |
|---|---:|
| Total records | 2,000 |
| Unique shipment IDs | 2,000 |
| Duplicate IDs | 0 |
| Missing IDs | 0 |

This confirmed that `Shipment_ID` uniquely identifies each shipment.

### Delivery Date Validation

I compared actual delivery dates with shipment dates and identified three groups of records.

| Validation result | Records |
|---|---:|
| Valid date sequences | 1,952 |
| Missing delivery dates | 32 |
| Delivery dates earlier than shipment dates | 16 |

Rather than deleting or replacing these records, I retained them and added a validation column to distinguish valid, missing, and inconsistent dates.

I also kept estimated delivery dates separate from actual delivery dates.

### Numeric Data Validation

I checked shipment cost, distance, and weight for missing or invalid values.

The checks identified:

- No zero or negative costs among non-null cost records.
- No zero or negative shipment distances.
- One zero-weight shipment (`SH10829`).
- Missing cost values that were retained without imputation.

These checks helped identify which records may require additional attention during analysis.

## 2. Designing the Star Schema

After validating the dataset, I organized it into a star schema.

### Fact Table

`FactShipments` contains the shipment-level records, including:

- Shipment ID
- Shipment, delivery, and estimated delivery dates
- Carrier, warehouse, destination, and status keys
- Shipment cost, weight, distance, and transit days
- Data validation information

### Dimension Tables

I created seven dimension tables:

| Dimension | Purpose |
|---|---|
| DimCarrier | Carrier information |
| DimWarehouse | Origin warehouse |
| DimDestination | Shipment destination |
| DimStatus | Shipment status |
| DimDistanceBucket | Distance categories |
| DimShipmentDate | Shipment calendar |
| DimDeliveryDate | Delivery calendar |

The categorical dimensions were created using Power Query references, keeping distinct values for each relationship key.

For date analysis, I created separate calendar dimensions containing Year, Quarter, Month, Month Number, and Week.

This allows shipment dates and actual delivery dates to be filtered independently.

### Relationships

<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/d46ac962-6082-4cde-b3c2-96348545ee50" />

The model uses one-to-many relationships between dimension tables and `FactShipments`, with single-direction filtering.

The dimension columns serve as unique relationship keys, while the corresponding columns in `FactShipments` serve as foreign keys.

## 3. Validating the Model

After creating the relationships, I checked the existing DAX measures.

<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/32515684-b693-4d23-bbc9-80063b16a6c5" />



| Measure | Result |
|---|---:|
| Total Shipments | 2,000 |
| Eligible Shipments | 1,801 |
| On-Time Delivery % | 89.39% |

The eligible shipment calculation considers shipment status, missing delivery dates, and invalid delivery-date sequences.

These checks confirmed that the measures returned results consistent with the validation findings.

## 4. Key Takeaways

Working on this project helped me understand how data quality checks and semantic modeling support reliable reporting.

I gained hands-on experience with:

- Validating data at the correct grain.
- Identifying missing values and inconsistent dates.
- Handling anomalies without making unsupported assumptions.
- Creating fact and dimension tables.
- Building one-to-many relationships.
- Using separate date dimensions for different reporting requirements.
- Checking KPI calculations after model changes.

The resulting model provides a structured foundation for shipment and carrier performance analysis.

## Next Step: Carrier Performance Analysis

The next stage focuses on analyzing carrier costs, delivery reliability, and shipment performance using Power BI.

[Carrier Cost & Delivery Performance Analysis](https://github.com/Utkarshaaa13/Carrier-Cost-Delivery-Performance-PowerBI)
