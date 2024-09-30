# **Create Subnet and Deploy It: DeFi Empire Game**

## **Overview**
Welcome to **DeFi Empire**, a blockchain-based gaming experience inspired by the DeFi Kingdoms, built on the **Avalanche** network. In this game, players can collect, build, and engage in battles with digital assets. By participating in game activities, players can earn rewards while enjoying a seamless integration of decentralized finance and gaming.

This project is a **DeFi Kingdom clone** that merges **DeFi** and gaming to create a captivating universe for users.

---

## **Prerequisites**
Before starting, ensure you have the following tools and resources:

- A Unix-based computer (MacOS or Linux) or **WSL** installed on Windows
- **Remix Online IDE**
- **MetaMask Wallet** extension installed
- Web browser
- **Kali Linux** (preferred)

---

## **Project Steps**

### **1. Set Up Your EVM Subnet**
Create a custom **EVM subnet** on the **Avalanche Network**. Follow the instructions in the guide and refer to [Avalanche documentation](https://docs.avax.network/) for guidance. This step is essential for deploying your smart contracts with low fees and creating a custom in-game token.

### **2. Define Your Native Currency**
Develop your own native currency, which will serve as the in-game currency for your DeFi Kingdom clone. This currency will be used for:

- In-game transactions
- Rewards distribution
- General activities within the game

### **3. Connect to MetaMask**
To connect your EVM Subnet to **MetaMask**, follow the detailed instructions in the provided guide. Ensure you select your custom EVM subnet as the network in MetaMask to enable interaction with your deployed contracts.

### **4. Deploy Basic Building Blocks**
Using **Solidity** and **Remix**, deploy the foundational smart contracts for your game. These contracts will define critical game mechanics such as:

- Battling
- Exploring
- Trading

Example contracts are provided below for your reference.

### **5. Test Your Application**
Leverage **Remix** to interact with your deployed smart contracts and test game functionalities like:

- Deploying tokens
- Setting up liquidity pools
- Simulating game actions such as battling, exploring, and trading

Ensure all game components function as expected within your DeFi Kingdom clone.

---

## **Smart Contracts**

### **ERC20 Token Contract**
This contract implements a standard **ERC20 token** with features such as transfers, allowances, minting, and burning. Players will use this token as the in-game currency.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ERC20 {
    uint public totalSupply;
    string public name = "ARYAN";
    string public symbol = "AYN";
    uint8 public decimals = 18;

    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address to, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint amount) external returns (bool) {
        allowance[from][msg.sender] -= amount;
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        emit Transfer(from, to, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### **Vault Contract**
The **Vault contract** allows users to deposit and withdraw tokens while keeping track of the total supply and individual balances. This is crucial for managing liquidity within the game.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

---

## **Usage**
To begin your DeFi Kingdom adventure:

1. Set up your **EVM Subnet** using the provided guide and Avalanche documentation.
2. Define your native currency for in-game use.
3. Connect your EVM Subnet to MetaMask.
4. Deploy the foundational smart contracts using Remix.
5. Test your game functionalities by interacting with deployed contracts via Remix.

---

## **Author**
**Rajat Bodh**

---

## **License**
This project is licensed under the **MIT License**.
