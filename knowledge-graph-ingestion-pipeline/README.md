
# Knowledge Graph Ingestion Pipeline (TMCF / MCF)

This module documents the design of an **agentic pipeline for converting raw public datasets into knowledge-graph–ready formats**.

The system automatically transforms heterogeneous datasets (CSV / JSON) into:
- **TMCF (Template Meta Content Framework)**
- **MCF (Meta Content Framework)**

These formats are required for ingestion into large-scale knowledge graph platforms such as public data graphs.

The focus here is on **schema reasoning, metadata inference, and agent orchestration**, not on any client-specific implementation.

---

## Problem Statement

Public datasets are typically:
- Poorly documented
- Inconsistent in structure
- Lacking standardized metadata
- Difficult to integrate into semantic systems

Knowledge graphs require **strict schema compliance**, including:
- Clear definition of statistical variables
- Explicit metadata (measured property, population, units, constraints)
- Valid and unique identifiers (DCIDs)

Manually converting datasets into TMCF/MCF is:
- Time-consuming
- Error-prone
- Not scalable

The challenge was to design a system that can **understand datasets like a human** and automatically generate valid, schema-compliant outputs.

---

## High-Level Solution

This system uses a **multi-agent architecture** to analyze datasets, infer meaning, and generate knowledge-graph mappings.

Instead of hardcoded rules, the pipeline:
- Reasons over column names and sample values
- Infers dataset structure (long vs wide form)
- Identifies semantic roles
- Generates compliant identifiers
- Produces ready-to-ingest metadata files

---

## Agentic Architecture Overview

The pipeline is composed of specialized agents, each responsible for a distinct stage of reasoning.

---

### 1. Data Loader Agent

**Purpose:**  
Ingest raw datasets and prepare them for semantic analysis.

**Responsibilities:**
- Load CSV / JSON files
- Extract sample rows
- Normalize column names
- Pass structured samples downstream

This agent ensures later stages operate on clean, consistent inputs.

---

### 2. Schema Analysis Agent

**Purpose:**  
Understand what each column represents.

**Responsibilities:**
- Infer semantic meaning of columns
- Distinguish dimensions vs measures
- Detect date, geography, population-related fields
- Reason using column names and example values

This agent replaces manual schema interpretation.

---

### 3. Dataset Form Detection Agent

**Purpose:**  
Determine dataset layout.

**Responsibilities:**
- Identify **long-form** vs **wide-form** datasets
- Decide how many statistical variables exist
- Influence how many TMCF nodes are generated

This step is critical because dataset form directly affects TMCF structure.

---

### 4. Role Classification Agent

**Purpose:**  
Map columns to TMCF roles.

**Responsibilities:**
- Assign roles such as:
  - observationAbout
  - observationDate
  - variableMeasured
  - value
- Ensure each statistical variable has a valid role mapping

This agent ensures semantic correctness of the mapping.

---

### 5. DCID Generation Agent

**Purpose:**  
Generate unique, schema-compliant identifiers for statistical variables.

**Responsibilities:**
- Infer:
  - measuredProperty
  - statType
  - populationType
  - unit
  - constraints (age, gender, etc.)
- Generate canonical DCIDs
- Ensure consistency across datasets

This is the most complex reasoning step in the pipeline.

---

### 6. TMCF / MCF Generation Agent

**Purpose:**  
Produce final metadata artifacts.

**Responsibilities:**
- Generate TMCF templates matching dataset structure
- Generate corresponding MCF nodes
- Ensure syntactic and semantic validity
- Prepare outputs for ingestion

---

## End-to-End Flow

1. Raw dataset is loaded  
2. Schema semantics are inferred  
3. Dataset form is detected  
4. Columns are mapped to semantic roles  
5. DCIDs are generated  
6. TMCF and MCF files are produced  

The number of generated TMCF nodes always matches the number of statistical variables detected.

---

## Key Design Principles

### Agent-Based Reasoning
Each agent focuses on **one cognitive task**, reducing complexity and improving accuracy.

### Schema-First Design
Metadata correctness is prioritized over raw data ingestion.

### LLM-Guided Inference
LLMs are used where deterministic rules fail — especially for semantic understanding.

### Scalability
The pipeline generalizes across datasets without hardcoding domain logic.

---

## Validation & Results

- Tested on **50+ heterogeneous public datasets**
- Achieved ~**80% schema compliance**
- Reduced manual metadata creation effort significantly
- Accelerated onboarding into knowledge graph systems

---

## Tech Stack (Conceptual)

- **Language:** Python  
- **Agent Orchestration:** LangGraph  
- **LLM Framework:** LangChain  
- **Inference Models:** Large Language Models (LLMs)  
- **Metadata Standards:** TMCF / MCF  
- **Execution Environment:** Cloud-native pipelines  

---

## What This Module Demonstrates

- Knowledge graph ingestion design
- Agentic reasoning over structured data
- Automated metadata generation
- Scalable semantic data onboarding

This project reflects real-world challenges in **public data standardization and semantic integration**, solved using agentic AI design.
