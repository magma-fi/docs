# Stability Pool and Liquidations

## Stability Pool

The stability pool serves as a primary safeguard to ensure the solvency of the system. Its role is to provide the necessary liquidity to cover the debt from liquidated vaults, thereby guaranteeing that the total ioUSD supply is always adequately collateralized.

Whenever a vault is liquidated, an equivalent amount of ioUSD (corresponding to the remaining debt of the vault) is destroyed from the stability pool's balance to settle its debt. As a result, the entirety of the vault's collateral is transferred to the stability pool.

The stability pool is financed by users (known as stability providers) depositing ioUSD into it. Over time, these stability providers witness a proportional reduction in their ioUSD deposits, but in return, they gain a proportional share of the liquidated collaterals.

Given that vaults are usually liquidated at slightly above 77% Loan-To-Value (LTV) ratios, it is anticipated that stability providers will amass a larger USD value of collaterals relative to the debt they offset.

## Benefits for Stability Providers

Stability providers benefit from financing the stability pool in several ways.

### Collateral Distribution

To start with, stability providers get access to discounted collaterals (from all the supported collaterals in Magma) from liquidations, without the need to spend gas or run liquidation bots.

As liquidations happen just above the Maximum Loan-To-Value (MLTV) ratio, which is 77%, stability providers receive a discount of `100-MLTV` on the liquidated collateral, thus experiencing a net gain when a vault is liquidated.

{% hint style="info" %}
**Example:**

Assume a total of 1,000,000 ioUSD exists in the Stability Pool, and you've deposited 100,000 ioUSD.

If a vault with 100,000 ioUSD debt and 9,000,000 IOTX collateral faces liquidation when the IOTX price is $0.017, you'll be impacted based on your 10% pool share.

Specifically, your deposit will decrease by 10% of the liquidated debt, amounting to 10,000 ioUSD. This means your deposit will diminish from 100,000 to 90,000 ioUSD. In compensation, you'll acquire 10% of the liquidated collateral: 900,000 IOTX. At the given price, this collateral is valued at $15,000, rendering you a net profit of $5,000 from the liquidation event.
{% endhint %}

### Yields from Deposited Collateral

Magma will put collateral to work through generating yields. Take IOTX for example. IOTX deposited in Magma will be staked on Bedrock, an IOTX liquid staking protocol, to accrue rewards. A portion of yields from this collateral will be distributed to the Stability Pool.

### $Magma Token Emissions

Furthermore, the Stability Pool is natively integrated into the $MGM emission system and therefore they accrue emissions while providing liquidity.

Stability providers can coordinate and vote to maximize the share of emissions directed toward Stability Pool deposits.

### Cooling-down period for deposit

As a general rule, you can withdraw the deposit made to the Stability Pool at any time. However, there is a cooling-down period, which is initially set at 7 days.

When there are Vaults with a MLTV ratio of over 77`%` that have not been liquidated yet, withdrawals are temporarily suspended.

冷静期内继续获得相应奖励，并参与清算，但7天后就自动退出稳定池子，不参与清算，不获得收益，

如果每次取不一样的数？？？

### Can I lose money by depositing funds to the Stability Pool?

While liquidations will occur at a MLTV above 77`%` most of the time, it is theoretically possible that a Vault gets liquidated below `77%` in a flash crash or due to an oracle failure. In such a case, you may experience a loss since the collateral gain will be smaller than the reduction of your deposit.&#x20;

If ioUSD is trading above `$1`, liquidations may become unprofitable for Stability Providers even at a MLTV ratio higher than `77%`. However, this loss is hypothetical since ioUSD is expected to return to the peg, so the “loss” only materializes if you had withdrawn your deposit and sold the ioUSD at a price above `$1`.

{% hint style="danger" %}
Please note: A hack, exploit or a bug that results in losses for the users can never be fully excluded.
{% endhint %}

## Liquidations

In order to maintain full collateral backing for the entire ioUSD supply, Vaults that rise above the MLTV ratio of 77% are subject to liquidation.

The debt associated with the liquidated Vault is nullified and absorbed by the Stability Pool, and the collateral is redistributed amongst the stability providers.

Despite the liquidation, the Vault owner retains the full amount of ioUSD borrowed, but suffers an overall loss up to 50% in value. Consequently, it is crucial for borrowers to maintain their collateral ratio below the maximum threshold of 77%, and, ideally, they should aim to keep it below 50%.

{% hint style="warning" %}
Please note: The Maximum System LTV ratio is 65%. This activates [Recovery Mode](recovery-mode.md).
{% endhint %}

### Liquidators

Anyone can initiate the liquidation of an Vault once its collateral ratio rises above the MLTV ratio of 77%. To incentivise this action, the liquidator is rewarded with gas compensation.

