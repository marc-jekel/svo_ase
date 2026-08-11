# svo_ase

Analysis code and data for **"Social Value Orientation Predicts Directional Search in Allocation Decisions"** (Jekel, Lisovoj, & Fiedler), currently under revision at *Decision* (Manuscript DEC-2025-0121).

The paper reports three studies (Pilot Study, Study 1, Study 2) testing whether social value orientation (SVO) predicts the *direction* of information search in dictator-game-style allocation decisions.

## Repository structure

```
SVO_ASE_SCRIPT.Rmd   # Main analysis script (data cleaning, all reported statistics and figures)
SVO_ASE_SCRIPT.html  # Rendered/knitted output of the script above
data/                # Raw survey exports, unchanged since collection
data_clean/          # Derived, cleaned CSVs used for quick verification (see below)
figures/             # Output figures (PDF) as used in the manuscript
project.Rproj         # RStudio project file
```

### `data/`

Raw, per-study survey exports (Pilot Study, Study 1, Study 2), as originally collected. Not renamed or restructured — `SVO_ASE_SCRIPT.Rmd` reads directly from these files.

### `data_clean/`

A derived set of cleaned, consistently-named CSV files (one row per participant / per trial / per search step), documented in `data_clean/CODEBOOK.md`. These exist to make individual statistics quickly spot-checkable without re-running the full analysis script, and were independently verified against the corresponding published values in the manuscript. See the codebook for column definitions and a full missing-data breakdown.

### `SVO_ASE_SCRIPT.Rmd`

The single source of truth for all reported analyses: data cleaning, SVO angle/type classification, the ASE (Attraction Search Effect) hypothesis tests, mediation models, moderator analyses (age, gender, left/right response bias), and all manuscript figures.

## Data availability

The raw and cleaned data are included directly in this repository (`data/`, `data_clean/`).
