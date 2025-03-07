# R&A Data Apis

Report and Analytics Data APIs

1. [What is R&A Data APIs](#1-what-is-r&a-data-apis)
2. [FAQ](#2-faq)

## 1. What is R&A Data APIs
Used for Data Extraction


## 2. FAQ
a. Will R&A Data APIs replace OPERA REST API
No, the new GraphQL API is designed primarily for bulk data extraction. For advanced filtering and retrieving smaller datasets, the REST API should be used. The GraphQL API enforces certain mandatory filters to discourage using it for fetching single or small numbers of records.

b. Can customers misuse the GraphQL API for bulk data extraction and overload the database or service?
There are multiple safeguards in place to prevent misuse. Data constraints limit the volume of data that can be extracted, including maximum date ranges, maximum data points, mandatory input filters, and a cap on the number of records per request. Additionally, database profiling restricts the resources allocated to individual users or requests, such as limiting CPU usage.
