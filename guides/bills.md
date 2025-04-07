---
icon: file-invoice-dollar
---

# Bills

## Overview

Use Ramp’s API to manage vendors, bills, and payments for Accounts Payable processes. Before starting, familiarize yourself with the basics of Ramp Bill Pay via the Help Center or this video overview.

The following endpoints are now publicly available. They are subject to breaking changes during the beta period, which will be announced in the [changelog](https://docs.ramp.com/developer-api/v1/changelog). There is no scheduled end date for the Beta period.

* Vendors: GET Vendor by ID, GET Vendors (List), POST Vendors, PATCH Vendors, DELETE Vendor
* Vendor Contacts: GET Vendor Contacts by ID, GET Vendor Contacts
* Vendor Accounts: GET Vendor Accounts by ID, GET Vendor Accounts
* Bills: POST Bills, PATCH Bills, DELETE Bills

## Current API Limitations

* Draft bills are not supported.
* Bills created in the API are automatically approved, and approval will not be re-triggered on bill edits made in the API.
* Payments may be created alongside the bill, with limited support for updating payment details once created. Batch payments (creating a single payment for multiple bills) are not supported.
* Managing bank accounts in the API is not supported.
* Attaching images of invoices is not supported.

## Relevant Endpoints

| Endpoints                                                                           | Use                                                                                                                                     | Relevant Scopes                       |
| ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| [Bills](https://docs.ramp.com/developer-api/v1/api/bills)                           | Manage bills and payments to vendors.                                                                                                   | `bills:read`, `bills:write`           |
| [Vendors](https://docs.ramp.com/developer-api/v1/api/vendors)                       | Manage the individuals or institutions which are eligible to receive payments. View contact information and accounts related to vendor. | `vendors:read`, `vendors:write`       |
| [Accounting Vendors](https://docs.ramp.com/developer-api/v1/api/accounting-vendors) | Establish a connection between a Vendor and an Accounting Vendor so that vendors can be used when coding bills for accounting purposes. | `accounting:read`, `accounting:write` |
| [Users](https://docs.ramp.com/developer-api/v1/api/users)                           | Manage the employees who may act as the owners for vendor records.                                                                      | `users:read`, `users:write`           |
| [Entities](https://docs.ramp.com/developer-api/v1/api/entities)                     | View entities in business, including relevant bill pay bank accounts.                                                                   | `entities:read`                       |

## Vendor Management

* **Vendor**: A vendor in Ramp represents a business or individual that provides goods or services to your company. Vendors act as recipients for payments and are required for bill creation and reconciliation.
* **Vendor Owner**: A Vendor owner is a Ramp user who is responsible for maintaining relationships with, and often payments to, a particular vendor. An employee who manages your Facebook Ads spend, for example, would be a suitable Vendor owner of Facebook Ads.

The Bills API uses information from the Users and Vendors endpoints. A high-level flow for creating a new bill might look like:

<figure><img src="https://docs.ramp.com/assets/bill-pay-flow-RJxgQwZl.png" alt=""><figcaption></figcaption></figure>

## Vendors and Accounting Vendors

The Vendor object in the Ramp API is not the same as the Accounting Vendor object. Vendors can be used for bill payments, while Accounting Vendors are used in accounting workflows.

You can establish a connection between an Accounting Vendor and a Vendor by including the `accounting_vendor_remote_id` or `vendor_tracking_category_option_id`; if not passed, a new accounting field option will be created matching the Vendor name.
