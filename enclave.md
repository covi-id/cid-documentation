
<div align="center">
    <img src="./imgs/logo-dark.png">
</div>
<h3>
    Covi-ID is an open source risk management tool designed to protect privacy.
</h3>

---

# Covi-ID JSON-RPC Backend

The following is the public-facing interface for frontend/backend application to consume and interact with the SGX enclave

## Introduction

## Architecture

**Core Components**

* Marketing Site: www.coviid.me
* Mobile Application: Android/iOS app store
* Web Platform: www.app.covid.me
* Backend: www.api.coviid.me
  * Enclave
  * Enigma Rust Implementation for Intel SGX

### Stack

* **Backend Application (RESTful API)**
  * Framework: Node
  * ORM: File Storage - HashMap
  * Sms provider: Clickatell
  * Authentication: X-api key, DH Rotational Keys

* **Frontend Application (Client)**
  * Framework: ReactJS
  * State Management: Redux
  * Build Pipeline: AWS Code Deploy -> moving to azure pipelines

* **Enclave**
  * Framework: Rust
  * Compute: Intel-SGX based service that uses Trusted Execution Environments (TEE) Azure-DC1s* 
  * OS: Ubuntu Bionic (18.04) or newer
  * Storage: Trusted Memory

* **Infrastructure**
  * Provider: Azure
  * VM: Private Compute Instance - SGX-capable computer host with SGX enabled in the BIOS - Azure-DC1s* 
  * OS: Ubuntu Bionic (18.04) or newer
  * Database: MySQL


## Credits

`We want to acknowledge and credit Enigma/SafeTrace from which this repository is forked ` 

....

# Installation

## Requirements

The code in this folder assumes that it will be run on the same server that runs the code in the [enclave](../enclave) folder, which requires SGX (see that folder for additional information). Otherwise adjust the `ENCLAVE_URI` in [index.js](index.js) accordingly.

## Setup

1. Clone this repository

2. Move into this `api-server` subfolder:

	```bash
	cd api-server
	```

3. Install package dependencies:

	```bash
	yarn install
	```

## Development

For development or debugging purposes, this server can be run directly with:

```bash
node ./index.js
```

## Production

To put this server in production, one can use [pm2](https://pm2.keymetrics.io/docs/usage/startup/):

```bash
npm run pm2-startup
```
Then, simply copy/paste the line PM2 command gives you above and the startup script will be configured for your OS.
In Ubuntu, you can start the system service as:

```bash
sudo systemctl start pm2-${USER}
```

*You would need to run this for the api-server and enclave.

## Enclave

### Requirements

* A suitable computer system with SGX enabled in the BIOS.
* Ubuntu Bionic 18.04 or newer.
* Rust language support.

### Installation

1. Clone the repository.
2. Install the SGX driver and the SDK.
3 .Move into the enclave/safetrace subfolder.

```bach
cd enclave/safetrace
```

4. Compile the code.

```bash
make
```

5. Run the enclave code.

```bash
cd bin
```

```bash
./safetrace-app
```

# Limitiations / Improvements

# Endpoints

Documented below are the endpoints and functionality of the interface exposing the enclave functionality.

Please see [Simulation](../client/simluation.js) for examples of how to implement from the client side.

## General

### **Get Signature**

Request the enclaves signature for verification

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_general}

**Request**

Method name: `getSignature`

**Returns**

* `status` (Integer) - `0` if the operation was successful
* `signature` (String) - Signature of `enclavePubKey`

```json
{ 
	"id": "",
	"type": "getSignature",
	"result": {
	 	"signature": "" 
	} 
}
```

### **Get New Encryption Key**

Requests a public encryption from the enclave that will be used to encrypt/decrypt data for the next user request.

**Request**

Method name: `newTaskEncryptionKey`

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_ekey}

**Parameters**

