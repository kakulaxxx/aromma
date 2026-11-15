# Aromma: token launchpad on GIWA Sepolia

Aromma is a community launchpad built with [`@ammora-protocol/sdk`](https://www.npmjs.com/package/@ammora-protocol/sdk) 0.3.9. Anyone can launch an ERC-20 on an Ammora ALC bonding curve, trade it with ETH, graduate it into an Ammora concentrated-liquidity pool, keep trading it there, and claim creator fees.

Live app: https://kakulaxxx.github.io/aromma/ · Network: GIWA Sepolia testnet (chain 91342). Testnet only.

## Features

**Launch**
- Name, symbol, image, description and website. Uses `buildCreateAmmoraV4Launch` against the V4 LaunchFactory.
- The selected Config is read live with `getAmmoraV4ConfigAtBlock`; the result is confirmed with `parseAmmoraV4LaunchCreatedReceipt`.
- `verifyAmmoraV4CoreAtBlock` runs before every launch.

**Trade on the bonding curve**
- Quotes with `AmmoraV4LaunchCurveClient.quoteExactInputAtBlock`, fills classified with `classifyAmmoraV4ExactInputFill` (graduation-boundary buys supported).
- Slippage-bound `swapExactInput`, exact approvals, simulation before signing.
- Pay with ETH: the app wraps only the missing WETH before the buy.
- Quick amounts, minimum received, fee and price impact (from `nextSqrtPrice`).
- Each curve is checked against the factory's `tokenForCurve` and the reviewed fee policy before trading is enabled.

**Graduation and pool trading**
- When a curve reaches `ReadyToMigrate`, anyone can run `planMigrateAtBlock` from the token page.
- Graduated tokens trade in their Ammora pool through `AmmoraSwapClient.prepareExactIn` (concentrated, with compounding fallback).

**Creator fees**
- Portfolio lists your launches, reads each FeeVault through `launchContext`, shows `claimable` WETH and token fees, and claims with `claimFor`.

**Explore and token pages**
- All `LaunchCreated` events from the Ammora V4 factory, with filters: New, Near graduation, Top raised, Graduated.
- Token page: price, market cap, holders, trade count, price chart from curve `Swap` events, recent activity and top holders.
- Share on X, copy address, links to the GIWA faucet and bridge.

## Tech

One static `index.html`. viem and the Ammora SDK load as ES modules from esm.sh, so there is no build step and no backend. Reads are batched through Multicall3 because the public RPC is rate limited. Launch history, trades and holders come from the GIWA explorer API.

Aromma is an independent project and is not operated by Ammora Labs.
