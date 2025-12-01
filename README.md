# Gavel&Graph - AI Frontiers: LLM - Group 9
# Stare Decisis AI: Graph RAG for Texas Criminal Law

## Authors

Project developed for the **AI Frontiers: LLM** course.

  * **Claudia Brandetti**
  * **Denise Di Franza**
  * **Anastasia Farinaro**
  * **Daniele Sarcina**

> **Bridging the Semantic Gap in Legal Research with Hybrid Graph Retrieval-Augmented Generation.**
````markdown

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Neo4j](https://img.shields.io/badge/Neo4j-Graph%20Database-orange)
![Gemini](https://img.shields.io/badge/AI-Google%20Gemini%202.0-8E75B2)
![Streamlit](https://img.shields.io/badge/Frontend-Streamlit-red)
````

An intelligent legal assistant that combines semantic search with legal citation network analysis.

## Overview

**Stare Decisis AI** is an intelligent legal assistant designed to navigate the complexity of the US Common Law system. Unlike traditional keyword-based search engines, which fail to capture legal nuance, this project utilizes a **Graph RAG (Retrieval-Augmented Generation)** architecture.

By transforming  **7791 raw legal documents** (Texas Criminal Reports) into a Knowledge Graph, the system can identify relevant precedents based on **semantic similarity** (Vector Search) and **legal authority** (Citation Network Analysis), generating strategic defense memos in real-time.

---

## Key Features

* ** Hybrid Retrieval Engine:** Combines **Vector Search** (to find similar facts) with **Graph Traversal** (to find authoritative precedents cited by those cases).
* ** Citation Network Analysis:** Traverses the graph (Depth 1-2) to uncover the "legal pillars" behind a case, identifying winning strategies (e.g., *Reversed* judgements).
* ** Strategic Filtering:** Allows users to filter results based on the desired outcome (e.g., *Defense/Acquittal* vs. *Prosecution/Conviction*).
* ** LLM-Driven Extraction:** Utilizes Google Gemini 2.0 to extract structured metadata (Offense, Punishment, Decision, Conviction) from unstructured 19th-20th century texts.
* ** Interactive Visualization:** Features a dynamic graph visualization to explore the connections between the user's case and historical precedents.

---

##  Architecture

The system operates in two main phases: **The Builder** (Batch Processing) and **The Assistant** (Inference).

```mermaid
graph TD
    subgraph Data_Pipeline
        A[Raw JSON Data] -->|Parser| B(Data Cleaning)
        B -->|LLM Extraction| C{Gemini 2.0}
        C -->|Metadata| D[Neo4j Graph]
        C -->|Text| E[Vector Embeddings]
        E --> D
    end
    subgraph Inference_Engine
        User[User Query] -->|Embedding| Search[Hybrid Search]
        Search -->|Vector Sim| Anchor[Anchor Cases]
        Anchor -->|Graph Traversal| Prec[Precedents]
        Prec -->|Context Injection| Gen[Gemini Generation]
        Gen --> Output[Strategic Memo]
    end
````

-----

##  Tech Stack

  * **Language:** Python 3.10+
  * **Database:** Neo4j (Graph DB + Vector Index)
  * **LLM & Embeddings:** Google Gemini 2.0 Flash & `text-embedding-004`
  * **Frontend:** Streamlit & Streamlit-Agraph
  * **Data Source:** [Case.law](https://case.law) (Harvard Caselaw Access Project)

-----

##  Project Structure

```bash
├── data_pipeline/
│   ├── case_law_crawler_tx.py       # Downloads raw cases
│   ├── json_to_csv_parser_tx.py     # Converts JSON to CSV
│   ├── llm_extraction_tx.py         # Extracts Offense/Decision via LLM
│   └── create_embeddings_final.py   # Generates Vectors & Indexing
├── app.py                           # Main Streamlit Application (RAG Engine)
├── requirements.txt                 # Dependencies
└── README.md                        # Documentation
```

-----

## Quick Start

### 1\. Prerequisites

  * Python 3.10+
  * A running **Neo4j Database** (Desktop or AuraDB).
  * A **Google Gemini API Key**.

### 2\. Installation

Clone the repository:

```bash
git clone [https://github.com/denise-df/Gavel-Graph.git](https://github.com/enise-df/Gavel-Graph.git)
cd Gavel-Graph
```

Install dependencies:

```bash
pip install -r requirements.txt
```

### 3\. Configuration

Create a `key.txt` file (or set environment variables) for your API keys:

```text
# Inside key.txt
YOUR_GEMINI_API_KEY
```

Create a `neo4j_pass.txt` for your database password.

### 4\. Running the App

Once the database is populated (see `data_pipeline` scripts), launch the interface:

```bash
streamlit run app.py
```
![Immagine WhatsApp 2025-11-30 ore 13 25 56_4684df20](https://github.com/user-attachments/assets/70cf8971-3a13-4e2e-ba61-6703fda7e7ea)
![Immagine WhatsApp 2025-11-30 ore 13 29 35_995768eb](https://github.com/user-attachments/assets/65264338-fc82-46f1-a209-40fd8010808f)



-----

## Evaluation & Results

We conducted an ablation study comparing **Vector-Only Search** vs. **Graph RAG**.

  * **Vector Search:** Efficient at finding textually similar descriptions but often retrieves legally irrelevant cases (e.g., finding *Burglary* cases with "sleeping victims" instead of "sleeping defendants").
  * **Graph RAG:** Significantly improves relevance by retrieving the *authoritative cases* cited by the vector matches, successfully identifying procedural defenses (e.g., *McLemore v. State*) that standard search misses.

-----

*Disclaimer: This tool is for academic and educational purposes only and does not constitute professional legal advice.*
