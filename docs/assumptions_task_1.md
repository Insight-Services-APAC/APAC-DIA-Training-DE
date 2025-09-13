# Understanding Confirmation

I confirm that I understand the following about the data schemas:

- **The purpose of each table in the business context**  
  Customers, Products, Stores, Suppliers (dimensions), Orders (facts), Events/Sensors (activity and IoT), Exchange Rates, Shipments, and Returns — and how they work together to represent the company’s data.

- **Required data types and precision for each field**  
  BIGINT for IDs, DECIMAL for prices/rates (with correct precision/scale), DATE for dates, TIMESTAMP with microsecond precision for event/transaction times, and Boolean/String types where appropriate.

- **Partitioning strategies for large tables**  
  Orders partitioned by `order_dt`, Events by `event_dt`, Sensors by `store_id` and `month` — to keep large datasets efficient to query and process.

- **Expected anomalies and their rates**  
  Each dataset includes controlled anomalies such as malformed emails, missing prices, invalid foreign keys, duplicates, out-of-range sensor readings, and schema evolution for Returns — in the specified percentages.

- **Relationships between tables (foreign keys)**  
  Orders link to Customers and Stores, Order Lines link to Orders and Products, Shipments link to Orders, and Returns link to Orders and Products — with about 99% valid references and 1% intentional violations.

- **File formats and naming conventions**  
  CSV for most tables, JSONL for events, XLSX for exchange rates, Parquet for shipments, Delta for returns — and following the required folder/partition naming (e.g. `orders/order_dt=YYYY-MM-DD/part-0001.csv`).

# My Assumptions

## General
- I use UTC for all timestamps and keep order → shipment → delivery → return sequence correct.
- About 1% of foreign keys are intentionally invalid for testing.
- File names follow the given patterns.

## Customers
- Around 5% customers are VIP.
- 0.5–1% emails are broken, 0.2% duplicate natural_keys.
- Some phone numbers and address fields are left empty.

## Products
- About 0.3% of prices are missing or invalid.
- Some discontinued products do not have discontinued_dt.

## Stores
- A few rows have wrong lat/lon values.
- A few duplicate store_codes are added.

## Orders
- Orders are spread across the year.
- 1% customer_id/store_id are invalid.
- 0.05% order_ids are duplicated across files.

## Order Lines
- 1% invalid product_ids.
- Rare negative qty or zero price to simulate errors.

## Events
- 0.05% of lines are malformed JSON.
- Some missing user_id or session_id.

## Sensors
- 0.1–0.5% readings are out of range.
- A few rows missing sensor_ts.

## Exchange Rates
- Weekend rates copied from Friday.

## Shipments
- Some shipments missing delivered_at (still in transit).
- Some delivered late on purpose.

