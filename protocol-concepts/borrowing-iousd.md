# Borrowing ioUSD

## Depositing and Borrowing

Anyone may obtain liquidity at any time in an entirely permissionless manner after depositing supported collateral into a Vault. At first, only `IOTX` can be used as collateral locked in Vaults and this allows Vault owners to withdraw up to 77% of its current dollar value in the form of `ioUSD` stablecoins. In other words, the user's Vault must always maintain a Maximum Loan-to-Value ratio (MLTV) of 77%, defined as the ratio of the withdrawn liquidity to the current dollar value of the collateral.

Borrowers can repay or borrow more liquidity within the limits of the MLTV whenever they wish. Within the same limit, they can retrieve their collateral. Moreover, a Vault can be topped up with more collateral as needed.

_<mark style="color:yellow;">The protocol imposes a minimum loan of 100</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">`ioUSD`</mark><mark style="color:yellow;">. Therefore, Vaults can only be opened with an initial loan of at least 100</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">`ioUSD`</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">and may never go below a debt of loan</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">`ioUSD`</mark><mark style="color:yellow;">, unless fully repaid and closed.</mark>_

## Vault

A Vault is where you open and maintain your loan. Vaults maintain two balances:&#x20;

1. An asset (`IOTX`) acting as collateral&#x20;
2. A loan denominated in `ioUSD`.

You can change the amount of each by adding collateral or repaying debt.

## Collateral ratio and Loan to Value Ratio

The terms Collateral Ratio and Loan-to-Value (LTV) ratio are both used to describe the relationships between the collateral, the borrowed amount, and the value of the collateral. Understanding the distinction between these two concepts is crucial for users who want to maintain a healthy position in the protocol.

### Collateral Ratio

The Collateral Ratio is the proportion of the total value of the Collateral to the total Value of the Borrowed amount. It is expressed as a percentage and indicates the degree to which the collateral backs the borrowed amount.

$$
\displaystyle{C}{R}={\left(\frac{{{C}{V}}}{{{B}{V}}}\right)}⋅{100}
$$

A higher Collateral Ratio means that the borrower has more collateral relative to the borrowed amount, which reduces the risk for the protocol.&#x20;

### Loan-to-Value (LTV) Ratio

The LTV ratio, on the other hand, is the proportion of the total value of the borrowed amount to the total value of the collateral. It is also expressed as a percentage and indicates the degree to which the borrowed amount is covered by the collateral.

$$
\displaystyle{L}{T}{V}={\left(\frac{{{B}{V}}}{{{C}{V}}}\right)}⋅{100}
$$

A lower LTV ratio implies a safer position for the borrower, as it means that they have more collateral relative to the borrowed amount, reducing the risk of liquidation. In Magma, it is essential to maintain a healthy LTV ratio to avoid liquidations and ensure a smooth borrowing experience.

This documentation will always refer to LTV, for consistency and simplicity.

## Multiple Collateral

**Magma** will look to support multiple collateral types, where users can open multiple vaults, one for each supported collateral type.\
\
It is important to note the following points:

1. Collateral types are segregated, therefore each collateral asset's Total Collateral Ratio (TCR) is calculated independently.&#x20;
2. Collateral asset types may have different protocol parameters, so at any given time using different collateral might be cheaper or more expensive than the others (see [Fees](fees.md)).

## Liquidation Reserve

When a borrower opens a new Vault, an amount of 1 `ioUSD` is reserved and held back by the protocol as a compensation for the gas costs if the Vault needs to be liquidated at some point. The 1 `ioUSD` is added to the Vault’s debt, impacting its collateral ratio.

When a borrower closes their Vault, the Liquidation Reserve is refunded, i.e. the corresponding 1 `ioUSD` debt on the Vault is cancelled. The borrower therefore needs to pay back 1 `ioUSD` less to fully pay off their debt.

## Borrowing Fee

The protocol charges a one-time Borrowing Fee for the borrowed liquidity. The fee is added to the Vault’s debt and is given by a base rate + 0.5% (see [Redemption Mechanism](redemption-mechanism.md)) multiplied by the amount of liquidity drawn by the borrower. The minimum Borrowing Fee is 0.5%, and the maximum is 5%.

## Restrictions due to Recovery Mode.

Borrower operations are restricted in several respects when the system is in Recovery Mode or on the verge of reaching this state (see [Recovery Mode](recovery-mode.md)).

To avoid liquidation despite collateral price changes, it is highly recommended to keep the collateral ratio of a Vault well above the MCR. Given that in Recovery Mode, liquidations may even affect Vaults with higher collateral ratios (maximally up to 200%), risk averse borrowers should sufficiently collateralize their Vaults to avoid being near the bottom tiers of collateralization relative to other Vaults whenever the system is close to Recovery Mode.

Maintaining a relatively high collateral ratio high also reduces the risk of getting hit by a redemption.

## Why would I want to put my collateral on Magma?

### 1. Capital efficiency and Productive Collateral

Since less collateral is needed for the same loan, Magma offers more capital efficiency than other decentralized lending platforms. Your productive asset continues to provide you with interest even when it is used as a collateral.&#x20;

In time Magma will determine whether users will be able to mint `ioUSD` using `USDC` as collateral, removing liquidation risk and opening doors for activities like investing in Real World Assets.

Further along the road map is the potential for a multi-chain stablecoin using Magma. We can lookin to explore DeFi further with `ioUSD`.

&#x20;
