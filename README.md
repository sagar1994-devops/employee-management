# Employee Management Application Deployment Guide

This guide explains how to deploy the **Employee Management Application** using the following tech stack:

* Frontend: React
* Backend: Spring Boot
* Database: MySQL (Amazon RDS)
* Infrastructure: AWS EC2

---

# Architecture

User → React Frontend → Spring Boot Backend → MySQL (RDS)

Frontend runs on **port 3000**
Backend runs on **port 8080**

---

# Step 1: Create RDS Database

Create an RDS instance with the following configuration:

Engine: MySQL
Database Name: employeedb
Username: admin
Password: admin123

Endpoint

```
database-1.c5yo48g0g39.ap-northeast-1.rds.amazonaws.com
```

Make sure the RDS security group allows **MySQL access (port 3306)** from EC2.

---

# Step 2: Create EC2 Instance

Launch an EC2 instance and connect using SSH.

Update the system

```
sudo yum update -y
```

Install Java (required for Spring Boot backend)

```
sudo yum install java-21-amazon-corretto-devel -y
```

Install Maven

```
sudo yum install maven -y
```

Install Git

```
sudo yum install git -y
```

Verify Git installation

```
git --version
```

---

# Step 3: Install Node.js

Install Node.js repository

```
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
```

Install Node.js and npm

```
sudo yum install nodejs -y
```

Verify installation

```
node -v
npm -v
```

---

# Step 4: Clone the Repository

Clone the project repository

```
git clone https://github.com/sagar1994-devops/employee-management.git
```

---

# Step 5: Configure Backend

Edit the backend configuration file

```
vi /home/ec2-user/employee-management/employeemanagmentbackend/src/main/resources/application.properties
```

Update database connection details with your RDS information.

---

# Step 6: Build and Run Backend

Go to the backend directory

```
cd /home/ec2-user/employee-management/employeemanagmentbackend
```

Build the application

```
mvn clean package
```

Go to the generated artifact directory

```
cd /home/ec2-user/employee-management/employeemanagmentbackend/target
```

Run the backend service

```
java -jar employeemanagmentbackend-0.0.1-SNAPSHOT.jar
```

Verify backend

```
http://<EC2-PUBLIC-IP>:8080/employee
```

---

# Step 7: Configure Frontend

Edit frontend service file

```
vi /home/ec2-user/employee-management/employeemanagement-frontend/src/service/EmployeeService.js
```

Update backend URL

```
const BASE_URL = "http://<EC2-PUBLIC-IP>:8080/employee";
```

---

# Step 8: Run Frontend Application

Go to the frontend directory

```
cd /home/ec2-user/employee-management/employeemanagement-frontend
```

Install dependencies

```
npm install
```

Start the React application

```
npm start
```

Visit the application

```
http://<EC2-PUBLIC-IP>:3000
```

---

# Step 9: Run Backend as a Service

Create backend service

```
sudo vi /etc/systemd/system/backend.service
```

Add

```
[Unit]
Description=Spring Boot Backend Service
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/employee-management/employeemanagmentbackend/target
ExecStart=/usr/bin/java -jar employeemanagmentbackend-0.0.1-SNAPSHOT.jar
SuccessExitStatus=143
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Reload systemd

```
sudo systemctl daemon-reload
```

Start backend

```
sudo systemctl start backend
```

Enable auto start

```
sudo systemctl enable backend
```

---

# Step 10: Run Frontend as a Service

Create frontend service

```
sudo vi /etc/systemd/system/frontend.service
```

Add

```
[Unit]
Description=React Frontend Service
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/employee-management/employeemanagement-frontend
ExecStart=/usr/bin/npm start
Restart=always
RestartSec=5
Environment=PORT=3000

[Install]
WantedBy=multi-user.target
```

Reload systemd

```
sudo systemctl daemon-reload
```

Start frontend

```
sudo systemctl start frontend
```

Enable auto start

```
sudo systemctl enable frontend
```

---

# Access Application

Frontend

```
http://<EC2-PUBLIC-IP>:3000
```

Backend API

```
http://<EC2-PUBLIC-IP>:8080/employee
```

---

# Author

Sagar Misal
DevOps Engineer

