# Learning Plan — Modern Sales Data Science Project
> Tailored for a Data Engineer helping on Kedro + Databricks + LLM pipeline project
> Last updated: June 2026

---

## How to Use This Plan
- Follow the **order** — each module builds on the previous
- ✅ = Your DE background already covers this (light review only)
- 🔴 = New territory — spend most time here
- 🟡 = Partial knowledge — fill the gaps

---

## MODULE 1 — Kedro Core Concepts 🔴
> **Goal:** Understand how Kedro structures a data science project — nodes, pipelines, catalog, parameters

### 1.1 What is Kedro & Why It Exists
- How Kedro solves the "messy notebook" problem in data science teams
- Kedro vs plain Python scripts vs Airflow
- Project folder structure overview

**Resources:**
- 📺 [Introduction to Data Pipelines and Kedro](https://www.youtube.com/watch?v=rf8yBHsDOj4) — start here
- 📺 [Get Started with Kedro — Create Your First Pipeline](https://www.youtube.com/watch?v=3YeE_gvDCvw)
- 📄 [Kedro Concepts — Official Docs](https://docs.kedro.org/en/stable/get_started/kedro_concepts.html)

---

### 1.2 Kedro Project Structure (Folder Layout)
```
project/
├── conf/
│   ├── base/
│   │   ├── catalog.yml       ← dataset definitions
│   │   └── parameters.yml    ← model/pipeline parameters
│   └── local/                ← local overrides (not committed)
├── src/
│   └── pipelines/
│       └── new_location/     ← one folder per sales motion
│           ├── nodes.py      ← actual transformation logic
│           └── pipeline.py   ← wires nodes together
├── data/
│   ├── 01_raw/
│   ├── 02_intermediate/
│   └── 04_feature/
└── notebooks/                ← exploratory work
```

**Resources:**
- 📄 [Kedro Configuration Basics](https://docs.kedro.org/en/stable/configuration/configuration_basics.html)
- 📄 [Data Catalog YAML Examples](https://docs.kedro.org/en/stable/catalog-data/data_catalog_yaml_examples/)

---

### 1.3 Nodes — The Building Blocks
- A node = a Python function wrapped by Kedro
- Inputs and outputs are named datasets (matched to catalog)
- Nodes are pure functions — no side effects

**Resources:**
- 📺 [Assemble Nodes into a Kedro Pipeline](https://www.youtube.com/watch?v=P__gFG1TmMo)
- 📺 [Make Complex Kedro Pipelines — Refactor into Functions](https://www.youtube.com/watch?v=zvAnE05-agw)

---

### 1.4 Data Catalog (catalog.yml)
- Think of it like your ETL source/target config — but for ML datasets
- Supports CSV, Parquet, Delta, Spark DataFrames, APIs
- Datasets are loaded/saved automatically by Kedro

**Resources:**
- 📺 [Add Datasets Programmatically to a Kedro Catalog](https://www.youtube.com/watch?v=CIRVpMqWEIs)
- 📄 [Data Catalog YAML Examples](https://docs.kedro.org/en/stable/catalog-data/data_catalog_yaml_examples/)

---

### 1.5 Parameters (parameters.yml)
- Store model hyperparameters, thresholds, config values
- Accessed inside nodes via `params:` prefix in catalog
- Easy to swap environments (dev vs prod)

**Resources:**
- 📺 [Kedro Parameters](https://www.youtube.com/watch?v=Jj5cQ5bqcjg)
- 📺 [Passing Arguments to Kedro via Command Line](https://www.youtube.com/watch?v=aZmjnFT4RyQ)
- 📄 [Parameters — Official Docs](https://docs.kedro.org/en/stable/configuration/parameters.html)

---

### 1.6 Modular Pipelines (critical for this project)
- Each sales motion (new_location, renewal, channel_partner) = one modular pipeline
- Pipelines are composed and registered in `pipeline_registry.py`
- Running a specific motion: kedro run --pipeline=new_location

**Resources:**
- 📺 [Kedro Spaceflights Tutorial — Data Engineering Pipeline](https://www.youtube.com/watch?v=OPkB0OCygcw)
- 📺 [Kedro Introduction for Data Science — Full Playlist](https://www.youtube.com/playlist?list=PLZ5P9y9ALLeLqll4odGhw53S0UxNqWKR0)
- 📄 [Modular Pipelines — Official Docs](https://docs.kedro.org/en/stable/nodes_and_pipelines/modular_pipelines.html)

---

### 1.7 Dataset Versioning
- Kedro can version datasets automatically (like Delta history but simpler)
- Useful for tracking model outputs across runs

**Resources:**
- 📺 [Apply Versioning to Datasets in Kedro](https://www.youtube.com/watch?v=iRwy-IStfPo)

---

## MODULE 2 — Kedro + Databricks Integration 🔴
> **Goal:** Understand how the Modern Sales pipelines run on Databricks

### 2.1 How Kedro Runs on Databricks
- Kedro pipelines are submitted as Databricks jobs/workflows
- Nctebooks can be used as Kedro nodes via kedro-databricks plugin
- Delta tables registered in the Kedro DataCatalog as SparkDatasets

**Resources:**
- 📺 [Deploy Your Kedro Pipelines to Databricks](https://www.youtube.com/watch?v=9ZttTN6zDM0)
- 📺 [Databricks & SparkDatasetV2: Modern Workflows with Kedro](https://www.youtube.com/watch?v=kmWTReVDjRM)
- 📺 [Visualise Your Kedro Pipelines in Databricks Notebooks](https://www.youtube.com/watch?v=fuxXtJPypAY)

---

### 2.2 PySpark Inside Kedro Nodes
- Nodes receive a Spark DataFrame instead of Pandas
- SparkDataSet in catalog.yml points to ADLS/Delta paths
- Partition strategies, schema enforcement apply as normal

**Resources:**
- 📺 [How to Setup PySpark for Your Kedro Pipeline](https://www.youtube.com/watch?v=vYBMpPZep6E)

---

### 2.3 Kedro-Viz (Pipeline Visualisation)
- Visual DAG of the entire pipeline - very useful to understand the New Location flow
- Run locally: kedro viz

**Resources:**
- 📺 [Kedro Spaceflights Tutorial — Data Science Pipeline](https://www.youtube.com/watch?v=a64I1pewAoA)

---

## MODULE 3 — Data Science Pipeline Architecture 🟡
> **Goal:** Understand the 3-layer pipeline structure used in Modern Sales

### 3.1 The 3-Layer Pattern (Data Prep → Modeling → Reporting)

Raw Input Tables (Delta / ADLS) -> [DATA PREP LAYER] -> [MODELING LAYER] -> [REPORTING LAYER] -> Top 20 Opportunities

**Resources:**
- 📺 [Build Production-Level ML & Data Pipeline with Kedro](https://www.youtube.com/watch?v=z8N-117Lzao)
- 📺 [How to Build Production-Ready Data Science Pipelines with Kedro](https://www.youtube.com/watch?v=but_AZV0rhA)

---

### 3.2 Deterministic vs Probabilistic Models
- Deterministic = rule-based scoring
- Probabilistic = ML model output
- Modern Sales combines both into a single opportunity score

**Resources:**
- 📺 [Probabilistic vs Deterministic Models - 2 Min](https://www.youtube.com/watch?v=U8kuVAvam50)
- 📺 [Deterministic vs Probabilistic Models: Which One Wins?](https://www.youtube.com/watch?v=bDjCT6QMHLI)

---

### 3.3 Feature Engineering Basics
- Joins, aggregations, window functions (your SQL/PySpark skills apply directly)
> You already know this from DE - just apply it in the ML context

---

### 3.4 Train/Test Split & Model Evaluation (light awareness)
- Not building models, but you need to read and understand modeling nodes

**Resources:**
- 📺 [Kedro Spaceflights Tutorial - Data Science Pipeline](https://www.youtube.com/watch?v=a64I1pewAoA)

---

## MODULE 4 — LLM / Generative AI in the Pipeline 🔴
> Goal: Understand how LLMs generate sales prompts from structured data

### 4.1 How LLMs Are Used in Modern Sales
- Input: structured data (TGM, news, engagement history)
- Output: natural language prompt shown to BDM in the app
- Each sales motion has its own prompt template

### 4.2 Prompt Engineering Basics
- System prompt vs user prompt
- Injecting structured data into prompt templates
- Controlling output format (JSON, bullet points)

**Resources:**
- 📺 [Prompt Engineering Essentials - Getting Better Results from LLMs](https://www.youtube.com/watch?v=LAF-lACf2QY)
- 📺 [Prompt Engineering Master Class for Engineers 2024](https://www.youtube.com/watch?v=ujnLJru2LIs)
- 📺 [Prompt Engineering Full Course 2025 - Edureka](https://www.youtube.com/watch?v=YvKJ8Xyjs6E)

### 4.3 Parsing LLM Output Back into Pipeline
- LLM returns text -> parse into structured columns
- This is a Kedro node like any other

---

## MODULE 5 — Azure DevOps & Sprint Workflow

### 5.1 ADO Board Navigation
- Sprint board, backlog, user stories, tasks
- Currently on Sprint 29

### 5.2 Git Branching in ADO
- Feature branch -> PR -> review -> merge
> You already know ADO/JIRA - just get access via Sulabh and orient to board layout

---

## Recommended Learning Order & Timeline

| Week | Focus | Key Deliverable |
|-------|-------|----------------|
| Week 1 | Module 1 (Kedro Core) | Run the Spaceflights tutorial locally |
| Week 2 | Module 2 (Kedro + Databricks) | Understand New Location pipeline in Databricks |
| Week 3 | Module 3 (DS Pipeline Architecture) | Read through New Location nodes.py and pipeline.py |
| Week 4 | Module 4 (LLM basics) | Understand the prompt generation node |
| Ongoing | Module 5 (ADO) | Pick up a user story with guidance |

---

## Quick Reference - Key Commands

```bash
kedro run --pipeline=new_location
kedro pipeline list
kedro viz
kedro run --params=model.threshold:0.7
```

---

## Key Docs Bookmarks

| Resource | Link |
|----------|------|
| Kedro Official Docs | https://docs.kedro.org |
| Kedro Concepts | https://docs.kedro.org/en/stable/get_started/kedro_concepts.html |
| Data Catalog YAML | https://docs.kedro.org/en/stable/catalog-data/data_catalog_yaml_examples/ |
| Parameters Docs | https://docs.kedro.org/en/stable/configuration/parameters.html |
| Modular Pipelines | https://docs.kedro.org/en/stable/nodes_and_pipelines/modular_pipelines.html |
| Kedro-Databricks Plugin | https://github.com/getindata/kedro-databricks |

---
*Focus: New Location pipeline is the entry point - cleanest, simplest, recommended by Lakshman*