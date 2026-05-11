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
