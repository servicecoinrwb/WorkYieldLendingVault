Work Yield Lending Vault
A decentralized lending platform where WYT (Work Yield Token) holders can borrow pUSD against their positions. The vault includes a liquidation mechanism to ensure solvency and is funded by protocol revenue and borrower interest, creating a self-sustaining loop.

TL;DR
Deposit WYT → Borrow pUSD without selling your assets.

Flexible borrowing with a clear, variable interest rate.

Protocol-funded liquidity via revenue and recycled interest payments.

Liquidation system to maintain protocol health and protect lenders.

On-chain health checks using WYT's intrinsic backing value for safe LTV.

Key Features
Borrow Against Your Assets: Lock WYT as collateral to get instant pUSD liquidity.

Protocol-Funded Liquidity: The vault is funded by protocol revenue (e.g., from WYT redemptions) and interest payments that are recycled back into the lending pool.

On-Chain Health & Liquidations: The protocol uses WYT’s intrinsic backing value for collateral pricing, ensuring loans remain safely over-collateralized. A built-in liquidation system allows authorized liquidators to repay unhealthy debt in exchange for collateral, securing the vault.

How It Works
Deposit Collateral
Users deposit WYT into the vault, where the tokens are locked as collateral.

Borrow pUSD
Borrow up to the configured Loan-to-Value (LTV) ratio of your WYT collateral's value.

Repay Loan
Repay the principal plus any accrued interest at any time to unlock your WYT collateral.

Liquidation
If a borrower's collateral value drops and their position becomes unhealthy (i.e., exceeds the liquidationThreshold), an authorized liquidator can repay the debt. In return, they receive the borrower's collateral plus a bonus, ensuring the vault remains solvent.

Vault Funding

Interest Payments → Returned directly to the vault's liquidity pool.

Protocol Revenue → Admins can call fundVault(pUSDAmount) to add external funds (e.g., from WYT redemption fees).

Contracts
WorkYieldLendingVault: 0x2D93a04ccE60697Eb35Ae4C62A0854481DB55e5a

WYT (ERC-20): 0xccF4eaa301058Ec5561a07Cc38A75F47a2912EA5

pUSD (ERC-20): 0xdddD73F5Df1F0DC31373357beAC77545dC5A6f3F

Frontend dApp
The index.html file provides a user-friendly interface to:

Connect Wallet (Plume Network).

View balances (pUSD, WYT), deposited collateral, and current loan details.

Manage Collateral: Deposit and withdraw WYT.

Borrow & Repay: Take out or pay back pUSD loans.

Liquidator Zone: A dedicated section for authorized liquidators to act on unhealthy positions.

Admin Panel: For authorized admins to manage risk parameters and fund the vault.

Risk Parameters (Admin Controlled)
Parameter

Description

Example Value

loanToValueRatio

Max borrow amount vs. collateral value.

5000 (50%)

interestRate

The annual interest rate for all loans.

500 (5%)

liquidationThreshold

The LTV at which a loan is considered unhealthy.

8000 (80%)

liquidationBonus

The collateral bonus awarded to liquidators.

500 (5%)

The protocol intentionally uses the backing value of WYT (not its market price) for all calculations to prevent price manipulation risk.

Admin & Public Functions
User Functions
depositCollateral(amount)

withdrawCollateral(amount)

borrow(pUSDAmount)

repay(pUSDAmount)

Liquidator Functions
liquidate(borrowerAddress)

Admin Functions
fundVault(pUSDAmount)

withdrawVaultFunds(amount)

setLoanToValueRatio(ltvBps)

setInterestRate(rateBps)

setLiquidationThreshold(thresholdBps)

setLiquidationBonus(bonusBps)

pause() / unpause()

Security & Notes
The protocol is secured by over-collateralization and a liquidation mechanism.

Pricing is based on WYT's intrinsic backing value to reduce oracle manipulation risk.

Critical functions are role-gated to admins and liquidators.

The contract is pausable by admins for emergency response.

External audits are recommended before a mainnet launch.

License
MIT
