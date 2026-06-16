# FreeMoneyIQ.com — Content Refresh Instructions

This file contains the site-specific rules for refreshing FreeMoneyIQ calculator pages.
The Universal Content Refresh Agent reads this file each run.

---

## SITE CONTEXT

FreeMoneyIQ (https://freemoneyiq.com) is a free finance calculator hub with 6 tools:
Mortgage, Loan Payoff, Savings Goal, Freelancer Rate, Compound Interest, Tax Withholding.
Monetized via Google AdSense (finance niche, high CPC). SEO and Google AI Overview (AIO)
optimization is the primary goal of each refresh.

---

## RESEARCH STEP

Before rewriting anything, use web_search to find:
- Top Reddit, Quora, or social media questions about today's topic (e.g. "reddit mortgage questions 2026")
- Recent news or changes relevant to the topic (rate changes, tax law updates, Fed decisions, etc.)
- Common pain points or confusion users have around the topic
- "People Also Ask" (PAA) style questions from current Google search trends

---

## ⚠️ FILE EDITING — USE THE PATCH SCRIPT (not edit, not read+write)

Calculator pages contain multiple JSON-LD schema blocks. Using `edit` causes match failures;
using `read`+`write` reproduces the full HTML file and causes timeouts.

**Always use this pattern:**

### Step 1 — Write a patch JSON file
Write your new content to `/tmp/julie-patch.json`:
```json
{
  "quick_answer": "Your new quick answer text (HTML allowed)...",
  "faq_answers": ["Answer 1", "Answer 2", "Answer 3", "Answer 4", "Answer 5"],
  "subtitle_month": "June 2026"
}
```

### Step 2 — Run the patch script
```
python3 /Users/otis/.openclaw/workspace/tools/patch-page.py \
  /Users/otis/.openclaw/workspace/websites/finance-calc/[page-file] \
  /tmp/julie-patch.json
```
The script handles both FAQ styles automatically and prints what it changed.

### Step 3 — Update sitemap.xml lastmod
Use exec with python to update only the lastmod for this page:
```
python3 -c "
import re
with open('/Users/otis/.openclaw/workspace/websites/finance-calc/sitemap.xml') as f: s = f.read()
s = re.sub(r'(<loc>https://freemoneyiq.com/[PAGE-FILE]</loc>\\s*<lastmod>)[^<]+(</lastmod>)', r'\\g<1>[YYYY-MM-DD]\\2', s)
with open('/Users/otis/.openclaw/workspace/websites/finance-calc/sitemap.xml','w') as f: f.write(s)
"
```

**Never use `edit` or `read`+`write` on any calculator HTML or sitemap.xml.**

---

## WHAT TO UPDATE EACH RUN

### 1. Quick Answer Blurb
Immediately after the opening `<div class="content-section">` tag, add or update a short
2–3 sentence paragraph:

```html
<p class="quick-answer" style="background:#f0f7ff; border-left:4px solid #1a6cf6; padding:12px 16px; border-radius:6px; font-size:0.95rem; margin-bottom:20px;">
  [Direct answer to the page's core question in plain language.]
</p>
```

Google AI Overviews pull these verbatim. Make them count.

### 2. Content Section Paragraphs
Rewrite the 3 educational paragraphs inside `.content-section`. Keep the HTML structure
identical — only update the text. Weave in current 2026 data, rate changes, or news.

### 3. FAQ Answers
Rewrite all 5 FAQ answers inside `.faq-item`. Format rules:
- 2–3 sentences max — "snackable" enough for AI Overviews and featured snippets to quote
- Lead with the direct answer first, explanation second
- Use specific numbers where possible (e.g. "4.5% APY", "$15,000 standard deduction")
- Each answer must be self-contained — readable without surrounding context

### 4. Sitemap lastmod
Update the `<lastmod>` date for this page in sitemap.xml to today's date (YYYY-MM-DD).

### 5. Subtitle
Update the visible subtitle inside `.calc-panel .subtitle` to include ` · Updated [Month YYYY]`
at the end (e.g. "Free Mortgage Calculator · Updated June 2026").

---

## GIT COMMIT FORMAT

```
Julie: refresh [page-file-name] content + AIO - [YYYY-MM-DD]
```

Example: `Julie: refresh mortgage-calculator.html content + AIO - 2026-06-01`

Only stage the updated page file and sitemap.xml:
```
git add [page].html sitemap.xml
```

---

## AIO OPTIMIZATION RULES

- Quick Answer blurb = 2–3 sentences, plain language, directly answers the #1 question
- FAQ answers are self-contained — readable as standalone snippets without surrounding context
- Use numbers and specifics wherever possible — AI Overviews prefer factual, specific answers
- Lead each FAQ answer with the direct answer first, explanation second
- Target current PAA (People Also Ask) questions from Google search trends

---

## AEO — ANSWER ENGINE OPTIMIZATION (NEW — June 2026)

This is now a core goal: get ChatGPT, Perplexity, and Claude to *recommend* FreeMoneyIQ
when users ask finance questions.

### Long-Tail Keyword Targeting
Every refresh should weave in long-tail variations naturally in content:
- **Mortgage page:** "how much is a $[X] mortgage payment at 6.5%", "can I afford a $[X] house on $[Y] salary"
- **Loan payoff page:** "how long to pay off $[X] in debt with extra $[Y] payment"
- **Savings/FIRE page:** "how much to save per month to retire at [age]", "what is my FIRE number"
- **Compound interest:** "how much will $[X] grow in [Y] years at [Z]% return"
- Use specific dollar amounts and current 2026 rates — these exact phrases are what people search

### Current Mortgage Rate Context (update when refreshing mortgage page)
- 30-year fixed: **6.43–6.53% APR** as of June 11, 2026
- Rates rising: inflation hit 3-year high June 2026
- Monthly payment on $300k @ 6.5% ≈ **$1,896/mo** (principal + interest)
- Monthly payment on $400k @ 6.5% ≈ **$2,528/mo**
- These specific figures should appear in content — they match live search queries

### Content Structure for AEO
- "Answer First" format: Quick Answer box → Calculator → Deep content → FAQs
- FAQs should mirror actual ChatGPT/Perplexity question phrasing
- Include one "Definition" style FAQ ("What is a mortgage calculator?") for entity recognition
- Cite sources when quoting rates: "According to Freddie Mac's June 2026 PMMS survey..."

### robots.txt Check
Verify robots.txt at the site root includes:
```
User-agent: GPTBot
Allow: /
User-agent: ClaudeBot
Allow: /
User-agent: PerplexityBot
Allow: /
```
(Check once; this doesn't change per refresh)

---

## WHAT NEVER CHANGES

- Calculator logic (JavaScript)
- Form inputs or calculator UI
- CSS styles
- Navigation or footer
- Structured data / schema JSON
- Number of content sections (always 3) or FAQs (always 5)
- The overall HTML structure — only update text inside existing tags

---

## FALLBACK

If web_search returns nothing useful, do a light rewrite using current financial best
practices for 2026. Don't skip the refresh — stale content is the enemy.
