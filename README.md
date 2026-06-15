# GraphRAG

A complete GraphRAG (Graph Retrieval-Augmented Generation) project for Hindi cinema. The app combines a Neo4j knowledge graph of Bollywood movies, actors, directors, composers, production houses, and awards with a retrieval-and-reasoning pipeline to answer plain-English questions about Indian films.

## What this project does

- Stores Bollywood entities and relationships in Neo4j
- Computes embeddings for graph nodes to support semantic retrieval
- Traverses the graph around the most relevant entities
- Uses an LLM to turn graph context into grounded answers
- Exposes the system through both a FastAPI backend and a Streamlit frontend

## Repository structure

src/
  - db.py        — Neo4j connection wrapper
  - loader.py    — Loads the ontology and Bollywood dataset into Neo4j
  - embeddings.py — Computes and stores node embeddings for vector search
  - graphrag.py  — Full GraphRAG pipeline (retrieve → traverse → answer)
  - api.py       — FastAPI backend for Q&A and graph APIs
  - app.py       — Streamlit UI for chat, exploration, and stats
  - data/data.py — Bollywood graph dataset (people, movies, awards, relations)

## How the GraphRAG pipeline works

1. Vector search
   - The user question is embedded.
   - The most relevant graph nodes are retrieved by semantic similarity.

2. Graph traversal
   - The system walks a small neighbourhood around each matched node.
   - Related facts are collected as structured triples and subgraph context.

3. Answer generation
   - The retrieved graph context is passed to an LLM.
   - The answer is generated from verified graph facts rather than free-form guessing.

## Example flow

User question → vector search → graph traversal → structured context → LLM answer

## Tech stack

- Python 3.11+
- Neo4j
- FastAPI
- Streamlit
- LangChain
- sentence-transformers / HuggingFace embeddings
- Groq-backed LLM in the current GraphRAG implementation

## Getting started

1. Install dependencies

   pip install -e .

2. Configure environment variables

   Set up the Neo4j connection details before running the app:

   - NEO4J_URI
   - NEO4J_USERNAME
   - NEO4J_PASSWORD

   If your LLM setup requires a provider key, make sure it is available in the environment as well.

3. Load the graph into Neo4j

   python src/loader.py

4. Generate embeddings for semantic search

   python src/embeddings.py

5. Start the backend

   python src/api.py

   The API will run on http://localhost:8000

6. Start the frontend

   streamlit run src/app.py

   The app will open on http://localhost:8501

## What you can ask

You can try questions such as:

- Which films did Aamir Khan and AR Rahman work on together?
- Which directors have won both Filmfare and National Awards?
- Which movies produced by Yash Raj Films feature Shah Rukh Khan?
- Which actors worked with both Rajkumar Hirani and Sanjay Leela Bhansali?

## Notes

The dataset in src/data/data.py covers a wide range of Hindi cinema from 1995 to 2024, including iconic films, producers, actors, directors, composers, and awards. The current GraphRAG implementation is designed to answer questions using graph-grounded evidence from that corpus.
