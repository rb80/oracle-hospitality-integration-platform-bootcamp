# Implementation Use Cases

- [1 Get Token](#1-get-token)
- [2 Create Guest Profile](#2-create-guest-profile)
- [3 Create Company Profile](#3-create-company-profile)
- [4 Create AR Account for Company Profile](#4-create-ar-account-for-company-profile)
- [5-Create Travel Agent Profile](#5-create-travel-agent-profile)
- [6 Create AR Account for Travel Agent Profile](#6-create-ar-account-for-travel-agent-profile)
- [7-Fetch Hotel Availability](#7-fetch-hotel-availability)
- [8-Create Reservation](#8-create-reservation)
- [9-Create Multi Leg Reservation](#9-create-multi-leg-reservation)
- [10-Modify Reservation Add Preference](#10-modify-reservation-add-preference)
- [11-Create Routing Instruction to Company on Window 2](#11-create-routing-instruction-to-company-on-window-2)
- [12-Modify Reservation to update Payment Method on Window 2](#12-modify-reservation-to-update-payment-method-on-window-2)
- [13-Create Routing Instruction to Travel Agent on Window 3](#13-create-routing-instruction-to-travel-agent-on-window-3)
- [14-Modify Reservation to update Payment Method on Window 3](#14-modify-reservation-to-update-payment-method-on-window-3)
- [15-Convert PAN into Token](#15-convert-pan-into-token)
- [16-Modify Reservation to Insert Credit Card Token as Payment Method on Window 1](#16-modify-reservation-to-insert-credit-card-token-as-payment-method-on-window-1)
- [17-Pre Authorise Credit Card](#17-pre-authorise-credit-card)
- [17-Pre Authorize Credit Card](#17-pre-authorize-credit-card)
- [18-Fetch Available Hotel Rooms](#18-fetch-available-hotel-rooms)
- [19-Assign Inspected Vacant Rooms to Reservation](#19-assign-inspected-vacant-rooms-to-reservation)
- [20-Modify Reservation to Pre-Register the Arrival](#20-modify-reservation-to-pre-register-the-arrival)
- [21-Checkin the reservation](#21-checkin-the-reservation)
- [22-Create Room Key](#22-create-room-key)
- [23-Create a Service Request](#23-create-a-service-request)
- [24-Set Wake up Call](#24-set-wake-up-call)
- [24-Set Wake up Call](#24-set-wake-up-call-1)
- [25-Create Cashier](#25-create-cashier)
- [26-Post Billing Charges on windows 1 and 2](#26-post-billing-charges-on-windows-1-and-2)
- [27-Create Advance Room Charges](#27-create-advance-room-charges)
- [28-Fetch Folio postings from each window](#28-fetch-folio-postings-from-each-window)
- [29-Post Payment on each Window 1](#29-post-payment-on-each-window-1)
- [28-Fetch Folio](#28-fetch-folio)
- [29-Post Payment on each Window 1](#29-post-payment-on-each-window-1-1)
- [30-Post Payment on each Window 2-3](#30-post-payment-on-each-window-2-3)
- [31-Modify Reservation status to Early Departure](#31-modify-reservation-status-to-early-departure)
- [32-Close Folio Windows 1-3](#32-close-folio-windows-1-3)
- [33-Posting CheckOut](#33-posting-checkout)
- [34-Email Invoice](#34-email-invoice)

<!-- TOC end -->

1. [Get Token]
2. [Profile - Create Guest Profile]
3. [Profile - Create Company Profile] (Optional)
4. [Profile - Create AR Account for Company Profile] (Optional) 
5. [Profile - Create Travel Agent Profile] (Optional)
6. [Profile - Create Accounts Receivable Number on Travel Agent] (Optional)
7. [Booking - Fetch Hotel Availability]
8. [Booking - Create Reservation]
9. [Booking - Create Multi Leg Reservation] (Optional)
10. [Pre Arrival - Modify Reservation Add Preference] (Optional)
11. [Pre Arrival - Create Routing Instruction to Company on Window 2] (Optional)
12. [Pre Arrival - Modify Reservation to update Payment Method on Window 2] (Optional)
13. [Pre Arrival - Create Routing Instruction to Travel Agent on Window 3] (Optional)
14. [Pre Arrival - Modify Reservation to update Payment Method on Window 3] (Optional)
15. [Pre Arrival - Convert Pan into token]
16. [Pre Arrival - Modify Reservation to Insert Credit Card Token as Payment Method on Window 1]
17. [Pre Arrival - Pre Authorize Credit Card]
18. [Pre Arrival- Fetch Available Hotel Rooms]
19. [Pre Arrival - Assign Inspected  Vacant Rooms to Reservation]
20. [Pre Arrival- Modify Reservation to Pre-Register the Arrival]
21. [Checkin the reservation]
22. [Checkin- Create Room Key] (For illustration Only)
23. [Stay- Create a Service Request] (Optional)
24. [Stay - Set Wakeup call] (Optional)
25. [Stay - Create Cashier]
26. [Stay - Post Billing Charges on windows 1 and 2]
27. [Stay - Create Advance Room Charges]
28. [Checkout- Fetch Folio postings from each window]
29. [Checkout - Post Payment on each Window 1]
30. [Checkout - Post Payment on each Window 2-3]
31. [Checkout - Modify Reservation status to Early Departure]
32. [Checkout- Close Folios]
33. [Checkout - Posting Checkout]
34. [Checkout - Email Invoice] (Optional)

<!-- TOC --><a name="1-get-token"></a>
## 1 Get Token

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

<!-- TOC --><a name="2-create-guest-profile"></a>
## 2 Create Guest Profile

Create a guest Profile with minimum data. This is the main Guest Profile who is staying in the hotel

Note that within the postman collection provided, from the POST response `ProfileId` will be automatically inserted into the Postman environment variables.

<!-- TOC --><a name="3-create-company-profile"></a>
## 3 Create Company Profile

Create Company Profile where by adding an AR address in the payload. This is required for successful checkout of the folio to Accounts Receivable. This is to show that Reservation can be linked to other Profile types as well. Please note that you can only attach maximum of one Company Profile.

1. Change `companyName`and `address`as required`. Do not change the address type 'AR ADDRESS'
2. Once Company Profile is created, ensure `getProfile` API is executed so that `AR address id` is inserted into environment variables `CompanyArAddressId`

<!-- TOC --><a name="4-create-ar-account-for-company-profile"></a>
## 4 Create AR Account for Company Profile

This API is to create an Account Receivable Number (AR Number) to the Company Profile created previously. This is required later if you want to check out the Opera Folio Window to City Ledger (Direct Billing).

1. Account Receivable account types (AR Types) enable you to categorize AR accounts. The account type selected in each AR Account is used for filtering in both the application and also on reports, such as when generating an AR aging report subtotaled by account type. Account types also determine the stationery templates to use when generating statements and reminder letters for each AR account. Fetch AR types which is required to create Company AR account and set environment variable `ArAccountType`
2. Create Company AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable is populated `CompanyAccountNo`
3. Use getProfile API to check all of the above values are responded correctly


<!-- TOC --><a name="5-create-travel-agent-profile"></a>
## 5-Create Travel Agent Profile

Create Travel Agent Profile with AR Address. This is to show that Reservation can be linked to other Profile types as well. Please note that you can only attach maximum of one Travel Agent Profile. The `TravelAgentProfileId` is auto populated in Environment variables with Test Scripts inserted into collection

1. Once Travel Agent Profile is created, ensure `getProfile` API is executed so that `TaArAddressId` is inserted into environment variables
2. Account Receivable account types (AR Types) enable you to categorize AR accounts. The account type selected in each AR Account is used for filtering in both the application and also on reports, such as when generating an AR aging report subtotaled by account type. Account types also determine the stationery templates to use when generating statements and reminder letters for each AR account.

Fetch AR types which is required to create Travel Agent AR account and set environment variable `ArAccountType`. If the value doesn't differ from Company, no changes required.


<!-- TOC --><a name="6-create-ar-account-for-travel-agent-profile"></a>
## 6 Create AR Account for Travel Agent Profile

1. Create Travel Agent AR Account. Make sure the AR number (accountNo) is inserted and also the environment variable `TravelAgentAccountNo` is populated
2. Use getProfile API to check all of the above values are responded correctly

<!-- TOC --><a name="7-fetch-hotel-availability"></a>
## 7-Fetch Hotel Availability

Fetch Hotel Availability which is required for creating Reservation. This provides available ratePlanCode, room type and room rate amount.

After successful fetch, make sure following are inserted into Environment variables

1. Room Type
2. RatePlanCode

For testing purposes please pick Room type `DEL` and RatePlanCode `BARRO` as they are relevant later for updating reservation. Set environment variables `RoomType` and `RatePlanCode` respectively.

The variables `Currentdate` and `Currentdateplus1` variable are created from getToken API test scripts.

<!-- TOC --><a name="8-create-reservation"></a>
## 8-Create Reservation

Create Reservation with Company and Travel Agent attached.

Change `id` values within `reservationIdList` and `ExternalReferences`
Other field changes required maybe are `Company Name` and `Travel Agent Name` as per your changes from earlier steps.

1. Fetch MarketCode.  This API is required so that the variable `MarketCode` can be inserted into the environment variable
2. Fetch SourceCode.  This API is required so that the variable `SourceCode` can be inserted into the environment variable
3. Fetch BookingMedium.  This API is required so that the variable `BookingMedium` can be inserted into the environment variable. This is commonly called Origin Code in OPERA Cloud UI.
4. Fetch Guarantee Code (Reservation Type).  This API is required so that the variable `GuaranteeCode` can be inserted into the environment variable
5. Fetch Payment Method.  This API is required so that the variable `PaymentMethod` can be inserted into the environment variable
6. Fetch Reservation.  Once postReservation is executed, please check whether all required details are entered correctly with this API. Ensure the value from reservationIdList Confirmation is copied into environment variable `Confirmationid` and also `id` from `externalReferences` into `ExternalReferences`

<!-- TOC --><a name="9-create-multi-leg-reservation"></a>
## 9-Create Multi Leg Reservation

Create Reservation which is multi leg reservation. This API is to show you how to create Multileg and will not be further used in this collection and therefore do not require any environment variables. 


Create Reservation which is multi leg reservation
Change the value within `id` of `ExternalReferences` or `idContext`
After successful post, make sure the environment Variable `ReservationId2`

1. Verify the data with `getReservations` whereby making use of `confirmationNumberList` query parameter which shows you can fetch all leg Reservations with one API

<!-- TOC --><a name="10-modify-reservation-add-preference"></a>
## 10-Modify Reservation Add Preference

Pre Arrival where a preference will be added.

1. Fetch Preference.  To insert the preference, you will need to fetch all preferences which can be attached on the reservation. For testing purposes, will be using preference group `ROOM FEATURES`. Make sure you use for testing purpose  `FBAL` as environment variable `PreferenceCode`
2. Fetch Reservation.  Once putReservation is executed, check whether your preference has been attached on the reservation

<!-- TOC --><a name="11-create-routing-instruction-to-company-on-window-2"></a>
## 11-Create Routing Instruction to Company on Window 2

Posting a routing instruction to existing reservation where `Food` charges goes to the Company which is linked to the reservation. Make sure the routing code `FOOD` is directed to window 2.

1. Fetch the routing Codes with `getRoutingCodes` and set Environment Variable of `CompanyRoutingCode`

<!-- TOC --><a name="12-modify-reservation-to-update-payment-method-on-window-2"></a>
## 12-Modify Reservation to update Payment Method on Window 2

1. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 2 will be paid by Direct Billing as Payment Method
2. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload.

<!-- TOC --><a name="13-create-routing-instruction-to-travel-agent-on-window-3"></a>
## 13-Create Routing Instruction to Travel Agent on Window 3

Posting a routing instruction to existing reservation where `Room` charges goes to the Travel Agent which is linked to the reservation. Make sure the routing is done to window 3. Make sure the routing code `BB` is directed to window 3.

1. Fetch the routing Codes with `getRoutingCodes` API and set Environment Variable of `TravelAgentRoutingCode`

<!-- TOC --><a name="14-modify-reservation-to-update-payment-method-on-window-3"></a>
## 14-Modify Reservation to update Payment Method on Window 3

1. putReservation.  Once Routing is done, modification is required to existing Reservation to inform that window 3 will be paid by Direct Billing as Payment Method
2. To find the Payment Method use `getPaymentMethod` API. For testing purpose use `INV`. No Environment is defined here. Use the value within payload.

<!-- TOC --><a name="15-convert-pan-into-token"></a>
## 15-Convert PAN into Token

Converts Primary Account Number (PAN) into Token issued by Payment Service Providers
This is required to updated Window 1 payment method which belongs to guest
Take any test Credit Card numbers and insert into the payload within the tag `pan`
Kindly note that this environment is linked to a PSP simulator and therefore every PAN number conversion will respond with different Token numbers for same PAN number. 

<!-- TOC --><a name="16-modify-reservation-to-insert-credit-card-token-as-payment-method-on-window-1"></a>
## 16-Modify Reservation to Insert Credit Card Token as Payment Method on Window 1

Update existing Payment Method using this API. Make sure you update the tags

1. cardNumber with `token` value  which you received in `openPaymentTokenExchange`
2. cardNumberMasked with `pan`  which you received in `openPaymentTokenExchange`
3. Expiration Date
4. citId is an id which is usually sent by PSP into OPERA through OPI. This is not visible anywhere in OPERA Cloud UI. It is saved in OPERA DB only. 

And any other value which you changed

<!-- TOC --><a name="17-pre-authorise-credit-card"></a>
## 17-Pre Authorise Credit Card
This API is useful for many Kiosk Partners who wants to save time to Pre Authorise card prior checkin API. Make sure the terminalId has the value for your testing is `PD1` 

getHotelInterface API will show whether OPI is installed and configured. Look for `activeFlag=true`


<!-- TOC --><a name="18-fetch-available-hotel-rooms"></a>
## 18-Fetch Available Hotel Rooms

Find vacant and inspected rooms so that you can assign to the reservation.
Make sure you insert the `RoomNumber` into environment variable.

<!-- TOC --><a name="19-assign-inspected-vacant-rooms-to-reservation"></a>
## 19-Assign Inspected Vacant Rooms to Reservation

Assign room which you got from earlier API call.

<!-- TOC --><a name="20-modify-reservation-to-pre-register-the-arrival"></a>
## 20-Modify Reservation to Pre-Register the Arrival

Pre Register the guest. Ensure the `arrivalTime` is updated in the payload

<!-- TOC --><a name="21-checkin-the-reservation"></a>
## 21-Checkin the reservation

Checkin the reservation with the same roomId which you assigned.

<!-- TOC --><a name="22-create-room-key"></a>
## 22-Create Room Key

Creating a Key. KeyType by default should be `New`

> Please note this API is only for illustration purpose as the API is not available yet.

<!-- TOC --><a name="23-create-a-service-request"></a>
## 23-Create a Service Request

1. Fetch Service request Codes

2. Create a Service request to provide towel by Housekeeping department. Ensure you change the dates within the payload

3. Fetch the Service Request Codes applied to the reservation


<!-- TOC --><a name="24-set-wake-up-call-1"></a>
## 24-Set Wake up Call

Create a Wakeup call on the reservation. Ensure you change the dates within the payload


2. Create a Wakeup call on the reservation.

3. getWakeUpcall.  Use this API to check whether the postWakeUpcall was inserted successfully into the reservation

<!-- TOC --><a name="25-create-cashier"></a>
## 25-Create Cashier

This API is required for `postBillingCharges" whereby it is mandatory that API requires a cashier id.

1. getCashier.  Use this API to check whether postCashier API has successfully inserted cashier id. If inserted, please insert the environment variable `CashierId`

<!-- TOC --><a name="26-post-billing-charges-on-windows-1-and-2"></a>
## 26-Post Billing Charges on windows 1 and 2

1. getTransactionCode
Use this API to find the required transaction Code. For testing purpose we require transaction Sub Group value `COM`where by you will need to find transaction Code `5000`. Make sure this inserted into environment Variable `TransactionCode`

2. Post charges (5000) to the window 1. The amount can be of your choice

3. getTransactionCode
Use this API to find the required transaction Code. For testing purpose we require transaction Sub Group value `FOD`where by you will need to find transaction Code `2800`. Make sure this inserted into environment Variable `TransactionCode`.

4. Post charges (2800) to the window 1. The amount can be of your choice

<!-- TOC --><a name="27-create-advance-room-charges"></a>
## 27-Create Advance Room Charges

As we are testing and no End of Day Routine will be run, use this API to post Room Charges in advance which will post room charges to window 3 as per Routing instructions you inserted earlier. 

<!-- TOC --><a name="28-fetch-folio-postings-from-each-window"></a>
## 28-Fetch Folio postings from each window
Use this API to fetch Folios from each window. Remember there are 3 Windows which should have Charges (Balances)

<!-- TOC --><a name="29-post-payment-on-each-window-1"></a>
## 29-Post Payment on each Window 1
1. Fetch the cardId required for posting payment against Credit Card which was insrted earlier. 

2. Use this API to post payment against the folio on each Window. There should be no balance left. Window 1 should be paid against Credit Card


<!-- TOC --><a name="30-post-payment-on-each-window-2-3"></a>
## 30-Post Payment on each Window 2-3

Use this API to post payment against the folio on each Window. There should be no balance left
1. Window 2 should be paid against payment Method `INV` which is invoiced to Company
2. Window 3 should be paid against payment Method `INV` which is invoiced to Travel Agent

<!-- TOC --><a name="31-modify-reservation-status-to-early-departure"></a>
## 31-Modify Reservation status to Early Departure

As we are testing and no End of Day Routine will be run, use this API to change the Reservation to be able checkout Early.

<!-- TOC --><a name="32-close-folio-windows-1-3"></a>
## 32-Close Folio Windows 1-3

Use this folio to settle the folio prior checkout. *This API needs to be executed to all 3 windows as charges were on all these 3 windows*. Make sure that you change folioWindow value for each API calls.

<!-- TOC --><a name="33-posting-checkout"></a>
## 33-Posting CheckOut

Use this API to post checkout.

<!-- TOC --><a name="34-email-invoice"></a>
## 34-Email Invoice

Send copy of the invoice to email. Change the value within `emailAddress`

1. getFolioTypes.  Use this API to fetch the folio type configured for executing postEmailFolioReport API

2. Send copy of the invoice to email. Change the value within `emailAddress`

3. Fetch copy of the invoice in Base64 format. Use any public website to convert Base64 into pdf to view it. 