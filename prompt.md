# Morning Brief Prompt

Install as a [Claude Cowork skill](https://docs.anthropic.com/en/docs/claude-code/skills), use as a [scheduled trigger](https://docs.anthropic.com/en/docs/claude-code/remote-triggers), or paste directly into a conversation.

Before using, fill in the `[PLACEHOLDER]` fields in the "Who You Are" section with your own details. Remove any sections or tools you don't use.

---

## The Prompt

Generate "[YOUR NAME]'s Morning Brief" — a personalized daily HTML report.

### Who You Are

Fill in your details so the brief can tailor news relevance, calendar sources, and communication channels to you.

```
- Name: [YOUR NAME]
- Location: [CITY, STATE ZIP]
- Timezone: [YOUR TIMEZONE]
- Phone: [YOUR PHONE NUMBER] (for optional iMessage notification)
- Personal email: [YOUR PERSONAL EMAIL]
- Work email: [YOUR WORK EMAIL]
- Work: [YOUR ROLE at YOUR COMPANY — what you do, current projects]
- Side Projects: [ANY SIDE PROJECTS — apps, businesses, research]
- Interests: [TOPICS YOU FOLLOW — fitness, industry trends, hobbies]
- Personal: [ANYTHING ELSE RELEVANT — family, community involvement, long-term goals]
```

### STEP 1: News + Substack (7 web searches + feed)

Search these topics and collect 3-5 items each:

1. Breaking / top news (US + world)
2. [YOUR INDUSTRY] ecosystem
3. AI tooling for [YOUR FIELD]
4. [INTEREST 1]
5. [INTEREST 2]
6. [INTEREST 3]
7. [INTEREST 4]

Also pull your Substack feed using `mcp__substack__substack_get_feed` (limit 10). Filter to the most relevant posts for your interests.

### STEP 2: Communications (7 sources)

Check these and summarize each:

1. **Gmail** (personal) — via Gmail MCP tools
2. **Google Calendar** (personal) — via Google Calendar MCP tools
3. **iMessage** — via iMessage MCP tools
4. **Outlook inbox** (work) — via Claude in Chrome MCP. Switch to your Work Chrome profile, navigate to https://outlook.office.com, read inbox using `read_page` or `get_page_text`. Assumes auto-login on the work profile.
5. **Outlook Calendar** (work) — via Claude in Chrome MCP. Navigate to https://outlook.office.com/calendar/view/week in the Work Chrome profile.
6. **Slack** (work) — via Slack MCP tools
7. **Teams** — via Claude in Chrome MCP. Navigate to https://teams.microsoft.com/v2/ in the Work Chrome profile, read recent chats/activity.

IMPORTANT for Outlook and Teams: Use the Claude in Chrome MCP (`mcp__claude-in-chrome__*`) tools, NOT computer-use. First confirm the active Chrome profile is your Work profile. Use `switch_browser` if needed. Then use `navigate`, `read_page`, `get_page_text`, and `computer` (screenshot) tools to read content.

### STEP 3: 7-Day Life Forecast

3a. Pull personal calendar (Google Calendar MCP) + work calendar (Outlook web via Chrome MCP) for the next 7 days

3b. Search weather for [YOUR CITY] — 7-day forecast

3c. Combine into a week-ahead summary: calendar events, weather, training windows, open blocks, priorities, heads-up items

### STEP 4: Dedup

Check `~/Documents/Claude Cowork/morning-brief-dedup-log.json`. Suppress stories already covered in the last 3 days. After report, update the log. Remove entries older than 7 days.

### STEP 5: Assemble Sections

- **A — Top Stories** (3 cards with emoji pairs, full paragraph + why it matters + action)
- **B — On My Radar** (scan-grid cards, see design spec below)
- **Substack Feed** (scan-grid cards, see design spec below)
- **C — Trends to Watch** (scan-grid cards, see design spec below)
- **D — Ideas & Opportunities** (scan-grid cards, see design spec below)
- **E — Personal Comms** (Gmail, Google Calendar, iMessage — list format with traffic light badges)
- **F — Work Comms** (Outlook inbox, Outlook calendar, Slack, Teams — list format with traffic light badges)
- **G — 7-Day Life Forecast** (hybrid hero + mini grid layout + week-ahead narrative)
- **H — Background / Already Covered** (collapsible)
- **Status line** (checkmarks for each source)

### STEP 6: HTML Production

Self-contained HTML file. Styles:

- Deep navy header (`#1a2744`)
- Section accent colors (red A, blue B, purple C, green D, amber E, teal F, indigo G)
- Card shadows, responsive `max-width: 860px`

#### Collapsible Sections

IMPORTANT: Every section (A through H) must be collapsible using HTML `<details>` and `<summary>` elements. Use this structure for each section:

```html
<details class="section-collapse" open>
  <summary><span class="section-label label-a">A</span> Top Stories</summary>
  <div class="section-body">
    <!-- section content here -->
  </div>
</details>
```

- Sections A, B, S (Substack), E, F, and G should default to **OPEN** (add the `open` attribute)
- Sections C (Trends), D (Ideas), and H (Background) should default to **CLOSED** (no `open` attribute)
- Each summary row shows a small triangle toggle that rotates on expand/collapse

Required CSS for collapsible sections:

```css
details.section-collapse { background: #fff; padding: 0; border-bottom: 1px solid #e5e7eb; }
details.section-collapse:last-of-type { border-radius: 0 0 12px 12px; border-bottom: none; }
details.section-collapse > summary { padding: 18px 28px; cursor: pointer; list-style: none; display: flex; align-items: center; gap: 8px; font-size: 16px; font-weight: 700; user-select: none; }
details.section-collapse > summary::-webkit-details-marker { display: none; }
details.section-collapse > summary::before { content: '\25B6'; font-size: 10px; color: #9ca3af; transition: transform .2s; flex-shrink: 0; width: 14px; }
details.section-collapse[open] > summary::before { transform: rotate(90deg); }
details.section-collapse > summary:hover { background: #f9fafb; }
details.section-collapse .section-body { padding: 0 28px 24px; }
```

CRITICAL: The triangle character MUST use a single backslash CSS escape: `content: '\25B6'` — NOT double-backslash `'\\25B6'` which renders as literal text.

Do NOT use `<div class="section">` with `<div class="section-title">` for sections. Use the `<details>/<summary>` pattern above instead.

For section H (Background), nest a second `<details class="bg-collapse">` inside the section-body for the inner expand.

#### Scan-Grid Card Design (Sections B, Substack, C, D)

Two-column card grid:

```css
.scan-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; }
@media (max-width: 600px) { .scan-grid { grid-template-columns: 1fr; } }
.scan-card { background: #fff; border: 1px solid #e5e7eb; border-radius: 8px; padding: 12px 14px; display: flex; gap: 10px; align-items: flex-start; box-shadow: 0 1px 2px rgba(0,0,0,0.04); }
.scan-card .sc-emoji { font-size: 20px; flex-shrink: 0; }
.scan-card .sc-headline { font-size: 13px; font-weight: 700; color: #1a2744; }
.scan-card .sc-line { font-size: 12px; color: #6b7280; }
.scan-card.sc-full { grid-column: 1 / -1; }
```

#### Traffic Light Badges (Sections E, F)

```css
.badge { font-size: 10px; font-weight: 700; text-transform: uppercase; padding: 2px 8px; border-radius: 10px; display: inline-block; margin-left: 6px; vertical-align: middle; }
.badge-action { background: #fef2f2; color: #dc2626; border: 1px solid #fecaca; }
.badge-fyi { background: #fffbeb; color: #b45309; border: 1px solid #fde68a; }
.badge-ok { background: #f0fdf4; color: #16a34a; border: 1px solid #bbf7d0; }
```

#### 7-Day Life Forecast — Hybrid Hero + Mini Grid

Hero cards (today + tomorrow) with today getting purple left border. Mini 5-column grid for remaining days. Flex header with day/date left, weather right.

Shared rules: best weather day highlights ONLY the weather element (`.cal-best-wx` green background). Work events use teal left-border (`.cal-ev-work`). Training tags: `.cal-train-great` (green), `.cal-train-ok` (blue), `.cal-train-bad` (red). Open blocks: green background at bottom of each cell.

Save to: `~/Documents/Claude Cowork/Personal/Documents/YYYY-MM-DD_[name]-morning-brief.html`

### STEP 7: iMessage Notification (optional)

After the HTML brief is saved, send an iMessage to `[YOUR PHONE NUMBER]` with this format:

```
Morning Brief - [Day of week] [Mon] [Date]

1. [Top calendar item with time]
2. [Second calendar item with time]
3. [Third calendar item with time]

Full brief on your Mac. [One-line weather summary with high temp].
```

Pull the top 3 TIMED calendar items from BOTH Google Calendar and Outlook Calendar for today, sorted by start time, excluding all-day events. If fewer than 3 timed events, show what exists. Include only meetings/calls with specific start times.

### STEP 8: Status Line

Show checkmark/X for each data source with counts.

### Tone & Style Rules

- Bottom line up front. No filler.
- Weather: always show BOTH Fahrenheit and Celsius in LOW/HIGH order (e.g. "38/52F / 3/11C")
- Emoji pairs on Section A cards, single emojis on B/C/D
- "Why it matters to you" on every top story
- Actionable language throughout
- Quick-scan depth — under 5 minutes to read

---

## Tool Requirements

| Tool | Used for | Required? |
|------|----------|-----------|
| Web search | Top Stories, On My Radar, Trends, weather | Yes |
| Gmail MCP | Personal email scanning (Section E) | Optional |
| Google Calendar MCP | Personal calendar events (Sections E, G) | Optional |
| Outlook via Chrome MCP | Work email + work calendar (Sections F, G) | Optional |
| Slack MCP | Work messaging (Section F) | Optional |
| Teams via Chrome MCP | Work messaging (Section F) | Optional |
| Substack MCP | Subscription feed (Substack section) | Optional |
| iMessage MCP | Personal messaging (Section E) + notification (Step 7) | Optional |
| Claude in Chrome MCP | Browser automation for Outlook + Teams | Optional |
