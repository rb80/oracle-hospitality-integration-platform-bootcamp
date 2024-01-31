# Implementation Use Cases

1. [OAuth Token](#1-gettoken)
2. [Profile - Create Guest Profile](#2-postguestprofile)
3. [Profile - Create Company Profile with AR address](#3-postCompany)
4. [Profile - Create Company AR account](#4-postAccount)
5. [Profile - Create Travel Agent Profile with AR address](#5-postProfile)
6. [Profile - Create Travel Agent AR account](#6-postAccount)
7. [Booking - Fetch Hotel Availability](#7-gethotelavailability)
8. [Booking - Create Reservation](#8-postreservation)
9. [Booking - Create Multi Leg Reservation](#9-postreservation)
10. [Pre Arrival - Add Preference](#10-putreservation)
11. [Pre Arrival - Add Routing to Company](#11-postrouting)
12. [Pre Arrival - Add Payment Method](#12-putreservation)
13. [Pre Arrival - Add Routing to TA](#13-postrouting)
14. [Pre Arrival - Add Payment Method](#14-putreservation)
15. [Pre Arrival - Convert Pan into token](#15-openPaymentTokenExchange)
16. [Pre Arrival - Add Payment Method](#16-postpaymentmethod)
17. [Pre Arrival- Get available rooms](#17-gethotelrooms)
18. [Pre Arrival - Assign Rooms](#18-postroomassignment)
19. [Pre Arrival- PreCheckin](#19-precheckin)
20. [Checkin](#20-checkin)
21. [Checkin- Posting room keys](#21-postroomkey)
22. [Stay- Posting Service Request](#22-postservicerequest)
23. [Stay - Set wakeup call](#23-postwakupcall)
24. [Stay - Create Cashier](#23-postCashiers)
25. [Stay - Create Cashier](#23-postBillingCharges)
26. [Stay - Create Cashier](#23-postBillingCharges)
27. [Stay - Posting of Advance Folio](#27-postAdvanceRoomCharges)
28. [Checkout- Fetch Folio](#28-getfolio)
29. [Checkout - Post Billing Payment](#29-postbillingpayment)
30. [Checkout - Post Billing Payment](#30-postbillingpayment)
31. [Checkout - Post Billing Payment](#31-postbillingpayment)
32. [Checkout - Post Billing Payment](#32-putEarlyDeparture)
33. [Checkout - Post Billing Payment](#33-postFinalCharges)
34. [Checkout- Settle Folio](#34-postfolios)
35. [Checkout - Posting Checkout](#35-postcheckout)
36. [Checkout - Send Guest invoice to email address](#36-postemailfolioreport)

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

1. Change `companyName`and `address`as required`
2. Once Company Profile is created, ensure `getProfile` API is executed so that `AR address id` is inserted into environment variables `CompanyArAddressId`

## 4. PostAccount
1. Fetch AR types which is required to create Company AR account and set environment variable `ArAccountType`
2. Create Company AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable is populated `CompanyAccountNo`
3. Use getProfile API to check all of the above values are responded correctly

## 5. PostProfile

Create Travel Agent Profile with AR Address. This is required for successful checkout of the folio to Accounts Receivable. The TA profile id is auto populated in Environment variables with Test Scripts inserted into collection

1. Once Travel Agent Profile is created, ensure `getProfile` API is executed so that `TaArAddressId` is inserted into environment variables
2. Fetch AR types which is required to create Travel Agent AR account and set environment variable `ArAccountType`. If the value doesnt differ from Company, no changes required.

## 6. PostAccount
1. Create Travel Agent AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable `TravelAgentAccountNo` is populated
2. Use getProfile API to check all of the above values are responded correctly

## 7. getHotelAvailability

Fetch Hotel Availability which is required for creating Reservation.

After successful fetch, make sure following are inserted into Environment variables

1. Room Type
2. RatePlanCode

For testing purposes please pick Room type `DEL` and RatePlanCode `BARRO` as they are relevant later for updating reservation. Set environment variables accordingly

The variables `Currentdate` and `Currentdateplus1` variable are created from getToken API test scripts.

## 8. postReservation

Create Reservation with Company and Travel Agent attached /linked
Change `id` values within `reservationIdList` and `ExternalReferences`
Other field changes required maybe are `Company Name` and `Travel Agent Name`


1. getMarketCode.  This API is required so that the variable `MarketCode` can be inserted into the environment variable
2. getSourceCode.  This API is required so that the variable `SourceCode` can be inserted into the environment variable
3. getBookingMedium.  This API is required so that the variable `BookingMedium` can be inserted into the environment variable. This is commonly called Origin Code in OPERA Cloud UI.
4. getGuaranteeCode.  This API is required so that the variable `GuaranteeCode` can be inserted into the environment variable
5. getPaymentMethod.  This API is required so that the variable `PaymentMethod` can be inserted into the environment variable
6. getReservation.  Once postReservation is executed, please check whether all required details are entered correctly with this API. Ensure the value from reservationIdList Confirmation is copied into environment variable `Confirmationid`

## 9. postReservation (multiLeg Reservation)
Create Reservation which is multi leg reservation
Change the value within `id` of `ExternalReferences` or `idContext`
After successful post, make sure the environment Variable `ReservationId2`
1. Verify the data with `getReservation` whereby Leg 2 should be visible within External References type=OPERA

## 10. putReservation

Pre Arrival where a preference will be added.

1. getPreference.  To insert the preference, you will need to fetch all preferences which can be attached on the reservation. For testing purposes, will be using preference group `ROOM FEATURES`. Make sure you use for testing purpose  `FBAL` as environment variable `PreferenceCode`
2. getReservation.  Once putReservation is executed, check whether your preference has been attached on the reservation

## 11. postRouting

Posting a routing instruction to existing reservation where `Food` charges goes to the Company which is linked to the reservation. Make sure the routing code `FOOD` is directed to window 2.

1. Fetch the routing Codes with `getRoutingCodes` and set Environment Variable of `CompanyRoutingCode`

## 12. putReservation
1. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 2 will be paid by Direct Billing as Payment Method
2. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload. 

## 13. postRouting

Posting a routing instruction to existing reservation where `Room` charges goes to the Travel Agent which is linked to the reservation. Make sure the routing is done to window 3. Make sure the routing code `BB` is directed to window 3.

1. Fetch the routing Codes with `getRoutingCodes` API and set Environment Variable of `TravelAgentRoutingCode`

## 14. putReservation
1. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 3 will be paid by Direct Billing as Payment Method
2. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload.

## 15. openPaymentTokenExchange
Converts Primary Account Number (PAN) into Token issued by Payment Service Providers
This is required to updated Window 1 payment method which belongs to guest
Take any test Credit Card numbers and insert into the payload within the tag `pan`

## 16. postPaymentMethods
Update existing Payment Method using this API. Make sure you update the tags 
1. cardNumber with `token` value  which you received in `openPaymentTokenExchange`
2. cardNumberMasked with `pan`  which you received in `openPaymentTokenExchange`
3. Expiration Date
And any other value which you changed


## 17. getHotelRooms

Find vacant and inspected rooms so that you can assign to the reservation.
Make sure you insert the `RoomNumber` into environment variable.

## 18. postRoomAssignment

Assign room which you got from earlier API call.

## 19. preCheckin

Pre Register the guest. Ensure the `arrivalTime` is updated in the payload

## 20. checkin

Checkin the reservation with the same roomId which you assigned.

## 21. postRoomKey

Creating a Key. KeyType by default should be `New`

*Please note this API is only for illustration purpose as the API is not available yet*

## 22. postServiceRequest

Create a Service request to provide towel by Housekeeping department. Ensure you change the dates within the payload

1. getServiceRequestCodes.  Fetch the Service Request Codes and Department Code

## 23. postWakupcall

Create a Wakeup call on the reservation. Ensure you change the dates within the payload

1. getWakeUpcall.  Use this API to check whether the postWakeUpcall was inserted successfully into the reservation

## 24. postCashiers
This API is required for `postBillingCharges" whereby it is mandatory that API requires a cashier id. 
1.  getCashier.  Use this API to check whether postCashier API has successfully inserted cashier id. If inserted, please insert the environment variable `CashierId`


## 25. postBillingCharges 
1. getTransactionCode
Use this API to find the required transaction Code. For testing purpose we require transaction Sub Group value `FOD`where by you will need to find transaction Code `2800`. Make sure this inserted into environment Variable `TransactionCode`
2. Post charges (2800) to the window 2

## 26. postBillingCharges 
1. getTransactionCode
Use this API to find the required transaction Code. For testing purpose we require transaction Sub Group value `COM`where by you will need to find transaction Code `5000`. Make sure this inserted into environment Variable `TransactionCode`
2. Post charges (5000) to the window 1

## 27. postAdvanceRoomCharges
As we are testing and no End of Day Routine will be run, use this API to post Room Charges in advance

## 28. getFolio
Use this API to fetch Folios from each window.

## 29. postBillingPayment
Use this API to post payment against the folio on Window 1. There should be no balance left

## 30. postBillingPayment
Use this API to post payment against the folio on Window 2. There should be no balance left

## 31. postBillingPayment
Use this API to post payment against the folio on Window 3. There should be no balance left

## 32. putEarlyDeparture
As we are testing and no End of Day Routine will be run, use this API to change the Reservation to be able checkout Early.

## 33. postFinalCharges
Ensure that you are applying final charges

## 34. postFolios
Use this folio to settle the folio prior checkout. *This API needs to be executed to all 3 windows as charges were on all these 3 windows*. Make sure that you change folioWindow value for each API calls. 

## 35. postCheckout

Use this API to post checkout.

## 36. postEmailFolioReport

Send copy of the invoice to email. Change the value within `emailAddress`

1. getFolioTypes.  Use this API to fetch the folio type configured for executing postEmailFolioReport API
