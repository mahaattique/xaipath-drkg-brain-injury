# Explainable Knowledge Graph-Based Drug Repurposing for Acute Brain Injury: Extending XAIPath to DRKG

**Author:** Maha Attique  
**Institution:** Boston University School of Public Health  
**Program:** M.S. Public Health Data Science (graduating August 2026)  
**Mentor:** Dr. Huimin Cheng, PhD, Assistant Professor of Biostatistics, BUSPH  
**Clinical Collaborator:** Dr. Charlene Ong, Neurocritical Care, Boston Medical Center  

---

## Overview

This project extends [XAIPath](https://doi.org/10.1038/s41598-026-50149-2) (Perdomo-Quinteiro et al., 2026), a GraphSAGE-based explainable drug repurposing pipeline, from its original knowledge graph (NeDRex) to the [Drug Repurposing Knowledge Graph (DRKG)](https://github.com/gnn4dr/DRKG) focusing on **acute brain injury** (ischemic stroke, intracranial hemorrhage, traumatic brain injury).

This is my PH890 Mentored Research Experience project for the BUSPH MS program. Work began January 2026 under Dr. Cheng's mentorship, with the independent project direction finalized May 22, 2026.

---

## Motivation

Acute brain injury remains a leading cause of long-term disability and mortality, with limited therapeutic options beyond acute intervention. Drug repurposing offers a faster, lower-cost route to new treatments. However, most GNN-based repurposing pipelines are black boxes — a ranked list of candidate drugs with no mechanistic explanation is difficult for clinicians to act on.

XAIPath addresses this by pairing GraphSAGE link prediction with a post-hoc explainability layer: extracting paths between drug and disease nodes, clustering them via MinHash + K-means, and validating against DrugMechDB. The authors flagged extension to other knowledge graphs as open future work — this project does exactly that.

---

## Methods

- **Knowledge graph:** DRKG (97,238 entities, 5.87M triplets, 107 relation types across DrugBank, Hetionet, GNBR, STRING, IntAct, DGIdb)
- **Primary model:** GraphSAGE (Hamilton et al., 2017)
- **Baselines:** TransE (Bordes et al., 2013), DistMult (Yang et al., 2015), ComplEx (Trouillon et al., 2016)
- **Evaluation:** MRR, Hits@1/3/10 with confidence intervals across multiple random seeds
- **Explainability:** Path extraction + MinHash/K-means clustering (Broder, 1997)
- **Validation:** DrugMechDB, negative controls, structured literature corroboration
- **Network analysis:** Disease subgraph structural characterization
- **Demo:** Interactive Streamlit/Gradio application

---

## Project Structure
├── notebooks/          # Jupyter notebooks (numbered by pipeline stage)
├── src/                # Python scripts for training, evaluation, explainability
├── data/               # Data loading instructions (DRKG downloaded separately)
├── results/            # Model outputs, evaluation metrics, candidate lists
├── app/                # Interactive demo application
├── embedding_analysis/ # DRKG pretrained embedding analysis (from original repo)
├── drkg_with_dgl/      # DGL graph loading (from original repo)
└── requirements.txt    # Python dependencies

---

## Environment Setup

```bash
# Load modules (BU SCC)
module load python3/3.10.12 cuda/11.8

# Activate virtual environment
source /projectnb/chenggrp/Maha/drkg_env/bin/activate

# Install dependencies
pip install -r requirements.txt
```

---

## Status

- [x] Literature review and project scoping
- [x] Environment setup (DGL 2.1.0, PyTorch 2.1.0, Python 3.10)
- [x] DRKG repository cloned and explored
- [ ] Baseline training (TransE, DistMult, ComplEx)
- [ ] GraphSAGE training and benchmarking
- [ ] Explainability layer implementation
- [ ] Validation against DrugMechDB and negative controls
- [ ] Network analysis of disease subgraph
- [ ] Interactive demo application
- [ ] Manuscript and poster

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
All original project code in `src/`, `notebooks/`, and `app/` is MIT licensed (see `LICENSE-PROJECT.md`).