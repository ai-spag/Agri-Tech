# 🌱 Contributing to Agri-Tech Pioneers Infrastructure (Solana + Rust)

Welcome to the Agri-Tech Pioneers DAO and Infrastructure Repository!  
We’re building a decentralized agricultural system powered by Solana smart contracts and IoT sensors. Thank you for helping build the future of food, blockchain, and education!

---

## 🛠️ 1. Setting Up the Development Environment

### ✅ Prerequisites
- [Rust](https://rustup.rs/) (recommended: latest stable)
- [Solana CLI](https://docs.solana.com/cli/install-solana-cli-tools) (>= v1.17)
- [Anchor](https://book.anchor-lang.com/chapter_2/installation.html) framework for Solana smart contract development
- [Node.js](https://nodejs.org/) & npm (for front-end and GraphQL tooling)
- Git + GitHub CLI

### 🛠️ Clone and Initialize
```bash
git clone {enter website}
cd agri-tech-infrastructure
anchor build
```

### 🔧 Setup Localnet (Optional)
```bash
solana-test-validator
anchor deploy
```

---

## 🥪 2. Project Structure

```
contracts/           # Rust-based smart contracts for DAO, crops, leasing
infrastructure/      # Solana nodes, sensor integration, data streams
dao/                 # Governance contracts, proposals, metadata
curriculums/         # 12-week bootcamp curriculum (Markdown/JSON)
docs/                # Whitepapers, grant proposals, technical reports
marketing/           # Pitch decks, brand assets, onboarding guides
```

---

## 💡 3. Coding Standards & Best Practices

- Use `snake_case` for Rust and `camelCase` for JavaScript/TypeScript.
- Comment all structs, enums, and public functions using `///` doc comments.
- Anchor: use deterministic seeds (e.g. `b"proposal"` + pubkey) for PDAs.
- Keep smart contracts modular and auditable.

Example Rust Struct:
```rust
#[account]
pub struct CropProposal {
    pub name: String,
    pub votes: u64,
    pub authority: Pubkey,
}
```

---

## 🔁 4. Submitting Pull Requests

1. Fork this repository.
2. Create a new branch: `git checkout -b feature/my-change`.
3. Follow the commit convention: `feat:`, `fix:`, `docs:`, `test:`, etc.
4. Push your branch: `git push origin feature/my-change`.
5. Open a Pull Request against the `main` branch.

> Please ensure your code passes linting and tests (`anchor test`).

### ✅ Pull Request Checklist:
- [ ] Code compiles and runs
- [ ] Smart contract tests written if applicable
- [ ] Comments and documentation are included
- [ ] Solana deployment addresses are not hardcoded unless necessary
- [ ] If modifying state, backward compatibility is considered

---

## 📡 5. IoT Sensor Integration

We support vertical planter and environmental IoT data collection (pH, moisture, humidity, temp).

### Integration Requirements:
- Data should be timestamped and locally signed
- Use MQTT or WebSocket bridge to relay to a blockchain indexer
- Structure:
```json
{
  "sensor_id": "name_numerical_#",
  "data": {
    "moisture": 41.3,
    "ph": 6.7
  },
  "timestamp": 1715132122
}
```

> Upload to blockchain via smart contract logs or oracles (coming Q3 2025).

---

## 🌍 Cross-Team Collaboration

- Education: Please sync with `curriculums/` before pushing workshop code.
- DAO: Update governance logic in `dao/` and leave notes in `docs/GOVERNANCE.md`.
- Infrastructure: Discuss sensor deployments and on-chain mirror designs in Issues.

---

## 📬 Contact & Support

Open a GitHub Issue or reach out via email:  
📩 **{official support email}**  
🌐 {official website}

---

Thank you for growing with us.  
🌿 *Agri-Tech Pioneers – Food. Code. Equity.*

