# Antigravity CLI setup for the Universal Career Agent

This is the zero-additional-subscription runtime path for this repository. It uses Google Antigravity CLI with the free Individual plan; it does not use Claude Code, Anthropic API credits, or Ollama Cloud credits.

## Cost and limits

Google lists its Antigravity Individual plan at **$0/month**. The free tier has basic weekly rate limits; Google does not publish a fixed request count, and limits/model availability can change. Google's current model matrix lists Claude Sonnet 4.6, Claude Opus 4.6, and GPT-OSS-120B among the models available on Free & Plus accounts. This is a Claude model accessed through Antigravity, not Claude Code or a Claude Pro subscription. See the current [model list](https://antigravity.google/docs/models) and verify the models shown for your account with `agy models`. Stay on the Individual/free plan and do not enable paid AI credits or paid overages.

Google's current FAQ lists India among the supported geographies for personal accounts. Sign-in still depends on an eligible personal Google account and Google's age/region checks.

Official references:
- [Antigravity pricing](https://antigravity.google/pricing)
- [Antigravity plans and baseline quota](https://antigravity.google/docs/plans)
- [Antigravity CLI getting started](https://antigravity.google/docs/cli-getting-started)
- [Antigravity customization discovery](https://antigravity.google/docs/customization-discovery?tab=cli)
- [Antigravity MCP documentation](https://antigravity.google/docs/mcp)

## 1. Install on Windows

Open PowerShell and run the official Windows installer:

```powershell
irm https://antigravity.google/cli/install.ps1 | iex
```

Open a **new** PowerShell window after installation, then run:

```powershell
agy --version
```

The default Windows binary directory is:

```text
C:\Users\<username>\AppData\Local\agy\bin
```

If PowerShell says `agy` is not recognized, first open a new terminal. If it is still missing, check whether the directory above is in your user PATH; do not change the PowerShell execution policy to Unrestricted.

## 2. Sign in on the free plan

1. Launch Antigravity CLI and complete browser sign-in with your personal Google account.
2. Stay on the Individual/free plan. Do not select Google AI Pro, Ultra, a paid credit pool, or API pay-as-you-go.
3. Run `agy models` in a separate PowerShell window to see the model slugs available to your account. Start `agy`, then use `/model` to select a model from the interactive CLI. Prefer Claude Sonnet 4.6 for normal development work; use Claude Opus 4.6 for harder repository analysis when it is available in your model list. These are Anthropic Claude models offered through Antigravity, not Claude Code.
4. Run `/usage` to inspect the remaining quota. Free-tier usage has weekly rate limits; when the quota is exhausted, wait for the free refresh. Do not turn on paid AI credits or overages.
5. Keep in mind that the agent model is cloud-hosted: prompts and any resume or project content you ask it to analyse may be sent to Google's service. The repository remains on your PC, but model processing is not fully local.

## 3. Open this repository

In a new PowerShell terminal:

```powershell
cd "$HOME\Downloads\ai-job-search"
agy
```

Antigravity discovers workspace instructions from `AGENTS.md`, workspace skills in `.agents/skills/`, and MCP server definitions in `.agents/mcp_config.json`.

This repository includes:
- `.agents/skills/universal-career-agent/SKILL.md` — Universal Career Agent skill.
- `.agents/skills/*/SKILL.md` — existing portal CLI skills.
- `AGENTS.md` — shared workspace guidance.
- `.agents/mcp_config.json` — Playwright MCP setup.

The upstream Claude Code commands under `.claude/commands/` are workflow specifications, not native Antigravity slash commands. The Antigravity skill tells the agent when to use those specifications and when it must fall back to supported workspace tools.

## 4. Check the Playwright MCP server

The workspace configuration is:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest", "--extension"]
    }
  }
}
```

Install the [Playwright browser extension](https://playwright.dev/mcp/configuration/browser-extension) in the Chrome/Edge profile you intend to use. Open that browser and sign in to the career portal yourself.

Inside Antigravity CLI, run:

```text
/mcp
```

Confirm that `playwright` is connected. If it is disconnected, review the MCP status/logs, confirm Node.js 20+ and `npx` are available, then restart Antigravity after configuration changes. Do not start Claude Code and Antigravity against the same browser profile at the same time.

## 5. Read-only smoke tests

First test workspace awareness:

```text
Inspect this workspace and report:
1. The candidate-profile file and whether it contains real facts or placeholders.
2. The portal search skills that are actually present.
3. The Universal Career Agent skill and its application-approval rule.
4. Whether the Playwright MCP server is connected.
Do not edit files or open an application form.
```

Then test browser visibility:

```text
Use Playwright to inspect the currently open browser tab.
Do not navigate, click, type, submit, or change the page.
Report the title, current URL, and visible job/application sections.
```

Do not move to form filling until both read-only tests pass.

## 6. Important limitations

- The free Individual plan has weekly rate limits. Use `/usage` to check them. If you reach the limit, wait for the free quota to refresh; do not enable paid overages.
- Antigravity is a cloud-model runtime, not a fully local model. Do not send sensitive documents unless you are comfortable with Google's data handling for your account and region.
- Portal-specific adapters still require testing. The existence of Playwright does not mean Amazon Jobs, Indeed, or Workday have complete search adapters.
- CAPTCHA, MFA, bot checks, and access restrictions must be handled manually. Never bypass them.
- The agent may prepare and populate an application, but you must review it. Do not allow final submission without your explicit approval for that specific application.
