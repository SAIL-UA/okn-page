---
layout: default
title: "Knowledge Graph Database"
nav_order: 4
---

# Knowledge Graph Database

This section covers how to work with knowledge graph databases.

## Introduction

In this project, all Knowledge Graph components are maintained within a PostgreSQL database. There are three key considerations that led to the decision to adopt PostgreSQL for managing the Knowledge Graph:

1. **Dual functionality for knowledge representation and RAG queries**: The Knowledge Graph must support both knowledge representation and Retrieval-Augmented Generation (RAG) queries. A relational database is ideal for structuring the Knowledge Graph in our project. By aggregating triples from specific domains or categories into dedicated tables, we can follow a classic waterfall pattern for Knowledge Graph generation. Additionally, components such as chunks, entity descriptions, and more can be easily managed using the same relational database, allowing one backend database to serve both systems simultaneously.

2. **Long-term scalability and evolution**: The OKN project is designed to be a long-term initiative, with the Knowledge Graph expected to grow and evolve over time. As intermediate results are generated and analyzed for the continued development of the Knowledge Graph, a mature and stable database is necessary to manage fragmented data that may need refinement in future processes. PostgreSQL provides this stability and is well-suited to handling growing data volumes and complexity.

3. **Community support and alignment with other teams**: PostgreSQL boasts a strong and active community, and its use is widespread among other OKN teams. By adopting PostgreSQL for our Knowledge Graph, we simplify future collaboration and data alignment with other teams working on related projects.

## Tools

Some tools for knowledge graph databases include...

