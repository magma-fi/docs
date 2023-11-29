# Redemption Mechanism

### Peg mechanisms

`WEN` maintains a close association with `USD` due to its redeemability for collaterals at face value (i.e., 1 `WEN` can be exchanged for $1 worth of a chosen collateral), and the mandated Maximum Loan-to-Value (MLTV) ratio of 77%. These conditions create a price floor and ceiling, respectively, through arbitrage opportunities. These are known as "hard peg mechanisms" as they rely on explicit operations.

`WEN` also utilizes more indirect methods to maintain `USD` parity — referred to as "soft peg mechanisms". One such mechanism is parity as a Schelling point. As the Magma protocol treats `WEN` as equivalent to `USD`, parity between the two is an inherent equilibrium state of the system.&#x20;

Another mechanism is the minting fee on new debts. As redemptions rise (indicating `WEN` is below $1), the base rate also increases — thus making borrowing less appealing, which prevents new `WEN` from flooding the market and driving the price below $1.

### Redemptions

A redemption refers to the act of exchanging `WEN` for collateral at face value, assuming 1 `WEN` is exactly equal to `$1`. Therefore, for X `WEN`, you receive X dollars worth of collateral in return. Users are free to redeem their `WEN` for any collateral of choice among those supported by Magma at any time without restrictions.&#x20;

### Redemption fee&#x20;

Under normal circumstances, the redemption fee is calculated using the formula:

`(baseRate + 0.5%) * CollateralDrawn`

For instance, if the prevailing redemption fee is `1%`, the price of `IOTX` is `$0.02`, and you redeem `100 WEN`, you would receive `4950 IOTX` (`5000 IOTX` less a redemption fee of `50 IOTX`).

Note that the redeemed amount contributes to the calculation of the `baseRate` and may influence the redemption fee, particularly for large amounts.

Redemption fees are initially collected as protocol income. But governance can decide whether it should be split between the protocol and redeemed Vaults.&#x20;

### **baseRate calculation**

Redemption fees are based on the `baseRate` state variable in `Vault Manager`, which is dynamically updated. The `baseRate` rises with each redemption and decays proportionally to the time elapsed since the last redemption or `WEN` issuance.

Each redemption causes the following changes:

* The `baseRate` experiences decay relative to the time passed since the last fee event.
* The `baseRate` increases by an amount in proportion to the fraction of the total `WEN` supply that was redeemed.

## Redemption FAQ

### Is a redemption the same as paying back my debt?&#x20;

No, redemptions are a completely separate mechanism.&#x20;

### What if my vault is redeemed against?&#x20;

If your Vault is redeemed against, you _do not_ incur a net loss. However, you will lose some of your collateral exposure. Your Vault's CR will also improve after a redemption.&#x20;

### How can I avoid being redeemed against?&#x20;

The most effective way to prevent redemption against your vault is to maintain a high collateral ratio relative to the rest of the vaults in the system.&#x20;

{% hint style="info" %}
_Keep in mind: The riskiest vaults (i.e., the least collateralized vaults) are targeted first when a redemption occurs._
{% endhint %}
