# RefModel Replication — gpt-oss:20b Reproduction Kit

Reproduction kit for **"Evaluating gpt-oss for Refactoring Detection: A Replication
of RefModel"**, produced for the Experimental Software Engineering course at UFCG
(Stage 3 / Etapa 3 deliverable).

This work replicates the [RefModel](https://github.com/brain-ufcg/RefModel) study,
evaluating the open-weight model **gpt-oss:20b**, run locally via
[Ollama](https://ollama.com/), on RefModel's refactoring-detection task and
datasets.

This repository does **not** contain RefModel's source code, datasets, or
evaluation scripts — only the results and paper produced by this replication.
To reproduce the experiment you clone the original RefModel repository as
described below.

---

## Contents

- [`results/`](./results/) — result spreadsheets produced by this replication
  (per-case results for the synthetic dataset, summarized below).
- [`paper/`](./paper/) — the paper describing this replication (PDF, see the
  folder's [README](./paper/README.md) for status).

---

## Requirements

- [Ollama](https://ollama.com/) runtime
- The `gpt-oss:20b` model pulled locally (`ollama pull gpt-oss:20b`)
- A machine with at least ~16 GB RAM (this replication ran on a MacBook Pro M3,
  18 GB unified memory)
- Python 3 with the dependencies listed in RefModel's own `requirements.txt`
  (installed after cloning RefModel, see below)
- A clone of the original [RefModel repository](https://github.com/brain-ufcg/RefModel),
  which provides the pipeline script, refactoring definitions, and datasets used here

---

## Reproduction steps

### 1. Clone RefModel

```bash
git clone https://github.com/brain-ufcg/RefModel.git
cd RefModel
```

### 2. Install Ollama and pull the model

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama pull gpt-oss:20b
```

### 3. Install Python dependencies

```bash
cd RefModel
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

### 4. Run the pipeline

From inside the `RefModel/` subfolder, against RefModel's own datasets
(`dataset/full-and-small-programs.csv` for the synthetic/JDolly cases,
`dataset/diff-of-large-programs.csv` for the real-world diffs):

```bash
# Synthetic (JDolly) dataset — full-file mode
python3 RefModel.py --mode complete --backend ollama \
  --model gpt-oss:20b --temperature 0.6 \
  --csv ../dataset/full-and-small-programs.csv \
  --definitions definitions/refactoring_definitions.txt \
  --out data/output/gpt-oss-20b-synthetic-results.csv

# Real-world dataset — diff-only mode
python3 RefModel.py --mode diff --backend ollama \
  --model gpt-oss:20b --temperature 0.6 \
  --csv ../dataset/diff-of-large-programs.csv \
  --definitions definitions/refactoring_definitions.txt \
  --out data/output/gpt-oss-20b-realworld-results.csv
```

### 5. Compute precision/recall

Compare the `LLM Answer` column of each output CSV against the dataset's
ground-truth refactoring type per case (as RefModel's own
`evaluation/` folder does) to obtain precision and recall per refactoring
type. The per-case results and derived metrics from this replication are in
[`results/`](./results/).

---

## Results summary

| Dataset               | # Transformations | Precision | Recall |
|-----------------------|-------------------:|----------:|-------:|
| Synthetic (JDolly)     |                858 |     94.5% |  95.4% |
| Real-world (diffs)     |                 44 |     84.4% |  86.4% |

**Setup:** Ollama, `gpt-oss:20b`, temperature `0.6`, MacBook Pro M3 (18 GB RAM).

Detailed per-case results:

- Synthetic (JDolly, 858 cases):
  [`results/gpt-oss-20b-synthetic-jdolly-858-results.csv`](./results/gpt-oss-20b-synthetic-jdolly-858-results.csv)
- Real-world (64 diffs — 44 Java, 10 Go, 10 Python; headline metrics refer to
  the Java subset):
  [`results/gpt-oss-20b-realworld-diffs-results.csv`](./results/gpt-oss-20b-realworld-diffs-results.csv)

---

## Credits

This is a replication of RefModel. All credit for the original tool, datasets,
and evaluation methodology goes to its authors:

```bibtex
@inproceedings{refModel2025,
  author    = {Pedro Simões and Rohit Gheyi and Rian Melo and Jonhnanthan Oliveira and Márcio Ribeiro and Wesley K. G. Assunção},
  title     = {RefModel: Detecting Refactorings using Foundation Models},
  booktitle = {Brazilian Symposium on Software Engineering},
  year      = {2025},
  url       = {https://arxiv.org/abs/2507.11346}
}
```

Original repository: [github.com/brain-ufcg/RefModel](https://github.com/brain-ufcg/RefModel)
