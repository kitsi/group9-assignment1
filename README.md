Encode Solidity Bootcamp Group 9, HW 1

Due: May 1, 2023

Group Members: Kit Sidhu, Hussam Alsaliti, Zachary Halaby, Xuelan Wu

---

### 1. Change Message Strings

#### a) hard coding

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract HelloWorld {
    string private text;

    constructor() {
        text = "Hello World!";
    }

    function helloWorld() public view  returns (string memory)  {
        return text;
    }
}
```

The simplest way to change the message in the above code is to change the hard coded text on line 8 to something different ex, `text = "Hi Everyone"`

| Transaction hash                                                   | Status  |
| ------------------------------------------------------------------ | ------- |
| 0x3d342c7de2bf5773ba4285e72e471e2c8da31735ee4a8074419bc06ea00b3aef | Success |

#### b) using setter

```solidity
contract HelloWorld {
    string private text;

    constructor() {
        text = "Hello World";
    }

    function helloWorld() public view  returns (string memory)  {
        return text;
    }

    function setText(string memory newText) public {
        text = newText;
    }

```

Changed text to new text using setText function

| Transaction hash                                                   | Status  |
| ------------------------------------------------------------------ | ------- |
| 0xd1f32e3bc37d4584e66c7420eaa89ca7e8a0a2d4271dbf2db75bbcf2ff8fa15d | Success |

### 2. Change Owners

#### a) hard coding

```

```

This example adds a require method within the text setter, hard coding the address. If a different address attempts to change the text, it will fail.

| Transaction hash                                                   | Status  |
| ------------------------------------------------------------------ | ------- |
| 0x3d342c7de2bf5773ba4285e72e471e2c8da31735ee4a8074419bc06ea00b3aef | Success |
| 0x114fae8f4fbc4a6a02cff5d80f251a061edae589e6d5f400c0e370f2b344a915 | Fail    |

#### b) using constructor

```solidity
contract HelloWorld {
    string private text;
    address public owner;

    constructor (address desiredOwner) {
        owner = desiredOwner;
        text = "Hello World";
    }

    function helloWorld() public view returns (string memory) {
        return text;
    }

    function setText(string calldata newText) public {
        require (msg.sender == owner, "Not the owner");
        text = newText;
    }
}
```

| Transaction hash                                                   | Status  |
| ------------------------------------------------------------------ | ------- |
| 0xc1fb4f3999f64a98a2dd3045384c46b4f1d20875218e13c0e5783f49e279f626 | Success |
| 0x2e4b443c8bc080f55010146207a0e5bbcb522921f8e6fd757e9e7490933eb09e | Fail    |

First the owner is set in the constructor upon deployment to the address of the person deploying the contract and it succeeds when the text is set. Next, a different address is used to deploy the contract and when the person deploying the contract tried to set the text, it fails.

#### c) using modifier

```solidity
contract HelloWorld {
    string private text;
    address public owner;

    constructor() {
        text = "Hello World";
        owner = msg.sender;
    }

    function helloWorld() public view returns (string memory) {
        return text;
    }

    function setText(string calldata newText) public onlyOwner {
        text = newText;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        owner = newOwner;
    }

    modifier onlyOwner()
    {
        require (msg.sender == owner, "Caller is not the owner");
        _;
    }
}
```

This example uses a modifier that can be reused for any number of functions to automate the restriction. Here the modifier is used in the setText function so that only the owner can set the text.

| Transaction hash                                                   | Status  |
| ------------------------------------------------------------------ | ------- |
| 0x9b4e84c4f3aab9f2eb7f527d1658cca3331cd5f140a58c5b2c4ba34e59bf5a5f | Success |
| 0x0e666f9c17ed0b262b432c2c823a392654ab8937c6b690904aa9729f871b1344 | Fail    |

---

Assignment:

- Interact with “HelloWorld.sol” within your group to change message strings and change owners
- Write a report with each function execution and the transaction hash, if successful, or the revert reason, if failed
