# Altworth Markets - Complete Documentation Index

**Last Updated**: November 7, 2025
**Purpose**: Central directory of all Altworth Markets documentation

---

## 🎯 Start Here

**New to Altworth?** Choose your path:

| I am a... | Start Here |
|-----------|------------|
| **New Developer** | [Frontend Developer Onboarding](../../front-end/docs/DEVELOPER_ONBOARDING.md) |
| **Product Manager / Stakeholder** | [User Journeys](./USER_JOURNEYS.md) |
| **System Architect** | [System Overview](./SYSTEM_OVERVIEW.md) |
| **Backend Engineer** | [Technical Flows](../../backend/docs/TECHNICAL_FLOWS.md) |
| **Database Administrator** | [State Machines](../../backend/docs/STATE_MACHINES.md) |

---

## 📚 Documentation Suite

### **Organization-Level Documentation** (This Repository)

High-level, cross-repository documentation for all team members.

#### [System Overview](./SYSTEM_OVERVIEW.md) (~30 pages)
**Purpose**: High-level system architecture and design

**Contents**:
- Executive summary & platform vision
- Complete architecture diagrams
- Technology stack breakdown
- Component interaction maps
- Security & authentication patterns
- Network architecture

**Audience**: All team members, stakeholders, new hires

---

#### [User Journeys](./USER_JOURNEYS.md) (~60 pages)
**Purpose**: Persona-based user scenarios from end-user perspective

**Contents**:
- 4 Primary User Personas:
  - Sarah (Vintage Collector)
  - Marcus (RWA Investor)
  - Yuki (Gacha Gamer)
  - Alex (Platform Admin)
- Journey maps with UI mockups
- Success metrics & KPIs
- Edge cases & error scenarios

**Audience**: Product managers, designers, stakeholders, QA

---

#### [Cross-Repo Coordination](./CROSS_REPO_COORDINATION.md)
**Purpose**: How to work across multiple repositories

**Contents**: (Existing document)
- Cross-repository workflow
- Issue tracking across repos
- Release coordination

**Audience**: All developers

---

### **Frontend Documentation** ([front-end/docs/](../../front-end/docs/))

Frontend-specific guides for React/Next.js development.

#### [Developer Onboarding](../../front-end/docs/DEVELOPER_ONBOARDING.md) (~60 pages)
**Purpose**: Complete onboarding guide for frontend developers

**Contents**:
- 15-minute quick start
- Environment setup (frontend, backend, wallet)
- Repository structure walkthrough
- Core workflow code examples
- Testing strategy
- Common tasks & troubleshooting

**Audience**: New frontend developers, contributors

---

#### Other Frontend Docs
- [Architecture Analysis](../../front-end/docs/ARCHITECTURE_ANALYSIS.md) - React state management patterns
- [Deep Architecture Analysis](../../front-end/docs/DEEP_ARCHITECTURE_ANALYSIS.md) - Zustand vs React Query
- [NFT Transfer Flow](../../front-end/docs/NFT_TRANSFER_FLOW.md) - Pack reveal mechanics
- [API Endpoints](../../front-end/docs/API_ENDPOINTS.md) - Backend API reference
- [Terminology](../../front-end/TERMINOLOGY.md) - Domain glossary (Capsule vs Pack)
- [Testing Strategy](../../front-end/docs/TESTING_STRATEGY.md) - Testing approach
- [Deployment Guide](../../front-end/docs/DEPLOYMENT.md) - Netlify deployment
- [Security Guide](../../front-end/docs/SECURITY.md) - Frontend security best practices

---

### **Backend Documentation** ([backend/docs/](../../backend/docs/))

Backend-specific guides for API, database, and blockchain integration.

#### [Backend Architecture](../../backend/ARCHITECTURE.md) (~40 pages)
**Purpose**: Backend system design and technical decisions

**Contents**: (Existing document)
- Layered architecture
- Database schema
- Authentication flow
- Vault-based custody model
- Technology decisions (Bun, PostgreSQL, Solana)

**Audience**: Backend developers, database admins

---

#### [Technical Flows](../../backend/docs/TECHNICAL_FLOWS.md) (~45 pages)
**Purpose**: Detailed sequence diagrams for all major operations

