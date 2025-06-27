# 🚀 Packaging an app using AWS CodeBuild & S3

In this project, I will set up CodeBuild 

<img width="707" alt="Screenshot 2025-06-27 at 11 12 21 PM" src="https://github.com/user-attachments/assets/20cc95ec-3f0a-4f77-aa7a-9b7a269f2a5b" />


## 📁 Prerequisites

* AWS Account
* AWS CLI configured
* GitHub repository with your Java web application and a valid `buildspec.yml` file
* S3 bucket created to store build artifacts

---

## ✅ Step 1: Create an S3 Bucket

Before setting up CodeBuild, create an S3 bucket where build artifacts will be stored.

---

## 🛠️ Step 2: Create a CodeBuild Project

1. Go to the **AWS CodeBuild** service.
2. Click on **Create Project**.
3. **Configure Project Settings**:

   * **Project Name**: Give your project a relevant name.
   
     <img width="699" alt="Screenshot 2025-06-27 at 11 05 43 PM" src="https://github.com/user-attachments/assets/990bd8eb-fe8f-41c3-85a0-052fdfba3a62" />

4. **Source**:

   * **Source Provider**: Select `GitHub`.
   * If it's not connected yet, click **"Connect to GitHub"** to authorize access.
   * **Uncheck** the **WebHook** option (we’re not triggering builds on pushes).
     
5. **Environment**:

   * Choose **Managed Image**.
   * Select the image **"Amazon Linux 2"** with **Corretto 8** runtime.
     
6. **Buildspec**:

   * Choose **"Use a buildspec file"**.
     
7. **Artifacts**:

   * **Type**: S3
   * **Bucket name**: Select the S3 bucket you created earlier.
   * **Packaging**: Select **Zip**.

Click **Create build project** to finish setup.


<img width="693" alt="Screenshot 2025-06-27 at 11 06 58 PM" src="https://github.com/user-attachments/assets/a54c6a74-9773-4c3e-ad2f-528c8276bbd9" />

---

## 🔐 Step 3: Grant CodeBuild Access to CodeArtifact

By default, CodeBuild will create a service role. However, this role doesn’t include permissions to access AWS CodeArtifact.

1. Go to **IAM > Roles**.
2. Find the service role created by CodeBuild (e.g., `codebuild-your-project-name-service-role`).

 <img width="697" alt="Screenshot 2025-06-27 at 11 07 16 PM" src="https://github.com/user-attachments/assets/1f1fd728-1f2a-428a-8180-41d72a22f0ad" />
 
3. Click **Add Permissions**.
4. Attach the **AWSCodeArtifactReadOnlyAccess** or a custom policy that allows access to:
   
<img width="696" alt="Screenshot 2025-06-27 at 11 08 06 PM" src="https://github.com/user-attachments/assets/c2e2a410-587d-428f-bea1-7a69f2895f3d" />

```json
{
  "Effect": "Allow",
  "Action": [
    "codeartifact:GetAuthorizationToken",
    "codeartifact:GetRepositoryEndpoint",
    "codeartifact:ReadFromRepository"
  ],
  "Resource": "*"
}
```

---

## 📝 Step 4: Create the `buildspec.yml` File

Make sure this file exists at the **root** of your project repository:

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto8
  pre_build:
    commands:
      - echo Initializing environment
      - export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain devops --domain-owner 957491401343 --region ap-south-1 --query authorizationToken --output text`
      - mvn test
  build:
    commands:
      - echo Build started on `date`
      - mvn -s settings.xml compile
  post_build:
    commands:
      - echo Build completed on `date`
      - mvn -s settings.xml package

reports:
  junit_reports:
    files:
      - target/surefire-reports/*.xml

artifacts:
  files:
    - target/javawebapp_devops.war
    - appspec.yml
    - scripts/**/*
  discard-paths: no
```

---

## ▶️ Step 5: Start the Build

1. Go to your CodeBuild project.
2. Click **Start Build**.
3. Monitor the logs and wait for the build to complete.

<img width="694" alt="Screenshot 2025-06-27 at 11 08 56 PM" src="https://github.com/user-attachments/assets/46f1ac09-5190-474f-a06b-e4d66fbd872e" />

Once successful, we should see the build artifact (a `.zip` file) in your specified S3 bucket.

---

## 📦 Step 6: Verify S3 Bucket

Navigate to the S3 bucket, and confirm that the output `.zip` artifact has been created successfully.

<img width="704" alt="Screenshot 2025-06-27 at 11 09 11 PM" src="https://github.com/user-attachments/assets/a1f31384-8432-4d75-861f-81f3f01d0381" />

---

## ✅ Result

We now have a working CI pipeline where:

* CodeBuild pulls source code from GitHub
* Builds and packages your Java application
* Stores the output `.war` and related files into S3
