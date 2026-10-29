# 🎵 Multivariate Data Analysis of Spotify Tracks

Exploration and interpretation of a Spotify dataset of high-popularity tracks using **dimensionality reduction, clustering and classification techniques** in R, with the aim of understanding the relationship between audio features and musical genres.

> Project for *Data Analysis* (Multivariate Analysis) · Universitat Politècnica de Catalunya (UPC) · Bachelor's in Data Science and Engineering (GCED) · Academic year 2024–2025
> Authors: **Oriol Parent**, **Nicolás Jiménez** and **Lucas Viner**

---

## 📌 Overview

**Dataset:** [Spotify Music Dataset](https://www.kaggle.com/datasets/solomonameh/spotify-music-dataset) (Ameh, 2023, Kaggle): 1,686 high-popularity tracks and 29 variables, including numerical audio features (`energy`, `tempo`, `danceability`, `loudness`, `valence`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `duration_ms`, `track_popularity`) and categorical metadata (`playlist_genre`, `track_artist`...).

**Preprocessing:**
- Removal of the single row with a missing album name and of irrelevant metadata (IDs, URLs, musical keys).
- Numeric type conversion.
- Filtering to the **top 5 genres** (pop, rock, latin, hip-hop, electronic) and their top 5 subgenres, giving a dataset (`sp_filtered`) better suited to multivariate analysis.

## 🔬 Techniques and findings

| Technique | What was done | Main findings |
|---|---|---|
| **PCA** | Numerical variables, categorical as supplementary. 4 components kept using the scree-plot elbow. | Four interpretable axes: commercial sound and loudness vs. acoustic/mellow, danceability and brevity vs. long instrumental pieces, musical complexity, and lyric-driven (rap focus) vs. melodic mainstream. Rock lies at the low end of Dim 1 and 2; hip-hop and latin lead on Dim 2 and 4. |
| **MDS** | Metric MDS with Euclidean distance (GOF 52.5% after removing collinear variables), metric MDS with **Gower distance** (mixed data, including genre) and non-metric MDS (stress 23.2). | Euclidean MDS shows no sharp genre clusters. Gower MDS separates genres much better (pop, latin, rock), with a rhythm-dominated to melody-driven horizontal axis. |
| **MCA** | Numerical variables discretised into low / medium / high categories, 2 dimensions retained, no Guttman effect detected. | Dim 1 separates hip-hop and latin from rock (speechiness, danceability vs. instrumentalness). Dim 2 is linked to energy and valence. Pop and electronic sit near the origin (generic, commercial tracks). |
| **Cluster analysis** | Hierarchical clustering (single, complete, average, centroid, Ward), Pseudo-F index, K-means and HCPC. Ward with **k = 4** kept. | Four profiles: *Calm and acoustic*, *Popular and powerful*, *Rhythmic and spoken*, *Instrumental and emotional*. K-means and HCPC gave overlapping, hard-to-interpret clusters. |
| **LDA / QDA + MANOVA** | Normality tests, Box's M test (unequal covariances, so QDA preferred over LDA), Box-Cox transformation, MANOVA, stepwise classification and Naive Bayes. | **QDA reaches ≈ 70.4% accuracy** (Q-statistic = 926.3 ≫ 9.49 critical value). Speechiness, duration and acousticness discriminate genres best, while liveness and tempo contribute little. Backward stepwise: 0.635, Naive Bayes: 0.632. Electronic vs. pop is the main source of confusion. |

**Overall conclusion:** popular tracks are organised along three continuums, *sonic intensity*, *rhythmic accessibility* and *lyrical focus*, which partition genres into recognisable segments. These patterns are consistent across all the techniques used.

## 📁 Repository structure

```
.
├── PCA_MDS.Rmd                    # Preprocessing, PCA and MDS (Euclidean, Gower, non-metric)
├── MCA_Cluster.Rmd                # Multiple Correspondence Analysis and cluster analysis
├── LDA_MANOVA.Rmd                 # Normality, Box's M, QDA, MANOVA, stepwise, Naive Bayes
├── report.pdf                     # Final report with figures, interpretation and appendices
├── high_popularity_spotify_data.csv   # Dataset (read by the three .Rmd files)
└── README.md
```

> ⚠️ The three `.Rmd` files read `high_popularity_spotify_data.csv` from their own folder. It was not included with the uploaded files, so download it from the Kaggle link above and place it next to the `.Rmd` files.

## ▶️ How to reproduce

**Requirements:** R (≥ 4.0), RStudio (recommended) and a LaTeX distribution (e.g. TinyTeX) to knit the PDF output.

```r
install.packages(c(
  "tidyverse", "corrplot", "factoextra", "FactoMineR", "gridExtra", "plotly",
  "MASS", "smacof", "cluster", "aplpack", "viridis", "RColorBrewer", "knitr",
  "kableExtra", "ggplot2", "dplyr", "dendextend", "klaR", "e1071"
))
```

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   ```
2. Add `high_popularity_spotify_data.csv` to the folder.
3. Open each `.Rmd` in RStudio and knit it (or run it chunk by chunk). They are independent of each other: each one runs its own preprocessing.

## 🛠️ Tools

- **R** · `FactoMineR`, `factoextra` (PCA, MCA, HCPC), `smacof` / `cmdscale` / `cluster::daisy` (MDS, Gower distance), `dendextend` (dendrograms), `klaR` and `MASS` (QDA, stepwise classification), `e1071` (Naive Bayes)
- **R Markdown / knitr** · reproducible analysis and report

## 📚 References

- Ameh, S. (2023). *Spotify Music Dataset*. Kaggle. https://www.kaggle.com/datasets/solomonameh/spotify-music-dataset
- Acar, N. (2025). Course materials on PCA, MDS, MCA, cluster analysis, discriminant analysis, multivariate normal distribution and multivariate hypothesis testing. Universitat Politècnica de Catalunya.

## 👥 Authors

- **Oriol Parent**
- **Nicolás Jiménez**
- **Lucas Viner**
