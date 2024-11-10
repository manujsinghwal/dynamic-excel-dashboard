# First Class Handicrafts - Logistics and Supply Chain Tracking Dashboard

### Project Overview
This project involves building a dynamic Excel dashboard to track the performance of various carriers used by First Class Handicrafts, a small handicraft company based in Delhi, India. The dashboard provides real-time insights into shipment deliveries, delays, and performance comparisons across multiple carriers.
\
The purpose is to help the Last Mile Team evaluate carrier efficiency, identify bottlenecks, and make data-driven decisions to improve operations.

### Data Source:
Data is pulled from a MySQL database using ODBC connection into MS Excel.
The dataset includes the following fields:
1. **ShipmentId:** An unique identification for each shipment.
2. **Carrier:** Carrier name, through which "First Class Handicraft" shipping their products across different regions in India.
3. **CarrierService:** A shipping service type.
4. **OriginCity**
5. **OriginState**
6. **OriginCountry**
7. **DestinationCity**
8. **DestinationState**
9. **DestinationCountry**
10. **ShipmentImportedTimestamp:** When shipment gets imported through website.
11. **ShipmentFulfilledSLATimestamp:** Every imported order have a SLA timestamp, time by which shipment should be fulfilled (picked and packed).
12. **ShipmentFulfilledTimestamp:** Actual shipment fulfilled timestamp, actual time by which shipment was packed and had a label for the carrier and carrier service.
13. **ShipmentCarrierPickUpTimestamp:** Timestamp when shipment picked by the carrier for delivery.
14. **ShipmentExpectedDeliveryTimestamp:** Expected delivery timestamp for shipment based on the carrier service used.
15. **ShipmentDeliveredTimestamp:** Timestamp when shipment gets delivered to the customer.
16. **OrderStatus:** Fulfilled or Cancelled
17. **DeliveryStatus:** Delivered or InTransit
18. **DeliverySubStatus** Delayed, OnTime or Pending

### Problem Statement
The Last Mile Team at First Class Handicrafts needs a way to:
1. Track carrier performance to understand how well different carriers are delivering shipments.
2. Identify delayed shipments and compare the performance of carriers across various regions and service types.
3. Dynamically update and refresh data daily to focus on shipments imported three days before the current date.
4. Filter data interactively to analyze the best and worst performing carriers.

### Dashboard Features
1. **Real-Time Data Updates:**
* Data shown in the dashboard is based on shipments imported three days before the current date to align with carrier service levels (Next Day, Two Day, and Standard (Three Day) Delivery).
* A refresh button (available in Excel) allows the team to update the data dynamically without needing to re-enter manually.
  
2. **Carrier Performance Evaluation:**
* Analyze key metrics:
  \
  i. Total orders delivered vs. total delayed orders.
  \
  ii. Average delay hours for each carrier.
  \
  iii. Best and worst performing carriers in terms of timely deliveries.

### Workflow
1. **Data Import:**
The following SQL query is used to fetch the required last mile shipment data for the dashboard. The query pulls data from the MySQL database for shipments imported three days prior to the current date.

```sql
-- Query: Shipment Data
-- Writer: Manuj Singhwal
-- Description: Fetch last mile data for curdate() - 3

WITH shipment_data_1 AS (
    SELECT
        o.ShipmentId,
        c.Carrier,
        cs.CarrierService,
        l1.City AS OriginCity,
        l1.State AS OriginState,
        l1.Country AS OriginCountry,
        l2.City AS DestinationCity,
        l2.State AS DestinationState,
        l2.Country AS DestinationCountry,
        o.ShipmentImportedTimestamp,
        o.ShipmentFulfilledSLATimestamp,
        o.ShipmentFulfilledTimestamp,
        o.ShipmentCarrierPickUpTimestamp,
        o.ShipmentDeliveredTimestamp,
        os.OrderStatus,
        CASE
            WHEN o.CarrierServiceId = 11 THEN date_add(ShipmentCarrierPickUpTimestamp, INTERVAL 1 DAY) -- Next Day Delivery
            WHEN o.CarrierServiceId = 13 THEN date_add(ShipmentCarrierPickUpTimestamp, INTERVAL 2 DAY) -- Two Day Delivery
            WHEN o.CarrierServiceId = 12 THEN date_add(ShipmentCarrierPickUpTimestamp, INTERVAL 3 DAY) -- Standard (Three) Day Delivery
        END AS ShipmentExpectedDeliveryTimestamp
    FROM fact_orders AS o
    JOIN dim_carriers AS c
        ON c.CarrierId = o.CarrierId
    JOIN dim_carrier_services AS cs
        ON cs.CarrierServiceId = o.CarrierServiceId
    JOIN dim_locations AS l1
        ON l1.LocationId = o.OriginLocationId
    JOIN dim_locations AS l2
        ON l2.LocationId = o.DestinationLocationId
    JOIN dim_order_status AS os
        ON os.StatusId = o.StatusId
    WHERE date(ShipmentImportedTimestamp) = '2024-10-01' -- Use date_sub(curdate(), interval 3 day) for general use case
),

shipment_data_2 AS (
    SELECT
        *,
        CASE
            WHEN ShipmentDeliveredTimestamp IS NULL THEN 'InTransit'
            ELSE 'Delivered'
        END AS DeliveryStatus,
        CASE
            WHEN ShipmentDeliveredTimestamp IS NULL THEN 'Pending'
            WHEN ShipmentDeliveredTimestamp > ShipmentExpectedDeliveryTimestamp THEN 'Delayed'
            WHEN ShipmentDeliveredTimestamp <= ShipmentExpectedDeliveryTimestamp THEN 'OnTime'
        END AS DeliverySubStatus
    FROM shipment_data_1
)

SELECT
    *,
    CASE
        WHEN DeliverySubStatus = 'Delayed' THEN ROUND(timestampdiff(MINUTE, ShipmentExpectedDeliveryTimestamp, ShipmentDeliveredTimestamp) / 60.0, 2)
        ELSE 0
    END AS DeliveryDelayHour
FROM shipment_data_2;
```

3. **Data Refresh:**
Users can refresh the entire report with a single click using Excel’s Refresh All button to pull updated data from the MySQL database.

4. **Performance Insights:**
The dashboard provides clear insights into which carriers are consistently on time and which are contributing to delivery delays. This enables the team to take corrective actions based on real-time data.
