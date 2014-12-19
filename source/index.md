---
title: SmartWallet API

language_tabs:
  - Json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - mcc

search: true
---

# Introduction

Welcome to the SmartWallet! 
You can use our API to access SmartWallet endpoints, which can create, update, get information about account and execute all transaction operations (Create, Refund, Update)


# Authentication

SmartWallet uses API keys to allow access to the API. 

SmartWallet expects for the API key to be included in all API requests to the server in a header that looks like the following:

`SmartWalletKey: E485075C-A77D-404A-A966-72AD93E1CB7D`

<aside class="notice">
You must replace `E485075C-A77D-404A-A966-72AD93E1CB7D` with your personal API key.
</aside>

# Account

## Create Account

> Request object used to create an account.

```json
{
    "AccountReference": "sample string 8",
    "Addresses": [
        {
            "AddressNumber": "sample string 2",
            "City": "sample string 4",
            "Complement": "sample string 3",
            "Country": "sample string 8",
            "District": "sample string 5",
            "State": "sample string 6",
            "StreetAddress": "sample string 1",
            "ZipCode": "sample string 7"
        }
    ],
    "DbaName": "sample string 2",
    "DocumentNumber": "sample string 5",
    "DocumentType": "sample string 4",
    "Email": "sample string 6",
    "FinancialDetails": [
        {
            "Bank": {
                "AccountNumber": "sample string 3",
                "AgencyNumber": "sample string 2",
                "BankCode": "sample string 1"
            },
            "Destination": "sample string 1",
            "Email": "sample string 2"
        }
    ],
    "LegalName": "sample string 1",
    "MCC": 9,
    "PhoneNumber": "sample string 7",
    "RequestKey": "4636d485-583e-4f7d-a776-bdc4d9654950",
    "SmartWalletKey": "309d448f-246b-4f4e-8e97-dbfb771844fb",
    "SocialUserName": "sample string 3"
}
```

> The above request returns an answear like this:

```json
{
    "AccountKey": "f489675e-6e9d-49e5-bea3-17f38163e079",
    "AccountReference": "sample string 4",
    "Acquirer": {
        "AcquireCode": "sample string 2",
        "AcquirerAffiliationKey": "sample string 1",
        "AcquirerName": "sample string 3"
    },
    "DbaName": "sample string 3",
    "DocumentNumber": "sample string 5",
    "DocumentType": 0,
    "Errors": [
        {
            "DocumentationUrl": "sample string 4",
            "ErrorCode": 1,
            "ErrorDescription": "sample string 3",
            "ParameterName": "sample string 2"
        }
    ],
    "LegalName": "sample string 2",
    "RequestKey": "0fb908dd-d489-46f0-86b5-482206be3047"
}
```

This endpoint is used to create an new account linked to your SmartWallet, that is represented by your SmartWalletKey.

### HTTP Request

`POST https://smartwallet.mundipaggone.com/Account/`

### Parameters

Parameter | Description
--------- | -----------
AccountReference | Identification for the account defined by your system.
LegalName | The legal name for the company or person.
DbaName | Name that your company or person uses to do business.
DocumentNumber | The number of the document that identifies you company ou person legaly.
DocumentType | Type of the document (CPNJ / CPF).
Email | Email address for your account.
PhoneNumber | Phone number for this account.
Addresses | Collection of addresses.
FinancialDetails | Account financial details.
RequestKey | Key that identifies this request to the API. (If not set, will be defined by the API)
MCC | Mcc table.


<aside class="warning">
At the response object you will find the **AccountKey**. This is the key that will be used to execute all actions related to that account, like updating informations and add credit.

You must keep that information stored, or ortherwise you will not be able to execute any operations related to that account.

**"AccountKey": "ba1c2524-bd81-4f9d-8f97-673781768c59"**
</aside>

## Update Account

`PATCH https://smartwallet.mundipaggone.com/Account/{AccountKey}`

This endpoint is used to update an existing account. The request object is the same used at the creation of an account.
Every data send will replace the existing data for that account.

Ex: If you want to update only the DbaName and the LegalName you can send only these specific fields.

````json
{
    "DbaName": "André Galdino Updated",    
    "LegalName": "André Galdino Updated"
}
```

## Get a Specific Account

> This request returns a JSON like this: 

```json
{
    "AccountKey": "f0b1e74c-0798-47e4-838d-fe02c9cecc71",
    "LegalName": "André Galdino",
    "DbaName": "André Galdino",
    "AccountReference": "André",
    "DocumentNumber": "0321654987",
    "DocumentType": "CPF",
    "RequestKey": "8CE94ECB-7D8B-4D6C-A341-CA2C259A9C5C",
    "Errors": null
}
```

`GET https://smartwallet.mundipaggone.com/Account/{AccountKey}`

This endpoint will return the basic informations for a specific account.

### Exemple:
HTTP's GET request for this endpoint using the AccountKey **F0B1E74C-0798-47E4-838D-FE02C9CECC71**

