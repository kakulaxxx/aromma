# Aromma: token launchpad on GIWA Sepolia

Aromma is a community launchpad built with [`@ammora-protocol/sdk`](https://www.npmjs.com/package/@ammora-protocol/sdk) 0.3.9. Anyone can launch an ERC-20 on an Ammora ALC bonding curve, trade it with WETH, and graduate it into an Ammora concentrated-liquidity pool.

Live app: GitHub Pages link of this repo. Network: GIWA Sepolia testnet (chain 91342). Testnet only.

## Features

- **Launch a token**: name, symbol, image and description. Uses `buildCreateAmmoraV4Launch` against the V4 LaunchFactory, reads the selected Config live with `getAmmoraV4ConfigAtBlock`, and confirms the result with `parseAmmoraV4LaunchCreatedReceipt`.
- **Explore launches**: every `LaunchCreated` event from the Ammora V4 factory, with live curve status and graduation progress from `AmmoraV4LaunchCurveClient.getStateAtBlock`.
- **Buy and sell on the curve**: quotes with `quoteExactInputAtBlock`, fill classification with `classifyAmmoraV4ExactInputFill` (graduation-boundary buys supported), slippage-bound `swapExactInput` calldata, exact approvals and a simulation before signing.
- **Graduation**: when a curve reaches `ReadyToMigrate`, anyone can run `planMigrateAtBlock` from the token page.
- **Safety checks**: `verifyAmmoraV4CoreAtBlock` before launching; each curve is checked against the factory's `tokenForCurve` and the reviewed fee policy before trading is enabled.
- **Wallet**: connect any injected wallet, auto-add GIWA Sepolia, wrap and unwrap ETH/WETH, list your own launches.

## Tech

A single static `index.html`. viem and the Ammora SDK are loaded as ES modules from esm.sh, so there is no build step and no backend. Launch history comes from the GIWA explorer log API, with an RPC fallback.

Aromma is an independent project and is not operated by Ammora Labs.
