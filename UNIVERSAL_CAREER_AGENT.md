# Universal Career Agent — setup and verification

This repository adds a supervised browser-control layer to the existing AI Job Search workflow. It does not replace the existing `/setup`, `/scrape`, `/rank`, `/apply`, `/interview`, `/upskill`, or `/outcome` specifications.

## Supported runtimes

### Recommended: Google Antigravity CLI (free individual plan)

For users who do not have paid Claude Code access, use the Google Antigravity CLI. Its Individual plan is listed at $0/month and includes agent models with basic weekly rate limits. Google does not publish a fixed numeric quota for that baseline tier, and limits can change. Do not enable paid AI credits or billing if you intend to remain on the free plan.

See [ANTIGRAVITY_SETUP.md](ANTIGRAVITY_SETUP.md) for Windows installation, sign-in, workspace setup, and smoke tests.

### Claude Code

Claude Code remains supported for users who already have an eligible paid Claude plan or Anthropic API credits. A free Claude chat account alone does not provide the paid Claude Code access required by the upstream project. Do not purchase a plan just for this repository unless you decide the cost is worthwhile.

## What is being connected

- **Career intelligence:** existing AI Job Search profile, portal-search skills, evaluation rules, CV/cover-letter workflows, interview preparation, and tracker.
- **Browser execution:** Microsoft Playwright MCP connected to an existing authenticated browser session.
- **Portal coverage goal:** Amazon Jobs, LinkedIn Jobs, Indeed, Workday-hosted career sites, and other employer career portals.
- **Human approval:** required before any final application submission.

The portal list is a compatibility goal, not a claim that each portal has a dedicated working adapter. Existing search adapters must be inspected and tested individually. Browser access to a site is not equivalent to a tested search integration.

## Configuration locations

The same Playwright server definition is intentionally stored in three runtime-specific locations:

| Runtime | Configuration |
|---|---|
| Claude Code opened from this repository | Root `.mcp.json` |
| Claude Code marketplace plugin installation | `plugins/ai-job-search/.mcp.json` |
| Google Antigravity CLI workspace | `.agents/mcp_config.json` |

Keep the three definitions aligned when the Playwright server command changes. Do not run multiple automation clients against the same browser profile at the same time.

Current configuration:

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

## Claude marketplace vs MCP server

The repository contains its own Claude marketplace manifest at `.claude-plugin/marketplace.json`. The `ai-job-search` plugin is in `plugins/ai-job-search/`; its `.claude-plugin/plugin.json`, `skills/`, and `.mcp.json` are packaged together.

An MCP server repository is not automatically a Claude plugin marketplace. Do not add `microsoft/playwright-mcp` or unrelated application repositories as marketplace sources.

## Browser setup

1. Install the Playwright MCP browser extension in the Chrome or Edge profile you intend to use. Use the official [Playwright browser-extension guide](https://playwright.dev/mcp/configuration/browser-extension).
2. Open the browser yourself and sign in to the target career portal normally.
3. Complete SSO and MFA yourself. Never share credentials or session cookies with the agent.
4. Start the chosen runtime from the repository root. Approve the Playwright MCP server when prompted.
5. Confirm the server is connected before asking it to inspect a page.

## Read-only smoke test

Use this prompt in the chosen agent:

```text
Use the Playwright MCP server to inspect the currently open browser tab.
Do not navigate, click, type, submit, or change the page.
Report the page title, current URL, and visible job/application sections.
```

Then confirm the agent can read a public job listing and identify a form field label without changing account or application data.

## Smoke-test checklist

- [ ] The chosen runtime launches from this repository.
- [ ] Node.js 20+ and `npx` are available.
- [ ] The runtime discovers the `universal-career-agent` skill.
- [ ] Playwright MCP appears in the runtime's connected MCP servers.
- [ ] The browser extension is installed and connected to the intended Chrome/Edge profile.
- [ ] The agent reads the active tab title and URL without changing page state.
- [ ] The agent can inspect a public job listing.
- [ ] The agent can read form labels without entering or submitting data.
- [ ] The existing fit-evaluation and application-preparation workflow remains available in the selected runtime.
- [ ] The agent stops for explicit approval before each final application submission.

## Portal coverage

Existing adapters live under `.agents/skills/`. Before claiming support for a portal, inspect its skill, run its documented search, verify the results are current, and confirm its location/keyword filters. Amazon Jobs, Indeed, and Workday still require separate live validation before they can be described as supported search adapters.

If a portal cannot be controlled reliably, fall back to a user-provided job URL or pasted job description and continue with the offline fit evaluation and application-preparation workflow.

## Privacy and security

- Do not put personal candidate documents in this public repository. Keep the populated `CLAUDE.md`, CV, documents, and application history in a private working copy.
- Never commit passwords, OTPs, cookies, browser profiles, tokens, or session exports.
- Do not expose a local MCP server or browser-control endpoint to the public internet.
- Follow site terms, access policies, rate limits, and normal sign-in flows.
- Stop for CAPTCHA, MFA, bot checks, or other access challenges; do not attempt to bypass them.
- Review every application before submission. Final submission always requires explicit approval for that specific application.
