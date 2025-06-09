#  ICT171 Assignment 2 – Cloud Server Project

**Name:** Ronald Justin  

**Student Number:** 35565552  

**Live Site:** [http://ron10.shop](http://ron10.shop)  

Public IP:** 18.143.248.1

**Video explainer** https://youtu.be/0as9QgjTC0E?si=2pAUnhDAdvIXjyEc

---

##  Overview

This documentation explains how I created and deployed my cloud server project using Amazon EC2, based entirely on the official ICT171 lab instructions. The project involved launching a Free Tier Ubuntu instance, installing and configuring Apache, and deploying a professionally styled static HTML website for a fictional clothing store. I registered the domain ron10.shop through GoDaddy and linked it to my EC2 instance using DNS configuration. All files were transferred securely using SCP, and the server was managed via SSH. The site includes a homepage, responsive product gallery, cart section, contact area, and a discussion panel explaining the build process. This documentation is written to allow anyone — including myself in a crisis — to rebuild the entire server from scratch in under 2 hours. It reflects my learning of core cloud infrastructure concepts, Linux administration, static site hosting, and practical web deployment skills.
---

##  Server Setup Instructions

### 1. Launch an EC2 Instance
- Log in to [https://aws.amazon.com/ec2](https://aws.amazon.com/ec2)
- Launch a new **Ubuntu 20.04 LTS** (Free Tier eligible)
- Instance type: `t2.micro`
- Storage: 8GB (default)
- Tag: skip
- Security group: Name it `ssh-and-web`
  - Allow port **22 (SSH)** and **80 (HTTP)**

### 2. Key Pair
- Create a new key pair with suitable name 
- Keep this file safe

---

##  Connect to the Server

```bash
cd path/to/your/key
chmod 400 webserver-key.pem
ssh -i "webserver-key.pem" ubuntu@18.143.248.1
```

---

## Install Apache

```bash
sudo apt update
sudo apt install apache2 -y
```

Test in browser:  
Visit `http://18.143.248.1` or `http://ron10.shop`

---

##  Setup Domain with GoDaddy

- Purchased domain: `ron10.shop` for $2
- Set an A-record pointing to EC2’s Elastic IP
- DNS propagation may take 30–60 mins

---

##  Editing the Website

```bash
sudo nano /var/www/html/index.html
```

Replace the content with your own HTML. My live version contains:
- Student info
- Project description
- Licensing section

 ## Using wget
```bash
wget http://example.com/sample.pdf
sudo cp sample.pdf /var/www/html/
```

### Using SCP
```bash
scp -i webserver-key.pem localfile.txt ubuntu@18.143.248.1:/home/ubuntu/
ssh in, then:
sudo mv localfile.txt /var/www/html/
```

---

 Bash Setup Script
 `scripts/setup.sh`:
```bash
#!/bin/bash
sudo apt update
sudo apt install apache2 -y
sudo ufw allow 'Apache Full'
sudo systemctl enable apache2
sudo systemctl start apache2
```

Purpose: Automates Apache setup and firewall rules.

---

 Total Cost of Ownership (TCO)

| Item               | Cost (AUD/year) |
|--------------------|-----------------|
| EC2 Free Tier      | $0.00           |
| EBS (8GB)          | $14.40          |
| Domain (GoDaddy)   | $2.00           |
| **Total**          | **$16.40**      |

---

 Troubleshooting

- **index.html Permission**: Use `sudo`
- **Site not loading**: Ensure port 80 is open
- **DNS not working**: Wait for propagation and clear browser cache

---

 Reflection

This project helped me understand how to deploy a cloud-based website using Amazon EC2. I learned how to set up an Ubuntu server, install Apache, and link a custom domain using GoDaddy DNS. I also created a simple clothing store website using HTML and CSS.I gained hands-on experience with SSH, SCP, and working in a Linux environment. Documenting everything in the README.md helped me understand the importance of clear instructions and reproducibility. Overall, this assignment gave me practical skills in cloud hosting and server setup.
---
##  Project Structure

```
ict171-server-project/
├── documentation/                 
├── scripts/
│   └── setup.sh
├── website/
│   └── index.html
├── README.md                      
```

---

