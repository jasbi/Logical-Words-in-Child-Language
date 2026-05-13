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
