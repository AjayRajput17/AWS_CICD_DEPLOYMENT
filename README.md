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

---

### 🧑‍💻 Student Info

**Name**: Ajay Kailas Ingle  
**PRN**: 202302040021  
**Roll No**: 66  

---

## 📁 Required Files in Root Directory

Ensure the following files exist in the **root** of your MERN project before setting up the CI/CD pipeline:

- `buildspec.yml`
- `appspec.yml`

Also, create a `scripts/` folder and add the following shell scripts:

- `before_install.sh`
- `app_stop.sh`
- `app_start.sh`

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

---

### 🏗️ Step 2: Launch Infrastructure with CloudFormation

1. Go to **AWS CloudFormation**.
2. Click `Create stack > With new resources (standard)`.
3. Upload your infrastructure template file.
4. Follow the wizard and launch the stack.

---

### 🔐 Step 3: Create IAM Role

1. Go to **IAM > Roles**.
2. Create a new role.
3. Attach the managed policy: `AWSCodeDeployRole`.

---

### 📦 Step 4: Create CodeDeploy Application

1. Go to **AWS CodeDeploy**.
2. Click `Create application`.

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

---

### 📁 Step 6: Create an S3 Bucket

1. Go to **Amazon S3**.
2. Create a new bucket to store artifacts.

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

---

### 📡 Step 8: Create a Pipeline in CodePipeline

1. Go to **AWS CodePipeline > Create pipeline**.
2. **Pipeline Settings**:
   - **Name**: `Mern-Todo-App-Pipeline`
   - **Service Role**: Let CodePipeline create one
   - **Artifact Store**: Choose default or custom S3 location
3. **Review & Create**: Review all settings and click `Create Pipeline`

---

### ✅ Final Step: Test the Deployment

- Copy and paste the **public IP** of your EC2 instance into the browser to verify deployment.

---

> ℹ️ This README summarizes the full CI/CD deployment pipeline for a MERN Blog Application using AWS CodePipeline, CodeBuild, CodeDeploy, CloudFormation, and SSM.

