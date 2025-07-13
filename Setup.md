**Environmental Setup Guide**

**1. Install WSL2 and Ubuntu (for Windows users)**\
purpose : Set up a Linux environment on Windows using WSL2 and Ubuntu.
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

Step3: Launch Ubuntu
Once started, open Ubuntu from the start menu.
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










