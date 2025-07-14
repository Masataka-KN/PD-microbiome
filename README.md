
## 📖 Overview

This repository contains code, pipeline, and documentation for metagenomic and 16S rRNA microbiome analysis related to Parkinson's Disease (PD).  
Our goal is to compare the gut microbiota between PD patients and healthy controls, and investigate functional changes such as CsgA-related metabolism.

## 🧪 Project Structure

PD-microbiome/
├── data/ # Raw data (not included in this repository)
├── scripts/ # Analysis scripts (Python/R/Bash)
├── results/ # Figures and processed data
└── docs/ # Supplementary materials and diagrams


## ⚙️ Requirements

- OS: Ubuntu 22.04 / WSL2
- Tools:
    - QIIME2
    - HUMAnN2 / HUMAnN3
    - Kraken2 / Bracken
    - MetaBAT2 (for MAG construction)
    - Python 3.x
    - R 4.x
- Package list: See `requirements.txt` (planned)

## 🚀 How to Run

1. Clone this repository:

2 Prepare data:

Place raw FASTQ files into data/ (this folder is ignored by Git).

3 Run analysis:

Execute scripts in scripts/ according to the order below:

scripts/
├── preprocessing.sh   # Quality control (fastp, trimmomatic, etc.)
├── taxonomic_profile.py   # Kraken2 / Bracken analysis
├── functional_profile.py  # HUMAnN2 pipeline
└── diversity_analysis.R   # Alpha and beta diversity analysis

4 Figures and tables will be stored in results/

Processed scripts and pipeline logs are tracked in Git

Goals Compare alpha diversity between PD and healthy control

Detect changes in short-chain fatty acid (SCFA) pathways

Analyze amyloid-related genes (e.g., CsgA)

Build functional networks (MetaCyc, KEGG)
