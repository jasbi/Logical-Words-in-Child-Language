# Logical-Words-in-Child-Language
Looking at morphemes corresponding to logical concepts in English, Spanish, Mandarin,

## Mandarin 
This is the Mandarin section of the Logical Words in Child Language project, which examines morphemes corresponding to logical concepts, including negation, conjunction, disjunction, and conditionals in Mandarin.

For Mandarin, the analysis tracks the developmental onset of 12 target function words in spontaneous child speech, drawing on 20 corpora from the CHILDES database. 

### Function Words Analyzed
| Category | Words |
|---|---|
| Negation | 不 *bù*, 没 *méi*, 没有 *méiyǒu*, 别 *bié* |
| Conjunction | 和 *hé*, 跟 *gēn*, 而且 *érqiě* |
| Disjunction | 或者 *huòzhě*, 还是 *háishi* |
| Conditional | 如果 *rúguǒ*, 要是 *yàoshi*, 的话 *dehuà* |

Ambiguous items are disambiguated by context: **还是** is counted only in interrogative utterances (the "or" sense), and **跟** is counted only when flanked by nominal or pronominal POS tags on both sides (the "and" sense).

### Data source
All data come from **[CHILDES](https://talkbank.org/childes/)** (Child Language Data Exchange System), the child-language component of the [TalkBank](https://talkbank.org/) system, maintained by Brian MacWhinney at Carnegie Mellon and supported by NICHD grant HD082736.

The Mandarin pipeline pulls tokens from 20 Mandarin corpora hosted on CHILDES via the [`childesr`](https://github.com/langcog/childesr) R package, which queries the [CHILDES database backend](https://sla.talkbank.org/TBB/childes):
 
```
AcadLang, BJCMC, Chang1, Chang2, ChangPlay, ChangPN, Erbaugh,
LiReading, LiZhou, NSCtoys, TCCM, TCCM-Reading, Tong, Xinjiang,
Zhou1, Zhou2, Zhou3, ZhouAssessment, ZhouDinner, ZhouNarratives
```

### Getting Started 

#### Prerequisites
 
* **R** (≥ 4.2)
* A working **C++ toolchain and Stan** (required by `brms`); see the [brms installation guide](https://paul-buerkner.github.io/brms/)
* For rendering Chinese characters in PDF output: **XeLaTeX** and a Simplified Chinese font (e.g. PingFang SC on macOS)
* Required R packages:
  ```r
  install.packages(c("childesr", "tidyverse", "arrow", "brms",
                     "ggeffects", "ggrepel", "gridExtra"))
  ```

#### Installation
1. Clone the main repo
   ```sh
   git clone https://github.com/github_username/Logical-Words-in-Child-Language.git
   ```
2. Navigate to the Mandarin section
   ```sh
   cd Logical-Words-in-Child-Language/mandarin
   ```
3. Open the project in RStudio (or your preferred R environment) with the working directory set to the `mandarin/` folder
4. The pipeline will create `mandarin_data/` and `mandarin_model/` subdirectories on first run

#### Usage
Knit the two R Markdown files in order:
 
1. **`logicword_extraction_mandarin.Rmd`** — pulls tokens from the 20 Mandarin CHILDES corpora via `childesr`, applies age and speaker filtering, tags target function words (with context-based disambiguation for 还是 and 跟), and saves a processed token file. The CHILDES download runs on first knit only; subsequent knits load the cached feather file.
2. **`MandarinPlotsAndCurves.Rmd`** — computes cumulative relative frequencies per age and speaker, then fits a Bayesian Gompertz lag model in `brms` for each word's cumulative frequency in child speech


## Limitation

## License

## Contact

## Acknowledgments
# Logical Words in Child Language: Spanish

This section of the Logical Words in Child Language project examines morphemes corresponding to core logical concepts in Spanish, including negation, conjunction, disjunction, and affirmation.

The analysis focuses on high-frequency Spanish function words that encode logical relations in spontaneous child speech, with particular attention to their syntax, semantics, and developmental emergence.

---

# Overview of Logical Morphemes in Spanish

In Spanish, logical relations are primarily expressed through closed-class function words:

- Negation is typically marked with **no**, which precedes the conjugated verb.
- Conjunction links words, phrases, or clauses through forms such as **y** (“and”).
- Disjunction expresses alternatives through **o** (“or”).
- Affirmation is expressed through **sí** (“yes”), which functions as an affirmative polarity marker or discourse response particle.

Spanish also includes more complex logical constructions such as **ni…ni** (“neither…nor”) and contrastive conjunctions like **pero** (“but”) and **sino** (“rather/instead”).

---

# Function Words Analyzed

| Category | Spanish Forms | Description of Function | Representative Research |
|---|---|---|---|
| Negation | no, nunca, nadie, nada, ningún | Negates propositions, actions, or existential reference. | Richard Zanuttini (1997); Dale April Koike |
| Conjunction | y, pero, sino | Coordinates clauses or phrases; additive or adversative relations. | Bosque & Demonte (1999); Luis López (2009) |
| Disjunction | o, u | Expresses logical alternatives or choices. | Paul Portner (2009) |
| Affirmation | sí | Marks affirmation or positive response. | Bosque & Demonte (1999) |

---

# Important Distinction

| Form | Meaning | Function |
|---|---|---|
| sí | “yes” | affirmation |
| si | “if” | conditional |

Because CHILDES transcripts may omit accents inconsistently, preprocessing may require normalization to distinguish these forms carefully.

---

# Example Sentences

| Category | Spanish | Translation |
|---|---|---|
| Negation | *No quiero leche.* | “I do not want milk.” |
| Conjunction | *María y Juan juegan.* | “María and Juan are playing.” |
| Disjunction | *¿Quieres té o café?* | “Do you want tea or coffee?” |
| Affirmation | *Sí, quiero más.* | “Yes, I want more.” |

---

# Spanish Extraction Pipeline

## Target Logical Words

The extraction script should identify and tag logical morphemes during preprocessing.

### Suggested Target Forms

```r
logical_words <- c(
  "no", "nunca", "nadie", "nada", "ningún",
  "y", "pero", "sino",
  "o", "u",
  "sí"
)
```

---

# Extraction File Modifications

Initialize all tokens as nonlogical by default:

```r
Spanish_tokens$word <- "nonlogical"
```

Then assign logical categories by matching glosses:

```r
for (x in logical_words) {
  Spanish_tokens$word[Spanish_tokens$gloss == x] <- x
}
```

Optional normalization for inconsistent accent encoding:

```r
Spanish_tokens$gloss <- str_trim(tolower(Spanish_tokens$gloss))
```

Create higher-level semantic categories:

```r
Spanish_tokens$category <- "other"

Spanish_tokens$category[
  Spanish_tokens$word %in%
    c("no", "nunca", "nadie", "nada", "ningún")
] <- "negation"

Spanish_tokens$category[
  Spanish_tokens$word %in%
    c("y", "pero", "sino")
] <- "conjunction"

Spanish_tokens$category[
  Spanish_tokens$word %in%
    c("o", "u")
] <- "disjunction"

Spanish_tokens$category[
  Spanish_tokens$word %in%
    c("sí")
] <- "affirmation"
```

---

# Spanish Plotting Pipeline

The second R Markdown file (`SpanishPlotsAndCurves.Rmd`) should:

- Load the processed token dataframe
- Filter logical function words
- Compute frequencies by child age
- Generate developmental plots
- Fit growth curves (e.g., Gompertz or logistic models)

---

# Example Plotting Workflow

## Load Processed Data

```r
library(tidyverse)
library(arrow)

Spanish_tokens <- read_feather(
  "spanish_data/SpanishLogicalWords.feather"
)
```

---

## Filter Logical Words

```r
Spanish_logic <- Spanish_tokens %>%
  filter(word != "nonlogical")
```

---

## Compute Frequencies by Age

```r
Spanish_freq <- Spanish_logic %>%
  group_by(target_child_name, age, word, category) %>%
  summarise(
    n = n(),
    .groups = "drop"
  )
```

---

## Individual Word Plots

Example for **no**:

```r
ggplot(
  Spanish_freq %>% filter(word == "no"),
  aes(x = age, y = n)
) +
  geom_point(alpha = .4) +
  geom_smooth(method = "loess") +
  labs(
    title = "Development of 'no'",
    x = "Age",
    y = "Frequency"
  ) +
  theme_minimal()
```

Example for **sí**:

```r
ggplot(
  Spanish_freq %>% filter(word == "sí"),
  aes(x = age, y = n)
) +
  geom_point(alpha = .4) +
  geom_smooth(method = "loess") +
  labs(
    title = "Development of 'sí'",
    x = "Age",
    y = "Frequency"
  ) +
  theme_minimal()
```

---

## Faceted Plots Across Categories

```r
ggplot(
  Spanish_freq,
  aes(x = age, y = n, color = word)
) +
  geom_smooth(se = FALSE) +
  facet_wrap(~category, scales = "free_y") +
  theme_minimal() +
  labs(
    title = "Logical Function Words Across Development",
    x = "Age",
    y = "Frequency"
  )
```

---

# Suggested Research Goals

The Spanish pipeline can investigate:

- Developmental onset of logical operators
- Frequency growth across age
- Relative acquisition order of negation vs conjunction vs affirmation
- Differences between semantic and syntactic complexity
- Cross-linguistic comparison with Mandarin and English

---

# Linguistic Notes on sí

| Topic | Description |
|---|---|
| Affirmation | sí functions as an affirmative polarity marker and discourse response particle |
| Polarity semantics | Positive vs negative polarity systems in discourse |
| Syntax/pragmatics | sí can appear as emphatic affirmation (“Sí quiero”) or discourse response |

---
