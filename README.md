# employee-management<img width="1417" alt="Screenshot 2022-06-14 at 14 47 09" src="https://user-images.githubusercontent.com/64640469/173594367-d363f981-2478-4466-8e3d-738eaf720fd2.png">

# Employee Management Application Deployment Guide

This guide explains how to deploy the **Employee Management Application** using the following tech stack:

* **Frontend:** React
* **Backend:** Spring Boot
* **Database:** MySQL (Amazon RDS)
* **Infrastructure:** Amazon EC2

---

# Architecture

Frontend (React) → Backend (Spring Boot) → Database (Amazon RDS MySQL)

---

# Step 1: Create RDS Database

Create an RDS instance with the following configuration:

Engine: MySQL
Database Name: employeedb
Username: admin
Password: admin123

Endpoint:

```
database-1.c5yo48g0g39.ap-northeast-1.rds.amazonaws.com
```

Make sure the RDS security group allows access from your EC2 instance.

---

# Step 2: Create EC2 Instance

Launch an EC2 instance and connect using SSH.

Update the system:

```
sudo yum update -y
```

Install Java (for Spring Boot backend):

```
sudo yum install java-21-amazon-corretto-devel -y
```

Install Maven:

```
sudo yum install maven -y
```

---

# Step 3: Install Node.js

Install Node.js repository:

```
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
```

Install Node.js and npm:

```
sudo yum install nodejs -y
```

Verify installation:

```
node -v
npm -v
```

---

# Step 4: Clone the Repository
Install Git:
```
sudo yum install git -y
```

Clone the project repository:

```
git clone https://github.com/sagar1994-devops/employee-management.git
```

---

# Step 5: Configure Backend

Edit the backend configuration file:

```
vi /home/ec2-user/employee-management/employeemanagmentbackend/src/main/resources/application.properties
```

Update the database configuration with your RDS details.

---

# Step 6: Build and Run Backend

Navigate to the backend directory:

```
cd /home/ec2-user/employee-management/employeemanagmentbackend
```

Build the application:

```
mvn clean package
```

Go to the generated artifact folder:

```
cd /home/ec2-user/employee-management/employeemanagmentbackend/target
```

Run the Spring Boot application:

```
java -jar employeemanagmentbackend-0.0.1-SNAPSHOT.jar
```

Your backend service will start.

Test it using the EC2 public IP:

```
http://<EC2-PUBLIC-IP>:8080/employee
```

Make sure the EC2 **security group allows inbound traffic**.

---

# Step 7: Configure Frontend

Edit the frontend service configuration file:

```
vi /home/ec2-user/employee-management/employeemanagement-frontend/src/service/EmployeeService.js
```

Update the backend URL:

```
const BASE_URL = "http://<EC2-PUBLIC-IP>:8080/employee";
```

---

# Step 8: Run Frontend Application

Navigate to the frontend directory:

```
cd /home/ec2-user/employee-management/employeemanagement-frontend
```

Install dependencies:

```
npm install
```

Start the React application:

```
npm start
```

---

# Step 9: Access the Application

Open your browser and visit:

```
http://<EC2-PUBLIC-IP>:3000
```

You should now see the **Employee Management Application running successfully.**

---

# Notes

* Ensure EC2 security groups allow ports **3000 and 8080**
* Ensure RDS security group allows inbound **MySQL (3306)** from EC2
* Replace `<EC2-PUBLIC-IP>` with your actual instance IP
* Do not expose sensitive credentials in public repositories

If you want, I can also help you make a **much more professional README (with diagrams, architecture image, and DevOps deployment flow)** so your GitHub profile looks **strong for DevOps recruiters**.
