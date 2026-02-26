# agent-need

> Agents keep picking outdated tools. This registry helps them make better choices.

An open-source, agent-readable **tool recommendation registry** with verified pricing data.

The core is a single file: [`needs.json`](./needs.json). An agent reads it, knows what tool to use for a given task, and gets ready-to-run code. All pricing verified from official sources (Feb 2026).

## Why this exists

YC CEO Garry Tan was building a project with Claude Code. The agent automatically chose Whisper V1 for audio transcription — processing one hour of audio took a full hour. Switching to Groq Whisper would have been 216x faster and 9x cheaper.

**Why did the agent pick wrong? Because old tools have more documentation online, so the agent defaulted to them.**

This happens every day in countless agent workflows. `agent-need` gives agents a structured, up-to-date recommendation source so they stop guessing.

## Registry (26 tasks)

| Task | Best Pick | Why |
|------|-----------|-----|
| Audio transcription | Groq Whisper | 216x real-time, $0.04/hr (9x cheaper than OpenAI) |
| Send email | Resend | Agent-friendly API, 3K emails/mo free |
| Receive email | Resend Inbound | Webhook-based, simple setup |
| Database | Supabase | Postgres + auto REST API + auth, 500MB free |
| Object storage | Cloudflare R2 | S3-compatible, zero egress fees ever |
| Web scraping | Firecrawl | Returns clean markdown for agents |
| Authentication | Clerk | Dedicated agent toolkit + MCP server, 50K MRUs free |
| Payments | Stripe | Gold-standard API docs, 2.9% + $0.30 |
| Vector database | Qdrant | Open source, 60-80% cheaper than Pinecone |
| Caching | Upstash Redis | HTTP-based (serverless), 500K commands/mo free |
| Search | Meilisearch | Open source (MIT), has MCP server, 8.9x cheaper than Algolia |
| SMS | Twilio | Best docs, $15.50 trial credit |
| Background jobs | Inngest | Serverless, 50K executions/mo free |
| Real-time | Ably | 6M messages/mo free (most generous) |
| PDF generation | Puppeteer | HTML-to-PDF, free and open source |
| Image processing | Cloudinary | URL-based transforms, 25 credits/mo free |
| Error tracking | Sentry | Industry standard, 5K errors/mo free |
| AI gateway | LiteLLM | Open source, 100+ LLM providers via single API |
| LLM observability | Langfuse | Open source, 26M+ SDK installs, traces LLM calls |
| Agent memory | Mem0 | Persistent memory for agents, 10K memories free |
| Notifications | Novu | Open source, multi-channel (push+email+SMS+in-app) |
| Analytics | PostHog | Open source, 1M events/mo free, all-in-one |
| Deployment | Vercel | Git push to deploy, best DX for Next.js |
| ORM | Drizzle ORM | SQL-first, 7.4KB, zero-dep, serverless-native |
| Text-to-speech | ElevenLabs | Most natural voices, 70+ languages |
| Feature flags | PostHog | Unlimited flags free, linked to analytics |

Full details, code examples, and alternatives in [`needs.json`](./needs.json).

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
    "why": "216x real-time speed, $0.04/hr...",
    "setup": { "install": "pip install groq", "env": ["GROQ_API_KEY"] },
    "code": { "python": "...", "curl": "..." },
    "pricing": "$0.04/hr (v3-turbo)...",
    "free_tier": "28,800 audio-seconds/day",
    "docs": "https://...",
    "last_verified": "2026-02-25"
  },
  "alternatives": [...]
}
```

Design principles:
- **Opinionated** — Gives a single best pick, not a list of 50 options
- **Code-ready** — Every recommendation includes copy-paste executable code
- **Verified** — All pricing from official sources with `last_verified` dates
- **Practical** — Includes free tier details, install commands, env vars

## Agent discovery

This project includes [`llms.txt`](./llms.txt) (following the emerging LLM discovery standard) and [`AGENTS.md`](./AGENTS.md) with detailed usage instructions written specifically for AI agents.

## Contributing

PRs welcome to add new tasks or update existing recommendations. Each entry needs:

- Clear task description and search keywords
- Recommended tool with specific reasoning (speed, cost, DX)
- Install commands and required environment variables
- Ready-to-run code examples (Python and/or Node.js minimum)
- Verified pricing with source links
- At least one alternative
- Verification date

## License

MIT
