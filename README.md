\# 🚀 Multi-OS Jenkins Setup (Ubuntu + Rocky Linux)
This project demonstrates the implementation of a distributed Jenkins architecture on AWS EC2, featuring an Ubuntu-based controller and a Rocky Linux (CentOS-based) agent. It showcases cross-platform job execution using secure SSH-based communication.

Architecture
============

[ Jenkins Controller (EC2 - t3.large) ]
                |
                |  SSH (Key-based authentication)
                ↓
[ Jenkins Agent (EC2 - t3.large) ]
[ Jenkins Agent (Rocky Linux EC2) ]

- Controller schedules jobs
- Agent executes builds
- Communication via SSH keys

\---
Technologies Used
=================

\* Jenkins

\* AWS EC2

\* Ubuntu Linux

\* Rocky-9

\* SSH

\* Git

Step-by-Step Setup
==================

1️⃣ Launch EC2 Instances

\* Instance Type: t3.large

\* OS: Ubuntu

\* Security Group:



&#x20; \* Port 22 (SSH)

&#x20; \* Port 8080 (Jenkins UI)

\### 2️⃣ Install Java (Both Machines)



```bash

sudo apt update

sudo apt install openjdk-21-jdk -y

```
\### 3️⃣ Setup Jenkins (Controller)



```bash

wget https://get.jenkins.io/war-stable/latest/jenkins.war

java -jar jenkins.war --httpPort=8080

```
Access Jenkins:
http://<controller-ip>:8080

Step: 3.1: for centos:
For centOS

Implementation Steps
====================

Launched Rocky Linux EC2 instance (AWS Marketplace)
Installed Java (OpenJDK 21) using dnf
Configured passwordless SSH from controller → agent
Added Rocky node in Jenkins using SSH launcher
Verified agent connectivity and executor availability
Executed job on Rocky agent using label restriction

------->>>
4️⃣ Configure SSH (Controller → Agent)
setup ssh keys:- goal: Controller should connect to Agent WITHOUT password

Login to controller: 
bash:
ssh -i keypair.pem ubuntu@<CONTROLLER_PUBLIC_IP>
generate ssh keys:
ssh-keygen
--> public and private keys
Copy public key:
cat ~/.ssh/id_ed25519.pub

login to agent:
ssh -i keypair.pem ubuntu@<AGENT_PUBLIC_IP>

Add key on Agent:
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
-----
Paste the key
👉 Save (Ctrl + X → Y → Enter)
-----
Fix permissions: 
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

FINAL TEST (IMPORTANT): controller: ssh ubuntu@<AGENT_PUBLIC_IP>
\---



\### 5️⃣ Add Agent in Jenkins



\* Go to: Manage Jenkins → Nodes → New Node

\* Name: agent-1

\* Type: Permanent Agent



Configuration:



\* Remote root directory: `/home/ubuntu`

\* Labels: `linux`

\* Launch method: Launch via SSH

\* Credentials: SSH private key

\* Host verification: Non-verifying



\---



\### 6️⃣ Test Job Execution



Create a Freestyle Job:



Build Step:



```bash

echo "Running on agent"

hostname

```



Restrict job to:



```

linux

```



\---



\## ✅ Output



The job executes on the agent machine:



```

Running on agent

ip-172-31-xxx-xxx

\## 🎯 Key Learnings



\* Jenkins distributed architecture

\* Controller vs Agent concept

\* SSH-based authentication

\* Remote job execution

\* AWS EC2 setup



\---



\## 🚀 Outcome



Successfully implemented a \*\*Jenkins Master-Agent setup\*\* where:



\* Controller schedules jobs

\* Agent executes jobs

\* Communication is done via SSH



\---



\## 📸 Future Improvements



\* Add screenshots of setup

\* Convert to Jenkins Pipeline

\* Integrate with GitHub (auto trigger)

\* Add multiple agents

=======================================================




Author
Mohit Verma



