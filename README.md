# 🚀 SpeedRun Ethereum - Challenge 5: Build a DEX

A complete implementation of a **Decentralized Exchange (DEX)** with an **Automated Market Maker (AMM)** on the Sepolia testnet. This project demonstrates how decentralized token swaps work using the constant product formula (x*y=k).

## 📋 Project Overview

This repository contains a fully functional DEX that allows users to:
- **Swap tokens**: Exchange ETH for Balloons ($BAL) tokens and vice versa
- **Provide liquidity**: Add ETH and $BAL to earn trading fees as a Liquidity Provider
- **Withdraw liquidity**: Remove liquidity and receive proportional token amounts
- **Trade with slippage protection**: Automatic price adjustment based on market conditions

## 🎯 What We Built

### Smart Contracts (Solidity)

#### 1. **Balloons.sol** - ERC20 Token
- Standard ERC20 implementation
- Mints 1000 BAL tokens to the deployer on initialization
- Used as the paired token for ETH in the DEX

#### 2. **DEX.sol** - Automated Market Maker
A complete AMM implementation with the following functions:

| Function | Description |
|----------|-------------|
| `init(tokens)` | Initialize DEX with ETH and Balloons at 1:1 ratio |
| `price(xInput, xReserves, yReserves)` | Calculate output amount using constant product formula with 0.3% fee |
| `ethToToken()` | Swap ETH for Balloons tokens (payable) |
| `tokenToEth(tokenInput)` | Swap Balloons tokens for ETH |
| `deposit()` | Add liquidity and receive LP tokens (payable) |
| `withdraw(amount)` | Remove liquidity by burning LP tokens |
| `getLiquidity(lp)` | Query liquidity balance for an address |

### Key Features

✅ **Constant Product Formula**: x*y=k maintains market equilibrium  
✅ **Trading Fees**: 0.3% fee per transaction incentivizes liquidity providers  
✅ **Slippage Protection**: Automatic price adjustment prevents manipulation  
✅ **Liquidity Pools**: Users earn fees by providing liquidity  
✅ **LP Tokens**: Tracking liquidity provider positions  
✅ **Full Verification**: All contracts verified on Etherscan  

## 📊 Smart Contract Architecture

```
DEX Contract
├── State Variables
│   ├── token (IERC20) - Reference to Balloons token
│   ├── totalLiquidity (uint256) - Total LP tokens in circulation
│   └── liquidity (mapping) - LP token balance per user
│
├── Core Functions
│   ├── init() - Initialize with reserves
│   ├── price() - Calculate swap output
│   ├── ethToToken() - Swap execution
│   ├── tokenToEth() - Swap execution
│   ├── deposit() - Liquidity provision
│   ├── withdraw() - Liquidity removal
│   └── getLiquidity() - Balance query
│
└── Events
    ├── EthToTokenSwap
    ├── TokenToEthSwap
    ├── LiquidityProvided
    └── LiquidityRemoved
```

## 🌐 Deployed Addresses (Sepolia Testnet)

