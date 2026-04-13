# Research Helper

A **Claude Skill** that drafts academic
research paper sections (Executive Summary, Introduction, Conclusion) using a
citation-first, literature-grounded workflow. Optionally generates a
**Methodology Annex** with diagrams, equations, and dataset specifications, and
exports to **LaTeX/PDF** and/or **Word**.

Designed for empirical research in economics, business, and the social
sciences, but field-agnostic.

---

## What it does

Most LLMs draft papers in the wrong order: prose first, citations bolted on
afterwards (often hallucinated). Research Helper inverts the workflow:

1. **Search the literature** across academic and institutional sources
   (Semantic Scholar, Google Scholar, arXiv, NBER, RePEc, SSRN, OECD, IMF,
   World Bank, BIS, EU).
2. **Build an explicit reference table** with every source the draft can use,
   tagged with its rhetorical role (CARS framework: Territory / Niche /
   Occupation).
3. **Get user approval** of the reference table before drafting.
4. **Draft the section**, citing only from the approved table.
5. **Audit** with the Anderson checklist plus a forward/backward citation
   cross-check. Zero mismatches required.
6. **Compile** to LaTeX+PDF, Word, or both via the bundled build script.

The reference table is the single source of truth for citations. Hallucinated
references are blocked by construction, not by hope.

---

## Features

- **Two modes** — `Full` loads all reference files; `Light` loads only those
  needed for the chosen section/field via a routing table. Same 8-step
  pipeline, different context budget.
- **Title-as-input shortcut** — `/research-helper [paper title]` pre-populates
  topic and infers field where possible.
- **CARS-structured introductions** — Create A Research Space (Swales) applied
  invisibly: Territory, Niche, Occupation moves without exposing the labels.
- **Anderson 5-point quality audit** — every Intro/ExecSum must pass.
- **Methodology Annex (optional)** — flowchart (TikZ for PDF, structured table
  for Word), LaTeX equations, dataset table with access levels, and a
  methodology↔data mapping.
- **Data-feasibility filter** — silently shapes the research question to what a
  masters student can actually obtain and analyse. Constraints never appear in
  the output prose.
- **Style discipline** — no em-dashes, no fabricated results, future tense for
  pre-study writing, explicit RQ/hypothesis required.
- **Dual output** — `scripts/build.py` produces PDF (via LaTeX) and `.docx`
  (via `python-docx`, not pandoc, so TikZ and complex math survive).

---

## Installation

This is a Claude Code skill. Drop the folder into your skills directory:

```bash
git clone https://github.com/rubilamb/research-helper.git \
  ~/.claude/skills/research-helper
```

Restart Claude Code (or run `/skills` to refresh). The skill auto-loads when
its triggers fire.

### Build script dependencies

```bash
pip install python-docx
```

For PDF output you need a working LaTeX installation (TeX Live, MacTeX, or
MiKTeX) with `pdflatex` and `bibtex` on `PATH`.

---

## Usage

In Claude Code, invoke the skill with any of:

```
/research-helper
/research-helper Carbon pricing and firm-level innovation in the EU ETS
write my introduction
draft research paper
write executive summary
write conclusion
```

The skill will then ask, one question at a time:

1. Full mode or Light mode
2. Which section (Exec Summary / Introduction / Conclusion)
3. Research topic *(skipped if title was supplied)*
4. Field(s) *(skipped if inferable)*
5. Path to a folder of literature PDFs, if any
6. Specific must-cite papers
7. Whether a methodology is already defined
8. Whether to generate a Methodology Annex
9. Output format: LaTeX+PDF, Word, or both
10. Save location *(Full mode only)*

It then runs the 8-step pipeline. You will be asked to **approve the
reference table** before any prose is drafted.

---

## The 8-step pipeline

| # | Step | Purpose |
|---|------|---------|
| 1 | Gather context | Topic, field, papers, RQ if available |
| 2 | Search literature & build reference table | 15–25 sources, CARS-tagged, presented for approval |
| 3 | Assess data feasibility *(Intro/ExecSum)* | Silently shape RQ to obtainable datasets |
| 4 | Classify sources & shape RQ | Assign Territory/Niche/Occupation roles |
| 5 | Draft the section | Cite only from the approved table |
| 6 | Methodology Annex *(if requested)* | Flowchart, equations, dataset table, mapping |
| 7 | Quality audit *(Intro/ExecSum)* | Anderson 5/5 + forward/backward citation check |
| 8 | Generate output | Compile via `scripts/build.py` |

Both modes run all 8 steps. Mode only changes which reference files are
loaded into context.

---

## Reference routing (Light mode)

| Section | Field | Files loaded |
|---|---|---|
| Introduction / Exec Summary | Economics / Finance | `cars-framework.md`, `anderson-checklist.md`, `citation-rules.md`, `economics-conventions.md` |
| Introduction / Exec Summary | Other | `cars-framework.md`, `anderson-checklist.md`, `citation-rules.md` |
| Conclusion | Any | `section-guides.md`, `citation-rules.md` |

Loaded on demand in both modes: `methodology-annex-guide.md` (only when an
annex is requested).

---

## Repository layout

```
research-helper/
├── SKILL.md                  Skill manifest + pipeline definition
├── assets/
│   └── template.tex          LaTeX template used by build.py
├── references/               Tier-3 reference files, loaded on demand
│   ├── cars-framework.md
│   ├── anderson-checklist.md
│   ├── economics-conventions.md
│   ├── section-guides.md
│   ├── citation-rules.md
│   ├── methodology-annex-guide.md
│   └── worked-example.md
└── scripts/
    └── build.py              PDF / DOCX builder
```

---

## Build script

```bash
python scripts/build.py --input draft.json --format pdf
python scripts/build.py --input draft.json --format docx
python scripts/build.py --input draft.json --format both
```

The skill writes the JSON companion file for you during Step 8; you should
not normally invoke `build.py` by hand.

---

## When to use it (and when not to)

**Good fit**
- You have a clear topic and need a structurally sound, well-cited draft.
- You are comparing AI-assisted vs manual writing.
- You want enforced citation discipline rather than hopeful prompting.

**Poor fit**
- You want the *learning process* of writing your own first draft.
- The topic is too novel for academic search to return useful sources.
- Qualitative / humanities framing where CARS does not apply.
- Pure brainstorming, where structure gets in the way.

**Known risks**
- *Hallucination* — heavily mitigated by the citation-first pipeline, but not
  zero. Always spot-check the reference table.
- *Over-reliance* — the skill optimises the artefact, not your skill as a
  writer.
- *Prompt sensitivity* — different runs produce different outputs.
- *Context pressure* in Full mode for very long literature folders.

---

## Constraints the skill enforces

**Never**: invent a citation (uses `[CITATION NEEDED]` instead), display CARS
labels in output, cite outside the reference table, fabricate results, mention
data feasibility constraints in the prose, use text-based arrows for
flowcharts or plain text for equations, use em-dashes.

**Always**: check for a title at invocation, build and present the reference
table before drafting, include at least one citation per paragraph, run the
Anderson checklist on Intro/ExecSum, cross-check citations after drafting,
generate `references.bib` alongside `.tex`, and ask (never assume) about the
Methodology Annex.

---

## Author

Built by [@rubilamb](https://github.com/rubilamb).
