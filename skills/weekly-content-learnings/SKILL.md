---
name: weekly-content-learnings
description: Weekly HeyAmos content review that goes one layer deeper than the app's Insights and Actions, then updates a content writer guide for the brand's writing agent. Use when the user asks for weekly content learnings, what content AI picked up this week, whether earlier suggested actions went live or helped, which prompts are worth the effort, or to create or update their content-writer-guide.md.
---

# Weekly content learnings and writer guide

This is a read-only workflow. You never change anything in HeyAmos. Follow the writing rules in the `geo-analyst` skill's `references/writing-style.md`, and its `references/tool-usage.md` for parameter formats.

## Step 0: Explain and ask

Say: "Each week this reads your Insights and Actions, then digs one layer deeper. It checks whether earlier actions went live, compares the new articles AI picked up with those it didn't, and updates a content writer guide for your writing agent."

Ask whether to run it now. Offer to run it weekly, the day after new Insights arrive, if a scheduling feature is available. Ask whether they have last week's `content-writer-guide.md`.

Call `get_brand_profile` to confirm which brand and workspace you're connected to, and say which one.

## Step 1: Check the data is fresh

1. Call `get_what_changed` and note the week label exactly as the app shows it.
2. Call `get_citations_by_domain_type` for the last 8 days.
3. If there is no data, say "Latest answers are more than 8 days old, so there's nothing new to learn yet." and stop.
4. Otherwise note the latest answer date.

## Step 2: Read what the app already says

Read the last few weeks of `get_what_changed`, `get_suggested_actions` and `get_alerts`. Start from these. Don't repeat them; add what they don't say.

## Step 3: Did earlier actions happen, and help?

Find actions suggested more than once in recent weeks. For the top few:

- Check whether each went live: open the brand's site, its sitemap, or use `get_owned_domain_pages_list`. For external actions, open the third-party page and look for the brand.
- If it's live, compare the page's citations and its target prompt's visibility with before.

## Step 4: What AI picked up, and why

- **Picked up vs not.** List the brand's pages published or discovered in the last 4 weeks (`get_owned_domain_pages_list` with `startDate`, plus the sitemap). Check each against the brand's citations for the latest answer day (`get_citations` with `type` set to `owned`). Split them into picked up and not picked up.
- **Open a few of each on the web.** For each, note: title, meta description, headings, whether it opens with a quick answer, lists or tables, an FAQ, schema, published and updated dates, and length. Note what the picked-up pages share that the others lack.
- **Across weeks.** Compare the brand's cited pages over the last 4 answer days. Which carry you every week, which are new, and which faded? Check whether a faded page moved or broke.
- **Outside pages.** Open competitor or third-party pages that gained citations. Search the web for new coverage from the last month. Check whether last week's "learned" points still hold.
- **AI tools.** Call `get_citations_by_domain` for each tracked AI tool. Note where the brand ranks and which sources each tool leans on.

## Step 5: Which prompts deserve the effort

Get each prompt's search volume from `get_all_prompts_with_metrics` (`get_top_prompts_by_volume` shows the top 5). Compare each prompt's visibility over recent weeks and give a one-line call:

- **Keep working it:** it has real volume and you're absent, weak or rising.
- **Swap it:** it has little or no volume and hasn't moved, or you already win it steadily.

List high-volume prompts first. Swaps are suggestions only; don't delete anything.

## Step 6: Write it up

**The report** (about 500 words):

- **Header:** week label and latest answer date.
- **What we learned:** 3–5 points the app's tabs don't say, each with evidence.
- **Picked up vs not:** a small table of new pages, cited or not, and the main format differences.
- **Earlier actions:** the action, times suggested, live or not, and whether it helped.
- **Prompts:** keep or swap per prompt, with its volume.
- **What changed in the guide:** rules added, kept or dropped.
- **What to do next:** 2–4 plain steps.
- **Not checked:** what you couldn't check. For Search Console and GA4 (`get_gsc_status`, `get_ga4_status`), say only connected or not.

**`content-writer-guide.md`** is a short rule list for the brand's writing agent:

- Each rule names a picked-up URL as its evidence.
- Mark a rule seen on only one page as "early signal".
- If the user has last week's guide, read it first. Add rules with new evidence, keep rules that still hold, and drop rules the data no longer supports.
- Keep it under about 300 words.

Write the guide to a file if you can write files; otherwise put it in your reply as a Markdown block.

## Ground rules

- Use only numbers from this run's tool calls and pages you opened. Never guess.
- Say "pages with X were cited more", not "X causes citations".
- Write plainly, to "you". Don't copy tool headings.
- If a page won't open, say so.
- If you use `check_external_web_page_mention`, make it your last call. If it fails, read the page on the web instead.
