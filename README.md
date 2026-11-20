# Automated-Sequence-to-Structure-and-Functional-Annotation

# An API-Driven Framework for the Automated Sequence-to-Structure and Functional Annotation of Novel Protein Families

This repository contains the complete computational implementation for an automated, open-source pipeline designed to bridge the sequence-to-structure gap in protein biology. By interfacing directly with open-access database Application Programming Interfaces (APIs), this framework eliminates manual data curation barriers to streamline sequence extraction, family domain classification, AI-driven tertiary structural evaluation, interaction hotspot mapping, and parallel molecular docking.

The sequence-to-structure modules use the human neurokine **FAM19A5** (UniProt ID: `Q7Z5A7`) as a baseline case study, while the API-driven hotspot-to-docking module demonstrates end-to-end ligand screening and binding-site validation on acetylcholinesterase (**UniProt ID: `P22303`**) across Google Colab and Kaggle environments.

---

## Repository Architecture

The framework is organized into modular Jupyter Notebooks tracking the core functional phases detailed in the accompanying manuscript:

```text
├── Uniport.ipynb               # Phase 1: Ingestion, stream data querying, & property profiling
├── Intrepro.ipynb              # Phase 2: Flattening JSON metadata for domain boundary mapping
├── Alphafold.ipynb             # Phase 3: PDB/CIF data parsing, pLDDT scores, & PAE matrix analysis
├── Docking_Colab_Kaggle.ipynb  # Phase 4: PDBe-KB hotspots, BioBB preparation, & parallel AutoDock Vina docking
├── requirements.txt            # Unified Python dependencies configuration
└── README.md                   # Documentation manual

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

### 4. Hotspot-Guided Parallel Molecular Docking (`Docking_Colab_Kaggle.ipynb`)

* Queries the **PDBe-KB Graph API** (`ligand_sites`) and **UniProtKB REST API** (`Active site` / `Binding site` annotations) to rank residue contact counts and call binding hotspots (e.g., `2xSD`, `mean+2SD`, or fixed thresholds).
* Filters interacting ligands by hotspot contact, atom/torsion limits, and optional additive/ion flagging, then fetches ideal chemical component SDFs via `biobb_io.api.ideal_sdf` and converts them to `.pdbqt` in parallel using Open Babel (`biobb_chemistry.babelm.babel_convert`).
* Prepares the AlphaFold receptor structure with `biobb_structure_utils.utils.str_check_add_hydrogens` and defines a size-capped docking grid box around hotspot and UniProt site residues via `biobb_vs.utils.box_residues`.
* Runs parallel **AutoDock Vina** (`v1.2.5`) docking across configurable speed/quality profiles (`fast`, `standard`, `thorough`), supports resumable runs and multi-part ligand splits, and checks docked poses against hotspot contacts.

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
    [ PDBe Graph API ]    ──> Ligand Contact Profiling & Hotspot Thresholding
            │
            ▼
 [ BioBB + Open Babel ]   ──> Receptor Protonation, Grid Box Sizing, & Ligand PDBQT Prep
            │
            ▼
   [ AutoDock Vina ]      ──> Parallel Pose Generation & Hotspot Contact Verification

```

---

## Installation & Environment Setup

### Prerequisites

* Python 3.8 or higher (Python 3.13 tested in cloud notebook runs)
* Working terminal or an integrated notebook environment with internet access (e.g., **Google Colab**, **Kaggle** with Session *Internet = ON*, or **VS Code**)
* For `Docking_Colab_Kaggle.ipynb`: **Open Babel** (`obabel`) and **AutoDock Vina** (`v1.2.5`) alongside BioExcel Building Blocks (`biobb_io`, `biobb_structure_utils`, `biobb_chemistry`, `biobb_vs`, `biobb_structure_checking`)

### Setup Steps

1. **Clone the Repository:**

```bash
git clone [https://github.com/username/repository-name.git](https://github.com/username/repository-name.git)
cd repository-name

```

2. **Install Required Scientific Libraries:**

Install baseline Python dependencies via `pip`:

```bash
pip install -r requirements.txt

```

If installing core analytical libraries manually:

```bash
pip install requests pandas numpy matplotlib seaborn jupyter

```

For the Colab/Kaggle molecular docking notebook (`Docking_Colab_Kaggle.ipynb`), the setup cell automatically installs `biobb_io`, `biobb_structure_utils`, `biobb_chemistry`, `biobb_vs`, `biobb_structure_checking`, `biopython`, `nglview`, `pandas`, `matplotlib`, and `requests`, and sets up `obabel` and AutoDock Vina `v1.2.5` when missing.

3. **Launch the Pipeline:**

Run the notebooks sequentially or open `Docking_Colab_Kaggle.ipynb` directly in Google Colab or Kaggle:

```bash
jupyter notebook

```

---

## Core Visualizations Generated

The algorithms within this suite produce publication-ready figures and tables mapping out structural and docking metrics:

* **UniProt Ingestion Scatterplots:** Correlation models linking total peptide link counts against formula weights (Da).
* **pLDDT Trend Lines:** Sequential graph plots mapping trace metrics over shaded confidence thresholds (Very High > 90, Confident 70–90, Low < 50).
* **PAE Intensity Distortions:** Color-coded heatmaps translating positional alignment error coordinates across dynamic domain folds.
* **AlphaMissense Alignments:** Dual-axis coordinate comparisons assessing core evolutionary vulnerability markers side by side.
* **Hotspot Contact Profiles & Ligand Docking Summaries:** Per-residue PDBe-KB contact scatterplots with hotspot threshold lines (`hotspot_profile.png`), filtered ligand tables, and per-ligand docking/hotspot-contact summaries.

---

## Citation

If you adapt this pipeline or build frameworks using these automated workflow elements, please cite the core manuscript:

> **An API-Driven Framework for the Automated Sequence-to-Structure and Functional Annotation of Novel Protein Families.** *Code and Data Availability Release, 2026.*

---

## License

This suite is open-source under the terms of the [MIT License](https://www.google.com/search?q=LICENSE). Data ingestion remains subject to the usage conditions specified by individual source providers (EMBL-EBI, UniProt Consortium, PDBe-KB, and DeepMind AlphaFold Database).

```

```
