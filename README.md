# Explainable Knowledge Graph-Based Drug Repurposing for Acute Brain Injury: Extending XAIPath to DRKG

**Author:** Maha Attique  
**Institution:** Boston University School of Public Health  
**Program:** M.S. Public Health Data Science (graduating August 2026)  
**Mentor:** Dr. Huimin Cheng, PhD, Assistant Professor of Biostatistics, BUSPH  
**Clinical Collaborator:** Dr. Charlene Ong, Neurocritical Care, Boston Medical Center  

---

## Overview

This project extends [XAIPath](https://doi.org/10.1038/s41598-026-50149-2) (Perdomo-Quinteiro et al., 2026), a GraphSAGE-based explainable drug repurposing pipeline, from its original knowledge graph (NeDRex) to the [Drug Repurposing Knowledge Graph (DRKG)](https://github.com/gnn4dr/DRKG) with a focus on **acute brain injury** (ischemic stroke, intracranial hemorrhage, traumatic brain injury).

This is my PH890 Mentored Research Experience project for the BUSPH MS program. Work began January 2026 under Dr. Cheng's and Dr. Ong's mentorship, with the independent project direction finalized May 2026.

---

## Motivation

Acute brain injury is a leading cause of long-term disability and mortality, with limited therapeutic options beyond acute intervention. Drug repurposing is a faster, lower-cost route to new treatments. However, most GNN-based repurposing pipelines are black boxes. A ranked list of candidate drugs with no mechanistic explanation is difficult for clinicians to act on.

XAIPath addresses this by pairing GraphSAGE link prediction with a post-hoc explainability layer: extracting paths between drug and disease nodes, clustering them via MinHash + K-means, and validating against DrugMechDB. The authors flagged extension to other knowledge graphs as open future work, which was the motivation behind this project.

---

## Methods

- **Knowledge graph:** DRKG (97,238 entities, 5.87M triplets, 107 relation types across DrugBank, Hetionet, GNBR, STRING, IntAct, DGIdb)
- **Primary model:** GraphSAGE (Hamilton et al., 2017) - trained from scratch on DRKG treatment edges
- **Baselines:** TransE pretrained (Bordes et al., 2013), DistMult (Yang et al., 2015), ComplEx (Trouillon et al., 2016)
- **Evaluation:** MRR and Hits@1/3/10 on filtered compound-disease link prediction
- **Validation:** Negative control drug-disease pairs, structured literature corroboration
- **Network analysis:** Disease subgraph structural characterization (degree, connectivity)

---

## Results

**Compound-Disease Link Prediction (filtered MRR, 500 test triples)**

| Model | MRR | Hits@1 | Hits@3 | Hits@10 |
|---|---|---|---|---|
| TransE (pretrained) | 0.347 | 0.220 | 0.406 | 0.568 |
| DistMult | 0.059 | 0.022 | 0.052 | 0.128 |
| ComplEx | 0.066 | 0.028 | 0.064 | 0.116 |
| GraphSAGE | 0.012 | 0.000 | 0.016 | 0.028 |

**Top TransE candidates (acute brain injury):** Heparin, Warfarin, Simvastatin, Dipyridamole, Epoprostenol  
**Top GraphSAGE candidates:** Dabigatran, Decoglurant, Methylsamidorphan, AZD8330  
**Negative control validation:** Clear score separation between top candidates (~-0.18 mean) and true negatives (~-2.22 mean)


---

## Project Structure
├── notebooks/                              # Jupyter notebooks (numbered by pipeline stage)
│ ├── 01_data_exploration.ipynb             # DRKG loading, disease node identification
│ ├── 02_baseline_transE.ipynb              # TransE pretrained embedding scoring
│ ├── 03_disease_subgraph_analysis.ipynb    # Network statistics on brain injury subgraph
│ ├── 04_negative_controls.ipynb            # Negative control validation
│ ├── 05_distmult_complex_training.ipynb    # DistMult and ComplEx training from scratch
│ ├── 06_benchmark_evaluation.ipynb         # MRR/Hits@k across all four models
│ └── 07_graphsage_training.ipynb           # GraphSAGE training and scoring
├── results/                                # Model outputs, evaluation metrics, candidate lists
├── literature/                             # Literature review notes
├── data/                                   # Downloaded automatically via notebook utility
├── embedding_analysis/                     # Original DRKG embedding analysis (from source repo)
├── drkg_with_dgl/                          # Original DRKG DGL loading notebook (from source repo)
├── drug_repurpose/                         # Original DRKG COVID-19 repurposing notebooks
└── requirements.txt                        # Python dependencies

---


## Environment Setup

```bash
# Load modules (BU SCC)
module load python3/3.10.12 gcc/12.2.0 cuda/11.8

# Activate virtual environment
source /projectnb/chenggrp/Maha/drkg_env/bin/activate

# Set Python path
export PYTHONPATH=/projectnb/chenggrp/Maha/drkg_env/lib/python3.10/site-packages:$PYTHONPATH

# Data is downloaded automatically via download_and_extract() in notebooks
```

---

## Status

- [x] Literature review and project scoping
- [x] Environment setup (DGL 2.4.0+cu121, PyTorch 2.1.0, Python 3.10, BU SCC)
- [x] DRKG loaded and verified (97,238 entities, 5.87M triplets)
- [x] Target disease nodes identified (7 MESH IDs for stroke, ICH, TBI)
- [x] Disease subgraph network analysis
- [x] TransE baseline scoring, top 100 candidates generated and named
- [x] DistMult and ComplEx trained from scratch
- [x] GraphSAGE trained from scratch (5 epochs, final loss 0.626)
- [x] Benchmark evaluation, MRR/Hits@k across all four models
- [x] Negative control validation
- [ ] Explainability layer (MinHash + K-means path clustering) -> future work
- [ ] DrugMechDB validation -> future work
- [ ] Interactive demo application -> future work
- [x] Manuscript (in progress)

---

## References

- Perdomo-Quinteiro, P., Guney, E., & Belmonte-Hernández, A. (2026). Generating explainable hypotheses for drug repurposing with graph neural networks. *Scientific Reports*, 16(1), 18840.
- Ioannidis, V. N., et al. (2020). DRKG - Drug Repurposing Knowledge Graph for COVID-19. GitHub.
- Hamilton, W. L., Ying, R., & Leskovec, J. (2017). Inductive representation learning on large graphs. *NeurIPS*, 30.
- Bordes, A., et al. (2013). Translating embeddings for modeling multi-relational data. *NeurIPS*, 26.
- Yang, B., et al. (2015). Embedding entities and relations for learning and inference in knowledge bases. *ICLR*.
- Trouillon, T., et al. (2016). Complex embeddings for simple link prediction. *ICML*, 48, 2071–2080.
- Gonzalez-Cavazos, A. C., et al. (2023). DrugMechDB: A curated database of drug mechanisms. *Scientific Data*, 10(1), 632.

---

## License

Original DRKG code is licensed under its original terms (see `LICENSE`).  
All original project code in `notebooks/` and `results/` is MIT licensed (see `LICENSE-PROJECT.md`).