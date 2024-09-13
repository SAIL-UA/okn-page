---
layout: default
title: "Ontology Construction"
nav_order: 3
---

# Ontology Construction

This project’s aim is to process the **NSDUH 2022 data and codebook** to extract class structures as defined by domain experts. The extracted data will be used for ontology construction, specifically for mapping entities and relationships within the data.

## Main Features of this Project

### Class Structure Parsing from PDF into JSON

#### [updated_parse_en_clusters.py](https://github.com/SAIL-UA/OKN/blob/main/dataset/pdf-table-parser-main/updated_parse_en_clusters.py)

This script processes the NSDUH-2022-DS0001-info-codebook.pdf file to extract the class and variable mappings defined by domain experts. It reads specific pages from the PDF, processes the text to extract relevant class structures, and generates a JSON file (`category.json`) that organizes these structures hierarchically.

The JSON output is used for ontology construction tasks, particularly for entity and relation mapping.

### Installation

#### Set up Your Python Environment

Ensure you have Python installed on your system. It is recommended to use a virtual environment to avoid dependency conflicts.

```bash
python -m venv env
source env/bin/activate  # On Windows use `env\Scripts\activate`
