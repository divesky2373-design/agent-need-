# Daily Registry Update Guide

## How to run a daily update

Use this prompt with Claude Code (or any AI coding agent) to update `needs.json`:

```
Read needs.json in this repository. For each task entry:

1. Web search for the current pricing of the "best" tool and each alternative
2. Check if there are any new tools that have launched or gained significant traction
3. Verify that all code examples still work with the latest SDK versions
4. Update pricing, free_tier, and last_verified fields with today's date
5. If a previously-recommended tool has been deprecated, replaced, or significantly degraded, update the "best" pick
6. Add any new task categories that have become common in agent workflows

After updating, commit the changes with a message like:
"Update registry: verify pricing and tools for YYYY-MM-DD"

Focus on accuracy. Only update fields where you find confirmed changes from official sources.
```

## Automation options

### Option A: Claude Code CLI (recommended)

Run this on a schedule (cron, GitHub Actions, etc.):

```bash
cd /path/to/agent-need-
claude -p "$(cat scripts/update-prompt.txt)"
```

### Option B: GitHub Actions

See `.github/workflows/daily-update.yml` (create this if you want fully automated updates).

Example workflow:

```yaml
name: Daily Registry Update
on:
  schedule:
    - cron: '0 9 * * *'  # 9 AM UTC daily
  workflow_dispatch:  # manual trigger

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Update registry
        run: |
          # Install Claude Code CLI
          npm install -g @anthropic-ai/claude-code
          # Run update
          claude -p "$(cat scripts/update-prompt.txt)" --allowedTools "WebSearch,WebFetch,Read,Write,Edit"
      - name: Commit and push
        run: |
          git config user.name "agent-need-bot"
          git config user.email "bot@agent-need.dev"
          git add needs.json
          git diff --cached --quiet || git commit -m "Update registry: $(date +%Y-%m-%d)"
          git push
```

### Option C: Manual

Just open Claude Code in this repo and say:

> Check and update all pricing in needs.json. Search official pricing pages for each tool.

## What to check

For each tool entry, verify:

1. **Pricing** — Has it changed? New tiers? Free tier modified?
2. **SDK versions** — Is the install command still current?
3. **Code examples** — Do they still work with the latest API?
4. **Alternatives** — Any new strong competitors?
5. **Deprecations** — Has any recommended tool been deprecated?
6. **New categories** — Are agents commonly needing tools for tasks not yet covered?

## Quality checklist

Before committing an update:

- [ ] All `last_verified` dates updated to today
- [ ] All pricing numbers come from official pricing pages
- [ ] Code examples are syntactically valid
- [ ] No broken documentation URLs
- [ ] `schema_version` bumped if structure changed
