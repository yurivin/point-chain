Evmos inflation
https://docs.evmos.org/modules/inflation/01_concepts.html

It has 2 types

1. team vesting - linear inflation. This amount is equal to the total inflation allocatod for team vesting after 4 years (25% * 1B = 250M). Over time, unvested tokens on these accounts are converted into vested tokens at a linear rate. Team members cannot delegate, transfer or execute Ethereum transaction with unvested tokens until they are unlocked represented as vested tokens.

2. exponential inflation - The inflation distribution for staking, usage incentives and community pool is implemented through an exponential formula, a.k.a. the Half Life. Inflation is minted in daily epochs.

## Exponential Inflation formula
```
periodProvision = exponentialDecay       *  bondingIncentive
f(x)            = (a * (1 - r) ^ x + c)  *  (1 + maxVariance - bondedRatio * (maxVariance / bondingTarget))

epochProvision = periodProvision / epochsPerPeriod

where (with default values):
x = variable    = year
a = 300,000,000 = initial value
r = 0.5         = decay factor
c = 9,375,000   = long term supply

bondedRatio   = variable  = fraction of the staking tokens which are currently bonded
maxVariance   = 0.4       = the max amount to increase inflation
bondingTarget = 0.66      = our optimal bonded ratio
```
## Inflation genesis part
```
"inflation": {
      "params": {
        "mint_denom": "apoint",
        "exponential_calculation": {
          "a": "300000000.000000000000000000",
          "r": "0.500000000000000000",
          "c": "9375000.000000000000000000",
          "bonding_target": "0.660000000000000000",
          "max_variance": "0.000000000000000000"
        },
        "inflation_distribution": {
          "staking_rewards": "0.533333334000000000",
          "usage_incentives": "0.333333333000000000",
          "community_pool": "0.133333333000000000"
        },
        "enable_inflation": true
      },
      "period": "0",
      "epoch_identifier": "day",
      "epochs_per_period": "365",
      "skipped_epochs": "0"
    },
```
## Calulations example for a given genesis example

```
//apoint is a minimal part of a point token

//Year(period) x=1 and bondedRatio = 0.5
(300000000 * (1 - 0.5)^1 + 9375000) * (1 + 0 - 0.5 * (0 / 0.66) = 159375000apoint 

//Year(period) x=2 and bondedRatio = 0.7
(300000000 * (1 - 0.5)^2 + 9375000) * (1 + 0 - 0.7 * (0 / 0.66) = 16875000apoint
```