# Citation Verification Protocol and Literature Search Configuration

This file governs how citations are found, stored, verified, and formatted.
Load this file at Step 2 (literature search) and Step 7 (citation audit).

---

## User-Provided Papers

If the user has a folder with downloaded literature papers, scan it first:

1. Ask for the folder path
2. Read each PDF to extract: title, author(s), year, journal, abstract
3. Add these papers to the reference table with Download = "User provided"
4. Then search for additional papers to fill gaps in the literature

User-provided papers are treated as verified sources. They skip the
accessibility check since the user already has them.

---

## The Citation-First Principle

The core rule of this skill: **search first, write second**.

Traditional (broken) flow:
  Write text -> try to add citations -> hallucinated references

Correct flow:
  Search literature -> build reference table -> write AROUND the citations

Every factual claim in the output must trace back to a paper in the
reference table. If no source exists, mark `[CITATION NEEDED]`.

---

## Literature Search Sources (Free, No Account Required)

The skill uses free, publicly accessible academic search platforms.
No paid account is required for any of these.

### Primary sources (use web search to query these)

**Semantic Scholar** (semanticscholar.org)
- Open API and website, no account needed
- Structured metadata: authors, year, title, abstract, citation count, DOI
- Covers all academic fields
- Best for: finding papers with full metadata and citation networks

**Google Scholar** (scholar.google.com)
- No account needed, broadest coverage of any academic search engine
- Includes journal articles, conference papers, theses, books, preprints
- Best for: casting a wide net, finding highly cited seminal papers
- Limitation: metadata less structured than Semantic Scholar

**arXiv** (arxiv.org)
- Open access preprint repository, no account needed
- Strong in: economics (econ.*), computer science, quantitative finance,
  statistics, mathematics
- All papers are freely downloadable as PDF
- Best for: recent working papers and preprints

**NBER Working Papers** (nber.org/papers)
- Economics working papers, freely searchable
- Many papers have free PDF access; some require NBER subscription
  but most are also on author websites or SSRN
- Best for: high-quality economics research before journal publication

**RePEc/IDEAS** (ideas.repec.org)
- Economics and finance papers, freely searchable
- Links to open access versions when available
- Best for: finding published economics papers with download links

**SSRN** (ssrn.com)
- Social science preprints, freely downloadable
- Covers economics, finance, law, management
- Best for: working papers in social sciences

### Institutional sources (reports, policy papers, data documentation)

- **United Nations** (un.org, unctad.org, undp.org): SDG reports, trade
  data documentation, development policy papers
- **OECD** (oecd.org, oecd-ilibrary.org): economic surveys, policy briefs,
  working papers, statistics documentation
- **IMF** (imf.org): World Economic Outlook, working papers, country
  reports, policy discussion papers
- **World Bank** (worldbank.org): policy research working papers,
  development reports, data documentation
- **European Commission** (ec.europa.eu): policy papers, Eurostat
  methodology notes, research briefs
- **BIS** (bis.org): financial stability reports, working papers,
  quarterly reviews

### HTML and web sources (any source that adds value)

- Policy briefs and research centre publications
- Government statistical agency reports and methodology notes
- Think tank publications (Brookings, CEPR, VoxEU, VoxDev, etc.)
- Blog posts from reputable economists or researchers (with caveats)
- News articles from specialist outlets (e.g., Financial Times, The
  Economist) when citing facts or data, not opinions
- Conference proceedings and presentation slides with citable content
- Technical documentation for datasets or methodologies

When citing HTML or web sources, always include the full URL and access
date. Use `@misc` or `@online` BibTeX types for these.

### Search strategy
- Generate 5-7 targeted search queries (4-8 words each)
- Search across at least 2 different sources to cross-verify papers
- Keep queries short; long queries dilute relevance
- Include the core topic plus an applied angle
  (e.g., "renewable energy employment causal" not just "renewable energy")
- Prioritise recent papers (last 5 years) but include seminal older works

---

## Paper Accessibility Verification

**Every paper in the reference table must be downloadable by the user.**

After finding a paper, verify that the user can access the full text
through at least one of these channels:

1. **Open access**: paper is freely available on the journal website,
   publisher site, or an open repository (arXiv, SSRN, RePEc)
2. **Author website or institutional repository**: many authors host
   PDFs on their personal or university pages
