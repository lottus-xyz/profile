<div align="center">
  <a href="https://lottus.xyz">
    <img src="https://lottus.xyz/og-image.png" alt="LOTTUS Protocol" width="100%" />
  </a>

  # LOTTUS PROTOCOL

  **Redefining On-Chain Probability.**
  *Engineering the transparency layer for fair digital interactions on Base.*

  [Website](https://lottus.xyz) • [How It Works](https://lottus.xyz/howitworks) • [X (Twitter)](https://x.com/lottus_xyz)

  ---
</div>

### ⚡ Status: Private Alpha on Base Sepolia

LOTTUS is currently in **Private Alpha** on Base Sepolia. Core contracts are deployed and undergoing iterative testing with live Chainlink VRF integration.

> **Note to Developers & Auditors:**
> Our core smart contract and interface repositories (`lottus-protocol`, `lottus-interface`) are currently **private** (`🔒`) while undergoing internal security testing. They will be open-sourced upon Mainnet launch to ensure full transparency and verifiability.

---

### 🌀 What is LOTTUS?

LOTTUS is a decentralized protocol that replaces opaque, server-side randomness with an auditable, trust-minimized on-chain architecture. Speculative positions are represented as tradable ERC-721A assets with immutable parameters — settlement is fully deterministic and permissionlessly executable by anyone.

**Core Principles:**

- **Verifiable Entropy** — Randomness is generated incrementally over 7-day cycles via Chainlink VRF. No single entity — including the team — can influence outcomes.
- **Liquid State Assets** — Positions are ERC-721A NFTs whose conditional value evolves as entropy is revealed daily. Tradable on any Base-compatible marketplace before settlement.
- **Gas-Bounded Settlement** — Permissionless batch processing with O(1) gas per transaction. No admin keys required to resolve cycles.
- **Liquidity Retention** — Unclaimed value compounds into subsequent cycles via an autonomous rollover engine, building deeper pools over time.

---

### 🏗 Architecture

```
User → mint() → LottusCore ← Chainlink VRF (7 daily seeds)
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
      LottusNFT  Tier Pots  ReserveVault
      (ERC-721A) (5 tiers)  (Protocol Reserve)
```

The protocol operates as a state machine across defined cycles. Two cycles run in parallel: one resolving (entropy collection + settlement) and one accepting new positions. All state transitions are permissionless — anyone can trigger seed requests, tally batches, and cycle finalization.

**Key Design Decisions:**

| Decision | Rationale |
| :--- | :--- |
| Forward-Minting (N+1) | Positions are always minted for the *next* cycle, making VRF front-running structurally impossible |
| Cursor-Based Tally | Gas-bounded O(1) batch processing prevents DoS on settlement |
| Snapshot Freeze | Participant set and economic parameters are frozen at tally start — no late manipulation |
| Pull-Payment Claims | CEI pattern + ReentrancyGuard for all ETH distributions |
| UUPS + Freeze | Upgradeable during alpha, with an irreversible `freezeUpgrades()` for full immutability post-launch |

---

### 🛠 Tech Stack

| Category | Technology |
| :--- | :--- |
| **Network** | ![Base](https://img.shields.io/badge/Base_L2-0052FF?style=flat-square&logo=base&logoColor=white) |
| **Contracts** | ![Solidity](https://img.shields.io/badge/Solidity-363636?style=flat-square&logo=solidity&logoColor=white) ![Foundry](https://img.shields.io/badge/Foundry-EA5403?style=flat-square&logo=foundry&logoColor=white) |
| **Standards** | ![OpenZeppelin](https://img.shields.io/badge/OpenZeppelin-4E5EE4?style=flat-square&logo=openzeppelin&logoColor=white) ![ERC-721A](https://img.shields.io/badge/ERC--721A-FF4154?style=flat-square) |
| **Randomness** | ![Chainlink](https://img.shields.io/badge/Chainlink_VRF_V2.5-375BD2?style=flat-square&logo=chainlink&logoColor=white) |
| **Automation** | ![Chainlink](https://img.shields.io/badge/Chainlink_Automation-375BD2?style=flat-square&logo=chainlink&logoColor=white) |
| **Interface** | ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) |
| **Web3** | ![wagmi](https://img.shields.io/badge/wagmi-35324a?style=flat-square) ![viem](https://img.shields.io/badge/viem-1C1C1E?style=flat-square) ![Reown](https://img.shields.io/badge/Reown_AppKit-6A6FF5?style=flat-square) |
| **Backend** | ![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white) |
| **Governance** | ![Safe](https://img.shields.io/badge/Safe_Multisig-12FF80?style=flat-square&logo=safe&logoColor=black) + 48h Timelock |

---

### 📐 Protocol Parameters

| Parameter | Value |
| :--- | :--- |
| Cycle Duration | 7 days |
| Entropy Sources | 7 VRF seeds per cycle (1 per day) |
| Number Range | 7 unique numbers from 1–50 |
| Winning Tiers | 5 (3/7 through 7/7 match) |
| Settlement | Permissionless, gas-bounded batches |
| Claim System | Pull-payment with configurable window |
| Upgradeability | UUPS proxy with irreversible freeze option |

---

### 🛡 Security

Security is our highest priority. The protocol is designed around trust-minimization — no admin keys can influence outcomes, and all critical operations are permissionless.

- **Standards:** OpenZeppelin Contracts (Access Control, ReentrancyGuard, Pausable, UUPS)
- **Randomness:** Chainlink VRF V2.5 with cryptographic proof verification on-chain
- **Governance:** Safe Multisig (2-of-3) → Timelock (48h) → Protocol Contracts
- **Solvency Invariant:** `SUM(payouts) ≤ cyclePool` — structurally enforced, not just checked
- **Bug Bounties:** Program launching with Mainnet
- **Security Contact:** [security@lottus.xyz](mailto:security@lottus.xyz)
- **RFC 9116:** [https://lottus.xyz/.well-known/security.txt](https://lottus.xyz/.well-known/security.txt)

---

### 🗺 Roadmap

| Phase | Status |
| :--- | :--- |
| Protocol Design & Specification | ✅ Complete |
| Core Contracts (LottusCore, LottusNFT, ReserveVault) | ✅ Deployed on Base Sepolia |
| Chainlink VRF V2.5 Integration | ✅ Live on Testnet |
| Chainlink Automation (Daily Upkeep) | ✅ Active |
| Interface (Mint, Dashboard, Stats) | ✅ Alpha |
| XP & Engagement System | 🔧 In Progress |
| Security Audit | ⏳ Upcoming |
| Mainnet Launch on Base | ⏳ Upcoming |
| Open-Source Contracts | ⏳ At Mainnet Launch |
| Bug Bounty Program | ⏳ At Mainnet Launch |

---

### 📄 Documentation

- **[How It Works](https://lottus.xyz/howitworks)** — Visual walkthrough of the settlement cycle
- **Protocol Specification** — Available upon request for auditors and collaborators
- **Contract Documentation** — Will be published with open-source release

---

<div align="center">

  **[Initialize Sequence →](https://lottus.xyz)**

  <br />

  <sub>Built on Base. Powered by Chainlink VRF. Secured by OpenZeppelin.</sub>

  <sub>© 2026 LOTTUS Protocol. All rights reserved.</sub>

</div>
