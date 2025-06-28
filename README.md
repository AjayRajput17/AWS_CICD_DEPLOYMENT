# 🚀 Deploying a MERN Blog Application using CI/CD Pipeline (AWS)

## 👥 Group Members

1. Vedant Chaudhari  
2. Ajay Ingle  
3. Sarthak Matte  
4. Abhishek Sengar  

---

### 📌 Project: MERN Blog Application

---

## 🧠 Architecture Diagram

*(Insert your architecture diagram image here if available)*
![image](https://github.com/user-attachments/assets/5838d839-49da-4d76-bdd0-032986d085f1)

---



## 📁 Required Files in Root Directory

Ensure the following files exist in the **root** of your MERN project before setting up the CI/CD pipeline:

- `buildspec.yml`
- `appspec.yml`

Also, create a `scripts/` folder and add the following shell scripts:

- `before_install.sh`
- `app_stop.sh`
- `app_start.sh`
![image](https://github.com/user-attachments/assets/edf42e8a-1912-45b0-ae19-0c23386c617f)


Push all changes to your GitHub repository.

---

## 🛠️ Step-by-Step Deployment Guide

### ✅ Step 1: Configure Environment Variables in AWS Systems Manager

Navigate to **AWS Systems Manager > Parameter Store** and add the following parameters:

| Name | Type | Description |
|------|------|-------------|
| `/mern-app/prod/mongodb-uri` | SecureString | Your MongoDB Atlas connection string |
| `/blog-cicd/jwt-secret` | SecureString | A long, random string used for signing JWTs |
| `/blog-cicd/port` | String | Port number for Node.js app (e.g., `8000`) |
![image](https://github.com/user-attachments/assets/4281f84c-53f0-4520-b47e-28cae72fe95b)

---

### 🏗️ Step 2: Launch Infrastructure with CloudFormation

1. Go to **AWS CloudFormation**.
2. Click `Create stack > With new resources (standard)`.
3. Upload your infrastructure template file.
4. Follow the wizard and launch the stack.
![image](https://github.com/user-attachments/assets/744ba9bc-dc88-4cf0-98f0-c587f50b7cf7)
![image](https://github.com/user-attachments/assets/3dfef412-e3a5-4a79-bf05-170e343c807f)


---

### 🔐 Step 3: Create IAM Role

1. Go to **IAM > Roles**.
2. Create a new role.
3. Attach the managed policy: `AWSCodeDeployRole`.
![image](https://github.com/user-attachments/assets/ede98ed1-247c-463c-baeb-61b8696e6c5a)

---

### 📦 Step 4: Create CodeDeploy Application

1. Go to **AWS CodeDeploy**.
2. Click `Create application`.
![image](https://github.com/user-attachments/assets/7af43487-4414-491b-9301-e23da048d797)

---

### 🚀 Step 5: Create Deployment Group

Configure as follows:

- **Service Role**: Select the IAM role created (`CodeDeployRole`)
- **Deployment Type**: In-place
- **Environment**: Amazon EC2 instances
- **Tag Filters**:
  - **Key**: `Deploy`
  - **Value**: `mern-podcast-app`
- **Deployment Settings**: `CodeDeployDefault.OneAtATime`
- **Load Balancer**: *Disable* (uncheck)
![image](https://github.com/user-attachments/assets/d672d8df-404b-494e-a293-767dc1bb8fbe)
![image](https://github.com/user-attachments/assets/82a192fc-2054-414d-9706-a8bfa836e453)

---

### 📁 Step 6: Create an S3 Bucket

1. Go to **Amazon S3**.
2. Create a new bucket to store artifacts.
![image](https://github.com/user-attachments/assets/66f33119-9eb4-4bb0-a38f-fad45a0c201d)

---

### 🔧 Step 7: Create CodeBuild Project

1. Go to **AWS CodeBuild > Create Project**.
2. Environment:
   - **Image**: Managed image
   - **OS**: Amazon Linux 2
   - **Runtime**: Standard (latest version)
3. Service Role: *Let CodeBuild create a new role*
4. **Buildspec**: Use `buildspec.yml` from your repo
5. Artifacts:
   - **Type**: Amazon S3
   - **Bucket**: Select previously created bucket
   - **Packaging**: Zip
6. Logs: Default (CloudWatch Logs)
![image](https://github.com/user-attachments/assets/17d616f5-ff90-4dfd-bbaf-2a1bec9b52f4)
![image](https://github.com/user-attachments/assets/ae1babc1-bd52-47c3-a4a4-084da3a4c90b)
![image](https://github.com/user-attachments/assets/4d35b641-f7e5-48ac-a817-6703f1cfe1e6)
![image](https://github.com/user-attachments/assets/58b07df0-31c9-4c86-9ee6-2d6143f6c1c2)
![image](https://github.com/user-attachments/assets/07328851-ad04-45da-8785-196bddf0d6f2)

---

### 📡 Step 8: Create a Pipeline in CodePipeline

1. Go to **AWS CodePipeline > Create pipeline**.
2. **Pipeline Settings**:
   - **Name**: `Mern-Todo-App-Pipeline`
   - **Service Role**: Let CodePipeline create one
   - **Artifact Store**: Choose default or custom S3 location
3. **Review & Create**: Review all settings and click `Create Pipeline`
![image](https://github.com/user-attachments/assets/213724bd-c60a-4fff-a6ad-1d19cd4991b6)
![image](https://github.com/user-attachments/assets/938a7ed3-f335-40e8-bbdd-c4bb0b992ff8)
![image](https://github.com/user-attachments/assets/a14696c4-0d06-4c22-8087-916ba60b0fd3)
![image](https://github.com/user-attachments/assets/1842cf2a-22ae-4ee8-990c-fcdfe91790be)


---

### ✅ Final Step: Test the Deployment

- Copy and paste the **public IP** of your EC2 instance into the browser to verify deployment.
![image](https://github.com/user-attachments/assets/49b34775-25f1-4aa4-b241-d41414e8515c)
---

> ℹ️ This README summarizes the full CI/CD deployment pipeline for a MERN Blog Application using AWS CodePipeline, CodeBuild, CodeDeploy, CloudFormation, and SSM.

