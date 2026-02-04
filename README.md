# 🚀 Jenkins LTS on Amazon Linux 2023 (AWS EC2)

Welcome! 👋  
This repository is a **gold‑standard, community‑friendly guide** to installing **Jenkins LTS (Long‑Term Support)** on **Amazon Linux 2023** running on **AWS EC2**.

Whether you're **new to Jenkins**, preparing for **DevOps interviews**, or setting up **real-world CI/CD**, this guide is designed to be:
- ✅ Easy to follow  
- 🔐 Security-aware  
- ☁️ AWS‑aligned  
- 🧠 Production-minded  

---

## 🌟 Why This Guide?

Most Jenkins tutorials online:
- ❌ Use outdated OS versions  
- ❌ Install weekly (unstable) Jenkins builds  
- ❌ Store AWS access keys insecurely  
- ❌ Ignore AWS + Linux 2023 realities  

This guide fixes all of that.

---

## 🧰 What You’ll Learn

✔ Install **Jenkins LTS (Stable)**  
✔ Use **Amazon Linux 2023** correctly (`dnf`, not legacy yum)  
✔ Configure **Java 21 (Amazon Corretto)**  
✔ Manage Jenkins via **systemd**  
✔ Upgrade Jenkins safely (LTS only)  
✔ Follow **real DevOps best practices**

---

## 🗺️ Architecture Overview

```
Developer → Browser → Jenkins (EC2)
                     |
                     └── /var/lib/jenkins (EBS gp3)
```

🔒 Optional: Jenkins behind ALB + HTTPS  
☁️ Optional: Jenkins agents on EC2 / Docker

---

## 📋 Prerequisites

Before you start, make sure you have:

- ☁️ **AWS Account**
- 🖥️ **EC2 Instance**
  - OS: Amazon Linux 2023
  - Type: `t3.medium` or higher (recommended)
- 🔐 **Security Group**
  - SSH (22) → Your IP
  - Jenkins (8080) → Your IP / ALB
- 🔑 **EC2 Key Pair**
- 🎭 **IAM Role attached to EC2** (recommended)

---

## 🔌 Step 1: Connect to EC2

```bash
ssh -i my-key.pem ec2-user@<EC2_PUBLIC_IP>
```

✅ If this works, you’re ready to go!

---

## 🔄 Step 2: Update the System

```bash
sudo dnf update -y
```

📌 If kernel updates occur:
```bash
sudo reboot
```

Reconnect after reboot.

---

## ☕ Step 3: Install Java (Mandatory)

Jenkins LTS officially supports:
- ✅ Java 17
- ✅ Java 21 (recommended)

We’ll use **Amazon Corretto 21** 👇

```bash
sudo dnf install java-21-amazon-corretto -y
```

Verify:
```bash
java -version
```

Expected output:
```
openjdk version "21"
```

---

## 📦 Step 4: Add Jenkins LTS Repository

⚠️ **IMPORTANT:** This is the **LTS repo**, not weekly builds.

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/rpm-stable/jenkins.repo
```

🔑 Import Jenkins GPG key:
```bash
sudo rpm --import https://pkg.jenkins.io/rpm-stable/jenkins.io-2026.key
```

Verify:
```bash
dnf repolist | grep jenkins
```

---

## 🧩 Step 5: Install Jenkins LTS

```bash
sudo dnf install jenkins -y
```

Check version:
```bash
jenkins --version
```

You should see something like:
```
2.4xx.x (LTS)
```

🎉 Congrats — Jenkins LTS is installed!

---

## ▶️ Step 6: Start Jenkins

```bash
sudo systemctl daemon-reload
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Check status:
```bash
sudo systemctl status jenkins
```

---

## 🌐 Step 7: Open Jenkins UI

Open your browser:

```
http://<EC2_PUBLIC_IP>:8080
```

If it loads — you’re winning 🏆

---

## 🔓 Step 8: Unlock Jenkins

Get the initial admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste it into the UI.

---

## ⚙️ Step 9: First-Time Setup

1️⃣ Click **Install suggested plugins**  
2️⃣ Create your **Admin User**  
3️⃣ Save & Finish  

🎊 Jenkins is now ready!

---

## 🔍 Step 10: Confirm LTS Version

In Jenkins UI:
```
Manage Jenkins → About Jenkins
```

You should see:
```
Jenkins 2.xxx.x (LTS)
```

---

## ⬆️ Upgrading Jenkins (Safe & LTS Only)

```bash
sudo dnf upgrade jenkins -y
sudo systemctl restart jenkins
```

✅ This will **never** install weekly builds.

---

## 🔐 Production Best Practices (Highly Recommended)

✔ Use **IAM Roles** (no AWS access keys in Jenkins)  
✔ Restrict port **8080**  
✔ Use **ALB + HTTPS**  
✔ Backup `/var/lib/jenkins` via **EBS snapshots**  
✔ Use **gp3 EBS volumes (30–50GB+)**  
✔ Monitor CPU & memory (Jenkins loves RAM)

---

## 🧠 Common Mistakes to Avoid

❌ Installing Jenkins Weekly  
❌ Using `yum` blindly on AL2023  
❌ Running Jenkins on `t2.micro`  
❌ Storing AWS secrets in Jenkins UI  
❌ Exposing Jenkins publicly without auth  

---

## 🧪 Tested On

| Component | Version |
|---------|--------|
Amazon Linux | 2023 |
Java | Corretto 21 |
Jenkins | LTS |
AWS | EC2 |

---

## 🚀 What’s Next?

Want to level up? Try:
- Jenkins behind **ALB + HTTPS**
- **EC2 auto-scaling Jenkins agents**
- Jenkins with **Docker**
- Jenkins on **EKS**
- CI/CD pipelines (Maven, Gradle, Node.js)

---

## 📚 References

- Jenkins Docs → https://www.jenkins.io/doc/
- Jenkins RPM Repo → https://pkg.jenkins.io/
- Amazon Linux 2023 → https://docs.aws.amazon.com/linux/

---

## 🏷️ License

MIT License  
Feel free to fork, share, and help the community ❤️

---

⭐ **If this repo helped you, give it a star!** ⭐
