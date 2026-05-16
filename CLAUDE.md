# CLAUDE.md

Context file for the group research essay project. Read this first before responding to any request in this project.

---

## Project overview

**Title:** *Reporting the Climate Crisis: An Event-Study Analysis of News Sentiment Around Natural Disasters in USA and Australia*

**Type:** In-class group research project (climate change course).

**Team:** Nicola Heim, Javiera Rubio, Sreeja Banerjee.

**Core idea:** A text-mining and event-study comparison of how the *Huffington Post* (US) and *ABC News* (Australia) report on climate-related natural disasters between 2012 and 2021, using sentiment analysis and topic modelling, anchored to verified disaster events from EM-DAT.

---

## Deliverables

| Output | Length | Assessment |
|---|---|---|
| Group research essay | 4,000 words | Group |
| Individual reflective report (per team member) | 1,000 words | Individual |

**Required essay structure** (from `Project_essay_guideline.pdf`):
1. Title
2. Introduction (topic, motivation, research question)
3. Literature review / background
4. Data (sources, variables, coverage, preprocessing)
5. Methodology (methods + justification)
6. Results (tables, figures, maps, visualisations)
7. Discussion (implications, limitations, extensions)
8. Conclusion
9. References

Appendices are optional.

**Individual reflective report** must cover: (1) specific contribution, (2) understanding of methods used, (3) critical reflection on process and teamwork.

> **Important note from the brief:** The project is *not* judged on statistical significance, confirmation of hypotheses, or "expected" results. Unexpected or weak findings are valid if interpreted critically. Emphasis is on research design, data analysis, teamwork, and scientific reasoning.

---

## Research questions

1. Does the **frequency** of climate-related news headlines increase after a climate disaster?
2. Does headline **sentiment** become more negative in the aftermath of climate disasters?
3. Is there a difference in the **intensity and sentiment** of HuffPost and ABC coverage of climate disasters, and how does this relate to country-specific disaster exposure?

---

## Datasets (in project folder)

| File | Source | Description | Notes |
|---|---|---|---|
| `EMDAT.csv` | EM-DAT (public.emdat.be) | 300 natural disaster events, AU + US, 2012–2021 | Already filtered: technological subgroup excluded; climatological types kept (flood, storm, wildfire, drought, extreme temperature) |
| `Project essay guideline.pdf` | Course materials | Assessment criteria & required structure | Read before drafting any essay section |
| `Climate project #2.pdf` | Team presentation deck | Current state of the project (motivation, lit review, data, methodology, open questions) | Reflects decisions already made |

**Datasets referenced in eda.ipynb** (sourced from Kaggle):
- HuffPost US news category dataset (~209k records → ~61.7k after date filter → 1,811 climate-relevant headlines across 10 categories: U.S. NEWS, POLITICS, WORLD NEWS, ENVIRONMENT, GREEN, BUSINESS, SCIENCE, MEDIA, IMPACT, THE WORLDPOST).
- ABC News Australia headlines dataset (~1M raw → ~580k after date filter → 15,534 climate-relevant headlines; single unlabelled pool, no category filter applied).

**Climate keyword filter (shared across both corpora):**
`climate change`, `global warming`, `sea level`, `climate`, `flood`, `wildfire`, `bushfire`, `drought`, `hurricane`, `cyclone`, `heatwave`, `emissions`, `carbon`.

---

## Methodology summary

**Pipeline:**
1. **Preprocessing** — tokenisation, lowercasing, stopword removal, normalisation; keyword + date filtering (2012–2021).
2. **Sentiment analysis** — VADER (Valence Aware Dictionary and sEntiment Reasoner) on headline text.
3. **Topic modelling** — Latent Dirichlet Allocation (LDA).
4. **Disaster event integration** — EM-DAT events used as anchors.
   - Event window: −3 to +3 months around disaster peak.
   - Control window: same calendar months in non-disaster years.
5. **Analysis outputs:**
   - Time-series plots of climate headline share + sentiment per outlet.
   - Event-window charts of sentiment ±3 months around major disasters.
   - Correlation heatmap of disaster severity × sentiment.
   - Summary table comparing disaster responsiveness (AU vs US).

**Potential case study:** 2019–2020 Australian Black Summer Bushfires (within study window, EM-DAT verified, strong cross-country media salience, useful for validating the pipeline before scaling).

---

## Coding & analysis conventions

- **Language:** Python.
- **Environment:** Jupyter notebooks.
- **Expected libraries:** `pandas`, `numpy`, `matplotlib`/`seaborn`, `nltk`, `vaderSentiment` (or `nltk.sentiment.vader`), `gensim` or `sklearn` for LDA, `wordcloud` (optional), `scipy.stats` for any inferential tests.
- **Code style:**
  - Reproducible — load data from project paths, no hardcoded local paths.
  - Cells should be small, labelled, and follow the methodology pipeline order.
  - Random seeds set where stochastic methods are used (LDA in particular).
  - Save intermediate cleaned datasets to a `data/processed/` subfolder rather than re-running heavy preprocessing each time.
