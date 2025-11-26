# DecentraLabs Tokenomics

**DecentraLabs** is a decentralized marketplace for accessing online/remote laboratories shared by educational and research institutions. This document outlines the tokenomics of the project and how the native token ($LAB) will fuel the ecosystem.

## 🧩 Token Utility

The native token of DecentraLabs, **$LAB**, plays several key roles:

- 💳 **Payment for reservations**: Users pay for lab access using the $LAB token.
- 🎓 **Institutional grants**: Universities can preload institutional treasuries to fund student access without requiring individual wallets.
- 🔒 **Provider staking**: Lab providers must stake tokens as a quality guarantee (800 $LAB base + 200 per additional lab beyond 10).
- 🧑‍🏫 **Provider incentives**: Lab providers receive $LAB tokens for each completed reservation.
- 🚀 **Early adopters incentives**: Institutions that join early as lab providers receive 1,000 $LAB tokens upon registration (800 auto-staked + 200 to institutional treasury).
- 🗳️ **Governance** *(future)*: $LAB holders will be able to vote on protocol decisions (e.g., lab listings/delistings, fee models, technical upgrades, support for authorization/authentication mechanisms).

## 🏛️ Stakeholders and Economic Flows

### Students / Researchers
- Access via wallet login or institutional account using SSO/SAML2 (schacPersonalUniqueCode).
- Reserve labs by paying in $LAB:
  - **With personal wallet**: Direct payment from user's balance.
  - **Institutional users (SSO)**: Payment from institutional treasury (managed by an institutional wallet or a backend relayer).
- No wallet required for SSO users - authentication handled by institutional identity provider.

### Staff / Institutions / Providers
- Verified manually by the team or community when registering via wallet, automatically via an Identity Provider (IdP) when using federated SSO access, or through Decentralized ID (DID) credentials issued by their institutions.
- Can register and list labs to receive payments in $LAB for their use.
- **Initial allocation**: 1,000 $LAB tokens upon registration while the provider pool has capacity:
  - 800 $LAB automatically staked (required to list labs).
  - 200 $LAB deposited to institutional treasury (for SSO users).
  - If the provider pool is exhausted, providers can still onboard but receive zero tokens.
- **Staking requirements**:
  - Base stake: 800 $LAB (covers first 10 labs).
  - Additional stake: +200 $LAB per lab beyond the 10th.
  - **Lock periods**:
    - 180 days from initial auto-stake.
    - 30 days from last reservation completion/cancellation.
- **Slashing**: Tokens can be burned for service failures or misconduct.
- Funding student access via smart contract-based institutional treasuries.

## 🏦 Institutional Treasuries

Institutional treasuries enable providers (universities, research centers) to fund lab access for their users without requiring each user to have a wallet or hold tokens directly.

### Architecture

Each provider has an institutional treasury managed through the `InstitutionalTreasuryFacet`:

- **Auto-initialization**: When a provider registers, 200 $LAB tokens are automatically deposited to their institutional treasury if the provider pool has capacity; if the pool is exhausted, the treasury is initialized with zero but limits/configuration are still set.
- **Default limit**: Each institutional user (identified by SAML2 `schacPersonalUniqueCode`) has a default spending limit of 10 $LAB tokens.
- **Centralized spending**: Provider can set custom limits per user or deposit additional funds as needed.

### Backend Authorization Pattern

Since institutional users (students, researchers) authenticate via SSO/SAML2 and don't have wallets:

1. **Auto-authorization and additional backend authorizations**: Provider's address is automatically authorized as backend upon registration and may call `authorizeBackend(backendAddress)` to authorize additional backend relayers.
2. **Backend spends on behalf of users**: The authorized backend can call `spendFromInstitutionalTreasury(provider, puc, amount)` to spend tokens for a specific user (identified by their `puc`).
3. **Per-user limits enforced**: The contract tracks how much each user has spent and enforces the limit set by the provider.

### Key Functions

- `depositToInstitutionalTreasury(uint256 amount)`: Provider deposits additional funds to treasury.
- `setInstitutionalUserLimit(uint256 limit)`: Provider sets spending limit per institutional user.
- `authorizeBackend(address backend)`: Provider authorizes a backend to spend on behalf of users.
- `revokeBackend()`: Provider revokes backend authorization.
- `spendFromInstitutionalTreasury(address provider, string memory puc, uint256 amount)`: Authorized backend spends tokens for a user (requires backend authorization).

### Security Model

