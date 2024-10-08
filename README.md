# First Class Handicrafts - Logistics and Supply Chain Tracking Dashboard

### Project Overview
This project involves building a dynamic Excel dashboard to track the performance of various carriers used by First Class Handicrafts, a small handicraft company based in Delhi, India. The dashboard provides real-time insights into shipment deliveries, delays, and performance comparisons across multiple carriers.
\
The purpose is to help the Last Mile Team evaluate carrier efficiency, identify bottlenecks, and make data-driven decisions to improve operations.

### Data Source:
Data is pulled from a MySQL database using ODBC connection into MS Excel.
The dataset includes the following fields:
1. **ShipmentId:** An unique identification for each shipment.
2. **Carrier:** Carrier name, through which "First Class Handicraft" shipping their products across entire India.
3. **CarrierService:** A shipping service type
4. **OriginCity**
5. **OriginState**
6. **OriginCountry**
7. **DestinationCity**
8. **DestinationState**
9. **DestinationCountry**
10. **ShipmentImportedTimestamp:** When shipment imported to our system (throught website).
11. **ShipmentFulfilledSLATimestamp:** Every imported order have an SLA timestamp, time by which shipment should be fulfilled (picked and packed).
12. **ShipmentFulfilledTimestamp:** Actual shipment fulfilled timestamp, actual time by which shipment assigned to a carrier service.
13. **ShipmentCarrierPickUpTimestamp:** Timestamp when shipment picked by the carrier.
14. **ShipmentDeliveredTimestamp:** Timestamp when shipment gets delivered to the customer.
15. **OrderStatus:** Fulfilled or Cancelled
16. **ShipmentExpectedDeliveryTimestamp:** Expected delivery timestamp for shipment based on the carrier service used.
17. **DeliveryStatus:** Delivered or InTransit
18. **DeliverySubStatus** Delayed, OnTime or Pending
