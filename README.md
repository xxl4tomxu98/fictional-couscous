# fictional-couscous
Overview
A Neo4j implementation of GraphRAG approach for knowledge graph-based retrieval augmented generation.

Extract entities and relationships from unstructured text
Build a knowledge graph in Neo4j using LLM
Generate summaries for nodes and relationships
Detect and summarize communities within the graph
Leverage this graph structure for enhanced RAG
The implementation uses Anthropic and OpenAI's models for text processing and Neo4j's powerful graph capabilities including the Graph Data Science (GDS) library.

Installation
pip install -e .
Requirements
Neo4j Aura or Desktop database (5.26+)
APOC plugin installed in Neo4j
Graph Data Science (GDS) library installed in Neo4j
OpenAI API key

Quick Start
import os

from fictional-couscous import MyGraphRAG
from neo4j import GraphDatabase

# Set your environment variables
os.environ["OPENAI_API_KEY"] = "your-openai-api-key"
os.environ["NEO4J_URI"] = "neo4j://127.0.0.1:7687"
os.environ["NEO4J_USERNAME"] = "neo4j"
os.environ["NEO4J_PASSWORD"] = "password"

# Connect to Neo4j
driver = GraphDatabase.driver(
    os.environ["NEO4J_URI"], 
    auth=(os.environ["NEO4J_USERNAME"], os.environ["NEO4J_PASSWORD"])
)

# Initialize MsGraphRAG
kb_graph = MyGraphRAG(driver=driver, model='gpt-4o')

# Define example texts and entity types
example_texts = [
    "Tom is an American",
    "Tom lives in Washington DC", 
    "Tom went to school in Delaware"
]
allowed_entities = ["Person", "Nationality", "Location"]

# Extract entities and relationships
result = kb_graph.extract_nodes_and_rels(example_texts, allowed_entities)
print(result)

# Generate summaries for nodes and relationships
result = kb_graph.summarize_nodes_and_rels()
print(result)

# Identify and summarize communities
result = kb_graph.summarize_communities()
print(result)

# Close the connection
kb_graph.close()
How It Works
Extract Nodes and Relationships: The library uses Anthropic and OpenAI's models to extract entities and relationships from your text data, creating a structured graph.

Summarize Nodes and Relationships: Each entity and relationship is summarized to capture its essence across all mentions in the source documents.

Community Detection: The Leiden algorithm is applied to identify communities of related entities.

Community Summarization: Each community is summarized to provide a high-level understanding of the concepts it contains.