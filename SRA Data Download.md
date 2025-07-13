Step1: Miniconda Installation
```Ubuntu
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Step2: Install Sra-toolkit, pigz(Oarallel gzip)
```Ubuntu
sudo apt install sra-toolkit
```
Purpose: Tools for downloading and converting data from NCBI's Sequence Read Archive (SRA).

Altenative (recommended for latest version):
```Ubuntu
conda install -c bioconda sra-tool
```
```Ubuntu
sudo apt install pigz
```

Step3: Download SRA Data\
Create a directory for SRA Data
```Ubuntu
mkdir -p ~/sra_files
```
Download sra file using prefetch
```
prefetch SRR10403500 -O ~/sra_files
```

Step4: Convert .sra to FASTQ\
Create output sirectory for FASTQ:
```
mkdir -p ~/fastq_data
```
Convert .sra to pired-end FASTQ and use multi-threading:
```
fasterq-dump ~/sra_files/SRR10403500.sra -e 8 -p --split-files -O ~fastq_data
```
-e-8 : Use 8 threads\
--split-files : Output paired-end reads as _1.fastq and _2.fastq

Step5: Compress FASTQ Files using pigz:
```
pigz -p 8 ~/fastq_data/SRR10403500_1.fastq
pigz -p 8 ~/fastq_data/SRR10403500_2.fastq
```
-p 8 : Use 8 CPU cores for compression

Step6: Alternative: Direct FASTQ Dpwnload from ENA\
In some cases. you can download Fastq.gz files directly from ENA(faster and simpler)
```
cd ~/fastq_data
wget https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR104/000/SRR10403500/SRR10403500_1.fastq.gz
wget https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR104/000/SRR10403500/SRR10403500_2.fastq.gz
```

Step7: Check File Sizes and Contents
```
ls -lh ~/sra_files/
du -sh ~/sra_files/SRR10403500/SRR10403500.sra
```

Step8: Inspect Reads\
Use vb-dump to display the first read in the sra file
```
vdb-dump -R 1 -C READ ~/sra_files/SRR10403500/SRR10403500.sra
```

Step9: Organize FASTQ Files\
Move compressed Fastq files into data/ directory
```
mkdir data QC
mv SRR10403500_1.fastq.gz data/
mv SRR10403500_2.fastq.gz data/
```

###Monitor Disk Usage\
check disk usage every 10 seconds:
```
watch -n -10 'du -h ~sra_files'
```























