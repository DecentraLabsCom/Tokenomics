# DecentraLabs Tokenomics

**DecentraLabs** is a decentralized marketplace for accessing online/remote laboratories shared by educational and research institutions. This document outlines the tokenomics of the project and how the native token ($LAB) will fuel the ecosystem.

## 🧩 Token Utility

The native token of DecentraLabs, **$LAB**, plays several key roles:

- 💳 **Payment for reservations**: Users pay for lab access using the $LAB token.
- 🎓 **Institutional grants**: Universities can preload wallets or treasuries to fund student access.
- 🧑‍🏫 **Provider incentives**: Lab providers receive $LAB tokens for each completed reservation.
- 🚀 **Early adopters incentives**: Institutions that join early as lab providers receive additional $LAB tokens upon registration.
- 🗳️ **Governance** *(future)*: $LAB holders will be able to vote on protocol decisions (e.g., lab listings/delistings, fee models, technical upgrades, support for authorization/authentication mechanisms).

## 🏛️ Stakeholders and Economic Flows

### Students / Researchers
- Access via wallet login or, in the near-term future, through their institutional account using SSO.
- Reserve labs by paying in $LAB, either with personal funds or institutional balances.

### Staff / Institutions / Providers
- Verified manually by the team or community when registering via wallet, automatically via an Identity Provider (IdP) -when using a federated SSO access-, or through Decentralized ID (DID) credentials issued by their institutions.
- Can register and list labs and receive payments in $LAB for their use.
- Funding student access via smart contract-based treasuries.

## 🏦 Institutional Treasuries

Each institution will be able to configure a treasury within the smart contract system:

- Allocate funds to specific student wallets, institutional accounts or DIDs.
- Define daily/monthly usage limits.
- Payments are made directly from the treasury, provided sufficient balance.

## 💰 Token Supply and Issuance Policy

> ⚠️ *Note: This is a preliminary policy and may evolve over the course of the project.*

- 🔢 **Initial total supply**: 1 million $LAB.
- 📦 **Initial distribution**:
  - 40% Lab usage incentives (usage mining)
  - 20% Project treasury
  - 20% Institutional funding pool
  - 10% Founding team
  - 10% Liquidity & exchange listings

- 🔁 **Controlled inflation**:
  - Additional emissions for usage and governance participation will be subject to community decisions.

## 💼 Revenue Distribution

Tokens paid for lab access are distributed as follows:
- 85% to the lab provider
- 12% to the project treasury
- 3% to student subsidies or governance incentives

Marketplaces may apply additional and variable fees for lab access.

## 🛡️ Security and Verification

- All economic flows are to be governed by smart contracts.
- Identity verification is handled initially by the team/community (when lab providers register only using their wallets), but the goal is to delegate it to federated IdPs and, in the future, decentralized identity frameworks (Polygon ID, Truvera, etc.).
- Lab access is managed through marketplaces like the upcoming DecentraLabs Marketplace.
- Lab providers are required to stake $LAB tokens as a performance guarantee. If a reserved lab fails to deliver the expected service (e.g., it's offline or inaccessible), a portion of the staked amount is deducted from the provider's treasury. This mechanism ensures accountability, similar to slashing mechanisms in decentralized networks where nodes are penalized for dishonest or unreliable behavior.

## 🔮 Roadmap & Future Utility

- **Staking $LAB** for governance participation.
- **NFTs** as proof of lab completion or credentials.
- **Cross-protocol interoperability** with other educational and scientific ecosystems.

---

Have feedback or suggestions? Open an issue or start a discussion. We're building this ecosystem together!
