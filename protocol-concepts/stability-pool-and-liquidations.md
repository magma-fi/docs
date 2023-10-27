# Stability Pool and Liquidations

## Stability Pool

The stability pool serves as a primary safeguard to ensure the solvency of the system. Its role is to provide the necessary liquidity to cover the debt from liquidated vaults, thereby guaranteeing that the total ioUSD supply is always adequately collateralized.

Whenever a vault is liquidated, an equivalent amount of ioUSD (corresponding to the remaining debt of the vault) is destroyed from the stability pool's balance to settle its debt. As a result, the entirety of the vault's collateral is transferred to the stability pool.

The stability pool is financed by users (known as stability providers) depositing ioUSD into it. Over time, these stability providers witness a proportional reduction in their ioUSD deposits, but in return, they gain a proportional share of the liquidated collaterals.

Given that vaults are usually liquidated at slightly below 150% collateral ratios, it is anticipated that stability providers will amass a larger usd value of collaterals relative to the debt they offset.

## Benefits for Stability Providers

Stability providers benefit from financing the stability pool in several ways.

### Collateral Distribution

To start with, stability providers get access to discounted collaterals (from all the supported collaterals in Magma) from liquidations, without the need to spend gas or run liquidation bots.

As liquidations happen just below the MCR, which is greater than 100%, stability providers receive a discount of `MCR - 100%` on the liquidated collateral, thus experiencing a net gain when a vault is liquidated.

{% hint style="info" %}
**Example:**

Assume a total of 1,000,000 ioUSD exists in the Stability Pool, and you've deposited 100,000 ioUSD.

If a vault with 100,000 ioUSD debt and 9,000,000 IOTX collateral faces liquidation when the IOTX price is $0.017, you'll be impacted based on your 10% pool share.

Specifically, your deposit will decrease by 10% of the liquidated debt, amounting to 10,000 ioUSD. This means your deposit will diminish from 100,000 to 90,000 ioUSD. In compensation, you'll acquire 10% of the liquidated collateral: 900,000 IOTX. At the given price, this collateral is valued at $15,000, rendering you a net profit of $5,000 from the liquidation event.
{% endhint %}

### Yields from collateral

Magma will put collateral into work to generate yields. Take IOTX for example. IOTX deposited will be staked into Bedrock - an IOTX liquid staking protocol - to accrue rewards. A portion of yields from collateral will be distributed into the Stability Pool.

### $Magma Token Emissions

Furthermore, the Stability Pool is natively integrated into the $Magma emission system and therefore they accrue emissions while providing liquidity.

Stability providers can coordinate and vote to maximize the share of emissions directed toward Stability Pool deposits.

### Cooling-down period for deposit

As a general rule, you can withdraw the deposit made to the Stability Pool at any time. However, there is a cooling-down period, which is initially set at 7 days.

When there are Vaults with a collateral ratio below`150%` that have not been liquidated yet, withdrawals are temporarily suspended.

冷静期内继续获得相应奖励，并参与清算，但7天后就自动退出稳定池子，不参与清算，不获得收益，

如果每次取不一样的数？？？

### Can I lose money by depositing funds to the Stability Pool?

While liquidations will occur at a collateral ratio well above `100%` most of the time, it is theoretically possible that a Vault gets liquidated below `100%` in a flash crash or due to an oracle failure. In such a case, you may experience a loss since the collateral gain will be smaller than the reduction of your deposit.&#x20;

If ioUSD is trading above `$1`, liquidations may become unprofitable for Stability Providers even at collateral ratios higher than `100%`. However, this loss is hypothetical since ioUSD is expected to return to the peg, so the “loss” only materializes if you had withdrawn your deposit and sold the ioUSD at a price above `$1`.

Please note that a hack or a bug that results in losses for the users can never be fully excluded.

## Liquidations

In order to maintain full collateral backing for the entire ioUSD supply, Vaults that fall below the minimum collateral ratio of 150% are subject to liquidation.

