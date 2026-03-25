# Deep Company and Product Research Skill

## Overview

Deep Company and Product Research is a high-depth intelligence skill designed to perform exhaustive analysis of any company or product. It executes a structured multi-phase pipeline combining web search, document retrieval, and synthesis to generate a comprehensive research report.

The output is a professionally formatted PDF report exceeding 20 pages, built entirely on verifiable sources. This skill is intended for due diligence, competitive intelligence, technical evaluation, and strategic decision-making.

https://agentshare.io/skills/597b9dce-38d5-4f6a-ac7e-d944d4669ab2

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/8eab82a6-1e5d-412f-ae8d-7d89d45ab058" />

---

## When to Use

Trigger this skill immediately when any of the following conditions are met:

- A company or product URL is provided
- The user asks to research, analyze, or deep dive into a company or product
- A company or product name is mentioned with intent to understand it
- The user asks about competitors, architecture, tech stack, or capabilities

This skill should not be bypassed for such queries, as it produces significantly higher quality results than direct responses.

---

## Capabilities

- Executes 20 to 30 targeted searches across diverse sources
- Extracts and verifies data from official websites, documentation, and engineering blogs
- Analyzes GitHub repositories and open source contributions
- Infers technology stack from job postings and infrastructure signals
- Retrieves patents and research papers from authoritative databases
- Aggregates customer case studies and review platform insights
- Maps competitive landscape and market positioning
- Produces a structured, multi-section intelligence report

---

## Research Pipeline

### Phase 0: Target Identification

Extract:
- Target name
- URL if available
- Research intent

If only a URL is provided, fetch and extract metadata before proceeding.

---

### Phase 1: Systematic Search

Minimum 20 searches, ideally 25 to 30.

#### Foundation
- Official website
- Founders and team
- Engineering blog
- GitHub presence
- Wikipedia or Crunchbase

#### Product Deep Dive
- Architecture and system design
- API and documentation
- Features and capabilities
- Pricing and use cases
- Reviews and walkthroughs

#### Technology Stack
- Languages and frameworks
- Infrastructure and cloud
- Job postings for stack inference
- DevOps and tooling
- Open source contributions

#### Patents and Research
- Google Patents
- arXiv, Semantic Scholar
- IEEE and ACM publications

#### Customers and Market
- Case studies
- Customer lists
- Reviews from G2, Capterra, Trustpilot
- Testimonials and deployments

#### Competitive Landscape
- Competitors and alternatives
- Market positioning
- Funding and valuation

---

### Phase 2: Data Structuring

Organize findings into structured sections:

- Overview
- Product and architecture
- Technology stack
- Patents and research
- Customers and case studies
- Competitive landscape
- Funding
- Sources

All claims must be traceable to a source.

---

### Phase 3: Report Generation

Generate a PDF using ReportLab.

#### Report Structure

- Cover page
- Table of contents
- Executive summary

Sections:
1. Company Overview
2. Product Deep Dive
3. Technology Stack
4. Patents and Research Papers
5. Customer Base and Case Studies
6. Competitive Landscape
7. Sources and References

---

### Phase 4: Quality Checks

Ensure:

- Minimum 20 pages
- All sections are present
- No empty sections
- All sources are valid and verifiable
- Tech stack entries include source attribution
- PDF renders without errors

---

### Phase 5: Output

- Save report to `/mnt/user-data/outputs/`
- Present file to user
- Provide concise summary including:
  - Company overview
  - Key technical insights
  - Tech stack highlights
  - Research and patent activity
  - Customer and deployment context

---

## Technical Requirements

- Tools: web_search, web_fetch, bash_tool, create_file, present_files
- Python libraries:
  - reportlab
  - requests

Install dependency:

```bash
pip install reportlab --break-system-packages -q
