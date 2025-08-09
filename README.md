# Work Yield Lending Vault

*A decentralized lending platform where WYT (Work Yield Token) holders borrow **pUSD** against their positions. Liquidity is funded by protocol revenue and borrower interest, creating a self-sustaining loop.*

---

## TL;DR

* **Deposit WYT → Borrow pUSD** without selling
* **Fixed terms** (e.g., 30-day & 90-day) with predictable APR
* **Protocol-funded liquidity** via redemption fees + interest
* **On-chain health** using intrinsic WYT backing for safe LTV

---

## Key Features

* **Borrow Against Your Assets**: Lock WYT as collateral; get instant pUSD liquidity.
* **Fixed-Term Loans**: Predefined terms (e.g., **30d** short-term, **90d** long-term) with fixed APRs.
* **Protocol-Funded Liquidity**: Vault funded by **protocol revenue** (e.g., WYT redemptions) + **interest recycled** back into the pool.
* **On-Chain Health Checks**: Uses WYT’s **backing value** for pricing; loans stay safely **over-collateralized**.

---

## How It Works

1. **Deposit Collateral**
   Users deposit **WYT** to the vault. Tokens are locked as collateral.

2. **Borrow pUSD**
   Borrow up to the configured **LTV** of your WYT collateral. Choose a loan term (e.g., **30d**/**90d**) with its APR.

3. **Repay Loan**
   Repay principal **+** accrued interest to unlock your WYT.

4. **Vault Funding**

   * **Interest Payments** → returned directly to the vault.
   * **Protocol Revenue** → admins can call `fundVault(pUSDAmount)` (e.g., from WYT redemption fees).

**Health model:**

```
collateralValue = WYT_amount * backingPrice()  // from WYT’s intrinsic backing model
maxBorrow      = collateralValue * LTV
healthFactor   = (collateralValue * LTV_buffer) / currentDebt
```

---

## Architecture (at a glance)

```
+---------------------+         fundVault(pUSD)
|  WorkYield Protocol | ------------------------------+
|  (redemption fees)  |                               |
+----------+----------+                               v
           |                                    +-----------+
           |                                    | Lending   |
           |  price/backing                     |  Vault    |
           +----------------------------------> |  (pUSD)   |
                                               +-----+-----+
                                                     |
   deposit(WYT) / withdraw(WYT)                       | borrow/repay(pUSD)
       +----------------------------------------------+------------------+
       |                                                                 |
       v                                                                 v
+--------------+                                               +------------------+
|  User (WYT)  | <--------- unlock WYT on repay -------------- |  User (pUSD)     |
+--------------+                                               +------------------+
```

---

## Contracts

> Replace with your live addresses once deployed.

* **WorkYieldLendingVault**: `0xCcDc74D8D4a3a54e3bB65FBcE6733f4c2317e340`
* **WYT (ERC-20)**: `0xccF4eaa301058Ec5561a07Cc38A75F47a2912EA5`
* **pUSD (ERC-20)**: `0xdddD73F5Df1F0DC31373357beAC77545dC5A6f3F`

Interfaces used:

```solidity
interface IERC20 {
  function balanceOf(address a) external view returns (uint256);
  function allowance(address o, address s) external view returns (uint256);
  function approve(address s, uint256 v) external returns (bool);
  function transfer(address to, uint256 v) external returns (bool);
  function transferFrom(address f, address t, uint256 v) external returns (bool);
  function decimals() external view returns (uint8);
}
```

---

## Frontend Quickstart

**index.html** gives a simple UI to:

* **Connect Wallet** (Plume)
* See **pUSD/WYT balances**, deposited collateral, current loan
* **Manage Collateral** (deposit/withdraw WYT)
* **Borrow / Repay** pUSD
* **Admin** tab for funding + risk settings (admin-only)

### Connect Flow

1. Click **Connect Wallet** (Plume).
2. UI loads: **pUSD**, **WYT**, **your collateral**, **your loan**, **vault pUSD liquidity**.

### Typical Actions

* **Deposit WYT** → `approve(WYT, vault)` then `depositCollateral(amount)`.
* **Borrow pUSD** → choose term (30/90 days), confirm APR, `borrow(amount, termId)`.
* **Repay** → `repay(amount)` (principal + interest) → **withdraw** WYT (optional).
* **Admin** → `fundVault(pUSD)`, adjust LTV/APR/terms, `pause()`/`unpause()` if needed.

---

## Risk Parameters

| Parameter           | Description                                | Example             |
| ------------------- | ------------------------------------------ | ------------------- |
| `loanToValueRatio`  | Max borrow vs. collateral backing value    | 60%                 |
| `terms[]`           | Fixed terms with duration + APR            | 30d @ 8%, 90d @ 12% |
| `liquidationBuffer` | Health buffer above LTV (no auctions here) | 10%                 |
| `maxVaultUtil`      | Optional cap on utilization                | 90%                 |
| `backingOracle`     | WYT intrinsic backing price source         | WYT view fn         |

> The protocol intentionally uses **backing value** (not market price) to avoid manipulation.

---

## Math (simple)

* **Borrow limit**: `maxBorrow = collateralBacking * LTV`
* **Interest (fixed-term)**:

  ```
  interest = principal * APR * (days/365)
  payoff   = principal + interest
  ```
* **Health factor** (informal):

  ```
  health = (collateralBacking * LTV_buffer) / currentDebt
  must stay > 1
  ```

---

## Example (ethers.js)

```js
import { ethers } from "ethers";
const vault = new ethers.Contract(VAULT_ADDR, VAULT_ABI, signer);
const wyt   = new ethers.Contract(WYT_ADDR,   ERC20_ABI,  signer);
const pUSD  = new ethers.Contract(PUSD_ADDR,  ERC20_ABI,  signer);

// 1) Deposit 1,000 WYT
await wyt.approve(VAULT_ADDR, ethers.utils.parseUnits("1000", 18));
await vault.depositCollateral(ethers.utils.parseUnits("1000", 18));

// 2) Borrow 500 pUSD on 30-day term (termId = 0)
await vault.borrow(ethers.utils.parseUnits("500", 18), 0);

// ... time passes ...

// 3) Repay full payoff
const payoff = await vault.currentPayoff(await signer.getAddress());
await pUSD.approve(VAULT_ADDR, payoff);
await vault.repay(payoff);

// 4) Withdraw 1,000 WYT
await vault.withdrawCollateral(ethers.utils.parseUnits("1000", 18));
```

---

## Admin Guide

* **Fund the Vault**

  ```solidity
  function fundVault(uint256 pUSDAmount) external onlyRole(DEFAULT_ADMIN_ROLE)
  ```

  Transfer **pUSD** into the vault’s liquidity pool.

* **Manage Risk**

  ```solidity
  function setLoanToValue(uint256 bps) external onlyRole(RISK_ROLE)
  function setTerm(uint8 id, uint32 days_, uint256 aprBps) external onlyRole(RISK_ROLE)
  function setOracle(address backingOracle) external onlyRole(RISK_ROLE)
  ```

* **Circuit Breakers**

  ```solidity
  function pause() external onlyRole(PAUSER_ROLE)
  function unpause() external onlyRole(PAUSER_ROLE)
  ```

* **Roles**

  * `DEFAULT_ADMIN_ROLE`: full control, can grant/revoke roles
  * `RISK_ROLE`: risk parameters & oracle
  * `PAUSER_ROLE`: pause/unpause

---

## Public API (typical)

**User**

* `depositCollateral(uint256 amount)`
* `withdrawCollateral(uint256 amount)` *(checks debt safety)*
* `borrow(uint256 pUSDAmount, uint8 termId)`
* `repay(uint256 pUSDAmount)`
* Views: `collateralOf(address)`, `debtOf(address)`, `currentPayoff(address)`, `availableLiquidity()`, `backingPrice()`, `maxBorrowable(address)`

**Admin**

* `fundVault(uint256 pUSDAmount)`
* `setLoanToValue(uint256 bps)`
* `setTerm(uint8 id, uint32 days_, uint256 aprBps)`
* `setOracle(address)`
* `pause() / unpause()`

**Events**

* `CollateralDeposited(user, amount)`
* `CollateralWithdrawn(user, amount)`
* `Borrowed(user, amount, termId, aprBps, dueAt)`
* `Repaid(user, amount, interest)`
* `VaultFunded(sender, amount)`
* `RiskParamsUpdated(...)`
* `Paused / Unpaused(account)`

---

## Getting Started (Dev)

### Prereqs

* Node 18+, pnpm/yarn/npm
* Hardhat (or Foundry)
* RPC + private key for **Plume**

### Install

```bash
pnpm i
# or: yarn / npm i
```

### Env

Create `.env`:

```
PRIVATE_KEY=0x...
RPC_URL=https://rpc.plume.org
```

### Build & Test

```bash
pnpm hardhat compile
pnpm hardhat test
```

### Deploy

```bash
pnpm hardhat run scripts/deploy.ts --network plume
```

> Populate constructor args: `WYT`, `pUSD`, initial `LTV`, default terms, oracle, roles.

---

## Security & Notes

* Uses **over-collateralization** with **backing-based pricing** to reduce manipulation.
* **No third-party liquidators**; users stay within limits or are blocked from risky actions.
* **Pausable** for incident response.
* **Role-gated** risk changes.
* Recommend external audits before mainnet deployment.

---

## Roadmap

* Optional **grace period** & late-fee schedule
* **Per-user term caps** / global utilization guards
* **Frontend**: richer charts & health factor visuals
* **Subgraph** for analytics + history

---

## License

MIT

---

## Credits

Built by the **Work Yield** team. Uses OpenZeppelin libraries and standard ERC-20 interfaces.
