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

This project focuses on automating the deployment and release cycle using Terraform, Ansible, Jenkins, and Kubernetes. It provisions infrastructure using Terraform, configures instances with Ansible, sets up a Jenkins CI/CD pipeline, and integrates Kubernetes for scalable deployments.

---
## Table of Contents
- [Infrastructure Setup](#infrastructure-setup)
- [Installation](#installation)
- [Configuration](#configuration)
- [CI/CD Pipeline](#cicd-pipeline)
- [Monitoring and Analytics](#monitoring-and-analytics)
- [Conclusion](#conclusion)

---
## Infrastructure Setup
### Instances & Roles
1. **Jenkins_Terraform_Ansible** - Manages automation and infrastructure provisioning.
2. **Kmaster** - Kubernetes Master Node.
3. **Kslave1** - Kubernetes Worker Node 1.
4. **Kslave2** - Kubernetes Worker Node 2.

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
#### Install Jenkins & Docker on `Jenkins_Terraform_Ansible`:
```bash
sudo apt update
sudo apt install openjdk-17-jre -y
download Jenkins
wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt update
sudo apt install jenkins -y
sudo apt install docker.io -y
```

#### Install Kubernetes on `Kmaster` & `Kslave`:
```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
```

#### Initialize Kubernetes Cluster on `Kmaster`:
```bash
sudo kubeadm init --pod-network-cidr=192.168.1.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### Join Worker Nodes (`KslaveX`) to Cluster:
```bash
kubeadm join <Kmaster-IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash <HASH>
```

---
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

