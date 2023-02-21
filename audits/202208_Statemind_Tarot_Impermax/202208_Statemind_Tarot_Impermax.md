# Yearn StrategyImperamaxLender Interim Security Review Report (Final, with responses)

# General

## Project Brief

Title | Description
--- | ---
Client             | Yearn
Project name       | StrategyImperamaxLender
Timeline           | 02-08-2022 - 
Number of auditors | 5
Initial commit     | 43b3b1aaf811e8ec7b84d9217fd977717b0dd956


### Short Overview

Tarot is a decentralized lending protocol on Fantom and Optimism. It is based on working with LP tokens. It has three main functionalities: lending, borrowing and leverage farming. Lenders provide liquidity in one of the two tokens of pair. Borrowers can borrow this tokens using LP tokens as collateral. Due to available tokens in Tarot pool, users can convert them to LP tokens to enable leverage farming. 
`StrategyImperamaxLender` contract in the auditing scope is yearn strategy based on lending `want` token to several Tarot pools.


### Project Scope

The audit covered the following files:

File name | Link
--- | ---
StrategyImperamaxLender.sol | [StrategyImperamaxLender.sol](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol)





## Finding Severity breakdown

Each vulnerability discovered during the audit is classified based on their severity and likelihood and have the following classification:

Severity | Description
--- | ---
Critical | Bugs leading to assets theft, funds locking, or any other issues that lead to fund loss.
High     | Bugs that break contract core logic or lead to the contract state corruption, or any bugs that require manual recovery.
Medium   | Bugs leading to partial failure of the contract minor logic under specific conditions or significant gas optimizations..
Informational | Bugs, suggestions, or optimizations that do not have a significant impact in terms of contract security.



Based on the feedback received from the Client regarding the report, all issues assigned the following statuses:

Status | Description
--- | ---
Fixed        | Recommended fixes have been made to the project code and no longer affect its security.
Acknowledged | The Customer is aware of the finding. Recommendations for the finding are planned to be resolved in the future.


## Summary of findings

Severity | # of Findings
--- | ---
CRITICAL| 0
HIGH    | 0
MEDIUM  | 5
INFORMATIONAL | 12

## Client's updates with findings incorporated
- https://github.com/dudesahn/StrategyImperamaxLender/blob/26c53a115ca27366226f6de896fa172b59ae8e06/contracts/StrategyImperamaxLender.sol


## FINDINGS REPORT

### Critical



### High



### Medium

#### 1. Possibility of unintentional reordering of the pools

##### Description

Function [manuallySetAllocations](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L429) calls `reorderPools()` function at the line [L460](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L460) without checking boolian `reorder` flag.

##### Recommendation

We recommend adding if statement checking `reorder` value.

##### Status
**NEW**

##### Client's comments
 > Agree, added.

#### 2. Exchange rates update in the function `attemptToRemovePool`

##### Description

Before performing changes in the function  [`attemptToRemovePool`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L477) it makes sense to updated exchange rates for a higher precision. Same in [`liquidatePosition`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L284) and [`getCurrentPoolAllocations`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L141).

##### Recommendation

Add [`updateExchangeRates`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L251) at the beggining of above mentioned functions.

##### Status
**NEW**

##### Client's comments
 > Added to beginning of `attemptToRemovePool()`. Did not add to `getCurrentPoolAllocations()` as this is an external view and I don't want to make it state-changing. `liquidatePosition()` does not require this as it will be called in `_withdraw`().

#### 3. The function `reorderPool` is lackig in several instances

##### Description

In comments to the function  [`_deposit`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L271) it is specified, that we want to deposit to highest utilization pair, but in order to insure that, at the beginning of the fuction we need to call 


if (reorder) {reorderPools();} 


Same is correct for function  [`_withdraw`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L310) as we want to depost / withdraw funds from pools based on their utilization.

##### Recommendation

Add function `reorderPools` to ensure that pools associated arrays are duly sorted.

##### Status
**NEW**

##### Client's comments
 > The goal behind `reorderPools()` was not for it to be called on every withdrawal, as this is needlessly gas intensive for users and only a marginal benefit. Instead, by calling it on every harvest, Yearn takes on this gas cost. Furthermore, `adjustPosition()` will only realistically be called during harvests, as we have no use for tends in this strategy. Should the order of the pools require an update due to massive fluctuations in pool utilization, we can either harvest, or call `reorderPools()` externally.

#### 4. Unsafe transfer of tokens

##### Description

In [Line 277](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L277) unsafe method **transfer()** is used.

Same problem: 

- [Line 454](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L454)

##### Recommendation

It is recommended to use **safeTransfer()**.

##### Status
**NEW**

##### Client's comments
 > Updated the call in line 454, but didn't update line 277 as `_deposit()` is only called in `adjustPosition()`, and is only called if the value passed is greater than 0.

#### 5. Different check logic

##### Description

In [**liquidatePosition(uint256 _amountNeeded)**](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L284) it is allowed to have dust loss from **_withdraw()** method. But in [**_withdraw(uint256 _amountToWithdraw)**](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L310) such dust loss will cause the revert of function execution, because of require on [Line 396-399](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L396).

##### Recommendation

Add one more check for dust loss in **require()** on [Line 396-399](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L396) of **_withdraw(uint256 _amountToWithdraw)** function. Because of check for dust loss, adding 5 wei in [Line 360](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L360) can be omitted. To avoid double calculations loss value can be returned.

##### Status
**NEW**

##### Client's comments
 > Addressed in `ef2fa6d`. Here, I removed the extra addition of 5 wei, and had `_withdraw()` return a loss as suggested. However, I made some further adjustments in how to treat loss around withdrawals: 
