## Collateral plugin for Ribbon Finance Earn Vaults (rEARN-USDC and rEARN-stETH)

This repository contains Ribbon finance V2 [earn vault](https://docs.ribbon.finance/ribbon-earn/introduction-to-ribbon-earn) collateral plugins for Reserve Protocol, submitted under the Gitcoin Reserve Launch Hackathon by Tiki#0503. The chosen vaults have high (100% and 99.5% respectively) capital protection, which makes them ideal collateral for Reserve Protocol.

All tests were run against master, commit 2a8e112ef0dc1b537077bbf367abef8236031596 on Reserve Protocol. There is a repo with the plugin in context [here](https://github.com/tikisailor/ReserveProtocol/tree/feature/ribbon-earn-collateral)

## rEARN-USDC collateral

### Units
  
- `tok` : `rEARN`
- `ref` : `USDC`
- `target` : `USD`
- `UoA` : `USD`
  
### Testing

Clone Reserve Protocol, then: 
  
- Place contents of `.RibbonEarnUsdcCollateralPlugin/assets` into `./contracts/plugins/assets`
- Place contents of `.RibbonEarnUsdcCollateralPlugin/mocks` into `./contracts/plugins/mocks`
- Place contents of `.RibbonEarnUsdcCollateralPlugin/test` into `./test/integration/individual-collateral`
  
In `.env` set 
  
- `FORK=1`  
- `MAINNET_BLOCK=16000000` - to make sure the vault exists

The test expects the following entries in `./common/configuration.ts`
  
- `networkConfig['31337'].tokens['rEARN] = '0x84c2b16FA6877a8fF4F3271db7ea837233DFd6f0'` 
- `networkConfig['1'].tokens['rEARN] = '0x84c2b16FA6877a8fF4F3271db7ea837233DFd6f0'`

Install, compile, run
  
run `yarn`  
run `yarn compile`  
run `yarn test:integration`  
  
### Deployment (Ethereum mainnet)

The following constructor parameters are expected
  
- `fallbackPrice_` should be set to 1e18
- `chainlinkFeed_` expects USDC/USD feed (8 decimals) - `0x8fFfFfd4AfB6115b954Bd326cbe7B4BA576818f6`
- `erc20_` expects Ribbon USDC Earn Vault (rEARN) - `0x84c2b16FA6877a8fF4F3271db7ea837233DFd6f0`
- `maxTradeVolume_` {UoA} The max trade volume, in UoA
- `oracleTimeout_` {s} The number of seconds until a oracle value becomes invalid
- `targetName_` bytes32 formatted string of target symbol (USD)
- `delayUntilDefault_` {s} The number of seconds the collatral is allowed to stay IFFY
- `defaultThreshold_` {%} how much price of ref (stEth) can fall below target (Eth). E.g. `0.05`


## rEARN-stETH collateral

### Units

- `tok` : `rEARN-stETH`
- `ref` : `stETH`
- `target` : `ETH`
- `UoA` : `USD`

### Testing

Clone Reserve Protocol, then: 

- Place contents of `.RibbonEarnStEthCollateralPlugin/assets` into `./contracts/plugins/assets`
- Place contents of `.RibbonEarnStEthCollateralPlugin/mocks` into `./contracts/plugins/mocks`
- Place contents of `.RibbonEarnStEthCollateralPlugin/test` into `./test/integration/individual-collateral`

In `.env` set 

- `FORK=1`
- `MAINNET_BLOCK=16150061` - to make sure the vault exists

The test expects the following entries in `./common/configuration.ts`

- `networkConfig['31337'].tokens['rEARN_STETH] = '0xCE5513474E077F5336cf1B33c1347FDD8D48aE8c'`
- `networkConfig['1'].tokens['rEARN_STETH] = '0xCE5513474E077F5336cf1B33c1347FDD8D48aE8c'`
- `networkConfig['31337'].tokens['STETH] = '0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84'` 
- `networkConfig['1'].tokens['STETH] = '0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84'`
- `networkConfig['1'].chainlinkFeeds['STETH] = '0xcfe54b5cd566ab89272946f602d76ea879cab4a8'`
- `networkConfig['131337].chainlinkFeeds['STETH] = '0xcfe54b5cd566ab89272946f602d76ea879cab4a8'`

Install, compile, run
  
run `yarn`  
run `yarn compile`  
run `yarn test:integration`  

#### Mocking rEARN-stETH

Since the vault just came live days before submission of this collateral plugin, there 
were no rewards yet to test against. Instead there is two tests that mock rEARN-stETH
to simulate appreciation / depreciation of rEARN-stETH. They are switched off by default.
To run it, switch them on (category "Issuance/Appreciation/Redemption") and set variable `MOCK=true` in 
`.RibbonEarnStEthCollateralPlugin/test/RibbonEarnStEthCollateral.test.ts`. This will
cause some other tests to fail.

### Deployment (Ethereum mainnet)

The following constructor parameters are expected

- `fallbackPrice_` not used but has to be set to a non zero number
- `chainlinkFeed_` stETH/USD feed (8 decimals) - `0xcfe54b5cd566ab89272946f602d76ea879cab4a8`
- `erc20_` Ribbon stETH Earn Vault (rEARN-stETH) - `0xCE5513474E077F5336cf1B33c1347FDD8D48aE8c`
- `maxTradeVolume_` {UoA} The max trade volume, in UoA
- `oracleTimeout_` {s} The number of seconds until a oracle value becomes invalid
- `targetName_` bytes32 formatted string of target symbol (ETH)
- `delayUntilDefault_` {s} The number of seconds the collatral is allowed to stay IFFY
- `defaultThreshold_` {%} how much price of ref (stEth) can fall below target (Eth). E.g. `0.05`
- `chainlinkFeedFallback_` ETH/USD feed (8 decimals) - `0x5f4ec3df9cbd43714fe2740f5e3616155c5b8419`
- `volatilityBuffer_` {%} since the underlying rEARN-stETH strategy is 99.5% capital protected, 
we don't want the collateral to default during temporary drawdown. `volatilityBuffer_` defines 
the amount of revenue we will hide from the protocol in order to avoid default. However, due to 
how Reserve Protocol works, this value has to be rather small. A reasonable value seems to be `0.02` 
which would equate to four consecutive in the money (losing) trades. More info [here](https://github.com/reserve-protocol/protocol/blob/master/docs/collateral.md#revenue-hiding)

## How to get your vault shares from Ribbon Finance

The steps in volved to claim your rEARN and rEARN-stETH tokens can be found [here](https://docs.ribbon.finance/theta-vault/how-to-transfer-vault-positions)