# Transaction

## Create Transaction

> Request object used to create a transaction 

```json
{
    "CreditCardCollection": [
        {
            "Amount": 100,
            "CreditCardNumber": "123654889",
            "CreditCardType": "",
            "Cvv2": "528",
            "ExpirationDate": "2014-07-01",
            "HolderName": "ANDRE GALDINO"
        }
    ],
    "CreditItemCollection": [
        {
            "AccountKey": "f0b1e74c-0798-47e4-838d-fe02c9cecc71",
            "Amount": 100,
            "CurrencyIsoCode": "BRL",
            "Description": "GenericItem",
            "FeeType": "Percent",
            "FeeValue": 10,
            "HoldTransaction": false,
            "ItemReference": "ItemReference123",
            "SplitTransaction": true,
            "LiabilityShift": "Client"
        }
    ],
    "Order": {
        "Amount": 100,
        "OrderDate": "2014-07-28",
        "OrderReference": "0123",
        "PaymentDate": "2014-07-28",
        "RefundDate": null
    },
    "RequestKey": "00000000-0000-0000-0000-000000000000"
}
```

> The response for te above request looks like this:

```json
{
    "CreditFinancialMovementCollection": [
        {
            "ItemReference": "GenericItem",
            "AccountKey": "991aa261-9fe7-42c7-bbd7-8ef286c2d4df",
            "FinancialMovementKey": "ce5c421b-8690-411b-b984-7741abb5dc2c"
        }
    ],
    "RequestKey": "3797f66d-a465-4e84-ab09-c300b276a227",
    "Errors": null
}
```


### HTTP REQUEST
`POST https://smartwallet.mundipaggone.com/Transaction/`

### Request Parameters
Parameter | Description
--------- | -----------
CreditCardCollection | Collection of credit cards used to pay this transaction.
CreditItemCollection | Collection of itens, each item identifies a new credit input to the referenced account.
Order (Optional) | In use case of a MarketPlace you can sent iformations about the order.
RequestKey | Key that identifies this request to the API. (If not set, will be defined by the API)

### CreditCardCollection Parameters
Parameter | Description
--------- | -----------
Amount | Amount to be charged in this credit card.
CrediCardNumber | Number of the credit card.
CreditCardType | Type of the credit card.
Cvv2 | Credit card's security number.
ExpirationDate | Credit card's expiration date.
HolderName | Credit card's holder name.

### CreditCardCollection Parameters
Parameter | Description
--------- | -----------
AccountKey | Key that identifies tha account that will receive this credit value.
Amount | Amount that will be credited in this account.
CurrencyIsoCode | Code that represents the currency that you're dealing with.
Description | A small description of the item.
FeeType | Define the type of the fee value. **Percent** or **Flat**
FeeValue | Value of the fee charged for this transaction.
HoldTransaction | Boolean value that defines if the transaction will be blocked at the moment of creation or not.
ItemReference | Reference for the item.
SplitTransaction | Boolean value that defines if this transaction will be splited between the account receiving the credit and the account that owns this SmartWallet or not.
LiabilityShift | Explained above *

### Order Parameters
Parameter | Description
--------- | -----------
Amount | Total amount of the order.
OrderDate | Date of the order.
OrderReference | Identification for this order (Ex: Order number).
PaymentDate | Date that this order was paid.
RefundDate | Data that this order was refunded.


### LiabilityShift
Sending the field LiabilityShift you can define the way that the refund will be made.
There are two possible values:

Parameter | Description
--------- | -----------
Client | When the refund is requested by the client and both the MasterAccount and the SubMerchantAccount will receive theirs amount back.
Partner | When the refund is requested by the partner and because of that the client will be afected. In this case the partner will continue to pay the comission over the transaction if the transaction was splited.

Sending is optional, and in case it is not sent the default value will be **CLIENT**.

<aside class="warning">
At the response object you will find the **FinancialMovementKey**. This is the key that will be used to execute all actions related a existing transaction.

You must keep that information stored, or ortherwise you will not be able to execute any operations related to that transaction.

**"FinancialMovementKey": "ba1c2524-bd81-4f9d-8f97-673781768c59"**
</aside>

## Refund Transaction
### HTTP REQUEST

`POST https://smartwallet.mundipaggone.com/Transaction/{financialMovmentKey}`

This endpoint is used to Refund a existing transaction.

## Update Transaction
### Http request
>Update Status exemple

```json
{
    "RequestKey": "00000000-0000-0000-0000-000000000000", 
    "Status": "Used"
}
```
`PATCH https://smartwallet.mundipaggone.com/Transaction/{financialMovmentKey}`

When creating a transaction there is the option to Block it and retain it values to not be accounted in the total values. After that it's possible to update that transaction status to two possible values:

Parameter | Description
----------|------------
Used | When the transaction status is set to Used it sum up to the total value of the account that received that credit.
Expired | When the transaction status is set to Expired the total value is added to the MasterAccount of that SmartWallet.