| Contract | Address | Link |
|----------|---------|------|
| **Balloons Token** | `0x3eb368Dba6b2Af761F27820642FaDA77362112a0` | [Etherscan](https://sepolia.etherscan.io/token/0x3eb368Dba6b2Af761F27820642FaDA77362112a0) |
| **DEX Contract** | `0x3aD1Fc4C4aae3739152C420157Ccd7338E81622d` | [Etherscan](https://sepolia.etherscan.io/address/0x3aD1Fc4C4aae3739152C420157Ccd7338E81622d) |

**Status**: ✅ Both contracts verified on Etherscan

## 🚀 Live Frontend

**Production URL**: https://speedrunqdex.vercel.app/

The frontend is deployed on Vercel and connects to the Sepolia testnet. Users can interact with the DEX through an intuitive web interface.

## 🏗️ Project Structure

```
challenge-dex/
├── packages/
│   ├── hardhat/
│   │   ├── contracts/
│   │   │   ├── Balloons.sol
│   │   │   └── DEX.sol
│   │   ├── deploy/
│   │   │   └── 00_deploy_dex.ts
│   │   ├── test/
│   │   │   └── DEX.test.ts
│   │   └── hardhat.config.ts
│   │
│   └── nextjs/
│       ├── app/
│       │   ├── page.tsx
│       │   ├── dex/
│       │   ├── debug/
│       │   └── layout.tsx
│       ├── components/
│       ├── scaffold.config.ts
│       └── public/
│
└── README.md
```

## 🔧 Setup & Installation

### Prerequisites

- **Node.js** >= v20.18.3
- **Yarn** v1 or v2+
- **Git**
- **MetaMask** or any Web3 wallet

### Step 1: Clone & Install

```bash
git clone https://github.com/yourusername/speedRun-Ethereum-Quest5.git
cd speedRun-Ethereum-Quest5
cd challenge-dex
yarn install
```

### Step 2: Local Development

#### Terminal 1 - Start Local Blockchain

```bash
yarn chain
```

This starts Hardhat's local Ethereum node at `http://localhost:8545`

#### Terminal 2 - Deploy Contracts Locally

```bash
yarn deploy
```

This deploys both Balloons and DEX contracts to the local network.

#### Terminal 3 - Start Frontend

```bash
yarn start
```

Frontend will be available at `http://localhost:3000`

## 🧪 Testing

Run the automated test suite:

```bash
yarn hardhat:test
```

Expected output: **17/17 tests passing** ✅

Tests validate:
- Contract initialization
- Price calculation accuracy
- Token swaps (both directions)
- Liquidity provision and withdrawal
- Fee collection
- Event emissions

## 🌍 Deploying to Sepolia Testnet

### Step 1: Get Test ETH

Visit a Sepolia faucet to get test ETH:
- [QuickNode Faucet](https://faucet.quicknode.com/ethereum/sepolia)
- [Alchemy Faucet](https://www.alchemy.com/faucets/ethereum-sepolia)
- [Sepolia Faucet](https://sepoliafaucet.com)

### Step 2: Configure Hardhat

Edit `packages/hardhat/hardhat.config.ts`:

```typescript
defaultNetwork: "sepolia"
```

### Step 3: Generate Deployer Account

```bash
yarn generate
```

This creates a new wallet. Fund it with test ETH from the faucet.

### Step 4: Deploy to Sepolia

```bash
yarn deploy --network sepolia
```

Contracts will be deployed to Sepolia testnet.

### Step 5: Verify Contracts

```bash
yarn verify --network sepolia
```

Contracts become publicly visible on Etherscan.

### Step 6: Deploy Frontend to Vercel

```bash
cd packages/nextjs
yarn next:build
vercel --prod
```

Follow the Vercel prompts to deploy. Your frontend will be live!

## 💡 How the AMM Works

### Constant Product Formula: x*y=k

The DEX uses the XY=K formula where:
- **x** = ETH reserves
- **y** = Balloons reserves
- **k** = constant invariant

When a user swaps tokens:
1. Input tokens are added to reserves
2. Output tokens are calculated to maintain k
3. 0.3% fee is applied to the input amount
4. Output is transferred to the user

**Example**:
```
Initial State: 
- ETH reserve = 5
- BAL reserve = 5
- k = 5 * 5 = 25

User swaps 1 ETH:
- Input with fee = 1 * 997 / 1000 = 0.997 ETH
- New ETH reserve = 5 + 0.997 = 5.997
- Required BAL output = (0.997 * 5) / (5 + 0.997) = 0.829 BAL
- New BAL reserve = 5 - 0.829 = 4.171
- Check: 5.997 * 4.171 ≈ 25 ✅
```

### Liquidity Provision

When a user deposits liquidity:
1. They send equal proportions of both assets
2. They receive LP tokens representing their share
3. They earn 0.3% of all trades proportional to their share
4. They can withdraw anytime to claim their share + earned fees

## 📱 Using the Frontend

1. **Connect Wallet**
   - Visit https://speedrunqdex.vercel.app/
   - Click "Connect Wallet"
   - Select your Web3 wallet (MetaMask, etc.)
   - Ensure you're on the Sepolia testnet

2. **Get Test Tokens**
   - You start with 10 Balloons ($BAL)
   - Get test ETH from a faucet

3. **Swap Tokens**
   - Go to "DEX" tab
   - Enter amount to swap
   - View estimated output and fees
   - Confirm transaction

4. **Provide Liquidity**
   - Go to "Pools" tab
   - Deposit equal proportions of ETH and BAL
   - Receive LP tokens
   - Earn trading fees

5. **Withdraw Liquidity**
   - View your LP position
   - Withdraw your share + earned fees

## 🔐 Security Considerations

- ✅ Re-entrancy protection via state updates before external calls
- ✅ Input validation on all functions
- ✅ Safe transfer patterns for ERC20 tokens
- ✅ No floating point arithmetic (all integers)
- ✅ Verified on Etherscan

## 📚 Learning Outcomes

By completing this challenge, you'll understand:

1. **How AMMs work** - The mechanics behind protocols like Uniswap
2. **Constant product formula** - x*y=k and its implications
3. **Liquidity pools** - How LP tokens work and fee distribution
4. **Slippage** - Why large trades impact price more
5. **Solidity best practices** - Smart contract patterns and security
6. **Web3 integration** - Frontend interaction with smart contracts
7. **Testnet deployment** - Deploying to public networks
8. **Contract verification** - Making contracts transparent on Etherscan

## 🎓 Key Formulas

### Price Calculation
```
yOutput = (xInput * 997 * yReserves) / ((xReserves * 1000) + (xInput * 997))
```

Where:
- `997/1000` represents the 0.3% fee
- Numerator: fee-adjusted input × output reserves
- Denominator: (input reserves × 1000) + fee-adjusted input

### LP Token Minting
```
liquidityMinted = (ethInput * totalLiquidity) / ethReserves
```

### Withdrawal Calculation
```
ethOutput = (lpTokensWithdrawn * ethReserves) / totalLiquidity
balOutput = (lpTokensWithdrawn * balReserves) / totalLiquidity
```

## 🐛 Troubleshooting

### Contracts not showing on Frontend
- Check `packages/nextjs/contracts/deployedContracts.ts`
- Ensure you're on the correct network (Sepolia)
- Verify contract addresses are correct

### Transactions failing
- Check gas prices on Sepolia
- Ensure wallet has enough test ETH
- Check token approvals (need to approve DEX before trading BAL)

### Build errors
```bash
# Clear cache and reinstall
rm -rf node_modules .next
yarn install
yarn next:build
```

## 📖 Resources

- [Uniswap V2 Whitepaper](https://uniswap.org/whitepaper.pdf)
- [Scaffold-ETH Documentation](https://docs.scaffoldeth.io/)
- [Solidity Documentation](https://docs.soliditylang.org/)
- [OpenZeppelin ERC20](https://docs.openzeppelin.com/contracts/4.x/erc20)
- [Hardhat Documentation](https://hardhat.org/docs)
- [Sepolia Testnet](https://www.alchemy.com/dapps/sepolia-faucet)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## ✅ Submission Checklist

Before submitting to SpeedRunEthereum.com:

- ✅ Contracts deployed to Sepolia
- ✅ Both contracts verified on Etherscan
- ✅ Frontend deployed to Vercel
- ✅ All 7 checkpoints completed
- ✅ 17/17 tests passing
- ✅ Can swap tokens successfully
- ✅ Can provide/withdraw liquidity
- ✅ Fees are collected correctly

## 🎯 Next Steps

After completing this challenge:

1. **Test on Sepolia**: https://speedrunqdex.vercel.app/
2. **Submit to SpeedRunEthereum**: https://www.speedrunethereum.com/
3. **Contract Address**: `0x3aD1Fc4C4aae3739152C420157Ccd7338E81622d`
4. **Etherscan Link**: https://sepolia.etherscan.io/address/0x3aD1Fc4C4aae3739152C420157Ccd7338E81622d

---

**Built with ❤️ using Scaffold-ETH 2**

**Challenge Completed**: November 7, 2025