**Contents**:
- Authentication flows (nonce-based signatures)
- Gacha system flows (end-to-end pull & reveal)
- NFT operations flows (two-phase signing)
- Analytics & data flows
- Admin operations flows
- Error handling & retry logic
- Blockchain transaction flows
- Caching & optimization flows

**Audience**: Backend developers, API integrators, QA

---

#### [State Machines](../../backend/docs/STATE_MACHINES.md) (~35 pages)
**Purpose**: Entity lifecycle state transitions

**Contents**:
- 7 Core State Machines:
  - Gacha Pull (6 states)
  - Sealed Pack (6 states)
  - Card (4 states)
  - Pool (6 states)
  - User (6 states)
  - Transaction (6 states)
- State transition rules & guards
- Error states & recovery strategies
- Database invariants

**Audience**: Backend developers, database admins, QA

---

#### [Frontend Integration Guide](../../backend/docs/FRONTEND-INTEGRATION-GUIDE.md)
**Purpose**: How to call backend APIs from frontend

**Contents**: (Existing document)
- API endpoint reference
- Authentication flow
- React code examples
- Error handling

**Audience**: Frontend developers integrating with backend

---

#### Other Backend Docs
- [Environment Setup](../../backend/docs/ENVIRONMENT_SETUP.md) - Backend configuration
- [Three-Tier Testing Strategy](../../backend/docs/testing/THREE-TIER-TESTING-STRATEGY.md) - Testing approach
- [Monitoring Guide](../../backend/docs/MONITORING.md) - Sentry, logs, metrics
- [Rollback Procedures](../../backend/docs/ROLLBACK.md) - Emergency procedures

---

### **SDK Documentation** ([sdk/docs/](../../sdk/docs/))

TypeScript SDK for integrating with Altworth APIs.

#### [SDK Architecture](../../sdk/docs/ARCHITECTURE.md)
**Purpose**: SDK package structure and API documentation

**Contents**: (Existing document)
- 4-package architecture
- API reference
- Type system
- React hooks

**Audience**: Frontend developers, SDK contributors

---

#### Other SDK Docs
- [Debugging Guide](../../sdk/docs/DEBUGGING-GUIDE.md) - Troubleshooting SDK issues
- [API Endpoints](../../sdk/docs/api-endpoints.md) - Endpoint reference

---

### **Contracts Documentation** ([contracts/](../../contracts/))

Solana smart contracts (Anchor framework).

#### [Contracts README](../../contracts/README.md)
**Purpose**: Smart contract deployment and testing

**Contents**: (Existing document)
- Program architecture
- Deployment instructions
- Testing guide

**Audience**: Smart contract developers, auditors

---

## 🗺️ Documentation Map

### By Role

**Frontend Developer**:
1. [Developer Onboarding](../../front-end/docs/DEVELOPER_ONBOARDING.md) ← Start here
2. [System Overview](./SYSTEM_OVERVIEW.md)
3. [API Endpoints](../../front-end/docs/API_ENDPOINTS.md)
4. [SDK Architecture](../../sdk/docs/ARCHITECTURE.md)

**Backend Developer**:
1. [Backend Architecture](../../backend/ARCHITECTURE.md) ← Start here
2. [Technical Flows](../../backend/docs/TECHNICAL_FLOWS.md)
3. [State Machines](../../backend/docs/STATE_MACHINES.md)
4. [System Overview](./SYSTEM_OVERVIEW.md)

**Product Manager**:
1. [User Journeys](./USER_JOURNEYS.md) ← Start here
2. [System Overview](./SYSTEM_OVERVIEW.md)
3. [Terminology](../../front-end/TERMINOLOGY.md)

**QA Engineer**:
1. [User Journeys](./USER_JOURNEYS.md) ← Start here
2. [Technical Flows](../../backend/docs/TECHNICAL_FLOWS.md)
3. [State Machines](../../backend/docs/STATE_MACHINES.md)
4. [Testing Strategy](../../front-end/docs/TESTING_STRATEGY.md)

**DevOps / SRE**:
1. [System Overview](./SYSTEM_OVERVIEW.md) ← Start here
2. [Deployment Guide](../../front-end/docs/DEPLOYMENT.md)
3. [Backend Architecture](../../backend/ARCHITECTURE.md)
4. [Monitoring Guide](../../backend/docs/MONITORING.md)

