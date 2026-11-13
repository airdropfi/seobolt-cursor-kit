# SEOBolt for Cursor and Grok Build

Audit a website for Google and AI search from Cursor or Grok Build, using your own [SEOBolt](https://seobolt.app) account. This kit connects the SEOBolt MCP server and adds a rule and eight skills. SEOBolt reports and suggests; nothing on your site is edited until you have seen the diff and said yes.

[![Add SEOBolt to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/link/mcp/install?name=seobolt&config=eyJ1cmwiOiJodHRwczovL3Nlb2JvbHQuYXBwL21jcCIsImhlYWRlcnMiOnsiQXV0aG9yaXphdGlvbiI6IkJlYXJlciAke2VudjpTRU9CT0xUX0FQSV9LRVl9In19)

The button opens Cursor and asks you to confirm adding the `seobolt` MCP server. Prefer the app link? `cursor://anysphere.cursor-deeplink/mcp/install?name=seobolt&config=eyJ1cmwiOiJodHRwczovL3Nlb2JvbHQuYXBwL21jcCIsImhlYWRlcnMiOnsiQXV0aG9yaXphdGlvbiI6IkJlYXJlciAke2VudjpTRU9CT0xUX0FQSV9LRVl9In19`

## At a glance

- **What it is:** SEOBolt is SEO and AI-search audit software built for AI agents to operate: your agent crawls a site, reads the audit and the fix plan with code snippets, applies the fixes and re-crawls to verify, while you assign the work, approve changes and review the results.
- **Primary user:** An AI agent (ChatGPT, Claude, Claude Code, Cursor, Gemini CLI or your own) calling the MCP server or REST API. Humans tag the work, approve the changes and review what the agent did.
- **Who pays:** Teams and companies that own websites (agencies, SaaS and e-commerce teams, developers, freelance SEOs, founders) and individuals who ask their assistant to run an audit. The outcome you buy: your agent audits and fixes, you approve and review.
- **What it does not do:** It has no proprietary backlink or keyword index and no live rank checker; tools that need that data say so instead of guessing. It does not query ChatGPT, Claude or Perplexity in the free audit and promises no citations or rankings.
- **Price:** Free plan: $0/month (3 crawls/month, 2 projects, 100 pages per crawl). Paid plans from $19/month: Starter $19, Pro $49, Business $99, billed monthly. Pricing: https://seobolt.app/pricing
- **Integrations:** MCP server (27 tools) for ChatGPT, Claude, Claude Code, Cursor and Gemini CLI; REST API; Google Search Console (Pro and Business); IndexNow submission.
- **Human oversight:** You connect an assistant on a consent screen, see it as a named key in Settings (last used, expiry) and revoke it there at any time; write tools are flagged so clients ask before running them; crawls and fix plans stay visible in your dashboard. This applies to accounts you created yourself. Details: https://seobolt.app/mcp-seo#oversight
- **Data and security:** OAuth sign-in with PKCE on seobolt.app; the assistant never sees your password. OAuth-issued keys are stored only as a hash, expire after 90 days and can be revoked in Settings. Privacy: https://seobolt.app/privacy
- **Start here:** Run the free audit at https://seobolt.app/free-audit (no signup), or add the MCP server to your assistant (https://seobolt.app/mcp-seo) and ask it to list your SEOBolt projects.
- **MCP server:** https://seobolt.app/mcp (ChatGPT: https://seobolt.app/mcp/chatgpt). Official MCP Registry name: app.seobolt/seobolt. npm: mcp-server-seobolt.

## Install in 60 seconds

1. Create an API key in SEOBolt (Dashboard > Settings > API Keys) and export it as `SEOBOLT_API_KEY`.
2. Add to `~/.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "seobolt": {
      "url": "https://seobolt.app/mcp",
      "headers": { "Authorization": "Bearer ${env:SEOBOLT_API_KEY}" }
    }
  }
}
```
3. Settings > MCP: `seobolt` should list its tools.

## Example prompts (you tag and review, the agent works)

- *Individual:* Use SEOBolt to list my projects and tell me which site has the lowest health score. Summarize the top three issues so I can decide what to fix first.
- *Small business:* Assign to SEOBolt: crawl my site, then give me the fix plan in plain language. Show me what you did and what you found so I can approve it.
- *Agency or team:* Use SEOBolt on every project in the account: start a crawl for each, then report health scores in a table. I review before anything is changed.
- *Developer:* Get the SEOBolt fix plan for this project, apply the fixes in this repo on a branch, and show me the diff. After I deploy, re-crawl and compare the health score.

## More

- Site and MCP docs: https://seobolt.app/mcp-seo (markdown: https://seobolt.app/mcp-seo.md)
- MCP server card: https://seobolt.app/.well-known/mcp/server-card.json
- Official MCP Registry: `app.seobolt/seobolt` (https://registry.modelcontextprotocol.io/v0.1/servers?search=seobolt)
- Pricing: https://seobolt.app/pricing

## Sign-in: API key, not OAuth

SEOBolt's OAuth sign-in only accepts redirect addresses on its own allow-list (Claude, ChatGPT, Perplexity, Gemini, Copilot Studio and local loopback apps). Cursor's `cursor://` callback is not on it, so **Cursor connects with a SEOBolt API key sent as an `Authorization: Bearer` header**. Claude Code and Gemini CLI use OAuth instead; see the SEOBolt Claude and Gemini repositories.

1. In SEOBolt open Dashboard, Settings, API Keys (https://seobolt.app/dashboard/settings) and create a key. It is shown once; treat it like a password. You can revoke it there.
2. Put it in an environment variable that Cursor can see, for example `export SEOBOLT_API_KEY=...` in the shell you launch Cursor from. Never commit the key to a repository.
3. Click the button above, or add this to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

   ```json
   {
     "mcpServers": {
       "seobolt": {
         "url": "https://seobolt.app/mcp",
         "headers": {
           "Authorization": "Bearer ${env:SEOBOLT_API_KEY}"
         }
       }
     }
   }
   ```

   If you would rather not use an environment variable, replace `${env:SEOBOLT_API_KEY}` with the key itself in your own user-level file only: `"Authorization": "Bearer YOUR_API_KEY"`.
4. Open Settings, then MCP: `seobolt` should show a green dot and its tools. A 401 means the key is missing or wrong.
5. Optional, the rule: copy `rules/seobolt.mdc` to `.cursor/rules/seobolt.mdc` in your project. It is set to "Agent Decides", so Cursor applies it when you ask about SEO.
6. Optional, the skills: copy the folders in `skills/` to `~/.cursor/skills/`.

Cursor asks you to approve MCP tool calls by default. Leave that on.

## What it contains

| Component | Name | What it does |
|---|---|---|
| MCP server | `seobolt` (`https://seobolt.app/mcp`) | The 27 SEOBolt tools, authenticated with your API key. |
| Rule | `rules/seobolt.mdc` | Numbers from tool output only, approval before edits, plan limits, no result promises. |
| Skill | `seo-audit-and-fix` | Audit, read the fix plan, apply approved fixes in your repo, re-crawl and compare. |
| Skill | `seo-agent-rules` | The permission rules every other skill follows. |
| Skill | `seo-fix-existing`, `seo-weekly-review` | Search Console based fixes and a weekly review. Need Search Console connected and a Pro plan or above. |
| Skill | `seo-geo-check` | Monthly AI-citation check with `seo_ai_citations`. |
| Skill | `seo-page-brief`, `seo-ship-page` | Brief and ship one page on your site's own template, behind a checklist. |
| Skill | `seo-link-outreach` | Link outreach list and email drafts. Drafts only; nothing is sent. |

## Install in Grok Build

```bash
grok mcp add --transport http seobolt https://seobolt.app/mcp --header "Authorization: Bearer YOUR_API_KEY"
```

Grok Build also loads `.cursor/mcp.json`. We have not tested Grok Build with it. The `.mcp.json` in this repository is the same server for loaders that read that file.

## What it will and will not do

- The audit, fix-plan, keyword and Search Console tools are read-only. `seo_start_crawl` starts a real crawl that counts against your plan's crawl allowance (its description says Free is 3 crawls a month). `seo_set_competitors` replaces a project's competitor list.
- Brand Radar tools need Starter or above; AI citation checks and the Search Console tools need Pro or above. A lower plan gets a plan-required answer. The MCP server itself works on every plan, including Free.
- Every number comes from tool output. Suggestions are not changes: edits to existing pages need your approval, and the rule and skills never delete pages, edit robots.txt, noindex or canonical tags, send email or buy anything.
- `seo_get_checkout_url` only returns a plan-upgrade link for you to open yourself.
- No ranking or traffic result is promised.

Try the prompts in [examples/prompts.md](examples/prompts.md).

## Data and privacy

The MCP server receives what the tools need: project ids, URLs, keywords and settings you ask your agent to send, plus the audit and Search Console data on your SEOBolt account. This repository stores nothing and runs no code on your machine; it is a server address, Markdown instructions and a logo. See the [privacy policy](https://seobolt.app/privacy) and [terms](https://seobolt.app/terms).

## Layout

```text
.cursor-plugin/plugin.json   Cursor Plugin manifest (also read by Grok Build)
mcp.json                     MCP server for Cursor (API key from SEOBOLT_API_KEY)
.mcp.json                    the same server for other loaders
rules/seobolt.mdc            rule
skills/                      eight skills (SKILL.md)
assets/logo.png              256x256 logo
examples/prompts.md          example prompts
```

## Support

https://seobolt.app/support or support@seobolt.app. Documentation: https://seobolt.app/mcp-seo.

## License

MIT for the files in this repository (see `LICENSE`). The SEOBolt service is governed by its terms.
