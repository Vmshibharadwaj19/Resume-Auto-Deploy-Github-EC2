
# 🚀 Resume Hosting via GitHub Actions on AWS EC2  
**Author:** Vamshi Prasad Goteti  
**Date:** 27/06/2025  
**Email:** vamshibharadwaj19@gmail.com  

---

## 📘 Project Overview  
This project showcases how I hosted my **personal resume website** on an AWS EC2 instance and set up **automated deployment** using **GitHub Actions**. The CI/CD pipeline ensures that any changes pushed to my GitHub repository are deployed live without manual effort.

---

## 🧰 Tools & Services Used  
- **GitHub Actions**: To create a CI/CD workflow  
- **AWS EC2 (Ubuntu)**: Hosting the resume site  
- **Apache Web Server**: To serve the HTML file  
- **SSH Key Pair**: For secure server access  
- **GitHub Secrets**: To store sensitive keys and config  
- **index.html**: My resume in HTML format  

---

## 🛠️ Project Setup Steps

### 1. 🚀 Launch EC2 Instance  
- Ubuntu-based instance created on AWS  
- Apache2 installed and started  
- Port 22 (SSH), 80 (HTTP) opened in **security group**

### 2. 🔐 Generate SSH Key  
- Created `.pem` key pair via AWS Console  
- Added private key to GitHub Secrets as `EC2_SSH_KEY`  

---

## 🧩 GitHub Secrets Configuration  

| Secret Name       | Description                          |
|-------------------|--------------------------------------|
| `EC2_SSH_KEY`     | Private key from AWS key-pair        |
| `HOST_DNS`        | Public DNS of EC2 instance           |
| `USERNAME`        | SSH username (e.g., `ubuntu`)        |
| `TARGET_DIR`      | Directory to deploy to (e.g., `/home/ubuntu`) |

---

## ⚙️ GitHub Actions Workflow (`.github/workflows/deploy.yml`)

```yaml
name: Deploy Resume to EC2

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Deploy to EC2 via SSH
        uses: easingthemes/ssh-deploy@main
        env:
          SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
          REMOTE_HOST: ${{ secrets.HOST_DNS }}
          REMOTE_USER: ${{ secrets.USERNAME }}
          TARGET: ${{ secrets.TARGET_DIR }}

      - name: Configure Apache and Move Files
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST_DNS }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo apt-get update -y
            sudo apt-get install apache2 -y
            sudo systemctl start apache2
            sudo systemctl enable apache2
            sudo cp /home/ubuntu/index.html /var/www/html/index.html
```

---

## 🌐 What Happens Automatically?

- Push to the `main` branch triggers deployment  
- HTML resume file (`index.html`) is sent to EC2  
- Apache serves the file at `http://<public-dns>`  
- Apache service is started and enabled via workflow  
-Repository Name-EWP ,.git/workflows/yml to EC2
-Reference link - http://ec2-52-207-236-91.compute-1.amazonaws.com/
---

## 🎯 Outcome

With this setup:
- I can update my resume in VS Code  
- Push to GitHub → It auto-updates on the live site  
- No manual file transfer or server login needed  

---

## 🧠 What I Learned

- Writing and modifying GitHub Actions YAML files  
- Setting up Apache on EC2  
- Using GitHub Secrets securely  
- Automating deployment end-to-end  

---

## 🔗 Future Enhancements

- Add custom domain  
- Set up HTTPS with SSL  
- Auto-build resume from Markdown or LaTeX  

---

**🌐 Visit:** [NextWork.org](https://nextwork.org) for more DevOps and Cloud projects.  

**Everyone deserves to be in a job they love.**