Liquidating Vaults involves certain gas costs that the liquidator must bear. To mitigate these costs, the protocol allows batch liquidations of multiple Vaults, reducing the cost per Vault. However, to ensure liquidations remain profitable even when gas prices skyrocket, the protocol provides a gas compensation determined by the following formula:

`Gas Compensation = 1 ioUSD + 0.5% of Vault's collateral`&#x20;

The 1 ioUSD is sourced from the Liquidation Reserve, while the variable 0.5% portion is taken from the liquidated collateral. This slightly diminishes the liquidation gain for stability providers.

#### How do liquidations work?

In Normal Mode, liquidations can only happen if your LTV goes above the Maximum LTV (77%). In Recovery Mode, the following parameters affect the behavior of Magma during Recovery Mode:

`ILTV = Individual Loan-to-Value ratio` (Maximum ILTV = 77%)

`MLTV = Maximum Loan-to-Value ratio` (Maximum System LTV = 65%)

`TLTV = current Total Loan-to-Value ratio`

`SP ioUSD = Amount of ioUSD in the Stability Pool`

`Vault Debt = The amount of ioUSD taken as a loan from a Vault`

| Condition                                      | Liquidation Behavior                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ILTV >=100%`                                  | Redistribute all debt and collateral (minus compensation) to active Vaults of the same collateral type.                                                                                                                                                                                                                                                                                                              |
| `MLTV < ILTV < 100% & SP ioUSD > Vault debt`   | ioUSD in the Stability Pool equal to the Vault's debt is offset with the Vault's debt. The Vault's collateral (minus the liquidator compensation) is shared between depositors.                                                                                                                                                                                                                                      |
| `MLTV < ILTV < 100% & SP ioUSD < Vault debt`   | The total Stability Pool ioUSD is offset with an equal amount of debt from the Vault. A fraction of the Vault's collateral (equal to the ratio of its offset debt to its entire debt) is shared between Stability Providers. The remaining debt and collateral (minus the liquidator compensation) is redistributed to active Vaults to reduce their TVL.                                                            |
| `TLTV < ILTV <= MLTV & SP ioUSD >= Vault debt` | The Stability Pool `ioUSD` is offset with an equal amount of debt from the Vault. A fraction of collateral with dollar value equal to `1.3 * debt` for IOTX is shared between Stability Providers. Nothing is redistributed to other active Vaults. Since its ILTV was `< 0.77`, the Vault has a collateral remainder, which is sent to the `CollSurplusPool` and is claimable by the borrower. The Vault is closed. |
| `TLTV < ILTV <= MLTV & SP ioUSD < Vault debt`  | Do nothing.                                                                                                                                                                                                                                                                                                                                                                                                          |
| `ILTV <= TLTV`                                 | Do nothing.                                                                                                                                                                                                                                                                                                                                                                                                          |

A good rule of thumb is to be under the TLTV and the MLTV in Recovery Mode. It is advisable to also monitor the TLTV to avoid liquidation. It is important to note that, because of its design, actions taken by users interacting with the Smart Contracts developed by Magma will always value 1 ioUSD as if it is worth $1 USD.

This does not mean that the value of ioUSD will always be equal to $1 USD; users need to be aware that the price of ioUSD can, and very likely will, move below or above those ranges in Decentralized or Centralized Exchanges. It is important that users understand those mechanisms very well before interacting with Magma.

### What happens if the Stability Pool is empty when liquidations occur?&#x20;

If the Stability Pool is empty, the system uses a secondary liquidation mechanism called redistribution. In such a case, the system redistributes the debt and collateral from liquidated Vaults to all other existing Vaults. The redistribution of debt and collateral is done in proportion to the recipient Vault's collateral amount.&#x20;

#### Claiming liquidated collateral

Collateral transferred to the stability pool in the contexts of liquidations can be claimed by depositors. Before being able to claim, the user must accrue their current apportioned amounts.

Operations which perform such an accruing are:

1. Depositing to the pool
2. Withdrawing from the pool (also 0 amount)
3. Claiming Magma rewards

To avoid an extra transactions the claimer can assess if the accrued value is up to date with the following client side check:

```python

claimable = sp.getDepositorCollateralGain(account)
for i in range(len(claimable)):
    if claimable[i] > 0:
        assert sp.collateralGainsByDepositor(account, i) == claimable[i]

```

if this check passes, the user can call `claimCollateralGains` normally using the indexes with non-zero claimable amounts in the claimable array. if this check fails, they must either:

1. Claim any pending $MGM rewards first
2. Send a transaction to withdraw 0 ioUSD from the stability pool, updating the claimable balance.
