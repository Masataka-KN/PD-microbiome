# Functional and Taxonomic Profiling Pipeline Summary

This project uses **HUMAnN3** (BioBakery3) for **functional profiling** and **MetaPhlAn3** for **taxonomic profiling**.

## Tools and Databases Overview

| Tool / Database | Role | Notes |
|----------------|------|-------|
| **HUMAnN3** | Functional profiling pipeline | Converts sequencing reads into gene family and pathway abundances (MetaCyc, KEGG). |
| **MetaPhlAn3** | Taxonomic profiling | Identifies microbial species composition from metagenomic reads. HUMAnN3 internally calls MetaPhlAn. |
| **ChocoPhlAn** | Microbial pangenome database | Used by HUMAnN3 for mapping reads to microbial genes. Required for pathway reconstruction. |
| **UniRef90/50** | Protein family database | Used by HUMAnN3 to assign functions to unmapped reads via translated search (DIAMOND). |

---

## MetaPhlAn Version Management

- **MetaPhlAn marker databases (e.g., `mpa_v30_CHOCOPhlAn_201901`)** are downloaded during HUMAnN3 setup.
- **MetaPhlAn executable (`metaphlan` command)** is installed separately via `pip` or `conda`.

### Current Setup:

| Tool | Version | Status |
|------|---------|--------|
| **MetaPhlAn** | 3.0.14 | Installed manually. Compatible with HUMAnN3. |
| **HUMAnN3** | 3.x.x | Uses MetaPhlAn 3.x series. |
| **ChocoPhlAn** | mpa_v30_CHOCOPhlAn_201901 | For gene mapping and taxonomic profiling. |
| **UniRef** | uniref90/uniref50 (optional) | For translated search and functional mapping. |

---

## Notes on Compatibility

- **MetaPhlAn 3.0.14** is fully compatible with HUMAnN3 (recommended).
- Installing `MetaPhlAn` separately (even if it already exists in the HUMAnN3 environment) is **safe**, as long as the versions are consistent.
- **MetaPhlAn 4.x is NOT compatible with HUMAnN3.**

---

## Workflow Summary




step1: Environment Setup\
create the biobakery3 conda environment:
```
conda create -n biobakery3 -c bioconda -c conda-forge humann
conda activate biobakery3
```

Step2: Download Databases\
If you only want to do MetaCyc, you only need Humann3 and do not need to install uniref or ChocopLan.

ChocoPhlAn(Nucleotide database for species-specific genes)
```
mkdir -p ~/humann3_db/chocophlan
humann_databases --download chocophlan full ~/humann3_db/chocophlan
```

UniRef90(Protein databases for functional annotation)
```
mkdir -p ~/humann3_db/uniref
humann_databases --download uniref uniref90_diamond ~/humann3_db/uniref
```

Step3: Run HUMAnN3\
Use the R1 Fastq file after prinseq++ filtering
```
humann \
  --input QC/prinseq/SRR10403500_prinseq_good_out_R1.fastq \
  --output humann3_output \
  --threads 8
```
Note: HUMANn3 uses MetaPhlAn internally for taxonomic profiling from R1

Step4: MetaPhlAn Version Fix(if needed):\
In case of database compatibility issues. set MetaPhlAn to version 3.0.14
```
pip uninstall metaphlan -y
pip install metaphlan==3.0.14
```

Also, remove old database files if needed
```
rm /path/to/your/env/lib/python3.12/site-packages/metaphlan/metaphlan_databases/mpa_v31_CHOCOPhlAn_201901.*

```

Step5: MetaphlAn3(Taxonomic Profiling)\
Run MetaPhlAn3 to get species compositions:
```
metaphlan \
  QC/prinseq/SRR10403500_prinseq_good_out_R1.fastq.gz,QC/prinseq/SRR10403500_prinseq_good_out_R2.fastq.gz \
  --input_type fastq \
  --bowtie2db ./humann3_db/chocophlan \
  --bowtie2out metaphlan3_output/SRR10403500.bowtie2.bz2 \
  -o metaphlan3_output/SRR10403500_profile.txt
```

Step6: HuManN3(Functional Profiling)\
Run HuManN3 to generate MetaCyc pathway abundances.:
```
humann \
  --input QC/prinseq/SRR10403500_prinseq_good_out_R1.fastq.gz \
  --output humann3_output \
  --metaphlan-options "--bowtie2db ./humann3_db/chocophlan" \
  --nucleotide-database ./humann3_db/chocophlan \
  --protein-database ./humann3_db/uniref
```





