3. **Preprint version**: check arXiv, SSRN, or NBER for a free version
   of a paywalled journal article
4. **University library**: most universities provide access to major
   journals through library subscriptions

For each source in the reference table, include **Source** and **Download** columns:

| # | Author(s) | Year | Title | Journal/Publisher | Key Finding | Role | Source | BibTeX Key | Download |
|---|-----------|------|-------|-------------------|-------------|------|--------|------------|----------|

The **Source** column must be one of:
- **User provided**: from the user's folder or explicitly named by user
- **Academic search**: found via Semantic Scholar, Google Scholar, arXiv, etc.
- **Institutional**: from UN, OECD, IMF, World Bank, EU, BIS, etc.
- **Web/HTML**: from policy briefs, think tanks, blogs, or other web sources

The **Download** column should contain one of:
- A direct URL to a freely available PDF or HTML page
- "Open Access" if available directly from the publisher
- "User provided" if from the user's folder
- "Via university library" if paywalled but available through
  standard institutional access
- "DOI: 10.xxxx/xxxxx" as a fallback (user can resolve via library)

### Accessibility rules
- Prioritise papers that have a free PDF available somewhere
- If a paper is completely paywalled with no free version, note this
  and suggest an alternative paper covering similar content
- NEVER include a paper the user cannot reasonably obtain
- When presenting the reference table to the user, highlight any papers
  that may require library access so they can confirm availability

---

## The Reference Table

After searching, compile ALL retrieved papers into a structured table:

| # | Author(s) | Year | Title | Journal | Key Finding | Role | BibTeX Key | Download |
|---|-----------|------|-------|---------|-------------|------|------------|----------|

### Column definitions
- **#**: Sequential number for easy reference
- **Author(s)**: Last names. Use "et al." for 3+ authors in text, full list in bib
- **Year**: Publication year
- **Title**: Full paper title
- **Journal**: Journal name, "Working Paper", or "Preprint"
- **Key Finding**: 1-sentence summary of main result relevant to user's topic
- **Role**: How this paper serves the section being written
  - For introductions: Territory (Move 1), Niche (Move 2), or Occupation (Move 3)
  - For other sections: Background, Method, Comparison, Data Source
- **BibTeX Key**: Generated key in format `authorYear` (e.g., `smith2020`)
- **Download**: URL to free PDF, "Open Access", "Via university library", or DOI

### Reference table rules
- Present the table to the user for approval BEFORE drafting
- User may add papers they already know, remove irrelevant ones, or re-classify
- The table is the SINGLE SOURCE OF TRUTH for citations in the draft
- No citation may appear in the draft that is not in this table
- Highlight any papers that require library access

---

## BibTeX Generation

For every paper in the reference table, generate a BibTeX entry:

```bibtex
@article{smith2020,
  author  = {Smith, John and Jones, Mary},
  title   = {The Effect of X on Y: Evidence from Z},
  journal = {Journal of Economics},
  year    = {2020},
  volume  = {15},
  number  = {3},
  pages   = {123--145},
  doi     = {10.1234/example}
}
```

### BibTeX rules
- Use `@article` for journal papers, `@techreport` for working papers,
  `@inproceedings` for conference papers, `@book` for books
- Include DOI when available
- If volume/pages are unknown, omit them (do NOT fabricate)
- Save all entries to `references.bib` in the output folder

---

## Citation Audit (Post-Draft)

After drafting, run this cross-check:

### Forward check (draft -> table)
For every `(Author, Year)` citation in the draft:
1. Does it exist in the reference table? If NO: REMOVE or mark `[CITATION NEEDED]`
2. Is the claim accurately attributed? If NO: correct the claim
3. Is the year correct? If NO: fix it

### Backward check (table -> draft)
For every paper in the reference table:
1. Is it cited in the draft? If NO: consider if it should be
2. Is it cited in the right context? If NO: move or re-contextualise

### Result
- 0 unverified citations = PASS
- Any unverified citation = FAIL, revise before delivery

---

## In-Text Citation Format

Use author-year format throughout:
- One author: "Smith (2020) found..."  or  "...has been shown (Smith, 2020)"
- Two authors: "Smith and Jones (2021)"
- Three or more: "Smith et al. (2022)"
- Multiple citations: "(Smith, 2020; Jones et al., 2021; Lee, 2022)"
  Order chronologically within parenthetical citations.
