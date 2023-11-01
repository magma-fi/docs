# Fees

### Introduction

The **Magma** protocol is designed with flexibility and adaptability at its core, allowing it to accommodate the varying needs of its users. \
\
The protocol offers two types of fees: &#x20;

* Fixed fees ([Minting fee](fees.md#1.-minting-fees) and [Redemption fee](fees.md#2.-redemption-fees))
* [Borrowing interes](fees.md#3.-borrow-interest-rate)[t rate](fees.md#3.-borrow-interest-rate)

The aim of these fees is to maintain the stability and liquidity of the protocol while also providing incentives for user participation.

Both fixed fees and continuous interest can be adjusted by the governance of the **Magma** Protocol. This means that the Magma community can vote on the parameters of these fees for each collateral type, allowing the protocol to adapt to changing market conditions and ensuring that it continues to offer competitive rates for its users.

### 1. Minting Fees

Minting fees are one-time fees charged at the moment of borrowing. In the **Magma** protocol, this fee is applied when a user borrows `ioUSD` against their collateral.

Every time `ioUSD` is withdrawn from an vault, a minting fee is charged on the drawn amount and added to the debt. Please note that the minting fee is variable (and determined algorithmically), with a minimum value of `0.5%` under normal operation. The fee is `0%` during [`Recovery Mode`](broken-reference). \
A `10 ioUSD` `Liquidation Reserve` charge will be applied as well, but returned upon repayment of debt.

#### **Minting fee calculation**

The minting fee is added to the debt of the vault and is determined by a `baseRate`. The fee rate is confined to a range between `0.5%` and `5%` and is multiplied by the amount of liquidity drawn by the borrower.

{% hint style="info" %}
**Example:**

The borrowing fee stands at `0.5%` and the borrower wants to receive `4,000 ioUSD` to their wallet. Being charged a borrowing fee of `20.00 ioUSD`, the borrower will incur a debt of `4,220 ioUSD` after the Liquidation Reserve and mint fee are added.
{% endhint %}

### 2. Redemption fees

In Magma, redemptions are transactions where users can exchange their `ioUSD` for a collateral of their choice at face value, subject to a fee known as the Redemption Fee. The purpose of this fee is to protect the system during periods of low collateral ratio by disincentivizing redemptions and ensuring the system remains solvent. \
\
You can find more detail on how it's calculated [here](redemption-mechanism.md#redemption-fee).

### 3. Borrow Interest Rate

Borrow Interest Rate is a fee that accrues over time on outstanding debt. This is common in most lending protocols and serves as a source of revenue that helps maintain the protocol's operations. In Magma, this fee is charged on the `ioUSD` borrowed against the collateral and the DAO will have a say concerning its parameters.

#### Interest Calculation

Interest accrues to all borrowers in a market when any blockchain wallet address interacts with the protocol contracts, calling one of these functions: `liquidateaVault`, `batchLiquidateVault`, `redeemCollateral`, `openVault`, `addColl`, `withdrawColl`, `repayDebt`, `adjustVault`, `closeVault`.&#x20;

Successful execution of one of these functions triggers the `_accrueActiveInterests` method, which causes interest to be added to the underlying balance of every borrower in the market. Interest accrues for the current block, as well as each prior block in which the `_accrueActiveInterests` method was not triggered (no user interacted with the `Vault Manager` contract). Interest compounds only during blocks in which one of the aforementioned methods is invoked.

#### **Example of interest accrual:**

* Alice mints `10,000 ioUSD` from the Magma protocol.&#x20;
* At the time of supply, the `interestRate` is `0.00000031709792 per second`.&#x20;
* No one interacts with the `Vault Manager` contract for 100 seconds.&#x20;
* Subsequently, Bob borrows some `ioUSD`.&#x20;
* Alice’s underlying debt is now `10,000.317097919837646 ioUSD` (which is `0.00000031709792 times 100 seconds times the principal`, plus the original `10,000 ioUSD`).&#x20;
* Alice’s underlying `ioUSD` balance in subsequent blocks will have interest accrued based on the new value of `10,000.317097919837646 ioUSD` instead of the initial `10,000 ioUSD`.

### Interest Rate Index and Borrowing Balance

#### **Implementation**

Each time a transaction occurs, the Interest Rate Index for the asset is updated to compound the interest since the prior index. This is done using the interest for the period, denominated by $$r * t$$ , calculated using a per-second interest rate. The formula for this is:

$$
activeInterestIndex_{c,n} = activeInterestIndex_{c,(n-1)} * (1 + r * t)
$$

In this formula:

*   $$activeInterestIndex_{c,n}$$ represents the Interest Rate Index for collateral type $$c$$

    &#x20;at time $$n$$
* $$r$$ represents the per-second interest rate
* $$t$$ represents the time period since the last index update

The market's total borrowing outstanding is also updated to include the interest accrued since the last index. The formula for this update is:

$$
totalDebt_{c,n} = totalDebt_{c,(n-1)} * (1 + r * t)
$$

In this formula:

* $$totalDebt_{c,n}$$ represents the total debt balance for collateral type $$c$$ at time $$n$$

Each individual `vault` tracks the `activeInterestIndex` at the time of its last debt change.

The current vault debt at any time is then:

$$
currentVaultDebt = \frac{vault.debt * activeInterestIndex}{vault.activeInterestIndex}
$$

#### **Limitations**

When calculating interest in `_accrueActiveInterests` simple interest is applied over the seconds since the last update. This will underestimate the amount of interest that would be calculated if it were compounded every second.

The code is designed to accrue interest as frequently as possible. Additionally, the size of the discrepancy between the computed and theoretical interest will depend on the volume of transactions being handled by the **Magma** protocol, which may change unpredictably.
