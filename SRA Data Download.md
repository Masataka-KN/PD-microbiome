Step1: Miniconda Installation:
```Ubuntu
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```



Step2: Install Sra-toolkit, pigz(Oarallel gzip)
Sra-toolkit:
```Ubuntu
sudo apt install sra-toolkit
```
Purpose: Tools for downloading and converting data from NCBI's Sequence Read Archive (SRA).

Altenative (recommended for latest version):
```Ubuntu
conda install -c bioconda sra-tool
```
###
Do not mix apt install sra-toolkit and conda install sra-tools.
Use conda for consistency.

pigz:
```Ubuntu
sudo apt install pigz
```


Step3: Download SRA Data\
Create a directory for SRA Data
(Probably no need to manually create a folder for SRR10403500, but mentioned)
```Ubuntu
mkdir -p ~/sra_files
mkdir -p ~/sra_files/SRR10403500
```
Download sra file using prefetch:
```
prefetch SRR10403500 -O ~/sra_files
```
Check downloaded files:
```
ls -lh ~/sra_files/
```

**SRA File**
For details on the [sra](https://www.ncbi.nlm.nih.gov/sra/docs/submitformats/) format, refer to the official NCBI documentation.

In SRA data management, each Run (SRR) has its own unique data structure and metadata.\
Therefore, it is recommended to create separate folders for each Run to manage the data independently.

**Checking Download Progress**\
⓵Check file size:
```
ls -lh ~/sra_files/SRR10403510
```
⓶Real-time monitoring(every 5s):
```
watch -n 5 "ls -lh ~/sra_files/SRR10403500"
```
⓷Monitor CPU and I/O usage:
```
htop
```


Step4: Convert .sra to FASTQ\
Create output sirectory for FASTQ:
```
mkdir -p ~/fastq_data
```
Convert .sra to pired-end FASTQ and use multi-threading:
```
fasterq-dump ~/sra_files/SRR10403517/SRR10403517.sra -e 8 -p --split-files -O ~/fastq_data/SRR10403517
```
-e-8 : Use 8 threads\
--split-files : Output paired-end reads as _1.fastq and _2.fastq

Check file contents:
```
ls -lh ~/fastq_data/SRR10403500/
```

Step5: Compress FASTQ Files using pigz:
```
pigz -p 8 ~/fastq_data/SRR10403500_1.fastq
pigz -p 8 ~/fastq_data/SRR10403500_2.fastq
```
-p 8 : Use 8 CPU cores for compression

Check file contents:
```
ls -lh ~/fastq_data/SRR10403500/

SRR10403500_1.fastq.gz
SRR10403500_2.fastq.gz
```

Step6: Alternative: Direct FASTQ Dpwnload from ENA\
In some cases. you can download Fastq.gz files directly from ENA(faster and simpler):
```
cd ~/fastq_data
wget https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR104/000/SRR10403500/SRR10403500_1.fastq.gz
wget https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR104/000/SRR10403500/SRR10403500_2.fastq.gz
```


Step7: Inspect Reads\
Use vb-dump to display the first read in the sra file
```
vdb-dump -R 1 -C READ ~/sra_files/SRR10403500/SRR10403500.sra
```
-R 1 : Display only the first record(read)
-C READ : Show only the READ column(the sequence data)

vdb-dump is automatically installed together with sra-tools


Step8: Organize FASTQ Files\
Move compressed Fastq files into data/ directory:
```
mkdir -p data QC
mv SRR10403500_1.fastq.gz data/
mv SRR10403500_2.fastq.gz data/
```
QC folder is created for quality control results.
























