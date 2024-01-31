# Contactless Journey

This collection will provide you a head start on major APIs which are required for Contactless application like Kiosk.

1. [Get Token](#1-gettoken)
2. [Profile - Create Guest Profile](#2-postguestprofile)
3. [Profile - Create Company Profile](#3-postcompany) (Optional)
4. [Profile - Create Accounts Receivable Number on Company Profile](#4-postaccount) (Optional)
5. [Profile - Create Travel Agent Profile](#5-postprofile) (Optional)
6. [Profile - Create Accounts Receivable Number on Travel Agent](#6-postaccount) (Optional)
7. [Booking - Fetch Hotel Availability](#7-gethotelavailability)
8. [Booking - Create Reservation](#8-postreservation)
9. [Booking - Create Multi Leg Reservation](#9-postreservation) (Optional)
10. [Pre Arrival - Modify Reservation Add Preference](#10-putreservation) (Optional)
11. [Pre Arrival - Create Routing Instruction to Company on Window 2](#11-postrouting) (Optional)
12. [Pre Arrival - Modify Reservation to update Payment Method on Window 2](#12-putreservation) (Optional)
13. [Pre Arrival - Create Routing Instruction to Travel Agent on Window 3](#13-postrouting) (Optional)
14. [Pre Arrival - Modify Reservation to update Payment Method on Window 3](#14-putreservation) (Optional)
15. [Pre Arrival - Convert Pan into token](#15-openpaymenttokenexchange)
16. [Pre Arrival - Modify Reservation to Insert Credit Card Token as Payment Method on Window 1](#16-postpaymentmethod)
17. [Pre Arrival - Pre Authorise Credit Card](#17-postauthorisation)
18. [Pre Arrival- Fetch Available Hotel Rooms](#18-gethotelrooms)
19. [Pre Arrival - Assign Inspected  Vacant Rooms to Reservation](#19-postroomassignment)
20. [Pre Arrival- Modify Reservation to insert Pre-Registeration](#20-precheckin)
21. [Checkin the reservation](#21-checkin)
22. [Checkin- Create Room Key](#22-postroomkey) (For illustration Only)
23. [Stay- Create a Service Request](#23-postservicerequest) (Optional)
24. [Stay - Set Wakeup call](#24-postwakupcall) (Optional)
25. [Stay - Create Cashier](#25-postcashiers)
26. [Stay - Post Billing Charges on windows 1 and 2](#26-postbillingcharges)
27. [Stay - Create Advance Room Charges](#27-postadvanceroomcharges)
28. [Checkout- Fetch Folio](#28-getfolio)
29. [Checkout - Post Payment on each Window 1](#29-postbillingpayment)
30. [Checkout - Post Payment on each Window 2-3](#30-postbillingpayment)
31. [Checkout - Modify Reservation status to Early Departure](#31-putearlydeparture)
32. [Checkout- Close Folios](#32-postfolios)
33. [Checkout - Posting Checkout](#33-postcheckout)
34. [Checkout - Email Invoice](#34-postemailfolioreport) (Optional)

## 1. gettoken
### Get Token
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

## 2-postguestprofile
### Create Guest Profile


Create a guest Profile with minimum data. This is the main Guest Profile who is staying in the hotel

Note that within the postman collection provided, from the POST response `ProfileId` will be automatically inserted into the Postman environment variables.

## 3-postcompany
### Create Company Profile

Create Company Profile where by adding an AR address in the payload. This is required for successful checkout of the folio to Accounts Receivable. This is to show that Reservation can be linked to other Profile types as well. Please note that you can only attach maximum of one Company Profile. 

1. Change `companyName`and `address`as required`
2. Once Company Profile is created, ensure `getProfile` API is executed so that `AR address id` is inserted into environment variables `CompanyArAddressId`

## 4-postaccount
### Create Accounts Receivable Number (AR Account Number) on Company Profile
This API is to create an Account Receivable Number (AR Number) to the Company Profile created previously. This is required later if you want to check out the Opera Folio Window to City Ledger (Direct Billing).

1. Account Receivable account types (AR Types) enable you to categorize AR accounts. The account type selected in each AR Account is used for filtering in both the application and also on reports, such as when generating an AR aging report subtotaled by account type. Account types also determine the stationery templates to use when generating statements and reminder letters for each AR account. Fetch AR types which is required to create Company AR account and set environment variable `ArAccountType`
2. Create Company AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable is populated `CompanyAccountNo`
3. Use getProfile API to check all of the above values are responded correctly

## 5-postprofile
### Create Travel Agent Profile

Create Travel Agent Profile with AR Address. This is to show that Reservation can be linked to other Profile types as well. Please note that you can only attach maximum of one Travel Agent Profile. The `TravelAgentProfileId` is auto populated in Environment variables with Test Scripts inserted into collection

1. Once Travel Agent Profile is created, ensure `getProfile` API is executed so that `TaArAddressId` is inserted into environment variables
2. Account Receivable account types (AR Types) enable you to categorize AR accounts. The account type selected in each AR Account is used for filtering in both the application and also on reports, such as when generating an AR aging report subtotaled by account type. Account types also determine the stationery templates to use when generating statements and reminder letters for each AR account. 
Fetch AR types which is required to create Travel Agent AR account and set environment variable `ArAccountType`. If the value doesnt differ from Company, no changes required.

## 6-postaccount
### Create Accounts Receivable Number (AR Account Number) on Travel Agent Profile

1. Create Travel Agent AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable `TravelAgentAccountNo` is populated
2. Use getProfile API to check all of the above values are responded correctly

## 7-gethotelavailability
### Fetch Hotel Availability

Fetch Hotel Availability which is required for creating Reservation. This provides available ratePlanCode, room type and room rate amount.

After successful fetch, make sure following are inserted into Environment variables

1. Room Type
2. RatePlanCode

For testing purposes please pick Room type `DEL` and RatePlanCode `BARRO` as they are relevant later for updating reservation. Set environment variables `RoomType` and `RatePlanCode` respectively. 

The variables `Currentdate` and `Currentdateplus1` variable are created from getToken API test scripts.

## 8-postreservation
### Create Reservation

Create Reservation with Company and Travel Agent attached. 

Change `id` values within `reservationIdList` and `ExternalReferences`
Other field changes required maybe are `Company Name` and `Travel Agent Name` as per your changes from earlier steps. 

1. Fetch MarketCode.  This API is required so that the variable `MarketCode` can be inserted into the environment variable
2. Fetch SourceCode.  This API is required so that the variable `SourceCode` can be inserted into the environment variable
3. Fetch BookingMedium.  This API is required so that the variable `BookingMedium` can be inserted into the environment variable. This is commonly called Origin Code in OPERA Cloud UI.
4. Fetch Guarantee Code (Reservation Type).  This API is required so that the variable `GuaranteeCode` can be inserted into the environment variable
5. Fetch Payment Method.  This API is required so that the variable `PaymentMethod` can be inserted into the environment variable
6. Fetch Reservation.  Once postReservation is executed, please check whether all required details are entered correctly with this API. Ensure the value from reservationIdList Confirmation is copied into environment variable `Confirmationid`

## 9-postreservation
###  Create Multi Leg Reservation

Create Reservation which is multi leg reservation
Change the value within `id` of `ExternalReferences` or `idContext`
After successful post, make sure the environment Variable `ReservationId2`

1. Verify the data with `getReservation` whereby Leg 2 should be visible within External References type=OPERA

## 10-putreservation
### Modify Reservation Add Preference

Pre Arrival where a preference will be added.

1. Fetch Preference.  To insert the preference, you will need to fetch all preferences which can be attached on the reservation. For testing purposes, will be using preference group `ROOM FEATURES`. Make sure you use for testing purpose  `FBAL` as environment variable `PreferenceCode`
2. Fetch Reservation.  Once putReservation is executed, check whether your preference has been attached on the reservation

## 11-postrouting
### Create Routing Instruction to Company on Window 2

Posting a routing instruction to existing reservation where `Food` charges goes to the Company which is linked to the reservation. Make sure the routing code `FOOD` is directed to window 2.

1. Fetch the routing Codes with `getRoutingCodes` and set Environment Variable of `CompanyRoutingCode`

## 12-putreservation
### Modify Reservation to update Payment Method on Window 2

1. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 2 will be paid by Direct Billing as Payment Method
2. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload.

## 13-postrouting
### Create Routing Instruction to Travel Agent on Window 3

Posting a routing instruction to existing reservation where `Room` charges goes to the Travel Agent which is linked to the reservation. Make sure the routing is done to window 3. Make sure the routing code `BB` is directed to window 3.

1. Fetch the routing Codes with `getRoutingCodes` API and set Environment Variable of `TravelAgentRoutingCode`

## 14-putreservation
### Modify Reservation to update Payment Method on Window 3

1. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 3 will be paid by Direct Billing as Payment Method
2. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload.

## 15-openpaymenttokenexchange
### Convert PAN into Token

Converts Primary Account Number (PAN) into Token issued by Payment Service Providers
This is required to updated Window 1 payment method which belongs to guest
Take any test Credit Card numbers and insert into the payload within the tag `pan`

## 16-postpaymentmethod
## Modify Reservation to Insert Credit Card Token as Payment Method on Window 1

Update existing Payment Method using this API. Make sure you update the tags

1. cardNumber with `token` value  which you received in `openPaymentTokenExchange`
2. cardNumberMasked with `pan`  which you received in `openPaymentTokenExchange`
3. Expiration Date
And any other value which you changed

## 17-postauthorisation
### Pre Authorise Credit Card
This API is useful for many Kiosk Partners who wants to save time to Pre Authorise card prior checkin API.

## 18-gethotelrooms
### 18. Fetch Available Hotel Rooms

Find vacant and inspected rooms so that you can assign to the reservation.
Make sure you insert the `RoomNumber` into environment variable.

## 19-postroomassignment
### 19. Assign Inspected  Vacant Rooms to Reservation

Assign room which you got from earlier API call.

## 20-precheckin
### 20. Modify Reservation to insert Pre-Registeration

Pre Register the guest. Ensure the `arrivalTime` is updated in the payload

# 21-checkin
### Checkin the reservation

Checkin the reservation with the same roomId which you assigned.

## 22-postroomkey
### Create Room Key

Creating a Key. KeyType by default should be `New`

*Please note this API is only for illustration purpose as the API is not available yet*

## 23-postservicerequest
### Create a Service Request

Create a Service request to provide towel by Housekeeping department. Ensure you change the dates within the payload

1. getServceRequestCodes.  Fetch the Service Request Codes and Department Code

## 24-postwakupcall
### Set Wake up Call

Create a Wakeup call on the reservation. Ensure you change the dates within the payload

1. getWakeUpcall.  Use this API to check whether the postWakeUpcall was inserted successfully into the reservation

## 25-postCashiers
### Create Cashier

This API is required for `postBillingCharges" whereby it is mandatory that API requires a cashier id.

1. getCashier.  Use this API to check whether postCashier API has successfully inserted cashier id. If inserted, please insert the environment variable `CashierId`

## 26-postbillingcharges
### Post Billing Charges on windows 1 and 2

1. getTransactionCode
Use this API to find the required transaction Code. For testing purpose we require transaction Sub Group value `FOD`where by you will need to find transaction Code `2800`. Make sure this inserted into environment Variable `TransactionCode`
2. Post charges (2800) to the window 2

3. getTransactionCode
Use this API to find the required transaction Code. For testing purpose we require transaction Sub Group value `COM`where by you will need to find transaction Code `5000`. Make sure this inserted into environment Variable `TransactionCode`
4. Post charges (5000) to the window 1


## 27-postadvanceroomcharges
### Create Advance Room Charges

As we are testing and no End of Day Routine will be run, use this API to post Room Charges in advance

## 28-getfolio
### Fetch Folio

Use this API to fetch Folios from each window. Remember there are 3 Windows which should have Charges (Balances)

## 29-postbillingpayment
### Post Payment on each Window 1

Use this API to post payment against the folio on each Window. There should be no balance left
Window 1 should be paid against Credit Card


## 30-postbillingpayment
### Post Payment on each Window 2-3

Use this API to post payment against the folio on each Window. There should be no balance left
Window 2 should be paid against payment Method `INV` which is invoiced to Company
Window 3 should be paid against payment Method `INV` which is invoiced to Travel Agent

## 31-putearlydeparture
### Modify Reservation status to Early Departure

As we are testing and no End of Day Routine will be run, use this API to change the Reservation to be able checkout Early.

## 32-postfolios
### Close Folio Windows 1-3

Use this folio to settle the folio prior checkout. *This API needs to be executed to all 3 windows as charges were on all these 3 windows*. Make sure that you change folioWindow value for each API calls.

## 33-postcheckout
### Posting CheckOut

Use this API to post checkout.

## 34-postemailfolioreport
### Email Invoice

Send copy of the invoice to email. Change the value within `emailAddress`

1. getFolioTypes.  Use this API to fetch the folio type configured for executing postEmailFolioReport API