* `userPubKey` (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `enclavePubKey` (String) - 64-byte public key for Diffie-Hellman
* `signature` (String) - Signature of `enclavePubKey`

**Example**

```
curl -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","id":1, "method": "newTaskEncryptionKey","params": { "userPubKey":"cc955077ff7aeb67e544bb0dfad0a5ac1d3117f4115c528d38da9c2337cb033ec08f1d12a580d2ccfed02144e70d961c72e28e92ef48b9056c08137918c5ab2d"}}' http://13.82.53.99:8080
```
---

## Wallet

### **Create Wallet**

Create a new covi-ID wallet 

Encrypted Data Json Payload:

* `first_name`: String
* `last_name`: String
* `photo_reference`: String: base64
* `mobile_number`: String {country_code}{mobile_number} Ex: 27735995564
* `hashed_mobile_number`: String - SHA256(mobile_number)
* `is_my_mobile_number`: bool
* `mobile_number_verified`: bool
* `has_consent`: bool
* `created_at`: String - Datetime: YYYY-MM-DDTHH:mm:ss

**Request**

Method name: `createWallet`

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_general}

**Parameters**

* `encryptedData` (String) - encrypted data
* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful

```json
{ 
	"id": "3d6564b44e",
	"type": "createWalletResult",
	"result": {
	 	"status": 0 
	} 
}
```

 **Example**

Method: `createWallet`
```json
{
	 "jsonrpc":"2.0",
	 "id":1, 
	 "method": "createWallet",
	 "result": { 
		 "userPubKey":"{user_pub_key}",
		 "encryptedData":"{encrypted_data}",
	 }
}
```

### **Get Wallet**

Get wallet endpoint

Encrypted Data Json Payload:

 * `walletId`: String

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_restricted}

**Request**

Method name: `getWallet`

**Parameters**

* `encryptedData` (String) - encrypted data
* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful
* `encryptedOutput` returns a walletObject and needs to be decrypted to obtain the json object.

```json
	{
		 "walletId": "", 
		 "first_name": "", 
		 "last_name": "", 
		 "photo_reference": "{base64}", 
		 "hashed_mobile_number": "", 
		 "mobile_number_verified": 0, 
		 "has_consent": 0, 
		 "created_at": "",  
	}
```
**Successsful Operation**

```json
    { 
        "id": "4078a17e30",
        "type": "GetWallet",
        "result": {
			"status": 0,
			"encryptedOutput": "25b2ec3c7b1ed1fdd0bf2fcc517......"
		}
	}
```

**Failed Operation**

 ```json
	{ 
		"id": "da7d3d68ff",
		"type": "GetWallet",
		"result": {
			"status": -1,
		}
   }
```

### **Get Wallet Status**

Get walletStatus endpoint

Encrypted Data Json Payload:

 * `walletId`: String

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_verifier}

**Request**

Method name: `getWalletStatus`

**Parameters**

* `encryptedData` (String) - encrypted data
* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful
* `encryptedOutput` returns a walletObject and needs to be decrypted to obtain the json object.
  
```json
	{
		 "walletId": "", 
		 "first_name": "", 
		 "last_name": "", 
		 "photo_reference": "{base64}", 
		 "status": 0, 
	}
```
**Successsful Operation**

```json
    { 
        "id": "4078a17e30",
        "type": "getWalletStatus",
        "result": {
			"status": 0,
			"encryptedOutput": "25b2ec3c7b1ed1fdd0bf2fcc517......"
		}
	}
```

**Failed Operation**

 ```json
	{ 
		"id": "da7d3d68ff",
		"type": "getWalletStatus",
		"result": {
			"status": -1,
		}
   }
```

### **Confirm Mobile Verification**

Confirm OTP sent to mobile number related to a wallet

Encrypted Data Json Payload:

* `otp`: String

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_general}

**Request**

Method name: `mobileConfirm`

**Parameters**

* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman
* `encryptedData` (String) - encrypted data
* `encryptedUserId` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful
  
**Successsful Operation**

```json
    { 
        "id": "4078a17e30",
        "type": "mobileConfirm",
        "result": {
			"status": 0,
			"limt": 3,
		}
	}
```

