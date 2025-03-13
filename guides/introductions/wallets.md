# Wallets Overview
Payfonte assigns three types of wallets to each merchant for every country they subscribe to. 
These wallets are designed to manage different aspects of the merchant's financial transactions. 
This document provides a detailed explanation of the three wallet types and the actions that can be performed on these wallets.

## Wallet Types

### Main Wallet
The Main Wallet is the primary wallet for every merchant account. It serves as the central hub for managing funds.

**Purpose**
- Receives funds from the Deposit Wallet at regular intervals.
- Funds can be transferred from the Main Wallet for payout and disbursement purposes.

**Usage**
- Funds are transferred here from the Deposit Wallet.
- Funds can be moved to the Payout Wallet for the purpose of making payouts.

### Deposit Wallet
The Deposit Wallet is where all funds collected from payments are initially deposited.

**Purpose**
- Acts as the initial receiving wallet for all payment collections.
- At regular intervals, funds from the Deposit Wallet are transferred to the Main Wallet to be available for further use.

**Usage**
- Collects payment values.
- Automatically transfers funds to the Main Wallet.

### Payout Wallet
The Payout Wallet is used for handling payouts from the platform.

**Purpose**
- Funds required for payouts are deducted from this wallet.
- Merchants need to fund this wallet to execute payouts.

**Usage**
- Funds can be transferred from the Main Wallet to the Payout Wallet.
- Executes all payout transactions.



## Wallet Actions

### Transfer Funds
Funds can be transferred between wallets as needed. This action allows for the movement of funds from `main` wallet to another to facilitate different types of transactions.
   ![image ](../_images/transfer.png)

### Suspend Wallet
Merchants have the ability to suspend a wallet in case of security concerns, such as a compromised `disbursement PIN` or compromised `API Credentials`.
   ![image](../_images/suspend.png)

#### Effect of Suspension:
A suspended wallet has a "post no debit" status. Funds can still be deposited into a suspended wallet, but all outgoing transactions (deductions) will be rejected.
Usage: Suspend the wallet to prevent unauthorized disbursements while still allowing deposits.

#### Summary
The three types of wallets in Payfonte's system are designed to streamline and secure the financial operations of merchants. The Main Wallet serves as the central account, the Deposit Wallet collects initial payment funds, and the Payout Wallet manages payout transactions. Merchants can transfer funds between these wallets and suspend them if necessary to protect their accounts.

This documentation ensures that merchants understand the functionality and management of their wallets, facilitating smooth and secure financial transactions on the Payfonte platform.





