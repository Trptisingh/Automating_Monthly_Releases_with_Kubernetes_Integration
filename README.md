<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=24&pause=1000&color=00FF00&center=true&width=800&lines=Automating+Monthly+Releases+with+Kubernetes+Integration" />
</p>

# Automating Monthly Releases with Kubernetes Integration
![Terraform](https://img.shields.io/badge/Terraform-0.14-blue?style=flat-square&logo=terraform)
![Ansible](https://img.shields.io/badge/Ansible-2.9-red?style=flat-square&logo=ansible)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-orange?style=flat-square&logo=jenkins)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.22-blue?style=flat-square&logo=kubernetes)
![Prometheus](https://img.shields.io/badge/Monitoring-Prometheus-red?style=flat-square&logo=prometheus)
![Grafana](https://img.shields.io/badge/Dashboard-Grafana-yellow?style=flat-square&logo=grafana)
![Git](https://img.shields.io/badge/Git-Version%20Control-orange?style=flat-square&logo=git)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue?style=flat-square&logo=docker)
![Bash](https://img.shields.io/badge/Bash-Scripting-green?style=flat-square&logo=gnu-bash)

This project automates the deployment and release cycle using Terraform, Ansible, Jenkins, and Kubernetes. It provisions Terraform infrastructure, configures instances with Ansible, sets up a Jenkins CI/CD pipeline, and integrates Kubernetes for scalable deployments.

---
## Table of Contents
- [Infrastructure Setup](#infrastructure-setup)
- [Installation](#installation)
- [Configuration](#configuration)
- [CI/CD Pipeline](#cicd-pipeline)
- [Monitoring and Analytics](#monitoring-and-analytics)
- [Conclusion](#conclusion)

# Project Requirements  

## 🏗️ 1. Infrastructure Requirements  

To ensure seamless deployment and automation, the following infrastructure components are required:  

### 🖥️ Compute Instances  

| Instance                         | Role & Purpose                          | CPU  | RAM   | Storage  | OS                  |
|----------------------------------|------------------------------------|------|------|---------|--------------------|
| **Jenkins_Terraform_Ansible** | Automation & Provisioning           | 4 vCPUs  | 8GB   | 50GB SSD  | Ubuntu 22.04 / CentOS 8  |
| **Kubernetes Master (Kmaster)** | Controls Kubernetes Cluster        | 4 vCPUs  | 16GB  | 100GB SSD | Ubuntu 22.04 / CentOS 8  |
| **Kubernetes Worker (Kslave1)** | Runs containerized applications    | 4 vCPUs  | 8GB   | 100GB SSD | Ubuntu 22.04 / CentOS 8  |
| **Kubernetes Worker (Kslave2)** | Ensures load balancing & scaling  | 4 vCPUs  | 8GB   | 100GB SSD | Ubuntu 22.04 / CentOS 8  |

### 🌐 Networking & Security  

- **Private IP Connectivity** – All nodes must communicate securely.  
- **Firewall Rules:**  
  - **Jenkins:** `8080`  
  - **Kubernetes API:** `6443`  
  - **Prometheus:** `9090`  
  - **Grafana:** `3000`  
  - **SSH Access:** `22` (for remote management)  
- **VPC/Subnet Configuration** – For Kubernetes networking.  

---

## 🛠️ 2. Software & Tools Requirements  

### Infrastructure as Code (IaC)  
- **Terraform (v0.14+)** – Infrastructure provisioning.  
-  **Ansible (v2.9+)** – Automated configuration management.  

### CI/CD Pipeline  
- **Jenkins** – Continuous Integration & Deployment.  
-  **Git** – Version control & repository management.  
- **Docker** – Containerized builds & deployments.  

### Kubernetes Ecosystem  
- **Kubernetes (v1.22+)** – Container orchestration.  
- **Kubectl** – Kubernetes cluster management.  
- **Kubeadm** – Kubernetes cluster setup.  

### Monitoring & Logging  
-  **Prometheus** – System monitoring & alerting.  
-  **Grafana** – Performance visualization dashboard.  

---

## 🔐 3. Access & Authentication Requirements  
-  **SSH Key-Based Authentication** – Secure communication between nodes.  
-  **Jenkins User Permissions** – Grant access for pipeline execution.  
- **Kubernetes RBAC (Role-Based Access Control)** – Secure cluster operations.  

---


---
## 🚀 Infrastructure Setup  

###  Instances & Roles  

| Instance                        | Role & Responsibilities                                  |
|----------------------------------|----------------------------------------------------------|
| **Jenkins_Terraform_Ansible** | Manages automation, infrastructure provisioning, and CI/CD pipeline. |
| **Kmaster**                   | Kubernetes Master Node – Controls cluster management and API server. |
| **Kslave1**                   | Kubernetes Worker Node 1 – Runs workloads and manages containerized applications. |
| **Kslave2**                   | Kubernetes Worker Node 2 – Ensures load distribution and scalability. |

### Networking Requirements
- Ensure all nodes can communicate over SSH and required ports.
- Open ports for Jenkins (8080), Kubernetes API, and other necessary services.

---
## Installation
### **Step 1: Provision Infrastructure with Terraform**
#### Install Terraform:
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform -y
```

#### Define Infrastructure in `main.tf`:
> [!NOTE]
> Available in the Repository with the name of the ***main.tf***
#### Apply Terraform Configuration:
```bash
terraform init
terraform plan
terraform apply
```

---
## Configuration
### **Step 2: Install Ansible & Configure Cluster**
#### Install Ansible on `Jenkins_Terraform_Ansible`:
```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

#### Set Up SSH Key-Based Authentication:
```bash
ssh-keygen
sudo cat ~/.ssh/id_rsa.pub
```
Copy the key to `~/.ssh/authorized_keys` on **Kmaster, Kslave1, Kslave2**.

#### Configure Ansible Inventory (`/etc/ansible/hosts`):
```ini
[kmaster]
<Private-IP-of-Kmaster>

[workers]
<Private-IP-of-Kslave1>
<Private-IP-of-Kslave2>
```

#### Test Connectivity:
```bash
ansible -m ping all
```

---
## CI/CD Pipeline
### **Step 3: Deploy Jenkins & Kubernetes**

The installation process is automated using an **Ansible playbook (`playbook.yaml`)**. Below are the required setup scripts, which are available in the repository.

#### Install Java, Jenkins & Docker on `Jenkins_Terraform_Ansible`
> 📜 **Note:** The installation is managed by the script [`Jenkins_terraform_ansible.sh`](./Jenkins_terraform_ansible.sh).  

#### Install Java & Docker on `Kmaster`
> 📜 **Note:** The installation is managed by the script [`Kmaster.sh`](./Kmaster.sh).  
---

### **Step 1: Perform Syntax Check**
Before running the playbook, check for any syntax errors:

```bash
ansible-playbook playbook.yaml --syntax-check

## Monitoring and Analytics
### **Step 4: Install Prometheus & Grafana**
#### Install Prometheus:
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.34.0/prometheus-2.34.0.linux-amd64.tar.gz
tar xvfz prometheus-2.34.0.linux-amd64.tar.gz
cd prometheus-2.34.0.linux-amd64
./prometheus --config.file=prometheus.yml
```

#### Install Grafana:
```bash
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

- Access Grafana at `http://<Your-Instance-IP>:3000`
- Default Login: `admin` / `admin`

---
## Conclusion
By integrating Terraform, Ansible, Jenkins, Kubernetes, and monitoring tools, this project establishes an efficient CI/CD pipeline for automating monthly releases with Kubernetes. This setup ensures scalable deployments, seamless automation, and real-time monitoring, boosting software delivery efficiency.