**Failed Operation**

 ```json
	{ 
		"id": "da7d3d68ff",
		"type": "mobileConfirm",
		"result": {
			"status": -1,
		}
   }
```

### **Mobile Verification (Resend)**

Generate a OTP for mobile verification for a wallet

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_general}

**Request**

Method name: `mobileVerify`

**Parameters**

* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman
* `encryptedUserId` - (String) - 64-byte public key for Diffie-Hellman
* `mobileNumber` - (String) Ex 27734995587 {countrycode}{mobileNumber}

**Returns**

* `status` (Integer) - `0` if the operation was successful
  
**Successsful Operation**

```json
    { 
        "id": "4078a17e30",
        "type": "mobileVerify",
        "result": {
			"status": 0,
			"limt": 3,
		}
	}
```

**Failed Operation**

 ```json
	{ 
		"id": "da7d3d68ff",
		"type": "mobileVerify",
		"result": {
			"status": -1,
		}
   }
```

### **Add Wallet Location Receipt**

Wallet location receipts are created when a user is checked in or out using the verifier application

Encrypted Data Json Payload:

* `lat`: f64
* `lng`: f64
* `scan_type`: ScanType: (CheckIn, CheckOut, Denied)
* `created_at`: String - Datetime: YYYY-MM-DDTHH:mm:ss

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_general}

**Request**

Method name: `addWalletLocationReceipt`

**Parameters**

* `encryptedData` (String) - encrypted data
* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful

```json
{ 
	"id": "3d6564b44e",
	"type": "addWalletLocationReceipt",
	"result": {
	 	"status": 0 
	} 
}
```

**Example**

Encrypted data example

```json
{
	"lat": 40.757339,
	"lng": -73.985992,
	"scan_type": "CheckIn",
	"created_at": "1970-01-01T00:00:00.000"
}
```

Method: `addWalletLocationReceipt`
```json
{
	 "jsonrpc":"2.0",
	 "id":1, 
	 "method": "addWalletLocationReceipt",
	 "result": { 
		 "userPubKey":"{user_pub_key}",
		 "encryptedData":"{encrypted_data}",
	 }
}
```

### **Add Wallet Test Result**

Adding test results to a users wallet.

Encrypted Data Json Payload:

* `test_type`: TestType,
* `laboratory_status`: LaboratoryStatus,
* `has_received_results`: bool,
* `result_status`: ResultStatus,
* `laboratory`: Laboratory,
* `tested_at`: String,
* `issued_at`: String,
* `reference_number`: String,
* `has_consent`: bool,
* `permission_granted_at`: Option<String>,
* `created_at`: String,

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_general}

**Request**

Method name: `addWalletTestResult`

**Parameters**

* `encryptedData` (String) - encrypted data
* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful

```json
{ 
	"id": "3d6564b44e",
	"type": "addWalletTestResult",
	"result": {
	 	"status": 0 
	} 
}
```

**Example**

Encrypted data example

```json
{
	"test_type": "",
	"laboratory_status": "",
	"has_received_results": 0,
	"result_status": "",
	"laboratory": "",
	"tested_at": "",
	"issued_at": "",
	"reference_number": "",
	"has_consent": "",
	"permission_granted_at": "",
	"created_at": ""
}
```

Method: `addWalletTestResult`
```json
{
	 "jsonrpc":"2.0",
	 "id":1, 
	 "method": "addWalletTestResult",
	 "result": { 
		 "userPubKey":"{user_pub_key}",
		 "encryptedData":"{encrypted_data}",
	 }
}
```

### **Get Wallet Test Result**

Get Wallet Test Results

Encrypted Data Json Payload:

 * `walletId`: String

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_request_restricted}

**Request**

Method name: `getWalletTestResult`

**Parameters**

* `encryptedData` (String) - encrypted data
* `userPubKey` - (String) - 64-byte public key for Diffie-Hellman

**Returns**

* `status` (Integer) - `0` if the operation was successful
* `encryptedOutput` returns a walletObject and needs to be decrypted to obtain the json object.
  
**Encrypted output**

