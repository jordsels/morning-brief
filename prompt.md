# Morning Brief Prompt

Paste this prompt into Claude Code (or use it as a scheduled trigger) to generate a personalized daily HTML briefing.

---

## The Prompt

Generate a morning brief as a single self-contained HTML file. The brief should be a personal daily briefing that synthesizes information from my connected tools and the web into one scannable document.

### Output format

- Single HTML file with inline CSS (no external dependencies)
- Mobile-responsive (works on phone and desktop)
- Clean, modern design with a dark header showing the title and today's date
- Each section has a color-coded title bar and uses card-based layouts
- Use a badge system for urgency: red "Action" badges for items needing a response, amber "FYI" badges for awareness items, and green "OK" badges for confirmed/completed items

### Sections

Build the brief with these sections in order:

**A — Top Stories** (red accent)
- Use **web search** to find 2-4 major news stories relevant to my industry, role, and interests
- Each story gets a full-width card with: emoji icon, headline, 2-3 sentence summary, "Why it matters to you" line connecting it to my work or projects, and an italicized action/talking point
- Prioritize stories I can act on or that affect my work this week

**B — On My Radar** (blue accent)
- Use **web search** to find 4-6 professional/industry articles, product updates, or developments worth knowing about
- Display as a 2-column scannable grid of small cards, each with: emoji, linked headline, and one-line summary explaining relevance
- These are less urgent than Top Stories — more "worth skimming" items

**Substack Feed** (orange accent)
- Use the **Substack MCP tools** (`substack_get_feed`) to pull recent posts from my subscriptions
- Display 4-6 posts as a 2-column grid (same format as On My Radar): emoji, linked headline, one-line summary with relevance note
- If there are more than 6, put the rest in a collapsible "X more posts" section

**C — Trends to Watch** (purple accent)
- Synthesize 2-3 emerging trends from the web search and Substack reading
- Full-width cards with: emoji, trend name as headline, and a one-line explanation of why it matters
- These should be patterns, not individual news items

**D — Ideas & Opportunities** (green accent)
- Based on everything gathered so far, suggest 2-3 actionable ideas or opportunities
- Full-width cards with: emoji, idea name, and one-line description of what to do
- Connect dots between news, trends, and my current projects/interests

**E — Personal Comms** (amber accent)
- **Gmail**: Use Gmail tools to scan inbox. List items needing action with "Action" badges, confirmations with "OK" badges, and FYI items. Summarize promotional/newsletter volume in one italic line
- **Google Calendar**: Use Google Calendar tools to show today's and tomorrow's events with times, locations, and any prep notes (e.g., "arrive 15 min early, bring X")
- **iMessage**: If available, summarize any unread messages with sender and brief context

**F — Work Comms** (teal accent)
- **Outlook**: Scan work inbox for action items, FYIs, and confirmations. Badge each item. Summarize bulk/notification emails in one italic line
- **Work Calendar**: Show the full work week's meetings with day labels and meeting counts. Badge busy days as "Heavy" and light days as "Light"
- **Slack**: Check DMs and key channels for anything needing a response. Badge with "Input needed" or "Done" as appropriate
- **Teams**: Summarize any unread activity. If it's just meeting recordings/notifications, note "No action" and move on

**G — 7-Day Life Forecast** (indigo accent)
- Build a responsive grid with one card per day for the next 7 days
- Each day card includes:
  - Day name + date number in a header row
  - Weather: emoji icon + high/low temperature (use **web search** for forecast)
  - Weather detail line (e.g., "Partly sunny", "Heavy rain")
  - Training/outdoor recommendation tag: green "Best outdoor day" / blue "Good outdoor day" / red "Indoor day" based on weather
  - Events from both personal and work calendars, with times and locations
  - Alert lines for events needing prep (what to bring, travel time, etc.)
  - An "open block" footer showing the largest free window and a suggestion for how to use it
- Highlight today's card with a colored border
- Highlight the best weather day with a green-accented temperature display
- After the grid, add a "Week Ahead Summary" narrative box with subsections:
  - **Focus Areas**: Key priorities and action items for the week
  - **Open Windows**: Best time blocks for deep work or side projects
  - **Heads Up**: Logistical things to remember (travel time, what to bring, deadlines)
  - **Training Windows**: Best days/times for outdoor exercise based on weather

**H — Background** (collapsible)
- A collapsed `<details>` section for context that's relevant but not new
- Items that were covered in previous briefs or are ongoing background trends
- Keeps the main brief focused on what's new and actionable

### Footer status line

End with a status box listing every data source checked and its result count:
- Web searches: X queries run
- Gmail: checked (X unread — Y action items)
- Google Calendar: checked (X events, 7 days)
- Outlook: checked (X inbox, Y other)
- Slack: checked (X threads)
- Etc.

This serves as a receipt so I know nothing was missed.

### Key principles

1. **Relevance over completeness** — Only include items that connect to my work, projects, or interests. Skip generic news
2. **Actionable framing** — Every item should answer "so what?" with a next step or talking point
3. **Scannability** — Use the card grid for quick scanning, full cards only for top stories
4. **Deduplication** — Don't repeat the same story across sections. If something appeared in Top Stories, don't also put it in On My Radar
5. **Honest sourcing** — The status line should accurately reflect what was checked. If a tool wasn't available, say so

### Customization

Adapt the sections to your own setup:
- Swap Gmail/Outlook for whatever email you use
- Remove Slack/Teams sections if you don't use them
- Add or remove the Substack section based on whether you have the MCP server configured
- Adjust the training/outdoor recommendations based on your fitness routine
- Change the "Why it matters to you" framing to reflect your own role and interests

---

## Tool requirements

This prompt uses the following tools. Not all are required — the brief adapts to what's available:

| Tool | Used for | Required? |
|------|----------|-----------|
| Web search | Top Stories, On My Radar, Trends, weather | Yes |
| Gmail MCP | Personal email scanning | Optional |
| Google Calendar MCP | Personal + shared calendar events | Optional |
| Outlook (via browser) | Work email scanning | Optional |
| Slack MCP | Work messaging | Optional |
| Teams (via browser) | Work messaging | Optional |
| Substack MCP | Subscription feed | Optional |
| iMessage (via extension) | Personal messaging | Optional |
