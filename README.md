# Automated-Sequence-to-Structure-and-Functional-Annotation

# An API-Driven Framework for the Automated Sequence-to-Structure and Functional Annotation of Novel Protein Families

This repository contains the complete computational implementation for an automated, open-source pipeline designed to bridge the sequence-to-structure gap in protein biology. By interfacing directly with open-access database Application Programming Interfaces (APIs), this framework eliminates manual data curation barriers to streamline sequence extraction, family domain classification, AI-driven tertiary structural evaluation, and interaction surface mapping.

The architecture uses the human neurokine **FAM19A5** (UniProt ID: `Q7Z5A7`) as a baseline case study to demonstrate validation.

---

## Repository Architecture

The framework is organized into modular Jupyter Notebooks, tracking the core functional phases detailed in the accompanying manuscript:

```text
├── Uniport.ipynb          # Phase 1: Ingestion, stream data querying, & property profiling
├── Intrepro.ipynb         # Phase 2: Flattening JSON metadata for domain boundary mapping
├── Alphafold.ipynb        # Phase 3: PDB/CIF data parsing, pLDDT scores, & PAE matrix analysis
├── requirements.txt       # Unified Python dependencies configuration
└── README.md              # Documentation manual

```

### Module Breakdown

### 1. Sequence Ingestion & Exploratory Analytics (`Uniport.ipynb`)

* Submits programmatic queries to the **UniProtKB REST API** endpoint matching target taxonomic filters.
* Automates client-side pagination, HTTP stream processing, and multi-page record parsing.
* Generates structural property profiles, evaluating patterns like **Sequence Length vs. Molecular Mass** profiles.

### 2. Federated Family Classification (`Intrepro.ipynb`)

* Communicates directly with the **InterPro API** to extract literature-referenced signatures from multiple federated source member databases.
* Features custom JSON flatteners designed to translate highly nested responses into organized tabular data.
* Isolates specific functional boundaries (such as confirming the TAFA chemokine-like domain localized between positions **42–131**).

### 3. AI Structural Conformation Metrics (`Alphafold.ipynb`)

* Retrieves structural coordinate archives (`.pdb` and `.cif` formats) on demand via the **AlphaFold Protein Structure Database API**.
* Parses backbone-level reliability fields to calculate minimal, maximum, and average **predicted Local Distance Difference Test (pLDDT)** statistics.
* Processes array coordinate fields using `NumPy` to generate **Predicted Aligned Error (PAE)** distance matrices.
* Integrates mutational tolerability alignments by mapping rescaled structural metrics alongside **AlphaMissense pathogenicity scores**.

---

## Pipeline Execution Roadmap

```text
  [ UniProtKB REST API ] ──> Programmatic Sequence & Mass Mining
            │
            ▼
   [ InterPro REST API ]  ──> JSON Flattening & Domain Boundary Resolution
            │
            ▼
 [ AlphaFold Database ]   ──> Structural PDB/CIF Retrieval & pLDDT/PAE Extraction
            │
            ▼
    [ PDBe Graph API ]    ──> Molecular Interaction Mapping & Target Site Scoring

```

---

## Installation & Environment Setup

### Prerequisites

* Python 3.8 or higher
* Working terminal or an integrated environment supporting Jupyter Notebook architecture (e.g., Google Colab, VS Code)

### Setup Steps

1. **Clone the Repository:**
```bash
git clone https://github.com/username/repository-name.git
cd repository-name

```


2. **Install Required Scientific Libraries:**
The analytical scripts leverage the standard scientific Python ecosystem. Install all baseline dependencies directly via `pip`:
```bash
pip install -r requirements.txt

```


*If a custom file is not used, install manually:*
```bash
pip install requests pandas numpy matplotlib seaborn jupyter

```


3. **Launch the Pipeline:**
Run the modules in sequential order to process target structural targets:
```bash
jupyter notebook

```



---

## Core Visualizations Generated

The algorithms within this suite produce publication-ready vector figures mapping out structural metrics:

* **UniProt Ingestion Scatterplots:** Correlation models linking total peptide link counts against formula weights (Da).
* **pLDDT Trend Lines:** Sequential graph plots mapping trace metrics over shaded confidence thresholds (Very High > 90, Confident 70–90, Low < 50).
* **PAE Intensity Distortions:** Color-coded heatmaps translating positional alignment error coordinates across dynamic domain folds.
* **AlphaMissense Alignments:** Dual-axis coordinate comparisons assessing core evolutionary vulnerability markers side by side.

---

## Citation

If you adapt this pipeline or build frameworks using these automated workflow elements, please cite the core manuscript:

> **An API-Driven Framework for the Automated Sequence-to-Structure and Functional Annotation of Novel Protein Families.** *Code and Data Availability Release, 2026.*

---

## License

This suite is open-source under the terms of the [MIT License](https://www.google.com/search?q=LICENSE). Data ingestion remains subject to the usage conditions specified by individual source providers (EMBL-EBI, UniProt Consortium, and DeepMind AlphaFold Database).
