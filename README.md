# AAI5004_Asn1: Recommender System Type Analysis

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Subhash-269/AAI5004_Asn1/blob/main/AAI5004_Asn1_VenkatNeelraj_Nitta.ipynb)

**Course:** AAI 6650 Recommendation Engines, Module 2, Research Assignment 1<br>
**Author:** Venkat Neelraj Nitta, Northeastern University

This notebook compares the four main recommender system types on **one shared dataset and one target user**. The domain is course recommendations on a technical upskilling (Learning & Development) platform:

| § | Approach | Signal used |
|---|---|---|
| 1 | Dataset | Real IBM enrollments, a 12 × 15 working matrix, and course genre tags |
| 2 | User-based collaborative filtering (CF) | Cosine similarity, top-3 neighbors, similarity-weighted prediction |
| 3 | Content-based filtering (CBF) | 14 IBM genre tags and a mean-centered user profile |
| 4 | Knowledge-based filtering (KBS) | Stated goals plus prerequisite rules, with constraint relaxation |
| 5 | Hybrid | Prerequisite gate (cascade), then an adaptive weighted blend of CF, CBF and KBS |
| 6 | Comparison | Top-5 table, grouped bar chart, Jaccard overlap, commentary |
| 7 | Cold-start check | A real learner with a single enrollment |
| 8 | Scale check | User-based CF over the full 33,901-learner platform |

## Dataset

Real learner–course enrollments from the **IBM Skills Network course recommender dataset**, used in IBM's [*Machine Learning Capstone*](https://www.coursera.org/learn/machine-learning-capstone) course on Coursera:

- [`ratings.csv`](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-ML321EN-SkillsNetwork/labs/datasets/ratings.csv): 233,306 enrollments by 33,901 learners on 126 courses
- [`course_genre.csv`](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-ML321EN-SkillsNetwork/labs/datasets/course_genre.csv): 14 binary genre tags per course

The rating is the **enrollment mode**: `3` = completed with a certificate, `2` = audited without completing. It is implicit feedback, and 95% of the values are `3`. The definition comes from IBM's lab *Exploratory Data Analysis on Online Course Enrollment Data* (Luo, 2021); a [public copy](https://github.com/mboccenti/Recommender_system_project) is available.

The KBS course levels, prerequisite rules and learning-goals forms were written for this notebook, because the dataset has none. The notebook labels them as authored.

## Key findings

- **CF saturates on implicit data.** Every prediction is 3.00, so the ranking comes down to a neighbor-support tie-break. Across the full platform, the three most similar learners have taken only courses the target has already taken, so CF can score none of the candidates.
- **CBF over-specializes.** The coarse genre tags score 5 of the 8 candidate courses at exactly 0.
- **KBS is the only method that enforces readiness.** It blocks courses whose prerequisites have not been completed.
- **A plain weighted hybrid is not enough.** It ranked a course third even though the learner had not completed its prerequisite. The final design filters on prerequisites first (cascade) and then blends the three scores (weighted).

## How to run

1. Click the **Open in Colab** badge above.
2. Choose **Runtime → Run all**.

No setup is needed. The notebook uses only `numpy`, `matplotlib` and the Python standard library, with no recommender libraries. The data downloads from IBM's public storage at runtime. If the download fails, an embedded copy of the 12 × 15 working matrix is used, and every section except the §8 scale check still runs.

To run locally:

```bash
pip install numpy matplotlib jupyter
jupyter notebook AAI5004_Asn1_VenkatNeelraj_Nitta.ipynb
```

## References

- Burke, R. (2002). Hybrid recommender systems: Survey and experiments. *User Modeling and User-Adapted Interaction, 12*(4), 331–370. https://doi.org/10.1023/A:1021240730564
- Felfernig, A., & Burke, R. (2008). Constraint-based recommender systems: Technologies and research issues. *Proceedings of the 10th International Conference on Electronic Commerce*. https://doi.org/10.1145/1409540.1409544
- Hu, Y., Koren, Y., & Volinsky, C. (2008). Collaborative filtering for implicit feedback datasets. *2008 Eighth IEEE International Conference on Data Mining*, 263–272. https://doi.org/10.1109/ICDM.2008.22
- IBM Skills Network. (n.d.). *Course recommender system datasets* [Data set]. IBM ML321EN Machine Learning Capstone.
- Luo, Y. (2021). *Exploratory data analysis on online course enrollment data* [Jupyter notebook lab]. In *Machine learning capstone* (IBM ML321EN). IBM Skills Network.

The full reference list is at the end of the notebook.
