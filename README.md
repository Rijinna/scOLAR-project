# scOLAR: single-cell Ontology-guided Lineage-Aware open-set Recognition
Note: The core implementation is currently in a private repository due to upcoming publication. Full code access can be provided upon request.


**scOLAR** is a computational framework designed for high-precision cell-type annotation in single-cell RNA-seq (scRNA-seq) data, specifically focusing on **Fine-grained Open-Set Recognition (OSR)**. By integrating **Cell Ontology** as a geometric prior and employing **adversarial decision-level regularization**, scOLAR mitigates the "familiarity trap" where novel subtypes are mistakenly classified into known coarse lineages.

---

## Motivation

Existing methods, such as `scBOL`, face significant challenges in cross-dataset cell-type annotation:

* **The Familiarity Trap**: Models often over-rely on coarse-grained semantic features, misclassifying unseen sub-types into known broad categories.


* **Underutilized Ontology**: Cell Ontology hierarchy is typically restricted to label mapping or post-processing, rather than being integrated into the model's representation and decision mechanisms.


* **Overconfidence**: Traditional softmax-based probabilities enforce a simplex constraint that leads to overconfidence in open-set scenarios.



---

## Key Contributions

* **Ontology-guided Geometry Prior**: Elevates Cell Ontology from a preprocessing tool to a **geometric weak prior**. It uses a **Hierarchical Centripetal Loss ($L_{HCL}$)** to constrain the relative topology of the prototype space.


* **Lineage-Aware Decision**: Introduces a **Decision-level Adversarial Loss ($L_{dec-adv}$)** to suppress coarse-level shortcuts, forcing the model to focus on fine-grained specificity rather than broad lineage commonalities.


* **Robust Open-set Inference**: Utilizes **Maximum Logit Score (MLS)** instead of softmax, with a threshold $\alpha$ calibrated via **Pseudo-Novel Simulation** within the reference set.


* **Biological Interpretability**: Implements a **Lineage Consistency Check (LCC)** that transforms "unknown clusters" into "potential novel subtypes under a known lineage".



---

## Methodology

The scOLAR framework follows a logical pipeline: **Representation Robustness → Prototype Geometry → Decision Debiasing → Inference Loop → Biological Explanation**.

### Core Loss Function

The model is optimized through a joint objective:


$$L_{total} = \lambda_1 L_{CE} + \lambda_2 L_{HCL} + \lambda_3 L_{dec-adv} + \gamma L_{recon}$$



* **$L_{HCL}$ (Hierarchical Centripetal Loss)**: Ensures that fine-grained prototypes under the same coarse parent are closer in relative rank.


* **$L_{dec-adv}$ (Adversarial Loss)**: Penalizes excessive reliance on coarse-level similarity when making fine-grained decisions.


* **$L_{recon}$ (Denoising Reconstruction)**: Stabilizes representations against scRNA-seq sparsity (optional, decayed during later stages).



---

## Quick Start

### Installation

```bash
git clone https://github.com/YourUsername/scOLAR.git
cd scOLAR
pip install -r requirements.txt

```

### Data Preparation

To leverage the Cell Ontology DAG, parse the latest `.obo` file:

```bash
python scripts/parse_ontology.py --input data/cl.obo --output data/cl_dag.pkl

```

### Basic Usage

```bash
python main.py --dataset YourDataset --lambda_hcl 0.1 --lambda_adv 0.05 --epochs 200

```

---

## Benchmark Comparison (Work in Progress)

Preliminary results indicate that scOLAR outperforms current baselines (e.g., `scBOL`) in detecting novel subtypes while maintaining high accuracy for known classes.

| Method | AUROC (Open-set) | F1-macro (Closed-set) | Lineage Consistency |
| --- | --- | --- | --- |
| scBOL (Baseline) | 0.XX | 0.XX | Low |
| **scOLAR (Ours)** | **0.XX** | **0.XX** | **High** |

---

## Repository Structure

```text
scOLAR/
├── core/               # Core logic: GNN backbones and Ontology managers
├── losses/             # Implementation of L_HCL and L_dec-adv
├── utils/              # Preprocessing, DAG traversal, and MLS calibration
├── experiments/        # Scripts for reproduction and benchmark testing
├── requirements.txt
└── README.md

```

---

## Citation

If you find this work helpful for your research, please consider citing our working paper:

```bibtex
@article{liuyuqiao2026scolar,
  title  = {scOLAR: single-cell Ontology-guided Lineage-Aware open-set Recognition},
  author = {Yuqiao Liu},
  journal = {Working Paper},
  year   = {2026},
  url    = {https://github.com/Rijinna/scOLAR}
}

```

---

## Contact

**Yuqiao Liu (RiJinna)** 

 **Undergraduate** | Software Engineering, Sichuan University  
 **Exchange Student** | National Taiwan University of Science and Technology (Taiwan Tech)  
 **Research Interests**: Graph Neural Networks (GNNs), Bioinformatics, Open-set Recognition 

- **Email**: rijinna0@gmail.com
- **GitHub**: https://github.com/Rijinna

*Feel free to reach out for collaboration on GNNs or Single-cell analysis!*
