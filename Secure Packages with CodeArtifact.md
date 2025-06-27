# 🔧 Using AWS CodeArtifact to Store and Manage Dependencies 

This guide explains how to configure **AWS CodeArtifact** as your Maven package repository, set up the necessary IAM permissions, and compile your project to publish artifacts into the repository.

<img width="708" alt="Screenshot 2025-06-27 at 11 45 07 AM" src="https://github.com/user-attachments/assets/5ed52b90-cf15-49b3-b63a-583195cf0b8c" />

---

## 📦 Create a CodeArtifact Repository

1. Navigate to **AWS CodeArtifact** in the AWS Console.
2. Click **Create repository**.
3. Provide a repository name (e.g., `my-maven-repo`).
4. Set **Maven Central** as the upstream repository.
5. Click **Create repository**.

> ✅ This creates two repositories: your custom repo and `maven-central` as its upstream source.

---

## 🏷️ Create a Domain

1. Go to **Domains** in CodeArtifact.
2. Click **Create domain**.
3. Name the domain (e.g., `devops`).
4. Associate it with your repository.

<img width="699" alt="Screenshot 2025-06-27 at 11 51 03 AM" src="https://github.com/user-attachments/assets/bf2a8878-f3aa-47ff-8512-f5d09cbd21db" />


---

## 🔐 IAM Policy and Role Setup

To allow EC2 (or other AWS services) to pull from CodeArtifact, you'll need an IAM **policy** and a **role**.

### ✅ Policy JSON

Create a new policy with the following permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codeartifact:GetAuthorizationToken",
        "codeartifact:GetRepositoryEndpoint",
        "codeartifact:ReadFromRepository"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "sts:GetServiceBearerToken",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "sts:AWSServiceName": "codeartifact.amazonaws.com"
        }
      }
    }
  ]
}
```

### ✅ Create IAM Role

1. Go to **IAM > Roles > Create role**.
2. Select **EC2** as the use case.
3. Attach the policy you just created.
4. Name the role (e.g., `CodeArtifactAccessRole`).

<img width="746" alt="Screenshot 2025-06-27 at 11 46 29 AM" src="https://github.com/user-attachments/assets/72bbffd8-6b19-4562-85f6-22eb6cf015a3" />


---

## 🔗 Attach IAM Role to EC2 Instance

1. Open your EC2 instance in the AWS Console.
2. Click **Actions > Security > Modify IAM Role**.
3. Select your role (`CodeArtifactAccessRole`) from the list.
4. Save changes.

<img width="701" alt="Screenshot 2025-06-27 at 11 51 27 AM" src="https://github.com/user-attachments/assets/dcf25e1b-2b07-44d2-a258-c695a72e7907" />


---

## 🧪 Test the Connection

1. Go to your **CodeArtifact repository** in AWS.
2. Click **View connection instructions**.
3. Follow the steps:

   * Choose **Maven**.
   * Select your **domain**, **repository**, and **region**.
   * Follow steps 3–5 to set up your `settings.xml`.

* The settings.xml file that I used to set up the CodeArtifact is present in the other repository called the javawebapp_devops *
---

## 🛠️ Compile and Upload to CodeArtifact

After configuration, run:

```bash
mvn compile -s settings.xml
```

> 🔄 This will compile and upload Maven packages into your CodeArtifact repository.

---

## 📁 View Packages in CodeArtifact

* Go to the repository in the AWS Console.
* View the **Packages** tab to verify that artifacts were uploaded.

