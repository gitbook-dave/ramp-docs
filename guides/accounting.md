---
icon: calculator
---

# Accounting

## Overview

Ramp’s API can be used to keep data in sync between Ramp and an external accounting system.

## Relevant Endpoints

| Endpoints                                                                                                             | Use                                                                                          |
| --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [Accounting Connection](https://docs.ramp.com/developer-api/v1/api/accounting-connections)                            | Understand and manage the current state of an account's Accounting Connection.               |
| [Accounting Custom Fields](https://docs.ramp.com/developer-api/v1/api/accounting#post-developer-v1-accounting-fields) | Leverage fields to code transactions, reimbursements, and bills.                             |
| [Accounting Vendors](https://docs.ramp.com/developer-api/v1/api/accounting-vendors)                                   | Manage Vendor options for coding transactions, reimbursements, bills, and purchase orders.   |
| [Accounting Sync](https://docs.ramp.com/developer-api/v1/api/accounting-syncs#post-developer-v1-accounting-syncs)     | Mark objects as ‘synced’ after successful addition to the remote accounting system.          |
| [Accounting GL Accounts](https://docs.ramp.com/developer-api/v1/api/accounting-gl-accounts)                           | Manage GL accounts used for coding transactions, reimbursements, bills, and purchase orders. |
| [Entities](https://docs.ramp.com/developer-api/v1/api/entities)                                                       | View business entities associated with a Ramp account.                                       |
| [Transactions](https://docs.ramp.com/developer-api/v1/api/transactions)                                               | Fetch accounting codings and metadata from card transactions.                                |
| [Bills](https://docs.ramp.com/developer-api/v1/api/bills)                                                             | Fetch accounting codings and metadata from bills and payments.                               |
| [Reimbursements](https://docs.ramp.com/developer-api/v1/api/reimbursements)                                           | Fetch accounting codings and metadata from employee reimbursements.                          |
| [Cashbacks](https://docs.ramp.com/developer-api/v1/api/cashbacks)                                                     | Fetch accounting codings and metadata from cashback earned via Ramp cards.                   |
| [Statements](https://docs.ramp.com/developer-api/v1/api/statements)                                                   | Fetch payments for Ramp card statements.                                                     |

## Understanding the Accounting Connection

When working with the Accounting APIs, it’s crucial to understand the behavior of the Accounting Connection. A single Ramp account can only have one active accounting connection at a time. Each accounting connection is uniquely associated with its own set of:

* Accounting Custom Fields
* Accounting Vendors
* Accounting GL Accounts

Switching the accounting connection will result in a loss of data from these fields, but it is possible to re-activate a connection which will restore them.

Within Ramp, you can view the current Accounting Connection within the **Settings** drawer on the **Accounting** tab:

<figure><img src="https://docs.ramp.com/assets/current-accounting-connection-vqChXzt5.png" alt=""><figcaption></figcaption></figure>

You can check the status of an account’s accounting connection by using the [Fetch Accounting Connection](https://docs.ramp.com/developer-api/v1/api/accounting-connections) endpoint. There are 2 types of accounting connections:

### Direct Connection

Direct connections are managed by Ramp and are activated when a Ramp customer onboards onto one of our out-of-the-box Accounting Integrations such as QuickBooks or Netsuite.

* Direct connections can only be enabled in the Ramp UI, as they require authorization with the remote system.
* Direct connections can be disabled via API or in the Ramp UI.
* When a direct connection is enabled, the following fields are **read-only** via API:
  * Accounting Custom Fields
  * Accounting Vendors
  * Accounting GL Accounts

### UCSV Connection

The UCSV connection is the default connection for new sandbox accounts. The UCSV connection is more flexible, allowing for accounting fields to be managed freely.

Ramp customers typically use the UCSV connection to manually customize their accounting preferences and export data, uploading it into their ERP in the desired format.

API integrators can manage this manual process on behalf of customers by managing Accounting Fields programmatically and fetching the latest data to upload into the remote system.
