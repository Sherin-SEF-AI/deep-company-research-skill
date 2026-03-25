---
name: deep-company-research
description: >
  Exhaustive research skill for any company or product. Use this skill IMMEDIATELY whenever:
  - The user shares a company or product URL (any domain URL that looks like a company/product website)
  - The user says "research this company", "research this product", "deep dive on X", "find everything about X"
  - The user pastes a company name, product name, or startup name and wants information
  - The user says "what do you know about [company/product]", "tell me about [company]", "analyze [company]"
  - The user asks about competitors, tech stack, how a product works, or what technology a company uses
  This skill performs 20+ targeted web searches across official docs, GitHub, patent databases, engineering blogs,
  job postings, news archives, and review platforms. Output is always a professionally formatted, exhaustive PDF
  report (20+ pages) saved to /mnt/user-data/outputs/. Never skip this skill for company/product research tasks —
  it will always produce a better result than answering from memory alone.
compatibility: "Requires: web_search, web_fetch, bash_tool, create_file, present_files tools. Python libs: reportlab, requests (auto-installed)."
---

# Deep Company & Product Research Skill

## Purpose
Perform exhaustive, multi-source research on any company or product and deliver a structured, 20+ page PDF
intelligence report. Every claim must come from a verifiable web source. No assumptions, no hallucinations.

---

## Phase 0 — Target Identification

Before searching, extract:
- **Target name**: company name, product name, or both
- **URL** (if provided): use as the primary seed
- **Research intent**: competitive analysis, due diligence, partnership evaluation, technical assessment?

If only a URL is given, fetch it first to extract the company/product name and brief description before proceeding.

```python
# Quick target extraction — run before any searches
# web_fetch(url) → extract name, tagline, product category
```

---

## Phase 1 — Systematic Search Pipeline

Execute searches in this exact order. Each phase builds on the previous.
**Minimum total searches: 20. Target: 25-30 for exhaustive coverage.**
Run web_fetch on the most information-dense URLs returned by each search batch.

### 1.1 Foundation Searches (5 searches)
```
"{company}" official website overview                      # Already fetched if URL given
"{company}" about team founders history
"{company}" engineering blog OR tech blog
"{company}" GitHub repository
"{company}" Wikipedia OR Crunchbase
```

### 1.2 Product Deep-Dive (6 searches)
```
"{product}" how it works architecture overview
"{product}" technical documentation API
"{product}" whitepaper OR research report
"{product}" demo review walkthrough {current_year}
"{product}" features capabilities breakdown
"{product}" pricing model customers use case
```

### 1.3 Technology Stack & Infrastructure (5 searches)
```
"{company}" technology stack programming languages frameworks
"{company}" site:stackshare.io OR site:builtwith.com
"{company}" job postings engineer required skills {current_year}
"{company}" infrastructure cloud provider AWS GCP Azure
"{company}" open source contributions libraries
```
> Job postings are the single most reliable source for real tech stack. Always fetch 2-3 job listing pages.

### 1.4 Patents & Research Papers (4 searches)
```
"{company}" site:patents.google.com
"{company}" site:arxiv.org OR site:semanticscholar.org
"{company}" IEEE OR ACM research publication
"{company}" inventor patent filing
```
> For each patent or paper found: record title, filing date, patent number, abstract summary, and technical claims.

### 1.5 Customer Base & Case Studies (5 searches)
```
"{company}" case studies customers success stories
"{company}" clients enterprise customers list
"{company}" site:g2.com OR site:capterra.com reviews
"{company}" site:trustpilot.com OR site:producthunt.com
"{company}" customer testimonial quote deployment
```

### 1.6 Competitive & Market Context (3 searches)
```
"{company}" competitors alternatives comparison
"{company}" market share industry position {current_year}
"{company}" funding investment raise valuation
```

### 1.7 Deep-Fetch Priority URLs
After all searches, fetch full content from:
- Official docs / developer portal
- Any engineering blog posts found
- Top 2-3 job listing pages (LinkedIn, Greenhouse, Lever, Workday)
- Any patent pages found on Google Patents
- G2/Capterra review pages
- Any GitHub README or org page

