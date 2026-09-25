# semantic-narrative-topology

Replication materials for the manuscript **"Local Topological
Organization of Semantic Narrative Spaces: A Distance-Sensitive
Statistical Framework for Human and AI-Generated Narratives."**

## Overview

This repository contains the data and code used to reproduce the revised
core analyses, robustness checks, and manuscript figures. The workflow
is designed to run in Google Colab and can also be executed from a local
clone of the repository.

The empirical results should be interpreted as properties of the
analyzed corpus and embedding representation, rather than as universal
properties of Human- or AI-generated language.

## Repository contents

-   `ASMBI_replication_FINAL_REPOSITORY.ipynb` --- main replication
    notebook.
-   `extracted_corpus_from_orange.csv` --- narrative corpus used by the
    analysis.
-   `requirements.txt` --- Python packages required by the notebook.
-   `README.md` --- documentation for the replication materials.

Tables, numerical summaries, and manuscript figures are generated
automatically when the notebook is executed and are written to the
output directory defined in the notebook.

## Data

The distributed input file is `extracted_corpus_from_orange.csv`.

The source file contains 256 rows: 64 Human-labeled records and 192
AI-labeled records. Nine AI records have empty text fields. The notebook
removes empty narratives before embedding extraction, yielding the final
analytical corpus of **247 non-empty narratives**:

-   64 Human-authored narratives;
-   183 AI-generated narratives.

The original generation design targeted three independently generated AI
narratives for each Human narrative. After removal of empty or
unavailable texts, 61 prompts retain the complete structure of one Human
narrative and three corresponding AI narratives. These 61 complete
prompt blocks, comprising 244 narratives, are used for the primary
prompt-aware graph inference.

Four typographical inconsistencies in sample identifiers are corrected
explicitly in the notebook before constructing the prompt blocks.

## AI-generation information

The AI narratives were generated using OpenAI's `gpt-3.5-turbo-0301`
model snapshot. The generation design used separate generation runs for
the AI narratives associated with a source prompt.

The original prompt template and the values of generation parameters
such as `temperature`, `top_p`, and maximum-token settings were not
retained in the available research records and therefore cannot be
reconstructed reliably. Consequently, the repository supports
reproduction of the embedding, statistical, graph, and topological
analyses from the preserved corpus, but not exact regeneration of the
original AI narratives from the source prompts.

## Text representation

Narratives are embedded using the pretrained Sentence-Transformers
model:

`sentence-transformers/paraphrase-multilingual-mpnet-base-v2`

The resulting representation has **768 dimensions**. Narratives are
passed to the encoder in their original textual form. No lowercasing,
stopword removal, stemming, lemmatization, or systematic punctuation
removal is applied before embedding extraction. The model is used
without task-specific fine-tuning.

## Analyses reproduced

The notebook reproduces the following revised analyses:

1.  **Distance-sensitive geometry**
    -   Euclidean distance;
    -   Manhattan distance;
    -   cosine distance;
    -   regularized Mahalanobis distance using Ledoit-Wolf covariance
        shrinkage.
2.  **Unsupervised distance diagnostics**
    -   average-linkage agglomerative clustering with two clusters;
    -   silhouette coefficients used as exploratory diagnostics;
    -   label-free k-nearest-neighbor stability under 1%, 5%, and 10%
        perturbation levels, with 500 replications per level.
3.  **Graph analysis**
    -   cosine-based k-nearest-neighbor graphs;
    -   local density;
    -   mean local distance;
    -   graph degree;
    -   weighted clustering;
    -   betweenness centrality;
    -   Human-neighbor ratio;
    -   source assortativity.
4.  **Prompt-aware inference**
    -   primary analysis based on the 61 complete prompt blocks;
    -   within-prompt block permutation inference;
    -   10,000 permutations for the primary `k = 10` analysis.
5.  **Neighborhood-size sensitivity**
    -   graph analyses for `k = 5, 10, 15, 20`.
6.  **Prompt-balanced resampling**
    -   one AI narrative randomly selected within each complete prompt;
    -   balanced samples of 61 Human and 61 AI narratives;
    -   graph reconstruction over 1,000 replications.
7.  **Persistent homology**
    -   persistence diagrams for the full Human and AI point clouds;
    -   quantitative `H1` comparison under equal-size resampling;
    -   500 random AI subsamples of size 64.
8.  **Manuscript visualizations**
    -   UMAP visualization;
    -   kNN graph displayed in UMAP coordinates;
    -   persistence diagrams.

UMAP-based figures are descriptive visualizations only. Inferential
graph analyses are conducted using the original high-dimensional
embedding representation.

## Computational implementation

Some computationally intensive steps use optimized but mathematically
equivalent implementations. In particular, neighborhood-stability
calculations avoid repeatedly constructing full pairwise distance
matrices, and regularized Mahalanobis geometry is evaluated through an
equivalent transformed-space representation.

These optimizations preserve the analytical design, replication counts,
randomization structure, and substantive conclusions. Direct and
optimized implementations were checked during development and produced
numerically equivalent results within computational tolerance.

## Randomization and reproducibility

The notebook fixes the random seeds used for the principal stochastic
procedures. Major replication counts are:

-   neighborhood stability: 500 replications per perturbation level;
-   prompt-block permutation inference: 10,000 permutations;
-   prompt-balanced graph resampling: 1,000 replications;
-   equal-size persistent-homology analysis: 500 AI subsamples.

Generated outputs are written to `/content/asmbi_outputs/` when the
notebook is run in Google Colab.

## Running the notebook

### Google Colab

1.  Open `ASMBI_replication_FINAL_REPOSITORY.ipynb` in Google Colab.
2.  If the repository files are available in the current working
    directory, the notebook automatically detects
    `extracted_corpus_from_orange.csv`.
3.  Alternatively, upload `extracted_corpus_from_orange.csv` to
    `/content/`.
4.  Run the notebook from top to bottom.
5.  Generated tables, summaries, and figures are saved in
    `/content/asmbi_outputs/`.

### Local execution

Clone or download the repository, install the packages listed in
`requirements.txt`, and run the notebook from the repository root. The
notebook automatically detects `extracted_corpus_from_orange.csv` when
it is located in the same directory as the notebook.

The first notebook cell contains a Google Colab package-installation
command. When running locally, the dependencies can instead be installed
from `requirements.txt`.

## Software environment

The analysis uses Python together with NumPy, pandas, SciPy,
scikit-learn, Sentence-Transformers, NetworkX, UMAP, ripser, persim,
tqdm, and Matplotlib.

Exact historical package versions from the original exploratory
environment were not retained. The repository therefore records the
required package set and provides a self-contained Colab installation
cell for the finalized replication workflow.

## Interpretation boundary

The analyses characterize this corpus under the specified embedding
representation, distance definitions, graph construction, and resampling
procedures. They are not intended to establish universal differences
between Human and AI-generated narratives.

## Citation

If using these materials, please cite the associated manuscript:

> Ferrari et al. *Local Topological Organization of Semantic Narrative
> Spaces: A Distance-Sensitive Statistical Framework for Human and
> AI-Generated Narratives.*

Full bibliographic information will be added after publication.

## Contact

For questions concerning the replication materials, please contact the
corresponding authors through the information provided in the
manuscript.
