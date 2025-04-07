---
icon: arrow-right-arrow-left
---

# Transfer

The Transfers API is designed to provide clear visibility into wire transfer payments, especially for paying down card statement balances. Transfers may include debits (pulling funds from one account) and credits (depositing funds into another), and they can be subject to various stages of processing before completion. Transfers typically go through the ODFI (Originating Depository Financial Institution), which sends requests to the RDFI (Receiving Depository Financial Institution), often with intermediary steps like routing through the Federal Reserve (Fed).

## Use Cases

Provide visibility into the status of wire transfers used to pay down card statement balances.

Retrieve detailed records of past wire transactions, including status changes and reconciliation timestamps. This enables easy reporting and simplifies reconciling Ramp card payments within broader accounting records.

## Transfer Statuses

### **CANCELED**

_The transfer has been canceled and will not be processed._\
This state is terminal and indicates the transfer was intentionally stopped, either by the customer or due to an operational decision.

### **COMPLETED**

_The transfer has successfully settled and reached its final state._\
Funds have moved as intended, and no further actions are required. Any subsequent issues would require a new transfer or reversal process.

### **ERROR**

_The transfer encountered a terminal error and could not be completed._\
This can happen due to issues like invalid account information or system errors. The transfer will need to be retried with corrected details.

### **INITIATED**

_A record of the transfer has been created in Ramp’s system._\
The process of moving funds has started, but no external actions have been taken yet.

### **NOT\_ACKED**

_Ramp did not receive an acknowledgment from the bank after submitting the transfer._\
The transfer may need to be retried or investigated for connectivity issues with the bank.

### **NOT\_ENOUGH\_FUNDS**

_The transfer could not proceed due to insufficient funds in the originating account._\
Additional funds need to be added to the account before re-attempting the transfer.

### **PROCESSING\_BY\_ODFI**

_The transfer file has been submitted to the Originating Bank (ODFI), and the cutoff time for same-day processing has passed._\
The ODFI has accepted the file and will proceed with sending it to the Fed.

### **REJECTED\_BY\_ODFI**

_The transfer was rejected by the ODFI before it was sent to the Federal Reserve._\
The rejection usually occurs due to formatting errors or invalid account information in the transfer file.

### **RETURNED\_BY\_RDFI**

_The transfer failed at the Receiving Bank (RDFI) due to an issue provided by the receiving bank._\
Common reasons include closed accounts or incorrect account numbers. This requires corrective action to resolve.

### **SUBMITTED\_TO\_FED**

_The transfer file has been sent to the Federal Reserve and is awaiting routing to the RDFI._\
The transfer is now in the Fed’s hands and will soon be sent to the receiving bank.

### **SUBMITTED\_TO\_RDFI**

_The transfer has been delivered to the Receiving Bank (RDFI), and we are awaiting the final transfer of funds._ This is the final step before the funds are made available to the receiving party.

### **UNNECESSARY**

_The transfer is for $0 and does not require processing through the ACH system._\
This is a terminal state, indicating no further actions are required.
