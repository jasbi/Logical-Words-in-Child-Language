# Logical Words in Child Language

A cross-linguistic corpus study of when children start producing logical function words, currently focused on **negation**. Using child–parent transcripts from CHILDES and Bayesian Gompertz growth-curve models, the project estimates a population-level **onset of production** for each negator. It also compares **sentential** and **discourse** negation across six languages.

| Language | Target negators | Folder | Status |
|---|---|---|---|
| Mandarin | 不 *bù*, 没 *méi* | `mandarin/` | Revised pipeline |
| English | *no*, *not* / *n't* | `english/` | Revised pipeline |
| French | *non*, *pas* | `french/` | Revised pipeline |
| Japanese | *nai*, *iya* | `japanese/` | Revised pipeline |
| Spanish | *no* | `spanish/` | Coding rules drafted; code still uses the earlier template |
| German | *nein*, *nicht*, *kein* | — | Coding rules drafted; pipeline not yet added |

---

## Repository layout

```
Logical-Words-in-Child-Language/
├── mandarin/     Mandarin_functionword_extraction.Rmd → Mandarin_Negation_PlotsAndCurves.Rmd
├── english/      English_functionword_extraction.Rmd  → English_Negation_PlotsAndCurves.Rmd
├── french/       French_functionword_extraction.Rmd   → French_Negation_PlotsAndCurves.Rmd
├── japanese/     Japanese_functionword_extraction.Rmd → Japanese_Negation_PlotsAndCurves.Rmd
├── spanish/      earlier-template extraction and plotting files
├── summary/      Combined_Negation_Curves_and_Onsets.Rmd (all languages on one set of figures)
├── bucld/        BUCLD 51 and LSA 2025 abstracts and their figure data
└── childes_logicwords.Rproj
```

Each language pipeline writes to its own folder:

```
<language>/
├── <language>_data/raw_data/          cached CHILDES downloads, exclusion records
├── <language>_data/processed_data/    processed tokens (.feather), rate tables and fitted curves (.RDS)
└── <language>_model/modeling/         cached brms model fits
```

These folders are created on the first run.

---

## Method

**Data.** Word tokens come from [CHILDES](https://talkbank.org/childes/) via the [`childesr`](https://github.com/langcog/childesr) R package, pinned to database release **2026.1**:

- English: Eng-NA and Eng-UK collections
- French: French collection
- Japanese: Japanese collection
- Mandarin: 20 corpora (AcadLang, BJCMC, Chang1, Chang2, ChangPlay, ChangPN, Erbaugh, LiReading, LiZhou, NSCtoys, TCCM, TCCM-Reading, Tong, Xinjiang, Zhou1, Zhou2, Zhou3, ZhouAssessment, ZhouDinner, ZhouNarratives)

**Exclusions.** Tokens are kept if the target child's age is known and between 0 and 72 months. Each extraction file saves a summary of every exclusion step.

**Speakers.** The target child is coded as *child*. All other speakers are grouped as *parents*.

**Counting.** Counts are at the token level. For each speaker group and month, the cumulative rate is the running count of a negator per 1,000 tokens that group has produced up to that month. Child months 0–10 are fixed at zero as a baseline.

**Growth curves.** A three-parameter Gompertz curve is fit to each child trajectory in `brms`:

```
y = A · exp( −exp( (μ·e / A) · (λ − age) + 1 ) )
```

- **A** is the upper asymptote.
- **μ** is the maximum growth rate.
- **λ** is the lag.

All three parameters have uniform priors, with λ bounded to 10–72 months. **λ is the onset estimate.** It's a curve parameter, not the age of a child's first recorded use.

**Scope.** Each negator token is also labeled **sentential** (inside a clause), **discourse** (a standalone response) or **mixed** (a discourse negator followed by a sentential one, e.g. *No, I don't want it*). Separate curves and onsets are fit for each word-by-scope combination.

---

## Getting started

### Prerequisites
- **R** ≥ 4.2
- A C++ toolchain and **Stan** for `brms` (see the [brms installation guide](https://paul-buerkner.github.io/brms/))
- **XeLaTeX** and a Simplified Chinese font (e.g. PingFang SC) to knit the Mandarin and summary PDFs
- R packages:
  ```r
  install.packages(c("childesr", "tidyverse", "arrow", "brms",
                     "ggeffects", "ggrepel", "gridExtra", "stringi"))
  ```

### Install
```sh
git clone https://github.com/jasbi/Logical-Words-in-Child-Language.git
cd Logical-Words-in-Child-Language
```
Open `childes_logicwords.Rproj` in RStudio.

### Run one language
Set the working directory to that language's folder, then knit two files in order:

1. **`<Language>_functionword_extraction.Rmd`**
   - Downloads the corpora one file per corpus, caching each and reusing it on later runs.
   - Applies the exclusions, assigns speaker roles and age in months, and codes each negator and its scope.
   - Saves the result to `<language>_data/processed_data/<language>_tokens_processed.feather`.
   - To force a fresh download, set `<language>_force_download <- TRUE`.
2. **`<Language>_Negation_PlotsAndCurves.Rmd`**
   - Computes the cumulative rates and fits growth curves for overall negation, each negator, and each word × scope.
   - Saves fitted curves and onset tables to `processed_data/`.
   - Fitted models are cached in `<language>_model/modeling/` and refit only when the data or model change.

> **Japanese** also needs `japanese/japanese_neg_lexicon.csv` (columns `form`, `target`, `form_type`) before extraction.

### Combine languages
After running the language pipelines, knit **`summary/Combined_Negation_Curves_and_Onsets.Rmd`**. It reads three files from each language's `processed_data/` folder and refits nothing:

- `<language>_sentential_negation_childdata.RDS`
- `<language>_discourse_negation_childdata.RDS`
- `<language>_negation_scope_age_of_production.RDS`

It draws three figures:

1. All sentential growth curves
2. All discourse growth curves
3. Onset estimates with 95% credible intervals

Two settings at the top of the file:

- `allow_missing_languages`: leave it `FALSE` to stop when a language's files are missing, or set it to `TRUE` to skip that language with a warning. Spanish and German don't produce these files yet, so set it to `TRUE` for now.
- `normalize_curves`: set to `TRUE` to scale each curve to 0–1 so growth timing is easier to compare.

---


## Data and software credits
- MacWhinney, B. (2000). *The CHILDES Project: Tools for Analyzing Talk*. CHILDES is part of [TalkBank](https://talkbank.org/), supported by NICHD grant HD082736.
