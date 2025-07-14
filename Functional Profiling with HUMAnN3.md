step1: Environment Setup\
create the biobakery3 conda environment:
```
conda create -n biobakery3 -c bioconda -c conda-forge humann
conda activate biobakery3
```

Step2: Download Databases\
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

Step5: Convert Gene Families to Metacyc:
```
humann_rename_table \
  --input humann3_output/SRR10403500_prinseq_good_out_R1_pathabundance.tsv \
  --output humann3_output/SRR10403500_prinseq_good_out_R1_pathabundance_named.tsv \
  --names metacyc-pwy
```