---

## Phase 2 — Data Structuring

After all searches and fetches, organize findings into this canonical structure before writing the PDF:

```python
report_data = {
    "meta": {
        "target_name": str,
        "target_url": str,
        "report_date": str,       # today's date
        "research_depth": int,    # number of sources consulted
    },
    "overview": {
        "description": str,
        "founded": str,
        "hq_location": str,
        "employee_count": str,
        "category": str,          # e.g. "AI Safety Platform", "ADAS Software"
        "tagline": str,
    },
    "product": {
        "core_functionality": str,
        "architecture_summary": str,
        "key_features": list,     # bullet list
        "api_details": str,
        "deployment_modes": list, # cloud / on-prem / edge / hybrid
        "integration_ecosystem": list,
    },
    "tech_stack": {
        "languages": list,
        "frameworks_libraries": list,
        "infrastructure": list,   # cloud providers, databases, message queues
        "ml_ai_stack": list,      # models, frameworks (PyTorch, ONNX, TensorRT...)
        "devops_tooling": list,
        "hardware_targets": list, # Jetson, x86, FPGA, etc.
        "sources": list,          # where each was confirmed
    },
    "patents": [
        {
            "title": str,
            "number": str,
            "filing_date": str,
            "inventors": list,
            "abstract": str,
            "key_claims": list,
            "url": str,
        }
    ],
    "research_papers": [
        {
            "title": str,
            "authors": list,
            "venue": str,         # conference, journal, arXiv
            "year": str,
            "summary": str,
            "url": str,
        }
    ],
    "customers": {
        "named_customers": list,
        "industries_served": list,
        "case_study_highlights": list,  # [{customer, problem, solution, result}]
        "review_summary": str,
        "g2_rating": str,
    },
    "competitive_landscape": {
        "direct_competitors": list,
        "differentiators": list,
        "market_position": str,
    },
    "funding": {
        "total_raised": str,
        "latest_round": str,
        "investors": list,
        "valuation": str,
    },
    "sources": list,  # all URLs consulted, deduplicated
}
```

---

## Phase 3 — PDF Generation

Use `reportlab` with the Platypus layout engine. Follow the PDF SKILL.md conventions.
Read `/mnt/skills/public/pdf/SKILL.md` for library reference if needed.

### Install dependency
```bash
pip install reportlab --break-system-packages -q
```

### PDF Structure (20+ pages target)

```
Cover Page          — Company/product name, logo placeholder, report date, "Confidential Research Report"
Table of Contents   — Auto-generated with page numbers
Executive Summary   — 1 page: what this company is, what they've built, why it matters
─────────────────────────────────────────────────────────────
Section 1: Company Overview          (2-3 pages)
  - Description, founding story, mission
  - Location, team size, leadership
  - Business model, revenue approach
  - Timeline of key milestones

Section 2: Product Deep-Dive         (4-5 pages)
  - What the product does (plain language + technical)
  - Architecture diagram description (text-based ASCII or described)
  - Core feature breakdown with technical depth
  - API / SDK / integration capabilities
  - Deployment options (cloud, on-prem, edge, hybrid)
  - Pricing model if known

Section 3: Technology Stack          (3-4 pages)
  - Programming languages (with confirmation source)
  - ML/AI frameworks and models used
  - Infrastructure and cloud stack
  - DevOps and CI/CD tooling
  - Hardware targets / edge deployment
  - Open-source contributions
  - Stack inference from job postings (clearly labeled)

Section 4: Patents & Research Papers (3-4 pages)
  - Each patent: number, title, filing date, abstract, key claims
  - Each paper: title, authors, venue, year, summary
  - IP strategy analysis (what areas they're protecting)
  - Research trajectory and technical bets

Section 5: Customer Base & Cases     (3-4 pages)
  - Named customers (with industry)
  - Detailed case studies (problem / solution / outcome)
  - Review platform ratings and themes
  - Industries and verticals served
  - Deployment scale indicators

Section 6: Competitive Landscape     (1-2 pages)
  - Direct competitors table
  - Differentiators
  - Market positioning

Section 7: Sources & References      (1-2 pages)
  - All URLs consulted, organized by category
  - Research date and methodology note
─────────────────────────────────────────────────────────────
```

