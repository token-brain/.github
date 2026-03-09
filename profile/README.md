# Token Brain

Custom MCP servers built on Cloudflare Workers, designed to give LLMs the persistent memory and external capabilities they don't have natively.

## The MCP servers

| MCP Server | What it does | Repo |
|--------|-------------|------|
| **Second Brain Vault MCP** | Read/write access to a personal document vault backed by Cloudflare R2. File operations, search, directory browsing, presigned URLs for binary transfers. | [mcp-second-brain-R2](https://github.com/token-brain/mcp-second-brain-R2) |
| **GitHub Issues MCP** | Full GitHub Issues management (issues, labels, milestones, comments) scoped to authorized repositories. Uses the OAuth token from authentication; no separate PAT needed. | [mcp-github-issues](https://github.com/token-brain/mcp-github-issues) |

Both servers connect directly to Claude.ai and Claude Desktop via remote MCP (HTTPS transport) with GitHub OAuth authentication handled by Cloudflare's `workers-oauth-provider`.

## Why these exist

Claude's remote MCP support is one of its most powerful features. But in practice, connecting custom tools requires a working OAuth flow; Claude.ai expects full OAuth discovery even when no authentication is configured.

Cloudflare Workers with GitHub OAuth turned out to be the path that actually works: the Worker handles the OAuth dance that Claude expects, the tools run on the edge, and Claude connects without issues. These two servers are the result.

## The bigger picture

These servers are components of a broader personal infrastructure for augmenting LLMs with persistent context and agency:

- **Obsidian vault** as source of truth (markdown-first principle)
- **Cloudflare R2 + Vault MCP** for remote document access from any Claude session
- **GitHub Issues MCP** for project and task management
- **Graphiti + Neo4j** for semantic memory and knowledge graph
- **Claude skills** (custom instruction files) for consistent behavior across sessions

The goal is to give LLMs persistent memory, document access, and the ability to act on external systems; not as a novelty, but as practical daily infrastructure.

## What I learned building these

**The OAuth situation is messy.** As of early 2026, Claude.ai's custom connector flow expects OAuth endpoints even in "authless" mode. Cloudflare Workers with `workers-oauth-provider` and GitHub as identity provider is the most reliable pattern I've found.

**Cloudflare Workers are a good fit for MCP.** Edge deployment means low latency, R2/KV bindings give you storage without managing databases, and the OAuth library handles the protocol complexity.

**Start with what the LLM can't do, not with what would be cool.** The vault server exists because I was tired of copy-pasting context between sessions. The GitHub server exists because I wanted to stop being the manual bridge between "what to do" and "track what's done." Each server solves one real friction point.

## About

Token Brain is a project by **Olivier Heckendorn**, engineer (INPG, ENSERG-ENSIMAG, systems architecture) and independent AI consultant based in France. A life lived as an archipelago: engineer and artist. From building early government websites in the 90s, to luxury photography (Cartier, Chanel, Dior), to teaching AI today; 30 years translating technology for human audiences.

These servers are part of a daily practice and also part of what gets taught. Understanding how to extend AI tools, not just use them, is increasingly the skill that matters.

Read more at [Between Carbon and Silicon](https://olivierh.bearblog.dev).

## License

MIT
