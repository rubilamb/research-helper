# Methodology Annex Guide
# Load when user requests a methodology annex for Introduction or Executive Summary.

This annex is a companion document appended after the main section and
bibliography. It advises on data methodology, equations, and datasets.
It is NOT part of the Introduction or Executive Summary text itself.

**This annex is optional.** Only generate it if the user explicitly
requests it (question 8 in the input phase). If the user already has
a methodology, incorporate theirs and only add dataset mapping.

---

## Purpose

The Methodology Annex serves three functions:
1. Visualise the data methodology pipeline with a proper diagram
2. Specify the key equations and models with LaTeX formatting
3. Identify the datasets required, with verified accessibility

---

## Section A: Methodology Flowchart

### What the flowchart shows
The flowchart depicts the DATA METHODOLOGY pipeline, not the research
process. It shows how data flows through the analysis.

**For economics and finance fields (econometrics)**, the pipeline typically includes:
Data Collection, Data Cleaning and Variable Construction, Descriptive
Statistics, Main Model Estimation (e.g., OLS, FE, IV, DID, RDD),
Robustness Checks, Heterogeneity Analysis, Sensitivity Analysis.

**For economics and finance fields (machine learning)**, the pipeline typically includes:
Data Collection, Feature Engineering, Train/Test Split, Model Selection
and Training, Hyperparameter Tuning, Cross-Validation, Model Evaluation,
Interpretation (SHAP, feature importance).

**For other fields**, adapt the pipeline to the discipline's standard
methodology. The key principle remains: show the data analysis flow,
not the research planning process.

### Diagram format (LaTeX/PDF output)

ALWAYS use tikz with proper styling. Never use text-based arrows.
The diagram must look professional with coloured boxes, rounded corners,
and clear directional arrows.

```latex
\usepackage{tikz}
\usetikzlibrary{shapes.geometric, arrows.meta, positioning, calc}

\begin{figure}[H]
\centering
\begin{tikzpicture}[
    node distance=1.5cm and 2.5cm,
    mainbox/.style={
        rectangle, draw=black!70, fill=blue!8,
        rounded corners=4pt, minimum width=5cm,
        minimum height=1cm, align=center,
        font=\small\sffamily, line width=0.8pt
    },
    databox/.style={
        rectangle, draw=black!70, fill=green!8,
        rounded corners=4pt, minimum width=5cm,
        minimum height=1cm, align=center,
        font=\small\sffamily, line width=0.8pt
    },
    resultbox/.style={
        rectangle, draw=black!70, fill=orange!10,
        rounded corners=4pt, minimum width=5cm,
        minimum height=1cm, align=center,
        font=\small\sffamily, line width=0.8pt
    },
    arrow/.style={
        -{Stealth[length=3mm, width=2mm]},
        thick, draw=black!60
    },
    sidelabel/.style={
        font=\footnotesize\itshape, text=gray
    }
]

% Econometric pipeline example
\node[databox] (data) {Data Collection\\{\footnotesize Source: [Dataset Name]}};
\node[mainbox, below=of data] (clean) {Data Cleaning \& Variable Construction\\{\footnotesize Missing values, outliers, transformations}};
\node[mainbox, below=of clean] (desc) {Descriptive Statistics\\{\footnotesize Summary tables, correlation matrix}};
\node[mainbox, below=of desc] (main) {Main Estimation: [Model Name]\\{\footnotesize Equation (1): $Y_{it} = \alpha + \beta X_{it} + \varepsilon_{it}$}};
\node[mainbox, below=of main] (robust) {Robustness Checks\\{\footnotesize Alternative specs, subsample analysis}};
\node[resultbox, below=of robust] (results) {Results \& Interpretation};

\draw[arrow] (data) -- (clean);
\draw[arrow] (clean) -- (desc);
\draw[arrow] (desc) -- (main);
\draw[arrow] (main) -- (robust);
\draw[arrow] (robust) -- (results);

% Side annotations
\node[sidelabel, right=1cm of data] {Eq. (1) variables};
\node[sidelabel, right=1cm of main] {See Section B};

\end{tikzpicture}
\caption{Econometric methodology pipeline}
\label{fig:methodology}
\end{figure}
```

### Diagram format (Word output)

For Word output, the LaTeX tikz diagram cannot render directly. Instead:
1. Generate the tikz diagram as part of the .tex file
2. Compile to PDF first, then convert
3. If Word-only output is requested, provide a detailed structured
   description that the user can recreate in Word using SmartArt or
   a drawing tool. Format as a numbered list with clear hierarchy:

