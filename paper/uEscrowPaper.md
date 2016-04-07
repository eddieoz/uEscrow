#uEscrow Paper

Anon and trusty escrow service for trading digital assets, powered by smart-contracts.

##Scope

1. Smart-contract (dapp): agnostic, holds all escrow rules.
2. App: connects to smart-contract and execute functions.

## Specs

###Smart-Contract

The Smart-Contract layer is a storage with all information about the escrow.

* Escrow ID
* Timestamp
* escrowA:
  * Asset 
  * Withdraw wallet
  * Amount expected
  * Deposit wallet
* escrowB: 
  * Asset
  * Withdraw wallet
  * Amount expected
  * Deposit wallet
* Expiration time 
* Escrow condition
  * Open
  * Withdraw
  * Expired
  * Executed
  * Error

Functions:

1) Register new escrow:

* Escrow ID
* Timestamp
* escrowA:
  * Asset 
  * Withdraw wallet
  * Amount expected
  * Deposit wallet
* escrowB: 
  * Asset
  * Withdraw wallet
  * Amount expected
  * Deposit wallet
* Expiration time 
* Escrow condition = Open

2) Check if there's an open escrow

If balances from EscrowID[]\(escrowA\(Deposit Wallet >= Amount expected\) AND escrowB\(Deposit Wallet >= Amount expected\)\) then EscrowID[]\(Escrow condition = Withdraw\)

3) Receives a sucessfully transfer for a EscrowID[]\(Escrow condition = Withdraw\)

If true: EscrowID[]\(Escrow condition = Executed\) ELSE EscrowID[]\(Escrow condition = Error\)

4) Check if there's a expired escrow

If now\(\) \> EscrowID[]\(Expiration time\) then EscrowID[]\(Escrow condition = Expired\)

###App

The App know the assets being trade and is responsible for reading information from smart-contract and executing orders. It has 3 simple functions:

1) Create new escrow: Sends the information to the smart-contract with information about new escrow and creates deposit wallets for holding the assets until the condition meets.
We will receive information for two agents, called escrowA and escrowB.

* escrowA:
  * Asset
  * Withdraw wallet
  * Amount expected
* escrowB: 
  * Asset
  * Withdraw wallet
  * Amount expected
* Expiring time  
  
2) Checks if there's an EscrowID[]\(Escrow condition == Open\)

Check balances from EscrowID[]\(escrowA\(Deposit wallet\) AND escrowB\(Deposit wallet\)\) and inform the smart-contract about the balance.

3) Checks if there's an EscrowID[]\(Escrow condition == Withdraw\)

EscrowID[]\(escrowA\(Deposit wallet\) send balance to escrowB\(Withdraw wallet\) AND escrowB\(Deposit wallet\) send balance to escrowA\(Withdraw wallet\)\)
Return the transfer status to the Smart-Contract 

4) Checks if there's an EscrowID[]\(Escrow condition == Expired\)

EscrowID[]\(escrowA\(Deposit wallet\) send balance to escrowA\(Withdraw wallet\) AND escrowB\(Deposit wallet\) send balance to escrowB\(Withdraw wallet\)\)

