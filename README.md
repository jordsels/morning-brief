# Morning Brief

A prompt for [Claude Cowork](https://claude.ai/code) that generates a personalized daily HTML briefing by pulling from your connected tools (email, calendar, Slack, Substack, web search) and synthesizing everything into one scannable document.

## What it does

Generates a single self-contained HTML file with:

- **Top Stories** — Major news relevant to your work and interests, with "why it matters" context
- **On My Radar** — Industry articles and developments worth knowing about
- **Substack Feed** — Highlights from your newsletter subscriptions
- **Trends to Watch** — Emerging patterns synthesized from the day's reading
- **Ideas & Opportunities** — Actionable ideas connecting current events to your projects
- **Personal Comms** — Gmail inbox, personal calendar, iMessage summary
- **Work Comms** — Outlook, work calendar, Slack, Teams summary
- **7-Day Life Forecast** — Calendar + weather + open time blocks + training windows

Every item is badged (Action / FYI / OK) so you can scan quickly.

## Prerequisites

- [Claude Cowork](https://claude.ai/code) (desktop app or web) with access to web search
- One or more connected tools (see table below)

## Connected tools

The brief adapts to whatever tools you have configured. More tools = richer brief, but none are strictly required beyond web search.

| Tool | What it adds | Setup |
|------|-------------|-------|
| Web search | Top stories, trends, weather forecasts | Built in |
| Gmail | Personal email highlights | Claude Cowork integration |
| Google Calendar | Personal events, open time blocks | Claude Cowork integration |
| Outlook | Work email highlights | Access via Chrome browser automation |
| Slack | Work message highlights | Claude Cowork integration |
| Teams | Work message highlights | Access via Chrome browser automation |
| Substack MCP | Newsletter feed highlights | [substack-mcp](https://github.com/jordsels/substack-mcp) server |
| iMessage | Personal message summary | Claude iMessage extension |

## Usage

### As a skill (recommended)

Install `prompt.md` as a [Claude Cowork skill](https://docs.anthropic.com/en/docs/claude-code/skills) so you can invoke it with a slash command like `/morning-brief`. This is the cleanest way to use it regularly.

### As a scheduled trigger

Set up a [scheduled trigger](https://docs.anthropic.com/en/docs/claude-code/remote-triggers) to run the prompt automatically each morning — wake up to a fresh brief.

### One-off

You can also just ask Claude Cowork to generate a morning brief and point it at `prompt.md` for the format spec.

## Customization

The prompt is designed to be adapted:

- **Swap tools** — Use whatever email/calendar/messaging you have. Remove sections for tools you don't use.
- **Change sections** — Add, remove, or reorder sections to match what you care about.
- **Adjust framing** — The "why it matters" lines are written relative to your role and interests. Update the prompt to reflect your own context.
- **Training/fitness** — The 7-day forecast includes outdoor training recommendations. Adjust or remove based on your routine.

## Output

The brief is saved as a single `.html` file with inline CSS. No external dependencies — just open it in a browser. It's mobile-responsive and designed to be read on a phone over coffee.

## License

MIT
