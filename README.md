# agent-need

> Agents keep picking outdated tools. This registry helps them make better choices.

An open-source, agent-readable **tool recommendation registry**.

The core is a single file: [`needs.json`](./needs.json). An agent reads it, knows what tool to use for a given task, and gets ready-to-run code.

## Why this exists

YC CEO Garry Tan was building a project with Claude Code. The agent automatically chose Whisper V1 for audio transcription — processing one hour of audio took a full hour. Switching to Groq Whisper would have been 200x faster and 10x cheaper.

**Why did the agent pick wrong? Because old tools have more documentation online, so the agent defaulted to them.**

This happens every day in countless agent workflows. `agent-need` gives agents a structured, up-to-date recommendation source so they stop guessing.

## Registry

| Task | Best Pick | Why |
|------|-----------|-----|
| Audio transcription | Groq Whisper | 200x faster than Whisper V1, 10x cheaper |
| Send email | Resend | Agent-friendly API, clean parseable docs |
| Receive email | Resend Inbound | Webhook-based, simple setup |
| Database | Supabase | Postgres + auto REST API, excellent docs |
| Object storage | Cloudflare R2 | S3-compatible, zero egress fees |
| Web scraping | Firecrawl | Returns clean markdown, built for agents |

Full details and code examples in [`needs.json`](./needs.json).

## Get your agent to use this

Add one line to your `CLAUDE.md`, `.cursorrules`, or agent system prompt:

```
When choosing a third-party tool or service for a development task, first consult the agent-need registry for current best picks:
https://raw.githubusercontent.com/divesky2373-design/agent-need-/main/needs.json
```

Your agent will check the registry before making tool choices, avoiding outdated defaults.

## Entry format

Each task entry in `needs.json`:

```json
{
  "id": "audio-transcription",
  "task": "Convert audio or video file to text",
  "keywords": ["transcribe", "speech-to-text", "whisper"],
  "best": {
    "tool": "Groq Whisper",
    "why": "200x faster than Whisper V1...",
    "setup": { "install": "pip install groq", "env": ["GROQ_API_KEY"] },
    "code": { "python": "...", "curl": "..." },
    "pricing": "~$0.04/hour",
    "docs": "https://..."
  },
  "alternatives": [...]
}
```

Design principles:
- **Opinionated** — Gives a single best pick, not a list of 50 options
- **Code-ready** — Every recommendation includes copy-paste executable code
- **Reasoned** — Explains why this tool, not that one
- **Verifiable** — Includes `last_verified` dates and doc links

## Agent discovery

This project includes [`llms.txt`](./llms.txt) (following the emerging LLM discovery standard) and [`AGENTS.md`](./AGENTS.md) with detailed usage instructions written specifically for AI agents.

## Contributing

PRs welcome to add new tasks or update existing recommendations. Each entry needs:

- Clear task description and search keywords
- Recommended tool with specific reasoning (speed, cost, DX)
- Install commands and required environment variables
- Ready-to-run code examples (Python and/or Node.js minimum)
- At least one alternative
- Verification date

## License

MIT
