# Findings / items

Here is a list of issues/suggestions as well as some more open questions/observations about the code:

List of issues/suggestions:
    1) In _initializeStrategy, sanToken is granted infinite approval, which as far as I can tell is not really needed.
    2) In name, the suggested naming convention (according to the yearn strategy example) has "Strategy" as a prefix.
    3) The _swap function used to swap ANGLE tokens back to want for reinvestment, can be susceptible to financial manipulation. In order for this to be economically viable, the swap amount should be significant. For this reason, harvest should be called frequently enough in order to make such attacks unprofitable.
    4) Inconsistent use of SafeERC20: prepareMigration does make use of the library, while the rest of the code does.
    5) It seems logical to introduce the withdraw counterpart to the depositToStableMaster function.
    6) It's good practice to add sanity checks to the setters: setTreasury could check that the provided address is not zero and setKeepInBips could verify that the new value is between 0 and _denominator (=10000)

## Questions / observations

    1) In estimatedTotalAssets, the ANGLE tokens are not taken into account. From my understanding, this is because these tokens are always "transient": they are acquired and swapped immediately (prepareReturn). Just thinking out loud to verify my understanding here.
    2) Shouldn't the gaugeTokens be included in the protectedTokens list?