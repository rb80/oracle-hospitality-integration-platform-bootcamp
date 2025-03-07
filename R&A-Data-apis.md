# R&A Data Apis

Report and Analytics Data APIs

1. [What is R&A Data APIs](#1-what-is-r&a-data-apis)
2. [Pre Requisites](#2-pre-requisites)
3. [Using R&A Data Apis](#3-using-r&a-data-apis)
4. [Subject Areas](#4-subject-areas)
5. [FAQ](#5-faq)

## 1. What is R&A Data APIs
For bulk data access where extraction of high levels of data is required to be done, these series of APIs can be used. These are called R&A Data APIs and they allow bulk data to be queried from the R&A Data Platform to avoid any performance impact on the transactional OPERA Cloud platform.

## 2. Pre-Requisites
In order to be able to use these APIs, you need the following product versions:
•	R&A Platform v24.4
•	OHIP Platform v24.3
•	OCIM as the Identity Platform (these APIs are not available for SSD environments)

Additionally, In order to be able to use these APIs, Hoteliers are required to purchase an additional add-on SKUs. For vendors, the existing OHIP Subscription can be used with a pay per call model or they can be included in the Hotel’s SKU contract. Please contact your Oracle Representative for further details.

## 3. Using R&A Data APIs
In order for you to be able to fetch the data from the environment you want, you need to match sure that environment is available in OHIP. For this, you need to add the environment via OHIP Developer Portal. If the required environment is already available, please ignore this step.

To be able to invoke any OHIP API, an application must be registered within OHIP Developer Portal. This application will provide you with the credentials you need to invoke these APIs. Please register one application where you subscribe to the R&A Data APIs plan (GraphQL Plan).

## 4. Subject Areas
These APIs provide access to data from existing R&A Subject Areas. Each API will provide access to one subject area. The details of which data points can be fetched for each Subject Area can be found on the respective API specification.  All these APIs are available under one single endpoint – OHIP Gateway endpoint, followed by “/rna/v1/graphql/”.

List of subject areas available:
* Financial-Transaction Details
* Bookings-Reservation
* Bookings-Block
* Statistics-Reservations Daily
* Bookings-Reservation
* Changes Log
* Rates-Codes
* Statistics-Managers Report
* Statistics-Reservations Summary
* Inventory-Rooms
* Financial-Transactions Summary
* OSEM (coming with 25.1 release)


## 5. FAQ
a. Will R&A Data APIs replace OPERA REST API
No, the new GraphQL API is designed primarily for bulk data extraction. For advanced filtering and retrieving smaller datasets, the REST API should be used. The GraphQL API enforces certain mandatory filters to discourage using it for fetching single or small numbers of records.

b. Can customers misuse the GraphQL API for bulk data extraction and overload the database or service?
There are multiple safeguards in place to prevent misuse. Data constraints limit the volume of data that can be extracted, including maximum date ranges, maximum data points, mandatory input filters, and a cap on the number of records per request. Additionally, database profiling restricts the resources allocated to individual users or requests, such as limiting CPU usage.

c. Does this mean that I no longer need to include Reporting and Analytics Data Access (B97404) when quoting one of the two new SKUs? (B110418)/ B110419)
Correct. Once we have reached General Availability, Reporting and Analytics Data Access (B97404) will not need to be sold; however, until GA status, only Reporting and Analytics Data Access (B97404) can be sold.

d. I have contracts with Reporting and Analytics Data Access (B97404) to be renewed prior the expected General Availability timeframes for the new SKUs. What should I do?
The contract should be renewed without any changes to B97404. SKU updates should be handled on the following contract renewal.

e. What are the key differences between the two SKUs?
Both SKUs provide access to R&A Data APIs for the customer’s own use. The difference lies in how API usage is charged in the case they have a vendor wishing to also access the R&A Data APIs: with one SKU, the partner pays per use; with the other, the customer covers the partner’s unrestricted access, eliminating per-use charges.

f. How can customers decide which is the best fit?
- Choosing the right SKU depends on both the customer and their vendor(s) R&A Data API needs. If vendors have specific requirements, they will communicate them directly to customers similar to OHIP partners. 
- If a customer is not using a vendor requiring R&A Data API access, then “Data Access for Customers” is the best choice.

g. Is there an ideal SKU for an existing R&A Data Access customer to choose when their contract is up for renewal? 
- To achieve a like-for-like functionality conversion, use Data Access for Customers (B110418), which allows customers to keep using the R&A data model and send data via SFTP or object storage. 
- If needed, 3rd party vendors can access the R&A Data API on a pay-per-usage basis (via OHIP SKU B92141). 
- Alternatively, the customer may purchase Data Access for Customers & Vendors (B110419) if they want to grant the vendor unrestricted access without per-usage charges. Kindly note that significant discounts will not be permitted with this option.

h. What is the best resource if I still have questions about these SKUs or If I want to better understand which SKU I should propose to my customer?
Email? 

i. How do I know if I should sell B110418 or B110419?
The key to distinguish these 2 SKUs is by understand who owns the IP of the solution. If the IP is owned by the customer, SKU B110418 should be used even if that solution is developed by an SI (SI implements but IP is owned by customer). Otherwise SKU B110419 should be used.