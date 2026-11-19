---
name: install-legal-data-hunter
description: Install and set up the Legal Data Hunter MCP server. Use when the user wants to connect their AI agent to legal data, install legal-data-hunter, add a legal search MCP, or search legal documents across jurisdictions.
allowed-tools: Bash Read Write
---

# Install Legal Data Hunter

Install the Legal Data Hunter MCP server and verify the connection. Legal Data Hunter provides access to 38M+ legal documents across 230+ jurisdictions (case law, legislation, and official doctrine) via hybrid semantic + keyword search. The endpoint is `https://legaldatahunter.com/mcp` (Streamable HTTP).

## Step 1: Detect environment and install

Pick the command for the agent you are running in.

**Claude Code**:

```bash
claude mcp add --transport http legal-data-hunter https://legaldatahunter.com/mcp
```

**Codex** (CLI, IDE extension, or ChatGPT desktop app, which share the same config):

```bash
codex mcp add legal-data-hunter --url https://legaldatahunter.com/mcp
codex mcp login legal-data-hunter
```

**Cursor, VS Code, Windsurf, JetBrains, Copilot, and other clients**:

```bash
npx legal-data-hunter setup --yes
```

This auto-detects the installed clients and writes the MCP config to the right place. For Cursor you can also add this to `.cursor/mcp.json` (or `~/.cursor/mcp.json`):

```json
{"mcpServers": {"legal-data-hunter": {"url": "https://legaldatahunter.com/mcp"}}}
```

For other clients, see https://legaldatahunter.com/connect.

## Step 2: Sign in

On first use, the client opens a browser window for OAuth sign-in with GitHub or Google. No API key is needed. Every account has a free daily allowance; if the user needs more, point them to https://legaldatahunter.com/pricing.

## Step 3: Verify installation

Confirm the MCP server is connected: reconnect MCP servers in the client and check that `Legal Data Hunter` appears with its tools. The server exposes 7 tools:

| Tool | Purpose |
|------|---------|
| `discover_countries()` | List all 230+ indexed jurisdictions with document counts |
| `discover_sources(country)` | List available sources for a country (e.g. `"FR"`, `"DE"`, `"EU"`) |
| `search(query, ...)` | Hybrid semantic + keyword search across jurisdictions |
| `resolve_reference(citation)` | Look up a specific citation: ECLI, CELEX, article number, case number |
| `get_document(id)` | Retrieve the full text of a document by ID |
| `get_filters()` | List available filter options (courts, document types, date ranges) |
| `report_source_issue(...)` | Report a problem with a data source or request a new one |

## Step 4: Getting started

Once connected, here are useful first queries to try:

**Explore coverage:**
- Call `discover_countries()` to see all indexed jurisdictions and their document counts
- Call `discover_sources("FR")` to see French court databases, legislation sources, and doctrine

**Search across jurisdictions:**
- `search("unfair dismissal", country="DE", doc_type="case_law")` for German case law on unfair dismissal
- `search("artificial intelligence regulation", country="EU")` for EU legislation on AI
- `search("data protection breach notification", country="UK")` for UK GDPR enforcement

**Resolve specific citations:**
- `resolve_reference("ECLI:EU:C:2014:317")` to look up a CJEU decision by ECLI
- `resolve_reference("32016R0679")` to look up the GDPR by CELEX number
- `resolve_reference("Article 49 TFEU")` to find Treaty provisions

**Retrieve full documents:**
- Use `get_document(id)` with an ID returned from search or resolve_reference to get the complete text

## Tips

- **Natural language works**: The search engine understands legal concepts, not just keywords. "Can an employer fire someone for social media posts?" works as well as "dismissal social media misconduct".
- **Cross-reference**: Search one jurisdiction, then use the same query in another to compare legal approaches across borders.
- **Citation chains**: Use `resolve_reference()` to find a law, then `search()` with that law's name to find all court decisions that cite it.
- **Filters narrow results**: Call `get_filters()` first to see what filter options are available, then pass them to `search()`.
- **Missing a source?** Use `report_source_issue()` to request a new legal source or report a problem.

## Links

- Product: https://legaldatahunter.com
- Connect guide: https://legaldatahunter.com/connect
- Docs: https://legaldatahunter.com/docs
- Pricing: https://legaldatahunter.com/pricing
- Skills on GitHub: https://github.com/legaldatahunter/skills
- MCP endpoint: https://legaldatahunter.com/mcp
