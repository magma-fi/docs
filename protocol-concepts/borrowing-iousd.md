# Borrowing ioUSD

## Depositing and Borrowing

Anyone may obtain liquidity at any time in an entirely permissionless manner after depositing supported collateral into a Vault. At first, only IOTX can be used as collateral locked in Vaults and this allows Vault owners to withdraw up to 66.7% of its current dollar value in the form of ioUSD stablecoins. In other words, the Vault must always maintain a Minimum Collateral Ratio (MCR) of 150%, defined as the ratio of the current dollar value of the collateral to the withdrawn liquidity.&#x20;

Borrowers can repay or borrow more liquidity within the limits of the MCR whenever they wish. Within the same limit, they can retrieve their collateral. Moreover, a Vault can be topped up with more collateral as needed.

_The protocol imposes a minimum debt of 1,000 ioUSD. Therefore, Vaults can only be opened with an initial debt of at least 1,000 ioUSD and may never go below a debt of 1,000 ioUSD, unless fully repaid and closed._

## Vault

A Vault is where you open and maintain your loan. Vaults maintain two balances:&#x20;

1. An asset (IOTX) acting as collateral&#x20;
2. A debt denominated in ioUSD.

You can change the amount of each by adding collateral or repaying debt.

## Collateral Ratio (CR)

This is the ratio between the dollar value of the collateral in your Vault and its debt in ioUSD.&#x20;

The Collateral Ratio is the proportion of the total value of the Collateral to the total Value of the Borrowed amount. As ioUSD is deemed to be $1 in Magma, the total value of Borrowed amount is the same as the amount of ioUSD borrowed.

$$
CR = Collateral Value / Borrowed Value = Collateral Value / No.of ioUSD borrowed
$$

The collateral ratio of your Vault will fluctuate over time as the price of IOTX changes. A higher Collateral Ratio means that the borrower has more collateral relative to the borrowed amount, which reduces the risk for the protocol. Minimum collateral ratio for IOTX is set at 150%.&#x20;

## **Minimum Collateral Ratio (MCR)**

The Minimum Collateral Ratio (MCR) denotes the foundational debt-to-collateral ratio that, when maintained, prevents liquidation during standard operations, termed as Normal Mode.&#x20;

This parameter, integral to the protocol, is determined by governance for each specific collateral type. For instance, with an MCR set at `150%`, a vault holding a debt of `10,000` `ioUSD` requires a collateral valuation of no less than `$20,000` to safeguard against liquidation (a ratio of 200%).

To avoid being liquidated in [Recovery Mode](broken-reference) it's advisable to maintain a ratio well beyond `250%`, ideally around `300%`.

## Multiple Collateral

**Magma** supports multiple collateral types, therefore users can open multiple vaults, one for each supported collateral.\
\
It is important to note the following points:

1. Collateral types are segregated, therefore each collateral asset's Total Collateral Ratio (TCR) is calculated independently.&#x20;
2. Collateral asset types may have different protocol parameters, so at any given time using different collateral might be cheaper or more expensive than the others (see [Fees](broken-reference)).

## Liquidation Reserve

When a borrower opens a new Vault, an amount of 1 ioUSD is reserved and held back by the protocol as a compensation for the gas costs if the Vault needs to be liquidated at some point. The 1 ioUSD is added to the Vault’s debt, impacting its collateral ratio.

When a borrower closes their Vault, the Liquidation Reserve is refunded, i.e. the corresponding 1 ioUSD debt on the Vault is cancelled. The borrower therefore needs to pay back 1 ioUSD less to fully pay off their debt.

## Borrowing Fee

The protocol charges a one-time Borrowing Fee for the borrowed liquidity. The fee is added to the Vault’s debt and is given by a base rate + 0.5% (see [Redemption Mechanism](redemption-mechanism.md)) multiplied by the amount of liquidity drawn by the borrower. The minimum Borrowing Fee is 0.5%, and the maximum is 5%.

## Restrictions due to Recovery Mode.

Borrower operations are restricted in several respects when the system is in Recovery Mode or on the verge of reaching this state (see [Recovery Mode](recovery-mode.md)).



To avoid liquidation despite collateral price changes, it is highly recommended to keep the collateral ratio of a Vault well above the MCR. Given that in Recovery Mode, liquidations may even affect Vaults with higher collateral ratios (maximally up to 200%), risk averse borrowers should sufficiently collateralize their Vaults to avoid being near the bottom tiers of collateralization relative to other Vaults whenever the system is close to Recovery Mode.

Maintaining a relatively high collateral ratio high also reduces the risk of getting hit by a redemption .

&#x20;
