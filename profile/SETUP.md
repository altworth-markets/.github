# Altworth Markets - Developer Setup

> **New to the platform?** Start here to get your development environment running.

---

## 🚀 Quick Start

```bash
# 1. Clone the monorepo (if you haven't already)
git clone https://github.com/altworth-markets/altworth-markets.git
cd altworth-markets

# 2. Choose what you're working on (see table below)
```

---

## 📦 What Are You Working On?

| Component | Description | Setup Guide | Time |
|-----------|-------------|-------------|------|
| **Backend API** | Node.js API server (Hono + PostgreSQL) | [backend/README.md](./backend/README.md#-quick-start) | 3-5 min |
| **Frontend UI** | React + Next.js web application | [front-end/README.md](./front-end/README.md#-quick-start) | 3 min |
| **Solana Contracts** | On-chain programs (Anchor framework) | [contracts/README.md](./contracts/README.md#-quick-start) | 5 min |
| **SDK Packages** | TypeScript SDK for Solana integration | [gacha-sdk/README.md](./gacha-sdk/README.md#-quick-start) | 2 min |

**👉 Click the setup guide link for your component to get started.**

---

## 🔥 Full-Stack Local Development

Want to run the entire platform locally? Follow this order:

### Terminal 1: Backend API

```bash
cd backend

# Install dependencies
pnpm install

# Run automated setup (database + migrations + seed data)
pnpm run quick-start

# Start server
pnpm dev

# Expected output: "Server running on http://localhost:3000"
```

**Note**: Backend uses **Doppler** for secrets. See [backend setup decision tree](./backend/docs/guides/SETUP_DECISION_TREE.md) if you don't have Doppler access.

### Terminal 2: Frontend UI

```bash
cd front-end

# Install dependencies
npm install

# Start development server
npm run dev:local

# Expected output: "Ready on http://localhost:3001"
```

### Verify Everything Works

```bash
# Check backend health
curl http://localhost:3000/health

# Check frontend (open in browser)
open http://localhost:3001
```

---

## 📋 Prerequisites

All repositories require:

| Tool | Minimum Version | Purpose | Installation |
|------|-----------------|---------|--------------|
| **Node.js** | 22.0+ (backend needs 24+) | JavaScript runtime | [nodejs.org](https://nodejs.org/) |
| **pnpm** | 10.0+ | Package manager | `npm install -g pnpm` |
| **Docker Desktop** | Latest | PostgreSQL, Redis | [docker.com](https://www.docker.com/products/docker-desktop/) |

**Verify installations:**

```bash
node --version   # Should be v22+ or v24+
pnpm --version   # Should be 10+
docker --version # Should be 20+
```

---

## 🔧 Package Manager

This monorepo uses **pnpm** (not npm, not yarn, not bun).

### Install pnpm

```bash
npm install -g pnpm
```

### Why pnpm?

- **Fast**: Disk-efficient with content-addressable storage
- **Strict**: Prevents phantom dependencies
- **Workspace-friendly**: Built-in monorepo support

### Common Commands

```bash
# See available scripts in any repo
pnpm run

# Install dependencies
pnpm install

# Run a script
pnpm run dev
pnpm run build
pnpm test
```

---

## 🗂️ Monorepo Structure

```
altworth-markets/
├── backend/          # Node.js API server (Hono + PostgreSQL + Redis)
├── front-end/        # React + Next.js web application
├── contracts/        # Solana programs (Anchor framework)
├── gacha-sdk/        # TypeScript SDK packages (monorepo within monorepo)
├── landing/          # Marketing landing page
├── .github/          # Organization-level docs and workflows
└── SETUP.md          # This file
```

---

## 🧭 Decision Trees

Not sure which setup path to take?

- **Backend setup**: [SETUP_DECISION_TREE.md](./backend/docs/guides/SETUP_DECISION_TREE.md)
  - Doppler vs .env
  - Automated vs manual setup
  - Troubleshooting common issues

- **Frontend setup**: See [front-end/README.md](./front-end/README.md#backend-connection)
  - Local vs hosted backend
  - Environment variables

---

## 📚 Additional Resources

### New Developer?

- 📖 **[Developer Onboarding](./front-end/docs/DEVELOPER_ONBOARDING.md)** - Comprehensive guide for new team members

### Understanding the Architecture?

- 🏗️ **[System Architecture](./backend/docs/architecture/SYSTEM_ARCHITECTURE.md)** - How everything fits together
- 🗄️ **[Database Schema](./backend/docs/architecture/DATABASE_SCHEMA.md)** - PostgreSQL tables and relationships
- 🔗 **[Cross-Repo Coordination](./.github/profile/CROSS_REPO_COORDINATION.md)** - How repos work together

### Working with Specific Technologies?

- **Solana/Web3**: [contracts/README.md](./contracts/README.md) + [gacha-sdk/README.md](./gacha-sdk/README.md)
- **Backend API**: [backend/docs/architecture/](./backend/docs/architecture/)
- **Frontend**: [front-end/docs/](./front-end/docs/)

---

## 🐛 Troubleshooting

### "Command not found: pnpm"

```bash
# Install pnpm globally
npm install -g pnpm

# Verify installation
pnpm --version
```

### "Port already in use"

```bash
# Find what's using the port (example: 3000)
lsof -i :3000

# Kill the process
kill -9 <PID>

# Or use different port
SERVER_PORT=3001 pnpm dev
```

### "Docker connection refused"

```bash
# Start Docker Desktop (macOS)
open -a Docker

# Or start Docker daemon (Linux)
sudo systemctl start docker

# Verify Docker is running
docker info
```

### "Database connection failed"

```bash
# Check if database container is running
docker ps | grep postgres

# Start database
cd backend
pnpm db:up && pnpm db:wait
```

### Still Stuck?

- **Backend issues**: [backend troubleshooting guide](./backend/docs/operations/TROUBLESHOOTING.md)
- **Frontend issues**: [frontend README](./front-end/README.md#troubleshooting)
- **SDK issues**: [SDK troubleshooting](./gacha-sdk/README.md#-troubleshooting)

---

## 🎯 Philosophy: Code as Documentation

> **IMPORTANT**: Text documentation can go stale. **The `package.json` scripts are the single source of truth.**

To see all available commands in any repository:

```bash
pnpm run        # Backend, SDK
npm run         # Frontend
```

Read more: [Code as Documentation Philosophy](./.github/CODE-AS-DOCUMENTATION.md)

---

## 🤝 Contributing

1. **Check the component's README** for specific contribution guidelines
2. **Run validation** before pushing:
   ```bash
   pnpm run format      # Auto-fix formatting
   pnpm run type-check  # TypeScript validation
   pnpm run lint        # Linting
   pnpm test            # Tests
   pnpm run build       # Build validation
   ```
3. **Follow commit conventions**: See [git workflow](./.github/profile/GIT_WORKFLOW.md)

---

## 📞 Need Help?

- 💬 **Team chat**: Ask in #engineering
- 🐛 **Bug reports**: [GitHub Issues](https://github.com/altworth-markets)
- 📖 **Documentation issues**: [.github repository](https://github.com/altworth-markets/.github/issues)

---

**Last Updated**: 2025-12-03
**Maintained By**: Engineering Team
**Related**: [CODE-AS-DOCUMENTATION.md](./.github/CODE-AS-DOCUMENTATION.md), [DOCUMENTATION-FIX-PLAN.md](./.github/DOCUMENTATION-FIX-PLAN.md)
