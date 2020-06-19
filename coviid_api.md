<div align="center">
    <img src="./imgs/logo-dark.png">
</div>
<h1>
    CoviID .Net Core API Documentation
</h1>
<h3>
    Covi-ID is an open source risk management tool designed to protect privacy.
</h3>

The following document contains details about the REST API that is hosted on AWS. The code for this API can be found [here](https://github.com/covi-id/cid-api-core)

---

# Index

* [Database Design](#database-design)
* [Endpoints](#End-points)
* [Annexture](#annexture)
  * [Database Design Source](#database-design-source)

---
## Database Design

<div align="center">
    <img src="./imgs/Coviid-API-Database-Design.PNG">
</div>

The source code for the database design can be seen below. This source code can be used with an app like [Dbdiagrams](https://dbdiagrams.io).

```
table OtpTokens
{
  Id bigint
  Code integer 
  MobileNumber string [note: 'serverPk']
  isUsed boolean
  CreatedAt datetime
  ExpireAt datetime
}

table Organisations
{
  Id guid
  Name string
  Payload string 
  CreatedAt datetime
}

table OrganisationAccessLogs
{
  Id guid
  Organisation Organisations [ref: > Organisations.Id]
  ScanType ScanType
  CreatedAt datetime
}

table Wallets
{
  Id guid
  CreatedAt datetime
  VerifiedAt datetime
}

table WalletDetails
{
  Id guid
  WalletId Wallets [ref: > Wallets.Id]
  FirstName string  [note: 'serverPk, userPK']
  LastName string [note: 'serverPk, userPK']
  MobileNumber string [note: 'serverPk']
  isMyMobileNumber bool 
  PhotoReference string  [note: 'serverPk, userPK']
  HasConsent bool
  CreatedAt datetime
}

table WalletTestResults
{
  Id guid
  WalletId Wallets [ref: > Wallets.Id]
  TestType TestType
  LabStatus LabStatus
  hasReceivedResults bool
  ResultStatus ResultStatus
  Laboratory Laboratory
  TestedAt datetime
  IssuedAt datetime 
  ReferenceNumber string  [note: 'serverPk, userPK']
  HasConsent boolean
  PermissionGrantedAt datetime
  CreatedAt datetime
}

table WalletLocationReceipt
{
  Id guid
  WalletId Wallets [ref: > Wallets.Id]
  Longitude decimal 
  Latitude decimal 
  ScanType ScanType 
  CreatedAt datetime 
}

table session
{
  Id guid
  WalletId Wallets [ref: > Wallets.Id]
  ExpireAt datetime
  isUsed boolean
  CreateAt datetime
}

enum ScanType
{
  CheckIn
  CheckOut
  Denied
}

enum Laboratory
{
  NHLS
  Lancet
  Pathcare
}

enum LaboratoryStatus
{
  Unsent
  InProgress
  Verified
}

enum ResultStatus
{
  Untested
  Negative
  Positive
}

enum TestType
{
   Covid19
}
```


## End-points 

The end-points for this API are implemented in a RESTful standard and a detailed file can be found [here](https://github.com/covi-id/cid-documentation/blob/master/end_points.md).