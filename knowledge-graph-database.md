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

## Targeted Dataset

As an example, consider the NSDUH (National Survey on Drug Use and Health) dataset, available at the official [NSDUH website](https://www.samhsa.gov/data/data-we-collect/nsduh-national-survey-drug-use-and-health). Conducted annually by the Substance Abuse and Mental Health Services Administration (SAMHSA), this survey provides nationally representative data on various topics including the use of tobacco, alcohol, and drugs; substance use disorders; mental health issues; and the receipt of substance use and mental health treatment. The survey covers the civilian, noninstitutionalized population aged 12 or older in the United States.

The NSDUH dataset offers two main resources:

- **The Dataset**: This consists of the formatted survey results, where each row corresponds to a single, anonymous respondent's survey, and each column represents a question in the survey, encoded as a variable code.

- **The Codebook**: This resource provides detailed explanations for each variable code, along with statistical summaries of the responses. The preface of the codebook also introduces foundational information about the survey, including statistical methods, relevant regulations, and policies related to security and privacy.


## Ontology and Knowledge Graph Generating




## Database Design

The database design follows the KG paradigm that entities and relationships are mantained in specific tables. 

Take the NSDUH dataset for example, the main topics of this dataset are substance abuse and mental health. 

After constructing the Ontology based on the NSDUH codebook, the incident types about the substance abuse are extracted and 

## Tools

Some tools for knowledge graph databases include...

