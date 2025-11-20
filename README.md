# MLops-Complete-Pipeline

This repository showcases an **end-to-end machine learning pipeline** built with modern **MLOps** practices. It uses **DVC (Data Version Control)** for:

- Experimentation and reproducibility  
- Versioning of data and models  
- Orchestration of pipeline stages  

and is designed to integrate with **cloud storage (e.g. AWS S3)** for remote data/model storage.

---

## 🎯 Project Goals

- Implement a complete ML pipeline from **raw data → trained model → evaluation**  
- Make every step **reproducible and versioned** using DVC  
- Track experiments, metrics, and artifacts for iterative improvement  
- Demonstrate how to connect local ML workflows with **cloud object storage** (AWS S3)

---

## 📁 Repository Structure

At the top level, the project contains:

```text
.
├── .dvc/            # DVC internals and cache metadata
├── dvclive/         # Live metrics and experiment logs (created by dvclive / DVC)
├── experiments/     # Experiment-related configs / artifacts
├── src/             # Source code for the ML pipeline
├── .dvcignore       # Ignore rules for DVC
├── .gitignore       # Ignore rules for Git
├── dvc.yaml         # DVC pipeline definition (stages, deps, outs)
├── dvc.lock         # Lockfile ensuring reproducible runs
├── params.yaml      # Central configuration: paths, hyperparameters, etc.
├── project_flow.txt # High-level project flow / notes
├── LICENSE          # MIT License
└── README.md        # Project documentation (this file)
```

> The `src/` directory usually contains modular scripts such as:
> - Data ingestion
> - Data preprocessing / cleaning
> - Feature engineering
> - Model training
> - Model evaluation

You can add details about each script here once you finalize the code inside `src/`.

---

## 🧱 Core Technologies

- **Language:** Python  
- **Version Control:** Git  
- **Data & Model Versioning / Pipeline:** [DVC](https://dvc.org/)  
- **Experiment Logging:** `dvclive` (via DVC experiments)  
- **Cloud Remote (example):** AWS S3 (configured as a DVC remote)

---

## ⚙️ Setup & Installation

### 1. Clone the Repository

```bash
git clone https://github.com/upratham/MLops-Complete-Pipeline.git
cd MLops-Complete-Pipeline
```

### 2. Create a Virtual Environment (Optional but Recommended)

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

### 3. Install Dependencies

If you have a `requirements.txt`, install everything with:

```bash
pip install -r requirements.txt
```

If not, you will at least need:

```bash
pip install dvc "dvc[s3]" dvclive
# plus the usual ML stack used in your src/ code:
# e.g. pandas, numpy, scikit-learn, matplotlib, etc.
```

---

## 📦 DVC Configuration

### Initialize DVC (if not already initialized)

If `.dvc/` and `dvc.yaml` already exist, you can skip this step. Otherwise:

```bash
dvc init
git add .dvc .dvcignore dvc.yaml
git commit -m "Initialize DVC pipeline"
```

### Configure Remote Storage (AWS S3 Example)

Replace the example bucket/path with your own:

```bash
dvc remote add -d storage s3://your-bucket-name/path/to/project
# Optional: configure a custom endpoint if needed
# dvc remote modify storage endpointurl https://s3.amazonaws.com

git add .dvc/config
git commit -m "Configure DVC remote storage"
```

Then, you can push/pull data and models:

```bash
dvc push   # Upload tracked data/models to remote
dvc pull   # Download data/models from remote
```

---

## 🏃 Running the Pipeline

The pipeline is defined in **`dvc.yaml`** and consumes parameters from **`params.yaml`**.

### 1. Reproduce the Entire Pipeline

```bash
dvc repro
```

This command will:

- Execute all stages in the correct order (e.g. ingestion → preprocessing → training → evaluation)  
- Use the parameters specified in `params.yaml`  
- Update outputs (processed data, models, metrics) under DVC tracking

### 2. Run a Specific Stage

If you only want to re-run a single stage (e.g. `train`):

```bash
dvc repro train
```

Use the stage names defined in your `dvc.yaml` file.

---

## 🧪 Experiments & Metrics

DVC supports lightweight experiment tracking:

### Run an Experiment

```bash
dvc exp run
```

You can:

- Modify `params.yaml` directly, or  
- Override parameters via the CLI (depending on how `dvc.yaml` is configured)

### Compare Experiments

```bash
dvc exp show
```

This displays a table of experiments, parameters, and metrics. If your pipeline logs metrics via **dvclive**, you’ll also see updated metrics and plots under `dvclive/`.

---

## 🔧 Configuration via `params.yaml`

Use `params.yaml` to centrally manage:

- Data paths (raw / processed)
- Model hyperparameters
- Train/validation split parameters
- Any other configuration you want to keep versioned

Example structure (adjust to your actual file):

```yaml
data:
  raw_path: data/raw/data.csv
  processed_path: data/processed/data.csv

train:
  test_size: 0.2
  random_state: 42
  n_estimators: 100
```

Your scripts in `src/` should read from `params.yaml` so that changing configuration doesn’t require editing code.

---

## 🗺️ Project Flow

The file **`project_flow.txt`** in the project root gives a high-level overview of:

- The conceptual steps of the pipeline  
- Data flow between stages  
- Any manual or environment-specific setup notes

This README focuses on usage and setup; you can use `project_flow.txt` to document the story and design decisions of the project.

---

## ✅ Best Practices for Using This Repo

- Track **code and configs** (like `params.yaml`, `dvc.yaml`) in Git  
- Track **datasets, models, and large artifacts** in DVC  
- After meaningful changes:
  1. Update `params.yaml` or code in `src/`  
  2. Run `dvc repro` or `dvc exp run`  
  3. Commit your changes to Git  
  4. Run `dvc push` to sync data/models to remote storage  

This pattern ensures every experiment is reproducible and your entire ML workflow is versioned end-to-end.

---

## 📜 License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.
