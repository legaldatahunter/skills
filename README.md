# Legal Data Hunter skills

Open-source legal research skills for AI agents that use the [Legal Data Hunter](https://legaldatahunter.com) MCP. The MCP searches official law from 230+ jurisdictions and 38M+ documents (legislation, case law, official doctrine), and every result links to the official text. Citation resolution covers 55+ countries. The skills teach the agent how to research, cite and structure legal work with it.

## Skills

| Skill | Use it for | Example |
|---|---|---|
| `legal-research` | General legal research: statutes, case law, official doctrine, single or multi-jurisdiction questions, memo-style answers | `GDPR Article 46 SCC transfers and the Schrems II line of cases` |
| `cross-border-comparison` | Side-by-side comparison of how several jurisdictions treat the same question | `France, Germany and the EU on the GDPR one-stop-shop` |
| `resolve-citation` | Turning one citation or informal reference (ECLI, CELEX, article, docket number) into the exact document and a verified URL | `Article 1240 of the French Code civil` |
| `legal-citations` | Citation discipline: verified inline links, no invented URLs, Sources section | `How should I cite CJEU case law in this memo?` |
| `legal-risk-assessment` | Severity x likelihood scoring per jurisdiction and escalation to outside counsel | `Score our cross-border data-transfer exposure` |
| `multi-issue-memo` | Long-form memo that resolves each enumerated sub-issue with its own statute and leading case | `(1) liability of the building's keeper, (2) fault of the victim` |
| `install-legal-data-hunter` | Installing and verifying the MCP server from inside the agent | `Install Legal Data Hunter` |

Skills load on demand, so you can also just describe the task. As slash commands, Claude Code plugin skills are namespaced: `/legal-data-hunter:legal-research`. In Codex, use `$legal-research`.

## Connect the MCP first

The skills need the Legal Data Hunter MCP:

```
https://legaldatahunter.com/mcp
```

Setup instructions for each client are at https://legaldatahunter.com/connect. On first use you sign in with GitHub or Google in the browser (OAuth); there is no API key. Every account has a free daily allowance, and paid plans are listed at https://legaldatahunter.com/pricing.

## Install

### Claude Code

Plugin (skills and MCP together):

```bash
claude plugin install legal-data-hunter --marketplace legaldatahunter/skills
```

Or in two steps, from a session (`/plugin marketplace add legaldatahunter/skills`, then `/plugin install legal-data-hunter@legaldatahunter`) or the shell:

```bash
claude plugin marketplace add legaldatahunter/skills
claude plugin install legal-data-hunter@legaldatahunter
```

MCP only:

```bash
claude mcp add --transport http legal-data-hunter https://legaldatahunter.com/mcp
```

### Claude.ai, Claude desktop and Cowork

Open Customize > Plugins > Add > Add marketplace, enter `legaldatahunter/skills`, then add the Legal Data Hunter plugin. Plugins saved to your account are available in chat, Cowork and Claude Code. To add only the connector, use the [Claude directory listing](https://claude.ai/directory/a38cc6ae-c7fb-4136-b50e-cf49a4ded4b3).

### Codex

```bash
codex mcp add legal-data-hunter --url https://legaldatahunter.com/mcp
codex mcp login legal-data-hunter
npx skills add legaldatahunter/skills -a codex
```

Use `-g` to install the skills for your user instead of the current repo. Invoke a skill with `$legal-research` or `/skills`. The MCP config is shared by the Codex CLI, the IDE extension and the ChatGPT desktop app.

### ChatGPT

Use the official [Legal Data Hunter app](https://chatgpt.com/plugins/plugin_asdk_app_6a09fca2104c8191b160df27196228dd) and sign in with OAuth when prompted. Standalone skills are available in the ChatGPT desktop app, which shares its skills and MCP config with Codex; follow the Codex steps above.

### Cursor

```bash
npx skills add legaldatahunter/skills -a cursor
npx legal-data-hunter setup --yes
```

The first command installs the skills (add `-g` for `~/.cursor/skills`). The second detects Cursor and writes the MCP config. To do it by hand, add this to `.cursor/mcp.json` or `~/.cursor/mcp.json`:

```json
{"mcpServers": {"legal-data-hunter": {"url": "https://legaldatahunter.com/mcp"}}}
```

### Google Antigravity

The easiest route is the official plugin, which bundles the MCP and these skills: install it from the Antigravity marketplace once listed, or run `agy plugin install https://github.com/legaldatahunter/antigravity-plugin`.

For skills only: `npx skills add legaldatahunter/skills -a antigravity`. To add the MCP by hand, put this in `mcp_config.json`:

```json
{"mcpServers": {"legal-data-hunter": {"serverUrl": "https://legaldatahunter.com/mcp"}}}
```

### Any agent

```bash
npx skills add legaldatahunter/skills
```

This is interactive: pick the skills and agents. Useful flags: `-a <agent>`, `-g` (global), `--skill <name>`, `--list`, `-y`. It requires Node.js 22.20+ and installs skills only, so connect the MCP separately (see above). With the GitHub CLI (2.90+):

```bash
gh skill install legaldatahunter/skills --all --agent <agent>
```

## Repo layout

```
.claude-plugin/
  plugin.json          Claude Code plugin manifest
  marketplace.json     Claude Code marketplace catalog
.mcp.json              MCP server config bundled with the plugin
skills/<name>/
  SKILL.md             The skill (Agent Skills format)
  agents/openai.yaml   Codex metadata and MCP dependency
  references/          Extra material loaded on demand (legal-research)
assets/logo.svg
```

## Disclaimer

Legal Data Hunter assists with legal workflows but does not provide legal advice. Have qualified professionals review the output.

## License

[MIT](LICENSE)

## Links

- Product: https://legaldatahunter.com
- Connect guide: https://legaldatahunter.com/connect
- Docs: https://legaldatahunter.com/docs
- Pricing: https://legaldatahunter.com/pricing
- Support: https://legaldatahunter.com/support or zach@legaldatahunter.com
