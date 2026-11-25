# Question Answering over Knowledge Graphs with Ontology-Aware RAG

Final-year project, BSc Computer Science, University of Manchester.
Supervisor: Dr Jiaoyan Chen.

## Goal

This project studies whether giving a large language model (LLM) ontology
information about Wikidata helps it translate natural-language questions
into correct SPARQL queries.

For each question, the system retrieves ontology information about the
entities and properties involved:

- **Entity classes**: instance of (P31)
- **Parent classes**: subclass of (P279)
- **Property information**: labels, descriptions, and property constraints
  (P2302), such as the allowed subject and value types

This information is added to the prompt, and the LLM generates a SPARQL
query, which is run against the Wikidata Query Service.

**Research question:** Does ontology-aware retrieval improve question
answering accuracy on the QALD-10 benchmark, compared with baselines that
use no ontology information?

## Pipeline

```
question → retrieve ontology information → build prompt → LLM writes SPARQL → run query → answer
```

The pipeline is a fixed sequence of steps (not an agent), built with
LangChain and LangGraph.

## Repository structure

- `src/` – source code
- `data/raw/` – original datasets, e.g. QALD-10 (not committed)
- `data/processed/` – processed data and cached Wikidata responses (not committed)
- `results/` – experiment outputs
- `notebooks/` – exploration
- `docs/logbook.md` – project log

## Setup

Tested with Python 3.14 on Windows.

```
git clone https://github.com/1adamata/kgqa-schema.git
cd kgqa-schema
python -m venv .venv
.venv\Scripts\Activate.ps1        # Windows
source .venv/bin/activate         # macOS / Linux
python -m pip install -r requirements.txt
```

### LLM

The LLM is configurable, and the code does not depend on a specific model:

- **Local model:** install [Ollama](https://ollama.com) and pull a model.
- **Hosted model:** install the provider's LangChain package and put its
  API key in a `.env` file in the project root. `.env` is gitignored and
  is never committed.

The models used in the experiments will be listed here.