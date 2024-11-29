# Providing ioUSD Liquidity

ioUSD can be traded on DEXs such as the popular Mimo and Izumi exchanges. The following pages include instructions to providing liquidity to ioUSD pairings.

### **Impermanent loss** (IL)

{% hint style="success" %}
The liquidity protocol in use by Mimo Exchange for ioUSD farms (Uniswap v2) provides two sides to each trade, target and source.

In this case it is WIOTX and ioUSD.

As one token is traded for the other, there is a shift in the composition of the total pool. This means that tokens you add to the pool will fluctuate in value and quantity as the market is utilised. IL is the loss that can occur when removing your position compared to the initial pairing amounts.

Further information on IL can be found here: [https://www.coinbase.com/en-gb/learn/crypto-glossary/what-is-impermanent-loss](https://www.coinbase.com/en-gb/learn/crypto-glossary/what-is-impermanent-loss)
{% endhint %}

### **Concentrated Liquidity**

{% hint style="success" %}
The AMM liquidity model from Uniswap was improved with their v3 model - concentrated liquidity ([https://docs.uniswap.org/concepts/protocol/concentrated-liquidity](https://docs.uniswap.org/concepts/protocol/concentrated-liquidity)).

Liquidity pool tokens are represented by NFTs in Uniswap v3. This allows the liquidity provider to select a range in which their LP is utilised. It doesn't mitigate IL on its own, but it does give more flexibility for users to actively manage liquidity positions through using different liquidity ranges for the assets they commit to a liquidity pool.
{% endhint %}
