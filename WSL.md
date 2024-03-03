# Installation de WSL (Windows Subsystem for Linux) sur Windows

## 1. Install the Virtual Machine Platform 
This can easily be done through an elevated PowerShell prompt with the following command :  
`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart` 

## 2. Install WSL 
Plusieurs façons d'installer WSL:
- **Mode Automatique (Windows 11) :**  
Lancer la commande PowerShell en tant que Administrateur `wsl --install`
- **Mode manuel :**
  - par ligne de commande PowerShell 
`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
  - par l'interface graphique:  ![image](https://github.com/leanh4/divers/assets/84317780/e95fb293-eda2-44f6-bfaf-ebd865e2e47a)
-  **Via Windows Store [Windows-subsystem-for-linux](https://apps.microsoft.com/detail/9p9tqf7mrm4r?hl=fr-fr&gl=FR) :**  
Pour faire fontionner systemd sur WSL, il faut installer WSL version Windows app store, sinon il y a l'erreur suivante :  
> System has not been booted with systemd as init system (PID 1). Can’t operate. Failed to connect to bus: Host is down.  

Lien vers WSL sur Microsoft Windows Store
[windows-subsystem-for-linux](https://apps.microsoft.com/detail/9p9tqf7mrm4r?hl=fr-fr&gl=FR)

## 2. Reboot
Reboot your Windows workstation to instantiate the components correctly.

## 3. Download and install the Linux Kernel update package
Récupérer et Installer  [WSL 2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)  

## 4.  Set the default version to WSL 2
The next step is to set the default WSL version to WSL 2. This is easily done with the command below:  
`wsl --set-default-version 2`

## 5. Install Ubuntu
1. At first, use the following command to list all the available Linux distros: `wsl --list --online` 
2. Install Ubuntu : `wsl --install -d Ubuntu-22.04`



## 6. Install Docker on WSL 2
Now for the part we have been waiting for, installing Docker. Just like any Ubuntu distribution, you can install Docker from the command line.
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker install 
sudo service docker start
sudo docker run hello-world

# If problem :
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy

sudo usermod -a -G docker $USER
sudo /etc/init.d/docker start
```

## 6. Commandes utiles de WSL
`wsl -t Ubuntu-22.04 ` : Terminate Distrib  
`wsl --shutdown` : Terminate toutes les Distrib  
`wsl --unregister <DistributionName>` :  Désinstaller DistributionName  
[https://learn.microsoft.com/fr-fr/windows/wsl/basic-commands](https://learn.microsoft.com/fr-fr/windows/wsl/basic-commands)



