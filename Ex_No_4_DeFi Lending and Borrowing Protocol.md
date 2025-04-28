# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:

Step 1: Users deposit ETH into the smart contract to earn interest over time.

Step 2: Users can borrow ETH by providing enough collateral (at least 150% of the borrowed amount).

Step 3: The contract checks that collateral is sufficient before allowing the loan.

Step 4: Interest is calculated dynamically based on how much ETH is borrowed compared to total deposits.

Step 5: If a borrower’s collateral value drops below the safe level (liquidation threshold), they can be liquidated.

Step 6: Liquidators can repay a borrower's debt and claim their collateral to maintain system stability.



# Program:
#### Developed by: SARON XAVIER A
#### Register number: 212223230197
#### Date: 28/04/2025

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Not enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```

# Output :

### Borrow
![image](https://github.com/user-attachments/assets/b29134ba-4d5d-4a0e-b8eb-8d892db84adb)


### Liquidate
![image](https://github.com/user-attachments/assets/690e61c9-baec-4738-874c-1a5daa0030f7)


### ReduceCollateral
![image](https://github.com/user-attachments/assets/e1170c60-31ac-4903-be9c-aa79bed4a43d)


### Borrowed
![image](https://github.com/user-attachments/assets/dfaeb569-1c3b-4742-bcd0-2e2cbdcabc94)


### Collateral
![image](https://github.com/user-attachments/assets/373d8856-a6ba-421a-b7bb-fde197ebc2df)


### Deposits
![image](https://github.com/user-attachments/assets/c61aa31c-1ce0-453a-868f-92046436ae9a)


### Interest rate
![image](https://github.com/user-attachments/assets/0b17f35f-9ae6-48bb-9d9a-10b9453624d5)


### Liquidation threshold call
![image](https://github.com/user-attachments/assets/e9c5f4db-bb33-4d51-a1bb-db64e8434c81)


### Owner
![image](https://github.com/user-attachments/assets/e8707256-83ca-4d69-9712-1f0769444c8c)


# RESULT : 

Thus, to build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi is executed successfully.