---

### By Topic

**Understanding the System**:
- [System Overview](./SYSTEM_OVERVIEW.md) - High-level architecture
- [Backend Architecture](../../backend/ARCHITECTURE.md) - Backend design
- [SDK Architecture](../../sdk/docs/ARCHITECTURE.md) - SDK structure

**User Flows**:
- [User Journeys](./USER_JOURNEYS.md) - Persona-based scenarios
- [Technical Flows](../../backend/docs/TECHNICAL_FLOWS.md) - Sequence diagrams
- [NFT Transfer Flow](../../front-end/docs/NFT_TRANSFER_FLOW.md) - Pack reveal mechanics

**Database & State**:
- [State Machines](../../backend/docs/STATE_MACHINES.md) - Entity lifecycles
- [Backend Architecture](../../backend/ARCHITECTURE.md) - Database schema

**API Integration**:
- [Frontend Integration Guide](../../backend/docs/FRONTEND-INTEGRATION-GUIDE.md) - API usage
- [API Endpoints](../../front-end/docs/API_ENDPOINTS.md) - Endpoint reference
- [SDK Architecture](../../sdk/docs/ARCHITECTURE.md) - SDK API

**Getting Started**:
- [Developer Onboarding](../../front-end/docs/DEVELOPER_ONBOARDING.md) - Frontend setup
- [Backend Environment Setup](../../backend/docs/ENVIRONMENT_SETUP.md) - Backend setup
- [Frontend Environment Setup](../../front-end/docs/ENVIRONMENT_SETUP.md) - Frontend setup

---

## 📊 Documentation Statistics

| Category | Documents | Total Pages |
|----------|-----------|-------------|
| **Organization-Level** | 3 docs | ~90 pages |
| **Frontend** | 15+ docs | ~150 pages |
| **Backend** | 20+ docs | ~200 pages |
| **SDK** | 5+ docs | ~50 pages |
| **Contracts** | 3+ docs | ~20 pages |
| **Total** | **40+ docs** | **~510 pages** |

---

## 🔄 Documentation Maintenance

### When to Update

**Always update docs when**:
- Adding new features
- Changing API endpoints
- Modifying state machines
- Updating architecture
- Changing deployment process

### How to Update

1. Find relevant document using this index
2. Make changes in appropriate repository
3. Update cross-references if needed
4. Commit with `docs:` prefix
5. Update this index if adding new docs

### Ownership

| Document Category | Maintained By |
|-------------------|---------------|
| Organization-level | Tech Lead |
| Frontend | Frontend Team |
| Backend | Backend Team |
| SDK | SDK Team |
| Contracts | Blockchain Team |

---

## 🎯 Quick Reference

### Key URLs

| Resource | URL |
|----------|-----|
| **Production** | https://altworth.netlify.app |
| **Staging** | https://staging.altworth.com |
| **Backend API (Prod)** | https://backend.altworth.com |
| **Backend API (Stage)** | https://backend-staging.altworth.com |
| **Organization GitHub** | https://github.com/altworth-markets |
| **Documentation** | https://github.com/altworth-markets/.github/blob/main/profile/DOCUMENTATION_INDEX.md |

### Key Concepts

| Term | Definition | Doc Reference |
|------|------------|---------------|
| **Capsule** | Container from gacha pull | [Terminology](../../front-end/TERMINOLOGY.md) |
| **Gacha Pool** | Collection of packs with drop rates | [System Overview](./SYSTEM_OVERVIEW.md#core-concepts) |
| **Two-Phase Signing** | Backend + User + Backend signature | [Technical Flows](../../backend/docs/TECHNICAL_FLOWS.md#nft-operations-flows) |
| **Vault-Based Custody** | Platform never owns NFTs | [Backend Architecture](../../backend/ARCHITECTURE.md#vault-based-custody-model) |

---

## 💬 Feedback & Questions

**Found an issue with documentation?**
- Create issue in relevant repository
- Tag with `documentation` label
- Suggest improvements

**Need clarification?**
- Ask in `#dev` Slack channel
- Tag document maintainer
- Check [FAQ section](#) (coming soon)

---

**Last Review**: November 7, 2025
**Maintained By**: Altworth Markets Tech Lead
**Next Review**: When onboarding next team member