> * `prepareReturn()` 
>     * Removed the use of safeMath in some parts of `prepareReturn` when not needed to save some gas
>     * Adjusted how `debtPayment` and `_loss` were handled in `prepareReturn()` in the event that we needed to free up more loose want. 
>         * Previously, in the event that we couldn't free up our full `totalToFree`, `_profit` and `_debtPayment` would add up to more than our want balance if we had a profit. The new code will take losses if we don't retrieve the amount of `want` we requested. The updated code is:
> ```
>             if (wantBal >= _debtOutstanding) {
>                 _profit = wantBal - _debtOutstanding;
>             } else {
>                 _profit = 0;
>                 _loss = _debtOutstanding - wantBal;
>                 _debtPayment = wantBal;
>             }
> ```
> * `liquidatePosition()`
>     * The code in `liquidatePosition()` was also updated to simplify the check for losses against `dustThreshold` this is the only place where this check takes place, as we only care about protecting users on withdrawal from large losses due to locked funds. In other scenarios that `_withdraw()` is called, we do the following:
>         * `prepareReturn()`
>             * Report losses in line 246, detailed above
>             * Since strategies using `healthCheck` will revert on any lossy harvests without manual override, this will prevent any losses from going unnoticed
>         * `liquidateAllPositions()`
>             * Losses are reported in `harvest()`, as this is only called via `harvest()` when `emergencyExit == true`
>         * `manuallySetAllocations()`
>             * Theoretically we take "losses" here, but they are not reported and don't matter, as we are just moving funds around
>         * `emergencyWithdraw()`
>             * Again, we are just trying to get out the maximum amount of funds we can, and thus don't report losses
> * `_withdraw()`
>     * Removed the section from previous L342-L56. This section was only used to completely drain the strategy in emergent situations, and realistically was a waste of gas and bytecode. Without it we can get very similar results.
>     * updated old L390 from `remainingUnderlyingNeeded = 0` to `break`. The point was to prevent another loop from happening, and `break` is more efficient here.
>     * Removed L394-L400. Again, this was part of a very specific check for if we were emergently shutting down the strategy, and is not needed with adjustments above.
>     * Added return of `withdrawalLoss`, new L377. This allows us to check in `liquidatePosition` if our losses are dust-sized or not.
> * `manuallySetAllocations()`
>     * Instead of using our special case of `_withdraw(type(uint256).max)`, we instead are using `_withdraw(stakedBalance())`. It accomplishes virtually the same thing, and will only leave at most a tiny amount of dust in the previous pools which is not an issue.


### Informational

#### 1. Redundant function call

##### Description

Function [manuallySetAllocations](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L429)  calls `updateExchangeRates()` function at the line [L440](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L440) which would be called again in the `_withdraw` function call at the line [L443](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L443).

##### Recommendation

We recommend removing `updateExchangeRates()` function call at the line [L440](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L440).

##### Status
**NEW**

##### Client's comments
 > Left this as-is; I don't want to make any of the views state-changing, but would like to use the most accurate number for our withdrawal input. Also added an `updateExchangeRates()` to `liquidateAllPositions()` for the same reasons.

#### 2. Redundant if statement

##### Description

In the function [attemptToRemovePool](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L477) it is possible to simplify if statements at the lines [L509 - L515](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L509).

##### Recommendation

```solidity
else
     preventDeposits.push(boolHelperPool[i]);

```

##### Status
**NEW**

##### Client's comments
 > Agree, added.

#### 3. 5 wei as a buffer for calculation of losses

##### Description

The number - [5 wei](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L360), it's calculation and rationale of it's application raise questions. Also, later the variable [remainingbTokenNeeded](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L359) is used to compare it with [ableToPullInbToken](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L363), and for the sake of consistency it would make sense to make the same correction to [ableToPullInbToken](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L340), especially taking into account that calculation method is the same.

##### Recommendation

If some buffer is required as an assurance, it makes sense to correct calculation of [ableToPullInbToken](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L340) in order to make logic more coherent (or remove this correction at all). Also, the number itself seems to be semi-random, it may be probably better to anchor it to some value, e.g. [dustThreshold](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L340), or [dustThreshold](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L340)/const (for example [dustThreshold](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L340)/4).

##### Status
**NEW**

##### Client's comments
 > Addressed as a part of changes in medium finding #5.

#### 4. Excessive "if" statement in a `prepareReturn` function

##### Description

After [this calculation](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L239) _debtPayment is by definition not greater than wantBal. [Comparison](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L242) is not nessesary.

##### Recommendation

Remove [conditional expression](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L242).

##### Status
**NEW**

##### Client's comments
 > Updated as a part of medium finding #5 to better address the intended goal.

#### 5. Gas could be saved on pools number

##### Description

Most likely strategy will be interacting with at most several dozens of pools. Thus, arrays with fixed size could be used instead of dynamic ones to store the data - maximum number of pools could be one order of magnitude greater than probable number of pools, for instance 1000 (again, it is unlikely that more than 100 pools will be used by the strategy). This way substential amount of gas could be saved.

##### Recommendation

Set a maximum number of pools and use array of fixed size for pool of arrays and all derrivative arrays ([pools[]](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L28), [preventDeposits[]](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L29), [utilizations[]](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L128)). If this change is implemented, some additional syntactic corrections in conract are nessesary (e.g. checks for overflow when adding new pool).

##### Status
**NEW**

##### Client's comments
 > While using fixed arrays would save gas, this would require a signficant re-write and is out of scope at the moment.

#### 6. Gas could be saved on intermediate variables

##### Description

Across the contract there are many cases, when gas could be saved by avoiding intermediary variables, for example instead of

  [address targetPool = pools[i];
   IBorrowable(targetPool).exchangeRate();](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L254)
  
one could use

  IBorrowable(pools[i]).exchangeRate();
  

All the nessesary details could be provided in more specified comments, in order not to loose understandability and plainness of functions ad variables.

##### Recommendation

Avoid usage of intermediary variables.

##### Status
**NEW**

##### Client's comments
 > Would removing these intermediate variables help? For instance, in the example listed, my code as written is storing the state variable element `pools[i]` in memory, and then reading from memory twice, whereas the suggested fixes is reading from state twice. Shouldn't the latter be more expensive gas-wise?

#### 7. Gas could be saved on shrinking of "if" statement

##### Description

The expression in function [`attemptToRemovePool`](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L477) could be optimized:
```solidity
    ...
       if (addressHelperPool[i] == _poolToRemove) {
          // this is our pool we are targeting, but it's not empty yet
          preventDeposits.push(true);
      } else if (!boolHelperPool[i]) {
          // these are normal pools that allow deposits
          preventDeposits.push(false);
      } else {
          // this allows us to be emptying multiple pools at once. if the pool is emptying but not the one we're removing, leave it alone.
          preventDeposits.push(true);
      }
    ...
```
      
