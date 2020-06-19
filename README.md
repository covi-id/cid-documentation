<div align="center">
    <img src="./imgs/logo-dark.png">
</div>
<h3>
    Covi-ID is an open source risk management tool designed to protect privacy.
</h3>

---

# Index

#### [Introduction](#Introduction-two)
* [Problem](#Problem)
* [Solution](#Solution)
* [Composition](#Composition)
* [Benefits](#Benefits-of-Covi-ID)

#### [Functionality Overview](#Functionality-Overview)
* [Overview](#Overview)
* [Data Flow](#Data-Flow)

#### [Going Forward](#going-forward)

The implementation repositories can be found here:

#### [> `API Core`](https://github.com/covi-id/cid-api-core) 
#### [> `Web App`](https://github.com/covi-id/cid-web-app)
#### [> `Mobile App`](https://github.com/covi-id/cid-mob-app)

Additional documentation

#### [> `End Points Documentation`](https://github.com/covi-id/cid-documentation/blob/master/end_points.md)

---

<div align="center">
    <img src="./imgs/logo-dark.png">
</div>

# Introduction

## Problem

As countries attempt to kickstart their economy in the wake of the COVID-19 crisis, it is vital that efficient measures are in place to contain future outbreaks of the virus. Businesses are looking for ways to protect their employees and customers, and to enable disease surveillance, ultimately preventing the spread of infection.

Businesses currently struggle with:

* Fulfilling corporate responsibilities to protect their employees and staff.
* Implementing efficient track and trace solutions, which are often time consuming and costly.

## Solution

Covi-ID offers an easy-to-use mobile verifier app that provides businesses with a simple, and free way to fulfil their responsibility to society during COVID times. This takes the shape of a simple system that can:

* Enable contact tracing by capturing “check-ins” and “check-outs” related to an organisation's geolocation.
* By partnering with third parties such as SafePlaces, customers/employees can be notified if they have been exposed to a confirmed COVID-19 case, giving them extra peace of mind without creating additional cost for the business.

## Composition

The Covi-ID solution is made up of two components:

* Covi-ID Web application (platform) - provides the ability for users to easily generate their personal Covi-ID which acts as their personal data wallet.
* Covi-ID Mobile app - enables organisations to monitor the flow of individuals on their premises and enable a contact tracing mechanism for their employees and clients. Currently only available on Android, iStore distribution pending review. 

## Benefits of Covi-ID

* Individuals can easily generate QR codes from the Covi-ID web application.
* Verifiers can scan an individual's QR code using the Covi-ID verifier app to “check-in” or “check-out” of their premises.
* Whenever a verifier scans the QR code of an individual, the verifier leaves a token with a geolocation and time stamp in the individual’s personal data wallet.
* Covi-ID uses the latest privacy-preserving technology to support manual track and trace solutions.
* Individuals who test positive for COVID-19 can volunteer to submit their data to the SafePlaces application, which creates a real-time map of COVID-19 hotspots across South Africa.

---

# Functionality

## Overview

The Covi-ID solution offers several functionalities:

* **Managing access to an organisation's premises**: The mobile app is used by a security guard or individual managing access to an organisation’s premises. On entering a premises, an individual presents their pre-generated Covi-ID QR code to the verifier. Using the mobile app, the verifier scans the QR code and checks the individual in. On exiting the building, the verifier again scans the individuals QR code and checks the individual out.

* **COVID-19 location tracking:** On scanning an individual's QR code and checking them into or out of the organisation's premises, the verifier’s geolocation receipt (shared from the mobile app) is stored in the individual’s Covi-ID wallet. The latest geolocation receipt is stored along with all others from other Covi-ID establishments that the individual has visited over the past 14 days. This wallet is only accessible by the individual through the use of their unique QR code. 

* **Update COVID-19 status:** Should an individual test positive for COVID-19, their status can be updated on the Covi-ID server. The individual uploads their QR code via the Web Platform to access their Covi-ID wallet. The individual is then able to enter the details of their test, including the test reference number, laboratory through which the test was conducted, and test date. The individual can volunteer to submit their anonymous geolocation receipts to Safe Places. Once the data is submitted to the Safe Places server, it contributes to a real-time map of COVID-19 hotspots.

* **Accessible for users without a mobile phone:** In the case that an individual does not have access to their own computer or smartphone, a family member, friend or affiliated organisation can generate a Covi-ID on their behalf by following the standard setup process using the individuals personal details, but by adding the mobile number of a close relative or friend. The generated QR code can be printed out for later use.


## Data Flow

The system is designed to protect the privacy of users who contribute location check in data. The core privacy preserving capabilities are enabled by the use of a Trusted Execution Environment (TEE), which encrypts data using a private-public keypair, in both transit and at rest This enables the Covi-ID system to render the database operator unable to read the database contents, a core requirement of returning true data ownership and privacy back to the data owner.

---

# Going Forward
The next version Covi-ID platform has been built to include a secure enclave implementation that acts as a Trusted Execution Environment (TEE). Details around this implementation can be found [here](https://github.com/covi-id/cid-documentation/blob/master/enclave.md).

The current version of the Covi-ID platform will thus be deprecated but details regarding this implementation can be found [here](https://github.com/covi-id/cid-documentation/blob/master/coviid_api.md).