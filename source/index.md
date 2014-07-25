---
title: SmartWallet API

language_tabs:
  - Json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the SmartWallet! You can use our API to access SmartWallet endpoints, which can create, update, get information about account and execute all transaction operations (Create, Refund, Update)


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
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint is used to create an new account linked to your SmartWallet, that is represented by your SmartWalletKey.

### HTTP Request

`GET https://smartwallet.mundipaggone.com/Account/`
