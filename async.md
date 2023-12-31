# Async

For long-running operations such as adding, updating, or retrieving large amounts of data, there is a series of Oracle Hospitality Property APIs called asynchronous ("Async") APIs.

1. [How to call Async APIs](#1-how-to-call-async-apis)
2. [How frequently should I send HEAD?](#2-how-frequently-should-i-send-head)
3. [When will the response be available?](#3-when-will-the-response-be-available)
4. [Async APIs call limits](#4-async-apis-call-limits)
5. [Async APIs](#5-async-apis)
6. [Async APIs Size Limits](#6-async-apis-size-limits)
7. [Creating an External System in OPERA Cloud](#7-creating-an-external-system-in-opera-cloud)

## 1. How to call Async APIs

For Async APIs the data flow is a three step process. All 3 steps must be performed, that is, you cannot skip any step.

a. **POST**  request is the first step from an external system to OPERA Cloud, which can either be to:

* post bulk data to OPERA Cloud.  This starts a process to accept the data into OPERA Cloud. The request size maximum is 2048 KB.  Keep in mind, if posting bulk data to OPERA Cloud and it does not meet the API specifications, a validation error will be returned.
* to fetch bulk data from OPERA Cloud.  If fetching data, there may be a date limit range.

Once you have sent this post request to OPERA Cloud, you should receive a 202 Accepted response.  And in the header parameter Location a summary ID is returned.  This summary ID is important and required in the next step.

b. **HEAD** request is the second step from an external system to OPERA Cloud to check the status of the process started with POST request in the first step. Use the header parameter Location from the POST response in this HEAD request.  Once the process is completed, the HEAD request returns status of Completed and again, a header parameter Location will contain a summary ID required in step 3.

c. **GET** request is the third step from an external system to OPERA Cloud to either collect the bulk data, or confirm the post of data was successful. Use the Location Header returned by the HEAD response in step two. This GET response returns the requested data or log details in the case you have added data, and if advises if there is any failure.

## 2. How frequently should I send HEAD?

Since OPERA Cloud 23.2 HEAD requests return a header `retry-after` containing the number of seconds we recommend waiting before calling HEAD again.

Since OPERA Cloud 23.2. the final GET call to obtain the result of the Async API will include a `retry-after` header containing the number of seconds that will need to be waited before calling the same Async API again.

## 3. When will the response be available?

There is no set length of time; different requests require different amounts of processing, and operational use of OPERA Cloud will affect the speed of responding to the request.

## 4. Async APIs call limits

A maximum of 90 requests per minute per gateway can be made to the Async API. Bear this in mind when determining how frequently to poll HEAD.
___

In the operation `startReservationsDailySummaryProcess` in the OPERA Cloud Reservation Asynchronous API a request that uses the parameter `lastModifiedDate` (available from OPERA Cloud 22.5) can be called only once every 3 hours.
A given request body can be called only once every 30 minutes when starting an Asynchronous API request.
___

If partner requests data with `startDate` and `endDate` and a duplicate request (having same criteria) is sent within 30 minutes the following response is received:
```Identical request was received 0 minutes ago. Please allow at least 30 minutes between asynchronous requests.```
___

If partner requests data with `startLastModifiedDate` & `endLastModifiedDate`; irrespective of whether the subsequent request is duplicate or new, the partner will not be able to request the data again for the next 3 hours. This is intended behavior. The thought process is no one should use `LastModifiedDate` criteria more frequently than 3 hrs because it is very heavy on database.
___

Since OPERA Cloud 23.2 HEAD requests return a header `retry-after` containing the number of seconds we recommend waiting before calling HEAD again.

Since OPERA Cloud 23.2 the final GET call to obtain the result of the Async API will include a `retry-after` header containing the number of seconds that will need to be waited before calling the same Async API again.
___

## 5. Async APIs

* ReservationDailySummary
* BlockAllocationSummary
* RevenueInventoryStatistics
* SellLimits
* RestrictionProcess
* BestAvailableRatePlans
* DailyRatePlanSchedules
* HurdleRates
* RatePlanHeaders

## 6. Async APIs Size Limits

Each API has 2 MB size limit.

For example if you want to update Daily Rates using `DailyRatePlanSchedules` the maximum content in the body should be size of 2MB.

## 7. Creating an External System in OPERA Cloud

Please follow [this guide](https://docs.oracle.com/en/industries/hospitality/integration-platform/ohipu/t_polling_api.htm#PollingAPIpull-170089A2) to see how External System is created in OPERA Cloud which is required for Async APIs.

If your application is subscribed to the Streaming API you can also fetch Async APIs using the external system code displayed on the Application > Events > Subscribed tab in the Developer Portal
