<div align="center">
    <img src="./imgs/logo-dark.png">
</div>
<h3>
    Covi-ID V2.0 is an open source risk management tool designed to protect privacy.
</h3>

---


# Index

#### [Version Two](#version-two)
* [Version Two Features and Changes](#version-two-features-and-changes)
* [Migration](#migration)
#### [How Covi ID works](#how-covi-id-works)
* [DevOps](#devops)
    * [Database Design](#database-design)
    * [Data View](#data-view)
    * [QR Code Storage](#qr-code-storage)
* [Status Logic](#status-logic)
#### [Sequence Diagrams](#sequence-diagrams)
* [User Generates non SSI Wallet](#user-generates-non-ssi-wallet)
* [User Adds Test Results](#user-adds-test-results)
* [User Check In](#user-check-in)
* [User Check Out](#user-check-out)
* [User Cancels Check In](#user-cancels-check-in)
#### [Going Forward](#going-forward)

The implementation repositories can be found here:
#### [> `API Core`](https://github.com/covi-id/cid-api-core) 
#### [> `Web App`](https://github.com/covi-id/cid-web-app)
#### [> `Mobile App`](https://github.com/covi-id/cid-mob-app)

---

<div align="center">
    <img src="./imgs/logo-dark.png">
</div>

# Version Two

## Version Two Features and Changes

* The system architecture will be modified so that the user identities (Wallet ID’s) created and their associated data will be stored in the Covi ID servers and database (existing user information on StreetCred will need to be imported into Covi ID servers accordingly - this will occur in the next sprint).
    * The generation of the UUID occurs when the Covi ID (Wallet ID) is written to the database as the database possesses built-in capabilities to generate the UUID when writing new records to the table. 
* User identities will now be created with the generation of a secret key. Symmetric cryptography was employed. Advanced Encryption Standard was used. 
* Privacy-preserving techniques are employed to protect user data. User data will be stored in an encrypted manner using the secret key.
* This key is stored in the users QR Code along with their Wallet ID.
* User data cannot be decrypted without the secret key and even when decrypted, this is only done in a raw state of transit and this key is never stored by Covi ID’s servers. The state of transit is required for the Covi ID API to pull the users status and return it to the verifier. 
 
It is important to note that despite moving away from the StreetCred SSI management system, the concept of SSI still remains as encryption techniques have been employed. 

## Migration

It has been determined that the existing code-base from Covi-ID Version 1.0 will be modified and adjusted accordingly. A new code base was not created. 

In terms of the encryption, the code relating to it will be segmented within the code-base making any modification to any encryption aspects easily accessible without upsetting the entire code structure. To begin with, all data will be encrypted at a base level.

---

# How Covi ID Works

## DevOps

### Database Design

### Data View

### QR Code Storage

## Status Logic

---

# Sequence Diagrams

## User Generates non SSI Wallet

## User Adds Test Results

## User Check In

## User Check Out

## User Cancels Check In

---

# Going Forward