- **Provider control**: Only the provider can authorize/revoke backends and set limits.
- **Backend authorization**: Only the authorized backend can execute spending operations.
- **Per-user tracking**: Individual spending limits prevent abuse.
- **Event logging**: All operations emit events for transparency and auditing.

## 💰 Token Supply and Issuance Policy

- **Maximum supply**: 1 million $LAB (cap enforced).
- **Mint authority**: after pool initialization, `MINTER_ROLE` should move to governance (timelock/DAO) and be revoked from the Diamond. No deployer premint.
- **Provider allocation (20% pool)**:
  - 1,000 $LAB per provider (800 auto-staked + 200 to institutional treasury) while pool capacity remains; if the pool is exhausted, providers still onboard but receive zero tokens.
- **Initial distribution**:
  - 20% Providers: un-minted pool used to grant 1,000 $LAB per provider as they onboard.
  - 15% Treasury: preminted to an on-chain timelock (multisig-controlled, delay configurable).
  - 12% Liquidity: preminted to the same timelock; LP provisioning/removal must go through governance.
  - 10% Ecosystem growth: 2% premint; admin-only top-ups of 2% when balance < 1,000 $LAB, up to 10% total.
  - 15% Student subsidies: 3% premint; admin-only top-ups of 3% when balance < 1,000 $LAB, up to 15% total.
  - 10% Founding team: premint to a vesting wallet (6-month cliff, ~36-month linear).
  - 18% Reserve: un-minted; only mintable via governed `mintFromReserve`, still under the global cap.
- **Controlled emissions**:
  - Provider mint bounded by the provider pool; governed reserve mint; admin-only top-ups for subsidies and ecosystem.

## 💼 Revenue Distribution

Tokens paid for lab access are distributed as follows:
- **70%** to the lab provider (incentivizes quality service and lab availability).
- **15%** to the project treasury (operations, development, and can be allocated to liquidity if needed).
- **10%** to student subsidies (funding lab access for students with financial need).
- **5%** to governance incentives (rewards for community participation in governance decisions).
- **Cancellation fee**: for confirmed/active reservations, a 3% fee is kept (1% provider, 1% treasury, 1% governance); 97% is refunded to the payer. Zero-price reservations have no split/fee.

**Notes:**
- Marketplaces may apply additional and variable fees for lab access.
- Liquidity for exchanges is provided from the initial 12% allocation in the token supply distribution.
- The project treasury and reserve allocations could be used to add liquidity if market conditions require it.

## 🛡️ Security and Verification

### Provider Staking Mechanism

Lab providers must stake $LAB tokens as a performance guarantee:

- **Base stake**: 800 $LAB (covers first 10 labs).
- **Additional stake**: +200 $LAB per lab beyond the 10th.
- **Formula**: `requiredStake = 800 + max(0, listedLabs - 10) * 200`

**Lock periods**:
- **Initial lock**: 180 days from the moment tokens are auto-staked on provider registration.
- **Reservation lock**: Additional 30-day lock from the last reservation completion/cancellation.

**Slashing**: If a reserved lab fails to deliver the expected service (e.g., offline or inaccessible), a portion of the staked amount is deducted from the provider's balance. This mechanism ensures accountability, similar to slashing in decentralized networks.

### Institutional Treasury Security

- **Backend authorization**: Only institution-authorized backends can spend from institutional treasuries.
- **Per-user limits**: Default 10 $LAB per institutional user (configurable by institution).
- **Spending tracking**: Smart contract tracks individual user spending to enforce limits.
- **Institution control**: Only the institution can authorize/revoke backends and modify limits.

### Identity Verification

- **Provider verification**:
  - Wallet-based registration: Initially verified by team/community.
  - SSO registration: Automatically verified via federated IdPs (SAML2).
  - Future: Decentralized identity frameworks (Polygon ID, Truvera, etc.).
- **Institutional users**: Authenticated via SSO using `schacPersonalUniqueCode` (puc).

### Smart Contract Governance

- All economic flows are governed by smart contracts (Diamond Pattern / EIP-2535).
- Lab access is managed through marketplaces like the DecentraLabs Marketplace.
- Event logging ensures transparency and auditability for all operations.

## 🔮 Roadmap & Future Utility

- **Staking $LAB** for governance participation.
- **NFTs** as proof of lab completion or credentials.
- **Cross-protocol interoperability** with other educational and scientific ecosystems.

---

Have feedback or suggestions? Open an issue or start a discussion. We're building this ecosystem together!

