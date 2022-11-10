# Findings / items

- [Low] weth constant is unusued
- [Low] ethToWant does not do a conversion. It is used in BaseStrategy::harvestTrigger, so it seems logical to attempt and provide an accurate estimation here.
- [Low] Adding a sanity check in setPoolFee that there exists a pool for the triple (STG, USDC, newPoolFee) would help prevent against human error. E.g. it could call getPool on UniswapV3Factory (0x1F98431c8aD98523631AE4a59f267346ea31F984).
- [Opt] Requires at the end of the constructor could check _uniV3Swapper and _curvePool to get rid of the casts and save some gas
- [Opt] In prepareReturn, the assignment _totalAssets = estimatedTotalAssets(); after the if (_toLiquidate > _wantBalance) {...] can be moved inside the if body to save some gas.
- [Opt] In prepareReturn, the assignment _totalAssets = estimatedTotalAssets(); after the if (_toLiquidate > _wantBalance) {...] can be moved inside the if body to save some gas.
- [Opt] In _sellRewardsUniv3, you could call the more specialized uniV3Swapper.exactInputSingle(params); instead of the more general exactInput.
- [Opt] In adjustPosition, the if (_amountToDeposit > 0) { ... } if-statement will always be taken.
- [Opt] In _withdrawFromLP, the min operation at the start of the function is a no-op, as the function is always called with _lpAmount = balanceOfUnstakedLPToken()

## Questions

- I noticed that there is no slippage check on the Uniswap swap, unlike the Curve logic. Is there a reason for this asymmetry?