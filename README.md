<p align="center">
  <img src="public/xata-logo.svg" alt="Xata" width="80" />
</p>

<h1 align="center">Next.js + Xata + Claude Code Starter</h1>

<p align="center">
  Production-ready starter for building AI apps with zero-downtime migrations, instant branches and anonymized cloning.
</p>

<p align="center">
  <a href="https://xata.io/documentation">Docs</a> · <a href="https://github.com/xataio/pgroll">pgroll</a> · <a href="https://xata.io/discord">Discord</a>
</p>

---

## Features

- **Zero-downtime migrations** — pgroll's expand-contract pattern lets old and new code run simultaneously
- **Instant database branches** — copy-on-write isolation for every feature, no shared dev database chaos
- **Anonymized production cloning** — debug with real data distributions without exposing PII
- **Claude Code commands** — branching, migrating and cloning as part of your agentic workflow

## Prerequisites

- Node.js 18+
- A [Xata account](https://console.xata.io)
- [Xata CLI](https://xata.io/documentation/cli) installed

## Quick start

### 1. Clone and install

```bash
git clone https://github.com/xataio/nextjs-claude-code-starter
cd nextjs-claude-code-starter
npm install
```

### 2. Install the Xata CLI

```bash
curl -fsSL https://xata.io/install.sh | bash
export PATH="$HOME/.config/xata/bin:$PATH"
```

### 3. Connect to Xata

```bash
xata auth login
xata init
```

Follow the prompts to select your organization, project and branch.

### 4. Set up your environment

```bash
xata branch url
# Copy the connection string
cp .env.example .env.local
# Open .env.local and paste the connection string as the value of DATABASE_URL
```

### 5. Initialize pgroll and run migrations

```bash
xata roll init
xata roll start migrations/001_create_users.yaml
xata roll complete
xata roll start migrations/002_add_role.yaml
xata roll complete
xata roll start migrations/003_add_teams.yaml
xata roll complete
```

`roll init` is a one-time setup. Each `roll start` begins the **expand phase** (both old and new schemas serve traffic), and `roll complete` runs the **contract phase** (old schema removed).

### 6. Start the app

```bash
npm run dev
```

You'll see users and teams tables — empty but with the correct columns. Add a test user:

```bash
psql $(xata branch url) -c "INSERT INTO users (email, name) VALUES ('test@example.com', 'Test User');"
```

---

## Claude Code skills

This starter includes Claude Code skills for Xata workflows. Open Claude Code in this directory and use these slash commands:

| Command | Description |
|---------|-------------|
| `/project:setup` | Connect to Xata and configure the project |
| `/project:branch-create` | Create an isolated database branch |
| `/project:migration-start` | Start a zero-downtime migration |
| `/project:migration-complete` | Complete an ongoing migration |
| `/project:migration-rollback` | Roll back a failed migration |
| `/project:clone-production` | Clone production with PII anonymization |

<p align="center">
  <img src="public/xata-claude-branch-demo.png" alt="Claude Code creating a Xata branch" width="700" />
</p>
<p align="center"><em>Claude Code running /branch-create — names the branch, creates it, waits for ready and returns the connection string.</em></p>

---

## Project structure

```
.
├── .claude/skills/      # Claude Code slash command skills
├── migrations/           # pgroll migration files (YAML)
│   ├── 001_create_users.yaml
│   ├── 002_add_role.yaml
│   └── 003_add_teams.yaml
├── src/
│   ├── app/             # Next.js app router
│   └── lib/             # Database connection (postgres driver)
├── .env.example
└── package.json
```

---

## Xata CLI reference

| Command | What it does |
|---------|--------------|
| `xata auth login` | Authenticate with Xata |
| `xata init` | Link project to current folder |
| `xata branch create --name <name>` | Create an isolated database branch |
| `xata branch wait-ready <name>` | Wait for a branch to be ready |
| `xata branch url [branch]` | Get connection string |
| `xata roll init` | One-time pgroll setup |
| `xata roll start <file>` | Begin expand phase (both schemas live) |
| `xata roll complete` | Contract phase (old schema removed) |
| `xata roll status` | Check migration progress |
| `xata roll rollback` | Undo expand phase if needed |
| `xata clone start --source-url <url>` | Clone with PII anonymization |

---

## Learn more

- [Xata Documentation](https://xata.io/documentation)
- [pgroll on GitHub](https://github.com/xataio/pgroll)
- [Data Anonymization](https://xata.io/documentation/core-concepts/data-anonymization)

## License

MIT
