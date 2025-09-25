# Assessing Bias in Attention‑Based Large Language Models for Patient Problem Statements

## 0) Plain, neutral summary

Repository layout centered on patient‑dilemma experiments with multiple LLMs. Directories separate verbatim outputs, normalized tables, audited aggregates, and figure exports. Descriptions below aim to be descriptive rather than prescriptive.

---

## 1) Top level

* `LICENSE` — licensing text.
* `README.md` — this document.

---

## 2) Data layer (`/data`)

Purpose: keep inputs/outputs and their derivations grouped by model and stage.

### 2.1 Per‑model folders

Each model has a folder under `data/` with the following subfolders:

* `raw/` — files as produced by a run or API call.
* `procesed/` — normalized CSVs aligned to a common schema used downstream.
* `verified/` — audited roll‑ups (counts, proportions, IDs) used by plots.

Models present in this tree: `deepseek/`, `falcon/`, `gpt/`, `grok/`, `llama/`, `mistral/`.

**Notes**

* File names indicate batch/run order where applicable.
* Spelling of `procesed` matches existing paths in scripts.

### 2.2 GPT specifics (`/data/gpt`)

* `input/` — JSONL request lots: `lote_1.jsonl` … `lote_32.jsonl`.
* `output/raw/` — per‑lot, per‑shard JSONL responses in subfolders `lote1/ … lote5/`.
* `output/procesed/` — normalized CSVs derived from the raw responses.
* `verified/` — audited aggregates for GPT.

### 2.3 Patients (`/data/patients`)

* `patients_used.csv` — synthetic cohort with attributes.
* `patients_duel.csv` — pairings for binary dilemmas.
* `patients_distribution.csv` — attribute frequencies in the cohort.
* `prompt_used.txt` — prompt text used during runs.

### 2.4 Verified table expectations (common across models)

* `ids_elegidos.csv` — chosen/non‑chosen identifiers by `pair_id`.
* `distribucion_eleccion.csv` — counts by attribute×category.
* `proporciones_ajustadas.csv` — selection rates and intervals with a stated reference category.
* `respuestas_<model>_verificado.csv` — long‑format table used by plotting scripts.

Columns and formats follow the conventions in Section 5.

---

## 3) Code layer (`/src`)

Organization: input generators/adapters and output normalizers/utilities.

### 3.1 Patient utilities (`/src/input/patients`)

* `Synthetic_Patient_Profiles.py` — emits the cohort files in `/data/patients`.
* `Pairwise_Creation.py` — emits pairings used as dilemmas.
* `Prompt_Construction.py` — emits the canonical prompt file.

### 3.2 Model adapters (`/src/input/models/<model>/generation.py`)

* One `generation.py` per model folder: `deepseek-v3/`, `falcon/`, `gpt/`, `grok/`, `llama/`, `mistral/`.
* Typical parameters include input location, output location, and runtime options.
* Output targets the model’s `data/<model>/raw/` (or the GPT substructure under `data/gpt/output/raw/`).

### 3.3 Output utilities (`/src/output`)

* `<model>/clean_answer.py` — transforms `raw/` into `procesed/` following the common schema.
* `general/Dataset_Consolidation.py` — merges per‑model `procesed/` tables for cross‑model analysis.
* `general/Bias_Metrics.py` — produces files under each `verified/` directory.

---

## 4) Visualization layer (`/visualization`)

All figures are exported in `PDF` and `PNG` formats. Subfolders group chart families; each family contains `comparative/` (cross‑model) and per‑model folders (`Deepseek`, `GPT`, `Grok`, `LLaMA`, `Mistral`).

### 4.1 Bar charts (`/visualization/bar_charts`)

* Attributes covered: `age`, `sex`, `religion`, `social_class`, `education`, `health_status`, `criminal_record`, `drug_use`.
* Style variants appear as suffixes: `_gray`, `_viridis`, `_blue`.

### 4.2 Pie charts (`/visualization/pie_charts`)

* Same attribute coverage as bar charts for quick share views.

### 4.3 Spider charts (`/visualization/spider_charts`)

* Radar‑style summaries across categories.

### 4.4 Styled tables (`/visualization/styled_tables`)

* Tabular summaries corresponding to the audited proportions.

---


## 5) License

See `LICENSE` at repository root.
