# IDS 570: Text as Data — Data Exploration
**Political Economies Corpus | Duke University, Spring 2026**

---

## Overview

This repository contains the R Markdown notebook for the Data Exploration assignment in IDS 570: Text as Data. The analysis applies three complementary quantitative methods to a corpus of 20 Early Modern English texts on political economy drawn from the Early English Books Online (EEBO) database.

The goal is to map the structure of the corpus, identify patterns of similarity and difference, and generate research questions — mirroring how exploratory text analysis is used in humanities and social-science research.

---

## Corpus

20 EEBO texts spanning the late 16th to early 18th century, all related to the theme of political economy. They include:

- The **Malynes–Misselden–Mun debate** (1620s): mercantilist arguments about exchange rates, bullion, and the balance of trade
- **East India Company** treatises (1680s–1700s): defenses and critiques of the Company's trade monopoly
- **Poverty and governance** pamphlets: proposals for poor relief and domestic social policy
- **April** (A06790): a political allegory — the literary outlier of the corpus
- **Lion Key** (A93819): a legal defense brief by Josiah Child over property rights at Lion Key wharf — the legal-procedural outlier of the corpus

---

## Methods

| Approach | Purpose |
|---|---|
| **TF-IDF** | Identify what makes each document lexically distinctive relative to the corpus |
| **Pearson Correlation** | Measure pairwise similarity between documents across vocabulary space |
| **Syntactic Complexity** | Profile sentence structure using the Week 05 framework (MLS, C/S, DC/C, DC/S, Coord, CN) |

Syntactic analysis compares **April** vs. **Lion_Key** — selected as the two most genre-distinct texts in the corpus: one literary allegory, one legal brief.

---

## Repository Structure
```
├── data_exploration.Rmd   # Main analysis notebook (code + interpretive write-up)
├── *.txt                  # 20 corpus texts (place in same folder as .Rmd)
├── *.udpipe               # English udpipe model (auto-downloaded on first run)
└── README.md
```

---

## How to Run

### 1. Prerequisites
```r
install.packages(c(
  "quanteda", "quanteda.textstats",
  "tidyverse", "udpipe", "tidytext", "ggplot2", "scales"
))
```

### 2. Setup

Place `data_exploration.Rmd` in the **same folder** as all 20 `.txt` corpus files. The script detects them automatically — no file paths to configure.

### 3. First run only

The udpipe English model (~15 MB) downloads automatically the first time the syntactic complexity chunk runs. After that it is reused from the local file.

### 4. Run

Open `data_exploration.Rmd` in RStudio and either:
- Run chunk by chunk: `Ctrl + Shift + Enter`
- Knit the full document: `Ctrl + Shift + K`

---

## Normalization Choices

| Step | Applied | Reason |
|---|---|---|
| Long S (`ſ` → `s`) | ✅ Required | Prevents `ſhall` and `shall` from being treated as different tokens |
| Remove numbers | ✅ Applied | Exchange rates, folio numbers, and legal dates are noise |
| Collapse whitespace | ✅ Applied | Irregular EEBO typesetting artifacts |
| Normalize `u`/`v`, `i`/`j` | ❌ Not applied | Preserves period-specific orthographic signals |
| Standardize variant spellings | ❌ Not applied | TF-IDF is robust to variation; homogenizing risks erasing register signals |

---

## Outputs

The notebook produces the following inline:

- **TF-IDF ECDF heatmap** — top 3 terms per document, color-scaled by within-document percentile rank
- **Pearson correlation heatmap** — pairwise similarity matrix across all 20 documents
- **Syntactic complexity lollipop chart** — 8-measure comparison of April vs. Lion_Key
- **Shared TF-IDF terms bar chart** — corpus-wide vocabulary appearing in 5+ documents
- **Tables** — top TF-IDF terms per document, most/least similar pairs, syntactic complexity profile

---

## Key Findings

- The corpus divides into three topical clusters: **Monetary/Exchange**, **Colonial Trade**, and **Poverty & Governance**
- `Discourse_1`/`Discourse_2` and `Poor_Eng_1`/`Poor_Eng_2` show near-perfect Pearson r ≈ 1.00, suggesting reprints or revised editions
- `April` and `Lion_Key` are both genre outliers in TF-IDF and Pearson — but for opposite reasons: one literary, one legal-procedural
- Syntactic analysis confirms the genre difference is structural: `April` builds sentence complexity through coordination and accumulation (epideictic rhetoric); `Lion_Key` builds it through subordination and evidentiary embedding (legal-procedural writing)

---

## Dependencies

- R ≥ 4.1
- `quanteda` ≥ 3.0
- `udpipe` ≥ 0.8
- `tidyverse` ≥ 1.3
- `ggplot2` ≥ 3.4
- `scales`, `tidytext`
