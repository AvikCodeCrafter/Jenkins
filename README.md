# Jenkins LTS Installation on Amazon Linux 2023 (AWS EC2)

This repository provides a step-by-step guide to install Jenkins LTS on an Amazon EC2 instance running Amazon Linux 2023 (AL2023).

The guide follows official Jenkins recommendations, uses Amazon Corretto Java 21, and avoids outdated or insecure practices.

---

## What This Guide Covers

- Jenkins Long-Term Support (LTS) installation
- Amazon Linux 2023-compatible commands (dnf)
- Java 21 (Amazon Corretto)
- systemd-based Jenkins service management
- Safe upgrade path within LTS releases

---

## Prerequisites

Before starting, ensure you have:

- An AWS account
- An EC2 instance with:
  - Amazon Linux 2023 AMI
  - Instance type: t3.medium or higher (recommended)
- A Security Group with:
  - SSH (22) → Your IP
  - Jenkins (8080) → Your IP or ALB
- An EC2 Key Pair
- (Recommended) IAM Role attached to EC2

---

## Connect to the EC2 Instance

```bash
ssh -i my-key.pem ec2-user@<EC2_PUBLIC_IP>
```

---

## Update System Packages

```bash
sudo dnf update -y
```

If kernel updates are installed, reboot:

```bash
sudo reboot
```

Reconnect after reboot.

---

## Install Java

Jenkins LTS officially supports Java 17 and Java 21.  
Amazon Corretto 21 is recommended for Amazon Linux 2023.

```bash
sudo dnf install java-21-amazon-corretto -y
```

Verify:

```bash
java -version
```

---

## Add Jenkins LTS Repository

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/rpm-stable/jenkins.repo
```

Import Jenkins GPG key:

```bash
sudo rpm --import https://pkg.jenkins.io/rpm-stable/jenkins.io-2026.key
```

Verify repository:

```bash
dnf repolist | grep jenkins
```

---

## Install Jenkins LTS

```bash
sudo dnf install jenkins -y
```

Verify version:

```bash
jenkins --version
```

---

## Start and Enable Jenkins

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

## Access Jenkins Web UI

Open your browser:

```
http://<EC2_PUBLIC_IP>:8080
```

---

## Unlock Jenkins

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## Complete Jenkins Setup

- Install suggested plugins
- Create admin user
- Save and finish setup

---

## Upgrade Jenkins (LTS)

```bash
sudo dnf upgrade jenkins -y
sudo systemctl restart jenkins
```

---

## Production Recommendations

- Restrict port 8080
- Use HTTPS via ALB or reverse proxy
- Use IAM Role instead of AWS access keys
- Backup /var/lib/jenkins using EBS snapshots
- Use gp3 EBS volumes (30–50 GB minimum)

---

## References

- Jenkins Documentation: https://www.jenkins.io/doc/
- Jenkins RPM Repo: https://pkg.jenkins.io/
- Amazon Linux 2023 Docs: https://docs.aws.amazon.com/linux/

---

License: MIT
