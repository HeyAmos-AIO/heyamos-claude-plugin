---
name: brand-accuracy-check
description: Check what AI tools actually say about a brand using HeyAmos answer data — fact-check statements against the brand's site, find what drives negative or mixed sentiment, show how AI positions the brand against competitors, and trace each problem to the pages behind it. Use when the user asks whether AI gets their facts right, why answers are negative, how AI perceives or describes them, or for a brand accuracy, sentiment or perception audit.
---

# Brand accuracy, sentiment and perception check

This is a read-only workflow. You never change anything in HeyAmos. Follow the writing rules in the `geo-analyst` skill's `references/writing-style.md`, and its `references/tool-usage.md` for parameter formats.

## Step 0: Explain and ask

Say: "This reads what AI tools actually say about your brand. It checks the facts against your site, finds what makes answers negative, shows how AI positions you against competitors, and finds the pages behind any problem."

Ask whether to run it now, and offer to run it monthly if a scheduling feature is available. Ask whether there are facts AI must get right (prices, fees, partners, locations, licences).

Call `get_brand_profile` to confirm which brand and workspace you're connected to, and note the tracked AI tools.

## Step 1: Check the data is fresh

Call `get_citations_by_domain_type` for the last 8 days. If there is no data, say "Latest answers are more than 8 days old, so there's nothing new to check yet." and stop. Otherwise note the latest answer date and use the last 4 weeks of answers.

## Step 2: Read what the app already says

Read the latest `get_what_changed`, `get_suggested_actions` and `get_alerts`. Note anything about tone or facts. Don't repeat it; add what it misses.

## Step 3: Pull real answers

1. Use `get_all_prompts_with_metrics` to note each prompt's sentiment and which prompts name the brand.
2. Call `query_mentions` with `isIncluded` true, once per tracked AI tool (filter with `llm`, since results don't say which tool). Then again with `sentiment` set to `negative`, and to `mixed`. Page through until you have the real extracts.
3. If no prompt names the brand, that's fine: read how AI describes it when it comes up on its own.
4. Separate extracts that describe the brand from ones that only cite its pages.

## Step 4: Accuracy

List every fact the answers state about the brand: prices and fees, features, partners, locations, numbers, claims. Check each against the brand's own site on the web and `get_brand_profile`. Mark each **right**, **wrong**, **outdated**, or **not on your site**. Keep the exact quote and the AI tool. If the brand's own pages disagree with each other, say so.

## Step 5: Sentiment

Call `get_mention_negative_themes` and read the negative and mixed extracts. Say how answers lean overall and what drives the negatives. Separate fair hesitations (fees or limits that really exist) from unfair or wrong ones.

## Step 6: Perception

- Call `get_mention_brand_values` and `get_mention_choice_factors`. What is the brand known for, and why does AI pick it or pass?
- Note which brands AI lists next to it, and how it describes the brand against them (`get_mention_competing_themes` helps).
- Note differences between AI tools in tone and facts.

## Step 7: Sources

For each wrong or negative statement, find the pages behind it: `get_citations` filtered by prompt and AI tool (use `search` for your domain), or `query_mentions` with `citationUrl`. Open those pages on the web. Say whether each is the brand's, a third party's or a competitor's, and whether it's out of date.

## Step 8: Write it up

The report (about 500 words):

- **Header:** latest answer date and the period checked.
- **In short:** 3 sentences — the overall picture, what AI gets right, the most important fix.
- **Accuracy:** a table of statement (exact quote), AI tool, verdict, the right fact, and the source.
- **Sentiment:** the overall lean, the negative themes with a quote each, and whether each is fair.
- **Perception:** what you're known for, why AI picks you or doesn't, who you're listed with, differences between AI tools.
- **What to do next:** 2–4 plain fixes, such as the page to correct or add, or the source to address.
- **Not checked:** what you couldn't check. For Search Console and GA4 (`get_gsc_status`, `get_ga4_status`), say only connected or not.

## Ground rules

- Quote real answers exactly. Never invent or improve a quote.
- Call a fact wrong only when the brand's site or a reliable source says otherwise. Otherwise mark it "not verified".
- Most answers are neutral. Say so when it's true. A sentiment number is never a finding without a quote.
- Write plainly, to "you". Say ChatGPT, not OpenAI.
- If a page won't open, say so.
- If you use `check_external_web_page_mention`, make it your last call. If it fails, read the page on the web instead.