To the new version:

```solidity
    ...
      if (!boolHelperPool[i] || addressHelperPool[i] != _poolToRemove) {
          // these are normal pools that allow deposits
          preventDeposits.push(false);
      } else {
          // this is our pool we are targeting, but it's not empty yet + this allows us to be emptying multiple pools at once. if the pool is emptying but not the one we're removing, leave it alone.
          preventDeposits.push(true);
      }
    ...
```

##### Recommendation

Optimize code according to the reference.

##### Status
**NEW**

##### Client's comments
 > Updated to use format from informational finding #2.

#### 8. Gas optimization

##### Description

In function [**_withdraw(uint256 _amountToWithdraw)**](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L310) if-else statements on [Lines 345 - 354](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L345) and on [Lines 363 - 392](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L363) can be simplified.

##### Recommendation

```solidity
    ...
        for (uint256 i = 0; i < pools.length; i++) {
            // save some gas by storing locally
            address currentPool = pools[i];

            // how much want our strategy has supplied to this pool
            uint256 suppliedToPool = wantSuppliedToPool(currentPool);

            // total liquidity available in the pool in want
            uint256 poolLiquidity = want.balanceOf(currentPool);

            // the minimum of the previous two values is the most want we can withdraw from this pool

            uint256 ableToPullInUnderlying = 0;
            uint256 ableToPullInbToken = 0;

            if (poolLiquidity >= suppliedToPool) {
                ableToPullInUnderlying = suppliedToPool;
                ableToPullInbToken = IBorrowable(currentPool).balanceOf(address(this));
            } else {
                ableToPullInUnderlying = poolLiquidity;
                ableToPullInbToken = ableToPullInUnderlying.mul(BTOKEN_DECIMALS).div(IBorrowable(currentPool).exchangeRateLast());
            }

            if (ableToPullInUnderlying == 0) {
                continue;
            }

            // check if we need to pull as much as possible from our pools
            if (params.debtRatio == 0 || _amountToWithdraw == type(uint256).max || vaultAPIExtended(address(vault)).emergencyShutdown()) {
                IBorrowable(currentPool).transfer(currentPool, ableToPullInbToken);
                IBorrowable(currentPool).redeem(address(this));

                continue;
            }

            uint256 withdraw_amount = Math.min(remainingUnderlyingNeeded.mul(BTOKEN_DECIMALS).div(IBorrowable(currentPool).exchangeRateLast()), ableToPullInbToken);

            IBorrowable(currentPool).transfer(currentPool, withdraw_amount);
            uint256 pulled = IBorrowable(currentPool).redeem(address(this));

            withdrawn = withdrawn.add(pulled);

            // don't want to overflow
            if (remainingUnderlyingNeeded > pulled) {
                remainingUnderlyingNeeded = remainingUnderlyingNeeded.sub(pulled);
            } else {
                remainingUnderlyingNeeded = 0;
            }
        }
    ...
```

##### Status
**NEW**

##### Client's comments
 > While I did take some suggestions from this, I did not use it as-is. My goal here was not to optimize for gas overall, but to optimize for the most common withdrawals, meaning from users withdrawing less than the amount in any one pool. Thus, it makes sense to have this be the first option, and then for deeper withdrawals do the other math that is needed there instead of simplifying it all together.

#### 9. Gas optimization

##### Description

In for loop the use of **i++**, **i--** is more gas expsensive than **++i**, **--i** .

Lines with issue:

- [Line 111](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L111)
- [Line 129](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L129)
- [Line 143](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L143)
- [Line 151](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L151)
- [Line 253](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L253)
- [Line 273](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L273)
- [Line 321](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L321)
- [Line 415](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L415)
- [Line 433](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L433)
- [Line 448](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L448)
- [Line 468](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L468)
- [Line 505](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L505)
- [Line 530](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L530)

##### Recommendation

Use **++i**, **--i** instead of **i++**, **i--**.

##### Status
**NEW**

##### Client's comments
 > Agree, added.

#### 10. Gas optimization

##### Description

In [**harvestTrigger(uint256 callCostInWei)**](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L571) there are three **if statements** which return the same value.

##### Recommendation

It is recommended to unite three statements in one.

```solidity
    function harvestTrigger(uint256 callCostInWei) public view virtual override returns (bool) {
        StrategyParams memory params = vault.strategies(address(this));

        if (block.timestamp.sub(params.lastReport) > maxReportDelay || forceHarvestTriggerOnce ||vault.creditAvailable() > creditThreshold ) {
            return true;
        }

        // otherwise, we don't harvest
        return false;
    }
```

##### Status
**NEW**

##### Client's comments
 > Agree, but leaving as-is for stylistic reasons.

#### 11. Missing error message

##### Description

In [Line 79](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L79) there is no error message.

##### Recommendation

It is recommended to provide error message for additional information and for gas optimization.

##### Status
**NEW**

##### Client's comments
 > Agree, added.

#### 12. Hardcoded values

##### Description

