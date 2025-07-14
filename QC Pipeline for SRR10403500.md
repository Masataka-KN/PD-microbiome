Step1: Environmental Setup\
I used conda to install all necessary tools in one environment to avoid version conflicts
```
conda create -n qcenv -c bioconda -c conda-forge python=3.10 \
    fastqc multiqc fastp trim-galore prinseq-plus-plus kneaddata bowtie2 pigz seqkit -y

conda activate qcenv
```

Step2: Quality Check - FastQC & MultiQC\
i visualize the WGS data before QC
```
mkdir -p QC/fastqc_raw

fastqc -t 8 -o QC/fastqc_raw data/SRR10403500_1.fastq.gz data/SRR10403500_2.fastq.gz

multiqc QC/fastqc_raw -o QC/multiqc_raw
```

Step3: Adapter and Quality Trimming - Trim Galore
The dependency package for trim_galore does not yet support python=3.13, \
so create python=3.10 in a separate environment.
```
mkdir -p QC/trim_galore

trim_galore --paired --cores 8 --quality 20 --gzip \
  --basename SRR10403500 -o QC/trim_galore \
  data/SRR10403500_1.fastq.gz data/SRR10403500_2.fastq.gz
```

Step4: Contaminatio Removal - Kneaddata(Human Genome & Phix)\
Dowload and Build Databases- Prepare both Phix and Human genome Bowtie2 incides.\
For Phix:
```
esearch -db nucleotide -query "NC_001422.1" | efetch -format fasta > phix.fasta
bowtie2-build phix.fasta phix
mkdir -p ~/kneaddata_db/phix
mv phix.* ~/kneaddata_db/phix/
```
For Human Genome\
 Download from kneaddata's prebuilt human genome detabases

Run Kenaddata:
```
kneaddata \
  -i data/SRR10403500_1.fastq.gz \
  -i data/SRR10403500_2.fastq.gz \
  -o kneaddata_output \
  -db ~/kneaddata_db/human_genome \
  -db ~/kneaddata_db/phix \
  --threads 8
```

Step5: Prinseq++
```
mkdir -p QC/prinseq

prinseq++ \
  -threads 8 \
  -fastq kneaddata_output/SRR10403500_1_kneaddata_unmatched_1.fastq \
  -fastq2 kneaddata_output/SRR10403500_1_kneaddata_unmatched_2.fastq \
  -min_len 50 \
  -min_qual_mean 20 \
  -derep \
  -out_gz \
  -out_name QC/prinseq/SRR10403500_prinseq
```

Step6: After QC - FastQC & MultiQC
```
mkdir -p QC/fastqc_prinseq

fastqc -t 8 QC/prinseq/*.fastq.gz -o QC/fastqc_prinseq

multiqc QC/fastqc_prinseq -o QC/multiqc_prinseq
```


**Troubleshooting: ICU Library Errors in FastQC**
FastQC uses libicu for:

Generating reports with proper string encoding\
Handling internationalized text or character sorting\
Possibly for GUI elements or PDF exports (even in CLI mode, some dependencies remain)

Common Error:\
When running FastQC, you might encounter:
```
error while loading shared libraries: libicui18n.so.56: cannot open shared object file: No such file or directory
```
This happens because FastQC expects an older ICU version (56), but recent Linux distributions provide ICU 74 or newer.

Solution: Create Symbolic Links:\
This happens because FastQC expects an older ICU version (56), but recent Linux distributions provide ICU 74 or newer.
```
sudo ln -sf /usr/lib/x86_64-linux-gnu/libicuuc.so.74 /usr/lib/x86_64-linux-gnu/libicuuc.so.56
sudo ln -sf /usr/lib/x86_64-linux-gnu/libicui18n.so.74 /usr/lib/x86_64-linux-gnu/libicui18n.so.56
sudo ln -sf /usr/lib/x86_64-linux-gnu/libicudata.so.74 /usr/lib/x86_64-linux-gnu/libicudata.so.56
```





















