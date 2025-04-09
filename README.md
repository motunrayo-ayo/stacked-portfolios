# StackedPortfolios - Automated Portfolio Management Protocol

![Stacks L2 Compatible](https://img.shields.io/badge/Stacks-Layer%202-5546FF)
![Bitcoin Finalized](https://img.shields.io/badge/Security-Bitcoin%20Finalized-orange)

## Overview

A non-custodial portfolio management system operating on Stacks Layer 2 that enables automated rebalancing of multi-asset portfolios with Bitcoin-grade security. Implements institutional-grade financial primitives compliant with Bitcoin DeFi standards.

## Key Features

- 🛡️ Non-Custodial Architecture with Principal-Based Ownership
- ⚖️ Multi-Asset Portfolios (Up to 10 Tokens)
- 🔄 24-Hour Rebalance Cycles
- 📊 Basis Point Precision Allocations (0.01% Granularity)
- 💸 0.25% Protocol Fee Structure
- 🔐 Bitcoin-Anchored Security via Stacks Proof-of-Transfer

## Technical Specification

### Core Data Structures

```clarity
;; Portfolio Metadata
{
    owner: principal,
    created-at: uint,
    last-rebalanced: uint,
    total-value: uint,
    active: bool,
    token-count: uint
}

;; Asset Allocation Template
{
    target-percentage: uint,  // In basis points (10000 = 100%)
    current-amount: uint,     // Current token balance
    token-address: principal  // SIP-010 compliant
}
```

### System Constants

| Constant                   | Value  | Description                  |
| -------------------------- | ------ | ---------------------------- |
| `MAX-TOKENS-PER-PORTFOLIO` | 10     | Maximum assets per portfolio |
| `BASIS-POINTS`             | 10,000 | 1 BP = 0.01%                 |
| `PROTOCOL-FEE`             | 25 BPs | 0.25% fee on rebalance       |

### Error Handling System

| Error Code                     | Description                     |
| ------------------------------ | ------------------------------- |
| ERR-NOT-AUTHORIZED (u100)      | Unauthorized principal action   |
| ERR-INVALID-PORTFOLIO (u101)   | Nonexistent portfolio ID        |
| ERR-MAX-TOKENS-EXCEEDED (u107) | Portfolio asset limit violation |
| ERR-INVALID-PERCENTAGE (u106)  | Allocation outside 0-100% range |

## Core Functions

### Portfolio Management

- `create-portfolio`: Initializes new portfolio with token allocations
  - Parameters:
    - `initial-tokens`: List of SIP-010 token contracts
    - `percentages`: Allocation percentages in basis points
- `rebalance-portfolio`: Executes allocation adjustments
  - Auto-triggered after 144 blocks (~24 hours)
  - Applies protocol fee in native STX

### Financial Operations

- `update-portfolio-allocation`: Adjusts target percentages
  - Requires portfolio ownership
  - Basis point validation
- `calculate-rebalance-amounts`: View function for rebalance simulation
  - Returns delta values for each asset

## Security Architecture

1. **Principal-Based Authorization**  
   All operations validate owner principal against Stacks wallet signatures

2. **Bitcoin-Finalized State**  
   Portfolio states anchor to Bitcoin blocks via Stacks L2 consensus

3. **Asset Isolation**  
   Token holdings remain in users' custodial wallets

4. **Fee Safeguards**  
   Protocol fees auto-convert to BTC via Arkadiko Protocol vaults

## Usage Examples

### Portfolio Creation

```clarity
(create-portfolio
    (list 'SP3FBR2AGK5H9QBDH3EEN6DF8EK8JY7RX8QJ5SVTE.sip-010-token
          'SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.asset-token)
    (list u6000 u4000))  // 60%/40% allocation
```

### Portfolio Rebalancing

```clarity
(rebalance-portfolio u1)  // u1 = portfolio ID
```

### Allocation Update

```clarity
(update-portfolio-allocation
    u1     // Portfolio ID
    u0     // Token index
    u5500) // New 55% allocation
```

## Contributing

1. Fork repository
2. Create feature branch (`git checkout -b feature/improve-rebalance`)
3. Commit changes with Clarity standards
4. Push to branch
5. Submit PR with detailed change description
