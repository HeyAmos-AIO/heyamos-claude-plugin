---
name: prompt-set-builder
description: Plan and add the right prompts to track in HeyAmos for a brand. Use when the user wants to generate, choose, expand or refresh the prompts (questions) HeyAmos tracks across AI tools, asks which prompts to add, or wants to build a starter, solid or deep prompt set by topic.
---

# Build a HeyAmos prompt set

Help the user choose decision-stage prompts to track, then add them to their workspace once they approve. Follow the `geo-analyst` skill's `references/tool-usage.md` for parameter formats.

## Step 1: Ask first

Call `get_brand_profile` so you know the brand, its tracked competitors, markets and AI tools. Then ask, and wait for the answers before going further:

1. What are the key topics or service areas most important to your business? (e.g. "corporate events", "wealth management for expats", "Shopify migration")
2. What's your main focus?
   - Competitor comparisons — how you stack up against rivals
   - Category discovery — showing up when people ask about your category or a problem you solve
   - Brand narrative — how AI describes and positions your brand
   - All of the above
3. Are you local/regional or global? If local, which cities or regions?
4. Which competitors matter most? (Confirm or add to those in the brand profile.)
5. How many prompts do you want?
   - ~25 (starter set)
   - ~50 (solid coverage)
   - ~100+ (deep coverage)
6. Any extra context — brand guide, messaging doc, ideal customer description? Paste or upload it.

## Step 2: Research

- Research the brand's website beyond the homepage: services, about, case studies, "who we serve". Understand what it does, who it serves, and where.
- Do the same for the main competitors.
- Read what HeyAmos already has:
  - `get_all_prompts_with_metrics` — the current prompts, so you don't duplicate them.
  - `get_high_volume_missing_prompts` and `get_prompt_suggestions` — gaps HeyAmos has found.
  - `get_ai_search_search_terms`, `get_ai_search_discovery_search_terms` and `ai_search_topic_clusters` — real demand signals for the category.
  - `convert_keywords_to_prompts` — turn key topics into natural prompts.

## Step 3: Generate prompts

Organize by topic to hit the target count. For each topic:

- Topic name and why it matters for the business (one line).
- Prompts: a mix of search-style ("best X for Y") and conversational ("I'm looking for...").
- For each prompt: a volume signal and a priority (High / Medium / Low). Take volume from HeyAmos search-term data where it exists; otherwise label it as your estimate.

Weight toward the focus from question 2:

- Competitor comparisons → more head-to-head and alternatives prompts.
- Category discovery → more "best [category] for [use case]" prompts.
- Brand narrative → more review, reputation and perception prompts.

## Step 4: Summary

1. Total prompts, broken down by topic.
2. The top 5 highest-volume prompts to prioritize.
3. Topics where volume is low but strategic value is high.

## Step 5: Approve, then add

1. Ask for approval or tweaks. Revise until the user approves the set.
2. Confirm the settings before adding: which AI tools (`llmProviders`, default to the tracked providers in the brand profile) and `frequency` (default `weekly`; never `hourly` unless asked, since it uses many credits). Mention that new prompts use simulation credits.
3. If the user wants topics, create missing ones with `add_prompt_topic`.
4. Add the approved prompts with `bulk_add_prompt` (each item: `text`, `llmProviders`, `frequency`, and `priority` / `category` where set). Duplicates by text are skipped.
5. Report how many were added and how many were skipped as duplicates. Assign topics with `update_prompt_topic` if needed.

Never add, delete or change prompts without the user's explicit approval of the final list.

## Rules

- Decision-stage prompts only: someone choosing a provider, not learning about a topic.
- For a local business, every prompt needs location context.
- Quality over quantity. Don't pad the list.
- Plain language, phrased the way a real buyer would ask.
