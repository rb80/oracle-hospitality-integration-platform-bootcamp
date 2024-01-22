# Contactless Journey

This collection will provide you a head start on major APIs which are required for Contactless application like Kiosk.

1. [OAuth Token](#1-gettoken)
2. [Profile - Create Guest Profile](#2-postguestprofile)
3. [Profile - Create Company Profile with AR address](#3-postcompany)
4. [Profile - Create Travel Agent Profile with AR address](#4-postprofile)
5. [Booking - Fetch Hotel Availability](#5-gethotelavailability)
6. [Booking - Create Reservation](#6-postreservation)
7. [Booking - Create Multi Leg Reservation](#7-postreservation)
8. [Pre Arrival - Add Preference](#8-putreservation)
9. [Pre Arrival - Add Routing to Company](#9-postrouting)
10. [Pre Arrival - Add Routing to TA](#10-postrouting)
11. [Pre Arrival- Get available rooms](#11-gethotelrooms)
12. [Pre Arrival - Assign Rooms](#12-postroomassignment)
13. [Pre Arrival- PreCheckin](#13-precheckin)
14. [Checkin](#14-checkin)
15. [Checkin- Posting room keys](#15-postroomkey)
16. [Stay- Posting Service Request](#16-postservicerequest)
17. [Stay - Set wakeup call](#17-postwakupcall)
18. [Stay - Posting charges](#18-postbillingcharges)
19. [Checkout- Fetch Folio](#19-getfolio)
20. [Checkout - Post Billing Payment](#20-postbillingpayment)
21. [Checkout- Settle Folio](#21-postfolios)
22. [Checkout - Posting Checkout](#22-postcheckout)
23. [Checkout - Send Guest invoice to email address](#23-postemailfolioreport)

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

Note that within the postman collection provided, from the POST response `ProfileId` will be automatically inserted into the Postman environment variables.

## 3. postCompany

Create Company Profile where by adding an AR address in the payload. This is required for successful checkout of the folio to Accounts Receivable.

1. Once Company Profile is created, ensure `getProfile` API is executed so that AR address id is inserted into environment variables `CompanyArAddressId`
2. Fetch AR types which is required to create Company AR account and set environment variable `ArAccountType`
3. Create Company AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable is populated `CompanyAccountNo`
4. Use getProfile API to check all of the above values are responded correctly

## 4. PostProfile

Create Travel Agent Profile with AR Address. This is required for successful checkout of the folio to Accounts Receivable. The TA profile id is auto populated in Environment variables with Test Scripts inserted into collection

1. Once Travel Agent Profile is created, ensure `getProfile` API is executed so that `TaArAddressId` is inserted into environment variables
2. Fetch AR types which is required to create Travel Agent AR account and set environment variable `ArAccountType`. If the value doesnt differ from Company, no changes required.
3. Create Travel Agent AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable `TravelAgentAccountNo` is populated
4. Use getProfile API to check all of the above values are responded correctly

## 5. getHotelAvailability

Fetch Hotel Availability which is required for creating Reservation.

After successful fetch, make sure following are inserted into Environment variables

1. Room Type
2. RatePlanCode

For testing purposes please pick Room type `DEL` and RatePlanCode `BARRO` as they are relevant later for updating reservation. Set environment variables accordingly

The variables `CurrentDate` and `CurrentDateplus1` variable are created from getToken API test scripts.

## 6. postReservation

Create Reservation with Company and Travel Agent attached /linked
Change `id` values within `reservationIdList` and `ExternalReferences`
Other field changes required maybe is `Company Name` and `Travel Agent Name`


1. getMarketCode.  This API is required so that the variable `MarketCode` can be inserted into the environment variable
2. getSourceCode.  This API is required so that the variable `SourceCode` can be inserted into the environment variable
3. getBookingMedium.  This API is required so that the variable `BookingMedium` can be inserted into the environment variable. This is commonly called Origin Code in OPERA Cloud UI.
4. getGuaranteeCode.  This API is required so that the variable `GuaranteeCode` can be inserted into the environment variable
5. getPaymentMethod.  This API is required so that the variable `PaymentMethod` can be inserted into the environment variable
6. getReservation.  Once postReservation is executed, please check whether all required details are entered correctly with this API. Ensure the value from reservationIdList Confirmation is copied into environment variable `Confirmationid`

## 7. postReservation (multiLeg Reservation)
Create Reservation which is multi leg reservation
Change the value within `id` of `ExternalReferences` or `idContext`
After successful post, make sure the environment Variable `ReservationId2`
1. Verify the data with `getReservation`

## 8. putReservation

Pre Arrival where a preference will be added.

1. getPreference.  To insert the preference, you will need to fetch all preferences which can be attached on the reservation. For testing purposes, will be using preference group `ROOM FEATURES`. Make sure you use for testing purpose  `FBAL` as environment variable `PreferenceCode`
2. getReservation.  Once putReservation is executed, check whether your preference has been attached on the reservation

## 9. postRouting

Posting a routing instruction to existing reservation where `Food` charges goes to the Company which is linked to the reservation. Make sure the routing code `FOOD` is directed to window 2.

1. Fetch the routing Codes with `getRoutingCodes` and set Environment Variable of `CompanyRoutingCode`
2. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 2 will be paid by Direct Billing as Payment Method
3. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload. 

## 10. postRouting

Posting a routing instruction to existing reservation where `Room` charges goes to the Travel Agent which is linked to the reservation. Make sure the routing is done to window 3. Make sure the routing code `BB` is directed to window 3.

1. Fetch the routing Codes with `getRoutingCodes` and set Environment Variable of `TravelAgentRoutingCode`
2. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 3 will be paid by Direct Billing as Payment Method

## 11. getHotelRooms

Find vacant and inspected rooms so that you can assign to the reservation.
Make sure you insert the `RoomNumber` into environment variable.

## 12. postRoomAssignment

Assign room which you got from earlier API call.

## 13. preCheckin

Pre Register the guest. Ensure the `arrivalTime` is updated in the payload

## 14. checkin

Checkin the reservation with the same roomId which you assigned.

## 15. postRoomKey

Creating a Key. KeyType by default should be `New`

## 16. postServiceRequest

Create a Service request to provide towel by Housekeeping department. Ensure you change the dates within the payload

1. getServceRequestCodes.  Fetch the Service Request Codes and Department Code

## 17. postWakupcall

Create a Wakeup call on the reservation. Ensure you change the dates within the payload

1. getWakeUpcall.  Use this API to check whether the postWakeUpcall was inserted successfully into the reservation

## 18. postBillingCharges

Post charges to the room.

1. postCashier.  To Post charges you are required a cashier id. Use this API to create the cashier id
2. getCashier.  Use this API to check whether postCashier API has successfully inserted cashier id
3. getTransactionCode.  Use this API to find the relevant transaction code to successfully post charges

## 19. getFolio

Use this API to fetch Folios from each window.

## 20. postBillingPayment

Use this API to post payment against the folio from a specific window.

## 21. postFolios

Use this folio to settle the folio prior checkout.

## 22. postCheckout

Use this API to post checkout.

## 23. postEmailFolioReport

Send copy of the invoice to email.

1. getFolioTypes.  Use this API to fetch the folio type configured for executing postEmailFolioReport API