In [**_initializeStrat(string memory _name)**](https://github.com/dudesahn/StrategyImperamaxLender/blob/43b3b1aaf811e8ec7b84d9217fd977717b0dd956/contracts/StrategyImperamaxLender.sol#L50) **maxReportDelay**, **dustThreshold** variables are hardcoded.

##### Recommendation

It is recommended to set these variables as function parameters and add require for **dustThreshold**.

##### Status
**NEW**

##### Client's comments
 > Will leave this as-is, as they are good starting values for deployment.



## Appendix A. Linter

### Error/compiler-version
 - [contracts/StrategyImperamaxLender.sol:2](contracts/StrategyImperamaxLender.sol:2) - Compiler version 0.6.12 does not satisfy the 0.8.13 semver requirement


### Error/ordering
 - [contracts/StrategyImperamaxLender.sol:48](contracts/StrategyImperamaxLender.sol:48) - Function order is incorrect, event definition can not go after constructor (line 42)


### Error/func-name-mixedcase
 - [contracts/interfaces/IRouter.sol:10](contracts/interfaces/IRouter.sol:10) - Function name must be in mixedCase


### Error/max-line-length
 - [contracts/StrategyImperamaxLender.sol:29](contracts/StrategyImperamaxLender.sol:29) - Line length must be no more than 100 but current length is 107.

 - [contracts/StrategyImperamaxLender.sol:30](contracts/StrategyImperamaxLender.sol:30) - Line length must be no more than 100 but current length is 143.

 - [contracts/StrategyImperamaxLender.sol:32](contracts/StrategyImperamaxLender.sol:32) - Line length must be no more than 100 but current length is 132.

 - [contracts/StrategyImperamaxLender.sol:33](contracts/StrategyImperamaxLender.sol:33) - Line length must be no more than 100 but current length is 132.

 - [contracts/StrategyImperamaxLender.sol:66](contracts/StrategyImperamaxLender.sol:66) - Line length must be no more than 100 but current length is 161.

 - [contracts/StrategyImperamaxLender.sol:81](contracts/StrategyImperamaxLender.sol:81) - Line length must be no more than 100 but current length is 106.

 - [contracts/StrategyImperamaxLender.sol:89](contracts/StrategyImperamaxLender.sol:89) - Line length must be no more than 100 but current length is 109.

 - [contracts/StrategyImperamaxLender.sol:93](contracts/StrategyImperamaxLender.sol:93) - Line length must be no more than 100 but current length is 103.

 - [contracts/StrategyImperamaxLender.sol:110](contracts/StrategyImperamaxLender.sol:110) - Line length must be no more than 100 but current length is 105.

 - [contracts/StrategyImperamaxLender.sol:116](contracts/StrategyImperamaxLender.sol:116) - Line length must be no more than 100 but current length is 140.

 - [contracts/StrategyImperamaxLender.sol:126](contracts/StrategyImperamaxLender.sol:126) - Line length must be no more than 100 but current length is 108.

 - [contracts/StrategyImperamaxLender.sol:149](contracts/StrategyImperamaxLender.sol:149) - Line length must be no more than 100 but current length is 108.

 - [contracts/StrategyImperamaxLender.sol:163](contracts/StrategyImperamaxLender.sol:163) - Line length must be no more than 100 but current length is 162.

 - [contracts/StrategyImperamaxLender.sol:185](contracts/StrategyImperamaxLender.sol:185) - Line length must be no more than 100 but current length is 102.

 - [contracts/StrategyImperamaxLender.sol:187](contracts/StrategyImperamaxLender.sol:187) - Line length must be no more than 100 but current length is 114.

 - [contracts/StrategyImperamaxLender.sol:284](contracts/StrategyImperamaxLender.sol:284) - Line length must be no more than 100 but current length is 124.

 - [contracts/StrategyImperamaxLender.sol:331](contracts/StrategyImperamaxLender.sol:331) - Line length must be no more than 100 but current length is 101.

 - [contracts/StrategyImperamaxLender.sol:340](contracts/StrategyImperamaxLender.sol:340) - Line length must be no more than 100 but current length is 134.

 - [contracts/StrategyImperamaxLender.sol:343](contracts/StrategyImperamaxLender.sol:343) - Line length must be no more than 100 but current length is 138.

 - [contracts/StrategyImperamaxLender.sol:358](contracts/StrategyImperamaxLender.sol:358) - Line length must be no more than 100 but current length is 134.

 - [contracts/StrategyImperamaxLender.sol:360](contracts/StrategyImperamaxLender.sol:360) - Line length must be no more than 100 but current length is 119.

 - [contracts/StrategyImperamaxLender.sol:373](contracts/StrategyImperamaxLender.sol:373) - Line length must be no more than 100 but current length is 134.

 - [contracts/StrategyImperamaxLender.sol:395](contracts/StrategyImperamaxLender.sol:395) - Line length must be no more than 100 but current length is 127.

 - [contracts/StrategyImperamaxLender.sol:397](contracts/StrategyImperamaxLender.sol:397) - Line length must be no more than 100 but current length is 136.

 - [contracts/StrategyImperamaxLender.sol:407](contracts/StrategyImperamaxLender.sol:407) - Line length must be no more than 100 but current length is 105.

 - [contracts/StrategyImperamaxLender.sol:463](contracts/StrategyImperamaxLender.sol:463) - Line length must be no more than 100 but current length is 106.

 - [contracts/StrategyImperamaxLender.sol:476](contracts/StrategyImperamaxLender.sol:476) - Line length must be no more than 100 but current length is 142.

 - [contracts/StrategyImperamaxLender.sol:516](contracts/StrategyImperamaxLender.sol:516) - Line length must be no more than 100 but current length is 118.

 - [contracts/StrategyImperamaxLender.sol:523](contracts/StrategyImperamaxLender.sol:523) - Line length must be no more than 100 but current length is 110.

 - [contracts/StrategyImperamaxLender.sol:538](contracts/StrategyImperamaxLender.sol:538) - Line length must be no more than 100 but current length is 148.

 - [contracts/StrategyImperamaxLender.sol:541](contracts/StrategyImperamaxLender.sol:541) - Line length must be no more than 100 but current length is 118.

 - [contracts/StrategyImperamaxLender.sol:544](contracts/StrategyImperamaxLender.sol:544) - Line length must be no more than 100 but current length is 150.

 - [contracts/StrategyImperamaxLender.sol:556](contracts/StrategyImperamaxLender.sol:556) - Line length must be no more than 100 but current length is 129.


### Error/private-vars-leading-underscore
 - [contracts/StrategyImperamaxLender.sol:22](contracts/StrategyImperamaxLender.sol:22) - 'BASIS_PRECISION' should start with _

 - [contracts/StrategyImperamaxLender.sol:23](contracts/StrategyImperamaxLender.sol:23) - 'BTOKEN_DECIMALS' should start with _

 - [contracts/StrategyImperamaxLender.sol:32](contracts/StrategyImperamaxLender.sol:32) - 'forceHarvestTriggerOnce' should start with _

 - [contracts/StrategyImperamaxLender.sol:35](contracts/StrategyImperamaxLender.sol:35) - 'stratName' should start with _

 - [contracts/StrategyImperamaxLender.sol:38](contracts/StrategyImperamaxLender.sol:38) - 'isOriginal' should start with _

 - [contracts/StrategyImperamaxLender.sol:157](contracts/StrategyImperamaxLender.sol:157) - 'wantSuppliedToPool' should start with _

 - [contracts/StrategyImperamaxLender.sol:199](contracts/StrategyImperamaxLender.sol:199) - 'prepareReturn' should start with _

 - [contracts/StrategyImperamaxLender.sol:251](contracts/StrategyImperamaxLender.sol:251) - 'updateExchangeRates' should start with _

 - [contracts/StrategyImperamaxLender.sol:260](contracts/StrategyImperamaxLender.sol:260) - 'adjustPosition' should start with _

 - [contracts/StrategyImperamaxLender.sol:284](contracts/StrategyImperamaxLender.sol:284) - 'liquidatePosition' should start with _

 - [contracts/StrategyImperamaxLender.sol:408](contracts/StrategyImperamaxLender.sol:408) - 'liquidateAllPositions' should start with _

 - [contracts/StrategyImperamaxLender.sol:414](contracts/StrategyImperamaxLender.sol:414) - 'prepareMigration' should start with _

 - [contracts/StrategyImperamaxLender.sol:595](contracts/StrategyImperamaxLender.sol:595) - 'protectedTokens' should start with _


### Error/contract-name-camelcase
 - [contracts/StrategyImperamaxLender.sol:13](contracts/StrategyImperamaxLender.sol:13) - Contract name must be in CamelCase


### Error/max-states-count
 - [contracts/StrategyImperamaxLender.sol:17](contracts/StrategyImperamaxLender.sol:17) - Contract has 8 states declarations but allowed no more than 3


### Error/code-complexity
 - [contracts/StrategyImperamaxLender.sol:171](contracts/StrategyImperamaxLender.sol:171) - Function has cyclomatic complexity 8 but allowed no more than 5

 - [contracts/StrategyImperamaxLender.sol:310](contracts/StrategyImperamaxLender.sol:310) - Function has cyclomatic complexity 9 but allowed no more than 5

 - [contracts/StrategyImperamaxLender.sol:477](contracts/StrategyImperamaxLender.sol:477) - Function has cyclomatic complexity 10 but allowed no more than 5


### Error/function-max-lines
 - [contracts/StrategyImperamaxLender.sol:199](contracts/StrategyImperamaxLender.sol:199) - Function body contains 49 lines but allowed no more than 40 lines

 - [contracts/StrategyImperamaxLender.sol:310](contracts/StrategyImperamaxLender.sol:310) - Function body contains 90 lines but allowed no more than 40 lines

 - [contracts/StrategyImperamaxLender.sol:477](contracts/StrategyImperamaxLender.sol:477) - Function body contains 67 lines but allowed no more than 40 lines


### Error/no-unused-vars
 - [contracts/StrategyImperamaxLender.sol:260](contracts/StrategyImperamaxLender.sol:260) - Variable "_debtOutstanding" is unused

 - [contracts/StrategyImperamaxLender.sol:571](contracts/StrategyImperamaxLender.sol:571) - Variable "callCostInWei" is unused


### Error/not-rely-on-time
 - [contracts/StrategyImperamaxLender.sol:575](contracts/StrategyImperamaxLender.sol:575) - Avoid to make time-based decisions in your business logic


### Error/no-empty-blocks
 - [contracts/StrategyImperamaxLender.sol:593](contracts/StrategyImperamaxLender.sol:593) - Code contains empty blocks

 - [contracts/StrategyImperamaxLender.sol:595](contracts/StrategyImperamaxLender.sol:595) - Code contains empty blocks





## Appendix B. Slither

### High/Medium/unchecked-transfer

[StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545) ignores return value by [IBorrowable(_poolToRemove).transfer(_poolToRemove,balanceOfbToken)](..contracts/StrategyImperamaxLender.sol#L499)

[StrategyImperamaxLender.manuallySetAllocations(uint256[])](..contracts/StrategyImperamaxLender.sol#L429-L461) ignores return value by [want.transfer(targetPool,toDeposit)](..contracts/StrategyImperamaxLender.sol#L454)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).transfer(currentPool,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L352)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).transfer(currentPool,balanceOfbToken)](..contracts/StrategyImperamaxLender.sol#L348)

[StrategyImperamaxLender._deposit(uint256)](..contracts/StrategyImperamaxLender.sol#L271-L282) ignores return value by [want.transfer(targetPool,_depositAmount)](..contracts/StrategyImperamaxLender.sol#L277)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).transfer(currentPool,balanceOfbToken_scope_1)](..contracts/StrategyImperamaxLender.sol#L377)

[StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545) ignores return value by [IBorrowable(_poolToRemove).transfer(_poolToRemove,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L525)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).transfer(currentPool,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L380)

[StrategyImperamaxLender.prepareMigration(address)](..contracts/StrategyImperamaxLender.sol#L414-L424) ignores return value by [bToken.transfer(_newStrategy,balanceOfbToken)](..contracts/StrategyImperamaxLender.sol#L421)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).transfer(currentPool,remainingbTokenNeeded)](..contracts/StrategyImperamaxLender.sol#L364)


### Informational/High/assembly

[BaseStrategyInitializable.clone(address,address,address,address)](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L925-L946) uses assembly
	- [INLINE ASM](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L934-L941)


### Informational/High/naming-convention

Parameter [StrategyImperamaxLender.initialize(address,address,address,address,string)._keeper](..contracts/StrategyImperamaxLender.sol#L63) is not in mixedCase

Parameter [StrategyImperamaxLender.initialize(address,address,address,address,string)._strategist](..contracts/StrategyImperamaxLender.sol#L61) is not in mixedCase

Parameter [StrategyImperamaxLender.cloneTarotLender(address,address,address,address,string)._vault](..contracts/StrategyImperamaxLender.sol#L73) is not in mixedCase

Parameter [StrategyImperamaxLender.cloneTarotLender(address,address,address,address,string)._name](..contracts/StrategyImperamaxLender.sol#L77) is not in mixedCase

Parameter [StrategyImperamaxLender.liquidatePosition(uint256)._amountNeeded](..contracts/StrategyImperamaxLender.sol#L284) is not in mixedCase

Parameter [StrategyImperamaxLender.cloneTarotLender(address,address,address,address,string)._keeper](..contracts/StrategyImperamaxLender.sol#L76) is not in mixedCase

Parameter [StrategyImperamaxLender.wantSuppliedToPool(address)._pool](..contracts/StrategyImperamaxLender.sol#L157) is not in mixedCase

Parameter [StrategyImperamaxLender.emergencyWithdraw(uint256)._amountToWithdraw](..contracts/StrategyImperamaxLender.sol#L403) is not in mixedCase

Parameter [StrategyImperamaxLender.setStrategyParams(bool,uint256,uint256)._reorder](..contracts/StrategyImperamaxLender.sol#L559) is not in mixedCase

Parameter [StrategyImperamaxLender.prepareReturn(uint256)._debtOutstanding](..contracts/StrategyImperamaxLender.sol#L199) is not in mixedCase

Parameter [StrategyImperamaxLender.setStrategyParams(bool,uint256,uint256)._dustThreshold](..contracts/StrategyImperamaxLender.sol#L560) is not in mixedCase

Parameter [StrategyImperamaxLender.initialize(address,address,address,address,string)._name](..contracts/StrategyImperamaxLender.sol#L64) is not in mixedCase

Parameter [StrategyImperamaxLender.initialize(address,address,address,address,string)._vault](..contracts/StrategyImperamaxLender.sol#L60) is not in mixedCase

Parameter [StrategyImperamaxLender.manuallySetAllocations(uint256[])._ratios](..contracts/StrategyImperamaxLender.sol#L429) is not in mixedCase

Parameter [StrategyImperamaxLender.cloneTarotLender(address,address,address,address,string)._strategist](..contracts/StrategyImperamaxLender.sol#L74) is not in mixedCase

Parameter [StrategyImperamaxLender.cloneTarotLender(address,address,address,address,string)._rewards](..contracts/StrategyImperamaxLender.sol#L75) is not in mixedCase

Parameter [StrategyImperamaxLender.setForceHarvestTriggerOnce(bool)._forceHarvestTriggerOnce](..contracts/StrategyImperamaxLender.sol#L550) is not in mixedCase

Parameter [StrategyImperamaxLender.prepareMigration(address)._newStrategy](..contracts/StrategyImperamaxLender.sol#L414) is not in mixedCase

Contract [vaultAPIExtended](..contracts/StrategyImperamaxLender.sol#L13-L15) is not in CapWords

Parameter [StrategyImperamaxLender.attemptToRemovePool(address)._poolToRemove](..contracts/StrategyImperamaxLender.sol#L477) is not in mixedCase

Parameter [StrategyImperamaxLender.setStrategyParams(bool,uint256,uint256)._creditThreshold](..contracts/StrategyImperamaxLender.sol#L561) is not in mixedCase

Parameter [StrategyImperamaxLender.initialize(address,address,address,address,string)._rewards](..contracts/StrategyImperamaxLender.sol#L62) is not in mixedCase

Parameter [StrategyImperamaxLender.addTarotPool(address)._newPool](..contracts/StrategyImperamaxLender.sol#L464) is not in mixedCase


### Informational/High/pragma

Different versions of Solidity are used:
	- Version used: ['0.6.12', '>=0.6.0<0.7.0', '^0.6.0', '^0.6.2']
	- [0.6.12](..contracts/interfaces/IBorrowable.sol#L1)
	- [0.6.12](..contracts/interfaces/IRouter.sol#L1)
	- [0.6.12](..contracts/StrategyImperamaxLender.sol#L2)
	- [ABIEncoderV2](..contracts/StrategyImperamaxLender.sol#L3)
	- [^0.6.2](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/utils/Address.sol#L3)
	- [^0.6.0](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/token/ERC20/IERC20.sol#L3)
	- [^0.6.0](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/math/Math.sol#L3)
	- [^0.6.0](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/token/ERC20/SafeERC20.sol#L3)
	- [^0.6.0](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/math/SafeMath.sol#L3)
	- [>=0.6.0<0.7.0](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L2)
	- [ABIEncoderV2](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L3)


### Low/Medium/calls-loop

[StrategyImperamaxLender.getEachPoolUtilization()](..contracts/StrategyImperamaxLender.sol#L127-L138) has external calls inside a loop: [totalSupplied = IBorrowable(currentPool).totalBalance().add(totalBorrows)](..contracts/StrategyImperamaxLender.sol#L135)

[StrategyImperamaxLender.wantSuppliedToPool(address)](..contracts/StrategyImperamaxLender.sol#L157-L161) has external calls inside a loop: [bTokenBalance = IBorrowable(_pool).balanceOf(address(this))](..contracts/StrategyImperamaxLender.sol#L158)

[StrategyImperamaxLender.manuallySetAllocations(uint256[])](..contracts/StrategyImperamaxLender.sol#L429-L461) has external calls inside a loop: [IBorrowable(targetPool).mint(address(this))](..contracts/StrategyImperamaxLender.sol#L455)

[StrategyImperamaxLender.stakedBalance()](..contracts/StrategyImperamaxLender.sol#L110-L119) has external calls inside a loop: [bTokenBalance = IBorrowable(currentPool).balanceOf(address(this))](..contracts/StrategyImperamaxLender.sol#L115)

[StrategyImperamaxLender.manuallySetAllocations(uint256[])](..contracts/StrategyImperamaxLender.sol#L429-L461) has external calls inside a loop: [want.transfer(targetPool,toDeposit)](..contracts/StrategyImperamaxLender.sol#L454)

[StrategyImperamaxLender.wantSuppliedToPool(address)](..contracts/StrategyImperamaxLender.sol#L157-L161) has external calls inside a loop: [currentExchangeRate = IBorrowable(_pool).exchangeRateLast()](..contracts/StrategyImperamaxLender.sol#L159)

[StrategyImperamaxLender.getEachPoolUtilization()](..contracts/StrategyImperamaxLender.sol#L127-L138) has external calls inside a loop: [totalBorrows = IBorrowable(currentPool).totalBorrows()](..contracts/StrategyImperamaxLender.sol#L134)

[StrategyImperamaxLender.stakedBalance()](..contracts/StrategyImperamaxLender.sol#L110-L119) has external calls inside a loop: [currentExchangeRate = IBorrowable(currentPool).exchangeRateLast()](..contracts/StrategyImperamaxLender.sol#L116)

[StrategyImperamaxLender.balanceOfWant()](..contracts/StrategyImperamaxLender.sol#L105-L107) has external calls inside a loop: [want.balanceOf(address(this))](..contracts/StrategyImperamaxLender.sol#L106)


### Low/Medium/events-maths

[StrategyImperamaxLender.setStrategyParams(bool,uint256,uint256)](..contracts/StrategyImperamaxLender.sol#L558-L567) should emit an event for: 
	- [dustThreshold = _dustThreshold](..contracts/StrategyImperamaxLender.sol#L564) 
	- [creditThreshold = _creditThreshold](..contracts/StrategyImperamaxLender.sol#L566) 


### Low/Medium/reentrancy-benign

Reentrancy in [StrategyImperamaxLender.initialize(address,address,address,address,string)](..contracts/StrategyImperamaxLender.sol#L59-L69):
	External calls:
	- [_initialize(_vault,_strategist,_rewards,_keeper)](..contracts/StrategyImperamaxLender.sol#L67)
		- [returndata = address(token).functionCall(data,SafeERC20: low-level call failed)](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/token/ERC20/SafeERC20.sol#L69)
		- [want.safeApprove(_vault,uint256(- 1))](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L355)
		- [(success,returndata) = target.call{value: weiValue}(data)](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/utils/Address.sol#L123)
		- [vault.approve(rewards,uint256(- 1))](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L366)
	External calls sending eth:
	- [_initialize(_vault,_strategist,_rewards,_keeper)](..contracts/StrategyImperamaxLender.sol#L67)
		- [(success,returndata) = target.call{value: weiValue}(data)](../root/.brownie/packages/OpenZeppelin/openzeppelin-contracts@3.1.0/contracts/utils/Address.sol#L123)
	State variables written after the call(s):
	- [_initializeStrat(_name)](..contracts/StrategyImperamaxLender.sol#L68)
		- [dustThreshold = 100](..contracts/StrategyImperamaxLender.sol#L53)
	- [_initializeStrat(_name)](..contracts/StrategyImperamaxLender.sol#L68)
		- [maxReportDelay = 172800](..contracts/StrategyImperamaxLender.sol#L52)
	- [_initializeStrat(_name)](..contracts/StrategyImperamaxLender.sol#L68)
		- [stratName = _name](..contracts/StrategyImperamaxLender.sol#L56)

Reentrancy in [StrategyImperamaxLender.prepareReturn(uint256)](..contracts/StrategyImperamaxLender.sol#L199-L249):
	External calls:
	- [updateExchangeRates()](..contracts/StrategyImperamaxLender.sol#L209)
		- [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)
	State variables written after the call(s):
	- [reorderPools()](..contracts/StrategyImperamaxLender.sol#L213)
		- [(preventDeposits[low1],preventDeposits[high1]) = (preventDeposits[high1],preventDeposits[low1])](..contracts/StrategyImperamaxLender.sol#L187)

Reentrancy in [StrategyImperamaxLender.manuallySetAllocations(uint256[])](..contracts/StrategyImperamaxLender.sol#L429-L461):
	External calls:
	- [updateExchangeRates()](..contracts/StrategyImperamaxLender.sol#L440)
		- [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)
	- [_withdraw(type()(uint256).max)](..contracts/StrategyImperamaxLender.sol#L443)
		- [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)
		- [IBorrowable(currentPool).transfer(currentPool,balanceOfbToken)](..contracts/StrategyImperamaxLender.sol#L348)
		- [IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L349)
		- [IBorrowable(currentPool).transfer(currentPool,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L352)
		- [IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L353)
		- [IBorrowable(currentPool).transfer(currentPool,remainingbTokenNeeded)](..contracts/StrategyImperamaxLender.sol#L364)
		- [pulled = IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L365)
		- [IBorrowable(currentPool).transfer(currentPool,balanceOfbToken_scope_1)](..contracts/StrategyImperamaxLender.sol#L377)
		- [pulled_scope_0 = IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L378)
		- [IBorrowable(currentPool).transfer(currentPool,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L380)
		- [pulled_scope_0 = IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L381)
	State variables written after the call(s):
	- [reorderPools()](..contracts/StrategyImperamaxLender.sol#L460)
		- [(preventDeposits[low1],preventDeposits[high1]) = (preventDeposits[high1],preventDeposits[low1])](..contracts/StrategyImperamaxLender.sol#L187)


### Low/Medium/timestamp

[StrategyImperamaxLender.harvestTrigger(uint256)](..contracts/StrategyImperamaxLender.sol#L571-L591) uses timestamp for comparisons
	Dangerous comparisons:
	- [block.timestamp.sub(params.lastReport) > maxReportDelay](..contracts/StrategyImperamaxLender.sol#L575)


### Medium/High/incorrect-equality

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) uses a dangerous strict equality:
	- [require(bool,string)(params.debtRatio == 0 || vaultAPIExtended(address(vault)).emergencyShutdown() || _amountToWithdraw == type()(uint256).max,Low liquidity)](..contracts/StrategyImperamaxLender.sol#L396-L399)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) uses a dangerous strict equality:
	- [ableToPullInUnderlying == 0](..contracts/StrategyImperamaxLender.sol#L335)

[StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545) uses a dangerous strict equality:
	- [require(bool,string)(IBorrowable(_poolToRemove).balanceOf(address(this)) == 0,Balance not zero)](..contracts/StrategyImperamaxLender.sol#L502)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) uses a dangerous strict equality:
	- [params.debtRatio == 0 || _amountToWithdraw == type()(uint256).max || vaultAPIExtended(address(vault)).emergencyShutdown()](..contracts/StrategyImperamaxLender.sol#L343)


### Medium/High/tautology

[StrategyImperamaxLender._deposit(uint256)](..contracts/StrategyImperamaxLender.sol#L271-L282) contains a tautology or contradiction:
	- [i >= 0](..contracts/StrategyImperamaxLender.sol#L273)


### Medium/Medium/reentrancy-no-eth

Reentrancy in [BaseStrategy.setRewards(address)](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L419-L425):
	External calls:
	- [vault.approve(rewards,0)](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L421)
	State variables written after the call(s):
	- [rewards = _rewards](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L422)

Reentrancy in [StrategyImperamaxLender.manuallySetAllocations(uint256[])](..contracts/StrategyImperamaxLender.sol#L429-L461):
	External calls:
	- [updateExchangeRates()](..contracts/StrategyImperamaxLender.sol#L440)
		- [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)
	- [_withdraw(type()(uint256).max)](..contracts/StrategyImperamaxLender.sol#L443)
		- [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)
		- [IBorrowable(currentPool).transfer(currentPool,balanceOfbToken)](..contracts/StrategyImperamaxLender.sol#L348)
		- [IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L349)
		- [IBorrowable(currentPool).transfer(currentPool,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L352)
		- [IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L353)
		- [IBorrowable(currentPool).transfer(currentPool,remainingbTokenNeeded)](..contracts/StrategyImperamaxLender.sol#L364)
		- [pulled = IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L365)
		- [IBorrowable(currentPool).transfer(currentPool,balanceOfbToken_scope_1)](..contracts/StrategyImperamaxLender.sol#L377)
		- [pulled_scope_0 = IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L378)
		- [IBorrowable(currentPool).transfer(currentPool,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L380)
		- [pulled_scope_0 = IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L381)
	State variables written after the call(s):
	- [reorderPools()](..contracts/StrategyImperamaxLender.sol#L460)
		- [(pools[low1],pools[high1]) = (pools[high1],pools[low1])](..contracts/StrategyImperamaxLender.sol#L186)

Reentrancy in [StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545):
	External calls:
	- [IBorrowable(_poolToRemove).transfer(_poolToRemove,ableToPullInbToken)](..contracts/StrategyImperamaxLender.sol#L525)
	- [IBorrowable(_poolToRemove).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L526)
	State variables written after the call(s):
	- [i_scope_0 < boolHelperPool.length](..contracts/StrategyImperamaxLender.sol#L530)
	- [pools.push(addressHelperPool[i_scope_0])](..contracts/StrategyImperamaxLender.sol#L541)
	- [preventDeposits.push(true)](..contracts/StrategyImperamaxLender.sol#L533)
	- [preventDeposits.push(false)](..contracts/StrategyImperamaxLender.sol#L536)
	- [preventDeposits.push(true)](..contracts/StrategyImperamaxLender.sol#L539)

Reentrancy in [StrategyImperamaxLender.prepareReturn(uint256)](..contracts/StrategyImperamaxLender.sol#L199-L249):
	External calls:
	- [updateExchangeRates()](..contracts/StrategyImperamaxLender.sol#L209)
		- [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)
	State variables written after the call(s):
	- [reorderPools()](..contracts/StrategyImperamaxLender.sol#L213)
		- [(pools[low1],pools[high1]) = (pools[high1],pools[low1])](..contracts/StrategyImperamaxLender.sol#L186)

Reentrancy in [StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545):
	External calls:
	- [IBorrowable(_poolToRemove).transfer(_poolToRemove,balanceOfbToken)](..contracts/StrategyImperamaxLender.sol#L499)
	- [IBorrowable(_poolToRemove).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L500)
	State variables written after the call(s):
	- [i < boolHelperPool.length](..contracts/StrategyImperamaxLender.sol#L505)
	- [pools.push(addressHelperPool[i])](..contracts/StrategyImperamaxLender.sol#L516)
	- [preventDeposits.push(false)](..contracts/StrategyImperamaxLender.sol#L511)
	- [preventDeposits.push(true)](..contracts/StrategyImperamaxLender.sol#L514)


### Medium/Medium/unused-return

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L349)

[StrategyImperamaxLender._withdraw(uint256)](..contracts/StrategyImperamaxLender.sol#L310-L401) ignores return value by [IBorrowable(currentPool).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L353)

[StrategyImperamaxLender.manuallySetAllocations(uint256[])](..contracts/StrategyImperamaxLender.sol#L429-L461) ignores return value by [IBorrowable(targetPool).mint(address(this))](..contracts/StrategyImperamaxLender.sol#L455)

[StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545) ignores return value by [IBorrowable(_poolToRemove).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L500)

[StrategyImperamaxLender._deposit(uint256)](..contracts/StrategyImperamaxLender.sol#L271-L282) ignores return value by [IBorrowable(targetPool).mint(address(this))](..contracts/StrategyImperamaxLender.sol#L278)

[StrategyImperamaxLender.attemptToRemovePool(address)](..contracts/StrategyImperamaxLender.sol#L477-L545) ignores return value by [IBorrowable(_poolToRemove).redeem(address(this))](..contracts/StrategyImperamaxLender.sol#L526)

[StrategyImperamaxLender.updateExchangeRates()](..contracts/StrategyImperamaxLender.sol#L251-L258) ignores return value by [IBorrowable(targetPool).exchangeRate()](..contracts/StrategyImperamaxLender.sol#L256)


### Optimization/High/constable-states

[StrategyImperamaxLender.isOriginal](..contracts/StrategyImperamaxLender.sol#L38) should be constant

[BaseStrategyInitializable.isOriginal](../root/.brownie/packages/iearn-finance/yearn-vaults@0.4.3-1/contracts/BaseStrategy.sol#L906) should be constant




## Appendix C. Tests

### Tests result

`50 passed in 154.11s (0:02:34)`

### Tests coverage

| Function | Coverage |
|-|-|
| StrategyImperamaxLender._deposit | 100.0% |
| StrategyImperamaxLender._withdraw | 100.0% |
| StrategyImperamaxLender.addTarotPool | 100.0% |
| StrategyImperamaxLender.adjustPosition | 100.0% |
| StrategyImperamaxLender.cloneTarotLender | 100.0% |
| StrategyImperamaxLender.manuallySetAllocations | 100.0% |
| StrategyImperamaxLender.prepareMigration | 100.0% |
| StrategyImperamaxLender._reorderPools | 93.8% |
| StrategyImperamaxLender.prepareReturn | 93.8% |
| StrategyImperamaxLender.attemptToRemovePool | 91.7% |
| StrategyImperamaxLender.liquidatePosition | 81.2% |
| StrategyImperamaxLender.harvestTrigger | 79.2% |




**** FOR INTERNAL USE ONLY ****
