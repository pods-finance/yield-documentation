---
description: Let's simulate the user flow in this section.
---

# User Flow

## Round 0: Vault Creation

### Round 0: Start Round

The vault is created and, in Round 0, the following happens:

* User A: deposits 100 stETH
* User B: deposits 200 stETH
* User C: deposits 300 stETH

### Round 0: End Round

As Round 0 is the very first round, no yield was accrued. With the `EndRound`, the deposits and withdrawals are paused.&#x20;

### Round 0: Deposits Processing Window

After the `endRound`, the deposits from users A, B, and C are processed, and their shares are created according to the following function:

$$
newShares = amountDeposited * totalSupply/totalAssets
$$

As User A was the first one to deposit into the vault, he is the one that sets the starting number of `totalSupply` and `totalAssets` of the vault, where:

$$
sharesA = amountDeposited = totalSupply = totalAssets
$$

Considering this, the amount of shares each user gets is:

* `sharesA`: 100 \* 100/100 = 100 shares
* `sharesB`: 200 \* 100/100 = 200 shares
* `sharesC`: 300 \* 300/300 = 300 shares

## Round 1

### Round 1: Start Round

At the start of Round 1:

* New deposits and withdrawals are re-enabled;
* The processed deposits (600 stETH) are moved from the vault to the Yield Source, in this case, Lido Finance and
* The initial position is set: 100 (user A) + 200 (user B) + 300 (user C) = 600&#x20;

Right after the deposits are allowed again, a new deposit is made into stETHvv:&#x20;

* User D: deposits 100 stETH

### Round 1: End Round

Considering that 600 stETH remained idle in the contract, and 4.2 stETH was generated in interest (considering a 0.7% weekly yield on stETH). From this 4.2 ETH, we will take 50% (2.1 ETH) of that and transfer it to the Investor Wallet.&#x20;

The 2.1 ETH transferred to the Investor Wallet is used to pay for the premium of buying the put and call options, setting up the strangle strategy for the next week.&#x20;

### Round 1: Deposits Processing Window

The deposit window is very short, and it shouldn't take more than an hour.&#x20;

* User D's deposit is processed;
* User D's shares are created:&#x20;
  1. amount = 100 stETH
  2. `totalSupply` = 600 shares
  3. `totalAssets` = 600 stETH + 4.2 stETH (interest) - 2.1 stETH (yield sent to Investor Wallet) = 602.1 stETH
  4. `sharesD` = 100 \* 600/602.1 = 99.65 shares

## Round 2

### Round 2: Start Round

At the start of Round 2:

* New deposits and withdrawals are re-enabled;
* The initial position is set: 600 (previous deposits) + 100 (user D) + 2.1 (yield of the week) = 702.1

Right after the deposits are allowed again, a new deposit is made into stETHvv:&#x20;

* User E: deposits 100 stETH

### Round 2: End Round

Let's say that:

* The call options bought by the Investor Wallet in Round 1 ended _in-the-money_ and generated 10 stETH of profit
  1. A 20% performance fee will be charged (2 stETH) and sent to the Pods treasury
  2. The remaining 8 ETH will be transferred from the Investor Wallet to the Vault&#x20;
* 4.9147 stETH of interest was generated by the Lido, which implies that:
  1. 2.45735 stETH (50%) is transferred to the Investor Wallet
  2. And these funds will be used to buy the new put and call options of the week

### Round 2: Deposits Processing Window

* User E's deposit is processed;
* User E's shares are created:&#x20;
  1. amount = 100 ETH
  2. `totalSupply` = 99.65 (`sharesD`) + 100 (`sharesA`)+ 200 (`sharesB`)+ 300 (`sharesC`) = 699.65 shares
  3. `totalAssets` = 702.1 (initial position) + 4.9147 (weekly interest) - 2.45735 (yield sent to Investor Wallet) + 10 (exercised options) - 2 (performance fee) = 712.55735 ETH
  4. `sharesE` = 100 \* 699.65 / 712.55735 = 98.19 shares

And so it goes the next rounds...

### Observations

Users must go through one full round after the deposit has been made to be exposed to the strangle strategy.&#x20;
