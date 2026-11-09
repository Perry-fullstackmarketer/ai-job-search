# Universal Career Agent — setup and verification

This adds a browser-control layer to the existing AI Job Search workspace. It does not replace the existing `/setup`, `/scrape`, `/rank`, `/apply`, `/interview`, `/upskill`, or `/outcome` workflows.

## What is being connected

- **Career intelligence:** existing AI Job Search workflows and candidate profile.
- **Browser execution:** Microsoft Playwright MCP.
- **Portal coverage goal:** Amazon Jobs, LinkedIn Jobs, Indeed, Workday-hosted career sites, and other employer career portals.
- **Human approval:** required before a final application submission.

Portal compatibility depends on each site's current UI, authentication, and automation policies. This setup is not a promise that every portal will work without intervention.

## Important: marketplace vs MCP server

The GitHub repositories shown in the setup screenshots are MCP servers or application projects, not necessarily Claude plugin marketplaces. Claude's marketplace installer expects a repository containing a valid `.claude-plugin/marketplace.json`. Do not add `microsoft/playwright-mcp` or unrelated application repositories as marketplaces.

This repository already contains its own marketplace manifest. Playwright is connected separately as an MCP server through the root `.mcp.json`.

## Requirements

- Node.js 20 or newer.
- Claude Code installed and authenticated.
- Microsoft Edge or Google Chrome.
- Playwright MCP browser extension installed in the browser profile you intend to use.
- This repository cloned locally.

Official references:
- Playwright MCP: https://github.com/microsoft/playwright-mcp
- Playwright browser extension: https://playwright.dev/mcp/configuration/browser-extension
- Playwright MCP setup: https://playwright.dev/docs/getting-started-mcp

## Setup on Windows

1. Open PowerShell in the local repository folder.
2. Confirm prerequisites:

   ```powershell
   node --version
   npx --version
   claude --version
   ```

   Use Node.js 20 or newer.

3. Install the Playwright MCP browser extension in the Edge or Chrome profile where you are already signed in to the job portals. Use the official Playwright documentation above to locate the extension.
4. Confirm the repository-root `.mcp.json` exists. It configures:

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

5. Close and reopen Claude Code from this repository so it loads the project MCP configuration. Approve the Playwright MCP server when Claude Code asks.
6. Open Edge/Chrome and the target career portal yourself. Sign in normally and complete any MFA/SSO prompts yourself.
7. In Claude Code, ask:

   ```text
   Use Playwright to inspect the currently open browser tab. Do not click Apply or Submit.
   Tell me the page title, current URL, and the visible job/application sections.
   ```

8. Confirm that Claude can inspect the intended tab. Then test a harmless navigation to a public job listing before using a live application form.

## Smoke test checklist

- [ ] `node --version` reports v20 or newer.
- [ ] `npx --version` succeeds.
- [ ] Playwright MCP appears in Claude Code's connected MCP servers.
- [ ] The browser extension is installed and connected.
- [ ] Claude can read the title and URL of the current tab.
- [ ] Claude can inspect a public job listing without changing account data.
- [ ] Claude can read a form field label without entering or submitting data.
- [ ] The existing `/apply` workflow still evaluates fit and prepares documents.
- [ ] A final application submission is blocked until the user explicitly approves that specific application.

## Using it

From Claude Code in this repository, invoke the `universal-career-agent` skill or ask naturally, for example:

- "Find marketing and business development roles in India on Amazon Jobs and LinkedIn."
- "Inspect this Workday job posting and evaluate it against my profile."
- "Prepare the application for this role, fill the non-sensitive fields, and stop before final submission."

The agent should continue using the repository's existing fit evaluation, CV tailoring, cover-letter generation, interview preparation, and application tracking rules.

## Using the same browser from multiple AI clients

A browser profile should not be controlled by two automation clients at the same time. Do not run Claude Code and another Playwright MCP client against the same profile concurrently.

Claude Code can use this local stdio MCP configuration. ChatGPT custom MCP connections are a separate capability: ChatGPT connects to a remote HTTPS MCP endpoint or a supported Secure MCP Tunnel, not directly to a local stdio process. Availability depends on the ChatGPT plan and workspace permissions. On plans without custom MCP access, use ChatGPT for analysis of exported job descriptions/application data, while Claude Code controls the local authenticated browser.

Do not publish a local MCP server or expose browser-control endpoints to the public internet without authentication, authorization, and a reviewed security design.

## Privacy

- Never commit passwords, OTPs, cookies, browser profiles, or session exports.
- Do not place personal candidate documents in a public repository.
- Keep the local candidate profile and application records in a private working copy.
- Use normal sign-in, SSO, and MFA flows; pause for manual verification when required.
- Review every application before submission.
