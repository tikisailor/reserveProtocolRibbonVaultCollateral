## Collateral plugin for Ribbon Finance Earn Vaults (rEARN-USDC and rEARN-stETH)

This repository contains Ribbon finance V2 earn vault collateral plugins for Reserve Protocol, submitted under the Gitcoin Reserve Launch Hackathon by Tiki#0503. The chosen vaults have high (100% and 99.5% respectively) capital protection, which makes them ideal collateral for Reserve Protocol.

## rEARN-USDC collateral

### Units

`tok` : `rEARN`
`ref` : `USDC`
`target` : `USD`
`UoA` : `USD`

### Testing

clone Reserve Protocol, then: 
- Place contents of `.RibbonEarnUsdcCollateralPlugin/assets` into `./contracts/plugins/assets`
- Place contents of `.RibbonEarnUsdcCollateralPlugin/mocks` into `./contracts/plugins/mocks`
- Place contents of `.RibbonEarnUsdcCollateralPlugin/test` into `./test/integration/individual-collateral`

in `.env` set 
`FORK=1`
`MAINNET_BLOCK=16000000` - to make sure the vault exists

run `yarn`
run `yarn compile`
run `yarn test:integration`

### Deployment

`chainlinkFeed_` expects usdc/usd feed (8 decimals) - - Ethereum Mainnet: `0x8fFfFfd4AfB6115b954Bd326cbe7B4BA576818f6`
`erc20_` expects Ribbon USDC Earn Vault (rEARN) - Ethereum Mainnet: `0x84c2b16FA6877a8fF4F3271db7ea837233DFd6f0`
`fallbackPrice_` should be set to 1e18


## rEARN-stETH collateral
