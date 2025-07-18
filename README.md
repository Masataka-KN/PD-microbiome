## Environment and Version Information

### Operating System
- **OS:** Windows 11 Pro
- **WSL Version:** 2.5.9.0
- **WSL Kernel:** 6.6.87.2-1
- **WSLg Version:** 1.0.66
- **Ubuntu 24.04.2 LTS**
- **Miniconda**
- **Conda 25.5.1**

### Main Tools and Versions

| Tool | Version | Description |
|---|---|---|
| **FastQC** | 0.12.1 | Quality check of FASTQ files |
| **MultiQC** | 1.30 | Summarizing QC reports |
| **Kneaddata** | 0.12.3 | Host (human) and PhiX contamination removal |
| **Prinseq++** | 1.2 | Quality filtering and deduplication |
| **Trim Galore** | 0.6.10 | Adapter and quality trimming |
| **Cutadapt** | 5.1 | Adapter trimming backend (used by Trim Galore) |
| **pigz** | 2.8 | Parallel gzip compression |
| **MetaPhlAn** | 3.0.14 | Taxonomic profiling |
| **HUMAnN** | 3.9 | Functional profiling (MetaCyc, KEGG) |
| **sra-tools** | 3.0.3 | SRA data retrieval (prefetch / fasterq-dump) |

### Python Versions

| Environment | Python Version | Purpose |
|---|---|---|
| **biobakery3 (HUMAnN / MetaPhlAn environment)** | Python 3.12.11 | For HUMAnN3 and MetaPhlAn3 |
| **Trim Galore environment** | Python 3.10.18 | To avoid compatibility issues with Cutadapt |
| **Base environment** | Python 3.13.5 | For Quality Control |


