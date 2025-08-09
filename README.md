---

# 🏦 Work Yield Lending Vault

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Network](https://img.shields.io/badge/network-Plume-blue)
![Contract Status](https://img.shields.io/badge/status-Deployed-success)
![Version](https://img.shields.io/badge/version-1.0.0-orange)

*A decentralized lending platform where **WYT** (Work Yield Token) holders can borrow **pUSD** against their collateral. Includes a liquidation system to ensure solvency and is funded by protocol revenue + borrower interest, creating a self-sustaining loop.*

---

## 📌 TL;DR

* 💰 **Deposit WYT → Borrow pUSD** without selling your assets
* 📈 Flexible borrowing with **variable interest rate**
* 🔄 **Protocol-funded liquidity** from revenue + recycled interest
* 🛡 **Liquidation system** to protect lenders & protocol
* 📊 On-chain health checks using **WYT’s intrinsic backing value** for safe LTV

---

## ✨ Key Features

* **Borrow Against Your Assets** — Lock WYT as collateral for instant pUSD liquidity.
* **Protocol-Funded Liquidity** — Pool funded by protocol revenue (e.g., WYT redemption fees) and borrower interest.
* **On-Chain Health & Liquidations** — Uses WYT’s **intrinsic backing value** for collateral pricing; liquidations are triggered when loans become unsafe.

---

## 🔍 How It Works

```
1️⃣ Deposit WYT → Locked as collateral
2️⃣ Borrow pUSD → Up to Loan-to-Value (LTV) limit
3️⃣ Repay Loan → Principal + accrued interest
4️⃣ Liquidation → If collateral value falls below threshold, liquidators repay debt and receive collateral + bonus
```

### Architecture Diagram

```
        +------------------------+
        |  WorkYield Protocol    |
        | (redemption fees)      |
        +-----------+------------+
                    |
                    v
            fundVault(pUSD)
                    |
        +-----------v------------+
        |   Lending Vault         |
        |   (pUSD Liquidity)      |
        +-----+-----------+-------+
              |           |
   deposit(WYT)    borrow/repay(pUSD)
              |           |
              v           v
        +---------+  +-----------+
        |  User   |  | Liquidator|
        +---------+  +-----------+
```

---

## 🛠 Vault Funding

* **Interest Payments** → Returned directly to the liquidity pool
* **Protocol Revenue** → Admins call `fundVault(pUSDAmount)` to add pUSD from other sources (e.g., redemption fees)

---

## 📜 Contract Addresses

| Contract              | Address                                      |
| --------------------- | -------------------------------------------- |
| WorkYieldLendingVault | `0x2D93a04ccE60697Eb35Ae4C62A0854481DB55e5a` |
| WYT (ERC-20)          | `0xccF4eaa301058Ec5561a07Cc38A75F47a2912EA5` |
| pUSD (ERC-20)         | `0xdddD73F5Df1F0DC31373357beAC77545dC5A6f3F` |

---

## 💻 Frontend dApp

The **index.html** file lets users:

* 🔗 **Connect Wallet** (Plume Network)
* 📊 **View Balances** — pUSD, WYT, collateral, active loans
* 📥 **Manage Collateral** — Deposit / withdraw WYT
* 💵 **Borrow & Repay** — Take or repay loans
* 🛠 **Liquidator Zone** — For authorized liquidators to act on unhealthy positions
* 🏛 **Admin Panel** — Manage risk parameters, fund vault

---

## ⚙️ Risk Parameters (Admin Controlled)

| Parameter              | Description                         | Example    |
| ---------------------- | ----------------------------------- | ---------- |
| `loanToValueRatio`     | Max borrow vs. collateral value     | 5000 (50%) |
| `interestRate`         | Annual interest rate                | 500 (5%)   |
| `liquidationThreshold` | LTV at which a loan is liquidatable | 8000 (80%) |
| `liquidationBonus`     | Collateral bonus for liquidators    | 500 (5%)   |

> 🛡 **Security Note:** All calculations use **backing value** of WYT, not market price, to prevent manipulation.

---

## 📚 Functions

### **User Functions**

```solidity
depositCollateral(uint256 amount)
withdrawCollateral(uint256 amount)
borrow(uint256 pUSDAmount)
repay(uint256 pUSDAmount)
```

### **Liquidator Functions**

```solidity
liquidate(address borrowerAddress)
```

### **Admin Functions**

```solidity
fundVault(uint256 pUSDAmount)
withdrawVaultFunds(uint256 amount)
setLoanToValueRatio(uint256 ltvBps)
setInterestRate(uint256 rateBps)
setLiquidationThreshold(uint256 thresholdBps)
setLiquidationBonus(uint256 bonusBps)
pause()
unpause()
```

---

## 🔐 Security & Notes

* **Over-collateralization** + liquidation ensures solvency
* **Backing-based pricing** reduces oracle manipulation risk
* **Role-gated functions** for admin & liquidators
* **Pausable** in emergencies

---

## 📄 License

MIT

---
