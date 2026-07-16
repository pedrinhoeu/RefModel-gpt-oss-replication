# Results

## `gpt-oss-20b-synthetic-jdolly-858-results.csv`

Per-case results for the synthetic (JDolly) dataset: 858 program transformations
covering 10 refactoring types, run through `gpt-oss:20b` via Ollama.

Columns:

| Column       | Meaning                                                      |
|--------------|---------------------------------------------------------------|
| `tool`       | Source refactoring tool for the transformation (Eclipse, Netbeans, JRRT) |
| `refactoring`| Ground-truth refactoring type                                 |
| `input`      | Original program source                                       |
| `output`     | Transformed program source                                    |
| `LLM`        | Model used (`gpt-oss:20b`)                                     |
| `Date`       | Date the case was run                                          |
| `LLM Output` | Full raw response from the model                               |
| `LLM Answer` | Refactoring type extracted from the model's response           |
| `Correct`    | Whether `LLM Answer` matches the ground-truth `refactoring`     |

## `gpt-oss-20b-realworld-diffs-results.csv`

Per-case results for the real-world dataset (diffs of large programs): 64 diffs
from real open-source projects (44 Java, 10 Go, 10 Python), run through
`gpt-oss:20b` via Ollama in diff mode. The headline metrics in the main
[README](../README.md) (86.4% recall, 84.4% precision over 44 transformations)
refer to the Java subset, matching the original RefModel evaluation.

Columns:

| Column       | Meaning                                                      |
|--------------|---------------------------------------------------------------|
| `baseline`   | Baseline tool/reference the case comes from                   |
| `project`    | Source open-source project                                     |
| `Diff`       | The code diff given to the model                               |
| `size`       | Diff size                                                      |
| `language`   | Programming language (java, golang, python)                    |
| `LLM`        | Model used (`gpt-oss:20b`)                                     |
| `Date`       | Date the case was run                                          |
| `LLM Output` | Full raw response from the model                               |
| `LLM Answer` | Refactoring type(s) extracted from the model's response         |
