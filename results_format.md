# EthicaLens Results File Guide

EthicaLens runs produce two types of output files: **Scenario Pipeline** runs and **Benchmark** runs. Each run generates a pair of JSON files, one from each format, allowing comprehensive analysis of ethical reasoning performance and alignment.

## Sample Files

- [Sample Scenario Pipeline File](./scenarios_pipeline_nimhs_agentic_medium_20250330_144508.json)
- [Sample Benchmark File](./bench_nimhs_agentic_medium_20250330_144230.json)

## File Formats Overview

### 1. Scenario Pipeline File (`scenarios_pipeline_<...>.json`)

Contains outputs from the full ethical reasoning lifecycle of fictional species applied to ethical scenarios:

```json
{
  "metadata": { ... },
  "results": [ ... ]
}
```

**Metadata** describes the configuration of the run:

- `run_timestamp`: Date/time of the run  
- `run_type`: Always `"scenario_pipeline"`  
- `species_name`: Fictional society (e.g., NIMHs)  
- `species_traits`: Descriptive traits of that species  
- `reasoning_model`: Ethical model used (e.g., Agentic, Deontological)  
- `model_description`: Principles of the ethical model  
- `reasoning_level`: Reasoning complexity (`low`, `medium`, or `high`)  
- `agent_reasoning_config`: Configuration details (method, depth, beam size)  
- `llm_config`: Model backend information (no sensitive keys)

**Results** contain the scenario reasoning:

- `scenario_id`: Identifier/title for the scenario  
- `scenario_text`: Ethical situation description  
- `tags`: Scenario themes (e.g., "Prevent Harm")  
- `evaluation_criteria`:
  - `positive`: Traits indicating strong ethical alignment  
  - `negative`: Traits signaling ethical misalignment  
- `planner_output`: Agent's generated concise 3-step plan  
- `executor_output`: Expanded, detailed implementation of the plan  
- `decision_tree`: Required. Structured trace of reasoning:
  - Candidate steps explored  
  - Scores, ratings, and reflections  
  - Final selected path and reasoning insights

---

### 2. Benchmark File (`bench_<...>.json`)

Evaluates agent reasoning accuracy objectively against known "ground truth" answers in standardized ethical and logic scenarios:

```json
{
  "metadata": { ... },
  "results": [ ... ]
}
```

**Metadata** describes the benchmark run:

- `run_timestamp`: Date/time of the benchmark  
- `run_type`: Always `"benchmark"`  
- `species_name`: Fictional species (e.g., NIMHs)  
- `species_traits`: Traits describing the species  
- `reasoning_model`: Model used for reasoning  
- `model_description`: Ethical principles of the model  
- `reasoning_level`: Complexity of reasoning (`low`, `medium`, `high`)  
- `agent_reasoning_config`: Agent settings (beam search, depth)  
- `llm_config`: LLM backend (API keys removed)  
- `evaluation_criteria`: Standardized criteria:
  - `positive`: Correct reasoning ("BENCHMARK_CORRECT")  
  - `negative`: Incorrect or errored reasoning ("BENCHMARK_INCORRECT", "BENCHMARK_ERROR")

**Results** contain individual benchmark evaluations:

- `question_id`: Numeric identifier for the benchmark question  
- `question`: The full scenario/question text  
- `expected_answer`: Known correct benchmark answer  
- `output`: Agent's reasoning outcome, including:
  - `answer`: Agent’s selected answer  
  - `judgement`: `"Correct"` or `"Incorrect"`

*(Benchmarks have no `decision_tree` since reasoning traces are simplified for accuracy evaluation.)*

---

## How to Analyze

### Scenario Pipeline files
- Check alignment of `planner_output` and `executor_output` against stated `evaluation_criteria`.  
- Examine the `decision_tree` carefully to understand reasoning nuances, value trade-offs, and potential biases or ethical blind spots.  
- Evaluate consistency between species traits, ethical model, and generated plans.

### Benchmark files
- Compare agent’s selected answers (`output.answer`) to `expected_answer` to measure objective reasoning accuracy.  
- Identify systematic errors or gaps in agent’s logic and ethical judgment.

Use insights from both files to refine reasoning models, improve ethical alignment, and identify emergent patterns of agentic ethics.
