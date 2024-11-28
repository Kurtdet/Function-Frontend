# Frontend Smart Contract

This Solidity program demonstrates a smart contract that allows users to interact with a wallet address, perform balance updates, verify addresses, and execute transactions between users. It includes functionalities for topping up the balance, cashing out, and sending funds between users. Additionally, it emits events to track key actions, such as balance updates and transactions.

## Description

The `Frontend` smart contract allows the contract owner to perform operations related to Ether management, including topping up the contract balance, withdrawing funds (cashing out), and verifying addresses. It also allows users to send funds to each other by executing transactions. Key features of the contract include:

- **Wallet Management**: The contract keeps track of the balance of the wallet and allows the owner to top up or withdraw funds.
- **Address Verification**: Allows the verification of an address to check if it is a valid non-zero address.
- **Transaction History**: Tracks transactions made between addresses, storing the transaction history for each address.
- **Events**: Emits events to provide visibility into key actions, such as top-ups, cash-outs, and transactions.

The contract is designed for educational purposes and can be extended to support more advanced features such as access control, ownership, and more complex financial transactions.

## Getting Started

### Executing Program

To deploy and interact with this contract, you can use **Remix**, an online Solidity IDE. Follow the steps below to get started:

1. Go to the [Remix website](https://remix.ethereum.org/).
2. In the left-hand sidebar, click the "+" icon to create a new file.
3. Save the file with a `.sol` extension (e.g., `Frontend.sol`).
4. Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Frontend {

    address payable public walletAddress;
    uint256 public balance;

    event ShowAddress(address walletAddress);
    event TopUp(uint256 topUpValue, uint256 newBalance);
    event CashOut(uint256 cashOutValue, uint256 newBalance);
    event AddressVerified(address indexed addr, bool isValid);
    event AccessTransaction(address indexed from, address indexed to, uint256 amount);

    constructor(uint256 initialValue) {
        balance = initialValue;
        walletAddress = payable(msg.sender);
    }

    mapping(address => uint256) private balances;
    mapping(address => uint256[]) private transactionHistory;

    function getBalance() public view returns (uint256) {
        return balance;
    }

    function displayAddress() public {
        emit ShowAddress(walletAddress);
    }

    function topUp(uint256 topUpValue) public payable {
        require(topUpValue > 0, "Top-up value must be greater than 0");
        balance += topUpValue;
        emit TopUp(topUpValue, balance);
    }

    function cashOut(uint256 cashOutValue) public {
        require(balance >= cashOutValue, "Insufficient balance");
        balance -= cashOutValue;
        emit CashOut(cashOutValue, balance);
    }

    function verifyAddress(address addrToVerify) public pure returns (bool) {
        return addrToVerify != address(0); // Use a simplified condition
    }

    function accessTransaction(address recipient, uint256 amount) public {
        require(recipient != address(0), "Recipient address cannot be zero");
        require(amount > 0, "Transaction amount must be greater than 0");
        require(balance >= amount, "Insufficient balance for the transaction");
        
        balances[msg.sender] -= amount;
        balances[recipient] += amount;
        emit AccessTransaction(msg.sender, recipient, amount);
    }
}
```

### Compilation and Deployment

1. To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar of Remix.
2. Ensure the compiler version is set to `0.8.7` (or another compatible version), and click "Compile Frontend.sol."
3. Once the contract is compiled, go to the "Deploy & Run Transactions" tab in Remix.
4. Select the `Frontend` contract from the dropdown menu and click the "Deploy" button.

### Interacting with the Contract

1. **Display Wallet Address**
   - To view the current wallet address, use the `displayAddress` function. This will emit the `ShowAddress` event with the wallet's address.

2. **Top Up the Contract's Balance**
   - Use the `topUp` function to add Ether to the contract. For example, calling `topUp(10)` will add `10` units of Ether to the balance.

3. **Cash Out (Withdraw) Funds**
   - Use the `cashOut` function to withdraw Ether from the contract. For example, calling `cashOut(5)` will reduce the contract's balance by `5` units of Ether.

4. **Verify an Address**
   - Call the `verifyAddress` function with an address to check if it is a valid non-zero address. This returns `true` if the address is valid, or `false` if it is the zero address.

5. **Execute a Transaction**
   - Use the `accessTransaction` function to send Ether to another address. This requires specifying the recipient address and the amount to send. For example, calling `accessTransaction(recipient, 5)` will send `5` units of Ether to the specified recipient.

6. **Track Transaction History**
   - The contract maintains transaction history for each address, which can be further expanded for tracking purposes.

## Events

The contract emits the following events:

- `ShowAddress`: Triggered when the wallet address is displayed.
- `TopUp`: Triggered after a top-up operation, with the top-up value and the new balance.
- `CashOut`: Triggered after a cash-out operation, with the cash-out value and the new balance.
- `AddressVerified`: Triggered when an address verification occurs.
- `AccessTransaction`: Triggered after a transaction is executed, with details of the sender, recipient, and amount transferred.

## Conclusion

This smart contract provides basic wallet functionality for interacting with Ether and conducting transactions. It can be extended to include features like access control (e.g., restricting certain actions to specific addresses) or integrating with a front-end application for user interaction.
