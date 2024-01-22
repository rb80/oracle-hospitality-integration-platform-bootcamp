# Contactless Journey

This collection will provide you a head start on major APIs which are required for Contactless application like Kiosk.

1. [OAuth Token](#1-gettoken)
2. [Profile - Create Guest Profile](#2-postguestprofile)
3. [Profile - Create Company Profile with AR address](#3-postcompany)
4. [Profile - Create Travel Agent Profile with AR address](#4-postprofile)
5. [Booking - Fetch Hotel Availability](#5-gethotelavailability)
6. [Booking - Create Reservation](#6-postreservation)
7. [Pre Arrival - Add Preference](#7-putreservation)
8. [Pre Arrival - Add Routing to Company](#8-postrouting)
9. [Pre Arrival - Add Routing to TA](#9-postrouting)
10. [Pre Arrival- Get available rooms](#10-gethotelrooms)
11. [Pre Arrival - Assign Rooms](#11-postroomassignment)
12. [Pre Arrival- PreCheckin](#12-precheckin)
13. [Checkin](#13-checkin)
14. [Checkin- Posting room keys](#14-postroomkey)
15. [Stay- Posting Service Request](#15-postservicerequest)
16. [Stay - Set wakeup call](#16-postwakupcall)
17. [Stay - Posting charges](#17-postbillingcharges)
18. [Checkout- Fetch Folio](#18-getfolio)
19. [Checkout - Post Billing Payment](#19-postbillingpayment)
20. [Checkout- Settle Folio](#20-postfolios)
21. [Checkout - Posting Checkout](#21-postcheckout)
22. [Checkout - Send Guest invoice to email address](#22-postemailfolioreport)

## 1. getToken

This is required to access Oracle Hospitality OPERA Cloud REST APIs.

To obtain a token include the following headers:

- A Basic authentication header using the base64 hash of your Client ID and Client Secret in the format ClientID:ClientSecret - base64 encoded to the Basic Access Authorization standard
- Your application key in the x-app-key header

And the following body parameters:

- Body parameters for obtaining your initial access token
- `grant_type`. Required to be `password` or `client_credentials`
- username. If `grant_type` is `password`, set this to your OPERA Cloud integration username. (commonly known as interface id)
- password. If `grant_type` is password, set this to your OPERA Cloud integration user's password. (commonly known as interface key)
- scope. If `grant_type` is client_credentials, set this to your assigned scope.

## 2. postGuestProfile

Create a guest Profile with minimum data.

Note that within the postman collection provided, from the POST response `profileId` will be automatically inserted into the Postman environment variables.

## 3. postCompany

Create Company Profile where by adding an AR address in the payload. This is required for successful checkout of the folio to Accounts Receivable.

1. Once Company Profile is created, ensure `getProfile` API is executed so that AR address id is inserted into environment variables
2. Fetch AR types which is required to create Company AR account
3. Create Company AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable is populated
4. Use getProfile API to check all of the above values are responded correctly

## 4. PostProfile

Create Travel Agent Profile with AR Address. This is required for successful checkout of the folio to Accounts Receivable.

1. Once Travel Agent Profile is created, ensure `getProfile` API is executed so that AR address id is inserted into environment variables
2. Fetch AR types which is required to create Travel Agent AR account
3. Create Travel Agent AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable is populated
4. Use getProfile API to check all of the above values are responded correctly

## 5. getHotelAvailability

Fetch Hotel Availability which is required for creating Reservation.

After successful fetch, make sure following are inserted into Environment variables

1. Room Type
2. RatePlanCode

For testing purposes please pick room type `DEL` and ratePlanCode `BARRO`

The variables `currentDate` and `currentDateplus1` variable are created from getToken API test scripts.

## 6. postReservation

Create Reservation with Company and Travel Agent attached /linked

1. getMarketCode.  This API is required so that the variable `MarketCode` can be inserted into the environment variable
2. getSourceCode.  This API is required so that the variable `SourceCode` can be inserted into the environment variable
3. getBookingMedium.  This API is required so that the variable `BookingMedium` can be inserted into the environment variable. This is commonly called Origin Code in OPERA Cloud UI.
4. getGuaranteeCode.  This API is required so that the variable `GuaranteeCode` can be inserted into the environment variable
5. getPaymentMethod.  This API is required so that the variable `PaymentMethod` can be inserted into the environment variable
6. getReservation.  Once postReservation is executed, please check whether all required details are entered correctly with this API.

## 7. putReservation

Pre Arrival where a preference will be added.

1. getPreference.  To insert the preference, you will need to fetch all preferences which can be attached on the reservation. For testing purposes, will be using preference group `ROOM FEATURES`
2. getReservation.  Once putReservation is executed, check whether your preference has been attached on the reservation

## 8. postRouting

Posting a routing instruction to existing reservation where `Food` charges goes to the Company which is linked to the reservation. Make sure the routing is done to window 2.

1. getRoutingCodes
2. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 2 will be paid by Direct Billing as Payment Method

## 9. postRouting

Posting a routing instruction to existing reservation where `Room` charges goes to the Travel Agent which is linked to the reservation. Make sure the routing is done to window 3.

1. getRoutingCodes
2. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 3 will be paid by Direct Billing as Payment Method

## 10. getHotelRooms

Find vacant and inspected rooms so that you can assign to the reservation.
Make sure you insert the `roomId` into environment variable.

## 11. postRoomAssignment

Assign room which you got from earlier API call.

## 12. preCheckin

Pre Register the guest

## 13. checkin

Checkin the reservation with the same roomId which you assigned.

## 14. postRoomKey

Creating a Key. KeyType by default should be `New`

## 15. postServiceRequest

Create a Service request to provide towel by Housekeeping department

1. getServce request.  Check whether earlier API postServiceRequest was inserted correctly on the Reservation

## 16. postWakupcall

Create a Wakeup call on the reservation.

1. getWakeUpcall.  Use this API to check whether the postWakeUpcall was inserted successfully into the reservation

## 17. postBillingCharges

Post charges to the room.

1. postCashier.  To Post charges you are required a cashier id. Use this API to create the cashier id
2. getCashier.  Use this API to check whether postCashier API has successfully inserted cashier id
3. getTransactionCode.  Use this API to find the relevant transaction code to successfully post charges

## 18. getFolio

Use this API to fetch Folios from each window.

## 19. postBillingPayment

Use this API to post payment against the folio from a specific window.

## 20. postFolios

Use this folio to settle the folio prior checkout.

## 21. postCheckout

Use this API to post checkout.

## 22. postEmailFolioReport

Send copy of the invoice to email.

1. getFolioTypes.  Use this API to fetch the folio type configured for executing postEmailFolioReport API
