# Recovery Mode

## Introduction&#x20;

Recovery Mode is activated when the Maximum Loan-to-Value (MLTV) ratio of the system rises above `65%`. In this state, any Vault with a collateral ratio above the MLTV is subject to liquidation.&#x20;

Furthermore, the system puts a halt to any borrower transactions that could potentially exacerbate the increase in MLTV. The only way new `ioUSD` can be generated during this period is by either improving the collateral ratio of existing Vaults, or by establishing a new Vault with a collateral ratio that is `less than or` equal to `65%`.&#x20;

Generally, if an adjustment to an existing Vault leads to an increase in its collateral ratio, the transaction will only go through if the resultant MLTV remains below `65%`.&#x20;

### Definition of TCR&#x20;

The Total Loan-to-Value (TLTV) ratio, refers to the proportion of the total outstanding debt across the protocol in relation to the total value of all collaterals within the protocol, evaluated at their current prices.

Put differently, it's the collective value of all the cumulative debt of all Vaults divided by all Vault collateral, both denominated in USD.&#x20;

### Rationale for the Recovery Mode&#x20;

The primary objective of Recovery Mode is to encourage actions that rapidly decrease the TLTV to below `65%`, and to motivate `ioUSD` holders to refill the Stability Pool. From an economic perspective, Recovery Mode is structured to promote additional collateral deposits and debt repayments.&#x20;

Moreover, the very prospect of entering Recovery Mode serves as a preventative measure, steering the system away from ever entering this state. It's important to note that Recovery Mode is not an optimal state for the system to operate in.&#x20;

### Impact on fees

During Recovery Mode, while the redemption fee remains unaffected, the Minting fee is reduced to 0% to stimulate borrowing activities that positively impact the TLTV

### **Impact on liquidations**

While in Recovery Mode, if your Vault’s Individual Loan-to-Value (ILTV) ratio rises above the TLTV, your Vault can be liquidated (even if your Vault's collateral ratio is below `77%`). To prevent this from happening, in both Normal and Recovery Mode, a user should maintain their ILTV below `65%.`

During Recovery Mode, the liquidation loss is capped at `50%` of a Vault's collateral. Any residual amount, i.e. the collateral above `50%` (and below the TLTV), can be recouped by the borrower who faced liquidation, by claiming the surplus collateral.

This implies that a borrower will encounter the same liquidation "penalty" (`50%`) in Recovery Mode as they would in Normal Mode if their Vault undergoes liquidation.