```json
	{
		"test_type": "",
		"laboratory_status": "",
		"has_received_results": 0,
		"result_status": "",
		"laboratory": "",
		"tested_at": "",
		"issued_at": "",
		"reference_number": "",
		"has_consent": "",
		"permission_granted_at": "",
		"created_at": ""
	}
```
  
**Successsful operation**

```json
    { 
        "id": "4078a17e30",
        "type": "getWalletTestResult",
        "result": {
			"status": 0,
			"encryptedOutput":"{encrypted_data}",

		}
	}
```

**Failed operation**

 ```json
	{ 
		"id": "da7d3d68ff",
		"type": "getWalletTestResult",
		"result": {
			"status": -1,
		}
   }
```

## **Tracing Consent**

The followng endpoint is used in the Tracing admin system, when a user uploads their postive rest results, a unqiue code is generated for consent `wallet: consent_code` and retuned to the user to keep. When the tracer recieves this code from the infected indviudal, they will login to a tracing admin and input the mobile number and consent code. This will trigger a decryption task of the location data and submisson task to safeplaces api.

PlainText Json Payload:

 * `mobileNumber`: String `Ex: 27735995564`
 * `consent_code`: String `Ex: KJ28PH`

**Request**

Method name: `submitTracingConsent`

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_consent}

**Payload**

```json
	{
		"mobileNumber": "{mobile_number}",
		"consenCcode": "{consent_code}",
	}
```

**Returns**

* `status` (Integer) - `0` if the operation was successful

**Example**

```
curl -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","id":1, "method": "submitTracingConsent","params": { "mobileNumber":"27843251502","consentCode":"TtGbX0dp"}}' http://13.82.53.99:8080
```
---
  
**Encrypted output**

**Successsful operation**

```json
    { 
        "id": "4078a17e30",
        "type": "submitTracingConsent",
        "result": {
			"status": 0,
		}
	}
```
**Failed operation**

 ```json
	{ 
		"id": "da7d3d68ff",
		"type": "submitTracingConsent",
		"result": {
			"status": -1,
		}
   }
```
# Endpoint Auth

**Headers**

* `Content-Type`: application/json
* `X-API-Key` {x_api_key_consent}

**Types**

* x_api_key_consent
* x_api_key_request_restricted
* x_api_key_request_general

# QR Construction

```json
{
	"env": "dev",
	"sig": "coviid.{enclaveSignature}",
	"wId": "{uuiud}"
}
```

# Data Specification

### **Wallet**

```c
struct Wallet {
    id: String,
    details: WalletDetails,
    receipts: Vec<WalletLocationReceipt>,
    test_results: Vec<WalletTestResult>,
	consent_code: String,
	consent_at: String,
    created_at: String,
    verified_at: String
}

struct WalletDetails {
    first_name: String,
    last_name: String,
    photo_reference: String,
	mobile_number: String,
	hashed_mobile_number: String,
	is_my_mobile_number: bool,
	mobile_number_verified: bool,
	has_consent: bool
}

struct WalletOtp {
	walletId: String,
	otp: String
	limit: i32
}
```

### **Location**

```c
struct WalletLocationReceipt {
    latitude: f64,
    longitude: f64,
    scan_type: ScanType,
    created_at: String
}
```
### **Test Results**

```c
struct WalletTestResult {
    test_type: TestType,
    laboratory_status: LaboratoryStatus,
    has_received_results: bool,
    result_status: ResultStatus,
    laboratory: Laboratory,
    tested_at: String,
    issued_at: String,
    reference_number: String,
    has_consent: bool,
    permission_granted_at: Option<String>,
    created_at: String,
}
```

### **Enums**

```c
enum ScanType {
	CheckIn,
    CheckOut,
    Denied
}

enum TestType {
    Covid19
}

enum LaboratoryStatus {
    Unsent,
    InProgress,
    Verified
}

enum Laboratory {
    NHLS = 0,
    Lancet = 1,
    Pathcare = 2,
}

enum ResultStatus {
    Untested = 0,
    Negative = 1,
    Positive = 2
}
```
