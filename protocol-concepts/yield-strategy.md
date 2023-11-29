# Yield Strategy

The first collateral to be used in Magma is `IOTX`, which will be sent to `IOTX` Voting to earn rewards.

## Strategy manager

The `IOTX` users deposited will be sent to a strategy manager contract and allocated as below:

* **Staking:** Around 80% of total IOTX collateral will be sent to Bedrock liquid staking protocol to earn yields.
* **Buffer:** The rest of the collateral will sit as a buffer. So whenever users want to close their Vaults and reclaim collateral, there's always liquid IOTX for withdrawals.

## Yield Distribution

* 80% of yields will be sent to the Stability Pool as rewards and DEXs as bribes to incentivise liquidity for `WEN` and Magma `MGM`.
* 20% will be allocated to Magma `MGM` holders.