### PDF Generation Script Template
See `/home/claude/deep-company-research/references/pdf_template.md` for the full ReportLab
generation script. Key conventions:

```python
from reportlab.lib.pagesizes import A4
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.platypus import (
    SimpleDocTemplate, Paragraph, Spacer, PageBreak,
    Table, TableStyle, HRFlowable
)
from reportlab.lib.units import inch, cm

# Color palette — professional intelligence report aesthetic
PRIMARY_COLOR   = colors.HexColor('#0D1B2A')   # deep navy
ACCENT_COLOR    = colors.HexColor('#1B6CA8')   # corporate blue
SECTION_COLOR   = colors.HexColor('#F4F6F9')   # light section bg
TEXT_COLOR      = colors.HexColor('#1A1A2E')
MUTED_COLOR     = colors.HexColor('#6B7280')

# Always use A4, 1.5cm margins
doc = SimpleDocTemplate(
    output_path,
    pagesize=A4,
    rightMargin=1.5*cm, leftMargin=1.5*cm,
    topMargin=2*cm, bottomMargin=2*cm
)
```

**Critical rendering rules:**
- Never use Unicode subscript/superscript characters — use `<sub>` and `<super>` XML tags in Paragraphs
- Escape all `<`, `>`, `&` in user-sourced content before passing to Paragraph
- Use `Table` with `TableStyle` for all structured data (tech stack, patents, competitors)
- Add `HRFlowable` horizontal rules between major sections
- Page numbers via `doc.build(story, onFirstPage=header_footer, onLaterPages=header_footer)`

---

## Phase 4 — Quality Checks

Before saving the PDF, verify:

- [ ] Minimum 20 pages generated (`len(reader.pages) >= 20`)
- [ ] All 5 mandatory sections present (Product, Tech Stack, Patents, Customers, Sources)
- [ ] No section is empty — if no patents found, state "No public patents identified as of [date]"
- [ ] Tech stack entries each have a source annotation
- [ ] All URLs in sources section are real (came from search results, not invented)
- [ ] PDF renders without errors (no `[]` glyph boxes)
- [ ] File saved to `/mnt/user-data/outputs/{company_name}_research_report_{date}.pdf`

---

## Phase 5 — Delivery

```python
# Present the file
present_files([f"/mnt/user-data/outputs/{filename}"])
```

After presenting, give the user a 4-5 sentence summary covering:
1. What the company/product is in one sentence
2. The most technically interesting finding
3. Key tech stack highlights
4. Patent/research activity level
5. Notable customers or deployment context

---

## Edge Cases

**No official website available**: Work entirely from search results — news, GitHub, job boards, LinkedIn.

**Stealth/very early startup**: Reduce patent section, expand job postings analysis and founder background.
Search LinkedIn, Twitter/X, YC profile, PH profile aggressively.

**Product with no public docs**: Infer architecture from job postings, GitHub issues, conference talks on YouTube.
Label all inferred data clearly as `[Inferred from: source]`.

**Multiple products under one company**: Research each product separately in Section 2 with sub-sections.
Consolidate tech stack across all products.

**Non-English content**: Fetch and summarize — use web_fetch which handles most encodings.

---

## Search Quality Rules

- Never repeat the same query twice
- Every query must be distinct in intent, not just rephrased
- Prefer primary sources: official docs > engineering blogs > news > secondhand summaries
- For tech stack: job postings > BuiltWith > blog posts > press releases
- For patents: Google Patents is canonical — always use it directly
- Mark every data point with its source in the structured data before writing to PDF
- If a source contradicts another, note both and flag the discrepancy in the report
