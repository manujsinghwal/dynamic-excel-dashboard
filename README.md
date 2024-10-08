# First Class Handicrafts - Logistics and Supply Chain Tracking Dashboard

### Project Overview
This project involves building a dynamic Excel dashboard to track the performance of various carriers used by First Class Handicrafts, a small handicraft company based in Delhi, India. The dashboard provides real-time insights into shipment deliveries, delays, and performance comparisons across multiple carriers.
\
The purpose is to help the Last Mile Team evaluate carrier efficiency, identify bottlenecks, and make data-driven decisions to improve operations.

### Data Source:
Data is pulled from a MySQL database using ODBC connection into MS Excel.
The dataset includes the following information:
Shipment Details: Shipment ID, Carrier, Origin, Destination, Timestamps, Order Status, Delivery Status.
Delivery Information: SLA, Actual Delivery Time, Delay Hours.
Carrier Services: Next Day Delivery, Two Day Delivery, Standard (Three Day Delivery).

### Data Structure:
1. **ShipmentId:** An unique identification for each shipment. 
2. **Carrier:** Carrier name, through which "First Class Handicraft" shipping their products across entire India.
3. **CarrierService:** A shipping service type
4. OriginCity
5. OriginState
6. OriginCountry
7. DestinationCity
8. DestinationState
9. DestinationCountry
10. ShipmentImportedTimestamp
11. ShipmentFulfilledSLATimestamp
12. ShipmentFulfilledTimestamp
13. ShipmentCarrierPickUpTimestamp
14. ExpectedDeliveryTimestamp
15. ShipmentDeliveredTimestamp
