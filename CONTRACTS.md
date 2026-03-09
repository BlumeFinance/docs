# Contract Addresses

All contracts are deployed on [XRPL EVM Sidechain](https://docs.xrplevm.org/) and verified on Blockscout.

---

## Mainnet (Chain ID: 1440000)

Explorer: [explorer.xrplevm.org](https://explorer.xrplevm.org)

### BlumeSwap (AMM DEX)

| Contract | Address |
|----------|---------|
| Factory | [`0x0F0F367e1C407C28821899E9bd2CB63D6086a945`](https://explorer.xrplevm.org/address/0x0F0F367e1C407C28821899E9bd2CB63D6086a945) |
| Router | [`0x3a5FF5717fCa60b613B28610A8Fd2E13299e306C`](https://explorer.xrplevm.org/address/0x3a5FF5717fCa60b613B28610A8Fd2E13299e306C) |

### BlumePad (Token Launchpad)

| Contract | Address |
|----------|---------|
| Factory | [`0x8681A2566E35b14196cAc0E349DB585992d313BA`](https://explorer.xrplevm.org/address/0x8681A2566E35b14196cAc0E349DB585992d313BA) |

### Common Tokens

| Token | Address | Decimals |
|-------|---------|----------|
| WXRP | [`0x7C21a90E3eCD3215d16c3BBe76a491f8f792d4Bf`](https://explorer.xrplevm.org/address/0x7C21a90E3eCD3215d16c3BBe76a491f8f792d4Bf) | 18 |
| USDC | [`0xa16148c6Ac9EDe0D82f0c52899e22a575284f131`](https://explorer.xrplevm.org/address/0xa16148c6Ac9EDe0D82f0c52899e22a575284f131) | 6 |

---

## Testnet (Chain ID: 1449000)

Explorer: [explorer.testnet.xrplevm.org](https://explorer.testnet.xrplevm.org)

### BlumeSwap (AMM DEX)

| Contract | Address |
|----------|---------|
| Factory | [`0xa67Dfa5C47Bec4bBbb06794B933705ADb9E82459`](https://explorer.testnet.xrplevm.org/address/0xa67Dfa5C47Bec4bBbb06794B933705ADb9E82459) |
| Router | [`0xC17E3517131E7444361fEA2083F3309B33a7320A`](https://explorer.testnet.xrplevm.org/address/0xC17E3517131E7444361fEA2083F3309B33a7320A) |

### BlumePad (Token Launchpad)

| Contract | Address |
|----------|---------|
| Factory | [`0xe7a942B532333761641f3326a4583956C557Bee2`](https://explorer.testnet.xrplevm.org/address/0xe7a942B532333761641f3326a4583956C557Bee2) |

### BlumeLend (Lending)

| Contract | Address |
|----------|---------|
| BlumeLend | [`0x266f283A2FEA75304B132Cb9F3b795B6266A8Ec1`](https://explorer.testnet.xrplevm.org/address/0x266f283A2FEA75304B132Cb9F3b795B6266A8Ec1) |
| AdaptiveCurveIrm | [`0x814fC6b1E07F16aCB536aCc262Fae66114ddDD72`](https://explorer.testnet.xrplevm.org/address/0x814fC6b1E07F16aCB536aCc262Fae66114ddDD72) |
| BandOracleAdapter | [`0xBBE1b60a438Da8f04ef1031d4604eD31F5935c4E`](https://explorer.testnet.xrplevm.org/address/0xBBE1b60a438Da8f04ef1031d4604eD31F5935c4E) |

### Common Tokens (Testnet)

| Token | Address | Decimals |
|-------|---------|----------|
| WXRP | [`0x664950b1F3E2FAF98286571381f5f4c230ffA9c5`](https://explorer.testnet.xrplevm.org/address/0x664950b1F3E2FAF98286571381f5f4c230ffA9c5) | 18 |
| MockUSDC | [`0xC6dD7E13EeEBE873e24716426687c303A2A4489c`](https://explorer.testnet.xrplevm.org/address/0xC6dD7E13EeEBE873e24716426687c303A2A4489c) | 6 |

---

## Notes

- All contracts are verified on Blockscout — ABIs are available directly from the explorer.
- BlumeLend is testnet-only. Mainnet deployment is pending.
- BlumePad tokens are ERC-20 with bonding curve mechanics. After graduation, they trade on BlumeSwap like any other token.
- BlumeSwap Router wraps/unwraps native XRP automatically via `*ETH` method variants (e.g., `swapExactETHForTokens`).
