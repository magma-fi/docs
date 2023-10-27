# Recovery Mode

## Introduction&#x20;

Recovery Mode is activated when the Total Collateral Ratio (**TCR**) of the system dips below `200%`. In this state, any Vault with a collateral ratio less than the TCR is subject to liquidation.&#x20;

Furthermore, the system puts a halt to any borrower transactions that could potentially exacerbate the decrease in TCR. The only way new `ioUSD` can be generated during this period is by either improving the collateral ratio of existing Vaults, or by establishing a new Vault with a collateral ratio that is greater than or equal to `200%`.&#x20;

Generally, if an adjustment to an existing Vault leads to a decrease in its collateral ratio, the transaction will only go through if the resultant TCR remains above `200%`.&#x20;

### Definition of TCR&#x20;

The Total Collateral Ratio, or TCR, refers to the proportion of the total value of all collaterals within the protocol, evaluated at their present prices, in relation to the total outstanding debt across the protocol.&#x20;

Put differently, it's the collective value of all Vaults' collateral measured in USD, divided by the cumulative debt of all Vaults, denominated in `ioUSD`.&#x20;

### Rationale for the Recovery Mode&#x20;

The primary objective of Recovery Mode is to encourage actions that rapidly increase the Total Collateral Ratio (TCR) to over `200%`, and to motivate `ioUSD` holders to refill the Stability Pool. From an economic perspective, Recovery Mode is structured to promote additional collateral deposits and debt repayments.&#x20;

Moreover, the very prospect of entering Recovery Mode serves as a preventive measure, steering the system away from ever entering this state. It's important to note that Recovery Mode is not an optimal state for the system to operate in.&#x20;

### Impact on fees

During Recovery Mode, while the redemption fee remains unaffected, the Minting fee is reduced to 0% to stimulate borrowing activities that positively impact the TCR.

### **Impact on liquidations**

While in Recovery Mode, if your Vault’s Individual Collateral Ratio **(ICR)** falls below the TCR, your Vault can be liquidated (even if your Vault's collateral ratio is above `150%`). To prevent this from happening, in both Normal and Recovery Mode, a user should maintain their collateral ratio over `200%.`

During Recovery Mode, the liquidation loss is capped at `150%` of a Vault's collateral. Any residual amount, i.e. the collateral above `150%` (and below the Total Collateral Ratio or TCR), can be recouped by the borrower who faced liquidation by claiming the surplus collateral.

This implies that a borrower will encounter the same liquidation "penalty" (`50%`) in Recovery Mode as they would in Normal Mode if their Vault undergoes liquidation.
