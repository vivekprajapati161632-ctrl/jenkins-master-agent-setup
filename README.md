Here’s a clean, professional, **GitHub-ready README.md** version of your content 👇

---

# 🚀 Multi-OS Jenkins Setup (Ubuntu + Rocky Linux)

This project demonstrates a **distributed Jenkins architecture** on AWS EC2 using:

* 🧠 **Ubuntu-based Jenkins Controller**
* ⚙️ **Rocky Linux (CentOS-based) Jenkins Agent**
* 🔐 Secure **SSH-based communication**
* 🔄 Cross-platform job execution

---

## 🏗️ Architecture

```
          ┌──────────────────────────────┐
          │ Jenkins Controller (Ubuntu)  │
          │ EC2 - t3.large              │
          └────────────┬────────────────┘
                       │
                       │ SSH (Key-based Auth)
                       ▼
        ┌──────────────────────────────┐
        │ Jenkins Agent (Rocky Linux)  │
        │ EC2 - t3.large              │
        └──────────────────────────────┘
```

### 🔁 Workflow

* Controller schedules jobs
* Agent executes builds
* Communication via SSH keys

---

## 🛠️ Technologies Used

* Jenkins
* AWS EC2
* Ubuntu Linux
* Rocky Linux (Rocky-9)
* SSH
* Git

---

## ⚙️ Step-by-Step Setup

### 1️⃣ Launch EC2 Instances

* **Instance Type:** `t3.large`
* **Operating Systems:**

  * Ubuntu (Controller)
  * Rocky Linux (Agent)
* **Security Group Ports:**

  * `22` → SSH
  * `8080` → Jenkins UI

---

### 2️⃣ Install Java (Both Machines)

```bash
sudo apt update
sudo apt install openjdk-21-jdk -y
```

👉 For Rocky Linux (Agent):

```bash
sudo dnf install java-21-openjdk -y
```

---

### 3️⃣ Setup Jenkins (Controller)

```bash
wget https://get.jenkins.io/war-stable/latest/jenkins.war
java -jar jenkins.war --httpPort=8080
```

🌐 Access Jenkins UI:

```
http://<CONTROLLER_PUBLIC_IP>:8080
```

---

### 4️⃣ Configure SSH (Controller → Agent)

#### 🎯 Goal:

Controller connects to Agent **without password**

---

#### 🔹 Step 1: Login to Controller

```bash
ssh -i keypair.pem ubuntu@<CONTROLLER_PUBLIC_IP>
```

#### 🔹 Step 2: Generate SSH Key

```bash
ssh-keygen
```

#### 🔹 Step 3: Copy Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

---

#### 🔹 Step 4: Login to Agent

```bash
ssh -i keypair.pem ubuntu@<AGENT_PUBLIC_IP>
```

#### 🔹 Step 5: Add Key to Agent

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

👉 Paste the copied public key and save

---

#### 🔹 Step 6: Fix Permissions

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

 Step 7: Final Test ✅

```bash
ssh ubuntu@<AGENT_PUBLIC_IP>
```

---

Add Agent in Jenkins

Navigate to:

**Manage Jenkins → Nodes → New Node**

Configuration:

* **Name:** `agent-1`
* **Type:** Permanent Agent
* **Remote Root Directory:** `/home/ubuntu`
* **Labels:** `linux`
* **Launch Method:** Launch via SSH
* **Credentials:** Add SSH private key
* **Host Verification:** Non-verifying

---

### 6️⃣ Test Job Execution

Create a **Freestyle Job**

#### Build Step:

```bash
echo "Running on agent"
hostname
```

 Restrict Job To:

```
linux
```

---

 Output

```
Running on agent
ip-172-31-xxx-xxx
```

✔️ Job successfully executed on the **agent machine**

---

 Key Learnings

* Jenkins Distributed Architecture
* Controller vs Agent Concept
* SSH-Based Authentication
* Remote Job Execution
* AWS EC2 Deployment

---

 Outcome

Successfully implemented a **Jenkins Master-Agent setup**:

* 🧠 Controller schedules jobs
* ⚙️ Agent executes jobs
* 🔐 Secure communication via SSH


Future Improvements
- Add setup screenshots
-Convert to Jenkins Pipeline
-GitHub integration (auto-trigger builds)
-Add multiple agents
Author
Vivek Prajapati
