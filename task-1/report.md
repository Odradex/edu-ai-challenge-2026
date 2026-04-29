# Task 1 — Leaderboard Replication: Report

## Approach

I used GitHub Copilot (Claude Sonnet 4.6) inside VS Code to build the leaderboard entirely through prompting — no manual HTML, CSS, or JavaScript written by hand.

### Starting point

The original leaderboard is a SharePoint-hosted web part. I had no access to the source code, so I used the browser's **Save Page As** (complete page) to capture the fully rendered HTML. This saved not just the markup but also all inlined `<style>` blocks injected by the web part at runtime — which turned out to be the most valuable part, since they contained every CSS rule for the leaderboard component with actual values (colours, spacing, font sizes, gradients). I then extracted two focused snippets from that file: the main list markup (`content.html`) and an expanded row (`expanded.html`). Together these gave me the full component structure, all CSS class names with their styles, icon names, and the data shape.

Before writing any code I asked Copilot to analyse the snippets and tell me what was missing. It identified the need for screenshots, the dropdown option values, and whether filters re-rank or just hide rows. I provided a screenshot of the page and answered those questions.

### What went wrong first

My initial prompt was too open-ended — I asked Copilot to "replicate the leaderboard". It immediately tried to generate the entire application in one shot: a 400-line HTML file with inline styles, hardcoded data, and broken filter logic. It was unstructured and hard to iterate on.

I had to stop it and explicitly say: **start small — empty page and data.json first**. That reset the approach entirely.

### Step-by-step build

Once I forced a smaller scope, progress was steady:

1. **`data.json`** — fake users with activities, categories, dates, and points. No real names, photos, or org data from the original.
2. **Podium (top 3)** — HTML + CSS for the 2-1-3 visual layout, rank badges, score pills.
3. **List rows** — expandable cards with category icons, TOTAL score, and a RECENT ACTIVITY table inside.
4. **Header + filter bar** — title, subtitle, three dropdowns (Year, Quarter, Category) and a search input.
5. **Filtering logic** — year and quarter derived from activity dates at runtime; all four controls re-sort and re-rank the entire list live.
6. **Styling polish** — hover shadow on cards, grey outer background to match the embedded look, podium block height fixes.

Each step was a single focused prompt. Copilot handled the implementation; I reviewed the output in the browser and gave the next instruction.

### Data replacement

The original leaderboard contains real employee names, profile photos, and internal org codes. I replaced all of it with:

- **Names**: internationally diverse fictional names (no real people).
- **Roles and org codes**: plausible but entirely made-up titles and codes following the same `XX.UX.DX.GX` format.
- **Avatars**: initials-based coloured circles — no photos at all.
- **Activity names**: generic `[EDU]`-prefixed titles that match the style of the originals without reusing any real event names.
- **Scores and dates**: spread across 2024–2026 and all four quarters to make the filters useful.

The category taxonomy (Public Speaking, Education, University Partnership) was kept because it's structural, not personally identifying.

### Tools used

- **GitHub Copilot Chat** (Claude Sonnet 4.6) in VS Code — all code generation and iteration
- **Saved page HTML** as the reference for structure and CSS
- **Python `http.server`** for local preview during development
- **GitHub Pages** for deployment
