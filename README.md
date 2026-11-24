# Legal Data Hunter skills

Open-source legal research skills for AI agents that use the [Legal Data Hunter](https://legaldatahunter.com) MCP. The MCP searches official law from 230+ jurisdictions and 86M+ documents (legislation, case law, official doctrine), and every result links to the official text. Citation resolution covers 55+ countries. The skills teach the agent how to research, cite and structure legal work with it.

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

Skills load on demand, so you can also just describe the task. To run one explicitly, use `/legal-research` in Claude Code or `$legal-research` in Codex.

## Connect the MCP first

The skills need the Legal Data Hunter MCP:

```
https://legaldatahunter.com/mcp
```

Setup instructions for each client are at https://legaldatahunter.com/connect. On first use you sign in with GitHub or Google in the browser (OAuth); there is no API key. Every account has a free daily allowance, and paid plans are listed at https://legaldatahunter.com/pricing.

## Install

### Claude Code

Add the MCP:

```bash
claude mcp add --transport http legal-data-hunter https://legaldatahunter.com/mcp
```

Add the skills:

```bash
npx skills add legaldatahunter/skills -a claude-code
```

Add `-g` to install them for all your projects instead of the current repo.

To install by hand, clone the repo and copy the skill folders. For your personal skills (all projects):

```bash
git clone https://github.com/legaldatahunter/skills.git
mkdir -p ~/.claude/skills
cp -r skills/skills/* ~/.claude/skills/
```

For one project only, copy them into that project's `.claude/skills/` instead, run from the project root with the path to wherever you cloned the repo (`mkdir -p .claude/skills && cp -r /path/to/skills/skills/* .claude/skills/`), and commit that folder to share the skills with your team. Each skill ends up at `~/.claude/skills/<skill-name>/SKILL.md` (or `.claude/skills/<skill-name>/SKILL.md`).

### Claude.ai, Claude desktop and Cowork

Connector: use the [Claude directory listing](https://claude.ai/directory/a38cc6ae-c7fb-4136-b50e-cf49a4ded4b3), or add it as a custom connector. Open Customize > Connectors > Add > Add custom connector, enter the name `Legal Data Hunter` and the URL `https://legaldatahunter.com/mcp`, click Continue, then sign in. On Team and Enterprise plans, an Owner adds the connector under Organization settings > Connectors, then members connect it under Customize > Connectors.

Skills: zip each skill folder so that the folder itself is at the top level of the zip. For example, from where you cloned the repo:

```bash
cd skills/skills
zip -r legal-research.zip legal-research/
```

Upload the zip in Customize > Skills and turn the skill on. Skills enabled on your claude.ai account are also used in Cowork. Personal skills in `~/.claude/skills` are not loaded in Cowork, so upload them there too.

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
skills/<name>/
  SKILL.md             The skill (Agent Skills format)
  agents/openai.yaml   Codex metadata and MCP dependency
  references/          Extra material loaded on demand (legal-research)
LICENSE                MIT license
README.md
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
