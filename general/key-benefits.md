# Key Benefits

## Multiple collateral with separate positions

Magma supports multiple forms of yield-generating collateral, including LSTs (like `IOTX`, which can be staked) and yield-amplified assets (like DAI, which can earn DAI savings rate).&#x20;

Each position has a single-type collateral, with corresponding parameters, which isolates risks from each other.

## Interest-free liquidity

Users can obtain liquidity against their collateral for free. Users are free to utilize `WEN` to participate in the broader DeFi market consisting of many different products which are designed to generate yield.

## Hard price floor

Magma follows a maximally expansive monetary policy through providing free liquidity at zero issuance costs by default. On the other hand, the issued `WEN` tokens are fully redeemable against the collateral. This enables the protocol to grow rapidly, but not so fast as to lose control over the peg.&#x20;

`WEN` tokens can be returned to the protocol (redeemed) in exchange for an `IOTX` amount worth the face value of the returned `WEN` minus a Redemption Fee. This direct price stability mechanism results in a price floor of $1.

At lower rates, arbitrageurs can make profits by redeeming `WEN` for `IOTX` and immediately selling the latter at a higher dollar price than the current value of the returned `WEN`. Arbitrageurs will thus help to restore the peg by driving demand for WEN, as the Redemption Fees are designed to enable arbitrage gains whenever the peg is broken.

## Governance-minimised algorithmic monetary policy

Unlike competing platforms, Magma does not rely on a governance mechanism to vote on monetary interventions like changing an interest rate. All protocol parameters are either preset and immutable, or algorithmically controlled by the protocol itself, making governance redundant.

Magma uses the current fraction of redeemed `WEN` as an indicator of a peg deviation in order to autonomously set a base rate, which determines both the Redemption Fee and the Borrowing Fee. The base rate increases with the number of redeemed tokens and tends to decay to 0% again when no redemptions take place.

As opposed to an unpredictably fluctuating interest-rate, the Borrowing Fee immediately and predictably reduces the attractiveness of new loans and throttles the generation of fresh `WEN`. In addition, redemption of `WEN` for `IOTX` directly decreases the current stablecoin supply and may motivate low-collateral borrowers to repay their loans, which has the same effect. These measures exert upward pressure on the value of `WEN` whenever it is less than $1, and help to stabilize its price.



##

