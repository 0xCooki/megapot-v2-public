# MegaPot V2 Contracts

MegaPot V2 is a decentralized jackpot protocol where users purchase NFT-based jackpot tickets and liquidity providers fund prize pools. The system uses Pyth Network entropy for provably fair drawings, automatically distributes winnings based on number matches, and includes cross-chain bridge functionality. The protocol is accessible across multiple chains, including Base, Ethereum, BNB Chain, Arbitrum, and more.

## Architecture

For how the contracts fit together, see [docs/architecture.md](docs/architecture.md).

## Deployed on Base

These are the Base mainnet (chain 8453) deployments.

| Contract | Address |
| --- | --- |
| [Jackpot](contracts/Jackpot.sol) | [`0x3bAe643002069dBCbcd62B1A4eb4C4A397d042a2`](https://basescan.org/address/0x3bAe643002069dBCbcd62B1A4eb4C4A397d042a2) |
| [JackpotLPManager](contracts/JackpotLPManager.sol) | [`0xE63E54DF82d894396B885CE498F828f2454d9dCf`](https://basescan.org/address/0xE63E54DF82d894396B885CE498F828f2454d9dCf) |
| [JackpotTicketNFT](contracts/JackpotTicketNFT.sol) | [`0x48FfE35AbB9f4780a4f1775C2Ce1c46185b366e4`](https://basescan.org/address/0x48FfE35AbB9f4780a4f1775C2Ce1c46185b366e4) |
| [TicketNFTArt](contracts/nftArt/TicketNFTArt.sol) | [`0xb41d8679772995e3b72bbaDDA4909eCa9DFF70fB`](https://basescan.org/address/0xb41d8679772995e3b72bbaDDA4909eCa9DFF70fB) |
| [GuaranteedMinimumPayoutCalculator](contracts/GuaranteedMinimumPayoutCalculator.sol) | [`0x97a22361b6208aC8cd9afaea09D20feC47046CBD`](https://basescan.org/address/0x97a22361b6208aC8cd9afaea09D20feC47046CBD) |
| [ScaledEntropyProvider](contracts/ScaledEntropyProvider.sol) | [`0x5D030DEC2e0d38935e662C0d2feD44B050c8Ae51`](https://basescan.org/address/0x5D030DEC2e0d38935e662C0d2feD44B050c8Ae51) |
| [JackpotBridgeManager](contracts/JackpotBridgeManager.sol) | [`0x7FD89658D8B4dD42FA2c1acDe0B6b877Bea03A80`](https://basescan.org/address/0x7FD89658D8B4dD42FA2c1acDe0B6b877Bea03A80) |
| [JackpotAutoSubscription](contracts/JackpotAutoSubscription.sol) | [`0x2694Bd48f3e6B4775943067DC842C93bf5F19DcD`](https://basescan.org/address/0x2694Bd48f3e6B4775943067DC842C93bf5F19DcD) |
| [BatchPurchaseFacilitator](contracts/BatchPurchaseFacilitator.sol) | [`0xBA343479D98a1Ed333899999D95a7343B808a76F`](https://basescan.org/address/0xBA343479D98a1Ed333899999D95a7343B808a76F) |
| [TicketAutoCompoundVault](contracts/TicketAutoCompoundVault.sol) | [`0xfa6a75366E0A9dF56d67E4B4141050b438DB2A5E`](https://basescan.org/address/0xfa6a75366E0A9dF56d67E4B4141050b438DB2A5E) |
| [JackpotRandomTicketBuyer](contracts/JackpotRandomTicketBuyer.sol) | [`0xb9560b43b91dE2c1DaF5dfbb76b2CFcDaFc13aBd`](https://basescan.org/address/0xb9560b43b91dE2c1DaF5dfbb76b2CFcDaFc13aBd) |
| [FisherYatesRejection](contracts/lib/FisherYatesWithRejection.sol) | [`0x9a9de5b407ceB9EBBf6A58256DFb350a32D52850`](https://basescan.org/address/0x9a9de5b407ceB9EBBf6A58256DFb350a32D52850) |

## Prerequisites

- Node.js 20+
- Yarn package manager
- Git

## Installation

```bash
git clone git@github.com:coordinationlabs/megapot-v2-public.git
cd megapot-v2-public
yarn install
```

## Development Commands

### Compilation

```bash
# Clean and compile contracts
yarn build

# Compile only
yarn compile

# Clean artifacts and cache
yarn clean
```

### Testing

#### Basic Testing

```bash
# Run all tests
yarn test

# Run tests with fresh compilation
yarn test:clean

# Run tests without compilation (faster)
yarn test:fast
```
### Coverage

#### Full Coverage (Memory Intensive)

```bash
# Run coverage on all contracts (requires 4GB+ memory)
yarn coverage
```

#### Parallel Coverage (Recommended)

For better performance and memory management, run coverage on specific test groups:

```bash
# Library tests
yarn hardhat coverage --testfiles "test/lib/*.spec.ts"

# Core jackpot contract
yarn hardhat coverage --testfiles "test/jackpot.spec.ts"

# Ecosystem contracts (LP Manager, Bridge, NFT)
yarn hardhat coverage --testfiles "{test/jackpotBridgeManager.spec.ts,test/jackpotLPManager.spec.ts,test/jackpotTicketNFT.spec.ts}"

# Utility contracts (Payout Calculator, Entropy Provider)
yarn hardhat coverage --testfiles "{test/guaranteedMinimumPayoutCalculator.spec.ts,test/scaledEntropyProvider.spec.ts}"
```

#### Memory Considerations

For large coverage, you will need to increase Node.js memory:

```bash
NODE_OPTIONS="--max-old-space-size=6144" yarn coverage
```

#### Coverage Configuration

Coverage excludes interface and mock contracts (see `.solcover.js`). The configuration includes:
- Yul optimizer enabled for accurate gas reporting
- 4-minute timeout for complex test scenarios
- Optimized compiler settings for coverage runs

## Project Structure

```
contracts/
├── Jackpot.sol                            # Main jackpot contract
├── JackpotLPManager.sol                   # Liquidity provider management
├── JackpotTicketNFT.sol                   # Ticket NFT contract
├── GuaranteedMinimumPayoutCalculator.sol  # Prize calculations
├── ScaledEntropyProvider.sol              # Pyth randomness provider
├── JackpotBridgeManager.sol               # Cross-chain bridge
├── JackpotAutoSubscription.sol            # Prepaid multi-drawing purchases
├── BatchPurchaseFacilitator.sol           # Large single-drawing orders split across transactions
├── TicketAutoCompoundVault.sol            # Auto-compound winnings vault
├── JackpotRandomTicketBuyer.sol           # Random-number buyTickets wrapper
├── nftArt/                                # On-chain ticket SVG metadata
├── interfaces/                            # Contract interfaces
├── lib/                                   # Utility libraries
├── external/                              # Vendored helpers
└── mocks/                                 # Test mock contracts

test/
├── jackpot.spec.ts                        # Main contract tests
├── jackpotLPManager.spec.ts               # LP management tests
├── jackpotTicketNFT.spec.ts               # Ticket NFT tests
├── ticketNFTArt.spec.ts                   # Ticket NFT art tests
├── guaranteedMinimumPayoutCalculator.spec.ts  # Payout tests
├── scaledEntropyProvider.spec.ts          # Entropy provider tests
├── jackpotBridgeManager.spec.ts           # Bridge tests
├── jackpotAutoSubscription.spec.ts        # Auto-subscription tests
├── batchPurchaseFacilitator.spec.ts       # Batch purchase tests
├── ticketAutoCompoundVault.spec.ts        # Auto-compound vault tests
├── jackpotRandomTicketBuyer.spec.ts       # Random buyer tests
├── e2e/                                   # Full-drawing scenarios
└── lib/                                   # Library tests

utils/
├── deploys.ts                 # Deployment helpers and scripts
├── protocolUtils.ts           # Utilities for interacting with the protocol
├── constants.ts               # Common constants (addresses, config)
├── contracts.ts               # Contract type imports
├── types.ts                   # Shared TypeScript types for protocol objects
├── common/                    # Utilities for blockchain interaction and units
└── test/                      # Fixtures and other utilities for test setup and tear down

```