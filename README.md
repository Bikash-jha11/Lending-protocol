Solana Lending Protocol
This repository contains a modular, decentralized lending protocol built using the Anchor Framework on Solana. 
It facilitates peer-to-pool lending, allowing users to earn interest on deposits and access liquidity through overcollateralized loans.

🏗 System Architecture:
The protocol is designed for high performance and safety, utilizing a modular approach to separate concerns and minimize the attack surface.

Core Modules:
1.deposit.rs: Handles the logic for users providing liquidity. It mints "cTokens" (collateral tokens) representing the user's share of the pool plus accrued interest.
2.withdraw.rs: Manages the redemption of cTokens for the underlying assets, ensuring the pool maintains enough liquidity to satisfy the request.
3.borrow.rs: Implements the debt creation engine. It validates the user’s Loan-to-Value (LTV) ratio and ensures they have sufficient collateral locked before issuing funds.
4.repay.rs: Processes the return of borrowed assets. It calculates the principal and interest owed since the last interaction to clear the debt.
5.liquidate.rs: The protocol’s safety valve. If a borrower's collateral value falls below the Liquidation Threshold, this module allows third-party liquidators to repay the debt in exchange for a discounted portion of the collateral.
6.admin.rs: Contains privileged instructions for the protocol DAO or multisig to update risk parameters, such as interest rate curves and fee structures.
7.mod.rs: The central hub that declares and exports the above sub-modules for the main program entry point.

📈 Technical Specifications:
Interest Rate Model:The protocol utilizes a jump rate model where interest increases linearly with utilization until a "kink" point, after which it increases exponentially to discourage 100% utilization.
Risk Management Overcollateralization: Users must provide more value in collateral than they intend to borrow.Oracle Integration: Price feeds are pulled via Pyth or Switchboard to ensure real-time valuation of assets.
Health Factor: A calculated metric $(H)$ where $H > 1$ is required to avoid liquidation.


🛠 Development & Deployment:
Prerequisites --> Rust v1.75+Solana CLI v1.18+
Anchor Framework v0.29.0 + SetupSync 
Dependencies:Bashanchor build

Configure Provider: 
Update Anchor.toml with your local or devnet wallet path. 
Run Tests:Bashanchor test


🔒 Security Considerations
Reentrancy: Inherently handled by Solana's architecture, but additional state checks are implemented.
Price Manipulation: Protected by using weighted moving average oracles.
Audit Status: This code is currently unaudited. Use at your own risk.
