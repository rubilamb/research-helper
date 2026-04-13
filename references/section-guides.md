# Section Guides for Non-Introduction Sections

This file provides structural guidance for all paper sections OTHER than
the introduction. The introduction uses its own dedicated references
(cars-framework.md + anderson-checklist.md).

Load this file when the user requests any of the sections below.

---

## Literature Review

### Purpose
Synthesise existing research THEMATICALLY, not study-by-study. Show how
the body of work connects, conflicts, and leaves gaps.

### Structure
1. Opening paragraph: scope of the review, how it is organised
2. Theme 1: group related studies, identify consensus and conflicts
3. Theme 2: different angle or methodology stream
4. Theme 3 (if needed): emerging or contrarian perspectives
5. Summary paragraph: what the literature collectively shows and what
   remains unresolved (this feeds into the introduction's Move 2)

### Citation rules
- Every theme paragraph must cite at least 2-3 papers
- Use comparative language: "While Smith (2020) found X, Jones (2021)
  argues Y, suggesting that..."
- Never list studies without connecting them logically
- All citations must come from the reference table

### Common mistakes
- Writing a study-by-study summary ("Smith did X. Jones did Y. Lee did Z.")
- Including papers that are not relevant to the research question
- Not identifying patterns, conflicts, or gaps across studies

---

## Methodology

### Purpose
Describe what you did (or will do) with enough detail that another
researcher could replicate the study.

### Structure
1. Research design: type of study (experimental, quasi-experimental,
   observational, survey, etc.)
2. Data: source, sample, time period, variables, descriptive statistics
3. Model specification: formal equations in LaTeX
4. Identification strategy: what variation you exploit, why it is valid
5. Estimation method: OLS, IV, DID, RDD, ML model, etc.
6. Robustness checks: alternative specifications, placebo tests, etc.

### LaTeX equation format
```latex
\begin{equation}
Y_{it} = \alpha + \beta X_{it} + \gamma Z_{it} + \delta_i + \theta_t + \varepsilon_{it}
\label{eq:main}
\end{equation}
```

Define every variable after the equation:
"where $Y_{it}$ is [outcome] for unit $i$ in period $t$, $X_{it}$ is
[treatment], $Z_{it}$ is a vector of controls, $\delta_i$ and $\theta_t$
are unit and time fixed effects, and $\varepsilon_{it}$ is the error term."

### Citation rules
- Cite the methodological paper that introduces or validates your approach
- Cite the data source
- If using an identification strategy from prior work, cite it

---

## Results

### Purpose
Present findings clearly, distinguishing between main results, robustness
checks, and exploratory analysis.

### Structure
1. Main results: present the primary regression/analysis table
2. Interpretation: explain magnitude, significance, and economic meaning
3. Robustness: alternative specifications that confirm (or qualify) findings
4. Heterogeneity (if applicable): subgroup analysis
5. Limitations of findings

### Table format
Use `\begin{table}` with clear labels, notes explaining significance stars,
standard errors in parentheses, and number of observations.

### Citation rules
- Compare your results to prior findings cited in the introduction
- "Consistent with Smith (2020), we find that..."
- "In contrast to Jones (2021), our results suggest..."

---

## Conclusion

### Purpose
Summarise what was done, what was found, and why it matters. Mirror the
introduction. Do NOT introduce new information or citations.

### Structure
1. Restate the research question and key finding (1-2 sentences)
2. Summarise main results (brief, no new analysis)
3. Contributions: what does this add to the literature?
4. Policy implications (if applicable)
5. Limitations and future research directions

### Rules
- Do NOT introduce new data, analysis, or citations not in prior sections
- Do NOT repeat the abstract verbatim
- Keep it concise: 2-4 paragraphs
- End on the contribution or future direction, not a limitation

---

## Executive Summary (if requested)

### Purpose
A standalone summary of the ENTIRE paper for readers who may not read
the full document. Unlike an introduction, it includes findings.

### Structure
1. Context: why this research matters (1-2 sentences)
2. Research question and approach (1-2 sentences)
3. Key findings (2-3 sentences)
4. Implications (1-2 sentences)

### Citation rules
- Cite 2-3 foundational papers that frame the work
- Keep citations minimal; this is a summary, not a review

### Length
- Typically 150-300 words
- Must stand alone without the rest of the paper
