# Async

For long-running operations such as adding, updating, or retrieving large amounts of data, there is a series of Oracle Hospitality Property APIs called asynchronous ("Async") APIs.

- [Async](#async)
  - [0. Objective](#0-objective)
  - [1. What are Async APIs](#1-what-are-async-apis)
  - [2. Workflow](#2-workflow)
  - [3. Best Practices](#3-best-practices)
  - [4. Types and Usage Recommendations](#4-types-and-usage-recommendations)
    - [:one: Operations that fetch data from OPERA Cloud](#one-operations-that-fetch-data-from-opera-cloud)
      - [Module: Inventory (INVASYNC)](#module-inventory-invasync)
      - [Module: Blocks (BLKASYNC) block allocation summary](#module-blocks-blkasync-block-allocation-summary)
      - [Module: Reservations (RSVASYNC) reservation daily summary](#module-reservations-rsvasync-reservation-daily-summary)
    - [:two: Operations that POST Data to OPERA Cloud](#two-operations-that-post-data-to-opera-cloud)
      - [Module: Rate Plan (RTPASYNC)](#module-rate-plan-rtpasync)
      - [Module: Availability (PARASYNC)](#module-availability-parasync)
      - [Module: Inventory (INVASYNC) sell limits](#module-inventory-invasync-sell-limits)
      - [Module: Blocks (BLKASYNC) shift blocks](#module-blocks-blkasync-shift-blocks)
      - [Module: Reservation (RSVASYNC) create block reservations](#module-reservation-rsvasync-create-block-reservations)
      - [Module: Profiles (CRMASYNC)](#module-profiles-crmasync)
  - [5. Creating an External System in OPERA Cloud](#5-creating-an-external-system-in-opera-cloud)
  - [6. Implementation Guide](#6-implementation-guide)
  - [7. References](#7-references)
  - [8. Upcoming Async APIs](#8-upcoming-async-apis)
    - [Inventory Asynchronous (INVASYNC) new operation](#inventory-asynchronous-invasync-new-operation)
    - [Blocks Asynchronous (BLKASYNC) updates](#blocks-asynchronous-blkasync-updates)
    - [Blocks Asynchronous (BLKASYNC) new operation](#blocks-asynchronous-blkasync-new-operation)
    - [Cashiering Asynchronous (CSHASYNC) new operation](#cashiering-asynchronous-cshasync-new-operation)
  - [9. Async Lab](#9-async-lab)
    - [Send the first request](#send-the-first-request)
    - [Send the second request](#send-the-second-request)
    - [Send the third request](#send-the-third-request)

## 0. Objective

After completing this lab you will have:

1. Understood more about asynchronous APIs, their workflow and best practices for their use
2. Called an asynchronous API from start to finish

## 1. What are Async APIs

The adoption of Revenue Management has become more prevalent among properties aiming to optimize the value of each room. The asynchronous Property APIs offer a means to collect and update bulk data efficiently. There are two distinct approaches to retrieve data from OPERA Cloud through asynchronous Property APIs and business event-driven Property APIs. The asynchronous Property APIs are specifically designed for revenue management systems, allowing smooth updates of bulk data like inventory, restrictions, and room rates in OPERA Cloud. These APIs are fully compatible with OPERA Cloud versions 23.1.x and above.

## 2. Workflow

For an [overview of async APIs, including workflow diagrams, see this page.](https://docs.oracle.com/en/industries/hospitality/integration-platform/ohipu/c_oracle_hospitality_async_apis_using_the_apis.htm)

The Asynchronous APIs involve a three-step data flow process, and it is imperative to execute all three steps without skipping any. Please refer to the `Best Practices` section for additional guidelines.

:a: POST request is the first step from an external system to OPERA Cloud, which can either be to

- POST Data to OPERA Cloud - This starts a process to accept the data into OPERA Cloud.
- Fetch Data from OPERA Cloud - This starts a process to retrieve data from OPERA Cloud.

Once you have sent this post request to OPERA Cloud, you should receive an HTTP 202 Accepted response if the request is valid.  The response header parameter "location" provides a URL which contains a request ID. This ID is required in step 2.

:warning: **Note**: Please refer to the usage recommendations to learn more about the associated API limits. If the bulk data to be posted to or retrieved from OPERA Cloud doesn't align with the API specifications, a validation error will be generated.

:b: HEAD request is the second step from an external system to OPERA Cloud to check the status of the process started with POST request in the first step. Use the header parameter "location" from the POST response in this HEAD request.  Once the process is completed, the HEAD response returns a header parameter "status" with value "COMPLETED". Similar to Step 1, the header parameter "location" will return a URL that contains a request ID required in step 3.

:c: GET request is the third step from an external system to OPERA Cloud to either collect the bulk data, or confirm the post of data was successful. Use the ID returned in the "location" header URL returned by the HEAD response in step 2. The GET response provides the requested data or log specifics, particularly if you've added data, and indicates any potential failures.

## 3. Best Practices

- Asynchronous APIs allow a maximum of 150 requests per minute for each application, encompassing POST, HEAD, and GET requests. Please take this into account when deciding the polling frequency for HEAD requests.
- After sending a POST request, please allow a waiting period of at least 1-2 minutes before initiating the HEAD request.
- When sending a HEAD request, wait for the HTTP status 201 Created response before proceeding with the GET request.
- If the HEAD request gives an HTTP Status 200 OK response, please allow another 1-2 minutes before resending the HEAD request. In other words, if the HEAD response hasn't returned a 201 Created response with a header location, it is likely that the job has not finished yet. Please allow another 1-2 minutes before sending the GET request. After receiving an HTTP status 201 Created, you can proceed with the GET request.
- Remember that multiple retries of the HEAD call might be necessary, depending on the volume of data being returned.

## 4. Types and Usage Recommendations

The currently supported Async API operations have been listed below. You can access examples of these operations in the References section.

The Asynchronous APIs can be classified into two types according to how they interact with OPERA Cloud:

### :one: Operations that fetch data from OPERA Cloud

This is applicable to the following operations along with the limits specified below:

#### Module: Inventory (INVASYNC)
  
Enables you to retrieve Revenue Inventory Statistics

You can use this API to fetch revenue inventory statistics for past, present, and future reservations from OPERA Cloud. You will be able to filter using stay date (with a start and end date) to fetch inventory data.

#### Module: Blocks (BLKASYNC) block allocation summary

Enables you to retrieve Block Allocation Summary

You can use this API will fetch Block allocation information for a hotel, and specified date range. The block allocated inventory, rates and room type statistics, including revenue, are returned as part of the response.

#### Module: Reservations (RSVASYNC) reservation daily summary

Enables you to retrieve Reservation Daily Summary

This API allows external systems to retrieve a summary of reservations for a specified hotel and date range

### :two: Operations that POST Data to OPERA Cloud

This is applicable to the following operations:

#### Module: Rate Plan (RTPASYNC)

This includes:

- startSetDailyRatePlanSchedulesProcess
- startSetBestAvailableRatePlansProcess
- startHurdleRatesProcess

Enables you to create Daily Rate Plan Schedules

You can use this API to add and/or update the rate price schedule to existing OPERA Daily Rate plans.

Enables you to create Best Available Rate Plans by Length of Stay or by Day.

You can use this API to post new or update existing Best Available Rate by Length Of Stay or by DAY to OPERA Cloud.

Enables you to create Hurdle Rates

You can use this API to create hurdle rates in OPERA Cloud by date.

#### Module: Availability (PARASYNC)

Enables you to create Restrictions

A user can send various restrictions to OPERA Cloud by specifying restriction details in the request. You can set restrictions for a whole year and have multiple restrictions on a given day.  However, there can be a hierarchy of restrictions.

#### Module: Inventory (INVASYNC) sell limits

Enables you to create Sell Limits

You can use this API to create sell limits in OPERA Cloud by date.

#### Module: Blocks (BLKASYNC) shift blocks

Provides the ability to shift block dates for a business block with rooms inventory or events or both.

#### Module: Reservation (RSVASYNC) create block reservations

Enables you to create multiple block reservations in OPERA Cloud for a specific block as well as match the associated names with existing profiles.

#### Module: Profiles (CRMASYNC)

Allows you to import Stay Records for a profile

## 5. Creating an External System in OPERA Cloud

Please follow [this guide](https://docs.oracle.com/en/industries/hospitality/integration-platform/ohipu/t_polling_api.htm#PollingAPIpull-170089A2) to see how an External System is created in OPERA Cloud which is required for Async APIs.

If your application is subscribed to the Streaming API you can also fetch Async APIs using the external system code displayed on the Application > Events > Subscribed tab in the Developer Portal.

## 6. Implementation Guide

To find more information on Async APIs, please review our Implementation Guide [Revenue Management Systems (RMS) Implementation Guide](https://docs.oracle.com/en/industries/hospitality/integration-platform/msrig/c_business_context.htm).

## 7. References

1. [Async User Guide](https://docs.oracle.com/search/?q=Oracle+Hospitality+Asynchronous+APIs&lang=en&book=OHIPU&library=en%2Findustries%2Fhospitality%2Fintegration-platform)
2. [RMS Implementation Guide](https://docs.oracle.com/search/?q=Revenue%20Management%20Systems%20(RMS)%20Implementation%20Guide&pg=1&size=10&showfirstpage=true&lang=en)

## 8. Upcoming Async APIs

### Inventory Asynchronous (INVASYNC) new operation

Added a new operation getInventoryStatisticsAsync.

A new getInventoryStatisticsAsync operation will be introduced in the Inventory Asynchronous (INV Async) API with OPERA Cloud version 24.5 and higher. This allows you to retrieve hotel inventory statistics for a specified date range (up to 365 days) provided in the request.

### Blocks Asynchronous (BLKASYNC) updates

Updated operation getBlockAllocationSummary.

The getBlockAllocationSummary operation for the Block Async API (BLK Async API) is updated to include the currency element at the block level. When you send the request for the allocationSummary, and the currency information is available, it is returned in the response at the block level.

A new criterion includeNetRates will be introduced in the getBlockAllocationSummary operation for the Block Asynchronous (BLK Async) API in the OPERA Cloud version 25.2 and higher. This enables to display the net rates information in the response after the rates section.

### Blocks Asynchronous (BLKASYNC) new operation

Added a new operation recalculateRoomForecast.

A new asynchronous operation, recalculateRoomForecast will be introduced in the Block Async (BLKASYNC) API with the OPERA Cloud version 25.1 and higher. This will enable you to recalculate the room forecast on the room and rate grid every time there is a change that impacts the forecasted revenue. The changes that impact the forecasted revenue include adding or changing a rate code to the block header, shifting the block to a new date where the rate code has a different schedule, adding or changing a rate code transaction to the existing rate, and adding or changing packages on the block header.  

### Cashiering Asynchronous (CSHASYNC) new operation

Added a new operation getFinancialPostingsNetVat.

A new asynchronous operation getFinancialPostingsNetVat will be introduced in the Cashiering  Async (CSHASYNC) API with the OPERA Cloud version 25.1 and higher. This will enable you to retrieve financial postings along with their net VAT breakdown.

## 9. Async Lab

1. Ensure the environment selected is "Bootcamp Reseller".
2. Set the `ExtSystemCode` parameter to the external system code provided to you by the Oracle team.

### Send the first request

1. In Postman expand the `1. Property REST APIs By Module` collection, then expand the folder `Asynchronous APIs`, then expand the folder `Reservation (RSVASYNC)`.
2. Click the request `start Reservations Daily Summary Process`
3. Click the `Body` tab and change the dates in the payload to the below
4. Send the request
5. Open the console, click the request, and scroll down to the Response Headers.  Highlight the last part of the URL in the `Location` header and choose `Set:Bootcamp Reseller` then choose `SummaryId`

``` json
{
  "criteria": {
    "hotelId": "{{HotelId}}",
    "timeSpan": {
      "startDate": "2025-01-02",
      "endDate": "2025-03-31"
    }
  }
}
```

### Send the second request

1. In Postman expand the `1. Property REST APIs By Module` collection, then expand the folder `Asynchronous APIs`, then expand the folder `Reservation (RSVASYNC)`.
2. Click the request `get Reservations Process Status`
3. Send the request
4. Look in the Console to see the response headers
5. If the `status` header is `COMPLETED` then:

a. Highlight the last part of the URL in the `Location` response header and choose `Set:Bootcamp Reseller` then choose `SummaryId`
b. Proceed to send the third request

### Send the third request

1. In Postman expand the `1. Property REST APIs By Module` collection, then expand the folder `Asynchronous APIs`, then expand the folder `Reservation (RSVASYNC)`.
2. Click the request `get Reservations Daily Summary`
3. Send the request
