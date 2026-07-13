# Literature Review Notes

## Core Paper: XAIPath

**Perdomo-Quinteiro, P., Guney, E., & Belmonte-Hernández, A. (2026)**
*Generating explainable hypotheses for drug repurposing with graph neural networks. Scientific Reports, 16(1), 18840.*

- Introduces XAIPath: a GraphSAGE-based drug repurposing pipeline with a post-hoc explainability layer
- Pipeline: train GraphSAGE on NeDRex knowledge graph for link prediction → extract simple paths between top drug-disease pairs → cluster paths via MinHash + K-means → validate clusters against DrugMechDB
- Key case study: loteprednol–stroke, directly relevant to this project's disease focus and is a reimplementation sanity check
- Authors flag extension to other knowledge graphs (including DRKG) as future work, which is the primary novelty claim of this project

- **Role in this project:** Anchor methodology. Every component of this project's pipeline follows XAIPath's design.

---

## Knowledge Graph: DRKG

**Ioannidis, V. N., Song, X., Manchanda, S., Li, M., Pan, X., Zheng, D., Ning, X., Zeng, X., & Karypis, G. (2020)**
*DRKG - Drug Repurposing Knowledge Graph for COVID-19. GitHub.*

- Integrates 6 source databases: DrugBank, Hetionet, GNBR, STRING, IntAct, DGIdb
- 97,238 entities, 5.87 million triplets, 107 relation types
- Ships with pretrained TransE embeddings and DGL-based tooling for GNN training
- Originally developed for COVID-19 drug repurposing, so this project extends it to acute brain injury

- **Role in this project:** Primary data source. Replaces NeDRex as the knowledge graph. Larger and more widely used than NeDRex.

---

## Validation Database — DrugMechDB

**Gonzalez-Cavazos, A. C., Tanska, A., Mayers, M., Carvalho-Silva, D., Sridharan, B., Rewers, P. A., Sankarlal, U., Jagannathan, L., & Su, A. I. (2023)**
*DrugMechDB: A curated database of drug mechanisms. Scientific Data, 10(1), 632.*

- Manually curated database of drug mechanisms expressed as paths through a biomedical knowledge graph
- Covers 4,583 drug indications with 32,249 relationships across 14 biological scales
- Designed specifically to train and evaluate computational drug repurposing models
- Since XAIPath uses DrugMechDB for path cluster validation, this project does the same

- **Role in this project:** Primary validation resource. Path clusters from the explainability layer are cross-referenced against DrugMechDB to confirm mechanistic plausibility. Candidates not in DrugMechDB are to be handled via structured literature search.

---

## Embedding Baselines

**Bordes, A., Usunier, N., Garcia-Duran, A., Weston, J., & Yakhnenko, O. (2013)**
*Translating embeddings for modeling multi-relational data. NeurIPS, 26.*

- Introduces TransE: models relationships as translations in embedding space (h + r ≈ t for a valid triplet head, relation, tail)
- Simple, interpretable, computationally efficient
- DRKG ships with pretrained TransE embeddings, so no retraining needed for baseline use

- **Role in this project:** Baseline 1. Used as a simple pretrained reference point, so GraphSAGE is benchmarked against it.

---

**Yang, B., Yih, W., He, X., Gao, J., & Deng, L. (2015)**
*Embedding entities and relations for learning and inference in knowledge bases. ICLR.*

- Introduces DistMult: bilinear scoring function using diagonal relation matrices, which is simpler than full bilinear models, strong empirical performance
- Outperformed TransE on Freebase link prediction at time of publication
- Configurable in DGL-KE as a scoring function swap from TransE, so no new model architecture needed

- **Role in this project:** Baseline 2. Second embedding baseline alongside TransE, adding comparative depth to the benchmark without additional engineering cost.

---

**Trouillon, T., Welbl, J., Riedel, S., Gaussier, É., & Bouchard, G. (2016)**
*Complex embeddings for simple link prediction. ICML, 48, 2071–2080.*

- Introduces ComplEx: uses complex-valued embeddings to handle both symmetric and antisymmetric relations, which is a limitation of DistMult
- Uses Hermitian dot product, remains linear in space and time
- Configurable in DGL-KE as a scoring function swap, which is the same pipeline as TransE and DistMult

- **Role in this project:** Baseline 3. Third embedding baseline; together with TransE and DistMult, it'll give a comprehensive view of classical embedding method performance against GraphSAGE.

---

## Primary Model — GraphSAGE

**Hamilton, W. L., Ying, R., & Leskovec, J. (2017)**
*Inductive representation learning on large graphs. NeurIPS, 30.*

- Introduces GraphSAGE: inductive GNN that learns by aggregating features from sampled neighborhoods rather than the full graph
- Inductive design means it can generalize to unseen nodes, which is important for large, heterogeneous graphs like DRKG
- Neighborhood sampling makes it computationally tractable at scale
- XAIPath uses GraphSAGE as its primary model, using the same architecture makes it easier to apply explainability layer without modification

- **Role in this project:** Primary model. Trained on DRKG for drug-disease link prediction, benchmarked against TransE, DistMult, and ComplEx.

---

## Explainability Layer Components

**Broder, A. Z. (1997)**
*On the resemblance and containment of documents. Proceedings of the Compression and Complexity of Sequences 1997, pp. 21–29. IEEE.*

- Introduces MinHash: a locality-sensitive hashing technique for efficiently estimating Jaccard similarity between sets
- Originally developed for web document deduplication at AltaVista
- XAIPath uses MinHash to encode extracted paths between drug-disease node pairs before clustering

- **Role in this project:** First step of the explainability layer's clustering pipeline. Paths are MinHash-encoded then fed into K-means.

---

**MacQueen, J. B. (1967)**
*Some methods for classification and analysis of multivariate observations. Proceedings of the 5th Berkeley Symposium on Mathematical Statistics and Probability, 1, 281–297.*

- Introduces K-means clustering: iterative algorithm assigning observations to the nearest centroid, minimizing within-cluster variance
- Standard, widely used clustering method; computationally efficient
- XAIPath uses K-means on MinHash-encoded paths to surface distinct mechanistic clusters

- **Role in this project:** Second step of the explainability layer's clustering pipeline. Clusters of similar paths represent distinct candidate mechanistic explanations for a drug-disease prediction.