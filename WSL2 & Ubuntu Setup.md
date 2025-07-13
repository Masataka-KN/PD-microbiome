**Environmental Setup Guide**

**1. Install WSL2 and Ubuntu (for Windows users)**\
purpose : Set up a Linux environment on Windows using WSL2 and Ubuntu.\
          This will be the foundation for further bioinformatics or data analysis workflows.

Step1: Install WSL2
Open Windows PowerShell as Administrator and run:
```PowerShell
wsl --install
```
After installation, restart your PC when prompted.

Step2: Verify WSL2 Installation
```PowerShell  
wsl -l -v  
```
Example output:
```
NAME      STATE       VERSION
Ubuntu    Runnig      2
```
If you see Ubuntu and VERSION2, WSL2 is installed collectly.

Step3: Launch Ubuntu\
Once started, open Ubuntu from the start menu.\
You will be asked to set a username and password(use alphanumeric characters.)

Step4: Check Ubuntu Verson\
Inside the Ubuntu, verify the OS version:
```Ubuntu  
cat /etc/os-release
```

Example output:
```  
PRETTY_NAME="Ubuntu 24.04.2 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.2 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```
If the output looks like similar, your Ubuntu environment is ready.

Step5: Update and Upgrade System Packages
```Ubuntu
sudo apt update
sudo apt upgrade --y
```
Purpose: Keeping the system up-to-date ensures better compatibilit and security.

Step6: Install Essential Packages
```Ubuntu
sudo apt install build--essential wget unzip git python3-pip htop
```
| Package      | Purpose                                |
|--------------|----------------------------------------|
| build-essential | Compiler and build tools (gcc, g++, make, etc.) |
| wget         | For downloading files via HTTP/HTTPS   |
| unzip        | For extracting `.zip` files            |
| curl         | For data transfer via HTTP/HTTPS       |
| git          | For version control                    |
| python3-pip  | Python package manager                 |
| htop         | Interactive process viewer             |


Step6: Install Miniconda
```Ubuntu
cd ~
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
After installation, either:
Restart the terminal, or\
Run:
```Ubuntu
source ~/.bashrc
```

Step7: Install Entrez Direct (EDirect)
Entrez Direct(EDirect) is a suite of UNIX command-line tools for accessing NCBI's databases\
programmatically.
```Ubuntu
sh -c "$(curl -fsSL https://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh)"
```

Add EDirect to PATH
```Ubuntu
echo 'export PATH=$PATH:$HOME/edirect' >> ~/.bashrc
source ~/.bashrc
```
Key Tools in EDirect:
| Command  | Function |
|----------|----------|
| **esearch** | Performs text-based searches in NCBI databases (e.g., search for genes, organisms, SRA entries). |
| **efetch**  | Retrieves detailed data based on `esearch` results (e.g., FASTA sequences, GenBank records, metadata). |












