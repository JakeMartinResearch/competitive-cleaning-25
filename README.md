# Competitive cleaning

This repository contains the data and R Markdown analysis for the manuscript:

> Brooker et al. (2025) Competitive cleaning: behavioural variation supports coexistence of two juvenile sympatric cleanerfishes. https://doi.org/10.1098/rsbl.2025.0079 

The analysis investigates how interspecific competition affects client-directed behaviour in two juvenile cleaner wrasses, the bluestreak cleanerfish (*Labroides dimidiatus*) and bicolor cleanerfish (*Labroides bicolor*). Trials compared cleaner responses to a model client fish in competitive cleaner-cleaner pairings and non-competitive cleaner-coris pairings.

## Study overview

The behavioural experiment was conducted in November 2019 at the Centre de Recherches Insulaires et Observatoire de l'Environnement (CRIOBE), Moorea, French Polynesia. Juvenile bluestreak cleanerfish, bicolor cleanerfish, and clown coris (*Coris aygula*) were collected from shallow reef sites around Moorea. Striated surgeonfish (*Ctenochaetus striatus*) were used as model clients.

Trials consisted of paired juvenile fishes exposed to a bagged client fish for 5 min after acclimation. Videos were scored in BORIS for:

- number of interactions with the client;
- time spent interacting with the client;
- time spent in front, middle, back, approach, and pipe zones;
- aggressive behaviours, including chases and bites.

The main derived response variable is a client association score:

```text
association score = (front-zone proportion * 1) +
                    (middle-zone proportion * 0) +
                    (back-zone proportion * -1)
```

Higher values indicate greater association with the client fish.

## Repository structure

```text
.
|-- competitive-cleaning-r.Rmd   # Main analysis notebook
|-- index.html                   # Rendered HTML analysis report
|-- input-data/
|   `-- cleaner-fish-data.xlsx   # Behavioural trial data
|-- output-fig/
|   |-- fig_1.pdf                # Client interaction count figure
|   `-- fig_2.pdf                # Client association score figure
|-- index_files/                 # Supporting files for rendered HTML
`-- competitive-cleaning-r_cache/ # Cached R Markdown objects
```

## Data

The input workbook `input-data/cleaner-fish-data.xlsx` contains 60 rows and 49 columns. Each row is a focal-fish observation from a paired trial. There are 30 paired trials in total, with two focal-fish observations per trial.

The workbook includes three pair treatments:

- `Bicolor_Bluestreak`
- `Bicolor_Coris`
- `Bluestreak_Coris`

The focal species represented in the data are `Bicolor`, `Bluestreak`, and `Coris`. The analysis focuses primarily on the cleaner species comparisons and excludes coris focal-fish estimates from the manuscript figures.

Key columns include:

- `trial_id`, `treatment`, `comp`, `species`
- `interaction_n`, `interaction_s`
- zone-use variables for `front`, `middle`, `back`, `approach`, and `pipe`
- aggressive behaviour counts: `chase_n`, `bite_n`, `cleaning bite_n`

## Analysis workflow

The main notebook, `competitive-cleaning-r.Rmd`, performs the following steps:

1. Loads required packages and project directories.
2. Imports `cleaner-fish-data.xlsx`.
3. Tidies treatment, competitor, species, and trial identifiers.
4. Derives total back-zone and front-zone use, zone proportions, total zone transitions, and client association score.
5. Visualises raw interaction and association-score distributions.
6. Models the number of client interactions using count models, including Poisson, zero-inflated Poisson, zero-inflated negative binomial, and hurdle alternatives.
7. Uses a zero-inflated Poisson generalized linear mixed model for the reported client-interaction analysis.
8. Models client association score using a linear mixed model.
9. Runs planned contrasts between cleaner species and competition scenarios using `emmeans`.
10. Saves manuscript-ready figures to `output-fig/`.

## Required R packages

The analysis uses the following R packages:

```r
c(
  "ggplot2", "ggthemes", "ggfortify", "ggridges", "gghalves",
  "ggExtra", "plotly", "colorspace", "ggrepel", "ggdist", "gt",
  "tinytex", "data.table", "stringr", "tidyverse", "janitor",
  "readxl", "broom", "lme4", "glmmTMB", "car", "emmeans",
  "bbmle", "magrittr", "lmerTest", "rptR", "RNOmni",
  "AICcmodavg", "performance"
)
```

The notebook currently loads packages with `pacman::p_load()`, so `pacman` is also required. The analysis notes that `TMB` and `glmmTMB` may need to be installed from source if binary installation causes compatibility problems.

## Reproducing the analysis

Open the project file `EXP-2025-competitive-cleaning.Rproj` in RStudio, then render the main notebook:

```r
rmarkdown::render("competitive-cleaning-r.Rmd")
```

The custom knit setting writes the rendered report to:

```text
index.html
```

The manuscript figures are written to:

```text
output-fig/fig_1.pdf
output-fig/fig_2.pdf
```

## Outputs

- `index.html`: complete rendered analysis report.
- `output-fig/fig_1.pdf`: number of interactions with the client fish across cleaner species and competition scenario.
- `output-fig/fig_2.pdf`: client association score across cleaner species and competition scenario.

## License and citation

The analysis notebook lists the data licence as CC0.

The associated publication link, DOI, and recommended citation are still marked as pending in the analysis notebook. Once available, add the final publication details here and in `competitive-cleaning-r.Rmd`.

## Contact

Jake M. Martin  
Email: [jake.martin@deakin.edu.au](mailto:jake.martin@deakin.edu.au)  
Alt email: [jake.martin.research@gmail.com](mailto:jake.martin.research@gmail.com)  
Website: [jakemartin.org](https://jakemartin.org/)  
GitHub: [JakeMartinResearch](https://github.com/JakeMartinResearch)
