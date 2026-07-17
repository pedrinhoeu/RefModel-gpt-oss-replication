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

### 3. Run the notebook

Open the RefModel notebook on Google Colab or locally:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/brain-ufcg/RefModel/blob/main/RefModel/notebook/RefModel.ipynb)

Or access it directly on GitHub:
[RefModel/notebook/RefModel.ipynb](https://github.com/brain-ufcg/RefModel/blob/main/RefModel/notebook/RefModel.ipynb)

### 4. Configure the model

Inside the notebook, set the model to `gpt-oss:20b` and the temperature
to `0.6`. Run all cells to execute the pipeline against the desired
dataset (synthetic or real-world). The notebook handles data loading,
prompt construction, model querying, and result collection.

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
