# Logical-Words-in-Child-Language
Looking at morphemes corresponding to logical concepts in Spanish and Mandarin.

## Mandarin 
This is the Mandarin section of the Logical Words in Child Language project, which examines morphemes corresponding to logical concepts, including negation, conjunction, disjunction, and conditionals in Mandarin.

For Mandarin, the analysis tracks the developmental onset of 12 target function words in spontaneous child speech, drawing on 20 corpora from the CHILDES database. 

### Function Words Analyzed
| Category | Mandarin Forms | Description of Function | Representative Research |
|---|---|---|---|
| Negation | 不 *bù*, 没 *méi*, 没有 *méiyǒu*, 别 *bié* | Negates actions, states, possession, completion, or commands. | Lin (2003); Ernst (1995); Po-Lun & Pan (2001) |
| Conjunction | 和 *hé*, 跟 *gēn*, 而且 *érqiě* | Coordinates nouns, people, phrases, or adds related information. | Li & Thompson (1981); Xing (2001); Lü (1999) |
| Disjunction | 或者 *huòzhě*, 还是 *háishi* | Expresses alternatives; 或者 is mainly used in statements, while 还是 is mainly used in questions. | MY Erlewine (2025) |
| Conditional | 如果 *rúguǒ*, 要是 *yàoshi*, 的话 *dehuà* | Marks factual, hypothetical, or spoken conditional clauses. | Wang (1985); Liu et al. (2001); Chao (1968) |

### Example Sentences
| Category | Mandarin | Translation |
|---|---|---|
| Negation | *妈妈给宝宝讲故事好不好？不要。* | “How about Mom tells Baby a story? Don’t want to.” |
| Conjunction | *妈妈和老鼠。* | “Mom and the mouse.” |
| Disjunction | *是开心还是害怕？* | “Is he/she happy or scared?” |
| Conditional | *如果下雨天……* | “If it’s a rainy day …” |

Ambiguous items are disambiguated by context: **还是** is counted only in interrogative utterances, in the “or” sense, and **跟** is counted only when flanked by nominal or pronominal POS tags on both sides, in the “and” sense.

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
* For rendering Chinese characters in PDF output: **XeLaTeX** and a Simplified Chinese font, such as PingFang SC on macOS
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
3. Open the project in RStudio, or your preferred R environment, with the working directory set to the `mandarin/` folder
4. The pipeline will create `mandarin_data/` and `mandarin_model/` subdirectories on first run

#### Usage
Knit the two R Markdown files in order:
 
1. **`logicword_extraction_mandarin.Rmd`** pulls tokens from the 20 Mandarin CHILDES corpora via `childesr`, applies age and speaker filtering, tags target function words with context based disambiguation for 还是 and 跟, and saves a processed token file. The CHILDES download runs on first knit only; subsequent knits load the cached feather file.
2. **`MandarinPlotsAndCurves.Rmd`** computes cumulative relative frequencies per age and speaker, then fits a Bayesian Gompertz lag model in `brms` for each word's cumulative frequency in child speech.


## Spanish

This is the Spanish section of the Logical Words in Child Language project, which examines morphemes corresponding to logical concepts, including negation, conjunction, disjunction, and affirmation in Spanish.

For Spanish, the analysis tracks the developmental emergence of high-frequency target function words in spontaneous child speech, with attention to their syntax, semantics, and use in discourse.

### Function Words Analyzed
| Category | Spanish Forms | Description of Function | Representative Research |
|---|---|---|---|
| Negation | no, nunca, nadie, nada, ningún | Negates propositions, actions, or existential reference. | Richard Zanuttini (1997); Dale April Koike |
| Conjunction | y, pero, sino | Coordinates clauses or phrases; additive or adversative relations. | Bosque & Demonte (1999); Luis López (2009) |
| Disjunction | o, u | Expresses logical alternatives or choices. | Paul Portner (2009) |
| Affirmation | sí | Marks affirmation or positive response. | Bosque & Demonte (1999) |

### Example Sentences
| Category | Spanish | Translation |
|---|---|---|
| Negation | *No quiero leche.* | “I do not want milk.” |
| Conjunction | *María y Juan juegan.* | “María and Juan are playing.” |
| Disjunction | *¿Quieres té o café?* | “Do you want tea or coffee?” |
| Affirmation | *Sí, quiero más.* | “Yes, I want more.” |

Ambiguous items should be handled carefully during preprocessing. In particular, **sí** means “yes” and functions as an affirmative polarity marker, while **si** means “if” and functions as a conditional marker. Because CHILDES transcripts may omit accents inconsistently, Spanish preprocessing may require normalization or manual checking to distinguish these forms.

### Data source
All data come from **[CHILDES](https://talkbank.org/childes/)** (Child Language Data Exchange System), the child-language component of the [TalkBank](https://talkbank.org/) system, maintained by Brian MacWhinney at Carnegie Mellon and supported by NICHD grant HD082736.

The Spanish pipeline pulls tokens from Spanish CHILDES corpora via the [`childesr`](https://github.com/langcog/childesr) R package, which queries the [CHILDES database backend](https://sla.talkbank.org/TBB/childes).

### Getting Started

#### Prerequisites

* **R** (≥ 4.2)
* A working **C++ toolchain and Stan** (required by `brms`); see the [brms installation guide](https://paul-buerkner.github.io/brms/)
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
2. Navigate to the Spanish section
   ```sh
   cd Logical-Words-in-Child-Language/spanish
   ```
3. Open the project in RStudio, or your preferred R environment, with the working directory set to the `spanish/` folder
4. The pipeline will create `spanish_data/` and `spanish_model/` subdirectories on first run

#### Usage
Knit the two R Markdown files in order:

1. **`logicword_extraction_spanish.Rmd`** pulls tokens from Spanish CHILDES corpora via `childesr`, applies age and speaker filtering, tags target function words, and saves a processed token file. The CHILDES download runs on first knit only; subsequent knits load the cached feather file.
2. **`SpanishPlotsAndCurves.Rmd`** computes cumulative relative frequencies per age and speaker, then fits a growth curve model for each word's cumulative frequency in child speech.