> **Methodology Pipeline:**
> 1. **Data Collection** (green box): [Dataset Name], [variables]
> 2. **Data Cleaning** (blue box): Missing values, outliers, transformations
> 3. **Descriptive Statistics** (blue box): Summary tables, correlations
> 4. **Main Estimation** (blue box): [Model], Equation (1)
> 5. **Robustness Checks** (blue box): Alternative specifications
> 6. **Results** (orange box): Interpretation of coefficients

### ML pipeline example (tikz)

```latex
\begin{figure}[H]
\centering
\begin{tikzpicture}[
    node distance=1.2cm,
    mainbox/.style={rectangle, draw=black!70, fill=blue!8, rounded corners=4pt,
        minimum width=5cm, minimum height=0.9cm, align=center,
        font=\small\sffamily, line width=0.8pt},
    databox/.style={rectangle, draw=black!70, fill=green!8, rounded corners=4pt,
        minimum width=5cm, minimum height=0.9cm, align=center,
        font=\small\sffamily, line width=0.8pt},
    evalbox/.style={rectangle, draw=black!70, fill=orange!10, rounded corners=4pt,
        minimum width=5cm, minimum height=0.9cm, align=center,
        font=\small\sffamily, line width=0.8pt},
    arrow/.style={-{Stealth[length=3mm, width=2mm]}, thick, draw=black!60}
]
\node[databox] (raw) {Raw Data Collection};
\node[mainbox, below=of raw] (feat) {Feature Engineering};
\node[mainbox, below=of feat] (split) {Train / Validation / Test Split};
\node[mainbox, below=of split] (model) {Model Training\\{\footnotesize Random Forest, XGBoost, Neural Net}};
\node[mainbox, below=of model] (tune) {Hyperparameter Tuning\\{\footnotesize Grid search, Bayesian optimisation}};
\node[mainbox, below=of tune] (cv) {Cross-Validation (k-fold)};
\node[evalbox, below=of cv] (eval) {Evaluation\\{\footnotesize RMSE, MAE, $R^2$, AUC}};
\node[evalbox, below=of eval] (interp) {Interpretation\\{\footnotesize SHAP values, feature importance}};

\draw[arrow] (raw) -- (feat);
\draw[arrow] (feat) -- (split);
\draw[arrow] (split) -- (model);
\draw[arrow] (model) -- (tune);
\draw[arrow] (tune) -- (cv);
\draw[arrow] (cv) -- (eval);
\draw[arrow] (eval) -- (interp);
\end{tikzpicture}
\caption{Machine learning methodology pipeline}
\label{fig:ml_methodology}
\end{figure}
```

---

## Section B: Equations and Models

### Format requirements
ALL equations MUST be written in proper LaTeX format. Never write
equations as plain text (e.g., "Y = a + bX + e" is NOT acceptable).

**For LaTeX/PDF output:**
Use `\begin{equation}` for single equations or `\begin{align}` for
systems of equations. Number all equations.

**For Word output:**
Pandoc converts LaTeX equations to Word's OMML equation format
(native Word equation editor with proper symbols, subscripts, and
superscripts). If this conversion fails, include the raw LaTeX source
and instruct the user: "Paste the following into Word's equation editor
(Insert > Equation) for proper rendering."

### Econometric equation examples

**Panel fixed effects:**
```latex
\begin{equation}
Y_{it} = \alpha + \beta X_{it} + \gamma \mathbf{Z}_{it} + \delta_i + \theta_t + \varepsilon_{it}
\label{eq:panel_fe}
\end{equation}
```

**Difference-in-differences:**
```latex
\begin{equation}
Y_{it} = \alpha + \beta_1 \text{Treat}_i + \beta_2 \text{Post}_t + \beta_3 (\text{Treat}_i \times \text{Post}_t) + \gamma \mathbf{Z}_{it} + \varepsilon_{it}
\label{eq:did}
\end{equation}
```

**Instrumental variables (2SLS):**
```latex
\begin{align}
\text{First stage:} \quad & X_{it} = \pi_0 + \pi_1 Z_{it} + \mathbf{W}_{it}\boldsymbol{\pi}_2 + \nu_{it} \label{eq:iv_first} \\
\text{Second stage:} \quad & Y_{it} = \beta_0 + \beta_1 \hat{X}_{it} + \mathbf{W}_{it}\boldsymbol{\beta}_2 + \varepsilon_{it} \label{eq:iv_second}
\end{align}
```