- **When reviewing code:** check for data leakage between event and control windows, sensitivity of VADER scores to headline length, and LDA topic coherence — not just whether it runs.

---

## Writing conventions

- **Citation style:** APA 7th edition. In-text: (Author, Year); reference list alphabetised, hanging indent, DOIs where available.
- **Tone:** Academic, measured, third-person. Avoid hype words ("groundbreaking", "alarming", "unprecedented") unless directly quoting a source.
- **Hedging:** Use it. Findings are exploratory, the corpora are imbalanced, and the brief explicitly values critical interpretation over confident claims.
- **Numbers:** Spell out one–nine in prose, numerals for 10+; always numerals for percentages, dates, sample sizes.
- **Visuals:** Every figure/table needs a number, a caption, and a referent in the body text. Don't drop charts in without discussion.
- **Word budget guide (rough, 4,000 words total):**
  - Introduction: ~400
  - Literature review: ~700
  - Data: ~600
  - Methodology: ~700
  - Results: ~900
  - Discussion: ~500
  - Conclusion: ~200

---

## How Claude can help

Claude is being used for:
- **Drafting and editing essay sections** — produce drafts that match the team's voice, then iterate. Always flag where claims need a citation.
- **Methodology & analysis review** — sanity-check the event-study design, sentiment scoring choices, topic modelling parameters, and statistical comparisons.
- **Code review (Python)** — review notebooks for correctness, reproducibility, and clarity. Suggest fixes; don't rewrite wholesale unless asked.
- **Individual reflective reports** — help each team member articulate their own contribution and reflection. **Each report must be in that person's voice and reflect their actual work** — do not generate generic reflections, and do not reuse phrasing across team members.
- **Citations & references** — format references in APA 7, suggest additional relevant literature, flag missing citations in drafts.

**Things to avoid:**
- Don't fabricate citations, statistics, or quotes. If a source is needed and not provided, say so.
- Don't overstate findings. The brief rewards critical reflection, not confident claims.
- Don't ignore the limitations already identified (see below) when drafting Results or Discussion.
- Don't merge or rewrite content from the three individual reflective reports — they are assessed separately.

---

## Open decisions (pending team discussion)

These are flagged in the presentation as next steps. If asked, help reason through them; don't assume they're resolved.

1. **HuffPost vs ABC corpus imbalance** (1,811 vs 15,534 climate headlines).
   - Option A: Keep analysis in proportional / rate terms (e.g., share of monthly headlines).
   - Option B: Supplement HuffPost with another US outlet (and confront political-tendency comparability).
   - Option A confirmed as best choice
2. **EM-DAT event weighting.**
   - By count? By total deaths? By total economic damage (USD)? Composite severity index?
   - Currently leaning toward exploring severity weighting but not yet decided.

---

## Known limitations to acknowledge in the essay

These are already identified by the team and should be addressed in the Discussion:

- **Source bias:** HuffPost is left-leaning progressive; ABC has a public-broadcaster impartiality mandate but is often perceived as left-leaning. Both may over-index on climate urgency.
- **Absolute count imbalance:** 8.6× more climate-relevant headlines from ABC than HuffPost — driven by corpus size, not necessarily editorial intensity.
- **False convergence risk:** Both outlets lean similarly, so cross-country differences may be masked. Internal validity holds; external validity (US vs Australian media broadly) does not.
- **Headlines ≠ articles:** Headlines may be more dramatic than article body text, especially for digital-native outlets using clickbait features (Khawar & Boukes, 2024).
- **Public broadcaster vs commercial platform:** Editorial model differences (ABC public-service mandate vs HuffPost ad-supported) may explain sentiment differences as much as national discourse does.
- **Domestic vs international coverage:** A HuffPost headline about Australian bushfires is counted as US coverage — risks inflating cross-country correlation around major events like Black Summer.

---

## Key references already in the literature review

- Boykoff, M. T., & Boykoff, J. M. (2007). Climate change and journalistic norms: A case-study of US mass-media coverage.
- Schirmag et al. (2025).
- Schmidt, A., Ivanova, A., & Schäfer, M. S. (2013). Media attention for climate change around the world: A comparative analysis of newspaper coverage in 27 countries.
- Meier & Eskjær (2024).
- Noviello et al. (2022).
- Khawar & Boukes (2024). *Digital Journalism* — HuffPost clickbait features, 2015–2021.

Full APA references need to be completed; flag any missing details when these are cited in drafts.