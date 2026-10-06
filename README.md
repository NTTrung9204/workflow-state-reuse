# Supplementary Data

## Directory Structure

```text
dataset/
├── benchmark/
│   ├── analytics_tasks.json                  # 13 Data Analysis benchmark prompts
│   ├── multi_agent_tasks.json                # 6 Multi-turn workflow scenarios
│   ├── sql_tasks.json                        # 20 Text-to-SQL benchmark prompts
│   ├── visualization_tasks.json              # 20 Visualization benchmark prompts
│   └── results/
│       ├── workflow-reuse.json               # Workflow-State Reuse evaluation results
│       ├── Gemini/                           # Evaluation runs across DA, SQL, Viz, Multi (v1–v3)
│       ├── Gemma_4/                          # Evaluation runs across DA, SQL, VIZ, Multi (V1–V3)
│       └── Qwen/                             # Evaluation runs across DA, SQL, Viz, Multi (v1–v3)
├── process_data/
│   ├── process_dataset.csv                   # Chemical process dataset (6,636 rows × 216 parameters)
│   └── schema.json                           # Process variable schema metadata (216 parameters)
└── rag_benchmark/
    ├── queries_and_relevance_labels.json     # 1,050 QA benchmark queries with relevance labels
    ├── bge-reranker-base_evaluation_result.json    # Evaluation results for BAAI/bge-reranker-base
    └── bge-reranker-v2-m3_evaluation_result.json   # Evaluation results for BAAI/bge-reranker-v2-m3
```

## 1. Benchmark Data (`benchmark/`)

The `benchmark/` folder contains benchmark task inputs, multi-model evaluation reports, workflow reuse metrics, and supporting execution outputs.

### Benchmark Tasks (Inputs)

| File | Tasks | Evaluated Scope | Description |
| :--- | :--- | :--- | :--- |
| `analytics_tasks.json` | 13 | Data Analysis (`DA`) | Numerical analysis, statistical correlations, and parameter sensitivity prompts. |
| `sql_tasks.json` | 20 | Text-to-SQL (`SQL`) | Natural language database querying and aggregation prompts. |
| `visualization_tasks.json` | 20 | Visualization (`Viz`) | Plotting, trend analysis, and multi-variable visualization prompts. |
| `multi_agent_tasks.json` | 6 | Multi-Agent (`Multi`) | Multi-turn scenarios (`multi_1`–`multi_5`, `multi_7`) combining querying, analytics, and visualization. |

### Evaluation Results

Results are partitioned under `benchmark/results/` across three foundation models: `Gemini`, `Gemma_4`, and `Qwen`.

Each model directory contains 12 evaluation suites covering four agent configurations evaluated over three experimental runs (`v1`, `v2`, `v3`):
* `DA` / `DA_V*`: Data Analysis agent evaluations.
* `SQL` / `SQL_V*`: Text-to-SQL agent evaluations.
* `Viz` / `VIZ_V*`: Visualization agent evaluations.
* `Multi` / `Multi_v*`: Multi-turn workflow evaluations.

### Workflow-State Reuse Evaluation (`benchmark/results/workflow-reuse.json`)

`benchmark/results/workflow-reuse.json` provides the primary quantitative evaluation results for **Workflow-State Reuse** across multi-turn interactions.

### Supporting Execution Artifacts

* **Evaluation Reports (`.docx`)**:
  * *Single-Agent (`DA`, `SQL`, `Viz`)*: Each run folder contains a summary report (e.g., `DA_agent_evaluation_report.docx`) detailing prompts, generated code, execution status, and accuracy metrics.
  * *Multi-Agent (`Multi`)*: Located in `evaluation_results/` (or `evaluation_reports/` for Gemini/Qwen `Multi_v3`), containing scenario-level reports named `report_eval_multi_<id>_<timestamp>_<hash>.docx`.
* **Intermediate Runtime Data**: Preserved primarily in `v3` runs (and select earlier runs) to support traceability:
  * `charts/`: Rendered `.png` visualization plots (e.g., `chart_<timestamp>.png`).
  * `sql_results/`: CSV exports of intermediate query results (`sql_results/eval_<ts>_<hash>/sql_agent_<ts>.csv`).
  * `optimizer/`: CSV trial logs and `.meta.json` files for parameter optimization calls.

## 2. Process Data (`process_data/`)

* **`process_dataset.csv`**: Tabular chemical process dataset (6,636 rows × 216 columns) used as the external relational database queried and analyzed during evaluation tasks.
* **`schema.json`**: Structured metadata dictionary defining column names, data types, physical units, and process descriptions for 216 variables.

## 3. RAG Benchmark (`rag_benchmark/`)

* **`queries_and_relevance_labels.json`**: Benchmark input containing 1,050 domain QA queries across 8 scientific literature sources, including question category (`factual`, `analytical`, `reasoning`, `paraphrase`, `definition`), reference source, page number, and ground-truth supporting text span.
* **`bge-reranker-base_evaluation_result.json`**: Retrieval and reranking evaluation results for `BAAI/bge-reranker-base` across all 1,050 queries.
* **`bge-reranker-v2-m3_evaluation_result.json`**: Retrieval and reranking evaluation results for `BAAI/bge-reranker-v2-m3` across all 1,050 queries.

Both result files record per-query dense retrieval candidates (FAISS), cross-encoder scores, and IR metrics (MRR, Hit@k, Precision@k, Recall@k, NDCG@k).

