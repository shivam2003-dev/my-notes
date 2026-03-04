# AI-assisted Web Pentesting Using Claude Code + Burp MCP

This guide is a practical runbook for setting up and using Claude Code with Burp MCP for authorized web security testing.

Use this only on systems you are explicitly permitted to test.

## 1. What this setup gives you

- Query Burp traffic in natural language from Claude Code
- Generate focused security test plans for selected endpoints
- Create Repeater/Intruder workflows faster
- Summarize findings into structured markdown reports

## 2. Prerequisites

- Burp Suite with the MCP Server extension installed
- `mcp-proxy.jar` extracted from Burp MCP tab
- Claude Code CLI (`claude`)
- Java installed (`java -version`)

## 2.1 macOS installation (prerequisites + Burp MCP setup)

### A. Burp Suite (macOS)

1. Download Burp Suite for macOS from PortSwigger (Community or Professional).
2. Install it (drag to `Applications` if using `.dmg`).
3. Launch Burp and complete first-run setup.

### B. Install MCP Server extension in Burp

1. In Burp, open `Extensions` -> `BApp Store`.
2. Search for `MCP Server`.
3. Click `Install`.
4. Open the `MCP` tab and confirm server settings (SSE endpoint should be available).

### C. Extract `mcp-proxy.jar` from Burp MCP tab

1. In Burp `MCP` tab, use the export/download option for `mcp-proxy.jar`.
2. Save it to:

```bash
/Users/shivamkumar/Documents/claude/mcp-proxy.jar
```

3. Verify:

```bash
ls -l /Users/shivamkumar/Documents/claude/mcp-proxy.jar
java -jar /Users/shivamkumar/Documents/claude/mcp-proxy.jar --help
```

### D. Install Java on macOS

If Java is missing, install via Homebrew:

```bash
brew install openjdk
echo 'export PATH="$(brew --prefix openjdk)/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
java -version
```

### E. Install/verify Claude Code CLI (`claude`)

Install Claude Code CLI using the official Anthropic installer/docs, then verify:

```bash
claude --version
claude --help
```

If `claude` already exists (as in your setup), no reinstall is required.

## 3. Install and configure MCP in Claude Code

Claude Code v2.1.68 uses `~/.claude.json` for MCP registrations when added via CLI.

Run this once:

```bash
claude mcp add --scope user burp -- java -jar /Users/shivamkumar/Documents/claude/mcp-proxy.jar --sse-url http://127.0.0.1:9876
```

Verify:

```bash
claude mcp list
claude mcp get burp
```

Expected status: `✓ Connected`

## 4. Where to create `CLAUDE.md`

Create `CLAUDE.md` in the project directory where you launch `claude`.

For your Burp project:

```bash
/Users/shivamkumar/Documents/claude/CLAUDE.md
```

If you run Claude in another folder later, create another `CLAUDE.md` in that folder.

## 5. Recommended `CLAUDE.md` template

```md
# Pentest Project

## Authorization
- This testing is authorized by: <client/team name>
- Test window: <dates/times>
- Out-of-scope assets: <list>

## Target Scope
- *.example.com
- api.example.com:443

## Application Context
- App type: REST API / Web app
- Auth: JWT / session / OAuth
- User roles: Guest, User, Admin
- Tech stack: <framework/db/cache>

## Test Objectives
- Authentication and session handling
- Authorization and IDOR checks
- Input validation and injection checks
- SSRF/callback behavior (where applicable)
- Sensitive data exposure

## Rules of Engagement
- No destructive tests in production
- Rate-limit active testing
- Confirm risky actions before execution

## Output
- Save findings to `./reports/`
- Markdown format with evidence
```

## 6. Recommended workflow

1. Start Burp and ensure proxy traffic is present.
2. Start Claude Code in the project folder containing `CLAUDE.md`.
3. Confirm MCP is connected (`claude mcp list`).
4. Ask Claude to map endpoints and auth requirements from Burp history.
5. Ask Claude to generate test plans for one endpoint at a time.
6. Execute high-signal tests first; validate manually in Repeater.
7. Capture evidence and response diffs.
8. Generate a report draft and refine severity/impact.

## 7. Useful prompt pack

Use these prompts in Claude Code:

- `List high-value API endpoints from Burp history and group by auth requirement.`
- `Create a security test checklist for POST /api/login (auth bypass, injection, rate limiting).`
- `Prepare Repeater-ready requests for authorization and input-validation tests on /api/profile.`
- `Analyze Burp proxy history for JavaScript files and extract potential sensitive endpoints/config values.`
- `Summarize confirmed findings with evidence, impact, and remediation in markdown.`

## 8. Reporting format (recommended)

For each issue include:

- Title
- Severity
- Endpoint and method
- Preconditions
- Reproduction steps
- Observed vs expected behavior
- Evidence (request/response snippets, timestamps)
- Impact
- Remediation
- Retest status

## 9. Troubleshooting

### MCP not visible in Claude

Run:

```bash
claude mcp list
claude mcp get burp
```

If missing, re-add:

```bash
claude mcp add --scope user burp -- java -jar /Users/shivamkumar/Documents/claude/mcp-proxy.jar --sse-url http://127.0.0.1:9876
```

### Connected but no data

- Confirm Burp proxy is running and has captured traffic.
- Confirm Burp MCP extension is enabled.
- Confirm SSE URL matches Burp MCP config (`http://127.0.0.1:9876` in your setup).

### Debug verification

Run `claude --debug` and check log for lines similar to:

- `MCP server "burp": Successfully connected`
- `Connection established with capabilities`

## 10. Command history (reference)

```bash
ls -la ~/.claude && echo '--- settings.json ---' && cat ~/.claude/settings.json && echo '--- mcp.json ---' && cat ~/.claude/mcp.json && echo '--- debug files ---' && ls -lt ~/.claude/debug | head -n 5
tail -n 200 /Users/shivamkumar/.claude/debug/9cf58b1c-c480-48b3-a0fb-a5e000af5779.txt
rg -n "mcp|MCP|mcpServers|burp|server" /Users/shivamkumar/.claude/debug/9cf58b1c-c480-48b3-a0fb-a5e000af5779.txt | head -n 120
claude --help | sed -n '1,220p'
cat ~/.claude.json | sed -n '1,260p'
claude mcp --help && echo '---' && claude mcp list
ls -l /Users/shivamkumar/Documents/claude/mcp-proxy.jar && java -jar /Users/shivamkumar/Documents/claude/mcp-proxy.jar --help | sed -n '1,120p'
claude mcp add --help | sed -n '1,260p'
claude mcp list --help | sed -n '1,220p'
claude mcp add --scope user burp -- java -jar /Users/shivamkumar/Documents/claude/mcp-proxy.jar --sse-url http://127.0.0.1:9876 && claude mcp list && echo '--- get burp ---' && claude mcp get burp
rg -n "\"mcpServers\"|\"burp\"|\"disabledMcpjsonServers\"|\"enabledMcpjsonServers\"" ~/.claude.json | sed -n '1,120p'
sed -n '228,280p' ~/.claude.json
claude mcp list && echo '---' && claude mcp get burp
tail -n 180 /Users/shivamkumar/.claude/debug/3597bf5c-88cf-457b-9548-1f2da13c072b.txt
nl -ba /Users/shivamkumar/.claude/debug/3597bf5c-88cf-457b-9548-1f2da13c072b.txt | rg "MCP server \"burp\"|Server stderr:|connected with capabilities|\\[MCP\\] Server \"burp\" connected" | sed -n '1,120p'
nl -ba /Users/shivamkumar/.claude.json | sed -n '236,274p'
```
