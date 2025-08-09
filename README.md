Work Yield Lending Vault
A decentralized lending platform that allows users to borrow pUSD by using their WYT (Work Yield Token) as collateral. This vault is funded by protocol revenue and interest payments, creating a self-sustaining financial ecosystem.

Key Features
Borrow Against Your Assets: WYT holders can deposit their yield-generating tokens as collateral to get instant liquidity (pUSD) without having to sell.

Fixed-Term Loans: The platform offers clear, predictable loan options with fixed APRs, such as a 30-day short-term loan and a 90-day long-term loan.

Protocol-Funded Liquidity: The vault is funded by the protocol itself, using revenue from the main Work Yield platform (e.g., redemption fees) and interest paid by borrowers.

On-Chain Health Checks: The system uses an intrinsic price model based on the WYT contract's backing to ensure all loans remain safely over-collateralized.

How It Works
The Work Yield Lending Vault operates as a "protocol-as-lender" model, creating a two-sided market that benefits both WYT holders and the protocol itself.

1. Deposit Collateral
A user who holds WYT can deposit their tokens into the vault. This WYT is locked as collateral, securing their borrowing position.

2. Borrow pUSD
Once collateral is deposited, the user can borrow pUSD against its value, up to the protocol's Loan-to-Value (LTV) ratio. The user can choose from predefined loan terms, each with a different duration and APR.

3. Repay Loan
To reclaim their WYT collateral, the user must repay the pUSD they borrowed plus any interest that has accrued during the loan term.

4. Vault Funding
The pUSD available for borrowing comes from two primary sources, creating a sustainable financial loop:

Interest Payments: All interest paid by borrowers is returned directly to the vault.

Protocol Revenue: The platform administrator can use the fundVault function to deposit pUSD earned from other platform activities (such as redemption fees from the main WYT contract).

Getting Started with the Frontend
The index.html file provides a user-friendly interface for interacting with the lending vault.

Connect Wallet: Click the "Connect Wallet" button to connect to the Plume network.

View Balances: Once connected, your pUSD, WYT, deposited collateral, and current loan balances will be displayed. You can also see the total pUSD available for borrowing in the vault.

Manage Collateral: Use the "Manage Collateral" tab to deposit or withdraw your WYT tokens.

Borrow & Repay: Use the "Borrow pUSD" and "Repay Loan" tabs to take out new loans or pay back existing ones.

Admin Functions
The contract owner (DEFAULT_ADMIN_ROLE) has several key capabilities:

Fund the Vault: Use the fundVault function to add pUSD liquidity to the contract.

Manage Risk Parameters: Adjust the loanToValueRatio, interestRate, and other parameters.

Pause/Unpause: Halt key contract functions in case of an emergency.Work Yield Lending Vault
A decentralized lending platform that allows users to borrow pUSD by using their WYT (Work Yield Token) as collateral. This vault is funded by protocol revenue and interest payments, creating a self-sustaining financial ecosystem.

Key Features
Borrow Against Your Assets: WYT holders can deposit their yield-generating tokens as collateral to get instant liquidity (pUSD) without having to sell.

Fixed-Term Loans: The platform offers clear, predictable loan options with fixed APRs, such as a 30-day short-term loan and a 90-day long-term loan.

Protocol-Funded Liquidity: The vault is funded by the protocol itself, using revenue from the main Work Yield platform (e.g., redemption fees) and interest paid by borrowers.

On-Chain Health Checks: The system uses an intrinsic price model based on the WYT contract's backing to ensure all loans remain safely over-collateralized.

How It Works
The Work Yield Lending Vault operates as a "protocol-as-lender" model, creating a two-sided market that benefits both WYT holders and the protocol itself.

1. Deposit Collateral
A user who holds WYT can deposit their tokens into the vault. This WYT is locked as collateral, securing their borrowing position.

2. Borrow pUSD
Once collateral is deposited, the user can borrow pUSD against its value, up to the protocol's Loan-to-Value (LTV) ratio. The user can choose from predefined loan terms, each with a different duration and APR.

3. Repay Loan
To reclaim their WYT collateral, the user must repay the pUSD they borrowed plus any interest that has accrued during the loan term.

4. Vault Funding
The pUSD available for borrowing comes from two primary sources, creating a sustainable financial loop:

Interest Payments: All interest paid by borrowers is returned directly to the vault.

Protocol Revenue: The platform administrator can use the fundVault function to deposit pUSD earned from other platform activities (such as redemption fees from the main WYT contract).

Getting Started with the Frontend
The index.html file provides a user-friendly interface for interacting with the lending vault.

Connect Wallet: Click the "Connect Wallet" button to connect to the Plume network.

View Balances: Once connected, your pUSD, WYT, deposited collateral, and current loan balances will be displayed. You can also see the total pUSD available for borrowing in the vault.

Manage Collateral: Use the "Manage Collateral" tab to deposit or withdraw your WYT tokens.

Borrow & Repay: Use the "Borrow pUSD" and "Repay Loan" tabs to take out new loans or pay back existing ones.

Admin Functions
The contract owner (DEFAULT_ADMIN_ROLE) has several key capabilities:

Fund the Vault: Use the fundVault function to add pUSD liquidity to the contract.

Manage Risk Parameters: Adjust the loanToValueRatio, interestRate, and other parameters.

Pause/Unpause: Halt key contract functions in case of an emergency.
