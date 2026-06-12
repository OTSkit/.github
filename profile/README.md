<p align="center">
  <img src="https://raw.githubusercontent.com/OTSkit/.github/main/profile/logo.png" alt="OTSkit" width="720" />
</p>

# OTSkit

## Cryptographic timestamps anchored to Bitcoin — for every file, every agent, every release.

Prove that a document existed before a specific moment in time. No subscription. No account.
No server to trust. The proof lives in a `.ots` file and in the Bitcoin blockchain — and stays
valid forever, even if OTSkit disappears tomorrow.

---

## How it works

[OpenTimestamps](https://opentimestamps.org) is an open standard for decentralized timestamp
proofs. When you stamp a file, its cryptographic hash is submitted to public calendar servers
that batch-anchor it into a Bitcoin block. The result is a compact `.ots` proof file you keep
locally — no trusted third party required to verify it, ever.

---

## Why OTSkit

Most existing OpenTimestamps tools are either abandoned, Python-only, or lack the resilience
needed for production pipelines. OTSkit is a TypeScript-native implementation built for
real-world use: circuit breakers, local SQLite persistence, automatic upgrade scheduling, and
an MCP server so any AI agent can stamp files from a conversation.

**If OTSkit disappears tomorrow, every proof ever generated stays valid forever.**

---

## Use cases

**AI agent audit trail** — Every document your agent processes gets a tamper-proof timestamp.
Prove what your agent did, on what file, and when.

**File integrity** — Stamp contracts, reports, or source code. Verify later that nothing changed.

**Software releases** — Timestamp each build artifact or SBOM. Prove your release existed
before a specific date.

**Digital preservation** — Generate OAIS/PREMIS-compliant preservation packages anchored
to Bitcoin.

---

## The ecosystem

| Package | What it does |
|---|---|
| [`@otskit/core`](https://github.com/OTSkit/OTSkit-core) | OpenTimestamps protocol engine — zero dependencies, TypeScript-native |
| [`@otskit/client`](https://github.com/OTSkit/OTSkit-client) | Production SDK with circuit breakers, SSRF protection, auto-retry |
| [`@otskit/mcp`](https://github.com/OTSkit/OTSkit-MCP) | MCP server — 8 tools for Claude, Codex, and any MCP-compatible agent |
| [OTSkit Skills](https://github.com/OTSkit/OTSkit-skills) | Agent skills for BagIt/OAIS/PREMIS digital preservation workflows |

### `@otskit/core`

The low-level protocol engine. A zero-dependency, pure TypeScript implementation of the
OpenTimestamps protocol — create, serialize, deserialize, merge, and verify timestamp proofs
with no npm supply chain risk. Supports Merkle tree batching so thousands of documents can be
anchored in a single Bitcoin transaction. Built with TypeScript 6 strict mode and a
fail-closed security posture that rejects any malformed input by default.

### `@otskit/client`

The production SDK. Wraps the full stamp → upgrade → verify workflow with enterprise-grade
resilience: per-calendar circuit breakers, exponential backoff with jitter, dual timeouts, and
configurable N-of-M submission thresholds so one failing calendar never blocks the rest.
Requires Node.js 20+ (uses native `crypto`, `dns`, and `net` APIs). Tree-shakeable dual
ESM/CJS build with `@otskit/core` as its only runtime dependency.

### `@otskit/mcp`

An MCP server exposing 8 timestamping tools to any MCP-compatible AI agent — Claude, Codex,
or any agent that speaks the Model Context Protocol. Stores all proofs in a local SQLite
database, runs an automatic upgrade scheduler to detect Bitcoin confirmations, and includes a
CLI for direct use outside of agents. Setup is a single command.

### OTSkit Skills

Agent skills for producing standards-compliant digital preservation packages. Say *"preserve
this folder"* and your agent generates a BagIt (RFC 8493) archive with OAIS/PREMIS metadata,
computes its hash, and anchors it to Bitcoin — delivering four portable files: a `.zip`, a
`.sha256`, a `.ots` proof, and a stamp ID for future verification.

---

## Quick start

### With an AI agent (MCP)

> New to MCP? → [5-minute setup guide](https://github.com/OTSkit/OTSkit-MCP#agent-setup)

```bash
npm install -g @otskit/mcp
ots-mcp setup claude-code   # or: claude, codex
```

Restart your agent and ask: *"Stamp this file and prove it exists on Bitcoin."*

### In your TypeScript code

```typescript
import { OpenTimestampsClient } from '@otskit/client';
import { createHash } from 'node:crypto';
import { readFileSync } from 'node:fs';

const hash = createHash('sha256').update(readFileSync('document.pdf')).digest('hex');
const client = new OpenTimestampsClient();
const proof = await client.stamp(hash); // Returns .ots proof as Buffer

// Later — upgrade when Bitcoin has confirmed (~10–60 min)
const upgraded = await client.upgrade(proof);
```

---

## How OTSkit compares

The closest alternatives are [OriginStamp](https://originstamp.com) (commercial, proprietary
proof format), the [OpenTimestamps CLI](https://opentimestamps.org) (Python, no TypeScript SDK),
and [getAlby/ots-mcp](https://github.com/getAlby/ots-mcp) (MCP-only, no local persistence):

| | OTSkit | OriginStamp | OpenTimestamps CLI | getAlby/ots-mcp |
|---|---|---|---|---|
| Cost per stamp | Free | ~$12K/yr (500K stamps) | Free | Free |
| Proof format | .ots (open standard) | Proprietary (JSON/PDF) | .ots (open standard) | .ots (open standard) |
| Works if service disappears | ✅ Always | ⚠️ Only if you downloaded the proof | ✅ Always | ✅ Always |
| TypeScript SDK | ✅ Native | ⚠️ Abandoned (2021) | ⚠️ Exists, not primary | ❌ |
| MCP integration (AI agents) | ✅ 8 tools | ❌ | ❌ | ✅ 6 tools |
| Local persistence (SQLite) | ✅ | ❌ Cloud only | ❌ Files only | ❌ |
| Auto-upgrade scheduler | ✅ | ❌ | ❌ | ❌ |
| Resilience (circuit breakers) | ✅ Per-calendar | ❌ | ⚠️ Partial | ❌ |
| OAIS / PREMIS preservation | ✅ via Skills | ❌ | ❌ | ❌ |
| Open source | ✅ MIT | ⚠️ SDKs archived | ✅ LGPL | ✅ MIT |

---

## What this proves — and what it doesn't

OTSkit proves that **a specific file existed before a specific Bitcoin block**. That's the
guarantee, and it's a strong one.

It does **not** prove:
- Who created the file
- That the file hasn't changed since
- Anything about legal validity in any jurisdiction

The proof is cryptographic, not legal. What it means in a legal context depends on your
jurisdiction and circumstances. Consult a lawyer for use in legal proceedings.

---

*MIT License · Built on the [OpenTimestamps](https://opentimestamps.org/) open standard*
