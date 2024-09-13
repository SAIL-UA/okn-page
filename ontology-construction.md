---
layout: default
title: "Ontology Construction"
nav_order: 3
---

# Ontology Construction

This project’s aim is to process the **NSDUH 2022 data and codebook** to extract class structures as defined by domain experts. The extracted data will be used for ontology construction, specifically for mapping entities and relationships within the data.

## Main Features of this Project

### Class Structure Parsing from PDF into JSON

#### [code_book_class_structure_extraction.py](https://github.com/SAIL-UA/OKN/blob/main/ontology/code_book_class_structure_extraction.py)

This script processes the NSDUH-2022-DS0001-info-codebook.pdf file to extract the class and variable mappings defined by domain experts. It reads specific pages from the PDF, processes the text to extract relevant class structures, and generates a JSON file (`category.json`) that organizes these structures hierarchically.

The JSON output is used for ontology construction tasks, particularly for entity and relation mapping.

### Installation

To install the necessary dependencies, run the following command:

```bash
pip install pymupdf pandas
```

### Run script

```bash
python code_book_class_structure_extraction.py --pdf_file /path/to/NSDUH-2022-DS0001-info-codebook.pdf

```