The debt associated with the liquidated Vault is nullified and absorbed by the Stability Pool, and the collateral is redistributed amongst the stability providers.

Despite the liquidation, the Vault owner retains the full amount of ioUSD borrowed, but suffers an overall loss up to 50% in value. Consequently, it is crucial for borrowers to maintain their collateral ratio above the minimum threshold of 150%, and, ideally, they should aim to keep it above 200%.

### Liquidators

Anyone can initiate the liquidation of an Vault once its collateral ratio falls below the Minimum Collateral Ratio of 150%. To incentivise this action, the liquidator is rewarded with a gas compensation.

Liquidating Vaults involves certain gas costs that the liquidator must bear. To mitigate these costs, the protocol allows batch liquidations of multiple Vaults, reducing the cost per Vault. However, to ensure liquidations remain profitable even when gas prices skyrocket, the protocol provides a gas compensation determined by the following formula:

Gas Compensation = 1 ioUSD + 0.5% of Vault's collateral&#x20;

The 1 ioUSD is sourced from the Liquidation Reserve, while the variable 0.5% portion is taken from the liquidated collateral. This slightly diminishes the liquidation gain for stability providers

### Liquidations rules

The way liquidations are enacted varies depending on certain conditions. This table elucidates all the different scenarios.

**`ICR`**` ``= Individual Collateral Ratio`

**`MCR`**` ``= Minimum Collateral Ratio = 130%`

**`TCR`**` ``= Total Collateral Ratio = 200%`

**`SP`**` ``= Stability Pool`

<table data-header-hidden><thead><tr><th width="220">Condition                                              </th><th width="220">SP ioUSD &#x3C;-> Vault Debt</th><th>Liquidation Behavior</th></tr></thead><tbody><tr><td>Condition                                              </td><td></td><td>Liquidation Behavior</td></tr><tr><td>ICR &#x3C;=100%</td><td></td><td>Redistribute all debt and collateral (minus collateral gas compensation) to active vaults.</td></tr><tr><td>100% &#x3C; ICR &#x3C; MCR &#x26; SP ioUSD > Vault debt</td><td></td><td>ioUSD in the Stability Pool equal to the Vault's debt is offset with the Vault's debt. The Vault's collateral (minus gas compensation) is shared between depositors.</td></tr><tr><td>100% &#x3C; ICR &#x3C; MCR &#x26; SP ioUSD &#x3C; Vault debt</td><td></td><td>The total Stability Pool ioUSD is offset with an equal amount of debt from the Vault. A fraction of the Trove's collateral (equal to the ratio of its offset debt to its entire debt) is shared between depositors. The remaining debt and collateral (minus ETH gas compensation) is redistributed to active Vaults.</td></tr><tr><td>MCR &#x3C;= ICR &#x3C; 200% &#x26; SP ioUSD >= Vault debt</td><td></td><td>The Stability Pool ioUSD is offset with an equal amount of debt from the Vault. A fraction of collateral with dollar value equal to <code>1.2 * debt</code> is shared between depositors. Nothing is redistributed to other active Vaults. Since its ICR was <code>> 1.5</code>, the Vault has a collateral remainder, which is sent to the <code>CollSurplusPool</code> and is claimable by the borrower. The Vault is closed.</td></tr><tr><td>MCR &#x3C;= ICR &#x3C; 200% &#x26; SP ioUSD &#x3C; Vault debt</td><td></td><td>Do nothing.</td></tr><tr><td>ICR >= 200%</td><td></td><td>Do nothing.</td></tr></tbody></table>

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

if this check passes, the user can call `claimCollateralGains` normally using the indexes with non zero claimable amounts in the claimable array. if this check fails, they must either:

1. claim any pending $Pillar rewards first
2. send a transaction to withdraw 0 ioUSD from the stability pool, updating the claimable balance.
