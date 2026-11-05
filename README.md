# HeyAmos plugin for Claude

[HeyAmos](https://heyamos.com) tracks how your brand shows up in AI answers — ChatGPT, Gemini, Claude, Perplexity, DeepSeek and Copilot. This plugin connects Claude to your HeyAmos workspace and adds skills that turn that data into plain-language insights and next steps: visibility, ranking, sentiment, the sources AI cites, competitor gaps, and what to publish next.

## What's included

### MCP connector

| Name | URL | Auth |
| --- | --- | --- |
| `heyamos` | `https://engine.heyamos.com/api/v1/mcp-with-oauth` | OAuth (sign in with your HeyAmos account) |

When you connect, you sign in to HeyAmos and choose a workspace. The connection only sees that workspace.

### Skills

| Skill | Use it to |
| --- | --- |
| `geo-analyst` | Answer any question from your HeyAmos data, or build a client report: visibility, ranking, sentiment, citations, competitor gaps, "what moved the needle", recommendations. Includes the HeyAmos terminology, writing style and FAQ (benchmarks, KPIs, methodology). |
| `weekly-content-learnings` | Each week, check whether earlier suggested actions went live and helped, compare new pages AI picked up with those it didn't, decide which prompts to keep or swap, and update a `content-writer-guide.md` for your writing agent. |
| `brand-accuracy-check` | Fact-check what AI says about your brand against your site, find what drives negative sentiment, see how AI positions you against competitors, and trace each problem to the pages behind it. |
| `prompt-set-builder` | Plan a set of decision-stage prompts to track (starter, solid or deep coverage), organized by topic, and add them to your workspace once you approve. |

## Getting started

1. Add the plugin from the Claude directory (in Claude, open **Customize → Plugins**), or in Claude Code run `/plugin` and pick HeyAmos.
2. Connect the **HeyAmos** connector and sign in with your HeyAmos account. In Claude Code, run `/mcp` and authenticate `heyamos`.
3. Ask a question, for example:
   - "How visible is my brand in AI answers this week, and what should I do next?"
   - "Run the weekly content learnings."
   - "Does AI get our pricing right?"
   - "Help me build 50 prompts to track."

You need a HeyAmos account with at least one workspace. Some tools (Google Analytics 4, Search Console, Bing Webmaster, Google Ads, ChatGPT Ads, bot traffic) only appear when that integration is connected in your HeyAmos workspace settings.

## What it reads, changes and sends

- **Reads** your HeyAmos workspace data through the HeyAmos MCP server: brand profile, prompts and their metrics, AI answer extracts, citations, insights, suggested actions, alerts, and connected analytics integrations.
- **Changes** data only through the prompt tools (add or delete prompts, manage prompt topics). The skills always show you the exact changes and ask before calling them. Every other tool is read-only.
- **Sends** tool requests only to `engine.heyamos.com`, authenticated with your HeyAmos OAuth sign-in. The skills may also ask Claude to open your website, competitor pages and other public web pages to check facts and page formats, using Claude's own web tools.
- The plugin itself contains only Markdown instructions, configuration and an icon image. It runs no local code, scripts or hooks, and stores nothing.

Data handling by HeyAmos is covered by the [HeyAmos privacy policy](https://heyamos.com/privacy-policy).

## Support

Questions or problems: contact the HeyAmos team through [heyamos.com](https://heyamos.com).

## License

[MIT](LICENSE)
