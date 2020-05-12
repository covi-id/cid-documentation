[> Back to `README.md`]()

# Index
#### [Organisation endpoints](#organisation-endpoints)
* [Get an organisation](#get-an-organisation)
* [Check in of a user](#check-in-of-a-user)
* [Check out of a user](#check-out-of-a-user)
* [Access denied](#access-denied)
#### [Wallet endpoints](#wallet-endpoints)
* [Create wallet](#create-wallet)
### [OTP endpoints](#otp-endpoints)
* [Confirm OTP](#confirm-otp)
* [Resend OTP](#resend-otp)
### [Add Test Results Endpoints](#add-test-results-endpoints)
* [Add test result](#add-test-result)
* [Get Wallet Status](#get-wallet-status)

---
# Organisation endpoints

## Get an organisation
**GET** [http://<our_server_URL>/api/organisations/{id}](http://<our_server_URL>/api/organisations/{id})
**Headers:**
```
x-api-key: “{api key}”,
```

## Check in of a user
**POST** [http://<our_server_URL>/api/organisations/{id}/check_in](http://<our_server_URL>/api/organisations/{id}/check_in)
**Headers:**
```
x-api-key: “{api key}”,
Payload:
{
	“walletId”: “{walletId}”,
	“long”: 1234,
	“lat”: 1234
}
```
**Response:**
```
{
   "data": {
       "balance": 10
   },
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

## Check out of a user
**POST** [http://<our_server_URL>/api/organisations/{id}/check_out](http://<our_server_URL>/api/organisations/{id}/check_out)
**Headers:**
```
x-api-key: “{api key}”,
```
**Payload:**
```
{
	“walletId”: “{walletId}”,
	“long”: 1234,
	“lat”: 1234
}
```
**Response:**
```
{
   "data": {
       "balance": 10
   },
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

## Access denied
**POST** [https://<our_server_URL>/api/organisations/{id}/denied](https://<our_server_URL>/api/organisations/{id}/denied)
**Headers:**
```
x-api-key: “{api key}”,
```
**Payload:** 
```
{
	“walletId”: “{walletId}”,
	“long”: 1234,
	“lat”: 1234
}
```
**Response:**
```
{
   "data": {
       "balance": 10
   },
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

# Wallet endpoints

## Create wallet
**POST** [https://<our_server_URL>/api/wallets](https://<our_server_URL>/api/wallets)
**Headers:**
```
x-api-key: “{api key}”,
```
**Payload:**
```
{
	“mobileNumber”: “27765408650”,
	“mobileNumberReference: “0765408650”
}
```
**Response:**
```
{
 "data": 
{
	“token”: “{token that will be needed for OTP confirmation and resend}”
}
   },
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

# OTP endpoints

## Confirm OTP
**POST** [https://<our_server_URL>/api/auth/otp](https://<our_server_URL>/api/auth/otp)
**Headers:** 
```
x-api-key: “{api key}”,
Authorization: “{token from create wallet or from resend otp}”
```
**Payload:**
```
{
	“otp”: {otp},
	“walletDetaills”:
    {
        “firstName”: “{firstName}”, Max 50, min 2
        “lastName”: “{lastName}”, Max 50, min 2
        “photo”: “{base 64 photo}”
        “IdType”:”{IdentificationDocument}” //Enum 
        “IdValue”:”{Identification value}” Max 13, min 6
    },
    “testResult”:
    {
        “testedAt”: “{"0001-01-01}”,
        “resultStatus”: “{Positive}”, //Enum
        “Laboratory”: “{Lancey}”, //Enum
        “referenceNumber”:” {123456AB}”,
        “hasConsent”: true
    }
}
```
**Response:**
```
{
"data": {
	“walletId”: “{walletId}”,
	“key”: “{user’s secret key}” 
},
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

## Enums
```
IdType
    {
        IdentificationDocument = 0,
        Passport = 1
    }
ResultStatus
    {
        Untested = 0,
        Negative = 1,
        Positive = 2,
    }
Laboratory
    {
        NHLS = 0,
        Lancet = 1,
        Pathcare = 2,
    }
```

## Resend OTP
**POST** [https://<our_server_URL>/api/auth/otp/resend](https://<our_server_URL>/api/auth/otp/resend)
**Headers:**
```
x-api-key: “{api key}”,
Authorization: “{token from create wallet}”
```
**Payload:**
```
{
	“mobileNumber”: “27765408650”
}
```
**Response:**
```
{
"data": {
	“token” : “{token that will be needed for OTP confirmation}”
},
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

# Add Test Results Endpoints

## Add test result
**POST** [https://<our_server_URL>/api/test_results](https://<our_server_URL>/api/test_results)
**Headers:**
```
x-api-key: “{api key}”,
```
**Payload:**
```
{
  "WalletId": “{walletId}”,
  "resultStatus": “{Positive}”,  // Enum
  "laboratory": “{Lancet}”, //Enum
  "referenceNumber":”{12345AB}”,
  "testedAt": "0001-01-01", 
  "hasConsent": false
}
```
**Response:**
```
{
 "data":  {
true
}
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

## Enums
```
ResultStatus
    {
        Untested = 0,
        Negative = 1,
        Positive = 2,
    }
LaboratoryStatus
    {
        Unsent,
        InProgress,
        Verified
    }
Laboratory
    {
        NHLS = 0,
        Lancet = 1,
        Pathcare = 2,
    }
```

## Get Wallet Status
**POST** [https://<our_server_URL>/api/wallets/{walletId}/status](https://<our_server_URL>/api/wallets/{walletId}/status)
**Headers:**
```
x-api-key: “{api key}”,
```
**Payload:**
```
{
  	“key”: "{key}"
}
```
**Response:**
```
{
 "data":  
{
  "firstName": “{firstname}”,
  "lastName":”{lastname}”,
  "photoUrl": “{https://S3bukceturl}”,
  "resultStatus": “{Positive}”, // Enum displayed return as a string
  "status": {2} //Correspond with the enum
}
   },
   "meta": {
       "success": true,
       "code": 200,
       "message": null
   }
}
```

## Enums
```
ResultStatus
    {
        Untested = 0,
        Negative = 1,
        Positive = 2,
    }
```

