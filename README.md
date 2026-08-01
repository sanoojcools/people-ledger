# People Ledger

> A decentralized professional identity protocol where employers cryptographically attest to your work history, you own those attestations in your wallet, and an AI agent automatically generates privacy-preserving proofs that match you to jobs.

**Live Demo:** [sanoojcools.github.io/people-ledger](https://sanoojcools.github.io/people-ledger)

---

## The Problem

Reference checks are broken. When Sarah, a senior engineer at AWS, wants to join Stripe, here is what happens:

- Stripe asks for her performance review. Her manager is on paternity leave and does not respond for 10 days.
- AWS HR will not share ratings externally. They confirm dates and title only.
- Stripe can only verify her title and dates. So they down-level her offer from L7 to L5.
- She loses $80,000 in year-one compensation because a PDF could not be trusted.

This happens every day. The people who built the systems cannot prove they built the systems.

---

## The Solution

People Ledger is a three-layer protocol:

| Layer | What It Does | Tech |
|-------|---------------|------|
| **Trust Layer** | Employers cryptographically sign credentials. Employees own them forever. | Solidity, ERC-721, ECDSA |
| **Privacy Layer** | Prove you are qualified without revealing how qualified. | ZK-SNARKs, Groth16 |
| **Intelligence Layer** | AI agent reads job descriptions, extracts requirements, auto-generates proofs. | NLP, Pattern Matching |

---

## Architecture

```
Employer Portal          Blockchain              Employee Wallet
    |                        |                         |
    |-- Upload Review -----> |                         |
    |   AI Extractor         |                         |
    |   (parse unstructured) |                         |
    |                        |                         |
    |-- Mint Credential ----> |-- Store Hash --------> |
    |   (ERC-721 NFT)        |   (on-chain)            |-- Own Credential
    |                        |                         |
    |                        |                         |-- ZK Proof Generation
    |                        |                         |   (prove threshold)
    |                        |                         |
Job Description            AI Matching Agent         Verifier Portal
    |                        |                         |
    |-- Parse Requirements -> |-- Query Chain --------> |
    |                        |   (find matches)        |-- Verify Proof
    |                        |                         |   (without seeing data)
```

---

## Features

### 1. Issue Credential
Employers mint ERC-721 credentials to employee wallets. Three visibility levels:
- **Public** -- Full disclosure
- **ZK-Verifiable** -- Proof only, data encrypted
- **Private** -- Holder only

### 2. AI Extractor
Paste a raw performance review email or PDF text. The AI agent:
- Extracts structured fields (name, title, level, rating, skills)
- Maps them to the credential schema
- Auto-fills the mint form
- One-click credential creation

### 3. AI Matching
Paste a job description. The AI agent:
- Extracts requirements (level, tenure, skills, performance)
- Queries the chain for matching credentials
- Identifies which attributes need ZK proofs
- Generates a proof request bundle

### 4. ZK Proof Generation
Select an attribute. Generate a Groth16 range proof:
- Prove `rating >= "Exceeds Expectations"` without revealing the exact rating
- Prove `tenure >= 4 years` without revealing exact dates
- Prove `level >= L6` without revealing exact level

### 5. Verification
Instant on-chain verification:
- Credential status (active/revoked)
- Issuer reputation score
- Cryptographic signature validity
- Zero-knowledge proof verification

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Smart Contract | Solidity 0.8.26, OpenZeppelin |
| Development | Hardhat |
| Frontend | Vanilla JS, CSS3 |
| ZK Proofs | Circom/Groth16 (integration ready) |
| AI Layer | Browser-based NLP, pattern matching |
| Deployment | GitHub Pages, Polygon Amoy (testnet) |

---

## Contract Features

- **ERC-721 Enumerable** -- Each credential is an NFT the employee truly owns
- **Issuer Registry** -- Domain verification and reputation scoring
- **Batch Minting** -- Issue credentials to entire cohorts
- **Revocation** -- On-chain audit trail with reason
- **ZK Verification Hooks** -- Ready for Circom/Groth16 integration

---

## Local Development

```bash
# 1. Install dependencies
cd hardhat && npm install

# 2. Start local blockchain
npx hardhat node

# 3. Deploy contract (new terminal)
npx hardhat run scripts/deploy.js --network localhost

# 4. Serve frontend
cd ../frontend && python -m http.server 3000
```

---

## Testnet Deployment

```bash
# 1. Get free MATIC from Polygon Amoy faucet
# 2. Set PRIVATE_KEY in .env
# 3. Deploy
npx hardhat run scripts/deploy.js --network polygonAmoy
```

---

## Roadmap

- [x] Solidity contract with issuer registry
- [x] Frontend with 6-tab demo flow
- [x] AI Matching agent (job description parser)
- [x] AI Extraction agent (performance review parser)
- [x] ZK proof concept (range proofs)
- [ ] Circom circuit integration for real ZK proofs
- [ ] Polygon Amoy testnet deployment
- [ ] MetaMask wallet connection
- [ ] Employer dashboard with batch minting
- [ ] Credential revocation workflow

---

## The Story

> "Sarah doesn't send her review. She doesn't beg her manager. She connects her wallet. Stripe's AI agent reads the job description, queries the chain, and finds her. She generates a zero-knowledge proof. Stripe sees the proof. They never see the number. Offer extended in 48 hours."

---

## License

MIT

---

Built by [Sanooj Cools](https://sanoojcools.github.io) -- ex-Amazon, ex-AWS, ex-Swvl. I saw reference checks fail talented people every day. This is my attempt to fix it.
