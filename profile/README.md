<div align="center">
  <img src="./logo.svg" alt="Altworth" width="200" />
  <br />
  <br />
  <strong>Live shopping platform with RWA tokenization and Gachapon gamification on Solana blockchain</strong>
</div>

---

## About

Altworth is a next-generation live shopping platform that combines real-world asset (RWA) tokenization with engaging Gachapon-style gamification. Built on the Solana blockchain, the platform enables users to purchase tokenized assets through an interactive, game-like experience powered by Web3 technology.

### Core Focus Areas

- **Live Shopping**: Interactive shopping experience with real-time engagement
- **RWA Tokenization**: Real-world assets represented as NFTs on Solana
- **Gachapon Gamification**: Japanese capsule-toy inspired mechanics for an engaging purchase experience

## Architecture

Our platform is built with a microservices architecture:

- **[Frontend](https://github.com/altworth-markets/front-end)** - Next.js application with Web3 integration
  - **Live Demo**: [altworth.netlify.app](https://altworth.netlify.app)
  - Stack: Next.js 15, TypeScript, Tailwind CSS, Solana Web3.js
  - Features: Wallet integration, Gachapon capsule purchase, NFT claiming, admin dashboard

- **[Backend](https://github.com/altworth-markets/backend)** - RESTful API server
  - Stack: Bun runtime, Hono framework, PostgreSQL
  - Features: Item/RWA management, capsule operations, buyback pricing, transaction tracking

- **[SDK](https://github.com/altworth-markets/sdk)** - TypeScript SDK for platform integration
  - Packages: `@altworth-markets/sdk-core`, `sdk-react`, `sdk-solana`, `sdk-types`
  - Published to: GitHub Packages
  - Features: Type-safe API client, React hooks, Solana utilities

- **[Contracts](https://github.com/altworth-markets/contracts)** - Solana smart contracts
  - Stack: Anchor framework, Rust
  - Features: NFT minting, Gachapon mechanics, marketplace operations

## Tech Stack

**Blockchain & Web3**
- Solana blockchain (devnet/mainnet)
- Anchor framework for smart contracts
- Solana Web3.js for client integration
- Helius RPC for reliable blockchain access

**Frontend**
- Next.js 15 with App Router
- TypeScript for type safety
- Tailwind CSS for styling
- Wallet adapter for multi-wallet support

**Backend**
- Bun runtime for performance
- Hono web framework
- PostgreSQL database
- Drizzle ORM

**Infrastructure**
- Netlify for frontend hosting
- GitHub Actions for CI/CD
- Docker for local development
- GitHub Packages for private SDK distribution

## How It Works

1. **Admin Setup**: Platform administrators add tokenized RWAs/NFTs and create Gachapon capsules
2. **User Experience**: Users connect their Web3 wallet and purchase Gachapon capsules using cryptocurrency
3. **Reveal & Claim**: Gamified reveal experience shows the tokenized assets, users can claim NFTs or sell back to platform
4. **Live Shopping**: Real-time, interactive shopping experience with blockchain transparency

## Security & Best Practices

- **API Key Protection**: Secure RPC proxy pattern keeps Helius API keys server-side
- **Authentication**: GitHub Packages for private SDK access
- **Testing**: Comprehensive test coverage with Jest
- **Type Safety**: Full TypeScript coverage across all repositories
- **Code Quality**: ESLint, Prettier, and automated CI checks

## Documentation

### 📚 Start Here

**New to Altworth?** Choose your path based on your role:

| I am a... | Start Here |
|-----------|------------|
| **New Developer** | [Frontend Developer Onboarding](https://github.com/altworth-markets/front-end/blob/main/docs/DEVELOPER_ONBOARDING.md) |
| **Product Manager / Stakeholder** | [User Journeys](https://github.com/altworth-markets/.github/blob/main/profile/USER_JOURNEYS.md) |
| **System Architect** | [System Overview](https://github.com/altworth-markets/.github/blob/main/profile/SYSTEM_OVERVIEW.md) |
| **Backend Engineer** | [Technical Flows](https://github.com/altworth-markets/backend/blob/main/docs/TECHNICAL_FLOWS.md) |
| **Database Administrator** | [State Machines](https://github.com/altworth-markets/backend/blob/main/docs/STATE_MACHINES.md) |

### 📖 Documentation Hub

**Complete documentation index**: [DOCUMENTATION_INDEX.md](https://github.com/altworth-markets/.github/blob/main/profile/DOCUMENTATION_INDEX.md)

**Key Documentation**:
- **[System Overview](https://github.com/altworth-markets/.github/blob/main/profile/SYSTEM_OVERVIEW.md)** - High-level architecture and platform vision (~30 pages)
- **[User Journeys](https://github.com/altworth-markets/.github/blob/main/profile/USER_JOURNEYS.md)** - Persona-based scenarios with UI flows (~60 pages)
- **[Technical Flows](https://github.com/altworth-markets/backend/blob/main/docs/TECHNICAL_FLOWS.md)** - Detailed sequence diagrams (~45 pages)
- **[State Machines](https://github.com/altworth-markets/backend/blob/main/docs/STATE_MACHINES.md)** - Entity lifecycle state machines (~35 pages)
- **[Developer Onboarding](https://github.com/altworth-markets/front-end/blob/main/docs/DEVELOPER_ONBOARDING.md)** - Complete setup guide (~60 pages)

**Total**: 40+ documentation files, ~510 pages

## Getting Started

Visit our [frontend repository](https://github.com/altworth-markets/front-end) for setup instructions and development guides.

**Live Platform**: [altworth.netlify.app](https://altworth.netlify.app)

## Contributing

This is a private organization. If you're a team member, check individual repository READMEs for contribution guidelines and development setup.

---

<div align="center">
  <sub>Built with ❤️ on Solana blockchain</sub>
</div>
