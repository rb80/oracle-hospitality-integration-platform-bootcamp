# R&A Data APIs Use Cases

* [0 Introduction](#0-introduction)
* [1 Get Token](#1-get-token)
* [2 Fetch Profiles changed in the last 3 days](#2-fetch-profiles)
* [3 Fetch Reservations changed in the last 3 days](#3-fetch-reservations)
* [4 Fetch Transaction details for these reservations](#4-fetch-transaction-details)

## 0 Introduction

In this bootcamp, we will go through the usage of R&A Data APIs to fetch data using streaming technologies from your R&A platform.

We will be using a client [published in GitHub](https://github.com/luisweir/rna-data-api-client) developed by Luis Weir (it is not an Oracle official R&A Data API client). This client connects to OHIP and executes the queries you define. The output will be stored locally in your laptop as text files.

## 1 Get Token

This is required to access Oracle Hospitality APIs.

To obtain a token include the following headers:

* A Basic authentication header using the base64 hash of your Client ID and Client Secret in the format ClientID:ClientSecret - base64 encoded to the Basic Access Authorization standard
* Your application key in the x-app-key header

And the following body parameters:

* Body parameters for obtaining your initial access token
* `grant_type`. We're using `client_credentials`
* username. Use with client Id value
* password. Use with Client Secret Value
* scope. Use value "urn:opc:hgbu:ws:__myscopes__"

## 2 Fetch Profiles

Here you will be fetching the profiles you've modified during the OHIP Lab and store them locally.

For this, you'll use the Profile-Individuals Subject Area API and filter by the property you've been using as well as by their updated date. You can modify the input provided to change the requested data points.

If you're using the [sample GraphQL client](https://github.com/luisweir/rna-data-api-client), in folder "queries" there are some sample queries you can leverage. Create your version of the GraphQL query you want to use to fetch your profiles. Under folder "filters", you have also some samples of filter. Create your version to filter profiles changed in the last 3 days of the property you've been using.

Input parameters needed:

* profileallDetailsResortRegistered (optional)
* profileallDetailsUpdateDate

## 3 Fetch Reservations

Here you will be fetching the reservations you've created during the OHIP Lab and store them locally.

For this, you'll use the Booking-Reservation Subject Area API and filter by the property you've been using as well as by their begin and end dates. You can modify the input provided to change the requested data points.

If you're using the [sample GraphQL client](https://github.com/luisweir/rna-data-api-client), in folder "queries" there are some sample queries you can leverage. Create your version of the GraphQL query you want to use to fetch your reservations. Under folder "filters", you have also some samples of filter. Create your version to filter reservations changed in the last 3 days of the property you've been using.

Input parameters needed:

* reservationDetailsResort
* reservationDetailsTruncBeginDate
* reservationDetailsTruncEndDate

## 4 Fetch Transaction Details

Here you will be fetching the transactions you've created during the OHIP Lab and store them locally.

For this, you'll use the Financial-TransactionDetails Subject Area API and filter by the property you've been using as well as by their business and transaction dates. You can modify the input provided to change the requested data points.

If you're using the [sample GraphQL client](https://github.com/luisweir/rna-data-api-client), in folder "queries" there are some sample queries you can leverage. Create your version of the GraphQL query you want to use to fetch your transactions. Under folder "filters", you have also some samples of filter. Create your version to filter transactions created in the last 3 days related to your reservations.

Input parameters needed:

* financialtransDetailsResvNameId
* financialtransDetailsBusinessdate
* financialtransDetailsResort
* financialtransDetailsTrxDate