**Regression discontinuity:**
```latex
\begin{equation}
Y_i = \alpha + \beta D_i + f(X_i - c) + \gamma D_i \cdot f(X_i - c) + \varepsilon_i
\label{eq:rdd}
\end{equation}
```

### Variable definitions
After EVERY equation, define all variables:

"where $Y_{it}$ is [outcome] for unit $i$ in period $t$, $X_{it}$ is
[variable of interest], $\mathbf{Z}_{it}$ is a vector of controls,
$\delta_i$ are unit fixed effects, $\theta_t$ are time fixed effects,
and $\varepsilon_{it}$ is the idiosyncratic error term."

### Citation rules for equations
- Cite the paper that introduced or popularised each method
- If using an established identification strategy, cite the originator
- If adapting a method from another context, cite both the original
  and the adaptation

---

## Section C: Dataset Requirements

### Dataset table format

| Dataset | Source | Variables Needed | Period | Frequency | Access Level | URL or DOI |
|---------|--------|-----------------|--------|-----------|-------------|------------|

### Access Level definitions
- **Open**: Freely downloadable, no registration needed
  (e.g., World Bank Open Data, FRED, Eurostat, UN Comtrade)
- **Free Registration**: Requires free account creation
  (e.g., OECD.Stat, IMF IFS, WRDS for students with institutional access)
- **Institutional**: Available through university library or subscription
  (e.g., Bloomberg, Refinitiv, Compustat via WRDS)
- **API**: Available via free or authenticated API
  (e.g., Eurostat API, ENTSO-E Transparency Platform, Census API)
- **Restricted**: Requires application or special access
  (e.g., administrative data, confidential surveys)

### Data feasibility rules
- Prioritise Open and Free Registration datasets
- For Institutional datasets, confirm that a typical university library
  would provide access
- For Restricted datasets, flag this to the user and suggest alternatives
- NEVER recommend a dataset without verifying it covers the required
  variables and time period
- If the ideal dataset is not accessible, propose an alternative and
  explain any trade-offs

### Common open data sources by field

**Economics and Finance:**
- FRED (Federal Reserve Economic Data): macroeconomic time series
- World Bank Open Data: development indicators, cross-country
- Eurostat: EU economic and social statistics
- OECD.Stat: OECD country statistics
- IMF IFS: international financial statistics
- Penn World Table: GDP, productivity, trade
- ENTSO-E: European energy market data
- UN Comtrade: international trade flows

**Business and Management:**
- Compustat (via WRDS): firm financials
- CRSP (via WRDS): stock returns
- Bureau van Dijk / Orbis: firm-level global data
- SEC EDGAR: US company filings (free)

**Social Science:**
- IPUMS: census and survey microdata
- European Social Survey: attitudes and values
- World Values Survey: cross-national attitudes
- OECD PISA: education performance

---

## Section D: Methodology-Data Mapping

Create a table linking each methodological step to its data requirements:

| Methodology Step | Equation | Dataset(s) | Key Variables | Notes |
|-----------------|----------|------------|---------------|-------|
| Main estimation | Eq. (1) | [Dataset 1] | Y, X, Z | Panel structure required |
| Instrument | Eq. (2) | [Dataset 2] | IV variable | Must be exogenous |
| Robustness | Eq. (1) variant | [Dataset 1] | Alternative Y | Sensitivity check |

This mapping ensures:
1. Every equation has a data source
2. Every variable is obtainable
3. The research question is feasible given the data

### Feasibility check
Before finalising, verify:
- [ ] All datasets are accessible to a masters student
- [ ] Time periods overlap across datasets (if merging)
- [ ] Key variables exist in the data (not just assumed)
- [ ] Sample size is sufficient for the chosen method
- [ ] Data format is compatible (panel, cross-section, time series)

If any check fails, revise the methodology or suggest an alternative
dataset. The research question should adapt to what is feasible, not
the other way around.

---

## Formatting Rules

- Use LaTeX tikz for all flowcharts and diagrams (PDF output)
- Use structured descriptions for Word-only output
- Use LaTeX equation environments for all equations
- Include figure and table captions
- Number all equations
- Define every variable after every equation
- Cite methodological papers and data sources
- NEVER use em-dashes
- NEVER use text-based arrows for flowcharts
- NEVER write equations as plain text
