# Building a Multi-Tier Java Web Application On-Premises

## 🚀 Project Overview
This project demonstrates how to manually deploy a **multi-tier Java web application** on a local infrastructure using **Vagrant and VirtualBox**. It provides hands-on experience with essential DevOps tools while configuring each service step by step.

## 🎯 Purpose
The goal of this project is to help developers understand the **manual setup of a multi-tier architecture** before moving towards **automation and cloud deployment**. By configuring each component manually, you will:
- Gain deeper insights into **system dependencies and interactions**.
- Improve **troubleshooting skills** for real-world deployments.
- Lay the groundwork for **future automation** with tools like Ansible and Terraform.  

## 🔧 Tech Stack & Tools Used
| Tool          | Purpose                                         |
|--------------|-----------------------------------------------|
| **Vagrant**  | Automates virtual machine setup              |
| **VirtualBox** | Runs the virtual machines                   |
| **Tomcat**   | Java application server                      |
| **Nginx**    | Reverse proxy and static content server      |
| **RabbitMQ** | Message broker for async communication       |
| **Memcached** | In-memory caching for performance           |
| **MySQL**    | Relational database for data storage        |
| **Maven**    | Build automation tool for Java projects      |
| **Git**      | Version control system                       |

## 🏗️ Architecture
The architecture consists of **five virtual machines** running the following services:
- **Web Server (Nginx)**
- **Application Server (Tomcat)**
- **Database Server (MySQL)**
- **Message Broker (RabbitMQ)**
- **Cache Server (Memcached)**

![Project Architecture](./architecture.gif)![2025-01-3114-44-13online-video-cutter com-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/36036a1f-05b7-409d-9113-cd3ed7942f45)



## 📌 Project Setup

### 1️⃣ Prerequisites
Before you start, make sure you have:
- **Vagrant** installed → [Download Vagrant](https://www.vagrantup.com/downloads)
- **VirtualBox** installed → [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- **Git Bash or any terminal**

### 2️⃣ Clone This Repository
```sh
$ git clone https://github.com/yourusername/multi-tier-java-app.git
```

### 3️⃣ Start the Virtual Machines
Run the following command to create and start all VMs:
```sh
$ vagrant up
```
⏳ **Note:** This process may take a few minutes depending on your system.

### 4️⃣ Access Virtual Machines
Each service runs on its dedicated VM. To SSH into a specific machine, use:
```sh
$ vagrant ssh <vm-name>
```
Example:
```sh
$ vagrant ssh db01
```

### 5️⃣ Manual Service Setup
#### 🗄️ **Database Setup (MySQL)**
```sh
$ sudo yum install -y mariadb-server
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ mysql_secure_installation
$ mysql -u root -p
```
- Create a database and user:
```sql
CREATE DATABASE accounts;
GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'%' IDENTIFIED BY 'admin123';
FLUSH PRIVILEGES;
```
- Import database:
```sh
$ mysql -u root -padmin123 accounts < src/main/resources/db_backup.sql
```

#### 🏗️ **Application Server Setup (Tomcat)**
```sh
$ sudo yum install -y java-11-openjdk git maven wget
$ wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.tar.gz
$ tar xzvf apache-tomcat-9.0.75.tar.gz -C /usr/local/tomcat
$ sudo systemctl start tomcat
$ sudo systemctl enable tomcat
```

#### 🚀 **Deploy Application on Tomcat**
```sh
$ git clone -b main https://github.com/hkhcoder/vprofile-project.git
$ cd vprofile-project
$ mvn install
$ sudo cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
$ sudo systemctl restart tomcat
```

#### 🌐 **Web Server Setup (Nginx)**
```sh
$ sudo apt update && sudo apt install nginx -y
$ sudo systemctl start nginx
$ sudo systemctl enable nginx
```
- Configure **Nginx Reverse Proxy**:
```sh
$ sudo vim /etc/nginx/sites-available/vproapp
```
- Add the following configuration:
```nginx
upstream vproapp {
  server app01:8080;
}
server {
  listen 80;
  location / {
    proxy_pass http://vproapp;
  }
}
```
- Apply the new configuration:
```sh
$ sudo ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/
$ sudo systemctl restart nginx
```

## 🔥 Testing & Validation
Once all services are set up, verify that everything is running correctly:
```sh
$ vagrant ssh web01
$ curl -I http://localhost
```
You should see an HTTP **200 OK** response.

## 🚀 Next Steps
This is just the **first step**! In the upcoming posts, we will:
- Automate the setup with **scripts & Ansible** 🛠️
- Deploy the app on **AWS using EC2 & managed services** ☁️
- Containerize with **Docker & Kubernetes** 🐳
- Implement **CI/CD with Jenkins & ArgoCD** 🔄

## 👨‍💻 Contributors
- **Mohamed Elweza**
- **Nader Ashour**

---

📌 **Follow this repository for updates!**
✅ If you found this useful, don't forget to **star ⭐ the repo**!

💬 Feel free to open issues or contribute to improve this setup!
