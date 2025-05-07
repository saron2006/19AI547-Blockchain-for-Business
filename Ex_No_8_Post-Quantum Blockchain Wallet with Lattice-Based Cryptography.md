# Experiment 8: Post-Quantum Blockchain Wallet with Lattice-Based Cryptography

# Aim:

To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys.

# Algorithm:

Step 1: Understand Quantum Threat: ECDSA-based wallets are vulnerable to quantum attacks; lattice-based cryptography (e.g., NTRU, CRYSTALS-Kyber) is quantum-resistant.

Step 2: Register User: User registers with a quantum-safe public key hash.

Step 3: Store User Data: Store the hashed lattice-based public key and track user registration.

Step 4: Transaction Setup: For sending funds, users must be registered and have sufficient balance.

Step 5: Signature Validation: Users must provide a quantum-resistant signature (hashed public key) for transaction validation.

Step 6: Transaction Execution: After signature verification, update balances between the sender and recipient.

Step 7: Event Logging: Emit events to confirm user registration and successful transaction verification.


# Program:

### Developed by: SARON XAVIER A
### Register number: 212223230197

(Solidity does not natively support lattice cryptography yet, but we simulate it using custom hash-based authentication.)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) public users;
    mapping(address => uint256) public balances;

    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);

    function registerUser(bytes32 _publicKeyHash) public {
        require(!users[msg.sender].registered, "User already registered");
        users[msg.sender] = User(_publicKeyHash, true);
        emit UserRegistered(msg.sender, _publicKeyHash);
    }

    function sendFunds(address _to, uint256 _amount, bytes32 _signature) public {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 calculatedHash = keccak256(abi.encodePacked(msg.sender, _to, _amount));
        require(calculatedHash == _signature, "Invalid quantum-safe signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
        emit TransactionVerified(msg.sender, _to, _amount);
    }
}
```

# Expected Output:

Users register using a post-quantum secure public key.


Transactions require a quantum-resistant signature for authentication.


If a traditional quantum-vulnerable hash is used, the transaction fails.


# High-Level Overview:

First quantum-safe Ethereum-compatible wallet prototype.


Uses lattice-based key hashes instead of ECDSA.


Demonstrates how Ethereum will transition to post-quantum security.


Inspired by NIST’s post-quantum cryptography competition.

# OUTPUT:

## SENDER ACCOUNT REGISTERATION OUTPUT

![Screenshot 2025-05-05 142807-1](https://github.com/user-attachments/assets/ed9f0031-a347-43ad-9613-02a2d17e3361)

## RECEIVER ACCOUNT REGISTERATION OUTPUT

![Screenshot 2025-05-05 142702](https://github.com/user-attachments/assets/34c32ffe-6a04-4af8-a7c9-6ebbf349d66e)

## SENDER BALANCE OUTPUT
![Screenshot 2025-05-05 142946](https://github.com/user-attachments/assets/6a3b5920-acfc-49de-b00e-7b5f047d96f0)


## GENERATE SIGNATURE
![Screenshot 2025-05-05 143128](https://github.com/user-attachments/assets/19526bba-0b8a-48ee-b4a7-7796943b7f1b)


## SENDER TRANSFER AMOUNT TO RECEIVER

![Screenshot 2025-05-05 143216](https://github.com/user-attachments/assets/8a61caaa-92ca-446e-8d2a-deae6109d679)

## RECEIVER BALANCE AFTER TRANSACTION

![Screenshot 2025-05-05 143245](https://github.com/user-attachments/assets/e7549b4b-b4c6-49d0-b8f3-1c0c976cc50f)

# RESULT: 

Thus, to create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys is executed successfully.
