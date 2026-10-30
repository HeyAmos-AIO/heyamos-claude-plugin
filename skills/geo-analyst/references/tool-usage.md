# HeyAmos MCP tool usage notes

How to format inputs for the `heyamos` MCP tools. For the server's own, always-current playbook, call `how_to_use_heyamos_and_tools`.

## Workspace

The connection is scoped to the workspace the user signed in to. `workspaceSlug` is injected by the server and is not part of any tool's input. Never ask the user for it.

## Providers (`llm`)

- Use only the `llm` (provider) parameter. Do not use the `model` filter.
- In HeyAmos settings, "models tracked" means the LLM providers tracked.
- Call `get_brand_profile` first and read "Tracked Providers (Models)" before filtering.
- Use the exact technical IDs, not display names:
  - `openai` (ChatGPT)
  - `anthropic` (Claude)
  - `google` (Gemini)
  - `perplexity`
  - `deepseek`
  - `copilot`
- When writing for the user, use the display names (ChatGPT, Claude, Gemini...).

## Geography and dates

- `country`: two-letter ISO 3166-1 alpha-2 codes, such as `US`, `GB`, `CA`, `AU`, `IN`. Never the full country name.
- `startDate`, `endDate`: `YYYY-MM-DD`, such as `2026-08-01`. Never relative terms like "last week". Work out the actual dates first.

## Enumerations

- `sentiment`: `positive`, `negative`, `neutral`, `mixed`.
- `priority`: `low`, `medium`, `high`. Use `high` for primary brand and transactional terms.
- `frequency`: `hourly`, `daily`, `weekly`, `monthly`. Default to `weekly` or `daily`. `hourly` uses a lot of credits; avoid it unless the user insists.

## Google Analytics 4

- `organicOnly`: `"true"` limits traffic to the Organic Search channel (SEO/AEO traffic). Recommended when analyzing search impact.
- `customOnly`: defaults to `"true"`, which hides automatic GA4 events (`page_view`, `session_start`, `user_engagement`) and keeps client-defined conversions (`generate_lead`, `purchase`). Set `"false"` only when you need every event.
- `eventName`: case-insensitive substring filter. `"demo"` matches `request_demo` and `demo_signup`.

## Google Ads

- `get_gads_*` tools appear only when Google Ads is connected for the workspace. They are analytics only (overview, campaigns, keywords, search terms, device, trends). Never try to create, edit or pause ads.
- `customerId` is ignored; the workspace's default account is used.
- Dates default to roughly the last 28 days.
- If the tools are missing, tell the user to connect Google Ads in Workspace Settings. Never invent paid-search numbers.

## Google Search Console and Bing Webmaster

- `siteUrl`, `propertyId` and `customerId` may appear in schemas for legacy reasons but are ignored. The workspace's default connected account is used. Omit them.
- Integration tools (GA4, GSC, Bing, Google Ads, ChatGPT Ads, bot traffic) may be missing when the integration isn't connected. Check the matching `get_*_status` tool and report "connected" or "not connected".

## Pagination

- Tools that take `page` and `limit` default to `page` 1 and `limit` 20 (or 50 for larger sets).
- If results show more data is available, increment `page` until you have what you need.

## Writes

Only these tools change data: `add_prompt`, `bulk_add_prompt`, `delete_prompt`, `add_prompt_topic`, `update_prompt_topic`, `delete_prompt_topic`. Show the user exactly what will change and get a clear yes first. `bulk_add_prompt` takes `prompts`, an array of objects with at least `text`, `llmProviders` and `frequency`; duplicates by text are skipped.

## `check_external_web_page_mention`

It analyzes one external page live and can be slow. If you use it, make it the last call in a run. If it fails, read the page on the web instead.
