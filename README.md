# Shipment Data Validation & Semantic Modeling using Power BI

## About the Project

This project is an extension of my [Carrier Cost & Delivery Performance Analysis](https://github.com/Utkarshaaa13/Carrier-Cost-Delivery-Performance-PowerBI).

In the first project, I focused on understanding carrier costs, delivery performance, and shipment trends. For this extension, I wanted to look more closely at the data behind those metrics.

Before relying on a dashboard, I wanted to answer a few questions: Are shipment IDs unique? Are delivery dates reliable? Are there missing values or unusual records that could affect the results? And how can the data be organized to make reporting easier?

I used Power Query to validate the shipment data and Power BI to build a star schema, while keeping the existing KPI calculations unchanged.

**Tools Used:** Power BI, Power Query, DAX  
**Dataset:** 2,000 shipment records

## 1. Data Validation

I started by checking the structure and quality of the shipment data.

### Shipment ID Validation

Each row represents one shipment, so `Shipment_ID` should be unique.

Using Power Query, I grouped the records by shipment ID and checked the number of occurrences.

- Total shipments: 2,000
- Unique shipment IDs: 2,000
- Duplicate shipment IDs: 0
- Missing shipment IDs: 0

This confirmed that the dataset follows a shipment-level grain.

### Delivery Date Validation

Next, I compared `Shipment_Date` and `Delivery_Date` to identify missing or inconsistent dates.

| Finding | Records |
|---|---:|
| Valid delivery dates | 1,952 |
| Missing delivery dates | 32 |
| Delivery dates before shipment dates | 16 |

The 16 invalid records had delivery dates earlier than their shipment dates, which is not a valid delivery sequence.

Instead of replacing these values, I retained the records and used a validation column to identify the issues.

I also kept `Estimated_Delivery_Date` separate from the actual delivery date, since an estimated date should not be treated as proof that a shipment was delivered.

### Numeric Validation

I checked shipment cost, distance, and weight for invalid values.

- No zero or negative costs among non-null cost records.
- No zero or negative shipment distances.
- One shipment, `SH10829`, had a recorded weight of 0 kg.
- Some cost values were missing and were not replaced with assumed values.

The zero-weight record was retained as an anomaly rather than deleted.

## 2. Building the Star Schema

After completing the validation checks, I worked on organizing the data into a star schema.

The model has one central fact table, `FactShipments`, containing shipment-level records, costs, distances, weights, dates, and relationship keys.

I created separate dimension tables for:

- Carrier
- Origin Warehouse
- Destination
- Shipment Status
- Distance Bucket
- Shipment Date
- Delivery Date

For the categorical dimensions, I used Power Query references and removed duplicate values to create unique lookup tables.

For dates, I created calendar tables with Year, Quarter, Month, Month Number, and Week attributes.

I used separate shipment-date and delivery-date dimensions because I wanted to filter shipments by when they were shipped and when they were delivered independently.

The relationships use one-to-many cardinality with single-direction filtering.

### Final Data Model

*Insert Power BI Model View screenshot here.*

## 3. Checking the Existing KPIs

After building the model, I checked the existing DAX measures to make sure they were still calculating successfully.

| KPI | Result |
|---|---:|
| Total Shipments | 2,000 |
| Eligible Shipments | 1,801 |
| On-Time Delivery % | 89.39% |

The eligible shipment measure considers delivery status and checks that the actual delivery date is present and not earlier than the shipment date.

I kept the existing KPI definitions rather than changing the calculations during the modeling work.

These results are current-model checks; a separate comparison with the original model's saved KPI values has not been documented.

## 4. What I Learned

This project helped me understand how much work happens before data reaches a report.

Even with a relatively small dataset of 2,000 shipments, I found missing dates, invalid date sequences, and a zero-weight record.

I also gained practical experience creating dimension tables, setting up one-to-many relationships, and working with separate date dimensions for different reporting needs.

My main takeaway was that data validation and semantic modeling are important parts of building reports that people can understand and trust.

---

**Related Project:** [Carrier Cost & Delivery Performance Analysis](https://github.com/Utkarshaaa13/Carrier-Cost-Delivery-Performance-PowerBI)
